---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488778"
---
# <a name="interfaces"></a><span data-ttu-id="b2f45-101">Interfejsy</span><span class="sxs-lookup"><span data-stu-id="b2f45-101">Interfaces</span></span>

<span data-ttu-id="b2f45-102">Interfejs definiuje kontrakt.</span><span class="sxs-lookup"><span data-stu-id="b2f45-102">An interface defines a contract.</span></span> <span data-ttu-id="b2f45-103">Klasa lub struktura, która implementuje interfejs musi być zgodne z jego umową.</span><span class="sxs-lookup"><span data-stu-id="b2f45-103">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="b2f45-104">Interfejs może dziedziczyć z wielu interfejsach podstawowych, a klasa lub struktura może zaimplementować wiele interfejsów.</span><span class="sxs-lookup"><span data-stu-id="b2f45-104">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="b2f45-105">Interfejsy mogą zawierać metody, właściwości, zdarzeń i indeksatorów.</span><span class="sxs-lookup"><span data-stu-id="b2f45-105">Interfaces can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="b2f45-106">Interfejs, sama nie zapewnia implementacje dla elementów członkowskich, które definiuje.</span><span class="sxs-lookup"><span data-stu-id="b2f45-106">The interface itself does not provide implementations for the members that it defines.</span></span> <span data-ttu-id="b2f45-107">Interfejs określa jedynie elementy członkowskie, które muszą być dostarczane przez klasy lub struktury, które implementują interfejs.</span><span class="sxs-lookup"><span data-stu-id="b2f45-107">The interface merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

## <a name="interface-declarations"></a><span data-ttu-id="b2f45-108">Deklaracje interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-108">Interface declarations</span></span>

