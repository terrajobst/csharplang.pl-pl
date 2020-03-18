---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484658"
---
# <a name="pattern-based-fixed-statement"></a><span data-ttu-id="4a5f7-101">Instrukcja `fixed` na podstawie wzorca</span><span class="sxs-lookup"><span data-stu-id="4a5f7-101">Pattern-based `fixed` statement</span></span>

## <a name="summary"></a><span data-ttu-id="4a5f7-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="4a5f7-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="4a5f7-103">Wprowadź wzorzec, który zezwoli, aby typy uczestniczyły w instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-103">Introduce a pattern that would allow types to participate in `fixed` statements.</span></span> 

## <a name="motivation"></a><span data-ttu-id="4a5f7-104">Motywacją</span><span class="sxs-lookup"><span data-stu-id="4a5f7-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="4a5f7-105">Język zapewnia mechanizm przypinania zarządzanych danych i uzyskiwania natywnego wskaźnika do bazowego buforu.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-105">The language provides a mechanism for pinning managed data and obtain a native pointer to the underlying buffer.</span></span>

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

<span data-ttu-id="4a5f7-106">Zestaw typów, które mogą uczestniczyć w `fixed` jest stałe i ograniczony do tablic i `System.String`.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-106">The set of types that can participate in `fixed` is hardcoded and limited to arrays and `System.String`.</span></span> <span data-ttu-id="4a5f7-107">Typy "Special" zakodowana nie są skalowane, gdy są wprowadzane nowe elementy podstawowe, takie jak `ImmutableArray<T>`, `Span<T>``Utf8String`.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-107">Hardcoding "special" types does not scale when new primitives such as `ImmutableArray<T>`, `Span<T>`, `Utf8String` are introduced.</span></span> 

<span data-ttu-id="4a5f7-108">Ponadto bieżące rozwiązanie dla `System.String` opiera się na dość sztywnym interfejsie API.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-108">In addition, the current solution for `System.String` relies on a fairly rigid API.</span></span> <span data-ttu-id="4a5f7-109">Kształt interfejsu API oznacza, że `System.String` jest obiektem ciągłym, który osadza dane zakodowane UTF16 w ustalonym przesunięciu z nagłówka obiektu.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-109">The shape of the API implies that `System.String` is a contiguous object that embeds UTF16 encoded data at a fixed offset from the object header.</span></span> <span data-ttu-id="4a5f7-110">Takie podejście znalazło problemy w kilku propozycjach, które mogą wymagać zmian w układzie bazowym.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-110">Such approach has been found problematic in several proposals that could require changes to the underlying layout.</span></span> <span data-ttu-id="4a5f7-111">Może być możliwe przełączenie się do bardziej elastycznego, który oddzieli `System.String` obiektu od jego wewnętrznej reprezentacji na potrzeby niezarządzanej międzyoperacyjności.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-111">It would be desirable to be able to switch to something more flexible that decouples `System.String` object from its internal representation for the purpose of unmanaged interop.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="4a5f7-112">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="4a5f7-112">Detailed design</span></span>
[design]: #detailed-design

