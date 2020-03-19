---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509508"
---
# <a name="attributes-on-local-functions"></a>Atrybuty funkcji lokalnych

## <a name="attributes"></a>Atrybuty

Deklaracje funkcji lokalnych mogą teraz mieć [atrybuty](../spec/attributes.md). Parametry i parametry typu w funkcjach lokalnych mogą mieć atrybuty.

Atrybuty o określonym znaczeniu w przypadku zastosowania do metody, jej parametrów lub parametrów typu będą miały takie samo znaczenie, gdy są stosowane do funkcji lokalnej, jej parametrów lub parametrów typu.

Funkcja lokalna może być taka sama w tym samym sensie jak [Metoda warunkowa](../spec/attributes.md#the-conditional-attribute) , dekorowania nazwy ją z `[ConditionalAttribute]`. Należy również `static`warunkowej funkcji lokalnej. Wszystkie ograniczenia dotyczące metod warunkowych stosują się również do warunkowych funkcji lokalnych, w tym, że typem zwracanym musi być `void`.

## <a name="extern"></a>Modyfikator

Modyfikator `extern` jest teraz dozwolony dla funkcji lokalnych. Dzięki temu funkcja lokalna jest zewnętrzna w tym samym sensie co [Metoda zewnętrzna](../spec/classes.md#external-methods).

Podobnie jak w przypadku metody zewnętrznej, *treść funkcji lokalnej* zewnętrznej funkcji lokalnej musi być średnikiem. *Treść funkcji lokalnej* z średnikami jest dozwolona tylko w zewnętrznej funkcji lokalnej. 

Należy również `static`zewnętrzną funkcję lokalną.

## <a name="syntax"></a>Składnia

[Gramatyka funkcji lokalnych](csharp-7.0/local-functions.md#syntax-grammar) jest modyfikowana w następujący sposób:
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
