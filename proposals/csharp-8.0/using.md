---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485057"
---
# <a name="pattern-based-using-and-using-declarations"></a><span data-ttu-id="6e1c4-101">"oparty na wzorcu przy użyciu" i "deklaracji using"</span><span class="sxs-lookup"><span data-stu-id="6e1c4-101">"pattern-based using" and "using declarations"</span></span>

## <a name="summary"></a><span data-ttu-id="6e1c4-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="6e1c4-102">Summary</span></span>

<span data-ttu-id="6e1c4-103">Język dodaje dwie nowe możliwości wokół instrukcji `using`, aby zarządzanie zasobami było prostsze: `using` powinna rozpoznać wzorzec jednorazowy oprócz `IDisposable` i dodać deklarację `using` do języka.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-103">The language will add two new capabilities around the `using` statement in order to make resource management simpler: `using` should recognize a disposable pattern in addition to `IDisposable` and add a `using` declaration to the language.</span></span>

## <a name="motivation"></a><span data-ttu-id="6e1c4-104">Motywacją</span><span class="sxs-lookup"><span data-stu-id="6e1c4-104">Motivation</span></span>

<span data-ttu-id="6e1c4-105">`using` instrukcji jest skutecznym narzędziem do zarządzania zasobami, ale wymaga to dość bitu procedury.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-105">The `using` statement is an effective tool for resource management today but it requires quite a bit of ceremony.</span></span> <span data-ttu-id="6e1c4-106">Metody, które mają wiele zasobów do zarządzania, mogą otrzymać składniowo ugrzęźnięcia z serii instrukcji `using`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-106">Methods that have a number of resources to manage can get syntactically bogged down with a series of `using` statements.</span></span> <span data-ttu-id="6e1c4-107">Ta składnia jest wystarczająca, że większość wytycznych dotyczących stylu kodowania jawnie ma wyjątek wokół nawiasów klamrowych w tym scenariuszu.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-107">This syntax burden is enough that most coding style guidelines explicitly have an exception around braces for this scenario.</span></span> 

<span data-ttu-id="6e1c4-108">Deklaracja `using` usuwa wiele procedury w tym miejscu i pobiera C# wartość par z innymi językami, które zawierają bloki zarządzania zasobami.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-108">The `using` declaration removes much of the ceremony here and gets C# on par with other languages that include resource management blocks.</span></span> <span data-ttu-id="6e1c4-109">Ponadto `using` oparte na wzorcu umożliwiają deweloperom rozszerzanie zestawu typów, które mogą uczestniczyć w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-109">Additionally the pattern-based `using` lets developers expand the set of types that can participate here.</span></span> <span data-ttu-id="6e1c4-110">W wielu przypadkach usunięcie konieczności tworzenia typów otoki, które istnieją tylko w celu zezwolenia na użycie wartości w instrukcji `using`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-110">In many cases removing the need to create wrapper types that only exist to allow for a values use in a `using` statement.</span></span> 

<span data-ttu-id="6e1c4-111">Te funkcje umożliwiają deweloperom uproszczenie i rozszerzanie scenariuszy, w których można zastosować `using`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-111">Together these features allow developers to simplify and expand the scenarios where `using` can be applied.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="6e1c4-112">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="6e1c4-112">Detailed Design</span></span> 

### <a name="using-declaration"></a><span data-ttu-id="6e1c4-113">using, deklaracja</span><span class="sxs-lookup"><span data-stu-id="6e1c4-113">using declaration</span></span>

<span data-ttu-id="6e1c4-114">Język zezwoli na dodanie `using` do deklaracji zmiennej lokalnej.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-114">The language will allow for `using` to be added to a local variable declaration.</span></span> <span data-ttu-id="6e1c4-115">Takie oświadczenie będzie miało taki sam skutek jak zadeklarowanie zmiennej w instrukcji `using` w tej samej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-115">Such a declaration will have the same effect as declaring the variable in a `using` statement at the same location.</span></span>

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

<span data-ttu-id="6e1c4-116">Okres istnienia `using` lokalnego będzie sięgał do końca zakresu, w którym jest zadeklarowany.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-116">The lifetime of a `using` local will extend to the end of the scope in which it is declared.</span></span> <span data-ttu-id="6e1c4-117">`using` lokalne zostaną następnie usunięte w odwrotnej kolejności, w której zostały zadeklarowane.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-117">The `using` locals will then be disposed in the reverse order in which they are declared.</span></span> 

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

