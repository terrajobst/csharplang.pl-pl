---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484532"
---
# <a name="nullable-enhanced-common-type"></a><span data-ttu-id="b286a-101">Ulepszony typ wspólny dopuszczający wartość null</span><span class="sxs-lookup"><span data-stu-id="b286a-101">Nullable-Enhanced Common Type</span></span>

* <span data-ttu-id="b286a-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="b286a-102">[x] Proposed</span></span>
* <span data-ttu-id="b286a-103">[] Prototype: brak</span><span class="sxs-lookup"><span data-stu-id="b286a-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="b286a-104">[] Implementacja: brak</span><span class="sxs-lookup"><span data-stu-id="b286a-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="b286a-105">[] — Specyfikacja: patrz poniżej</span><span class="sxs-lookup"><span data-stu-id="b286a-105">[ ] Specification: See below</span></span>

## <a name="summary"></a><span data-ttu-id="b286a-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="b286a-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b286a-107">Istnieje sytuacja, w której bieżące algorytmy typowego typu są intuicyjne i umożliwiają programistom Dodawanie, co się stało, jak nadmiarowe rzutowanie na kod.</span><span class="sxs-lookup"><span data-stu-id="b286a-107">There is a situation in which the current common-type algorithm results are counter-intuitive, and results in the programmer adding what feels like a redundant cast to the code.</span></span> <span data-ttu-id="b286a-108">W przypadku tej zmiany wyrażenie, takie jak `condition ? 1 : null`, spowoduje wystąpienie wartości typu `int?`, a wyrażenie takie jak `condition ? x : 1.0`, gdzie `x` jest typu `int?`, spowoduje powstanie wartości typu `double?`.</span><span class="sxs-lookup"><span data-stu-id="b286a-108">With this change, an expression such as `condition ? 1 : null` would result in a value of type `int?`, and an expression such as `condition ? x : 1.0` where `x` is of type `int?` would result in a value of type `double?`.</span></span>

## <a name="motivation"></a><span data-ttu-id="b286a-109">Motywacją</span><span class="sxs-lookup"><span data-stu-id="b286a-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b286a-110">Jest to typowa przyczyna tego, co może być niezbędny dla programisty, taki jak potrzebny kod standardowy.</span><span class="sxs-lookup"><span data-stu-id="b286a-110">This is a common cause of what feels to the programmer like needless boilerplate code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b286a-111">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="b286a-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b286a-112">Modyfikujemy sposób [znajdowania najlepszego wspólnego typu zestawu wyrażeń](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) , aby mieć wpływ na następujące sytuacje:</span><span class="sxs-lookup"><span data-stu-id="b286a-112">We modify the specification for [finding the best common type of a set of expressions](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) to affect the following situations:</span></span>

- <span data-ttu-id="b286a-113">Jeśli jedno wyrażenie ma typ wartości niedopuszczający wartości null `T` a drugi jest literałem null, wynik jest typu `T?`.</span><span class="sxs-lookup"><span data-stu-id="b286a-113">If one expression is of a non-nullable value type `T` and the other is a null literal, the result is of type `T?`.</span></span>
- <span data-ttu-id="b286a-114">Jeśli jedno wyrażenie ma typ wartości null `T?` a druga jest typu wartości `U`i istnieje niejawna konwersja z `T` do `U`, wynik jest typu `U?`.</span><span class="sxs-lookup"><span data-stu-id="b286a-114">If one expression is of a nullable value type `T?` and the other is of a value type `U`, and there is an implicit conversion from `T` to `U`, then the result is of type `U?`.</span></span>

<span data-ttu-id="b286a-115">Ma to wpływ na następujące aspekty języka:</span><span class="sxs-lookup"><span data-stu-id="b286a-115">This is expected to affect the following aspects of the language:</span></span>

