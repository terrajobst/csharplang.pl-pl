---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484546"
---
# <a name="throw-expression"></a><span data-ttu-id="e2f32-101">Wyrażenie throw</span><span class="sxs-lookup"><span data-stu-id="e2f32-101">Throw expression</span></span>

<span data-ttu-id="e2f32-102">Rozszerzy zestaw formularzy wyrażeń do uwzględnienia</span><span class="sxs-lookup"><span data-stu-id="e2f32-102">We extend the set of expression forms to include</span></span>

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

<span data-ttu-id="e2f32-103">Reguły typu są następujące:</span><span class="sxs-lookup"><span data-stu-id="e2f32-103">The type rules are as follows:</span></span>

- <span data-ttu-id="e2f32-104">*Throw_expression* nie ma typu.</span><span class="sxs-lookup"><span data-stu-id="e2f32-104">A *throw_expression* has no type.</span></span>
- <span data-ttu-id="e2f32-105">*Throw_expression* jest konwertowany na każdy typ przez niejawną konwersję.</span><span class="sxs-lookup"><span data-stu-id="e2f32-105">A *throw_expression* is convertible to every type by an implicit conversion.</span></span>

<span data-ttu-id="e2f32-106">*Wyrażenie throw* zwraca wartość wygenerowaną przez ocenę *null_coalescing_expression*, która musi określać wartość typu klasy `System.Exception`, klasy typu, która pochodzi od `System.Exception` lub typu parametru typu, który ma `System.Exception` (lub jego podklasę) jako obowiązującą klasę bazową.</span><span class="sxs-lookup"><span data-stu-id="e2f32-106">A *throw expression* throws the value produced by evaluating the *null_coalescing_expression*, which must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="e2f32-107">Jeśli Obliczanie wyrażenia powoduje `null`, zamiast niego zostanie zgłoszony `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="e2f32-107">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="e2f32-108">Zachowanie w czasie wykonywania obliczania *wyrażenia throw* jest takie samo, [jak określone dla *instrukcji throw*](../../spec/statements.md#the-throw-statement).</span><span class="sxs-lookup"><span data-stu-id="e2f32-108">The behavior at runtime of the evaluation of a *throw expression* is the same [as specified for a *throw statement*](../../spec/statements.md#the-throw-statement).</span></span>

<span data-ttu-id="e2f32-109">Reguły analizy przepływu są następujące:</span><span class="sxs-lookup"><span data-stu-id="e2f32-109">The flow-analysis rules are as follows:</span></span>

- <span data-ttu-id="e2f32-110">Dla każdej zmiennej *v*, *v* jest przede wszystkim przypisane przed *null_coalescing_expression* *throw_expression* Jeśli przed *throw_expression*.</span><span class="sxs-lookup"><span data-stu-id="e2f32-110">For every variable *v*, *v* is definitely assigned before the *null_coalescing_expression* of a *throw_expression* iff it is definitely assigned before the *throw_expression*.</span></span>
- <span data-ttu-id="e2f32-111">Dla każdej zmiennej *v*, *v* jest ostatecznie przypisana po *throw_expression*.</span><span class="sxs-lookup"><span data-stu-id="e2f32-111">For every variable *v*, *v* is definitely assigned after *throw_expression*.</span></span>

<span data-ttu-id="e2f32-112">*Wyrażenie throw* jest dozwolone tylko w następujących kontekstach składni:</span><span class="sxs-lookup"><span data-stu-id="e2f32-112">A *throw expression* is permitted in only the following syntactic contexts:</span></span>
- <span data-ttu-id="e2f32-113">Jako drugi lub trzeci operand operatora warunkowego Trzyelementowy `?:`</span><span class="sxs-lookup"><span data-stu-id="e2f32-113">As the second or third operand of a ternary conditional operator `?:`</span></span>
- <span data-ttu-id="e2f32-114">Jako drugi operand operatora łączącego wartości null `??`</span><span class="sxs-lookup"><span data-stu-id="e2f32-114">As the second operand of a null coalescing operator `??`</span></span>
- <span data-ttu-id="e2f32-115">Jako treść wyrażenia lambda lub metody.</span><span class="sxs-lookup"><span data-stu-id="e2f32-115">As the body of an expression-bodied lambda or method.</span></span>
