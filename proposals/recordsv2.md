---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2019
ms.locfileid: "79485001"
---

# <a name="records-v2"></a><span data-ttu-id="03e94-101">Rekordy w wersji 2</span><span class="sxs-lookup"><span data-stu-id="03e94-101">Records v2</span></span>

<span data-ttu-id="03e94-102">W przeszłości zawarto informacje dotyczące rekordów jako funkcję umożliwiającą pracę z danymi.</span><span class="sxs-lookup"><span data-stu-id="03e94-102">In the past we've thought about records as a feature to enable working with data.</span></span>

<span data-ttu-id="03e94-103">"Praca z danymi" jest obszerną grupą z szeregiem aspektów, dzięki czemu może być interesująca, aby zapoznać się z każdą z nich.</span><span class="sxs-lookup"><span data-stu-id="03e94-103">"Working with data" is a big group with a number of facets, so it may be interesting to look at each in isolation.</span></span> <span data-ttu-id="03e94-104">Zacznijmy od przejrzenia przykładu rekordów dzisiaj i niektórych jego wad.</span><span class="sxs-lookup"><span data-stu-id="03e94-104">Let's start by looking at an example of records today and some of its drawbacks.</span></span>

<span data-ttu-id="03e94-105">Na przykład prosty rekord zostanie zdefiniowany dzisiaj w następujący sposób</span><span class="sxs-lookup"><span data-stu-id="03e94-105">For instance, a simple record would be defined today as follows</span></span>

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

<span data-ttu-id="03e94-106">a użycie zostanie odczytane</span><span class="sxs-lookup"><span data-stu-id="03e94-106">and the usage would read</span></span>

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

<span data-ttu-id="03e94-107">Ten kod ma znaczące zalety:</span><span class="sxs-lookup"><span data-stu-id="03e94-107">There are significant advantages in this code:</span></span>

1. <span data-ttu-id="03e94-108">Definicja jest odporna na wersje, ale właściwości można łatwo dodać lub przenieść</span><span class="sxs-lookup"><span data-stu-id="03e94-108">The definition is version resilient, properties can easily be added or moved</span></span>
2. <span data-ttu-id="03e94-109">Właściwości można ustawić w dowolnej kolejności, a nazwy w inicjacji zawsze pasują do metod dostępu</span><span class="sxs-lookup"><span data-stu-id="03e94-109">Properties can be set in any order, and the names in the initialization always match the accessors</span></span>
3. <span data-ttu-id="03e94-110">Właściwości z wartościami domyślnymi można po prostu pominąć</span><span class="sxs-lookup"><span data-stu-id="03e94-110">Properties with default values can simply be skipped</span></span>

<span data-ttu-id="03e94-111">Pierwsza Luka polega na tym, że właściwości muszą być teraz modyfikowalne.</span><span class="sxs-lookup"><span data-stu-id="03e94-111">The first flaw is that the properties must now be mutable.</span></span>

# <a name="mutability"></a><span data-ttu-id="03e94-112">Zmienność</span><span class="sxs-lookup"><span data-stu-id="03e94-112">Mutability</span></span>

<span data-ttu-id="03e94-113">Co chcemy zrobić, C# aby określić element członkowski `readonly` w inicjatorach obiektów.</span><span class="sxs-lookup"><span data-stu-id="03e94-113">What we'd like is for C# to provide a way to set a `readonly` member in object initializers.</span></span>
<span data-ttu-id="03e94-114">Ponieważ niektóre typy mogą nie zostać zaprojektowane z tą inicjalizacją, należy również pamiętać, że jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="03e94-114">Since some types may not have been designed with this initialization in mind, we'd also like it to be opt-in.</span></span>

<span data-ttu-id="03e94-115">Proponowane rozwiązanie jest nowym modyfikatorem, `initonly`, który można zastosować do właściwości i pól:</span><span class="sxs-lookup"><span data-stu-id="03e94-115">The proposed solution is a new modifier, `initonly`, that can be applied to properties and fields:</span></span>

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="03e94-116">Codegen to Surprisingly proste do przodu: ustawimy pole tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="03e94-116">The codegen for this is surprisingly straight forward: we just set the readonly field.</span></span>
<span data-ttu-id="03e94-117">W odróżnieniu od właściwości obniżone będą wyglądać:</span><span class="sxs-lookup"><span data-stu-id="03e94-117">Specifically, the lowered properties would look like:</span></span>

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

