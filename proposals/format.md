---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "79485015"
---
# <a name="efficient-params-and-string-formatting"></a><span data-ttu-id="370a4-101">Wydajne parametry i formatowanie ciągów</span><span class="sxs-lookup"><span data-stu-id="370a4-101">Efficient Params and String Formatting</span></span>

## <a name="summary"></a><span data-ttu-id="370a4-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="370a4-102">Summary</span></span>
<span data-ttu-id="370a4-103">Ta kombinacja funkcji spowoduje zwiększenie wydajności formatowania `string` wartości i przekazanie argumentów stylu `params`.</span><span class="sxs-lookup"><span data-stu-id="370a4-103">This combination of features will increase the efficiency of formatting `string` values and passing of `params` style arguments.</span></span>

## <a name="motivation"></a><span data-ttu-id="370a4-104">Motywacją</span><span class="sxs-lookup"><span data-stu-id="370a4-104">Motivation</span></span>
<span data-ttu-id="370a4-105">Narzuty przydziału dla formatowania `string` wartości mogą przeważać nad wydajnością wielu aplikacji tekstowych opartych na tekście: od nałożenia oddziału według typów `struct`, alokacji `object[]` dla `params` i pośrednich alokacji `string` podczas wywołań `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="370a4-105">The allocation overhead of formatting `string` values can dominate the performance of many text based applications: from the boxing penalty of `struct` types, the `object[]` allocation for `params` and the intermediate `string` allocations during `string.Format` calls.</span></span> <span data-ttu-id="370a4-106">W celu utrzymania wydajności takie aplikacje często wymagają porzucenia funkcji produktywności, takich jak `params` i `string` Interpolacja i przechodzenie do niestandardowych, ręcznie zakodowanych rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="370a4-106">In order to maintain efficiency such applications often need to abandon productivity features such as `params` and `string` interpolation and move to non-standard, hand coded solutions.</span></span> 

<span data-ttu-id="370a4-107">Jako przykładu należy wziąć pod uwagę program MSBuild.</span><span class="sxs-lookup"><span data-stu-id="370a4-107">Consider MSBuild as an example.</span></span> <span data-ttu-id="370a4-108">Jest to zapisanie przy użyciu wielu nowoczesnych C# funkcji przez deweloperów, którzy mają świadomość wydajności.</span><span class="sxs-lookup"><span data-stu-id="370a4-108">This is written using a lot of modern C# features by developers who are conscious of performance.</span></span> <span data-ttu-id="370a4-109">Jeszcze w jednym reprezentatywnym przykładzie kompilacja MSBuild wygeneruje 262MB alokacji `string` przy użyciu minimalnej szczegółowości.</span><span class="sxs-lookup"><span data-stu-id="370a4-109">Yet in one representative build sample MSBuild will generate 262MB of `string` allocation using minimal verbosity.</span></span> <span data-ttu-id="370a4-110">Z tego 1/2 alokacji są alokacje krótkoterminowe w ramach `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="370a4-110">Of that 1/2 of the allocations are short lived allocations inside `string.Format`.</span></span> <span data-ttu-id="370a4-111">Te funkcje mogą usunąć większość z programu na pulpicie .NET i uzyskać niemal zero na platformie .NET Core ze względu na dostępność `Span<T>`</span><span class="sxs-lookup"><span data-stu-id="370a4-111">These features would remove much of that on .NET Desktop and get it down to nearly zero on .NET Core due to the availability of `Span<T>`</span></span>

<span data-ttu-id="370a4-112">Zestaw funkcji języka opisanych w tym miejscu umożliwia aplikacjom kontynuowanie korzystania z tych funkcji, z niewielką ilością lub bez zmian w bazie kodu aplikacji, przy jednoczesnym usunięciu niezamierzonych nakładów związanych z alokacją w większości przypadków.</span><span class="sxs-lookup"><span data-stu-id="370a4-112">The set of language features described here will enable applications to continue using these features, with very little or no churn to their application code base, while removing the unintended allocation overhead in the majority of cases.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="370a4-113">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="370a4-113">Detailed Design</span></span> 
<span data-ttu-id="370a4-114">Istnieje zestaw funkcji, które zostaną użyte w tym miejscu do osiągnięcia następujących wyników:</span><span class="sxs-lookup"><span data-stu-id="370a4-114">There are a set of features that will be used here to achieve these results:</span></span>

- <span data-ttu-id="370a4-115">Rozwijanie `params` w celu obsługi szerszego zestawu typów kolekcji.</span><span class="sxs-lookup"><span data-stu-id="370a4-115">Expanding `params` to support a broader set of collection types.</span></span>
- <span data-ttu-id="370a4-116">Umożliwienie deweloperom dostosowywania sposobu wykonywania interpolacji `string`.</span><span class="sxs-lookup"><span data-stu-id="370a4-116">Allowing for developers to customize how `string` interpolation is achieved.</span></span> 
- <span data-ttu-id="370a4-117">Zezwalanie na interpolowane `string`, aby powiązać je z bardziej wydajnymi przeciążeniami `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="370a4-117">Allowing for interpolated `string` to bind to more efficient `string.Format` overloads.</span></span>

### <a name="extending-params"></a><span data-ttu-id="370a4-118">Rozszerzanie parametrów</span><span class="sxs-lookup"><span data-stu-id="370a4-118">Extending params</span></span>
<span data-ttu-id="370a4-119">Język będzie zezwalać na `params` w sygnaturze metody w celu posiadania typów `Span<T>`, `ReadOnlySpan<T>` i `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="370a4-119">The language will allow for `params` in a method signature to have the types `Span<T>`, `ReadOnlySpan<T>` and `IEnumerable<T>`.</span></span> <span data-ttu-id="370a4-120">Te same reguły wywołania będą stosowane do tych nowych typów, które mają zastosowanie do `params T[]`:</span><span class="sxs-lookup"><span data-stu-id="370a4-120">The same rules for invocation will apply to these new types that apply to `params T[]`:</span></span>

