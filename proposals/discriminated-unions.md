---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2019
ms.locfileid: "79485127"
---

# <a name="discriminated-unions--enum-class"></a><span data-ttu-id="7917f-101">Związki rozłączne/`enum class`</span><span class="sxs-lookup"><span data-stu-id="7917f-101">Discriminated unions / `enum class`</span></span>

<span data-ttu-id="7917f-102">`enum class`es są nowym rodzajem deklaracji typu, czasami nazywanymi związkami rozłącznych, gdzie każde możliwe wystąpienie typu jest wymienione, a każde wystąpienie nie nakłada się.</span><span class="sxs-lookup"><span data-stu-id="7917f-102">`enum class`es are a new kind of type declaration, sometimes referred to as discriminated unions, where each every possible instance the type is listed, and each instance is non-overlapping.</span></span>

<span data-ttu-id="7917f-103">`enum class` jest definiowana przy użyciu następującej składni:</span><span class="sxs-lookup"><span data-stu-id="7917f-103">An `enum class` is defined using the following syntax:</span></span>

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

<span data-ttu-id="7917f-104">Przykładowa składnia:</span><span class="sxs-lookup"><span data-stu-id="7917f-104">Sample syntax:</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a><span data-ttu-id="7917f-105">Semantyki</span><span class="sxs-lookup"><span data-stu-id="7917f-105">Semantics</span></span>

<span data-ttu-id="7917f-106">Definicja `enum class` definiuje typ główny, który jest klasą abstrakcyjną o tej samej nazwie co deklaracja `enum class` i zestaw składowych, z których każdy ma typ, który jest podtypem typu głównego.</span><span class="sxs-lookup"><span data-stu-id="7917f-106">An `enum class` definition defines a root type, which is an abstract class of the same name as the `enum class` declaration, and a set of members, each of which has a type which is a subtype of the root type.</span></span> <span data-ttu-id="7917f-107">Jeśli istnieje wiele definicji `partial enum class`, wszystkie elementy członkowskie będą traktowane jako elementy członkowskie definicji klasy wyliczeniowej.</span><span class="sxs-lookup"><span data-stu-id="7917f-107">If there are multiple `partial enum class` definitions, all members will be considered members of the enum class definition.</span></span> <span data-ttu-id="7917f-108">W przeciwieństwie do definicji klasy abstrakcyjnej zdefiniowanej przez użytkownika, `enum class` głównym typem jest domyślnie częściowe i zdefiniowane do posiadania domyślnego *prywatnego* konstruktora bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="7917f-108">Unlike a user-defined abstract class definition, the `enum class` root type is partial by default and defined to have a default *private* parameter-less constructor.</span></span>

<span data-ttu-id="7917f-109">Należy zwrócić uwagę, że ponieważ typ główny jest zdefiniowany jako klasa abstrakcyjna częściowa, można również dodać częściowe definicje *typu głównego* , gdzie są dozwolone formularze składni standardowej dla treści klasy.</span><span class="sxs-lookup"><span data-stu-id="7917f-109">Note that, since the root type is defined to be a partial abstract class, partial definitions of the *root type* may also be added, where standard syntax forms for a class body are allowed.</span></span>
<span data-ttu-id="7917f-110">Jednak żadne typy nie mogą dziedziczyć bezpośrednio od typu głównego w jakiejkolwiek deklaracji, poza tymi określonymi jako elementy członkowskie `enum class`.</span><span class="sxs-lookup"><span data-stu-id="7917f-110">However, no types may directly inherit from the root type in any declaration, aside from those specified as `enum class` members.</span></span> <span data-ttu-id="7917f-111">Ponadto dla typu głównego nie można używać konstruktorów zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7917f-111">In addition, no user-defined constructors are permitted for the root type.</span></span>

<span data-ttu-id="7917f-112">Istnieją cztery rodzaje deklaracji składowej `enum class`:</span><span class="sxs-lookup"><span data-stu-id="7917f-112">There are four kinds of `enum class` member declarations:</span></span>

* <span data-ttu-id="7917f-113">Proste składowe klasy</span><span class="sxs-lookup"><span data-stu-id="7917f-113">simple class members</span></span>

* <span data-ttu-id="7917f-114">złożone składowe klas</span><span class="sxs-lookup"><span data-stu-id="7917f-114">complex class members</span></span>

* <span data-ttu-id="7917f-115">`enum class` członkowie</span><span class="sxs-lookup"><span data-stu-id="7917f-115">`enum class` members</span></span>

* <span data-ttu-id="7917f-116">elementy członkowskie wartości.</span><span class="sxs-lookup"><span data-stu-id="7917f-116">value members.</span></span>

### <a name="simple-class-members"></a><span data-ttu-id="7917f-117">Proste składowe klasy</span><span class="sxs-lookup"><span data-stu-id="7917f-117">Simple class members</span></span>

