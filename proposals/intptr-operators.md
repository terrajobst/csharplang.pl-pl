---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484560"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a><span data-ttu-id="d764e-101">Operatory powinny być uwidocznione dla `System.IntPtr` i `System.UIntPtr`</span><span class="sxs-lookup"><span data-stu-id="d764e-101">Operators should be exposed for `System.IntPtr` and `System.UIntPtr`</span></span>

* <span data-ttu-id="d764e-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="d764e-102">[x] Proposed</span></span>
* <span data-ttu-id="d764e-103">[] Prototyp: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="d764e-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="d764e-104">[] Implementacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="d764e-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="d764e-105">[] Specyfikacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="d764e-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="d764e-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="d764e-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d764e-107">Środowisko CLR obsługuje zestaw operatorów dla typów `System.IntPtr` i `System.UIntPtr` (`native int`).</span><span class="sxs-lookup"><span data-stu-id="d764e-107">The CLR supports a set of operators for the `System.IntPtr` and `System.UIntPtr` types (`native int`).</span></span> <span data-ttu-id="d764e-108">Te operatory mogą być widoczne w `III.1.5` specyfikacji Common Language Infrastructure (`ECMA-335`).</span><span class="sxs-lookup"><span data-stu-id="d764e-108">These operators can be seen in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="d764e-109">Jednak te operatory nie są obsługiwane przez C#program.</span><span class="sxs-lookup"><span data-stu-id="d764e-109">However, these operators are not supported by C#.</span></span>