<span data-ttu-id="03e94-118">Środowisko CLR uważa, że pola tylko do odczytu nie mogą być możliwe do sprawdzenia, ale nie są niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="03e94-118">The CLR considers setting readonly fields to be unverifiable, but not unsafe.</span></span> <span data-ttu-id="03e94-119">W celu zapewnienia bardziej zaawansowanego weryfikatora proponowana jest następująca reguła: pole tylko do odczytu może być modyfikowane tylko wewnątrz metod `initonly` lub na nowym obiekcie, który znajduje się na stosie CLR i nie został opublikowany za pośrednictwem magazynu lub wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="03e94-119">To support a more advanced verifier, the following rule is proposed: a readonly field can be modified only inside `initonly` methods, or on a new object that is on the CLR stack and has not been published via a store or method call.</span></span>

<span data-ttu-id="03e94-120">Powinno to rozwiązać wiele problemów z zmienność w przykładzie `UserInfo`, bez konieczności wykonywania skomplikowanych lub kruchyych strategii emisji.</span><span class="sxs-lookup"><span data-stu-id="03e94-120">This should solve many of the problems with mutability in the `UserInfo` example, while not requiring complicated or brittle emit strategies.</span></span> <span data-ttu-id="03e94-121">Jednak niezmienności jest obecnym nowym problemem: łatwe Konstruowanie obiektu ze zmianami.</span><span class="sxs-lookup"><span data-stu-id="03e94-121">However, immutability does present a new problem: easily constructing an object with changes.</span></span>

# <a name="with-ing"></a><span data-ttu-id="03e94-122">Z-usługą</span><span class="sxs-lookup"><span data-stu-id="03e94-122">With-ing</span></span>

<span data-ttu-id="03e94-123">Podczas programowania przy użyciu niezmienności wprowadzanie zmian w obiekcie odbywa się przez konstruowanie kopii ze zmianami zamiast wprowadzania zmian bezpośrednio w obiekcie.</span><span class="sxs-lookup"><span data-stu-id="03e94-123">When programming with immutability, making changes to an object is done by constructing a copy with changes instead of making the changes directly on the object.</span></span> <span data-ttu-id="03e94-124">Niestety, nie istnieje wygodny sposób wykonania tej czynności w programie C#, nawet w przypadku rekordów w stylu bieżącym.</span><span class="sxs-lookup"><span data-stu-id="03e94-124">Unfortunately, there's no convenient way to do this in C#, even with current-style records.</span></span> <span data-ttu-id="03e94-125">Wcześniej zaproponowano, aby dla rekordów, które implementują tę funkcję, był dostarczany jakiś rodzaj automatycznie generowanej metody with.</span><span class="sxs-lookup"><span data-stu-id="03e94-125">It's been previously proposed that some kind of autogenerated "With" method be provided for records that implements that functionality.</span></span> <span data-ttu-id="03e94-126">Jeśli mamy taki mechanizm, ważne jest, aby współpracował z `initonly` członkami.</span><span class="sxs-lookup"><span data-stu-id="03e94-126">If we have such a mechanism, it's important that it work with `initonly` members.</span></span> <span data-ttu-id="03e94-127">Aby to osiągnąć, proponowane jest dodanie wyrażenia `with`, analogiczne do inicjatora obiektu.</span><span class="sxs-lookup"><span data-stu-id="03e94-127">To achieve this, it's proposed that we add a `with` expression, analogous to an object initializer.</span></span> <span data-ttu-id="03e94-128">Przykładowe użycie będzie następujące:</span><span class="sxs-lookup"><span data-stu-id="03e94-128">Sample usage would be as follows:</span></span>

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

