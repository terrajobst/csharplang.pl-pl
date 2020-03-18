---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484700"
---
# <a name="conditional-ref-expressions"></a><span data-ttu-id="311fc-101">Warunkowe wyrażenia ref</span><span class="sxs-lookup"><span data-stu-id="311fc-101">Conditional ref expressions</span></span>

<span data-ttu-id="311fc-102">Wzorzec powiązania zmiennej ref z jednym lub innym wyrażeniem warunkowo nie jest obecnie można wyrazić elementu w C#.</span><span class="sxs-lookup"><span data-stu-id="311fc-102">The pattern of binding a ref variable to one or another expression conditionally is not currently expressible in C#.</span></span>

<span data-ttu-id="311fc-103">Typowym obejściem jest wprowadzenie metody podobnej do:</span><span class="sxs-lookup"><span data-stu-id="311fc-103">The typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="311fc-104">Należy zauważyć, że nie jest to dokładne zastąpienie Trzyelementowy, ponieważ wszystkie argumenty muszą być oceniane w miejscu wywołania.</span><span class="sxs-lookup"><span data-stu-id="311fc-104">Note that this is not an exact replacement of a ternary since all arguments must be evaluated at the call site.</span></span>

<span data-ttu-id="311fc-105">Następujące elementy nie będą działały zgodnie z oczekiwaniami:</span><span class="sxs-lookup"><span data-stu-id="311fc-105">The following will not work as expected:</span></span>

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

<span data-ttu-id="311fc-106">Proponowana składnia będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="311fc-106">The proposed syntax would look like:</span></span>

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

<span data-ttu-id="311fc-107">Powyższa próba ze specyfikatorem "Choice" może być _prawidłowo_ zapisywana przy użyciu parametru "Trzyelementowy" jako:</span><span class="sxs-lookup"><span data-stu-id="311fc-107">The above attempt with "Choice" can be _correctly_ written using ref ternary as:</span></span>

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="311fc-108">Różnica polega na tym, że wyrażenia sekwencji i alternatywne są dostępne w sposób _naprawdę_ warunkowo, więc nie widzimy awarii, jeśli ```arr == null```</span><span class="sxs-lookup"><span data-stu-id="311fc-108">The difference from Choice is that consequence and alternative expressions are accessed in a _truly_ conditional manner, so we do not see a crash if ```arr == null```</span></span>

<span data-ttu-id="311fc-109">Odwołaniem do sieci Trzyelementowy jest tylko typ Trzyelementowy, w przypadku którego alternatywa i skutki są odwołaniami.</span><span class="sxs-lookup"><span data-stu-id="311fc-109">The ternary ref is just a ternary where both alternative and consequence are refs.</span></span> <span data-ttu-id="311fc-110">W naturalny sposób będzie wymagane przekazanie/alternatywne operandy LValues.</span><span class="sxs-lookup"><span data-stu-id="311fc-110">It will naturally require that consequence/alternative operands are LValues.</span></span> <span data-ttu-id="311fc-111">Będzie również wymagało, aby z kolei wszystkie typy, które są do siebie konwertowane.</span><span class="sxs-lookup"><span data-stu-id="311fc-111">It will also require that consequence and alternative have types that are identity convertible to each other.</span></span>

<span data-ttu-id="311fc-112">Typ wyrażenia będzie obliczany podobnie jak w przypadku zwykłego elementu Trzyelementowy.</span><span class="sxs-lookup"><span data-stu-id="311fc-112">The type of the expression will be computed similarly to the one for the regular ternary.</span></span> <span data-ttu-id="311fc-113">I.E.</span><span class="sxs-lookup"><span data-stu-id="311fc-113">I.E.</span></span> <span data-ttu-id="311fc-114">w przypadku, gdy sekwencje i alternatywy mają konwersję tożsamości, ale różne typy, będą stosowane istniejące reguły scalania typu.</span><span class="sxs-lookup"><span data-stu-id="311fc-114">in a case if consequence and alternative have identity convertible, but different types, the existing type-merging rules will apply.</span></span>

<span data-ttu-id="311fc-115">Bezpieczne do zwrócenia będą uznawane za nieostrożnie od operandów warunkowych.</span><span class="sxs-lookup"><span data-stu-id="311fc-115">Safe-to-return will be assumed conservatively from the conditional operands.</span></span> <span data-ttu-id="311fc-116">Jeśli funkcja nie będzie mogła zwrócić całej rzeczy, nie będzie mogła zostać zwrócona.</span><span class="sxs-lookup"><span data-stu-id="311fc-116">If either is unsafe to return the whole thing is unsafe to return.</span></span>

<span data-ttu-id="311fc-117">Parametr "Trzyelementowy" ref to element LValue, który może być przesłany/przypisywany/zwracany przez odwołanie;</span><span class="sxs-lookup"><span data-stu-id="311fc-117">Ref ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="311fc-118">Jest to LValue, ale może być również przypisany do.</span><span class="sxs-lookup"><span data-stu-id="311fc-118">Being an LValue, it can also be assigned to.</span></span> 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

<span data-ttu-id="311fc-119">Typ "Trzyelementowy" można również użyć w zwykłym kontekście (nie ref).</span><span class="sxs-lookup"><span data-stu-id="311fc-119">Ref ternary can be used in a regular (not ref) context as well.</span></span> <span data-ttu-id="311fc-120">Mimo że nie będzie to typowe, ponieważ można używać zwykłej sieci Trzyelementowy.</span><span class="sxs-lookup"><span data-stu-id="311fc-120">Although it would not be common since you could as well just use a regular ternary.</span></span>

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

<span data-ttu-id="311fc-121">Uwagi dotyczące implementacji:</span><span class="sxs-lookup"><span data-stu-id="311fc-121">Implementation notes:</span></span> 

<span data-ttu-id="311fc-122">Złożoność implementacji wydaje się stanowić rozmiar rozwiązania typu umiarkowane-to-Large.</span><span class="sxs-lookup"><span data-stu-id="311fc-122">The complexity of the implementation would seem to be the size of a moderate-to-large bug fix.</span></span> <span data-ttu-id="311fc-123">-I. E nie jest bardzo kosztowna.</span><span class="sxs-lookup"><span data-stu-id="311fc-123">- I.E not very expensive.</span></span>
<span data-ttu-id="311fc-124">Nie myślę, że potrzebujemy żadnych zmian w składni lub analizie.</span><span class="sxs-lookup"><span data-stu-id="311fc-124">I do not think we need any changes to the syntax or parsing.</span></span>
<span data-ttu-id="311fc-125">Nie ma wpływu na metadane lub międzyoperacyjność.</span><span class="sxs-lookup"><span data-stu-id="311fc-125">There is no effect on metadata or interop.</span></span> <span data-ttu-id="311fc-126">Ta funkcja jest całkowicie oparta na wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="311fc-126">The feature is completely expression based.</span></span>
<span data-ttu-id="311fc-127">Brak wpływu na Debugowanie/PDB</span><span class="sxs-lookup"><span data-stu-id="311fc-127">No effect on debugging/PDB either</span></span>
