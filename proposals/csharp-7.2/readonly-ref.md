---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2019
ms.locfileid: "79485113"
---
# <a name="readonly-references"></a><span data-ttu-id="26104-101">Odwołania tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="26104-101">Readonly references</span></span>

* <span data-ttu-id="26104-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="26104-102">[x] Proposed</span></span>
* <span data-ttu-id="26104-103">[x] prototyp</span><span class="sxs-lookup"><span data-stu-id="26104-103">[x] Prototype</span></span>
* <span data-ttu-id="26104-104">Implementacja [x]: uruchomiono</span><span class="sxs-lookup"><span data-stu-id="26104-104">[x] Implementation: Started</span></span>
* <span data-ttu-id="26104-105">[] Specyfikacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="26104-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="26104-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="26104-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="26104-107">Funkcja "odwołania tylko do odczytu" jest w rzeczywistości grupą funkcji, które wykorzystują wydajność przekazywania zmiennych przez odwołanie, ale bez uwidaczniania danych do modyfikacji:</span><span class="sxs-lookup"><span data-stu-id="26104-107">The "readonly references" feature is actually a group of features that leverage the efficiency of passing variables by reference, but without exposing the data to modifications:</span></span>  
- <span data-ttu-id="26104-108">parametry `in`</span><span class="sxs-lookup"><span data-stu-id="26104-108">`in` parameters</span></span>
- <span data-ttu-id="26104-109">`ref readonly` zwraca</span><span class="sxs-lookup"><span data-stu-id="26104-109">`ref readonly` returns</span></span>
- <span data-ttu-id="26104-110">struktury `readonly`</span><span class="sxs-lookup"><span data-stu-id="26104-110">`readonly` structs</span></span>
- <span data-ttu-id="26104-111">`ref`/`in` metody rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="26104-111">`ref`/`in` extension methods</span></span>
- <span data-ttu-id="26104-112">`ref readonly` lokalne</span><span class="sxs-lookup"><span data-stu-id="26104-112">`ref readonly` locals</span></span>
- <span data-ttu-id="26104-113">`ref` wyrażenia warunkowe</span><span class="sxs-lookup"><span data-stu-id="26104-113">`ref` conditional expressions</span></span>

## <a name="passing-arguments-as-readonly-references"></a><span data-ttu-id="26104-114">Przekazywanie argumentów jako odwołań tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="26104-114">Passing arguments as readonly references.</span></span>

<span data-ttu-id="26104-115">Istnieje już oferta, która dotyka tego tematu https://github.com/dotnet/roslyn/issues/115 jako specjalnego przypadku parametrów tylko do odczytu bez przechodzenia do wielu szczegółów.</span><span class="sxs-lookup"><span data-stu-id="26104-115">There is an existing proposal that touches this topic https://github.com/dotnet/roslyn/issues/115 as a special case of readonly parameters without going into many details.</span></span>
<span data-ttu-id="26104-116">W tym miejscu chcę potwierdzić, że pomysł nie jest bardzo nowy.</span><span class="sxs-lookup"><span data-stu-id="26104-116">Here I just want to acknowledge that the idea by itself is not very new.</span></span>

### <a name="motivation"></a><span data-ttu-id="26104-117">Motywacją</span><span class="sxs-lookup"><span data-stu-id="26104-117">Motivation</span></span>

