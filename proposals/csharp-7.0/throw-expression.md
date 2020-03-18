---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484546"
---
# <a name="throw-expression"></a>Wyrażenie throw

Rozszerzy zestaw formularzy wyrażeń do uwzględnienia

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

Reguły typu są następujące:

- *Throw_expression* nie ma typu.
- *Throw_expression* jest konwertowany na każdy typ przez niejawną konwersję.

*Wyrażenie throw* zwraca wartość wygenerowaną przez ocenę *null_coalescing_expression*, która musi określać wartość typu klasy `System.Exception`, klasy typu, która pochodzi od `System.Exception` lub typu parametru typu, który ma `System.Exception` (lub jego podklasę) jako obowiązującą klasę bazową. Jeśli Obliczanie wyrażenia powoduje `null`, zamiast niego zostanie zgłoszony `System.NullReferenceException`.

Zachowanie w czasie wykonywania obliczania *wyrażenia throw* jest takie samo, [jak określone dla *instrukcji throw*](../../spec/statements.md#the-throw-statement).

Reguły analizy przepływu są następujące:

- Dla każdej zmiennej *v*, *v* jest przede wszystkim przypisane przed *null_coalescing_expression* *throw_expression* Jeśli przed *throw_expression*.
- Dla każdej zmiennej *v*, *v* jest ostatecznie przypisana po *throw_expression*.

*Wyrażenie throw* jest dozwolone tylko w następujących kontekstach składni:
- Jako drugi lub trzeci operand operatora warunkowego Trzyelementowy `?:`
- Jako drugi operand operatora łączącego wartości null `??`
- Jako treść wyrażenia lambda lub metody.
