---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484553"
---
# <a name="simplified-null-argument-checking"></a><span data-ttu-id="b55c1-101">Uproszczone sprawdzanie argumentów o wartości null</span><span class="sxs-lookup"><span data-stu-id="b55c1-101">Simplified Null Argument Checking</span></span>

## <a name="summary"></a><span data-ttu-id="b55c1-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="b55c1-102">Summary</span></span>
<span data-ttu-id="b55c1-103">Ta propozycja zapewnia uproszczoną składnię do sprawdzania poprawności argumentów metody nie są `null` i wyrzucają `ArgumentNullException` odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="b55c1-103">This proposal provides a simplified syntax for validating method arguments are not `null` and throwing `ArgumentNullException` appropriately.</span></span>

## <a name="motivation"></a><span data-ttu-id="b55c1-104">Motywacją</span><span class="sxs-lookup"><span data-stu-id="b55c1-104">Motivation</span></span>
<span data-ttu-id="b55c1-105">Prace nad projektowaniem typów referencyjnych dopuszczających wartość null spowodowały badanie kodu niezbędnego do weryfikacji argumentu `null`.</span><span class="sxs-lookup"><span data-stu-id="b55c1-105">The work on designing nullable reference types has caused us to examine the code necessary for `null` argument validation.</span></span> <span data-ttu-id="b55c1-106">Uwzględniając, że NRT nie ma wpływu na deweloperów wykonujących kod, nadal musi dodawać `if (arg is null) throw` kod kliszy kotłowej nawet w projektach, które są w pełni `null` czyste.</span><span class="sxs-lookup"><span data-stu-id="b55c1-106">Given that NRT doesn't affect code execution developers still must add `if (arg is null) throw` boiler plate code even in projects which are fully `null` clean.</span></span> <span data-ttu-id="b55c1-107">Dzięki temu możemy poznać minimalną składnię argumentu `null` walidacji w języku.</span><span class="sxs-lookup"><span data-stu-id="b55c1-107">This gave us the desire to explore a minimal syntax for argument `null` validation in the language.</span></span> 

<span data-ttu-id="b55c1-108">Ta składnia weryfikacji parametrów `null` powinna być często sparowana z NRT, dlatego propozycja jest w pełni niezależna od niej.</span><span class="sxs-lookup"><span data-stu-id="b55c1-108">While this `null` parameter validation syntax is expected to pair frequently with NRT the proposal is fully independent of it.</span></span> <span data-ttu-id="b55c1-109">Składnia może być używana niezależnie od dyrektyw `#nullable`.</span><span class="sxs-lookup"><span data-stu-id="b55c1-109">The syntax can be used independent of `#nullable` directives.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b55c1-110">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="b55c1-110">Detailed Design</span></span> 

### <a name="null-validation-parameter-syntax"></a><span data-ttu-id="b55c1-111">Składnia parametru weryfikacji o wartości null</span><span class="sxs-lookup"><span data-stu-id="b55c1-111">Null validation parameter syntax</span></span>
<span data-ttu-id="b55c1-112">Operator wykrzyknika, `!`, może być umieszczony po nazwie parametru na liście parametrów, co spowoduje, C# że kompilator emituje standardowy `null` Sprawdzanie kodu dla tego parametru.</span><span class="sxs-lookup"><span data-stu-id="b55c1-112">The bang operator, `!`, can be positioned after a parameter name in a parameter list and this will cause the C# compiler to emit standard `null` checking code for that parameter.</span></span> <span data-ttu-id="b55c1-113">Jest to określane jako `null` składni parametrów walidacji.</span><span class="sxs-lookup"><span data-stu-id="b55c1-113">This is referred to as `null` validation parameter syntax.</span></span> <span data-ttu-id="b55c1-114">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b55c1-114">For example:</span></span>

``` csharp
void M(string name!) {
    ...
}
```

<span data-ttu-id="b55c1-115">Zostanie przetłumaczony na:</span><span class="sxs-lookup"><span data-stu-id="b55c1-115">Will be translated into:</span></span>

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

