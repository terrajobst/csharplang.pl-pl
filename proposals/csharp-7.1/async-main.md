---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484798"
---
# <a name="async-main"></a><span data-ttu-id="a99bf-101">Asynchroniczny, główny</span><span class="sxs-lookup"><span data-stu-id="a99bf-101">Async Main</span></span>

* <span data-ttu-id="a99bf-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="a99bf-102">[x] Proposed</span></span>
* <span data-ttu-id="a99bf-103">[] Prototyp</span><span class="sxs-lookup"><span data-stu-id="a99bf-103">[ ] Prototype</span></span>
* <span data-ttu-id="a99bf-104">[] Implementacja</span><span class="sxs-lookup"><span data-stu-id="a99bf-104">[ ] Implementation</span></span>
* <span data-ttu-id="a99bf-105">[] — Specyfikacja</span><span class="sxs-lookup"><span data-stu-id="a99bf-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="a99bf-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a99bf-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a99bf-107">Zezwalaj na używanie `await` w metodzie głównej/EntryPoint aplikacji przez umożliwienie, aby punkt wejścia zwracał `Task` / `Task<int>` i być oznaczony `async`.</span><span class="sxs-lookup"><span data-stu-id="a99bf-107">Allow `await` to be used in an application's Main / entrypoint method by allowing the entrypoint to return `Task` / `Task<int>` and be marked `async`.</span></span>

## <a name="motivation"></a><span data-ttu-id="a99bf-108">Motywacją</span><span class="sxs-lookup"><span data-stu-id="a99bf-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a99bf-109">Jest bardzo powszechny podczas uczenia C#się, podczas pisania narzędzi opartych na konsoli i podczas pisania małych aplikacji testowych w celu wywołania i `await` metod `async` z poziomu głównego.</span><span class="sxs-lookup"><span data-stu-id="a99bf-109">It is very common when learning C#, when writing console-based utilities, and when writing small test apps to want to call and `await` `async` methods from Main.</span></span>  <span data-ttu-id="a99bf-110">Obecnie dodaliśmy poziom złożoności w tym miejscu, wymuszając takie `await`, aby można było wykonać w oddzielnym metodzie asynchronicznej, co sprawia, że deweloperzy muszą pisać zwyczajowo, takie jak następujące:</span><span class="sxs-lookup"><span data-stu-id="a99bf-110">Today we add a level of complexity here by forcing such `await`'ing to be done in a separate async method, which causes developers to need to write boilerplate like the following just to get started:</span></span>

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

<span data-ttu-id="a99bf-111">Firma Microsoft może usunąć konieczność stosowania tego standardowego i ułatwić rozpoczęcie pracy po prostu przez umożliwienie głównej `async`, w taki sposób, aby można było używać `await`s.</span><span class="sxs-lookup"><span data-stu-id="a99bf-111">We can remove the need for this boilerplate and make it easier to get started simply by allowing Main itself to be `async` such that `await`s can be used in it.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a99bf-112">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="a99bf-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a99bf-113">Następujące podpisy są obecnie dozwolone dla punktu wejścia:</span><span class="sxs-lookup"><span data-stu-id="a99bf-113">The following signatures are currently allowed entrypoints:</span></span>

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

<span data-ttu-id="a99bf-114">Rozszerzono listę dozwolonych elementów wejścia do uwzględnienia:</span><span class="sxs-lookup"><span data-stu-id="a99bf-114">We extend the list of allowed entrypoints to include:</span></span>

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

<span data-ttu-id="a99bf-115">Aby uniknąć zagrożeń związanych ze zgodnością, nowe sygnatury będą traktowane jako prawidłowy punkt wejścia, jeśli nie są obecne przeciążenia poprzedniego zestawu.</span><span class="sxs-lookup"><span data-stu-id="a99bf-115">To avoid compatibility risks, these new signatures will only be considered as valid entrypoints if no overloads of the previous set are present.</span></span>
<span data-ttu-id="a99bf-116">Język/kompilator nie wymaga, aby punkt wejścia był oznaczony jako `async`, ale oczekuje się, że większość z nich zostanie oznaczona jako taka.</span><span class="sxs-lookup"><span data-stu-id="a99bf-116">The language / compiler will not require that the entrypoint be marked as `async`, though we expect the vast majority of uses will be marked as such.</span></span>

