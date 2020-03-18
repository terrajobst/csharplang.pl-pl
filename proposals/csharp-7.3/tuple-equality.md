---
ms.openlocfilehash: f238a711e710bbac7f5b7400fa938bd85dec00c6
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485253"
---
# <a name="support-for--and--on-tuple-types"></a>Obsługa funkcji = = i! = na typach krotek

Zezwalaj na wyrażenia `t1 == t2`, gdzie `t1` i `t2` są kolekcjami typu krotka lub wartość null tego samego kardynalności, i Oceń je w sposób niezależny od `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` (przy założeniu `var temp1 = t1; var temp2 = t2;`).

Z drugiej strony zezwolisz na `t1 != t2` i ocenimy ją jako `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`.

W przypadku dopuszczania wartości null są używane dodatkowe sprawdzenia dla `temp1.HasValue` i `temp2.HasValue`. Na przykład `nullableT1 == nullableT2` jest oceniane jako `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`.

Gdy wynikiem porównania elementów jest wynik nielogiczny (na przykład w przypadku, gdy użyto niebool `operator ==` lub `operator !=`, lub w porównaniu dynamicznej), ten wynik zostanie przekonwertowany na `bool` lub uruchamiany przez `operator true` lub `operator false`, aby uzyskać `bool`. Porównanie spójnej kolekcji zawsze jest zakończone zwróceniem `bool`.

Począwszy od C# 7,2, taki kod generuje błąd (`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`), chyba że istnieje `operator==`zdefiniowany przez użytkownika.

## <a name="details"></a>Szczegóły

W przypadku powiązania operatora `==` (lub `!=`) istniejące reguły są następujące: (1) dynamiczne przypadki, (2) rozwiązanie przeciążenia i (3) kończy się niepowodzeniem.
Ta propozycja dodaje przypadek krotki między (1) i (2): Jeśli oba operandy operatora porównania są krotki (mają typy krotek lub są literałami krotek) i mają zgodną Kardynalność, to porównanie jest wykonywane jako element. Równość tego krotki jest również podnoszenia do spójnych krotek.

Oba operandy (i, w przypadku literałów krotek, ich elementy) są oceniane w kolejności od lewej do prawej. Każda para elementów jest następnie używana jako operandy do powiązania operatora `==` (lub `!=`), rekursywnie. Wszystkie elementy z typem czasu kompilacji `dynamic` powodują wystąpienie błędu. Wyniki tych porównania elementów są używane jako operandy w łańcuchu operatorów warunkowych i (lub).

Na przykład w kontekście `(int, (int, int)) t1, t2;`, `t1 == (1, (2, 3))` oszacować jako `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`.

Gdy literał krotki jest używany jako operand (po obu stronach), otrzymuje przekonwertowany typ krotki utworzony przez konwersje elementów, które są wprowadzane podczas wiązania operatora `==` (lub `!=`) elementu. 

Na przykład, w `(1L, 2, "hello") == (1, 2L, null)`, konwertowany typ dla obu literałów krotek jest `(long, long, string)`, a drugi literał nie ma typu naturalnego.


### <a name="deconstruction-and-conversions-to-tuple"></a>Dekonstrukcja i konwersje do krotki
W `(a, b) == x`, fakt, że `x` może dekonstrukcja do dwóch elementów nie odgrywa roli. To może wystąpić w przyszłej propozycji, chociaż może to spowodować wygenerowanie pytań dotyczących `x == y` (to proste porównanie lub porównanie elementów, a jeśli tak, to przy użyciu jakiej kardynalności?).
Podobnie konwersje do krotki nie odgrywają żadnej roli.

### <a name="tuple-element-names"></a>Nazwy elementów krotki

Podczas konwertowania literału krotki ostrzegamy, gdy w literale podano jawną nazwę elementu krotki, ale nie pasuje ona do nazwy docelowej elementu krotki.
Używamy tej samej reguły w porównaniu z porównaniem krotek, więc przy założeniu, że `(int a, int b) t` będziemy ostrzegać `d` w `t == (c, d: 0)`.

### <a name="non-bool-element-wise-comparison-results"></a>Wyniki porównania elementów innych niż bool

Jeśli porównanie elementów jest dynamiczne w równości spójnej kolekcji, używamy dynamicznego wywołania operatora `false` i Negate, aby uzyskać `bool` i kontynuować dalsze porównania elementów. 

Jeśli porównanie elementów zwraca inny typ inny niż bool w równości spójnej kolekcji, istnieją dwa przypadki:
- Jeśli typ inny niż bool jest konwertowany na `bool`, stosowana jest ta konwersja,
- Jeśli nie istnieje taka konwersja, ale typ ma `false`operatora, użyjemy go i Negate wynik.

W przypadku nierówności krotek te same reguły są stosowane, z tą różnicą, że użyjemy operatora `true` (bez negacji) zamiast operatora `false`.

Reguły te są podobne do reguł związanych z używaniem typu niebool w instrukcji `if` i innych istniejących kontekstach.

## <a name="evaluation-order-and-special-cases"></a>Kolejność oceny i specjalne przypadki
Wartość po lewej stronie jest szacowana jako pierwsza, a następnie wartość po prawej stronie, a następnie porównania elementów od lewej do prawej (w tym konwersje i wczesne wyjście na podstawie istniejących reguł dla operatorów warunkowych i/lub).

Na przykład jeśli istnieje konwersja z typu `A` do typu `B` i `(A, A) GetTuple()`metody, Ocena `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` oznacza:
- `new A(1)`
- `new B(2)`
- `new B(3)`
- `new B(4)`
- `GetTuple()`
- następnie są oceniane konwersje i porównania elementów i logiki warunkowej (Konwertuj `new A(1)` na typ `B`, a następnie porównaj je z `new B(4)`itd.).

### <a name="comparing-null-to-null"></a>Porównywanie `null` z `null`

Jest to specjalny przypadek od zwykłych porównań, który przeprowadzi do porównania krotek. Porównanie `null == null` jest dozwolone, a literały `null` nie pobierają żadnego typu.
W przypadku równości krotek oznacza to, że `(0, null) == (0, null)` jest również dozwolone, a `null` i literały krotek nie pobierają typu.

### <a name="comparing-a-nullable-struct-to-null-without-operator"></a>Porównywanie struktury dopuszczającej wartości null do `null` bez `operator==`

Jest to inny specjalny przypadek od zwykłych porównań, który przeprowadzi do porównania krotek.
Jeśli masz `struct S` bez `operator==`, porównanie `(S?)x == null` jest dozwolone i jest interpretowane jako `((S?).x).HasValue`.
W przypadku równości krotek stosowana jest ta sama reguła, więc `(0, (S?)x) == (0, null)` jest dozwolony.

## <a name="compatibility"></a>Zgodność

Jeśli ktoś zapisał własne typy `ValueTuple` z implementacją operatora porównania, zostałby wcześniej pobrany przez rozpoznanie przeciążenia. Jednak od momentu, gdy nowa Krotka jest wcześniejsza niż Rozpoznanie przeciążenia, możemy obsłużyć ten przypadek z porównaniem spójnej kolekcji, zamiast polegać na porównaniu zdefiniowanej przez użytkownika.

----

Odnosi się do [operatorów relacyjnych i typów](../../spec/expressions.md#relational-and-type-testing-operators) odnoszących się do [#190](https://github.com/dotnet/csharplang/issues/190)