<span data-ttu-id="7917f-118">Prosta deklaracja składowej klasy definiuje nową, zagnieżdżoną klasę "Records" (celowo niezdefiniowana w tym dokumencie) o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="7917f-118">A simple class member declaration defines a new nested "record" class (intentionally left undefined in this document) with the same name.</span></span> <span data-ttu-id="7917f-119">Klasa zagnieżdżona dziedziczy po typie głównym.</span><span class="sxs-lookup"><span data-stu-id="7917f-119">The nested class inherits from the root type.</span></span>

<span data-ttu-id="7917f-120">Powyższy przykładowy kod,</span><span class="sxs-lookup"><span data-stu-id="7917f-120">Given the sample code above,</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

<span data-ttu-id="7917f-121">Deklaracja `enum class` ma semantykę równoważną następującej deklaracji</span><span class="sxs-lookup"><span data-stu-id="7917f-121">the `enum class` declaration has semantics equivalent to the following declaration</span></span>

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a><span data-ttu-id="7917f-122">Złożone składowe klas</span><span class="sxs-lookup"><span data-stu-id="7917f-122">Complex class members</span></span>

<span data-ttu-id="7917f-123">Można również zagnieżdżać całą deklarację klasy poniżej deklaracji `enum class`.</span><span class="sxs-lookup"><span data-stu-id="7917f-123">You can also nest an entire class declaration below an `enum class` declaration.</span></span> <span data-ttu-id="7917f-124">Będzie ona traktowana jako Klasa zagnieżdżona typu głównego.</span><span class="sxs-lookup"><span data-stu-id="7917f-124">It will be treated as a nested class of the root type.</span></span> <span data-ttu-id="7917f-125">Składnia zezwala dowolnej deklaracji klasy, ale jest wymagana dla składowej klasy złożonej, aby dziedziczyć z bezpośredniej deklaracji `enum class`.</span><span class="sxs-lookup"><span data-stu-id="7917f-125">The syntax allows any class declaration, but it is required for the complex class member to inherit from the direct containing `enum class` declaration.</span></span> 

### <a name="enum-class-members"></a><span data-ttu-id="7917f-126">`enum class` członkowie</span><span class="sxs-lookup"><span data-stu-id="7917f-126">`enum class` members</span></span>

<span data-ttu-id="7917f-127">`enum classes` mogą być zagnieżdżone w innych, np.</span><span class="sxs-lookup"><span data-stu-id="7917f-127">`enum classes` can be nested under each other, e.g.</span></span>

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

<span data-ttu-id="7917f-128">Jest to prawie identyczne z semantyką `enum class`najwyższego poziomu, z tą różnicą, że zagnieżdżona Klasa enum definiuje zagnieżdżony typ główny, a wszystko poniżej zagnieżdżonej klasy enum jest podtypem typu zagnieżdżonego, a nie typu głównego najwyższego poziomu.</span><span class="sxs-lookup"><span data-stu-id="7917f-128">This is almost identical to the semantics of a top-level `enum class`, except that the nested enum class defines a nested root type, and everything below the nested enum class is a subtype of the nested root type, instead of the top-level root type.</span></span>

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a><span data-ttu-id="7917f-129">Elementy członkowskie wartości</span><span class="sxs-lookup"><span data-stu-id="7917f-129">Value members</span></span>

<span data-ttu-id="7917f-130">`enum classes` może również zawierać elementy członkowskie wartości.</span><span class="sxs-lookup"><span data-stu-id="7917f-130">`enum classes` can also contain value members.</span></span> <span data-ttu-id="7917f-131">Elementy członkowskie wartości definiują publiczne właściwości statyczne tylko do pobrania dla typu głównego, które również zwracają typ główny, np.</span><span class="sxs-lookup"><span data-stu-id="7917f-131">Value members define public get-only static properties on the root type that also return the root type, e.g.</span></span>

```C#
enum class Color
{
    Red,
    Green
}
```

<span data-ttu-id="7917f-132">ma właściwości równoważne</span><span class="sxs-lookup"><span data-stu-id="7917f-132">has properties equivalent to</span></span>

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

<span data-ttu-id="7917f-133">Kompletna semantyka jest traktowana jako szczegóły implementacji, ale jest gwarantowane, że jedno unikatowe wystąpienie zostanie zwrócone dla każdej właściwości, a to samo wystąpienie zostanie zwrócone w powtórzonych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="7917f-133">The complete semantics are considered an implementation detail, but it is guaranteed that one unique instance will be returned for each property, and the same instance will be returned on repeated invocations.</span></span>


### <a name="switch-expression-and-patterns"></a><span data-ttu-id="7917f-134">Przełącz wyrażenie i wzorce</span><span class="sxs-lookup"><span data-stu-id="7917f-134">Switch expression and patterns</span></span>

