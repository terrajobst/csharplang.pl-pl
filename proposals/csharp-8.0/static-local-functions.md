---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2019
ms.locfileid: "79485099"
---
# <a name="static-local-functions"></a>statyczne funkcje lokalne

## <a name="summary"></a>Podsumowanie

Obsługa funkcji lokalnych, które nie zezwalają na przechwytywanie stanu z otaczającego zakresu.

## <a name="motivation"></a>Motywacją

Unikaj przypadkowego przechwytywania stanu z otaczającego kontekstu.
Zezwalaj na używanie funkcji lokalnych w scenariuszach, w których jest wymagana Metoda `static`.

## <a name="detailed-design"></a>Szczegółowy projekt

Funkcja lokalna zadeklarowana `static` nie może przechwycić stanu z otaczającego zakresu.
W wyniku tego elementy lokalne, parametry i `this` z otaczającego zakresu nie są dostępne w `static` funkcji lokalnej.

`static` funkcja lokalna nie może odwoływać się do członków wystąpienia z niejawnego lub jawnego `this` lub odwołania `base`.

Funkcja lokalna `static` może odwoływać się do `static` członków z otaczającego zakresu.

Funkcja lokalna `static` może odwoływać się do `constant` definicji z zakresu otaczającego.

`nameof()` w lokalnej funkcji `static` może odwoływać się do zmiennych lokalnych, parametrów lub `this` lub `base` z otaczającego zakresu.

Reguły ułatwień dostępu dla elementów członkowskich `private` w otaczającym zakresie są takie same dla `static` i nie`static`ych funkcji lokalnych.

`static` definicję funkcji lokalnej są emitowane jako metody `static` w metadanych, nawet jeśli są używane tylko w delegatze.

Funkcja lokalna nie`static` lub lambda może przechwycić stan z otaczającej `static` funkcji lokalnej, ale nie może przechwycić stanu poza otaczającą `static` funkcją lokalną.

Nie można wywołać funkcji lokalnej `static` w drzewie wyrażenia.

Wywołanie funkcji lokalnej jest emitowane jako `call`, a nie `callvirt`, niezależnie od tego, czy funkcja lokalna jest `static`.

Bez względu na to, czy funkcja lokalna jest `static`.

Usunięcie modyfikatora `static` z funkcji lokalnej w prawidłowym programie nie zmienia znaczenia programu.

## <a name="design-meetings"></a>Spotkania projektowe

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