<span data-ttu-id="03e94-129">Otrzymany `newUserName` obiekt będzie kopią `userInfo`, z `Username`em ustawionym na "angocke".</span><span class="sxs-lookup"><span data-stu-id="03e94-129">The resulting `newUserName` object would be a copy of `userInfo`, with `Username` set to "angocke".</span></span>
<span data-ttu-id="03e94-130">Codegen w wyrażeniu `with` będzie również podobna do inicjatora obiektów: tworzony jest nowy obiekt, a następnie `initonly` `Username` setter zostanie wywołana w treści metody.</span><span class="sxs-lookup"><span data-stu-id="03e94-130">The codegen on the `with` expression would also be similar to the object initializer: a new object is constructed, and then the `initonly` `Username` setter would be called in the method body.</span></span>

<span data-ttu-id="03e94-131">Oczywiście różnica polega na tym, że tworzony nowy obiekt nie jest prostym tworzeniem nowego obiektu, jest duplikatem oryginalnego obiektu.</span><span class="sxs-lookup"><span data-stu-id="03e94-131">Of course, the difference here is that the new object being constructed is not a simple new object creation, it is a duplicate of the original object.</span></span> <span data-ttu-id="03e94-132">Aby zapewnić tę funkcjonalność, wymagane jest, aby obiekt zapewniał "z konstruktorem", który zawiera zduplikowany obiekt.</span><span class="sxs-lookup"><span data-stu-id="03e94-132">To provide this functionality, we require that the object provide a "With constructor" that provides a duplicate object.</span></span> <span data-ttu-id="03e94-133">Przykładowy Konstruktor `With` wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="03e94-133">A sample `With` constructor would look like:</span></span>

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

<span data-ttu-id="03e94-134">W szczególności, wyrażenie `with` ustawi `initonly` elementy członkowskie, podobnie jak Inicjator obiektu, dlatego do obsługi weryfikacji musimy upewnić się, że nie można opublikować obiektu przed ustawieniem elementów członkowskich `initonly`.</span><span class="sxs-lookup"><span data-stu-id="03e94-134">Notably, the `with` expression will set `initonly` members, just like the object initializer, so to support verification we must ensure that the object cannot have been published before the `initonly` members are set.</span></span> <span data-ttu-id="03e94-135">Aby to wymusić, atrybut `WithConstructor` (lub odpowiadająca składnia) wymusza nową regułę dla metody: wszystkie instrukcje Return muszą bezpośrednio zawierać wyrażenie tworzenia obiektu, prawdopodobnie z inicjatorem obiektu.</span><span class="sxs-lookup"><span data-stu-id="03e94-135">To enforce this, the `WithConstructor` attribute (or equivalent syntax) will enforce a new rule for the method: all return statements must directly contain an object creation expression, possibly with an object initializer.</span></span>

<span data-ttu-id="03e94-136">Jeśli Konstruktor `With` wymaga walidacji, użytkownik może wprowadzić konstruktora w celu wykonania tej walidacji, np.</span><span class="sxs-lookup"><span data-stu-id="03e94-136">If the `With` constructor requires validation, the user may introduce a constructor to do that validation, e.g.</span></span>

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

<span data-ttu-id="03e94-137">Ostatnim elementem złożoności skojarzonym z `With` jest dziedziczenie.</span><span class="sxs-lookup"><span data-stu-id="03e94-137">The last piece of complexity associated with `With` is inheritance.</span></span> <span data-ttu-id="03e94-138">Jeśli rekord jest rozszerzalny, należy podać nowy `With` dla podklasy.</span><span class="sxs-lookup"><span data-stu-id="03e94-138">If your record is extensible, you will need to provide a new `With` for the subclass.</span></span> <span data-ttu-id="03e94-139">Można to osiągnąć w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="03e94-139">This can be achieved as follows:</span></span>

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

