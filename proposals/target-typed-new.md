---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281973"
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

## <a name="specification"></a>Specyfikacja
[design]: #detailed-design

Nowy formularz składni, *target_typed_new* *object_creation_expression* zostanie zaakceptowany, gdy *Typ* jest opcjonalny.

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

Wyrażenie *target_typed_new* nie ma typu. Istnieje jednak Nowa *Konwersja tworzenia obiektów* , która jest niejawną konwersją z wyrażenia, która istnieje w *target_typed_new* do każdego typu.

Uwzględniając typ docelowy `T`, typ `T0` jest `T`typ podstawowy, jeśli `T` jest wystąpieniem `System.Nullable`. W przeciwnym razie `T0` jest `T`. Znaczenie wyrażenia *target_typed_new* , które jest konwertowane na typ `T` jest takie samo, jak znaczenie odpowiadającego *object_creation_expression* , który określa `T0` jako typ.

Jest to błąd czasu kompilacji, jeśli *target_typed_new* jest używany jako operand operatora jednoargumentowego lub binarnego lub jeśli jest używany, gdzie nie podlega *konwersji tworzenia obiektów*.

> **Problem otwarty:** czy zezwalamy na delegatów i krotek jako typ docelowy?

Powyższe reguły obejmują delegatów (typ referencyjny) i krotek (typ struktury). Mimo że oba typy są konstrukcyjną, jeśli typ jest wywnioskowany, można już użyć funkcji anonimowej lub literału krotki.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>Różne

Poniżej przedstawiono konsekwencje specyfikacji:

- dozwolony jest `throw new()` (typ docelowy to `System.Exception`)
- `new` z typem docelowym nie jest dozwolony w przypadku operatorów binarnych.
- Nie jest dozwolone, gdy nie ma żadnego typu docelowego: operatory jednoargumentowe, kolekcja `foreach`, w `using`, w trakcie dekonstrukcji, w wyrażeniu `await`, jako właściwość typu anonimowego (`new { Prop = new() }`), w instrukcji `lock`, w `sizeof`, w instrukcji `fixed`, w dostępie do elementu członkowskiego (`new().field`), w wyniku operacji dynamicznie wysyłanej (`someDynamic.Method(new())`) w zapytaniu LINQ jako operand operatora `is` jako lewy argument operacji operatora `??` ,  ...
- Jest on również niedozwolony jako `ref`.
- Następujące rodzaje typów nie są dozwolone jako elementy docelowe konwersji
  - **Typy wyliczeniowe:** `new()` będzie działać (jak `new Enum()` działa, aby nadać wartość domyślną), ale `new(1)` nie będzie działać, ponieważ typy wyliczeniowe nie mają konstruktora.
  - **Typy interfejsów:** To działanie będzie takie samo jak odpowiednie wyrażenie tworzenia dla typów COM.
  - **Typy tablic:** tablice muszą mieć specjalną składnię, aby zapewnić długość.    
  - **dynamiczne:** nie zezwalamy na `new dynamic()`, więc nie zezwalamy na `new()` z `dynamic` jako typ docelowy.
  - **krotki:** Mają one takie samo znaczenie jak tworzenie obiektów przy użyciu typu podstawowego.
  - Wszystkie inne typy, które nie są dozwolone w *object_creation_expression* są również wykluczone, na przykład typy wskaźnika.   

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
- [LDM — 2020-03-25](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
