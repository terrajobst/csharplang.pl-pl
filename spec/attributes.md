# <a name="attributes"></a><span data-ttu-id="cb7e0-101">Atrybuty</span><span class="sxs-lookup"><span data-stu-id="cb7e0-101">Attributes</span></span>

<span data-ttu-id="cb7e0-102">Większość języka C# umożliwia programisty określić deklaratywne informacji na temat jednostek zdefiniowane w programie.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-102">Much of the C# language enables the programmer to specify declarative information about the entities defined in the program.</span></span> <span data-ttu-id="cb7e0-103">Na przykład dostępność metody w klasie jest określona przez urządzanie ją za pomocą *method_modifier*s `public`, `protected`, `internal`, i `private`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-103">For example, the accessibility of a method in a class is specified by decorating it with the *method_modifier*s `public`, `protected`, `internal`, and `private`.</span></span>

<span data-ttu-id="cb7e0-104">C# umożliwia programistom wymyślić nowych rodzajów informacji deklaratywne, o nazwie ***atrybuty***.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-104">C# enables programmers to invent new kinds of declarative information, called ***attributes***.</span></span> <span data-ttu-id="cb7e0-105">Programiści można dołączyć atrybuty do różnych obiektów programu i pobrać informacje o atrybutach w środowisku uruchomieniowym.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-105">Programmers can then attach attributes to various program entities, and retrieve attribute information in a run-time environment.</span></span> <span data-ttu-id="cb7e0-106">Na przykład zdefiniować strukturę `HelpAttribute` atrybut, który można umieścić w przypadku niektórych elementów programu (takich jak klasy i metody) umożliwia mapowanie tych elementów program do ich dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-106">For instance, a framework might define a `HelpAttribute` attribute that can be placed on certain program elements (such as classes and methods) to provide a mapping from those program elements to their documentation.</span></span>