<span data-ttu-id="03e94-140">Zwróć uwagę na jedną dodatkową część złożoności tutaj: Aby przesłonić Konstruktor `With` z typem pochodnym, język będzie również obsługiwał typy zwracane przez współwarianty w zastosowaniach.</span><span class="sxs-lookup"><span data-stu-id="03e94-140">Note one additional piece of complexity here: in order to override the `With` constructor with the derived type the language will also need to support covariant return types in overrides.</span></span>
<span data-ttu-id="03e94-141">Ta [Funkcja już](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)istnieje.</span><span class="sxs-lookup"><span data-stu-id="03e94-141">There is already a separate proposal for this feature [here](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md).</span></span>

<span data-ttu-id="03e94-142">**Wady**</span><span class="sxs-lookup"><span data-stu-id="03e94-142">**Drawbacks**</span></span>

- <span data-ttu-id="03e94-143">Wprowadzanie wszystkich instrukcji return w `WithConstructor`s zawiera nowe wyrażenia obiektu jest restrykcyjne.</span><span class="sxs-lookup"><span data-stu-id="03e94-143">Making all return statements in `WithConstructor`s contain new object expressions is restrictive.</span></span>
  <span data-ttu-id="03e94-144">Może to być możliwe do skorygowania przez analizę przepływu, która zapewnia, że nowy obiekt nie będzie miał metody</span><span class="sxs-lookup"><span data-stu-id="03e94-144">This could be possibly be mitigated by flow analysis that ensures the new object doesn't escape the method</span></span>
- <span data-ttu-id="03e94-145">Obsługa wariancji w zastąpieniach za pomocą wskazówek kompilatora będzie wymagała metod zastępczych, co zwiększy się z głębokością dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="03e94-145">Supporting variance in overrides through compiler tricks will require stub methods, which will grow quadratically with the inheritance depth.</span></span> <span data-ttu-id="03e94-146">Konieczność metody zastępczej jest spowodowana wymaganiem w czasie wykonywania, które zastępuje sygnatury dokładnie zgodne.</span><span class="sxs-lookup"><span data-stu-id="03e94-146">The need for a stub method is due to a runtime requirement that override signatures match exactly.</span></span> <span data-ttu-id="03e94-147">W przypadku poluzu wymagań środowiska uruchomieniowego metody zastępcze nie będą w ogóle wymagane.</span><span class="sxs-lookup"><span data-stu-id="03e94-147">If the runtime requirement were loosened, the stub methods would not be required at all.</span></span>
- <span data-ttu-id="03e94-148">Używanie łańcuchowych konstruktorów formularza `Type(Type original)` efektywnie rezerwuje ten konstruktor do użycia wzorca.</span><span class="sxs-lookup"><span data-stu-id="03e94-148">Using chained constructors of the form `Type(Type original)` effectively reserves that constructor for the use of the pattern.</span></span> <span data-ttu-id="03e94-149">Ponieważ konstruktory mają unikatową semantykę i nie można jej zmienić nazwy, może to być ograniczone.</span><span class="sxs-lookup"><span data-stu-id="03e94-149">Since constructors have unique semantics and cannot be re-named this could be limiting.</span></span>


## <a name="wrapping-it-all-up-records"></a><span data-ttu-id="03e94-150">Zawijaj wszystko: rekordy</span><span class="sxs-lookup"><span data-stu-id="03e94-150">Wrapping it all up: Records</span></span>

<span data-ttu-id="03e94-151">Powyższe funkcje umożliwiają zastosowanie stylu programowania, który był bardzo trudny.</span><span class="sxs-lookup"><span data-stu-id="03e94-151">The above features enable a style of programming that was very difficult before.</span></span> <span data-ttu-id="03e94-152">Jednak nawet w przypadku nowych funkcji może być całkiem pełny i podatne na dodawanie adnotacji do wszystkiego.</span><span class="sxs-lookup"><span data-stu-id="03e94-152">But even with the new features it could be quite verbose and error prone to annotate everything yourself.</span></span> <span data-ttu-id="03e94-153">Istnieje również kilka elementów, takich jak Equals i GetHashCode, które można już dzisiaj napisać, to właśnie pracochłonne.</span><span class="sxs-lookup"><span data-stu-id="03e94-153">There are also a few items, like Equals and GetHashCode, which can already be written today, it's just laborious.</span></span>
<span data-ttu-id="03e94-154">Ponadto znaczący wpływ na implementację tych nowych elementów pierwotnych polega na tym, że strukturalna równość jest taka, która powinna być zmieniana wraz z typem danych w miarę dodawania nowych danych, ale podczas ich obsługi ręcznie jest możliwe, że te rzeczy będą mogły zostać zsynchronizowane.</span><span class="sxs-lookup"><span data-stu-id="03e94-154">Moreover, a significant flaw in implementing equality on top of these new primitives is that structural equality is something that should change with your data type as new data is added, but when handling it manually it is likely that these things can get out of sync.</span></span>