<span data-ttu-id="26104-118">Przed tą funkcją C# nie było efektywnego wyrażania chęci przekazania zmiennych struktury do wywołań metod do odczytu, bez zamiaru modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="26104-118">Prior to this feature C# did not have an efficient way of expressing a desire to pass struct variables into method calls for readonly purposes with no intention of modifying.</span></span> <span data-ttu-id="26104-119">Okresowe przekazanie argumentu przez wartość oznacza kopiowanie, które dodaje niepotrzebne koszty.</span><span class="sxs-lookup"><span data-stu-id="26104-119">Regular by-value argument passing implies copying, which adds unnecessary costs.</span></span>  <span data-ttu-id="26104-120">Dzięki temu użytkownicy mogą używać argumentów przez-ref i polegać na komentarzach/dokumentacji, aby wskazać, że dane nie powinny być zmienione przez wywoływany.</span><span class="sxs-lookup"><span data-stu-id="26104-120">That drives users to use by-ref argument passing and rely on comments/documentation to indicate that the data is not supposed to be mutated by the callee.</span></span> <span data-ttu-id="26104-121">Nie jest to dobre rozwiązanie z wielu powodów.</span><span class="sxs-lookup"><span data-stu-id="26104-121">It is not a good solution for many reasons.</span></span>  
<span data-ttu-id="26104-122">Przykłady to wielowektorowe operatory matematyczne w bibliotekach graficznych, takich jak [XNA](https://msdn.microsoft.com/library/bb194944.aspx) , które są znane jako operandy ref ze względu na wydajność.</span><span class="sxs-lookup"><span data-stu-id="26104-122">The examples are numerous - vector/matrix math operators in graphics libraries like [XNA](https://msdn.microsoft.com/library/bb194944.aspx) are known to have ref operands purely because of performance considerations.</span></span> <span data-ttu-id="26104-123">Istnieje kod w kompilatorze Roslyn, który używa struktur, aby uniknąć przydziałów, a następnie przekazuje je przez odwołanie, aby uniknąć kopiowania kosztów.</span><span class="sxs-lookup"><span data-stu-id="26104-123">There is code in Roslyn compiler itself that uses structs to avoid allocations and then passes them by reference to avoid copying costs.</span></span>

### <a name="solution-in-parameters"></a><span data-ttu-id="26104-124">Rozwiązanie (`in` parametry)</span><span class="sxs-lookup"><span data-stu-id="26104-124">Solution (`in` parameters)</span></span>

<span data-ttu-id="26104-125">Podobnie jak parametry `out`, parametry `in` są przenoszone jako odwołania zarządzane z dodatkowymi gwarancjami z wywoływanego elementu.</span><span class="sxs-lookup"><span data-stu-id="26104-125">Similarly to the `out` parameters, `in` parameters are passed as managed references with additional guarantees from the callee.</span></span>  
<span data-ttu-id="26104-126">W przeciwieństwie do parametrów `out`, które _muszą_ być przypisane przez wywoływany przed innym użyciem, `in` parametry nie mogą być przypisywane przez obiekt wywoływany.</span><span class="sxs-lookup"><span data-stu-id="26104-126">Unlike `out` parameters which _must_ be assigned by the callee before any other use, `in` parameters cannot be assigned by the callee at all.</span></span>

<span data-ttu-id="26104-127">W wyniku `in` parametry umożliwiają skuteczne przekazywanie argumentów pośrednich bez ujawniania argumentów do mutacji przez wywoływany.</span><span class="sxs-lookup"><span data-stu-id="26104-127">As a result `in` parameters allow for effectiveness of indirect argument passing without exposing arguments to mutations by the callee.</span></span>

### <a name="declaring-in-parameters"></a><span data-ttu-id="26104-128">Deklarowanie parametrów `in`</span><span class="sxs-lookup"><span data-stu-id="26104-128">Declaring `in` parameters</span></span>

<span data-ttu-id="26104-129">parametry `in` są zadeklarowane za pomocą słowa kluczowego `in` jako modyfikatora w sygnaturze parametru.</span><span class="sxs-lookup"><span data-stu-id="26104-129">`in` parameters are declared by using `in` keyword as a modifier in the parameter signature.</span></span>

<span data-ttu-id="26104-130">We wszystkich celach `in` parametr jest traktowany jako zmienna `readonly`a.</span><span class="sxs-lookup"><span data-stu-id="26104-130">For all purposes the `in` parameter is treated as a `readonly` variable.</span></span> <span data-ttu-id="26104-131">Większość ograniczeń dotyczących używania parametrów `in` wewnątrz metody jest taka sama jak w przypadku pól `readonly`.</span><span class="sxs-lookup"><span data-stu-id="26104-131">Most of the restrictions on the use of `in` parameters inside the method are the same as with `readonly` fields.</span></span>

> <span data-ttu-id="26104-132">W rzeczywistości parametr `in` może reprezentować pole `readonly`.</span><span class="sxs-lookup"><span data-stu-id="26104-132">Indeed an `in` parameter may represent a `readonly` field.</span></span> <span data-ttu-id="26104-133">Podobieństwo ograniczeń nie jest współzakresem.</span><span class="sxs-lookup"><span data-stu-id="26104-133">Similarity of restrictions is not a coincidence.</span></span>

<span data-ttu-id="26104-134">Na przykład pola `in` parametru, który ma typ struktury, są rekursywnie klasyfikowane jako zmienne `readonly`.</span><span class="sxs-lookup"><span data-stu-id="26104-134">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- <span data-ttu-id="26104-135">parametry `in` są dozwolone wszędzie tam, gdzie są dozwolone zwykłe parametry ByVal.</span><span class="sxs-lookup"><span data-stu-id="26104-135">`in` parameters are allowed anywhere where ordinary byval parameters are allowed.</span></span> <span data-ttu-id="26104-136">Dotyczy to indeksatorów, operatorów (w tym konwersji), delegatów, wyrażeń lambda, funkcji lokalnych.</span><span class="sxs-lookup"><span data-stu-id="26104-136">This includes indexers, operators (including conversions), delegates, lambdas, local functions.</span></span>

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- <span data-ttu-id="26104-137">`in` nie jest dozwolona w połączeniu z `out` lub ze wszystkimi, które `out` nie łączą się z.</span><span class="sxs-lookup"><span data-stu-id="26104-137">`in` is not allowed in combination with `out` or with anything that `out` does not combine with.</span></span>

- <span data-ttu-id="26104-138">Nie jest dozwolone Przeciążenie na `ref`/`out`/`in` różnice.</span><span class="sxs-lookup"><span data-stu-id="26104-138">It is not permitted to overload on `ref`/`out`/`in` differences.</span></span>

- <span data-ttu-id="26104-139">Możliwe jest Przeciążenie zwykłych i `in`ych różnic.</span><span class="sxs-lookup"><span data-stu-id="26104-139">It is permitted to overload on ordinary byval and `in` differences.</span></span>

- <span data-ttu-id="26104-140">Na potrzeby OHI (Przeciążenie, ukrywanie, implementowanie) `in` zachowuje się podobnie do `out` parametr.</span><span class="sxs-lookup"><span data-stu-id="26104-140">For the purpose of OHI (Overloading, Hiding, Implementing), `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="26104-141">Wszystkie te same reguły mają zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="26104-141">All the same rules apply.</span></span>
<span data-ttu-id="26104-142">Na przykład Metoda zastępująca będzie musiała dopasować `in` parametry z parametrami `in` o typie konwersji tożsamości.</span><span class="sxs-lookup"><span data-stu-id="26104-142">For example the overriding method will have to match `in` parameters with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="26104-143">Na potrzeby konwersji obiektów delegowanych/lambda/grup metod `in` zachowuje się podobnie do `out` parametru.</span><span class="sxs-lookup"><span data-stu-id="26104-143">For the purpose of delegate/lambda/method group conversions, `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="26104-144">Wyrażenia lambda i odpowiednie sugestie konwersji grup metod będą musiały pasować do `in` parametrów delegata docelowego z parametrami `in` o typie konwersji tożsamości.</span><span class="sxs-lookup"><span data-stu-id="26104-144">Lambdas and applicable method group conversion candidates will have to match `in` parameters of the target delegate with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="26104-145">Na potrzeby wariancji ogólnej `in` parametry nie są wariantem.</span><span class="sxs-lookup"><span data-stu-id="26104-145">For the purpose of generic variance, `in` parameters are nonvariant.</span></span>

> <span data-ttu-id="26104-146">Uwaga: nie ma ostrzeżeń dotyczących parametrów `in`, które mają typ referencyjny lub podstawowy.</span><span class="sxs-lookup"><span data-stu-id="26104-146">NOTE: There are no warnings on `in` parameters that have reference or primitives types.</span></span>
<span data-ttu-id="26104-147">Może to być bezpunktowe, ale w niektórych przypadkach użytkownik musi/chcieć przekazać elementy pierwotne jako `in`.</span><span class="sxs-lookup"><span data-stu-id="26104-147">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="26104-148">Przykłady — przesłanianie metody ogólnej, takiej jak `Method(in T param)`, gdy `T` został zastąpiony jako `int`lub gdy mają takie metody jak `Volatile.Read(in int location)`</span><span class="sxs-lookup"><span data-stu-id="26104-148">Examples - overriding a generic method like `Method(in T param)` when `T` was substituted to be `int`, or when having methods like `Volatile.Read(in int location)`</span></span>
>
> <span data-ttu-id="26104-149">Możliwe jest posiadanie analizatora, który ostrzega w przypadku niewydajnego użycia parametrów `in`, ale zasady takiej analizy byłyby zbyt rozmyte, aby były częścią specyfikacji języka.</span><span class="sxs-lookup"><span data-stu-id="26104-149">It is conceivable to have an analyzer that warns in cases of inefficient use of `in` parameters, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="use-of-in-at-call-sites-in-arguments"></a><span data-ttu-id="26104-150">Używanie `in` w lokacjach wywołań.</span><span class="sxs-lookup"><span data-stu-id="26104-150">Use of `in` at call sites.</span></span> <span data-ttu-id="26104-151">(`in` argumenty)</span><span class="sxs-lookup"><span data-stu-id="26104-151">(`in` arguments)</span></span>

<span data-ttu-id="26104-152">Istnieją dwa sposoby przekazywania argumentów do parametrów `in`.</span><span class="sxs-lookup"><span data-stu-id="26104-152">There are two ways to pass arguments to `in` parameters.</span></span>

#### <a name="in-arguments-can-match-in-parameters"></a><span data-ttu-id="26104-153">argumenty `in` mogą być zgodne z parametrami `in`:</span><span class="sxs-lookup"><span data-stu-id="26104-153">`in` arguments can match `in` parameters:</span></span>

<span data-ttu-id="26104-154">Argument z modyfikatorem `in` w miejscu wywołania może pasować do `in` parametrów.</span><span class="sxs-lookup"><span data-stu-id="26104-154">An argument with an `in` modifier at the call site can match `in` parameters.</span></span>

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- <span data-ttu-id="26104-155">argument `in` musi być LValue do _odczytu_ (\*).</span><span class="sxs-lookup"><span data-stu-id="26104-155">`in` argument must be a _readable_ LValue(\*).</span></span>
<span data-ttu-id="26104-156">Przykład: `M1(in 42)` jest nieprawidłowy</span><span class="sxs-lookup"><span data-stu-id="26104-156">Example: `M1(in 42)` is invalid</span></span>

> <span data-ttu-id="26104-157">(\*) Pojęcie [LValue/rvalue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) różni się między językami.</span><span class="sxs-lookup"><span data-stu-id="26104-157">(\*) The notion of [LValue/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) vary between languages.</span></span>  
<span data-ttu-id="26104-158">Tutaj, LValue I oznaczają wyrażenie reprezentujące lokalizację, do której można odwoływać się bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="26104-158">Here, by LValue I mean an expression that represent a location that can be referred to directly.</span></span>
<span data-ttu-id="26104-159">I RValue oznacza wyrażenie, które zwraca wynik tymczasowy, który nie jest na siebie własny.</span><span class="sxs-lookup"><span data-stu-id="26104-159">And RValue means an expression that yields a temporary result which does not persist on its own.</span></span>  

- <span data-ttu-id="26104-160">W szczególności prawidłowe jest przekazywanie `readonly` pól, `in` parametrów lub innych `readonly`ych zmiennych jako argumentów `in`.</span><span class="sxs-lookup"><span data-stu-id="26104-160">In particular it is valid to pass `readonly` fields, `in` parameters or other formally `readonly` variables as `in` arguments.</span></span>
<span data-ttu-id="26104-161">Przykład: `dictionary[in Guid.Empty]` jest dozwolony.</span><span class="sxs-lookup"><span data-stu-id="26104-161">Example: `dictionary[in Guid.Empty]` is legal.</span></span> <span data-ttu-id="26104-162">`Guid.Empty` to statyczne pole tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="26104-162">`Guid.Empty` is a static readonly field.</span></span>

- <span data-ttu-id="26104-163">argument `in` musi mieć typ _identyfikujący tożsamość_ do typu parametru.</span><span class="sxs-lookup"><span data-stu-id="26104-163">`in` argument must have type _identity-convertible_ to the type of the parameter.</span></span>
<span data-ttu-id="26104-164">Przykład: `M1<object>(in Guid.Empty)` jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="26104-164">Example: `M1<object>(in Guid.Empty)` is invalid.</span></span> <span data-ttu-id="26104-165">`Guid.Empty` nie jest możliwa do _konwersji tożsamości_ na `object`</span><span class="sxs-lookup"><span data-stu-id="26104-165">`Guid.Empty` is not _identity-convertible_ to `object`</span></span>

<span data-ttu-id="26104-166">Motywacja dla powyższych reguł polega na tym, że argumenty `in`ą gwarantuje _alias_ zmiennej argumentu.</span><span class="sxs-lookup"><span data-stu-id="26104-166">The motivation for the above rules is that `in` arguments guarantee _aliasing_ of the argument variable.</span></span> <span data-ttu-id="26104-167">Wywoływany zawsze otrzymuje bezpośrednie odwołanie do tej samej lokalizacji, co reprezentowane przez argument.</span><span class="sxs-lookup"><span data-stu-id="26104-167">The callee always receives a direct reference to the same location as represented by the argument.</span></span>

- <span data-ttu-id="26104-168">w rzadkich sytuacjach, gdy argumenty `in` muszą być rozłożone na stosie z powodu `await` wyrażeń używanych jako operandy tego samego wywołania, zachowanie jest takie samo jak w przypadku argumentów `out` i `ref` — Jeśli zmienna nie może zostać rozlana w sposób referencyjny, zgłaszany jest błąd.</span><span class="sxs-lookup"><span data-stu-id="26104-168">in rare situations when `in` arguments must be stack-spilled due to `await` expressions used as operands of the same call, the behavior is the same as with `out` and `ref` arguments - if the variable cannot be spilled in referentially-transparent manner, an error is reported.</span></span>

<span data-ttu-id="26104-169">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="26104-169">Examples:</span></span>
1. <span data-ttu-id="26104-170">`M1(in staticField, await SomethingAsync())` jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="26104-170">`M1(in staticField, await SomethingAsync())`  is valid.</span></span>
<span data-ttu-id="26104-171">`staticField` to pole statyczne, do którego można uzyskać dostęp więcej niż jeden raz bez zauważalnych efektów ubocznych.</span><span class="sxs-lookup"><span data-stu-id="26104-171">`staticField` is a static field which can be accessed more than once without observable side effects.</span></span> <span data-ttu-id="26104-172">W związku z tym można podać kolejność efektów ubocznych i wymagań dotyczących aliasów.</span><span class="sxs-lookup"><span data-stu-id="26104-172">Therefore both the order of side effects and aliasing requirements can be provided.</span></span>
2. <span data-ttu-id="26104-173">`M1(in RefReturningMethod(), await SomethingAsync())` spowoduje wystąpienie błędu.</span><span class="sxs-lookup"><span data-stu-id="26104-173">`M1(in RefReturningMethod(), await SomethingAsync())`  will produce an error.</span></span>
<span data-ttu-id="26104-174">`RefReturningMethod()` jest `ref` zwracająca metodę.</span><span class="sxs-lookup"><span data-stu-id="26104-174">`RefReturningMethod()` is a `ref` returning method.</span></span> <span data-ttu-id="26104-175">Wywołanie metody mogło mieć zauważalne efekty uboczne, dlatego należy je ocenić przed operandem `SomethingAsync()`.</span><span class="sxs-lookup"><span data-stu-id="26104-175">A method call may have observable side effects, therefore it must be evaluated before the `SomethingAsync()` operand.</span></span> <span data-ttu-id="26104-176">Jednak wynik wywołania jest odwołaniem, które nie może być zachowane przez punkt zawieszenia `await`, co sprawia, że bezpośrednie wymaganie referencyjne nie jest możliwe.</span><span class="sxs-lookup"><span data-stu-id="26104-176">However the result of the invocation is a reference that cannot be preserved across the `await` suspension point which make the direct reference requirement impossible.</span></span>

> <span data-ttu-id="26104-177">Uwaga: Błędy rozlewania stosu są uznawane za ograniczenia specyficzne dla implementacji.</span><span class="sxs-lookup"><span data-stu-id="26104-177">NOTE: the stack spilling errors are considered to be implementation-specific limitations.</span></span> <span data-ttu-id="26104-178">W związku z tym nie mają one wpływu na rozpoznanie przeciążenia lub wnioskowanie lambda.</span><span class="sxs-lookup"><span data-stu-id="26104-178">Therefore they do not have effect on overload resolution or lambda inference.</span></span>

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a><span data-ttu-id="26104-179">Zwykłe argumenty ByVal mogą pasować do `in` parametrów:</span><span class="sxs-lookup"><span data-stu-id="26104-179">Ordinary byval arguments can match `in` parameters:</span></span>

<span data-ttu-id="26104-180">Regularne argumenty bez modyfikatorów mogą odpowiadać `in` parametrom.</span><span class="sxs-lookup"><span data-stu-id="26104-180">Regular arguments without modifiers can match `in` parameters.</span></span> <span data-ttu-id="26104-181">W takim przypadku argumenty mają takie same ograniczenia swobodne jak zwykłe argumenty ByVal.</span><span class="sxs-lookup"><span data-stu-id="26104-181">In such case the arguments have the same relaxed constraints as an ordinary byval arguments would have.</span></span>

<span data-ttu-id="26104-182">W tym scenariuszu jest to, że `in` parametry w interfejsach API mogą spowodować niewygodę dla użytkownika, gdy argumenty nie mogą być przekazane jako odwołanie bezpośrednie-ex: literały, obliczone lub `await`e wyniki lub argumenty, które wystąpiły w celu uzyskania bardziej szczegółowych typów.</span><span class="sxs-lookup"><span data-stu-id="26104-182">The motivation for this scenario is that `in` parameters in APIs may result in inconveniences for the user when arguments cannot be passed as a direct reference - ex: literals, computed or `await`-ed results or arguments that happen to have more specific types.</span></span>  
<span data-ttu-id="26104-183">Wszystkie te przypadki mają proste rozwiązanie do przechowywania wartości argumentu w tymczasowym lokalnym odpowiednim typie i przekazanie go jako argumentu `in`.</span><span class="sxs-lookup"><span data-stu-id="26104-183">All these cases have a trivial solution of storing the argument value in a temporary local of appropriate type and passing that local as an `in` argument.</span></span>  
<span data-ttu-id="26104-184">Aby ograniczyć potrzebę tego, że kompilator kodu standardowego może wykonać to samo przekształcenie, w razie potrzeby, gdy `in` modyfikator nie jest obecny w witrynie wywołania.</span><span class="sxs-lookup"><span data-stu-id="26104-184">To reduce the need for such boilerplate code compiler can perform the same transformation, if needed, when `in` modifier is not present at the call site.</span></span>  

<span data-ttu-id="26104-185">Ponadto, w niektórych przypadkach, takich jak wywołanie operatorów lub `in` metody rozszerzenia, nie ma znaczenia, aby określić `in` w ogóle.</span><span class="sxs-lookup"><span data-stu-id="26104-185">In addition, in some cases, such as invocation of operators, or `in` extension methods, there is no syntactical way to specify `in` at all.</span></span> <span data-ttu-id="26104-186">To samo wymaga określenia zachowania zwykłych argumentów ByVal, gdy pasują do `in` parametrów.</span><span class="sxs-lookup"><span data-stu-id="26104-186">That alone requires specifying the behavior of ordinary byval arguments when they match `in` parameters.</span></span>

<span data-ttu-id="26104-187">W szczególności:</span><span class="sxs-lookup"><span data-stu-id="26104-187">In particular:</span></span>

- <span data-ttu-id="26104-188">prawidłowe jest przekazanie RValues.</span><span class="sxs-lookup"><span data-stu-id="26104-188">it is valid to pass RValues.</span></span>
<span data-ttu-id="26104-189">W takim przypadku jest przesyłane odwołanie do tymczasowej.</span><span class="sxs-lookup"><span data-stu-id="26104-189">A reference to a temporary is passed in such case.</span></span>
<span data-ttu-id="26104-190">Przykład:</span><span class="sxs-lookup"><span data-stu-id="26104-190">Example:</span></span>
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- <span data-ttu-id="26104-191">niejawne konwersje są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="26104-191">implicit conversions are allowed.</span></span>

> <span data-ttu-id="26104-192">Jest to w rzeczywistości szczególnym przypadkiem przekazania RValue</span><span class="sxs-lookup"><span data-stu-id="26104-192">This is actually a special case of passing an RValue</span></span>  

<span data-ttu-id="26104-193">W takim przypadku jest przesyłane odwołanie do tymczasowej przekonwertowanej wartości.</span><span class="sxs-lookup"><span data-stu-id="26104-193">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="26104-194">Przykład:</span><span class="sxs-lookup"><span data-stu-id="26104-194">Example:</span></span>
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- <span data-ttu-id="26104-195">w przypadku odbiorcy metody rozszerzenia `in` (w przeciwieństwie do `ref` metod rozszerzenia) RValues lub niejawne _konwersje this-arguments_ są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="26104-195">in a case of a receiver of an `in` extension method (as opposed to `ref` extension methods), RValues or implicit _this-argument-conversions_ are allowed.</span></span>
<span data-ttu-id="26104-196">W takim przypadku jest przesyłane odwołanie do tymczasowej przekonwertowanej wartości.</span><span class="sxs-lookup"><span data-stu-id="26104-196">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="26104-197">Przykład:</span><span class="sxs-lookup"><span data-stu-id="26104-197">Example:</span></span>
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
<span data-ttu-id="26104-198">Więcej informacji na temat `ref`/`in` metod rozszerzenia znajduje się w dalszej części tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="26104-198">More information on `ref`/`in` extension methods is provided further in this document.</span></span>

- <span data-ttu-id="26104-199">rozłożenie argumentu z powodu `await` operandy mogą rozlać "według wartości", jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="26104-199">argument spilling due to `await` operands could spill "by-value", if necessary.</span></span>
<span data-ttu-id="26104-200">W scenariuszach, w których bezpośrednie odwołanie do argumentu nie jest możliwe z powodu interwencji, `await` zamiast niej zostanie rozlana kopia wartości argumentu.</span><span class="sxs-lookup"><span data-stu-id="26104-200">In scenarios where providing a direct reference to the argument is not possible due to intervening `await` a copy of the argument's value is spilled instead.</span></span>  
<span data-ttu-id="26104-201">Przykład:</span><span class="sxs-lookup"><span data-stu-id="26104-201">Example:</span></span>
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
<span data-ttu-id="26104-202">Ponieważ wynik wywołania bocznego jest odwołaniem, które nie może zostać zachowane między `await` zawieszeniem, zamiast tego zostanie zachowana tymczasowa zawierająca wartość rzeczywistą (podobnie jak w przypadku zwykłego parametru ByVal).</span><span class="sxs-lookup"><span data-stu-id="26104-202">Since the result of a side-effecting invocation is a reference that cannot be preserved across `await` suspension, a temporary containing the actual value will be preserved instead (as it would in an ordinary byval parameter case).</span></span>

#### <a name="omitted-optional-arguments"></a><span data-ttu-id="26104-203">Pominięto opcjonalne argumenty</span><span class="sxs-lookup"><span data-stu-id="26104-203">Omitted optional arguments</span></span>

<span data-ttu-id="26104-204">Aby określić wartość domyślną, jest dozwolony parametr `in`.</span><span class="sxs-lookup"><span data-stu-id="26104-204">It is permitted for an `in` parameter to specify a default value.</span></span> <span data-ttu-id="26104-205">Powoduje to, że odpowiedni argument jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="26104-205">That makes the corresponding argument optional.</span></span>

<span data-ttu-id="26104-206">Pominięcie opcjonalnego argumentu w witrynie wywołania skutkuje przekazaniem wartości domyślnej za pośrednictwem tymczasowej.</span><span class="sxs-lookup"><span data-stu-id="26104-206">Omitting optional argument at the call site results in passing the default value via a temporary.</span></span>

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a><span data-ttu-id="26104-207">Ogólne zachowanie aliasu</span><span class="sxs-lookup"><span data-stu-id="26104-207">Aliasing behavior in general</span></span>

<span data-ttu-id="26104-208">Podobnie jak zmienne `ref` i `out`, zmienne `in` są odwołaniami/aliasami do istniejących lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="26104-208">Just like `ref` and `out` variables, `in` variables are references/aliases to existing locations.</span></span>

<span data-ttu-id="26104-209">Gdy wywoływany element nie może zapisywać w nich, odczyt parametru `in` może obserwować różne wartości jako efekt uboczny innych ocen.</span><span class="sxs-lookup"><span data-stu-id="26104-209">While callee is not allowed to write into them, reading an `in` parameter can observe different values as a side effect of other evaluations.</span></span>

<span data-ttu-id="26104-210">Przykład:</span><span class="sxs-lookup"><span data-stu-id="26104-210">Example:</span></span>

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a><span data-ttu-id="26104-211">`in` parametry i przechwytywanie zmiennych lokalnych.</span><span class="sxs-lookup"><span data-stu-id="26104-211">`in` parameters and capturing of local variables.</span></span>  
<span data-ttu-id="26104-212">Na potrzeby przechwytywania lambda/async `in` parametry zachowują się tak samo jak parametry `out` i `ref`.</span><span class="sxs-lookup"><span data-stu-id="26104-212">For the purpose of lambda/async capturing `in` parameters behave the same as `out` and `ref` parameters.</span></span>

- <span data-ttu-id="26104-213">nie można przechwycić parametrów `in` w zamknięciu</span><span class="sxs-lookup"><span data-stu-id="26104-213">`in` parameters cannot be captured in a closure</span></span>
- <span data-ttu-id="26104-214">parametry `in` są niedozwolone w metodach iteratora</span><span class="sxs-lookup"><span data-stu-id="26104-214">`in` parameters are not allowed in iterator methods</span></span>
- <span data-ttu-id="26104-215">parametry `in` są niedozwolone w metodach asynchronicznych</span><span class="sxs-lookup"><span data-stu-id="26104-215">`in` parameters are not allowed in async methods</span></span>

### <a name="temporary-variables"></a><span data-ttu-id="26104-216">Zmienne tymczasowe.</span><span class="sxs-lookup"><span data-stu-id="26104-216">Temporary variables.</span></span>  
<span data-ttu-id="26104-217">Niektóre zastosowania przekazywania parametrów `in` mogą wymagać pośredniego użycia tymczasowej zmiennej lokalnej:</span><span class="sxs-lookup"><span data-stu-id="26104-217">Some uses of `in` parameter passing may require indirect use of a temporary local variable:</span></span>  
- <span data-ttu-id="26104-218">argumenty `in` są zawsze przesyłane jako aliasy bezpośrednie, gdy lokacja wywołań używa `in`.</span><span class="sxs-lookup"><span data-stu-id="26104-218">`in` arguments are always passed as direct aliases when call-site uses `in`.</span></span> <span data-ttu-id="26104-219">W takim przypadku czas tymczasowy nie jest używany.</span><span class="sxs-lookup"><span data-stu-id="26104-219">Temporary is never used in such case.</span></span>
- <span data-ttu-id="26104-220">argumenty `in` nie muszą być bezpośrednimi aliasami, gdy lokacja wywołań nie korzysta z `in`.</span><span class="sxs-lookup"><span data-stu-id="26104-220">`in` arguments are not required to be direct aliases when call-site does not use `in`.</span></span> <span data-ttu-id="26104-221">Gdy argument nie jest LValue, może być używany tymczasowy.</span><span class="sxs-lookup"><span data-stu-id="26104-221">When argument is not an LValue, a temporary may be used.</span></span>
- <span data-ttu-id="26104-222">parametr `in` może mieć wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="26104-222">`in` parameter may have default value.</span></span> <span data-ttu-id="26104-223">Gdy odpowiedni argument zostanie pominięty w miejscu wywołania, wartość domyślna jest przenoszona za pośrednictwem tymczasowej.</span><span class="sxs-lookup"><span data-stu-id="26104-223">When corresponding argument is omitted at the call site, the default value are passed via a temporary.</span></span>
- <span data-ttu-id="26104-224">argumenty `in` mogą mieć niejawne konwersje, w tym te, które nie zachowują tożsamości.</span><span class="sxs-lookup"><span data-stu-id="26104-224">`in` arguments may have implicit conversions, including those that do not preserve identity.</span></span> <span data-ttu-id="26104-225">W tych przypadkach jest używany tymczasowy.</span><span class="sxs-lookup"><span data-stu-id="26104-225">A temporary is used in those cases.</span></span>
- <span data-ttu-id="26104-226">odbiorniki zwykłych wywołań struktury nie mogą być zapisywalne LValues (**istniejąca sprawa!** ).</span><span class="sxs-lookup"><span data-stu-id="26104-226">receivers of ordinary struct calls may not be writeable LValues (**existing case!**).</span></span> <span data-ttu-id="26104-227">W tych przypadkach jest używany tymczasowy.</span><span class="sxs-lookup"><span data-stu-id="26104-227">A temporary is used in those cases.</span></span>

<span data-ttu-id="26104-228">Czas życia argumentu obiekty tymczasowe jest zgodny z najbliższym zakresem obejmującym lokację wywołania.</span><span class="sxs-lookup"><span data-stu-id="26104-228">The life time of the argument temporaries matches the closest encompassing scope of the call-site.</span></span>

<span data-ttu-id="26104-229">Formalny czas życia zmiennych tymczasowych jest semantycznie znaczący w scenariuszach obejmujących analizę ucieczki zmiennych zwracanych przez odwołanie.</span><span class="sxs-lookup"><span data-stu-id="26104-229">The formal life time of temporary variables is semantically significant in scenarios involving escape analysis of variables returned by reference.</span></span>

### <a name="metadata-representation-of-in-parameters"></a><span data-ttu-id="26104-230">Reprezentacja metadanych parametrów `in`.</span><span class="sxs-lookup"><span data-stu-id="26104-230">Metadata representation of `in` parameters.</span></span>
<span data-ttu-id="26104-231">Gdy `System.Runtime.CompilerServices.IsReadOnlyAttribute` jest stosowany do parametru ByRef, oznacza to, że parametr jest parametrem `in`.</span><span class="sxs-lookup"><span data-stu-id="26104-231">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a byref parameter, it means that the parameter is an `in` parameter.</span></span>

<span data-ttu-id="26104-232">Ponadto, jeśli metoda jest *abstrakcyjna* lub *wirtualna*, sygnatura takich parametrów (i tylko tych parametrów) musi mieć `modreq[System.Runtime.InteropServices.InAttribute]`.</span><span class="sxs-lookup"><span data-stu-id="26104-232">In addition, if the method is *abstract* or *virtual*, then the signature of such parameters (and only such parameters) must have `modreq[System.Runtime.InteropServices.InAttribute]`.</span></span>

<span data-ttu-id="26104-233">**Motywacja**: jest to wykonywane w celu zapewnienia, że w przypadku metody przesłaniania parametrów `in` są zgodne.</span><span class="sxs-lookup"><span data-stu-id="26104-233">**Motivation**: this is done to ensure that in a case of method overriding/implementing the `in` parameters match.</span></span>

<span data-ttu-id="26104-234">Te same wymagania dotyczą metod `Invoke` w delegatach.</span><span class="sxs-lookup"><span data-stu-id="26104-234">Same requirements apply to `Invoke` methods in delegates.</span></span>

<span data-ttu-id="26104-235">**Motywacja**: to gwarantuje, że istniejący kompilator nie będzie mógł po prostu zignorować `readonly` podczas tworzenia lub przypisywania delegatów.</span><span class="sxs-lookup"><span data-stu-id="26104-235">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when creating or assigning delegates.</span></span>

## <a name="returning-by-readonly-reference"></a><span data-ttu-id="26104-236">Zwracanie przez odwołanie tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="26104-236">Returning by readonly reference.</span></span>

### <a name="motivation"></a><span data-ttu-id="26104-237">Motywacją</span><span class="sxs-lookup"><span data-stu-id="26104-237">Motivation</span></span>
<span data-ttu-id="26104-238">Motywacja tej funkcji podrzędnej jest w przybliżeniu symetryczna do przyczyn parametrów `in`-unikając kopiowania, ale po stronie zwracającej.</span><span class="sxs-lookup"><span data-stu-id="26104-238">The motivation for this sub-feature is roughly symmetrical to the reasons for the `in` parameters - avoiding copying, but on the returning side.</span></span> <span data-ttu-id="26104-239">Przed tą funkcją, metoda lub indeksator miał dwie opcje: 1) zwraca przez odwołanie i jest narażony na możliwe mutacje lub 2) zwraca przez wartość, która skutkuje kopiowaniem.</span><span class="sxs-lookup"><span data-stu-id="26104-239">Prior to this feature, a method or an indexer had two options: 1) return by reference and be exposed to possible mutations or 2) return by value which results in copying.</span></span>

