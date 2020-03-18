---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484924"
---
# <a name="expression-variables-in-initializers"></a>Zmienne wyrażeń w inicjatorach

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Rozszerzono funkcje wprowadzone w C# 7, aby zezwolić na wyrażenia zawierające zmienne wyrażeń (out deklaracji zmiennych i wzorców deklaracji) w inicjatorach pól, inicjatorach właściwości, elemencie ctor i klauzulach zapytania.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Spowoduje to zakończenie kilku niepełnych krawędzi w języku, C# z powodu braku czasu.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Firma Microsoft usuwa ograniczenie uniemożliwiające deklarację zmiennych wyrażeń (deklaracji zmiennych out i wzorców deklaracji) w inicjatorze ctor. Taka zadeklarowana zmienna znajduje się w zakresie w całej treści konstruktora.

Firma Microsoft usuwa ograniczenie uniemożliwiające deklarację zmiennych wyrażeń (deklaracji zmiennych out i wzorców deklaracji) w inicjatorze pola lub właściwości. Taka zadeklarowana zmienna znajduje się w zakresie w całym wyrażeniu inicjującym.

Firma Microsoft usuwa ograniczenie uniemożliwiające deklarację zmiennych wyrażeń (deklaracji zmiennych out i wzorców deklaracji) w klauzuli wyrażenia zapytania, które są tłumaczone na treść lambda. Taka zadeklarowana zmienna znajduje się w zakresie w tym wyrażeniu klauzuli zapytania.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Brak.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Odpowiedni zakres zmiennych wyrażeń zadeklarowanych w tych kontekstach nie jest oczywisty i zachowuje dalsze dyskusje LDM.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

- [] Jakie są odpowiednie zakresy dla tych zmiennych?

## <a name="design-meetings"></a>Spotkania projektowe

Brak.
