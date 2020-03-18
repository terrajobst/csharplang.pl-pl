---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484525"
---
# <a name="callerargumentexpression"></a><span data-ttu-id="f6ced-101">CallerArgumentExpression</span><span class="sxs-lookup"><span data-stu-id="f6ced-101">CallerArgumentExpression</span></span>

* <span data-ttu-id="f6ced-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="f6ced-102">[x] Proposed</span></span>
* <span data-ttu-id="f6ced-103">[] Prototyp: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="f6ced-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="f6ced-104">[] Implementacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="f6ced-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="f6ced-105">[] Specyfikacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="f6ced-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="f6ced-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="f6ced-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="f6ced-107">Zezwól deweloperom na przechwytywanie wyrażeń przesłanych do metody, aby umożliwić lepsze komunikaty o błędach w interfejsie API diagnostyki/testowania i zmniejsz liczbę naciśnięć klawiszy.</span><span class="sxs-lookup"><span data-stu-id="f6ced-107">Allow developers to capture the expressions passed to a method, to enable better error messages in diagnostic/testing APIs and reduce keystrokes.</span></span>

## <a name="motivation"></a><span data-ttu-id="f6ced-108">Motywacją</span><span class="sxs-lookup"><span data-stu-id="f6ced-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="f6ced-109">Gdy Walidacja potwierdzenia lub argumentu nie powiedzie się, deweloper chce wiedzieć, jak to możliwe, gdzie i dlaczego nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="f6ced-109">When an assertion or argument validation fails, the developer wants to know as much as possible about where and why it failed.</span></span> <span data-ttu-id="f6ced-110">Jednak dzisiejsze interfejsy API diagnostyki nie w pełni ułatwiają to.</span><span class="sxs-lookup"><span data-stu-id="f6ced-110">However, today's diagnostic APIs do not fully facilitate this.</span></span> <span data-ttu-id="f6ced-111">Weź pod uwagę następującą metodę:</span><span class="sxs-lookup"><span data-stu-id="f6ced-111">Consider the following method:</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

<span data-ttu-id="f6ced-112">W przypadku niepowodzenia jednego z potwierdzeń ślad stosu będzie zawierać tylko nazwy pliku, numeru wiersza i metody.</span><span class="sxs-lookup"><span data-stu-id="f6ced-112">When one of the asserts fail, only the filename, line number, and method name will be provided in the stack trace.</span></span> <span data-ttu-id="f6ced-113">Deweloper nie będzie w stanie stwierdzić, które potwierdzenie nie powiodło się z tych informacji — (-y) musi otworzyć plik i przejść do podanego numeru wiersza, aby zobaczyć, co poszło źle.</span><span class="sxs-lookup"><span data-stu-id="f6ced-113">The developer will not be able to tell which assert failed from this information-- (s)he will have to open the file and navigate to the provided line number to see what went wrong.</span></span>

<span data-ttu-id="f6ced-114">Wynika to również z tego, że struktury testowania muszą dostarczać różne metody Assert.</span><span class="sxs-lookup"><span data-stu-id="f6ced-114">This is also the reason testing frameworks have to provide a variety of assert methods.</span></span> <span data-ttu-id="f6ced-115">W przypadku xUnit, `Assert.True` i `Assert.False` nie są często używane, ponieważ nie zapewniają wystarczającej liczby kontekstów, co nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="f6ced-115">With xUnit, `Assert.True` and `Assert.False` are not frequently used because they do not provide enough context about what failed.</span></span>

<span data-ttu-id="f6ced-116">Chociaż sytuacja jest lepsza dla weryfikacji argumentów, ponieważ nazwy nieprawidłowych argumentów są wyświetlane dla dewelopera, Deweloper musi przekazać te nazwy do wyjątków ręcznie.</span><span class="sxs-lookup"><span data-stu-id="f6ced-116">While the situation is a bit better for argument validation because the names of invalid arguments are shown to the developer, the developer must pass these names to exceptions manually.</span></span> <span data-ttu-id="f6ced-117">Jeśli powyższy przykład został ponownie zapisany w celu użycia tradycyjnego sprawdzania poprawności argumentu zamiast `Debug.Assert`, będzie wyglądać tak</span><span class="sxs-lookup"><span data-stu-id="f6ced-117">If the above example were rewritten to use traditional argument validation instead of `Debug.Assert`, it would look like</span></span>

