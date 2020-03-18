---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484532"
---
# <a name="nullable-enhanced-common-type"></a>Ulepszony typ wspólny dopuszczający wartość null

* [x] proponowane
* [] Prototype: brak
* [] Implementacja: brak
* [] — Specyfikacja: patrz poniżej

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Istnieje sytuacja, w której bieżące algorytmy typowego typu są intuicyjne i umożliwiają programistom Dodawanie, co się stało, jak nadmiarowe rzutowanie na kod. W przypadku tej zmiany wyrażenie, takie jak `condition ? 1 : null`, spowoduje wystąpienie wartości typu `int?`, a wyrażenie takie jak `condition ? x : 1.0`, gdzie `x` jest typu `int?`, spowoduje powstanie wartości typu `double?`.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Jest to typowa przyczyna tego, co może być niezbędny dla programisty, taki jak potrzebny kod standardowy.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Modyfikujemy sposób [znajdowania najlepszego wspólnego typu zestawu wyrażeń](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) , aby mieć wpływ na następujące sytuacje:

- Jeśli jedno wyrażenie ma typ wartości niedopuszczający wartości null `T` a drugi jest literałem null, wynik jest typu `T?`.
- Jeśli jedno wyrażenie ma typ wartości null `T?` a druga jest typu wartości `U`i istnieje niejawna konwersja z `T` do `U`, wynik jest typu `U?`.

Ma to wpływ na następujące aspekty języka:

- [wyrażenie Trzyelementowy](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)
- niejawnie wpisane [wyrażenie tworzenia tablicy](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)
- Wnioskowanie [typu zwracanego przez wyrażenie lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) dla wnioskowania o typie
- przypadki obejmujące typy ogólne, takie jak wywoływanie `M<T>(T a, T b)` jako `M(1, null)`.

Dokładniej zmieniamy następujące sekcje specyfikacji (wstawienia pogrubione, usunięcia w przekreolenie):

> #### <a name="output-type-inferences"></a>Wnioskowanie o typie danych wyjściowych
> 
> *Wnioskowanie o typie danych wyjściowych* jest wykonywane *z* wyrażenia `E` *do* typu `T` w następujący sposób:
> 
> *  Jeśli `E` jest funkcją anonimową z odroczonym typem zwracanym `U` ([wywnioskowany typ zwracany](expressions.md#inferred-return-type)), a `T` jest typem delegata lub typem drzewa *wyrażenia z `Tb`* typu zwracanego, *a następnie* z `U` *do* `Tb`[Lower-bound inferences](expressions.md#lower-bound-inferences).
> *  W przeciwnym razie, jeśli `E` jest grupą metod, a `T` jest typem delegata lub typem drzewa wyrażenia z typami parametrów `T1...Tk` i zwracany typ `Tb`, a Rozpoznanie przeciążenia `E` z typami *`T1...Tk` powoduje utworzenie* pojedynczej metody z typem zwracanym `U`, a następnie przekazanie *od* `U` *do* `Tb`.
> *  \* * W przeciwnym razie, jeśli `E` jest wyrażeniem z typem wartości null `U?`, zostanie wystawiona *Dolna wnioskowanie* *z* `U` *do* `T` i do `T`zostanie dodane *ograniczenie o wartości null* . **
> *  W przeciwnym razie, jeśli `E` jest wyrażeniem typu `U`, następuje *Dolna granica wnioskowania* *z* `U` *do* `T`.
> *  **W przeciwnym razie, jeśli `E` jest wyrażeniem stałym z wartością `null`, zostanie dodane *ograniczenie o wartości null* do `T`** 
> *  W przeciwnym razie nie są wykonywane żadne wnioskowania.

> #### <a name="fixing"></a>Mocowanie
> 
> *Nieustalona* zmienna typu `Xi` z zestawem granic jest *ustalona* w następujący sposób:
> 
> *  Zestaw *typów kandydatów* `Uj` uruchamiany jako zestaw wszystkich typów w zestawie granic dla `Xi`.
> *  Następnie analizujemy poszczególne powiązania `Xi` z kolei: dla każdego dokładnego `U` `Xi` wszystkich typów `Uj`, które nie są identyczne z `U` są usuwane z zestawu kandydatów. Dla każdej dolnej granicy `U` `Xi` wszystkie typy `Uj`, do których *nie* istnieje niejawna konwersja z `U` są usuwane z zestawu kandydatów. Dla każdej górnej granicy `U` `Xi` wszystkie typy `Uj` z których *nie* istnieje niejawna konwersja do `U` są usuwane z zestawu kandydatów.
> *  Jeśli wśród pozostałych typów kandydatów `Uj` istnieje unikatowy typ `V`, z którego istnieje niejawna konwersja na wszystkie inne typy kandydatów, ~~`Xi` jest naprawiona `V`.~~
>     -  **Jeśli `V` jest typem wartości, a istnieje wartość *null* dla `Xi`, `Xi` jest stała do `V?`**
>     -  **W przeciwnym razie `Xi` są naprawione `V`**
> *  W przeciwnym razie wnioskowanie o typie nie powiedzie się.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

W ramach tej oferty mogą wystąpić pewne niezgodności.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Brak.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

- [] Co to jest ważność niezgodności wprowadzona przez tę propozycję, jeśli istnieje, i jak może ona zostać moderowana?

## <a name="design-meetings"></a>Spotkania projektowe

Brak.
