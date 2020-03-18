---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484441"
---
# <a name="local-functions"></a><span data-ttu-id="a41c9-101">Funkcje lokalne</span><span class="sxs-lookup"><span data-stu-id="a41c9-101">Local functions</span></span>

<span data-ttu-id="a41c9-102">Rozszerzono C# obsługę deklaracji funkcji w zakresie bloku.</span><span class="sxs-lookup"><span data-stu-id="a41c9-102">We extend C# to support the declaration of functions in block scope.</span></span> <span data-ttu-id="a41c9-103">Funkcje lokalne mogą używać zmiennych (przechwytywania) z otaczającego zakresu.</span><span class="sxs-lookup"><span data-stu-id="a41c9-103">Local functions may use (capture) variables from the enclosing scope.</span></span>

<span data-ttu-id="a41c9-104">Kompilator używa analizy przepływu, aby wykryć zmienne, których używa funkcja lokalna przed przypisaniem do niej wartości.</span><span class="sxs-lookup"><span data-stu-id="a41c9-104">The compiler uses flow analysis to detect which variables a local function uses before assigning it a value.</span></span> <span data-ttu-id="a41c9-105">Każde wywołanie funkcji wymaga, aby zmienne były ostatecznie przypisane.</span><span class="sxs-lookup"><span data-stu-id="a41c9-105">Every call of the function requires such variables to be definitely assigned.</span></span> <span data-ttu-id="a41c9-106">Podobnie kompilator określa, które zmienne są ostatecznie przypisane do powrotu.</span><span class="sxs-lookup"><span data-stu-id="a41c9-106">Similarly the compiler determines which variables are definitely assigned on return.</span></span> <span data-ttu-id="a41c9-107">Takie zmienne są uważane za ostatecznie przypisane po wywołaniu funkcji lokalnej.</span><span class="sxs-lookup"><span data-stu-id="a41c9-107">Such variables are considered definitely assigned after the local function is invoked.</span></span>

<span data-ttu-id="a41c9-108">Funkcje lokalne mogą być wywoływane z punktu leksykalnego przed jego definicją.</span><span class="sxs-lookup"><span data-stu-id="a41c9-108">Local functions may be called from a lexical point before its definition.</span></span> <span data-ttu-id="a41c9-109">Instrukcje deklaracji funkcji lokalnych nie powodują ostrzeżenia, gdy nie są dostępne.</span><span class="sxs-lookup"><span data-stu-id="a41c9-109">Local function declaration statements do not cause a warning when they are not reachable.</span></span>

<span data-ttu-id="a41c9-110">do zrobienia: _zapis specyfikacji_</span><span class="sxs-lookup"><span data-stu-id="a41c9-110">TODO: _WRITE SPEC_</span></span>

## <a name="syntax-grammar"></a><span data-ttu-id="a41c9-111">Gramatyka składni</span><span class="sxs-lookup"><span data-stu-id="a41c9-111">Syntax grammar</span></span>

<span data-ttu-id="a41c9-112">Ta Gramatyka jest reprezentowana jako różnica w stosunku do bieżącej gramatyki specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="a41c9-112">This grammar is represented as a diff from the current spec grammar.</span></span>

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

<span data-ttu-id="a41c9-113">Funkcje lokalne mogą używać zmiennych zdefiniowanych w zakresie otaczającym.</span><span class="sxs-lookup"><span data-stu-id="a41c9-113">Local functions may use variables defined in the enclosing scope.</span></span> <span data-ttu-id="a41c9-114">Bieżąca implementacja wymaga, aby każda zmienna odczytana wewnątrz funkcji lokalnej była ostatecznie przypisana, tak jakby wykonywała funkcję lokalną w punkcie definicji.</span><span class="sxs-lookup"><span data-stu-id="a41c9-114">The current implementation requires that every variable read inside a local function be definitely assigned, as if executing the local function at its point of definition.</span></span> <span data-ttu-id="a41c9-115">Ponadto definicja funkcji lokalnej musi być "wykonywana" w dowolnym punkcie użycia.</span><span class="sxs-lookup"><span data-stu-id="a41c9-115">Also, the local function definition must have been "executed" at any use point.</span></span>

<span data-ttu-id="a41c9-116">Po przeprowadzeniu eksperymentowania z tym bitem (na przykład nie jest możliwe zdefiniowanie dwóch wzajemnie rekurencyjnych funkcji lokalnych), od momentu skorygowania, jak chcemy, aby określone przypisanie działało.</span><span class="sxs-lookup"><span data-stu-id="a41c9-116">After experimenting with that a bit (for example, it is not possible to define two mutually recursive local functions), we've since revised how we want the definite assignment to work.</span></span> <span data-ttu-id="a41c9-117">Poprawka (jeszcze nie zaimplementowana) polega na tym, że wszystkie zmienne lokalne odczytywane w funkcji lokalnej muszą być ostatecznie przypisane do każdego wywołania funkcji lokalnej.</span><span class="sxs-lookup"><span data-stu-id="a41c9-117">The revision (not yet implemented) is that all local variables read in a local function must be definitely assigned at each invocation of the local function.</span></span> <span data-ttu-id="a41c9-118">Jest to naprawdę bardziej delikatne niż dźwięki IT i istnieje wiele zadań, które pozostało do działania.</span><span class="sxs-lookup"><span data-stu-id="a41c9-118">That's actually more subtle than it sounds, and there is a bunch of work remaining to make it work.</span></span> <span data-ttu-id="a41c9-119">Gdy wszystko będzie gotowe, będzie można przenieść funkcje lokalne na koniec otaczającego go bloku.</span><span class="sxs-lookup"><span data-stu-id="a41c9-119">Once it is done you'll be able to move your local functions to the end of its enclosing block.</span></span>

<span data-ttu-id="a41c9-120">Nowe określone reguły przypisywania są niezgodne z wywnioskowania zwracanego typu funkcji lokalnej, więc prawdopodobnie zostanie usunięta pomoc techniczna do wywnioskowania typu zwracanego.</span><span class="sxs-lookup"><span data-stu-id="a41c9-120">The new definite assignment rules are incompatible with inferring the return type of a local function, so we'll likely be removing support for inferring the return type.</span></span>

<span data-ttu-id="a41c9-121">Jeśli funkcja lokalna nie zostanie przekonwertowana na delegata, przechwytywanie odbywa się w ramkach, które są typami wartości.</span><span class="sxs-lookup"><span data-stu-id="a41c9-121">Unless you convert a local function to a delegate, capturing is done into frames that are value types.</span></span> <span data-ttu-id="a41c9-122">Oznacza to, że nie otrzymujesz żadnego nacisku na odzyskiwanie z używania funkcji lokalnych z funkcją przechwytywania.</span><span class="sxs-lookup"><span data-stu-id="a41c9-122">That means you don't get any GC pressure from using local functions with capturing.</span></span>

### <a name="reachability"></a><span data-ttu-id="a41c9-123">Dostępność</span><span class="sxs-lookup"><span data-stu-id="a41c9-123">Reachability</span></span>

<span data-ttu-id="a41c9-124">Dodamy do specyfikacji</span><span class="sxs-lookup"><span data-stu-id="a41c9-124">We add to the spec</span></span>

> <span data-ttu-id="a41c9-125">Treść wyrażenia lambda z instrukcją lub funkcją lokalną jest uznawana za osiągalną.</span><span class="sxs-lookup"><span data-stu-id="a41c9-125">The body of a statement-bodied lambda expression or local function is considered reachable.</span></span>