- <span data-ttu-id="370a4-121">Nie można przeciążyć, gdy jedyną różnicą jest słowo kluczowe `params`.</span><span class="sxs-lookup"><span data-stu-id="370a4-121">Can't overload where the only difference is a `params` keyword.</span></span>
- <span data-ttu-id="370a4-122">Można wywołać, przekazując serię argumentów, które są niejawnie konwertowane na `T` lub jeden `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="370a4-122">Can invoke by passing a series of arguments that are implicitly convertible to `T` or a single `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` argument.</span></span>
- <span data-ttu-id="370a4-123">Musi być ostatnim parametrem w podpisie metody.</span><span class="sxs-lookup"><span data-stu-id="370a4-123">Must be the last parameter in a method signature.</span></span>
- <span data-ttu-id="370a4-124">Itp...</span><span class="sxs-lookup"><span data-stu-id="370a4-124">Etc ...</span></span> 

<span data-ttu-id="370a4-125">`Span<T>` i `ReadOnlySpan<T>` wariantów są określane jako `Span<T>` poniżej dla uproszczenia.</span><span class="sxs-lookup"><span data-stu-id="370a4-125">The `Span<T>` and `ReadOnlySpan<T>` variants will be referred to as `Span<T>` below for simplicity.</span></span> <span data-ttu-id="370a4-126">W przypadkach, w których zachowanie `ReadOnlySpan<T>` jest różne, zostanie jawnie wywołana.</span><span class="sxs-lookup"><span data-stu-id="370a4-126">In cases where the behavior of `ReadOnlySpan<T>` differs it will be explicitly called out.</span></span> 

<span data-ttu-id="370a4-127">Zaletą `Span<T>` warianty `params` zapewniają, że kompilator zapewnia doskonałą elastyczność w sposobie przydzielania magazynu zapasowego dla wartości `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="370a4-127">The advantage the `Span<T>` variants of `params` provides is it gives the compiler great flexibility in how it allocates the backing storage for the `Span<T>` value.</span></span> <span data-ttu-id="370a4-128">Przy `params T[]` kompilator musi przydzielić nowy `T[]` dla każdego wywołania metody `params`.</span><span class="sxs-lookup"><span data-stu-id="370a4-128">With a `params T[]` the compiler must allocate a new `T[]` for every invocation of a `params` method.</span></span> <span data-ttu-id="370a4-129">Ponowne użycie nie jest możliwe, ponieważ musi zajmować wywoływany, przechowywany i ponownie wykorzystany parametr.</span><span class="sxs-lookup"><span data-stu-id="370a4-129">Re-use is not possible because it must assume the callee stored and reused the parameter.</span></span> <span data-ttu-id="370a4-130">Może to prowadzić do dużego nieefektywności metod z dużą ilością `params` wywołań.</span><span class="sxs-lookup"><span data-stu-id="370a4-130">This can lead to a large inefficiency in methods with lots of `params` invocations.</span></span>

