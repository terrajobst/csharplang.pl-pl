---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704029"
---
# <a name="structs"></a><span data-ttu-id="e5084-101">Struktury</span><span class="sxs-lookup"><span data-stu-id="e5084-101">Structs</span></span>

<span data-ttu-id="e5084-102">Struktury są podobne do klas, które reprezentują struktury danych, które mogą zawierać składowe danych i składowe funkcji.</span><span class="sxs-lookup"><span data-stu-id="e5084-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="e5084-103">Jednak w przeciwieństwie do klas, struktury są typami wartości i nie wymagają przydziału sterty.</span><span class="sxs-lookup"><span data-stu-id="e5084-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="e5084-104">Zmienna typu struktury bezpośrednio zawiera dane struktury, podczas gdy zmienna typu klasy zawiera odwołanie do danych, które są znane jako obiekt.</span><span class="sxs-lookup"><span data-stu-id="e5084-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="e5084-105">Struktury są szczególnie przydatne w przypadku małych struktur danych, które mają semantykę wartości.</span><span class="sxs-lookup"><span data-stu-id="e5084-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="e5084-106">Liczby zespolone, punkty w układzie współrzędnych lub pary klucz-wartość w słowniku są wszystkimi dobrymi przykładami struktur.</span><span class="sxs-lookup"><span data-stu-id="e5084-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="e5084-107">Klucz do tych struktur danych polega na tym, że mają niewiele składowych danych, że nie wymagają użycia dziedziczenia lub referencyjnego tożsamości oraz że można je wygodnie zaimplementować przy użyciu semantyki wartości, w której przypisanie kopiuje wartość zamiast odwołania.</span><span class="sxs-lookup"><span data-stu-id="e5084-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="e5084-108">Zgodnie z opisem w [typach prostych](types.md#simple-types), typy proste dostarczone przez C#, takie jak `int`, `double` i `bool`, są w rzeczywistości wszystkimi typami struktury.</span><span class="sxs-lookup"><span data-stu-id="e5084-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="e5084-109">Tak jak te wstępnie zdefiniowane typy są strukturami, można również użyć struktur i przeciążania operatora w celu zaimplementowania nowych typów "pierwotnych" w C# języku.</span><span class="sxs-lookup"><span data-stu-id="e5084-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="e5084-110">Dwa przykłady takich typów są podane na końcu tego rozdziału ([przykłady struktury](structs.md#struct-examples)).</span><span class="sxs-lookup"><span data-stu-id="e5084-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="e5084-111">Deklaracje struktury</span><span class="sxs-lookup"><span data-stu-id="e5084-111">Struct declarations</span></span>

<span data-ttu-id="e5084-112">*Struct_declaration* to *type_declaration* ([deklaracje typu](namespaces.md#type-declarations)), które deklaruje nową strukturę:</span><span class="sxs-lookup"><span data-stu-id="e5084-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="e5084-113">*Struct_declaration* składa się z opcjonalnego zestawu *atrybutów* ([atrybutów](attributes.md)), po których następuje opcjonalny zestaw *struct_modifier*s ([Modyfikatory struktury](structs.md#struct-modifiers)), po którym następuje opcjonalny modyfikator `partial`, po którym następuje słowo kluczowe `struct` i *Identyfikator* , który nazywa strukturę, po którym następuje opcjonalna Specyfikacja *Type_parameter_list* ([parametry typu](classes.md#type-parameters)), a następnie opcjonalna Specyfikacja *struct_interfaces* ([częściowa modyfikator](structs.md#partial-modifier)), po którym następuje opcjonalna Specyfikacja *type_parameter_constraints_clause*s ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), po której następuje *struct_body* ([treść struktury](structs.md#struct-body)), opcjonalnie po której następuje średnik.</span><span class="sxs-lookup"><span data-stu-id="e5084-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="e5084-114">Modyfikatory struktury</span><span class="sxs-lookup"><span data-stu-id="e5084-114">Struct modifiers</span></span>

<span data-ttu-id="e5084-115">*Struct_declaration* może opcjonalnie zawierać sekwencję modyfikatorów struktury:</span><span class="sxs-lookup"><span data-stu-id="e5084-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

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

<span data-ttu-id="e5084-116">Jest to błąd czasu kompilacji dla tego samego modyfikatora do wyświetlenia wiele razy w deklaracji struktury.</span><span class="sxs-lookup"><span data-stu-id="e5084-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="e5084-117">Modyfikatory deklaracji struktury mają takie samo znaczenie jak dla deklaracji klasy ([deklaracji klas](classes.md#class-declarations)).</span><span class="sxs-lookup"><span data-stu-id="e5084-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="e5084-118">Modyfikator częściowy</span><span class="sxs-lookup"><span data-stu-id="e5084-118">Partial modifier</span></span>

<span data-ttu-id="e5084-119">Modyfikator `partial` wskazuje, że ta *struct_declaration* jest deklaracją typu częściowego.</span><span class="sxs-lookup"><span data-stu-id="e5084-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="e5084-120">Wiele deklaracji częściowej struktury o tej samej nazwie w otaczającej przestrzeni nazw lub deklaracji typu Połącz, aby utworzyć jedną deklarację struktury, zgodnie z regułami określonymi w [typach częściowych](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="e5084-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="e5084-121">Interfejsy struktury</span><span class="sxs-lookup"><span data-stu-id="e5084-121">Struct interfaces</span></span>

<span data-ttu-id="e5084-122">Deklaracja struktury może zawierać specyfikację *struct_interfaces* , w takim przypadku struktura jest określana do bezpośredniego zaimplementowania danego typu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="e5084-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="e5084-123">Implementacje interfejsów omówiono dokładniej w [implementacjach interfejsu](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="e5084-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="e5084-124">Treść struktury</span><span class="sxs-lookup"><span data-stu-id="e5084-124">Struct body</span></span>

<span data-ttu-id="e5084-125">*Struct_body* struktury definiuje elementy członkowskie struktury.</span><span class="sxs-lookup"><span data-stu-id="e5084-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="e5084-126">Elementy członkowskie struktury</span><span class="sxs-lookup"><span data-stu-id="e5084-126">Struct members</span></span>

<span data-ttu-id="e5084-127">Elementy członkowskie struktury składają się z elementów członkowskich wprowadzonych przez *struct_member_declaration*i członków dziedziczonych z typu `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="e5084-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

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

<span data-ttu-id="e5084-128">Z wyjątkiem różnic zanotowanych w [różnicach klas i struktur](structs.md#class-and-struct-differences), opisy elementów członkowskich klasy, które znajdują się w [składowych klasy](classes.md#class-members) za pomocą [iteratorów](classes.md#iterators) stosują się również do elementów członkowskich struktury.</span><span class="sxs-lookup"><span data-stu-id="e5084-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="e5084-129">Różnice klas i struktur</span><span class="sxs-lookup"><span data-stu-id="e5084-129">Class and struct differences</span></span>

<span data-ttu-id="e5084-130">Struktury różnią się od klas na kilka ważnych sposobów:</span><span class="sxs-lookup"><span data-stu-id="e5084-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="e5084-131">Struktury są typami wartości ([semantyką wartości](structs.md#value-semantics)).</span><span class="sxs-lookup"><span data-stu-id="e5084-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="e5084-132">Wszystkie typy struktur niejawnie dziedziczą z klasy `System.ValueType` ([dziedziczenie](structs.md#inheritance)).</span><span class="sxs-lookup"><span data-stu-id="e5084-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="e5084-133">Przypisanie do zmiennej typu struktury tworzy kopię przypisanej wartości ([przypisanie](structs.md#assignment)).</span><span class="sxs-lookup"><span data-stu-id="e5084-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="e5084-134">Wartość domyślna struktury jest wartością wygenerowaną przez ustawienie wszystkich pól typu wartości na wartości domyślne i wszystkie pola typu odwołania do `null` ([wartości domyślne](structs.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="e5084-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="e5084-135">Operacje pakowania i rozpakowywania są używane do konwersji między typem struktury a `object` ([opakowaniem i rozpakowywaniem](structs.md#boxing-and-unboxing)).</span><span class="sxs-lookup"><span data-stu-id="e5084-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="e5084-136">Znaczenie `this` jest różne dla struktur ([ten dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="e5084-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="e5084-137">Deklaracje pola wystąpienia dla struktury nie mogą zawierać inicjatorów zmiennych ([inicjatorów pól](structs.md#field-initializers)).</span><span class="sxs-lookup"><span data-stu-id="e5084-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="e5084-138">Struktura nie może deklarować konstruktora wystąpień bez parametrów ([konstruktorów](structs.md#constructors)).</span><span class="sxs-lookup"><span data-stu-id="e5084-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="e5084-139">Struktura nie może deklarować destruktora ([destruktory](structs.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="e5084-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="e5084-140">Semantyka wartości</span><span class="sxs-lookup"><span data-stu-id="e5084-140">Value semantics</span></span>

<span data-ttu-id="e5084-141">Struktury są typami wartości ([typami wartości](types.md#value-types)) i są określane jako semantyka wartości.</span><span class="sxs-lookup"><span data-stu-id="e5084-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="e5084-142">Klasy, z drugiej strony, to typy odwołań ([typy odwołań](types.md#reference-types)) i są określane jako semantyka odwołania.</span><span class="sxs-lookup"><span data-stu-id="e5084-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="e5084-143">Zmienna typu struktury bezpośrednio zawiera dane struktury, podczas gdy zmienna typu klasy zawiera odwołanie do danych, które są znane jako obiekt.</span><span class="sxs-lookup"><span data-stu-id="e5084-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="e5084-144">Gdy struktura `B` zawiera pole wystąpienia typu `A`, a `A` jest typem struktury, jest to błąd czasu kompilacji dla `A`, który jest zależny od `B` lub typu złożonego z `B`.</span><span class="sxs-lookup"><span data-stu-id="e5084-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="e5084-145">Struktura `X` ***bezpośrednio zależy*** od struktury `Y`, jeśli `X` zawiera pole wystąpienia typu `Y`.</span><span class="sxs-lookup"><span data-stu-id="e5084-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="e5084-146">W związku z tą definicją kompletny zestaw struktur, od których zależy struktura, jest przechodnim zamykaniem ***bezpośrednio zależnym od*** relacji.</span><span class="sxs-lookup"><span data-stu-id="e5084-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="e5084-147">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e5084-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="e5084-148">jest błędem, ponieważ `Node` zawiera pole wystąpienia własnego typu.</span><span class="sxs-lookup"><span data-stu-id="e5084-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="e5084-149">Inny przykład</span><span class="sxs-lookup"><span data-stu-id="e5084-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="e5084-150">jest błędem, ponieważ każdy z typów `A`, `B` i `C` zależy od siebie.</span><span class="sxs-lookup"><span data-stu-id="e5084-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="e5084-151">Klasy, istnieje możliwość, że dwie zmienne odwołują się do tego samego obiektu, i w tym przypadku operacje na jednej zmiennej mają wpływ na obiekt, do którego odwołuje się inna zmienna.</span><span class="sxs-lookup"><span data-stu-id="e5084-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="e5084-152">W przypadku struktur każda z tych zmiennych ma własną kopię danych (z wyjątkiem `ref` i `out` zmiennych parametrów) i nie jest możliwe, aby operacje na nich miały wpływ na inne.</span><span class="sxs-lookup"><span data-stu-id="e5084-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="e5084-153">Ponadto, ponieważ struktury nie są typami odwołań, nie jest możliwe, aby wartości typu struktury były `null`.</span><span class="sxs-lookup"><span data-stu-id="e5084-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="e5084-154">Dana deklaracja</span><span class="sxs-lookup"><span data-stu-id="e5084-154">Given the declaration</span></span>
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
<span data-ttu-id="e5084-155">fragment kodu</span><span class="sxs-lookup"><span data-stu-id="e5084-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="e5084-156">Wyświetla wartość `10`.</span><span class="sxs-lookup"><span data-stu-id="e5084-156">outputs the value `10`.</span></span> <span data-ttu-id="e5084-157">Przypisanie `a` do `b` tworzy kopię wartości, a `b` nie ma wpływ na przypisanie do `a.x`.</span><span class="sxs-lookup"><span data-stu-id="e5084-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="e5084-158">@No__t-0 zamiast zadeklarować jako Klasa, dane wyjściowe byłyby `100`, ponieważ `a` i `b` odwołują się do tego samego obiektu.</span><span class="sxs-lookup"><span data-stu-id="e5084-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="e5084-159">Dziedziczenie</span><span class="sxs-lookup"><span data-stu-id="e5084-159">Inheritance</span></span>

<span data-ttu-id="e5084-160">Wszystkie typy struktur niejawnie dziedziczą z klasy `System.ValueType`, co z kolei dziedziczy z klasy `object`.</span><span class="sxs-lookup"><span data-stu-id="e5084-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="e5084-161">Deklaracja struktury może określać listę zaimplementowanych interfejsów, ale nie jest możliwe, aby deklaracja struktury mogła określić klasę bazową.</span><span class="sxs-lookup"><span data-stu-id="e5084-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="e5084-162">Typy struktur nigdy nie są abstrakcyjne i zawsze są zapieczętowane niejawnie.</span><span class="sxs-lookup"><span data-stu-id="e5084-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="e5084-163">Modyfikatory `abstract` i `sealed` nie są w związku z tym niedozwolone w deklaracji struktury.</span><span class="sxs-lookup"><span data-stu-id="e5084-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="e5084-164">Ponieważ dziedziczenie nie jest obsługiwane dla struktur, zadeklarowana dostępność elementu członkowskiego struktury nie może być `protected` lub `protected internal`.</span><span class="sxs-lookup"><span data-stu-id="e5084-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="e5084-165">Składowe funkcji w strukturze nie mogą być `abstract` lub `virtual`, a modyfikator `override` jest dozwolony tylko w celu przesłonięcia metod dziedziczonych z `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="e5084-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="e5084-166">Przypisanie</span><span class="sxs-lookup"><span data-stu-id="e5084-166">Assignment</span></span>

<span data-ttu-id="e5084-167">Przypisanie do zmiennej typu struktury tworzy kopię przypisanej wartości.</span><span class="sxs-lookup"><span data-stu-id="e5084-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="e5084-168">Różni się to od przypisywania do zmiennej typu klasy, która kopiuje odwołanie, ale nie obiekt identyfikowany przez odwołanie.</span><span class="sxs-lookup"><span data-stu-id="e5084-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="e5084-169">Podobnie jak w przypadku przypisania, gdy struktura jest przekazana jako parametr wartości lub zwracana jako wynik elementu członkowskiego funkcji, tworzona jest kopia struktury.</span><span class="sxs-lookup"><span data-stu-id="e5084-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="e5084-170">Struktura może być przenoszona przez odwołanie do składowej funkcji przy użyciu parametru `ref` lub `out`.</span><span class="sxs-lookup"><span data-stu-id="e5084-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="e5084-171">Gdy właściwość lub indeksator struktury jest elementem docelowym przypisania, wyrażenie wystąpienia skojarzone z właściwością lub dostępem indeksatora musi być sklasyfikowane jako zmienna.</span><span class="sxs-lookup"><span data-stu-id="e5084-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="e5084-172">Jeśli wyrażenie wystąpienia jest sklasyfikowane jako wartość, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e5084-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="e5084-173">Opisano to szczegółowo w temacie [proste przypisanie](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="e5084-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="e5084-174">Wartości domyślne</span><span class="sxs-lookup"><span data-stu-id="e5084-174">Default values</span></span>

<span data-ttu-id="e5084-175">Zgodnie z opisem w [wartości domyślne](variables.md#default-values)kilka rodzajów zmiennych jest automatycznie inicjowanych do wartości domyślnej podczas ich tworzenia.</span><span class="sxs-lookup"><span data-stu-id="e5084-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="e5084-176">Dla zmiennych typów klas i innych typów odwołań ta wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="e5084-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="e5084-177">Jednak ponieważ struktury są typami wartości, które nie mogą być `null`, wartość domyślna struktury jest wartością wygenerowaną przez ustawienie wszystkich pól typu wartości na wartość domyślną i wszystkie pola typu odwołania do `null`.</span><span class="sxs-lookup"><span data-stu-id="e5084-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="e5084-178">Odwoływanie się do struktury `Point` zadeklarowanej powyżej, przykład</span><span class="sxs-lookup"><span data-stu-id="e5084-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="e5084-179">Inicjuje każdy `Point` w tablicy do wartości wygenerowanej przez ustawienie pól `x` i `y` na zero.</span><span class="sxs-lookup"><span data-stu-id="e5084-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="e5084-180">Wartość domyślna struktury odpowiada wartości zwracanej przez konstruktora domyślnego struktury ([konstruktorów domyślnych](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="e5084-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="e5084-181">W przeciwieństwie do klasy, struktura nie może deklarować konstruktora wystąpienia bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="e5084-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="e5084-182">Zamiast tego każda struktura niejawnie ma Konstruktor wystąpienia bez parametrów, który zawsze zwraca wartość, która wynika z ustawienia wszystkich pól typu wartości na wartości domyślne i wszystkie pola typu odwołania do `null`.</span><span class="sxs-lookup"><span data-stu-id="e5084-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="e5084-183">Struktury powinny zostać zaprojektowane w celu uwzględnienia domyślnego stanu inicjalizacji.</span><span class="sxs-lookup"><span data-stu-id="e5084-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="e5084-184">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="e5084-184">In the example</span></span>
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
<span data-ttu-id="e5084-185">zdefiniowany przez użytkownika Konstruktor wystąpień chroni przed wartościami null tylko wtedy, gdy jest on jawnie wywoływany.</span><span class="sxs-lookup"><span data-stu-id="e5084-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="e5084-186">W przypadkach, gdy zmienna `KeyValuePair` podlega inicjacji wartości domyślnej, pola `key` i `value` będą mieć wartość null, a struktura musi być przygotowana do obsługi tego stanu.</span><span class="sxs-lookup"><span data-stu-id="e5084-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="e5084-187">Pakowanie i rozpakowywanie</span><span class="sxs-lookup"><span data-stu-id="e5084-187">Boxing and unboxing</span></span>

<span data-ttu-id="e5084-188">Wartość typu klasy można przekonwertować na typ `object` lub do typu interfejsu, który jest implementowany przez klasę po prostu przez traktowanie odwołania jako innego typu w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e5084-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="e5084-189">Analogicznie, wartość typu `object` lub wartość typu interfejsu można przekonwertować z powrotem na typ klasy bez zmiany odwołania (ale oczywiście w tym przypadku jest wymagane sprawdzanie typu w czasie wykonywania).</span><span class="sxs-lookup"><span data-stu-id="e5084-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="e5084-190">Ponieważ struktury nie są typami odwołań, operacje te są implementowane inaczej dla typów struktury.</span><span class="sxs-lookup"><span data-stu-id="e5084-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="e5084-191">Gdy wartość typu struktury jest konwertowana na typ `object` lub do typu interfejsu, który jest implementowany przez strukturę, odbywa się operacja opakowywania.</span><span class="sxs-lookup"><span data-stu-id="e5084-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="e5084-192">Podobnie, gdy wartość typu `object` lub wartość typu interfejsu jest konwertowana z powrotem na typ struktury, wykonywana jest operacja rozpakowywania.</span><span class="sxs-lookup"><span data-stu-id="e5084-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="e5084-193">Kluczową różnicą między tymi samymi operacjami w typach klas jest to, że opakowanie i rozpakowywanie kopiuje wartość struktury do lub z wystąpienia zapakowanego.</span><span class="sxs-lookup"><span data-stu-id="e5084-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="e5084-194">W tym celu po rozpakowywania lub rozpakowywania zmiany wprowadzone do struktury nieopakowanej nie są odzwierciedlone w strukturze opakowanej.</span><span class="sxs-lookup"><span data-stu-id="e5084-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="e5084-195">Gdy typ struktury zastępuje metodę wirtualną dziedziczoną z `System.Object` (na przykład `Equals`, `GetHashCode` lub `ToString`), wywołanie metody wirtualnej za pośrednictwem wystąpienia typu struktury nie powoduje, że opakowanie nie występuje.</span><span class="sxs-lookup"><span data-stu-id="e5084-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="e5084-196">Jest to prawdziwe nawet wtedy, gdy struktura jest używana jako parametr typu, a wywołanie odbywa się za pomocą wystąpienia typu parametru typu.</span><span class="sxs-lookup"><span data-stu-id="e5084-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="e5084-197">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e5084-197">For example:</span></span>
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

<span data-ttu-id="e5084-198">Dane wyjściowe programu są następujące:</span><span class="sxs-lookup"><span data-stu-id="e5084-198">The output of the program is:</span></span>
```console
1
2
3
```

<span data-ttu-id="e5084-199">Mimo że jest to zły styl dla `ToString`, aby mieć efekt uboczny, w przykładzie pokazano, że nie nastąpiły żadne opakowanie dla trzech wywołań `x.ToString()`.</span><span class="sxs-lookup"><span data-stu-id="e5084-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="e5084-200">Podobnie opakowanie nigdy nie występuje niejawnie podczas uzyskiwania dostępu do elementu członkowskiego w parametrze typu ograniczonego.</span><span class="sxs-lookup"><span data-stu-id="e5084-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="e5084-201">Załóżmy na przykład, że interfejs `ICounter` zawiera metodę `Increment`, której można użyć do zmodyfikowania wartości.</span><span class="sxs-lookup"><span data-stu-id="e5084-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="e5084-202">Jeśli `ICounter` jest używany jako ograniczenie, implementacja metody `Increment` jest wywoływana z odwołaniem do zmiennej, która `Increment` została wywołana w, nigdy nie jako kopia w ramce.</span><span class="sxs-lookup"><span data-stu-id="e5084-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

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

<span data-ttu-id="e5084-203">Pierwsze wywołanie `Increment` modyfikuje wartość w zmiennej `x`.</span><span class="sxs-lookup"><span data-stu-id="e5084-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="e5084-204">Nie jest to równoznaczne z drugim wywołaniem `Increment`, które modyfikuje wartość w opakowanej kopii `x`.</span><span class="sxs-lookup"><span data-stu-id="e5084-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="e5084-205">W rezultacie dane wyjściowe programu są następujące:</span><span class="sxs-lookup"><span data-stu-id="e5084-205">Thus, the output of the program is:</span></span>
```console
0
1
1
```

<span data-ttu-id="e5084-206">Aby uzyskać więcej informacji na temat pakowania i rozpakowywania, zobacz [opakowanie i rozpakowywanie](types.md#boxing-and-unboxing).</span><span class="sxs-lookup"><span data-stu-id="e5084-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="e5084-207">Znaczenie tego</span><span class="sxs-lookup"><span data-stu-id="e5084-207">Meaning of this</span></span>

<span data-ttu-id="e5084-208">W ramach konstruktora wystąpienia lub składowej funkcji wystąpienia klasy `this` jest sklasyfikowana jako wartość.</span><span class="sxs-lookup"><span data-stu-id="e5084-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="e5084-209">W tym przypadku `this` może służyć do odwoływania się do wystąpienia, dla którego wywołano element członkowski funkcji, nie można przypisać do `this` w składowej funkcji klasy.</span><span class="sxs-lookup"><span data-stu-id="e5084-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="e5084-210">W konstruktorze wystąpienia struktury, `this` odpowiada parametrowi `out` typu struct i w elemencie członkowskim funkcji instancji struktury `this` odpowiada parametrowi `ref` typu struktury.</span><span class="sxs-lookup"><span data-stu-id="e5084-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="e5084-211">W obu przypadkach `this` jest klasyfikowane jako zmienna i istnieje możliwość zmodyfikowania całej struktury, dla której element członkowski funkcji został wywołany przez przypisanie do `this` lub przez przekazanie go jako parametru `ref` lub `out`.</span><span class="sxs-lookup"><span data-stu-id="e5084-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="e5084-212">Inicjatory pola</span><span class="sxs-lookup"><span data-stu-id="e5084-212">Field initializers</span></span>

<span data-ttu-id="e5084-213">Zgodnie z opisem w [wartości domyślne](structs.md#default-values), wartość domyślna struktury składa się z wartości, która wynika z ustawiania wszystkich pól typu wartości na wartości domyślne i wszystkie pola typu odwołania do `null`.</span><span class="sxs-lookup"><span data-stu-id="e5084-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="e5084-214">Z tego powodu struktura nie zezwala na deklaracje pól wystąpienia, aby uwzględnić inicjatory zmiennych.</span><span class="sxs-lookup"><span data-stu-id="e5084-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="e5084-215">To ograniczenie dotyczy tylko pól wystąpień.</span><span class="sxs-lookup"><span data-stu-id="e5084-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="e5084-216">Pola statyczne struktury mogą zawierać inicjatory zmiennych.</span><span class="sxs-lookup"><span data-stu-id="e5084-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="e5084-217">Przykład</span><span class="sxs-lookup"><span data-stu-id="e5084-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="e5084-218">Wystąpił błąd, ponieważ deklaracje pola wystąpienia obejmują inicjatory zmiennych.</span><span class="sxs-lookup"><span data-stu-id="e5084-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="e5084-219">Konstruktorów</span><span class="sxs-lookup"><span data-stu-id="e5084-219">Constructors</span></span>

<span data-ttu-id="e5084-220">W przeciwieństwie do klasy, struktura nie może deklarować konstruktora wystąpienia bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="e5084-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="e5084-221">Zamiast tego każda struktura niejawnie ma Konstruktor wystąpienia bez parametrów, który zawsze zwraca wartość, która wynika z ustawienia wszystkich pól typu wartości na wartości domyślne i wszystkie pola typu odwołania do wartości null ([konstruktory domyślne](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="e5084-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="e5084-222">Struktura może deklarować konstruktory wystąpień z parametrami.</span><span class="sxs-lookup"><span data-stu-id="e5084-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="e5084-223">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e5084-223">For example</span></span>
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

<span data-ttu-id="e5084-224">W przypadku powyższej deklaracji instrukcje</span><span class="sxs-lookup"><span data-stu-id="e5084-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="e5084-225">Oba te elementy tworzą `Point` z `x` i `y` inicjowane na zero.</span><span class="sxs-lookup"><span data-stu-id="e5084-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="e5084-226">Konstruktor wystąpienia struktury nie może zawierać inicjatora konstruktora w postaci `base(...)`.</span><span class="sxs-lookup"><span data-stu-id="e5084-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="e5084-227">Jeśli Konstruktor wystąpienia struktury nie określa inicjatora konstruktora, zmienna `this` odpowiada parametrowi `out` typu struktury i podobnej do parametru `out`, a `this` musi być ostatecznie przypisana ([przypisanie określone ](variables.md#definite-assignment)) w każdej lokalizacji, w której Konstruktor zwraca.</span><span class="sxs-lookup"><span data-stu-id="e5084-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="e5084-228">Jeśli Konstruktor wystąpienia struktury określa inicjator konstruktora, zmienna `this` odpowiada parametrowi `ref` typu struktury i podobnej do parametru `ref`, `this` jest uważana za ostatecznie przypisaną do treści konstruktora. .</span><span class="sxs-lookup"><span data-stu-id="e5084-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="e5084-229">Rozważmy implementację konstruktora wystąpień poniżej:</span><span class="sxs-lookup"><span data-stu-id="e5084-229">Consider the instance constructor implementation below:</span></span>
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

<span data-ttu-id="e5084-230">Żadna funkcja członkowska wystąpienia (w tym zestaw metod dostępu dla właściwości `X` i `Y`) może zostać wywołana, dopóki wszystkie pola w strukturze nie zostały ostatecznie przypisane.</span><span class="sxs-lookup"><span data-stu-id="e5084-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="e5084-231">Jedyny wyjątek obejmuje automatycznie zaimplementowane właściwości ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="e5084-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="e5084-232">Określone reguły przypisywania ([proste wyrażenia przypisania](variables.md#simple-assignment-expressions)), które wykluczają przypisanie do właściwości automatycznego typu struktury w konstruktorze wystąpienia tego typu struktury: takie przypisanie jest uznawane za określone przypisanie ukrytego pole zapasowe właściwości autoproperty.</span><span class="sxs-lookup"><span data-stu-id="e5084-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="e5084-233">W ten sposób można wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="e5084-233">Thus, the following is allowed:</span></span>

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

### <a name="destructors"></a><span data-ttu-id="e5084-234">Destruktory</span><span class="sxs-lookup"><span data-stu-id="e5084-234">Destructors</span></span>

<span data-ttu-id="e5084-235">Struktura nie może deklarować destruktora.</span><span class="sxs-lookup"><span data-stu-id="e5084-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="e5084-236">Konstruktory statyczne</span><span class="sxs-lookup"><span data-stu-id="e5084-236">Static constructors</span></span>

<span data-ttu-id="e5084-237">Konstruktory statyczne dla struktur są zgodne z większością reguł dla klas.</span><span class="sxs-lookup"><span data-stu-id="e5084-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="e5084-238">Wykonanie statycznego konstruktora dla typu struktury jest wyzwalane przez pierwsze z następujących zdarzeń, które mają wystąpić w domenie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e5084-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="e5084-239">Istnieje odwołanie do statycznego elementu członkowskiego typu struktury.</span><span class="sxs-lookup"><span data-stu-id="e5084-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="e5084-240">Wywoływany jest jawnie zadeklarowany Konstruktor typu struktury.</span><span class="sxs-lookup"><span data-stu-id="e5084-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="e5084-241">Tworzenie wartości domyślnych ([wartości domyślne](structs.md#default-values)) typów struktur nie wyzwala statycznego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e5084-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="e5084-242">(Przykładem jest początkowa wartość elementów w tablicy).</span><span class="sxs-lookup"><span data-stu-id="e5084-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="e5084-243">Przykłady struktury</span><span class="sxs-lookup"><span data-stu-id="e5084-243">Struct examples</span></span>

<span data-ttu-id="e5084-244">Poniżej przedstawiono dwa znaczące przykłady użycia typów `struct` do tworzenia typów, które mogą być używane podobnie do wstępnie zdefiniowanych typów języka, ale ze zmodyfikowaną semantyką.</span><span class="sxs-lookup"><span data-stu-id="e5084-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="e5084-245">Typ liczby całkowitej bazy danych</span><span class="sxs-lookup"><span data-stu-id="e5084-245">Database integer type</span></span>

<span data-ttu-id="e5084-246">W poniższej strukturze `DBInt` jest implementowany typ Integer, który może reprezentować pełny zestaw wartości typu `int` oraz dodatkowy stan, który wskazuje nieznaną wartość.</span><span class="sxs-lookup"><span data-stu-id="e5084-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="e5084-247">Typ z tymi cechami jest często używany w bazach danych.</span><span class="sxs-lookup"><span data-stu-id="e5084-247">A type with these characteristics is commonly used in databases.</span></span>

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

### <a name="database-boolean-type"></a><span data-ttu-id="e5084-248">Typ wartości logicznej bazy danych</span><span class="sxs-lookup"><span data-stu-id="e5084-248">Database boolean type</span></span>

<span data-ttu-id="e5084-249">Struktura `DBBool` jest implementowana przy użyciu trzech wartości typu logicznego.</span><span class="sxs-lookup"><span data-stu-id="e5084-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="e5084-250">Możliwe wartości tego typu to `DBBool.True`, `DBBool.False` i `DBBool.Null`, gdzie członek `Null` wskazuje nieznaną wartość.</span><span class="sxs-lookup"><span data-stu-id="e5084-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="e5084-251">Takie trzy typy logiczne są często używane w bazach danych.</span><span class="sxs-lookup"><span data-stu-id="e5084-251">Such three-valued logical types are commonly used in databases.</span></span>

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
