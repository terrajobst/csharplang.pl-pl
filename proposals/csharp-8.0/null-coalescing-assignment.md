---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/11/2019
ms.locfileid: "79484959"
---
# <a name="null-coalescing-assignment"></a><span data-ttu-id="4183e-101">przypisanie łączące o wartości null</span><span class="sxs-lookup"><span data-stu-id="4183e-101">null coalescing assignment</span></span>

* <span data-ttu-id="4183e-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="4183e-102">[x] Proposed</span></span>
* <span data-ttu-id="4183e-103">[] Prototyp: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="4183e-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="4183e-104">[] Implementacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="4183e-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="4183e-105">[] Specyfikacja: poniżej</span><span class="sxs-lookup"><span data-stu-id="4183e-105">[ ] Specification: Below</span></span>

## <a name="summary"></a><span data-ttu-id="4183e-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="4183e-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="4183e-107">Upraszcza typowy wzorzec kodowania, gdzie zmienna ma przypisaną wartość, jeśli jest równa null.</span><span class="sxs-lookup"><span data-stu-id="4183e-107">Simplifies a common coding pattern where a variable is assigned a value if it is null.</span></span>

<span data-ttu-id="4183e-108">W ramach tej propozycji zostanie również pożądany typ wymagania dotyczące `??`, aby zezwolić na wyrażenie, którego typem jest nieograniczonego parametru typu, który będzie używany po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="4183e-108">As part of this proposal, we will also loosen the type requirements on `??` to allow an expression whose type is an unconstrained type parameter to be used on the left-hand side.</span></span>

## <a name="motivation"></a><span data-ttu-id="4183e-109">Motywacją</span><span class="sxs-lookup"><span data-stu-id="4183e-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="4183e-110">Często widać kod w formularzu</span><span class="sxs-lookup"><span data-stu-id="4183e-110">It is common to see code of the form</span></span>

```csharp
if (variable == null)
{
    variable = expression;
}
```

<span data-ttu-id="4183e-111">Ta propozycja dodaje operator binarny, którego nie można przeładować do języka, który wykonuje tę funkcję.</span><span class="sxs-lookup"><span data-stu-id="4183e-111">This proposal adds a non-overloadable binary operator to the language that performs this function.</span></span>

<span data-ttu-id="4183e-112">Dla tej funkcji wprowadzono co najmniej osiem oddzielnych żądań społeczności.</span><span class="sxs-lookup"><span data-stu-id="4183e-112">There have been at least eight separate community requests for this feature.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="4183e-113">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="4183e-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="4183e-114">Dodamy nową formę operatora przypisania</span><span class="sxs-lookup"><span data-stu-id="4183e-114">We add a new form of assignment operator</span></span>

``` antlr
assignment_operator
    : '??='
    ;
```

