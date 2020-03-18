---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484672"
---
# <a name="non-trailing-named-arguments"></a><span data-ttu-id="ee184-101">Argumenty nazwane inne niż końcowe</span><span class="sxs-lookup"><span data-stu-id="ee184-101">Non-trailing named arguments</span></span>

## <a name="summary"></a><span data-ttu-id="ee184-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="ee184-102">Summary</span></span>
[summary]: #summary
<span data-ttu-id="ee184-103">Zezwalaj na używanie nazwanych argumentów w położeniu niekońcowym, o ile są one używane w poprawnej pozycji.</span><span class="sxs-lookup"><span data-stu-id="ee184-103">Allow named arguments to be used in non-trailing position, as long as they are used in their correct position.</span></span> <span data-ttu-id="ee184-104">Na przykład: `DoSomething(isEmployed:true, name, age);`.</span><span class="sxs-lookup"><span data-stu-id="ee184-104">For example: `DoSomething(isEmployed:true, name, age);`.</span></span>

## <a name="motivation"></a><span data-ttu-id="ee184-105">Motywacją</span><span class="sxs-lookup"><span data-stu-id="ee184-105">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ee184-106">Główną motywacją jest uniknięcie wpisywania nadmiarowych informacji.</span><span class="sxs-lookup"><span data-stu-id="ee184-106">The main motivation is to avoid typing redundant information.</span></span> <span data-ttu-id="ee184-107">Często należy nazwać argument, który jest literałem (na przykład `null`, `true`) na potrzeby wyjaśnienia kodu, a nie przekazywania argumentów poza kolejnością.</span><span class="sxs-lookup"><span data-stu-id="ee184-107">It is common to name an argument that is a literal (such as `null`, `true`) for the purpose of clarifying the code, rather than of passing arguments out-of-order.</span></span>
<span data-ttu-id="ee184-108">Jest to obecnie niedozwolone (`CS1738`), chyba że wszystkie poniższe argumenty są również nazwane.</span><span class="sxs-lookup"><span data-stu-id="ee184-108">That is currently disallowed (`CS1738`) unless all the following arguments are also named.</span></span>

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

<span data-ttu-id="ee184-109">Kilka dodatkowych przykładów:</span><span class="sxs-lookup"><span data-stu-id="ee184-109">Some additional examples:</span></span>
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

<span data-ttu-id="ee184-110">Będzie to również współpracowało z params:</span><span class="sxs-lookup"><span data-stu-id="ee184-110">This would also work with params:</span></span>
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a><span data-ttu-id="ee184-111">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="ee184-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="ee184-112">W § 7.5.1 (listy argumentów) Specyfikacja obecnie mówi:</span><span class="sxs-lookup"><span data-stu-id="ee184-112">In §7.5.1 (Argument lists), the spec currently says:</span></span>
> <span data-ttu-id="ee184-113">*Argument* o *nazwie argumentu* jest określany jako __argument nazwany__, podczas gdy *Argument* bez *argumentu-Name* jest __argumentem pozycyjnym__.</span><span class="sxs-lookup"><span data-stu-id="ee184-113">An *argument* with an *argument-name* is referred to as a __named argument__, whereas an *argument* without an *argument-name* is a __positional argument__.</span></span> <span data-ttu-id="ee184-114">Występuje błąd dla argumentu pozycyjnego, który ma być wyświetlany po nazwanym argumencie na *liście argumentów*.</span><span class="sxs-lookup"><span data-stu-id="ee184-114">It is an error for a positional argument to appear after a named argument in an *argument-list*.</span></span>

<span data-ttu-id="ee184-115">Celem tej oferty jest usunięcie tego błędu i zaktualizowanie reguł dotyczących znajdowania odpowiedniego parametru dla argumentu (§ 7.5.1.1):</span><span class="sxs-lookup"><span data-stu-id="ee184-115">The proposal is to remove this error and update the rules for finding the corresponding parameter for an argument (§7.5.1.1):</span></span>

<span data-ttu-id="ee184-116">Argumenty na liście argumentów konstruktorów wystąpień, metod, indeksatorów i delegatów:</span><span class="sxs-lookup"><span data-stu-id="ee184-116">Arguments in the argument-list of instance constructors, methods, indexers and delegates:</span></span>
- <span data-ttu-id="ee184-117">[istniejące reguły]</span><span class="sxs-lookup"><span data-stu-id="ee184-117">[existing rules]</span></span>
- <span data-ttu-id="ee184-118">Nienazwany argument odpowiada żadnemu parametrowi, gdy znajduje się po nazwie argumentu "out-of-position" lub nazwanego argumentu params.</span><span class="sxs-lookup"><span data-stu-id="ee184-118">An unnamed argument corresponds to no parameter when it is after an out-of-position named argument or a named params argument.</span></span>

