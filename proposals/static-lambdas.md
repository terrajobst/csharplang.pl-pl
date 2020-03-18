---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/22/2019
ms.locfileid: "79485085"
---
# <a name="static-lambdas"></a><span data-ttu-id="9b5f0-101">Statyczne wyrażenia lambda</span><span class="sxs-lookup"><span data-stu-id="9b5f0-101">Static lambdas</span></span>

## <a name="summary"></a><span data-ttu-id="9b5f0-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="9b5f0-102">Summary</span></span>

<span data-ttu-id="9b5f0-103">Obsługa wyrażeń lambda, które nie umożliwiają przechwytywania stanu z otaczającego zakresu.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-103">Support lambdas that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="9b5f0-104">Motywacją</span><span class="sxs-lookup"><span data-stu-id="9b5f0-104">Motivation</span></span>

<span data-ttu-id="9b5f0-105">Unikaj przypadkowego przechwytywania stanu z otaczającego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-105">Avoid unintentionally capturing state from the enclosing context.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="9b5f0-106">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="9b5f0-106">Detailed design</span></span>

<span data-ttu-id="9b5f0-107">Wyrażenia lambda z `static` nie mogą przechwycić stanu z otaczającego zakresu.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-107">A lambdas with `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="9b5f0-108">W wyniku tego elementy lokalne, parametry i `this` z otaczającego zakresu nie są dostępne w `static` lambda.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-108">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` lambda.</span></span>

<span data-ttu-id="9b5f0-109">`static` lambda nie może odwoływać się do składowych wystąpienia z niejawnego lub jawnego `this` lub odwołania `base`.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-109">A `static` lambda cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="9b5f0-110">`static` lambda może odwoływać się do `static` członków z otaczającego zakresu.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-110">A `static` lambda may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="9b5f0-111">`static` lambda może odwoływać się do definicji `constant` z otaczającego zakresu.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-111">A `static` lambda may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="9b5f0-112">`nameof()` w `static` lambda może odwoływać się do zmiennych lokalnych, parametrów lub `this` lub `base` z otaczającego zakresu.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-112">`nameof()` in a `static` lambda may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="9b5f0-113">Reguły ułatwień dostępu dla elementów członkowskich `private` w otaczającym zakresie są takie same dla `static` i nie`static` wyrażeń lambda.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-113">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` lambdas.</span></span>

<span data-ttu-id="9b5f0-114">Żadna gwarancja nie jest podejmowana w przypadku, gdy `static` wyrażenie lambda jest emitowane jako metoda `static` w metadanych.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-114">No guarantee is made as to whether a `static` lambda definition is emitted as a `static` method in metadata.</span></span> <span data-ttu-id="9b5f0-115">Jest to pozostało do implementacji kompilatora do optymalizacji.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-115">This is left up to the compiler implementation to optimize.</span></span>

<span data-ttu-id="9b5f0-116">Funkcja lokalna nie`static` lub lambda może przechwytywać stan z otaczającego `static` lambda, ale nie może przechwycić stanu poza otaczającą `static` lambda.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-116">A non-`static` local function or lambda can capture state from an enclosing `static` lambda but cannot capture state outside the enclosing `static` lambda.</span></span>

<span data-ttu-id="9b5f0-117">`static` lambda można użyć w drzewie wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-117">A `static` lambda can be used in an expression tree.</span></span>

<span data-ttu-id="9b5f0-118">Usunięcie modyfikatora `static` z wyrażenia lambda w prawidłowym programie nie zmienia znaczenia programu.</span><span class="sxs-lookup"><span data-stu-id="9b5f0-118">Removing the `static` modifier from a lambda in a valid program does not change the meaning of the program.</span></span>