<span data-ttu-id="b2f45-109">*Interface_declaration* jest *type_declaration* ([wpisz deklaracje](namespaces.md#type-declarations)) oświadcza, że nowy typ interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-109">An *interface_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new interface type.</span></span>

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

<span data-ttu-id="b2f45-110">*Interface_declaration* zawiera opcjonalny zestaw *atrybuty* ([atrybuty](attributes.md)), a następnie opcjonalny zestaw *interface_modifier*s ([interfejsu Modyfikatory](interfaces.md#interface-modifiers)), a następnie opcjonalnie `partial` modyfikator, następuje słowa kluczowego `interface` i *identyfikator* , nazwy interfejsu, następuje opcjonalny *variant_type_parameter_list* specyfikacji ([list parametrów typu Variant](interfaces.md#variant-type-parameter-lists)), a następnie opcjonalnie *interface_base* Specyfikacja ([podstawowa interfejsów](interfaces.md#base-interfaces)), a następnie opcjonalnie *type_parameter_constraints_clause*specyfikacji s ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) , a następnie *interface_body* ([interfejsu treści](interfaces.md#interface-body)), opcjonalnie następuje średnik.</span><span class="sxs-lookup"><span data-stu-id="b2f45-110">An *interface_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *interface_modifier*s ([Interface modifiers](interfaces.md#interface-modifiers)), followed by an optional `partial` modifier, followed by the keyword `interface` and an *identifier* that names the interface, followed by an optional *variant_type_parameter_list* specification ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)), followed by an optional *interface_base* specification ([Base interfaces](interfaces.md#base-interfaces)), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by an *interface_body* ([Interface body](interfaces.md#interface-body)), optionally followed by a semicolon.</span></span>

### <a name="interface-modifiers"></a><span data-ttu-id="b2f45-111">Modyfikatory interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-111">Interface modifiers</span></span>

<span data-ttu-id="b2f45-112">*Interface_declaration* może opcjonalnie obejmować sekwencję Modyfikatory interfejsu:</span><span class="sxs-lookup"><span data-stu-id="b2f45-112">An *interface_declaration* may optionally include a sequence of interface modifiers:</span></span>

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

<span data-ttu-id="b2f45-113">Jest to błąd czasu kompilacji dla tego samego modyfikator pojawią się wiele razy w deklaracji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-113">It is a compile-time error for the same modifier to appear multiple times in an interface declaration.</span></span>

<span data-ttu-id="b2f45-114">`new` Modyfikator jest dozwolona tylko w interfejsach zdefiniowany w klasie.</span><span class="sxs-lookup"><span data-stu-id="b2f45-114">The `new` modifier is only permitted on interfaces defined within a class.</span></span> <span data-ttu-id="b2f45-115">Określa, że interfejs ukrywa dziedziczonej składowej o tej samej nazwie, zgodnie z opisem w [nowy modyfikator](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="b2f45-115">It specifies that the interface hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="b2f45-116">`public`, `protected`, `internal`, I `private` Modyfikatory kontrolować ułatwień dostępu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the interface.</span></span> <span data-ttu-id="b2f45-117">W zależności od kontekstu, w którym występuje deklaracja interfejsu, tylko niektóre z tych modyfikatorów mogą być dozwolone ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="b2f45-117">Depending on the context in which the interface declaration occurs, only some of these modifiers may be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="b2f45-118">Modyfikatora "Partial"</span><span class="sxs-lookup"><span data-stu-id="b2f45-118">Partial modifier</span></span>

<span data-ttu-id="b2f45-119">`partial` Modyfikator wskazuje, że to *interface_declaration* jest deklaracją typu częściowego.</span><span class="sxs-lookup"><span data-stu-id="b2f45-119">The `partial` modifier indicates that this *interface_declaration* is a partial type declaration.</span></span> <span data-ttu-id="b2f45-120">Łączenie wielu deklaracjach częściowej interfejsu o takiej samej nazwie w deklaracji otaczającego przestrzeń nazw lub typ do postaci jednego interfejsu deklaracji, zgodnie z regułami określone w [typów częściowych](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="b2f45-120">Multiple partial interface declarations with the same name within an enclosing namespace or type declaration combine to form one interface declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="variant-type-parameter-lists"></a><span data-ttu-id="b2f45-121">Listy parametrów typu Variant</span><span class="sxs-lookup"><span data-stu-id="b2f45-121">Variant type parameter lists</span></span>

<span data-ttu-id="b2f45-122">Listy parametrów typu Variant może nastąpić tylko na typy interfejsów i delegatów.</span><span class="sxs-lookup"><span data-stu-id="b2f45-122">Variant type parameter lists can only occur on interface and delegate types.</span></span> <span data-ttu-id="b2f45-123">Różnica od zwykłych *type_parameter_list*s jest opcjonalny *variance_annotation* dla każdego parametru typu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-123">The difference from ordinary *type_parameter_list*s is the optional *variance_annotation* on each type parameter.</span></span>

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

<span data-ttu-id="b2f45-124">Jeśli jest adnotacji wariancji `out`, parametr typu jest określany jako ***kowariantne***.</span><span class="sxs-lookup"><span data-stu-id="b2f45-124">If the variance annotation is `out`, the type parameter is said to be ***covariant***.</span></span> <span data-ttu-id="b2f45-125">Jeśli jest adnotacji wariancji `in`, parametr typu jest określany jako ***kontrawariantne***.</span><span class="sxs-lookup"><span data-stu-id="b2f45-125">If the variance annotation is `in`, the type parameter is said to be ***contravariant***.</span></span> <span data-ttu-id="b2f45-126">Jeśli nie ma żadnych adnotacji wariancji, parametr typu jest określany jako ***niezmiennej***.</span><span class="sxs-lookup"><span data-stu-id="b2f45-126">If there is no variance annotation, the type parameter is said to be ***invariant***.</span></span>

<span data-ttu-id="b2f45-127">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="b2f45-127">In the example</span></span>
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
<span data-ttu-id="b2f45-128">`X` jest kowariantny, `Y` jest kontrawariantny i `Z` jest niezmienny.</span><span class="sxs-lookup"><span data-stu-id="b2f45-128">`X` is covariant, `Y` is contravariant and `Z` is invariant.</span></span>

#### <a name="variance-safety"></a><span data-ttu-id="b2f45-129">Wariancja bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="b2f45-129">Variance safety</span></span>

<span data-ttu-id="b2f45-130">Wystąpienie adnotacje dotyczące wariancji na liście parametrów typu typu ogranicza miejsca, w którym typów może mieć miejsce w deklaracji typu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-130">The occurrence of variance annotations in the type parameter list of a type restricts the places where types can occur within the type declaration.</span></span>

<span data-ttu-id="b2f45-131">Typ `T` jest ***niebezpiecznych danych wyjściowych*** jeśli posiada jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b2f45-131">A type `T` is ***output-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="b2f45-132">`T` jest kontrawariantnego parametru typu</span><span class="sxs-lookup"><span data-stu-id="b2f45-132">`T` is a contravariant type parameter</span></span>
*  <span data-ttu-id="b2f45-133">`T` jest typem tablicy o typie elementu niebezpiecznych danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="b2f45-133">`T` is an array type with an output-unsafe element type</span></span>
*  <span data-ttu-id="b2f45-134">`T` Typ interfejsu lub delegata `S<A1,...,Ak>` skonstruowany z typu ogólnego `S<X1,...,Xk>` miejsca dla co najmniej jednej `Ai` zawiera jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b2f45-134">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="b2f45-135">`Xi` jest kowariantny lub niezmiennej i `Ai` jest niebezpieczne danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-135">`Xi` is covariant or invariant and `Ai` is output-unsafe.</span></span>
   * <span data-ttu-id="b2f45-136">`Xi` jest kontrawariantny lub niezmiennej i `Ai` jest bezpieczna pod względem danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-136">`Xi` is contravariant or invariant and `Ai` is input-safe.</span></span>
   
<span data-ttu-id="b2f45-137">Typ `T` jest ***niebezpiecznych danych wejściowych*** jeśli posiada jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b2f45-137">A type `T` is ***input-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="b2f45-138">`T` jest kowariantny parametr typu</span><span class="sxs-lookup"><span data-stu-id="b2f45-138">`T` is a covariant type parameter</span></span>
*  <span data-ttu-id="b2f45-139">`T` jest typem tablicy o typie elementu niebezpiecznych danych wejściowych</span><span class="sxs-lookup"><span data-stu-id="b2f45-139">`T` is an array type with an input-unsafe element type</span></span>
*  <span data-ttu-id="b2f45-140">`T` Typ interfejsu lub delegata `S<A1,...,Ak>` skonstruowany z typu ogólnego `S<X1,...,Xk>` miejsca dla co najmniej jednej `Ai` zawiera jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b2f45-140">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="b2f45-141">`Xi` jest kowariantny lub niezmiennej i `Ai` jest niebezpieczne danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-141">`Xi` is covariant or invariant and `Ai` is input-unsafe.</span></span>
   * <span data-ttu-id="b2f45-142">`Xi` jest kontrawariantny lub niezmiennej i `Ai` jest niebezpieczne danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-142">`Xi` is contravariant or invariant and `Ai` is output-unsafe.</span></span>

<span data-ttu-id="b2f45-143">Intuicyjne typem niebezpiecznych danych wyjściowych jest zabronione w pozycji danych wyjściowych, a typem niebezpiecznych danych wejściowych jest zabronione w pozycji danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-143">Intuitively, an output-unsafe type is prohibited in an output position, and an input-unsafe type is prohibited in an input position.</span></span>

<span data-ttu-id="b2f45-144">Typ jest ***bezpieczne pod względem danych wyjściowych*** Jeśli nie jest dane wyjściowe — zagrożenie, i ***bezpieczne pod względem danych wejściowych*** Jeśli nie jest niebezpieczne danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-144">A type is ***output-safe*** if it is not output-unsafe, and ***input-safe*** if it is not input-unsafe.</span></span>

#### <a name="variance-conversion"></a><span data-ttu-id="b2f45-145">Konwersja wariancji</span><span class="sxs-lookup"><span data-stu-id="b2f45-145">Variance conversion</span></span>

<span data-ttu-id="b2f45-146">Adnotacje dotyczące wariancji ma na celu zapewnienia bardziej łagodne (ale nadal bezpiecznego typu) konwersji na typy interfejsów i delegatów.</span><span class="sxs-lookup"><span data-stu-id="b2f45-146">The purpose of variance annotations is to provide for more lenient (but still type safe) conversions to interface and delegate types.</span></span> <span data-ttu-id="b2f45-147">W tym celu definicje niejawne ([niejawne konwersje](conversions.md#implicit-conversions)) i jawne konwersje ([jawne konwersje](conversions.md#explicit-conversions)) upewnij użytkowania pojęcie wariancji — przetwarzania, która jest zdefiniowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b2f45-147">To this end the definitions of implicit ([Implicit conversions](conversions.md#implicit-conversions)) and explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) make use of the notion of variance-convertibility, which is defined as follows:</span></span>

<span data-ttu-id="b2f45-148">Typ `T<A1,...,An>` jest odchylenie przekonwertować na typ `T<B1,...,Bn>` Jeśli `T` interfejsem lub typem obiektu delegowanego jest zadeklarowana z parametrami typu variant `T<X1,...,Xn>`i dla każdego parametru typu variant `Xi` jedną z następujących posiada:</span><span class="sxs-lookup"><span data-stu-id="b2f45-148">A type `T<A1,...,An>` is variance-convertible to a type `T<B1,...,Bn>` if `T` is either an interface or a delegate type declared with the variant type parameters `T<X1,...,Xn>`, and for each variant type parameter `Xi` one of the following holds:</span></span>

*  <span data-ttu-id="b2f45-149">`Xi` jest kowariantny i istnieje niejawna konwersja odwołania lub tożsamości `Ai` do `Bi`</span><span class="sxs-lookup"><span data-stu-id="b2f45-149">`Xi` is covariant and an implicit reference or identity conversion exists from `Ai` to `Bi`</span></span>
*  <span data-ttu-id="b2f45-150">`Xi` jest kontrawariantny i niejawne odwołanie lub istnieje konwersja tożsamości z `Bi` do `Ai`</span><span class="sxs-lookup"><span data-stu-id="b2f45-150">`Xi` is contravariant and an implicit reference or identity conversion exists from `Bi` to `Ai`</span></span>
*  <span data-ttu-id="b2f45-151">`Xi` jest niezależna i tożsamości istnieje konwersja z `Ai` do `Bi`</span><span class="sxs-lookup"><span data-stu-id="b2f45-151">`Xi` is invariant and an identity conversion exists from `Ai` to `Bi`</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="b2f45-152">Interfejsy podstawowe</span><span class="sxs-lookup"><span data-stu-id="b2f45-152">Base interfaces</span></span>

<span data-ttu-id="b2f45-153">Interfejs może dziedziczyć z zero lub więcej typów interfejsów, które są nazywane ***jawne interfejsy podstawowe*** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-153">An interface can inherit from zero or more interface types, which are called the ***explicit base interfaces*** of the interface.</span></span> <span data-ttu-id="b2f45-154">Gdy interfejs ma jeden lub więcej jawne interfejsy podstawowe, następnie w deklaracji interfejsu, identyfikator interfejsu następuje dwukropek i przecinkami oddzielone listy typów interfejs podstawowy.</span><span class="sxs-lookup"><span data-stu-id="b2f45-154">When an interface has one or more explicit base interfaces, then in the declaration of that interface, the interface identifier is followed by a colon and a comma separated list of base interface types.</span></span>

```antlr
interface_base
    : ':' interface_type_list
    ;
```

<span data-ttu-id="b2f45-155">Dla typu skonstruowanego interfejsu jawnego interfejsy podstawowe są utworzone przez podejmowanie deklaracje jawne interfejs podstawowy deklaracją typu ogólnego oraz zastępując dla każdego *type_parameter* w interfejs podstawowy Deklaracja, odpowiedni *type_argument* skonstruowanego typu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-155">For a constructed interface type, the explicit base interfaces are formed by taking the explicit base interface declarations on the generic type declaration, and substituting, for each *type_parameter* in the base interface declaration, the corresponding *type_argument* of the constructed type.</span></span>

<span data-ttu-id="b2f45-156">Jawne interfejsy podstawowe interfejsu musi być co najmniej tak samo dostępna jako interfejs, sama ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="b2f45-156">The explicit base interfaces of an interface must be at least as accessible as the interface itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span> <span data-ttu-id="b2f45-157">Na przykład, jest to błąd kompilacji, aby określić `private` lub `internal` interfejsu w *interface_base* z `public` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-157">For example, it is a compile-time error to specify a `private` or `internal` interface in the *interface_base* of a `public` interface.</span></span>

<span data-ttu-id="b2f45-158">Jest to błąd czasu kompilacji, interfejsu bezpośrednio lub pośrednio dziedziczyć po sobie samym.</span><span class="sxs-lookup"><span data-stu-id="b2f45-158">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>

<span data-ttu-id="b2f45-159">***Podstawowa interfejsów*** interfejsu są jawne interfejsy podstawowe i ich interfejsy podstawowe.</span><span class="sxs-lookup"><span data-stu-id="b2f45-159">The ***base interfaces*** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="b2f45-160">Oznacza to zbiór interfejsy podstawowe jest pełne zamknięcie przechodnie jawne interfejsy podstawowe, jawne interfejsy podstawowe i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="b2f45-160">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="b2f45-161">Interfejs dziedziczy wszystkie elementy członkowskie jego interfejsy podstawowe.</span><span class="sxs-lookup"><span data-stu-id="b2f45-161">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="b2f45-162">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="b2f45-162">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="b2f45-163">interfejsy podstawowe z `IComboBox` są `IControl`, `ITextBox`, i `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-163">the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="b2f45-164">Innymi słowy `IComboBox` powyżej interfejs dziedziczy członków `SetText` i `SetItems` także `Paint`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-164">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="b2f45-165">Każdy interfejs podstawowy interfejsu muszą być bezpieczne pod względem danych wyjściowych ([bezpieczeństwa wariancji](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="b2f45-165">Every base interface of an interface must be output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span> <span data-ttu-id="b2f45-166">Klasa lub struktura, która implementuje interfejs, dodatkowo niejawnie implementuje wszystkie interfejsy podstawowe interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-166">A class or struct that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

### <a name="interface-body"></a><span data-ttu-id="b2f45-167">Treść interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-167">Interface body</span></span>

<span data-ttu-id="b2f45-168">*Interface_body* interfejsu definiuje składowych interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-168">The *interface_body* of an interface defines the members of the interface.</span></span>

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a><span data-ttu-id="b2f45-169">Elementy członkowskie interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-169">Interface members</span></span>

<span data-ttu-id="b2f45-170">Członkowie interfejsu są dziedziczone z interfejsy podstawowe elementy członkowskie i elementy członkowskie zadeklarowana przez sam interfejs.</span><span class="sxs-lookup"><span data-stu-id="b2f45-170">The members of an interface are the members inherited from the base interfaces and the members declared by the interface itself.</span></span>

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

<span data-ttu-id="b2f45-171">Deklaracja interfejsu może zadeklarować zero lub więcej elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="b2f45-171">An interface declaration may declare zero or more members.</span></span> <span data-ttu-id="b2f45-172">Członkowie interfejs musi być metody, właściwości, zdarzeń i indeksatorów.</span><span class="sxs-lookup"><span data-stu-id="b2f45-172">The members of an interface must be methods, properties, events, or indexers.</span></span> <span data-ttu-id="b2f45-173">Interfejs nie może zawierać stałe, pola, operatory, konstruktory wystąpień, destruktorów ani typów nie może zawierać interfejs statyczne elementy członkowskie dowolnego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="b2f45-173">An interface cannot contain constants, fields, operators, instance constructors, destructors, or types, nor can an interface contain static members of any kind.</span></span>

<span data-ttu-id="b2f45-174">Wszyscy członkowie interfejsu niejawnie mają dostęp publiczny.</span><span class="sxs-lookup"><span data-stu-id="b2f45-174">All interface members implicitly have public access.</span></span> <span data-ttu-id="b2f45-175">Jest to błąd kompilacji, dla deklaracji elementu członkowskiego interfejsu do uwzględnienia wszelkich modyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="b2f45-175">It is a compile-time error for interface member declarations to include any modifiers.</span></span> <span data-ttu-id="b2f45-176">W szczególności członków interfejsów nie można zadeklarować za pomocą modyfikatorów `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, lub `static`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-176">In particular, interfaces members cannot be declared with the modifiers `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="b2f45-177">Przykład</span><span class="sxs-lookup"><span data-stu-id="b2f45-177">The example</span></span>
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
<span data-ttu-id="b2f45-178">deklaruje interfejs, który zawiera jedną z możliwych rodzajów elementów członkowskich: Metody, właściwości, zdarzenia i indeksatora.</span><span class="sxs-lookup"><span data-stu-id="b2f45-178">declares an interface that contains one each of the possible kinds of members: A method, a property, an event, and an indexer.</span></span>

<span data-ttu-id="b2f45-179">*Interface_declaration* tworzy nowe miejsce do deklaracji ([deklaracje](basic-concepts.md#declarations)), a *interface_member_declaration*s natychmiast zawartych *interface_declaration* wprowadzenia nowych członków do tej deklaracji przestrzeni.</span><span class="sxs-lookup"><span data-stu-id="b2f45-179">An *interface_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *interface_member_declaration*s immediately contained by the *interface_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="b2f45-180">Następujące reguły mają zastosowanie do *interface_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="b2f45-180">The following rules apply to *interface_member_declaration*s:</span></span>

*  <span data-ttu-id="b2f45-181">Nazwa metody muszą różnić się od nazwy wszystkich właściwości i zdarzenia zadeklarowanych w tym samym interfejsie.</span><span class="sxs-lookup"><span data-stu-id="b2f45-181">The name of a method must differ from the names of all properties and events declared in the same interface.</span></span> <span data-ttu-id="b2f45-182">Ponadto, podpis ([podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading)) z metody muszą różnić się od podpisy wszystkich pozostałych metod zadeklarowanych w tym samym interfejsie i dwóch metod zadeklarowanych w tym samym interfejsie nie może mieć podpisów, różnią się jedynie przez `ref` i `out`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-182">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same interface, and two methods declared in the same interface may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="b2f45-183">Nazwa właściwości lub zdarzenia musi się różnić od nazw innych składowych zadeklarowanych w tym samym interfejsie.</span><span class="sxs-lookup"><span data-stu-id="b2f45-183">The name of a property or event must differ from the names of all other members declared in the same interface.</span></span>
*  <span data-ttu-id="b2f45-184">Podpis indeksatora musi się różnić od podpisy innymi indeksatorami zadeklarowanych w tym samym interfejsie.</span><span class="sxs-lookup"><span data-stu-id="b2f45-184">The signature of an indexer must differ from the signatures of all other indexers declared in the same interface.</span></span>

<span data-ttu-id="b2f45-185">Dziedziczone składowe interfejsu są specjalnie nie jest częścią przestrzeni deklaracji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-185">The inherited members of an interface are specifically not part of the declaration space of the interface.</span></span> <span data-ttu-id="b2f45-186">W związku z tym interfejs może zadeklarować członka o tej samej nazwie lub podpisie, jako dziedziczonego członka.</span><span class="sxs-lookup"><span data-stu-id="b2f45-186">Thus, an interface is allowed to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="b2f45-187">W takiej sytuacji interfejs pochodny element członkowski jest nazywany Ukryj składowe interfejs podstawowy.</span><span class="sxs-lookup"><span data-stu-id="b2f45-187">When this occurs, the derived interface member is said to hide the base interface member.</span></span> <span data-ttu-id="b2f45-188">Ukrywanie dziedziczonej składowej nie jest uważany za błąd, ale spowodować, że kompilator generuje ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="b2f45-188">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="b2f45-189">Aby pominąć to ostrzeżenie, musi zawierać deklaracji elementu członkowskiego interfejsu pochodnego `new` modyfikator, aby wskazać, że składowa pochodnej jest przeznaczona do Ukryj składową bazową.</span><span class="sxs-lookup"><span data-stu-id="b2f45-189">To suppress the warning, the declaration of the derived interface member must include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="b2f45-190">W tym temacie omówiono w dalszej [ukrywanie poprzez dziedziczenie](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="b2f45-190">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="b2f45-191">Jeśli `new` modyfikator znajduje się w deklaracji, która nie ukrywa dziedziczonego członka, w tym celu jest wyświetlane ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="b2f45-191">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning is issued to that effect.</span></span> <span data-ttu-id="b2f45-192">To ostrzeżenie jest pominięty, usuwając `new` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="b2f45-192">This warning is suppressed by removing the `new` modifier.</span></span>

<span data-ttu-id="b2f45-193">Należy pamiętać, że elementy członkowskie w klasie `object` nie są ściśle rzecz biorąc, elementy członkowskie dowolnego interfejsu ([interfejsu członków](interfaces.md#interface-members)).</span><span class="sxs-lookup"><span data-stu-id="b2f45-193">Note that the members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="b2f45-194">Jednak elementy członkowskie w klasie `object` są dostępne za pośrednictwem wyszukać składowej w dowolny typ interfejsu ([wyszukanie członka](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="b2f45-194">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="interface-methods"></a><span data-ttu-id="b2f45-195">Metody interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-195">Interface methods</span></span>

<span data-ttu-id="b2f45-196">Metody interfejsu są deklarowane przy użyciu *interface_method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="b2f45-196">Interface methods are declared using *interface_method_declaration*s:</span></span>

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

<span data-ttu-id="b2f45-197">*Atrybuty*, *Typ_wyniku*, *identyfikator*, i *formal_parameter_list* deklaracji metody interfejsu mają taki sam co oznacza, jak te deklaracji metody w klasie ([metody](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="b2f45-197">The *attributes*, *return_type*, *identifier*, and *formal_parameter_list* of an interface method declaration have the same meaning as those of a method declaration in a class ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="b2f45-198">Deklaracji metody interfejsu nie jest dozwolona, aby określić treści metody, a deklaracji w związku z tym zawsze kończy się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="b2f45-198">An interface method declaration is not permitted to specify a method body, and the declaration therefore always ends with a semicolon.</span></span>

<span data-ttu-id="b2f45-199">Każdego typu formalnego parametru metody interfejsu muszą być bezpieczne pod względem danych wejściowych ([bezpieczeństwa wariancji](interfaces.md#variance-safety)), i zwracanym typem musi być albo `void` lub bezpieczny w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-199">Each formal parameter type of an interface method must be input-safe ([Variance safety](interfaces.md#variance-safety)), and the return type must be either `void` or output-safe.</span></span> <span data-ttu-id="b2f45-200">Ponadto każdy ograniczenie typu klasy, ograniczenie typu interfejsu i ograniczenia parametru typu dowolnego typu jako parametru metody muszą być bezpieczne pod względem danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-200">Furthermore, each class type constraint, interface type constraint and type parameter constraint on any type parameter of the method must be input-safe.</span></span>

<span data-ttu-id="b2f45-201">Te reguły upewnij się, że wszelkie kowariantny lub kontrawariantny użycia interfejsu pozostaje bezpiecznegop typu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-201">These rules ensure that any covariant or contravariant usage of the interface remains type-safe.</span></span> <span data-ttu-id="b2f45-202">Na przykład</span><span class="sxs-lookup"><span data-stu-id="b2f45-202">For example,</span></span>
```csharp
interface I<out T> { void M<U>() where U : T; }
```
<span data-ttu-id="b2f45-203">jest niedozwolony ponieważ użycie `T` jako ograniczenia parametru typu na `U` nie jest bezpieczna pod danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-203">is illegal because the usage of `T` as a type parameter constraint on `U` is not input-safe.</span></span>

<span data-ttu-id="b2f45-204">To ograniczenie nie stosowanych wcześniej sytuacji będzie można naruszyć bezpieczeństwo typu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b2f45-204">Were this restriction not in place it would be possible to violate type safety in the following manner:</span></span>
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
<span data-ttu-id="b2f45-205">To jest faktycznie wywołanie `C.M<E>`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-205">This is actually a call to `C.M<E>`.</span></span> <span data-ttu-id="b2f45-206">Jednak, że wywołanie jest wymagane, aby `E` dziedziczyć `D`, więc zostałyby w tym miejscu naruszone bezpieczeństwo typów.</span><span class="sxs-lookup"><span data-stu-id="b2f45-206">But that call requires that `E` derive from `D`, so type safety would be violated here.</span></span>

### <a name="interface-properties"></a><span data-ttu-id="b2f45-207">Właściwości interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-207">Interface properties</span></span>

<span data-ttu-id="b2f45-208">Właściwości interfejsu są deklarowane przy użyciu *interface_property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="b2f45-208">Interface properties are declared using *interface_property_declaration*s:</span></span>

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

<span data-ttu-id="b2f45-209">*Atrybuty*, *typu*, i *identyfikator* deklaracji właściwości interfejsu mają takie samo znaczenie jak deklaracja właściwości w klasie ([ Właściwości](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="b2f45-209">The *attributes*, *type*, and *identifier* of an interface property declaration have the same meaning as those of a property declaration in a class ([Properties](classes.md#properties)).</span></span>

<span data-ttu-id="b2f45-210">Metod dostępu w deklaracji właściwości interfejsu odpowiadają metod dostępu w deklaracji właściwości klasy ([Akcesory](classes.md#accessors)), z tą różnicą, że treść metody dostępu musi być zawsze średnikiem.</span><span class="sxs-lookup"><span data-stu-id="b2f45-210">The accessors of an interface property declaration correspond to the accessors of a class property declaration ([Accessors](classes.md#accessors)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="b2f45-211">W związku z tym Akcesory wystarczy wskazać, czy właściwość jest odczytu i zapisu, tylko do odczytu lub tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-211">Thus, the accessors simply indicate whether the property is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="b2f45-212">Typ właściwości interfejsu muszą być bezpieczne dane wyjściowe, jeśli ma ona metody dostępu get i muszą być bezpieczne danych wejściowych w przypadku dostępu set.</span><span class="sxs-lookup"><span data-stu-id="b2f45-212">The type of an interface property must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-events"></a><span data-ttu-id="b2f45-213">Zdarzenia interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-213">Interface events</span></span>

<span data-ttu-id="b2f45-214">Zdarzenia interfejsu są deklarowane przy użyciu *interface_event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="b2f45-214">Interface events are declared using *interface_event_declaration*s:</span></span>

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

<span data-ttu-id="b2f45-215">*Atrybuty*, *typu*, i *identyfikator* deklaracji zdarzenia interfejsu mają takie samo znaczenie jak w deklaracji zdarzenia w klasie ([zdarzenia ](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="b2f45-215">The *attributes*, *type*, and *identifier* of an interface event declaration have the same meaning as those of an event declaration in a class ([Events](classes.md#events)).</span></span>

<span data-ttu-id="b2f45-216">Typ interfejsu zdarzenia musi być bezpieczne pod względem danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-216">The type of an interface event must be input-safe.</span></span>

### <a name="interface-indexers"></a><span data-ttu-id="b2f45-217">Indeksatory interfejsów</span><span class="sxs-lookup"><span data-stu-id="b2f45-217">Interface indexers</span></span>

<span data-ttu-id="b2f45-218">Indeksatory interfejsu są deklarowane przy użyciu *interface_indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="b2f45-218">Interface indexers are declared using *interface_indexer_declaration*s:</span></span>

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

<span data-ttu-id="b2f45-219">*Atrybuty*, *typu*, i *formal_parameter_list* deklaracji interfejsu indeksator mają takie samo znaczenie jak deklaracja indeksatora w klasie ([ Indeksatory](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="b2f45-219">The *attributes*, *type*, and *formal_parameter_list* of an interface indexer declaration have the same meaning as those of an indexer declaration in a class ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="b2f45-220">Metod dostępu w deklaracji interfejsu indeksatora odpowiadają metod dostępu w deklaracji klasy indeksatora ([indeksatory](classes.md#indexers)), z tą różnicą, że treść metody dostępu musi być zawsze średnikiem.</span><span class="sxs-lookup"><span data-stu-id="b2f45-220">The accessors of an interface indexer declaration correspond to the accessors of a class indexer declaration ([Indexers](classes.md#indexers)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="b2f45-221">W efekcie metod dostępu po prostu informujące o indeksatora odczytu i zapisu, tylko do odczytu lub tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-221">Thus, the accessors simply indicate whether the indexer is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="b2f45-222">Wszystkie typy parametrów formalnych indeksatora interfejsu muszą być bezpieczne pod względem danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-222">All the formal parameter types of an interface indexer must be input-safe .</span></span> <span data-ttu-id="b2f45-223">Ponadto wszelkie `out` lub `ref` typy parametrów formalnych również muszą być bezpieczne pod względem danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-223">In addition, any `out` or `ref` formal parameter types must also be output-safe.</span></span> <span data-ttu-id="b2f45-224">Należy pamiętać, że nawet `out` parametry są wymagane jako dane wejściowe, bezpieczne, ze względu na ograniczenie możliwości wykonywania platformy.</span><span class="sxs-lookup"><span data-stu-id="b2f45-224">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="b2f45-225">Typ indeksatora interfejsu muszą być bezpieczne dane wyjściowe, jeśli ma ona metody dostępu get i muszą być bezpieczne danych wejściowych w przypadku dostępu set.</span><span class="sxs-lookup"><span data-stu-id="b2f45-225">The type of an interface indexer must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-member-access"></a><span data-ttu-id="b2f45-226">Dostęp do elementu członkowskiego interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-226">Interface member access</span></span>

<span data-ttu-id="b2f45-227">Elementy członkowskie interfejsu są dostępne za pośrednictwem dostępu do elementu członkowskiego ([dostęp do elementu członkowskiego](expressions.md#member-access)) i dostęp indeksatora ([dostęp indeksatora](expressions.md#indexer-access)) wyrażeń formularza `I.M` i `I[A]`, gdzie `I` jest typem interfejsu `M` jest metoda, właściwość lub zdarzenie tego typu interfejsu i `A` to lista argumentu indeksatora.</span><span class="sxs-lookup"><span data-stu-id="b2f45-227">Interface members are accessed through member access ([Member access](expressions.md#member-access)) and indexer access ([Indexer access](expressions.md#indexer-access)) expressions of the form `I.M` and `I[A]`, where `I` is an interface type, `M` is a method, property, or event of that interface type, and `A` is an indexer argument list.</span></span>

<span data-ttu-id="b2f45-228">Dla interfejsów, które są ściśle pojedyncze dziedziczenie (każdego interfejsu w łańcuch dziedziczenia ma dokładnie zero lub jeden interfejs podstawowy bezpośrednich), wpływ wyszukanie członka ([wyszukanie członka](expressions.md#member-lookup)), wywołanie metody ([ Wywołań metod](expressions.md#method-invocations)), a dostęp indeksatora ([dostęp indeksatora](expressions.md#indexer-access)) reguły są dokładnie takie same jak dla klasy i struktury: Ukryj członków więcej uzyskiwana mniej pochodnego elementów członkowskich z tej samej nazwie lub podpisie.</span><span class="sxs-lookup"><span data-stu-id="b2f45-228">For interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effects of the member lookup ([Member lookup](expressions.md#member-lookup)), method invocation ([Method invocations](expressions.md#method-invocations)), and indexer access ([Indexer access](expressions.md#indexer-access)) rules are exactly the same as for classes and structs: More derived members hide less derived members with the same name or signature.</span></span> <span data-ttu-id="b2f45-229">Jednak dla interfejsów dziedziczenia wielokrotnego niejednoznaczności może wystąpić, gdy dwie lub więcej niepowiązanych interfejsy podstawowe zadeklarować składowych o tej samej nazwie lub podpisie.</span><span class="sxs-lookup"><span data-stu-id="b2f45-229">However, for multiple-inheritance interfaces, ambiguities can occur when two or more unrelated base interfaces declare members with the same name or signature.</span></span> <span data-ttu-id="b2f45-230">W tej sekcji przedstawiono kilka przykładów takich sytuacji.</span><span class="sxs-lookup"><span data-stu-id="b2f45-230">This section shows several examples of such situations.</span></span> <span data-ttu-id="b2f45-231">We wszystkich przypadkach ma jawnych rzutowań może służyć do rozwiązywania niejednoznaczności.</span><span class="sxs-lookup"><span data-stu-id="b2f45-231">In all cases, explicit casts can be used to resolve the ambiguities.</span></span>

<span data-ttu-id="b2f45-232">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="b2f45-232">In the example</span></span>
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
<span data-ttu-id="b2f45-233">pierwsze dwie instrukcje powodują błędy czasu kompilacji, ponieważ wyszukanie członka ([wyszukanie członka](expressions.md#member-lookup)) z `Count` w `IListCounter` jest niejednoznaczna.</span><span class="sxs-lookup"><span data-stu-id="b2f45-233">the first two statements cause compile-time errors because the member lookup ([Member lookup](expressions.md#member-lookup)) of `Count` in `IListCounter` is ambiguous.</span></span> <span data-ttu-id="b2f45-234">Jak pokazano w przykładzie, niejednoznaczności został rozwiązany przez rzutowanie `x` do typu odpowiedniego do interfejsu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="b2f45-234">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="b2f45-235">Takie rzutowania mieć żadnych kosztów środowiska wykonawczego — jedynie składają się one wyświetlania wystąpienia jako mniej pochodnego typu w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="b2f45-235">Such casts have no run-time costs—they merely consist of viewing the instance as a less derived type at compile-time.</span></span>

<span data-ttu-id="b2f45-236">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="b2f45-236">In the example</span></span>
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
<span data-ttu-id="b2f45-237">wywołanie `n.Add(1)` wybiera `IInteger.Add` , stosując zasady rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="b2f45-237">the invocation `n.Add(1)` selects `IInteger.Add` by applying the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="b2f45-238">Podobnie wywołanie `n.Add(1.0)` wybiera `IDouble.Add`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-238">Similarly the invocation `n.Add(1.0)` selects `IDouble.Add`.</span></span> <span data-ttu-id="b2f45-239">Podczas wstawiania ma jawnych rzutowań, istnieje tylko jeden Release candidate metody i w związku z tym żadnych niejednoznaczności.</span><span class="sxs-lookup"><span data-stu-id="b2f45-239">When explicit casts are inserted, there is only one candidate method, and thus no ambiguity.</span></span>

<span data-ttu-id="b2f45-240">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="b2f45-240">In the example</span></span>
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
<span data-ttu-id="b2f45-241">`IBase.F` składowa ukryta przez `ILeft.F` elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="b2f45-241">the `IBase.F` member is hidden by the `ILeft.F` member.</span></span> <span data-ttu-id="b2f45-242">Wywołanie `d.F(1)` wybiera w związku z tym `ILeft.F`, nawet jeśli `IBase.F` wydaje się nie być ukryte w ścieżce dostępu, który prowadzi przez `IRight`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-242">The invocation `d.F(1)` thus selects `ILeft.F`, even though `IBase.F` appears to not be hidden in the access path that leads through `IRight`.</span></span>

<span data-ttu-id="b2f45-243">Intuicyjne reguły dla ukrywanie w interfejsach dziedziczenia wielokrotnego to po prostu: Jeśli element członkowski jest ukryty w dowolnej ścieżce dostęp, jest on ukryty wszystkich ścieżek dostępu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-243">The intuitive rule for hiding in multiple-inheritance interfaces is simply this: If a member is hidden in any access path, it is hidden in all access paths.</span></span> <span data-ttu-id="b2f45-244">Ponieważ ścieżka dostępu z `IDerived` do `ILeft` do `IBase` ukrywa `IBase.F`, należy również jest ukryty w ścieżce dostępu z `IDerived` do `IRight` do `IBase`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-244">Because the access path from `IDerived` to `ILeft` to `IBase` hides `IBase.F`, the member is also hidden in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

## <a name="fully-qualified-interface-member-names"></a><span data-ttu-id="b2f45-245">Interfejs w pełni kwalifikowanej nazwy elementów członkowskich</span><span class="sxs-lookup"><span data-stu-id="b2f45-245">Fully qualified interface member names</span></span>

<span data-ttu-id="b2f45-246">Elementu członkowskiego interfejsu nazywa się czasem za jego ***w pełni kwalifikowana nazwa***.</span><span class="sxs-lookup"><span data-stu-id="b2f45-246">An interface member is sometimes referred to by its ***fully qualified name***.</span></span> <span data-ttu-id="b2f45-247">W pełni kwalifikowaną nazwę elementu członkowskiego interfejsu składa się z nazwy interfejsu, w którym składowa jest zadeklarowana, następuje kropka, następuje nazwa elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="b2f45-247">The fully qualified name of an interface member consists of the name of the interface in which the member is declared, followed by a dot, followed by the name of the member.</span></span> <span data-ttu-id="b2f45-248">W pełni kwalifikowaną nazwę elementu członkowskiego odwołuje się do interfejsu, w którym zadeklarowany jest elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="b2f45-248">The fully qualified name of a member references the interface in which the member is declared.</span></span> <span data-ttu-id="b2f45-249">Na przykład biorąc pod uwagę deklaracji</span><span class="sxs-lookup"><span data-stu-id="b2f45-249">For example, given the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
<span data-ttu-id="b2f45-250">w pełni kwalifikowana nazwa `Paint` jest `IControl.Paint` i w pełni kwalifikowana nazwa `SetText` jest `ITextBox.SetText`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-250">the fully qualified name of `Paint` is `IControl.Paint` and the fully qualified name of `SetText` is `ITextBox.SetText`.</span></span>

<span data-ttu-id="b2f45-251">W powyższym przykładzie nie jest możliwe do odwoływania się do `Paint` jako `ITextBox.Paint`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-251">In the example above, it is not possible to refer to `Paint` as `ITextBox.Paint`.</span></span>

<span data-ttu-id="b2f45-252">Gdy interfejs jest częścią przestrzeni nazw, w pełni kwalifikowaną nazwę elementu członkowskiego interfejsu zawiera nazwę przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="b2f45-252">When an interface is part of a namespace, the fully qualified name of an interface member includes the namespace name.</span></span> <span data-ttu-id="b2f45-253">Na przykład</span><span class="sxs-lookup"><span data-stu-id="b2f45-253">For example</span></span>
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

<span data-ttu-id="b2f45-254">Tutaj, w pełni kwalifikowana nazwa `Clone` metodą jest `System.ICloneable.Clone`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-254">Here, the fully qualified name of the `Clone` method is `System.ICloneable.Clone`.</span></span>

## <a name="interface-implementations"></a><span data-ttu-id="b2f45-255">Implementacje interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-255">Interface implementations</span></span>

<span data-ttu-id="b2f45-256">Interfejsy mogą być wykonywane przez klas i struktur.</span><span class="sxs-lookup"><span data-stu-id="b2f45-256">Interfaces may be implemented by classes and structs.</span></span> <span data-ttu-id="b2f45-257">Aby wskazać, że klasa lub struktura bezpośrednio implementuje interfejs, identyfikator interfejsu znajduje się na liście klas bazowych, klasy lub struktury.</span><span class="sxs-lookup"><span data-stu-id="b2f45-257">To indicate that a class or struct directly implements an interface, the interface identifier is included in the base class list of the class or struct.</span></span> <span data-ttu-id="b2f45-258">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b2f45-258">For example:</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

<span data-ttu-id="b2f45-259">Klasa lub struktura, która bezpośrednio implementuje interfejs także bezpośrednio implementuje wszystkie interfejsy podstawowe interfejsu niejawnie.</span><span class="sxs-lookup"><span data-stu-id="b2f45-259">A class or struct that directly implements an interface also directly implements all of the interface's base interfaces implicitly.</span></span> <span data-ttu-id="b2f45-260">Ta zasada obowiązuje, nawet jeśli w klasie lub strukturze nie jawnie wszystkie interfejsy podstawowe na liście klas bazowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-260">This is true even if the class or struct doesn't explicitly list all base interfaces in the base class list.</span></span> <span data-ttu-id="b2f45-261">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b2f45-261">For example:</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

<span data-ttu-id="b2f45-262">W tym miejscu klasy `TextBox` implementuje interfejsy `IControl` i `ITextBox`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-262">Here, class `TextBox` implements both `IControl` and `ITextBox`.</span></span>

<span data-ttu-id="b2f45-263">Gdy klasa `C` bezpośrednio implementuje interfejs, wszystkie klasy pochodne od C umożliwiający również implementuj interfejs niejawnie.</span><span class="sxs-lookup"><span data-stu-id="b2f45-263">When a class `C` directly implements an interface, all classes derived from C also implement the interface implicitly.</span></span> <span data-ttu-id="b2f45-264">Interfejsy podstawowe, określonym w deklaracji klasy mogą być typami skonstruowanego interfejsu ([skonstruowany typy](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="b2f45-264">The base interfaces specified in a class declaration can be constructed interface types ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="b2f45-265">Interfejs podstawowy nie może być parametrem typu samodzielnie, chociaż może pociągać za sobą parametry typu, które znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="b2f45-265">A base interface cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span> <span data-ttu-id="b2f45-266">Poniższy kod ilustruje, jak wdrożyć i rozszerzyć typy utworzone klasy:</span><span class="sxs-lookup"><span data-stu-id="b2f45-266">The following code illustrates how a class can implement and extend constructed types:</span></span>
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

<span data-ttu-id="b2f45-267">Interfejsy podstawowe w deklaracji klasy ogólnej musi spełniać reguły unikatowości opisanego w [unikatowości implementowane interfejsy](interfaces.md#uniqueness-of-implemented-interfaces).</span><span class="sxs-lookup"><span data-stu-id="b2f45-267">The base interfaces of a generic class declaration must satisfy the uniqueness rule described in [Uniqueness of implemented interfaces](interfaces.md#uniqueness-of-implemented-interfaces).</span></span>

### <a name="explicit-interface-member-implementations"></a><span data-ttu-id="b2f45-268">W jawnej implementacji elementu członkowskiego</span><span class="sxs-lookup"><span data-stu-id="b2f45-268">Explicit interface member implementations</span></span>

<span data-ttu-id="b2f45-269">Do celów implementacja interfejsy, klasy lub struktury może deklarować ***implementacji elementu członkowskiego interfejsu jawnego***.</span><span class="sxs-lookup"><span data-stu-id="b2f45-269">For purposes of implementing interfaces, a class or struct may declare ***explicit interface member implementations***.</span></span> <span data-ttu-id="b2f45-270">Implementacja interfejsu jawnego członka jest deklaracji metody, właściwości, zdarzenia lub indeksatora, która odwołuje się do interfejsu w pełni kwalifikowaną nazwę elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="b2f45-270">An explicit interface member implementation is a method, property, event, or indexer declaration that references a fully qualified interface member name.</span></span> <span data-ttu-id="b2f45-271">Na przykład</span><span class="sxs-lookup"><span data-stu-id="b2f45-271">For example</span></span>
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

<span data-ttu-id="b2f45-272">W tym miejscu `IDictionary<int,T>.this` i `IDictionary<int,T>.Add` są w jawnej implementacji elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="b2f45-272">Here `IDictionary<int,T>.this` and `IDictionary<int,T>.Add` are explicit interface member implementations.</span></span>

<span data-ttu-id="b2f45-273">W niektórych przypadkach nazwa elementu członkowskiego interfejsu nie może być odpowiednia dla klasy implementującej, w których przypadku składowej interfejsu informacje mogą być wprowadzone przy użyciu jawną implementacją członków.</span><span class="sxs-lookup"><span data-stu-id="b2f45-273">In some cases, the name of an interface member may not be appropriate for the implementing class, in which case the interface member may be implemented using explicit interface member implementation.</span></span> <span data-ttu-id="b2f45-274">Klasy implementującej abstrakcji pliku, na przykład, prawdopodobnie będzie implementować `Close` funkcja elementu członkowskiego, która obowiązuje przy zwalnianiu zasobów plików i zaimplementować `Dispose` metody `IDisposable` interfejs, za pomocą interfejsu jawnego implementacja elementu członkowskiego:</span><span class="sxs-lookup"><span data-stu-id="b2f45-274">A class implementing a file abstraction, for example, would likely implement a `Close` member function that has the effect of releasing the file resource, and implement the `Dispose` method of the `IDisposable` interface using explicit interface member implementation:</span></span>
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

<span data-ttu-id="b2f45-275">Nie jest możliwe uzyskanie dostępu jawną implementacją członków przy użyciu jego w pełni kwalifikowana nazwa wywołania metody, dostęp do właściwości lub indeksatora dostępu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-275">It is not possible to access an explicit interface member implementation through its fully qualified name in a method invocation, property access, or indexer access.</span></span> <span data-ttu-id="b2f45-276">Implementacja interfejsu jawnego członka może zostać oceniony jedynie przez wystąpienie interfejsu i w takim przypadku odwołuje się po prostu jego nazwa elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="b2f45-276">An explicit interface member implementation can only be accessed through an interface instance, and is in that case referenced simply by its member name.</span></span>

<span data-ttu-id="b2f45-277">Jest to błąd czasu kompilacji dla jawną implementacją członków do uwzględnienia modyfikatory dostępu i jest błąd kompilacji w celu uwzględnienia modyfikatorów `abstract`, `virtual`, `override`, lub `static`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-277">It is a compile-time error for an explicit interface member implementation to include access modifiers, and it is a compile-time error to include the modifiers `abstract`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="b2f45-278">W jawnej implementacji elementu członkowskiego ma właściwości o różnej dostępności niż pozostałe elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="b2f45-278">Explicit interface member implementations have different accessibility characteristics than other members.</span></span> <span data-ttu-id="b2f45-279">Ponieważ implementacji elementu członkowskiego interfejsu jawnego nigdy nie są dostępne za pośrednictwem ich w pełni kwalifikowana nazwa w wywołaniu metody lub dostęp do właściwości, są one w sensie prywatnych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-279">Because explicit interface member implementations are never accessible through their fully qualified name in a method invocation or a property access, they are in a sense private.</span></span> <span data-ttu-id="b2f45-280">Jednak ponieważ są one dostępne za pośrednictwem wystąpienia interfejsu, są one w sensie również publicznych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-280">However, since they can be accessed through an interface instance, they are in a sense also public.</span></span>

<span data-ttu-id="b2f45-281">W jawnej implementacji elementu członkowskiego służą dwa podstawowe cele:</span><span class="sxs-lookup"><span data-stu-id="b2f45-281">Explicit interface member implementations serve two primary purposes:</span></span>

*  <span data-ttu-id="b2f45-282">Ponieważ w jawnej implementacji elementu członkowskiego nie są dostępne za pośrednictwem wystąpienia klasy lub struktury, umożliwiają one implementacji interfejsów, które mają być wykluczone z publicznego interfejsu klasy lub struktury.</span><span class="sxs-lookup"><span data-stu-id="b2f45-282">Because explicit interface member implementations are not accessible through class or struct instances, they allow interface implementations to be excluded from the public interface of a class or struct.</span></span> <span data-ttu-id="b2f45-283">Jest to szczególnie przydatne, gdy klasa lub struktura implementuje wewnętrznego interfejsu, który jest nie interesujące użytkownika tej klasy lub struktury.</span><span class="sxs-lookup"><span data-stu-id="b2f45-283">This is particularly useful when a class or struct implements an internal interface that is of no interest to a consumer of that class or struct.</span></span>
*  <span data-ttu-id="b2f45-284">W jawnej implementacji elementu członkowskiego zezwala na Uściślanie składowych interfejsu, z tym samym podpisie.</span><span class="sxs-lookup"><span data-stu-id="b2f45-284">Explicit interface member implementations allow disambiguation of interface members with the same signature.</span></span> <span data-ttu-id="b2f45-285">Bez implementacji elementu członkowskiego interfejsu jawnego byłoby niemożliwe do klasy lub struktury mają różne implementacje interfejsu elementów członkowskich z tym samym podpisie i zwraca typ, jako jej byłyby niemożliwe do klasy lub struktury mają implementacji we wszystkich składowych interfejsu, w tym samym podpisie, ale różne typy zwracane.</span><span class="sxs-lookup"><span data-stu-id="b2f45-285">Without explicit interface member implementations it would be impossible for a class or struct to have different implementations of interface members with the same signature and return type, as would it be impossible for a class or struct to have any implementation at all of interface members with the same signature but with different return types.</span></span>

<span data-ttu-id="b2f45-286">W przypadku jawną implementacją członków był prawidłowy, klasy lub struktury należy nazwać interfejs na swojej liście klas bazowych, zawierający element, którego w pełni kwalifikowana nazwa, typ i typy parametrów dokładnie odpowiadają elementu członkowskiego interfejsu jawnego Implementacja.</span><span class="sxs-lookup"><span data-stu-id="b2f45-286">For an explicit interface member implementation to be valid, the class or struct must name an interface in its base class list that contains a member whose fully qualified name, type, and parameter types exactly match those of the explicit interface member implementation.</span></span> <span data-ttu-id="b2f45-287">W związku z tym w następującej klasy</span><span class="sxs-lookup"><span data-stu-id="b2f45-287">Thus, in the following class</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
<span data-ttu-id="b2f45-288">Deklaracja `IComparable.CompareTo` powoduje błąd w czasie kompilacji, ponieważ `IComparable` nie znajduje się na liście klas bazowych z `Shape` i nie jest interfejsem podstawowy `ICloneable`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-288">the declaration of `IComparable.CompareTo` results in a compile-time error because `IComparable` is not listed in the base class list of `Shape` and is not a base interface of `ICloneable`.</span></span> <span data-ttu-id="b2f45-289">Podobnie w deklaracjach</span><span class="sxs-lookup"><span data-stu-id="b2f45-289">Likewise, in the declarations</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
<span data-ttu-id="b2f45-290">Deklaracja `ICloneable.Clone` w `Ellipse` powoduje błąd w czasie kompilacji, ponieważ `ICloneable` nie jest jawnie wymienione na liście klas bazowych z `Ellipse`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-290">the declaration of `ICloneable.Clone` in `Ellipse` results in a compile-time error because `ICloneable` is not explicitly listed in the base class list of `Ellipse`.</span></span>

<span data-ttu-id="b2f45-291">Interfejs, w którym zadeklarowano ten element członkowski musi odwoływać się w pełni kwalifikowaną nazwę elementu członkowskiego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-291">The fully qualified name of an interface member must reference the interface in which the member was declared.</span></span> <span data-ttu-id="b2f45-292">W efekcie w deklaracjach</span><span class="sxs-lookup"><span data-stu-id="b2f45-292">Thus, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
<span data-ttu-id="b2f45-293">jawną implementacją członków z `Paint` musi być napisana jako `IControl.Paint`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-293">the explicit interface member implementation of `Paint` must be written as `IControl.Paint`.</span></span>

### <a name="uniqueness-of-implemented-interfaces"></a><span data-ttu-id="b2f45-294">Unikatowość implementowanych interfejsów</span><span class="sxs-lookup"><span data-stu-id="b2f45-294">Uniqueness of implemented interfaces</span></span>

<span data-ttu-id="b2f45-295">Interfejsy implementowane przez deklarację typu ogólnego musi pozostać unikatowe dla wszystkich możliwych typów skonstruowany.</span><span class="sxs-lookup"><span data-stu-id="b2f45-295">The interfaces implemented by a generic type declaration must remain unique for all possible constructed types.</span></span> <span data-ttu-id="b2f45-296">Bez tej reguły byłoby niemożliwe do należy określić poprawną metodę do wywołania dla niektórych typów skonstruowany.</span><span class="sxs-lookup"><span data-stu-id="b2f45-296">Without this rule, it would be impossible to determine the correct method to call for certain constructed types.</span></span> <span data-ttu-id="b2f45-297">Na przykład załóżmy, że zezwolono deklaracji klasy ogólnej do zapisania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b2f45-297">For example, suppose a generic class declaration were permitted to be written as follows:</span></span>
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

<span data-ttu-id="b2f45-298">Była to dozwolone, byłoby niemożliwe do należy określić które kodu do wykonania w następujących przypadkach:</span><span class="sxs-lookup"><span data-stu-id="b2f45-298">Were this permitted, it would be impossible to determine which code to execute in the following case:</span></span>
```csharp
I<int> x = new X<int,int>();
x.F();
```

<span data-ttu-id="b2f45-299">Aby ustalić, czy lista interfejsu deklaracją typu ogólnego jest prawidłowy, wykonywane są następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="b2f45-299">To determine if the interface list of a generic type declaration is valid, the following steps are performed:</span></span>

*  <span data-ttu-id="b2f45-300">Pozwól `L` się na liście interfejsów bezpośrednio określone w ogólnej klasy, struktury lub interfejsu deklaracji `C`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-300">Let `L` be the list of interfaces directly specified in a generic class, struct, or interface declaration `C`.</span></span>
*  <span data-ttu-id="b2f45-301">Dodaj do `L` podstawowa dowolne interfejsy interfejsów, które są już w `L`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-301">Add to `L` any base interfaces of the interfaces already in `L`.</span></span>
*  <span data-ttu-id="b2f45-302">Usuń duplikaty z `L`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-302">Remove any duplicates from `L`.</span></span>
*  <span data-ttu-id="b2f45-303">Jeśli wszystkie możliwe zbudowany typ utworzone na podstawie `C` będzie po argumentów typu są zastępowane do `L`, spowodować, że dwa interfejsy w `L` być identyczne i następnie deklaracji `C` jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="b2f45-303">If any possible constructed type created from `C` would, after type arguments are substituted into `L`, cause two interfaces in `L` to be identical, then the declaration of `C` is invalid.</span></span> <span data-ttu-id="b2f45-304">Deklaracje ograniczenia nie są uwzględniane podczas określania wszystkie możliwe typy utworzone.</span><span class="sxs-lookup"><span data-stu-id="b2f45-304">Constraint declarations are not considered when determining all possible constructed types.</span></span>

<span data-ttu-id="b2f45-305">W deklaracji klasy `X` powyżej listy interfejsu `L` składa się z `I<U>` i `I<V>`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-305">In the class declaration `X` above, the interface list `L` consists of `I<U>` and `I<V>`.</span></span> <span data-ttu-id="b2f45-306">Deklaracja jest nieprawidłowy, ponieważ skonstruowany dowolnego typu za pomocą `U` i `V` są tego samego typu spowodowałoby te dwa interfejsy być identyczne typy.</span><span class="sxs-lookup"><span data-stu-id="b2f45-306">The declaration is invalid because any constructed type with `U` and `V` being the same type would cause these two interfaces to be identical types.</span></span>

<span data-ttu-id="b2f45-307">Możliwe jest, określonych na dziedziczenie różnych poziomach w celu ujednolicenie interfejsów:</span><span class="sxs-lookup"><span data-stu-id="b2f45-307">It is possible for interfaces specified at different inheritance levels to unify:</span></span>
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

<span data-ttu-id="b2f45-308">Mimo że ten kod jest prawidłowy `Derived<U,V>` implementuje interfejsy `I<U>` i `I<V>`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-308">This code is valid even though `Derived<U,V>` implements both `I<U>` and `I<V>`.</span></span> <span data-ttu-id="b2f45-309">Kod</span><span class="sxs-lookup"><span data-stu-id="b2f45-309">The code</span></span>
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
<span data-ttu-id="b2f45-310">wywołuje metodę w `Derived`, ponieważ `Derived<int,int>` w praktyce oznacza ponowne wdrożenie `I<int>` ([interfejs ponowną implementację](interfaces.md#interface-re-implementation)).</span><span class="sxs-lookup"><span data-stu-id="b2f45-310">invokes the method in `Derived`, since `Derived<int,int>` effectively re-implements `I<int>` ([Interface re-implementation](interfaces.md#interface-re-implementation)).</span></span>

### <a name="implementation-of-generic-methods"></a><span data-ttu-id="b2f45-311">Implementacja metody ogólne</span><span class="sxs-lookup"><span data-stu-id="b2f45-311">Implementation of generic methods</span></span>

<span data-ttu-id="b2f45-312">Gdy metody ogólnej niejawnie implementuje metodę interfejsu ograniczeń dla każdego parametr typu metody musi być równoważna w obu deklaracjach (po dowolny typ interfejsu parametry są zastępowane odpowiednie argumenty typu), gdzie — metoda Parametry typu są identyfikowane przez numer porządkowy pozycji, od lewej do prawej.</span><span class="sxs-lookup"><span data-stu-id="b2f45-312">When a generic method implicitly implements an interface method, the constraints given for each method type parameter must be equivalent in both declarations (after any interface type parameters are replaced with the appropriate type arguments), where method type parameters are identified by ordinal positions, left to right.</span></span>

<span data-ttu-id="b2f45-313">Gdy metody ogólnej jawnie implementuje metodę interfejsu, jednak bez ograniczeń są dozwolone dla metody wykonawcze.</span><span class="sxs-lookup"><span data-stu-id="b2f45-313">When a generic method explicitly implements an interface method, however, no constraints are allowed on the implementing method.</span></span> <span data-ttu-id="b2f45-314">Zamiast tego ograniczenia są dziedziczone z metody interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-314">Instead, the constraints are inherited from the interface method</span></span>

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

<span data-ttu-id="b2f45-315">Metoda `C.F<T>` niejawnie implementuje `I<object,C,string>.F<T>`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-315">The method `C.F<T>` implicitly implements `I<object,C,string>.F<T>`.</span></span> <span data-ttu-id="b2f45-316">W tym przypadku `C.F<T>` jest nie wymagane (ani nie dozwolone) do określenia ograniczenie `T:object` ponieważ `object` niejawne ograniczenia dotyczące wszystkich parametrów typu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-316">In this case, `C.F<T>` is not required (nor permitted) to specify the constraint `T:object` since `object` is an implicit constraint on all type parameters.</span></span> <span data-ttu-id="b2f45-317">Metoda `C.G<T>` niejawnie implementuje `I<object,C,string>.G<T>` ponieważ ograniczenia odpowiadają w interfejsie, po zastąpieniu parametry typu interface odpowiednie argumenty typu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-317">The method `C.G<T>` implicitly implements `I<object,C,string>.G<T>` because the constraints match those in the interface, after the interface type parameters are replaced with the corresponding type arguments.</span></span> <span data-ttu-id="b2f45-318">Ograniczenie dla metody `C.H<T>` występuje błąd, ponieważ zamknięte typy (`string` w tym przypadku) nie może być używane jako warunki ograniczające.</span><span class="sxs-lookup"><span data-stu-id="b2f45-318">The constraint for method `C.H<T>` is an error because sealed types (`string` in this case) cannot be used as constraints.</span></span> <span data-ttu-id="b2f45-319">Pominięcie tego ograniczenia będą również błąd, ponieważ ograniczenia implementacje metod interfejsu niejawne muszą być zgodne.</span><span class="sxs-lookup"><span data-stu-id="b2f45-319">Omitting the constraint would also be an error since constraints of implicit interface method implementations are required to match.</span></span> <span data-ttu-id="b2f45-320">W związku z tym, nie jest możliwe implementować niejawnie `I<object,C,string>.H<T>`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-320">Thus, it is impossible to implicitly implement `I<object,C,string>.H<T>`.</span></span> <span data-ttu-id="b2f45-321">Ta metoda interfejsu można zaimplementować tylko przy użyciu jawną implementacją członków:</span><span class="sxs-lookup"><span data-stu-id="b2f45-321">This interface method can only be implemented using an explicit interface member implementation:</span></span>
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

<span data-ttu-id="b2f45-322">W tym przykładzie jawną implementacją członków wywołuje metodę publiczną o ściśle słabszy ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="b2f45-322">In this example, the explicit interface member implementation invokes a public method having strictly weaker constraints.</span></span> <span data-ttu-id="b2f45-323">Należy pamiętać, że przypisanie z `t` do `s` jest prawidłowy, ponieważ `T` dziedziczy z ograniczeniem `T:string`, mimo że to ograniczenie nie ma można wyrazić w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="b2f45-323">Note that the assignment from `t` to `s` is valid since `T` inherits a constraint of `T:string`, even though this constraint is not expressible in source code.</span></span>

### <a name="interface-mapping"></a><span data-ttu-id="b2f45-324">Mapowanie interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-324">Interface mapping</span></span>

<span data-ttu-id="b2f45-325">Klasy lub struktury, należy podać implementacji wszystkich członków interfejsów, które są wymienione na liście klas bazowych, klasy lub struktury.</span><span class="sxs-lookup"><span data-stu-id="b2f45-325">A class or struct must provide implementations of all members of the interfaces that are listed in the base class list of the class or struct.</span></span> <span data-ttu-id="b2f45-326">Proces trwa lokalizowanie implementacji członków interfejsów w implementujące klasy lub struktury jest znany jako ***mapowania interfejsu***.</span><span class="sxs-lookup"><span data-stu-id="b2f45-326">The process of locating implementations of interface members in an implementing class or struct is known as ***interface mapping***.</span></span>

<span data-ttu-id="b2f45-327">Mapowanie interfejsu dla klasy lub struktury `C` lokalizuje implementację dla każdego elementu członkowskiego każdy interfejs określony na liście klas bazowych z `C`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-327">Interface mapping for a class or struct `C` locates an implementation for each member of each interface specified in the base class list of `C`.</span></span> <span data-ttu-id="b2f45-328">Implementacja interfejsu określonego elementu członkowskiego `I.M`, gdzie `I` interfejs, w którym element członkowski `M` zadeklarowano, jest określana przez badanie każda klasa lub struktura `S`, począwszy od `C` i Powtarzanie dla każdego kolejnych klasę bazową `C`, dopóki nie znajduje się dopasowania:</span><span class="sxs-lookup"><span data-stu-id="b2f45-328">The implementation of a particular interface member `I.M`, where `I` is the interface in which the member `M` is declared, is determined by examining each class or struct `S`, starting with `C` and repeating for each successive base class of `C`, until a match is located:</span></span>

*  <span data-ttu-id="b2f45-329">Jeśli `S` zawiera deklarację jawnej implementacji elementu członkowskiego, który odpowiada `I` i `M`, ten element członkowski jest implementacja `I.M`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-329">If `S` contains a declaration of an explicit interface member implementation that matches `I` and `M`, then this member is the implementation of `I.M`.</span></span>
*  <span data-ttu-id="b2f45-330">W przeciwnym razie, jeśli `S` zawiera deklarację niestatycznej składowej publiczny, który odpowiada `M`, ten element członkowski jest implementacja `I.M`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-330">Otherwise, if `S` contains a declaration of a non-static public member that matches `M`, then this member is the implementation of `I.M`.</span></span> <span data-ttu-id="b2f45-331">Jeśli więcej niż jeden element członkowski są zgodne, jest nieokreślona składowej, która jest implementacją `I.M`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-331">If more than one member matches, it is unspecified which member is the implementation of `I.M`.</span></span> <span data-ttu-id="b2f45-332">Taka sytuacja może wystąpić tylko, jeśli `S` jest skonstruowanego typu, w której dwa elementy członkowskie zadeklarowane w typie ogólnym mają różnych podpisów, ale argumentów typu należy ich podpisy identyczne.</span><span class="sxs-lookup"><span data-stu-id="b2f45-332">This situation can only occur if `S` is a constructed type where the two members as declared in the generic type have different signatures, but the type arguments make their signatures identical.</span></span>

<span data-ttu-id="b2f45-333">Występuje błąd kompilacji, jeśli implementacje nie można znaleźć dla wszystkich elementów członkowskich wszystkie interfejsy określone na liście klas bazowych, z `C`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-333">A compile-time error occurs if implementations cannot be located for all members of all interfaces specified in the base class list of `C`.</span></span> <span data-ttu-id="b2f45-334">Należy pamiętać, że Członkowie interfejsu obejmują te elementy członkowskie, które są dziedziczone z interfejsy podstawowe.</span><span class="sxs-lookup"><span data-stu-id="b2f45-334">Note that the members of an interface include those members that are inherited from base interfaces.</span></span>

<span data-ttu-id="b2f45-335">Na potrzeby mapowania interfejsu, element członkowski klasy `A` pasuje do elementu członkowskiego interfejsu `B` po:</span><span class="sxs-lookup"><span data-stu-id="b2f45-335">For purposes of interface mapping, a class member `A` matches an interface member `B` when:</span></span>

*  <span data-ttu-id="b2f45-336">`A` i `B` są metody i nazwę typu, a list parametrów formalnych `A` i `B` są identyczne.</span><span class="sxs-lookup"><span data-stu-id="b2f45-336">`A` and `B` are methods, and the name, type, and formal parameter lists of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="b2f45-337">`A` i `B` są właściwości, nazwę i typ `A` i `B` są identyczne, i `A` ma tej samej metody dostępu jako `B` (`A` może mieć dodatkowe metody dostępu, jeśli nie jest interfejsem jawne implementacja elementu członkowskiego).</span><span class="sxs-lookup"><span data-stu-id="b2f45-337">`A` and `B` are properties, the name and type of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>
*  <span data-ttu-id="b2f45-338">`A` i `B` są zdarzenia oraz nazwę i typ `A` i `B` są identyczne.</span><span class="sxs-lookup"><span data-stu-id="b2f45-338">`A` and `B` are events, and the name and type of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="b2f45-339">`A` i `B` indeksatorów, typ i listy parametrów formalnych `A` i `B` są identyczne, i `A` ma tej samej metody dostępu jako `B` (`A` mają dodatkowe metody dostępu, jeśli nie jest dozwolone jawną implementacją członków).</span><span class="sxs-lookup"><span data-stu-id="b2f45-339">`A` and `B` are indexers, the type and formal parameter lists of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>

<span data-ttu-id="b2f45-340">Dostępne są następujące istotne implikacje algorytm mapowania interfejsu:</span><span class="sxs-lookup"><span data-stu-id="b2f45-340">Notable implications of the interface mapping algorithm are:</span></span>

*  <span data-ttu-id="b2f45-341">W jawnej implementacji elementu członkowskiego mają pierwszeństwo przed innych członków w tej samej klasie lub strukturze podczas określania składowej klasy lub struktury, która implementuje składową interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-341">Explicit interface member implementations take precedence over other members in the same class or struct when determining the class or struct member that implements an interface member.</span></span>
*  <span data-ttu-id="b2f45-342">Niepubliczne ani statyczne elementy członkowskie uczestniczyć w mapowania interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-342">Neither non-public nor static members participate in interface mapping.</span></span>

<span data-ttu-id="b2f45-343">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="b2f45-343">In the example</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
<span data-ttu-id="b2f45-344">`ICloneable.Clone` członkiem `C` staje się implementacja `Clone` w `ICloneable` ponieważ implementacji elementu członkowskiego interfejsu jawnego mają pierwszeństwo przed innymi członkami.</span><span class="sxs-lookup"><span data-stu-id="b2f45-344">the `ICloneable.Clone` member of `C` becomes the implementation of `Clone` in `ICloneable` because explicit interface member implementations take precedence over other members.</span></span>

<span data-ttu-id="b2f45-345">Jeśli klasa lub struktura implementuje dwa lub więcej interfejsów zawierający element członkowski o takiej samej nazwie, typie i typy parametrów, jest możliwe mapowanie każdego z tych składowych interfejsu na jeden element członkowski klasy lub struktury.</span><span class="sxs-lookup"><span data-stu-id="b2f45-345">If a class or struct implements two or more interfaces containing a member with the same name, type, and parameter types, it is possible to map each of those interface members onto a single class or struct member.</span></span> <span data-ttu-id="b2f45-346">Na przykład</span><span class="sxs-lookup"><span data-stu-id="b2f45-346">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

<span data-ttu-id="b2f45-347">W tym miejscu `Paint` obu metod `IControl` i `IForm` są mapowane na `Paint` method in Class metoda `Page`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-347">Here, the `Paint` methods of both `IControl` and `IForm` are mapped onto the `Paint` method in `Page`.</span></span> <span data-ttu-id="b2f45-348">Oczywiście również istnieje możliwość mają oddzielne interfejsu jawnego implementacji elementu członkowskiego dla obu metod.</span><span class="sxs-lookup"><span data-stu-id="b2f45-348">It is of course also possible to have separate explicit interface member implementations for the two methods.</span></span>

<span data-ttu-id="b2f45-349">Jeśli klasa lub struktura implementuje interfejs, który zawiera ukryte elementy członkowskie, niektóre elementy członkowskie niekoniecznie musi być implementowany przez implementacji elementu członkowskiego interfejsu jawnego.</span><span class="sxs-lookup"><span data-stu-id="b2f45-349">If a class or struct implements an interface that contains hidden members, then some members must necessarily be implemented through explicit interface member implementations.</span></span> <span data-ttu-id="b2f45-350">Na przykład</span><span class="sxs-lookup"><span data-stu-id="b2f45-350">For example</span></span>
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

<span data-ttu-id="b2f45-351">Implementację tego interfejsu wymaga co najmniej jedną jawną implementacją członków, a następnie może wykonać jedną z następujących form</span><span class="sxs-lookup"><span data-stu-id="b2f45-351">An implementation of this interface would require at least one explicit interface member implementation, and would take one of the following forms</span></span>
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

<span data-ttu-id="b2f45-352">Gdy klasa implementuje wiele interfejsów, które mają ten sam interfejs podstawowy, może istnieć tylko jedna implementacja interfejs podstawowy.</span><span class="sxs-lookup"><span data-stu-id="b2f45-352">When a class implements multiple interfaces that have the same base interface, there can be only one implementation of the base interface.</span></span> <span data-ttu-id="b2f45-353">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="b2f45-353">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
<span data-ttu-id="b2f45-354">nie jest możliwe mieć osobne implementacje dla `IControl` o nazwie na liście klas bazowych `IControl` dziedziczone przez `ITextBox`i `IControl` dziedziczone przez `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-354">it is not possible to have separate implementations for the `IControl` named in the base class list, the `IControl` inherited by `ITextBox`, and the `IControl` inherited by `IListBox`.</span></span> <span data-ttu-id="b2f45-355">W rzeczywistości nie są używane osobne tożsamości dla tych interfejsów.</span><span class="sxs-lookup"><span data-stu-id="b2f45-355">Indeed, there is no notion of a separate identity for these interfaces.</span></span> <span data-ttu-id="b2f45-356">Zamiast implementacje `ITextBox` i `IListBox` udostępnianie tego samego wdrożenia `IControl`, i `ComboBox` uznaje się wystarczy do zaimplementowania trzy interfejsy `IControl`, `ITextBox`, i `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-356">Rather, the implementations of `ITextBox` and `IListBox` share the same implementation of `IControl`, and `ComboBox` is simply considered to implement three interfaces, `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="b2f45-357">Elementy członkowskie klasy bazowej uczestniczyć w mapowania interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-357">The members of a base class participate in interface mapping.</span></span> <span data-ttu-id="b2f45-358">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="b2f45-358">In the example</span></span>
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
<span data-ttu-id="b2f45-359">Metoda `F` w `Class1` jest używany w `Class2`przez implementację `Interface1`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-359">the method `F` in `Class1` is used in `Class2`'s implementation of `Interface1`.</span></span>

### <a name="interface-implementation-inheritance"></a><span data-ttu-id="b2f45-360">Dziedziczenie implementacji interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-360">Interface implementation inheritance</span></span>

<span data-ttu-id="b2f45-361">Klasa dziedziczy wszystkie implementacje interfejsu udostępniane przez jej klas podstawowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-361">A class inherits all interface implementations provided by its base classes.</span></span>

<span data-ttu-id="b2f45-362">Bez jawnie ***ponownej implementacji*** interfejsu klasy pochodnej nie może w żaden sposób zmieniać mapowania interfejs dziedziczy jej klas podstawowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-362">Without explicitly ***re-implementing*** an interface, a derived class cannot in any way alter the interface mappings it inherits from its base classes.</span></span> <span data-ttu-id="b2f45-363">Na przykład w deklaracji</span><span class="sxs-lookup"><span data-stu-id="b2f45-363">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
<span data-ttu-id="b2f45-364">`Paint` method in Class metoda `TextBox` ukrywa `Paint` method in Class metoda `Control`, ale nie zmienia mapowania `Control.Paint` na `IControl.Paint`i zwraca `Paint` za pośrednictwem klasy będzie wystąpień i wystąpienia interfejsu ma następujące skutki</span><span class="sxs-lookup"><span data-stu-id="b2f45-364">the `Paint` method in `TextBox` hides the `Paint` method in `Control`, but it does not alter the mapping of `Control.Paint` onto `IControl.Paint`, and calls to `Paint` through class instances and interface instances will have the following effects</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

<span data-ttu-id="b2f45-365">Jednak gdy metody interfejsu jest mapowana na wirtualnej metody w klasie, jest możliwe dla klasy pochodne, aby przesłonić metodę wirtualną i zmień implementację interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-365">However, when an interface method is mapped onto a virtual method in a class, it is possible for derived classes to override the virtual method and alter the implementation of the interface.</span></span> <span data-ttu-id="b2f45-366">Na przykład poprawiania deklaracje powyżej do</span><span class="sxs-lookup"><span data-stu-id="b2f45-366">For example, rewriting the declarations above to</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
<span data-ttu-id="b2f45-367">teraz będzie można zauważyć następujące efekty</span><span class="sxs-lookup"><span data-stu-id="b2f45-367">the following effects will now be observed</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

<span data-ttu-id="b2f45-368">Ponieważ implementacji elementu członkowskiego interfejsu jawnego nie może być zadeklarowana wirtualnego, nie jest możliwe zastąpienie jawną implementacją członków.</span><span class="sxs-lookup"><span data-stu-id="b2f45-368">Since explicit interface member implementations cannot be declared virtual, it is not possible to override an explicit interface member implementation.</span></span> <span data-ttu-id="b2f45-369">Jednak go nadaje się doskonale do jawną implementacją członków do wywoływania innej metody i czy innej metody mogą być deklarowane wirtualnego Aby zezwolić na pochodne klasy, aby go zastąpić.</span><span class="sxs-lookup"><span data-stu-id="b2f45-369">However, it is perfectly valid for an explicit interface member implementation to call another method, and that other method can be declared virtual to allow derived classes to override it.</span></span> <span data-ttu-id="b2f45-370">Na przykład</span><span class="sxs-lookup"><span data-stu-id="b2f45-370">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

<span data-ttu-id="b2f45-371">W tym miejscu klasy pochodne `Control` można specjalizować wykonania `IControl.Paint` przez zastąpienie `PaintControl` metody.</span><span class="sxs-lookup"><span data-stu-id="b2f45-371">Here, classes derived from `Control` can specialize the implementation of `IControl.Paint` by overriding the `PaintControl` method.</span></span>

### <a name="interface-re-implementation"></a><span data-ttu-id="b2f45-372">Ponowna implementacja interfejsu</span><span class="sxs-lookup"><span data-stu-id="b2f45-372">Interface re-implementation</span></span>

<span data-ttu-id="b2f45-373">Klasę, która dziedziczy implementację interfejsu jest dozwolona na ***ponownego zaimplementowania*** interfejsu, umieszczając je na liście klas bazowych.</span><span class="sxs-lookup"><span data-stu-id="b2f45-373">A class that inherits an interface implementation is permitted to ***re-implement*** the interface by including it in the base class list.</span></span>

<span data-ttu-id="b2f45-374">Ponowna implementacja interfejsu jest zgodna dokładnie tego samego interfejsu reguły mapowania jako początkowej implementacji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-374">A re-implementation of an interface follows exactly the same interface mapping rules as an initial implementation of an interface.</span></span> <span data-ttu-id="b2f45-375">W efekcie mapowanie dziedziczony interfejs nie ma wpływu na mapowania interfejsu ustanowione dla ponowna implementacja interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-375">Thus, the inherited interface mapping has no effect whatsoever on the interface mapping established for the re-implementation of the interface.</span></span> <span data-ttu-id="b2f45-376">Na przykład w deklaracji</span><span class="sxs-lookup"><span data-stu-id="b2f45-376">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
<span data-ttu-id="b2f45-377">fakt, `Control` mapuje `IControl.Paint` na `Control.IControl.Paint` nie ma wpływu na ponowną implementację w `MyControl`, mapująca `IControl.Paint` na `MyControl.Paint`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-377">the fact that `Control` maps `IControl.Paint` onto `Control.IControl.Paint` doesn't affect the re-implementation in `MyControl`, which maps `IControl.Paint` onto `MyControl.Paint`.</span></span>

<span data-ttu-id="b2f45-378">Dziedziczone członka publicznego deklaracje i jawne dziedziczonego elementu członkowskiego, deklaracje uczestniczyć w trakcie mapowania interfejsu ponownie implementowane interfejsy.</span><span class="sxs-lookup"><span data-stu-id="b2f45-378">Inherited public member declarations and inherited explicit interface member declarations participate in the interface mapping process for re-implemented interfaces.</span></span> <span data-ttu-id="b2f45-379">Na przykład</span><span class="sxs-lookup"><span data-stu-id="b2f45-379">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

<span data-ttu-id="b2f45-380">Tutaj, implementacja `IMethods` w `Derived` mapuje metod interfejsu na `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, i `Base.I`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-380">Here, the implementation of `IMethods` in `Derived` maps the interface methods onto `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, and `Base.I`.</span></span>

<span data-ttu-id="b2f45-381">Gdy klasa implementuje interfejs go niejawnie implementuje również wszystkie interfejsy podstawowe tego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b2f45-381">When a class implements an interface, it implicitly also implements all of that interface's base interfaces.</span></span> <span data-ttu-id="b2f45-382">Podobnie, ponowna implementacja interfejsu jest również niejawnie ponowną implementację wszystkich interfejsów, interfejsy podstawowe.</span><span class="sxs-lookup"><span data-stu-id="b2f45-382">Likewise, a re-implementation of an interface is also implicitly a re-implementation of all of the interface's base interfaces.</span></span> <span data-ttu-id="b2f45-383">Na przykład</span><span class="sxs-lookup"><span data-stu-id="b2f45-383">For example</span></span>
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

<span data-ttu-id="b2f45-384">Tutaj, ponowną implementację elementu `IDerived` ponownie implementuje również `IBase`, mapowanie `IBase.F` na `D.F`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-384">Here, the re-implementation of `IDerived` also re-implements `IBase`, mapping `IBase.F` onto `D.F`.</span></span>

### <a name="abstract-classes-and-interfaces"></a><span data-ttu-id="b2f45-385">Klasy abstrakcyjne i interfejsy</span><span class="sxs-lookup"><span data-stu-id="b2f45-385">Abstract classes and interfaces</span></span>

<span data-ttu-id="b2f45-386">Takich jak klasy nieabstrakcyjnej klasy abstrakcyjnej muszą dostarczać implementacje, wszystkich elementów członkowskich interfejsów, które są wymienione na liście klas bazowych, klasy.</span><span class="sxs-lookup"><span data-stu-id="b2f45-386">Like a non-abstract class, an abstract class must provide implementations of all members of the interfaces that are listed in the base class list of the class.</span></span> <span data-ttu-id="b2f45-387">Jednak klasy abstrakcyjnej jest dozwolone metody interfejsu na metody abstrakcyjne mapowania.</span><span class="sxs-lookup"><span data-stu-id="b2f45-387">However, an abstract class is permitted to map interface methods onto abstract methods.</span></span> <span data-ttu-id="b2f45-388">Na przykład</span><span class="sxs-lookup"><span data-stu-id="b2f45-388">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

<span data-ttu-id="b2f45-389">Tutaj, implementacja `IMethods` mapuje `F` i `G` na metody abstrakcyjne, która musi zostać zastąpiona w nieabstrakcyjnej klasy, które wynikają z `C`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-389">Here, the implementation of `IMethods` maps `F` and `G` onto abstract methods, which must be overridden in non-abstract classes that derive from `C`.</span></span>

<span data-ttu-id="b2f45-390">Należy pamiętać, że implementacji elementu członkowskiego interfejsu jawnego nie może być abstrakcyjny, ale implementacji elementu członkowskiego interfejsu jawnego oczywiście są dozwolone do wywołania metody abstrakcyjne.</span><span class="sxs-lookup"><span data-stu-id="b2f45-390">Note that explicit interface member implementations cannot be abstract, but explicit interface member implementations are of course permitted to call abstract methods.</span></span> <span data-ttu-id="b2f45-391">Na przykład</span><span class="sxs-lookup"><span data-stu-id="b2f45-391">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

<span data-ttu-id="b2f45-392">Tutaj, klasy nieabstrakcyjnej, które wynikają z `C` byłoby potrzebnego do zastąpienia `FF` i `GG`, zapewniając w ten sposób wykonania rzeczywistego `IMethods`.</span><span class="sxs-lookup"><span data-stu-id="b2f45-392">Here, non-abstract classes that derive from `C` would be required to override `FF` and `GG`, thus providing the actual implementation of `IMethods`.</span></span>