<span data-ttu-id="370a4-131">Dana `Span<T>` warianty są `ref struct` element wywoływany nie może przechowywać argumentu.</span><span class="sxs-lookup"><span data-stu-id="370a4-131">Given `Span<T>` variants are `ref struct` the callee cannot store the argument.</span></span> <span data-ttu-id="370a4-132">W związku z tym kompilator może zoptymalizować Lokacje wywołań, podejmując akcje, takie jak ponowne użycie argumentu.</span><span class="sxs-lookup"><span data-stu-id="370a4-132">Hence the compiler can optimize the call sites by taking actions like re-using the argument.</span></span> <span data-ttu-id="370a4-133">Może to spowodować, że powtarzające się wywołania są bardzo wydajne w porównaniu do `T[]`.</span><span class="sxs-lookup"><span data-stu-id="370a4-133">This can make repeated invocations very efficient as compared to `T[]`.</span></span> <span data-ttu-id="370a4-134">Język, w którym nie ma żadnych szczególnych gwarancji dotyczących sposobu, w jaki takie callsites są zoptymalizowane.</span><span class="sxs-lookup"><span data-stu-id="370a4-134">The language though will make no specific guarantees about how such callsites are optimized.</span></span> <span data-ttu-id="370a4-135">Należy zauważyć, że kompilator jest bezpłatny do używania wartości innych niż `T[]` podczas wywoływania metody `params Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="370a4-135">Only note that the compiler is free to use values other than `T[]` when invoking a `params Span<T>` method.</span></span> 

<span data-ttu-id="370a4-136">Jedna taka potencjalna implementacja jest następująca.</span><span class="sxs-lookup"><span data-stu-id="370a4-136">One such potential implementation is the following.</span></span> <span data-ttu-id="370a4-137">Rozważ wszystkie wywołania `params` w treści metody.</span><span class="sxs-lookup"><span data-stu-id="370a4-137">Consider all `params` invocation in a method body.</span></span> <span data-ttu-id="370a4-138">Kompilator może przydzielić tablicę, która ma rozmiar równy największemu wy`params`emu wywołania i użyć go dla wszystkich wywołań, tworząc odpowiednio rozmiar `Span<T>` wystąpień na tablicy.</span><span class="sxs-lookup"><span data-stu-id="370a4-138">The compiler could allocate an array which has a size equal to the largest `params` invocation and use that for all of the invocations by creating appropriately sized `Span<T>` instances over the array.</span></span> <span data-ttu-id="370a4-139">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="370a4-139">For example:</span></span>

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

<span data-ttu-id="370a4-140">Kompilator może wybrać emisję treści `Go` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="370a4-140">The compiler could choose to emit the body of `Go` as follows:</span></span>

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

<span data-ttu-id="370a4-141">Może to znacznie zmniejszyć liczbę tablic przydzieloną w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="370a4-141">This can significantly reduce the number of arrays allocated in an application.</span></span> <span data-ttu-id="370a4-142">Alokacje mogą być jeszcze bardziej zmniejszane, jeśli środowisko uruchomieniowe zapewnia narzędzia do przydzielenia przez inteligentniejszy stos tablic.</span><span class="sxs-lookup"><span data-stu-id="370a4-142">Allocations can be even further reduced if the runtime provides utilities for smarter stack allocation of arrays.</span></span>

<span data-ttu-id="370a4-143">Ta optymalizacja nie może być zawsze stosowana, chociaż.</span><span class="sxs-lookup"><span data-stu-id="370a4-143">This optimization cannot always be applied though.</span></span> <span data-ttu-id="370a4-144">Mimo że obiekt wywoływany nie może przechwycić argumentu `params`, nadal można go przechwycić w obiekcie wywołującym, gdy istnieje `ref` lub `out / ref` parametr, który jest własnym typem `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="370a4-144">Even though the callee cannot capture the `params` argument it can still be captured in the caller when there is a `ref` or a `out / ref` parameter that is itself a `ref struct` type.</span></span> 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

<span data-ttu-id="370a4-145">Te przypadki są statycznie wykrywalne.</span><span class="sxs-lookup"><span data-stu-id="370a4-145">These cases are statically detectable though.</span></span> <span data-ttu-id="370a4-146">Może się to zdarzyć, gdy istnieje `ref` Return lub `ref struct` parametr przesłany przez `out` lub `ref`.</span><span class="sxs-lookup"><span data-stu-id="370a4-146">It potentially occurs whenever there is a `ref` return or a `ref struct` parameter passed by `out` or `ref`.</span></span> <span data-ttu-id="370a4-147">W takim przypadku kompilator musi przydzielić świeżą `T[]` dla każdego wywołania.</span><span class="sxs-lookup"><span data-stu-id="370a4-147">In such a case the compiler must allocate a fresh `T[]` for every invocation.</span></span> 

<span data-ttu-id="370a4-148">Niektóre inne potencjalne strategie optymalizacji zostały omówione na końcu tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="370a4-148">Several other potential optimization strategies are discussed at the end of this document.</span></span>

<span data-ttu-id="370a4-149">`IEnumerable<T>` wariant jest tylko wygodnym przeciążeniem.</span><span class="sxs-lookup"><span data-stu-id="370a4-149">The `IEnumerable<T>` variant is a merely a convenience overload.</span></span> <span data-ttu-id="370a4-150">Jest to przydatne w scenariuszach, które często wykorzystują `IEnumerable<T>`, ale również mają wiele `params` użycia.</span><span class="sxs-lookup"><span data-stu-id="370a4-150">It's useful in scenarios which have frequent uses of `IEnumerable<T>` but also have lots of `params` usage.</span></span> <span data-ttu-id="370a4-151">Gdy jest wywoływana w `T` argumentem, magazyn zapasowy zostanie przydzielony jako `T[]`, tak jak `params T[]` jest gotowe.</span><span class="sxs-lookup"><span data-stu-id="370a4-151">When invoked in `T` argument form the backing storage will be allocated as a `T[]` just as `params T[]` is done today.</span></span>

### <a name="params-overload-resolution-changes"></a><span data-ttu-id="370a4-152">zmiany rozdzielczości przeciążania parametrów</span><span class="sxs-lookup"><span data-stu-id="370a4-152">params overload resolution changes</span></span>
<span data-ttu-id="370a4-153">Ta propozycja oznacza, że język ma cztery warianty `params`, gdzie wcześniej.</span><span class="sxs-lookup"><span data-stu-id="370a4-153">This proposal means the language now has four variants of `params` where before it had one.</span></span> <span data-ttu-id="370a4-154">Jest to rozsądne dla metod definiowania przeciążeń metod, które różnią się tylko typem deklaracji `params`.</span><span class="sxs-lookup"><span data-stu-id="370a4-154">It is sensible for methods to define overloads of methods that differ only on the type of a `params` declarations.</span></span> 

<span data-ttu-id="370a4-155">Należy wziąć pod uwagę, że `StringBuilder.AppendFormat` oprócz `params object[]`należy dodać Przeciążenie `params ReadOnlySpan<object>`.</span><span class="sxs-lookup"><span data-stu-id="370a4-155">Consider that `StringBuilder.AppendFormat` would certainly add a `params ReadOnlySpan<object>` overload in addition to the `params object[]`.</span></span> <span data-ttu-id="370a4-156">Pozwoli to na znaczne zwiększenie wydajności poprzez zmniejszenie alokacji kolekcji bez konieczności wprowadzania jakichkolwiek zmian w kodzie wywołującym.</span><span class="sxs-lookup"><span data-stu-id="370a4-156">This would allow it to substantially improve performance by reducing collection allocations without requiring any changes to the calling code.</span></span> 

