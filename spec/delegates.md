---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704098"
---
# <a name="delegates"></a><span data-ttu-id="12123-101">Delegaty</span><span class="sxs-lookup"><span data-stu-id="12123-101">Delegates</span></span>

<span data-ttu-id="12123-102">Delegaty umożliwiają scenariusze, które inne języki — C++takie jak Pascal i moduł, są rozkierowane przy użyciu wskaźników funkcji.</span><span class="sxs-lookup"><span data-stu-id="12123-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="12123-103">W przeciwieństwie C++ do wskaźników funkcji Delegaty są w pełni zorientowane obiektowo i w C++ przeciwieństwie do wskaźników do funkcji składowych, Delegaty hermetyzują zarówno wystąpienie obiektu, jak i metodę.</span><span class="sxs-lookup"><span data-stu-id="12123-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="12123-104">Deklaracja delegata definiuje klasę, która pochodzi od klasy `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="12123-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="12123-105">Wystąpienie delegata hermetyzuje listę wywołań, która jest listą jednej lub więcej metod, z których każdy jest określany jako możliwy do wywołania.</span><span class="sxs-lookup"><span data-stu-id="12123-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="12123-106">Dla metod instancji obiekt, który można wywołać, składa się z wystąpienia i metody dla tego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="12123-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="12123-107">Dla metod statycznych obiekt, który można wywołać, składa się tylko z metody.</span><span class="sxs-lookup"><span data-stu-id="12123-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="12123-108">Wywoływanie wystąpienia delegata z odpowiednim zestawem argumentów powoduje, że każdy możliwy do wywołania obiekt delegowany jest wywoływany z danym zestawem argumentów.</span><span class="sxs-lookup"><span data-stu-id="12123-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="12123-109">Interesująca i przydatna właściwość wystąpienia delegata polega na tym, że nie wie ani nie chroni klasy metod, które są hermetyzowane; wszystkie te kwestie polegają na tym, że te metody są zgodne ([deklaracje delegatów](delegates.md#delegate-declarations)) z typem delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="12123-110">Dzięki temu Delegaty doskonale nadają się do wywołania "anonimowe".</span><span class="sxs-lookup"><span data-stu-id="12123-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="12123-111">Deklaracje delegatów</span><span class="sxs-lookup"><span data-stu-id="12123-111">Delegate declarations</span></span>

<span data-ttu-id="12123-112">*Delegate_declaration* to *type_declaration* ([deklaracje typu](namespaces.md#type-declarations)), które deklaruje nowy typ delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

<span data-ttu-id="12123-113">Jest to błąd czasu kompilacji dla tego samego modyfikatora do wyświetlenia wiele razy w deklaracji delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="12123-114">Modyfikator `new` jest dozwolony tylko w delegatach zadeklarowanych w innym typie. w takim przypadku określa, że taki delegat ukrywa dziedziczonego elementu członkowskiego o tej samej nazwie, zgodnie z opisem w [nowym modyfikatorze](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="12123-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="12123-115">Modyfikatory `public`, `protected`, `internal` i `private` kontrolują dostępność typu delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="12123-116">W zależności od kontekstu, w którym występuje deklaracja delegata, niektóre z tych modyfikatorów mogą nie być dozwolone ([zadeklarowane ułatwienia dostępu](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="12123-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="12123-117">Nazwa typu delegata to *Identyfikator*.</span><span class="sxs-lookup"><span data-stu-id="12123-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="12123-118">Opcjonalne *formal_parameter_list* określa parametry delegata, a *return_type* wskazuje zwracany typ delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="12123-119">Opcjonalne *variant_type_parameter_list* ([listy parametrów typu Variant](interfaces.md#variant-type-parameter-lists)) określa parametry typu dla samego delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="12123-120">Typem zwracanym typu delegata musi być `void` lub Output-Safe ([bezpieczeństwo wariancji](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="12123-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="12123-121">Wszystkie typy parametrów formalnych typu delegata muszą być bezpieczne dla danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="12123-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="12123-122">Ponadto wszystkie typy parametrów `out` lub `ref` muszą być również bezpieczne dla danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="12123-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="12123-123">Należy zauważyć, że nawet `out` parametry muszą być bezpieczne dla danych wejściowych z powodu ograniczenia podstawowej platformy wykonywania.</span><span class="sxs-lookup"><span data-stu-id="12123-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="12123-124">Typy delegatów C# w programie są odpowiednikami nazw, a nie są odpowiednikami strukturalnymi.</span><span class="sxs-lookup"><span data-stu-id="12123-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="12123-125">W każdym przypadku dwa różne typy delegatów, które mają te same listy parametrów i typ zwracany, są traktowane jako różne typy delegatów.</span><span class="sxs-lookup"><span data-stu-id="12123-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="12123-126">Jednak wystąpienia dwóch różnych, ale strukturalnie równoważnych typów delegatów mogą być porównywane jako równe ([delegatów równości](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="12123-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="12123-127">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="12123-127">For example:</span></span>

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

<span data-ttu-id="12123-128">Metody `A.M1` i `B.M1` są zgodne z typami delegatów `D1` i `D2`, ponieważ mają ten sam typ zwracany i listę parametrów; Jednak te typy delegatów są dwa różne typy, więc nie są zamienne.</span><span class="sxs-lookup"><span data-stu-id="12123-128">The methods `A.M1` and `B.M1` are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="12123-129">Metody `B.M2`, `B.M3` i `B.M4` są niezgodne z typami delegatów `D1` i `D2`, ponieważ mają różne typy zwracane lub listy parametrów.</span><span class="sxs-lookup"><span data-stu-id="12123-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="12123-130">Podobnie jak w przypadku innych deklaracji typu ogólnego, należy pomieścić argumenty typu, aby utworzyć skonstruowany typ delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="12123-131">Typy parametrów i typ zwracany skonstruowanego typu delegata są tworzone przez podstawianie, dla każdego parametru typu w deklaracji delegata, odpowiadającego mu argumentu typu złożonego typu delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="12123-132">Wynikowy typ zwracany i typy parametrów są używane w celu określenia, jakie metody są zgodne ze skonstruowanym typem delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="12123-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="12123-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="12123-134">Metoda `X.F` jest zgodna z typem delegata `Predicate<int>` i metodą `X.G` jest zgodna z typem delegata `Predicate<string>`.</span><span class="sxs-lookup"><span data-stu-id="12123-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="12123-135">Jedynym sposobem deklarowania typu delegata jest za pośrednictwem *delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="12123-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="12123-136">Typ delegata jest typem klasy, który jest pochodną `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="12123-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="12123-137">Typy delegatów są niejawnie `sealed`, więc nie jest dozwolone wyprowadzanie dowolnego typu z typu delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="12123-138">Nie jest również dozwolone uzyskanie typu klasy niedelegowanej z `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="12123-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="12123-139">Należy zauważyć, że `System.Delegate` nie należy do typu delegata; jest to typ klasy, z której są wyprowadzane wszystkie typy delegatów.</span><span class="sxs-lookup"><span data-stu-id="12123-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="12123-140">C#zawiera specjalną składnię dla tworzenia wystąpień delegata i wywołania.</span><span class="sxs-lookup"><span data-stu-id="12123-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="12123-141">Oprócz tworzenia wystąpienia, każda operacja, którą można zastosować do klasy lub wystąpienia klasy, można również zastosować odpowiednio do klasy lub wystąpienia obiektu delegowanego.</span><span class="sxs-lookup"><span data-stu-id="12123-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="12123-142">W szczególności możliwe jest uzyskanie dostępu do elementów członkowskich typu `System.Delegate` za pośrednictwem standardowej składni dostępu do składowej.</span><span class="sxs-lookup"><span data-stu-id="12123-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="12123-143">Zestaw metod, które są hermetyzowane przez wystąpienie delegata, nazywa się listą wywołania.</span><span class="sxs-lookup"><span data-stu-id="12123-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="12123-144">Po utworzeniu wystąpienia delegata ([delegowanie zgodności](delegates.md#delegate-compatibility)) z pojedynczej metody hermetyzuje tę metodę, a jej Lista wywołań zawiera tylko jeden wpis.</span><span class="sxs-lookup"><span data-stu-id="12123-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="12123-145">Jednak gdy dwa wystąpienia delegatów o wartości innej niż null są łączone, ich listy wywołań są łączone — w pozostałym operandzie Order Right a prawy operand — aby utworzyć nową listę wywołań, która zawiera co najmniej dwa wpisy.</span><span class="sxs-lookup"><span data-stu-id="12123-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="12123-146">Delegaty są łączone za pomocą @no__t binarnych-0 ([operator dodawania](expressions.md#addition-operator)) i operatory `+=` ([przypisanie złożone](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="12123-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="12123-147">Delegat może zostać usunięty z kombinacji delegatów, przy użyciu binarnego `-` ([operator odejmowania](expressions.md#subtraction-operator)) i operatory `-=` ([przypisanie złożone](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="12123-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="12123-148">Delegatów można porównywać pod kątem równości ([delegatów kryterium równości](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="12123-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="12123-149">Poniższy przykład pokazuje wystąpienie liczby delegatów i odpowiadające im listy wywołań:</span><span class="sxs-lookup"><span data-stu-id="12123-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

<span data-ttu-id="12123-150">Gdy jest tworzone wystąpienie `cd1` i `cd2`, każdy hermetyzuje jedną metodę.</span><span class="sxs-lookup"><span data-stu-id="12123-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="12123-151">Gdy zostanie utworzone wystąpienie `cd3`, zawiera on listę wywołań dwóch metod, `M1` i `M2` w tej kolejności.</span><span class="sxs-lookup"><span data-stu-id="12123-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="12123-152">Lista wywołań `cd4` zawiera `M1`, `M2` i `M1` w tej kolejności.</span><span class="sxs-lookup"><span data-stu-id="12123-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="12123-153">Na koniec Lista wywołań `cd5` zawiera `M1`, `M2`, `M1`, `M1` i `M2` w tej kolejności.</span><span class="sxs-lookup"><span data-stu-id="12123-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="12123-154">Aby uzyskać więcej przykładów łączenia (jak również usuwania) delegatów, zobacz [delegating wywołania](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="12123-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="12123-155">Delegowanie zgodności</span><span class="sxs-lookup"><span data-stu-id="12123-155">Delegate compatibility</span></span>

<span data-ttu-id="12123-156">Metoda lub delegat `M` jest ***zgodny*** z typem delegata `D`, jeśli spełnione są wszystkie następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="12123-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="12123-157">`D` i `M` mają tę samą liczbę parametrów, a każdy parametr w `D` ma te same Modyfikatory `ref` lub `out` jako odpowiedni parametr w `M`.</span><span class="sxs-lookup"><span data-stu-id="12123-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="12123-158">Dla każdego parametru wartości (parametru bez modyfikatora `ref` lub `out`), konwersja tożsamości ([konwersja tożsamości](conversions.md#identity-conversion)) lub niejawna konwersja odwołania ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions)) istnieje z typu parametru w `D` do odpowiedni typ parametru w `M`.</span><span class="sxs-lookup"><span data-stu-id="12123-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="12123-159">Dla każdego parametru `ref` lub `out` typ parametru w `D` jest taki sam jak typ parametru w `M`.</span><span class="sxs-lookup"><span data-stu-id="12123-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="12123-160">Dla zwracanego typu `D` @no__t istnieje konwersja niejawnego odwołania.</span><span class="sxs-lookup"><span data-stu-id="12123-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="12123-161">Tworzenie wystąpienia delegata</span><span class="sxs-lookup"><span data-stu-id="12123-161">Delegate instantiation</span></span>

<span data-ttu-id="12123-162">Wystąpienie delegata jest tworzone przez *delegate_creation_expression* ([wyrażenia tworzenia delegatów](expressions.md#delegate-creation-expressions)) lub konwersję do typu delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="12123-163">Nowo utworzone wystąpienie delegata odnosi się do:</span><span class="sxs-lookup"><span data-stu-id="12123-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="12123-164">Metoda statyczna, do której odwołuje się *delegate_creation_expression*lub</span><span class="sxs-lookup"><span data-stu-id="12123-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="12123-165">Obiekt docelowy (który nie może być `null`) i metoda wystąpienia, do której odwołuje się *delegate_creation_expression*, lub</span><span class="sxs-lookup"><span data-stu-id="12123-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="12123-166">Inny delegat.</span><span class="sxs-lookup"><span data-stu-id="12123-166">Another delegate.</span></span>

<span data-ttu-id="12123-167">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="12123-167">For example:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

<span data-ttu-id="12123-168">Po utworzeniu wystąpienia obiekty delegatów zawsze odwołują się do tego samego obiektu docelowego i metody.</span><span class="sxs-lookup"><span data-stu-id="12123-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="12123-169">Pamiętaj, że gdy dwa Delegaty są łączone lub jeden z nich jest usuwany z innego, nowy delegat ma własną listę wywołań; listy wywołań delegatów połączonych lub usuniętych pozostaną niezmienione.</span><span class="sxs-lookup"><span data-stu-id="12123-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="12123-170">Delegowanie wywołania</span><span class="sxs-lookup"><span data-stu-id="12123-170">Delegate invocation</span></span>

<span data-ttu-id="12123-171">C#zawiera specjalną składnię do wywoływania delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="12123-172">Gdy wystąpienie delegata o wartości innej niż null, którego lista wywołań zawiera jeden wpis, wywołuje jedną metodę z tymi samymi argumentami i zwraca tę samą wartość, co odwołanie do metody.</span><span class="sxs-lookup"><span data-stu-id="12123-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="12123-173">(Zobacz [delegowanie wywołań](expressions.md#delegate-invocations) , aby uzyskać szczegółowe informacje na temat delegowania wywołania.) Jeśli wystąpi wyjątek w trakcie wywołania tego delegata, a ten wyjątek nie jest przechwytywany w metodzie, która została wywołana, wyszukiwanie w klauzuli catch wyjątku kontynuuje się w metodzie, która wywołała delegata, tak jakby ta metoda była bezpośrednio wywołana Metoda, do której odwołuje się ten delegat.</span><span class="sxs-lookup"><span data-stu-id="12123-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="12123-174">Wywołanie wystąpienia delegata, którego lista wywołania zawiera wiele wpisów, jest realizowane przez wywołanie każdej metody z listy wywołań synchronicznie, w kolejności.</span><span class="sxs-lookup"><span data-stu-id="12123-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="12123-175">Każda metoda wywołana jest przenoszona do tego samego zestawu argumentów, co zostało przekazane do wystąpienia delegata.</span><span class="sxs-lookup"><span data-stu-id="12123-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="12123-176">Jeśli takie wywołanie delegata zawiera parametry odwołania ([parametry odwołania](classes.md#reference-parameters)), każde wywołanie metody zostanie wykonane z odwołaniem do tej samej zmiennej; zmiany tej zmiennej według jednej metody na liście wywołań będą widoczne dla metod w dalszej postaci listy wywołań.</span><span class="sxs-lookup"><span data-stu-id="12123-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="12123-177">Jeśli delegat delegowany zawiera parametry wyjściowe lub wartość zwracaną, ich końcowa wartość będzie pochodzić z wywołania ostatniego delegata na liście.</span><span class="sxs-lookup"><span data-stu-id="12123-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="12123-178">Jeśli wystąpi wyjątek podczas przetwarzania wywołania tego delegata, a ten wyjątek nie jest przechwytywany w metodzie, która została wywołana, wyszukiwanie w klauzuli catch wyjątku kontynuuje się w metodzie, która wywołała delegata, i inne inne metody Lista wywołań nie została wywołana.</span><span class="sxs-lookup"><span data-stu-id="12123-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="12123-179">Próba wywołania wystąpienia delegata, którego wartość jest równa null, spowoduje wyjątek typu `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="12123-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="12123-180">Poniższy przykład pokazuje, jak tworzyć wystąpienia, łączyć, usuwać i wywoływać delegatów:</span><span class="sxs-lookup"><span data-stu-id="12123-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

<span data-ttu-id="12123-181">Jak pokazano w instrukcji `cd3 += cd1;`, delegat może być obecny na liście wywołań wiele razy.</span><span class="sxs-lookup"><span data-stu-id="12123-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="12123-182">W tym przypadku jest to po prostu wywoływana raz na wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="12123-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="12123-183">Na liście wywołań, takiej jak w przypadku usunięcia tego delegata, ostatnie wystąpienie na liście wywołań jest aktualnie usunięte.</span><span class="sxs-lookup"><span data-stu-id="12123-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="12123-184">Bezpośrednio przed wykonaniem ostatecznej instrukcji `cd3 -= cd1;`, delegat `cd3` odwołuje się do pustej listy wywołań.</span><span class="sxs-lookup"><span data-stu-id="12123-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="12123-185">Próba usunięcia delegata z pustej listy (lub usunięcia nieistniejącej delegata z listy niepustej) nie jest błędem.</span><span class="sxs-lookup"><span data-stu-id="12123-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="12123-186">Tworzone dane wyjściowe to:</span><span class="sxs-lookup"><span data-stu-id="12123-186">The output produced is:</span></span>

```console
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
