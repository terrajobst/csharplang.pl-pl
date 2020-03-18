---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484574"
---
# <a name="lambda-attributes"></a><span data-ttu-id="e6a90-101">Atrybuty lambda</span><span class="sxs-lookup"><span data-stu-id="e6a90-101">Lambda Attributes</span></span>

* <span data-ttu-id="e6a90-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="e6a90-102">[x] Proposed</span></span>
* <span data-ttu-id="e6a90-103">[] Prototyp</span><span class="sxs-lookup"><span data-stu-id="e6a90-103">[ ] Prototype</span></span>
* <span data-ttu-id="e6a90-104">[] Implementacja</span><span class="sxs-lookup"><span data-stu-id="e6a90-104">[ ] Implementation</span></span>
* <span data-ttu-id="e6a90-105">[] — Specyfikacja</span><span class="sxs-lookup"><span data-stu-id="e6a90-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="e6a90-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="e6a90-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="e6a90-107">Zezwalaj na stosowanie atrybutów do wyrażeń lambda (i metod anonimowych) oraz do parametrów metody lambda/anonimowej, ponieważ mogą one być stosowane w zwykłych metodach.</span><span class="sxs-lookup"><span data-stu-id="e6a90-107">Allow attributes to be applied to lambdas (and anonymous methods) and to lambda / anonymous method parameters, as they can be on regular methods.</span></span>

## <a name="motivation"></a><span data-ttu-id="e6a90-108">Motywacją</span><span class="sxs-lookup"><span data-stu-id="e6a90-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="e6a90-109">Dwie podstawowe motywacje:</span><span class="sxs-lookup"><span data-stu-id="e6a90-109">Two primary motivations:</span></span>

1. <span data-ttu-id="e6a90-110">Aby zapewnić widoczność metadanych dla analizatorów w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e6a90-110">To provide metadata visible to analyzers at compile-time.</span></span>
2. <span data-ttu-id="e6a90-111">Aby zapewnić, że metadane są widoczne dla odbicia i narzędzi w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e6a90-111">To provide metadata visible to reflection and tooling at run-time.</span></span>

<span data-ttu-id="e6a90-112">Przykładowo (1): w przypadku kodu wrażliwego na wydajność warto mieć możliwość zdefiniowania analizatora, który flaguje, gdy zamknięcia i Delegaty są przydzieleni dla wyrażeń lambda, które są blisko stanu.</span><span class="sxs-lookup"><span data-stu-id="e6a90-112">As an example of (1): For performance-sensitive code, it is helpful to be able to have an analyzer that flags when closures and delegates are being allocated for lambdas that close over state.</span></span>  <span data-ttu-id="e6a90-113">Często deweloper tego kodu będzie mógł uniknąć przechwytywania jakiegokolwiek stanu, aby kompilator mógł wygenerować metodę statyczną i delegatem pamięci podręcznej dla tej metody, lub deweloper upewni się, że jedynym zamykanym stanem jest `this`, co pozwala kompilatorowi co najmniej uniknąć przydziału obiektu zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="e6a90-113">Often a developer of such code will go out of his or her way to avoid capturing any state, so that the compiler can generate a static method and a cacheable delegate for the method, or the developer will ensure that the only state being closed over is `this`, allowing the compiler at least to avoid allocating a closure object.</span></span>  <span data-ttu-id="e6a90-114">Jednak bez obsługi języka w celu ograniczania tego, co może być przechwytywane, jest to, że wszystko jest zbyt łatwe do odróżnienia od stanu.</span><span class="sxs-lookup"><span data-stu-id="e6a90-114">But, without language support for limiting what may be captured, it is all too easy to accidentally close over state.</span></span>  <span data-ttu-id="e6a90-115">Byłoby to przydatne, jeśli deweloper może dodawać adnotacje lambda z atrybutami, aby wskazać, jaki stan można zamknąć, na przykład:</span><span class="sxs-lookup"><span data-stu-id="e6a90-115">It would be valuable if a developer could annotate lambdas with attributes to indicate what state they're allowed to close over, for example:</span></span>

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