### <a name="solution-ref-readonly-returns"></a><span data-ttu-id="26104-240">Rozwiązanie (`ref readonly` zwraca)</span><span class="sxs-lookup"><span data-stu-id="26104-240">Solution (`ref readonly` returns)</span></span>  
<span data-ttu-id="26104-241">Funkcja umożliwia elementowi członkowskiemu zwracanie zmiennych przez odwołanie bez uwidaczniania ich do mutacji.</span><span class="sxs-lookup"><span data-stu-id="26104-241">The feature allows a member to return variables by reference without exposing them to mutations.</span></span>

### <a name="declaring-ref-readonly-returning-members"></a><span data-ttu-id="26104-242">Deklarowanie `ref readonly` zwracających elementy członkowskie</span><span class="sxs-lookup"><span data-stu-id="26104-242">Declaring `ref readonly` returning members</span></span>

<span data-ttu-id="26104-243">Kombinacja modyfikatorów `ref readonly` w sygnaturze zwrotnym służy do wskazywania, że element członkowski zwraca odwołanie tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="26104-243">A combination of modifiers `ref readonly` on the return signature is used to to indicate that the member returns a readonly reference.</span></span>

<span data-ttu-id="26104-244">Dla wszystkich celów `ref readonly` skład jest traktowany jako zmienna `readonly` — podobna do `readonly` pól i `in` parametrów.</span><span class="sxs-lookup"><span data-stu-id="26104-244">For all purposes a `ref readonly` member is treated as a `readonly` variable - similar to `readonly` fields and `in` parameters.</span></span>

