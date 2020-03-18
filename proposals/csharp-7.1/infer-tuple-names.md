---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484721"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a><span data-ttu-id="a8bf8-101">Wywnioskowanie nazw krotek (alias.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-101">Infer tuple names (aka.</span></span> <span data-ttu-id="a8bf8-102">inicjatory projekcji krotki)</span><span class="sxs-lookup"><span data-stu-id="a8bf8-102">tuple projection initializers)</span></span>

## <a name="summary"></a><span data-ttu-id="a8bf8-103">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a8bf8-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a8bf8-104">W wielu typowych przypadkach ta funkcja umożliwia pominięcie nazw elementów krotki i ich wywnioskowanie.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-104">In a number of common cases, this feature allows the tuple element names to be omitted and instead be inferred.</span></span> <span data-ttu-id="a8bf8-105">Na przykład zamiast wpisywać `(f1: x.f1, f2: x?.f2)`, można wywnioskować nazwy elementów "F1" i "F2" z `(x.f1, x?.f2)`.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-105">For instance, instead of typing `(f1: x.f1, f2: x?.f2)`, the element names "f1" and "f2" can be inferred from `(x.f1, x?.f2)`.</span></span>

<span data-ttu-id="a8bf8-106">Powoduje to równoległe zachowanie typów anonimowych, które umożliwiają wnioskowanie nazw członków podczas tworzenia.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-106">This parallels the behavior of  anonymous types, which allow inferring member names during creation.</span></span> <span data-ttu-id="a8bf8-107">Na przykład `new { x.f1, y?.f2 }` deklaruje członków "F1" i "F2".</span><span class="sxs-lookup"><span data-stu-id="a8bf8-107">For instance, `new { x.f1, y?.f2 }` declares members "f1" and "f2".</span></span>

<span data-ttu-id="a8bf8-108">Jest to szczególnie przydatne w przypadku używania krotek w LINQ:</span><span class="sxs-lookup"><span data-stu-id="a8bf8-108">This is particularly handy when using tuples in LINQ:</span></span>

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a><span data-ttu-id="a8bf8-109">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="a8bf8-109">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a8bf8-110">Istnieją dwie części zmiany:</span><span class="sxs-lookup"><span data-stu-id="a8bf8-110">There are two parts to the change:</span></span>

