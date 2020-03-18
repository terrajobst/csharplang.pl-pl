---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/22/2019
ms.locfileid: "79485085"
---
# <a name="static-lambdas"></a>Statyczne wyrażenia lambda

## <a name="summary"></a>Podsumowanie

Obsługa wyrażeń lambda, które nie umożliwiają przechwytywania stanu z otaczającego zakresu.

## <a name="motivation"></a>Motywacją

Unikaj przypadkowego przechwytywania stanu z otaczającego kontekstu.

## <a name="detailed-design"></a>Szczegółowy projekt

Wyrażenia lambda z `static` nie mogą przechwycić stanu z otaczającego zakresu.
W wyniku tego elementy lokalne, parametry i `this` z otaczającego zakresu nie są dostępne w `static` lambda.

`static` lambda nie może odwoływać się do składowych wystąpienia z niejawnego lub jawnego `this` lub odwołania `base`.

`static` lambda może odwoływać się do `static` członków z otaczającego zakresu.

`static` lambda może odwoływać się do definicji `constant` z otaczającego zakresu.

`nameof()` w `static` lambda może odwoływać się do zmiennych lokalnych, parametrów lub `this` lub `base` z otaczającego zakresu.

Reguły ułatwień dostępu dla elementów członkowskich `private` w otaczającym zakresie są takie same dla `static` i nie`static` wyrażeń lambda.

Żadna gwarancja nie jest podejmowana w przypadku, gdy `static` wyrażenie lambda jest emitowane jako metoda `static` w metadanych. Jest to pozostało do implementacji kompilatora do optymalizacji.

Funkcja lokalna nie`static` lub lambda może przechwytywać stan z otaczającego `static` lambda, ale nie może przechwycić stanu poza otaczającą `static` lambda.

`static` lambda można użyć w drzewie wyrażenia.

Usunięcie modyfikatora `static` z wyrażenia lambda w prawidłowym programie nie zmienia znaczenia programu.
