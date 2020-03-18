---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484588"
---
# <a name="fixed-sized-buffers"></a><span data-ttu-id="7e3c2-101">Bufory o ustalonym rozmiarze</span><span class="sxs-lookup"><span data-stu-id="7e3c2-101">Fixed Sized Buffers</span></span>

* <span data-ttu-id="7e3c2-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="7e3c2-102">[x] Proposed</span></span>
* <span data-ttu-id="7e3c2-103">[] Prototyp: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="7e3c2-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="7e3c2-104">[] Implementacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="7e3c2-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="7e3c2-105">[] Specyfikacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="7e3c2-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="7e3c2-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="7e3c2-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="7e3c2-107">Zapewnienie ogólnego przeznaczenia i bezpiecznego mechanizmu deklarowania buforów o stałym rozmiarze do C# języka.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-107">Provide a general-purpose and safe mechanism for declaring fixed sized buffers to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="7e3c2-108">Motywacją</span><span class="sxs-lookup"><span data-stu-id="7e3c2-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="7e3c2-109">Obecnie użytkownicy mają możliwość tworzenia buforów o ustalonym rozmiarze w niebezpiecznym kontekście.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-109">Today, users have the ability to create fixed-sized buffers in an unsafe-context.</span></span> <span data-ttu-id="7e3c2-110">Jednak wymaga to, aby użytkownik mógł poradzić sobie ze wskaźnikami, ręcznie wykonywać powiązane sprawdzenia i obsługuje tylko ograniczony zestaw typów (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`i `double`).</span><span class="sxs-lookup"><span data-stu-id="7e3c2-110">However, this requires the user to deal with pointers, manually perform bounds checks, and only supports a limited set of types (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`, and `double`).</span></span>

<span data-ttu-id="7e3c2-111">Najbardziej typową skargą jest to, że bufory o ustalonym rozmiarze nie mogą być indeksowane w bezpiecznym kodzie.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-111">The most common complaint is that fixed-size buffers cannot be indexed in safe code.</span></span> <span data-ttu-id="7e3c2-112">Brak możliwości użycia większej liczby typów jest drugi.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-112">Inability to use more types is the second.</span></span>

<span data-ttu-id="7e3c2-113">W przypadku kilku drobnych dostosowań możemy udostępnić bufory o stałym rozmiarze, które obsługują dowolny typ, mogą być używane w bezpiecznym kontekście i mieć wykonywane automatyczne sprawdzanie powiązań.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-113">With a few minor tweaks, we could provide general-purpose fixed-sized buffers which support any type, can be used in a safe context, and have automatic bounds checking performed.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="7e3c2-114">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="7e3c2-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="7e3c2-115">Jeden z nich deklaruje bezpieczny bufor o stałym rozmiarze przy użyciu następujących funkcji:</span><span class="sxs-lookup"><span data-stu-id="7e3c2-115">One would declare a safe fixed-sized buffer via the following:</span></span>

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

<span data-ttu-id="7e3c2-116">Deklaracja zostanie przetłumaczona na wewnętrzną reprezentację przez kompilator, który jest podobny do poniższego</span><span class="sxs-lookup"><span data-stu-id="7e3c2-116">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

<span data-ttu-id="7e3c2-117">Ponieważ takie bufory o ustalonym rozmiarze nie wymagają już używania `fixed`, warto zezwolić na każdy typ elementu.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-117">Since such fixed-sized buffers no longer require use of `fixed`, it makes sense to allow any element type.</span></span>  

> <span data-ttu-id="7e3c2-118">Uwaga: `fixed` nadal będą obsługiwane, ale tylko wtedy, gdy typ elementu to `blittable`</span><span class="sxs-lookup"><span data-stu-id="7e3c2-118">NOTE: `fixed` will still be supported, but only if the element type is `blittable`</span></span>

## <a name="drawbacks"></a><span data-ttu-id="7e3c2-119">Wady</span><span class="sxs-lookup"><span data-stu-id="7e3c2-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