<span data-ttu-id="b55c1-116">Wygenerowane `null` Sprawdzanie zostanie przeprowadzone przed dowolnym kodem utworzonym przez dewelopera w metodzie.</span><span class="sxs-lookup"><span data-stu-id="b55c1-116">The generated `null` check will occur before any developer authored code in the method.</span></span> <span data-ttu-id="b55c1-117">Gdy wiele parametrów zawiera operator `!`, sprawdzenia będą wykonywane w takiej samej kolejności, w jakiej są zadeklarowane parametry.</span><span class="sxs-lookup"><span data-stu-id="b55c1-117">When multiple parameters contain the `!` operator then the checks will occur in the same order as the parameters are declared.</span></span>

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

<span data-ttu-id="b55c1-118">Sprawdzenie będzie przeznaczone dla równości referencyjnych `null`, nie wywołuje `==` ani żadnych operatorów zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b55c1-118">The check will be specifically for reference equality to `null`, it does not invoke `==` or any user defined operators.</span></span> <span data-ttu-id="b55c1-119">Oznacza to również, że operator `!` może zostać dodany tylko do parametrów, których typ może być testowany pod kątem równości względem `null`.</span><span class="sxs-lookup"><span data-stu-id="b55c1-119">This also means the `!` operator can only be added to parameters whose type can be tested for equality against `null`.</span></span> <span data-ttu-id="b55c1-120">Oznacza to, że nie można jej użyć na parametrze, którego typ jest znany jako typ wartości.</span><span class="sxs-lookup"><span data-stu-id="b55c1-120">This means it can't be used on a parameter whose type is known to be a value type.</span></span>

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

<span data-ttu-id="b55c1-121">W przypadku konstruktora `null` Walidacja zostanie wykonana przed jakimkolwiek innym kodem w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="b55c1-121">In the case of a constructor the `null` validation will occur before any other code in the constructor.</span></span> <span data-ttu-id="b55c1-122">Obejmuje to:</span><span class="sxs-lookup"><span data-stu-id="b55c1-122">That includes:</span></span> 

- <span data-ttu-id="b55c1-123">Tworzenie łańcuchów do innych konstruktorów z `this` lub `base`</span><span class="sxs-lookup"><span data-stu-id="b55c1-123">Chaining to other constructors with `this` or `base`</span></span> 
- <span data-ttu-id="b55c1-124">Inicjatory pola, które są niejawnie wykonywane w konstruktorze</span><span class="sxs-lookup"><span data-stu-id="b55c1-124">Field initializers which implicitly occur in the constructor</span></span>

<span data-ttu-id="b55c1-125">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b55c1-125">For example:</span></span>

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

<span data-ttu-id="b55c1-126">Zostanie przetłumaczy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b55c1-126">Will be roughly translated into the following:</span></span>

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

<span data-ttu-id="b55c1-127">Uwaga: nie jest to kod C# prawny, ale zamiast tego wystarczy przybliżyć to, co robi implementacja.</span><span class="sxs-lookup"><span data-stu-id="b55c1-127">Note: this is not legal C# code but instead just an approximation of what the implementation does.</span></span> 

<span data-ttu-id="b55c1-128">Składnia parametru walidacji `null` będzie również prawidłowa na listach parametrów lambda.</span><span class="sxs-lookup"><span data-stu-id="b55c1-128">The `null` validation parameter syntax will also be valid on lambda parameter lists.</span></span> <span data-ttu-id="b55c1-129">Jest to prawidłowe, nawet w składni jednego parametru, który nie ma parens.</span><span class="sxs-lookup"><span data-stu-id="b55c1-129">This is valid even in the single parameter syntax that lacks parens.</span></span>

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

