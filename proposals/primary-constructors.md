---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2019
ms.locfileid: "79484973"
---
# <a name="primary-constructors"></a><span data-ttu-id="45529-101">Konstruktory podstawowe</span><span class="sxs-lookup"><span data-stu-id="45529-101">Primary constructors</span></span>

* <span data-ttu-id="45529-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="45529-102">[x] Proposed</span></span>
* <span data-ttu-id="45529-103">[] Prototyp: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="45529-103">[ ] Prototype: Not started</span></span>
* <span data-ttu-id="45529-104">[] Implementacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="45529-104">[ ] Implementation: Not started</span></span>
* <span data-ttu-id="45529-105">[] Specyfikacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="45529-105">[ ] Specification: Not started</span></span>

## <a name="summary"></a><span data-ttu-id="45529-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="45529-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="45529-107">Klasy mogą mieć listę parametrów, a kiedy tak, ich Specyfikacja klasy bazowej może mieć listę argumentów.</span><span class="sxs-lookup"><span data-stu-id="45529-107">Classes can have a parameter list, and when they do, their base class specification can have an argument list.</span></span>
<span data-ttu-id="45529-108">Parametry konstruktora podstawowego są w zakresie w całej deklaracji klasy, a jeśli są przechwytywane przez składową funkcji lub funkcję anonimową, są one przechowywane jako pola prywatne w klasie.</span><span class="sxs-lookup"><span data-stu-id="45529-108">Primary constructor parameters are in scope throughout the class declaration, and if they are captured by a function member or anonymous function, they are stored as private fields in the class.</span></span>

## <a name="motivation"></a><span data-ttu-id="45529-109">Motywacją</span><span class="sxs-lookup"><span data-stu-id="45529-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="45529-110">Często istnieje wiele standardowych kodów inicjalizacji programu.</span><span class="sxs-lookup"><span data-stu-id="45529-110">It is common to have a lot of boilerplate in program initialization code.</span></span> <span data-ttu-id="45529-111">W ogólnym przypadku dana część danych `x` jest określona wiele razy:</span><span class="sxs-lookup"><span data-stu-id="45529-111">In the general case, a given piece of data `x` is mentioned many times:</span></span>

- <span data-ttu-id="45529-112">Jako pole prywatne `_x`</span><span class="sxs-lookup"><span data-stu-id="45529-112">As a private field `_x`</span></span>
- <span data-ttu-id="45529-113">Jako parametr `x` konstruktora</span><span class="sxs-lookup"><span data-stu-id="45529-113">As a parameter `x` to a constructor</span></span>
- <span data-ttu-id="45529-114">W `_x = x;` przypisania pola z parametru w konstruktorze</span><span class="sxs-lookup"><span data-stu-id="45529-114">In an assignment `_x = x;` of the field from the parameter in the constructor</span></span>
- <span data-ttu-id="45529-115">Jako `X` właściwości</span><span class="sxs-lookup"><span data-stu-id="45529-115">As a property `X`</span></span>
- <span data-ttu-id="45529-116">W `x = value;` setter właściwości</span><span class="sxs-lookup"><span data-stu-id="45529-116">In the property setter `x = value;`</span></span>
- <span data-ttu-id="45529-117">W `return x;` metody pobierającej właściwości</span><span class="sxs-lookup"><span data-stu-id="45529-117">In the property getter `return x;`</span></span>

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

<span data-ttu-id="45529-118">W przypadku właściwości, które nie wymagają weryfikacji ani obliczeń, monotonnych czynności można zmniejszyć przy użyciu właściwości autoproperties, a tym samym wycofać potrzebę deklarowania jawnego pola zapasowego dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="45529-118">For properties that don't require validation or computation, the tedium can be reduced using auto-properties, thus cutting out the need to declare an explicit backing field for the property.</span></span> <span data-ttu-id="45529-119">Ale jeśli właściwość wymaga żadnych rodzajów logiki wykraczających poza wartość właściwości autoproperty, powyższym jest najlepszy użytkownik.</span><span class="sxs-lookup"><span data-stu-id="45529-119">But if your property requires any sort of logic beyond what an auto-property provides, the above is the best you an do.</span></span>

