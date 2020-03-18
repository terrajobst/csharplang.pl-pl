---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484707"
---
# <a name="pattern-matching-with-generics"></a>dopasowanie wzorca z typami ogólnymi

* [x] proponowane
* [] Prototyp:
* [] Implementacja:
* [] Specyfikacja:

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Specyfikacja dla [operatora exist C# as](../../spec/expressions.md#the-as-operator) pozwala na brak konwersji między typem operandu i określonym typem, gdy jest typem otwartym. Jednak w C# 7 wzorzec `Type identifier` wymaga konwersji między typem danych wejściowych a danym typem.

Firma Microsoft zaleca, aby przede wszystkim zmienić `expression is Type identifier`, a także zezwolić na warunki, gdy jest to dozwolone w C# 7, w przypadku, gdy `expression as Type` byłyby dozwolone. W każdym przypadku nowe przypadki są sytuacje, w których typ wyrażenia lub określony typ jest typem otwartym. 

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Przypadki, w których dopasowanie do wzorca powinno "oczywiście", nie można obecnie skompilować. Zobacz na przykład https://github.com/dotnet/roslyn/issues/16195.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Zmienimy akapit we specyfikacji dopasowania do wzorca (proponowane dodanie jest pogrubienie):

> Niektóre kombinacje typu statycznego po lewej stronie i danego typu są uznawane za niezgodne i powodują błąd czasu kompilacji. Wartość typu statycznego `E` jest uznawana za *wzorzec zgodną* z typem `T`, jeśli istnieje konwersja tożsamości, niejawna konwersja odwołania, konwersja z opakowania, jawna konwersja odwołania lub konwersja rozpakowywania z `E` do `T`**lub jeśli zarówno `E`, jak i `T` jest typem otwartym**. Jest to błąd czasu kompilacji, jeśli wyrażenie typu `E` nie jest zgodne ze wzorcem typu w wzorcu typów, z którym jest on dopasowany.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Brak.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Brak.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

Brak.

## <a name="design-meetings"></a>Spotkania projektowe

Menedżer LDM uznał to pytanie i odczuwał to zmianę poziomu błędu. Jest ona traktowana jako oddzielna funkcja języka, ponieważ po wprowadzeniu zmiany po wydaniu języka wprowadzono niezgodność do przodu. Użycie proponowanej zmiany wymaga, aby programista określił wersję języka 7,1.