<span data-ttu-id="cb7e0-107">Atrybuty są definiowane za pomocą deklaracji klasy atrybutów ([atrybutu klasy](attributes.md#attribute-classes)), które mogą mieć parametry pozycyjne i nazwane ([Positional i nazwanych parametrów](attributes.md#positional-and-named-parameters)).</span><span class="sxs-lookup"><span data-stu-id="cb7e0-107">Attributes are defined through the declaration of attribute classes ([Attribute classes](attributes.md#attribute-classes)), which may have positional and named parameters ([Positional and named parameters](attributes.md#positional-and-named-parameters)).</span></span> <span data-ttu-id="cb7e0-108">Atrybuty są dołączone do jednostki w programie C# przy użyciu specyfikacji atrybutu ([Specyfikacja atrybutu](attributes.md#attribute-specification)) i mogą być pobierane w czasie wykonywania jako instancje wieloatrybutowe ([wystąpienia atrybutów](attributes.md#attribute-instances)).</span><span class="sxs-lookup"><span data-stu-id="cb7e0-108">Attributes are attached to entities in a C# program using attribute specifications ([Attribute specification](attributes.md#attribute-specification)), and can be retrieved at run-time as attribute instances ([Attribute instances](attributes.md#attribute-instances)).</span></span>

## <a name="attribute-classes"></a><span data-ttu-id="cb7e0-109">Klasy atrybutów</span><span class="sxs-lookup"><span data-stu-id="cb7e0-109">Attribute classes</span></span>

<span data-ttu-id="cb7e0-110">Klasa, która pochodzi z klasy abstrakcyjnej `System.Attribute`, bezpośrednio lub pośrednio, jest ***klasy atrybutu***.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-110">A class that derives from the abstract class `System.Attribute`, whether directly or indirectly, is an ***attribute class***.</span></span> <span data-ttu-id="cb7e0-111">Deklaracja klasy atrybutu definiuje nowy rodzaj ***atrybut*** który można umieścić w deklaracji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-111">The declaration of an attribute class defines a new kind of ***attribute*** that can be placed on a declaration.</span></span> <span data-ttu-id="cb7e0-112">Zgodnie z Konwencją klasy atrybutów są nazywane z sufiksem `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-112">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="cb7e0-113">Przypadki użycia atrybutu może zawierać albo pominąć ten sufiks.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-113">Uses of an attribute may either include or omit this suffix.</span></span>

### <a name="attribute-usage"></a><span data-ttu-id="cb7e0-114">Użycie atrybutu</span><span class="sxs-lookup"><span data-stu-id="cb7e0-114">Attribute usage</span></span>

<span data-ttu-id="cb7e0-115">Ten atrybut `AttributeUsage` ([atrybut AttributeUsage](attributes.md#the-attributeusage-attribute)) służy do opisywania, jak można użyć klasy atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-115">The attribute `AttributeUsage` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)) is used to describe how an attribute class can be used.</span></span>

<span data-ttu-id="cb7e0-116">`AttributeUsage` ma parametr pozycyjne ([Positional i nazwanych parametrów](attributes.md#positional-and-named-parameters)), który umożliwia klasę atrybutu określić rodzaje deklaracje, które mogą służyć.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-116">`AttributeUsage` has a positional parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) that enables an attribute class to specify the kinds of declarations on which it can be used.</span></span> <span data-ttu-id="cb7e0-117">Przykład</span><span class="sxs-lookup"><span data-stu-id="cb7e0-117">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

<span data-ttu-id="cb7e0-118">definiuje klasę atrybutu o nazwie `SimpleAttribute` który można umieścić na *class_declaration*s i *interface_declaration*tylko s.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-118">defines an attribute class named `SimpleAttribute` that can be placed on *class_declaration*s and *interface_declaration*s only.</span></span> <span data-ttu-id="cb7e0-119">Przykład</span><span class="sxs-lookup"><span data-stu-id="cb7e0-119">The example</span></span>

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

<span data-ttu-id="cb7e0-120">Pokazuje kilka zastosowań `Simple` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-120">shows several uses of the `Simple` attribute.</span></span> <span data-ttu-id="cb7e0-121">Mimo że ten atrybut jest zdefiniowany o nazwie `SimpleAttribute`, gdy ten atrybut jest używany, `Attribute` sufiksu może zostać pominięta, wynikiem krótką nazwę `Simple`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-121">Although this attribute is defined with the name `SimpleAttribute`, when this attribute is used, the `Attribute` suffix may be omitted, resulting in the short name `Simple`.</span></span> <span data-ttu-id="cb7e0-122">W związku z tym w powyższym przykładzie są semantycznie równoważne do następujących:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-122">Thus, the example above is semantically equivalent to the following:</span></span>

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

<span data-ttu-id="cb7e0-123">`AttributeUsage` ma parametr o nazwie ([Positional i nazwanych parametrów](attributes.md#positional-and-named-parameters)) o nazwie `AllowMultiple`, która wskazuje, czy ten atrybut można określić więcej niż jeden raz dla danej jednostki.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-123">`AttributeUsage` has a named parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) called `AllowMultiple`, which indicates whether the attribute can be specified more than once for a given entity.</span></span> <span data-ttu-id="cb7e0-124">Jeśli `AllowMultiple` dla atrybutu klasy ma wartość true, a następnie jest tej klasy atrybutu ***klasy wielokrotnego użytku atrybutu***, można określić więcej niż jeden raz w jednostce.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-124">If `AllowMultiple` for an attribute class is true, then that attribute class is a ***multi-use attribute class***, and can be specified more than once on an entity.</span></span> <span data-ttu-id="cb7e0-125">Jeśli `AllowMultiple` dla atrybutu klasy ma wartość false lub jest nieokreślony, a następnie jest tej klasy atrybutu ***jednorazowy klasy atrybutu***i może być określona co najwyżej raz na jednostkę.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-125">If `AllowMultiple` for an attribute class is false or it is unspecified, then that attribute class is a ***single-use attribute class***, and can be specified at most once on an entity.</span></span>

<span data-ttu-id="cb7e0-126">Przykład</span><span class="sxs-lookup"><span data-stu-id="cb7e0-126">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

<span data-ttu-id="cb7e0-127">definiuje klasę wielokrotnego użytku atrybutu o nazwie `AuthorAttribute`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-127">defines a multi-use attribute class named `AuthorAttribute`.</span></span> <span data-ttu-id="cb7e0-128">Przykład</span><span class="sxs-lookup"><span data-stu-id="cb7e0-128">The example</span></span>

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

<span data-ttu-id="cb7e0-129">przedstawia deklarację klasy z dwa przypadki użycia `Author` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-129">shows a class declaration with two uses of the `Author` attribute.</span></span>

<span data-ttu-id="cb7e0-130">`AttributeUsage` zawiera inny nazwany parametr o nazwie `Inherited`, która wskazuje, czy atrybut, jeśli określony w klasie bazowej, również jest dziedziczone przez klasy, które pochodzą z tej klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-130">`AttributeUsage` has another named parameter called `Inherited`, which indicates whether the attribute, when specified on a base class, is also inherited by classes that derive from that base class.</span></span> <span data-ttu-id="cb7e0-131">Jeśli `Inherited` dla atrybutu klasy ma wartość true, a następnie ten atrybut jest dziedziczona.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-131">If `Inherited` for an attribute class is true, then that attribute is inherited.</span></span> <span data-ttu-id="cb7e0-132">Jeśli `Inherited` dla atrybutu klasy ma wartość false, a następnie ten atrybut nie jest dziedziczony.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-132">If `Inherited` for an attribute class is false then that attribute is not inherited.</span></span> <span data-ttu-id="cb7e0-133">Jeśli jest nieokreślony, jego wartość domyślna to true.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-133">If it is unspecified, its default value is true.</span></span>

<span data-ttu-id="cb7e0-134">Klasa atrybutu `X` niemający `AttributeUsage` atrybut jest dołączany do niego, podobnie jak w</span><span class="sxs-lookup"><span data-stu-id="cb7e0-134">An attribute class `X` not having an `AttributeUsage` attribute attached to it, as in</span></span>

```csharp
using System;

class X: Attribute {...}
```

<span data-ttu-id="cb7e0-135">Jest odpowiednikiem następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-135">is equivalent to the following:</span></span>

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a><span data-ttu-id="cb7e0-136">Parametry pozycyjne i nazwane</span><span class="sxs-lookup"><span data-stu-id="cb7e0-136">Positional and named parameters</span></span>

<span data-ttu-id="cb7e0-137">Klasy atrybutów może mieć ***parametry pozycyjne*** i ***nazwanych parametrów***.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-137">Attribute classes can have ***positional parameters*** and ***named parameters***.</span></span> <span data-ttu-id="cb7e0-138">Konstruktor publiczne wystąpienia każdej klasy atrybutu definiuje prawidłową sekwencją parametry pozycyjne dla tej klasy atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-138">Each public instance constructor for an attribute class defines a valid sequence of positional parameters for that attribute class.</span></span> <span data-ttu-id="cb7e0-139">Każdego niestatycznego publiczne odczytu / zapisu pola i właściwości dla atrybutu klasy definiuje nazwany parametr do klasy atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-139">Each non-static public read-write field and property for an attribute class defines a named parameter for the attribute class.</span></span>

<span data-ttu-id="cb7e0-140">Przykład</span><span class="sxs-lookup"><span data-stu-id="cb7e0-140">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

<span data-ttu-id="cb7e0-141">definiuje klasę atrybutu o nazwie `HelpAttribute` posiadającą jeden parametr pozycyjne, `url`, a jeden parametr o nazwie `Topic`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-141">defines an attribute class named `HelpAttribute` that has one positional parameter, `url`, and one named parameter, `Topic`.</span></span> <span data-ttu-id="cb7e0-142">Chociaż niestatyczna i publiczne, właściwość `Url` nie definiuje nazwany parametr, ponieważ nie jest odczytu i zapisu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-142">Although it is non-static and public, the property `Url` does not define a named parameter, since it is not read-write.</span></span>

<span data-ttu-id="cb7e0-143">Ten atrybut klasy mogą być używane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-143">This attribute class might be used as follows:</span></span>

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a><span data-ttu-id="cb7e0-144">Typy parametrów atrybutu</span><span class="sxs-lookup"><span data-stu-id="cb7e0-144">Attribute parameter types</span></span>

<span data-ttu-id="cb7e0-145">Typy parametry pozycyjne i nazwane atrybutu klasy nie są ograniczone do ***typy parametrów atrybutu***, które są:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-145">The types of positional and named parameters for an attribute class are limited to the ***attribute parameter types***, which are:</span></span>

*  <span data-ttu-id="cb7e0-146">Jednym z następujących typów: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-146">One of the following types: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span></span>
*  <span data-ttu-id="cb7e0-147">Typ `object`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-147">The type `object`.</span></span>
*  <span data-ttu-id="cb7e0-148">Typ `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-148">The type `System.Type`.</span></span>
*  <span data-ttu-id="cb7e0-149">Typ wyliczeniowy, pod warunkiem ma powszechnej dostępności i typy, w których jest zagnieżdżony, (jeśli istnieje) również mieć dostęp publiczny ([Specyfikacja atrybutu](attributes.md#attribute-specification)).</span><span class="sxs-lookup"><span data-stu-id="cb7e0-149">An enum type, provided it has public accessibility and the types in which it is nested (if any) also have public accessibility ([Attribute specification](attributes.md#attribute-specification)).</span></span>
*  <span data-ttu-id="cb7e0-150">Tablice jednowymiarowe z powyższych typów.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-150">Single-dimensional arrays of the above types.</span></span>
*  <span data-ttu-id="cb7e0-151">Argument konstruktora lub pole publiczne, która nie ma jednego z następujących typów nie może służyć jako parametr pozycyjne i nazwane w specyfikacji atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-151">A constructor argument or public field which does not have one of these types, cannot be used as a positional or named parameter in an attribute specification.</span></span>

## <a name="attribute-specification"></a><span data-ttu-id="cb7e0-152">Specyfikacji atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-152">Attribute specification</span></span>

<span data-ttu-id="cb7e0-153">***Specyfikacja atrybutu*** jest stosowanie wcześniej zdefiniowanego atrybutu do zgłoszenia.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-153">***Attribute specification*** is the application of a previously defined attribute to a declaration.</span></span> <span data-ttu-id="cb7e0-154">Atrybut jest fragmentem dodatkowych informacji deklaratywne, który jest określony w deklaracji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-154">An attribute is a piece of additional declarative information that is specified for a declaration.</span></span> <span data-ttu-id="cb7e0-155">Atrybuty można określić w zakresie globalnym (na przykład aby określić atrybuty na zawierającego zestaw lub moduł) i *type_declaration*s ([wpisz deklaracje](namespaces.md#type-declarations)), *class_member_declaration* s ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([interfejsu członków](interfaces.md#interface-members)), *struct_member _declaration*s ([składowe struktury](structs.md#struct-members)), *enum_member_declaration*s ([typu wyliczeniowego](enums.md#enum-members)), *accessor_declarations*  ([Akcesory](classes.md#accessors)), *event_accessor_declarations* ([zdarzenia podobne do pól](classes.md#field-like-events)), a *formal_parameter_list*s ([parametry metody](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="cb7e0-155">Attributes can be specified at global scope (to specify attributes on the containing assembly or module) and for *type_declaration*s ([Type declarations](namespaces.md#type-declarations)), *class_member_declaration*s ([Type parameter constraints](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([Interface members](interfaces.md#interface-members)), *struct_member_declaration*s ([Struct members](structs.md#struct-members)), *enum_member_declaration*s ([Enum members](enums.md#enum-members)), *accessor_declarations* ([Accessors](classes.md#accessors)), *event_accessor_declarations* ([Field-like events](classes.md#field-like-events)), and *formal_parameter_list*s ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="cb7e0-156">Atrybuty są określone w ***atrybutu sekcje***.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-156">Attributes are specified in ***attribute sections***.</span></span> <span data-ttu-id="cb7e0-157">Sekcja atrybut składa się z pary nawiasów kwadratowych, które otaczają przecinkami lista jednego lub więcej atrybutów.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-157">An attribute section consists of a pair of square brackets, which surround a comma-separated list of one or more attributes.</span></span> <span data-ttu-id="cb7e0-158">Kolejność, w którym atrybuty są określone w takiej liście i są ułożone kolejność, w którym sekcje dołączone do tej samej jednostki programu, nie ma znaczenia.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-158">The order in which attributes are specified in such a list, and the order in which sections attached to the same program entity are arranged, is not significant.</span></span> <span data-ttu-id="cb7e0-159">Na przykład specyfikacji atrybutu `[A][B]`, `[B][A]`, `[A,B]`, i `[B,A]` są równoważne.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-159">For instance, the attribute specifications `[A][B]`, `[B][A]`, `[A,B]`, and `[B,A]` are equivalent.</span></span>

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

<span data-ttu-id="cb7e0-160">Atrybut składa się z *attribute_name* i opcjonalną listę argumenty pozycyjne i nazwane.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-160">An attribute consists of an *attribute_name* and an optional list of positional and named arguments.</span></span> <span data-ttu-id="cb7e0-161">Argumenty pozycyjne (jeśli istnieje) poprzedzać nazwane argumenty.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-161">The positional arguments (if any) precede the named arguments.</span></span> <span data-ttu-id="cb7e0-162">Argument pozycyjny składa się z *attribute_argument_expression*; argument nazwany składa się z nazwy, następuje znak równości, następuje *attribute_argument_expression*, która razem , są ograniczone przez te same reguły jako przypisanie proste.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-162">A positional argument consists of an *attribute_argument_expression*; a named argument consists of a name, followed by an equal sign, followed by an *attribute_argument_expression*, which, together, are constrained by the same rules as simple assignment.</span></span> <span data-ttu-id="cb7e0-163">Kolejność argumentów nazwanych nie ma znaczenia.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-163">The order of named arguments is not significant.</span></span>

<span data-ttu-id="cb7e0-164">*Attribute_name* identyfikuje klasę atrybutów.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-164">The *attribute_name* identifies an attribute class.</span></span> <span data-ttu-id="cb7e0-165">Jeśli formie *attribute_name* jest *type_name* , a następnie ta nazwa musi odwoływać się do klasy atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-165">If the form of *attribute_name* is *type_name* then this name must refer to an attribute class.</span></span> <span data-ttu-id="cb7e0-166">W przeciwnym razie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-166">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="cb7e0-167">Przykład</span><span class="sxs-lookup"><span data-stu-id="cb7e0-167">The example</span></span>

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

<span data-ttu-id="cb7e0-168">powoduje błąd w czasie kompilacji, ponieważ próbuje ona użyć `Class1` jako atrybut klasy, gdy `Class1` nie jest to klasa atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-168">results in a compile-time error because it attempts to use `Class1` as an attribute class when `Class1` is not an attribute class.</span></span>

<span data-ttu-id="cb7e0-169">Pewnych kontekstach zezwalać na specyfikację atrybutu na więcej niż jeden element docelowy.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-169">Certain contexts permit the specification of an attribute on more than one target.</span></span> <span data-ttu-id="cb7e0-170">Program jawnie określić cel umieszczając *attribute_target_specifier*.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-170">A program can explicitly specify the target by including an *attribute_target_specifier*.</span></span> <span data-ttu-id="cb7e0-171">Gdy atrybut jest umieszczany na poziomie globalnym *global_attribute_target_specifier* jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-171">When an attribute is placed at the global level, a *global_attribute_target_specifier* is required.</span></span> <span data-ttu-id="cb7e0-172">W innych lokalizacjach uzasadnione domyślna jest stosowana, ale *attribute_target_specifier* może służyć potwierdzają lub zastąpić to ustawienie domyślne, w niektórych przypadkach niejednoznaczne (lub po prostu potwierdzają wartość domyślna w przypadku jednoznaczne).</span><span class="sxs-lookup"><span data-stu-id="cb7e0-172">In all other locations, a reasonable default is applied, but an *attribute_target_specifier* can be used to affirm or override the default in certain ambiguous cases (or to just affirm the default in non-ambiguous cases).</span></span> <span data-ttu-id="cb7e0-173">Ten sposób, zazwyczaj *attribute_target_specifier*s można pominąć, z wyjątkiem na poziomie globalnym.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-173">Thus, typically, *attribute_target_specifier*s can be omitted except at the global level.</span></span> <span data-ttu-id="cb7e0-174">Potencjalnie niejednoznaczne kontekstów są rozwiązywane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-174">The potentially ambiguous contexts are resolved as follows:</span></span>

*  <span data-ttu-id="cb7e0-175">Atrybut, który został określony w zakresie globalnym można zastosować do zestaw docelowy lub docelowy modułu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-175">An attribute specified at global scope can apply either to the target assembly or the target module.</span></span> <span data-ttu-id="cb7e0-176">Brak wartości domyślnej istnieje dla tego kontekstu, więc *attribute_target_specifier* jest zawsze wymagany w tym kontekście.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-176">No default exists for this context, so an *attribute_target_specifier* is always required in this context.</span></span> <span data-ttu-id="cb7e0-177">Obecność `assembly` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do obiektu docelowego zestawu; obecności `module` *attribute_target_specifier* Wskazuje, że ten atrybut ma zastosowanie do modułu, docelowej.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-177">The presence of the `assembly` *attribute_target_specifier* indicates that the attribute applies to the target assembly; the presence of the `module` *attribute_target_specifier* indicates that the attribute applies to the target module.</span></span>
*  <span data-ttu-id="cb7e0-178">Atrybut, który został określony w deklaracji delegate można zastosować delegata został zadeklarowany lub jego wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-178">An attribute specified on a delegate declaration can apply either to the delegate being declared or to its return value.</span></span> <span data-ttu-id="cb7e0-179">W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do delegata.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-179">In the absence of an *attribute_target_specifier*, the attribute applies to the delegate.</span></span> <span data-ttu-id="cb7e0-180">Obecność `type` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do delegata; obecności `return` *attribute_target_specifier* Wskazuje, że ten atrybut ma zastosowanie do wartości zwracanej.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-180">The presence of the `type` *attribute_target_specifier* indicates that the attribute applies to the delegate; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="cb7e0-181">Atrybut, który został określony w deklaracji metody można zastosować do metody został zadeklarowany lub jego wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-181">An attribute specified on a method declaration can apply either to the method being declared or to its return value.</span></span> <span data-ttu-id="cb7e0-182">W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do metody.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-182">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="cb7e0-183">Obecność `method` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do metody; obecności `return` *attribute_target_specifier* wskazuje czy ten atrybut ma zastosowanie do wartości zwracanej.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-183">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="cb7e0-184">Atrybut, który został określony w deklaracji operator można zastosować operatora został zadeklarowany lub jego wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-184">An attribute specified on an operator declaration can apply either to the operator being declared or to its return value.</span></span> <span data-ttu-id="cb7e0-185">W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do operatora.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-185">In the absence of an *attribute_target_specifier*, the attribute applies to the operator.</span></span> <span data-ttu-id="cb7e0-186">Obecność `method` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do operatora; obecności `return` *attribute_target_specifier* Wskazuje, że ten atrybut ma zastosowanie do wartości zwracanej.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-186">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the operator; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="cb7e0-187">Atrybut, który został określony w deklaracji zdarzenia, które pomija metod dostępu zdarzeń zastosować zdarzeń został zadeklarowany, skojarzonego pola (jeśli jest to zdarzenie nie jest abstrakcyjna) lub skojarzone Dodaj i Usuń metody.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-187">An attribute specified on an event declaration that omits event accessors can apply to the event being declared, to the associated field (if the event is not abstract), or to the associated add and remove methods.</span></span> <span data-ttu-id="cb7e0-188">W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-188">In the absence of an *attribute_target_specifier*, the attribute applies to the event.</span></span> <span data-ttu-id="cb7e0-189">Obecność `event` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do zdarzenia; obecności `field` *attribute_target_specifier* wskazuje Atrybut dotyczy polu. i obecności `method` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do metody.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-189">The presence of the `event` *attribute_target_specifier* indicates that the attribute applies to the event; the presence of the `field` *attribute_target_specifier* indicates that the attribute applies to the field; and the presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the methods.</span></span>
*  <span data-ttu-id="cb7e0-190">Atrybut, który został określony w deklaracji metody dostępu get w deklaracji właściwości lub indeksatora można zastosować skojarzoną metodę lub jego wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-190">An attribute specified on a get accessor declaration for a property or indexer declaration can apply either to the associated method or to its return value.</span></span> <span data-ttu-id="cb7e0-191">W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do metody.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-191">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="cb7e0-192">Obecność `method` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do metody; obecności `return` *attribute_target_specifier* wskazuje czy ten atrybut ma zastosowanie do wartości zwracanej.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-192">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="cb7e0-193">Atrybut, który został określony na dostępu set w deklaracji właściwości lub indeksatora można zastosować skojarzoną metodę lub jako pojedynczy parametr niejawne.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-193">An attribute specified on a set accessor for a property or indexer declaration can apply either to the associated method or to its lone implicit parameter.</span></span> <span data-ttu-id="cb7e0-194">W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do metody.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-194">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="cb7e0-195">Obecność `method` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do metody; obecności `param` *attribute_target_specifier* wskazuje Atrybut dotyczy parameter; obecność `return` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do wartości zwracanej.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-195">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="cb7e0-196">Atrybut jest określony w deklaracji metody dostępu add lub remove dla deklaracji zdarzenia można zastosować skojarzoną metodę lub jako pojedynczy parametr.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-196">An attribute specified on an add or remove accessor declaration for an event declaration can apply either to the associated method or to its lone parameter.</span></span> <span data-ttu-id="cb7e0-197">W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do metody.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-197">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="cb7e0-198">Obecność `method` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do metody; obecności `param` *attribute_target_specifier* wskazuje Atrybut dotyczy parameter; obecność `return` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do wartości zwracanej.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-198">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>

<span data-ttu-id="cb7e0-199">W innych kontekstach włączenia *attribute_target_specifier* jest dozwolone, ale niepotrzebne.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-199">In other contexts, inclusion of an *attribute_target_specifier* is permitted but unnecessary.</span></span> <span data-ttu-id="cb7e0-200">Na przykład deklarację klasy może zawierać lub pominąć specyfikator `type`:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-200">For instance, a class declaration may either include or omit the specifier `type`:</span></span>

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

<span data-ttu-id="cb7e0-201">Jest to błąd, aby określić nieprawidłową *attribute_target_specifier*.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-201">It is an error to specify an invalid *attribute_target_specifier*.</span></span> <span data-ttu-id="cb7e0-202">Na przykład specyfikator `param` nie można używać w deklaracji klasy:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-202">For instance, the specifier `param` cannot be used on a class declaration:</span></span>

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

<span data-ttu-id="cb7e0-203">Zgodnie z Konwencją klasy atrybutów są nazywane z sufiksem `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-203">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="cb7e0-204">*Attribute_name* formularza *type_name* może zawierać albo pominąć ten sufiks.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-204">An *attribute_name* of the form *type_name* may either include or omit this suffix.</span></span> <span data-ttu-id="cb7e0-205">Jeśli klasa atrybutu znajduje się zarówno z i bez tego sufiksu, niejednoznaczność jest obecny i powoduje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-205">If an attribute class is found both with and without this suffix, an ambiguity is present, and a compile-time error results.</span></span> <span data-ttu-id="cb7e0-206">Jeśli *attribute_name* została wpisana tak, aby jego najdalej z prawej strony *identyfikator* jest identyfikator dosłownego wyrażenia ([identyfikatory](lexical-structure.md#identifiers)), wówczas tylko atrybut bez sufiksu jest dopasowywany, umożliwiając w ten sposób niejednoznaczność zostać rozpoznane.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-206">If the *attribute_name* is spelled such that its right-most *identifier* is a verbatim identifier ([Identifiers](lexical-structure.md#identifiers)), then only an attribute without a suffix is matched, thus enabling such an ambiguity to be resolved.</span></span> <span data-ttu-id="cb7e0-207">Przykład</span><span class="sxs-lookup"><span data-stu-id="cb7e0-207">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

<span data-ttu-id="cb7e0-208">Pokazuje dwa atrybutu klasy o nazwie `X` i `XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-208">shows two attribute classes named `X` and `XAttribute`.</span></span> <span data-ttu-id="cb7e0-209">Ten atrybut `[X]` jest niejednoznaczny, ponieważ mogła się odwołać do jednej `X` lub `XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-209">The attribute `[X]` is ambiguous, since it could refer to either `X` or `XAttribute`.</span></span> <span data-ttu-id="cb7e0-210">Za pomocą identyfikator dosłownego wyrażenia umożliwia dokładne zamiar można określić w tych rzadkich przypadkach.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-210">Using a verbatim identifier allows the exact intent to be specified in such rare cases.</span></span> <span data-ttu-id="cb7e0-211">Ten atrybut `[XAttribute]` jest niejednoznaczny (mimo że będzie, jeśli było to klasa atrybutu o nazwie `XAttributeAttribute`!).</span><span class="sxs-lookup"><span data-stu-id="cb7e0-211">The attribute `[XAttribute]` is not ambiguous (although it would be if there was an attribute class named `XAttributeAttribute`!).</span></span> <span data-ttu-id="cb7e0-212">Jeśli w deklaracji klasy `X` zostanie usunięty, a następnie obu atrybutów odnoszą się do klasy atrybutu o nazwie `XAttribute`, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-212">If the declaration for class `X` is removed, then both attributes refer to the attribute class named `XAttribute`, as follows:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

<span data-ttu-id="cb7e0-213">Jest to błąd czasu kompilacji do użycia klasy atrybutu jednorazowego użycia więcej niż jeden raz na tej samej jednostki.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-213">It is a compile-time error to use a single-use attribute class more than once on the same entity.</span></span> <span data-ttu-id="cb7e0-214">Przykład</span><span class="sxs-lookup"><span data-stu-id="cb7e0-214">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

<span data-ttu-id="cb7e0-215">powoduje błąd w czasie kompilacji, ponieważ próbuje ona użyć `HelpString`, klasy atrybutu jednorazowego użycia, który jest więcej niż jeden raz na deklarację `Class1`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-215">results in a compile-time error because it attempts to use `HelpString`, which is a single-use attribute class, more than once on the declaration of `Class1`.</span></span>

<span data-ttu-id="cb7e0-216">Wyrażenie `E` jest *attribute_argument_expression* jeśli spełnione są wszystkie z następujących instrukcji:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-216">An expression `E` is an *attribute_argument_expression* if all of the following statements are true:</span></span>

*  <span data-ttu-id="cb7e0-217">Typ `E` jest typu parametru atrybutu ([typy parametrów atrybutu](attributes.md#attribute-parameter-types)).</span><span class="sxs-lookup"><span data-stu-id="cb7e0-217">The type of `E` is an attribute parameter type ([Attribute parameter types](attributes.md#attribute-parameter-types)).</span></span>
*  <span data-ttu-id="cb7e0-218">W czasie kompilacji, wartość `E` może zostać rozpoznana na jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-218">At compile-time, the value of `E` can be resolved to one of the following:</span></span>
   * <span data-ttu-id="cb7e0-219">Stała wartość.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-219">A constant value.</span></span>
   * <span data-ttu-id="cb7e0-220">Element `System.Type` obiektu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-220">A `System.Type` object.</span></span>
   * <span data-ttu-id="cb7e0-221">Jednowymiarowa tablica *attribute_argument_expression*s.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-221">A one-dimensional array of *attribute_argument_expression*s.</span></span>

<span data-ttu-id="cb7e0-222">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-222">For example:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

<span data-ttu-id="cb7e0-223">A *typeof_expression* ([typeof operator](expressions.md#the-typeof-operator)) używane jako wyrażenie argumentu atrybutu można odwoływać się do typu nieogólnego, zamknięte skonstruowanego typu lub niepowiązanych typu rodzajowego, ale go nie może odwoływać się Otwórz typu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-223">A *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)) used as an attribute argument expression can reference a non-generic type, a closed constructed type, or an unbound generic type, but it cannot reference an open type.</span></span> <span data-ttu-id="cb7e0-224">To, aby upewnić się, że wyrażenie może zostać rozpoznany w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-224">This is to ensure that the expression can be resolved at compile-time.</span></span>

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a><span data-ttu-id="cb7e0-225">Instancje wieloatrybutowe</span><span class="sxs-lookup"><span data-stu-id="cb7e0-225">Attribute instances</span></span>

<span data-ttu-id="cb7e0-226">***Wystąpienie atrybutu*** to wystąpienie, który reprezentuje atrybut, w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-226">An ***attribute instance*** is an instance that represents an attribute at run-time.</span></span> <span data-ttu-id="cb7e0-227">Atrybut jest zdefiniowana za pomocą klasy atrybutu argumentów pozycyjnych i argumenty nazwane.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-227">An attribute is defined with an attribute class, positional arguments, and named arguments.</span></span> <span data-ttu-id="cb7e0-228">Wystąpienie atrybutu jest wystąpieniem klasy atrybutu, który jest inicjowany za pomocą argumenty pozycyjne i nazwane.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-228">An attribute instance is an instance of the attribute class that is initialized with the positional and named arguments.</span></span>

<span data-ttu-id="cb7e0-229">Pobieranie wystąpienia atrybutu obejmuje zarówno w czasie kompilacji, jak i w czasie wykonywania przetwarzania, zgodnie z opisem w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-229">Retrieval of an attribute instance involves both compile-time and run-time processing, as described in the following sections.</span></span>

### <a name="compilation-of-an-attribute"></a><span data-ttu-id="cb7e0-230">Kompilacja atrybutu</span><span class="sxs-lookup"><span data-stu-id="cb7e0-230">Compilation of an attribute</span></span>

<span data-ttu-id="cb7e0-231">Kompilacja *atrybut* z klasy atrybutów `T`, *positional_argument_list* `P` i *named_argument_list* `N`, składa się z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-231">The compilation of an *attribute* with attribute class `T`, *positional_argument_list* `P` and *named_argument_list* `N`, consists of the following steps:</span></span>

*  <span data-ttu-id="cb7e0-232">Postępuj zgodnie z instrukcjami przetwarzanie w czasie kompilacji do kompilowania *object_creation_expression* formularza `new T(P)`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-232">Follow the compile-time processing steps for compiling an *object_creation_expression* of the form `new T(P)`.</span></span> <span data-ttu-id="cb7e0-233">Te kroki spowodować błąd kompilacji, albo określić konstruktora wystąpień `C` na `T` może być wywołana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-233">These steps either result in a compile-time error, or determine an instance constructor `C` on `T` that can be invoked at run-time.</span></span>
*  <span data-ttu-id="cb7e0-234">Jeśli `C` ma powszechnej dostępności, a następnie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-234">If `C` does not have public accessibility, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="cb7e0-235">Dla każdego *named_argument* `Arg` w `N`:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-235">For each *named_argument* `Arg` in `N`:</span></span>
   * <span data-ttu-id="cb7e0-236">Pozwól `Name` można *identyfikator* z *named_argument* `Arg`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-236">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span>
   * <span data-ttu-id="cb7e0-237">`Name` należy zidentyfikować niestatycznych odczytu i zapisu publiczne pole lub właściwość na `T`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-237">`Name` must identify a non-static read-write public field or property on `T`.</span></span> <span data-ttu-id="cb7e0-238">Jeśli `T` ma nie ma takiego pola lub właściwości, a następnie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-238">If `T` has no such field or property, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="cb7e0-239">Zachowaj następujące informacje dotyczące czasu wykonywania podczas tworzenia wystąpienia atrybutu: atrybut klasy `T`, Konstruktor wystąpienia `C` na `T`, *positional_argument_list* `P` i *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-239">Keep the following information for run-time instantiation of the attribute: the attribute class `T`, the instance constructor `C` on `T`, the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

### <a name="run-time-retrieval-of-an-attribute-instance"></a><span data-ttu-id="cb7e0-240">Pobieranie czasu wykonywania wystąpienia atrybutu</span><span class="sxs-lookup"><span data-stu-id="cb7e0-240">Run-time retrieval of an attribute instance</span></span>

<span data-ttu-id="cb7e0-241">Kompilacja *atrybut* daje to klasa atrybutu `T`, Konstruktor wystąpienia `C` na `T`, *positional_argument_list* `P`i *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-241">Compilation of an *attribute* yields an attribute class `T`, an instance constructor `C` on `T`, a *positional_argument_list* `P`, and a *named_argument_list* `N`.</span></span> <span data-ttu-id="cb7e0-242">Biorąc pod uwagę te informacje, wystąpienie atrybutu mogą być pobierane w czasie wykonywania, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-242">Given this information, an attribute instance can be retrieved at run-time using the following steps:</span></span>

*  <span data-ttu-id="cb7e0-243">Postępuj zgodnie z instrukcjami przetwarzanie w czasie wykonywania do wykonywania *object_creation_expression* formularza `new T(P)`, za pomocą konstruktora wystąpienia `C` ustalony w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-243">Follow the run-time processing steps for executing an *object_creation_expression* of the form `new T(P)`, using the instance constructor `C` as determined at compile-time.</span></span> <span data-ttu-id="cb7e0-244">Następujące kroki, powoduje wyjątek lub utworzyć wystąpienie `O` z `T`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-244">These steps either result in an exception, or produce an instance `O` of `T`.</span></span>
*  <span data-ttu-id="cb7e0-245">Dla każdego *named_argument* `Arg` w `N`, w kolejności:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-245">For each *named_argument* `Arg` in `N`, in order:</span></span>
   * <span data-ttu-id="cb7e0-246">Pozwól `Name` można *identyfikator* z *named_argument* `Arg`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-246">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span> <span data-ttu-id="cb7e0-247">Jeśli `Name` nie identyfikuje na niestatycznej publiczne odczytu / zapisu pole lub właściwość `O`, wówczas wyjątek jest zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-247">If `Name` does not identify a non-static public read-write field or property on `O`, then an exception is thrown.</span></span>
   * <span data-ttu-id="cb7e0-248">Pozwól `Value` być wynikiem oceny *attribute_argument_expression* z `Arg`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-248">Let `Value` be the result of evaluating the *attribute_argument_expression* of `Arg`.</span></span>
   * <span data-ttu-id="cb7e0-249">Jeśli `Name` identyfikuje pole na `O`, następnie ustaw to pole `Value`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-249">If `Name` identifies a field on `O`, then set this field to `Value`.</span></span>
   * <span data-ttu-id="cb7e0-250">W przeciwnym razie `Name` identyfikuje właściwość na `O`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-250">Otherwise, `Name` identifies a property on `O`.</span></span> <span data-ttu-id="cb7e0-251">Ustaw tę właściwość na `Value`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-251">Set this property to `Value`.</span></span>
   * <span data-ttu-id="cb7e0-252">Wynik jest `O`, wystąpienie klasy atrybutu `T` , została zainicjowana przy użyciu *positional_argument_list* `P` i *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-252">The result is `O`, an instance of the attribute class `T` that has been initialized with the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

## <a name="reserved-attributes"></a><span data-ttu-id="cb7e0-253">Atrybuty zarezerwowane</span><span class="sxs-lookup"><span data-stu-id="cb7e0-253">Reserved attributes</span></span>

<span data-ttu-id="cb7e0-254">Niewielka liczba atrybutów wpływa na język, w jakiś sposób.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-254">A small number of attributes affect the language in some way.</span></span> <span data-ttu-id="cb7e0-255">Te atrybuty obejmują:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-255">These attributes include:</span></span>

*  <span data-ttu-id="cb7e0-256">`System.AttributeUsageAttribute` ([Atrybut AttributeUsage](attributes.md#the-attributeusage-attribute)), który jest używany do opisu sposobów, w którym tworzy się klasę atrybutów może służyć.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-256">`System.AttributeUsageAttribute` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)), which is used to describe the ways in which an attribute class can be used.</span></span>
*  <span data-ttu-id="cb7e0-257">`System.Diagnostics.ConditionalAttribute` ([Atrybut Conditional](attributes.md#the-conditional-attribute)), która jest używana do definiowania metod warunkowych.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-257">`System.Diagnostics.ConditionalAttribute` ([The Conditional attribute](attributes.md#the-conditional-attribute)), which is used to define conditional methods.</span></span>
*  <span data-ttu-id="cb7e0-258">`System.ObsoleteAttribute` ([Przestarzałe atrybutu](attributes.md#the-obsolete-attribute)), który jest używany do oznaczania element członkowski jako przestarzałe.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-258">`System.ObsoleteAttribute` ([The Obsolete attribute](attributes.md#the-obsolete-attribute)), which is used to mark a member as obsolete.</span></span>
*  <span data-ttu-id="cb7e0-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` i `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Caller — atrybuty informacji](attributes.md#caller-info-attributes)), które są używane do dostarczania informacji na temat Kontekst wywołania do opcjonalnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` and `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Caller info attributes](attributes.md#caller-info-attributes)), which are used to supply information about the calling context to optional parameters.</span></span>

### <a name="the-attributeusage-attribute"></a><span data-ttu-id="cb7e0-260">Atrybut AttributeUsage</span><span class="sxs-lookup"><span data-stu-id="cb7e0-260">The AttributeUsage attribute</span></span>

<span data-ttu-id="cb7e0-261">Ten atrybut `AttributeUsage` służy do opisywania sposobu, w której można użyć klasy atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-261">The attribute `AttributeUsage` is used to describe the manner in which the attribute class can be used.</span></span>

<span data-ttu-id="cb7e0-262">Klasa, która zostanie nadany `AttributeUsage` atrybut musi pochodzić od klasy `System.Attribute`, bezpośrednio lub pośrednio.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-262">A class that is decorated with the `AttributeUsage` attribute must derive from `System.Attribute`, either directly or indirectly.</span></span> <span data-ttu-id="cb7e0-263">W przeciwnym razie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-263">Otherwise, a compile-time error occurs.</span></span>

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a><span data-ttu-id="cb7e0-264">Atrybut Conditional</span><span class="sxs-lookup"><span data-stu-id="cb7e0-264">The Conditional attribute</span></span>

<span data-ttu-id="cb7e0-265">Ten atrybut `Conditional` umożliwia określenie ***metoda warunkowa*** i ***klasy atrybut conditional***.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-265">The attribute `Conditional` enables the definition of ***conditional methods*** and ***conditional attribute classes***.</span></span>

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a><span data-ttu-id="cb7e0-266">Metoda warunkowa</span><span class="sxs-lookup"><span data-stu-id="cb7e0-266">Conditional methods</span></span>

<span data-ttu-id="cb7e0-267">Metoda ozdobione `Conditional` atrybutu jest metodą warunkowe.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-267">A method decorated with the `Conditional` attribute is a conditional method.</span></span> <span data-ttu-id="cb7e0-268">`Conditional` Atrybut wskazuje warunek, testując symbol kompilacji warunkowej.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-268">The `Conditional` attribute indicates a condition by testing a conditional compilation symbol.</span></span> <span data-ttu-id="cb7e0-269">Wywołania metody warunkowego, są uwzględnione lub pominięta, w zależności od tego, czy ten symbol jest zdefiniowany w punkcie połączenia.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-269">Calls to a conditional method are either included or omitted depending on whether this symbol is defined at the point of the call.</span></span> <span data-ttu-id="cb7e0-270">Jeśli zdefiniowano symbol wywołanie jest uwzględniany; w przeciwnym razie wywołanie (w tym ocena odbiornika i parametry połączenia) zostanie pominięty.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-270">If the symbol is defined, the call is included; otherwise, the call (including evaluation of the receiver and parameters of the call) is omitted.</span></span>

<span data-ttu-id="cb7e0-271">Metody warunkowego podlega następującym ograniczeniom:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-271">A conditional method is subject to the following restrictions:</span></span>

*  <span data-ttu-id="cb7e0-272">Metoda warunkowa musi być metodą w *class_declaration* lub *struct_declaration*.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-272">The conditional method must be a method in a *class_declaration* or *struct_declaration*.</span></span> <span data-ttu-id="cb7e0-273">Występuje błąd kompilacji, jeśli `Conditional` atrybut jest określony dla metody w deklaracji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-273">A compile-time error occurs if the `Conditional` attribute is specified on a method in an interface declaration.</span></span>
*  <span data-ttu-id="cb7e0-274">Metoda warunkowa musi mieć typ zwracany `void`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-274">The conditional method must have a return type of `void`.</span></span>
*  <span data-ttu-id="cb7e0-275">Metoda warunkowa nie może być oznaczona przy użyciu `override` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-275">The conditional method must not be marked with the `override` modifier.</span></span> <span data-ttu-id="cb7e0-276">Metoda warunkowego może być oznaczony za pomocą `virtual` modyfikator, jednak.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-276">A conditional method may be marked with the `virtual` modifier, however.</span></span> <span data-ttu-id="cb7e0-277">Zastąpienia taka metoda są niejawnie warunkowe i nie może być jawnie oznaczone za pomocą `Conditional` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-277">Overrides of such a method are implicitly conditional, and must not be explicitly marked with a `Conditional` attribute.</span></span>
*  <span data-ttu-id="cb7e0-278">Metoda warunkowa nie może być implementację metody interfejsu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-278">The conditional method must not be an implementation of an interface method.</span></span> <span data-ttu-id="cb7e0-279">W przeciwnym razie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-279">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="cb7e0-280">Ponadto błąd kompilacji występuje, jeśli metoda warunkowego jest stosowana w *delegate_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-280">In addition, a compile-time error occurs if a conditional method is used in a *delegate_creation_expression*.</span></span> <span data-ttu-id="cb7e0-281">Przykład</span><span class="sxs-lookup"><span data-stu-id="cb7e0-281">The example</span></span>

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

<span data-ttu-id="cb7e0-282">deklaruje `Class1.M` jako metody warunkowego.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-282">declares `Class1.M` as a conditional method.</span></span> <span data-ttu-id="cb7e0-283">`Class2`firmy `Test` metoda wywołuje tę metodę.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-283">`Class2`'s `Test` method calls this method.</span></span> <span data-ttu-id="cb7e0-284">Ponieważ symbol kompilacji warunkowej `DEBUG` jest zdefiniowana, jeśli `Class2.Test` jest wywołana, będzie wywoływać `M`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-284">Since the conditional compilation symbol `DEBUG` is defined, if `Class2.Test` is called, it will call `M`.</span></span> <span data-ttu-id="cb7e0-285">Jeśli symbol `DEBUG` miał nie został zdefiniowany, następnie `Class2.Test` nie może wywołać `Class1.M`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-285">If the symbol `DEBUG` had not been defined, then `Class2.Test` would not call `Class1.M`.</span></span>

<span data-ttu-id="cb7e0-286">Należy zauważyć, że dołączania lub wykluczania wywołanie metody warunkowego jest kontrolowana przez symbole kompilacji warunkowej w momencie wywołania.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-286">It is important to note that the inclusion or exclusion of a call to a conditional method is controlled by the conditional compilation symbols at the point of the call.</span></span> <span data-ttu-id="cb7e0-287">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="cb7e0-287">In the example</span></span>

<span data-ttu-id="cb7e0-288">Plik `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-288">File `class1.cs`:</span></span>

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

<span data-ttu-id="cb7e0-289">Plik `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-289">File `class2.cs`:</span></span>

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

<span data-ttu-id="cb7e0-290">Plik `class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-290">File `class3.cs`:</span></span>

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

<span data-ttu-id="cb7e0-291">klasy `Class2` i `Class3` każdy zawiera wywołania metody warunkowego `Class1.F`, który warunkowego opiera się na niezależnie od tego czy `DEBUG` jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-291">the classes `Class2` and `Class3` each contain calls to the conditional method `Class1.F`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="cb7e0-292">Ponieważ ten symbol jest zdefiniowany w kontekście `Class2` , ale nie `Class3`, wywołanie `F` w `Class2` jest uwzględniony, podczas wywołania `F` w `Class3` zostanie pominięty.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-292">Since this symbol is defined in the context of `Class2` but not `Class3`, the call to `F` in `Class2` is included, while the call to `F` in `Class3` is omitted.</span></span>

<span data-ttu-id="cb7e0-293">Używanie warunkowego metod w łańcuchu dziedziczenia może być mylące.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-293">The use of conditional methods in an inheritance chain can be confusing.</span></span> <span data-ttu-id="cb7e0-294">Wywołania metody warunkowego za pomocą `base`, w postaci `base.M`, podlegają zasadom wywołanie normalne metoda warunkowa.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-294">Calls made to a conditional method through `base`, of the form `base.M`, are subject to the normal conditional method call rules.</span></span> <span data-ttu-id="cb7e0-295">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="cb7e0-295">In the example</span></span>

<span data-ttu-id="cb7e0-296">Plik `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-296">File `class1.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

<span data-ttu-id="cb7e0-297">Plik `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-297">File `class2.cs`:</span></span>

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

<span data-ttu-id="cb7e0-298">Plik `class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-298">File `class3.cs`:</span></span>

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

<span data-ttu-id="cb7e0-299">`Class2` zawiera wywołanie `M` zdefiniowany w klasie bazowej.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-299">`Class2` includes a call to the `M` defined in its base class.</span></span> <span data-ttu-id="cb7e0-300">To wywołanie jest pominięty, ponieważ metoda podstawowa jest warunkowe na podstawie obecności symbol `DEBUG`, który jest niezdefiniowany.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-300">This call is omitted because the base method is conditional based on the presence of the symbol `DEBUG`, which is undefined.</span></span> <span data-ttu-id="cb7e0-301">Dlatego metoda zapisuje do konsoli "`Class2.M executed`" tylko.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-301">Thus, the method writes to the console "`Class2.M executed`" only.</span></span> <span data-ttu-id="cb7e0-302">Korzystać z *pp_declaration*s może wyeliminować takich problemów.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-302">Judicious use of *pp_declaration*s can eliminate such problems.</span></span>

#### <a name="conditional-attribute-classes"></a><span data-ttu-id="cb7e0-303">Atrybut Conditional klas</span><span class="sxs-lookup"><span data-stu-id="cb7e0-303">Conditional attribute classes</span></span>

<span data-ttu-id="cb7e0-304">Tworzy się klasę atrybutów ([atrybutu klasy](attributes.md#attribute-classes)) dekorowane za pomocą co najmniej jeden `Conditional` atrybutów jest ***klasy atrybut conditional***.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-304">An attribute class ([Attribute classes](attributes.md#attribute-classes)) decorated with one or more `Conditional` attributes is a ***conditional attribute class***.</span></span> <span data-ttu-id="cb7e0-305">Klasa atrybut conditional związku z tym jest skojarzona z symbole kompilacji warunkowej, zadeklarowany w jego `Conditional` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-305">A conditional attribute class is thus associated with the conditional compilation symbols declared in its `Conditional` attributes.</span></span> <span data-ttu-id="cb7e0-306">W tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-306">This example:</span></span>

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

<span data-ttu-id="cb7e0-307">deklaruje `TestAttribute` jako klasa atrybut conditional skojarzone z symbole kompilacji warunkowej `ALPHA` i `BETA`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-307">declares `TestAttribute` as a conditional attribute class associated with the conditional compilations symbols `ALPHA` and `BETA`.</span></span>

<span data-ttu-id="cb7e0-308">Atrybut specyfikacji ([Specyfikacja atrybutu](attributes.md#attribute-specification)) atrybutu warunkowego uwzględniono Jeśli co najmniej jeden z jego symbole kompilacji warunkowej skojarzone zdefiniowano punkcie specyfikacji, w przeciwnym razie atrybutu Specyfikacja zostanie pominięty.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-308">Attribute specifications ([Attribute specification](attributes.md#attribute-specification)) of a conditional attribute are included if one or more of its associated conditional compilation symbols is defined at the point of specification, otherwise the attribute specification is omitted.</span></span>

<span data-ttu-id="cb7e0-309">Należy zauważyć, że dołączania lub wykluczania specyfikacji atrybutu klasy atrybut conditional jest kontrolowane przez symbole kompilacji warunkowej w punkcie specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-309">It is important to note that the inclusion or exclusion of an attribute specification of a conditional attribute class is controlled by the conditional compilation symbols at the point of the specification.</span></span> <span data-ttu-id="cb7e0-310">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="cb7e0-310">In the example</span></span>

<span data-ttu-id="cb7e0-311">Plik `test.cs`:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-311">File `test.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

<span data-ttu-id="cb7e0-312">Plik `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-312">File `class1.cs`:</span></span>

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

<span data-ttu-id="cb7e0-313">Plik `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-313">File `class2.cs`:</span></span>

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

<span data-ttu-id="cb7e0-314">klasy `Class1` i `Class2` czy każdy dekorowane za pomocą atrybutu `Test`, który warunkowego opiera się na niezależnie od tego czy `DEBUG` jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-314">the classes `Class1` and `Class2` are each decorated with attribute `Test`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="cb7e0-315">Ponieważ ten symbol jest zdefiniowany w kontekście `Class1` , ale nie `Class2`, specyfikację `Test` atrybutu na `Class1` jest uwzględniony, podczas specyfikację `Test` atrybutu na `Class2` zostanie pominięty.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-315">Since this symbol is defined in the context of `Class1` but not `Class2`, the specification of the `Test` attribute on `Class1` is included, while the specification of the `Test` attribute on `Class2` is omitted.</span></span>

### <a name="the-obsolete-attribute"></a><span data-ttu-id="cb7e0-316">Atrybut przestarzałe</span><span class="sxs-lookup"><span data-stu-id="cb7e0-316">The Obsolete attribute</span></span>

<span data-ttu-id="cb7e0-317">Ten atrybut `Obsolete` służy do oznaczania typów i składowych typów, które nie powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-317">The attribute `Obsolete` is used to mark types and members of types that should no longer be used.</span></span>

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

<span data-ttu-id="cb7e0-318">Jeśli program używa typu lub elementu członkowskiego, który zostanie nadany `Obsolete` atrybutu, kompilator generuje ostrzeżenia lub błędu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-318">If a program uses a type or member that is decorated with the `Obsolete` attribute, the compiler issues a warning or an error.</span></span> <span data-ttu-id="cb7e0-319">Ściślej mówiąc, kompilator generuje ostrzeżenie, jeśli podano parametr nie błędu lub jeśli parametr błędu zostanie podana z wartością `false`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-319">Specifically, the compiler issues a warning if no error parameter is provided, or if the error parameter is provided and has the value `false`.</span></span> <span data-ttu-id="cb7e0-320">Kompilator generuje błąd, jeśli określono parametr błędu i ma wartość `true`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-320">The compiler issues an error if the error parameter is specified and has the value `true`.</span></span>

<span data-ttu-id="cb7e0-321">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="cb7e0-321">In the example</span></span>

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

<span data-ttu-id="cb7e0-322">Klasa `A` zostanie nadany `Obsolete` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-322">the class `A` is decorated with the `Obsolete` attribute.</span></span> <span data-ttu-id="cb7e0-323">Każde użycie `A` w `Main` wyników ostrzeżenia, który zawiera określony komunikat "Ta klasa jest przestarzała; Użyj klasy B."</span><span class="sxs-lookup"><span data-stu-id="cb7e0-323">Each use of `A` in `Main` results in a warning that includes the specified message, "This class is obsolete; use class B instead."</span></span>

### <a name="caller-info-attributes"></a><span data-ttu-id="cb7e0-324">Caller — atrybuty informacji</span><span class="sxs-lookup"><span data-stu-id="cb7e0-324">Caller info attributes</span></span>

<span data-ttu-id="cb7e0-325">Do celów takich jak rejestrowanie i raportowanie czasami jest przydatne w przypadku element członkowski funkcji uzyskać pewne informacje kompilacji dotyczące kodu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-325">For purposes such as logging and reporting, it is sometimes useful for a function member to obtain certain compile-time information about the calling code.</span></span> <span data-ttu-id="cb7e0-326">Caller — atrybuty informacji umożliwiają do przekazania takich informacji w sposób niewidoczny dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-326">The caller info attributes provide a way to pass such information transparently.</span></span>

<span data-ttu-id="cb7e0-327">Gdy parametr opcjonalny jest oznaczony za pomocą jednego z caller — atrybuty informacji, pomijając odnośnego argumentu w wywołaniu nie zawsze powoduje wartość domyślna parametru do zastąpienia.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-327">When an optional parameter is annotated with one of the caller info attributes, omitting the corresponding argument in a call does not necessarily cause the default parameter value to be substituted.</span></span> <span data-ttu-id="cb7e0-328">Zamiast tego Jeśli określone informacje o kontekście wywołania jest dostępna, te informacje zostaną przekazane jako wartość argumentu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-328">Instead, if the specified information about the calling context is available, that information will be passed as the argument value.</span></span>

<span data-ttu-id="cb7e0-329">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cb7e0-329">For example:</span></span>

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

<span data-ttu-id="cb7e0-330">Wywołanie `Log()` bez argumentów zostanie wydrukowana w wierszu numer i ścieżkę pliku wywołanie, a także nazwę elementu członkowskiego, w którym wystąpił wywołania.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-330">A call to `Log()` with no arguments would print the line number and file path of the call, as well as the name of the member within which the call occurred.</span></span>

<span data-ttu-id="cb7e0-331">Caller — atrybuty informacji może wystąpić w dowolnym miejscu, następujące parametry opcjonalne, w tym w deklaracjach delegata.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-331">Caller info attributes can occur on optional parameters anywhere, including in delegate declarations.</span></span> <span data-ttu-id="cb7e0-332">Jednak określonych caller — atrybuty informacji ma żadnych ograniczeń dotyczących rodzajów parametry, których można atrybutu tak, aby zawsze będzie niejawna konwersja z podstawioną wartość do typu parametru.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-332">However, the specific caller info attributes have restrictions on the types of the parameters they can attribute, so that there will always be an implicit conversion from a substituted value to the parameter type.</span></span>

<span data-ttu-id="cb7e0-333">Jest to błąd, musi mieć tego samego atrybutu obiektu wywołującego w informacje dotyczące parametru definiowania i wdrażania część deklaracji metody częściowej.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-333">It is an error to have the same caller info attribute on a parameter of both the defining and implementing part of a partial method declaration.</span></span> <span data-ttu-id="cb7e0-334">Tylko caller — atrybuty informacji w części definiujące są stosowane, natomiast caller — atrybuty informacji występuje tylko w części implementującej są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-334">Only caller info attributes in the defining part are applied, whereas caller info attributes occurring only in the implementing part are ignored.</span></span>

<span data-ttu-id="cb7e0-335">Informacje o wywołującym nie ma wpływu na rozpoznanie przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-335">Caller information does not affect overload resolution.</span></span> <span data-ttu-id="cb7e0-336">Zgodnie z atrybutami następujące parametry opcjonalne są nadal pominięty z kodu źródłowego obiektu wywołującego, funkcja rozpoznawania przeciążeń ignoruje tych parametrów w taki sam sposób ignoruje inne pominięty parametry opcjonalne ([Rozpoznanie przeciążenia](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="cb7e0-336">As the attributed optional parameters are still omitted from the source code of the caller, overload resolution ignores those parameters in the same way it ignores other omitted optional parameters ([Overload resolution](expressions.md#overload-resolution)).</span></span>

<span data-ttu-id="cb7e0-337">Informacje o wywołującym tylko zostanie zastąpiony, gdy funkcja jest jawnie wywoływana w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-337">Caller information is only substituted when a function is explicitly invoked in source code.</span></span> <span data-ttu-id="cb7e0-338">Niejawne wywołania, takie jak nadrzędnego niejawne Konstruktor wywołuje nie mają lokalizacji źródłowej i nie zostanie zastąpiony przez informacje o wywołującym.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-338">Implicit invocations such as implicit parent constructor calls do not have a source location and will not substitute caller information.</span></span> <span data-ttu-id="cb7e0-339">Ponadto wywołania, które są dynamicznie powiązane nie zostanie zastąpiony przez informacje o wywołującym.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-339">Also, calls that are dynamically bound will not substitute caller information.</span></span> <span data-ttu-id="cb7e0-340">Gdy informacji o obiekcie wywołującym opartego na atrybutach parametr zostanie pominięty w takich przypadkach w zamian jest używana wartość określoną wartość domyślną parametru.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-340">When a caller info attributed parameter is omitted in such cases, the specified default value of the parameter is used instead.</span></span>

<span data-ttu-id="cb7e0-341">Jedynym wyjątkiem jest wyrażenia zapytania.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-341">One exception is query-expressions.</span></span> <span data-ttu-id="cb7e0-342">Są one traktowane jako rozszerzenia składni, a jeśli wywołań, mogą zwiększyć Pomiń parametry opcjonalne z caller — atrybuty informacji, informacje o wywołującym zostaną zastąpione.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-342">These are considered syntactic expansions, and if the calls they expand to omit optional parameters with caller info attributes, caller information will be substituted.</span></span> <span data-ttu-id="cb7e0-343">Lokalizacja używana jest lokalizacja klauzul zapytania, które wywołanie został wygenerowany z.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-343">The location used is the location of the query clause which the call was generated from.</span></span>

<span data-ttu-id="cb7e0-344">Jeśli więcej niż jeden atrybut informacje obiekt wywołujący jest określony dla danego parametru, są preferowane w następującej kolejności: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-344">If more than one caller info attribute is specified on a given parameter, they are preferred in the following order: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span></span>

#### <a name="the-callerlinenumber-attribute"></a><span data-ttu-id="cb7e0-345">Atrybut CallerLineNumber</span><span class="sxs-lookup"><span data-stu-id="cb7e0-345">The CallerLineNumber attribute</span></span>

<span data-ttu-id="cb7e0-346">`System.Runtime.CompilerServices.CallerLineNumberAttribute` Jest dozwolone na temat parametrów opcjonalnych, gdy istnieje niejawna konwersja standardowa ([standardowe i niejawne konwersje](conversions.md#standard-implicit-conversions)) z wartości stałej `int.MaxValue` typowi parametru.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-346">The `System.Runtime.CompilerServices.CallerLineNumberAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from the constant value `int.MaxValue` to the parameter's type.</span></span> <span data-ttu-id="cb7e0-347">Daje to gwarancję, że można przekazać dowolną liczbę nieujemną wiersza do tej wartości bez błędu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-347">This ensures that any non-negative line number up to that value can be passed without error.</span></span>

<span data-ttu-id="cb7e0-348">Jeśli wywołanie funkcji z lokalizacji w kodzie źródłowym pomija parametr opcjonalny z `CallerLineNumberAttribute`, a następnie literał liczbowy reprezentujący numer wiersza w tej lokalizacji jest używana jako argument wywołania zamiast domyślnej wartości parametru.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-348">If a function invocation from a location in source code omits an optional parameter with the `CallerLineNumberAttribute`, then a numeric literal representing that location's line number is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="cb7e0-349">Jeśli wywołanie obejmuje wiele wierszy, wiersz wybrany zależy od implementacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-349">If the invocation spans multiple lines, the line chosen is implementation-dependent.</span></span>

<span data-ttu-id="cb7e0-350">Należy zauważyć, że numer wiersza może mieć wpływ `#line` dyrektywy ([wiersz dyrektywy](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="cb7e0-350">Note that the line number may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callerfilepath-attribute"></a><span data-ttu-id="cb7e0-351">Atrybut CallerFilePath</span><span class="sxs-lookup"><span data-stu-id="cb7e0-351">The CallerFilePath attribute</span></span>

<span data-ttu-id="cb7e0-352">`System.Runtime.CompilerServices.CallerFilePathAttribute` Jest dozwolone na temat parametrów opcjonalnych, gdy istnieje niejawna konwersja standardowa ([standardowe i niejawne konwersje](conversions.md#standard-implicit-conversions)) z `string` typowi parametru.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-352">The `System.Runtime.CompilerServices.CallerFilePathAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="cb7e0-353">Jeśli wywołanie funkcji z lokalizacji w kodzie źródłowym pomija opcjonalny parametr za pomocą `CallerFilePathAttribute`, następnie literał ciągu reprezentujący tej lokalizacji ścieżki pliku jest używana jako argument wywołania zamiast domyślnej wartości parametru.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-353">If a function invocation from a location in source code omits an optional parameter with the `CallerFilePathAttribute`, then a string literal representing that location's file path is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="cb7e0-354">Format ścieżki pliku zależy od implementacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-354">The format of the file path is implementation-dependent.</span></span>

<span data-ttu-id="cb7e0-355">Należy zauważyć, że ścieżka pliku mogą mieć wpływ na `#line` dyrektywy ([wiersz dyrektywy](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="cb7e0-355">Note that the file path may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callermembername-attribute"></a><span data-ttu-id="cb7e0-356">Atrybut CallerMemberName</span><span class="sxs-lookup"><span data-stu-id="cb7e0-356">The CallerMemberName attribute</span></span>

<span data-ttu-id="cb7e0-357">`System.Runtime.CompilerServices.CallerMemberNameAttribute` Jest dozwolone na temat parametrów opcjonalnych, gdy istnieje niejawna konwersja standardowa ([standardowe i niejawne konwersje](conversions.md#standard-implicit-conversions)) z `string` typowi parametru.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-357">The `System.Runtime.CompilerServices.CallerMemberNameAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="cb7e0-358">Wywołania funkcji z lokalizacji, w ramach treści funkcji składowej lub atrybut zastosowania do elementu członkowskiego funkcja sam lub jego typem zwracanym, parametry lub parametrów typu w kodzie źródłowym pomija parametr opcjonalny z `CallerMemberNameAttribute`, a następnie literał ciągu reprezentujący nazwę tego elementu członkowskiego jest używana jako argument wywołania zamiast domyślnej wartości parametru.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-358">If a function invocation from a location within the body of a function member or within an attribute applied to the function member itself or its return type, parameters or type parameters in source code omits an optional parameter with the `CallerMemberNameAttribute`, then a string literal representing the name of that member is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="cb7e0-359">Wywołania występujących w ramach metod ogólnych, aby uzyskać nazwę metody, sama jest używany bez lista parametrów typu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-359">For invocations that occur within generic methods, only the method name itself is used, without the type parameter list.</span></span>

<span data-ttu-id="cb7e0-360">Do wywołania, które występują w implementacji elementu członkowskiego interfejsu jawnego nazwę metody, sama jest używany bez poprzedzającego kwalifikacji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-360">For invocations that occur within explicit interface member implementations, only the method name itself is used, without the preceding interface qualification.</span></span>

<span data-ttu-id="cb7e0-361">Do wywołania, występujących w ramach metod dostępu właściwości lub zdarzenia nazwę elementu członkowskiego, używana jest właściwość lub zdarzenie, sam.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-361">For invocations that occur within property or event accessors, the member name used is that of the property or event itself.</span></span>

<span data-ttu-id="cb7e0-362">Wywołania występujących w ramach metod dostępu indeksatora, nazwę elementu członkowskiego, używany jest dostarczona przez `IndexerNameAttribute` ([atrybutu IndexerName](attributes.md#the-indexername-attribute)) elementu członkowskiego indeksatora, jeśli jest obecny lub domyślna nazwa `Item` inaczej.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-362">For invocations that occur within indexer accessors, the member name used is that supplied by an `IndexerNameAttribute` ([The IndexerName attribute](attributes.md#the-indexername-attribute)) on the indexer member, if present, or the default name `Item` otherwise.</span></span>

<span data-ttu-id="cb7e0-363">Do wywołania, znajdujących się w deklaracji konstruktory wystąpień, konstruktorów statycznych, destruktory i operatory elementu członkowskiego używana nazwa jest zależna od implementacji.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-363">For invocations that occur within declarations of instance constructors, static constructors, destructors and operators the member name used is implementation-dependent.</span></span>

## <a name="attributes-for-interoperation"></a><span data-ttu-id="cb7e0-364">Atrybuty do celów międzyoperacyjności</span><span class="sxs-lookup"><span data-stu-id="cb7e0-364">Attributes for Interoperation</span></span>

<span data-ttu-id="cb7e0-365">Uwaga: Ta sekcja ma zastosowanie tylko do wdrożenia programu Microsoft .NET C#.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-365">Note: This section is applicable only to the Microsoft .NET implementation of C#.</span></span>

### <a name="interoperation-with-com-and-win32-components"></a><span data-ttu-id="cb7e0-366">Współdziałanie z składniki COM i Win32</span><span class="sxs-lookup"><span data-stu-id="cb7e0-366">Interoperation with COM and Win32 components</span></span>

<span data-ttu-id="cb7e0-367">Środowiska wykonawczego .NET zawiera dużą liczbę atrybutów, które umożliwiają C# programy pod kątem współdziałania ze składnikami napisanymi przy użyciu modelu COM i bibliotek Win32 dll.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-367">The .NET run-time provides a large number of attributes that enable C# programs to interoperate with components written using COM and Win32 DLLs.</span></span> <span data-ttu-id="cb7e0-368">Na przykład `DllImport` atrybut może być używany na `static extern` metodę w celu wskazania, że implementacji metody ma zostać znaleziona w bibliotece DLL systemu Win32.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-368">For example, the `DllImport` attribute can be used on a `static extern` method to indicate that the implementation of the method is to be found in a Win32 DLL.</span></span> <span data-ttu-id="cb7e0-369">Te atrybuty znajdują się w `System.Runtime.InteropServices` przestrzeni nazw i szczegółowa dokumentacja tych atrybutów znajduje się w dokumentacji środowiska uruchomieniowego .NET.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-369">These attributes are found in the `System.Runtime.InteropServices` namespace, and detailed documentation for these attributes is found in the .NET runtime documentation.</span></span>

### <a name="interoperation-with-other-net-languages"></a><span data-ttu-id="cb7e0-370">Współdziałanie z innymi językami .NET</span><span class="sxs-lookup"><span data-stu-id="cb7e0-370">Interoperation with other .NET languages</span></span>

#### <a name="the-indexername-attribute"></a><span data-ttu-id="cb7e0-371">Atrybutu IndexerName</span><span class="sxs-lookup"><span data-stu-id="cb7e0-371">The IndexerName attribute</span></span>

<span data-ttu-id="cb7e0-372">Indeksatory są implementowane w środowisku .NET za pomocą właściwości indeksowanych i mieć nazwę w metadanych platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-372">Indexers are implemented in .NET using indexed properties, and have a name in the .NET metadata.</span></span> <span data-ttu-id="cb7e0-373">Jeśli nie `IndexerName` atrybut był obecny indeksatora, a następnie nazwę `Item` jest używane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-373">If no `IndexerName` attribute is present for an indexer, then the name `Item` is used by default.</span></span> <span data-ttu-id="cb7e0-374">`IndexerName` Atrybut umożliwia dla deweloperów zastąpić to ustawienie domyślne i określ inną nazwę.</span><span class="sxs-lookup"><span data-stu-id="cb7e0-374">The `IndexerName` attribute enables a developer to override this default and specify a different name.</span></span>

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