<span data-ttu-id="b55c1-130">Składnia jest również prawidłowa dla parametrów metod iteratora.</span><span class="sxs-lookup"><span data-stu-id="b55c1-130">The syntax is also valid on parameters to iterator methods.</span></span> <span data-ttu-id="b55c1-131">W przeciwieństwie do innego kodu w iteratoru `null` Walidacja będzie wykonywana, gdy wywoływana jest metoda iteratora, a nie gdy zostanie przeprowadzony bazowy moduł wyliczający.</span><span class="sxs-lookup"><span data-stu-id="b55c1-131">Unlike other code in the iterator the `null` validation will occur when the iterator method is invoked, not when the underlying enumerator is walked.</span></span> <span data-ttu-id="b55c1-132">Jest to prawdziwe w przypadku iteratorów tradycyjnych lub `async`.</span><span class="sxs-lookup"><span data-stu-id="b55c1-132">This is true for traditional or `async` iterators.</span></span>

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

<span data-ttu-id="b55c1-133">Operatora `!` można używać tylko dla list parametrów, które mają skojarzoną treść metody.</span><span class="sxs-lookup"><span data-stu-id="b55c1-133">The `!` operator can only be used for parameter lists which have an associated method body.</span></span> <span data-ttu-id="b55c1-134">Oznacza to, że nie można go użyć w metodzie `abstract`, `interface`, `delegate` lub `partial` metodzie.</span><span class="sxs-lookup"><span data-stu-id="b55c1-134">This means it cannot be used in an `abstract` method, `interface`, `delegate` or `partial` method definition.</span></span>

### <a name="extending-is-null"></a><span data-ttu-id="b55c1-135">Rozszerzanie ma wartość null</span><span class="sxs-lookup"><span data-stu-id="b55c1-135">Extending is null</span></span>
<span data-ttu-id="b55c1-136">Typy, dla których wyrażenie `is null` jest prawidłowe, zostaną rozszerzone w taki sposób, aby obejmowało parametry typu nieograniczone.</span><span class="sxs-lookup"><span data-stu-id="b55c1-136">The types for which the expression `is null` is valid will be extended to include unconstrained type parameters.</span></span> <span data-ttu-id="b55c1-137">Pozwoli to na wypełnienie zamiaru sprawdzenia dla `null` wszystkich typów, które sprawdzają `null`.</span><span class="sxs-lookup"><span data-stu-id="b55c1-137">This will allow it to fill the intent of checking for `null` on all types which a `null` check is valid.</span></span> <span data-ttu-id="b55c1-138">W przypadku typów, które nie są czasowo znane jako typy wartości.</span><span class="sxs-lookup"><span data-stu-id="b55c1-138">Specifically that is types which are not definitely known to be value types.</span></span> <span data-ttu-id="b55c1-139">Na przykład parametry typu, które są ograniczone do `struct` nie mogą być używane z tą składnią.</span><span class="sxs-lookup"><span data-stu-id="b55c1-139">For example Type parameters which are constrained to `struct` cannot be used with this syntax.</span></span>

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

<span data-ttu-id="b55c1-140">Zachowanie `is null` na parametrze typu będzie takie samo, jak `== null` dzisiaj.</span><span class="sxs-lookup"><span data-stu-id="b55c1-140">The behavior of `is null` on a type parameter will be the same as `== null` today.</span></span> <span data-ttu-id="b55c1-141">W przypadku wystąpienia parametru typu jako typu wartości kod zostanie oceniony jako `false`.</span><span class="sxs-lookup"><span data-stu-id="b55c1-141">In the cases where the type parameter is instantiated as a value type the code will be evaluated as `false`.</span></span> <span data-ttu-id="b55c1-142">W przypadku, gdy jest to typ referencyjny, kod będzie sprawdzał poprawność `is null`.</span><span class="sxs-lookup"><span data-stu-id="b55c1-142">For cases where it is a reference type the code will do a proper `is null` check.</span></span>

