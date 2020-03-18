---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484588"
---
# <a name="fixed-sized-buffers"></a>Bufory o ustalonym rozmiarze

* [x] proponowane
* [] Prototyp: nie uruchomiono
* [] Implementacja: nie uruchomiono
* [] Specyfikacja: nie uruchomiono

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Zapewnienie ogólnego przeznaczenia i bezpiecznego mechanizmu deklarowania buforów o stałym rozmiarze do C# języka.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Obecnie użytkownicy mają możliwość tworzenia buforów o ustalonym rozmiarze w niebezpiecznym kontekście. Jednak wymaga to, aby użytkownik mógł poradzić sobie ze wskaźnikami, ręcznie wykonywać powiązane sprawdzenia i obsługuje tylko ograniczony zestaw typów (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`i `double`).

Najbardziej typową skargą jest to, że bufory o ustalonym rozmiarze nie mogą być indeksowane w bezpiecznym kodzie. Brak możliwości użycia większej liczby typów jest drugi.

W przypadku kilku drobnych dostosowań możemy udostępnić bufory o stałym rozmiarze, które obsługują dowolny typ, mogą być używane w bezpiecznym kontekście i mieć wykonywane automatyczne sprawdzanie powiązań.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Jeden z nich deklaruje bezpieczny bufor o stałym rozmiarze przy użyciu następujących funkcji:

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

Deklaracja zostanie przetłumaczona na wewnętrzną reprezentację przez kompilator, który jest podobny do poniższego

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

Ponieważ takie bufory o ustalonym rozmiarze nie wymagają już używania `fixed`, warto zezwolić na każdy typ elementu.  

> Uwaga: `fixed` nadal będą obsługiwane, ale tylko wtedy, gdy typ elementu to `blittable`

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

* Mogą występować pewne wyzwania z poprzednią zgodnością, ale ponieważ istniejące bufory o ustalonym rozmiarze działają tylko z wybranymi typami pierwotnymi, powinno być możliwe, aby kompilator kontynuował "działające w trybie" w przypadku, gdy użytkownik traktuje bufor stały jako przytrzymaj.
* Niezgodne konstrukcje mogą wymagać użycia nieco innego kodowania `v2`, aby ukryć pola ze starego kompilatora.
* Pakowanie nie jest dobrze zdefiniowane w specyfikacji IL dla typów ogólnych. Mimo że podejście powinno działać, zostanie ono obramowane w przypadku nieudokumentowanych zachowań. Należy wprowadzić tę udokumentowaną i upewnić się, że inne JITs, takie jak mono, mają takie samo zachowanie.
* Określenie oddzielnego typu dla każdej długości (prawdopodobnie innej dla `readonly` pól, jeśli jest obsługiwana) będzie miało wpływ na metadane. Będzie ono powiązane z liczbą tablic różnych rozmiarów w danej aplikacji.
* `ref` Math nie jest formalnie możliwa do zweryfikowania (ponieważ jest niebezpieczna). Będziemy musieli znaleźć sposób aktualizacji reguł weryfikacji, aby wiedzieć, że nasze użycie jest prawidłowe.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Ręcznie Zadeklaruj struktury i użyj niebezpiecznego kodu do skonstruowania indeksatorów.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

- Czy zezwalamy na `readonly`?  (z indeksatorem ReadOnly)
- Czy zezwalamy na inicjatory tablicy?
- Czy `fixed` jest wymagane słowo kluczowe?
- `foreach`?
- tylko pola wystąpienia w strukturach?

## <a name="design-meetings"></a>Spotkania projektowe

Połącz się z uwagami dotyczącymi projektu, które mają wpływ na tę propozycję, i opisz w jednym zdaniu, dla każdej zmiany, do której zostały one wprowadzone.