```csharp
T Single<T>(this T[] array)
{
    if (array == null)
    {
        throw new ArgumentNullException(nameof(array));
    }

    if (array.Length != 1)
    {
        throw new ArgumentException("Array must contain a single element.", nameof(array));
    }

    return array[0];
}
```

<span data-ttu-id="f6ced-118">Należy zauważyć, że `nameof(array)` musi być przekazywany do każdego wyjątku, chociaż jest już jasne z kontekstu, który jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="f6ced-118">Notice that `nameof(array)` must be passed to each exception, although it's already clear from context which argument is invalid.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="f6ced-119">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="f6ced-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="f6ced-120">W powyższych przykładach, w tym ciąg `"array != null"` lub `"array.Length == 1"` w komunikacie potwierdzenia może pomóc deweloperowi określić, co się nie powiodło.</span><span class="sxs-lookup"><span data-stu-id="f6ced-120">In the above examples, including the string `"array != null"` or `"array.Length == 1"` in the assert message would help the developer determine what failed.</span></span> <span data-ttu-id="f6ced-121">Wprowadź `CallerArgumentExpression`: jest to atrybut, który może być używany przez platformę do uzyskania ciągu skojarzonego z określonym argumentem metody.</span><span class="sxs-lookup"><span data-stu-id="f6ced-121">Enter `CallerArgumentExpression`: it's an attribute the framework can use to obtain the string associated with a particular method argument.</span></span> <span data-ttu-id="f6ced-122">Dodamy ją do `Debug.Assert` takich jak</span><span class="sxs-lookup"><span data-stu-id="f6ced-122">We would add it to `Debug.Assert` like so</span></span>

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

<span data-ttu-id="f6ced-123">Kod źródłowy w powyższym przykładzie pozostanie taki sam.</span><span class="sxs-lookup"><span data-stu-id="f6ced-123">The source code in the above example would stay the same.</span></span> <span data-ttu-id="f6ced-124">Jednak kod, który faktycznie emituje kompilator, będzie odpowiadać</span><span class="sxs-lookup"><span data-stu-id="f6ced-124">However, the code the compiler actually emits would correspond to</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

<span data-ttu-id="f6ced-125">Kompilator specjalnie rozpoznaje atrybut na `Debug.Assert`.</span><span class="sxs-lookup"><span data-stu-id="f6ced-125">The compiler specially recognizes the attribute on `Debug.Assert`.</span></span> <span data-ttu-id="f6ced-126">Przekazuje ciąg skojarzony z argumentem, do którego odwołuje się Konstruktor atrybutu (w tym przypadku `condition`) w witrynie wywołania.</span><span class="sxs-lookup"><span data-stu-id="f6ced-126">It passes the string associated with the argument referred to in the attribute's constructor (in this case, `condition`) at the call site.</span></span> <span data-ttu-id="f6ced-127">Gdy jedno z przyczyn nie powiedzie się, deweloper będzie pokazywać warunek, który był fałszywy i będzie wiedzieć, który z nich nie powiódł się.</span><span class="sxs-lookup"><span data-stu-id="f6ced-127">When either assert fails, the developer will be shown the condition that was false and will know which one failed.</span></span>

<span data-ttu-id="f6ced-128">W przypadku walidacji argumentu nie można używać atrybutu bezpośrednio, ale może być używany przez klasę pomocnika:</span><span class="sxs-lookup"><span data-stu-id="f6ced-128">For argument validation, the attribute cannot be used directly, but can be made use of through a helper class:</span></span>

```csharp
public static class Verify
{
    public static void Argument(bool condition, string message, [CallerArgumentExpression("condition")] string conditionExpression = null)
    {
        if (!condition) throw new ArgumentException(message: message, paramName: conditionExpression);
    }

    public static void InRange(int argument, int low, int high,
        [CallerArgumentExpression("argument")] string argumentExpression = null,
        [CallerArgumentExpression("low")] string lowExpression = null,
        [CallerArgumentExpression("high")] string highExpression = null)
    {
        if (argument < low)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be less than {lowExpression} ({low}).");
        }

        if (argument > high)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be greater than {highExpression} ({high}).");
        }
    }

    public static void NotNull<T>(T argument, [CallerArgumentExpression("argument")] string argumentExpression = null)
        where T : class
    {
        if (argument == null) throw new ArgumentNullException(paramName: argumentExpression);
    }
}

T Single<T>(this T[] array)
{
    Verify.NotNull(array); // paramName: "array"
    Verify.Argument(array.Length == 1, "Array must contain a single element."); // paramName: "array.Length == 1"

    return array[0];
}

T ElementAt(this T[] array, int index)
{
    Verify.NotNull(array); // paramName: "array"
    // paramName: "index"
    // message: "index (-1) cannot be less than 0 (0).", or
    //          "index (6) cannot be greater than array.Length - 1 (5)."
    Verify.InRange(index, 0, array.Length - 1);

    return array[index];
}
```