<span data-ttu-id="26104-245">Na przykład pola elementu członkowskiego `ref readonly`, który ma typ struktury, są rekursywnie klasyfikowane jako zmienne `readonly`.</span><span class="sxs-lookup"><span data-stu-id="26104-245">For example fields of `ref readonly` member which has a struct type are all recursively classified as `readonly` variables.</span></span> <span data-ttu-id="26104-246">-Może przekazać je jako argumenty `in`, ale nie jako argumenty `ref` lub `out`.</span><span class="sxs-lookup"><span data-stu-id="26104-246">- It is permitted to pass them as `in` arguments, but not as `ref` or `out` arguments.</span></span>

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- <span data-ttu-id="26104-247">w tych samych miejscach są dozwolone `ref readonly` Returns, `ref` są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="26104-247">`ref readonly` returns are allowed in the same places were `ref` returns are allowed.</span></span>
<span data-ttu-id="26104-248">Dotyczy to indeksatorów, delegatów, wyrażeń lambda, funkcji lokalnych.</span><span class="sxs-lookup"><span data-stu-id="26104-248">This includes indexers, delegates, lambdas, local functions.</span></span>

- <span data-ttu-id="26104-249">Nie jest dozwolone Przeciążenie na `ref`/`ref readonly`/różnice.</span><span class="sxs-lookup"><span data-stu-id="26104-249">It is not permitted to overload on `ref`/`ref readonly` /  differences.</span></span>

- <span data-ttu-id="26104-250">Jest dozwolone Przeciążenie na zwykłych elementach ByVal i `ref readonly` Zwróć różnice.</span><span class="sxs-lookup"><span data-stu-id="26104-250">It is permitted to overload on ordinary byval and `ref readonly` return differences.</span></span>

- <span data-ttu-id="26104-251">Na potrzeby OHI (Przeciążenie, ukrywanie, implementowanie) `ref readonly` jest podobny, ale różni się od `ref`.</span><span class="sxs-lookup"><span data-stu-id="26104-251">For the purpose of OHI (Overloading, Hiding, Implementing), `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="26104-252">Na przykład metoda, która zastępuje `ref readonly` jeden, musi być `ref readonly` i mieć typ zamienny tożsamości.</span><span class="sxs-lookup"><span data-stu-id="26104-252">For example the a method that overrides `ref readonly` one, must itself be `ref readonly` and have identity-convertible type.</span></span>

- <span data-ttu-id="26104-253">Na potrzeby konwersji obiektów delegowanych/lambda/grup metod `ref readonly` jest podobna, ale różni się od `ref`.</span><span class="sxs-lookup"><span data-stu-id="26104-253">For the purpose of delegate/lambda/method group conversions, `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="26104-254">Wyrażenia lambda i odpowiednie sugestie konwersji grup metod muszą pasować do `ref readonly` zwracanego delegata docelowego z `ref readonly` zwracanym typem, który jest konwertowany na tożsamość.</span><span class="sxs-lookup"><span data-stu-id="26104-254">Lambdas and applicable method group conversion candidates have to match `ref readonly` return of the target delegate with `ref readonly` return of the type that is identity-convertible.</span></span>

- <span data-ttu-id="26104-255">Na potrzeby wariancji ogólnej `ref readonly` zwracane są nievariant.</span><span class="sxs-lookup"><span data-stu-id="26104-255">For the purpose of generic variance, `ref readonly` returns are nonvariant.</span></span>

> <span data-ttu-id="26104-256">Uwaga: nie ma żadnych ostrzeżeń dotyczących `ref readonly` zwraca, które mają typ referencyjny lub podstawowy.</span><span class="sxs-lookup"><span data-stu-id="26104-256">NOTE: There are no warnings on `ref readonly` returns that have reference or primitives types.</span></span>
<span data-ttu-id="26104-257">Może to być bezpunktowe, ale w niektórych przypadkach użytkownik musi/chcieć przekazać elementy pierwotne jako `in`.</span><span class="sxs-lookup"><span data-stu-id="26104-257">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="26104-258">Przykłady — przesłanianie metody ogólnej, takiej jak `ref readonly T Method()`, gdy `T` został zastąpiony jako `int`.</span><span class="sxs-lookup"><span data-stu-id="26104-258">Examples - overriding a generic method like `ref readonly T Method()` when `T` was substituted to be `int`.</span></span>
>
><span data-ttu-id="26104-259">Możliwe jest posiadanie analizatora, który ostrzega w przypadku niewydajnego użycia `ref readonly` zwraca, ale reguły dla takiej analizy byłyby zbyt rozmyte, aby były częścią specyfikacji języka.</span><span class="sxs-lookup"><span data-stu-id="26104-259">It is conceivable to have an analyzer that warns in cases of inefficient use of `ref readonly` returns, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="returning-from-ref-readonly-members"></a><span data-ttu-id="26104-260">Powrót z `ref readonly` elementów członkowskich</span><span class="sxs-lookup"><span data-stu-id="26104-260">Returning from `ref readonly` members</span></span>
<span data-ttu-id="26104-261">Wewnątrz treści metody składnia jest taka sama jak w przypadku zwykłych zwrotów ref.</span><span class="sxs-lookup"><span data-stu-id="26104-261">Inside the method body the syntax is the same as with regular ref returns.</span></span> <span data-ttu-id="26104-262">`readonly` zostanie wywnioskowana z metody zawierającej.</span><span class="sxs-lookup"><span data-stu-id="26104-262">The `readonly` will be inferred from the containing method.</span></span>

<span data-ttu-id="26104-263">Motywacja to, że `return ref readonly <expression>` jest niepotrzebna i zezwala tylko na niezgodności w części `readonly`, która zawsze spowoduje błędy.</span><span class="sxs-lookup"><span data-stu-id="26104-263">The motivation is that `return ref readonly <expression>` is unnecessary long and only allows for mismatches on the `readonly` part that would always result in errors.</span></span>
<span data-ttu-id="26104-264">`ref` jest jednak wymagana w celu zapewnienia spójności z innymi scenariuszami, w których coś jest przesyłane za pośrednictwem ścisłych aliasów a przez wartość.</span><span class="sxs-lookup"><span data-stu-id="26104-264">The `ref` is, however, required for consistency with other scenarios where something is passed via strict aliasing vs. by value.</span></span>

> <span data-ttu-id="26104-265">W przeciwieństwie do przypadku parametrów `in`, `ref readonly` zwraca nigdy nie zwracaj przez kopię lokalną.</span><span class="sxs-lookup"><span data-stu-id="26104-265">Unlike the case with `in` parameters, `ref readonly` returns never return via a local copy.</span></span> <span data-ttu-id="26104-266">Biorąc pod uwagę, że kopia powinna przestać istnieć natychmiast po powrocie z tego rozwiązania, byłoby bezpunktowe i niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="26104-266">Considering that the copy would cease to exist immediately upon returning such practice would be pointless and dangerous.</span></span> <span data-ttu-id="26104-267">W związku z tym `ref readonly` zwracane są zawsze bezpośrednimi odwołaniami.</span><span class="sxs-lookup"><span data-stu-id="26104-267">Therefore `ref readonly` returns are always direct references.</span></span>

