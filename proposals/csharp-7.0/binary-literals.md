---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484427"
---
# <a name="binary-literals"></a><span data-ttu-id="cd19e-101">Literały binarne</span><span class="sxs-lookup"><span data-stu-id="cd19e-101">Binary literals</span></span>

<span data-ttu-id="cd19e-102">Istnieje relatywnie typowe żądanie dodania literałów binarnych do C# i VB.</span><span class="sxs-lookup"><span data-stu-id="cd19e-102">There’s a relatively common request to add binary literals to C# and VB.</span></span> <span data-ttu-id="cd19e-103">W przypadku masek bitowych (np. flag wyliczeniowych) wydaje się, że jest ona użyteczna, ale również jest świetna tylko do celów edukacyjnych.</span><span class="sxs-lookup"><span data-stu-id="cd19e-103">For bitmasks (e.g. flag enums) this seems genuinely useful, but it would also be great just for educational purposes.</span></span>

<span data-ttu-id="cd19e-104">Literały binarne będą wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="cd19e-104">Binary literals would look like this:</span></span>

```csharp
int nineteen = 0b10011;
```

<span data-ttu-id="cd19e-105">Syntaktycznie i semantycznie są takie same jak literały szesnastkowe, z wyjątkiem używania `b`/`B` zamiast `x`/`X`, które mają tylko cyfry `0` i `1` i są interpretowane w Base 2 zamiast 16.</span><span class="sxs-lookup"><span data-stu-id="cd19e-105">Syntactically and semantically they are identical to hexadecimal literals, except for using `b`/`B` instead of `x`/`X`, having only digits `0` and `1` and being interpreted in base 2 instead of 16.</span></span>

<span data-ttu-id="cd19e-106">Nie ma to pewnej kosztów, aby zaimplementować te działania, i niewielkich nakładów związanych z koncepcją dla użytkowników języka.</span><span class="sxs-lookup"><span data-stu-id="cd19e-106">There’s little cost to implementing these, and little conceptual overhead to users of the language.</span></span>

## <a name="syntax"></a><span data-ttu-id="cd19e-107">Składnia</span><span class="sxs-lookup"><span data-stu-id="cd19e-107">Syntax</span></span>

<span data-ttu-id="cd19e-108">Gramatyka będzie następująca:</span><span class="sxs-lookup"><span data-stu-id="cd19e-108">The grammar would be as follows:</span></span>

```antlr
integer-literal:
    : ...
    | binary-integer-literal
    ;
binary-integer-literal:
    : `0b` binary-digits integer-type-suffix-opt
    | `0B` binary-digits integer-type-suffix-opt
    ;
binary-digits:
    : binary-digit
    | binary-digits binary-digit
    ;
binary-digit:
    : `0`
    | `1`
    ;
```