<span data-ttu-id="45529-120">Konstruktory podstawowe zamiast tego zmniejszają obciążenie przez umieszczenie argumentów konstruktora bezpośrednio w zakresie w całej klasie, co eliminuje ponownie, aby jawnie zadeklarować pole zapasowe.</span><span class="sxs-lookup"><span data-stu-id="45529-120">Primary constructors instead reduce the overhead by putting constructor arguments directly in scope throughout the class, again obviating the need to explicitly declare a backing field.</span></span> <span data-ttu-id="45529-121">W ten sposób powyższy przykład:</span><span class="sxs-lookup"><span data-stu-id="45529-121">Thus, the above example would become:</span></span>

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

<span data-ttu-id="45529-122">W tym przykładzie Konstruktor podstawowy zmniejsza liczbę nazwanych jednostek dla `x` z trzech do dwóch, co eliminuje pole `_x` zapasowe.</span><span class="sxs-lookup"><span data-stu-id="45529-122">In this example, the primary constructor reduces the number of named entities for `x` from three to two, obviating the `_x` backing field.</span></span> <span data-ttu-id="45529-123">Usuwa dwie z trzech deklaracji składowych (tylko deklaracje właściwości) i zmniejsza łączną liczbę wzmianki o `x`/`_x`/`X` z ośmiu do pięciu.</span><span class="sxs-lookup"><span data-stu-id="45529-123">It removes two out of three member declarations (keeping only the property declaration itself), and reduces the total number of mentions of `x`/`_x`/`X` from eight to five.</span></span>


## <a name="detailed-design"></a><span data-ttu-id="45529-124">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="45529-124">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="45529-125">Klasy mogą mieć listę parametrów:</span><span class="sxs-lookup"><span data-stu-id="45529-125">Classes can have a parameter list:</span></span>

``` c#
public class C(int i, string s)
{
    ...
}
```

<span data-ttu-id="45529-126">Lista parametrów powoduje, że Konstruktor ma być niejawnie zadeklarowany dla klasy, z taką samą dostępnością jak sama klasa.</span><span class="sxs-lookup"><span data-stu-id="45529-126">The parameter list causes a constructor to be implicitly declared for the class, with the same accessibility as the class itself.</span></span>

``` c#
new C(5, "Hello");
```

<span data-ttu-id="45529-127">Parametry konstruktora podstawowego są w zakresie w całej treści klasy.</span><span class="sxs-lookup"><span data-stu-id="45529-127">Primary constructor parameters are in scope throughout the class body.</span></span> <span data-ttu-id="45529-128">Jeśli są przechwytywane przez element członkowski funkcji lub funkcję anonimową, stają się one przechowywane jako pola prywatne w klasie.</span><span class="sxs-lookup"><span data-stu-id="45529-128">If they are captured by a function member or anonymous function, they become stored as private fields in the class.</span></span> <span data-ttu-id="45529-129">Jeśli są używane tylko podczas inicjowania, nie będą przechowywane w obiekcie.</span><span class="sxs-lookup"><span data-stu-id="45529-129">If they are only used during initialization they will not be stored in the object.</span></span>

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

<span data-ttu-id="45529-130">Jeśli Klasa z konstruktorem podstawowym ma specyfikację klasy bazowej, która może mieć listę argumentów.</span><span class="sxs-lookup"><span data-stu-id="45529-130">If a class with a primary constructor has a base class specification, that one can have an argument list.</span></span> <span data-ttu-id="45529-131">Służy to jako lista argumentów dla inicjatora `base(...)` niejawnie zadeklarowanego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="45529-131">This serves as the argument list to a `base(...)` initializer of the implicitly declared constructor.</span></span> <span data-ttu-id="45529-132">Jeśli nie podano listy argumentów, przyjmuje się, że jest ona pusta.</span><span class="sxs-lookup"><span data-stu-id="45529-132">If no argument list is provided, an empty one is assumed.</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
<span data-ttu-id="45529-133">Klasa może mieć również jawnie zdefiniowane konstruktory, ale te wszystkie muszą używać inicjatora `this(...)`.</span><span class="sxs-lookup"><span data-stu-id="45529-133">The class can have explicitly defined constructors as well, but those all have to use a `this(...)` initializer.</span></span> <span data-ttu-id="45529-134">Dzięki temu Konstruktor podstawowy jest zawsze wywoływany, gdy zostanie skonstruowane nowe wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="45529-134">This ensures that the primary constructor is always called when a new instance is constructed.</span></span>