## <a name="pattern"></a><span data-ttu-id="4a5f7-113">*Wzorzec*</span><span class="sxs-lookup"><span data-stu-id="4a5f7-113">*Pattern*</span></span> ##
<span data-ttu-id="4a5f7-114">"Ustalony" "stały" oparty na wzorcu musi:</span><span class="sxs-lookup"><span data-stu-id="4a5f7-114">A viable pattern-based “fixed” need to:</span></span>
-   <span data-ttu-id="4a5f7-115">Podaj odwołania zarządzane, aby przypiąć wystąpienie i zainicjować wskaźnik (najlepiej to jest to samo odwołanie)</span><span class="sxs-lookup"><span data-stu-id="4a5f7-115">Provide the managed references to pin the instance and to initialize the pointer (preferably this is the same reference)</span></span>
-   <span data-ttu-id="4a5f7-116">Przekazywanie jednoznacznie typu niezarządzanego elementu (tj. "char" dla "String")</span><span class="sxs-lookup"><span data-stu-id="4a5f7-116">Convey unambiguously the type of the unmanaged element   (i.e. “char” for “string”)</span></span>
-   <span data-ttu-id="4a5f7-117">Należy określić zachowanie w przypadku wartości pustej, jeśli nie ma niczego do odwołania.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-117">Prescribe the behavior in "empty" case when there is nothing to refer to.</span></span> 
-   <span data-ttu-id="4a5f7-118">Nie powinno wypychania autorów interfejsu API do decyzji projektowych, które pogarszają użycie typu poza `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-118">Should not push API authors toward design decisions that hurt the use of the type outside of `fixed`.</span></span>

<span data-ttu-id="4a5f7-119">Uważam, że powyższe może być spełnione poprzez rozpoznanie specjalnie nazwanego elementu członkowskiego zwracającego odwołanie: `ref [readonly] T GetPinnableReference()`.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-119">I think the above could be satisfied by recognizing a specially named ref-returning member: `ref [readonly] T GetPinnableReference()`.</span></span>

<span data-ttu-id="4a5f7-120">Aby można było używać instrukcji `fixed`, muszą być spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="4a5f7-120">In order to be used by the `fixed` statement the following conditions must be met:</span></span>

1. <span data-ttu-id="4a5f7-121">Dla tego typu jest dostarczany tylko jeden element członkowski.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-121">There is only one such member provided for a type.</span></span>
1. <span data-ttu-id="4a5f7-122">Zwraca przez `ref` lub `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-122">Returns by `ref` or `ref readonly`.</span></span> <span data-ttu-id="4a5f7-123">(`readonly` jest dozwolone, aby autorzy typów niezmiennych/tylko do odczytu mogli zaimplementować wzorzec bez dodawania zapisywalnego interfejsu API, który może być używany w bezpiecznym kodzie)</span><span class="sxs-lookup"><span data-stu-id="4a5f7-123">(`readonly` is permitted so that authors of immutable/readonly types could implement the pattern without adding writeable API that could be used in safe code)</span></span>
1. <span data-ttu-id="4a5f7-124">T jest typem niezarządzanym.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-124">T is an unmanaged type.</span></span>
<span data-ttu-id="4a5f7-125">(ponieważ `T*` jest typem wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-125">(since `T*` becomes the pointer type.</span></span> <span data-ttu-id="4a5f7-126">Ograniczenie zostanie naturalnie rozwinięte, jeśli/gdy pojęcie "niezarządzane" jest rozwinięte)</span><span class="sxs-lookup"><span data-stu-id="4a5f7-126">The restriction will naturally expand if/when the notion of "unmanaged" is expanded)</span></span>
1. <span data-ttu-id="4a5f7-127">Zwraca zarządzane `nullptr`, gdy nie ma żadnych danych do przypięcia — prawdopodobnie najtańszym sposobem przekazania opróżniania.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-127">Returns managed `nullptr` when there is no data to pin – probably the cheapest way to convey emptiness.</span></span>
<span data-ttu-id="4a5f7-128">(należy zauważyć, że "" ciąg zwraca odwołanie do ' \ 0 ', ponieważ ciągi są zakończone null)</span><span class="sxs-lookup"><span data-stu-id="4a5f7-128">(note that “” string returns a ref to '\0' since strings are null-terminated)</span></span>

<span data-ttu-id="4a5f7-129">Alternatywnie, `#3` możemy zezwolić na wynik w pustych przypadkach jako niezdefiniowane lub specyficzne dla implementacji.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-129">Alternatively for the `#3` we can allow the result in empty cases be undefined or implementation-specific.</span></span> <span data-ttu-id="4a5f7-130">Jednak może to spowodować, że interfejs API będzie bardziej niebezpieczny i podatny na nadużycia i niezamierzone obciążenia zgodności.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-130">That, however, may make the API more dangerous and prone to abuse and unintended compatibility burdens.</span></span> 

## <a name="translation"></a><span data-ttu-id="4a5f7-131">*Tłumaczenie*</span><span class="sxs-lookup"><span data-stu-id="4a5f7-131">*Translation*</span></span> ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

<span data-ttu-id="4a5f7-132">staną się następujące pseudokodzie (nie wszystkie można wyrazić elementu w C#)</span><span class="sxs-lookup"><span data-stu-id="4a5f7-132">becomes the following pseudocode (not all expressible in C#)</span></span>

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a><span data-ttu-id="4a5f7-133">Wady</span><span class="sxs-lookup"><span data-stu-id="4a5f7-133">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="4a5f7-134">GetPinnableReference jest przeznaczona do użycia tylko w `fixed`, ale nic nie pozwala na korzystanie z niego w bezpiecznym kodzie, dlatego implementacja musi mieć na uwadze.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-134">GetPinnableReference is intended to be used only in `fixed`, but nothing prevents its use in safe code, so implementor must keep that in mind.</span></span>

## <a name="alternatives"></a><span data-ttu-id="4a5f7-135">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="4a5f7-135">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="4a5f7-136">Użytkownicy mogą wprowadzać GetPinnableReference lub podobną składową i używać jej jako</span><span class="sxs-lookup"><span data-stu-id="4a5f7-136">Users can introduce GetPinnableReference or similar member and use it as</span></span>
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

<span data-ttu-id="4a5f7-137">Nie ma rozwiązania dla `System.String` w przypadku pożądanego rozwiązania alternatywnego.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-137">There is no solution for `System.String` if alternative solution is desired.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="4a5f7-138">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="4a5f7-138">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="4a5f7-139">[] Zachowanie w stanie "pusty".</span><span class="sxs-lookup"><span data-stu-id="4a5f7-139">[ ] Behavior in "empty" state.</span></span><span data-ttu-id="4a5f7-140">`nullptr`  - lub `undefined`?</span><span class="sxs-lookup"><span data-stu-id="4a5f7-140"> - `nullptr` or `undefined` ?</span></span> 
- <span data-ttu-id="4a5f7-141">[] Czy należy uwzględnić metody rozszerzające?</span><span class="sxs-lookup"><span data-stu-id="4a5f7-141">[ ] Should the extension methods be considered ?</span></span> 
- <span data-ttu-id="4a5f7-142">[] Jeśli w `System.String`zostanie wykryty wzorzec, czy powinien on zostać przegrany?</span><span class="sxs-lookup"><span data-stu-id="4a5f7-142">[ ] If a pattern is detected on `System.String`, should it win over ?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="4a5f7-143">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="4a5f7-143">Design meetings</span></span>

<span data-ttu-id="4a5f7-144">Jeszcze nie.</span><span class="sxs-lookup"><span data-stu-id="4a5f7-144">None yet.</span></span> 
