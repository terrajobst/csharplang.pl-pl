---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484560"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a>Operatory powinny być uwidocznione dla `System.IntPtr` i `System.UIntPtr`

* [x] proponowane
* [] Prototyp: nie uruchomiono
* [] Implementacja: nie uruchomiono
* [] Specyfikacja: nie uruchomiono

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Środowisko CLR obsługuje zestaw operatorów dla typów `System.IntPtr` i `System.UIntPtr` (`native int`). Te operatory mogą być widoczne w `III.1.5` specyfikacji Common Language Infrastructure (`ECMA-335`). Jednak te operatory nie są obsługiwane przez C#program.

Obsługę języka należy zapewnić dla pełnego zestawu operatorów obsługiwanych przez `System.IntPtr` i `System.UIntPtr`. Te operatory: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Obecnie użytkownicy mogą łatwo pisać C# aplikacje ukierunkowane na wiele platform przy użyciu różnych narzędzi i platform, takich jak `Xamarin`, `.NET Core`, `Mono`itd...

Podczas pisania kodu dla wielu platform często konieczne jest zapisanie kodu międzyoperacyjnego, który współdziała z konkretną platformą docelową w określony sposób. Może to obejmować pisanie kodu grafiki, wywoływanie pewnego interfejsu API systemu lub posługiwanie się istniejącą biblioteką natywną.

Ten kod międzyoperacyjności często musi zająć się dojściami, niezarządzaną pamięcią, a nawet tylko liczbami całkowitymi specyficznymi dla platformy.

Środowisko uruchomieniowe zapewnia obsługę tego przez zdefiniowanie zestawu operatorów, które mogą być używane dla typów pierwotnych `native int` (`System.IntPtr`) i `native unsigned int` (`System.UIntPtr`).

C#te operatory nigdy nie były obsługiwane, więc użytkownicy muszą obejść problem. Często zwiększa to złożoność kodu i obniża łatwość utrzymania kodu.

W związku z tym język powinien zacząć obsługiwać te operatory, aby pomóc w zwiększeniu poziomu języka w celu lepszego wsparcia tych wymagań.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Pełny zestaw obsługiwanych operatorów jest zdefiniowany w `III.1.5` specyfikacji Common Language Infrastructure (`ECMA-335`). Specyfikacja jest dostępna tutaj: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).

* Poniżej przedstawiono podsumowanie operatorów.
* Operatory niemożliwe do zweryfikowania zdefiniowane przez specyfikację interfejsu wiersza polecenia nie są wymienione i nie są obecnie częścią tej propozycji (chociaż może to również być rozważane).
* Dostarczenie słowa kluczowego (takiego jak `nint` i `nuint`) nie pozwala na deklarowanie literałów dla `System.IntPtr` i `System.UIntPtr` (takich jak 0n) nie jest częścią tej propozycji (chociaż może to być również konieczne).

### <a name="unary-plus-operator"></a>Jednoargumentowy operator plus

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a>Jednoargumentowy operator minus

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a>Operator dopełnienia bitowego

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a>Operatory rzutowania

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

### <a name="multiplication-operator"></a>Operator mnożenia

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

### <a name="division-operator"></a>Operator dzielenia

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

### <a name="remainder-operator"></a>Operator reszty

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

### <a name="addition-operator"></a>Operator dodawania

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

### <a name="subtraction-operator"></a>Operator odejmowania

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

### <a name="shift-operators"></a>Operatory przesunięcia

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a>Operatory porównywania liczb całkowitych

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

### <a name="integer-logical-operators"></a>Operatory logiczne Integer

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

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Rzeczywiste użycie tych operatorów może być małe i ograniczone dla użytkowników końcowych, którzy piszą biblioteki niższego poziomu lub kod międzyoperacyjny. Większość użytkowników końcowych prawdopodobnie zużywa te same biblioteki niższego poziomu, które byłyby niezależne od natywnych liczb całkowitych, dojść i kodu międzyoperacyjnego. W związku z tym nie potrzebują samych operatorów.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Czy struktura implementuje wymagane operatory przez zapisanie ich bezpośrednio w IL. Ponadto środowisko uruchomieniowe może zapewnić wewnętrzną obsługę operatorów zdefiniowanych przez platformę, więc w celu lepszego zoptymalizowania wydajności końcowej.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

Jakie części projektu nadal są do opracowania?

## <a name="design-meetings"></a>Spotkania projektowe

Połącz się z uwagami dotyczącymi projektu, które mają wpływ na tę propozycję, i opisz w jednym zdaniu, dla każdej zmiany, do której zostały one wprowadzone.