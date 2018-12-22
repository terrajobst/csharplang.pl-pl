# <a name="structs"></a><span data-ttu-id="80599-101">Struktury</span><span class="sxs-lookup"><span data-stu-id="80599-101">Structs</span></span>

<span data-ttu-id="80599-102">Struktury są podobne do klasy, w tym, że reprezentują one struktur danych, które mogą zawierać elementy członkowskie danych i funkcji elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="80599-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="80599-103">W przeciwieństwie do klasy, struktury są typami wartości i nie wymagają alokacji sterty.</span><span class="sxs-lookup"><span data-stu-id="80599-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="80599-104">Zmienną typu struktury bezpośrednio zawiera dane struktury, zmienna typu klasy zawiera odwołanie do danych, a ostatni znany jako obiekt.</span><span class="sxs-lookup"><span data-stu-id="80599-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="80599-105">Struktury są szczególnie przydatne w przypadku małych strukturach danych, które mają semantyki wartości.</span><span class="sxs-lookup"><span data-stu-id="80599-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="80599-106">Liczby zespolone, punkty w układzie współrzędnych lub par klucz wartość ze słownika są dobrym przykładem struktury.</span><span class="sxs-lookup"><span data-stu-id="80599-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="80599-107">Klucz do tych struktur danych jest kilka elementów członkowskich danych, że nie wymagają użycia dziedziczenie lub tożsamości referencyjnej i że można je łatwo implementować przy użyciu semantyki wartości lokalizacji przypisania kopiowania wartości zamiast odwołania.</span><span class="sxs-lookup"><span data-stu-id="80599-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="80599-108">Zgodnie z opisem w [typów prostych](types.md#simple-types), typy proste dostarczana przez C#, takie jak `int`, `double`, i `bool`, są w rzeczywistości wszystkie typy struktury.</span><span class="sxs-lookup"><span data-stu-id="80599-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="80599-109">Tak, jak te wstępnie zdefiniowanych typów są strukturami, użytkownik może również używać struktury i przeładowania operatora do wdrożenia nowych typów "pierwotne" w języku C#.</span><span class="sxs-lookup"><span data-stu-id="80599-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="80599-110">Dwa przykłady takich typów znajdują się na końcu tego rozdziału ([przykłady struktury](structs.md#struct-examples)).</span><span class="sxs-lookup"><span data-stu-id="80599-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="80599-111">Deklaracje struktury</span><span class="sxs-lookup"><span data-stu-id="80599-111">Struct declarations</span></span>

<span data-ttu-id="80599-112">A *struct_declaration* jest *type_declaration* ([wpisz deklaracje](namespaces.md#type-declarations)) określa nowa struktura:</span><span class="sxs-lookup"><span data-stu-id="80599-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="80599-113">A *struct_declaration* zawiera opcjonalny zestaw *atrybuty* ([atrybuty](attributes.md)), a następnie opcjonalny zestaw *struct_modifier*s ([Modyfikatory struktury](structs.md#struct-modifiers)), a następnie opcjonalnie `partial` modyfikator, następuje słowa kluczowego `struct` i *identyfikator* nazwy, struktury, a następnie Opcjonalnie *type_parameter_list* specyfikacji ([parametry typu](classes.md#type-parameters)), a następnie opcjonalnie *struct_interfaces* specyfikacji ([Modyfikatora "partial"](structs.md#partial-modifier))), a następnie opcjonalnie *type_parameter_constraints_clause*specyfikacji s ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), a następnie *struct_body* ([treści struktury](structs.md#struct-body)), opcjonalnie następuje średnik.</span><span class="sxs-lookup"><span data-stu-id="80599-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="80599-114">Modyfikatory — struktura</span><span class="sxs-lookup"><span data-stu-id="80599-114">Struct modifiers</span></span>

<span data-ttu-id="80599-115">A *struct_declaration* może opcjonalnie obejmować sekwencję Modyfikatory struktury:</span><span class="sxs-lookup"><span data-stu-id="80599-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

<span data-ttu-id="80599-116">Jest to błąd czasu kompilacji dla tego samego modyfikator pojawią się wiele razy w deklaracji struktury.</span><span class="sxs-lookup"><span data-stu-id="80599-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="80599-117">Modyfikatory deklaracji struktury mają takie samo znaczenie jak deklaracji klasy ([klasy deklaracje](classes.md#class-declarations)).</span><span class="sxs-lookup"><span data-stu-id="80599-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="80599-118">Modyfikatora "Partial"</span><span class="sxs-lookup"><span data-stu-id="80599-118">Partial modifier</span></span>

<span data-ttu-id="80599-119">`partial` Modyfikator wskazuje, że to *struct_declaration* jest deklaracją typu częściowego.</span><span class="sxs-lookup"><span data-stu-id="80599-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="80599-120">Wielu deklaracjach częściowej struktury o takiej samej nazwie w deklaracji otaczającego przestrzeń nazw lub typ są łączone w celu utworzenia jednej deklaracji struktury, zgodnie z regułami określone w [typów częściowych](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="80599-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="80599-121">Interfejsy — struktura</span><span class="sxs-lookup"><span data-stu-id="80599-121">Struct interfaces</span></span>

<span data-ttu-id="80599-122">Deklaracja struktury mogą obejmować *struct_interfaces* specyfikacji, w których przypadku struktury jest nazywany bezpośrednio zaimplementować typy danego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="80599-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="80599-123">Implementacje interfejsu są omówione w dalszych [implementacje interfejsu](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="80599-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="80599-124">Treść — struktura</span><span class="sxs-lookup"><span data-stu-id="80599-124">Struct body</span></span>

<span data-ttu-id="80599-125">*Struct_body* struktury definiuje elementy członkowskie struktury.</span><span class="sxs-lookup"><span data-stu-id="80599-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="80599-126">Składowe struktury</span><span class="sxs-lookup"><span data-stu-id="80599-126">Struct members</span></span>

<span data-ttu-id="80599-127">Elementy członkowskie struktury składają się z elementów członkowskich, wynikające z jego *struct_member_declaration*s i elementy członkowskie są dziedziczone z typu `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="80599-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

<span data-ttu-id="80599-128">Z wyjątkiem różnic, o których wspomniano w [klasy i struktury różnice](structs.md#class-and-struct-differences), opisy elementów członkowskich klasy w [elementy członkowskie klasy](classes.md#class-members) za pośrednictwem [Iteratory](classes.md#iterators) dotyczą — struktura członków, a także.</span><span class="sxs-lookup"><span data-stu-id="80599-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="80599-129">Klasy i struktury różnice</span><span class="sxs-lookup"><span data-stu-id="80599-129">Class and struct differences</span></span>

<span data-ttu-id="80599-130">Struktury różnią się od klas w kilka istotnych różnic:</span><span class="sxs-lookup"><span data-stu-id="80599-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="80599-131">Struktury są typami wartości ([wartość semantyki](structs.md#value-semantics)).</span><span class="sxs-lookup"><span data-stu-id="80599-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="80599-132">Wszystkie typy struktury niejawnie dziedziczą z klasy `System.ValueType` ([dziedziczenia](structs.md#inheritance)).</span><span class="sxs-lookup"><span data-stu-id="80599-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="80599-133">Przypisanie do zmiennej typu struktury tworzona jest kopia wartości przypisywane ([przypisania](structs.md#assignment)).</span><span class="sxs-lookup"><span data-stu-id="80599-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="80599-134">Wartość domyślna struktury to wartości generowane przez ustawienie dla wszystkich pola typu wartości przywrócić wartości domyślne i wszystkie odwołania pola typu do `null` ([wartości domyślne](structs.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="80599-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="80599-135">Operacje pakowania, jak i rozpakowywanie są używane do konwersji między typem struktury i `object` ([pakowania i rozpakowywania](structs.md#boxing-and-unboxing)).</span><span class="sxs-lookup"><span data-stu-id="80599-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="80599-136">Znaczenie `this` różni się dla struktur ([dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="80599-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="80599-137">Deklaracje pola wystąpienia struktury nie mogą zawierać inicjatory zmiennej ([pola inicjatory](structs.md#field-initializers)).</span><span class="sxs-lookup"><span data-stu-id="80599-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="80599-138">Struktura nie ma zezwolenia na Zadeklaruj Konstruktor wystąpienia bez parametrów ([konstruktory](structs.md#constructors)).</span><span class="sxs-lookup"><span data-stu-id="80599-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="80599-139">Struktura nie jest dozwolone było zadeklarowanie destruktora ([destruktory](structs.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="80599-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="80599-140">Semantyka wartości</span><span class="sxs-lookup"><span data-stu-id="80599-140">Value semantics</span></span>

<span data-ttu-id="80599-141">Struktury są typami wartości ([typy wartości](types.md#value-types)) i są określane jako ma wartość semantyki.</span><span class="sxs-lookup"><span data-stu-id="80599-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="80599-142">Klasy, z drugiej strony, są typami odwołań ([typy odwołań](types.md#reference-types)) i jest nazywany ma semantyką odwołań.</span><span class="sxs-lookup"><span data-stu-id="80599-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="80599-143">Zmienną typu struktury bezpośrednio zawiera dane struktury, zmienna typu klasy zawiera odwołanie do danych, a ostatni znany jako obiekt.</span><span class="sxs-lookup"><span data-stu-id="80599-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="80599-144">Gdy struktura `B` zawiera pole wystąpienia typu `A` i `A` jest typem struktury jest to błąd czasu kompilacji dla `A` zależą `B` lub typem skonstruowany na podstawie `B`.</span><span class="sxs-lookup"><span data-stu-id="80599-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="80599-145">Struktura `X` ***zależy od bezpośrednio*** struktury `Y` Jeśli `X` zawiera pole wystąpienia typu `Y`.</span><span class="sxs-lookup"><span data-stu-id="80599-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="80599-146">Biorąc pod uwagę tę definicję, kompletny zestaw struktury, od których zależy struktury jest przechodnia zamknięcia ***zależy od bezpośrednio*** relacji.</span><span class="sxs-lookup"><span data-stu-id="80599-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="80599-147">Na przykład</span><span class="sxs-lookup"><span data-stu-id="80599-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="80599-148">jest to błąd, ponieważ `Node` zawiera swój własny typ pola wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="80599-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="80599-149">Inny przykład</span><span class="sxs-lookup"><span data-stu-id="80599-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="80599-150">jest to błąd, ponieważ każdy z typów `A`, `B`, i `C` są zależne od siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="80599-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="80599-151">W przypadku klas jest możliwe w dwóch zmiennych odwoływać się do tego samego obiektu, a zatem możliwe dla operacji na jednej zmiennej miały wpływ na obiekt odwołuje się druga zmienna.</span><span class="sxs-lookup"><span data-stu-id="80599-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="80599-152">Przy użyciu struktury, zmienne każdego mają własne kopii danych (z wyjątkiem w przypadku właściwości `ref` i `out` zmiennych parametrów), i nie jest możliwe dla operacji na jednym wpłynie na inne.</span><span class="sxs-lookup"><span data-stu-id="80599-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="80599-153">Ponadto, ponieważ struktury nie są typami odwołań, nie jest możliwe w przypadku wartości typu struktury jako `null`.</span><span class="sxs-lookup"><span data-stu-id="80599-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="80599-154">Biorąc pod uwagę deklaracji</span><span class="sxs-lookup"><span data-stu-id="80599-154">Given the declaration</span></span>
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="80599-155">fragment kodu</span><span class="sxs-lookup"><span data-stu-id="80599-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="80599-156">Wyświetla wartość `10`.</span><span class="sxs-lookup"><span data-stu-id="80599-156">outputs the value `10`.</span></span> <span data-ttu-id="80599-157">Przypisywanie `a` do `b` tworzona jest kopia wartości, a `b` związku z tym jest niezależny od przypisania do `a.x`.</span><span class="sxs-lookup"><span data-stu-id="80599-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="80599-158">Gdyby `Point` zamiast tego zostało zadeklarowane jako klasa, dane wyjściowe będą `100` ponieważ `a` i `b` będzie odwoływać się do tego samego obiektu.</span><span class="sxs-lookup"><span data-stu-id="80599-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="80599-159">Dziedziczenie</span><span class="sxs-lookup"><span data-stu-id="80599-159">Inheritance</span></span>

<span data-ttu-id="80599-160">Wszystkie typy struktury niejawnie dziedziczą z klasy `System.ValueType`, która z kolei dziedziczy z klasy `object`.</span><span class="sxs-lookup"><span data-stu-id="80599-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="80599-161">Deklaracja struktura może określić listy implementowanych interfejsów, ale nie jest możliwe dla deklaracji struktury określić klasę bazową.</span><span class="sxs-lookup"><span data-stu-id="80599-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="80599-162">Typy struktury nigdy nie są abstrakcyjne i zawsze niejawnie są zamknięte.</span><span class="sxs-lookup"><span data-stu-id="80599-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="80599-163">`abstract` i `sealed` Modyfikatory w związku z tym są niedozwolone w deklaracji struktury.</span><span class="sxs-lookup"><span data-stu-id="80599-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="80599-164">Ponieważ dziedziczenie nie jest obsługiwane w przypadku struktur, nie może być deklarowana dostępność metody składowej struktury `protected` lub `protected internal`.</span><span class="sxs-lookup"><span data-stu-id="80599-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="80599-165">Funkcji składowych w strukturze nie może być `abstract` lub `virtual`i `override` modyfikator jest dozwolony tylko po to, aby zastąpić metod odziedziczone `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="80599-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="80599-166">Przypisanie</span><span class="sxs-lookup"><span data-stu-id="80599-166">Assignment</span></span>

<span data-ttu-id="80599-167">Przypisanie do zmiennej typu struktury tworzona jest kopia wartości jest przypisane.</span><span class="sxs-lookup"><span data-stu-id="80599-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="80599-168">To różni się od przypisania do zmiennej typu klasy, która kopiuje odwołania, ale nie identyfikowane przez odwołanie do obiektu.</span><span class="sxs-lookup"><span data-stu-id="80599-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="80599-169">Podobnie jak przypisanie, gdy struktury jest przekazywany jako parametr wartość lub zwrócone w wyniku funkcji elementu członkowskiego, tworzona jest kopia struktury.</span><span class="sxs-lookup"><span data-stu-id="80599-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="80599-170">Struktury mogą być przekazywane przez odwołanie do funkcji elementu członkowskiego, używając `ref` lub `out` parametru.</span><span class="sxs-lookup"><span data-stu-id="80599-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="80599-171">Gdy właściwość lub indeksator struktury jest elementem docelowym przypisania, związane z dostępem do właściwości lub indeksatora wyrażenia do wystąpienia muszą być klasyfikowane jako zmienną.</span><span class="sxs-lookup"><span data-stu-id="80599-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="80599-172">Jeśli wyrażenie wystąpienia jest klasyfikowana jako wartość, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="80599-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="80599-173">Jest to opisane w dalszej części [przypisanie proste](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="80599-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="80599-174">Wartości domyślne</span><span class="sxs-lookup"><span data-stu-id="80599-174">Default values</span></span>

<span data-ttu-id="80599-175">Zgodnie z opisem w [wartości domyślne](variables.md#default-values), kilka rodzajów zmienne są automatycznie inicjowane z wartością przywrócić wartości domyślne po ich utworzeniu.</span><span class="sxs-lookup"><span data-stu-id="80599-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="80599-176">Dla zmiennych typu klasy i inne typy odwołań, ta wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="80599-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="80599-177">Jednak ponieważ struktury są typami wartości, które nie może być `null`, wartością domyślną struktury jest wartością, generowane przez ustawienie dla wszystkich pola typu wartości przywrócić wartości domyślne i wszystkie odwołania pola typu do `null`.</span><span class="sxs-lookup"><span data-stu-id="80599-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="80599-178">Odwołujące się do `Point` struktury zadeklarowane powyżej przykładzie</span><span class="sxs-lookup"><span data-stu-id="80599-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="80599-179">Inicjuje każdy `Point` w tablicy wartości generowane przez ustawienie `x` i `y` pola na wartość zero.</span><span class="sxs-lookup"><span data-stu-id="80599-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="80599-180">Wartość domyślna struktury odnosi się do wartości zwracanej przez domyślny konstruktor obiektu struktury ([domyślne konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="80599-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="80599-181">W odróżnieniu od klasy struktury nie jest dozwolone Zadeklaruj Konstruktor wystąpienia bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="80599-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="80599-182">Zamiast tego, co struktura niejawnie ma Konstruktor wystąpienia bez parametrów, która zawsze zwraca wartość, która wynika z ustawienia wszystkie pola typu wartości, aby przywrócić wartości domyślne i wszystkie odwołania pola typu do `null`.</span><span class="sxs-lookup"><span data-stu-id="80599-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="80599-183">Struktury, powinny być zaprojektowane należy wziąć pod uwagę domyślny stan inicjowania nieprawidłowym stanie.</span><span class="sxs-lookup"><span data-stu-id="80599-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="80599-184">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="80599-184">In the example</span></span>
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
<span data-ttu-id="80599-185">Konstruktor zdefiniowany przez użytkownika wystąpień chroni przed wartości null, tylko gdy jest jawnie wywoływana.</span><span class="sxs-lookup"><span data-stu-id="80599-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="80599-186">W przypadkach, gdzie `KeyValuePair` zmiennej podlega inicjowania wartości domyślne, `key` i `value` pola będą miały wartość null, a struktura musi być przygotowana do obsługi tego stanu.</span><span class="sxs-lookup"><span data-stu-id="80599-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="80599-187">Konwersja boxing i konwersja unboxing</span><span class="sxs-lookup"><span data-stu-id="80599-187">Boxing and unboxing</span></span>

<span data-ttu-id="80599-188">Wartość typu klasy może być konwertowany na typ `object` lub do typu interfejsu, który jest implementowany przez klasę, po prostu, traktując odwołanie jako inny typ w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="80599-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="80599-189">Podobnie, wartość typu `object` lub wartość typu interfejsu można przekonwertować do typu klasy bez zmiany odwołania (ale oczywiście wyboru typu run-time jest wymagana w tym przypadku).</span><span class="sxs-lookup"><span data-stu-id="80599-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="80599-190">Ponieważ struktury nie są typami odwołań, te operacje są implementowane w inny sposób dla typów struktury.</span><span class="sxs-lookup"><span data-stu-id="80599-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="80599-191">Gdy wartość typu struktury jest konwertowany na typ `object` lub do typu interfejsu, który jest implementowany przez strukturę, odbywa się operacją pakowania.</span><span class="sxs-lookup"><span data-stu-id="80599-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="80599-192">Podobnie gdy wartość typu `object` lub wartości typu interfejsu, jest konwertowany do typu struktury, odbywa się operacja rozpakowania.</span><span class="sxs-lookup"><span data-stu-id="80599-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="80599-193">Najważniejszą różnicą z tej samej operacji na typy klas jest, czy konwersja boxing i konwersja unboxing kopiuje wartość struktury z wystąpienia programu prostokątnych ani do.</span><span class="sxs-lookup"><span data-stu-id="80599-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="80599-194">W związku z tym po operacji pakowania i rozpakowywanie zmiany wprowadzone do rozpakowanych struktury nie są odzwierciedlane w strukturze opakowanej.</span><span class="sxs-lookup"><span data-stu-id="80599-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="80599-195">Kiedy typ struktury zastępuje metody wirtualnej odziedziczone `System.Object` (takie jak `Equals`, `GetHashCode`, lub `ToString`), wywołanie metody wirtualnej za pomocą wystąpienia typu struktury nie powoduje pakowania wystąpić.</span><span class="sxs-lookup"><span data-stu-id="80599-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="80599-196">Ta zasada obowiązuje, nawet wtedy, gdy struktury jest używany jako parametr typu i wywołania odbywa się przez wystąpienie typu typu parametru.</span><span class="sxs-lookup"><span data-stu-id="80599-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="80599-197">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="80599-197">For example:</span></span>
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="80599-198">Dane wyjściowe programu są:</span><span class="sxs-lookup"><span data-stu-id="80599-198">The output of the program is:</span></span>
```
1
2
3
```

<span data-ttu-id="80599-199">Mimo że jest nieprawidłowy dla `ToString` ma skutki uboczne, w przykładzie pokazano, opakowywanie nie wystąpił trzech wywołań `x.ToString()`.</span><span class="sxs-lookup"><span data-stu-id="80599-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="80599-200">Podobnie pakowanie nigdy niejawnie występuje, gdy dostęp do składowej w parametrze typu ograniczone.</span><span class="sxs-lookup"><span data-stu-id="80599-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="80599-201">Na przykład, załóżmy, że interfejs `ICounter` zawiera metodę `Increment` który może służyć do modyfikowania wartości.</span><span class="sxs-lookup"><span data-stu-id="80599-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="80599-202">Jeśli `ICounter` jest używany jako ograniczenie, implementacja `Increment` metoda jest wywoływana z odwołaniem do zmiennej, `Increment` została wywołana na nigdy nie ramce kopii.</span><span class="sxs-lookup"><span data-stu-id="80599-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="80599-203">Pierwsze wywołanie `Increment` Modyfikuje wartość w zmiennej `x`.</span><span class="sxs-lookup"><span data-stu-id="80599-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="80599-204">Nie jest to równoważne drugie wywołanie `Increment`, który modyfikuje wartości w ramce kopię `x`.</span><span class="sxs-lookup"><span data-stu-id="80599-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="80599-205">W związku z tym dane wyjściowe programu jest:</span><span class="sxs-lookup"><span data-stu-id="80599-205">Thus, the output of the program is:</span></span>
```
0
1
1
```

<span data-ttu-id="80599-206">Aby uzyskać dodatkowe szczegóły dotyczące pakowania i rozpakowywania, zobacz [pakowania i rozpakowywania](types.md#boxing-and-unboxing).</span><span class="sxs-lookup"><span data-stu-id="80599-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="80599-207">Znaczenie tego</span><span class="sxs-lookup"><span data-stu-id="80599-207">Meaning of this</span></span>

<span data-ttu-id="80599-208">W ramach konstruktora wystąpienia lub funkcja składowa klasy `this` zostanie sklasyfikowany jako wartość.</span><span class="sxs-lookup"><span data-stu-id="80599-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="80599-209">W związku z tym, podczas gdy `this` może służyć do odwoływania się do wystąpienia dla którego funkcja elementu członkowskiego wywołano, nie jest możliwe do przypisania do `this` w funkcji składowej klasy.</span><span class="sxs-lookup"><span data-stu-id="80599-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="80599-210">W konstruktorze wystąpienia struktury `this` odpowiada `out` parametr typu struktury, a także wewnątrz funkcji składowej wystąpienia struktury, `this` odpowiada `ref` parametr typu struktury.</span><span class="sxs-lookup"><span data-stu-id="80599-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="80599-211">W obu przypadkach `this` zostanie sklasyfikowany jako zmienną, i można modyfikować na całej strukturze, dla której wywołano element członkowski funkcji przez przypisywanie `this` lub przez przekazanie jako `ref` lub `out` parametru.</span><span class="sxs-lookup"><span data-stu-id="80599-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="80599-212">Inicjatory pola</span><span class="sxs-lookup"><span data-stu-id="80599-212">Field initializers</span></span>

<span data-ttu-id="80599-213">Zgodnie z opisem w [wartości domyślne](structs.md#default-values), wartością domyślną struktury składa się z wartość, która wynika z ustawienia wszystkie pola typu wartości, aby przywrócić wartości domyślne i wszystkie odwołania pola typu do `null`.</span><span class="sxs-lookup"><span data-stu-id="80599-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="80599-214">Z tego powodu struktury nie zezwala na wystąpienia deklaracji pól, aby uwzględnić zmienną inicjatory.</span><span class="sxs-lookup"><span data-stu-id="80599-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="80599-215">To ograniczenie dotyczy tylko pola wystąpień.</span><span class="sxs-lookup"><span data-stu-id="80599-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="80599-216">Pola statyczne struktury mogą zawierać inicjatory zmiennej.</span><span class="sxs-lookup"><span data-stu-id="80599-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="80599-217">Przykład</span><span class="sxs-lookup"><span data-stu-id="80599-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="80599-218">jest w wyniku błędu, ponieważ deklaracji pól wystąpienia zawierać inicjatory zmiennej.</span><span class="sxs-lookup"><span data-stu-id="80599-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="80599-219">Konstruktorów</span><span class="sxs-lookup"><span data-stu-id="80599-219">Constructors</span></span>

<span data-ttu-id="80599-220">W odróżnieniu od klasy struktury nie jest dozwolone Zadeklaruj Konstruktor wystąpienia bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="80599-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="80599-221">Zamiast tego, co struktura niejawnie ma Konstruktor wystąpienia bez parametrów, która zawsze zwraca wartość, która wynika z ustawienia wszystkie pola typu wartości, aby przywrócić wartości domyślne i wszystkie odwołania pola typu do wartości null ([domyślne konstruktory](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="80599-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="80599-222">Struktury można zadeklarować konstruktorów wystąpienia o parametry.</span><span class="sxs-lookup"><span data-stu-id="80599-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="80599-223">Na przykład</span><span class="sxs-lookup"><span data-stu-id="80599-223">For example</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

<span data-ttu-id="80599-224">Podane powyżej deklaracji, instrukcje</span><span class="sxs-lookup"><span data-stu-id="80599-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="80599-225">Obie umożliwiają tworzenie `Point` z `x` i `y` inicjowane od zera.</span><span class="sxs-lookup"><span data-stu-id="80599-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="80599-226">Konstruktora wystąpienia struktury nie może zawierać inicjatora konstruktora formularza `base(...)`.</span><span class="sxs-lookup"><span data-stu-id="80599-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="80599-227">Jeśli konstruktora wystąpienia struktury nie określono inicjatora konstruktora `this` odnosi się zmienna `out` parametr typu struktury i podobnie jak `out` parametru `this` musi zostać zdecydowanie przypisany ( [Asercję określonego przypisania](variables.md#definite-assignment)) w każdej lokalizacji, gdzie Konstruktor zwraca.</span><span class="sxs-lookup"><span data-stu-id="80599-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="80599-228">Jeśli Konstruktor wystąpienia struktury określa inicjatora konstruktora `this` odpowiada zmiennej `ref` parametr typu struktury i podobnie jak `ref` parametru `this` jest uznawany za zdecydowanie przypisany na wpis w treści konstruktora.</span><span class="sxs-lookup"><span data-stu-id="80599-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="80599-229">Należy wziąć pod uwagę implementacji konstruktora wystąpienia poniżej:</span><span class="sxs-lookup"><span data-stu-id="80599-229">Consider the instance constructor implementation below:</span></span>
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

<span data-ttu-id="80599-230">Nie funkcję składową wystąpienia (łącznie z metody dostępu set dla właściwości `X` i `Y`) może być wywoływany, dopóki wszystkie pola struktury budowany został zdecydowanie przypisany.</span><span class="sxs-lookup"><span data-stu-id="80599-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="80599-231">Jedynym wyjątkiem obejmuje automatycznie implementowanych właściwości ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="80599-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="80599-232">Reguły asercję określonego przypisania ([przypisanie proste wyrażenia](variables.md#simple-assignment-expressions)) specjalnie wykluczone przypisania do właściwości automatycznej typu struktury w konstruktorze wystąpienia tego typu struktury: takie przypisanie jest uznawany za określony Przypisanie do pola pomocniczego ukryte właściwości auto.</span><span class="sxs-lookup"><span data-stu-id="80599-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="80599-233">W związku z tym że jest dozwolone:</span><span class="sxs-lookup"><span data-stu-id="80599-233">Thus, the following is allowed:</span></span>

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a><span data-ttu-id="80599-234">Destruktory</span><span class="sxs-lookup"><span data-stu-id="80599-234">Destructors</span></span>

<span data-ttu-id="80599-235">Struktura nie jest dozwolona było zadeklarowanie destruktora.</span><span class="sxs-lookup"><span data-stu-id="80599-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="80599-236">Konstruktory statyczne</span><span class="sxs-lookup"><span data-stu-id="80599-236">Static constructors</span></span>

<span data-ttu-id="80599-237">Większość tych samych zasad, jak w przypadku klasy konstruktorów statycznych dla struktury, postępuj zgodnie z.</span><span class="sxs-lookup"><span data-stu-id="80599-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="80599-238">Wykonanie Konstruktor statyczny dla elementu typ struktury jest wyzwalany przez pierwszy z następujących zdarzeń w domenie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="80599-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="80599-239">Odwołuje się do statycznego elementu członkowskiego typu struktury.</span><span class="sxs-lookup"><span data-stu-id="80599-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="80599-240">Zadeklarowany w sposób jawny Konstruktor typu struktury jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="80599-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="80599-241">Tworzenie wartości domyślne ([wartości domyślne](structs.md#default-values)) struktury typów nie będzie wyzwalać Konstruktor statyczny.</span><span class="sxs-lookup"><span data-stu-id="80599-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="80599-242">(Na przykład jest początkowa wartość elementów w tablicy).</span><span class="sxs-lookup"><span data-stu-id="80599-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="80599-243">Przykłady — struktura</span><span class="sxs-lookup"><span data-stu-id="80599-243">Struct examples</span></span>

<span data-ttu-id="80599-244">Poniżej pokazano dwa przykłady istotne przy użyciu `struct` typy, aby utworzyć typy, które może służyć podobnie do wstępnie zdefiniowanych typów języka, ale z semantyką zmodyfikowane.</span><span class="sxs-lookup"><span data-stu-id="80599-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="80599-245">Typ liczby całkowitej bazy danych</span><span class="sxs-lookup"><span data-stu-id="80599-245">Database integer type</span></span>

<span data-ttu-id="80599-246">`DBInt` Poniżej struktura implementuje typ całkowitoliczbowy, który może reprezentować pełnego zestawu wartości `int` typu oraz dodatkowy stan, który wskazuje nieznanej wartości.</span><span class="sxs-lookup"><span data-stu-id="80599-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="80599-247">Typ z następującą charakterystyką jest często używany w bazach danych.</span><span class="sxs-lookup"><span data-stu-id="80599-247">A type with these characteristics is commonly used in databases.</span></span>

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a><span data-ttu-id="80599-248">Typ wartości logicznej bazy danych</span><span class="sxs-lookup"><span data-stu-id="80599-248">Database boolean type</span></span>

<span data-ttu-id="80599-249">`DBBool` Poniżej struktura implementuje przechowywanymi w trzech typu logicznego.</span><span class="sxs-lookup"><span data-stu-id="80599-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="80599-250">Możliwe wartości tego typu są `DBBool.True`, `DBBool.False`, i `DBBool.Null`, gdzie `Null` elementu członkowskiego określa nieznaną wartość.</span><span class="sxs-lookup"><span data-stu-id="80599-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="80599-251">Takie przechowywanymi na trzy typy logiczne są często używane w bazach danych.</span><span class="sxs-lookup"><span data-stu-id="80599-251">Such three-valued logical types are commonly used in databases.</span></span>

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
