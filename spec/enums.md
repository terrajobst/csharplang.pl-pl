---
ms.openlocfilehash: c3b716e6eb331be2ee33fffeb42c1e2406cd3a5c
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229981"
---
# <a name="enums"></a><span data-ttu-id="b4eff-101">Wyliczenia</span><span class="sxs-lookup"><span data-stu-id="b4eff-101">Enums</span></span>

<span data-ttu-id="b4eff-102">***Typu wyliczeniowego*** jest typem wartości odrębne ([typy wartości](types.md#value-types)) oświadcza, że zestaw nazwanych stałych.</span><span class="sxs-lookup"><span data-stu-id="b4eff-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="b4eff-103">Przykład</span><span class="sxs-lookup"><span data-stu-id="b4eff-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="b4eff-104">deklaruje typ wyliczeniowy o nazwie `Color` z elementami członkowskimi `Red`, `Green`, i `Blue`.</span><span class="sxs-lookup"><span data-stu-id="b4eff-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="b4eff-105">Deklaracje wyliczeń</span><span class="sxs-lookup"><span data-stu-id="b4eff-105">Enum declarations</span></span>

<span data-ttu-id="b4eff-106">Deklaracja wyliczenia deklaruje nowy typ wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="b4eff-107">Deklaracja wyliczenia zaczyna się od słowa kluczowego `enum`i określa nazwę, ułatwień dostępu, typ podstawowy i elementów członkowskich wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

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

<span data-ttu-id="b4eff-108">Każdy typ wyliczenia ma odpowiedni typ całkowity, o nazwie ***typ bazowy*** typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="b4eff-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="b4eff-109">Ten typ podstawowy musi umożliwiać do reprezentowania wartości modułu wyliczającego zdefiniowane w wyliczeniu.</span><span class="sxs-lookup"><span data-stu-id="b4eff-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="b4eff-110">Deklaracja wyliczenia mogą jawnie deklarować podstawowym typem `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` lub `ulong`.</span><span class="sxs-lookup"><span data-stu-id="b4eff-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="b4eff-111">Należy pamiętać, że `char` nie można używać jako typu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="b4eff-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="b4eff-112">Deklaracja wyliczenia, które jawnie deklaruj typu podstawowego jest podstawowym typem `int`.</span><span class="sxs-lookup"><span data-stu-id="b4eff-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="b4eff-113">Przykład</span><span class="sxs-lookup"><span data-stu-id="b4eff-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="b4eff-114">deklaruje wyliczenie z podstawowym typem `long`.</span><span class="sxs-lookup"><span data-stu-id="b4eff-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="b4eff-115">Deweloper wybrać korzystanie z podstawowym typem `long`, jak w przykładzie, aby umożliwić korzystanie z wartości, które znajdują się w zakresie `long` , ale nie w zakresie `int`, lub zachować tę opcję, aby w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="b4eff-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="b4eff-116">Modyfikatory wyliczenia</span><span class="sxs-lookup"><span data-stu-id="b4eff-116">Enum modifiers</span></span>

<span data-ttu-id="b4eff-117">*Enum_declaration* może opcjonalnie obejmować sekwencję Modyfikatory wyliczenia:</span><span class="sxs-lookup"><span data-stu-id="b4eff-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="b4eff-118">Jest to błąd czasu kompilacji dla tego samego modyfikator pojawią się wiele razy w deklaracji wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="b4eff-119">Modyfikatorów deklaracji wyliczenia mają takie samo znaczenie jak deklaracji klasy ([klasy Modyfikatory](classes.md#class-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="b4eff-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="b4eff-120">Zauważ, że `abstract` i `sealed` modyfikatorów nie są dozwolone w deklaracji wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="b4eff-121">Wyliczenia nie może być abstrakcyjny i nie pozwalają na tworzenie wartości pochodnych.</span><span class="sxs-lookup"><span data-stu-id="b4eff-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="b4eff-122">Elementy członkowskie wyliczenia</span><span class="sxs-lookup"><span data-stu-id="b4eff-122">Enum members</span></span>

<span data-ttu-id="b4eff-123">Treść deklaracji typu wyliczenia definiuje zero lub więcej członków typu wyliczeniowego, które są nazwane stałe typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="b4eff-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="b4eff-124">Nie dwa elementy członkowskie wyliczenia może mieć takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="b4eff-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="b4eff-125">Każdy element członkowski wyliczenia ma skojarzoną wartość stałą.</span><span class="sxs-lookup"><span data-stu-id="b4eff-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="b4eff-126">Typ tej wartości jest podstawowym typem wyliczenia zawierającego.</span><span class="sxs-lookup"><span data-stu-id="b4eff-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="b4eff-127">Stała wartość dla każdego elementu członkowskiego wyliczenia musi być z zakresu od typu podstawowego dla wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="b4eff-128">Przykład</span><span class="sxs-lookup"><span data-stu-id="b4eff-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="b4eff-129">powoduje błąd w czasie kompilacji, ponieważ wartości stałych `-1`, `-2`, i `-3` nie znajdują się w zakresie bazowego typu całkowitego `uint`.</span><span class="sxs-lookup"><span data-stu-id="b4eff-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="b4eff-130">Wiele elementów członkowskich wyliczenia mogą mieć taką samą wartość skojarzona.</span><span class="sxs-lookup"><span data-stu-id="b4eff-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="b4eff-131">Przykład</span><span class="sxs-lookup"><span data-stu-id="b4eff-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="b4eff-132">zawiera wyliczenie, w której dwa elementy członkowskie wyliczenia — `Blue` i `Max` — są takie same skojarzone wartości.</span><span class="sxs-lookup"><span data-stu-id="b4eff-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="b4eff-133">Skojarzona wartość elementu członkowskiego wyliczenia przypisano jawnie lub niejawnie.</span><span class="sxs-lookup"><span data-stu-id="b4eff-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="b4eff-134">Jeśli deklaracja składowej wyliczenia ma *constant_expression* inicjator, wartość tego wyrażenie stałe niejawnie konwertowane na podstawowym typem wyliczenia, jest skojarzona wartość składowej wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="b4eff-135">Jeśli deklaracja składowej wyliczenia nie ma inicjatora, jej powiązaną wartość ustawiono niejawnie, wykonując następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="b4eff-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="b4eff-136">Jeśli element członkowski wyliczenia jest zadeklarowana w typie wyliczeniowym pierwszego elementu członkowskiego wyliczenia, jej powiązaną wartość wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="b4eff-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="b4eff-137">W przeciwnym razie skojarzona wartość składowej wyliczenia są uzyskiwane przez zwiększenie skojarzona wartość w formie tekstu poprzedniego elementu członkowskiego wyliczenia o jeden.</span><span class="sxs-lookup"><span data-stu-id="b4eff-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="b4eff-138">Ta zwiększona wartość musi należeć do zakresu wartości, które mogą być reprezentowane przez typ podstawowy, w przeciwnym razie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="b4eff-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="b4eff-139">Przykład</span><span class="sxs-lookup"><span data-stu-id="b4eff-139">The example</span></span>

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

<span data-ttu-id="b4eff-140">Wyświetla nazwy elementów członkowskich wyliczenia i ich skojarzone wartości.</span><span class="sxs-lookup"><span data-stu-id="b4eff-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="b4eff-141">Dane wyjściowe to:</span><span class="sxs-lookup"><span data-stu-id="b4eff-141">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="b4eff-142">z następujących powodów:</span><span class="sxs-lookup"><span data-stu-id="b4eff-142">for the following reasons:</span></span>

*  <span data-ttu-id="b4eff-143">element członkowski wyliczenia `Red` jest automatycznie przypisywana wartość zero (ponieważ nie ma inicjatora i jest pierwszy element członkowski wyliczenia).</span><span class="sxs-lookup"><span data-stu-id="b4eff-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="b4eff-144">element członkowski wyliczenia `Green` jest jawnie podana wartość `10`;</span><span class="sxs-lookup"><span data-stu-id="b4eff-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="b4eff-145">i elementów członkowskich wyliczenia `Blue` jest automatycznie przypisywana wartość większa o jeden od elementu członkowskiego, przechwyceniem formie tekstu.</span><span class="sxs-lookup"><span data-stu-id="b4eff-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="b4eff-146">Skojarzona wartość elementu członkowskiego wyliczenia nie mogą bezpośrednio lub pośrednio, należy użyć wartości członków skojarzonych wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="b4eff-147">Inne niż to ograniczenie zależność cykliczną inicjatorów składowych typu wyliczeniowego swobodnie mogą odwoływać się do innych inicjatorów składowych typu wyliczeniowego, niezależnie od ich pozycji tekstową.</span><span class="sxs-lookup"><span data-stu-id="b4eff-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="b4eff-148">W obrębie inicjatora składowej wyliczenia wartości innych elementów członkowskich wyliczenia są zawsze traktowane jako mające typ ich typu podstawowego, aby rzutowania są niezbędne, przy odwoływaniu się do innych elementów członkowskich wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="b4eff-149">Przykład</span><span class="sxs-lookup"><span data-stu-id="b4eff-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="b4eff-150">powoduje błąd w czasie kompilacji, ponieważ deklaracje `A` i `B` są cykliczne.</span><span class="sxs-lookup"><span data-stu-id="b4eff-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="b4eff-151">`A` zależy od `B` jawnie, i `B` zależy od `A` niejawnie.</span><span class="sxs-lookup"><span data-stu-id="b4eff-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="b4eff-152">Elementy członkowskie wyliczenia są o nazwie i o określonym zakresie w sposób dokładnie analogiczny do pól w klasach.</span><span class="sxs-lookup"><span data-stu-id="b4eff-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="b4eff-153">Zakres elementu członkowskiego wyliczenia jest zbiorem jej typ zawierający wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="b4eff-154">W tym zakresie typu wyliczeniowego może odnosić się według nazwy proste.</span><span class="sxs-lookup"><span data-stu-id="b4eff-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="b4eff-155">Od innego kodu nazwa elementu członkowskiego wyliczenia musi być kwalifikowana nazwą typu wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="b4eff-156">Elementy członkowskie wyliczenia nie mają żadnych deklarowana dostępność metody — element członkowski wyliczenia jest dostępny, jeśli jego zawierający typ wyliczenia jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="b4eff-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="b4eff-157">Typ System.Enum</span><span class="sxs-lookup"><span data-stu-id="b4eff-157">The System.Enum type</span></span>

<span data-ttu-id="b4eff-158">Typ `System.Enum` jest abstrakcyjna klasa bazowa wszystkich typów wyliczenia (jest to odrębne i różni się od typ podstawowy elementu typu wyliczeniowego) i elementy członkowskie są dziedziczone z `System.Enum` są dostępne w żadnych typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="b4eff-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="b4eff-159">Konwersja boxing ([konwersje Boxing](types.md#boxing-conversions)) istnieje z dowolnego typu enum do `System.Enum`i konwersji rozpakowującej ([konwersje rozpakowywania](types.md#unboxing-conversions)) istnieje z `System.Enum` do dowolnego typu wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="b4eff-160">Należy pamiętać, że `System.Enum` sama nie jest *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="b4eff-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="b4eff-161">Jest to raczej *class_type* z wszystkie *enum_type*pochodzą s.</span><span class="sxs-lookup"><span data-stu-id="b4eff-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="b4eff-162">Typ `System.Enum` dziedziczy z typu `System.ValueType` ([typu System.ValueType](types.md#the-systemvaluetype-type)), która z kolei dziedziczy z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="b4eff-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="b4eff-163">W czasie wykonywania, wartości typu `System.Enum` może być `null` lub odwołanie do wartości spakowanej dowolnego typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="b4eff-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="b4eff-164">Wartości wyliczenia i operacje</span><span class="sxs-lookup"><span data-stu-id="b4eff-164">Enum values and operations</span></span>

<span data-ttu-id="b4eff-165">Każdy typ wyliczenia definiuje typ samodzielny; Konwersja jawna wyliczenia ([Konwersje jawne wyliczenie](conversions.md#explicit-enumeration-conversions)) jest wymagana do konwertowania z zakresu od typu wyliczeniowego do typu całkowitego lub między dwoma typami wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="b4eff-166">Zbiór wartości, które mogą przejąć typ wyliczeniowy nie jest ograniczona przez jej elementów członkowskich wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="b4eff-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="b4eff-167">W szczególności dowolną wartość w podstawowym typem wyliczenia mogą być rzutowane na typ wyliczeniowy i jest różne prawidłową wartością tego typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="b4eff-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="b4eff-168">Elementy członkowskie wyliczenia mają typ ich typem zawierającym wyliczenia (z wyjątkiem w ramach innych inicjatorów składowych typu wyliczeniowego: zobacz [typu wyliczeniowego](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="b4eff-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="b4eff-169">Wartość elementu członkowskiego wyliczenia zostało zadeklarowane w typie wyliczenia `E` o wartość skojarzonego `v` jest `(E)v`.</span><span class="sxs-lookup"><span data-stu-id="b4eff-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="b4eff-170">Można używać następujących operatorów na wartości typu wyliczenia: `==`, `!=`, `<`, `>`, `<=`, `>=`  ([operatory porównania wyliczenie](expressions.md#enumeration-comparison-operators)) , binarne `+`  ([operator dodawania](expressions.md#addition-operator)), binarny `-`  ([operator odejmowania](expressions.md#subtraction-operator)), `^`, `&` , `|`  ([Operatorów logicznych wyliczenie](expressions.md#enumeration-logical-operators)), `~`  ([operator dopełnienia bitowego](expressions.md#bitwise-complement-operator)), `++` i `--`  ([Przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators) i [przedrostkowe i operatory dekrementacji](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="b4eff-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="b4eff-171">Każdy typ wyliczenia automatycznie dziedziczy z klasy `System.Enum` (który z kolei pochodzi od klasy `System.ValueType` i `object`).</span><span class="sxs-lookup"><span data-stu-id="b4eff-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="b4eff-172">W efekcie dziedziczonej metody i właściwości tej klasy można na wartościach typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="b4eff-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
