---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108375"
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

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