<span data-ttu-id="45529-135">Wszystkie inicjatory w treści klasy staną się przypisaniami w wygenerowanym konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="45529-135">All initializers in the class body will become assignments in the generated constructor.</span></span> <span data-ttu-id="45529-136">Oznacza to, że w przeciwieństwie do innych klas inicjatory zostaną uruchomione *po* wywołaniu konstruktora podstawowego, a nie przed.</span><span class="sxs-lookup"><span data-stu-id="45529-136">This means that, unlike other classes, initializers will run *after* the base constructor has been invoked, not before.</span></span> <span data-ttu-id="45529-137">Ponadto wygenerowana Klasa będzie zawierać przypisania umożliwiające zainicjowanie wszelkich prywatnych pól, które zostały wygenerowane w celu przechowywania parametrów konstruktora podstawowego, które zostały przechwycone przez elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="45529-137">In addition, the generated class will contain assignments to initialize any private fields that were generated to store primary constructor parameters that were captured by members.</span></span> <span data-ttu-id="45529-138">Te elementy członkowskie są ponownie zapisywane w celu użycia pola private zamiast parametru w sposób podobny do zamknięć dla wyrażeń lambda.</span><span class="sxs-lookup"><span data-stu-id="45529-138">Those members are rewritten to use the private field instead of the parameter in a manner similar to closures for lambda expressions.</span></span> <span data-ttu-id="45529-139">Wygenerowane pola podstawowe są inicjowane jako pierwsze, a następnie przypisania generowane przez inicjatora są wykonywane w kolejności występowania w klasie.</span><span class="sxs-lookup"><span data-stu-id="45529-139">The generated primary fields are initialized first, and then the initializer-generated assignments are executed in the order of appearance in the class.</span></span>

<span data-ttu-id="45529-140">Dla powyższego przykładu efekt deklaracji klasy jest tak, jakby został zapisany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="45529-140">For the above example, the effect of the class declaration is as if rewritten like this:</span></span>

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

<span data-ttu-id="45529-141">Przechwytywanie ma podobne ograniczenia dotyczące przechwytywania zmiennych lokalnych w wyrażeniach lambda.</span><span class="sxs-lookup"><span data-stu-id="45529-141">The capture has similar restrictions to the capture of local variables by lambda expressions.</span></span> <span data-ttu-id="45529-142">Na przykład parametry `ref` i `out` są dozwolone w konstruktorach podstawowych, ale nie mogą być przechwytywane przez te jednostki członkowskie.</span><span class="sxs-lookup"><span data-stu-id="45529-142">For instance, `ref` and `out` parameters are allowed in primary constructors, but cannot be captured my member bodies.</span></span>


## <a name="drawbacks"></a><span data-ttu-id="45529-143">Wady</span><span class="sxs-lookup"><span data-stu-id="45529-143">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="45529-144">W przybliżeniu istotny porządek.</span><span class="sxs-lookup"><span data-stu-id="45529-144">In rough order of significance.</span></span>

