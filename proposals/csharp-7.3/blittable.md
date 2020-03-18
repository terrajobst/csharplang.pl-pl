---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484623"
---
# <a name="unmanaged-type-constraint"></a>Niezarządzany typ ograniczenia

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Funkcja ograniczeń niezarządzanych umożliwi wymuszanie języka dla klasy typów znanych jako "typy niezarządzane" w specyfikacji C# językowej.  Jest to zdefiniowane w sekcji 18,2 jako typ, który nie jest typem referencyjnym i nie zawiera pól typu odwołania na żadnym poziomie zagnieżdżania.  

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Główną motywacją jest ułatwienie tworzenia kodu międzyoperacyjności niskiego poziomu w C#programie. Typy niezarządzane są jednym z podstawowych bloków konstrukcyjnych dla kodu międzyoperacyjności, ale brak obsługi w typach ogólnych uniemożliwia tworzenie procedur wielokrotnego użytku dla wszystkich typów niezarządzanych. Zamiast tego deweloperzy są zmuszeni do tworzenia tego samego kodu kliszy kotłowej dla każdego typu niezarządzanego w swojej bibliotece:

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

Aby włączyć ten typ scenariusza, język będzie wprowadzał nowe ograniczenie: niezarządzane:

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

To ograniczenie może zostać spełnione tylko przez typy, które pasują do definicji typu niezarządzanego w specyfikacji C# języka. Innym sposobem na to, że typ spełnia niezarządzane ograniczenie Jeśli, może być również używany jako wskaźnik. 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

Parametry typu z niezarządzanym ograniczeniem mogą używać wszystkich funkcji dostępnych dla typów niezarządzanych: wskaźniki, stałe itp... 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

To ograniczenie umożliwia również wydajne konwersje między danymi strukturalnymi i strumieniami bajtów. Jest to operacja, która jest wspólna w stosach sieci i warstwach serializacji:

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

Takie procedury są korzystne, ponieważ są provably bezpieczne w czasie kompilacji i w bezpłatnej alokacji.  Autorzy międzyoperacyjni dzisiaj nie mogą wykonać tej czynności (nawet jeśli jest to warstwa, w której wydajność jest krytyczna).  Zamiast tego muszą polegać na przydzielaniu procedur, które mają kosztowne testy środowiska uruchomieniowego, aby sprawdzić, czy wartości są prawidłowo niezarządzane.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Język wprowadzi nowe ograniczenie o nazwie `unmanaged`. W celu spełnienia tego ograniczenia typ musi być strukturą, a wszystkie pola typu muszą należeć do jednej z następujących kategorii:

- Mają typ `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` lub `UIntPtr`.
- Być dowolnego typu `enum`.
- Być typem wskaźnika.
- Być strukturą zdefiniowaną przez użytkownika, która satsifies ograniczenie `unmanaged`.

Pola wystąpienia wygenerowanego przez kompilator, takie jak te, automatycznie implementowane właściwości, muszą również spełniać te ograniczenia. 

Na przykład:

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

Nie można łączyć ograniczenia `unmanaged` z `struct`, `class` lub `new()`. To ograniczenie wynika z faktu, że `unmanaged` implikuje `struct` w związku z tym inne ograniczenia nie mają sensu.

Ograniczenie `unmanaged` nie jest wymuszane przez środowisko CLR tylko przez język. Aby zapobiec nieprawidłowym użyciu w innych językach, metody, które mają to ograniczenie, będą chronione przez mod-REQ. Uniemożliwi to innym językom używanie argumentów typu, które nie są typami niezarządzanymi.

Token `unmanaged` w ograniczeniu nie jest słowem kluczowym ani kontekstowym słowem kluczowym. Zamiast tego przypomina `var`, że jest on oceniany w tej lokalizacji i będzie:

- Powiąż ze zdefiniowanym przez użytkownika lub typem przywoływanym o nazwie `unmanaged`: będzie on traktowany tak samo, jak wszystkie inne nazwane typy ograniczenia są traktowane. 
- Powiąż z typem brak: ten element będzie interpretowany jako ograniczenie `unmanaged`.

W przypadku typu o nazwie `unmanaged` i jest on dostępny bez kwalifikacji w bieżącym kontekście, nie będzie można użyć ograniczenia `unmanaged`. Jest to równoległe reguły dotyczące `var` i typów zdefiniowanych przez użytkownika o tej samej nazwie. 

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Podstawową wadą tej funkcji jest zapewnienie niewielkiej liczby deweloperów: zazwyczaj autorów lub struktur bibliotek o niskim poziomie.  W związku z tym spędzamy cenny czas pracy w niewielkiej liczbie deweloperów. 

Te struktury są jednak często podstawą dla większości aplikacji platformy .NET.  W związku z tym, że usługa WINS wydajności/poprawność na tym poziomie może mieć efekt Ripple w ekosystemie platformy .NET.  To sprawia, że funkcja rozważa się nawet z ograniczonymi odbiorcami.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Istnieje kilka rozwiązań alternatywnych, które należy wziąć pod uwagę:

- Stan quo: funkcja nie jest uzasadniona ze względu na własne znaczenie i deweloperzy nadal używają niejawnego wyboru.

## <a name="questions"></a>Masz
[quesions]: #questions

### <a name="metadata-representation"></a>Reprezentacja metadanych

F# Język koduje ograniczenie w pliku podpisu, co oznacza C# , że nie można użyć ich reprezentacji. Należy wybrać nowy atrybut dla tego ograniczenia. Ponadto metoda, która ma to ograniczenie, musi być chroniona przez mod-REQ.

### <a name="blittable-vs-unmanaged"></a>Danych kopiowalnych a niezarządzana
F# Język ma bardzo [podobną funkcję](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) , która używa niezarządzanego słowa kluczowego. Nazwa danych kopiowalnych jest używana w Midori.  Warto chcieć zajrzeć do pierwszeństwa w tym miejscu i zamiast tego użyć niezarządzanego. 

**Rozwiązanie** Język zdecyduje się użyć niezarządzanego 

### <a name="verifier"></a>Weryfikatora

Czy należy zaktualizować weryfikator/środowisko uruchomieniowe, aby zrozumieć użycie wskaźników do parametrów typu ogólnego?  Lub może być po prostu działała bez zmian?

**Rozwiązanie** Nie trzeba zmieniać żadnych zmian. Wszystkie typy wskaźników są po prostu niemożliwym do zweryfikowania. 

## <a name="design-meetings"></a>Spotkania projektowe

Nie dotyczy