<span data-ttu-id="d764e-110">Obsługę języka należy zapewnić dla pełnego zestawu operatorów obsługiwanych przez `System.IntPtr` i `System.UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="d764e-110">Language support should be provided for the full set of operators supported by `System.IntPtr` and `System.UIntPtr`.</span></span> <span data-ttu-id="d764e-111">Te operatory: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span><span class="sxs-lookup"><span data-stu-id="d764e-111">These operators are: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span></span>

## <a name="motivation"></a><span data-ttu-id="d764e-112">Motywacją</span><span class="sxs-lookup"><span data-stu-id="d764e-112">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d764e-113">Obecnie użytkownicy mogą łatwo pisać C# aplikacje ukierunkowane na wiele platform przy użyciu różnych narzędzi i platform, takich jak `Xamarin`, `.NET Core`, `Mono`itd...</span><span class="sxs-lookup"><span data-stu-id="d764e-113">Today, users can easily write C# applications targeting multiple platforms using various tools and frameworks, such as: `Xamarin`, `.NET Core`, `Mono`, etc...</span></span>

<span data-ttu-id="d764e-114">Podczas pisania kodu dla wielu platform często konieczne jest zapisanie kodu międzyoperacyjnego, który współdziała z konkretną platformą docelową w określony sposób.</span><span class="sxs-lookup"><span data-stu-id="d764e-114">When writing cross-platform code, it is often necessary to write interop code that interacts with a particular target platform in a specific manner.</span></span> <span data-ttu-id="d764e-115">Może to obejmować pisanie kodu grafiki, wywoływanie pewnego interfejsu API systemu lub posługiwanie się istniejącą biblioteką natywną.</span><span class="sxs-lookup"><span data-stu-id="d764e-115">This could include writing graphics code, calling some System API, or interacting with an existing native library.</span></span>

<span data-ttu-id="d764e-116">Ten kod międzyoperacyjności często musi zająć się dojściami, niezarządzaną pamięcią, a nawet tylko liczbami całkowitymi specyficznymi dla platformy.</span><span class="sxs-lookup"><span data-stu-id="d764e-116">This interop code often has to deal with handles, unmanaged memory, or even just platform-specific sized integers.</span></span>

<span data-ttu-id="d764e-117">Środowisko uruchomieniowe zapewnia obsługę tego przez zdefiniowanie zestawu operatorów, które mogą być używane dla typów pierwotnych `native int` (`System.IntPtr`) i `native unsigned int` (`System.UIntPtr`).</span><span class="sxs-lookup"><span data-stu-id="d764e-117">The runtime provides support for this by defining a set of operators that can be used on the `native int` (`System.IntPtr`) and `native unsigned int` (`System.UIntPtr`) primitive types.</span></span>

<span data-ttu-id="d764e-118">C#te operatory nigdy nie były obsługiwane, więc użytkownicy muszą obejść problem.</span><span class="sxs-lookup"><span data-stu-id="d764e-118">C# has never supported these operators and so users have to work around the issue.</span></span> <span data-ttu-id="d764e-119">Często zwiększa to złożoność kodu i obniża łatwość utrzymania kodu.</span><span class="sxs-lookup"><span data-stu-id="d764e-119">This often increases code complexity and lowers code maintainability.</span></span>

<span data-ttu-id="d764e-120">W związku z tym język powinien zacząć obsługiwać te operatory, aby pomóc w zwiększeniu poziomu języka w celu lepszego wsparcia tych wymagań.</span><span class="sxs-lookup"><span data-stu-id="d764e-120">As such, the language should begin to support these operators to help advance the language to better support these requirements.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d764e-121">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="d764e-121">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="d764e-122">Pełny zestaw obsługiwanych operatorów jest zdefiniowany w `III.1.5` specyfikacji Common Language Infrastructure (`ECMA-335`).</span><span class="sxs-lookup"><span data-stu-id="d764e-122">The full set of operators supported are defined in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="d764e-123">Specyfikacja jest dostępna tutaj: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span><span class="sxs-lookup"><span data-stu-id="d764e-123">The specification is available here: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span></span>

* <span data-ttu-id="d764e-124">Poniżej przedstawiono podsumowanie operatorów.</span><span class="sxs-lookup"><span data-stu-id="d764e-124">A summary of the operators is provided below for convenience.</span></span>
* <span data-ttu-id="d764e-125">Operatory niemożliwe do zweryfikowania zdefiniowane przez specyfikację interfejsu wiersza polecenia nie są wymienione i nie są obecnie częścią tej propozycji (chociaż może to również być rozważane).</span><span class="sxs-lookup"><span data-stu-id="d764e-125">The unverifiable operators defined by the CLI spec are not listed and are not currently part of this proposal (although it may be worth considering these as well).</span></span>
* <span data-ttu-id="d764e-126">Dostarczenie słowa kluczowego (takiego jak `nint` i `nuint`) nie pozwala na deklarowanie literałów dla `System.IntPtr` i `System.UIntPtr` (takich jak 0n) nie jest częścią tej propozycji (chociaż może to być również konieczne).</span><span class="sxs-lookup"><span data-stu-id="d764e-126">Providing a keyword (such as `nint` and `nuint`) nor providing a way to for literals to be declared for `System.IntPtr` and `System.UIntPtr` (such as 0n) is not part of this proposal (although it may be worth considering these as well).</span></span>

### <a name="unary-plus-operator"></a><span data-ttu-id="d764e-127">Jednoargumentowy operator plus</span><span class="sxs-lookup"><span data-stu-id="d764e-127">Unary Plus Operator</span></span>

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a><span data-ttu-id="d764e-128">Jednoargumentowy operator minus</span><span class="sxs-lookup"><span data-stu-id="d764e-128">Unary Minus Operator</span></span>

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a><span data-ttu-id="d764e-129">Operator dopełnienia bitowego</span><span class="sxs-lookup"><span data-stu-id="d764e-129">Bitwise Complement Operator</span></span>

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a><span data-ttu-id="d764e-130">Operatory rzutowania</span><span class="sxs-lookup"><span data-stu-id="d764e-130">Cast Operators</span></span>

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a><span data-ttu-id="d764e-131">Operator mnożenia</span><span class="sxs-lookup"><span data-stu-id="d764e-131">Multiplication Operator</span></span>

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a><span data-ttu-id="d764e-132">Operator dzielenia</span><span class="sxs-lookup"><span data-stu-id="d764e-132">Division Operator</span></span>

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a><span data-ttu-id="d764e-133">Operator reszty</span><span class="sxs-lookup"><span data-stu-id="d764e-133">Remainder Operator</span></span>

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a><span data-ttu-id="d764e-134">Operator dodawania</span><span class="sxs-lookup"><span data-stu-id="d764e-134">Addition Operator</span></span>

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a><span data-ttu-id="d764e-135">Operator odejmowania</span><span class="sxs-lookup"><span data-stu-id="d764e-135">Subtraction Operator</span></span>

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a><span data-ttu-id="d764e-136">Operatory przesunięcia</span><span class="sxs-lookup"><span data-stu-id="d764e-136">Shift Operators</span></span>

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a><span data-ttu-id="d764e-137">Operatory porównywania liczb całkowitych</span><span class="sxs-lookup"><span data-stu-id="d764e-137">Integer Comparison Operators</span></span>

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a><span data-ttu-id="d764e-138">Operatory logiczne Integer</span><span class="sxs-lookup"><span data-stu-id="d764e-138">Integer Logical Operators</span></span>

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a><span data-ttu-id="d764e-139">Wady</span><span class="sxs-lookup"><span data-stu-id="d764e-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="d764e-140">Rzeczywiste użycie tych operatorów może być małe i ograniczone dla użytkowników końcowych, którzy piszą biblioteki niższego poziomu lub kod międzyoperacyjny.</span><span class="sxs-lookup"><span data-stu-id="d764e-140">The actual use of these operators may be small and limited to end-users who are writing lower level libraries or interop code.</span></span> <span data-ttu-id="d764e-141">Większość użytkowników końcowych prawdopodobnie zużywa te same biblioteki niższego poziomu, które byłyby niezależne od natywnych liczb całkowitych, dojść i kodu międzyoperacyjnego.</span><span class="sxs-lookup"><span data-stu-id="d764e-141">Most end-users would likely be consuming these lower level libraries themselves which would have the native sized integers, handles, and interop code abstracted away.</span></span> <span data-ttu-id="d764e-142">W związku z tym nie potrzebują samych operatorów.</span><span class="sxs-lookup"><span data-stu-id="d764e-142">As such, they would not have need of the operators themselves.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d764e-143">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="d764e-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="d764e-144">Czy struktura implementuje wymagane operatory przez zapisanie ich bezpośrednio w IL.</span><span class="sxs-lookup"><span data-stu-id="d764e-144">Have the framework implement the required operators by writing them directly in IL.</span></span> <span data-ttu-id="d764e-145">Ponadto środowisko uruchomieniowe może zapewnić wewnętrzną obsługę operatorów zdefiniowanych przez platformę, więc w celu lepszego zoptymalizowania wydajności końcowej.</span><span class="sxs-lookup"><span data-stu-id="d764e-145">Additionally, the runtime could provide intrinsic support for the operators defined by the framework, so as to better optimize the end performance.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d764e-146">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="d764e-146">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="d764e-147">Jakie części projektu nadal są do opracowania?</span><span class="sxs-lookup"><span data-stu-id="d764e-147">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="d764e-148">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="d764e-148">Design meetings</span></span>

<span data-ttu-id="d764e-149">Połącz się z uwagami dotyczącymi projektu, które mają wpływ na tę propozycję, i opisz w jednym zdaniu, dla każdej zmiany, do której zostały one wprowadzone.</span><span class="sxs-lookup"><span data-stu-id="d764e-149">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>