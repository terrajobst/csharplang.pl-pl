---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484609"
---
# <a name="stackalloc-array-initializers"></a><span data-ttu-id="e283e-101">Inicjatory tablic stackalloc</span><span class="sxs-lookup"><span data-stu-id="e283e-101">Stackalloc array initializers</span></span>

## <a name="summary"></a><span data-ttu-id="e283e-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="e283e-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="e283e-103">Zezwalaj na używanie składni inicjatora tablicy z `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="e283e-103">Allow array initializer syntax to be used with `stackalloc`.</span></span>

## <a name="motivation"></a><span data-ttu-id="e283e-104">Motywacją</span><span class="sxs-lookup"><span data-stu-id="e283e-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="e283e-105">W czasie tworzenia można inicjować ich elementy.</span><span class="sxs-lookup"><span data-stu-id="e283e-105">Ordinary arrays can have their elements initialized at creation time.</span></span> <span data-ttu-id="e283e-106">Wydaje się, że jest to możliwe w `stackalloc` przypadku.</span><span class="sxs-lookup"><span data-stu-id="e283e-106">It seems reasonable to allow that in `stackalloc` case.</span></span>

<span data-ttu-id="e283e-107">Pytanie, dlaczego taka składnia nie jest dozwolona w `stackalloc` występuje dość często.</span><span class="sxs-lookup"><span data-stu-id="e283e-107">The question of why such syntax is not allowed with `stackalloc` arises fairly frequently.</span></span>  
<span data-ttu-id="e283e-108">Zobacz na przykład [#1112](https://github.com/dotnet/csharplang/issues/1112)</span><span class="sxs-lookup"><span data-stu-id="e283e-108">See, for example, [#1112](https://github.com/dotnet/csharplang/issues/1112)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="e283e-109">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="e283e-109">Detailed design</span></span>

<span data-ttu-id="e283e-110">Zwykłe tablice można utworzyć za pomocą następującej składni:</span><span class="sxs-lookup"><span data-stu-id="e283e-110">Ordinary arrays can be created through the following syntax:</span></span>

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

<span data-ttu-id="e283e-111">Należy zezwolić na tworzenie tablic przyznanych przez stos przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="e283e-111">We should allow stack allocated arrays be created through:</span></span>  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

<span data-ttu-id="e283e-112">Semantyka wszystkich przypadków jest w przybliżeniu taka sama jak w przypadku tablic.</span><span class="sxs-lookup"><span data-stu-id="e283e-112">The semantics of all cases is roughly the same as with arrays.</span></span>  
<span data-ttu-id="e283e-113">Na przykład: w ostatnim przypadku typ elementu jest wywnioskowany z inicjatora i musi być typem niezarządzanym.</span><span class="sxs-lookup"><span data-stu-id="e283e-113">For example: in the last case the element type is inferred from the initializer and must be an "unmanaged" type.</span></span>

<span data-ttu-id="e283e-114">Uwaga: funkcja nie jest zależna od elementu docelowego, który jest `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="e283e-114">NOTE: the feature is not dependent on the target being a `Span<T>`.</span></span> <span data-ttu-id="e283e-115">Jest to tak samo samo jak w przypadku `T*`, więc nie wydaje się, że jest to uzasadnione w przypadku `Span<T>` przypadku.</span><span class="sxs-lookup"><span data-stu-id="e283e-115">It is just as applicable in `T*` case, so it does not seem reasonable to predicate it on `Span<T>` case.</span></span>  

## <a name="translation"></a><span data-ttu-id="e283e-116">Tłumaczenie</span><span class="sxs-lookup"><span data-stu-id="e283e-116">Translation</span></span>

<span data-ttu-id="e283e-117">Implementacja algorytmie może po prostu zainicjować tablicę bezpośrednio po utworzeniu za pomocą szeregu przypisań elementów.</span><span class="sxs-lookup"><span data-stu-id="e283e-117">The naive implementation could just initialize the array right after creation through a series of element-wise assignments.</span></span>  

<span data-ttu-id="e283e-118">Podobnie jak w przypadku tablic, może być możliwe i pożądane, aby wykrywać przypadki, w których wszystkie lub większość elementów są typami danych kopiowalnych i używają bardziej wydajnych technik przez kopiowanie do wstępnie utworzonego stanu wszystkich elementów stałych.</span><span class="sxs-lookup"><span data-stu-id="e283e-118">Similarly to the case with arrays, it might be possible and desirable to detect cases where all or most of the elements are blittable types and use more efficient techniques by copying over the pre-created state of all the constant elements.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="e283e-119">Wady</span><span class="sxs-lookup"><span data-stu-id="e283e-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

## <a name="alternatives"></a><span data-ttu-id="e283e-120">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="e283e-120">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="e283e-121">Jest to wygodna funkcja.</span><span class="sxs-lookup"><span data-stu-id="e283e-121">This is a convenience feature.</span></span> <span data-ttu-id="e283e-122">Nie można nic robić.</span><span class="sxs-lookup"><span data-stu-id="e283e-122">It is possible to just do nothing.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="e283e-123">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="e283e-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="e283e-124">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="e283e-124">Design meetings</span></span>

<span data-ttu-id="e283e-125">Jeszcze nie.</span><span class="sxs-lookup"><span data-stu-id="e283e-125">None yet.</span></span> 