- <span data-ttu-id="b286a-116">[wyrażenie Trzyelementowy](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span><span class="sxs-lookup"><span data-stu-id="b286a-116">the [ternary expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span></span>
- <span data-ttu-id="b286a-117">niejawnie wpisane [wyrażenie tworzenia tablicy](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span><span class="sxs-lookup"><span data-stu-id="b286a-117">implicitly typed [array creation expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span></span>
- <span data-ttu-id="b286a-118">Wnioskowanie [typu zwracanego przez wyrażenie lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) dla wnioskowania o typie</span><span class="sxs-lookup"><span data-stu-id="b286a-118">inferring the [return type of a lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) for type inference</span></span>
- <span data-ttu-id="b286a-119">przypadki obejmujące typy ogólne, takie jak wywoływanie `M<T>(T a, T b)` jako `M(1, null)`.</span><span class="sxs-lookup"><span data-stu-id="b286a-119">cases involving generics, such as invoking `M<T>(T a, T b)` as `M(1, null)`.</span></span>

<span data-ttu-id="b286a-120">Dokładniej zmieniamy następujące sekcje specyfikacji (wstawienia pogrubione, usunięcia w przekreolenie):</span><span class="sxs-lookup"><span data-stu-id="b286a-120">More precisely, we change the following sections of the specification (insertions in bold, deletions in strikethrough):</span></span>

> #### <a name="output-type-inferences"></a><span data-ttu-id="b286a-121">Wnioskowanie o typie danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="b286a-121">Output type inferences</span></span>
> 
> <span data-ttu-id="b286a-122">*Wnioskowanie o typie danych wyjściowych* jest wykonywane *z* wyrażenia `E` *do* typu `T` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b286a-122">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>
> 
> *  <span data-ttu-id="b286a-123">Jeśli `E` jest funkcją anonimową z odroczonym typem zwracanym `U` ([wywnioskowany typ zwracany](expressions.md#inferred-return-type)), a `T` jest typem delegata lub typem drzewa *wyrażenia z `Tb`* typu zwracanego, *a następnie* z `U` *do* `Tb`[Lower-bound inferences](expressions.md#lower-bound-inferences).</span><span class="sxs-lookup"><span data-stu-id="b286a-123">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="b286a-124">W przeciwnym razie, jeśli `E` jest grupą metod, a `T` jest typem delegata lub typem drzewa wyrażenia z typami parametrów `T1...Tk` i zwracany typ `Tb`, a Rozpoznanie przeciążenia `E` z typami *`T1...Tk` powoduje utworzenie* pojedynczej metody z typem zwracanym `U`, a następnie przekazanie *od* `U` *do* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="b286a-124">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="b286a-125">\* \* W przeciwnym razie, jeśli `E` jest wyrażeniem z typem wartości null `U?`, zostanie wystawiona *Dolna wnioskowanie* *z* `U` *do* `T` i do `T`zostanie dodane *ograniczenie o wartości null* .</span><span class="sxs-lookup"><span data-stu-id="b286a-125">\*\*Otherwise, if `E` is an expression with nullable value type `U?`, then a *lower-bound inference* is made *from* `U` *to* `T` and a *null bound* is added to `T`.</span></span> **
> *  <span data-ttu-id="b286a-126">W przeciwnym razie, jeśli `E` jest wyrażeniem typu `U`, następuje *Dolna granica wnioskowania* *z* `U` *do* `T`.</span><span class="sxs-lookup"><span data-stu-id="b286a-126">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
> *  <span data-ttu-id="b286a-127">**W przeciwnym razie, jeśli `E` jest wyrażeniem stałym z wartością `null`, zostanie dodane *ograniczenie o wartości null* do `T`**</span><span class="sxs-lookup"><span data-stu-id="b286a-127">**Otherwise, if `E` is a constant expression with value `null`, then a *null bound* is added to `T`**</span></span> 
> *  <span data-ttu-id="b286a-128">W przeciwnym razie nie są wykonywane żadne wnioskowania.</span><span class="sxs-lookup"><span data-stu-id="b286a-128">Otherwise, no inferences are made.</span></span>

> #### <a name="fixing"></a><span data-ttu-id="b286a-129">Mocowanie</span><span class="sxs-lookup"><span data-stu-id="b286a-129">Fixing</span></span>
> 
> <span data-ttu-id="b286a-130">*Nieustalona* zmienna typu `Xi` z zestawem granic jest *ustalona* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b286a-130">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>
> 
> *  <span data-ttu-id="b286a-131">Zestaw *typów kandydatów* `Uj` uruchamiany jako zestaw wszystkich typów w zestawie granic dla `Xi`.</span><span class="sxs-lookup"><span data-stu-id="b286a-131">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
> *  <span data-ttu-id="b286a-132">Następnie analizujemy poszczególne powiązania `Xi` z kolei: dla każdego dokładnego `U` `Xi` wszystkich typów `Uj`, które nie są identyczne z `U` są usuwane z zestawu kandydatów.</span><span class="sxs-lookup"><span data-stu-id="b286a-132">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="b286a-133">Dla każdej dolnej granicy `U` `Xi` wszystkie typy `Uj`, do których *nie* istnieje niejawna konwersja z `U` są usuwane z zestawu kandydatów.</span><span class="sxs-lookup"><span data-stu-id="b286a-133">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="b286a-134">Dla każdej górnej granicy `U` `Xi` wszystkie typy `Uj` z których *nie* istnieje niejawna konwersja do `U` są usuwane z zestawu kandydatów.</span><span class="sxs-lookup"><span data-stu-id="b286a-134">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
> *  <span data-ttu-id="b286a-135">Jeśli wśród pozostałych typów kandydatów `Uj` istnieje unikatowy typ `V`, z którego istnieje niejawna konwersja na wszystkie inne typy kandydatów, ~~`Xi` jest naprawiona `V`.~~</span><span class="sxs-lookup"><span data-stu-id="b286a-135">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then ~~`Xi` is fixed to `V`.~~</span></span>
>     -  <span data-ttu-id="b286a-136">**Jeśli `V` jest typem wartości, a istnieje wartość *null* dla `Xi`, `Xi` jest stała do `V?`**</span><span class="sxs-lookup"><span data-stu-id="b286a-136">**If `V` is a value type and there is a *null bound* for `Xi`, then `Xi` is fixed to `V?`**</span></span>
>     -  <span data-ttu-id="b286a-137">**W przeciwnym razie `Xi` są naprawione `V`**</span><span class="sxs-lookup"><span data-stu-id="b286a-137">**Otherwise   `Xi` is fixed to `V`**</span></span>
> *  <span data-ttu-id="b286a-138">W przeciwnym razie wnioskowanie o typie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="b286a-138">Otherwise, type inference fails.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="b286a-139">Wady</span><span class="sxs-lookup"><span data-stu-id="b286a-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="b286a-140">W ramach tej oferty mogą wystąpić pewne niezgodności.</span><span class="sxs-lookup"><span data-stu-id="b286a-140">There may be some incompatibilities introduced by this proposal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="b286a-141">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="b286a-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="b286a-142">Brak.</span><span class="sxs-lookup"><span data-stu-id="b286a-142">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="b286a-143">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="b286a-143">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="b286a-144">[] Co to jest ważność niezgodności wprowadzona przez tę propozycję, jeśli istnieje, i jak może ona zostać moderowana?</span><span class="sxs-lookup"><span data-stu-id="b286a-144">[ ] What is the severity of incompatibility introduced by this proposal, if any, and how can it be moderated?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="b286a-145">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="b286a-145">Design meetings</span></span>

<span data-ttu-id="b286a-146">Brak.</span><span class="sxs-lookup"><span data-stu-id="b286a-146">None.</span></span>
