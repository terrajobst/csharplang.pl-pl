---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484504"
---
# <a name="digit-separators"></a>Separatory cyfr

Możliwość grupowania cyfr w dużych literałach liczbowych będzie miała doskonały wpływ na czytelność i nie ma znaczących minusem. 

Dodanie literałów binarnych (#215) zwiększy prawdopodobieństwo długi literały numeryczne, więc dwie funkcje rozszerzają siebie nawzajem. 

Będziemy przestrzegać języka Java i innych i używać podkreślenia `_` jako separatora cyfr. Może wystąpić wszędzie w literale numerycznym (z wyjątkiem pierwszego i ostatniego znaku), ponieważ różne grupowania mogą mieć sens w różnych scenariuszach, w szczególności dla różnych baz liczbowych:

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

Każda sekwencja cyfr może być oddzielona znakami podkreślenia, co może być więcej niż jedną podkreśleniem między dwoma kolejnymi cyframi. Są one dozwolone w przypadku wartości dziesiętnych, a także wykładników, ale zgodnie z poprzednią regułą mogą nie pojawiać się obok wartości dziesiętnej (`10_.0`), obok znaku wykładnika (`1.1e_1`) lub obok specyfikatora typu (`10_f`). Gdy są używane w literałach binarnych i szesnastkowych, mogą nie występować bezpośrednio po `0x` lub `0b`.

Składnia jest prosta, a separatory nie mają wpływu na semantykę — są po prostu ignorowane.

Ta wartość ma szerokie wartości i jest łatwa do zaimplementowania.
