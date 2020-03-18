---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2019
ms.locfileid: "79485155"
---
# <a name="lambda-discard-parameters"></a><span data-ttu-id="5342c-101">Odrzuć parametry lambda</span><span class="sxs-lookup"><span data-stu-id="5342c-101">Lambda discard parameters</span></span>

## <a name="summary"></a><span data-ttu-id="5342c-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="5342c-102">Summary</span></span>

<span data-ttu-id="5342c-103">Zezwalaj na odrzucanie (`_`) do użycia jako parametry wyrażeń lambda i metod anonimowych.</span><span class="sxs-lookup"><span data-stu-id="5342c-103">Allow discards (`_`) to be used as parameters of lambdas and anonymous methods.</span></span>
<span data-ttu-id="5342c-104">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5342c-104">For example:</span></span>
- <span data-ttu-id="5342c-105">wyrażenia lambda: `(_, _) => 0`, `(int _, int _) => 0`</span><span class="sxs-lookup"><span data-stu-id="5342c-105">lambdas: `(_, _) => 0`, `(int _, int _) => 0`</span></span>
- <span data-ttu-id="5342c-106">Metody anonimowe: `delegate(int _, int _) { return 0; }`</span><span class="sxs-lookup"><span data-stu-id="5342c-106">anonymous methods: `delegate(int _, int _) { return 0; }`</span></span>

## <a name="motivation"></a><span data-ttu-id="5342c-107">Motywacją</span><span class="sxs-lookup"><span data-stu-id="5342c-107">Motivation</span></span>

<span data-ttu-id="5342c-108">Nieużywane parametry nie muszą mieć nazwy.</span><span class="sxs-lookup"><span data-stu-id="5342c-108">Unused parameters do not need to be named.</span></span> <span data-ttu-id="5342c-109">Intencje odrzucania są jasne, czyli nie są używane/odrzucane.</span><span class="sxs-lookup"><span data-stu-id="5342c-109">The intent of discards is clear, i.e. they are unused/discarded.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="5342c-110">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="5342c-110">Detailed design</span></span>

<span data-ttu-id="5342c-111">[Parametry metody](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) Na liście parametrów metody lambda lub anonimowej zawierającej więcej niż jeden parametr o nazwie `_`, takie parametry są odrzucane parametry.</span><span class="sxs-lookup"><span data-stu-id="5342c-111">[Method parameters](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) In the parameter list of a lambda or anonymous method with more than one parameter named `_`, such parameters are discard parameters.</span></span>
<span data-ttu-id="5342c-112">Uwaga: Jeśli pojedynczy parametr ma nazwę `_`, jest to zwykły parametr dla powodów zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="5342c-112">Note: if a single parameter is named `_` then it is a regular parameter for backwards compatibility reasons.</span></span>

<span data-ttu-id="5342c-113">Parametry Discard nie wprowadzają żadnych nazw do żadnych zakresów.</span><span class="sxs-lookup"><span data-stu-id="5342c-113">Discard parameters do not introduce any names to any scopes.</span></span>
<span data-ttu-id="5342c-114">Uwaga: oznacza to, że nie powodują ukrycia nazw `_` (podkreślenia).</span><span class="sxs-lookup"><span data-stu-id="5342c-114">Note this implies they do not cause any `_` (underscore) names to be hidden.</span></span>

<span data-ttu-id="5342c-115">[Proste nazwy](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) Jeśli `K` jest równa zero, a *simple_name* pojawia się w *bloku* , a jeśli *blok*(lub otaczający *blok*) obszar deklaracji zmiennych lokalnych ([deklaracji](basic-concepts.md#declarations)) zawiera zmienną lokalną, parametr (z wyjątkiem odrzucania parametrów) lub stała z nazwą `I`, wówczas *simple_name* odwołuje się do tej zmiennej lokalnej, parametru lub stałej i jest klasyfikowana jako zmienna lub wartość.</span><span class="sxs-lookup"><span data-stu-id="5342c-115">[Simple names](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter (with the exception of discard parameters) or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>

<span data-ttu-id="5342c-116">[Zakresy](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) Z wyjątkiem odrzucania parametrów zakres parametru zadeklarowany w *lambda_expression* ([wyrażenia funkcji anonimowej](expressions.md#anonymous-function-expressions)) to *anonymous_function_body* *lambda_expression* z wyjątkiem odrzucania parametrów, zakres parametru zadeklarowany w *anonymous_method_expression* ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)) to *blok* tego *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="5342c-116">[Scopes](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) With the exception of discard parameters, the scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression* With the exception of discard parameters, the scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>

## <a name="related-spec-sections"></a><span data-ttu-id="5342c-117">Powiązane sekcje specyfikacji</span><span class="sxs-lookup"><span data-stu-id="5342c-117">Related spec sections</span></span>
- [<span data-ttu-id="5342c-118">Odpowiednie parametry</span><span class="sxs-lookup"><span data-stu-id="5342c-118">Corresponding parameters</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