* <span data-ttu-id="45529-145">Propozycja używa składni, która została również zaproponowana dla rekordów pozycyjnych.</span><span class="sxs-lookup"><span data-stu-id="45529-145">The proposal uses syntax that has also been proposed for positional records.</span></span> <span data-ttu-id="45529-146">Jeśli potrzebujemy obu funkcji, wymagane jest pewne zakwaterowanie.</span><span class="sxs-lookup"><span data-stu-id="45529-146">If we desire both features, some accommodation is required.</span></span> <span data-ttu-id="45529-147">Na przykład</span><span class="sxs-lookup"><span data-stu-id="45529-147">E.g.</span></span> <span data-ttu-id="45529-148">zaproponowano modyfikator `data` dla rekordów.</span><span class="sxs-lookup"><span data-stu-id="45529-148">a `data` modifier on records has been proposed.</span></span>
* <span data-ttu-id="45529-149">Rozmiar alokacji skonstruowanych obiektów jest mniej oczywisty, ponieważ kompilator określa, czy przydzielić pole dla podstawowego parametru konstruktora na podstawie pełnego tekstu klasy.</span><span class="sxs-lookup"><span data-stu-id="45529-149">The allocation size of constructed objects is less obvious, as the compiler determines whether to allocate a field for a primary constructor parameter based on the full text of the class.</span></span> <span data-ttu-id="45529-150">To ryzyko jest podobne do niejawnego przechwytywania zmiennych przez wyrażenia lambda.</span><span class="sxs-lookup"><span data-stu-id="45529-150">This risk is similar to the implicit capture of variables by lambda expressions.</span></span>
* <span data-ttu-id="45529-151">Typowy Temptation (lub przypadkowe) może być przechwyceniem parametru "ten sam" na wielu poziomach dziedziczenia, ponieważ jest ona przenoszona do łańcucha konstruktorów, a nie jawnie allotting to pole chronione w klasie bazowej, co prowadzi do duplikowania alokacji dla tych samych danych w obiektach.</span><span class="sxs-lookup"><span data-stu-id="45529-151">A common temptation (or accidental pattern) might be to capture the "same" parameter at multiple levels of inheritance as it is passed up the constructor chain instead of explicitly allotting it a protected field at the base class, leading to duplicated allocations for the same data in objects.</span></span> <span data-ttu-id="45529-152">Jest to bardzo podobne do współczesnego ryzyka przesłaniania autowłaściwości przy użyciu właściwości autoproperties.</span><span class="sxs-lookup"><span data-stu-id="45529-152">This is very similar to today's risk of overriding auto-properties with auto-properties.</span></span> 
* <span data-ttu-id="45529-153">Jak wspomniano powyżej, nie ma miejsca dla dodatkowej logiki, która może być zwykle wyrażona w treści konstruktora.</span><span class="sxs-lookup"><span data-stu-id="45529-153">As proposed above, there is no place for additional logic that might usually expressed in constructor bodies.</span></span> <span data-ttu-id="45529-154">Poniższe rozszerzenie "podstawowe konstruktory" zawiera adresy.</span><span class="sxs-lookup"><span data-stu-id="45529-154">The "Primary constructor bodies" extension below addresses that.</span></span>
* <span data-ttu-id="45529-155">Zgodnie z proponowaną semantyką kolejności wykonywania jest nieco inne niż w przypadku zwykłych konstruktorów.</span><span class="sxs-lookup"><span data-stu-id="45529-155">As proposed, execution order semantics are subtly different than with ordinary constructors.</span></span> <span data-ttu-id="45529-156">Może to być możliwe do zaradzić, ale kosztem niektórych propozycji rozszerzenia (szczególnie "podstawowych jednostek konstruktorów").</span><span class="sxs-lookup"><span data-stu-id="45529-156">This could probably be remedied, but at the cost of some of the extension proposals (notably "Primary constructor bodies").</span></span>
* <span data-ttu-id="45529-157">Propozycja działa tylko wtedy, gdy jeden konstruktor może być wyznaczony jako podstawowy.</span><span class="sxs-lookup"><span data-stu-id="45529-157">The proposal only works when a single constructor can be designated primary.</span></span>
* <span data-ttu-id="45529-158">Nie ma żadnego sposobu, aby mieć osobną dostępność klasy i konstruktora podstawowego.</span><span class="sxs-lookup"><span data-stu-id="45529-158">There is no way to have separate accessibility of the class and the primary constructor.</span></span> <span data-ttu-id="45529-159">Na przykład w sytuacjach, w których konstruktory publiczne wszystkie Delegaty do jednego prywatnego konstruktora "Kompiluj-it-all", które byłyby konieczne.</span><span class="sxs-lookup"><span data-stu-id="45529-159">For instance, in situations where public constructors all delegate to one private "build-it-all" constructor that would be needed.</span></span> <span data-ttu-id="45529-160">W razie potrzeby można zaproponować składnię w późniejszym czasie.</span><span class="sxs-lookup"><span data-stu-id="45529-160">If necessary, syntax could be proposed for that later.</span></span>


## <a name="alternatives"></a><span data-ttu-id="45529-161">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="45529-161">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="45529-162">Pełne rekordy pozycyjne mogą być alternatywą lub mogą współistnieć z konstruktorami podstawowymi, w zależności od określonych.</span><span class="sxs-lookup"><span data-stu-id="45529-162">Full-on positional records may be an alternative, or may coexist with primary constructors, depending on the specifics.</span></span> <span data-ttu-id="45529-163">Mogą one mieć *więcej* skrótów w *mniejszej* liczbie scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="45529-163">They would allow for *more* abbreviation in a *smaller* number of scenarios.</span></span> <span data-ttu-id="45529-164">Dlatego obydwie są przydatne, ale mogą być zbyt obszernee, chyba że mogą one być ze sobą starannie zintegrowane.</span><span class="sxs-lookup"><span data-stu-id="45529-164">So both are potentially useful, but having both may be overkill, unless they can be somewhat neatly integrated with each other.</span></span>