### <a name="intersection-with-nullable-reference-types"></a><span data-ttu-id="b55c1-143">Część wspólna z typami odwołań dopuszczających wartość null</span><span class="sxs-lookup"><span data-stu-id="b55c1-143">Intersection with Nullable Reference Types</span></span>
<span data-ttu-id="b55c1-144">Wszystkie parametry, które mają zastosowany operator `!`, zaczynają się od nie`null`go stanu dopuszczającego wartość null.</span><span class="sxs-lookup"><span data-stu-id="b55c1-144">Any parameter which has a `!` operator applied to it's name will start with the nullable state being not `null`.</span></span> <span data-ttu-id="b55c1-145">Jest to prawdziwe, nawet jeśli typ samego parametru jest potencjalnie `null`.</span><span class="sxs-lookup"><span data-stu-id="b55c1-145">This is true even if the type of the parameter itself is potentially `null`.</span></span> <span data-ttu-id="b55c1-146">Może wystąpić w przypadku jawnego typu dopuszczającego wartość null, takiego jak powiedzieć `string?`lub z nieograniczeniem typu parametru.</span><span class="sxs-lookup"><span data-stu-id="b55c1-146">That can occur with an explicitly nullable type, such as say `string?`, or with an unconstrained type parameter.</span></span> 

<span data-ttu-id="b55c1-147">Gdy składnia `!` w parametrach jest łączona z jawnym typem dopuszczającym wartość null w parametrze, zostanie wygenerowane ostrzeżenie przez kompilator:</span><span class="sxs-lookup"><span data-stu-id="b55c1-147">When a `!` syntax on parameters is combined with an explicitly nullable type on the parameter then a warning will be issued by the compiler:</span></span>

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a><span data-ttu-id="b55c1-148">Otwarte problemy</span><span class="sxs-lookup"><span data-stu-id="b55c1-148">Open Issues</span></span>
<span data-ttu-id="b55c1-149">None</span><span class="sxs-lookup"><span data-stu-id="b55c1-149">None</span></span>

## <a name="considerations"></a><span data-ttu-id="b55c1-150">Zagadnienia do rozważenia</span><span class="sxs-lookup"><span data-stu-id="b55c1-150">Considerations</span></span>

### <a name="constructors"></a><span data-ttu-id="b55c1-151">Konstruktorów</span><span class="sxs-lookup"><span data-stu-id="b55c1-151">Constructors</span></span>
<span data-ttu-id="b55c1-152">Generowanie kodu dla konstruktorów oznacza, że istnieje małe, ale zauważalne, zmiana zachowania podczas przechodzenia z standardowej weryfikacji `null`ej dzisiaj i składni parametru weryfikacji `null` (`!`).</span><span class="sxs-lookup"><span data-stu-id="b55c1-152">The code generation for constructors means there is a small, but observable, behavior change when moving from standard `null` validation today and the `null` validation parameter syntax (`!`).</span></span> <span data-ttu-id="b55c1-153">Sprawdzanie `null` w standardowej weryfikacji odbywa się po obu inicjatorach pola i wywołaniach `base` lub `this`.</span><span class="sxs-lookup"><span data-stu-id="b55c1-153">The `null` check in standard validation occurs after both field initializers and any `base` or `this` calls.</span></span> <span data-ttu-id="b55c1-154">Oznacza to, że deweloper nie może koniecznie migrować 100% `null` weryfikacji do nowej składni.</span><span class="sxs-lookup"><span data-stu-id="b55c1-154">This means a developer can't necessarily migrate 100% of their `null` validation to the new syntax.</span></span> <span data-ttu-id="b55c1-155">Konstruktory z co najmniej wymagają pewnej inspekcji.</span><span class="sxs-lookup"><span data-stu-id="b55c1-155">Constructors at least require some inspection.</span></span>

<span data-ttu-id="b55c1-156">Po dokonaniu dyskusji chociaż została podjęta decyzja, że jest to bardzo mało prawdopodobne, aby spowodować poważne problemy z wdrażaniem.</span><span class="sxs-lookup"><span data-stu-id="b55c1-156">After discussion though it was decided that this is very unlikely to cause any significant adoption issues.</span></span> <span data-ttu-id="b55c1-157">Jest to bardziej logiczne, że sprawdzanie `null` zostanie uruchomione przed jakąkolwiek logiką w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="b55c1-157">It's more logical that the `null` check run before any logic in the constructor does.</span></span> <span data-ttu-id="b55c1-158">Może ponownie odwiedzać w przypadku wykrycia znaczących problemów ze zgodnością.</span><span class="sxs-lookup"><span data-stu-id="b55c1-158">Can revisit if significant compat issues are discovered.</span></span>