<span data-ttu-id="7917f-135">Istnieją pewne proponowane modyfikacje dopasowania do wzorca oraz wyrażenie przełącznika do obsługi `enum classes`.</span><span class="sxs-lookup"><span data-stu-id="7917f-135">There are some proposed adjustments to pattern matching and the switch expression to handle `enum classes`.</span></span> <span data-ttu-id="7917f-136">Wyrażenia Switch mogą już być zgodne z typami za pomocą wzorca zmiennej, ale obecnie dla typów referencyjnych żaden Zestaw ramion przełączania w wyrażeniu Switch nie jest uważany za kompletny, z wyjątkiem dopasowania do typu statycznego argumentu lub podtypu.</span><span class="sxs-lookup"><span data-stu-id="7917f-136">Switch expressions can already match types through the variable pattern, but for currently for reference types, no set of switch arms in the switch expression are considered complete, except for matching against the static type of the argument, or a subtype.</span></span>

<span data-ttu-id="7917f-137">Wyrażenia Switch zostałyby zmienione tak, aby, jeśli typ główny `enum class` jest typem statycznym argumentu dla wyrażenia Switch, a istnieje zestaw wzorców pasujących do wszystkich elementów członkowskich wyliczenia, przełącznik zostanie uznany za wyczerpujący.</span><span class="sxs-lookup"><span data-stu-id="7917f-137">Switch expressions would be changed such that, if the root type of an `enum class` is the static type of the argument to the switch expression, and there is a set of patterns matching all members of the enum, then the switch will be considered exhaustive.</span></span>

<span data-ttu-id="7917f-138">Ponieważ składowe wartości nie są stałymi i nie definiują nowych typów statycznych, nie można ich dopasować do wzorca.</span><span class="sxs-lookup"><span data-stu-id="7917f-138">Since value members are not constants and do not define new static types, they currently cannot be matched by pattern.</span></span> <span data-ttu-id="7917f-139">Aby to umożliwić, nowy wzorzec przy użyciu składni wzorca stałego zostanie dodany, aby umożliwić dopasowanie względem elementów członkowskich wartości `enum class`.</span><span class="sxs-lookup"><span data-stu-id="7917f-139">To make this possible, a new pattern using the constant pattern syntax will be added to allow match against `enum class` value members.</span></span> <span data-ttu-id="7917f-140">Dopasowanie jest zdefiniowane do sukcesu, jeśli i tylko wtedy, gdy argument dopasowania do wzorca i wartość zwrócona przez składową wartości `enum class` będzie równa równości, chociaż implementacja nie jest wymagana do wykonania tego sprawdzenia.</span><span class="sxs-lookup"><span data-stu-id="7917f-140">The match is defined to succeed if and only if the argument to the pattern match and the value returned by the `enum class` value member would be reference equal, although the implementation is not required to perform this check.</span></span>


## <a name="open-questions"></a><span data-ttu-id="7917f-141">Otwarte pytania</span><span class="sxs-lookup"><span data-stu-id="7917f-141">Open questions</span></span>

- <span data-ttu-id="7917f-142">[] Co robi algorytm Common Type o `enum class` członkowskich?</span><span class="sxs-lookup"><span data-stu-id="7917f-142">[ ] What does the common type algorithm say about `enum class` members?</span></span> <span data-ttu-id="7917f-143">Czy ten prawidłowy kod?</span><span class="sxs-lookup"><span data-stu-id="7917f-143">Is this valid code?</span></span>
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- <span data-ttu-id="7917f-144">[] Dodanie nowego wzorca tylko dla elementów członkowskich wartości wygląda intensywnie.</span><span class="sxs-lookup"><span data-stu-id="7917f-144">[ ] Adding a new pattern just for value members seems heavy handed.</span></span> <span data-ttu-id="7917f-145">Czy istnieje bardziej ogólna konstrukcja wersji, która ma sens?</span><span class="sxs-lookup"><span data-stu-id="7917f-145">Is there a more general version construction that makes sense?</span></span>
    - <span data-ttu-id="7917f-146">[] Elementy członkowskie wartości również nie mapują się również do równoległej konstrukcji klasy zagnieżdżonej ze względu na to</span><span class="sxs-lookup"><span data-stu-id="7917f-146">[ ] Value members also do not map well to a parallel nested class construction because of this</span></span>

- <span data-ttu-id="7917f-147">[] Jest przełączany do argumentu z `enum class` typem statycznym gwarantowanym jako czas stały?</span><span class="sxs-lookup"><span data-stu-id="7917f-147">[ ] Is switching against an argument with an `enum class` static type guaranteed to be constant-time?</span></span>

- <span data-ttu-id="7917f-148">[] Czy istnieje sposób, aby `enum class`es nie były uznawane za kompletne w wyrażeniu Switch?</span><span class="sxs-lookup"><span data-stu-id="7917f-148">[ ] Should there be a way to make `enum class`es not be considered complete in the switch expression?</span></span> <span data-ttu-id="7917f-149">Prefiks z `virtual`?</span><span class="sxs-lookup"><span data-stu-id="7917f-149">Prefix with `virtual`?</span></span>

- <span data-ttu-id="7917f-150">[] Jakie Modyfikatory powinny być dozwolone w `enum class`?</span><span class="sxs-lookup"><span data-stu-id="7917f-150">[ ] What modifiers should be permitted on `enum class`?</span></span>