* <span data-ttu-id="7e3c2-120">Mogą występować pewne wyzwania z poprzednią zgodnością, ale ponieważ istniejące bufory o ustalonym rozmiarze działają tylko z wybranymi typami pierwotnymi, powinno być możliwe, aby kompilator kontynuował "działające w trybie" w przypadku, gdy użytkownik traktuje bufor stały jako przytrzymaj.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-120">There could be some challenges with backwards compatibility, but given that the existing fixed-sized buffers only work with a selection of primitive types, it should be possible for the compiler to continue "just-working" if the user treats the fixed-buffer as a pointer.</span></span>
* <span data-ttu-id="7e3c2-121">Niezgodne konstrukcje mogą wymagać użycia nieco innego kodowania `v2`, aby ukryć pola ze starego kompilatora.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-121">Incompatible constructs may need to use slightly different `v2` encoding to hide the fields from old compiler.</span></span>
* <span data-ttu-id="7e3c2-122">Pakowanie nie jest dobrze zdefiniowane w specyfikacji IL dla typów ogólnych.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-122">Packing is not well defined in IL spec for generic types.</span></span> <span data-ttu-id="7e3c2-123">Mimo że podejście powinno działać, zostanie ono obramowane w przypadku nieudokumentowanych zachowań.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-123">While the approach should work, we will be bordering on undocumented behavior.</span></span> <span data-ttu-id="7e3c2-124">Należy wprowadzić tę udokumentowaną i upewnić się, że inne JITs, takie jak mono, mają takie samo zachowanie.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-124">We should make that documented and make sure other JITs like Mono have the same behavior.</span></span>
* <span data-ttu-id="7e3c2-125">Określenie oddzielnego typu dla każdej długości (prawdopodobnie innej dla `readonly` pól, jeśli jest obsługiwana) będzie miało wpływ na metadane.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-125">Specifying a separate type for every length (an possibly another for `readonly` fields, if supported) will have impact on metadata.</span></span> <span data-ttu-id="7e3c2-126">Będzie ono powiązane z liczbą tablic różnych rozmiarów w danej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-126">It will be bound by the number of arrays of different sizes in the given app.</span></span>
* <span data-ttu-id="7e3c2-127">`ref` Math nie jest formalnie możliwa do zweryfikowania (ponieważ jest niebezpieczna).</span><span class="sxs-lookup"><span data-stu-id="7e3c2-127">`ref` math is not formally verifiable (since it is unsafe).</span></span> <span data-ttu-id="7e3c2-128">Będziemy musieli znaleźć sposób aktualizacji reguł weryfikacji, aby wiedzieć, że nasze użycie jest prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-128">We will need to find a way to update verification rules to know that our use is ok.</span></span>

## <a name="alternatives"></a><span data-ttu-id="7e3c2-129">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="7e3c2-129">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="7e3c2-130">Ręcznie Zadeklaruj struktury i użyj niebezpiecznego kodu do skonstruowania indeksatorów.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-130">Manually declare your structures and use unsafe code to construct indexers.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="7e3c2-131">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="7e3c2-131">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="7e3c2-132">Czy zezwalamy na `readonly`?</span><span class="sxs-lookup"><span data-stu-id="7e3c2-132">should we allow `readonly`?</span></span>  <span data-ttu-id="7e3c2-133">(z indeksatorem ReadOnly)</span><span class="sxs-lookup"><span data-stu-id="7e3c2-133">(with readonly indexer)</span></span>
- <span data-ttu-id="7e3c2-134">Czy zezwalamy na inicjatory tablicy?</span><span class="sxs-lookup"><span data-stu-id="7e3c2-134">should we allow array initializers?</span></span>
- <span data-ttu-id="7e3c2-135">Czy `fixed` jest wymagane słowo kluczowe?</span><span class="sxs-lookup"><span data-stu-id="7e3c2-135">is `fixed` keyword necessary?</span></span>
- <span data-ttu-id="7e3c2-136">`foreach`?</span><span class="sxs-lookup"><span data-stu-id="7e3c2-136">`foreach`?</span></span>
- <span data-ttu-id="7e3c2-137">tylko pola wystąpienia w strukturach?</span><span class="sxs-lookup"><span data-stu-id="7e3c2-137">only instance fields in structs?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="7e3c2-138">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="7e3c2-138">Design meetings</span></span>

<span data-ttu-id="7e3c2-139">Połącz się z uwagami dotyczącymi projektu, które mają wpływ na tę propozycję, i opisz w jednym zdaniu, dla każdej zmiany, do której zostały one wprowadzone.</span><span class="sxs-lookup"><span data-stu-id="7e3c2-139">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>