<span data-ttu-id="f6ced-129">Propozycja dodania takiej klasy pomocnika do struktury jest w toku https://github.com/dotnet/corefx/issues/17068.</span><span class="sxs-lookup"><span data-stu-id="f6ced-129">A proposal to add such a helper class to the framework is underway at https://github.com/dotnet/corefx/issues/17068.</span></span> <span data-ttu-id="f6ced-130">Jeśli ta funkcja języka została zaimplementowana, można zaktualizować propozycję, aby skorzystać z tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="f6ced-130">If this language feature was implemented, the proposal could be updated to take advantage of this feature.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="f6ced-131">Metody rozszerzające</span><span class="sxs-lookup"><span data-stu-id="f6ced-131">Extension methods</span></span>

<span data-ttu-id="f6ced-132">`CallerArgumentExpression`można odwoływać się do parametru `this` w metodzie rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="f6ced-132">The `this` parameter in an extension method may be referenced by `CallerArgumentExpression`.</span></span> <span data-ttu-id="f6ced-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f6ced-133">For example:</span></span>

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

<span data-ttu-id="f6ced-134">`thisExpression` otrzyma wyrażenie odpowiadające obiektowi przed kropką.</span><span class="sxs-lookup"><span data-stu-id="f6ced-134">`thisExpression` will receive the expression corresponding to the object before the dot.</span></span> <span data-ttu-id="f6ced-135">Jeśli jest wywoływana za pomocą składni metody statycznej, np. `Ext.ShouldBe(contestant.Points, 1337)`, będzie ona zachowywać się tak, jakby pierwszy parametr nie był oznaczony `this`.</span><span class="sxs-lookup"><span data-stu-id="f6ced-135">If it's called with static method syntax, e.g. `Ext.ShouldBe(contestant.Points, 1337)`, it will behave as if first parameter wasn't marked `this`.</span></span>

<span data-ttu-id="f6ced-136">Zawsze powinno być wyrażenie odpowiadające parametrowi `this`.</span><span class="sxs-lookup"><span data-stu-id="f6ced-136">There should always be an expression corresponding to the `this` parameter.</span></span> <span data-ttu-id="f6ced-137">Nawet jeśli wystąpienie klasy wywołuje metodę rozszerzającą dla samej siebie, np. `this.Single()` z wewnątrz typu kolekcji, `this` jest przypuszczany przez kompilator, więc `"this"` zostanie przeniesiona.</span><span class="sxs-lookup"><span data-stu-id="f6ced-137">Even if an instance of a class calls an extension method on itself, e.g. `this.Single()` from inside a collection type, the `this` is mandated by the compiler so `"this"` will get passed.</span></span> <span data-ttu-id="f6ced-138">Jeśli ta reguła zostanie zmieniona w przyszłości, możemy rozważyć przekazanie `null` lub pustego ciągu.</span><span class="sxs-lookup"><span data-stu-id="f6ced-138">If this rule is changed in the future, we can consider passing `null` or the empty string.</span></span>

### <a name="extra-details"></a><span data-ttu-id="f6ced-139">Dodatkowe szczegóły</span><span class="sxs-lookup"><span data-stu-id="f6ced-139">Extra details</span></span>

