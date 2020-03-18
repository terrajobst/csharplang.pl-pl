---
ms.openlocfilehash: 6ec94aaabb2c52393a87ee450dbae972b6a50bd5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484714"
---
# <a name="allow-digit-separator-after-0b-or-0x"></a>Zezwalaj na separator cyfr po 0b lub 0x

W C# 7,2, rozszerzając zestaw miejsc, które separatory cyfr (znak podkreślenia) mogą pojawić się w literałach całkowitych. [Począwszy od C# 7,0, separatory są dozwolone między cyframi literału](../csharp-7.0/digit-separators.md). Teraz w C# 7,2 zezwalamy również na separatory cyfr przed pierwszą znaczącą cyfrą literału binarnego lub szesnastkowego po prefiksie.

```csharp
    123      // permitted in C# 1.0 and later
    1_2_3    // permitted in C# 7.0 and later
    0x1_2_3  // permitted in C# 7.0 and later
    0b101    // binary literals added in C# 7.0
    0b1_0_1  // permitted in C# 7.0 and later

    // in C# 7.2, _ is permitted after the `0x` or `0b`
    0x_1_2   // permitted in C# 7.2 and later
    0b_1_0_1 // permitted in C# 7.2 and later
```

Nie zezwalamy na literał dziesiętny, aby uzyskać znak podkreślenia wiodącego. Token, taki jak `_123`, jest identyfikatorem.
