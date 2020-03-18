---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484602"
---
# <a name="ref-local-reassignment"></a>Lokalne ponowne przypisanie odwołania

W C# 7,3 dodaliśmy obsługę ponownego powiązania referent zmiennej lokalnej ref lub parametru ref.

Do zestawu `assignment_operator`s dodajemy następujące elementy.

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

Operator `=ref` jest nazywany ***operatorem przypisania odwołania***. Nie jest to *złożony operator przypisania*. Lewy operand musi być wyrażeniem, które wiąże się z zmienną lokalną ref, parametrem ref (innym niż `this`) lub parametrem out. Prawy operand musi być wyrażeniem, które daje lvalue wyznaczanie wartości tego samego typu co lewy argument operacji.

Prawy operand musi być ostatecznie przypisany w punkcie przypisywania odwołania.

Gdy argument operacji po lewej stronie wiąże się z parametrem `out`, występuje błąd, jeśli ten parametr `out` nie został ostatecznie przypisany na początku operatora przypisania odwołania.

Jeśli Lewy argument operacji jest odwołaniem zapisywalnym (tj. wyznaczy coś innego niż `ref readonly` parametr lokalny lub `in`), prawy operand musi być lvalueem do zapisu.

Operator przypisania ref zwraca lvalue przypisanego typu. Jest zapisywalny, jeśli lewy operand jest zapisywalny (tj. nie `ref readonly` ani `in`).

Reguły bezpieczeństwa dla tego operatora są następujące:

- W przypadku ponownego przypisania odwołań `e1 = ref e2`*odwołanie typu "bezpieczny-do-ucieczki"* `e2` musi być co najmniej tak szerokie jak *"ref-Safe-to-Escape* " `e1`.

Where *"ref-Safe-to-Escape* " jest zdefiniowany w [zabezpieczeniach dla typów typu referencyjnego](../csharp-7.2/span-safety.md)