## <a name="possible-extensions"></a><span data-ttu-id="45529-165">Możliwe rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="45529-165">Possible extensions</span></span>
[extensions]: #possible-extensions

<span data-ttu-id="45529-166">Są to odmiany lub dodatki do podstawowej propozycji, które mogą być rozważane razem z nią lub na późniejszym etapie, jeśli uznają, że są użyteczne.</span><span class="sxs-lookup"><span data-stu-id="45529-166">These are variations or additions to the core proposal that may be considered in conjunction with it, or at a later stage if deemed useful.</span></span>

### <a name="primary-constructor-bodies"></a><span data-ttu-id="45529-167">Podstawowe treści konstruktora</span><span class="sxs-lookup"><span data-stu-id="45529-167">Primary constructor bodies</span></span>

<span data-ttu-id="45529-168">Konstruktory często zawierają logikę sprawdzania poprawności parametrów lub inny nieuproszczony kod inicjujący, który nie może być wyrażony jako inicjatory.</span><span class="sxs-lookup"><span data-stu-id="45529-168">Constructors themselves often contain parameter validation logic or other nontrivial initialization code that cannot be expressed as initializers.</span></span>

<span data-ttu-id="45529-169">Konstruktory podstawowe można rozszerzyć, aby umożliwić umieszczanie bloków instrukcji bezpośrednio w treści klasy.</span><span class="sxs-lookup"><span data-stu-id="45529-169">Primary constructors could be extended to allow statement blocks to appear directly in the class body.</span></span> <span data-ttu-id="45529-170">Te instrukcje zostaną wstawione w wygenerowanym konstruktorze w punkcie, w którym pojawiają się między inicjalizacją przypisania i dlatego zostałyby wykonane z inicjatorami.</span><span class="sxs-lookup"><span data-stu-id="45529-170">Those statements would be inserted in the generated constructor at the point where they appear between initializing assignments, and would thus be executed interspersed with initializers.</span></span> <span data-ttu-id="45529-171">Przykład:</span><span class="sxs-lookup"><span data-stu-id="45529-171">For instance:</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a><span data-ttu-id="45529-172">Pola inicjatora i funkcje inicjatora</span><span class="sxs-lookup"><span data-stu-id="45529-172">Initializer fields and initializer functions</span></span>

<span data-ttu-id="45529-173">W klasie z konstruktorem podstawowym możemy rozważyć użycie deklaracji pola i metody bez modyfikatorów dostępności, aby były bardziej podobne do zmiennych lokalnych i funkcji lokalnych:</span><span class="sxs-lookup"><span data-stu-id="45529-173">In a class with a primary constructor we could consider field and method declarations without accessibility modifiers to be more like local variables and local functions:</span></span>

* <span data-ttu-id="45529-174">Podobnie jak w przypadku parametrów konstruktora podstawowego "pola inicjatora" byłyby przechwytywane tylko do rzeczywistego pola prywatnego, jeśli były używane w elementach członkowskich funkcji.</span><span class="sxs-lookup"><span data-stu-id="45529-174">Just like primary constructor parameters the "initializer fields" would only be captured into an actual private field if they were used in function members.</span></span>
* <span data-ttu-id="45529-175">"Funkcje inicjatora" będą brane pod uwagę tylko w przypadku przechwytywania parametrów konstruktora podstawowego i pól inicjatora, jeśli były używane w innych elementach członkowskich funkcji.</span><span class="sxs-lookup"><span data-stu-id="45529-175">The "initializer functions" would only be considered to capture primary constructor parameters and initializer fields if they were themselves used in other function members.</span></span> <span data-ttu-id="45529-176">Jeśli nie przechwycono, mogą one być generowane w sposób bardziej optymalny, taki jak funkcje lokalne.</span><span class="sxs-lookup"><span data-stu-id="45529-176">If not captured, they could be generated in a more optimal fashion, like local functions.</span></span>
* <span data-ttu-id="45529-177">Podobnie jak parametry konstruktora podstawowego nie są dostępne za pośrednictwem dostępu do elementów członkowskich, ale tylko jako nazwa prosta.</span><span class="sxs-lookup"><span data-stu-id="45529-177">Just like primary constructor parameters they would not be available via member access, but only as a simple name.</span></span>

