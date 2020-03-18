---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484511"
---
# <a name="out-variable-declarations"></a>Deklaracje zmiennych wyjściowych

Funkcja *deklaracji zmiennej out* umożliwia zadeklarowanie zmiennej w lokalizacji, w której jest ona przenoszona jako argument `out`.

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

Zmienna zadeklarowana w ten sposób jest nazywana *zmienną out*. Dla typu zmiennej można użyć `var` kontekstowego słowa kluczowego. Zakres będzie taki sam jak w przypadku *zmiennej wzorca* wprowadzonej przy użyciu dopasowania do wzorca.

Zgodnie z specyfikacją języka (w sekcji 7.6.7 dostęp do elementu) lista argumentów dla dostępu do elementu (wyrażenie indeksowania) nie zawiera argumentów ref ani out. Jednak są one dozwolone przez kompilator dla różnych scenariuszy, na przykład indeksatory zadeklarowane w metadanych, które akceptują `out`.

W zakresie zmiennej lokalnej wprowadzonej przez argument_value jest to błąd czasu kompilacji, który odwołuje się do tej zmiennej lokalnej w pozycji tekstowej, która poprzedza jej deklarację.

Występuje również błąd odwołujący się do zmiennej out o typie określonym niejawnie (§ 8.5.1) na tej samej liście argumentów, która bezpośrednio zawiera jego deklarację.

Rozpoznawanie przeciążenia jest modyfikowane w następujący sposób:

Dodamy nową konwersję:

> Istnieje *Konwersja z wyrażenia* z deklaracji zmiennej niejawnie wpisanej do każdego typu.

Być

> Typ argumentu zmiennej wyjściowej jawnie wpisanej jest zadeklarowanym typem.

i

> Argument zmiennej wyjściowej z niejawnym typem nie ma typu.

*Konwersja z wyrażenia* z niejawnie wpisanej zmiennej out nie jest uważana za lepszą niż jakakolwiek inna *Konwersja z wyrażenia*.

Typ zmiennej out niejawnie wpisanej jest typem odpowiadającego parametru w podpisie metody wybranej przez metodę rozpoznawania przeciążenia.

Nowa `DeclarationExpressionSyntax` węzła składni zostanie dodana do reprezentowania deklaracji w argumencie out var.
