---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/11/2019
ms.locfileid: "79484959"
---
# <a name="null-coalescing-assignment"></a>przypisanie łączące o wartości null

* [x] proponowane
* [] Prototyp: nie uruchomiono
* [] Implementacja: nie uruchomiono
* [] Specyfikacja: poniżej

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Upraszcza typowy wzorzec kodowania, gdzie zmienna ma przypisaną wartość, jeśli jest równa null.

W ramach tej propozycji zostanie również pożądany typ wymagania dotyczące `??`, aby zezwolić na wyrażenie, którego typem jest nieograniczonego parametru typu, który będzie używany po lewej stronie.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Często widać kod w formularzu

```csharp
if (variable == null)
{
    variable = expression;
}
```

Ta propozycja dodaje operator binarny, którego nie można przeładować do języka, który wykonuje tę funkcję.

Dla tej funkcji wprowadzono co najmniej osiem oddzielnych żądań społeczności.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Dodamy nową formę operatora przypisania

``` antlr
assignment_operator
    : '??='
    ;
```

Które są zgodne z [istniejącymi regułami semantycznymi operatorów przypisania złożonego](../../spec/expressions.md#compound-assignment), z tą różnicą, że Elide przypisanie, jeśli lewa strona jest inna niż null. Reguły dla tej funkcji są następujące.

Podano `a ??= b`, gdzie `A` jest typem `a``B` jest typem `b`, a `A0` jest typem podstawowym `A`, jeśli `A` jest typem wartości null:

1. Jeśli `A` nie istnieje lub jest typem wartości niedopuszczających wartości null, wystąpi błąd w czasie kompilacji.
2. Jeśli `B` nie można przekonwertować niejawnie na `A` lub `A0` (jeśli `A0` istnieje), wystąpi błąd w czasie kompilacji.
3. Jeśli `A0` istnieje i `B` jest niejawnie konwertowany na `A0`, a `B` nie jest dynamiczna, typ `a ??= b` jest `A0`. `a ??= b` jest oceniane w czasie wykonywania jako:
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   z tą różnicą, że `a` są oceniane tylko raz.
4. W przeciwnym razie typ `a ??= b` jest `A`. `a ??= b` jest oceniane w czasie wykonywania jako `a ?? (a = b)`, z tą różnicą, że `a` jest obliczana tylko raz.


Aby wymusić wymagania dotyczące typu `??`, aktualizujemy specyfikację, w której obecnie stwierdza, że dane `a ?? b`, gdzie `A` jest typem `a`:

> 1. Jeśli istnieje i nie jest typem dopuszczającym wartość null lub typem referencyjnym, wystąpi błąd w czasie kompilacji.

To wymaganie należy do:

1. Jeśli istnieje i ma typ wartości niedopuszczający wartości null, wystąpi błąd w czasie kompilacji.

Dzięki temu operator łączenia o wartości null może współdziałać z niedozwolonymi parametrami typu, ponieważ parametr typu nieograniczonego T istnieje, nie jest typem dopuszczającym wartość null i nie jest typem referencyjnym.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Podobnie jak w przypadku dowolnej funkcji języka należy zwrócić uwagę na to, czy dodatkowa złożoność tego języka jest zapłacona w dodatkowym przejrzystości oferowanym C# przez funkcję.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Programista może ręcznie pisać `(x = x ?? y)`, `if (x == null) x = y;`lub `x ?? (x = y)`.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

- [] Wymaga przeglądu LDM
- [] Czy obsługujemy również `&&=` i operatory `||=`?

## <a name="design-meetings"></a>Spotkania projektowe

Brak.