### <a name="warning-when-mixing--and-"></a><span data-ttu-id="b55c1-159">Ostrzegaj przy miksowaniu?</span><span class="sxs-lookup"><span data-stu-id="b55c1-159">Warning when mixing ?</span></span> <span data-ttu-id="b55c1-160">lub!</span><span class="sxs-lookup"><span data-stu-id="b55c1-160">and !</span></span>
<span data-ttu-id="b55c1-161">Istniała długotrwała dyskusja dotycząca tego, czy należy wydać ostrzeżenie, gdy składnia `!` jest stosowana do parametru, który jawnie wpisano do typu dopuszczającego wartość null.</span><span class="sxs-lookup"><span data-stu-id="b55c1-161">There was a lengthy discussion on whether or not a warning should be issued when the `!` syntax is applied to a parameter which is explicitly typed to a nullable type.</span></span> <span data-ttu-id="b55c1-162">Na powierzchni, która wygląda jak Deklaracja bezsensowniea przez dewelopera, ale istnieją przypadki, w których hierarchie typów mogą zmusić deweloperów do takiej sytuacji.</span><span class="sxs-lookup"><span data-stu-id="b55c1-162">On the surface it seems like a nonsensical declaration by the developer but there are cases where type hierarchies could force developers into such a situation.</span></span> 

<span data-ttu-id="b55c1-163">Rozważmy następujące hierarchie klas w szeregu zestawów (przy założeniu, że wszystkie są kompilowane z włączonym sprawdzaniem `null`):</span><span class="sxs-lookup"><span data-stu-id="b55c1-163">Consider the following class hierarchy across a series of assemblies (assuming all are compiled with `null` checking enabled):</span></span>

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

<span data-ttu-id="b55c1-164">W tym miejscu autor `C3` zdecydował się dodać `null` walidacji do `o`parametru.</span><span class="sxs-lookup"><span data-stu-id="b55c1-164">Here the author of `C3` decided to add `null` validation to the parameter `o`.</span></span> <span data-ttu-id="b55c1-165">Jest to całkowicie zgodne z tym, w jaki sposób funkcja jest przeznaczona do użycia.</span><span class="sxs-lookup"><span data-stu-id="b55c1-165">This is completely in line with how the feature is intended to be used.</span></span>

<span data-ttu-id="b55c1-166">Teraz wyobraź sobie, że autor Assembly2 zdecyduje się na dodanie następującego przesłonięcia:</span><span class="sxs-lookup"><span data-stu-id="b55c1-166">Now imagine at a later date the author of Assembly2 decides to add the following override:</span></span>

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

<span data-ttu-id="b55c1-167">Jest to dozwolone w przypadku typów referencyjnych dopuszczających wartość null, ponieważ jest to prawne, aby zamówienie było bardziej elastyczne dla pozycji wejściowych.</span><span class="sxs-lookup"><span data-stu-id="b55c1-167">This is allowed by nullable reference types as it's legal to make the contract more flexible for input positions.</span></span> <span data-ttu-id="b55c1-168">Funkcja NRT ogólnie zezwala na rozsądną współkontrawariancjaę parametru/zwracaną wartość null.</span><span class="sxs-lookup"><span data-stu-id="b55c1-168">The NRT feature in general allows for reasonable co/contravariance on parameter / return nullability.</span></span> <span data-ttu-id="b55c1-169">Jednak język wykonuje sprawdzanie współ/kontrawariancja na podstawie najbardziej określonego przesłonięcia, a nie oryginalnej deklaracji.</span><span class="sxs-lookup"><span data-stu-id="b55c1-169">However the language does the co/contravariance checking based on the most specific override, not the original declaration.</span></span> <span data-ttu-id="b55c1-170">Oznacza to, że autor elementu Assembly3 wyświetli ostrzeżenie o typie `o` niezgodnym i będzie musiał zmienić podpis na następujący, aby wyeliminować:</span><span class="sxs-lookup"><span data-stu-id="b55c1-170">This means the author of Assembly3 will get a warning about the type of `o` not matching and will need to change the signature to the following to eliminate it:</span></span> 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

