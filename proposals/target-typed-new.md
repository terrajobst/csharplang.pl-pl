---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484539"
---

# <a name="target-typed-new-expressions"></a>Wyrażenia `new` z typem docelowym

* [x] proponowane
* [x] [prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)
* [] Implementacja
* [] — Specyfikacja

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Nie wymagaj specyfikacji typu dla konstruktorów, gdy typ jest znany. 

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Zezwalaj na inicjowanie pola bez duplikowania typu.
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
Zezwalaj na pominięcie typu, jeśli można go wywnioskować na podstawie użycia.
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
Tworzenie wystąpienia obiektu bez wypełniania tekstu.
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Składnia *object_creation_expression* zostanie zmodyfikowana, aby *Typ* był opcjonalny, gdy nawiasy są obecne. Jest to wymagane, aby rozwiązać niejednoznaczność za pomocą *anonymous_object_creation_expression*.
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
`new` z typem docelowym jest konwertowany na dowolny typ. W związku z tym nie przyczynia się do rozpoznania przeciążenia. Jest to przede wszystkim uniknięcie nieprzewidywalnych zmian.

Lista argumentów i wyrażenia inicjatora będą powiązane po ustaleniu typu.

Typ wyrażenia zostanie wywnioskowany na podstawie typu docelowego, który będzie wymagał jednego z następujących elementów:

- **Dowolny typ struktury**
- **Dowolny typ referencyjny**
- **Dowolny parametr typu** z konstruktorem lub ograniczeniem `struct`

z następującymi wyjątkami:

- **Typy wyliczeniowe:** nie wszystkie typy wyliczeniowe zawierają stałą zero, dlatego powinno być wskazane użycie jawnego elementu członkowskiego wyliczenia.
- **Typy interfejsów:** jest to funkcja niszowa, dlatego warto jawnie wspominać o typie.
- **Typy tablic:** tablice muszą mieć specjalną składnię, aby zapewnić długość.
- **Konstruktor domyślny struktury**: Ta reguła określa wszystkie typy pierwotne i większość typów wartości. Jeśli chcesz użyć wartości domyślnej takich typów, możesz zamiast tego napisać `default`.

Wszystkie inne typy, które nie są dozwolone w *object_creation_expression* są również wykluczone, na przykład typy wskaźnika.

> **Problem otwarty:** czy zezwalamy na delegatów i krotek jako typ docelowy?

Powyższe reguły obejmują delegatów (typ referencyjny) i krotek (typ struktury). Mimo że oba typy są konstrukcyjną, jeśli typ jest wywnioskowany, można już użyć funkcji anonimowej lub literału krotki.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> **Problem otwarty:** czy `throw new()` z `Exception` jako typ docelowy?

Mamy `throw null` dzisiaj, ale nie `throw default` (chociaż miałoby to ten sam skutek). Z drugiej strony `throw new()` mogą być rzeczywiście przydatne jako skróty `throw new Exception(...)`. Należy zauważyć, że jest już dozwolony przez bieżącą specyfikację. `Exception` jest typem referencyjnym, a Specyfikacja instrukcji throw wskazuje, że wyrażenie jest konwertowane na `Exception`.

> **Problem otwarty:** czy zezwalamy na użycie `new` typu docelowego z porównaniem zdefiniowanym przez użytkownika i operatorami arytmetycznymi?

W celu porównania `default` obsługuje tylko równości (zdefiniowane przez użytkownika i wbudowane) operatory. Czy warto również zapewnić obsługę innych operatorów dla `new()`?

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Brak.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Większość reklamacji na temat typów, które są zbyt długie, aby można było duplikować w zainicjowaniu pól, nie jest jedynym typem *argumentów* , możemy wnioskować o argumenty typu, takie jak `new Dictionary(...)` (lub podobne) i wnioskowania argumentów typu lokalnie z argumentów lub inicjatora kolekcji.

## <a name="questions"></a>Masz
[questions]: #questions

- Czy należy zabronić użycia w drzewach wyrażeń? znaleziono
- Jak działa funkcja z argumentami `dynamic`? (bez specjalnego traktowania)
- Jak technologia IntelliSense powinna współpracować z `new()`? (tylko wtedy, gdy istnieje pojedynczy typ docelowy)
## <a name="design-meetings"></a>Spotkania projektowe

- [LDM — 2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