1.  <span data-ttu-id="a8bf8-111">Spróbuj wnioskować o nazwę kandydującą dla każdego elementu krotki, który nie ma jawnej nazwy:</span><span class="sxs-lookup"><span data-stu-id="a8bf8-111">Try to infer a candidate name for each tuple element which does not have an explicit name:</span></span>
    -   <span data-ttu-id="a8bf8-112">Używanie tych samych reguł jako wnioskowania o nazwach dla typów anonimowych.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-112">Using same rules as name inference for anonymous types.</span></span>
        - <span data-ttu-id="a8bf8-113">W C#programie można to zrobić w trzech przypadkach: `y` (identyfikator), `x.y` (prosty dostęp do elementów członkowskich) i `x?.y` (dostęp warunkowy).</span><span class="sxs-lookup"><span data-stu-id="a8bf8-113">In C#, this allows three cases: `y` (identifier), `x.y` (simple member access) and `x?.y` (conditional access).</span></span>
        - <span data-ttu-id="a8bf8-114">W języku VB pozwala to na dodatkowe przypadki, takie jak `x.y()`.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-114">In VB, this allows for additional cases, such as `x.y()`.</span></span>
    -   <span data-ttu-id="a8bf8-115">Odrzucanie zarezerwowanych nazw krotek (z uwzględnieniem wielkości liter w C#, bez uwzględniania wielkości liter w języku VB), ponieważ są one zabronione lub już niejawne.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-115">Rejecting reserved tuple names (case-sensitive in C#, case-insensitive in VB), as they are either forbidden or already implicit.</span></span> <span data-ttu-id="a8bf8-116">Na przykład takie jak `ItemN`, `Rest`i `ToString`.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-116">For instance, such as `ItemN`, `Rest`, and `ToString`.</span></span>
    -   <span data-ttu-id="a8bf8-117">Jeśli nazwy kandydatów są zduplikowane (z uwzględnieniem wielkości liter C#w, w języku VB, bez uwzględniania wielkości liter), należy porzucić te kandydaci,</span><span class="sxs-lookup"><span data-stu-id="a8bf8-117">If any candidate names are duplicates (case-sensitive in C#, case-insensitive in VB) within the entire tuple, we drop those candidates,</span></span>
2.  <span data-ttu-id="a8bf8-118">Podczas konwersji (które sprawdzają i ostrzegają o upuszczaniu nazw z literałów krotek), wywnioskowane nazwy nie generują żadnych ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-118">During conversions (which check and warn about dropping names from tuple literals), inferred names would not produce any warnings.</span></span> <span data-ttu-id="a8bf8-119">Pozwala to uniknąć przerwania istniejącego kodu krotki.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-119">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="a8bf8-120">Należy zauważyć, że reguła obsługi duplikatów jest inna niż w przypadku typów anonimowych.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-120">Note that the rule for handling duplicates is different than that for anonymous types.</span></span> <span data-ttu-id="a8bf8-121">Na przykład `new { x.f1, x.f1 }` generuje błąd, ale nadal będzie można używać `(x.f1, x.f1)` (tylko bez nazw odroczonych).</span><span class="sxs-lookup"><span data-stu-id="a8bf8-121">For instance, `new { x.f1, x.f1 }` produces an error, but `(x.f1, x.f1)` would still be allowed (just without any inferred names).</span></span> <span data-ttu-id="a8bf8-122">Pozwala to uniknąć przerwania istniejącego kodu krotki.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-122">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="a8bf8-123">W celu zachowania spójności taka sama zostałaby zastosowana w przypadku krotek utworzonych przez dekonstrukcja przypisań (w C#):</span><span class="sxs-lookup"><span data-stu-id="a8bf8-123">For consistency, the same would apply to tuples produced by deconstruction-assignments (in C#):</span></span>

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

<span data-ttu-id="a8bf8-124">To samo dotyczy krotek w języku VB, przy użyciu reguł specyficznych dla języka VB do wnioskowania nazwy z wyrażenia i nazwy bez uwzględniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-124">The same would also apply to VB tuples, using the VB-specific rules for inferring name from expression and case-insensitive name comparisons.</span></span>

<span data-ttu-id="a8bf8-125">W przypadku korzystania C# z kompilatora 7,1 (lub nowszego) z wersją językową "7,0" nazwy elementów zostaną wywnioskowane (bez dostępności funkcji), ale w celu uzyskania dostępu do nich wystąpi błąd użycia lokacji.</span><span class="sxs-lookup"><span data-stu-id="a8bf8-125">When using the C# 7.1 compiler (or later) with language version "7.0", the element names will be inferred (despite the feature not being available), but there will be a use-site error for trying to access them.</span></span> <span data-ttu-id="a8bf8-126">Spowoduje to ograniczenie dodawania nowego kodu, który będzie później narażony na problem ze zgodnością (opisany poniżej).</span><span class="sxs-lookup"><span data-stu-id="a8bf8-126">This will limit additions of new code that would later face the compatibility issue (described below).</span></span>

## <a name="drawbacks"></a><span data-ttu-id="a8bf8-127">Wady</span><span class="sxs-lookup"><span data-stu-id="a8bf8-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a8bf8-128">Główna wada polega na tym, że nadano przerwanie C# zgodności z 7,0:</span><span class="sxs-lookup"><span data-stu-id="a8bf8-128">The main drawback is that this introduces a compatibility break from C# 7.0:</span></span>

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

<span data-ttu-id="a8bf8-129">Rada zgodności stwierdziła, że ten podział jest akceptowalny, biorąc pod względu na to, że jest ograniczony, C# i przedział czasu od momentu wysłania krotek (w 7,0).</span><span class="sxs-lookup"><span data-stu-id="a8bf8-129">The compatibility council found this break acceptable, given that it is limited and the time window since tuples shipped (in C# 7.0) is short.</span></span>

## <a name="references"></a><span data-ttu-id="a8bf8-130">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="a8bf8-130">References</span></span>
- [<span data-ttu-id="a8bf8-131">LDM Kwiecień, czwarty 2017</span><span class="sxs-lookup"><span data-stu-id="a8bf8-131">LDM April 4th 2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- <span data-ttu-id="a8bf8-132">[Dyskusja](https://github.com/dotnet/csharplang/issues/370) w witrynie GitHub (z podziękowaniami @alrz w celu wprowadzenia tego problemu)</span><span class="sxs-lookup"><span data-stu-id="a8bf8-132">[Github discussion](https://github.com/dotnet/csharplang/issues/370) (thanks @alrz for bringing this issue up)</span></span>
- [<span data-ttu-id="a8bf8-133">Projekt krotek</span><span class="sxs-lookup"><span data-stu-id="a8bf8-133">Tuples design</span></span>](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