<span data-ttu-id="03e94-155">W związku z tym jest proponowana C# obsługa nowej składni dla rekordów, a nie do udostępniania nowych funkcji, ale do ustawiania ustawień domyślnych i generowania kodu przeznaczonego do użycia w rekordach.</span><span class="sxs-lookup"><span data-stu-id="03e94-155">Therefore, it is proposed that C# support new syntax for records, not for providing new features, but for setting defaults and generating code designed for use in records.</span></span> <span data-ttu-id="03e94-156">Przykładowa składnia będzie wyglądać następująco</span><span class="sxs-lookup"><span data-stu-id="03e94-156">Example syntax would look like</span></span>

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="03e94-157">Wygenerowany kod dla tej klasy będzie uwzględniał wszystkie pola publiczne i właściwości automatycznie jako strukturalne elementy członkowskie rekordu.</span><span class="sxs-lookup"><span data-stu-id="03e94-157">The generated code for this class would regard all public fields and auto-properties as structural members of the record.</span></span> <span data-ttu-id="03e94-158">Elementy członkowskie rekordu można dostosować przy użyciu nowego atrybutu `RecordMember(bool)`, który może być używany do dołączania lub wykluczania elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="03e94-158">Record members could be customized using a new `RecordMember(bool)` attribute that could be used to either include or exclude members.</span></span> <span data-ttu-id="03e94-159">Elementy członkowskie rekordu będą `initonly` domyślnie i równość zostałyby wygenerowane automatycznie dla klasy na podstawie rekordów elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="03e94-159">Record members would be `initonly` by default and equality would be autogenerated for the class based on the record members.</span></span> <span data-ttu-id="03e94-160">W każdym momencie zachowanie tych elementów członkowskich może być dostosowane po prostu przez zadeklarowanie ich w źródle.</span><span class="sxs-lookup"><span data-stu-id="03e94-160">At any point the behavior of these members could be customized simply by declaring them in source.</span></span> <span data-ttu-id="03e94-161">Implementacja oparta na użytkowniku zastąpi domyślną implementację we wszystkich użyciach wzorców.</span><span class="sxs-lookup"><span data-stu-id="03e94-161">The user-written implementation would replace the default implementation in all pattern usage.</span></span>

<span data-ttu-id="03e94-162">Należy pamiętać, że równość w przypadku dziedziczenia jest złożona, ale wydaje się, że zostały one odpowiednio rozwiązane w [innych propozycjach rekordów](records.md).</span><span class="sxs-lookup"><span data-stu-id="03e94-162">Note that equality in the face of inheritance is complex, but seems to have been adequately solved in the [other records proposal](records.md).</span></span>

## <a name="primary-constructors"></a><span data-ttu-id="03e94-163">Konstruktory podstawowe</span><span class="sxs-lookup"><span data-stu-id="03e94-163">Primary constructors</span></span>

<span data-ttu-id="03e94-164">Poprzednia pozycja rekordu zawierała również nową składnię dla listy parametrów w samym typie, np.</span><span class="sxs-lookup"><span data-stu-id="03e94-164">Previous record proposal have also included a new syntax for a parameter list on the type itself, e.g.</span></span>

```C#
class Point(int X, int Y);
```

