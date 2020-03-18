---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2019
ms.locfileid: "79484987"
---
# <a name="target-typed-default-literal"></a><span data-ttu-id="099f0-101">Literał "domyślny" z typem docelowym</span><span class="sxs-lookup"><span data-stu-id="099f0-101">Target-typed "default" literal</span></span>

* <span data-ttu-id="099f0-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="099f0-102">[x] Proposed</span></span>
* <span data-ttu-id="099f0-103">[x] prototyp</span><span class="sxs-lookup"><span data-stu-id="099f0-103">[x] Prototype</span></span>
* <span data-ttu-id="099f0-104">Implementacja [x]</span><span class="sxs-lookup"><span data-stu-id="099f0-104">[x] Implementation</span></span>
* <span data-ttu-id="099f0-105">[] — Specyfikacja</span><span class="sxs-lookup"><span data-stu-id="099f0-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="099f0-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="099f0-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="099f0-107">Funkcja `default` wpisana przez obiekt docelowy jest krótszą różnicą formy operatora `default(T)`, co pozwala na pominięcie tego typu.</span><span class="sxs-lookup"><span data-stu-id="099f0-107">The target-typed `default` feature is a shorter form variation of the `default(T)` operator, which allows the type to be omitted.</span></span> <span data-ttu-id="099f0-108">Jego typ jest wnioskowany przez wpisanie wartości docelowej.</span><span class="sxs-lookup"><span data-stu-id="099f0-108">Its type is inferred by target-typing instead.</span></span> <span data-ttu-id="099f0-109">Poza tym zachowuje się tak jak `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="099f0-109">Aside from that, it behaves like `default(T)`.</span></span>

## <a name="motivation"></a><span data-ttu-id="099f0-110">Motywacją</span><span class="sxs-lookup"><span data-stu-id="099f0-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="099f0-111">Główną motywacją jest uniknięcie wpisywania nadmiarowych informacji.</span><span class="sxs-lookup"><span data-stu-id="099f0-111">The main motivation is to avoid typing redundant information.</span></span>

<span data-ttu-id="099f0-112">Na przykład podczas wywoływania `void Method(ImmutableArray<SomeType> array)`, *domyślny* literał zezwala na `M(default)` zamiast `M(default(ImmutableArray<SomeType>))`.</span><span class="sxs-lookup"><span data-stu-id="099f0-112">For instance, when invoking `void Method(ImmutableArray<SomeType> array)`, the *default* literal allows `M(default)` in place of `M(default(ImmutableArray<SomeType>))`.</span></span>

<span data-ttu-id="099f0-113">Ma to zastosowanie w wielu scenariuszach, takich jak:</span><span class="sxs-lookup"><span data-stu-id="099f0-113">This is applicable in a number of scenarios, such as:</span></span>