<span data-ttu-id="6e1c4-118">Nie ma ograniczeń dotyczących `goto`ani żadnej innej konstrukcji przepływu sterowania w ramach deklaracji `using`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-118">There are no restrictions around `goto`, or any other control flow construct in the face of a `using` declaration.</span></span> <span data-ttu-id="6e1c4-119">Zamiast tego kod działa podobnie jak w przypadku równoważnej instrukcji `using`:</span><span class="sxs-lookup"><span data-stu-id="6e1c4-119">Instead the code acts just as it would for the equivalent `using` statement:</span></span>

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

<span data-ttu-id="6e1c4-120">Lokalna deklaracja zadeklarowana w `using` lokalnej deklaracji będzie niejawnie tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-120">A local declared in a `using` local declaration will be implicitly read-only.</span></span> <span data-ttu-id="6e1c4-121">Jest to zgodne z zachowaniem wartości lokalnych zadeklarowanych w instrukcji `using`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-121">This matches the behavior of locals declared in a `using` statement.</span></span> 

<span data-ttu-id="6e1c4-122">Gramatyka języka dla deklaracji `using` będzie następująca:</span><span class="sxs-lookup"><span data-stu-id="6e1c4-122">The language grammar for `using` declarations will be the following:</span></span>

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

<span data-ttu-id="6e1c4-123">Ograniczenia dotyczące deklaracji `using`:</span><span class="sxs-lookup"><span data-stu-id="6e1c4-123">Restrictions around `using` declaration:</span></span>

- <span data-ttu-id="6e1c4-124">Może nie występować bezpośrednio wewnątrz etykiety `case`, ale zamiast tego musi znajdować się wewnątrz bloku wewnątrz etykiety `case`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-124">May not appear directly inside a `case` label but instead must be within a block inside the `case` label.</span></span>
- <span data-ttu-id="6e1c4-125">Nie może pojawić się jako część deklaracji zmiennej `out`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-125">May not appear as part of an `out` variable declaration.</span></span> 
- <span data-ttu-id="6e1c4-126">Musi mieć inicjator dla każdego deklaratoru.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-126">Must have an initializer for each declarator.</span></span>
- <span data-ttu-id="6e1c4-127">Typ lokalny musi być niejawnie konwertowany, aby można było `IDisposable` lub spełnić wzorce `using`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-127">The local type must be implicitly convertible to `IDisposable` or fulfill the `using` pattern.</span></span>

### <a name="pattern-based-using"></a><span data-ttu-id="6e1c4-128">oparta na wzorcu</span><span class="sxs-lookup"><span data-stu-id="6e1c4-128">pattern-based using</span></span>

<span data-ttu-id="6e1c4-129">Język doda pojęcie wzorca jednorazowego: to jest typ, który ma dostępną metodę wystąpienia `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-129">The language will add the notion of a disposable pattern: that is a type which has an accessible `Dispose` instance method.</span></span> <span data-ttu-id="6e1c4-130">Typy, które pasują do wzorca jednorazowego mogą uczestniczyć w instrukcji `using` lub deklaracji bez konieczności implementowania `IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-130">Types which fit the disposable pattern can participate in a `using` statement or declaration without being required to implement `IDisposable`.</span></span> 

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

<span data-ttu-id="6e1c4-131">Dzięki temu deweloperzy będą mogli korzystać z `using` w wielu nowych scenariuszach:</span><span class="sxs-lookup"><span data-stu-id="6e1c4-131">This will allow developers to leverage `using` in a number of new scenarios:</span></span>

- <span data-ttu-id="6e1c4-132">`ref struct`: te typy nie mogą zaimplementować interfejsów dzisiaj i dlatego nie mogą uczestniczyć w instrukcjach `using`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-132">`ref struct`: These types can't implement interfaces today and hence can't participate in `using` statements.</span></span>
- <span data-ttu-id="6e1c4-133">Metody rozszerzające umożliwiają deweloperom rozszerzanie typów w innych zestawach, aby uczestniczyły w instrukcjach `using`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-133">Extension methods will allow developers to augment types in other assemblies to participate in `using` statements.</span></span>

