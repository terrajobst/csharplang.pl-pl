---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484441"
---
# <a name="local-functions"></a>Funkcje lokalne

Rozszerzono C# obsługę deklaracji funkcji w zakresie bloku. Funkcje lokalne mogą używać zmiennych (przechwytywania) z otaczającego zakresu.

Kompilator używa analizy przepływu, aby wykryć zmienne, których używa funkcja lokalna przed przypisaniem do niej wartości. Każde wywołanie funkcji wymaga, aby zmienne były ostatecznie przypisane. Podobnie kompilator określa, które zmienne są ostatecznie przypisane do powrotu. Takie zmienne są uważane za ostatecznie przypisane po wywołaniu funkcji lokalnej.

Funkcje lokalne mogą być wywoływane z punktu leksykalnego przed jego definicją. Instrukcje deklaracji funkcji lokalnych nie powodują ostrzeżenia, gdy nie są dostępne.

do zrobienia: _zapis specyfikacji_

## <a name="syntax-grammar"></a>Gramatyka składni

Ta Gramatyka jest reprezentowana jako różnica w stosunku do bieżącej gramatyki specyfikacji.

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

Funkcje lokalne mogą używać zmiennych zdefiniowanych w zakresie otaczającym. Bieżąca implementacja wymaga, aby każda zmienna odczytana wewnątrz funkcji lokalnej była ostatecznie przypisana, tak jakby wykonywała funkcję lokalną w punkcie definicji. Ponadto definicja funkcji lokalnej musi być "wykonywana" w dowolnym punkcie użycia.

Po przeprowadzeniu eksperymentowania z tym bitem (na przykład nie jest możliwe zdefiniowanie dwóch wzajemnie rekurencyjnych funkcji lokalnych), od momentu skorygowania, jak chcemy, aby określone przypisanie działało. Poprawka (jeszcze nie zaimplementowana) polega na tym, że wszystkie zmienne lokalne odczytywane w funkcji lokalnej muszą być ostatecznie przypisane do każdego wywołania funkcji lokalnej. Jest to naprawdę bardziej delikatne niż dźwięki IT i istnieje wiele zadań, które pozostało do działania. Gdy wszystko będzie gotowe, będzie można przenieść funkcje lokalne na koniec otaczającego go bloku.

Nowe określone reguły przypisywania są niezgodne z wywnioskowania zwracanego typu funkcji lokalnej, więc prawdopodobnie zostanie usunięta pomoc techniczna do wywnioskowania typu zwracanego.

Jeśli funkcja lokalna nie zostanie przekonwertowana na delegata, przechwytywanie odbywa się w ramkach, które są typami wartości. Oznacza to, że nie otrzymujesz żadnego nacisku na odzyskiwanie z używania funkcji lokalnych z funkcją przechwytywania.

### <a name="reachability"></a>Dostępność

Dodamy do specyfikacji

> Treść wyrażenia lambda z instrukcją lub funkcją lokalną jest uznawana za osiągalną.