<span data-ttu-id="ee184-119">W szczególności zapobiega to wywoływaniu `void M(bool a = true, bool b = true, bool c = true, );` z `M(c: false, valueB);`.</span><span class="sxs-lookup"><span data-stu-id="ee184-119">In particular, this prevents invoking `void M(bool a = true, bool b = true, bool c = true, );` with `M(c: false, valueB);`.</span></span> <span data-ttu-id="ee184-120">Pierwszy argument jest używany poza pozycją (argument jest używany w pierwszej pozycji, ale parametr o nazwie "c" znajduje się w trzecim miejscu), dlatego następujące argumenty powinny mieć nazwę.</span><span class="sxs-lookup"><span data-stu-id="ee184-120">The first argument is used out-of-position (the argument is used in first position, but the parameter named "c" is in third position), so the following arguments should be named.</span></span>

<span data-ttu-id="ee184-121">Innymi słowy, nazwane argumenty, które nie są końcowe są dozwolone tylko wtedy, gdy nazwa i pozycja powodują znalezienie tego samego odpowiadającego parametru.</span><span class="sxs-lookup"><span data-stu-id="ee184-121">In other words, non-trailing named arguments are only allowed when the name and the position result in finding the same corresponding parameter.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="ee184-122">Wady</span><span class="sxs-lookup"><span data-stu-id="ee184-122">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ee184-123">Ta propozycja zaostrza istniejące subtleties z nazwanymi argumentami w ramach rozpoznawania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="ee184-123">This proposal exacerbates existing subtleties with named arguments in overload resolution.</span></span> <span data-ttu-id="ee184-124">Przykład:</span><span class="sxs-lookup"><span data-stu-id="ee184-124">For instance:</span></span>

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

<span data-ttu-id="ee184-125">Tę sytuację można uzyskać dzisiaj, zamieniając parametry:</span><span class="sxs-lookup"><span data-stu-id="ee184-125">You could get this situation today by swapping the parameters:</span></span>

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

<span data-ttu-id="ee184-126">Podobnie, jeśli masz dwie metody `void M(int a, int b)` i `void M(int x, string y)`, pomylone wywołanie `M(x: 1, 2)` spowoduje wygenerowanie diagnostyki na podstawie drugiego przeciążenia ("nie można konwertować z" int "na" String "").</span><span class="sxs-lookup"><span data-stu-id="ee184-126">Similarly, if you have two methods `void M(int a, int b)` and `void M(int x, string y)`, the mistaken invocation `M(x: 1, 2)` will produce a diagnostic based on the second overload ("cannot convert from 'int' to 'string'").</span></span> <span data-ttu-id="ee184-127">Ten problem już istnieje, gdy nazwany argument jest używany w pozycji końcowej.</span><span class="sxs-lookup"><span data-stu-id="ee184-127">This problem already exists when the named argument is used in a trailing position.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ee184-128">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="ee184-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ee184-129">Istnieje kilka rozwiązań alternatywnych, które należy wziąć pod uwagę:</span><span class="sxs-lookup"><span data-stu-id="ee184-129">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="ee184-130">Stan quo</span><span class="sxs-lookup"><span data-stu-id="ee184-130">The status quo</span></span>
- <span data-ttu-id="ee184-131">Zapewnianie pomocy środowiska IDE do wypełniania wszystkich nazw końcowych argumentów podczas wpisywania konkretnej nazwy w środku.</span><span class="sxs-lookup"><span data-stu-id="ee184-131">Providing IDE assistance to fill-in all the names of trailing arguments when you type specific a name in the middle.</span></span>

<span data-ttu-id="ee184-132">Oba te osoby poniosły więcej informacji o większej szczegółowości, ponieważ wprowadzają wiele nazwanych argumentów nawet wtedy, gdy potrzebna jest tylko jedna nazwa literału na początku listy argumentów.</span><span class="sxs-lookup"><span data-stu-id="ee184-132">Both of those suffer from more verbosity, as they introduce multiple named arguments even if you just need one name of a literal at the beginning of the argument list.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="ee184-133">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="ee184-133">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="ee184-134">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="ee184-134">Design meetings</span></span>
[ldm]: #ldm
<span data-ttu-id="ee184-135">Funkcja została krótko omówiona w programie LDM na 16 maja 2017, z zasadą zatwierdzania (OK, aby przenieść do propozycji/prototypu).</span><span class="sxs-lookup"><span data-stu-id="ee184-135">The feature was briefly discussed in LDM on May 16th 2017, with approval in principle (ok to move to proposal/prototype).</span></span> <span data-ttu-id="ee184-136">Został on również krótko omówiony w 28 czerwca 2017.</span><span class="sxs-lookup"><span data-stu-id="ee184-136">It was also briefly discussed on June 28th 2017.</span></span>

<span data-ttu-id="ee184-137">Odnosi się do początkowej dyskusji https://github.com/dotnet/csharplang/issues/518 odnosi się do problemu championed https://github.com/dotnet/csharplang/issues/570</span><span class="sxs-lookup"><span data-stu-id="ee184-137">Relates to initial discussion https://github.com/dotnet/csharplang/issues/518 Relates to championed issue https://github.com/dotnet/csharplang/issues/570</span></span>