<span data-ttu-id="a99bf-117">Gdy jeden z tych elementów jest zidentyfikowany jako punkt wejścia, kompilator wykryje rzeczywistą metodę punktu wejścia, która wywołuje jedną z następujących zakodowanych metod:</span><span class="sxs-lookup"><span data-stu-id="a99bf-117">When one of these is identified as the entrypoint, the compiler will synthesize an actual entrypoint method that calls one of these coded methods:</span></span>
- <span data-ttu-id="a99bf-118">```static Task Main()``` spowoduje, że kompilator emituje odpowiednik ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="a99bf-118">```static Task Main()``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="a99bf-119">```static Task Main(string[])``` spowoduje, że kompilator emituje odpowiednik ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="a99bf-119">```static Task Main(string[])``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="a99bf-120">```static Task<int> Main()``` spowoduje, że kompilator emituje odpowiednik ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="a99bf-120">```static Task<int> Main()``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="a99bf-121">```static Task<int> Main(string[])``` spowoduje, że kompilator emituje odpowiednik ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="a99bf-121">```static Task<int> Main(string[])``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>

<span data-ttu-id="a99bf-122">Przykładowe użycie:</span><span class="sxs-lookup"><span data-stu-id="a99bf-122">Example usage:</span></span>

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a><span data-ttu-id="a99bf-123">Wady</span><span class="sxs-lookup"><span data-stu-id="a99bf-123">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a99bf-124">Główna wadą jest po prostu dodatkową złożonością obsługi dodatkowych sygnatur wejścia/wyjścia.</span><span class="sxs-lookup"><span data-stu-id="a99bf-124">The main drawback is simply the additional complexity of supporting additional entrypoint signatures.</span></span>

## <a name="alternatives"></a><span data-ttu-id="a99bf-125">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="a99bf-125">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a99bf-126">Inne rozważane warianty:</span><span class="sxs-lookup"><span data-stu-id="a99bf-126">Other variants considered:</span></span>

<span data-ttu-id="a99bf-127">Zezwalanie na `async void`.</span><span class="sxs-lookup"><span data-stu-id="a99bf-127">Allowing `async void`.</span></span>  <span data-ttu-id="a99bf-128">Musimy zachować semantykę identyczną dla kodu wywołującego go bezpośrednio, co może utrudnić wygenerowanie go przez wygenerowany punkt wejścia (bez zwrócenia zadania).</span><span class="sxs-lookup"><span data-stu-id="a99bf-128">We need to keep the semantics the same for code calling it directly, which would then make it difficult for a generated entrypoint to call it (no Task returned).</span></span>  <span data-ttu-id="a99bf-129">Możemy rozwiązać ten problem, generując dwie inne metody, np.</span><span class="sxs-lookup"><span data-stu-id="a99bf-129">We could solve this by generating two other methods, e.g.</span></span>

```csharp
public static async void Main()
{
   ... // await code
}
```

<span data-ttu-id="a99bf-130">stanowi</span><span class="sxs-lookup"><span data-stu-id="a99bf-130">becomes</span></span>

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

<span data-ttu-id="a99bf-131">Istnieją również kwestie dotyczące promowania użycia `async void`.</span><span class="sxs-lookup"><span data-stu-id="a99bf-131">There are also concerns around encouraging usage of `async void`.</span></span>

<span data-ttu-id="a99bf-132">Użycie "MainAsync" zamiast "Main" jako nazwy.</span><span class="sxs-lookup"><span data-stu-id="a99bf-132">Using "MainAsync" instead of "Main" as the name.</span></span>  <span data-ttu-id="a99bf-133">Chociaż sufiks Async jest zalecany w przypadku metod zwracających zadania, to przede wszystkim funkcje biblioteki, które główne nie są, i obsługujące dodatkowe nazwy punktu wejścia poza "Main" nie są takie same.</span><span class="sxs-lookup"><span data-stu-id="a99bf-133">While the async suffix is recommended for Task-returning methods, that's primarily about library functionality, which Main is not, and supporting additional entrypoint names beyond "Main" is not worth it.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="a99bf-134">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="a99bf-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="a99bf-135">Nie dotyczy</span><span class="sxs-lookup"><span data-stu-id="a99bf-135">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="a99bf-136">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="a99bf-136">Design meetings</span></span>

<span data-ttu-id="a99bf-137">Nie dotyczy</span><span class="sxs-lookup"><span data-stu-id="a99bf-137">n/a</span></span>
