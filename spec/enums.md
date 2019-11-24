---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703967"
---
# <a name="enums"></a><span data-ttu-id="6d367-101">Wyliczenia</span><span class="sxs-lookup"><span data-stu-id="6d367-101">Enums</span></span>

<span data-ttu-id="6d367-102">***Typ wyliczeniowy*** jest odrębnym typem wartości ([typy wartości](types.md#value-types)), który deklaruje zestaw nazwanych stałych.</span><span class="sxs-lookup"><span data-stu-id="6d367-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="6d367-103">Przykład</span><span class="sxs-lookup"><span data-stu-id="6d367-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="6d367-104">deklaruje typ wyliczeniowy o nazwie `Color` z członkami `Red`, `Green`i `Blue`.</span><span class="sxs-lookup"><span data-stu-id="6d367-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="6d367-105">Deklaracje wyliczenia</span><span class="sxs-lookup"><span data-stu-id="6d367-105">Enum declarations</span></span>

<span data-ttu-id="6d367-106">Deklaracja wyliczenia deklaruje nowy typ wyliczeniowy.</span><span class="sxs-lookup"><span data-stu-id="6d367-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="6d367-107">Deklaracja wyliczenia rozpoczyna się od słowa kluczowego `enum`i definiuje nazwę, ułatwienia dostępu, typ podstawowy i elementy członkowskie wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="6d367-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

<span data-ttu-id="6d367-108">Każdy typ wyliczeniowy ma odpowiedni typ całkowity nazywany ***typem podstawowym*** typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="6d367-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="6d367-109">Ten typ podstawowy musi być w stanie reprezentować wszystkie wartości modułu wyliczającego zdefiniowane w wyliczeniu.</span><span class="sxs-lookup"><span data-stu-id="6d367-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="6d367-110">Deklaracja wyliczenia może jawnie zadeklarować typ podstawowy `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` lub `ulong`.</span><span class="sxs-lookup"><span data-stu-id="6d367-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="6d367-111">Należy zauważyć, że `char` nie może być używany jako typ podstawowy.</span><span class="sxs-lookup"><span data-stu-id="6d367-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="6d367-112">Deklaracja wyliczenia, która nie deklaruje jawnie typu podstawowego, ma typ podstawowy `int`.</span><span class="sxs-lookup"><span data-stu-id="6d367-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="6d367-113">Przykład</span><span class="sxs-lookup"><span data-stu-id="6d367-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="6d367-114">deklaruje Wyliczenie z typem podstawowym `long`.</span><span class="sxs-lookup"><span data-stu-id="6d367-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="6d367-115">Deweloper może zdecydować się na użycie podstawowego typu `long`, tak jak w przykładzie, aby umożliwić użycie wartości, które znajdują się w zakresie `long`, ale nie w zakresie `int`lub aby zachować tę opcję w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="6d367-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="6d367-116">Modyfikatory wyliczenia</span><span class="sxs-lookup"><span data-stu-id="6d367-116">Enum modifiers</span></span>

<span data-ttu-id="6d367-117">*Enum_declaration* może opcjonalnie zawierać sekwencję modyfikatorów wyliczenia:</span><span class="sxs-lookup"><span data-stu-id="6d367-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="6d367-118">Jest to błąd czasu kompilacji dla tego samego modyfikatora do wyświetlenia wiele razy w deklaracji wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="6d367-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="6d367-119">Modyfikatory deklaracji wyliczenia mają takie samo znaczenie jak dla deklaracji klasy ([Modyfikatory klas](classes.md#class-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="6d367-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="6d367-120">Należy jednak zauważyć, że Modyfikatory `abstract` i `sealed` są niedozwolone w deklaracji wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="6d367-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="6d367-121">Wyliczenia nie mogą być abstrakcyjne i nie zezwalają na wyprowadzanie.</span><span class="sxs-lookup"><span data-stu-id="6d367-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="6d367-122">Elementy członkowskie wyliczenia</span><span class="sxs-lookup"><span data-stu-id="6d367-122">Enum members</span></span>

<span data-ttu-id="6d367-123">Treść deklaracji typu wyliczeniowego definiuje zero lub więcej elementów członkowskich wyliczenia, które są nazwanymi stałymi typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="6d367-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="6d367-124">Żadne dwa elementy członkowskie wyliczenia nie mogą mieć takiej samej nazwy.</span><span class="sxs-lookup"><span data-stu-id="6d367-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="6d367-125">Każdy element członkowski wyliczenia ma skojarzoną wartość stałą.</span><span class="sxs-lookup"><span data-stu-id="6d367-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="6d367-126">Typ tej wartości jest typem podstawowym dla zawierającego Wyliczenie.</span><span class="sxs-lookup"><span data-stu-id="6d367-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="6d367-127">Wartość stała dla każdego elementu członkowskiego wyliczenia musi znajdować się w zakresie typu podstawowego dla wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="6d367-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="6d367-128">Przykład</span><span class="sxs-lookup"><span data-stu-id="6d367-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="6d367-129">powoduje błąd czasu kompilacji, ponieważ wartości stałe `-1`, `-2`i `-3` nie należą do zakresu podstawowej `uint`typu całkowitego.</span><span class="sxs-lookup"><span data-stu-id="6d367-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="6d367-130">Wiele elementów członkowskich wyliczenia może korzystać z tej samej skojarzonej wartości.</span><span class="sxs-lookup"><span data-stu-id="6d367-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="6d367-131">Przykład</span><span class="sxs-lookup"><span data-stu-id="6d367-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="6d367-132">pokazuje Wyliczenie, w którym znajdują się dwa elementy członkowskie wyliczenia--`Blue` i `Max`--mają tę samą skojarzoną wartość.</span><span class="sxs-lookup"><span data-stu-id="6d367-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="6d367-133">Skojarzona wartość elementu członkowskiego wyliczenia jest przypisywana niejawnie lub jawnie.</span><span class="sxs-lookup"><span data-stu-id="6d367-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="6d367-134">Jeśli deklaracja elementu członkowskiego wyliczenia ma inicjator *constant_expression* , wartość tego wyrażenia stałej, niejawnie przekonwertowana na typ podstawowy wyliczenia, jest skojarzoną wartością elementu członkowskiego wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="6d367-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="6d367-135">Jeśli deklaracja elementu członkowskiego wyliczenia nie ma inicjatora, jego skojarzona wartość jest ustawiana niejawnie w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="6d367-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="6d367-136">Jeśli składowa wyliczenia jest pierwszym elementem członkowskim wyliczenia zadeklarowanym w typie wyliczeniowym, jego skojarzona wartość jest równa zero.</span><span class="sxs-lookup"><span data-stu-id="6d367-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="6d367-137">W przeciwnym razie skojarzona wartość elementu członkowskiego wyliczenia jest uzyskiwana przez zwiększenie skojarzonej wartości składowej po raz ostatni przez jeden.</span><span class="sxs-lookup"><span data-stu-id="6d367-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="6d367-138">Ta zwiększona wartość musi znajdować się w zakresie wartości, które mogą być reprezentowane przez typ podstawowy, w przeciwnym razie wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6d367-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="6d367-139">Przykład</span><span class="sxs-lookup"><span data-stu-id="6d367-139">The example</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

<span data-ttu-id="6d367-140">drukuje nazwy elementów członkowskich wyliczenia i skojarzonych z nimi wartości.</span><span class="sxs-lookup"><span data-stu-id="6d367-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="6d367-141">Dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="6d367-141">The output is:</span></span>

```console
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="6d367-142">z następujących powodów:</span><span class="sxs-lookup"><span data-stu-id="6d367-142">for the following reasons:</span></span>

*  <span data-ttu-id="6d367-143">`Red` elementu członkowskiego wyliczenia jest automatycznie przypisana wartość zero (ponieważ nie ma inicjatora i jest pierwszym elementem członkowskim wyliczenia);</span><span class="sxs-lookup"><span data-stu-id="6d367-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="6d367-144">`Green` elementu członkowskiego wyliczenia jawnie podano wartość `10`;</span><span class="sxs-lookup"><span data-stu-id="6d367-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="6d367-145">a `Blue` elementu członkowskiego wyliczenia jest automatycznie przypisywana wartość, która jest większa od elementu członkowskiego, który jest poprzedzony znakiem.</span><span class="sxs-lookup"><span data-stu-id="6d367-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="6d367-146">Skojarzona wartość elementu członkowskiego wyliczenia może nie, bezpośrednio lub pośrednio, używać wartości własnego skojarzonego elementu członkowskiego wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="6d367-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="6d367-147">Oprócz tego ograniczenia cykliczności inicjatory składowych enum mogą swobodnie odwoływać się do innych inicjatorów składowych wyliczenia, niezależnie od ich pozycji tekstowych.</span><span class="sxs-lookup"><span data-stu-id="6d367-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="6d367-148">W obrębie inicjatora elementu członkowskiego wyliczenia wartości innych elementów członkowskich wyliczenia są zawsze traktowane jako mają typ ich typ podstawowy, więc rzutowania nie są konieczne w przypadku odwoływania się do innych elementów członkowskich wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="6d367-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="6d367-149">Przykład</span><span class="sxs-lookup"><span data-stu-id="6d367-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="6d367-150">powoduje błąd czasu kompilacji, ponieważ deklaracje `A` i `B` są cykliczne.</span><span class="sxs-lookup"><span data-stu-id="6d367-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="6d367-151">`A` zależy od `B` jawnie, a `B` zależy od niejawnego `A`.</span><span class="sxs-lookup"><span data-stu-id="6d367-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="6d367-152">Elementy członkowskie wyliczenia są nazwane i w zakresie dokładnie analogiczne do pól w klasach.</span><span class="sxs-lookup"><span data-stu-id="6d367-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="6d367-153">Zakres elementu członkowskiego wyliczenia jest treścią zawierającego typ wyliczeniowy.</span><span class="sxs-lookup"><span data-stu-id="6d367-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="6d367-154">W tym zakresie elementy członkowskie wyliczenia mogą być określane przez ich prostą nazwę.</span><span class="sxs-lookup"><span data-stu-id="6d367-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="6d367-155">Ze wszystkich innych kodów nazwa elementu członkowskiego wyliczenia musi być kwalifikowana nazwą jego typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="6d367-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="6d367-156">Elementy członkowskie wyliczenia nie mają zadeklarowanych dostępności — element członkowski wyliczenia jest dostępny, jeśli jego typ wyliczeniowy jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="6d367-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="6d367-157">Typ System. Enum</span><span class="sxs-lookup"><span data-stu-id="6d367-157">The System.Enum type</span></span>

<span data-ttu-id="6d367-158">Typ `System.Enum` jest abstrakcyjną klasą bazową wszystkich typów wyliczeniowych (jest to różne i inne niż typ podstawowy typu wyliczeniowego), a elementy członkowskie dziedziczone z `System.Enum` są dostępne w dowolnym typie wyliczeniowym.</span><span class="sxs-lookup"><span data-stu-id="6d367-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="6d367-159">Konwersja z opakowania ([konwersje pakujące](types.md#boxing-conversions)) istnieje z dowolnego typu wyliczeniowego do `System.Enum`, a konwersja rozpakowywania ([konwersje rozpakowywanie](types.md#unboxing-conversions)) istnieje z `System.Enum` do dowolnego typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="6d367-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="6d367-160">Należy pamiętać, że `System.Enum` nie należy do *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="6d367-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="6d367-161">Nie jest to *class_type* , z której pochodzą wszystkie *enum_type*s.</span><span class="sxs-lookup"><span data-stu-id="6d367-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="6d367-162">Typ `System.Enum` dziedziczy z typu `System.ValueType` ([Typ System. ValueType](types.md#the-systemvaluetype-type)), który z kolei dziedziczy po typie `object`.</span><span class="sxs-lookup"><span data-stu-id="6d367-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="6d367-163">W czasie wykonywania, wartość typu `System.Enum` może być `null` lub odwołanie do wartości opakowanej dowolnego typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="6d367-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="6d367-164">Wartości wyliczeniowe i operacje</span><span class="sxs-lookup"><span data-stu-id="6d367-164">Enum values and operations</span></span>

<span data-ttu-id="6d367-165">Każdy typ wyliczeniowy definiuje odrębny typ; jawna konwersja wyliczenia ([jawne konwersje wyliczenia](conversions.md#explicit-enumeration-conversions)) jest wymagana do konwersji między typem wyliczenia a typem całkowitym lub między dwoma typami wyliczeniowymi.</span><span class="sxs-lookup"><span data-stu-id="6d367-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="6d367-166">Zbiór wartości, dla których można zastosować typ wyliczeniowy, nie jest ograniczony przez elementy członkowskie wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="6d367-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="6d367-167">W szczególności każda wartość typu podstawowego wyliczenia może być rzutowana na typ wyliczeniowy i jest odrębną prawidłową wartością tego typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="6d367-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="6d367-168">Elementy członkowskie wyliczenia mają typ zawierający typ wyliczeniowy (z wyjątkiem innych inicjatorów elementów członkowskich wyliczenia: zobacz [elementy członkowskie wyliczenia](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="6d367-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="6d367-169">Wartość elementu członkowskiego wyliczenia zadeklarowana w typie wyliczeniowy `E` ze skojarzoną wartością `v` jest `(E)v`.</span><span class="sxs-lookup"><span data-stu-id="6d367-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="6d367-170">Następujące operatory mogą być używane na wartości typów wyliczeniowych: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Operatory porównania wyliczenia](expressions.md#enumeration-comparison-operators)), binarne `+` ([operator dodawania](expressions.md#addition-operator)), Binary `-` ([operator odejmowania](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Wyliczenie operatory logiczne](expressions.md#enumeration-logical-operators)), `~` ([operator](expressions.md#bitwise-complement-operator)dopełnienia bitowego), `++` i `--` ( [Operatory przyrostka zwiększania i zmniejszania](expressions.md#postfix-increment-and-decrement-operators) oraz [Operatory przyrostu i zmniejszania prefiksu](expressions.md#prefix-increment-and-decrement-operators).</span><span class="sxs-lookup"><span data-stu-id="6d367-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="6d367-171">Każdy typ wyliczeniowy jest automatycznie pochodzący z klasy `System.Enum` (co z kolei pochodzi od `System.ValueType` i `object`).</span><span class="sxs-lookup"><span data-stu-id="6d367-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="6d367-172">W ten sposób dziedziczone metody i właściwości tej klasy mogą być używane dla wartości typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="6d367-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