<span data-ttu-id="03e94-165">W nowym projekcie lista parametrów będzie funkcją ortogonalną C# , którą można czysto zintegrować z rekordami.</span><span class="sxs-lookup"><span data-stu-id="03e94-165">In the new design, the parameter list would be an orthogonal C# feature, which could be cleanly integrated with records.</span></span> <span data-ttu-id="03e94-166">Jeśli w rekordzie znajduje się Konstruktor podstawowy, będzie on miał nowe wartości domyślne, podobnie jak pola publiczne i właściwości automatyczne: parametry w konstruktorze podstawowym będą używane do generowania właściwości publicznego rekordu-elementu członkowskiego o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="03e94-166">If a primary constructor is included in a record, it would have new defaults, just like public fields and auto-properties: the parameters in the primary constructor would be used to generate public record-member properties with the same name.</span></span> <span data-ttu-id="03e94-167">Ponadto Konstruktor podstawowy może być teraz używany do generowania automatycznie dekonstruktora.</span><span class="sxs-lookup"><span data-stu-id="03e94-167">In addition, the primary constructor could now be used to auto-generate a deconstructor.</span></span>

<span data-ttu-id="03e94-168">Na przykład następujący rekord z konstruktorem podstawowym</span><span class="sxs-lookup"><span data-stu-id="03e94-168">For example, the following record with a primary constructor</span></span>

```C#
data class Point(int X, int Y);
```

<span data-ttu-id="03e94-169">będzie równoważne</span><span class="sxs-lookup"><span data-stu-id="03e94-169">would be equivalent to</span></span>

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

<span data-ttu-id="03e94-170">a końcowe generowanie powyższego poziomu byłyby</span><span class="sxs-lookup"><span data-stu-id="03e94-170">and the final generation of the above would be</span></span>

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

<span data-ttu-id="03e94-171">Należy zwrócić uwagę na to, że w przypadku klasy danych z konstruktorem podstawowym została pobrana jedna inna informacja na temat: zamiast ustawiania pól podstawowych wewnątrz wygenerowanego konstruktora chronionego, delegat do konstruktora podstawowego.</span><span class="sxs-lookup"><span data-stu-id="03e94-171">Note that we've taken one other piece of information into account for a data class with a primary constructor: instead of setting the primary fields inside the generated protected constructor, we delegate to the primary constructor.</span></span> <span data-ttu-id="03e94-172">Jeśli Klasa Point ma inny niepodstawowy element członkowski rekordu, np.</span><span class="sxs-lookup"><span data-stu-id="03e94-172">If the Point class had another non-primary record member, e.g.</span></span>

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

<span data-ttu-id="03e94-173">następnie spowoduje to zmianę wygenerowanego chronionego konstruktora w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="03e94-173">then that would change the generated protected constructor as follows:</span></span>

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

<span data-ttu-id="03e94-174">W szczególności nie odpowiada to na temat dziedziczenia rekordów za pomocą konstruktorów podstawowych.</span><span class="sxs-lookup"><span data-stu-id="03e94-174">Notably, this doesn't answer what to do about inheritance of records with primary constructors.</span></span> <span data-ttu-id="03e94-175">Na przykład,</span><span class="sxs-lookup"><span data-stu-id="03e94-175">For instance,</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

<span data-ttu-id="03e94-176">Zamiast rozwiązywania w dowolny sposób, bardziej jawne podejście może wymagać, aby lista parametrów była dostarczana z listą podstawową, np.</span><span class="sxs-lookup"><span data-stu-id="03e94-176">Rather than resolving in an arbitrary manner, a more explicit approach could require that a parameter list be provided with the base list, e.g.</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

<span data-ttu-id="03e94-177">Lista parametrów na liście podstawowej zostałaby następnie zastosowana do wywołania `base` w wygenerowanym konstruktorze głównym:</span><span class="sxs-lookup"><span data-stu-id="03e94-177">The parameter list in the base list would then be applied to a `base` call in the generated primary constructor:</span></span>

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

<span data-ttu-id="03e94-178">Tak jak w przypadku konstruktora podstawowego może oznaczać poza rekordem, który jest nadal otwarty do dalszej propozycji.</span><span class="sxs-lookup"><span data-stu-id="03e94-178">As for what a primary constructor could mean outside of a record, that is still open to further proposal.</span></span>
