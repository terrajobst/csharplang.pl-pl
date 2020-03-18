---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484476"
---
# <a name="static-delegates"></a><span data-ttu-id="6800a-101">Elementy delegowane statyczne</span><span class="sxs-lookup"><span data-stu-id="6800a-101">Static Delegates</span></span>

* <span data-ttu-id="6800a-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="6800a-102">[x] Proposed</span></span>
* <span data-ttu-id="6800a-103">[] Prototyp: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="6800a-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="6800a-104">[] Implementacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="6800a-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="6800a-105">[] Specyfikacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="6800a-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="6800a-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="6800a-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="6800a-107">Zapewnij ogólną, uproszczoną funkcję wywołania zwrotnego dla C# języka.</span><span class="sxs-lookup"><span data-stu-id="6800a-107">Provide a general-purpose, lightweight callback capability to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="6800a-108">Motywacją</span><span class="sxs-lookup"><span data-stu-id="6800a-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="6800a-109">Obecnie użytkownicy mają możliwość tworzenia wywołań zwrotnych przy użyciu typu `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="6800a-109">Today, users have the ability to create callbacks using the `System.Delegate` type.</span></span> <span data-ttu-id="6800a-110">Jednak są one dość ciężki (na przykład wymaganie alokacji sterty i zawsze mają obsługiwać wywołania zwrotne w łańcuchu).</span><span class="sxs-lookup"><span data-stu-id="6800a-110">However, these are fairly heavyweight (such as requiring a heap allocation and always having handling for chaining callbacks together).</span></span>

<span data-ttu-id="6800a-111">Ponadto `System.Delegate` nie zapewnia najlepszej współdziałania z niezarządzanymi wskaźnikami funkcji, tj. nie danych kopiowalnych i wymagających organizowania w dowolnym momencie przekroczenia granic zarządzanych/niezarządzanych.</span><span class="sxs-lookup"><span data-stu-id="6800a-111">Additionally, `System.Delegate` does not provide the best interop with unmanaged function pointers, namely due being non-blittable and requiring marshalling anytime it crosses the managed/unmanaged boundary.</span></span>

<span data-ttu-id="6800a-112">W przypadku kilku drobnych dostosowań możemy udostępnić nowy typ delegata, który jest dobrze uproszczony, ogólnego przeznaczenia i współdziałania z kodem natywnym.</span><span class="sxs-lookup"><span data-stu-id="6800a-112">With a few minor tweaks, we could provide a new type of delegate that is lightweight, general-purpose, and interops well with native code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="6800a-113">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="6800a-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="6800a-114">Jeden z nich deklaruje delegata statycznego za pośrednictwem następujących:</span><span class="sxs-lookup"><span data-stu-id="6800a-114">One would declare a static delegate via the following:</span></span>

```C#
static delegate int Func()
```

<span data-ttu-id="6800a-115">Jedna z nich może dodatkowo przywoływać deklarację podobną do `System.Runtime.InteropServices.UnmanagedFunctionPointer`, tak aby można było kontrolować konwencję wywoływania, kierowanie ciągów i Ustawianie ostatniego błędu.</span><span class="sxs-lookup"><span data-stu-id="6800a-115">One could additionally attribute the declaration with something similar to `System.Runtime.InteropServices.UnmanagedFunctionPointer` so that the calling convention, string marshalling, and set last error behavior can be controlled.</span></span> <span data-ttu-id="6800a-116">Uwaga: użycie samego `System.Runtime.InteropServices.UnmanagedFunctionPointer` nie będzie działać, ponieważ jest ono dostępne tylko dla delegatów.</span><span class="sxs-lookup"><span data-stu-id="6800a-116">NOTE: Using `System.Runtime.InteropServices.UnmanagedFunctionPointer` itself will not work, as it is only usable on Delegates.</span></span>

<span data-ttu-id="6800a-117">Deklaracja zostanie przetłumaczona na wewnętrzną reprezentację przez kompilator, który jest podobny do poniższego</span><span class="sxs-lookup"><span data-stu-id="6800a-117">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

<span data-ttu-id="6800a-118">Oznacza to, że jest ona wewnętrznie reprezentowana przez strukturę, która ma pojedynczy element członkowski typu `IntPtr` (Taka struktura jest danych kopiowalnych i nie wiąże się z żadną alokacją sterty):</span><span class="sxs-lookup"><span data-stu-id="6800a-118">That is to say, it is internally represented by a struct that has a single member of type `IntPtr` (such a struct is blittable and does not incur any heap allocations):</span></span>
* <span data-ttu-id="6800a-119">Element członkowski zawiera adres funkcji, która ma być wywołaniem zwrotnym.</span><span class="sxs-lookup"><span data-stu-id="6800a-119">The member contains the address of the function that is to be the callback.</span></span>
* <span data-ttu-id="6800a-120">Typ deklaruje metodę pasującą do sygnatury metody wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="6800a-120">The type declares a method matching the method signature of the callback.</span></span>
* <span data-ttu-id="6800a-121">Nazwą struktury nie powinna być User-konstrukcyjną (podobnie jak w przypadku innych wewnętrznie wygenerowanych struktur zapasowych).</span><span class="sxs-lookup"><span data-stu-id="6800a-121">The name of the struct should not be user-constructible (as we do with other internally generated backing structures).</span></span>
 * <span data-ttu-id="6800a-122">Na przykład: bufory o ustalonym rozmiarze generują strukturę o nazwie w formacie `<name>e__FixedBuffer` (`<` i `>` są częścią identyfikatora i nie konstrukcyjną identyfikatora C#, ale nadal są dostępne w Il).</span><span class="sxs-lookup"><span data-stu-id="6800a-122">For example: fixed size buffers generate a struct with a name in the format of `<name>e__FixedBuffer` (`<` and `>` are part of the identifier and make the identifier not constructible in C#, but still useable in IL).</span></span>
* <span data-ttu-id="6800a-123">Nazwa deklaracji metody powinna być dobrze znaną nazwą używaną we wszystkich statycznych typach delegatów (pozwala to kompilatorowi znać nazwę do wyszukania podczas określania sygnatury).</span><span class="sxs-lookup"><span data-stu-id="6800a-123">The name of the method declaration should be a well known name used across all static delegate types (this allows the compiler to know the name to look for when determining the signature).</span></span>

<span data-ttu-id="6800a-124">Wartość delegata statycznego może być powiązana tylko z metodą statyczną zgodną z sygnaturą wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="6800a-124">The value of the static delegate can only be bound to a static method that matches the signature of the callback.</span></span>

<span data-ttu-id="6800a-125">Łańcuchowe wywołania zwrotne nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="6800a-125">Chaining callbacks together is not supported.</span></span>

<span data-ttu-id="6800a-126">Wywołanie wywołania zwrotnego byłoby implementowane przez instrukcję `calli`.</span><span class="sxs-lookup"><span data-stu-id="6800a-126">Invocation of the callback would be implemented by the `calli` instruction.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="6800a-127">Wady</span><span class="sxs-lookup"><span data-stu-id="6800a-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="6800a-128">Statyczne Delegaty nie będą współdziałać z istniejącymi interfejsami API, które używają zwykłych delegatów (jeden z nich musi obsłużyć ten statyczny delegat w zwykłym delegatze tej samej sygnatury).</span><span class="sxs-lookup"><span data-stu-id="6800a-128">Static Delegates would not work with existing APIs that use regular delegates (one would need to wrap said static delegate in a regular delegate of the same signature).</span></span>
* <span data-ttu-id="6800a-129">Uwzględniając, że `System.Delegate` jest reprezentowana wewnętrznie jako zbiór pól `object` i `IntPtr` (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), możliwe byłoby umożliwienie niejawnej konwersji delegata statycznego na `System.Delegate`, który ma pasującą sygnaturę metody.</span><span class="sxs-lookup"><span data-stu-id="6800a-129">Given that `System.Delegate` is represented internally as a set of `object` and `IntPtr` fields (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), it would be possible to allow implicit conversion of a static delegate to a `System.Delegate` that has a matching method signature.</span></span> <span data-ttu-id="6800a-130">Możliwe jest również dostarczenie jawnej konwersji w odwrotnym kierunku, pod warunkiem, że `System.Delegate` zgodne ze wszystkimi wymaganiami w przypadku delegatów statycznych.</span><span class="sxs-lookup"><span data-stu-id="6800a-130">It would also be possible to provide an explicit conversion in the opposite direction, provided the `System.Delegate` conformed to all the requirements of being a static delegate.</span></span>

<span data-ttu-id="6800a-131">Konieczne jest wykonanie dodatkowych czynności w celu udostępnienia statycznego delegata w środowisku podstawowym.</span><span class="sxs-lookup"><span data-stu-id="6800a-131">Additional work would be needed to make Static Delegate readily usable in the core framework.</span></span>

## <a name="alternatives"></a><span data-ttu-id="6800a-132">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="6800a-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="6800a-133">TBD</span><span class="sxs-lookup"><span data-stu-id="6800a-133">TBD</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="6800a-134">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="6800a-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="6800a-135">Jakie części projektu nadal są do opracowania?</span><span class="sxs-lookup"><span data-stu-id="6800a-135">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="6800a-136">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="6800a-136">Design meetings</span></span>

<span data-ttu-id="6800a-137">Połącz się z uwagami dotyczącymi projektu, które mają wpływ na tę propozycję, i opisz w jednym zdaniu, dla każdej zmiany, do której zostały one wprowadzone.</span><span class="sxs-lookup"><span data-stu-id="6800a-137">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>


