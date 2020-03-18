---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484602"
---
# <a name="ref-local-reassignment"></a><span data-ttu-id="9dbdb-101">Lokalne ponowne przypisanie odwołania</span><span class="sxs-lookup"><span data-stu-id="9dbdb-101">Ref Local Reassignment</span></span>

<span data-ttu-id="9dbdb-102">W C# 7,3 dodaliśmy obsługę ponownego powiązania referent zmiennej lokalnej ref lub parametru ref.</span><span class="sxs-lookup"><span data-stu-id="9dbdb-102">In C# 7.3, we add support for rebinding the referent of a ref local variable or a ref parameter.</span></span>

<span data-ttu-id="9dbdb-103">Do zestawu `assignment_operator`s dodajemy następujące elementy.</span><span class="sxs-lookup"><span data-stu-id="9dbdb-103">We add the following to the set of `assignment_operator`s.</span></span>

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

<span data-ttu-id="9dbdb-104">Operator `=ref` jest nazywany ***operatorem przypisania odwołania***.</span><span class="sxs-lookup"><span data-stu-id="9dbdb-104">The `=ref` operator is called the ***ref assignment operator***.</span></span> <span data-ttu-id="9dbdb-105">Nie jest to *złożony operator przypisania*.</span><span class="sxs-lookup"><span data-stu-id="9dbdb-105">It is not a *compound assignment operator*.</span></span> <span data-ttu-id="9dbdb-106">Lewy operand musi być wyrażeniem, które wiąże się z zmienną lokalną ref, parametrem ref (innym niż `this`) lub parametrem out.</span><span class="sxs-lookup"><span data-stu-id="9dbdb-106">The left operand must be an expression that binds to a ref local variable, a ref parameter (other than `this`), or an out parameter.</span></span> <span data-ttu-id="9dbdb-107">Prawy operand musi być wyrażeniem, które daje lvalue wyznaczanie wartości tego samego typu co lewy argument operacji.</span><span class="sxs-lookup"><span data-stu-id="9dbdb-107">The right operand must be an expression that yields an lvalue designating a value of the same type as the left operand.</span></span>

<span data-ttu-id="9dbdb-108">Prawy operand musi być ostatecznie przypisany w punkcie przypisywania odwołania.</span><span class="sxs-lookup"><span data-stu-id="9dbdb-108">The right operand must be definitely assigned at the point of the ref assignment.</span></span>

<span data-ttu-id="9dbdb-109">Gdy argument operacji po lewej stronie wiąże się z parametrem `out`, występuje błąd, jeśli ten parametr `out` nie został ostatecznie przypisany na początku operatora przypisania odwołania.</span><span class="sxs-lookup"><span data-stu-id="9dbdb-109">When the left operand binds to an `out` parameter, it is an error if that `out` parameter has not been definitely assigned at the beginning of the ref assignment operator.</span></span>

<span data-ttu-id="9dbdb-110">Jeśli Lewy argument operacji jest odwołaniem zapisywalnym (tj. wyznaczy coś innego niż `ref readonly` parametr lokalny lub `in`), prawy operand musi być lvalueem do zapisu.</span><span class="sxs-lookup"><span data-stu-id="9dbdb-110">If the left operand is a writeable ref (i.e. it designates anything other than a `ref readonly` local or  `in` parameter), then the right operand must be a writeable lvalue.</span></span>

<span data-ttu-id="9dbdb-111">Operator przypisania ref zwraca lvalue przypisanego typu.</span><span class="sxs-lookup"><span data-stu-id="9dbdb-111">The ref assignment operator yields an lvalue of the assigned type.</span></span> <span data-ttu-id="9dbdb-112">Jest zapisywalny, jeśli lewy operand jest zapisywalny (tj. nie `ref readonly` ani `in`).</span><span class="sxs-lookup"><span data-stu-id="9dbdb-112">It is writeable if the left operand is writeable (i.e. not `ref readonly` or `in`).</span></span>

<span data-ttu-id="9dbdb-113">Reguły bezpieczeństwa dla tego operatora są następujące:</span><span class="sxs-lookup"><span data-stu-id="9dbdb-113">The safety rules for this operator are:</span></span>

- <span data-ttu-id="9dbdb-114">W przypadku ponownego przypisania odwołań `e1 = ref e2`*odwołanie typu "bezpieczny-do-ucieczki"* `e2` musi być co najmniej tak szerokie jak *"ref-Safe-to-Escape* " `e1`.</span><span class="sxs-lookup"><span data-stu-id="9dbdb-114">For a ref reassignment `e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

<span data-ttu-id="9dbdb-115">Where *"ref-Safe-to-Escape* " jest zdefiniowany w [zabezpieczeniach dla typów typu referencyjnego](../csharp-7.2/span-safety.md)</span><span class="sxs-lookup"><span data-stu-id="9dbdb-115">Where *ref-safe-to-escape* is defined in [Safety for ref-like types](../csharp-7.2/span-safety.md)</span></span>