<span data-ttu-id="26104-268">Przykład:</span><span class="sxs-lookup"><span data-stu-id="26104-268">Example:</span></span>

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- <span data-ttu-id="26104-269">Argument `return ref` musi być LValue (**istniejąca reguła**)</span><span class="sxs-lookup"><span data-stu-id="26104-269">An argument of `return ref` must be an LValue (**existing rule**)</span></span>
- <span data-ttu-id="26104-270">Argument `return ref` musi być "bezpiecznie do return" (**istniejąca reguła**)</span><span class="sxs-lookup"><span data-stu-id="26104-270">An argument of `return ref` must be "safe to return" (**existing rule**)</span></span>
- <span data-ttu-id="26104-271">W elemencie członkowskim `ref readonly` argument `return ref` _nie musi być zapisywalny_ .</span><span class="sxs-lookup"><span data-stu-id="26104-271">In a `ref readonly` member an argument of `return ref` is _not required to be writeable_ .</span></span>
<span data-ttu-id="26104-272">Na przykład taki element członkowski może odesłać odwołanie do pola tylko do odczytu lub jednego z jego parametrów `in`.</span><span class="sxs-lookup"><span data-stu-id="26104-272">For example such member can ref-return a readonly field or one of its `in` parameters.</span></span>

### <a name="safe-to-return-rules"></a><span data-ttu-id="26104-273">Bezpieczne reguły powrotu.</span><span class="sxs-lookup"><span data-stu-id="26104-273">Safe to Return rules.</span></span>
<span data-ttu-id="26104-274">Normalne bezpieczne reguły powrotu dla odwołań będą również stosowane do odwołań tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="26104-274">Normal safe to return rules for references will apply to readonly references as well.</span></span>

<span data-ttu-id="26104-275">Należy zauważyć, że `ref readonly` można uzyskać od zwykłego `ref` lokalnego/parametru/Return, ale nie odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="26104-275">Note that a `ref readonly` can be obtained from a regular `ref` local/parameter/return, but not the other way around.</span></span> <span data-ttu-id="26104-276">W przeciwnym razie bezpieczeństwo zwrotów `ref readonly` jest wywnioskowane w taki sam sposób jak w przypadku zwykłych `ref` Returns.</span><span class="sxs-lookup"><span data-stu-id="26104-276">Otherwise the safety of `ref readonly` returns is inferred the same way as for regular `ref` returns.</span></span>

<span data-ttu-id="26104-277">Biorąc pod uwagę, że RValues może być przekazana jako parametr `in` i zwrócony jako `ref readonly` potrzebujemy jeszcze jednej reguły — **rvalues nie są bezpieczne do zwrócenia przez odwołanie**.</span><span class="sxs-lookup"><span data-stu-id="26104-277">Considering that RValues can be passed as `in` parameter and returned as `ref readonly` we need one more rule - **RValues are not safe-to-return by reference**.</span></span>

> <span data-ttu-id="26104-278">Rozważ sytuację, w której RValue jest przekazana do parametru `in` za pośrednictwem kopii, a następnie zwrócona z powrotem w postaci `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="26104-278">Consider the situation when an RValue is passed to an `in` parameter via a copy and then returned back in a form of a `ref readonly`.</span></span> <span data-ttu-id="26104-279">W kontekście obiektu wywołującego wynik takiego wywołania jest odwołaniem do danych lokalnych i nie jest to niebezpieczne do zwrócenia.</span><span class="sxs-lookup"><span data-stu-id="26104-279">In the context of the caller the result of such invocation is a reference to local data and as such is unsafe to return.</span></span>
> <span data-ttu-id="26104-280">Gdy RValues nie będą bezpiecznie zwracać, istniejąca reguła `#6` już obsłużyć ten przypadek.</span><span class="sxs-lookup"><span data-stu-id="26104-280">Once RValues are not safe to return, the existing rule `#6` already handles this case.</span></span>

<span data-ttu-id="26104-281">Przykład:</span><span class="sxs-lookup"><span data-stu-id="26104-281">Example:</span></span>
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

<span data-ttu-id="26104-282">Zaktualizowane reguły `safe to return`:</span><span class="sxs-lookup"><span data-stu-id="26104-282">Updated `safe to return` rules:</span></span>

1.  <span data-ttu-id="26104-283">**odwołania do zmiennych na stercie są bezpieczne do zwrócenia**</span><span class="sxs-lookup"><span data-stu-id="26104-283">**refs to variables on the heap are safe to return**</span></span>
2.  <span data-ttu-id="26104-284">**Parametry ref/in są bezpieczne do zwrócenia**
`in` parametry naturalnie można zwrócić tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="26104-284">**ref/in parameters are safe to return**
`in` parameters naturally can only be returned as readonly.</span></span>
3.  <span data-ttu-id="26104-285">**parametry out są bezpieczne do zwrócenia** (ale muszą być ostatecznie przypisane, jak już dzisiaj)</span><span class="sxs-lookup"><span data-stu-id="26104-285">**out parameters are safe to return** (but must be definitely assigned, as is already the case today)</span></span>
4.  <span data-ttu-id="26104-286">**pola struktury wystąpienia są bezpieczne do zwrócenia, o ile odbiornik jest bezpieczny do zwrócenia**</span><span class="sxs-lookup"><span data-stu-id="26104-286">**instance struct fields are safe to return as long as the receiver is safe to return**</span></span>
5.  <span data-ttu-id="26104-287">**"This" nie jest bezpieczny do powrotu z elementów członkowskich struktury**</span><span class="sxs-lookup"><span data-stu-id="26104-287">**'this' is not safe to return from struct members**</span></span>
6.  <span data-ttu-id="26104-288">**odwołanie zwrócone z innej metody jest bezpieczne do zwrócenia, jeśli wszystkie odwołania/przekroczenia przekazania do tej metody są bezpieczne do zwrócenia.** 
w *szczególności nie ma znaczenia, jeśli odbiornik jest bezpieczny do zwrócenia, niezależnie od tego, czy odbiornik jest strukturą, klasą, czy typem jako parametr typu ogólnego.*</span><span class="sxs-lookup"><span data-stu-id="26104-288">**a ref, returned from another method is safe to return if all refs/outs passed to that method as formal parameters were safe to return.**
*Specifically it is irrelevant if receiver is safe to return, regardless whether receiver is a struct, class or typed as a generic type parameter.*</span></span>
7.  <span data-ttu-id="26104-289">**Rvalues nie są bezpieczne do zwrócenia przez odwołanie.** 
 *, w odniesieniu do rvalues, są bezpieczne do przekazania jako parametry.*</span><span class="sxs-lookup"><span data-stu-id="26104-289">**RValues are not safe to return by reference.**
*Specifically RValues are safe to pass as in parameters.*</span></span>

> <span data-ttu-id="26104-290">Uwaga: Istnieją dodatkowe reguły dotyczące bezpieczeństwa funkcji Returns, które mają być odtwarzane w przypadku, gdy są używane typy odwołania i odwołania ref.</span><span class="sxs-lookup"><span data-stu-id="26104-290">NOTE: There are additional rules regarding safety of returns that come into play when ref-like types and ref-reassignments are involved.</span></span>
> <span data-ttu-id="26104-291">Reguły są równie stosowane do `ref` i `ref readonly` członków, dlatego nie są tu wymienione.</span><span class="sxs-lookup"><span data-stu-id="26104-291">The rules equally apply to `ref` and `ref readonly` members and therefore are not mentioned here.</span></span>

### <a name="aliasing-behavior"></a><span data-ttu-id="26104-292">Zachowanie aliasu.</span><span class="sxs-lookup"><span data-stu-id="26104-292">Aliasing behavior.</span></span>
<span data-ttu-id="26104-293">`ref readonly` Członkowie zapewniają takie samo zachowanie aliasu jak zwykłe `ref` elementy członkowskie (z wyjątkiem do odczytu).</span><span class="sxs-lookup"><span data-stu-id="26104-293">`ref readonly` members provide the same aliasing behavior as ordinary `ref` members (except for being readonly).</span></span>
<span data-ttu-id="26104-294">W związku z tym na potrzeby przechwytywania w wyrażeniach lambda, Async, Iteratory, rozlewania stosu itp... te same ograniczenia mają zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="26104-294">Therefore for the purpose of capturing in lambdas, async, iterators, stack spilling etc... the same restrictions apply.</span></span> <span data-ttu-id="26104-295">co.</span><span class="sxs-lookup"><span data-stu-id="26104-295">- I.E.</span></span> <span data-ttu-id="26104-296">ze względu na niezdolność do przechwytywania rzeczywistych odwołań i ze względu na charakter częściowej oceny członków takie scenariusze są niedozwolone.</span><span class="sxs-lookup"><span data-stu-id="26104-296">due to inability to capture the actual references and due to side-effecting nature of member evaluation such scenarios are disallowed.</span></span>

> <span data-ttu-id="26104-297">Jest to dozwolone i wymagane do tworzenia kopii, gdy `ref readonly` Return jest odbiornikiem zwykłych metod struktury, które przyjmują `this` jako zwykłe odwołanie do zapisu.</span><span class="sxs-lookup"><span data-stu-id="26104-297">It is permitted and required to make a copy when `ref readonly` return is a receiver of regular struct methods, which take `this` as an ordinary writeable reference.</span></span> <span data-ttu-id="26104-298">Historycznie we wszystkich przypadkach, gdy takie wywołania są stosowane do zmiennej ReadOnly, tworzony jest kopia lokalna.</span><span class="sxs-lookup"><span data-stu-id="26104-298">Historically in all cases where such invocations are applied to readonly variable a local copy is made.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="26104-299">Reprezentacja metadanych.</span><span class="sxs-lookup"><span data-stu-id="26104-299">Metadata representation.</span></span>
<span data-ttu-id="26104-300">Gdy `System.Runtime.CompilerServices.IsReadOnlyAttribute` jest stosowany do powrotu metody zwracanej przez ByRef, oznacza to, że metoda zwraca odwołanie tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="26104-300">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to the return of a byref returning method, it means that the method returns a readonly reference.</span></span>

<span data-ttu-id="26104-301">Ponadto sygnatury wynikowe takich metod (i tylko te metody) muszą mieć `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span><span class="sxs-lookup"><span data-stu-id="26104-301">In addition, the result signature of such methods (and only those methods) must have `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span></span>

<span data-ttu-id="26104-302">**Motywacja**: to zapewnia, że istniejące kompilatory nie mogą po prostu ignorować `readonly` podczas wywoływania metod przy użyciu `ref readonly` zwraca</span><span class="sxs-lookup"><span data-stu-id="26104-302">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when invoking methods with `ref readonly` returns</span></span>

## <a name="readonly-structs"></a><span data-ttu-id="26104-303">Struktury tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="26104-303">Readonly structs</span></span>
<span data-ttu-id="26104-304">W skrócie — funkcja, która wprowadza `this` parametr wszystkich elementów członkowskich wystąpienia struktury, z wyjątkiem konstruktorów, `in` parametru.</span><span class="sxs-lookup"><span data-stu-id="26104-304">In short - a feature that makes `this` parameter of all instance members of a struct, except for constructors, an `in` parameter.</span></span>

