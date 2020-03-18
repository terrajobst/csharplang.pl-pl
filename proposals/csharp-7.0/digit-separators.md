---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484504"
---
# <a name="digit-separators"></a><span data-ttu-id="c844c-101">Separatory cyfr</span><span class="sxs-lookup"><span data-stu-id="c844c-101">Digit separators</span></span>

<span data-ttu-id="c844c-102">Możliwość grupowania cyfr w dużych literałach liczbowych będzie miała doskonały wpływ na czytelność i nie ma znaczących minusem.</span><span class="sxs-lookup"><span data-stu-id="c844c-102">Being able to group digits in large numeric literals would have great readability impact and no significant downside.</span></span> 

<span data-ttu-id="c844c-103">Dodanie literałów binarnych (#215) zwiększy prawdopodobieństwo długi literały numeryczne, więc dwie funkcje rozszerzają siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="c844c-103">Adding binary literals (#215) would increase the likelihood of numeric literals being long, so the two features enhance each other.</span></span> 

<span data-ttu-id="c844c-104">Będziemy przestrzegać języka Java i innych i używać podkreślenia `_` jako separatora cyfr.</span><span class="sxs-lookup"><span data-stu-id="c844c-104">We would follow Java and others, and use an underscore `_` as a digit separator.</span></span> <span data-ttu-id="c844c-105">Może wystąpić wszędzie w literale numerycznym (z wyjątkiem pierwszego i ostatniego znaku), ponieważ różne grupowania mogą mieć sens w różnych scenariuszach, w szczególności dla różnych baz liczbowych:</span><span class="sxs-lookup"><span data-stu-id="c844c-105">It would be able to occur everywhere in a numeric literal (except as the first and last character), since different groupings may make sense in different scenarios and especially for different numeric bases:</span></span>

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

<span data-ttu-id="c844c-106">Każda sekwencja cyfr może być oddzielona znakami podkreślenia, co może być więcej niż jedną podkreśleniem między dwoma kolejnymi cyframi.</span><span class="sxs-lookup"><span data-stu-id="c844c-106">Any sequence of digits may be separated by underscores, possibly more than one underscore between two consecutive digits.</span></span> <span data-ttu-id="c844c-107">Są one dozwolone w przypadku wartości dziesiętnych, a także wykładników, ale zgodnie z poprzednią regułą mogą nie pojawiać się obok wartości dziesiętnej (`10_.0`), obok znaku wykładnika (`1.1e_1`) lub obok specyfikatora typu (`10_f`).</span><span class="sxs-lookup"><span data-stu-id="c844c-107">They are allowed in decimals as well as exponents, but following the previous rule, they may not appear next to the decimal (`10_.0`), next to the exponent character (`1.1e_1`), or next to the type specifier (`10_f`).</span></span> <span data-ttu-id="c844c-108">Gdy są używane w literałach binarnych i szesnastkowych, mogą nie występować bezpośrednio po `0x` lub `0b`.</span><span class="sxs-lookup"><span data-stu-id="c844c-108">When used in binary and hexadecimal literals, they may not appear immediately following the `0x` or `0b`.</span></span>

<span data-ttu-id="c844c-109">Składnia jest prosta, a separatory nie mają wpływu na semantykę — są po prostu ignorowane.</span><span class="sxs-lookup"><span data-stu-id="c844c-109">The syntax is straightforward, and the separators have no semantic impact - they are simply ignored.</span></span>

<span data-ttu-id="c844c-110">Ta wartość ma szerokie wartości i jest łatwa do zaimplementowania.</span><span class="sxs-lookup"><span data-stu-id="c844c-110">This has broad value and is easy to implement.</span></span>