<span data-ttu-id="6e1c4-134">W sytuacji, gdy typ może być niejawnie konwertowany do `IDisposable`, a także pasuje do wzorca jednorazowego, wówczas `IDisposable` będzie preferowane.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-134">In the situation where a type can be implicitly converted to `IDisposable` and also fits the disposable pattern, then `IDisposable` will be preferred.</span></span> <span data-ttu-id="6e1c4-135">Chociaż jest to odwrotne podejście `foreach` (wzorzec preferowany przez interfejs), jest to konieczne w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-135">While this takes the opposite approach of `foreach` (pattern preferred over interface) it is necessary for backwards compatibility.</span></span>

<span data-ttu-id="6e1c4-136">Te same ograniczenia dotyczące tradycyjnych instrukcji `using` są stosowane również tutaj: zmienne lokalne zadeklarowane w `using` są tylko do odczytu, a `null` wartość nie spowoduje zgłoszenia wyjątku itp... Generowanie kodu będzie się różnić tylko w tym, że nie będzie można rzutować do `IDisposable` przed wywołaniem metody Dispose:</span><span class="sxs-lookup"><span data-stu-id="6e1c4-136">The same restrictions from a traditional `using` statement apply here as well: local variables declared in the `using` are read-only, a `null` value will not cause an exception to be thrown, etc ... The code generation will be different only in that there will not be a cast to `IDisposable` before calling Dispose:</span></span>

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

<span data-ttu-id="6e1c4-137">Aby można było dopasować wzorzec jednorazowy, Metoda `Dispose` musi być dostępna, bez parametrów i ma `void` zwracanego typu.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-137">In order to fit the disposable pattern the `Dispose` method must be accessible, parameterless and have a `void` return type.</span></span> <span data-ttu-id="6e1c4-138">Nie ma żadnych innych ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-138">There are no other restrictions.</span></span> <span data-ttu-id="6e1c4-139">Oznacza to, że można jawnie użyć metod rozszerzających.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-139">This explicitly means that extension methods can be used here.</span></span>

## <a name="considerations"></a><span data-ttu-id="6e1c4-140">Zagadnienia do rozważenia</span><span class="sxs-lookup"><span data-stu-id="6e1c4-140">Considerations</span></span>

### <a name="case-labels-without-blocks"></a><span data-ttu-id="6e1c4-141">etykiety przypadku bez bloków</span><span class="sxs-lookup"><span data-stu-id="6e1c4-141">case labels without blocks</span></span>

<span data-ttu-id="6e1c4-142">`using declaration` jest niedozwolony bezpośrednio wewnątrz etykiety `case` ze względu na komplikacje w całym rzeczywistym okresie istnienia.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-142">A `using declaration` is illegal directly inside a `case` label due to complications around its actual lifetime.</span></span> <span data-ttu-id="6e1c4-143">Jednym z potencjalnych rozwiązań jest po prostu nadanie mu tego samego okresu istnienia co `out var` w tej samej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-143">One potential solution is to simply give it the same lifetime as an `out var` in the same location.</span></span> <span data-ttu-id="6e1c4-144">Uznano o dodatkową złożoność implementacji funkcji i łatwość obejścia tego problemu (po prostu dodaj blok do etykiety `case`) nieuzasadnione przejęcie tej trasy.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-144">It was deemed the extra complexity to the feature implementation and the ease of the work around (just add a block to the `case` label) didn't justify taking this route.</span></span>

## <a name="future-expansions"></a><span data-ttu-id="6e1c4-145">Przyszłe rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="6e1c4-145">Future Expansions</span></span>

### <a name="fixed-locals"></a><span data-ttu-id="6e1c4-146">stałe lokalne</span><span class="sxs-lookup"><span data-stu-id="6e1c4-146">fixed locals</span></span>

<span data-ttu-id="6e1c4-147">Instrukcja `fixed` ma wszystkie właściwości `using` instrukcji, które mają na celu wymuszenie `using` wartości lokalnych.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-147">A `fixed` statement has all of the properties of `using` statements that motivated the ability to have `using` locals.</span></span> <span data-ttu-id="6e1c4-148">Należy również wziąć pod uwagę rozszerzenie tej funkcji, tak aby `fixed` lokalnych.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-148">Consideration should be given to extending this feature to `fixed` locals as well.</span></span> <span data-ttu-id="6e1c4-149">Zasady dotyczące okresu istnienia i określania kolejności powinny być stosowane równie dobrze w przypadku `using` i `fixed`.</span><span class="sxs-lookup"><span data-stu-id="6e1c4-149">The lifetime and ordering rules should apply equally well for `using` and `fixed` here.</span></span>
