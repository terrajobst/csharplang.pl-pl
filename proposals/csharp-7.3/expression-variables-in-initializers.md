---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484924"
---
# <a name="expression-variables-in-initializers"></a><span data-ttu-id="b1f65-101">Zmienne wyrażeń w inicjatorach</span><span class="sxs-lookup"><span data-stu-id="b1f65-101">Expression variables in initializers</span></span>

## <a name="summary"></a><span data-ttu-id="b1f65-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="b1f65-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="b1f65-103">Rozszerzono funkcje wprowadzone w C# 7, aby zezwolić na wyrażenia zawierające zmienne wyrażeń (out deklaracji zmiennych i wzorców deklaracji) w inicjatorach pól, inicjatorach właściwości, elemencie ctor i klauzulach zapytania.</span><span class="sxs-lookup"><span data-stu-id="b1f65-103">We extend the features introduced in C# 7 to permit expressions containing expression variables (out variable declarations and declaration patterns) in field initializers, property initializers, ctor-initializers, and query clauses.</span></span>

## <a name="motivation"></a><span data-ttu-id="b1f65-104">Motywacją</span><span class="sxs-lookup"><span data-stu-id="b1f65-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b1f65-105">Spowoduje to zakończenie kilku niepełnych krawędzi w języku, C# z powodu braku czasu.</span><span class="sxs-lookup"><span data-stu-id="b1f65-105">This completes a couple of the rough edges left in the C# language due to lack of time.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b1f65-106">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="b1f65-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b1f65-107">Firma Microsoft usuwa ograniczenie uniemożliwiające deklarację zmiennych wyrażeń (deklaracji zmiennych out i wzorców deklaracji) w inicjatorze ctor.</span><span class="sxs-lookup"><span data-stu-id="b1f65-107">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a ctor-initializer.</span></span> <span data-ttu-id="b1f65-108">Taka zadeklarowana zmienna znajduje się w zakresie w całej treści konstruktora.</span><span class="sxs-lookup"><span data-stu-id="b1f65-108">Such a declared variable is in scope throughout the body of the constructor.</span></span>

<span data-ttu-id="b1f65-109">Firma Microsoft usuwa ograniczenie uniemożliwiające deklarację zmiennych wyrażeń (deklaracji zmiennych out i wzorców deklaracji) w inicjatorze pola lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="b1f65-109">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a field or property initializer.</span></span> <span data-ttu-id="b1f65-110">Taka zadeklarowana zmienna znajduje się w zakresie w całym wyrażeniu inicjującym.</span><span class="sxs-lookup"><span data-stu-id="b1f65-110">Such a declared variable is in scope throughout the initializing expression.</span></span>

<span data-ttu-id="b1f65-111">Firma Microsoft usuwa ograniczenie uniemożliwiające deklarację zmiennych wyrażeń (deklaracji zmiennych out i wzorców deklaracji) w klauzuli wyrażenia zapytania, które są tłumaczone na treść lambda.</span><span class="sxs-lookup"><span data-stu-id="b1f65-111">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a query expression clause that is translated into the body of a lambda.</span></span> <span data-ttu-id="b1f65-112">Taka zadeklarowana zmienna znajduje się w zakresie w tym wyrażeniu klauzuli zapytania.</span><span class="sxs-lookup"><span data-stu-id="b1f65-112">Such a declared variable is in scope throughout that expression of the query clause.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="b1f65-113">Wady</span><span class="sxs-lookup"><span data-stu-id="b1f65-113">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="b1f65-114">Brak.</span><span class="sxs-lookup"><span data-stu-id="b1f65-114">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="b1f65-115">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="b1f65-115">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="b1f65-116">Odpowiedni zakres zmiennych wyrażeń zadeklarowanych w tych kontekstach nie jest oczywisty i zachowuje dalsze dyskusje LDM.</span><span class="sxs-lookup"><span data-stu-id="b1f65-116">The appropriate scope for expression variables declared in these contexts is not obvious, and deserves further LDM discussion.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="b1f65-117">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="b1f65-117">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="b1f65-118">[] Jakie są odpowiednie zakresy dla tych zmiennych?</span><span class="sxs-lookup"><span data-stu-id="b1f65-118">[ ] What is the appropriate scope for these variables?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="b1f65-119">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="b1f65-119">Design meetings</span></span>

<span data-ttu-id="b1f65-120">Brak.</span><span class="sxs-lookup"><span data-stu-id="b1f65-120">None.</span></span>
