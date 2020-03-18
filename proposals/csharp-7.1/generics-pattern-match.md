---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484707"
---
# <a name="pattern-matching-with-generics"></a><span data-ttu-id="8a202-101">dopasowanie wzorca z typami ogólnymi</span><span class="sxs-lookup"><span data-stu-id="8a202-101">pattern-matching with generics</span></span>

* <span data-ttu-id="8a202-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="8a202-102">[x] Proposed</span></span>
* <span data-ttu-id="8a202-103">[] Prototyp:</span><span class="sxs-lookup"><span data-stu-id="8a202-103">[ ] Prototype:</span></span>
* <span data-ttu-id="8a202-104">[] Implementacja:</span><span class="sxs-lookup"><span data-stu-id="8a202-104">[ ] Implementation:</span></span>
* <span data-ttu-id="8a202-105">[] Specyfikacja:</span><span class="sxs-lookup"><span data-stu-id="8a202-105">[ ] Specification:</span></span>

## <a name="summary"></a><span data-ttu-id="8a202-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="8a202-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="8a202-107">Specyfikacja dla [operatora exist C# as](../../spec/expressions.md#the-as-operator) pozwala na brak konwersji między typem operandu i określonym typem, gdy jest typem otwartym.</span><span class="sxs-lookup"><span data-stu-id="8a202-107">The specification for the [existing C# as operator](../../spec/expressions.md#the-as-operator) permits there to be no conversion between the type of the operand and the specified type when either is an open type.</span></span> <span data-ttu-id="8a202-108">Jednak w C# 7 wzorzec `Type identifier` wymaga konwersji między typem danych wejściowych a danym typem.</span><span class="sxs-lookup"><span data-stu-id="8a202-108">However, in C# 7 the `Type identifier` pattern requires there be a conversion between the type of the input and the given type.</span></span>

<span data-ttu-id="8a202-109">Firma Microsoft zaleca, aby przede wszystkim zmienić `expression is Type identifier`, a także zezwolić na warunki, gdy jest to dozwolone w C# 7, w przypadku, gdy `expression as Type` byłyby dozwolone.</span><span class="sxs-lookup"><span data-stu-id="8a202-109">We propose to relax this and change `expression is Type identifier`, in addition to being permitted in the conditions when it is permitted in C# 7, to also be permitted when `expression as Type` would be allowed.</span></span> <span data-ttu-id="8a202-110">W każdym przypadku nowe przypadki są sytuacje, w których typ wyrażenia lub określony typ jest typem otwartym.</span><span class="sxs-lookup"><span data-stu-id="8a202-110">Specifically, the new cases are cases where the type of the expression or the specified type is an open type.</span></span> 

## <a name="motivation"></a><span data-ttu-id="8a202-111">Motywacją</span><span class="sxs-lookup"><span data-stu-id="8a202-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="8a202-112">Przypadki, w których dopasowanie do wzorca powinno "oczywiście", nie można obecnie skompilować.</span><span class="sxs-lookup"><span data-stu-id="8a202-112">Cases where pattern-matching should "obviously" be permitted currently fail to compile.</span></span> <span data-ttu-id="8a202-113">Zobacz na przykład https://github.com/dotnet/roslyn/issues/16195.</span><span class="sxs-lookup"><span data-stu-id="8a202-113">See, for example, https://github.com/dotnet/roslyn/issues/16195.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="8a202-114">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="8a202-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="8a202-115">Zmienimy akapit we specyfikacji dopasowania do wzorca (proponowane dodanie jest pogrubienie):</span><span class="sxs-lookup"><span data-stu-id="8a202-115">We change the paragraph in the pattern-matching specification (the proposed addition is shown in bold):</span></span>

> <span data-ttu-id="8a202-116">Niektóre kombinacje typu statycznego po lewej stronie i danego typu są uznawane za niezgodne i powodują błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="8a202-116">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="8a202-117">Wartość typu statycznego `E` jest uznawana za *wzorzec zgodną* z typem `T`, jeśli istnieje konwersja tożsamości, niejawna konwersja odwołania, konwersja z opakowania, jawna konwersja odwołania lub konwersja rozpakowywania z `E` do `T`**lub jeśli zarówno `E`, jak i `T` jest typem otwartym**.</span><span class="sxs-lookup"><span data-stu-id="8a202-117">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`**, or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="8a202-118">Jest to błąd czasu kompilacji, jeśli wyrażenie typu `E` nie jest zgodne ze wzorcem typu w wzorcu typów, z którym jest on dopasowany.</span><span class="sxs-lookup"><span data-stu-id="8a202-118">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="8a202-119">Wady</span><span class="sxs-lookup"><span data-stu-id="8a202-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="8a202-120">Brak.</span><span class="sxs-lookup"><span data-stu-id="8a202-120">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="8a202-121">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="8a202-121">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="8a202-122">Brak.</span><span class="sxs-lookup"><span data-stu-id="8a202-122">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="8a202-123">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="8a202-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="8a202-124">Brak.</span><span class="sxs-lookup"><span data-stu-id="8a202-124">None.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="8a202-125">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="8a202-125">Design meetings</span></span>

<span data-ttu-id="8a202-126">Menedżer LDM uznał to pytanie i odczuwał to zmianę poziomu błędu.</span><span class="sxs-lookup"><span data-stu-id="8a202-126">LDM considered this question and felt it was a bug-fix level change.</span></span> <span data-ttu-id="8a202-127">Jest ona traktowana jako oddzielna funkcja języka, ponieważ po wprowadzeniu zmiany po wydaniu języka wprowadzono niezgodność do przodu.</span><span class="sxs-lookup"><span data-stu-id="8a202-127">We are treating it as a separate language feature because just making the change after the language has been released would introduce a forward incompatibility.</span></span> <span data-ttu-id="8a202-128">Użycie proponowanej zmiany wymaga, aby programista określił wersję języka 7,1.</span><span class="sxs-lookup"><span data-stu-id="8a202-128">Using the proposed change requires that the programmer specify language version 7.1.</span></span>