### <a name="motivation"></a><span data-ttu-id="26104-305">Motywacją</span><span class="sxs-lookup"><span data-stu-id="26104-305">Motivation</span></span>
<span data-ttu-id="26104-306">Kompilator musi założyć, że każde wywołanie metody w wystąpieniu struktury może zmodyfikować wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="26104-306">Compiler must assume that any method call on a struct instance may modify the instance.</span></span> <span data-ttu-id="26104-307">W rzeczywistości odwołanie do zapisu jest przesyłane do metody jako parametr `this` i w pełni włącza takie zachowanie.</span><span class="sxs-lookup"><span data-stu-id="26104-307">Indeed a writeable reference is passed to the method as `this` parameter and fully enables this behavior.</span></span> <span data-ttu-id="26104-308">Aby zezwolić na takie wywołania na zmiennych `readonly`, wywołania są stosowane do kopii tymczasowych.</span><span class="sxs-lookup"><span data-stu-id="26104-308">To allow such invocations on `readonly` variables, the invocations are applied to temp copies.</span></span> <span data-ttu-id="26104-309">Mogą one być nieintuicyjne i czasami zmuszają osoby do porzucenia `readonly` ze względów wydajnościowych.</span><span class="sxs-lookup"><span data-stu-id="26104-309">That could be unintuitive and sometimes forces people to abandon `readonly` for performance reasons.</span></span>  
<span data-ttu-id="26104-310">Przykład: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span><span class="sxs-lookup"><span data-stu-id="26104-310">Example: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span></span>

<span data-ttu-id="26104-311">Po dodaniu obsługi parametrów `in` i `ref readonly` zwraca problem z kopiowaniem z obroną będzie gorszy, ponieważ zmienne tylko do odczytu staną się bardziej popularne.</span><span class="sxs-lookup"><span data-stu-id="26104-311">After adding support for `in` parameters and `ref readonly` returns the problem of defensive copying will get worse since readonly variables will become more common.</span></span>

### <a name="solution"></a><span data-ttu-id="26104-312">Rozwiązanie</span><span class="sxs-lookup"><span data-stu-id="26104-312">Solution</span></span>
<span data-ttu-id="26104-313">Zezwalaj na modyfikator `readonly` w deklaracjach struktury, co spowoduje, że `this` być traktowany jako parametr `in` dla wszystkich metod wystąpienia struktury z wyjątkiem konstruktorów.</span><span class="sxs-lookup"><span data-stu-id="26104-313">Allow `readonly` modifier on struct declarations which would result in `this` being treated as `in` parameter on all struct instance methods except for constructors.</span></span>

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a><span data-ttu-id="26104-314">Ograniczenia dotyczące elementów członkowskich struktury tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="26104-314">Restrictions on members of readonly struct</span></span>
- <span data-ttu-id="26104-315">Pola wystąpienia struktury tylko do odczytu muszą być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="26104-315">Instance fields of a readonly struct must be readonly.</span></span>  
<span data-ttu-id="26104-316">**Motywacja:** można pisać tylko do zewnątrz, ale nie do elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="26104-316">**Motivation:** can only be written to externally, but not through members.</span></span>
- <span data-ttu-id="26104-317">Autowłaściwości wystąpienia struktury tylko do odczytu muszą być tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="26104-317">Instance autoproperties of a readonly struct must be get-only.</span></span>  
<span data-ttu-id="26104-318">**Motywacja:** konsekwencja ograniczenia dotyczącego pól wystąpień.</span><span class="sxs-lookup"><span data-stu-id="26104-318">**Motivation:** consequence of restriction on instance fields.</span></span>
- <span data-ttu-id="26104-319">Struktura ReadOnly nie może deklarować zdarzeń przypominających pola.</span><span class="sxs-lookup"><span data-stu-id="26104-319">Readonly struct may not declare field-like events.</span></span>  
<span data-ttu-id="26104-320">**Motywacja:** konsekwencja ograniczenia dotyczącego pól wystąpień.</span><span class="sxs-lookup"><span data-stu-id="26104-320">**Motivation:** consequence of restriction on instance fields.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="26104-321">Reprezentacja metadanych.</span><span class="sxs-lookup"><span data-stu-id="26104-321">Metadata representation.</span></span>
<span data-ttu-id="26104-322">Gdy `System.Runtime.CompilerServices.IsReadOnlyAttribute` jest stosowana do typu wartości, oznacza to, że typ jest `readonly struct`.</span><span class="sxs-lookup"><span data-stu-id="26104-322">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a value type, it means that the type is a `readonly struct`.</span></span>

<span data-ttu-id="26104-323">W szczególności:</span><span class="sxs-lookup"><span data-stu-id="26104-323">In particular:</span></span>
-  <span data-ttu-id="26104-324">Tożsamość typu `IsReadOnlyAttribute` jest nieważna.</span><span class="sxs-lookup"><span data-stu-id="26104-324">The identity of the `IsReadOnlyAttribute` type is unimportant.</span></span> <span data-ttu-id="26104-325">W rzeczywistości może być osadzony przez kompilator w zawierającym go zestawie, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="26104-325">In fact it can be embedded by the compiler in the containing assembly if needed.</span></span>

## <a name="refin-extension-methods"></a><span data-ttu-id="26104-326">`ref`/`in` metody rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="26104-326">`ref`/`in` extension methods</span></span>
<span data-ttu-id="26104-327">Istnieje w rzeczywistości istniejąca propozycja (https://github.com/dotnet/roslyn/issues/165) i odpowiedni prototyp żądania ściągnięcia (https://github.com/dotnet/roslyn/pull/15650).</span><span class="sxs-lookup"><span data-stu-id="26104-327">There is actually an existing proposal (https://github.com/dotnet/roslyn/issues/165) and corresponding prototype PR (https://github.com/dotnet/roslyn/pull/15650).</span></span>
<span data-ttu-id="26104-328">Chcę tylko potwierdzić, że ten pomysł nie jest zupełnie nowy.</span><span class="sxs-lookup"><span data-stu-id="26104-328">I just want to acknowledge that this idea is not entirely new.</span></span> <span data-ttu-id="26104-329">Jest to jednak istotne tutaj, ponieważ `ref readonly` eleganckie usuwa najbardziej contentious problem dotyczący takich metod — co należy zrobić z odbiornikami RValue.</span><span class="sxs-lookup"><span data-stu-id="26104-329">It is, however, relevant here since `ref readonly` elegantly removes the most contentious issue about such methods - what to do with RValue receivers.</span></span>

<span data-ttu-id="26104-330">Ogólnym pomysłem jest umożliwienie metodom rozszerzania `this` parametru przez odwołanie, o ile typ jest znany jako typ struktury.</span><span class="sxs-lookup"><span data-stu-id="26104-330">The general idea is allowing extension methods to take the `this` parameter by reference, as long as the type is known to be a struct type.</span></span>

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

<span data-ttu-id="26104-331">Przyczyny zapisu takich metod rozszerzających są przede wszystkim następujące:</span><span class="sxs-lookup"><span data-stu-id="26104-331">The reasons for writing such extension methods are primarily:</span></span>  
1.  <span data-ttu-id="26104-332">Unikaj kopiowania, gdy odbiornik jest dużą strukturą</span><span class="sxs-lookup"><span data-stu-id="26104-332">Avoid copying when receiver is a large struct</span></span>
2.  <span data-ttu-id="26104-333">Zezwalaj na mutację metod rozszerzających w strukturach</span><span class="sxs-lookup"><span data-stu-id="26104-333">Allow mutating extension methods on structs</span></span>

<span data-ttu-id="26104-334">Powody, dla których nie chcemy zezwalać na takie klasy</span><span class="sxs-lookup"><span data-stu-id="26104-334">The reasons why we do not want to allow this on classes</span></span>  
1.  <span data-ttu-id="26104-335">Jest to bardzo ograniczony cel.</span><span class="sxs-lookup"><span data-stu-id="26104-335">It would be of very limited purpose.</span></span>
2.  <span data-ttu-id="26104-336">Spowoduje to przerwanie długotrwałej niezmiennej, którą wywołanie metody nie może wyłączyć odbiornika`null`, aby stał się `null` po wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="26104-336">It would break long standing invariant that a method call cannot turn non-`null` receiver to become `null` after invocation.</span></span>
> <span data-ttu-id="26104-337">Obecnie zmienna inna niż`null` nie może stać się `null`, chyba że zostanie _jawnie_ przypisana lub przeniesiona przez `ref` lub `out`.</span><span class="sxs-lookup"><span data-stu-id="26104-337">In fact, currently a non-`null` variable cannot become `null` unless _explicitly_ assigned or passed by `ref` or `out`.</span></span>
> <span data-ttu-id="26104-338">Znacznie ułatwia to czytelność lub inną postać analizy "może to być wartość zerowa.</span><span class="sxs-lookup"><span data-stu-id="26104-338">That greatly aids readability or other forms of "can this be a null here" analysis.</span></span>
3.  <span data-ttu-id="26104-339">Jest trudne do uzgodnienia z semantyką "Szacuj raz" z dostępem warunkowym o wartości null.</span><span class="sxs-lookup"><span data-stu-id="26104-339">It would be hard to reconcile with "evaluate once" semantics of null-conditional accesses.</span></span>
<span data-ttu-id="26104-340">Przykład: `obj.stringField?.RefExtension(...)`-musi przechwycić kopię `stringField`, aby sprawdzenie wartości null było zrozumiałe, ale przypisania do `this` wewnątrz RefExtension nie zostaną odzwierciedlone z powrotem do pola.</span><span class="sxs-lookup"><span data-stu-id="26104-340">Example: `obj.stringField?.RefExtension(...)` - need to capture a copy of `stringField` to make the null check meaningful, but then assignments to `this` inside RefExtension would not be reflected back to the field.</span></span>

<span data-ttu-id="26104-341">Możliwość deklarowania metod rozszerzenia w **strukturach** przyjmujących pierwszy argument przez odwołanie było długotrwałym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="26104-341">An ability to declare extension methods on **structs** that take the first argument by reference was a long-standing request.</span></span> <span data-ttu-id="26104-342">Jedną z rozważań dotyczących blokowania była "co się stanie, jeśli odbiornik nie jest LValue?".</span><span class="sxs-lookup"><span data-stu-id="26104-342">One of the blocking consideration was "what happens if receiver is not an LValue?".</span></span>

- <span data-ttu-id="26104-343">Istnieje poprzednik, że jakakolwiek metoda rozszerzająca może być również wywoływana jako metoda statyczna (czasami jest to jedyny sposób na rozwiązanie niejednoznaczności).</span><span class="sxs-lookup"><span data-stu-id="26104-343">There is a precedent that any extension method could also be called as a static method (sometimes it is the only way to resolve ambiguity).</span></span> <span data-ttu-id="26104-344">Może to spowodować, że odbiorniki RValue powinny być niedozwolone.</span><span class="sxs-lookup"><span data-stu-id="26104-344">It would dictate that RValue receivers should be disallowed.</span></span>
- <span data-ttu-id="26104-345">Z drugiej strony jest stosowana procedura tworzenia wywołań na kopii w podobnych sytuacjach, gdy są wykorzystywane metody wystąpień struktury.</span><span class="sxs-lookup"><span data-stu-id="26104-345">On the other hand there is a practice of making invocation on a copy in similar situations when struct instance methods are involved.</span></span>

<span data-ttu-id="26104-346">Przyczyną niejawnego kopiowania jest fakt, że większość metod struktury nie modyfikuje struktury, chociaż nie jest w stanie wskazywać, że.</span><span class="sxs-lookup"><span data-stu-id="26104-346">The reason why the "implicit copying" exists is because the majority of struct methods do not actually modify the struct while not being able to indicate that.</span></span> <span data-ttu-id="26104-347">Z tego względu najbardziej praktyczne rozwiązanie miało po prostu nawiązać wywołanie w kopii, ale ta metoda jest znana w celu uszkodzenia wydajności i spowodowania błędów.</span><span class="sxs-lookup"><span data-stu-id="26104-347">Therefore the most practical solution was to just make the invocation on a copy, but this practice is known for harming performance and causing bugs.</span></span>

<span data-ttu-id="26104-348">Teraz, mając dostępność parametrów `in`, istnieje możliwość, aby rozszerzenie zasygnalizować zamiar.</span><span class="sxs-lookup"><span data-stu-id="26104-348">Now, with availability of `in` parameters, it is possible for an extension to signal the intent.</span></span> <span data-ttu-id="26104-349">W związku z tym Conundrum można rozwiązać przez wymaganie, aby rozszerzenia `ref` miały być wywoływane przy użyciu odbiorców zapisywalnych, podczas gdy rozszerzenia `in` umożliwiają niejawne kopiowanie, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="26104-349">Therefore the conundrum can be resolved by requiring `ref` extensions to be called with writeable receivers while `in` extensions permit implicit copying if necessary.</span></span>

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a><span data-ttu-id="26104-350">`in` rozszerzenia i typy ogólne.</span><span class="sxs-lookup"><span data-stu-id="26104-350">`in` extensions and generics.</span></span>
<span data-ttu-id="26104-351">Celem metod rozszerzenia `ref` jest mutacja odbiorcy bezpośrednio lub poprzez wywoływanie mutacji członków.</span><span class="sxs-lookup"><span data-stu-id="26104-351">The purpose of `ref` extension methods is to mutate the receiver directly or by invoking mutating members.</span></span> <span data-ttu-id="26104-352">W związku z tym `ref this T` rozszerzenia są dozwolone, o ile `T` jest ograniczone do struktury.</span><span class="sxs-lookup"><span data-stu-id="26104-352">Therefore `ref this T` extensions are allowed as long as `T` is constrained to be a struct.</span></span>

<span data-ttu-id="26104-353">Z drugiej strony metody rozszerzenia `in` istnieją przede wszystkim do zredukowania niejawnego kopiowania.</span><span class="sxs-lookup"><span data-stu-id="26104-353">On the other hand `in` extension methods exist specifically to reduce implicit copying.</span></span> <span data-ttu-id="26104-354">Jednak każde użycie parametru `in T` będzie musiało zostać wykonane za pomocą elementu członkowskiego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="26104-354">However any use of an `in T` parameter will have to be done through an interface member.</span></span> <span data-ttu-id="26104-355">Ze względu na to, że wszystkie elementy członkowskie interfejsu są traktowane jako mutacje, wszelkie takie zastosowania wymagały kopiowania.</span><span class="sxs-lookup"><span data-stu-id="26104-355">Since all interface members are considered mutating, any such use would require a copy.</span></span> <span data-ttu-id="26104-356">— Zamiast zmniejszać kopiowanie, efekt byłby przeciwieństwem.</span><span class="sxs-lookup"><span data-stu-id="26104-356">- Instead of reducing copying, the effect would be the opposite.</span></span> <span data-ttu-id="26104-357">Dlatego `in this T` nie jest dozwolone, gdy `T` jest parametrem typu ogólnego, niezależnie od ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="26104-357">Therefore `in this T` is not allowed when `T` is a generic type parameter regardless of constraints.</span></span>

### <a name="valid-kinds-of-extension-methods-recap"></a><span data-ttu-id="26104-358">Prawidłowe rodzaje metod rozszerzających (podsumowanie):</span><span class="sxs-lookup"><span data-stu-id="26104-358">Valid kinds of extension methods (recap):</span></span>
<span data-ttu-id="26104-359">Następujące formy deklaracji `this` w metodzie rozszerzenia są teraz dozwolone:</span><span class="sxs-lookup"><span data-stu-id="26104-359">The following forms of `this` declaration in an extension method are now allowed:</span></span>
1) <span data-ttu-id="26104-360">`this T arg`-regularne rozszerzenie ByVal.</span><span class="sxs-lookup"><span data-stu-id="26104-360">`this T arg` - regular byval extension.</span></span> <span data-ttu-id="26104-361">(**istniejąca sprawa**)</span><span class="sxs-lookup"><span data-stu-id="26104-361">(**existing case**)</span></span>
- <span data-ttu-id="26104-362">T może być dowolnego typu, w tym typów referencyjnych lub parametrów typu.</span><span class="sxs-lookup"><span data-stu-id="26104-362">T can be any type, including reference types or type parameters.</span></span>
<span data-ttu-id="26104-363">Wystąpienie będzie taka sama jak zmienna po wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="26104-363">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="26104-364">Zezwala na niejawne konwersje rodzaju _konwersji tego argumentu_ .</span><span class="sxs-lookup"><span data-stu-id="26104-364">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="26104-365">Może być wywoływana w RValues.</span><span class="sxs-lookup"><span data-stu-id="26104-365">Can be called on RValues.</span></span>