- <span data-ttu-id="099f0-114">Deklarowanie elementów lokalnych (`ImmutableArray<SomeType> x = default;`)</span><span class="sxs-lookup"><span data-stu-id="099f0-114">declaring locals (`ImmutableArray<SomeType> x = default;`)</span></span>
- <span data-ttu-id="099f0-115">operacje Trzyelementowy (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span><span class="sxs-lookup"><span data-stu-id="099f0-115">ternary operations (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span></span>
- <span data-ttu-id="099f0-116">Zwracanie w metodach i wyrażeniach lambda (`return default;`)</span><span class="sxs-lookup"><span data-stu-id="099f0-116">returning in methods and lambdas (`return default;`)</span></span>
- <span data-ttu-id="099f0-117">Deklarowanie wartości domyślnych dla parametrów opcjonalnych (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span><span class="sxs-lookup"><span data-stu-id="099f0-117">declaring default values for optional parameters (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span></span>
- <span data-ttu-id="099f0-118">uwzględnianie wartości domyślnych w wyrażeniach tworzenia tablic (`var x = new[] { default, ImmutableArray.Create(y) };`)</span><span class="sxs-lookup"><span data-stu-id="099f0-118">including default values in array creation expressions (`var x = new[] { default, ImmutableArray.Create(y) };`)</span></span>


## <a name="detailed-design"></a><span data-ttu-id="099f0-119">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="099f0-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="099f0-120">Zostanie wprowadzone nowe wyrażenie, litera *default* .</span><span class="sxs-lookup"><span data-stu-id="099f0-120">A new expression is introduced, the *default* literal.</span></span> <span data-ttu-id="099f0-121">Wyrażenie z tą klasyfikacją może być niejawnie konwertowane na dowolny typ przez *domyślną konwersję literału*.</span><span class="sxs-lookup"><span data-stu-id="099f0-121">An expression with this classification can be implicitly converted to any type, by a *default literal conversion*.</span></span> 

<span data-ttu-id="099f0-122">Wnioskowanie typu dla literału *domyślnego* działa tak samo jak dla literału *wartości null* , z tą różnicą, że dowolny typ jest dozwolony (nie tylko typy referencyjne).</span><span class="sxs-lookup"><span data-stu-id="099f0-122">The inference of the type for the *default* literal works the same as that for the *null* literal, except that any type is allowed (not just reference types).</span></span>

<span data-ttu-id="099f0-123">Ta konwersja generuje wartość domyślną typu wywnioskowanego.</span><span class="sxs-lookup"><span data-stu-id="099f0-123">This conversion produces the default value of the inferred type.</span></span>

<span data-ttu-id="099f0-124">*Domyślny* literał może mieć wartość stałą, w zależności od typu wywnioskowanego.</span><span class="sxs-lookup"><span data-stu-id="099f0-124">The *default* literal may have a constant value, depending on the inferred type.</span></span> <span data-ttu-id="099f0-125">Dlatego `const int x = default;` jest dozwolony, ale `const int? y = default;` nie jest.</span><span class="sxs-lookup"><span data-stu-id="099f0-125">So `const int x = default;` is legal, but `const int? y = default;` is not.</span></span>

<span data-ttu-id="099f0-126">*Domyślny* literał może być operandem operatorów równości, tak długo, jak inny operand ma typ.</span><span class="sxs-lookup"><span data-stu-id="099f0-126">The *default* literal can be the operand of equality operators, as long as the other operand has a type.</span></span> <span data-ttu-id="099f0-127">Dlatego `default == x` i `x == default` są prawidłowymi wyrażeniami, ale `default == default` jest niedozwolone.</span><span class="sxs-lookup"><span data-stu-id="099f0-127">So `default == x` and `x == default` are valid expressions, but `default == default` is illegal.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="099f0-128">Wady</span><span class="sxs-lookup"><span data-stu-id="099f0-128">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="099f0-129">Niewielka wada polega na tym, że literał *domyślny* może być używany zamiast literału *wartości null* w większości kontekstów.</span><span class="sxs-lookup"><span data-stu-id="099f0-129">A minor drawback is that *default* literal can be used in place of *null* literal in most contexts.</span></span> <span data-ttu-id="099f0-130">Dwa wyjątki są `throw null;` i `null == null`, które są dozwolone dla literału *null* , ale nie jako literału *domyślnego* .</span><span class="sxs-lookup"><span data-stu-id="099f0-130">Two of the exceptions are `throw null;` and `null == null`, which are allowed for the *null* literal, but not the *default* literal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="099f0-131">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="099f0-131">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="099f0-132">Istnieje kilka rozwiązań alternatywnych, które należy wziąć pod uwagę:</span><span class="sxs-lookup"><span data-stu-id="099f0-132">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="099f0-133">Stan quo: funkcja nie jest uzasadniona pod względem własnych cech i deweloperzy nadal używają operatora domyślnego z typem jawnym.</span><span class="sxs-lookup"><span data-stu-id="099f0-133">The status quo:  The feature is not justified on its own merits and developers continue to use the default operator with an explicit type.</span></span>
- <span data-ttu-id="099f0-134">Rozszerzanie literału null: jest to podejście w języku VB z `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="099f0-134">Extending the null literal: This is the VB approach with `Nothing`.</span></span> <span data-ttu-id="099f0-135">Możemy zezwolić na `int x = null;`.</span><span class="sxs-lookup"><span data-stu-id="099f0-135">We could allow `int x = null;`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="099f0-136">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="099f0-136">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="099f0-137">[x] powinna być *Domyślnie* dozwolona jako operand operatora *is* lub *as* ?</span><span class="sxs-lookup"><span data-stu-id="099f0-137">[x] Should *default* be allowed as the operand of the *is* or *as* operators?</span></span> <span data-ttu-id="099f0-138">Odpowiedź: nie Zezwalaj na `default is T`, Zezwalaj `x is default`, Zezwalaj na `default as RefType` (z ostrzeżeniem zawsze o wartości null)</span><span class="sxs-lookup"><span data-stu-id="099f0-138">Answer:  disallow `default is T`, allow `x is default`, allow `default as RefType` (with always-null warning)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="099f0-139">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="099f0-139">Design meetings</span></span>

- [<span data-ttu-id="099f0-140">POPRAWKA LDM 3/7/2017</span><span class="sxs-lookup"><span data-stu-id="099f0-140">LDM 3/7/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [<span data-ttu-id="099f0-141">POPRAWKA LDM 3/28/2017</span><span class="sxs-lookup"><span data-stu-id="099f0-141">LDM 3/28/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [<span data-ttu-id="099f0-142">POPRAWKA LDM 5/31/2017</span><span class="sxs-lookup"><span data-stu-id="099f0-142">LDM 5/31/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
