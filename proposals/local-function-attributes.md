---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509508"
---
# <a name="attributes-on-local-functions"></a><span data-ttu-id="9dfb4-101">Atrybuty funkcji lokalnych</span><span class="sxs-lookup"><span data-stu-id="9dfb4-101">Attributes on local functions</span></span>

## <a name="attributes"></a><span data-ttu-id="9dfb4-102">Atrybuty</span><span class="sxs-lookup"><span data-stu-id="9dfb4-102">Attributes</span></span>

<span data-ttu-id="9dfb4-103">Deklaracje funkcji lokalnych mogą teraz mieć [atrybuty](../spec/attributes.md).</span><span class="sxs-lookup"><span data-stu-id="9dfb4-103">Local function declarations are now permitted to have [attributes](../spec/attributes.md).</span></span> <span data-ttu-id="9dfb4-104">Parametry i parametry typu w funkcjach lokalnych mogą mieć atrybuty.</span><span class="sxs-lookup"><span data-stu-id="9dfb4-104">Parameters and type parameters on local functions are also allowed to have attributes.</span></span>

<span data-ttu-id="9dfb4-105">Atrybuty o określonym znaczeniu w przypadku zastosowania do metody, jej parametrów lub parametrów typu będą miały takie samo znaczenie, gdy są stosowane do funkcji lokalnej, jej parametrów lub parametrów typu.</span><span class="sxs-lookup"><span data-stu-id="9dfb4-105">Attributes with a specified meaning when applied to a method, its parameters, or its type parameters will have the same meaning when applied to a local function, its parameters, or its type parameters, respectively.</span></span>

<span data-ttu-id="9dfb4-106">Funkcja lokalna może być taka sama w tym samym sensie jak [Metoda warunkowa](../spec/attributes.md#the-conditional-attribute) , dekorowania nazwy ją z `[ConditionalAttribute]`.</span><span class="sxs-lookup"><span data-stu-id="9dfb4-106">A local function can be made conditional in the same sense as a [conditional method](../spec/attributes.md#the-conditional-attribute) by decorating it with a `[ConditionalAttribute]`.</span></span> <span data-ttu-id="9dfb4-107">Należy również `static`warunkowej funkcji lokalnej.</span><span class="sxs-lookup"><span data-stu-id="9dfb4-107">A conditional local function must also be `static`.</span></span> <span data-ttu-id="9dfb4-108">Wszystkie ograniczenia dotyczące metod warunkowych stosują się również do warunkowych funkcji lokalnych, w tym, że typem zwracanym musi być `void`.</span><span class="sxs-lookup"><span data-stu-id="9dfb4-108">All restrictions on conditional methods also apply to conditional local functions, including that the return type must be `void`.</span></span>

## <a name="extern"></a><span data-ttu-id="9dfb4-109">Modyfikator</span><span class="sxs-lookup"><span data-stu-id="9dfb4-109">Extern</span></span>

<span data-ttu-id="9dfb4-110">Modyfikator `extern` jest teraz dozwolony dla funkcji lokalnych.</span><span class="sxs-lookup"><span data-stu-id="9dfb4-110">The `extern` modifier is now permitted on local functions.</span></span> <span data-ttu-id="9dfb4-111">Dzięki temu funkcja lokalna jest zewnętrzna w tym samym sensie co [Metoda zewnętrzna](../spec/classes.md#external-methods).</span><span class="sxs-lookup"><span data-stu-id="9dfb4-111">This makes the local function external in the same sense as an [external method](../spec/classes.md#external-methods).</span></span>

<span data-ttu-id="9dfb4-112">Podobnie jak w przypadku metody zewnętrznej, *treść funkcji lokalnej* zewnętrznej funkcji lokalnej musi być średnikiem.</span><span class="sxs-lookup"><span data-stu-id="9dfb4-112">Similarly to an external method, the *local-function-body* of an external local function must be a semicolon.</span></span> <span data-ttu-id="9dfb4-113">*Treść funkcji lokalnej* z średnikami jest dozwolona tylko w zewnętrznej funkcji lokalnej.</span><span class="sxs-lookup"><span data-stu-id="9dfb4-113">A semicolon *local-function-body* is only permitted on an external local function.</span></span> 

<span data-ttu-id="9dfb4-114">Należy również `static`zewnętrzną funkcję lokalną.</span><span class="sxs-lookup"><span data-stu-id="9dfb4-114">An external local function must also be `static`.</span></span>

## <a name="syntax"></a><span data-ttu-id="9dfb4-115">Składnia</span><span class="sxs-lookup"><span data-stu-id="9dfb4-115">Syntax</span></span>

<span data-ttu-id="9dfb4-116">[Gramatyka funkcji lokalnych](csharp-7.0/local-functions.md#syntax-grammar) jest modyfikowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="9dfb4-116">The [local functions grammar](csharp-7.0/local-functions.md#syntax-grammar) is modified as follows:</span></span>
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
