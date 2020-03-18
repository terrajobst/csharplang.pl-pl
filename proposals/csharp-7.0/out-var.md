---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484511"
---
# <a name="out-variable-declarations"></a><span data-ttu-id="62b5b-101">Deklaracje zmiennych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="62b5b-101">Out variable declarations</span></span>

<span data-ttu-id="62b5b-102">Funkcja *deklaracji zmiennej out* umożliwia zadeklarowanie zmiennej w lokalizacji, w której jest ona przenoszona jako argument `out`.</span><span class="sxs-lookup"><span data-stu-id="62b5b-102">The *out variable declaration* feature enables a variable to be declared at the location that it is being passed as an `out` argument.</span></span>

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

<span data-ttu-id="62b5b-103">Zmienna zadeklarowana w ten sposób jest nazywana *zmienną out*.</span><span class="sxs-lookup"><span data-stu-id="62b5b-103">A variable declared this way is called an *out variable*.</span></span> <span data-ttu-id="62b5b-104">Dla typu zmiennej można użyć `var` kontekstowego słowa kluczowego.</span><span class="sxs-lookup"><span data-stu-id="62b5b-104">You may use the contextual keyword `var` for the variable's type.</span></span> <span data-ttu-id="62b5b-105">Zakres będzie taki sam jak w przypadku *zmiennej wzorca* wprowadzonej przy użyciu dopasowania do wzorca.</span><span class="sxs-lookup"><span data-stu-id="62b5b-105">The scope will be the same as for a *pattern-variable* introduced via pattern-matching.</span></span>

<span data-ttu-id="62b5b-106">Zgodnie z specyfikacją języka (w sekcji 7.6.7 dostęp do elementu) lista argumentów dla dostępu do elementu (wyrażenie indeksowania) nie zawiera argumentów ref ani out.</span><span class="sxs-lookup"><span data-stu-id="62b5b-106">According to Language Specification (section 7.6.7 Element access) the argument-list of an element-access (indexing expression) does not contain ref or out arguments.</span></span> <span data-ttu-id="62b5b-107">Jednak są one dozwolone przez kompilator dla różnych scenariuszy, na przykład indeksatory zadeklarowane w metadanych, które akceptują `out`.</span><span class="sxs-lookup"><span data-stu-id="62b5b-107">However, they are permitted by the compiler for various scenarios, for example indexers declared in metadata that accept `out`.</span></span>

<span data-ttu-id="62b5b-108">W zakresie zmiennej lokalnej wprowadzonej przez argument_value jest to błąd czasu kompilacji, który odwołuje się do tej zmiennej lokalnej w pozycji tekstowej, która poprzedza jej deklarację.</span><span class="sxs-lookup"><span data-stu-id="62b5b-108">Within the scope of a local variable introduced by an argument_value, it is a compile-time error to refer to that local variable in a textual position that precedes its declaration.</span></span>

<span data-ttu-id="62b5b-109">Występuje również błąd odwołujący się do zmiennej out o typie określonym niejawnie (§ 8.5.1) na tej samej liście argumentów, która bezpośrednio zawiera jego deklarację.</span><span class="sxs-lookup"><span data-stu-id="62b5b-109">It is also an error to reference an implicitly-typed (§8.5.1) out variable in the same argument list that immediately contains its declaration.</span></span>

<span data-ttu-id="62b5b-110">Rozpoznawanie przeciążenia jest modyfikowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="62b5b-110">Overload resolution is modified as follows:</span></span>

<span data-ttu-id="62b5b-111">Dodamy nową konwersję:</span><span class="sxs-lookup"><span data-stu-id="62b5b-111">We add a new conversion:</span></span>

> <span data-ttu-id="62b5b-112">Istnieje *Konwersja z wyrażenia* z deklaracji zmiennej niejawnie wpisanej do każdego typu.</span><span class="sxs-lookup"><span data-stu-id="62b5b-112">There is a *conversion from expression* from an implicitly-typed out variable declaration to every type.</span></span>

<span data-ttu-id="62b5b-113">Być</span><span class="sxs-lookup"><span data-stu-id="62b5b-113">Also</span></span>

> <span data-ttu-id="62b5b-114">Typ argumentu zmiennej wyjściowej jawnie wpisanej jest zadeklarowanym typem.</span><span class="sxs-lookup"><span data-stu-id="62b5b-114">The type of an explicitly-typed out variable argument is the declared type.</span></span>

<span data-ttu-id="62b5b-115">i</span><span class="sxs-lookup"><span data-stu-id="62b5b-115">and</span></span>

> <span data-ttu-id="62b5b-116">Argument zmiennej wyjściowej z niejawnym typem nie ma typu.</span><span class="sxs-lookup"><span data-stu-id="62b5b-116">An implicitly-typed out variable argument has no type.</span></span>

<span data-ttu-id="62b5b-117">*Konwersja z wyrażenia* z niejawnie wpisanej zmiennej out nie jest uważana za lepszą niż jakakolwiek inna *Konwersja z wyrażenia*.</span><span class="sxs-lookup"><span data-stu-id="62b5b-117">The *conversion from expression* from an implicitly-typed out variable declaration is not considered better than any other *conversion from expression*.</span></span>

<span data-ttu-id="62b5b-118">Typ zmiennej out niejawnie wpisanej jest typem odpowiadającego parametru w podpisie metody wybranej przez metodę rozpoznawania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="62b5b-118">The type of an implicitly-typed out variable is the type of the corresponding parameter in the signature of the method selected by overload resolution.</span></span>

<span data-ttu-id="62b5b-119">Nowa `DeclarationExpressionSyntax` węzła składni zostanie dodana do reprezentowania deklaracji w argumencie out var.</span><span class="sxs-lookup"><span data-stu-id="62b5b-119">The new syntax node `DeclarationExpressionSyntax` is added to represent the declaration in an out var argument.</span></span>
