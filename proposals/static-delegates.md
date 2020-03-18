---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484476"
---
# <a name="static-delegates"></a>Elementy delegowane statyczne

* [x] proponowane
* [] Prototyp: nie uruchomiono
* [] Implementacja: nie uruchomiono
* [] Specyfikacja: nie uruchomiono

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Zapewnij ogólną, uproszczoną funkcję wywołania zwrotnego dla C# języka.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Obecnie użytkownicy mają możliwość tworzenia wywołań zwrotnych przy użyciu typu `System.Delegate`. Jednak są one dość ciężki (na przykład wymaganie alokacji sterty i zawsze mają obsługiwać wywołania zwrotne w łańcuchu).

Ponadto `System.Delegate` nie zapewnia najlepszej współdziałania z niezarządzanymi wskaźnikami funkcji, tj. nie danych kopiowalnych i wymagających organizowania w dowolnym momencie przekroczenia granic zarządzanych/niezarządzanych.

W przypadku kilku drobnych dostosowań możemy udostępnić nowy typ delegata, który jest dobrze uproszczony, ogólnego przeznaczenia i współdziałania z kodem natywnym.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Jeden z nich deklaruje delegata statycznego za pośrednictwem następujących:

```C#
static delegate int Func()
```

Jedna z nich może dodatkowo przywoływać deklarację podobną do `System.Runtime.InteropServices.UnmanagedFunctionPointer`, tak aby można było kontrolować konwencję wywoływania, kierowanie ciągów i Ustawianie ostatniego błędu. Uwaga: użycie samego `System.Runtime.InteropServices.UnmanagedFunctionPointer` nie będzie działać, ponieważ jest ono dostępne tylko dla delegatów.

Deklaracja zostanie przetłumaczona na wewnętrzną reprezentację przez kompilator, który jest podobny do poniższego

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

Oznacza to, że jest ona wewnętrznie reprezentowana przez strukturę, która ma pojedynczy element członkowski typu `IntPtr` (Taka struktura jest danych kopiowalnych i nie wiąże się z żadną alokacją sterty):
* Element członkowski zawiera adres funkcji, która ma być wywołaniem zwrotnym.
* Typ deklaruje metodę pasującą do sygnatury metody wywołania zwrotnego.
* Nazwą struktury nie powinna być User-konstrukcyjną (podobnie jak w przypadku innych wewnętrznie wygenerowanych struktur zapasowych).
 * Na przykład: bufory o ustalonym rozmiarze generują strukturę o nazwie w formacie `<name>e__FixedBuffer` (`<` i `>` są częścią identyfikatora i nie konstrukcyjną identyfikatora C#, ale nadal są dostępne w Il).
* Nazwa deklaracji metody powinna być dobrze znaną nazwą używaną we wszystkich statycznych typach delegatów (pozwala to kompilatorowi znać nazwę do wyszukania podczas określania sygnatury).

Wartość delegata statycznego może być powiązana tylko z metodą statyczną zgodną z sygnaturą wywołania zwrotnego.

Łańcuchowe wywołania zwrotne nie są obsługiwane.

Wywołanie wywołania zwrotnego byłoby implementowane przez instrukcję `calli`.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Statyczne Delegaty nie będą współdziałać z istniejącymi interfejsami API, które używają zwykłych delegatów (jeden z nich musi obsłużyć ten statyczny delegat w zwykłym delegatze tej samej sygnatury).
* Uwzględniając, że `System.Delegate` jest reprezentowana wewnętrznie jako zbiór pól `object` i `IntPtr` (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), możliwe byłoby umożliwienie niejawnej konwersji delegata statycznego na `System.Delegate`, który ma pasującą sygnaturę metody. Możliwe jest również dostarczenie jawnej konwersji w odwrotnym kierunku, pod warunkiem, że `System.Delegate` zgodne ze wszystkimi wymaganiami w przypadku delegatów statycznych.

Konieczne jest wykonanie dodatkowych czynności w celu udostępnienia statycznego delegata w środowisku podstawowym.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

TBD

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

Jakie części projektu nadal są do opracowania?

## <a name="design-meetings"></a>Spotkania projektowe

Połącz się z uwagami dotyczącymi projektu, które mają wpływ na tę propozycję, i opisz w jednym zdaniu, dla każdej zmiany, do której zostały one wprowadzone.


