---
ms.openlocfilehash: 994b22f5375d57cfc4c7537c64345a27ddf3e416
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193879"
---
# <a name="delegates"></a><span data-ttu-id="ce272-101">Delegaty</span><span class="sxs-lookup"><span data-stu-id="ce272-101">Delegates</span></span>

<span data-ttu-id="ce272-102">Delegatów realizację scenariuszy, że inne języki — takich jak C++, Pascal i Modula — mają adresowane za pomocą wskaźników funkcji.</span><span class="sxs-lookup"><span data-stu-id="ce272-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="ce272-103">W przeciwieństwie do wskaźników funkcji języka C++ należy jednak obiekty delegowane są w pełni zorientowana obiektowo i w przeciwieństwie do języka C++ wskaźników do składowych, delegatów hermetyzacji zarówno w przypadku wystąpienia obiektu, jak i metody.</span><span class="sxs-lookup"><span data-stu-id="ce272-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="ce272-104">Deklaracja delegata definiuje klasę, która jest pochodną klasy `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="ce272-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="ce272-105">Wystąpienie delegata hermetyzuje listę wywołań, który znajduje się lista jedną lub więcej metod, z których każdy jest określany jako element możliwy do wywołania.</span><span class="sxs-lookup"><span data-stu-id="ce272-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="ce272-106">Dla wystąpienia metod, wywoływane jednostki składa się z wystąpieniem, a metoda, w tym wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="ce272-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="ce272-107">Obiekt możliwy do wywołania metody statyczne, składa się z tylko metody.</span><span class="sxs-lookup"><span data-stu-id="ce272-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="ce272-108">Wywoływanie wystąpienie delegata, za pomocą odpowiedniego zestawu argumentów powoduje, że każdy możliwy do wywołania jednostek pełnomocnika do wywołania z danego zestawu argumentów.</span><span class="sxs-lookup"><span data-stu-id="ce272-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="ce272-109">Interesujące i przydatne własności wystąpienie delegata jest, nie wiedzieć i nie interesuje klas, metod, który hermetyzuje; wszystko, co dla Ciebie ważne jest, tych metod zgodne ([delegować deklaracje](delegates.md#delegate-declarations)) z typem delegata.</span><span class="sxs-lookup"><span data-stu-id="ce272-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="ce272-110">To sprawia, że delegaty idealnie nadaje się do wywołania "anonimowy".</span><span class="sxs-lookup"><span data-stu-id="ce272-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="ce272-111">Deklaracje delegata</span><span class="sxs-lookup"><span data-stu-id="ce272-111">Delegate declarations</span></span>

<span data-ttu-id="ce272-112">A *delegate_declaration* jest *type_declaration* ([wpisz deklaracje](namespaces.md#type-declarations)) Określa nowy typ delegata.</span><span class="sxs-lookup"><span data-stu-id="ce272-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

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

<span data-ttu-id="ce272-113">Jest to błąd czasu kompilacji dla tego samego modyfikator pojawią się wiele razy w deklaracji delegate.</span><span class="sxs-lookup"><span data-stu-id="ce272-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="ce272-114">`new` Modyfikator jest dozwolone tylko w delegatów zadeklarowana wewnątrz innego typu, w którym to przypadku określa, że taki obiekt delegowany ukrywa dziedziczoną składową o tej samej nazwie, zgodnie z opisem w [nowy modyfikator](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="ce272-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="ce272-115">`public`, `protected`, `internal`, I `private` Modyfikatory kontrolować ułatwień dostępu dla typu delegata.</span><span class="sxs-lookup"><span data-stu-id="ce272-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="ce272-116">W zależności od kontekstu, w którym występuje deklaracja delegata, niektóre z tych modyfikatorów może nie być dozwolone ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="ce272-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="ce272-117">Nazwa typu delegata jest *identyfikator*.</span><span class="sxs-lookup"><span data-stu-id="ce272-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="ce272-118">Opcjonalny *formal_parameter_list* określa parametry delegata, i *Typ_wyniku* wskazuje typ zwracany obiektu delegowanego.</span><span class="sxs-lookup"><span data-stu-id="ce272-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="ce272-119">Opcjonalny *variant_type_parameter_list* ([list parametrów typu Variant](interfaces.md#variant-type-parameter-lists)) określa parametry typu delegata, sam.</span><span class="sxs-lookup"><span data-stu-id="ce272-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="ce272-120">Typem zwracanym typem obiektu delegowanego musi być albo `void`, lub bezpieczny w danych wyjściowych ([bezpieczeństwa wariancji](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="ce272-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="ce272-121">Wszystkie typy parametrów formalnych typ obiektu delegowanego musi być bezpieczne pod względem danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="ce272-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="ce272-122">Ponadto dowolne `out` lub `ref` typy parametrów także musi być bezpieczne pod względem danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="ce272-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="ce272-123">Należy pamiętać, że nawet `out` parametry są wymagane jako dane wejściowe, bezpieczne, ze względu na ograniczenie możliwości wykonywania platformy.</span><span class="sxs-lookup"><span data-stu-id="ce272-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="ce272-124">Typy delegatów w języku C# są nazwa jego odpowiednika, nie jest równorzędny strukturę.</span><span class="sxs-lookup"><span data-stu-id="ce272-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="ce272-125">W szczególności dwa typy delegatów różnych, które mają ten sam parametr wyświetla i zwraca typ są traktowane jako typy delegatów różnych.</span><span class="sxs-lookup"><span data-stu-id="ce272-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="ce272-126">Jednak wystąpień dwóch typów różne, ale strukturalnie równoważne delegata może porównać jako równe ([delegować Operatory równości](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="ce272-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="ce272-127">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ce272-127">For example:</span></span>

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

<span data-ttu-id="ce272-128">Metody `A.M1` i `B.M1` są zgodne z typami delegatów `D1` i `D2` , ponieważ mają one takie same zwracają typ i listą parametrów; jednak te typy delegatów są dwa różne typy, dzięki czemu nie wymienne.</span><span class="sxs-lookup"><span data-stu-id="ce272-128">The methods `A.M1` and `B.M1` are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="ce272-129">Metody `B.M2`, `B.M3`, i `B.M4` są zgodne z typami delegatów `D1` i `D2`, ponieważ mają one różne typy zwracane lub listy parametrów.</span><span class="sxs-lookup"><span data-stu-id="ce272-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="ce272-130">Podobnie jak innych deklaracji typu ogólnego argumentów typu podaje się w celu utworzenia typu skonstruowanego delegata.</span><span class="sxs-lookup"><span data-stu-id="ce272-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="ce272-131">Typy parametrów i typ zwracany typ delegata skonstruowane są tworzone, zastępując dla każdego parametru typu w deklaracji delegata, argument typu odpowiedniego typu skonstruowanego delegata.</span><span class="sxs-lookup"><span data-stu-id="ce272-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="ce272-132">Wynikowy typ zwracany i typy parametrów są używane w określeniu, jakie metody są zgodne z typem delegowanym skonstruowany.</span><span class="sxs-lookup"><span data-stu-id="ce272-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="ce272-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ce272-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="ce272-134">Metoda `X.F` jest zgodny z typem delegata `Predicate<int>` , a także metoda `X.G` jest zgodny z typem delegata `Predicate<string>` .</span><span class="sxs-lookup"><span data-stu-id="ce272-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="ce272-135">Jedynym sposobem, aby zadeklarować typ delegata jest za pośrednictwem *delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="ce272-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="ce272-136">Typ obiektu delegowanego jest typu klasy, która jest pochodną `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="ce272-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="ce272-137">Typy delegatów są niejawną kolekcją `sealed`, więc nie jest dozwolone do wyprowadzenia dowolnego typu z typem delegowanym.</span><span class="sxs-lookup"><span data-stu-id="ce272-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="ce272-138">Również nie jest dozwolone, aby utworzyć pochodny typ niedelegowany klasy z `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="ce272-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="ce272-139">Należy pamiętać, że `System.Delegate` jest sam nie typ delegata; jest typem klasy, z której pochodzą wszystkie typy delegatów.</span><span class="sxs-lookup"><span data-stu-id="ce272-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="ce272-140">C# zawiera specjalnej składni dla delegata, podczas tworzenia wystąpienia i wywoływania.</span><span class="sxs-lookup"><span data-stu-id="ce272-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="ce272-141">Z wyjątkiem wystąpienia żadnych operacji, które można zastosować do klasy lub wystąpienia klasy można również będą stosowane do delegata klasy lub wystąpienia, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="ce272-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="ce272-142">W szczególności jest możliwość dostępu do elementów członkowskich `System.Delegate` typu za pomocą składni dostępu do zwykłych elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="ce272-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="ce272-143">Zestaw metod zamknięte przez wystąpienie delegata jest wywoływana jako wywołanie listy tych praktyk.</span><span class="sxs-lookup"><span data-stu-id="ce272-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="ce272-144">Podczas tworzenia wystąpienia delegata ([delegować zgodności](delegates.md#delegate-compatibility)) z jednej metody hermetyzującej element tej metody, a jego listy wywołań zawiera tylko jeden wpis.</span><span class="sxs-lookup"><span data-stu-id="ce272-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="ce272-145">Jednak połączeniu dwa wystąpienia innych niż null, delegat swoimi listami wywołania są łączone — w kolejności pozostałych operand, a następnie prawy operand — w celu utworzenia nowej listy wywołania, który zawiera dwa lub więcej wpisów.</span><span class="sxs-lookup"><span data-stu-id="ce272-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="ce272-146">Delegaty są łączone za pomocą pliku binarnego `+` ([operator dodawania](expressions.md#addition-operator)) i `+=` operatorów ([przydział złożony](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="ce272-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="ce272-147">Delegat może zostać usunięty z kombinacji delegatów, za pomocą pliku binarnego `-` ([operator odejmowania](expressions.md#subtraction-operator)) i `-=` operatorów ([przydział złożony](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="ce272-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="ce272-148">Delegaty mogą być porównywane pod kątem równości ([delegować Operatory równości](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="ce272-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="ce272-149">W poniższym przykładzie pokazano tworzenie wystąpienia liczby obiektów delegowanych, i ich odpowiedniego wywołania listy:</span><span class="sxs-lookup"><span data-stu-id="ce272-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

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

<span data-ttu-id="ce272-150">Gdy `cd1` i `cd2` są tworzone, ich hermetyzacji jednej metody.</span><span class="sxs-lookup"><span data-stu-id="ce272-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="ce272-151">Podczas `cd3` jest uruchomiony, ma listę wywołań z dwóch metod `M1` i `M2`, w tej kolejności.</span><span class="sxs-lookup"><span data-stu-id="ce272-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="ce272-152">`cd4`firmy zawiera listę wywołań `M1`, `M2`, i `M1`, w tej kolejności.</span><span class="sxs-lookup"><span data-stu-id="ce272-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="ce272-153">Na koniec `cd5`firmy zawiera listę wywołań `M1`, `M2`, `M1`, `M1`, i `M2`, w tej kolejności.</span><span class="sxs-lookup"><span data-stu-id="ce272-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="ce272-154">Aby uzyskać więcej przykładów łączenie delegatów (a także jak usuwania), zobacz [delegować wywołania](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="ce272-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="ce272-155">Zgodność delegata</span><span class="sxs-lookup"><span data-stu-id="ce272-155">Delegate compatibility</span></span>

<span data-ttu-id="ce272-156">Metoda ani delegat `M` jest ***zgodne*** z typem delegowanym `D` jeśli spełnione są wszystkie z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="ce272-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="ce272-157">`D` i `M` mają taką samą liczbę parametrów i każdego parametru w `D` ma taką samą `ref` lub `out` Modyfikatory jako odpowiadającym mu parametrem w `M`.</span><span class="sxs-lookup"><span data-stu-id="ce272-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="ce272-158">Dla każdego parametru wartości (parametr bez `ref` lub `out` modyfikator), konwersja tożsamości ([konwersji tożsamości](conversions.md#identity-conversion)) lub niejawna konwersja odwołania ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) istnieje niż typ parametru w `D` odpowiedni typ parametru w `M`.</span><span class="sxs-lookup"><span data-stu-id="ce272-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="ce272-159">Dla każdego `ref` lub `out` parametru, wpisz parametr `D` jest taki sam jak typ parametru w `M`.</span><span class="sxs-lookup"><span data-stu-id="ce272-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="ce272-160">Istnieje tożsamość lub niejawna konwersja odwołania z typem zwracanym `M` do zwracanego typu `D`.</span><span class="sxs-lookup"><span data-stu-id="ce272-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="ce272-161">Podczas tworzenia wystąpienia delegata</span><span class="sxs-lookup"><span data-stu-id="ce272-161">Delegate instantiation</span></span>

<span data-ttu-id="ce272-162">Wystąpienie delegata jest tworzony przez *delegate_creation_expression* ([delegować Tworzenie wyrażenia](expressions.md#delegate-creation-expressions)) lub konwersji do typu delegata.</span><span class="sxs-lookup"><span data-stu-id="ce272-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="ce272-163">Wystąpienie delegata nowo utworzony następnie odwołuje się albo:</span><span class="sxs-lookup"><span data-stu-id="ce272-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="ce272-164">Statyczna metoda, do którego odwołuje się *delegate_creation_expression*, lub</span><span class="sxs-lookup"><span data-stu-id="ce272-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="ce272-165">Obiekt docelowy (która nie może być `null`) lub wystąpienie metody, do którego odwołuje się *delegate_creation_expression*, lub</span><span class="sxs-lookup"><span data-stu-id="ce272-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="ce272-166">Delegat innego.</span><span class="sxs-lookup"><span data-stu-id="ce272-166">Another delegate.</span></span>

<span data-ttu-id="ce272-167">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ce272-167">For example:</span></span>

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

<span data-ttu-id="ce272-168">Po utworzeniu wystąpienia delegata zawsze odwołuje się do tego samego obiektu docelowego i metody.</span><span class="sxs-lookup"><span data-stu-id="ce272-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="ce272-169">Należy pamiętać, że gdy dwa delegaty są połączone lub jeden zostanie usunięty z kolei nowe wyniki delegata z własną listą wywołania; wywołania listę obiektów delegowanych, połączone lub usunąć pozostaną bez zmian.</span><span class="sxs-lookup"><span data-stu-id="ce272-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="ce272-170">Wywołanie delegata</span><span class="sxs-lookup"><span data-stu-id="ce272-170">Delegate invocation</span></span>

<span data-ttu-id="ce272-171">C# zapewnia specjalnej składni wywoływania delegata.</span><span class="sxs-lookup"><span data-stu-id="ce272-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="ce272-172">Gdy wystąpienie delegata inną niż null, którego lista wywołania zawiera jeden wpis jest wywoływany, wywołuje ono jedną metodę, przy użyciu tych samych argumentów podano i zwraca taką samą wartość jak określonych do metody.</span><span class="sxs-lookup"><span data-stu-id="ce272-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="ce272-173">(Zobacz [delegowanie wywołań](expressions.md#delegate-invocations) szczegółowe informacje na temat wywołanie delegata.) Jeśli wystąpi wyjątek podczas wywołania obiektu delegowanego, a ten wyjątek nie jest wyłapywany wewnątrz metody, która została wywołana, wyszukaj klauzuli catch wyjątku jest kontynuowane w metodzie, która wywołuje delegata, tak, jakby były wywołane bezpośrednio tej metody Metoda, do której delegować, określane.</span><span class="sxs-lookup"><span data-stu-id="ce272-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="ce272-174">Wywołanie wystąpienie delegata, którego lista wywołania zawiera wiele pozycji rozpoczynające się za pomocą wywołania tych metod na liście wywołania synchronicznie, w kolejności.</span><span class="sxs-lookup"><span data-stu-id="ce272-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="ce272-175">Każda metoda tak zwane jest przekazywany tego samego zestawu argumentów, jak podano na wystąpienie delegata.</span><span class="sxs-lookup"><span data-stu-id="ce272-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="ce272-176">Jeśli wywołanie delegata zawierają parametry odwołań ([odwołania do parametrów](classes.md#reference-parameters)), nastąpi każdego wywołania metody w odniesieniu do tej samej zmiennej; zostaną zmiany w tej zmiennej według jedną metodę liście wywołania widoczna dalsze metod w dół listy wywołań.</span><span class="sxs-lookup"><span data-stu-id="ce272-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="ce272-177">Jeśli wywołanie delegata zawiera parametry wyjściowe lub wartości zwracanej, ich wartość końcową będą pochodzić z wywołanie delegata ostatniej, na liście.</span><span class="sxs-lookup"><span data-stu-id="ce272-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="ce272-178">Jeśli wystąpi wyjątek podczas przetwarzania wywołania takiego delegata, a ten wyjątek nie jest wyłapywany wewnątrz metody, która została wywołana, wyszukaj klauzuli catch wyjątku jest kontynuowane w metodzie, która wywołuje delegata, a wszystkie metody dalszej części na liście wywołania nie są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="ce272-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="ce272-179">Próba wywołania wystąpienie delegata, w których wartość wynosi null powoduje wyjątek typu `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="ce272-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="ce272-180">Poniższy przykład pokazuje, jak utworzyć wystąpienie, łączenie, Usuń i wywoływać delegatów:</span><span class="sxs-lookup"><span data-stu-id="ce272-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

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

<span data-ttu-id="ce272-181">Jak pokazano w instrukcji `cd3 += cd1;`, obiekt delegowany mogą być obecne na liście wywołania wiele razy.</span><span class="sxs-lookup"><span data-stu-id="ce272-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="ce272-182">W tym przypadku jest po prostu wywoływana raz na wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="ce272-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="ce272-183">Na liście wywołania takich po usunięciu delegata ostatniego wystąpienia na liście wywołania jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="ce272-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="ce272-184">Bezpośrednio przed wykonaniem instrukcji końcowego `cd3 -= cd1;`, delegat `cd3` odwołuje się do listy wywołań puste.</span><span class="sxs-lookup"><span data-stu-id="ce272-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="ce272-185">Próba usunięcia delegata z pustej listy (lub usuwanie delegata nieistniejącej z listy Niepuste) nie jest błąd.</span><span class="sxs-lookup"><span data-stu-id="ce272-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="ce272-186">Dane wyjściowe, generowane są:</span><span class="sxs-lookup"><span data-stu-id="ce272-186">The output produced is:</span></span>

```
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
