---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2019
ms.locfileid: "79484987"
---
# <a name="target-typed-default-literal"></a>Literał "domyślny" z typem docelowym

* [x] proponowane
* [x] prototyp
* Implementacja [x]
* [] — Specyfikacja

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Funkcja `default` wpisana przez obiekt docelowy jest krótszą różnicą formy operatora `default(T)`, co pozwala na pominięcie tego typu. Jego typ jest wnioskowany przez wpisanie wartości docelowej. Poza tym zachowuje się tak jak `default(T)`.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Główną motywacją jest uniknięcie wpisywania nadmiarowych informacji.

Na przykład podczas wywoływania `void Method(ImmutableArray<SomeType> array)`, *domyślny* literał zezwala na `M(default)` zamiast `M(default(ImmutableArray<SomeType>))`.

Ma to zastosowanie w wielu scenariuszach, takich jak:

- Deklarowanie elementów lokalnych (`ImmutableArray<SomeType> x = default;`)
- operacje Trzyelementowy (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)
- Zwracanie w metodach i wyrażeniach lambda (`return default;`)
- Deklarowanie wartości domyślnych dla parametrów opcjonalnych (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)
- uwzględnianie wartości domyślnych w wyrażeniach tworzenia tablic (`var x = new[] { default, ImmutableArray.Create(y) };`)


## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Zostanie wprowadzone nowe wyrażenie, litera *default* . Wyrażenie z tą klasyfikacją może być niejawnie konwertowane na dowolny typ przez *domyślną konwersję literału*. 

Wnioskowanie typu dla literału *domyślnego* działa tak samo jak dla literału *wartości null* , z tą różnicą, że dowolny typ jest dozwolony (nie tylko typy referencyjne).

Ta konwersja generuje wartość domyślną typu wywnioskowanego.

*Domyślny* literał może mieć wartość stałą, w zależności od typu wywnioskowanego. Dlatego `const int x = default;` jest dozwolony, ale `const int? y = default;` nie jest.

*Domyślny* literał może być operandem operatorów równości, tak długo, jak inny operand ma typ. Dlatego `default == x` i `x == default` są prawidłowymi wyrażeniami, ale `default == default` jest niedozwolone.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Niewielka wada polega na tym, że literał *domyślny* może być używany zamiast literału *wartości null* w większości kontekstów. Dwa wyjątki są `throw null;` i `null == null`, które są dozwolone dla literału *null* , ale nie jako literału *domyślnego* .

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Istnieje kilka rozwiązań alternatywnych, które należy wziąć pod uwagę:

- Stan quo: funkcja nie jest uzasadniona pod względem własnych cech i deweloperzy nadal używają operatora domyślnego z typem jawnym.
- Rozszerzanie literału null: jest to podejście w języku VB z `Nothing`. Możemy zezwolić na `int x = null;`.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

- [x] powinna być *Domyślnie* dozwolona jako operand operatora *is* lub *as* ? Odpowiedź: nie Zezwalaj na `default is T`, Zezwalaj `x is default`, Zezwalaj na `default as RefType` (z ostrzeżeniem zawsze o wartości null)

## <a name="design-meetings"></a>Spotkania projektowe

- [POPRAWKA LDM 3/7/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [POPRAWKA LDM 3/28/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [POPRAWKA LDM 5/31/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