- <span data-ttu-id="f6ced-140">Podobnie jak w przypadku innych atrybutów `Caller*`, takich jak `CallerMemberName`, ten atrybut może być używany tylko w przypadku parametrów z wartościami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="f6ced-140">Like the other `Caller*` attributes, such as `CallerMemberName`, this attribute may only be used on parameters with default values.</span></span>
- <span data-ttu-id="f6ced-141">Dozwolone są liczne parametry oznaczone `CallerArgumentExpression`, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="f6ced-141">Multiple parameters marked with `CallerArgumentExpression` are permitted, as shown above.</span></span>
- <span data-ttu-id="f6ced-142">Przestrzeń nazw atrybutu zostanie `System.Runtime.CompilerServices`.</span><span class="sxs-lookup"><span data-stu-id="f6ced-142">The attribute's namespace will be `System.Runtime.CompilerServices`.</span></span>
- <span data-ttu-id="f6ced-143">Jeśli podano `null` lub ciąg, który nie jest nazwą parametru (np. `"notAParameterName"`), kompilator przekaże pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="f6ced-143">If `null` or a string that is not a parameter name (e.g. `"notAParameterName"`) is provided, the compiler will pass in an empty string.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="f6ced-144">Wady</span><span class="sxs-lookup"><span data-stu-id="f6ced-144">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="f6ced-145">Osoby, które wiedzą, jak używać dekompilatorów, będą mogli zobaczyć część kodu źródłowego w witrynach wywołań dla metod oznaczonych za pomocą tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f6ced-145">People who know how to use decompilers will be able to see some of the source code at call sites for methods marked with this attribute.</span></span> <span data-ttu-id="f6ced-146">Może to być niepożądane/nieoczekiwane w przypadku oprogramowania zamkniętego źródła.</span><span class="sxs-lookup"><span data-stu-id="f6ced-146">This may be undesirable/unexpected for closed-source software.</span></span>

- <span data-ttu-id="f6ced-147">Chociaż nie jest to luka w samej funkcji, źródłem problemu może być to, że obecnie istnieje `Debug.Assert` interfejs API, który ma tylko `bool`.</span><span class="sxs-lookup"><span data-stu-id="f6ced-147">Although this is not a flaw in the feature itself, a source of concern may be that there exists a `Debug.Assert` API today that only takes a `bool`.</span></span> <span data-ttu-id="f6ced-148">Nawet jeśli Przeciążenie pobierające komunikat miał drugi parametr oznaczony przy użyciu tego atrybutu i został wykonany jako opcjonalny, kompilator nadal będzie musiał wybrać komunikat No-Message w rozdzielczości przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="f6ced-148">Even if the overload taking a message had its second parameter marked with this attribute and made optional, the compiler would still pick the no-message one in overload resolution.</span></span> <span data-ttu-id="f6ced-149">W związku z tym, przeciążenia bez komunikatów należy usunąć, aby skorzystać z tej funkcji, która byłaby zmianą binarną (choć nie źródłową).</span><span class="sxs-lookup"><span data-stu-id="f6ced-149">Therefore, the no-message overload would have to be removed to take advantage of this feature, which would be a binary (although not source) breaking change.</span></span>

## <a name="alternatives"></a><span data-ttu-id="f6ced-150">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="f6ced-150">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="f6ced-151">Jeśli w celu wyświetlenia kodu źródłowego w witrynach wywołań dla metod, które używają tego atrybutu, okazuje się, że będzie to problem, możemy wyrazić zgodę na efekty atrybutów.</span><span class="sxs-lookup"><span data-stu-id="f6ced-151">If being able to see source code at call sites for methods that use this attribute proves to be a problem, we can make the attribute's effects opt-in.</span></span> <span data-ttu-id="f6ced-152">Deweloperzy umożliwią korzystanie z tego atrybutu `[assembly: EnableCallerArgumentExpression]` całego zestawu, które znajdują się w `AssemblyInfo.cs`.</span><span class="sxs-lookup"><span data-stu-id="f6ced-152">Developers will enable it through an assembly-wide `[assembly: EnableCallerArgumentExpression]` attribute they put in `AssemblyInfo.cs`.</span></span>
  - <span data-ttu-id="f6ced-153">W przypadku, gdy efekty atrybutów nie są włączone, metody wywołujące oznaczone atrybutem nie mogą być błędem, aby zezwolić istniejącym metodom na korzystanie z atrybutu i zachować zgodność ze źródłem.</span><span class="sxs-lookup"><span data-stu-id="f6ced-153">In the case the attribute's effects are not enabled, calling methods marked with the attribute would not be an error, to allow existing methods to use the attribute and maintain source compatibility.</span></span> <span data-ttu-id="f6ced-154">Jednak atrybut zostanie zignorowany, a metoda zostanie wywołana z dodaną wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="f6ced-154">However, the attribute would be ignored and the method would be called with whatever default value was provided.</span></span>

```csharp
// Assembly1

void Foo(string bar); // V1
void Foo(string bar, string barExpression = "not provided"); // V2
void Foo(string bar, [CallerArgumentExpression("bar")] string barExpression = "not provided"); // V3

// Assembly2

Foo(a); // V1: Compiles to Foo(a), V2, V3: Compiles to Foo(a, "not provided")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")

// Assembly3

[assembly: EnableCallerArgumentExpression]

Foo(a); // V1: Compiles to Foo(a), V2: Compiles to Foo(a, "not provided"), V3: Compiles to Foo(a, "a")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")
```