<span data-ttu-id="b55c1-171">W tym momencie autor Assembly3 ma kilka opcji:</span><span class="sxs-lookup"><span data-stu-id="b55c1-171">At this point the author of Assembly3 has a few choices:</span></span>

- <span data-ttu-id="b55c1-172">Mogą zaakceptować/pominąć ostrzeżenie dotyczące `object?` i `object` niezgodności.</span><span class="sxs-lookup"><span data-stu-id="b55c1-172">They can accept / suppress the warning about `object?` and `object` mismatch.</span></span>
- <span data-ttu-id="b55c1-173">Mogą zaakceptować/pominąć ostrzeżenie dotyczące `object?` i `!` niezgodności.</span><span class="sxs-lookup"><span data-stu-id="b55c1-173">They can accept / suppress the warning about `object?` and `!` mismatch.</span></span>
- <span data-ttu-id="b55c1-174">Można po prostu usunąć `null` sprawdzanie poprawności (Usuń `!` i przeprowadzić jawne sprawdzanie)</span><span class="sxs-lookup"><span data-stu-id="b55c1-174">They can just remove the `null` validation check (delete `!` and do explicit checking)</span></span>

<span data-ttu-id="b55c1-175">Jest to prawdziwy scenariusz, ale teraz pomysłem jest przechodzenie do przodu z ostrzeżeniem.</span><span class="sxs-lookup"><span data-stu-id="b55c1-175">This is a real scenario but for now the idea is to move forward with the warning.</span></span> <span data-ttu-id="b55c1-176">W przypadku wypróbowania ostrzeżenia zdarza się częściej niż przewidywane, możemy je później usunąć (odwrotnie nie jest prawdziwe).</span><span class="sxs-lookup"><span data-stu-id="b55c1-176">If it turns out the warning happens more frequently than we anticipate then we can remove it later (the reverse is not true).</span></span>

### <a name="implicit-property-setter-arguments"></a><span data-ttu-id="b55c1-177">Niejawne argumenty metody ustawiającej właściwości</span><span class="sxs-lookup"><span data-stu-id="b55c1-177">Implicit property setter arguments</span></span>
<span data-ttu-id="b55c1-178">`value` argument parametru jest niejawny i nie jest wyświetlany na liście parametrów.</span><span class="sxs-lookup"><span data-stu-id="b55c1-178">The `value` argument of a parameter is implicit and does not appear in any parameter list.</span></span> <span data-ttu-id="b55c1-179">Oznacza to, że nie może być elementem docelowym tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="b55c1-179">That means it cannot be a target of this feature.</span></span> <span data-ttu-id="b55c1-180">Składnia metody ustawiającej właściwość może zostać rozszerzona w celu uwzględnienia listy parametrów, aby umożliwić stosowanie operatora `!`.</span><span class="sxs-lookup"><span data-stu-id="b55c1-180">The property setter syntax could be extended to include a parameter list to allow the `!` operator to be applied.</span></span> <span data-ttu-id="b55c1-181">Ale jest to poprzed pomysłem, że ta funkcja sprawia, że `null` sprawdzanie poprawności jest prostsze.</span><span class="sxs-lookup"><span data-stu-id="b55c1-181">But that cuts against the idea of this feature making `null` validation simpler.</span></span> <span data-ttu-id="b55c1-182">Ponieważ niejawny argument `value` po prostu nie będzie działać z tą funkcją.</span><span class="sxs-lookup"><span data-stu-id="b55c1-182">As such the implicit `value` argument just won't work with this feature.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="b55c1-183">Zagadnienia w przyszłości</span><span class="sxs-lookup"><span data-stu-id="b55c1-183">Future Considerations</span></span>
<span data-ttu-id="b55c1-184">None</span><span class="sxs-lookup"><span data-stu-id="b55c1-184">None</span></span>