<span data-ttu-id="4183e-115">Które są zgodne z [istniejącymi regułami semantycznymi operatorów przypisania złożonego](../../spec/expressions.md#compound-assignment), z tą różnicą, że Elide przypisanie, jeśli lewa strona jest inna niż null.</span><span class="sxs-lookup"><span data-stu-id="4183e-115">Which follows the [existing semantic rules for compound assignment operators](../../spec/expressions.md#compound-assignment), except that we elide the assignment if the left-hand side is non-null.</span></span> <span data-ttu-id="4183e-116">Reguły dla tej funkcji są następujące.</span><span class="sxs-lookup"><span data-stu-id="4183e-116">The rules for this feature are as follows.</span></span>

<span data-ttu-id="4183e-117">Podano `a ??= b`, gdzie `A` jest typem `a``B` jest typem `b`, a `A0` jest typem podstawowym `A`, jeśli `A` jest typem wartości null:</span><span class="sxs-lookup"><span data-stu-id="4183e-117">Given `a ??= b`, where `A` is the type of `a`, `B` is the type of `b`, and `A0` is the underlying type of `A` if `A` is a nullable value type:</span></span>

1. <span data-ttu-id="4183e-118">Jeśli `A` nie istnieje lub jest typem wartości niedopuszczających wartości null, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="4183e-118">If `A` does not exist or is a non-nullable value type, a compile-time error occurs.</span></span>
2. <span data-ttu-id="4183e-119">Jeśli `B` nie można przekonwertować niejawnie na `A` lub `A0` (jeśli `A0` istnieje), wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="4183e-119">If `B` is not implicitly convertible to `A` or `A0` (if `A0` exists), a compile-time error occurs.</span></span>
3. <span data-ttu-id="4183e-120">Jeśli `A0` istnieje i `B` jest niejawnie konwertowany na `A0`, a `B` nie jest dynamiczna, typ `a ??= b` jest `A0`.</span><span class="sxs-lookup"><span data-stu-id="4183e-120">If `A0` exists and `B` is implicitly convertible to `A0`, and `B` is not dynamic, then the type of `a ??= b` is `A0`.</span></span> <span data-ttu-id="4183e-121">`a ??= b` jest oceniane w czasie wykonywania jako:</span><span class="sxs-lookup"><span data-stu-id="4183e-121">`a ??= b` is evaluated at runtime as:</span></span>
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   <span data-ttu-id="4183e-122">z tą różnicą, że `a` są oceniane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="4183e-122">Except that `a` is only evaluated once.</span></span>
4. <span data-ttu-id="4183e-123">W przeciwnym razie typ `a ??= b` jest `A`.</span><span class="sxs-lookup"><span data-stu-id="4183e-123">Otherwise, the type of `a ??= b` is `A`.</span></span> <span data-ttu-id="4183e-124">`a ??= b` jest oceniane w czasie wykonywania jako `a ?? (a = b)`, z tą różnicą, że `a` jest obliczana tylko raz.</span><span class="sxs-lookup"><span data-stu-id="4183e-124">`a ??= b` is evaluated at runtime as `a ?? (a = b)`, except that `a` is only evaluated once.</span></span>


<span data-ttu-id="4183e-125">Aby wymusić wymagania dotyczące typu `??`, aktualizujemy specyfikację, w której obecnie stwierdza, że dane `a ?? b`, gdzie `A` jest typem `a`:</span><span class="sxs-lookup"><span data-stu-id="4183e-125">For the relaxation of the type requirements of `??`, we update the spec where it currently states that, given `a ?? b`, where `A` is the type of `a`:</span></span>

> 1. <span data-ttu-id="4183e-126">Jeśli istnieje i nie jest typem dopuszczającym wartość null lub typem referencyjnym, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="4183e-126">If A exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>

<span data-ttu-id="4183e-127">To wymaganie należy do:</span><span class="sxs-lookup"><span data-stu-id="4183e-127">We relax this requirement to:</span></span>

1. <span data-ttu-id="4183e-128">Jeśli istnieje i ma typ wartości niedopuszczający wartości null, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="4183e-128">If A exists and is a non-nullable value type, a compile-time error occurs.</span></span>

<span data-ttu-id="4183e-129">Dzięki temu operator łączenia o wartości null może współdziałać z niedozwolonymi parametrami typu, ponieważ parametr typu nieograniczonego T istnieje, nie jest typem dopuszczającym wartość null i nie jest typem referencyjnym.</span><span class="sxs-lookup"><span data-stu-id="4183e-129">This allows the null coalescing operator to work on unconstrained type parameters, as the unconstrained type parameter T exists, is not a nullable type, and is not a reference type.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="4183e-130">Wady</span><span class="sxs-lookup"><span data-stu-id="4183e-130">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="4183e-131">Podobnie jak w przypadku dowolnej funkcji języka należy zwrócić uwagę na to, czy dodatkowa złożoność tego języka jest zapłacona w dodatkowym przejrzystości oferowanym C# przez funkcję.</span><span class="sxs-lookup"><span data-stu-id="4183e-131">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="4183e-132">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="4183e-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="4183e-133">Programista może ręcznie pisać `(x = x ?? y)`, `if (x == null) x = y;`lub `x ?? (x = y)`.</span><span class="sxs-lookup"><span data-stu-id="4183e-133">The programmer can write `(x = x ?? y)`, `if (x == null) x = y;`, or `x ?? (x = y)` by hand.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="4183e-134">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="4183e-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="4183e-135">[] Wymaga przeglądu LDM</span><span class="sxs-lookup"><span data-stu-id="4183e-135">[ ] Requires LDM review</span></span>
- <span data-ttu-id="4183e-136">[] Czy obsługujemy również `&&=` i operatory `||=`?</span><span class="sxs-lookup"><span data-stu-id="4183e-136">[ ] Should we also support `&&=` and `||=` operators?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="4183e-137">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="4183e-137">Design meetings</span></span>

<span data-ttu-id="4183e-138">Brak.</span><span class="sxs-lookup"><span data-stu-id="4183e-138">None.</span></span>