<span data-ttu-id="45529-178">Ta funkcja może być używana w przypadku zmiennych tymczasowych i funkcji pomocnika, które mają zastosowanie tylko do inicjowania:</span><span class="sxs-lookup"><span data-stu-id="45529-178">This could be used for temporary variables and helper functions that are only relevant to initialization:</span></span>

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

<span data-ttu-id="45529-179">Może być zbyt delikatny, szczególnie ponieważ brak modyfikatorów dostępności w innym miejscu oznacza `private`.</span><span class="sxs-lookup"><span data-stu-id="45529-179">This may be too subtle, especially since the absence of accessibility modifiers elsewhere simply means `private`.</span></span> 

### <a name="initializer-statements"></a><span data-ttu-id="45529-180">Instrukcje inicjatora</span><span class="sxs-lookup"><span data-stu-id="45529-180">Initializer statements</span></span>

<span data-ttu-id="45529-181">Główną kombinacją powyższych elementów do rozszerzeń byłoby po prostu umożliwienie instrukcji bezpośrednio w treści klasy.</span><span class="sxs-lookup"><span data-stu-id="45529-181">A radical combination of the above to extensions would be to simply allow statements directly in the class body.</span></span> <span data-ttu-id="45529-182">Takie instrukcje są dokładnie takie jak odłożone treści konstruktorów proponowane powyżej, z tym wyjątkiem, że nie muszą być ujęte w `{ }`.</span><span class="sxs-lookup"><span data-stu-id="45529-182">Such statements are exactly as the interspersed constructor bodies proposed above, except they don't need to be enclosed in `{ }`.</span></span> <span data-ttu-id="45529-183">Aby to było dostatecznie przydatne, zmienne "Local" i funkcje pomocnika również muszą być można wyrazić elementu na najwyższym poziomie klasy, w sposób opisany w rozszerzeniu "pola inicjatora i funkcje inicjatora" powyżej.</span><span class="sxs-lookup"><span data-stu-id="45529-183">For this to be sufficiently useful, "local" variables and helper functions would need to also be expressible at the top level of the class, in the manner explored in the extension "Initializer fields and initializer functions" above.</span></span>


### <a name="member-access"></a><span data-ttu-id="45529-184">Dostęp do elementu członkowskiego</span><span class="sxs-lookup"><span data-stu-id="45529-184">Member access</span></span>

<span data-ttu-id="45529-185">Podstawowa propozycja traktuje podstawowe parametry konstruktora jako parametry, które można określać tylko jako proste nazwy.</span><span class="sxs-lookup"><span data-stu-id="45529-185">The core proposal treats primary constructor parameters as parameters that can only be referred as simple names.</span></span> <span data-ttu-id="45529-186">Alternatywą jest umożliwienie odwoływania się do nich tak, jakby były polami prywatnymi, tj. z dostępem do elementu członkowskiego, *nawet* Jeśli nie są czasami generowane jako pola.</span><span class="sxs-lookup"><span data-stu-id="45529-186">An alternative is to allow them to be referenced as if they were private fields, i.e. with a member access, *even* if they are sometimes not generated as fields.</span></span> <span data-ttu-id="45529-187">Umożliwi to odwoływanie się do nich jako `this.x`, gdy są one cieniowane przez zmienne lokalne i dostępne z innego wystąpienia jako `other.x`.</span><span class="sxs-lookup"><span data-stu-id="45529-187">This would allow them to be referenced as `this.x` when shadowed by local variables, and accessed from a different instance as `other.x`.</span></span>

<span data-ttu-id="45529-188">Jeśli zastosowano do rozszerzenia "pola inicjatora i funkcje inicjatora", to również zmniejszy stopień, w jakim różnią się od zwykłych prywatnych członków.</span><span class="sxs-lookup"><span data-stu-id="45529-188">If applied to the "initializer fields and initializer functions" extension this would also reduce the degree to which those were different from ordinary private members.</span></span> <span data-ttu-id="45529-189">Jedyną różnicą jest to, że kompilator jest bezpłatny, aby Elide je z obiektu, jeśli jest używany tylko podczas inicjacji.</span><span class="sxs-lookup"><span data-stu-id="45529-189">The only difference would then be that the compiler is free to elide them from the object if only used during initialization.</span></span>