- <span data-ttu-id="26104-366">`in this T self` - `in` rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="26104-366">`in this T self` - `in` extension.</span></span>
<span data-ttu-id="26104-367">T musi być rzeczywistym typem struktury.</span><span class="sxs-lookup"><span data-stu-id="26104-367">T must be an actual struct type.</span></span>
<span data-ttu-id="26104-368">Wystąpienie będzie taka sama jak zmienna po wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="26104-368">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="26104-369">Zezwala na niejawne konwersje rodzaju _konwersji tego argumentu_ .</span><span class="sxs-lookup"><span data-stu-id="26104-369">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="26104-370">Może być wywoływana w RValues (w razie konieczności może być wywoływana w tempie).</span><span class="sxs-lookup"><span data-stu-id="26104-370">Can be called on RValues (may be invoked on a temp if needed).</span></span>

- <span data-ttu-id="26104-371">`ref this T self` - `ref` rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="26104-371">`ref this T self` - `ref` extension.</span></span>
<span data-ttu-id="26104-372">T musi być typem struktury lub parametrem typu ogólnego ograniczonym jako strukturą.</span><span class="sxs-lookup"><span data-stu-id="26104-372">T must be a struct type or a generic type parameter constrained to be a struct.</span></span>
<span data-ttu-id="26104-373">Wystąpienie może być zapisywane przez wywołanie.</span><span class="sxs-lookup"><span data-stu-id="26104-373">Instance may be written to by the invocation.</span></span>
<span data-ttu-id="26104-374">Zezwala tylko na konwersje tożsamości.</span><span class="sxs-lookup"><span data-stu-id="26104-374">Allows only identity conversions.</span></span>
<span data-ttu-id="26104-375">Musi być wywoływana na zapisywalnym LValue.</span><span class="sxs-lookup"><span data-stu-id="26104-375">Must be called on writeable LValue.</span></span> <span data-ttu-id="26104-376">(nigdy nie wywołane za pośrednictwem temp).</span><span class="sxs-lookup"><span data-stu-id="26104-376">(never invoked via a temp).</span></span>

## <a name="readonly-ref-locals"></a><span data-ttu-id="26104-377">Lokalne odwołania tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="26104-377">Readonly ref locals.</span></span>

### <a name="motivation"></a><span data-ttu-id="26104-378">Motywacją.</span><span class="sxs-lookup"><span data-stu-id="26104-378">Motivation.</span></span>
<span data-ttu-id="26104-379">Po wprowadzeniu elementów członkowskich `ref readonly` były jasne przed użyciem, że muszą zostać sparowane z odpowiednim rodzajem lokalnego.</span><span class="sxs-lookup"><span data-stu-id="26104-379">Once `ref readonly` members were introduced, it was clear from the use that they need to be paired with appropriate kind of local.</span></span> <span data-ttu-id="26104-380">Obliczanie elementu członkowskiego może dawać lub obserwować efekty uboczne, dlatego jeśli wynik musi być używany więcej niż jeden raz, musi on być przechowywany.</span><span class="sxs-lookup"><span data-stu-id="26104-380">Evaluation of a member may produce or observe side effects, therefore if the result must be used more than once, it needs to be stored.</span></span> <span data-ttu-id="26104-381">Zwykłe `ref` lokalne nie pomagają w tym miejscu, ponieważ nie mogą być przypisane do `readonly` odwołania.</span><span class="sxs-lookup"><span data-stu-id="26104-381">Ordinary `ref` locals do not help here since they cannot be assigned a `readonly` reference.</span></span>   

### <a name="solution"></a><span data-ttu-id="26104-382">Narzędzie.</span><span class="sxs-lookup"><span data-stu-id="26104-382">Solution.</span></span>
<span data-ttu-id="26104-383">Zezwalaj na deklarowanie `ref readonly` lokalnych.</span><span class="sxs-lookup"><span data-stu-id="26104-383">Allow declaring `ref readonly` locals.</span></span> <span data-ttu-id="26104-384">Jest to nowy rodzaj `ref` lokalnych, które nie są zapisywalne.</span><span class="sxs-lookup"><span data-stu-id="26104-384">This is a new kind of `ref` locals that is not writeable.</span></span> <span data-ttu-id="26104-385">W wyniku `ref readonly` elementy lokalne mogą akceptować odwołania do zmiennych tylko do odczytu bez uwidaczniania tych zmiennych do zapisu.</span><span class="sxs-lookup"><span data-stu-id="26104-385">As a result `ref readonly` locals can accept references to readonly variables without exposing these variables to writes.</span></span>

### <a name="declaring-and-using-ref-readonly-locals"></a><span data-ttu-id="26104-386">Deklarowanie i używanie `ref readonly` lokalnych.</span><span class="sxs-lookup"><span data-stu-id="26104-386">Declaring and using `ref readonly` locals.</span></span>

<span data-ttu-id="26104-387">Składnia takich elementów lokalnych używa modyfikatorów `ref readonly` w witrynie deklaracji (w tej konkretnej kolejności).</span><span class="sxs-lookup"><span data-stu-id="26104-387">The syntax of such locals uses `ref readonly` modifiers at declaration site (in that specific order).</span></span> <span data-ttu-id="26104-388">Podobnie jak w przypadku zwykłych `ref` lokalnych, `ref readonly` lokalnych musi być inicjowane ref w deklaracji.</span><span class="sxs-lookup"><span data-stu-id="26104-388">Similarly to ordinary `ref` locals, `ref readonly` locals must be ref-initialized at declaration.</span></span> <span data-ttu-id="26104-389">W przeciwieństwie do zwykłych `ref` lokalnych, `ref readonly` lokalnych może odwoływać się do `readonly` LValues, takich jak parametry `in`, `readonly` pól, `ref readonly` metod.</span><span class="sxs-lookup"><span data-stu-id="26104-389">Unlike regular `ref` locals, `ref readonly` locals can refer to `readonly` LValues like `in` parameters, `readonly` fields, `ref readonly` methods.</span></span>

