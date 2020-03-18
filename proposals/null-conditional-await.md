---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484497"
---
# <a name="null-conditional-await"></a><span data-ttu-id="981e1-101">oczekiwanie na wartość null</span><span class="sxs-lookup"><span data-stu-id="981e1-101">null-conditional await</span></span>

* <span data-ttu-id="981e1-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="981e1-102">[x] Proposed</span></span>
* <span data-ttu-id="981e1-103">[] Prototype: brak</span><span class="sxs-lookup"><span data-stu-id="981e1-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="981e1-104">[] Implementacja: brak</span><span class="sxs-lookup"><span data-stu-id="981e1-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="981e1-105">[] — Specyfikacja: rozpoczęta, poniżej</span><span class="sxs-lookup"><span data-stu-id="981e1-105">[ ] Specification: Started, below</span></span>

## <a name="summary"></a><span data-ttu-id="981e1-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="981e1-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="981e1-107">Obsługuj wyrażenie `await? e`, które czeka `e`, jeśli ma wartość inną niż null, w przeciwnym razie powoduje `null`.</span><span class="sxs-lookup"><span data-stu-id="981e1-107">Support an expression of the form `await? e`, which awaits `e` if it is non-null, otherwise it results in `null`.</span></span>

## <a name="motivation"></a><span data-ttu-id="981e1-108">Motywacją</span><span class="sxs-lookup"><span data-stu-id="981e1-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="981e1-109">Jest to typowy wzorzec kodowania, a ta funkcja mogłaby mieć całkiem synergię z istniejącymi operatorami do łączenia wartości null i wartościami null.</span><span class="sxs-lookup"><span data-stu-id="981e1-109">This is a common coding pattern, and this feature would have nice synergy with the existing null-propagating and null-coalescing operators.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="981e1-110">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="981e1-110">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="981e1-111">Dodamy nową formę *await_expression*:</span><span class="sxs-lookup"><span data-stu-id="981e1-111">We add a new form of the *await_expression*:</span></span>

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

<span data-ttu-id="981e1-112">Operator `await` warunkowego null oczekuje na jego operand tylko wtedy, gdy ten operand ma wartość różną od null.</span><span class="sxs-lookup"><span data-stu-id="981e1-112">The null-conditional `await` operator awaits its operand only if that operand is non-null.</span></span> <span data-ttu-id="981e1-113">W przeciwnym razie wynik zastosowania operatora ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="981e1-113">Otherwise the result of applying the operator is null.</span></span>

<span data-ttu-id="981e1-114">Typ wyniku jest obliczany przy użyciu [reguł dla operatora warunkowego null](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span><span class="sxs-lookup"><span data-stu-id="981e1-114">The type of the result is computed using the [rules for the null-conditional operator](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span></span>

> <span data-ttu-id="981e1-115">**Uwaga:** Jeśli `e` jest typu `Task`, wówczas `await? e;` nie będzie nic robić, jeśli `e` jest `null`, i await `e`, jeśli nie jest `null`.</span><span class="sxs-lookup"><span data-stu-id="981e1-115">**NOTE:** If `e` is of type `Task`, then `await? e;` would do nothing if `e` is `null`, and await `e` if it is not `null`.</span></span>
>
> <span data-ttu-id="981e1-116">Jeśli `e` jest typu `Task<K>` gdzie `K` jest typem wartości, `await? e` będzie zwracać wartość typu `K?`.</span><span class="sxs-lookup"><span data-stu-id="981e1-116">If `e` is of type `Task<K>` where `K` is a value type, then `await? e` would yield a value of type `K?`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="981e1-117">Wady</span><span class="sxs-lookup"><span data-stu-id="981e1-117">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="981e1-118">Podobnie jak w przypadku dowolnej funkcji języka należy zwrócić uwagę na to, czy dodatkowa złożoność tego języka jest zapłacona w dodatkowym przejrzystości oferowanym C# przez funkcję.</span><span class="sxs-lookup"><span data-stu-id="981e1-118">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="981e1-119">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="981e1-119">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="981e1-120">Chociaż wymaga pewnego kodu, użycie tego operatora może być często zastąpione przez wyrażenie podobne do `(e == null) ? null : await e` lub instrukcji, takiej jak `if (e != null) await e`.</span><span class="sxs-lookup"><span data-stu-id="981e1-120">Although it requires some boilerplate code, uses of this operator can often be replaced by an expression something like `(e == null) ? null : await e` or a statement like `if (e != null) await e`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="981e1-121">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="981e1-121">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="981e1-122">[] Wymaga przeglądu LDM</span><span class="sxs-lookup"><span data-stu-id="981e1-122">[ ] Requires LDM review</span></span>

## <a name="design-meetings"></a><span data-ttu-id="981e1-123">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="981e1-123">Design meetings</span></span>

<span data-ttu-id="981e1-124">Brak.</span><span class="sxs-lookup"><span data-stu-id="981e1-124">None.</span></span>
