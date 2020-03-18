---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485057"
---
# <a name="pattern-based-using-and-using-declarations"></a>"oparty na wzorcu przy użyciu" i "deklaracji using"

## <a name="summary"></a>Podsumowanie

Język dodaje dwie nowe możliwości wokół instrukcji `using`, aby zarządzanie zasobami było prostsze: `using` powinna rozpoznać wzorzec jednorazowy oprócz `IDisposable` i dodać deklarację `using` do języka.

## <a name="motivation"></a>Motywacją

`using` instrukcji jest skutecznym narzędziem do zarządzania zasobami, ale wymaga to dość bitu procedury. Metody, które mają wiele zasobów do zarządzania, mogą otrzymać składniowo ugrzęźnięcia z serii instrukcji `using`. Ta składnia jest wystarczająca, że większość wytycznych dotyczących stylu kodowania jawnie ma wyjątek wokół nawiasów klamrowych w tym scenariuszu. 

Deklaracja `using` usuwa wiele procedury w tym miejscu i pobiera C# wartość par z innymi językami, które zawierają bloki zarządzania zasobami. Ponadto `using` oparte na wzorcu umożliwiają deweloperom rozszerzanie zestawu typów, które mogą uczestniczyć w tym miejscu. W wielu przypadkach usunięcie konieczności tworzenia typów otoki, które istnieją tylko w celu zezwolenia na użycie wartości w instrukcji `using`. 

Te funkcje umożliwiają deweloperom uproszczenie i rozszerzanie scenariuszy, w których można zastosować `using`.

## <a name="detailed-design"></a>Szczegółowy projekt 

### <a name="using-declaration"></a>using, deklaracja

Język zezwoli na dodanie `using` do deklaracji zmiennej lokalnej. Takie oświadczenie będzie miało taki sam skutek jak zadeklarowanie zmiennej w instrukcji `using` w tej samej lokalizacji.

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

Okres istnienia `using` lokalnego będzie sięgał do końca zakresu, w którym jest zadeklarowany. `using` lokalne zostaną następnie usunięte w odwrotnej kolejności, w której zostały zadeklarowane. 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

Nie ma ograniczeń dotyczących `goto`ani żadnej innej konstrukcji przepływu sterowania w ramach deklaracji `using`. Zamiast tego kod działa podobnie jak w przypadku równoważnej instrukcji `using`:

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

Lokalna deklaracja zadeklarowana w `using` lokalnej deklaracji będzie niejawnie tylko do odczytu. Jest to zgodne z zachowaniem wartości lokalnych zadeklarowanych w instrukcji `using`. 

Gramatyka języka dla deklaracji `using` będzie następująca:

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

Ograniczenia dotyczące deklaracji `using`:

- Może nie występować bezpośrednio wewnątrz etykiety `case`, ale zamiast tego musi znajdować się wewnątrz bloku wewnątrz etykiety `case`.
- Nie może pojawić się jako część deklaracji zmiennej `out`. 
- Musi mieć inicjator dla każdego deklaratoru.
- Typ lokalny musi być niejawnie konwertowany, aby można było `IDisposable` lub spełnić wzorce `using`.

### <a name="pattern-based-using"></a>oparta na wzorcu

Język doda pojęcie wzorca jednorazowego: to jest typ, który ma dostępną metodę wystąpienia `Dispose`. Typy, które pasują do wzorca jednorazowego mogą uczestniczyć w instrukcji `using` lub deklaracji bez konieczności implementowania `IDisposable`. 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

Dzięki temu deweloperzy będą mogli korzystać z `using` w wielu nowych scenariuszach:

- `ref struct`: te typy nie mogą zaimplementować interfejsów dzisiaj i dlatego nie mogą uczestniczyć w instrukcjach `using`.
- Metody rozszerzające umożliwiają deweloperom rozszerzanie typów w innych zestawach, aby uczestniczyły w instrukcjach `using`.

W sytuacji, gdy typ może być niejawnie konwertowany do `IDisposable`, a także pasuje do wzorca jednorazowego, wówczas `IDisposable` będzie preferowane. Chociaż jest to odwrotne podejście `foreach` (wzorzec preferowany przez interfejs), jest to konieczne w celu zapewnienia zgodności z poprzednimi wersjami.

Te same ograniczenia dotyczące tradycyjnych instrukcji `using` są stosowane również tutaj: zmienne lokalne zadeklarowane w `using` są tylko do odczytu, a `null` wartość nie spowoduje zgłoszenia wyjątku itp... Generowanie kodu będzie się różnić tylko w tym, że nie będzie można rzutować do `IDisposable` przed wywołaniem metody Dispose:

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

Aby można było dopasować wzorzec jednorazowy, Metoda `Dispose` musi być dostępna, bez parametrów i ma `void` zwracanego typu. Nie ma żadnych innych ograniczeń. Oznacza to, że można jawnie użyć metod rozszerzających.

## <a name="considerations"></a>Zagadnienia do rozważenia

### <a name="case-labels-without-blocks"></a>etykiety przypadku bez bloków

`using declaration` jest niedozwolony bezpośrednio wewnątrz etykiety `case` ze względu na komplikacje w całym rzeczywistym okresie istnienia. Jednym z potencjalnych rozwiązań jest po prostu nadanie mu tego samego okresu istnienia co `out var` w tej samej lokalizacji. Uznano o dodatkową złożoność implementacji funkcji i łatwość obejścia tego problemu (po prostu dodaj blok do etykiety `case`) nieuzasadnione przejęcie tej trasy.

## <a name="future-expansions"></a>Przyszłe rozszerzenia

### <a name="fixed-locals"></a>stałe lokalne

Instrukcja `fixed` ma wszystkie właściwości `using` instrukcji, które mają na celu wymuszenie `using` wartości lokalnych. Należy również wziąć pod uwagę rozszerzenie tej funkcji, tak aby `fixed` lokalnych. Zasady dotyczące okresu istnienia i określania kolejności powinny być stosowane równie dobrze w przypadku `using` i `fixed`.