- <span data-ttu-id="f6ced-155">Aby zapobiec występowaniu[ drawbacks] [problemu ze zgodnością binarną] za każdym razem, gdy chcemy dodać nowe informacje o wywołującym do `Debug.Assert`, rozwiązaniem alternatywnym będzie dodanie struktury `CallerInfo` do struktury zawierającej wszystkie niezbędne informacje o wywołującym.</span><span class="sxs-lookup"><span data-stu-id="f6ced-155">To prevent the [binary compatibility problem][drawbacks] from occurring every time we want to add new caller info to `Debug.Assert`, an alternative solution would be to add a `CallerInfo` struct to the framework that contains all the necessary information about the caller.</span></span>

```csharp
struct CallerInfo
{
    public string MemberName { get; set; }
    public string TypeName { get; set; }
    public string Namespace { get; set; }
    public string FullTypeName { get; set; }
    public string FilePath { get; set; }
    public int LineNumber { get; set; }
    public int ColumnNumber { get; set; }
    public Type Type { get; set; }
    public MethodBase Method { get; set; }
    public string[] ArgumentExpressions { get; set; }
}

[Flags]
enum CallerInfoOptions
{
    MemberName = 1, TypeName = 2, ...
}

public static class Debug
{
    public static void Assert(bool condition,
        // If a flag is not set here, the corresponding CallerInfo member is not populated by the caller, so it's
        // pay-for-play friendly.
        [CallerInfo(CallerInfoOptions.FilePath | CallerInfoOptions.Method | CallerInfoOptions.ArgumentExpressions)] CallerInfo callerInfo = default(CallerInfo))
    {
        string filePath = callerInfo.FilePath;
        MethodBase method = callerInfo.Method;
        string conditionExpression = callerInfo.ArgumentExpressions[0];
        ...
    }
}

class Bar
{
    void Foo()
    {
        Debug.Assert(false);

        // Translates to:

        var callerInfo = new CallerInfo();
        callerInfo.FilePath = @"C:\Bar.cs";
        callerInfo.Method = MethodBase.GetCurrentMethod();
        callerInfo.ArgumentExpressions = new string[] { "false" };
        Debug.Assert(false, callerInfo);
    }
}
```

<span data-ttu-id="f6ced-156">Zostało to pierwotnie zaproponowane w https://github.com/dotnet/csharplang/issues/87.</span><span class="sxs-lookup"><span data-stu-id="f6ced-156">This was originally proposed at https://github.com/dotnet/csharplang/issues/87.</span></span>

<span data-ttu-id="f6ced-157">Istnieje kilka wad tego podejścia:</span><span class="sxs-lookup"><span data-stu-id="f6ced-157">There are a few disadvantages of this approach:</span></span>

- <span data-ttu-id="f6ced-158">Mimo że jest to zrozumiałe dla odtworzenia, dzięki czemu można określić, które właściwości są potrzebne, może to znacząco obniżyć wydajność przez przydzielenie tablicy dla wyrażeń/wywołań `MethodBase.GetCurrentMethod` nawet wtedy, gdy potwierdzenie zakończy się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="f6ced-158">Despite being pay-for-play friendly by allowing you to specify which properties you need, it could still hurt perf significantly by allocating an array for the expressions/calling `MethodBase.GetCurrentMethod` even when the assert passes.</span></span>

- <span data-ttu-id="f6ced-159">Ponadto podczas przekazywania nowej flagi do atrybutu `CallerInfo` nie będzie to zmiana istotna, `Debug.Assert` nie będzie zagwarantować, że w rzeczywistości otrzymasz nowy parametr z witryn wywołań, które zostały skompilowane względem starej wersji metody.</span><span class="sxs-lookup"><span data-stu-id="f6ced-159">Additionally, while passing a new flag to the `CallerInfo` attribute won't be a breaking change, `Debug.Assert` won't be guaranteed to actually receive that new parameter from call sites that compiled against an old version of the method.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="f6ced-160">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="f6ced-160">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="f6ced-161">TBD</span><span class="sxs-lookup"><span data-stu-id="f6ced-161">TBD</span></span>

## <a name="design-meetings"></a><span data-ttu-id="f6ced-162">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="f6ced-162">Design meetings</span></span>

<span data-ttu-id="f6ced-163">Nie dotyczy</span><span class="sxs-lookup"><span data-stu-id="f6ced-163">N/A</span></span>