<span data-ttu-id="e6a90-116">Następnie można napisać Analizator do flagi, gdy stan jest przechwytywany nieprawidłowo, na przykład:</span><span class="sxs-lookup"><span data-stu-id="e6a90-116">Then an analyzer can be written to flag when state is captured incorrectly, for example:</span></span>

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a><span data-ttu-id="e6a90-117">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="e6a90-117">Detailed design</span></span>
[design]: #detailed-design

- <span data-ttu-id="e6a90-118">Używając takiej samej składni atrybutu jak w przypadku normalnych metod, atrybuty mogą być stosowane na początku metody lambda lub anonimowej, na przykład:</span><span class="sxs-lookup"><span data-stu-id="e6a90-118">Using the same attribute syntax as on normal methods, attributes may be applied at the beginning of a lambda or anonymous method, for example:</span></span>

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- <span data-ttu-id="e6a90-119">Aby uniknąć niejednoznaczności w przypadku, gdy atrybut ma zastosowanie do metody lambda lub do jednego z argumentów, atrybuty mogą być używane tylko wtedy, gdy parens są używane wokół dowolnych argumentów, na przykład:</span><span class="sxs-lookup"><span data-stu-id="e6a90-119">To avoid ambiguity as to whether an attribute applies to the lambda method or to one of the arguments, attributes may only be used when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- <span data-ttu-id="e6a90-120">Przy użyciu metod anonimowych parens nie są konieczne w celu zastosowania atrybutu do metody przed słowem kluczowym `delegate`, na przykład:</span><span class="sxs-lookup"><span data-stu-id="e6a90-120">With anonymous methods, parens are not needed in order to apply an attribute to the method before the `delegate` keyword, for example:</span></span>

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- <span data-ttu-id="e6a90-121">Można zastosować wiele atrybutów, używając standardowej składni rozdzielanej przecinkami lub składni pełnego atrybutu, na przykład:</span><span class="sxs-lookup"><span data-stu-id="e6a90-121">Multiple attributes may be applied, either via standard comma-delimited syntax or via full-attribute syntax, for example:</span></span>

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- <span data-ttu-id="e6a90-122">Atrybuty mogą być stosowane do parametrów metody anonimowej lub wyrażenia lambda, ale tylko wtedy, gdy parens są używane wokół dowolnych argumentów, na przykład:</span><span class="sxs-lookup"><span data-stu-id="e6a90-122">Attributes may be applied to the parameters to an anonymous method or lambda, but only when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- <span data-ttu-id="e6a90-123">Wiele atrybutów można zastosować do parametrów metody anonimowej lub wyrażenia lambda przy użyciu składni rozdzielanej przecinkami lub pełnego atrybutu, na przykład:</span><span class="sxs-lookup"><span data-stu-id="e6a90-123">Multiple attributes may be applied to the parameters of an anonymous method or lambda, using either the comma-delimited or full-attribute syntax, for example:</span></span>

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- <span data-ttu-id="e6a90-124">atrybuty dostosowane do `return`mogą być również używane w wyrażeniach lambda, na przykład:</span><span class="sxs-lookup"><span data-stu-id="e6a90-124">`return`-targeted attributes may also be used on lambdas, for example:</span></span>

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- <span data-ttu-id="e6a90-125">Kompilator wyprowadza atrybuty do wygenerowanej metody i argumentów do tych metod, tak jak w przypadku innych metod.</span><span class="sxs-lookup"><span data-stu-id="e6a90-125">The compiler outputs the attributes onto the generated method and arguments to those methods as it would for any other method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="e6a90-126">Wady</span><span class="sxs-lookup"><span data-stu-id="e6a90-126">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="e6a90-127">Nie dotyczy</span><span class="sxs-lookup"><span data-stu-id="e6a90-127">n/a</span></span>

## <a name="alternatives"></a><span data-ttu-id="e6a90-128">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="e6a90-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="e6a90-129">Nie dotyczy</span><span class="sxs-lookup"><span data-stu-id="e6a90-129">n/a</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="e6a90-130">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="e6a90-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="e6a90-131">Nie dotyczy</span><span class="sxs-lookup"><span data-stu-id="e6a90-131">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="e6a90-132">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="e6a90-132">Design meetings</span></span>

<span data-ttu-id="e6a90-133">Nie dotyczy</span><span class="sxs-lookup"><span data-stu-id="e6a90-133">n/a</span></span>