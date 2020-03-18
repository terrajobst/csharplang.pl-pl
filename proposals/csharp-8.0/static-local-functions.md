---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2019
ms.locfileid: "79485099"
---
# <a name="static-local-functions"></a><span data-ttu-id="15d14-101">statyczne funkcje lokalne</span><span class="sxs-lookup"><span data-stu-id="15d14-101">Static local functions</span></span>

## <a name="summary"></a><span data-ttu-id="15d14-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="15d14-102">Summary</span></span>

<span data-ttu-id="15d14-103">Obsługa funkcji lokalnych, które nie zezwalają na przechwytywanie stanu z otaczającego zakresu.</span><span class="sxs-lookup"><span data-stu-id="15d14-103">Support local functions that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="15d14-104">Motywacją</span><span class="sxs-lookup"><span data-stu-id="15d14-104">Motivation</span></span>

<span data-ttu-id="15d14-105">Unikaj przypadkowego przechwytywania stanu z otaczającego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="15d14-105">Avoid unintentionally capturing state from the enclosing context.</span></span>
<span data-ttu-id="15d14-106">Zezwalaj na używanie funkcji lokalnych w scenariuszach, w których jest wymagana Metoda `static`.</span><span class="sxs-lookup"><span data-stu-id="15d14-106">Allow local functions to be used in scenarios where a `static` method is required.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="15d14-107">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="15d14-107">Detailed design</span></span>

<span data-ttu-id="15d14-108">Funkcja lokalna zadeklarowana `static` nie może przechwycić stanu z otaczającego zakresu.</span><span class="sxs-lookup"><span data-stu-id="15d14-108">A local function declared `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="15d14-109">W wyniku tego elementy lokalne, parametry i `this` z otaczającego zakresu nie są dostępne w `static` funkcji lokalnej.</span><span class="sxs-lookup"><span data-stu-id="15d14-109">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` local function.</span></span>

<span data-ttu-id="15d14-110">`static` funkcja lokalna nie może odwoływać się do członków wystąpienia z niejawnego lub jawnego `this` lub odwołania `base`.</span><span class="sxs-lookup"><span data-stu-id="15d14-110">A `static` local function cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="15d14-111">Funkcja lokalna `static` może odwoływać się do `static` członków z otaczającego zakresu.</span><span class="sxs-lookup"><span data-stu-id="15d14-111">A `static` local function may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="15d14-112">Funkcja lokalna `static` może odwoływać się do `constant` definicji z zakresu otaczającego.</span><span class="sxs-lookup"><span data-stu-id="15d14-112">A `static` local function may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="15d14-113">`nameof()` w lokalnej funkcji `static` może odwoływać się do zmiennych lokalnych, parametrów lub `this` lub `base` z otaczającego zakresu.</span><span class="sxs-lookup"><span data-stu-id="15d14-113">`nameof()` in a `static` local function may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="15d14-114">Reguły ułatwień dostępu dla elementów członkowskich `private` w otaczającym zakresie są takie same dla `static` i nie`static`ych funkcji lokalnych.</span><span class="sxs-lookup"><span data-stu-id="15d14-114">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` local functions.</span></span>

<span data-ttu-id="15d14-115">`static` definicję funkcji lokalnej są emitowane jako metody `static` w metadanych, nawet jeśli są używane tylko w delegatze.</span><span class="sxs-lookup"><span data-stu-id="15d14-115">A `static` local function definition is emitted as a `static` method in metadata, even if only used in a delegate.</span></span>

<span data-ttu-id="15d14-116">Funkcja lokalna nie`static` lub lambda może przechwycić stan z otaczającej `static` funkcji lokalnej, ale nie może przechwycić stanu poza otaczającą `static` funkcją lokalną.</span><span class="sxs-lookup"><span data-stu-id="15d14-116">A non-`static` local function or lambda can capture state from an enclosing `static` local function but cannot capture state outside the enclosing `static` local function.</span></span>

<span data-ttu-id="15d14-117">Nie można wywołać funkcji lokalnej `static` w drzewie wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="15d14-117">A `static` local function cannot be invoked in an expression tree.</span></span>

<span data-ttu-id="15d14-118">Wywołanie funkcji lokalnej jest emitowane jako `call`, a nie `callvirt`, niezależnie od tego, czy funkcja lokalna jest `static`.</span><span class="sxs-lookup"><span data-stu-id="15d14-118">A call to a local function is emitted as `call` rather than `callvirt`, regardless of whether the local function is `static`.</span></span>

<span data-ttu-id="15d14-119">Bez względu na to, czy funkcja lokalna jest `static`.</span><span class="sxs-lookup"><span data-stu-id="15d14-119">Overload resolution of a call within a local function not affected by whether the local function is `static`.</span></span>

<span data-ttu-id="15d14-120">Usunięcie modyfikatora `static` z funkcji lokalnej w prawidłowym programie nie zmienia znaczenia programu.</span><span class="sxs-lookup"><span data-stu-id="15d14-120">Removing the `static` modifier from a local function in a valid program does not change the meaning of the program.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="15d14-121">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="15d14-121">Design meetings</span></span>

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