<span data-ttu-id="370a4-157">Aby ułatwić tym językowi, należy wprowadzić następującą regułę zerwania powiązania z rozwiązaniem przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="370a4-157">To facilitate this the language will introduce the following overload resolution tie breaking rule.</span></span> <span data-ttu-id="370a4-158">Gdy metody kandydujące różnią się tylko parametrem `params`, kandydatów będą preferowane w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="370a4-158">When the candidate methods differ only by the `params` parameter then the candidates will be preferred in the following order:</span></span>

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

<span data-ttu-id="370a4-159">Ta kolejność jest najbardziej wydajna dla ogólnego przypadku.</span><span class="sxs-lookup"><span data-stu-id="370a4-159">This order is the most to the least efficient for the general case.</span></span>

### <a name="variant"></a><span data-ttu-id="370a4-160">Typu</span><span class="sxs-lookup"><span data-stu-id="370a4-160">Variant</span></span>
<span data-ttu-id="370a4-161">CoreFX jest prototypem nowego typu zarządzanego o nazwie [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span><span class="sxs-lookup"><span data-stu-id="370a4-161">CoreFX is prototyping a new managed type named [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span></span> <span data-ttu-id="370a4-162">Ten typ jest przeznaczony do użycia w interfejsach API, które oczekują wartości heterogenicznych, ale nie chcesz, aby obciążenie było włączone przy użyciu `object` jako parametru.</span><span class="sxs-lookup"><span data-stu-id="370a4-162">This type is meant to be used in APIs which expect heterogeneous values but don't want the overhead brought on by using `object` as the parameter.</span></span> <span data-ttu-id="370a4-163">Typ `Variant` zapewnia Uniwersalny magazyn, ale unika alokacji opakowania dla najczęściej używanych typów.</span><span class="sxs-lookup"><span data-stu-id="370a4-163">The `Variant` type provides universal storage but avoids the boxing allocation for the most commonly used types.</span></span> <span data-ttu-id="370a4-164">Użycie tego typu w interfejsach API, takich jak `string.Format`, może wyeliminować obciążenie opakowania w większości przypadków.</span><span class="sxs-lookup"><span data-stu-id="370a4-164">Using this type in APIs like `string.Format` can eliminate the boxing overhead in the majority of cases.</span></span>

<span data-ttu-id="370a4-165">Ten typ nie musi być specjalny dla języka.</span><span class="sxs-lookup"><span data-stu-id="370a4-165">This type itself is not necessarily special to the language.</span></span> <span data-ttu-id="370a4-166">Jest ona wprowadzana w tym dokumencie osobno, gdy staną się szczegółami implementacji innych części wniosku.</span><span class="sxs-lookup"><span data-stu-id="370a4-166">It is being introduced in this document separately though as it becomes an implementation detail of other parts of the proposal.</span></span> 

### <a name="efficient-interpolated-strings"></a><span data-ttu-id="370a4-167">Wydajne parametry interpolowane</span><span class="sxs-lookup"><span data-stu-id="370a4-167">Efficient interpolated strings</span></span>
<span data-ttu-id="370a4-168">Ciągi interpolowane to popularne, jeszcze niewydajne funkcje w C#programie.</span><span class="sxs-lookup"><span data-stu-id="370a4-168">Interpolated strings are a popular yet inefficient feature in C#.</span></span> <span data-ttu-id="370a4-169">Najbardziej typowa składnia, używając interpolowanej `string` jako `string`, tłumaczy na wywołanie `string.Format(string, params object[])`.</span><span class="sxs-lookup"><span data-stu-id="370a4-169">The most common syntax, using an interpolated `string` as a `string`, translates into a `string.Format(string, params object[])` call.</span></span> <span data-ttu-id="370a4-170">Spowoduje to naliczenie przydziałów dla wszystkich typów wartości, pośrednich alokacji `string`, ponieważ implementacja w dużym stopniu używa `object.ToString` do formatowania, a także alokacji tablic, gdy liczba argumentów przekroczy wartość parametrów w przypadku przeciążenia "szybkie" `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="370a4-170">That will incur boxing allocations for all value types, intermediate `string` allocations as the implementation largely uses `object.ToString` for formatting as well as array allocations once the number of arguments exceeds the amount of parameters on the "fast" overloads of `string.Format`.</span></span> 

<span data-ttu-id="370a4-171">Język zmieni swoją interpolację, aby wziąć pod uwagę alternatywne przeciążenia `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="370a4-171">The language will change its interpolation lowering to consider alternate overloads of `string.Format`.</span></span> <span data-ttu-id="370a4-172">Rozważ wszystkie formy `string.Format(string, params)` i wybierz "najlepsze" Przeciążenie, które spełnia typy argumentów.</span><span class="sxs-lookup"><span data-stu-id="370a4-172">It will consider all forms of `string.Format(string, params)` and pick the "best" overload which satisfies the argument types.</span></span>
<span data-ttu-id="370a4-173">"Najlepsze" Przeciążenie `params` zostanie określone przez zasady omówione powyżej.</span><span class="sxs-lookup"><span data-stu-id="370a4-173">The "best" `params` overload will be determined by the rules discussed above.</span></span> <span data-ttu-id="370a4-174">Oznacza to, że interpolowane `string` mogą teraz wiązać się z bardzo wydajnymi przeciążeniami, takimi jak `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span><span class="sxs-lookup"><span data-stu-id="370a4-174">This means interpolated `string` can now bind to very efficient overloads like `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span></span> <span data-ttu-id="370a4-175">W wielu przypadkach spowoduje to usunięcie wszystkich przydziałów pośrednich.</span><span class="sxs-lookup"><span data-stu-id="370a4-175">In many cases this will remove all intermediate allocations.</span></span>

### <a name="customizable-interpolated-strings"></a><span data-ttu-id="370a4-176">Dostosowywalne ciągi interpolowane</span><span class="sxs-lookup"><span data-stu-id="370a4-176">Customizable interpolated strings</span></span>
<span data-ttu-id="370a4-177">Deweloperzy mogą dostosować zachowanie interpolowanych ciągów z `FormattableString`.</span><span class="sxs-lookup"><span data-stu-id="370a4-177">Developers are able to customize the behavior of interpolated strings with `FormattableString`.</span></span> <span data-ttu-id="370a4-178">Zawiera dane, które przechodzą do ciągu interpolowanego: format `string` i argumentów jako tablicę.</span><span class="sxs-lookup"><span data-stu-id="370a4-178">This contains the data which goes into an interpolated string: the format `string` and the arguments as an array.</span></span> <span data-ttu-id="370a4-179">Mimo to nadal ma alokację tablicy z opakowaniem i argumentem, a także alokację dla `FormattableString` (jest to `abstract class`).</span><span class="sxs-lookup"><span data-stu-id="370a4-179">This though still has the boxing and argument array allocation as well as the allocation for `FormattableString` (it's an `abstract class`).</span></span> <span data-ttu-id="370a4-180">W związku z tym nie jest to małe użycie w przypadku aplikacji, które są przydzielane w `string` formatowania.</span><span class="sxs-lookup"><span data-stu-id="370a4-180">Hence it's of little use to applications which are allocation heavy in `string` formatting.</span></span>

<span data-ttu-id="370a4-181">Aby format ciągu interpolowanego był wydajny, język rozpoznaje nowy typ: `System.ValueFormattableString`.</span><span class="sxs-lookup"><span data-stu-id="370a4-181">To make interpolated string formatting efficient the language will recognize a new type: `System.ValueFormattableString`.</span></span> <span data-ttu-id="370a4-182">Wszystkie interpolowane ciągi będą miały konwersję typu docelowego do tego typu.</span><span class="sxs-lookup"><span data-stu-id="370a4-182">All interpolated strings will have a target type conversion to this type.</span></span> <span data-ttu-id="370a4-183">Zostanie to zaimplementowane przez przetłumaczenie ciągu interpolowanego na wywołanie `ValueFormattableString.Create` dokładnie tak, jak zostało wykonane dla `FormattableString.Create` dzisiaj.</span><span class="sxs-lookup"><span data-stu-id="370a4-183">This will be implemented by translating the interpolated string into the call `ValueFormattableString.Create` exactly as is done for `FormattableString.Create` today.</span></span> <span data-ttu-id="370a4-184">Język będzie obsługiwał wszystkie opcje `params` opisane w tym dokumencie podczas wyszukiwania najbardziej odpowiedniej metody `ValueFormattableString.Create`.</span><span class="sxs-lookup"><span data-stu-id="370a4-184">The language will support all `params` options described in this document when looking for the most suitable `ValueFormattableString.Create` method.</span></span> 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

<span data-ttu-id="370a4-185">Reguły rozpoznawania przeciążenia zostaną zmienione na preferowane `ValueFormattableString` przez `string`, gdy argument jest ciągiem interpolowanym.</span><span class="sxs-lookup"><span data-stu-id="370a4-185">Overload resolution rules will be changed to prefer `ValueFormattableString` over `string` when the argument is an interpolated string.</span></span> <span data-ttu-id="370a4-186">Oznacza to, że będzie to przydatne w przypadku przeciążeń, które różnią się tylko `string` i `ValueFormattableString`.</span><span class="sxs-lookup"><span data-stu-id="370a4-186">This means it will be valuable to have overloads which differ only on `string` and `ValueFormattableString`.</span></span> <span data-ttu-id="370a4-187">Takie Przeciążenie już dziś z `FormattableString` nie jest cenne, ponieważ kompilator zawsze preferuje `string` wersji (chyba że deweloper użyje jawnego rzutowania).</span><span class="sxs-lookup"><span data-stu-id="370a4-187">Such an overload today with `FormattableString` is not valuable as the compiler will always prefer the `string` version (unless the developer uses an explicit cast).</span></span> 

## <a name="open-issues"></a><span data-ttu-id="370a4-188">Otwarte problemy</span><span class="sxs-lookup"><span data-stu-id="370a4-188">Open Issues</span></span>

### <a name="valueformattablestring-breaking-change"></a><span data-ttu-id="370a4-189">ValueFormattableStringa zmiana</span><span class="sxs-lookup"><span data-stu-id="370a4-189">ValueFormattableString breaking change</span></span>
<span data-ttu-id="370a4-190">Zmiana, która ma być preferowana `ValueFormattableString` podczas rozpoznawania przeciążenia przez `string` jest istotną zmianą.</span><span class="sxs-lookup"><span data-stu-id="370a4-190">The change to prefer `ValueFormattableString` during overload resolution over `string` is a breaking change.</span></span> <span data-ttu-id="370a4-191">Deweloper może zdefiniować typ o nazwie `ValueFormattableString` dzisiaj i używać go w przeciążeniach metody z `string`.</span><span class="sxs-lookup"><span data-stu-id="370a4-191">It is possible for a developer to have defined a type called `ValueFormattableString` today and use it in method overloads with `string`.</span></span> <span data-ttu-id="370a4-192">Ta proponowana zmiana spowodowałaby, że kompilator wybierze inne Przeciążenie po zaimplementowaniu tego zestawu funkcji.</span><span class="sxs-lookup"><span data-stu-id="370a4-192">This proposed change would cause the compiler to pick a different overload once this set of features was implemented.</span></span> 

<span data-ttu-id="370a4-193">Jest to prawdopodobnie niska.</span><span class="sxs-lookup"><span data-stu-id="370a4-193">The possibility of this seems reasonably low.</span></span> <span data-ttu-id="370a4-194">Typ musi mieć pełną nazwę `System.ValueFormattableString` i musi mieć `static` metody o nazwie `Create`.</span><span class="sxs-lookup"><span data-stu-id="370a4-194">The type would need the full name `System.ValueFormattableString` and it would need to have `static` methods named `Create`.</span></span> <span data-ttu-id="370a4-195">Biorąc pod względu, że deweloperzy są zdecydowanie odradzani od definiowania dowolnego typu w przestrzeni nazw `System` ten podział wydaje się taki sam, jak w przypadku rozsądnego naruszenia.</span><span class="sxs-lookup"><span data-stu-id="370a4-195">Given that developers are strongly discouraged from defining any type in the `System` namespace this break seems like a reasonable compromise.</span></span>

### <a name="expanding-to-more-types"></a><span data-ttu-id="370a4-196">Rozszerzanie do większej liczby typów</span><span class="sxs-lookup"><span data-stu-id="370a4-196">Expanding to more types</span></span>
<span data-ttu-id="370a4-197">Z tego względu będziemy mogli rozważyć dodanie `IList<T>`, `ICollection<T>` i `IReadOnlyList<T>` do zestawu kolekcji, dla których `params` jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="370a4-197">Given we're in the area we should consider adding `IList<T>`, `ICollection<T>` and `IReadOnlyList<T>` to the set of collections for which `params` is supported.</span></span> <span data-ttu-id="370a4-198">W ramach wdrożenia będzie kosztować niewielką ilość w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="370a4-198">In terms of implementation it will cost a small amount over the other work here.</span></span>

<span data-ttu-id="370a4-199">Program LDM musi zdecydować, czy pozostała do tego język.</span><span class="sxs-lookup"><span data-stu-id="370a4-199">LDM needs to decide if the complication to the language is worth it though.</span></span> <span data-ttu-id="370a4-200">Dodanie `IEnumerable<T>` usuwa bardzo konkretny punkt tarcia.</span><span class="sxs-lookup"><span data-stu-id="370a4-200">The addition of `IEnumerable<T>` removes a very specific friction point.</span></span> <span data-ttu-id="370a4-201">Brak tego rozwiązania `params` wielu klientów zmuszony do przydzielenia `T[]` z `IEnumerable<T>` podczas wywoływania metody `params`.</span><span class="sxs-lookup"><span data-stu-id="370a4-201">Lacking this `params` solution many customers were forced to allocate `T[]` from an `IEnumerable<T>` when calling a `params` method.</span></span> <span data-ttu-id="370a4-202">Dodanie `IEnumerable<T>` rozwiązuje ten problem.</span><span class="sxs-lookup"><span data-stu-id="370a4-202">The addition of `IEnumerable<T>` fixes this though.</span></span> <span data-ttu-id="370a4-203">Nie ma konkretnego punktu tarcia, który w tym miejscu rozwiązuje inne interfejsy.</span><span class="sxs-lookup"><span data-stu-id="370a4-203">There is no specific friction point that the other interfaces fix here.</span></span> 

## <a name="considerations"></a><span data-ttu-id="370a4-204">Zagadnienia do rozważenia</span><span class="sxs-lookup"><span data-stu-id="370a4-204">Considerations</span></span>

### <a name="variant2-and-variant3"></a><span data-ttu-id="370a4-205">Variant2 i Variant3</span><span class="sxs-lookup"><span data-stu-id="370a4-205">Variant2 and Variant3</span></span>
<span data-ttu-id="370a4-206">Zespół CoreFX ma także nieprzydzielany zestaw typów magazynu dla maksymalnie trzech argumentów `Variant`.</span><span class="sxs-lookup"><span data-stu-id="370a4-206">The CoreFX team also has a non-allocating set of storage types for up to three `Variant` arguments.</span></span> <span data-ttu-id="370a4-207">Są to pojedyncze `Variant`, `Variant2` i `Variant3`.</span><span class="sxs-lookup"><span data-stu-id="370a4-207">These are a single `Variant`, `Variant2` and `Variant3`.</span></span> <span data-ttu-id="370a4-208">Wszystko, co ma parę metod do uzyskania bezpłatnej alokacji `Span<Variant>` z nich: `CreateSpan` i `KeepAlive`.</span><span class="sxs-lookup"><span data-stu-id="370a4-208">All have a pair of methods for getting an allocation free `Span<Variant>` off of them: `CreateSpan` and `KeepAlive`.</span></span> <span data-ttu-id="370a4-209">Oznacza to, że dla `params Span<Variant>` do trzech argumentów można całkowicie bezpłatnie przydzielić witrynę wywołania.</span><span class="sxs-lookup"><span data-stu-id="370a4-209">This means for a `params Span<Variant>` of up to three arguments the call site can be entirely allocation free.</span></span>

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

<span data-ttu-id="370a4-210">Metodę `Go` można obniżyć do następujących:</span><span class="sxs-lookup"><span data-stu-id="370a4-210">The `Go` method can be lowered to the following:</span></span>

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

<span data-ttu-id="370a4-211">Wymaga to bardzo małego nakładu pracy nad propozycją w celu ponownego użycia `T[]` między wywołaniami `params Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="370a4-211">This requires very little work on top of the proposal to re-use `T[]` between `params Span<T>` calls.</span></span> <span data-ttu-id="370a4-212">Kompilator musi już zarządzać tymczasowym dla każdego wywołania i przeprowadzić oczyszczanie pracy po (nawet jeśli w jednym przypadku oznacza to, że jest to tylko oznaczenie wewnętrzne temp as Free).</span><span class="sxs-lookup"><span data-stu-id="370a4-212">The compiler already needs to manage a temporary per call and do clean up work after (even if in one case it's just marking an internal temp as free).</span></span> 

<span data-ttu-id="370a4-213">Uwaga: funkcja `KeepAlive` jest wymagana tylko na pulpicie.</span><span class="sxs-lookup"><span data-stu-id="370a4-213">Note: the `KeepAlive` function is only necessary on desktop.</span></span> <span data-ttu-id="370a4-214">W przypadku platformy .NET Core Metoda nie będzie dostępna i w związku z tym kompilator nie emituje wywołania do niego.</span><span class="sxs-lookup"><span data-stu-id="370a4-214">On .NET Core the method will not be available and hence the compiler won't emit a call to it.</span></span>

### <a name="clr-stack-allocation-helpers"></a><span data-ttu-id="370a4-215">Pomocnicy alokacji stosu CLR</span><span class="sxs-lookup"><span data-stu-id="370a4-215">CLR stack allocation helpers</span></span>
<span data-ttu-id="370a4-216">Środowisko CLR zapewnia tylko [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) na potrzeby alokacji stosu ciągłej pamięci.</span><span class="sxs-lookup"><span data-stu-id="370a4-216">The CLR only provides only [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) for stack allocation of contiguous memory.</span></span> <span data-ttu-id="370a4-217">Ta instrukcja jest ograniczona, ponieważ działa tylko dla typów `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="370a4-217">This instruction is limited in that it only works for `unmanaged` types.</span></span> <span data-ttu-id="370a4-218">Oznacza to, że nie można go użyć jako uniwersalnego rozwiązania do wydajnego alokowania magazynu zapasowego `params 
 Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="370a4-218">This means it can't be used as a universal solution for efficiently allocating the backing storage for `params 
 Span<T>`.</span></span> 

<span data-ttu-id="370a4-219">To ograniczenie nie dotyczy jednak niektórych zasadniczych ograniczeń, ale zamiast tego bardziej artefaktu historii.</span><span class="sxs-lookup"><span data-stu-id="370a4-219">This limitation is not some fundamental restriction though but instead more an artifact of history.</span></span> <span data-ttu-id="370a4-220">Środowisko CLR może dodać nowe kody operacji/wewnętrzne, które zapewniają uniwersalną alokację stosu.</span><span class="sxs-lookup"><span data-stu-id="370a4-220">The CLR could choose to add new op codes / intrinsics which provide universal stack allocation.</span></span> <span data-ttu-id="370a4-221">Można je następnie użyć do przydzielenia magazynu zapasowego dla większości wywołań `params Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="370a4-221">These could then be used to allocate the backing storage for most `params Span<T>` calls.</span></span>

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

<span data-ttu-id="370a4-222">Metodę `Go` można obniżyć do następujących:</span><span class="sxs-lookup"><span data-stu-id="370a4-222">The `Go` method can be lowered to the following:</span></span>

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

<span data-ttu-id="370a4-223">To podejście jest bardzo wydajne, ponieważ powoduje to dodatkowe użycie stosu.</span><span class="sxs-lookup"><span data-stu-id="370a4-223">While this approach is very heap efficient it does cause extra stack usage.</span></span> <span data-ttu-id="370a4-224">W algorytmie, który ma głębokiego stosu i wiele `params` użycia, może to spowodować, że `StackOverflowException` zostanie wygenerowany, gdy prosta alokacja `T[]` się powiedzie.</span><span class="sxs-lookup"><span data-stu-id="370a4-224">In an algorithm which has a deep stack and lots of `params` usage it's possible this could cause a `StackOverflowException` to be generated where a simple `T[]` allocation would succeed.</span></span> 

<span data-ttu-id="370a4-225">Niestety C# nie jest skonfigurowana dla typu analizy międzymetodowej, w której może zostać wyznaczony, czy wywołanie powinno korzystać z przydziału stosu lub sterty `params`.</span><span class="sxs-lookup"><span data-stu-id="370a4-225">Unfortunately C# is not set up for the type of inter-method analysis where it could make an educated determination of whether or not call should use stack or heap allocation of `params`.</span></span> <span data-ttu-id="370a4-226">Można tylko naprawdę rozważyć każde wywołanie.</span><span class="sxs-lookup"><span data-stu-id="370a4-226">It can only really consider each call on its own.</span></span>

<span data-ttu-id="370a4-227">Środowisko CLR jest najlepszą konfiguracją dla tego typu wyznaczania w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="370a4-227">The CLR is best setup for making this type of determination at runtime.</span></span> <span data-ttu-id="370a4-228">Dlatego, że środowisko uruchomieniowe może zapewnić dwie metody alokacji uniwersalnego stosu:</span><span class="sxs-lookup"><span data-stu-id="370a4-228">Hence we'd likely have the runtime provide two methods for universal stack allocation:</span></span>

1. <span data-ttu-id="370a4-229">`Span<T> StackAlloc<T>(int length)`: ma takie same zachowania i ograniczenia dotyczące `localloc`, z wyjątkiem tego, że może on korzystać z dowolnego typu `T`.</span><span class="sxs-lookup"><span data-stu-id="370a4-229">`Span<T> StackAlloc<T>(int length)`: this has the same behaviors and limitations of `localloc` except it can work on any type `T`.</span></span> 
1. <span data-ttu-id="370a4-230">`Span<T> MaybeStackAlloc<T>(int length)`: to środowisko uruchomieniowe może zdecydować się na wdrożenie przez wykonanie alokacji stosu lub sterty.</span><span class="sxs-lookup"><span data-stu-id="370a4-230">`Span<T> MaybeStackAlloc<T>(int length)`: this runtime can choose to implement this by doing a stack or heap allocation.</span></span> <span data-ttu-id="370a4-231">Środowisko uruchomieniowe może następnie użyć kontekstu wykonywania, w którym jest wywoływana, aby określić sposób przydzielenia `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="370a4-231">The runtime can then use the execution context in which it's called to determine how the `Span<T>` is allocated.</span></span> <span data-ttu-id="370a4-232">Obiekt wywołujący zawsze będzie traktować go tak, jakby był przydzielony przez stos.</span><span class="sxs-lookup"><span data-stu-id="370a4-232">The caller though will always treat it as if it were stack allocated.</span></span>

<span data-ttu-id="370a4-233">W przypadku bardzo prostych przypadków, takich jak jeden do dwóch argumentów C# , kompilator może zawsze używać `StackAlloc<T>` Variant.</span><span class="sxs-lookup"><span data-stu-id="370a4-233">For very simple cases, like one to two arguments, the C# compiler could always use `StackAlloc<T>` variant.</span></span> <span data-ttu-id="370a4-234">Jest to mało prawdopodobne, aby znacząco przyczynić się do wyczerpania stosu w większości przypadków.</span><span class="sxs-lookup"><span data-stu-id="370a4-234">This is unlikely to significantly contribute to stack exhaustion in most cases.</span></span> <span data-ttu-id="370a4-235">W przypadku innych przypadków kompilator może użyć `MaybeStackAlloc<T>` zamiast tego, aby umożliwić wykonanie wywołania przez środowisko uruchomieniowe.</span><span class="sxs-lookup"><span data-stu-id="370a4-235">For other cases the compiler could choose to use `MaybeStackAlloc<T>` instead and let the runtime make the call.</span></span>

<span data-ttu-id="370a4-236">Wybór będzie prawdopodobnie wymagał dokładniejszego zbadania i zbadania rzeczywistych aplikacji świata.</span><span class="sxs-lookup"><span data-stu-id="370a4-236">How we choose will likely require a deeper investigation and examination of real world apps.</span></span> <span data-ttu-id="370a4-237">Jednak jeśli te nowe funkcje wewnętrzne są dostępne, zapewnia to elastyczność tego typu.</span><span class="sxs-lookup"><span data-stu-id="370a4-237">But if these new intrinsics are available then it will give us this type of flexibility.</span></span>

### <a name="why-not-varargs"></a><span data-ttu-id="370a4-238">Dlaczego VarArgs?</span><span class="sxs-lookup"><span data-stu-id="370a4-238">Why not varargs?</span></span> 
<span data-ttu-id="370a4-239">Istniejąca funkcja [VarArgs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) została uznana za możliwe rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="370a4-239">The existing [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) feature was considered here as a possible solution.</span></span> <span data-ttu-id="370a4-240">Ta funkcja jest przeznaczona głównie dla C++scenariuszy/CLI i ma znane otwory dla innych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="370a4-240">This feature though is meant primarily for C++/CLI scenarios and has known holes for other scenarios.</span></span> <span data-ttu-id="370a4-241">Ponadto w systemie UNIX występuje znaczny koszt.</span><span class="sxs-lookup"><span data-stu-id="370a4-241">Additionally there is significant cost in porting this to Unix.</span></span> <span data-ttu-id="370a4-242">Dlatego nie było ono widoczne jako rozwiązanie do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="370a4-242">Hence it wasn't seen as a viable solution.</span></span>

## <a name="related-issues"></a><span data-ttu-id="370a4-243">Pokrewne problemy</span><span class="sxs-lookup"><span data-stu-id="370a4-243">Related Issues</span></span>
<span data-ttu-id="370a4-244">Ta specyfikacja jest związana z następującymi problemami:</span><span class="sxs-lookup"><span data-stu-id="370a4-244">This spec is related to the following issues:</span></span> 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