<span data-ttu-id="26104-390">We wszystkich celach `ref readonly` lokalny jest traktowany jako zmienna `readonly`a.</span><span class="sxs-lookup"><span data-stu-id="26104-390">For all purposes a `ref readonly` local is treated as a `readonly` variable.</span></span> <span data-ttu-id="26104-391">Większość ograniczeń dotyczących użycia jest taka sama jak w przypadku `readonly` pól lub `in` parametrów.</span><span class="sxs-lookup"><span data-stu-id="26104-391">Most of the restrictions on the use are the same as with `readonly` fields or `in` parameters.</span></span>

<span data-ttu-id="26104-392">Na przykład pola `in` parametru, który ma typ struktury, są rekursywnie klasyfikowane jako zmienne `readonly`.</span><span class="sxs-lookup"><span data-stu-id="26104-392">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a><span data-ttu-id="26104-393">Ograniczenia dotyczące stosowania `ref readonly` lokalnych</span><span class="sxs-lookup"><span data-stu-id="26104-393">Restrictions on use of `ref readonly` locals</span></span>
<span data-ttu-id="26104-394">Z wyjątkiem `readonly`, `ref readonly` lokalnych zachowuje się jak zwykłe `ref` lokalne i podlegają dokładnie tym samym ograniczeniom.</span><span class="sxs-lookup"><span data-stu-id="26104-394">Except for their `readonly` nature, `ref readonly` locals behave like ordinary `ref` locals and are subject to exactly same restrictions.</span></span>  
<span data-ttu-id="26104-395">Na przykład ograniczenia związane z przechwytywaniem w zamknięciu, deklarowanie w metodach `async` lub analiza `safe-to-return` równo dotyczy `ref readonly` lokalnych.</span><span class="sxs-lookup"><span data-stu-id="26104-395">For example restrictions related to capturing in closures, declaring in `async` methods or the `safe-to-return` analysis equally applies to `ref readonly` locals.</span></span>

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a><span data-ttu-id="26104-396">Wyrażenia `ref` Trzyelementowy.</span><span class="sxs-lookup"><span data-stu-id="26104-396">Ternary `ref` expressions.</span></span> <span data-ttu-id="26104-397">(alias "warunkowe LValues")</span><span class="sxs-lookup"><span data-stu-id="26104-397">(aka "Conditional LValues")</span></span>

### <a name="motivation"></a><span data-ttu-id="26104-398">Motywacją</span><span class="sxs-lookup"><span data-stu-id="26104-398">Motivation</span></span>
<span data-ttu-id="26104-399">Korzystanie z `ref` i `ref readonly` lokalnych narażonych na konieczność inicjalizacji takich elementów lokalnych przy użyciu jednej lub innej zmiennej docelowej na podstawie warunku.</span><span class="sxs-lookup"><span data-stu-id="26104-399">Use of `ref` and `ref readonly` locals exposed a need to ref-initialize such locals with one or another target variable based on a condition.</span></span>

<span data-ttu-id="26104-400">Typowym obejściem jest wprowadzenie metody podobnej do:</span><span class="sxs-lookup"><span data-stu-id="26104-400">A typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="26104-401">Należy zauważyć, że `Choice` nie jest dokładnym zastąpieniem Trzyelementowy, ponieważ _wszystkie_ argumenty muszą być oceniane w miejscu wywołania, co było wiodące w nieintuicyjnym zachowaniu i błędach.</span><span class="sxs-lookup"><span data-stu-id="26104-401">Note that `Choice` is not an exact replacement of a ternary since _all_ arguments must be evaluated at the call site, which was leading to unintuitive behavior and bugs.</span></span>

<span data-ttu-id="26104-402">Następujące elementy nie będą działały zgodnie z oczekiwaniami:</span><span class="sxs-lookup"><span data-stu-id="26104-402">The following will not work as expected:</span></span>

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a><span data-ttu-id="26104-403">Rozwiązanie</span><span class="sxs-lookup"><span data-stu-id="26104-403">Solution</span></span>
<span data-ttu-id="26104-404">Zezwalaj na specjalne wyrażenie warunkowe, którego wynikiem jest odwołanie do jednego z argumentów LValue na podstawie warunku.</span><span class="sxs-lookup"><span data-stu-id="26104-404">Allow special kind of conditional expression that evaluates to a reference to one of LValue argument based on a condition.</span></span>

### <a name="using-ref-ternary-expression"></a><span data-ttu-id="26104-405">Używanie wyrażenia `ref` Trzyelementowy.</span><span class="sxs-lookup"><span data-stu-id="26104-405">Using `ref` ternary expression.</span></span>

<span data-ttu-id="26104-406">Składnia `ref` wersji wyrażenia warunkowego jest `<condition> ? ref <consequence> : ref <alternative>;`</span><span class="sxs-lookup"><span data-stu-id="26104-406">The syntax for the `ref` flavor of a conditional expression is `<condition> ? ref <consequence> : ref <alternative>;`</span></span>

<span data-ttu-id="26104-407">Podobnie jak w przypadku zwykłego wyrażenia warunkowego tylko `<consequence>` lub `<alternative>` jest obliczana w zależności od wyniku wyrażenia warunku logicznego.</span><span class="sxs-lookup"><span data-stu-id="26104-407">Just like with the ordinary conditional expression only `<consequence>` or `<alternative>` is evaluated depending on result of the boolean condition expression.</span></span>

<span data-ttu-id="26104-408">W przeciwieństwie do zwykłego wyrażenia warunkowego, `ref` wyrażenie warunkowe:</span><span class="sxs-lookup"><span data-stu-id="26104-408">Unlike ordinary conditional expression, `ref` conditional expression:</span></span>
- <span data-ttu-id="26104-409">wymaga, aby `<consequence>` i `<alternative>` były LValues.</span><span class="sxs-lookup"><span data-stu-id="26104-409">requires that `<consequence>` and `<alternative>` are LValues.</span></span>
- <span data-ttu-id="26104-410">`ref` wyrażeniu warunkowym jest LValue i</span><span class="sxs-lookup"><span data-stu-id="26104-410">`ref` conditional expression itself is an LValue and</span></span>
- <span data-ttu-id="26104-411">`ref` wyrażenie warunkowe jest zapisywalne, jeśli oba `<consequence>` i `<alternative>` są zapisywalne LValues</span><span class="sxs-lookup"><span data-stu-id="26104-411">`ref` conditional expression is writeable if both `<consequence>` and `<alternative>` are writeable LValues</span></span>

<span data-ttu-id="26104-412">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="26104-412">Examples:</span></span>  
<span data-ttu-id="26104-413">`ref` Trzyelementowy jest LValue i ponieważ może być przekazane/przypisane/zwrócone przez odwołanie;</span><span class="sxs-lookup"><span data-stu-id="26104-413">`ref` ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="26104-414">Jest to LValue, ale może być również przypisany do.</span><span class="sxs-lookup"><span data-stu-id="26104-414">Being an LValue, it can also be assigned to.</span></span>
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

<span data-ttu-id="26104-415">Może służyć jako odbiornik wywołania metody i w razie potrzeby pomijać kopiowanie.</span><span class="sxs-lookup"><span data-stu-id="26104-415">Can be used as a receiver of a method call and skip copying if necessary.</span></span>
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

<span data-ttu-id="26104-416">`ref` Trzyelementowy może być używana w zwykłym kontekście (nie ref).</span><span class="sxs-lookup"><span data-stu-id="26104-416">`ref` ternary can be used in a regular (not ref) context as well.</span></span>
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a><span data-ttu-id="26104-417">Wady</span><span class="sxs-lookup"><span data-stu-id="26104-417">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="26104-418">Widzę dwa główne argumenty dla rozszerzonej obsługi odwołań i odwołań tylko do odczytu:</span><span class="sxs-lookup"><span data-stu-id="26104-418">I can see two major arguments against enhanced support for references and readonly references:</span></span>

1) <span data-ttu-id="26104-419">Problemy, które są rozwiązywane w tym miejscu, są bardzo stare.</span><span class="sxs-lookup"><span data-stu-id="26104-419">The problems that are solved here are very old.</span></span> <span data-ttu-id="26104-420">Dlaczego nagle rozwiązać ten problem, szczególnie ponieważ nie może on pomóc w istniejącym kodzie?</span><span class="sxs-lookup"><span data-stu-id="26104-420">Why suddenly solve them now, especially since it would not help existing code?</span></span>

<span data-ttu-id="26104-421">Po znalezieniu C# i użyciu platformy .NET w nowych domenach niektóre problemy stają się coraz bardziej widoczne.</span><span class="sxs-lookup"><span data-stu-id="26104-421">As we find C# and .Net used in new domains, some problems become more prominent.</span></span>  
<span data-ttu-id="26104-422">Jako przykłady środowisk, które są bardziej krytyczne niż średnia o nadmiarach obliczeń, można wyświetlić listę</span><span class="sxs-lookup"><span data-stu-id="26104-422">As examples of environments that are more critical than average about computation overheads, I can list</span></span>

* <span data-ttu-id="26104-423">scenariusze chmur/centrów danych, w których obliczanie jest rozliczane i czas odpowiedzi to przewagę konkurencyjną.</span><span class="sxs-lookup"><span data-stu-id="26104-423">cloud/datacenter scenarios where computation is billed for and responsiveness is a competitive advantage.</span></span>
* <span data-ttu-id="26104-424">Gry/VR/AR z wymaganiami dotyczącymi opóźnień w czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="26104-424">Games/VR/AR with soft-realtime requirements on latencies</span></span>     

<span data-ttu-id="26104-425">Ta funkcja nie ogranicza żadnej istniejącej siły, takiej jak bezpieczeństwo typu, a jednocześnie pozwala na obniżenie niższych kosztów w niektórych typowych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="26104-425">This feature does not sacrifice any of the existing strengths such as type-safety, while allowing to lower overheads in some common scenarios.</span></span>

2) <span data-ttu-id="26104-426">Czy można zagwarantować, że obiekt wywoływany będzie odtwarzany przez reguły, gdy zdecyduje się na `readonly` umów?</span><span class="sxs-lookup"><span data-stu-id="26104-426">Can we reasonably guarantee that the callee will play by the rules when it opts into `readonly` contracts?</span></span>

<span data-ttu-id="26104-427">W przypadku korzystania z `out`są podobne zaufania.</span><span class="sxs-lookup"><span data-stu-id="26104-427">We have similar trust when using `out`.</span></span> <span data-ttu-id="26104-428">Niepoprawna implementacja `out` może spowodować nieokreślone zachowanie, ale w rzeczywistości zdarza się to rzadko.</span><span class="sxs-lookup"><span data-stu-id="26104-428">Incorrect implementation of `out` can cause unspecified behavior, but in reality it rarely happens.</span></span>  

<span data-ttu-id="26104-429">Zastosowanie formalnych reguł weryfikacji znanych z `ref readonly` mogłoby jeszcze bardziej złagodzić problem z zaufaniem.</span><span class="sxs-lookup"><span data-stu-id="26104-429">Making the formal verification rules familiar with `ref readonly` would further mitigate the trust issue.</span></span>

### <a name="alternatives"></a><span data-ttu-id="26104-430">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="26104-430">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="26104-431">Główny konkurencyjny projekt to naprawdę "nic nie rób".</span><span class="sxs-lookup"><span data-stu-id="26104-431">The main competing design is really "do nothing".</span></span>

### <a name="unresolved-questions"></a><span data-ttu-id="26104-432">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="26104-432">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a><span data-ttu-id="26104-433">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="26104-433">Design meetings</span></span>

<span data-ttu-id="26104-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span><span class="sxs-lookup"><span data-stu-id="26104-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span></span>
