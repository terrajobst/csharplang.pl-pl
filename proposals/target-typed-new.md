---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217206"
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

- **Dowolny typ struktury** (w tym typy krotek)
- **Dowolny typ odwołania** (w tym typy delegatów)
- **Dowolny parametr typu** z konstruktorem lub ograniczeniem `struct`

z następującymi wyjątkami:

- **Typy wyliczeniowe:** nie wszystkie typy wyliczeniowe zawierają stałą zero, dlatego powinno być wskazane użycie jawnego elementu członkowskiego wyliczenia.
- **Typy interfejsów:** jest to funkcja niszowa, dlatego warto jawnie wspominać o typie.
- **Typy tablic:** tablice muszą mieć specjalną składnię, aby zapewnić długość.
- **dynamiczne:** nie zezwalamy na `new dynamic()`, więc nie zezwalamy na `new()` z `dynamic` jako typ docelowy.

Wszystkie inne typy, które nie są dozwolone w *object_creation_expression* są również wykluczone, na przykład typy wskaźnika.

Gdy typ docelowy jest typem wartości null, `new` wpisany przez element docelowy zostanie przekonwertowany na typ podstawowy zamiast typu dopuszczającego wartość null.

> **Problem otwarty:** czy zezwalamy na delegatów i krotek jako typ docelowy?

Powyższe reguły obejmują delegatów (typ referencyjny) i krotek (typ struktury). Mimo że oba typy są konstrukcyjną, jeśli typ jest wywnioskowany, można już użyć funkcji anonimowej lub literału krotki.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>Różne

`throw new()` jest niedozwolona.

`new` z typem docelowym nie jest dozwolony w przypadku operatorów binarnych.

Nie jest dozwolone, gdy nie ma żadnego typu docelowego: operatory jednoargumentowe, kolekcja `foreach`, w `using`, w trakcie dekonstrukcji, w wyrażeniu `await`, jako właściwość typu anonimowego (`new { Prop = new() }`), w instrukcji `lock`, w `sizeof`, w instrukcji `fixed`, w dostępie do elementu członkowskiego (`new().field`), w wyniku operacji dynamicznie wysyłanej (`someDynamic.Method(new())`) w zapytaniu LINQ jako operand operatora `is` jako lewy argument operacji operatora `??` ,  ...

Jest on również niedozwolony jako `ref`.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Istnieją pewne wątpliwości dotyczące `new` tworzenia nowych kategorii znaczących zmian, które zostały wprowadzone, ale istnieją już z `null` i `default`i nie było to znacznego problemu.

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
- [LDM — 2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM — 2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM — 2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM — 2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
