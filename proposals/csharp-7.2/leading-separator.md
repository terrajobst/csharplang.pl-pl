---
ms.openlocfilehash: 6ec94aaabb2c52393a87ee450dbae972b6a50bd5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484714"
---
# <a name="allow-digit-separator-after-0b-or-0x"></a><span data-ttu-id="64360-101">Zezwalaj na separator cyfr po 0b lub 0x</span><span class="sxs-lookup"><span data-stu-id="64360-101">Allow digit separator after 0b or 0x</span></span>

<span data-ttu-id="64360-102">W C# 7,2, rozszerzając zestaw miejsc, które separatory cyfr (znak podkreślenia) mogą pojawić się w literałach całkowitych.</span><span class="sxs-lookup"><span data-stu-id="64360-102">In C# 7.2, we extend the set of places that digit separators (the underscore character) can appear in integral literals.</span></span> <span data-ttu-id="64360-103">[Począwszy od C# 7,0, separatory są dozwolone między cyframi literału](../csharp-7.0/digit-separators.md).</span><span class="sxs-lookup"><span data-stu-id="64360-103">[Beginning in C# 7.0, separators are permitted between the digits of a literal](../csharp-7.0/digit-separators.md).</span></span> <span data-ttu-id="64360-104">Teraz w C# 7,2 zezwalamy również na separatory cyfr przed pierwszą znaczącą cyfrą literału binarnego lub szesnastkowego po prefiksie.</span><span class="sxs-lookup"><span data-stu-id="64360-104">Now, in C# 7.2, we also permit digit separators before the first significant digit of a binary or hexadecimal literal, after the prefix.</span></span>

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

<span data-ttu-id="64360-105">Nie zezwalamy na literał dziesiętny, aby uzyskać znak podkreślenia wiodącego.</span><span class="sxs-lookup"><span data-stu-id="64360-105">We do not permit a decimal integer literal to have a leading underscore.</span></span> <span data-ttu-id="64360-106">Token, taki jak `_123`, jest identyfikatorem.</span><span class="sxs-lookup"><span data-stu-id="64360-106">A token such as `_123` is an identifier.</span></span>