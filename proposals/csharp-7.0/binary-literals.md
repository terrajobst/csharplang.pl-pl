---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484427"
---
# <a name="binary-literals"></a>Literały binarne

Istnieje relatywnie typowe żądanie dodania literałów binarnych do C# i VB. W przypadku masek bitowych (np. flag wyliczeniowych) wydaje się, że jest ona użyteczna, ale również jest świetna tylko do celów edukacyjnych.

Literały binarne będą wyglądać następująco:

```csharp
int nineteen = 0b10011;
```

Syntaktycznie i semantycznie są takie same jak literały szesnastkowe, z wyjątkiem używania `b`/`B` zamiast `x`/`X`, które mają tylko cyfry `0` i `1` i są interpretowane w Base 2 zamiast 16.

Nie ma to pewnej kosztów, aby zaimplementować te działania, i niewielkich nakładów związanych z koncepcją dla użytkowników języka.

## <a name="syntax"></a>Składnia

Gramatyka będzie następująca:

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
