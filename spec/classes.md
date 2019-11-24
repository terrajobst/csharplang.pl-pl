---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703985"
---
# <a name="classes"></a><span data-ttu-id="cec0a-101">Klasy</span><span class="sxs-lookup"><span data-stu-id="cec0a-101">Classes</span></span>

<span data-ttu-id="cec0a-102">Klasa jest strukturą danych, która może zawierać składowe danych (stałe i pola), składowe funkcji (metody, właściwości, zdarzenia, indeksatory, operatory, konstruktory wystąpień, destruktory i konstruktory statyczne) i typy zagnieżdżone.</span><span class="sxs-lookup"><span data-stu-id="cec0a-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="cec0a-103">Typy klas obsługują dziedziczenie, mechanizm, za pomocą którego Klasa pochodna może zwiększyć i specjalizację klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="cec0a-104">Deklaracje klas</span><span class="sxs-lookup"><span data-stu-id="cec0a-104">Class declarations</span></span>

<span data-ttu-id="cec0a-105">*Class_declaration* jest *type_declaration* ([deklaracje typu](namespaces.md#type-declarations)), które deklaruje nową klasę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="cec0a-106">*Class_declaration* składa się z opcjonalnego zestawu *atrybutów* ([atrybutów](attributes.md)), po którym następuje opcjonalny zestaw *class_modifier*s ([Modyfikatory klas](classes.md#class-modifiers)), po którym następuje opcjonalny modyfikator `partial`, po którym następuje `class` słowa kluczowego i *Identyfikator* , który nadaje nazwę klasy, a po nim opcjonalne *type_parameter_list* ([parametry typu](classes.md#type-parameters)), a po nim opcjonalne specyfikacje *class_base* ([Specyfikacja bazowa klasy](classes.md#class-base-specification)), po których następuje przez opcjonalny zestaw *type_parameter_constraints_clause*s ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), po którym następuje *class_body* ([Treść klasy](classes.md#class-body)), opcjonalnie następuje średnik.</span><span class="sxs-lookup"><span data-stu-id="cec0a-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="cec0a-107">Deklaracja klasy nie może podawać *type_parameter_constraints_clause*s, chyba że zawiera również *type_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="cec0a-108">Deklaracja klasy dostarczająca *type_parameter_list* jest ***deklaracją klasy ogólnej***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="cec0a-109">Ponadto każda klasa zagnieżdżona wewnątrz deklaracji klasy generycznej lub ogólnej deklaracji struktury jest sama deklaracją klasy ogólnej, ponieważ parametry typu dla typu zawierającego muszą zostać dostarczone, aby utworzyć typ skonstruowany.</span><span class="sxs-lookup"><span data-stu-id="cec0a-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="cec0a-110">Modyfikatory klas</span><span class="sxs-lookup"><span data-stu-id="cec0a-110">Class modifiers</span></span>

<span data-ttu-id="cec0a-111">*Class_declaration* może opcjonalnie zawierać sekwencję modyfikatorów klasy:</span><span class="sxs-lookup"><span data-stu-id="cec0a-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

<span data-ttu-id="cec0a-112">Jest to błąd czasu kompilacji dla tego samego modyfikatora do wyświetlenia wiele razy w deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="cec0a-113">Modyfikator `new` jest dozwolony dla zagnieżdżonych klas.</span><span class="sxs-lookup"><span data-stu-id="cec0a-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="cec0a-114">Określa, że Klasa ukrywa dziedziczonego elementu członkowskiego o tej samej nazwie, zgodnie z opisem w [nowym modyfikatorze](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="cec0a-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="cec0a-115">Jest to błąd czasu kompilacji dla modyfikatora `new`, który ma być wyświetlany w deklaracji klasy, która nie jest deklaracją klasy zagnieżdżonej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="cec0a-116">Modyfikatory `public`, `protected`, `internal`i `private` kontrolują ułatwienia dostępu dla klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="cec0a-117">W zależności od kontekstu, w którym występuje deklaracja klasy, niektóre z tych modyfikatorów mogą nie być dozwolone ([zadeklarowane ułatwienia dostępu](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="cec0a-118">Modyfikatory `abstract`, `sealed` i `static` zostały omówione w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="cec0a-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="cec0a-119">Klasy abstrakcyjne</span><span class="sxs-lookup"><span data-stu-id="cec0a-119">Abstract classes</span></span>

<span data-ttu-id="cec0a-120">Modyfikator `abstract` jest używany do wskazania, że Klasa jest niekompletna i że jest przeznaczona do użycia tylko jako klasa bazowa.</span><span class="sxs-lookup"><span data-stu-id="cec0a-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="cec0a-121">Klasa abstrakcyjna różni się od klasy nieabstrakcyjnej w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cec0a-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="cec0a-122">Nie można bezpośrednio utworzyć wystąpienia klasy abstrakcyjnej i jest to błąd czasu kompilacji, aby użyć operatora `new` w klasie abstrakcyjnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="cec0a-123">Chociaż możliwe jest posiadanie zmiennych i wartości, których typy czasu kompilacji są abstrakcyjne, takie zmienne i wartości muszą być `null` lub zawierają odwołania do wystąpień klas nieabstrakcyjnych pochodzących od typów abstrakcyjnych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="cec0a-124">Klasa abstrakcyjna jest dozwolona (ale nie jest wymagana), aby zawierała abstrakcyjne elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="cec0a-125">Klasa abstrakcyjna nie może być zapieczętowana.</span><span class="sxs-lookup"><span data-stu-id="cec0a-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="cec0a-126">Gdy Klasa nieabstrakcyjna jest pochodną klasy abstrakcyjnej, Klasa nieabstrakcyjna musi zawierać rzeczywiste implementacje wszystkich dziedziczonych abstrakcyjnych elementów członkowskich, co zastępuje te abstrakcyjne elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="cec0a-127">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-127">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
<span data-ttu-id="cec0a-128">Klasa abstrakcyjna `A` wprowadza metodę abstrakcyjną `F`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="cec0a-129">Klasa `B` wprowadza dodatkową metodę `G`, ale ponieważ nie zapewnia implementacji `F`, `B` musi być zadeklarowana jako abstract.</span><span class="sxs-lookup"><span data-stu-id="cec0a-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="cec0a-130">Klasa `C` przesłania `F` i zapewnia rzeczywistą implementację.</span><span class="sxs-lookup"><span data-stu-id="cec0a-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="cec0a-131">Ponieważ w `C`nie ma abstrakcyjnych elementów członkowskich, `C` jest dozwolony (ale nie jest wymagany) jako nieabstrakcyjny.</span><span class="sxs-lookup"><span data-stu-id="cec0a-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="cec0a-132">Zapieczętowane klasy</span><span class="sxs-lookup"><span data-stu-id="cec0a-132">Sealed classes</span></span>

<span data-ttu-id="cec0a-133">Modyfikator `sealed` służy do zapobiegania wyprowadzaniu z klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="cec0a-134">Błąd czasu kompilacji występuje, jeśli Klasa zapieczętowana została określona jako klasa bazowa innej klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="cec0a-135">Klasa zapieczętowana nie może być również klasą abstrakcyjną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="cec0a-136">Modyfikator `sealed` jest używany głównie do zapobiegania niezamierzonemu wyznaczeniu, ale również zapewnia pewne optymalizacje w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="cec0a-137">W szczególności ze względu na to, że Klasa zapieczętowana nie ma żadnych klas pochodnych, istnieje możliwość przekształcenia wywołań elementów członkowskich funkcji wirtualnych w wystąpieniach klasy zapieczętowanej na wywołania niewirtualne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="cec0a-138">Klasy statyczne</span><span class="sxs-lookup"><span data-stu-id="cec0a-138">Static classes</span></span>

<span data-ttu-id="cec0a-139">Modyfikator `static` służy do oznaczania klasy zadeklarowanej jako ***Klasa statyczna***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="cec0a-140">Nie można utworzyć wystąpienia klasy statycznej, nie można jej użyć jako typu i może zawierać tylko statyczne elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="cec0a-141">Tylko Klasa statyczna może zawierać deklaracje metod rozszerzających ([metody rozszerzające](classes.md#extension-methods)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="cec0a-142">Deklaracja klasy statycznej podlega następującym ograniczeniom:</span><span class="sxs-lookup"><span data-stu-id="cec0a-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="cec0a-143">Klasa statyczna nie może zawierać modyfikatora `sealed` ani `abstract`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="cec0a-144">Należy jednak pamiętać, że ponieważ od klasy statycznej nie można utworzyć wystąpienia ani pochodnej, zachowuje się tak, jakby było zapieczętowane i abstrakcyjne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="cec0a-145">Klasa statyczna nie może zawierać specyfikacji *class_base* ([specyfikacji klasy podstawowej](classes.md#class-base-specification)) i nie może jawnie określić klasy bazowej lub listy zaimplementowanych interfejsów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="cec0a-146">Klasa statyczna niejawnie dziedziczy po typie `object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="cec0a-147">Klasa statyczna może zawierać tylko statyczne elementy członkowskie ([statyczne i składowe wystąpienia](classes.md#static-and-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="cec0a-148">Należy zauważyć, że stałe i zagnieżdżone typy są klasyfikowane jako statyczne elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="cec0a-149">Klasa statyczna nie może mieć elementów członkowskich z zazadeklarowaną dostępnością `protected` lub `protected internal`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="cec0a-150">Jest to błąd czasu kompilacji, który narusza którekolwiek z tych ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="cec0a-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="cec0a-151">Klasa statyczna nie ma konstruktorów wystąpień.</span><span class="sxs-lookup"><span data-stu-id="cec0a-151">A static class has no instance constructors.</span></span> <span data-ttu-id="cec0a-152">Nie można zadeklarować konstruktora wystąpienia w klasie statycznej i nie podano domyślnego konstruktora wystąpień ([konstruktorów domyślnych](classes.md#default-constructors)) dla klasy statycznej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="cec0a-153">Elementy członkowskie klasy statycznej nie są automatycznie statyczne i deklaracje składowych muszą jawnie zawierać modyfikator `static` (z wyjątkiem stałych i zagnieżdżonych typów).</span><span class="sxs-lookup"><span data-stu-id="cec0a-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="cec0a-154">Gdy Klasa jest zagnieżdżona w obrębie statycznej klasy zewnętrznej, Klasa zagnieżdżona nie jest klasą statyczną, chyba że jawnie zawiera modyfikator `static`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="cec0a-155">__Odwołujące się do typów klas statycznych__</span><span class="sxs-lookup"><span data-stu-id="cec0a-155">__Referencing static class types__</span></span>

<span data-ttu-id="cec0a-156">*Namespace_or_type_name* ([przestrzeń nazw i nazwy typów](basic-concepts.md#namespace-and-type-names)) mogą odwoływać się do klasy statycznej, jeśli</span><span class="sxs-lookup"><span data-stu-id="cec0a-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="cec0a-157">*Namespace_or_type_name* jest `T` w *namespace_or_type_name* `T.I`formularz, lub</span><span class="sxs-lookup"><span data-stu-id="cec0a-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="cec0a-158">*Namespace_or_type_name* jest `T` w *typeof_expression* ([Argument Lists](expressions.md#argument-lists)1) `typeof(T)`formularza.</span><span class="sxs-lookup"><span data-stu-id="cec0a-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="cec0a-159">*Primary_expression* ([Członkowie funkcji](expressions.md#function-members)) mogą odwoływać się do klasy statycznej, jeśli</span><span class="sxs-lookup"><span data-stu-id="cec0a-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="cec0a-160">*Primary_expression* jest `E` w *member_access* ([Sprawdzanie czasu kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) `E.I`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="cec0a-161">W każdym innym kontekście jest to błąd czasu kompilacji, który odwołuje się do klasy statycznej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="cec0a-162">Na przykład jest to błąd dla klasy statycznej, która ma być używana jako klasa bazowa, typ składnika ([typy zagnieżdżone](classes.md#nested-types)) elementu członkowskiego, argument typu ogólnego lub ograniczenie parametru typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="cec0a-163">Podobnie Klasa statyczna nie może być używana w typie tablicy, typ wskaźnika, wyrażenie `new`, wyrażenie rzutowania, wyrażenie `is`, wyrażenie `as`, wyrażenie `sizeof` lub wyrażenie wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="cec0a-164">Modyfikator częściowy</span><span class="sxs-lookup"><span data-stu-id="cec0a-164">Partial modifier</span></span>

<span data-ttu-id="cec0a-165">Modyfikator `partial` jest używany do wskazania, że ta *class_declaration* jest deklaracją typu częściowego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="cec0a-166">Wiele deklaracji typu częściowego o tej samej nazwie w otaczającej przestrzeni nazw lub deklaracji typu Połącz, aby utworzyć jedną deklarację typu, zgodnie z regułami określonymi w [typach częściowych](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="cec0a-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="cec0a-167">Posiadanie deklaracji klasy rozproszonej za pomocą oddzielnych segmentów tekstu programu może być przydatne, jeśli te segmenty są produkowane lub utrzymywane w różnych kontekstach.</span><span class="sxs-lookup"><span data-stu-id="cec0a-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="cec0a-168">Na przykład jedna część deklaracji klasy może być wygenerowana maszyną, podczas gdy druga zostaje ręcznie utworzona.</span><span class="sxs-lookup"><span data-stu-id="cec0a-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="cec0a-169">Oddzielenie tekstu między nimi uniemożliwia aktualizację przez jeden z powodu konfliktu z aktualizacjami.</span><span class="sxs-lookup"><span data-stu-id="cec0a-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="cec0a-170">Parametry typu</span><span class="sxs-lookup"><span data-stu-id="cec0a-170">Type parameters</span></span>

<span data-ttu-id="cec0a-171">Parametr typu jest prostym identyfikatorem, który wskazuje symbol zastępczy argumentu typu dostarczonego do utworzenia konstruowanego typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="cec0a-172">Parametr typu jest formalnym symbolem zastępczym dla typu, który zostanie dostarczony później.</span><span class="sxs-lookup"><span data-stu-id="cec0a-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="cec0a-173">Z kolei argument typu ([argumenty typu](types.md#type-arguments)) jest rzeczywistym typem, który jest zastępowany dla parametru typu podczas tworzenia konstruowanego typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

<span data-ttu-id="cec0a-174">Każdy parametr typu w deklaracji klasy definiuje nazwę w obszarze deklaracji ([deklaracji](basic-concepts.md#declarations)) tej klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="cec0a-175">Dlatego nie może mieć takiej samej nazwy jak inny parametr typu lub element członkowski zadeklarowany w tej klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="cec0a-176">Parametr typu nie może mieć takiej samej nazwy jak sam typ.</span><span class="sxs-lookup"><span data-stu-id="cec0a-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="cec0a-177">Specyfikacja bazowa klasy</span><span class="sxs-lookup"><span data-stu-id="cec0a-177">Class base specification</span></span>

<span data-ttu-id="cec0a-178">Deklaracja klasy może zawierać specyfikację *class_base* , która definiuje bezpośrednią klasę bazową klasy i interfejsy ([interfejsy](interfaces.md)) bezpośrednio zaimplementowane przez klasę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

<span data-ttu-id="cec0a-179">Klasa bazowa określona w deklaracji klasy może być skonstruowanym typem klasy ([skonstruowane typy](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="cec0a-180">Klasa bazowa nie może być parametrem typu, ale może zawierać parametry typu, które znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="cec0a-181">Klas podstawowych</span><span class="sxs-lookup"><span data-stu-id="cec0a-181">Base classes</span></span>

<span data-ttu-id="cec0a-182">Gdy *class_type* jest uwzględniona w *class_base*, określa bezpośrednią klasę bazową zadeklarowanej klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="cec0a-183">Jeśli deklaracja klasy nie ma *class_base*lub *class_base* wyświetla tylko typy interfejsów, przyjmuje się, że bezpośrednia klasa bazowa jest `object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="cec0a-184">Klasa dziedziczy składowe z bezpośredniej klasy podstawowej, zgodnie z opisem w [dziedziczeniu](classes.md#inheritance).</span><span class="sxs-lookup"><span data-stu-id="cec0a-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="cec0a-185">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="cec0a-186">Klasa `A` ma być bezpośrednią klasą bazową `B`, a `B` jest określana jako pochodna z `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="cec0a-187">Ponieważ `A` nie określa jawnie bezpośredniej klasy bazowej, jej bezpośrednia klasa bazowa jest niejawnie `object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="cec0a-188">W przypadku konstruowanego typu klasy, jeśli klasa bazowa jest określona w deklaracji klasy generycznej, Klasa bazowa typu konstruowanego jest uzyskiwana przez podstawianie dla każdego *type_parameter* w deklaracji klasy bazowej, odpowiadającego *type_argument* typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="cec0a-189">Podaną deklaracje klas ogólnych</span><span class="sxs-lookup"><span data-stu-id="cec0a-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="cec0a-190">Klasa bazowa złożonego typu `G<int>` zostałaby `B<string,int[]>`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="cec0a-191">Bezpośrednia klasa bazowa typu klasy musi być co najmniej równa dostępności jako samego typu klasy ([domeny dostępności](basic-concepts.md#accessibility-domains)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="cec0a-192">Na przykład jest to błąd czasu kompilowania dla klasy `public`, która dziedziczy z klasy `private` lub `internal`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="cec0a-193">Bezpośrednia klasa bazowa typu klasy nie może być żadnym z następujących typów: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`ani `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="cec0a-194">Ponadto deklaracja klasy generycznej nie może używać `System.Attribute` jako bezpośredniej lub pośredniej klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="cec0a-195">Podczas określania znaczenia bezpośredniej specyfikacji klasy bazowej `A` klasy `B`, przyjmuje się, że bezpośrednia klasa bazowa `B` jest tymczasowo `object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="cec0a-196">Intuicyjnie gwarantuje to, że znaczenie specyfikacji klasy bazowej nie może rekursywnie zależeć od siebie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="cec0a-197">Przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="cec0a-198">Wystąpił błąd, ponieważ w specyfikacji klasy bazowej `A<C.B>` bezpośrednia klasa bazowa `C` jest uznawana za `object`, a tym samym (zgodnie z regułami [przestrzeni nazw i nazw typów](basic-concepts.md#namespace-and-type-names)) `C` nie jest uważana za element członkowski `B`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="cec0a-199">Klasy bazowe typu klasy są bezpośrednią klasą bazową i jej klasami podstawowymi.</span><span class="sxs-lookup"><span data-stu-id="cec0a-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="cec0a-200">Innymi słowy, zestaw klas bazowych jest przechodnim zamknięciem bezpośredniej relacji między klasami podstawowymi.</span><span class="sxs-lookup"><span data-stu-id="cec0a-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="cec0a-201">W odniesieniu do powyższego przykładu klasy bazowe `B` są `A` i `object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="cec0a-202">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="cec0a-203">klasy bazowe `D<int>` są `C<int[]>`, `B<IComparable<int[]>>`, `A`i `object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="cec0a-204">Oprócz klasy `object`każdy typ klasy ma dokładnie jedną bezpośrednią klasę bazową.</span><span class="sxs-lookup"><span data-stu-id="cec0a-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="cec0a-205">Klasa `object` nie ma bezpośredniej klasy podstawowej i jest ostateczną klasą bazową wszystkich innych klas.</span><span class="sxs-lookup"><span data-stu-id="cec0a-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="cec0a-206">Gdy Klasa `B` dziedziczy z klasy `A`, jest to błąd czasu kompilacji dla `A` zależnie od `B`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="cec0a-207">Klasa ***bezpośrednio zależy*** od jej bezpośredniej klasy podstawowej (jeśli istnieje) i ***bezpośrednio zależy*** od klasy, w której jest od razu zagnieżdżona (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="cec0a-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="cec0a-208">W związku z tą definicją kompletny zestaw klas, od których zależy Klasa, jest bezpośrednie i przechodnie zamknięcie ***bezpośredniego zależy*** od relacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="cec0a-209">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="cec0a-210">jest błędem, ponieważ Klasa zależy od siebie samej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="cec0a-211">Podobnie, przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="cec0a-212">Wystąpił błąd, ponieważ klasy cyklicznie zależą od siebie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="cec0a-213">Na koniec przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="cec0a-214">powoduje błąd czasu kompilacji, ponieważ `A` zależy od `B.C` (jego bezpośrednia klasa bazowa), która zależy od `B` (bezpośrednio otaczającej klasy), która cyklicznie zależy od `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="cec0a-215">Należy zauważyć, że Klasa nie zależy od klas, które są w niej zagnieżdżone.</span><span class="sxs-lookup"><span data-stu-id="cec0a-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="cec0a-216">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="cec0a-217">`B` zależy od `A` (ponieważ `A` jest zarówno bezpośrednią klasą bazową, jak i bezpośrednio otaczającą ją klasą), ale `A` nie zależy od `B` (ponieważ `B` nie jest klasą bazową ani otaczającą klasę `A`).</span><span class="sxs-lookup"><span data-stu-id="cec0a-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="cec0a-218">W tym przypadku przykład jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-218">Thus, the example is valid.</span></span>

<span data-ttu-id="cec0a-219">Nie można dziedziczyć z klasy `sealed`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="cec0a-220">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="cec0a-221">Wystąpił błąd klasy `B`, ponieważ próbuje dziedziczyć z klasy `sealed` `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="cec0a-222">Implementacje interfejsu</span><span class="sxs-lookup"><span data-stu-id="cec0a-222">Interface implementations</span></span>

<span data-ttu-id="cec0a-223">Specyfikacja *class_base* może zawierać listę typów interfejsów, w takim przypadku Klasa jest określana do bezpośredniego zaimplementowania danego typu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="cec0a-224">Implementacje interfejsów omówiono dokładniej w [implementacjach interfejsu](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="cec0a-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="cec0a-225">Ograniczenia parametru typu</span><span class="sxs-lookup"><span data-stu-id="cec0a-225">Type parameter constraints</span></span>

<span data-ttu-id="cec0a-226">Deklaracje typu ogólnego i metody mogą opcjonalnie określać ograniczenia parametrów typu przez uwzględnienie *type_parameter_constraints_clause*s.</span><span class="sxs-lookup"><span data-stu-id="cec0a-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

<span data-ttu-id="cec0a-227">Każdy *type_parameter_constraints_clause* składa się z `where`tokena, po którym następuje nazwa parametru typu, po którym następuje dwukropek i lista ograniczeń dla tego parametru typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="cec0a-228">Dla każdego parametru typu może istnieć co najwyżej jedna klauzula `where`, a klauzule `where` mogą być wyświetlane w dowolnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="cec0a-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="cec0a-229">Podobnie jak tokeny `get` i `set` w metodzie dostępu do właściwości, token `where` nie jest słowem kluczowym.</span><span class="sxs-lookup"><span data-stu-id="cec0a-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="cec0a-230">Lista ograniczeń podanych w klauzuli `where` może zawierać dowolny z następujących składników, w następującej kolejności: jedno ograniczenie podstawowe, jedno lub więcej ograniczeń pomocniczych i ograniczenie konstruktora, `new()`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="cec0a-231">Ograniczenie podstawowe może być typem klasy lub ***ograniczeniem typu referencyjnego*** `class` lub `struct`***ograniczenia typu wartości*** .</span><span class="sxs-lookup"><span data-stu-id="cec0a-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="cec0a-232">Ograniczenie pomocnicze może być *type_parameter* lub *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="cec0a-233">Ograniczenie typu odwołania określa, że argument typu użyty dla parametru typu musi być typem referencyjnym.</span><span class="sxs-lookup"><span data-stu-id="cec0a-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="cec0a-234">Wszystkie typy klas, typy interfejsów, typy delegatów, typy tablic i parametry typu znane jako typ referencyjny (zgodnie z definicją poniżej) spełniają to ograniczenie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="cec0a-235">Ograniczenie typu wartości określa, że argument typu użyty dla parametru typu musi być typem wartości niedopuszczający wartości null.</span><span class="sxs-lookup"><span data-stu-id="cec0a-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="cec0a-236">Wszystkie typy struktur niedopuszczające wartości null, typy wyliczeniowe i parametry typu mające ograniczenie typu wartości spełniają to ograniczenie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="cec0a-237">Należy zauważyć, że chociaż sklasyfikowane jako typ wartości, typ dopuszczający wartość null ([Typy dopuszczające wartości null](types.md#nullable-types)) nie spełnia ograniczenia typu wartości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="cec0a-238">Parametr typu z ograniczeniem typu wartości nie może mieć również *constructor_constraint*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="cec0a-239">Typy wskaźników nigdy nie mogą być argumentami typu i nie są uważane za spełniające ograniczenia typu odwołania lub typu wartości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="cec0a-240">Jeśli ograniczenie jest typem klasy, typem interfejsu lub parametrem typu, ten typ określa minimalny "typ podstawowy", który każdy argument typu użyty dla tego parametru typu musi obsługiwać.</span><span class="sxs-lookup"><span data-stu-id="cec0a-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="cec0a-241">Za każdym razem, gdy jest używany typ skonstruowany lub metoda ogólna, argument typu jest sprawdzany względem ograniczeń w parametrze typu w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="cec0a-242">Dostarczony argument typu musi spełniać warunki opisane w temacie [spełnianie ograniczeń](types.md#satisfying-constraints).</span><span class="sxs-lookup"><span data-stu-id="cec0a-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="cec0a-243">Ograniczenie *class_type* musi spełniać następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="cec0a-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="cec0a-244">Typ musi być typem klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-244">The type must be a class type.</span></span>
*  <span data-ttu-id="cec0a-245">Typ nie może być `sealed`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="cec0a-246">Typ nie może być jednym z następujących typów: `System.Array`, `System.Delegate`, `System.Enum`lub `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="cec0a-247">Typ nie może być `object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-247">The type must not be `object`.</span></span> <span data-ttu-id="cec0a-248">Ponieważ wszystkie typy pochodzą od `object`, takie ograniczenie nie będzie miało wpływu, jeśli było dozwolone.</span><span class="sxs-lookup"><span data-stu-id="cec0a-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="cec0a-249">Co najwyżej jedno ograniczenie dla danego parametru typu może być typem klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="cec0a-250">Typ określony jako ograniczenie *INTERFACE_TYPE* musi spełniać następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="cec0a-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="cec0a-251">Typ musi być typem interfejsu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="cec0a-252">Typ nie może być określony więcej niż raz w danej klauzuli `where`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="cec0a-253">W obu przypadkach ograniczenie może dotyczyć dowolnego z parametrów typu skojarzonej deklaracji typu lub metody w ramach typu złożonego i może dotyczyć zadeklarowanego typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="cec0a-254">Każdy typ klasy lub interfejsu określony jako ograniczenie parametru typu musi być co najmniej jako dostępny ([ograniczenia dostępu](basic-concepts.md#accessibility-constraints)) jako zadeklarowany typ ogólny lub metoda.</span><span class="sxs-lookup"><span data-stu-id="cec0a-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="cec0a-255">Typ określony jako ograniczenie *type_parameter* musi spełniać następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="cec0a-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="cec0a-256">Typ musi być parametrem typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="cec0a-257">Typ nie może być określony więcej niż raz w danej klauzuli `where`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="cec0a-258">Ponadto nie może istnieć żadne cykle w grafie zależności parametrów typu, gdzie zależność jest relacją przechodnią zdefiniowaną przez:</span><span class="sxs-lookup"><span data-stu-id="cec0a-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="cec0a-259">Jeśli parametr typu `T` jest używany jako ograniczenie dla parametru typu `S`, `S` ***zależy od*** `T`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="cec0a-260">Jeśli parametr typu `S` zależy od parametru typu `T` i `T` zależy od parametru typu `U`, `S` ***zależy od*** `U`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="cec0a-261">Ta relacja jest błędem czasu kompilowania dla parametru typu, aby zależała od samego siebie (bezpośrednio lub pośrednio).</span><span class="sxs-lookup"><span data-stu-id="cec0a-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="cec0a-262">Wszystkie ograniczenia muszą być spójne między zależnymi parametrami typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="cec0a-263">Jeśli parametr typu `S` zależy od parametru typu `T`, wówczas:</span><span class="sxs-lookup"><span data-stu-id="cec0a-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="cec0a-264">`T` nie może mieć ograniczenia typu wartości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="cec0a-265">W przeciwnym razie `T` jest skutecznie zapieczętowany, dlatego `S` zostanie wymuszone tego samego typu co `T`, eliminując konieczność stosowania dwóch parametrów typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="cec0a-266">Jeśli `S` ma ograniczenie typu wartości, `T` nie może mieć ograniczenia *class_type* .</span><span class="sxs-lookup"><span data-stu-id="cec0a-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="cec0a-267">Jeśli `S` ma ograniczenie *class_type* `A` i `T` ma ograniczenie *class_type* `B`, należy wykonać konwersję tożsamości lub niejawną konwersję odwołań z `A` na `B` lub niejawną konwersję odwołań z `B` do `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="cec0a-268">Jeśli `S` również zależy od parametru typu `U`, a `U` ma ograniczenie *class_type* `A` i `T` ma ograniczenie *class_type* `B`, należy przeprowadzić konwersję tożsamości lub niejawną konwersję odwołań z `A` na `B` lub niejawną konwersję odwołania z `B` do `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="cec0a-269">Jest on prawidłowy dla `S`, aby miał ograniczenie typu wartości i `T` mieć ograniczenia typu odwołania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="cec0a-270">Efektywnie te limity `T` do typów `System.Object`, `System.ValueType`, `System.Enum`i dowolnego typu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="cec0a-271">Jeśli klauzula `where` parametru typu zawiera ograniczenie konstruktora (które ma postać `new()`), można użyć operatora `new`, aby utworzyć wystąpienia typu ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="cec0a-272">Dowolny argument typu użyty dla parametru typu z ograniczeniem konstruktora musi mieć publiczny Konstruktor bez parametrów (ten Konstruktor niejawnie istnieje dla dowolnego typu wartości) lub być parametrem typu, który ma ograniczenie typu wartości lub ograniczenie konstruktora (zobacz [ograniczenia parametru typu](classes.md#type-parameter-constraints) , aby uzyskać szczegółowe informacje).</span><span class="sxs-lookup"><span data-stu-id="cec0a-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="cec0a-273">Poniżej przedstawiono przykłady ograniczeń:</span><span class="sxs-lookup"><span data-stu-id="cec0a-273">The following are examples of constraints:</span></span>
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

<span data-ttu-id="cec0a-274">Poniższy przykład dotyczy błędu, ponieważ powoduje cykliczność w grafie zależności parametrów typu:</span><span class="sxs-lookup"><span data-stu-id="cec0a-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="cec0a-275">W poniższych przykładach przedstawiono dodatkowe nieprawidłowe sytuacje:</span><span class="sxs-lookup"><span data-stu-id="cec0a-275">The following examples illustrate additional invalid situations:</span></span>
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

<span data-ttu-id="cec0a-276">***Obowiązująca Klasa bazowa*** parametru typu `T` jest zdefiniowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cec0a-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="cec0a-277">Jeśli `T` nie ma ograniczeń podstawowych ani ograniczeń parametrów typu, jego skuteczna Klasa bazowa jest `object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="cec0a-278">Jeśli `T` ma ograniczenie typu wartości, jego obowiązująca Klasa bazowa jest `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="cec0a-279">Jeśli `T` ma ograniczenie *class_type* `C` ale nie ma żadnych ograniczeń *type_parameter* , jego obowiązująca Klasa bazowa jest `C`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="cec0a-280">Jeśli `T` nie ma ograniczenia *class_type* , ale ma co najmniej jedno ograniczenie *type_parameter* , jego skuteczna Klasa bazowa to najbardziej z nich typ ([podniesione operatory konwersji](conversions.md#lifted-conversion-operators)) w zestawie skutecznych klas podstawowych ograniczeń *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="cec0a-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="cec0a-281">Reguły spójności zapewniają, że ten typ jest najbardziej odnoszący się do tego typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="cec0a-282">Jeśli `T` ma ograniczenie *class_type* i co najmniej jeden *type_parameter* ograniczenia, jego obowiązująca Klasa bazowa to najbardziej z nich typ ([zniesione operatory konwersji](conversions.md#lifted-conversion-operators)) w zestawie składający się z ograniczenia *class_type* `T` i obowiązujących klas podstawowych jego ograniczeń *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="cec0a-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="cec0a-283">Reguły spójności zapewniają, że ten typ jest najbardziej odnoszący się do tego typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="cec0a-284">Jeśli `T` ma ograniczenie typu odwołania, ale nie ma żadnych ograniczeń *class_type* , jego obowiązująca Klasa bazowa jest `object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="cec0a-285">Na potrzeby tych reguł, jeśli T ma ograniczenie `V`, które jest *value_type*, Użyj zamiast tego najbardziej konkretnego typu podstawowego `V`, który jest *class_type*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="cec0a-286">Może to nigdy potrwać jawnie określone ograniczenie, ale może wystąpić, gdy ograniczenia metody generycznej są niejawnie dziedziczone przez zastępowanie deklaracji metody lub jawną implementację metody interfejsu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="cec0a-287">Te reguły zapewniają, że obowiązująca Klasa bazowa jest zawsze *class_type*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="cec0a-288">***Efektywny zestaw interfejsów*** parametru typu `T` jest zdefiniowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cec0a-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="cec0a-289">Jeśli `T` nie ma *secondary_constraints*, jego skuteczny zestaw interfejsów jest pusty.</span><span class="sxs-lookup"><span data-stu-id="cec0a-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="cec0a-290">Jeśli `T` ma ograniczenia *INTERFACE_TYPE* , ale nie ograniczenia *type_parameter* , jego obowiązujący zestaw interfejsów jest jego zestawem ograniczeń *INTERFACE_TYPE* .</span><span class="sxs-lookup"><span data-stu-id="cec0a-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="cec0a-291">Jeśli `T` nie ma ograniczeń *INTERFACE_TYPE* , ale ma ograniczenia *type_parameter* , jego skuteczny zestaw interfejsów jest złożeniem efektywnych zestawów interfejsów jego ograniczeń *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="cec0a-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="cec0a-292">Jeśli `T` ma zarówno ograniczenia *INTERFACE_TYPE* , jak i ograniczenia *type_parameter* , jego skuteczny zestaw interfejsów jest Unią zestawu ograniczeń *INTERFACE_TYPE* i obowiązującymi zestawami interfejsów jego ograniczeń *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="cec0a-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="cec0a-293">Parametr typu jest ***znany jako typ referencyjny*** , jeśli ma ograniczenie typu odwołania lub jego obowiązująca Klasa bazowa nie jest `object` ani `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="cec0a-294">Wartości typu parametru typu ograniczonego mogą służyć do uzyskiwania dostępu do elementów członkowskich wystąpienia IMPLIKOWANYCH przez ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="cec0a-295">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-295">In the example</span></span>
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
<span data-ttu-id="cec0a-296">Metody `IPrintable` mogą być wywoływane bezpośrednio na `x`, ponieważ `T` jest ograniczone do zawsze implementowania `IPrintable`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="cec0a-297">Treść klasy</span><span class="sxs-lookup"><span data-stu-id="cec0a-297">Class body</span></span>

<span data-ttu-id="cec0a-298">*Class_body* klasy definiuje elementy członkowskie tej klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="cec0a-299">Typy częściowe</span><span class="sxs-lookup"><span data-stu-id="cec0a-299">Partial types</span></span>

<span data-ttu-id="cec0a-300">Deklarację typu można podzielić na wiele ***deklaracji typu częściowego***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="cec0a-301">Deklaracja typu jest zbudowana z jego części, zgodnie z regułami w tej sekcji, whereupon jest traktowany jako jedna deklaracja w trakcie pozostałej części czasu kompilacji i przetwarzania w czasie wykonywania programu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="cec0a-302">*Class_declaration*, *struct_declaration* lub *interface_declaration* reprezentuje deklarację typu częściowego, jeśli zawiera modyfikator `partial`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="cec0a-303">`partial` nie jest słowem kluczowym i działa tylko jako modyfikator, jeśli występuje bezpośrednio przed jednym ze słów kluczowych `class`, `struct` lub `interface` w deklaracji typu lub przed typem `void` w deklaracji metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="cec0a-304">W innych kontekstach można używać ich jako normalnego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="cec0a-305">Każda część deklaracji typu częściowego musi zawierać modyfikator `partial`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="cec0a-306">Musi mieć taką samą nazwę i być zadeklarowane w tej samej przestrzeni nazw lub deklaracji typu co pozostałe części.</span><span class="sxs-lookup"><span data-stu-id="cec0a-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="cec0a-307">Modyfikator `partial` wskazuje, że dodatkowe części deklaracji typu mogą istnieć w innym miejscu, ale istnienie takich dodatkowych części nie jest wymagane; jest ona prawidłowa dla typu z pojedynczą deklaracją, aby uwzględnić modyfikator `partial`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="cec0a-308">Wszystkie części typu częściowego muszą zostać skompilowane w taki sposób, aby części mogły być scalane w czasie kompilacji w jednej deklaracji typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="cec0a-309">Typy częściowe nie zezwalają na rozszerzone już skompilowane typy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="cec0a-310">Zagnieżdżone typy można zadeklarować w wielu częściach przy użyciu modyfikatora `partial`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="cec0a-311">Zazwyczaj typ zawierający jest zadeklarowany przy użyciu `partial`, a każda część typu zagnieżdżonego jest zadeklarowana w innej części zawierającego go typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="cec0a-312">Modyfikator `partial` nie jest dozwolony w deklaracjach delegata ani wyliczeniowych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="cec0a-313">Atrybuty</span><span class="sxs-lookup"><span data-stu-id="cec0a-313">Attributes</span></span>

<span data-ttu-id="cec0a-314">Atrybuty typu częściowego są określane przez połączenie, w nieokreślonej kolejności, atrybutów każdej części.</span><span class="sxs-lookup"><span data-stu-id="cec0a-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="cec0a-315">Jeśli atrybut jest umieszczany w wielu częściach, jest równoważne do określenia atrybutu wielokrotnie w typie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="cec0a-316">Na przykład dwie części:</span><span class="sxs-lookup"><span data-stu-id="cec0a-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="cec0a-317">są równoważne deklaracji, takiej jak:</span><span class="sxs-lookup"><span data-stu-id="cec0a-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="cec0a-318">Atrybuty parametrów typu łączą się w podobny sposób.</span><span class="sxs-lookup"><span data-stu-id="cec0a-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="cec0a-319">Modyfikatory</span><span class="sxs-lookup"><span data-stu-id="cec0a-319">Modifiers</span></span>

<span data-ttu-id="cec0a-320">Gdy deklaracja typu częściowego zawiera specyfikację dostępności (Modyfikatory `public`, `protected`, `internal`i `private`), musi on wyrazić zgodę na wszystkie inne części, które zawierają specyfikację dostępności.</span><span class="sxs-lookup"><span data-stu-id="cec0a-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="cec0a-321">Jeśli żadna część typu częściowego nie zawiera specyfikacji dostępności, typ ma odpowiednią domyślną dostępność ([zadeklarowany dostęp](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="cec0a-322">Jeśli co najmniej jedna deklaracja częściowa typu zagnieżdżonego zawiera modyfikator `new`, żadne ostrzeżenie nie zostanie zgłoszone, jeśli typ zagnieżdżony ukrywa dziedziczonego elementu członkowskiego ([ukrywając przez dziedziczenie](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="cec0a-323">Jeśli co najmniej jedna deklaracja częściowa klasy zawiera modyfikator `abstract`, Klasa jest uznawana za abstrakcyjną ([klasy abstrakcyjne](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="cec0a-324">W przeciwnym razie Klasa jest uznawana za nieabstrakcyjną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="cec0a-325">Jeśli co najmniej jedna deklaracja częściowa klasy zawiera modyfikator `sealed`, Klasa jest uznawana za zapieczętowany ([klasy zapieczętowane](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="cec0a-326">W przeciwnym razie Klasa jest uznawana za niezapieczętowany.</span><span class="sxs-lookup"><span data-stu-id="cec0a-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="cec0a-327">Należy zauważyć, że Klasa nie może być abstrakcyjna i zapieczętowana.</span><span class="sxs-lookup"><span data-stu-id="cec0a-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="cec0a-328">Gdy modyfikator `unsafe` jest używany w deklaracji typu częściowego, tylko ta część jest traktowana jako niebezpieczny kontekst ([niebezpieczne konteksty](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="cec0a-329">Parametry i ograniczenia typu</span><span class="sxs-lookup"><span data-stu-id="cec0a-329">Type parameters and constraints</span></span>

<span data-ttu-id="cec0a-330">Jeśli typ ogólny jest zadeklarowany w wielu częściach, każda część musi określać parametry typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="cec0a-331">Każda część musi mieć taką samą liczbę parametrów typu i taką samą nazwę dla każdego parametru typu, w pożądanej kolejności.</span><span class="sxs-lookup"><span data-stu-id="cec0a-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="cec0a-332">Gdy częściowa deklaracja typu ogólnego zawiera ograniczenia (klauzule`where`), ograniczenia muszą zgadzać się ze wszystkimi innymi częściami, które zawierają ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="cec0a-333">W odniesieniu do każdej części, która zawiera ograniczenia muszą mieć ograniczenia dotyczące tego samego zestawu parametrów typu, a dla każdego parametru typu zestawy podstawowe, pomocnicze i konstruktory muszą być równoważne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="cec0a-334">Dwa zestawy ograniczeń są równoważne, jeśli zawierają te same elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="cec0a-335">Jeśli żadna część częściowego typu ogólnego określa ograniczenia parametru typu, parametry typu są uznawane za nieograniczone.</span><span class="sxs-lookup"><span data-stu-id="cec0a-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="cec0a-336">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-336">The example</span></span>
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
<span data-ttu-id="cec0a-337">jest poprawna, ponieważ te części, które zawierają ograniczenia (pierwsze dwa) skutecznie określają ten sam zestaw ograniczeń podstawowych, pomocniczych i konstruktorów dla tego samego zestawu parametrów typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="cec0a-338">Klasa bazowa</span><span class="sxs-lookup"><span data-stu-id="cec0a-338">Base class</span></span>

<span data-ttu-id="cec0a-339">Gdy deklaracja klasy częściowej zawiera specyfikację klasy bazowej, musi zgadzać się ze wszystkimi innymi częściami, które zawierają specyfikację klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="cec0a-340">Jeśli żadna część klasy częściowej nie zawiera specyfikacji klasy bazowej, Klasa bazowa stają się `System.Object` ([klasy bazowe](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="cec0a-341">Interfejsy podstawowe</span><span class="sxs-lookup"><span data-stu-id="cec0a-341">Base interfaces</span></span>

<span data-ttu-id="cec0a-342">Zestaw interfejsów podstawowych dla typu zadeklarowanego w wielu częściach jest złożeniem interfejsów podstawowych określonych w każdej części.</span><span class="sxs-lookup"><span data-stu-id="cec0a-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="cec0a-343">Określony interfejs podstawowy może być nazwany tylko raz w każdej części, ale jest dozwolony dla wielu części, aby nazwać te same interfejsy podstawowe.</span><span class="sxs-lookup"><span data-stu-id="cec0a-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="cec0a-344">Może istnieć tylko jedna implementacja elementów członkowskich dowolnego interfejsu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="cec0a-345">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="cec0a-346">zestaw interfejsów podstawowych dla klasy `C` jest `IA`, `IB`i `IC`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="cec0a-347">Zazwyczaj każda część zawiera implementację interfejsów zadeklarowanych w tej części; nie jest to jednak wymagane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="cec0a-348">Część może zapewnić implementację interfejsu zadeklarowanego w innej części:</span><span class="sxs-lookup"><span data-stu-id="cec0a-348">A part may provide the implementation for an interface declared on a different part:</span></span>
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a><span data-ttu-id="cec0a-349">Members</span><span class="sxs-lookup"><span data-stu-id="cec0a-349">Members</span></span>

<span data-ttu-id="cec0a-350">Z wyjątkiem metod częściowych ([metod częściowych](classes.md#partial-methods)) zestaw elementów członkowskich typu zadeklarowanych w wielu częściach jest po prostu Unią zestawu elementów członkowskich zadeklarowanych w każdej części.</span><span class="sxs-lookup"><span data-stu-id="cec0a-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="cec0a-351">Ciała wszystkich części deklaracji typu współdzielą ten sam obszar deklaracji ([deklaracje](basic-concepts.md#declarations)), a zakres każdego elementu członkowskiego ([zakresów](basic-concepts.md#scopes)) rozciąga się na treść wszystkich części.</span><span class="sxs-lookup"><span data-stu-id="cec0a-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="cec0a-352">Domena dostępności dowolnego elementu członkowskiego zawsze zawiera wszystkie części typu otaczającego; członek `private` zadeklarowany w jednej części jest swobodnie dostępny z innej części.</span><span class="sxs-lookup"><span data-stu-id="cec0a-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="cec0a-353">Jest to błąd czasu kompilacji, który deklaruje ten sam element członkowski w więcej niż jednej części typu, chyba że ten element członkowski jest typem z modyfikatorem `partial`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

<span data-ttu-id="cec0a-354">Kolejność elementów członkowskich w ramach typu jest rzadko istotna dla C# kodu, ale może być istotna w przypadku współdziałania z innymi językami i środowiskami.</span><span class="sxs-lookup"><span data-stu-id="cec0a-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="cec0a-355">W takich przypadkach kolejność elementów członkowskich w typie zadeklarowanym w wielu częściach jest niezdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="cec0a-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="cec0a-356">Metody częściowe</span><span class="sxs-lookup"><span data-stu-id="cec0a-356">Partial methods</span></span>

<span data-ttu-id="cec0a-357">Metody częściowe można definiować w jednej części deklaracji typu i zaimplementowane w innej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="cec0a-358">Implementacja jest opcjonalna; Jeśli żadna część nie implementuje metody częściowej, deklaracja częściowej metody i wszystkie wywołania do niej są usuwane z deklaracji typu, która wynika z kombinacji części.</span><span class="sxs-lookup"><span data-stu-id="cec0a-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="cec0a-359">Metody częściowe nie mogą definiować modyfikatorów dostępu, ale są niejawnie `private`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="cec0a-360">Typem zwracanym musi być `void`, a ich parametry nie mogą mieć modyfikatora `out`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="cec0a-361">Identyfikator `partial` jest rozpoznawany jako specjalne słowo kluczowe w deklaracji metody tylko wtedy, gdy jest wyświetlany bezpośrednio przed typem `void`; w przeciwnym razie można go użyć jako normalnego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="cec0a-362">Metoda częściowa nie może jawnie implementować metod interfejsu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="cec0a-363">Istnieją dwa rodzaje deklaracji metody częściowej: Jeśli treść deklaracji metody jest średnikiem, deklaracja jest ***określana jako definicje metody częściowej***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="cec0a-364">Jeśli treść jest przyznany jako *blok*, deklaracja jest określana jako ***implementująca Deklaracja metody częściowej***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="cec0a-365">Dla części deklaracji typu może istnieć tylko jedna definiująca deklarację metody częściowej z danym podpisem, a może istnieć tylko jedna implementująca Deklaracja metody częściowej z danym podpisem.</span><span class="sxs-lookup"><span data-stu-id="cec0a-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="cec0a-366">W przypadku podanej deklaracji metody częściowej należy określić odpowiadającą jej definicje metodę częściową, a deklaracje muszą być zgodne z określonymi w następujących przypadkach:</span><span class="sxs-lookup"><span data-stu-id="cec0a-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="cec0a-367">Deklaracje muszą mieć takie same Modyfikatory (choć niekoniecznie w tej samej kolejności), nazwę metody, liczbę parametrów typu i liczbę parametrów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="cec0a-368">Odpowiednie parametry w deklaracjach muszą mieć takie same Modyfikatory (choć nie muszą być w tej samej kolejności) i te same typy (różnice modulo w nazwach parametrów typu).</span><span class="sxs-lookup"><span data-stu-id="cec0a-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="cec0a-369">Odpowiednie parametry typu w deklaracjach muszą mieć takie same ograniczenia (różnice modulo w nazwach parametrów typu).</span><span class="sxs-lookup"><span data-stu-id="cec0a-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="cec0a-370">Implementacja deklaracji metody częściowej może pojawić się w tej samej części co odpowiadająca jej definiująca Deklaracja metody częściowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="cec0a-371">Tylko Definiowanie metody częściowej uczestniczy w rozpoznaniu przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="cec0a-372">W tym przypadku, niezależnie od tego, czy jest określona deklaracja implementująca, wyrażenia wywołania mogą zostać rozwiązane z wywołaniami metody częściowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="cec0a-373">Ponieważ metoda częściowa zawsze zwraca `void`, takie wyrażenia wywołania zawsze będą instrukcją wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="cec0a-374">Ponadto ponieważ metoda częściowa jest niejawnie `private`, takie instrukcje są zawsze wykonywane w jednej z części deklaracji typu, w ramach której zadeklarowano metodę częściową.</span><span class="sxs-lookup"><span data-stu-id="cec0a-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="cec0a-375">Jeśli żadna część deklaracji typu częściowego zawiera deklarację implementującą dla danej metody częściowej, każda instrukcja wyrażenia wywołująca ją jest po prostu usunięta z złożonej deklaracji typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="cec0a-376">W efekcie wyrażenie wywołania, łącznie ze wszystkimi wyrażeniami składowymi, nie ma wpływu na czas wykonywania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="cec0a-377">Sama metoda częściowa jest również usuwana i nie będzie składową deklaracji typu połączonego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="cec0a-378">Jeśli istnieje deklaracja implementująca dla danej metody częściowej, wywołania metod częściowych są zachowywane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="cec0a-379">Metoda częściowa daje podstawę deklaracji metody podobnej do implementującej deklaracji metody częściowej z wyjątkiem następujących:</span><span class="sxs-lookup"><span data-stu-id="cec0a-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="cec0a-380">Modyfikator `partial` nie jest uwzględniony</span><span class="sxs-lookup"><span data-stu-id="cec0a-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="cec0a-381">Atrybuty w deklaracji metody otrzymanej są połączonymi atrybutami definicji i implementowania deklaracji metody częściowej w nieokreślonej kolejności.</span><span class="sxs-lookup"><span data-stu-id="cec0a-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="cec0a-382">Duplikaty nie są usuwane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="cec0a-383">Atrybuty w parametrach deklaracji metody będącej wynikiem są połączone atrybuty odpowiednich parametrów definiujących i implementujących deklaracji metody częściowej w nieokreślonej kolejności.</span><span class="sxs-lookup"><span data-stu-id="cec0a-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="cec0a-384">Duplikaty nie są usuwane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-384">Duplicates are not removed.</span></span>

<span data-ttu-id="cec0a-385">Jeśli zdefiniowanie deklaracji, ale nie deklaracji implementującej, jest podano dla metody częściowej M, obowiązują następujące ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="cec0a-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="cec0a-386">Jest to błąd czasu kompilacji służący do tworzenia delegata metody ([delegatów tworzenia obiektów delegowanych](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="cec0a-387">Jest to błąd czasu kompilacji, który odwołuje się do `M` wewnątrz funkcji anonimowej, która jest konwertowana na typ drzewa wyrażenia ([Obliczanie konwersji funkcji anonimowych na typy drzewa wyrażeń](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="cec0a-388">Wyrażenia występujące jako część wywołania `M` nie wpływają na określony stan przypisania ([przypisanie określone](variables.md#definite-assignment)), co może potencjalnie doprowadzić do błędów czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="cec0a-389">`M` nie może być punktem wejścia dla aplikacji ([Uruchamianie aplikacji](basic-concepts.md#application-startup)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="cec0a-390">Metody częściowe są przydatne do umożliwienia jednej części deklaracji typu, aby dostosować zachowanie innej części, np., która jest generowana przez narzędzie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="cec0a-391">Rozważmy następującą deklarację klasy częściowej:</span><span class="sxs-lookup"><span data-stu-id="cec0a-391">Consider the following partial class declaration:</span></span>
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

<span data-ttu-id="cec0a-392">Jeśli ta klasa jest skompilowana bez żadnych innych części, definicje deklaracji metody częściowej i ich wywołania zostaną usunięte, a wynikiem połączonej deklaracji klasy będzie odpowiednik następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="cec0a-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

<span data-ttu-id="cec0a-393">Załóżmy, że inna część została określona, Jednakże, która zawiera deklaracje implementacji metod częściowych:</span><span class="sxs-lookup"><span data-stu-id="cec0a-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

<span data-ttu-id="cec0a-394">Następnie wynikiem połączonej deklaracji klasy będzie odpowiednik następujących:</span><span class="sxs-lookup"><span data-stu-id="cec0a-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a><span data-ttu-id="cec0a-395">Powiązanie nazw</span><span class="sxs-lookup"><span data-stu-id="cec0a-395">Name binding</span></span>

<span data-ttu-id="cec0a-396">Chociaż każda część rozszerzalnego typu musi być zadeklarowana w obrębie tego samego obszaru nazw, części są zwykle zapisywane w ramach różnych deklaracji przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="cec0a-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="cec0a-397">W ten sposób różne dyrektywy `using` ([dyrektywy using](namespaces.md#using-directives)) mogą być obecne dla każdej części.</span><span class="sxs-lookup"><span data-stu-id="cec0a-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="cec0a-398">Podczas interpretacji prostych nazw ([wnioskowanie o typie](expressions.md#type-inference)) w jednej części należy wziąć pod uwagę tylko dyrektywy `using` deklaracji przestrzeni nazw otaczającej tę część.</span><span class="sxs-lookup"><span data-stu-id="cec0a-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="cec0a-399">Może to spowodować, że ten sam identyfikator ma różne znaczenie w różnych częściach:</span><span class="sxs-lookup"><span data-stu-id="cec0a-399">This may result in the same identifier having different meanings in different parts:</span></span>
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a><span data-ttu-id="cec0a-400">Elementy członkowskie klasy</span><span class="sxs-lookup"><span data-stu-id="cec0a-400">Class members</span></span>

<span data-ttu-id="cec0a-401">Elementy członkowskie klasy składają się z elementów członkowskich wprowadzonych przez *class_member_declaration*s i członków dziedziczonych z bezpośredniej klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

<span data-ttu-id="cec0a-402">Elementy członkowskie typu klasy są podzielone na następujące kategorie:</span><span class="sxs-lookup"><span data-stu-id="cec0a-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="cec0a-403">Stałe, które reprezentują wartości stałe skojarzone z klasą ([stałe](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="cec0a-404">Pola, które są zmiennymi klasy ([pól](classes.md#fields)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="cec0a-405">Metody, które implementują obliczenia i akcje, które mogą być wykonywane przez klasę ([metody](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="cec0a-406">Właściwości, które definiują nazwane cechy i akcje związane z odczytem i pisaniem tych cech ([Właściwości](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="cec0a-407">Zdarzenia, które definiują powiadomienia, które mogą być generowane przez klasę ([zdarzenia](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="cec0a-408">Indeksatory, które zezwalają na indeksowanie wystąpień klasy w taki sam sposób (syntaktycznie) jak tablice ([indeksatory](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="cec0a-409">Operatory definiujące operatory wyrażeń, które mogą być stosowane do wystąpień klasy ([Operatory](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="cec0a-410">Konstruktory wystąpień, które implementują akcje wymagane do zainicjowania wystąpień klasy ([konstruktorów wystąpień](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="cec0a-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="cec0a-411">Destruktory, które implementują akcje do wykonania przed wystąpieniem klasy, zostaną trwale odrzucone ([destruktory](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="cec0a-412">Konstruktory statyczne, które implementują akcje wymagane do zainicjowania samej klasy ([konstruktory statyczne](classes.md#static-constructors)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="cec0a-413">Typy, które reprezentują typy, które są lokalne dla klasy ([typy zagnieżdżone](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="cec0a-414">Elementy członkowskie, które mogą zawierać kod wykonywalny, są określane zbiorczo jako *elementy członkowskie* typu klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="cec0a-415">Składowymi funkcji typu klasy są metody, właściwości, zdarzenia, indeksatory, operatory, konstruktory wystąpień, destruktory i konstruktory statyczne tego typu klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="cec0a-416">*Class_declaration* tworzy nowe miejsce deklaracji ([deklaracje](basic-concepts.md#declarations)), a *class_member_declaration*s bezpośrednio zawarte w *class_declaration* wprowadzić nowych członków do tego obszaru deklaracji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="cec0a-417">Do *class_member_declaration*s mają zastosowanie następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="cec0a-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="cec0a-418">Konstruktory wystąpień, destruktory i konstruktory statyczne muszą mieć taką samą nazwę jak bezpośrednio otaczającą klasę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="cec0a-419">Wszystkie inne elementy członkowskie muszą mieć nazwy, które różnią się od nazwy bezpośrednio otaczającej klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="cec0a-420">Nazwa stałej, pola, właściwości, zdarzenia lub typu musi różnić się od nazw wszystkich innych elementów członkowskich zadeklarowanych w tej samej klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="cec0a-421">Nazwa metody musi różnić się od nazw wszystkich innych metod, które nie są zadeklarowane w tej samej klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="cec0a-422">Ponadto sygnatura ([sygnatury i Przeciążenie](basic-concepts.md#signatures-and-overloading)) metody musi różnić się od sygnatur wszystkich innych metod zadeklarowanych w tej samej klasie, a dwie metody zadeklarowane w tej samej klasie mogą nie mieć sygnatur, które różnią się wyłącznie `ref` i `out`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="cec0a-423">Sygnatura konstruktora wystąpienia musi różnić się od sygnatur wszystkich innych konstruktorów wystąpień zadeklarowanych w tej samej klasie, a dwa konstruktory zadeklarowane w tej samej klasie mogą nie mieć sygnatur, które różnią się wyłącznie `ref` i `out`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="cec0a-424">Sygnatura indeksatora musi różnić się od sygnatur wszystkich innych indeksatorów zadeklarowanych w tej samej klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="cec0a-425">Sygnatura operatora musi różnić się od sygnatur wszystkich innych operatorów zadeklarowanych w tej samej klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="cec0a-426">Dziedziczone elementy członkowskie typu klasy ([dziedziczenie](classes.md#inheritance)) nie są częścią obszaru deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="cec0a-427">W efekcie Klasa pochodna może zadeklarować element członkowski o tej samej nazwie lub podpisie jako dziedziczonego elementu członkowskiego (co w efekcie powoduje ukrycie dziedziczonego elementu członkowskiego).</span><span class="sxs-lookup"><span data-stu-id="cec0a-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="cec0a-428">Typ wystąpienia</span><span class="sxs-lookup"><span data-stu-id="cec0a-428">The instance type</span></span>

<span data-ttu-id="cec0a-429">Każda deklaracja klasy ma skojarzony typ powiązania ([powiązane i niepowiązane typy](types.md#bound-and-unbound-types)), ***Typ wystąpienia***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="cec0a-430">Dla deklaracji klasy generycznej typ wystąpienia jest tworzony przez utworzenie typu złożonego ([konstruowane typy](types.md#constructed-types)) z deklaracji typu, z każdym z podanych argumentów typu, które są odpowiednim parametrem typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="cec0a-431">Ponieważ typ wystąpienia używa parametrów typu, może być używany tylko wtedy, gdy parametry typu znajdują się w zakresie; oznacza to, że wewnątrz deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="cec0a-432">Typ wystąpienia jest typem `this` dla kodu zapisaną wewnątrz deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="cec0a-433">Dla klas nieogólnych typ wystąpienia jest po prostu zadeklarowaną klasą.</span><span class="sxs-lookup"><span data-stu-id="cec0a-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="cec0a-434">Poniżej przedstawiono kilka deklaracji klasy wraz z ich typami wystąpień:</span><span class="sxs-lookup"><span data-stu-id="cec0a-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="cec0a-435">Elementy członkowskie skonstruowanych typów</span><span class="sxs-lookup"><span data-stu-id="cec0a-435">Members of constructed types</span></span>

<span data-ttu-id="cec0a-436">Niedziedziczone elementy członkowskie złożonego typu są uzyskiwane przez podstawianie, dla każdego *type_parameter* w deklaracji elementu członkowskiego, odpowiadającego *type_argument* typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="cec0a-437">Proces podstawiania jest oparty na semantycznym znaczeniu deklaracji typu i nie jest po prostu podstawianie tekstu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="cec0a-438">Na przykład, dana deklaracja klasy generycznej</span><span class="sxs-lookup"><span data-stu-id="cec0a-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="cec0a-439">skonstruowany typ `Gen<int[],IComparable<string>>` ma następujących członków:</span><span class="sxs-lookup"><span data-stu-id="cec0a-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="cec0a-440">Typ elementu członkowskiego `a` w deklaracji klasy generycznej `Gen` to "Dwuwymiarowa tablica `T`", więc typ elementu członkowskiego `a` w skonstruowanym typie powyżej to "Dwuwymiarowa tablica Jednowymiarowa tablicy `int`" lub `int[,][]`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="cec0a-441">W ramach elementów członkowskich funkcji wystąpienia typ `this` jest typem wystąpienia ([typem wystąpienia](classes.md#the-instance-type)) zawierającej deklaracji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="cec0a-442">Wszystkie elementy członkowskie klasy generycznej mogą używać parametrów typu z dowolnej klasy otaczającej, bezpośrednio lub jako część typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="cec0a-443">Gdy określony zamknięty typ skonstruowany ([otwarty i zamknięty](types.md#open-and-closed-types)) jest używany w czasie wykonywania, każde użycie parametru typu jest zastępowane rzeczywistym argumentem typu dostarczonym do typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="cec0a-444">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-444">For example:</span></span>
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a><span data-ttu-id="cec0a-445">Dziedziczenie</span><span class="sxs-lookup"><span data-stu-id="cec0a-445">Inheritance</span></span>

<span data-ttu-id="cec0a-446">Klasa ***dziedziczy*** elementy członkowskie jego bezpośredniego typu klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="cec0a-447">Dziedziczenie oznacza, że Klasa niejawnie zawiera wszystkie elementy członkowskie typu bezpośredniej klasy podstawowej, z wyjątkiem konstruktorów wystąpień, destruktorów i konstruktorów statycznych klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="cec0a-448">Oto kilka ważnych aspektów dziedziczenia:</span><span class="sxs-lookup"><span data-stu-id="cec0a-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="cec0a-449">Dziedziczenie jest przechodnie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-449">Inheritance is transitive.</span></span> <span data-ttu-id="cec0a-450">Jeśli `C` pochodzi od `B`, a `B` pochodzi od `A`, wówczas `C` dziedziczy członków zadeklarowanych w `B`, jak również członków zadeklarowanych w `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="cec0a-451">Klasa pochodna rozszerza swoją bezpośrednią klasę bazową.</span><span class="sxs-lookup"><span data-stu-id="cec0a-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="cec0a-452">Klasa pochodna może dodawać nowych członków do tych, które dziedziczy, ale nie może usunąć definicji dziedziczonego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="cec0a-453">Konstruktory wystąpień, destruktory i konstruktory statyczne nie są dziedziczone, ale wszystkie inne elementy członkowskie są, niezależnie od ich zadeklarowanej dostępności ([dostęp do elementów członkowskich](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="cec0a-454">Jednak w zależności od ich zadeklarowanej dostępności dziedziczone elementy członkowskie mogą nie być dostępne w klasie pochodnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="cec0a-455">Klasa pochodna może ***ukryć*** ([ukrywając przez dziedziczenie](basic-concepts.md#hiding-through-inheritance)) dziedziczone elementy członkowskie przez zadeklarowanie nowych członków o tej samej nazwie lub podpisie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="cec0a-456">Należy jednak zauważyć, że ukrycie dziedziczonego elementu członkowskiego nie powoduje usunięcia tego elementu członkowskiego — jedynie sprawia, że ten element członkowski jest niedostępny bezpośrednio za pomocą klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="cec0a-457">Wystąpienie klasy zawiera zestaw wszystkich pól wystąpienia zadeklarowanych w klasie i jej klasy bazowej, a niejawna konwersja ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions)) istnieje z klasy pochodnej do dowolnego typu klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="cec0a-458">W tym celu odwołanie do wystąpienia jakiejś klasy pochodnej może być traktowane jako odwołanie do wystąpienia którejkolwiek z jego klas podstawowych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="cec0a-459">Klasa może deklarować metody wirtualne, właściwości i indeksatory, a klasy pochodne mogą przesłaniać implementację tych elementów członkowskich funkcji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="cec0a-460">Dzięki temu klasy mogą mieć zachowanie polimorficzne, w którym akcje wykonywane przez wywołanie elementu członkowskiego funkcji różnią się w zależności od typu czasu wykonywania wystąpienia, za pomocą którego wywoływany jest element członkowski funkcji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="cec0a-461">Dziedziczone składowe typu konstruowanej klasy są elementami członkowskimi bezpośredniego typu klasy podstawowej ([klasy bazowe](classes.md#base-classes)), które można znaleźć przez podstawianie argumentów typu konstruowanego typu dla każdego wystąpienia odpowiednich parametrów typu w specyfikacji *class_base* .</span><span class="sxs-lookup"><span data-stu-id="cec0a-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="cec0a-462">Te elementy członkowskie z kolei są przekształcane przez podstawianie dla każdego *type_parameter* w deklaracji elementu członkowskiego, odpowiadającego *type_argument* specyfikacji *class_base* .</span><span class="sxs-lookup"><span data-stu-id="cec0a-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

<span data-ttu-id="cec0a-463">W powyższym przykładzie, skonstruowany typ `D<int>` ma niedziedziczonej składowej `public int G(string s)` uzyskany przez podstawianie argumentu typu `int` dla parametru typu `T`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="cec0a-464">`D<int>` ma także Dziedziczony element członkowski z deklaracji klasy `B`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="cec0a-465">Ten Dziedziczony element członkowski jest określany przez najpierw określenie typu klasy bazowej `B<int[]>` `D<int>` przez podstawianie `int` dla `T` w specyfikacji klasy podstawowej `B<T[]>`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="cec0a-466">Następnie jako argument typu do `B`, `int[]` jest zastępowany dla `U` w `public U F(long index)`, co powoduje zwrócenie dziedziczonego elementu członkowskiego `public int[] F(long index)`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="cec0a-467">Nowy modyfikator</span><span class="sxs-lookup"><span data-stu-id="cec0a-467">The new modifier</span></span>

<span data-ttu-id="cec0a-468">*Class_member_declaration* może zadeklarować składową o tej samej nazwie lub podpisie jako dziedziczonego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="cec0a-469">W takim przypadku element członkowski klasy pochodnej jest określany jako ***ukryty*** element członkowski klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="cec0a-470">Ukrycie dziedziczonego elementu członkowskiego nie jest uważane za błąd, ale powoduje, że kompilator wystawia ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="cec0a-471">Aby pominąć ostrzeżenie, Deklaracja elementu członkowskiego klasy pochodnej może zawierać modyfikator `new`, aby wskazać, że pochodna składowa jest przeznaczona do ukrycia podstawowego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="cec0a-472">Ten temat został omówiony w dalszej części w [ukrywaniu przez dziedziczenie](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="cec0a-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="cec0a-473">Jeśli modyfikator `new` jest zawarty w deklaracji, która nie ukrywa dziedziczonego elementu członkowskiego, zostanie wygenerowane ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="cec0a-474">To ostrzeżenie jest pomijane przez usunięcie modyfikatora `new`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="cec0a-475">Modyfikatory dostępu</span><span class="sxs-lookup"><span data-stu-id="cec0a-475">Access modifiers</span></span>

<span data-ttu-id="cec0a-476">*Class_member_declaration* może mieć jeden z pięciu możliwych rodzajów zadeklarowanych dostępności ([deklarowane ułatwienia dostępu](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`lub `private`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="cec0a-477">Z wyjątkiem kombinacji `protected internal`, jest to błąd czasu kompilacji, aby określić więcej niż jeden modyfikator dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="cec0a-478">Gdy *class_member_declaration* nie zawiera żadnych modyfikatorów dostępu, przyjmowana jest `private`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="cec0a-479">Typy składników</span><span class="sxs-lookup"><span data-stu-id="cec0a-479">Constituent types</span></span>

<span data-ttu-id="cec0a-480">Typy, które są używane w deklaracji elementu członkowskiego, są nazywane typami elementów tego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="cec0a-481">Możliwe typy składników to typ stałej, pola, właściwości, zdarzenia lub indeksatora, zwracany typ metody lub operatora oraz typy parametrów metody, indeksatora, operatora lub konstruktora wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="cec0a-482">Typy składników składowych muszą być co najmniej tak samo dostępne, jak sam element członkowski ([ograniczenia dotyczące ułatwień dostępu](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="cec0a-483">Elementy członkowskie statyczne i wystąpienia</span><span class="sxs-lookup"><span data-stu-id="cec0a-483">Static and instance members</span></span>

<span data-ttu-id="cec0a-484">Elementy członkowskie klasy są ***statycznymi elementami członkowskimi*** lub ***wystąpieniami***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="cec0a-485">Ogólnie mówiąc, warto traktować statyczne elementy członkowskie jako należące do typów klas i elementów członkowskich wystąpienia jako należące do obiektów (wystąpień typów klas).</span><span class="sxs-lookup"><span data-stu-id="cec0a-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="cec0a-486">Gdy deklaracja pola, metody, właściwości, zdarzenia, operatora lub konstruktora zawiera modyfikator `static`, deklaruje statyczny element członkowski.</span><span class="sxs-lookup"><span data-stu-id="cec0a-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="cec0a-487">Ponadto deklaracja stałej lub typu niejawnie deklaruje statyczny element członkowski.</span><span class="sxs-lookup"><span data-stu-id="cec0a-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="cec0a-488">Statyczne składowe mają następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="cec0a-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="cec0a-489">Gdy statyczny element członkowski `M` jest przywoływany w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `E.M`, `E` musi zwrócić uwagę na typ zawierający `M`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="cec0a-490">Jest to błąd czasu kompilacji dla `E`, aby zauważyć wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="cec0a-491">Pole statyczne określa dokładnie jedną lokalizację magazynu, która ma być współużytkowana przez wszystkie wystąpienia danego typu zamkniętej klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="cec0a-492">Bez względu na to, ile wystąpień danego typu zamkniętej klasy są tworzone, istnieje tylko jedna kopia pola statycznego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="cec0a-493">Statyczny element członkowski funkcji (metoda, właściwość, zdarzenie, operator lub Konstruktor) nie działa w konkretnym wystąpieniu i jest to błąd czasu kompilacji, aby odwołać się do `this` w takiej składowej funkcji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="cec0a-494">Gdy deklaracja pola, metody, właściwości, zdarzenia, indeksatora, konstruktora lub destruktora nie zawiera modyfikatora `static`, deklaruje element członkowski wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="cec0a-495">(Składowa wystąpienia jest czasami nazywana niestatyczną składową). Elementy członkowskie wystąpienia mają następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="cec0a-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="cec0a-496">Gdy element członkowski wystąpienia `M` jest przywoływany w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `E.M`, `E` musi zwrócić uwagę na wystąpienie typu zawierającego `M`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="cec0a-497">Jest to błąd czasu powiązania dla `E`, aby zauważyć typ.</span><span class="sxs-lookup"><span data-stu-id="cec0a-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="cec0a-498">Każde wystąpienie klasy zawiera oddzielny zestaw wszystkich pól wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="cec0a-499">Element członkowski funkcji wystąpienia (metoda, właściwość, indeksator, Konstruktor wystąpienia lub destruktor) działa w danym wystąpieniu klasy i można uzyskać do niego dostęp jako `this` ([ten dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="cec0a-500">Poniższy przykład ilustruje reguły uzyskiwania dostępu do statycznych i składowych wystąpień:</span><span class="sxs-lookup"><span data-stu-id="cec0a-500">The following example illustrates the rules for accessing static and instance members:</span></span>
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

<span data-ttu-id="cec0a-501">Metoda `F` pokazuje, że w elemencie członkowskim funkcji wystąpienia *simple_name* ([proste nazwy](expressions.md#simple-names)) mogą być używane do uzyskiwania dostępu do elementów członkowskich wystąpienia i statycznych elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="cec0a-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="cec0a-502">Metoda `G` pokazuje, że w składowej funkcji statycznej jest to błąd czasu kompilacji, aby uzyskać dostęp do elementu członkowskiego wystąpienia za pomocą *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="cec0a-503">Metoda `Main` pokazuje, że w *member_access* ([dostęp do elementów członkowskich](expressions.md#member-access)) należy uzyskać dostęp do składowych wystąpień za pomocą wystąpień, a statyczne elementy członkowskie muszą być dostępne za pomocą typów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="cec0a-504">Typy zagnieżdżone</span><span class="sxs-lookup"><span data-stu-id="cec0a-504">Nested types</span></span>

<span data-ttu-id="cec0a-505">Typ zadeklarowany w deklaracji klasy lub struktury jest nazywany ***typem zagnieżdżonym***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="cec0a-506">Typ, który jest zadeklarowany w ramach jednostki kompilacji lub przestrzeni nazw, jest nazywany ***typem niezagnieżdżonym***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="cec0a-507">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-507">In the example</span></span>
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
<span data-ttu-id="cec0a-508">Klasa `B` jest typem zagnieżdżonym, ponieważ jest zadeklarowana w klasie `A`, a Klasa `A` jest typem niezagnieżdżonym, ponieważ jest zadeklarowana w ramach jednostki kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="cec0a-509">W pełni kwalifikowana nazwa</span><span class="sxs-lookup"><span data-stu-id="cec0a-509">Fully qualified name</span></span>

<span data-ttu-id="cec0a-510">W pełni kwalifikowana nazwa (w[pełni kwalifikowana](basic-concepts.md#fully-qualified-names)nazwa) dla typu zagnieżdżonego jest `S.N` gdzie `S` jest w pełni kwalifikowaną nazwą typu, w którym jest zadeklarowany `N` typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="cec0a-511">Zadeklarowane ułatwienia dostępu</span><span class="sxs-lookup"><span data-stu-id="cec0a-511">Declared accessibility</span></span>

<span data-ttu-id="cec0a-512">Typy niezagnieżdżone mogą mieć `public` lub `internal` zadeklarowane ułatwienia dostępu i mieć domyślnie `internal` zadeklarowane ułatwienia dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="cec0a-513">Typy zagnieżdżone mogą mieć te formy zadeklarowanej dostępności, a także co najmniej jedną dodatkową postać zadeklarowanej dostępności, w zależności od tego, czy typ zawierający jest klasą czy strukturą:</span><span class="sxs-lookup"><span data-stu-id="cec0a-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="cec0a-514">Typ zagnieżdżony zadeklarowany w klasie może mieć jedną z pięciu postaci zadeklarowanej dostępności (`public`, `protected internal`, `protected`, `internal`lub `private`) i, podobnie jak inne elementy członkowskie klasy, domyślnie `private` zadeklarowane ułatwienia dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="cec0a-515">Zagnieżdżony typ, który jest zadeklarowany w strukturze, może mieć jedną z trzech postaci zadeklarowanej dostępności (`public`, `internal`lub `private`) i, podobnie jak w przypadku innych elementów członkowskich struktury, domyślnie `private` zadeklarowane ułatwienia dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="cec0a-516">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-516">The example</span></span>
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
<span data-ttu-id="cec0a-517">deklaruje prywatną `Node`klasy zagnieżdżonej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="cec0a-518">Ukryj</span><span class="sxs-lookup"><span data-stu-id="cec0a-518">Hiding</span></span>

<span data-ttu-id="cec0a-519">Zagnieżdżony typ może ukrywać ([ukrywając nazwę](basic-concepts.md#name-hiding)) podstawową składową.</span><span class="sxs-lookup"><span data-stu-id="cec0a-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="cec0a-520">Modyfikator `new` jest dozwolony w deklaracjach typu zagnieżdżonego, dzięki czemu ukrywanie może być wyrażone jawnie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="cec0a-521">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-521">The example</span></span>
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
<span data-ttu-id="cec0a-522">pokazuje zagnieżdżoną klasę `M`, która ukrywa `M` metody zdefiniowanej w `Base`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="cec0a-523">Ten dostęp</span><span class="sxs-lookup"><span data-stu-id="cec0a-523">this access</span></span>

<span data-ttu-id="cec0a-524">Typ zagnieżdżony i jego typ zawierający nie mają specjalnej relacji w odniesieniu do *this_access* ([ten dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="cec0a-525">W celu odwoływania się do elementów członkowskich wystąpienia typu zawierającego nie można używać w tym celu `this` w ramach typu zagnieżdżonego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="cec0a-526">W przypadkach, gdy Typ zagnieżdżony wymaga dostępu do elementów członkowskich wystąpienia typu zawierającego, można uzyskać dostęp, dostarczając `this` dla wystąpienia typu zawierającego jako argument konstruktora dla typu zagnieżdżonego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="cec0a-527">Poniższy przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-527">The following example</span></span>
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
<span data-ttu-id="cec0a-528">pokazuje tę technikę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-528">shows this technique.</span></span> <span data-ttu-id="cec0a-529">Wystąpienie `C` tworzy wystąpienie `Nested` i przekazuje własne `this` do konstruktora `Nested`, aby zapewnić kolejny dostęp do elementów członkowskich wystąpienia `C`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="cec0a-530">Dostęp do prywatnych i chronionych składowych typu zawierającego</span><span class="sxs-lookup"><span data-stu-id="cec0a-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="cec0a-531">Typ zagnieżdżony ma dostęp do wszystkich elementów członkowskich, które są dostępne dla tego typu zawierającego, w tym elementów członkowskich typu zawierającego `private` i `protected` zadeklarowane ułatwienia dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="cec0a-532">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-532">The example</span></span>
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
<span data-ttu-id="cec0a-533">pokazuje klasę `C`, która zawiera `Nested`klasy zagnieżdżonej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="cec0a-534">W `Nested`Metoda `G` wywołuje metodę statyczną `F` zdefiniowaną w `C`i `F` ma prywatnie zadeklarowane ułatwienia dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="cec0a-535">Typ zagnieżdżony może również uzyskiwać dostęp do chronionych elementów członkowskich zdefiniowanych w typie podstawowym typu zawierającego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="cec0a-536">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-536">In the example</span></span>
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
<span data-ttu-id="cec0a-537">Klasa zagnieżdżona `Derived.Nested` uzyskuje dostęp do chronionej metody `F` zdefiniowanej w klasie podstawowej `Derived``Base`przez wywołanie przez wystąpienie `Derived`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="cec0a-538">Zagnieżdżone typy w klasach ogólnych</span><span class="sxs-lookup"><span data-stu-id="cec0a-538">Nested types in generic classes</span></span>

<span data-ttu-id="cec0a-539">Deklaracja klasy generycznej może zawierać deklaracje typu zagnieżdżonego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="cec0a-540">Parametry typu otaczającej klasy mogą być używane w zagnieżdżonych typach.</span><span class="sxs-lookup"><span data-stu-id="cec0a-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="cec0a-541">Deklaracja typu zagnieżdżonego może zawierać dodatkowe parametry typu, które mają zastosowanie tylko do typu zagnieżdżonego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="cec0a-542">Każda deklaracja typu znajdująca się w deklaracji klasy generycznej jest niejawnie deklaracją typu ogólnego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="cec0a-543">Podczas pisania odwołania do typu zagnieżdżonego w typie ogólnym, zawierający typ skonstruowany, w tym jego argumenty typu, musi mieć nazwę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="cec0a-544">Jednakże z poziomu klasy zewnętrznej można używać typu zagnieżdżonego bez kwalifikacji; Typ wystąpienia klasy zewnętrznej może być niejawnie używany podczas konstruowania typu zagnieżdżonego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="cec0a-545">Poniższy przykład pokazuje trzy różne prawidłowe sposoby odwoływania się do złożonego typu utworzonego na podstawie `Inner`; pierwsze dwa są równoważne:</span><span class="sxs-lookup"><span data-stu-id="cec0a-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

<span data-ttu-id="cec0a-546">Chociaż jest to zły styl programowania, parametr typu w typie zagnieżdżonym może ukryć element członkowski lub parametr typu zadeklarowany w typie zewnętrznym:</span><span class="sxs-lookup"><span data-stu-id="cec0a-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="cec0a-547">Zastrzeżone nazwy składowych</span><span class="sxs-lookup"><span data-stu-id="cec0a-547">Reserved member names</span></span>

<span data-ttu-id="cec0a-548">Aby ułatwić bazowa C# implementacji w czasie wykonywania, dla każdej źródłowej deklaracji składowej, która jest właściwością, zdarzeniem lub indeksatorem, implementacja musi zarezerwować dwa podpisy metod na podstawie rodzaju deklaracji elementu członkowskiego, jego nazwy i typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="cec0a-549">Jest to błąd czasu kompilacji dla programu w celu zadeklarowania elementu członkowskiego, którego sygnatura pasuje do jednego z tych zarezerwowanych podpisów, nawet jeśli bazowa implementacja czasu wykonywania nie używa tych rezerwacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="cec0a-550">Nazwy zastrzeżone nie wprowadzają deklaracji, dlatego nie uczestniczą w wyszukiwaniu elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="cec0a-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="cec0a-551">Jednak skojarzone podpisy metod zarezerwowanych deklaracji, które uczestniczą w dziedziczeniu ([dziedziczenie](classes.md#inheritance)) i mogą być ukrywane za pomocą modyfikatora `new` ([nowy modyfikator](classes.md#the-new-modifier)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="cec0a-552">Rezerwacja tych nazw służy trzy cele:</span><span class="sxs-lookup"><span data-stu-id="cec0a-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="cec0a-553">Aby umożliwić podstawowej implementacji użycie zwykłego identyfikatora jako nazwy metody do pobierania lub ustawiania dostępu do funkcji C# języka.</span><span class="sxs-lookup"><span data-stu-id="cec0a-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="cec0a-554">Aby zezwolić innym językom na współdziałanie przy użyciu zwykłego identyfikatora jako nazwy metody do pobierania lub ustawiania C# dostępu do funkcji języka.</span><span class="sxs-lookup"><span data-stu-id="cec0a-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="cec0a-555">Aby zapewnić, że źródło zaakceptowane przez jeden z zgodnych kompilatorów jest akceptowane przez inny, przez wprowadzenie określonych nazw zarezerwowanych elementów członkowskich spójnych we C# wszystkich implementacjach.</span><span class="sxs-lookup"><span data-stu-id="cec0a-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="cec0a-556">Deklaracja destruktora ([destruktory](classes.md#destructors)) powoduje również, że podpis ma być zarezerwowany ([nazwy składowe zarezerwowane dla destruktorów](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="cec0a-557">Nazwy elementów członkowskich zarezerwowane dla właściwości</span><span class="sxs-lookup"><span data-stu-id="cec0a-557">Member names reserved for properties</span></span>

<span data-ttu-id="cec0a-558">Dla właściwości `P` ([Właściwości](classes.md#properties)) typu `T`następujące podpisy są zastrzeżone:</span><span class="sxs-lookup"><span data-stu-id="cec0a-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="cec0a-559">Oba podpisy są zastrzeżone, nawet jeśli właściwość jest tylko do odczytu lub tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="cec0a-560">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-560">In the example</span></span>
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
<span data-ttu-id="cec0a-561">Klasa `A` definiuje właściwość tylko do odczytu `P`, w ten sposób zachowując sygnatury dla metod `get_P` i `set_P`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="cec0a-562">Klasa `B` pochodzi od `A` i ukrywa oba te podpisy zarezerwowane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="cec0a-563">Przykład generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="cec0a-563">The example produces the output:</span></span>
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="cec0a-564">Nazwy elementów członkowskich zarezerwowane dla zdarzeń</span><span class="sxs-lookup"><span data-stu-id="cec0a-564">Member names reserved for events</span></span>

<span data-ttu-id="cec0a-565">W przypadku zdarzenia `E` ([zdarzenia](classes.md#events)) typu delegata `T`następujące podpisy są zastrzeżone:</span><span class="sxs-lookup"><span data-stu-id="cec0a-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="cec0a-566">Nazwy elementów członkowskich zarezerwowane dla indeksatorów</span><span class="sxs-lookup"><span data-stu-id="cec0a-566">Member names reserved for indexers</span></span>

<span data-ttu-id="cec0a-567">Dla indeksatora ([indeksatorów](classes.md#indexers)) typu `T` z `L`listy parametrów następujące podpisy są zastrzeżone:</span><span class="sxs-lookup"><span data-stu-id="cec0a-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="cec0a-568">Oba podpisy są zastrzeżone, nawet jeśli indeksator jest tylko do odczytu lub tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="cec0a-569">Ponadto nazwa elementu członkowskiego `Item` jest zarezerwowana.</span><span class="sxs-lookup"><span data-stu-id="cec0a-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="cec0a-570">Nazwy elementów członkowskich zarezerwowane dla destruktorów</span><span class="sxs-lookup"><span data-stu-id="cec0a-570">Member names reserved for destructors</span></span>

<span data-ttu-id="cec0a-571">W przypadku klasy zawierającej destruktor ([destruktory](classes.md#destructors)) następujący podpis jest zastrzeżony:</span><span class="sxs-lookup"><span data-stu-id="cec0a-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="cec0a-572">{1&gt;Stałe&lt;1}</span><span class="sxs-lookup"><span data-stu-id="cec0a-572">Constants</span></span>

<span data-ttu-id="cec0a-573">***Stała*** jest składową klasy, która reprezentuje wartość stałą: wartość, która może być obliczana w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="cec0a-574">*Constant_declaration* wprowadza jedną lub więcej stałych danego typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="cec0a-575">*Constant_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)), modyfikator `new` ([nowy modyfikator](classes.md#the-new-modifier)) i prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="cec0a-576">Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych przez *constant_declaration*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="cec0a-577">Mimo że stałe są uznawane za statyczne elementy członkowskie, *constant_declaration* nie wymagają ani nie dopuszcza modyfikatora `static`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="cec0a-578">Jest to błąd dla tego samego modyfikatora, który może występować wiele razy w deklaracji stałej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="cec0a-579">*Typ* *constant_declaration* określa typ elementów członkowskich wprowadzonych przez deklarację.</span><span class="sxs-lookup"><span data-stu-id="cec0a-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="cec0a-580">Po tym typie następuje lista *constant_declarator*s, z których każdy wprowadza nowy element członkowski.</span><span class="sxs-lookup"><span data-stu-id="cec0a-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="cec0a-581">*Constant_declarator* składa się z *identyfikatora* , który ma nazwę elementu członkowskiego, po którym następuje token "`=`", po którym następuje *constant_expression* ([wyrażenia stałe](expressions.md#constant-expressions)), które dają wartość elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="cec0a-582">*Typ* określony w deklaracji stałej musi mieć wartość `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, *enum_type*i *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="cec0a-583">Każdy *constant_expression* musi zwracać wartość typu docelowego lub typu, który można przekonwertować na typ docelowy przez niejawną konwersję ([konwersje niejawne](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="cec0a-584">*Typ* stałej musi być co najmniej tak samo jak sam sam ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cec0a-585">Wartość stałej jest uzyskiwana w wyrażeniu przy użyciu *simple_name* ([nazw prostych](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="cec0a-586">Stała może same uczestniczyć w *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="cec0a-587">W tym celu można użyć stałej w dowolnej konstrukcji, która wymaga *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="cec0a-588">Przykłady takich konstrukcji obejmują `case` etykiet, instrukcji `goto case`, `enum` deklaracji składowych, atrybutów i innych deklaracji stałych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="cec0a-589">Zgodnie z opisem w [wyrażeniach stałych](expressions.md#constant-expressions) *constant_expression* jest wyrażeniem, które może być w pełni oceniane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="cec0a-590">Ze względu na to, że jedynym sposobem utworzenia wartości innej niż null *reference_type* innej niż `string` jest zastosowanie operatora `new`, a ponieważ operator `new` nie jest dozwolony w *constant_expression*, jedyną możliwą wartością dla stałych *reference_type*s innych niż `string` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="cec0a-591">Gdy wymagana jest Nazwa symboliczna wartości stałej, ale jeśli typ tej wartości nie jest dozwolony w deklaracji stałej lub gdy wartość nie może zostać obliczona w czasie kompilacji przez *constant_expression*, w zamian może być używane pole `readonly` ([pola tylko do odczytu](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="cec0a-592">Deklaracja stałej, która deklaruje wiele stałych, jest równoważna z wieloma deklaracjami pojedynczych stałych z tymi samymi atrybutami, modyfikatorami i typem.</span><span class="sxs-lookup"><span data-stu-id="cec0a-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="cec0a-593">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="cec0a-594">odpowiada wyrażeniu</span><span class="sxs-lookup"><span data-stu-id="cec0a-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="cec0a-595">Stałe mogą zależeć od innych stałych w ramach tego samego programu, o ile zależności nie mają charakteru cyklicznego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="cec0a-596">Kompilator automatycznie organizuje ocenę stałych deklaracji w odpowiedniej kolejności.</span><span class="sxs-lookup"><span data-stu-id="cec0a-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="cec0a-597">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-597">In the example</span></span>
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
<span data-ttu-id="cec0a-598">Kompilator najpierw szacuje `A.Y`, następnie szacuje `B.Z`, a wreszcie szacuje `A.X`, generując wartości `10`, `11`i `12`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="cec0a-599">Deklaracje stałe mogą zależeć od stałych z innych programów, ale takie zależności są możliwe tylko w jednym kierunku.</span><span class="sxs-lookup"><span data-stu-id="cec0a-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="cec0a-600">W odniesieniu do powyższego przykładu, jeśli `A` i `B` zostały zadeklarowane w osobnych programach, możliwe jest, aby `A.X` zależały od `B.Z`, ale `B.Z` nie zależały jednocześnie od `A.Y`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="cec0a-601">Pola</span><span class="sxs-lookup"><span data-stu-id="cec0a-601">Fields</span></span>

<span data-ttu-id="cec0a-602">***Pole*** jest składową, która reprezentuje zmienną skojarzoną z obiektem lub klasą.</span><span class="sxs-lookup"><span data-stu-id="cec0a-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="cec0a-603">*Field_declaration* wprowadza co najmniej jedno pole danego typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="cec0a-604">*Field_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)), modyfikator `new` ([nowy modyfikator](classes.md#the-new-modifier)), prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)) i modyfikator `static` ([pola statyczne i wystąpienia](classes.md#static-and-instance-fields)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="cec0a-605">Ponadto *field_declaration* może zawierać modyfikator `readonly` ([pola tylko do odczytu](classes.md#readonly-fields)) lub modyfikator `volatile` ([pola nietrwałe](classes.md#volatile-fields)), ale nie oba jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="cec0a-606">Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych przez *field_declaration*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="cec0a-607">Jest to błąd dla tego samego modyfikatora, który może występować wiele razy w deklaracji pola.</span><span class="sxs-lookup"><span data-stu-id="cec0a-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="cec0a-608">*Typ* *field_declaration* określa typ elementów członkowskich wprowadzonych przez deklarację.</span><span class="sxs-lookup"><span data-stu-id="cec0a-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="cec0a-609">Po tym typie następuje lista *variable_declarator*s, z których każdy wprowadza nowy element członkowski.</span><span class="sxs-lookup"><span data-stu-id="cec0a-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="cec0a-610">*Variable_declarator* składa się z *identyfikatora* , który jest nazwą tego elementu członkowskiego, opcjonalnie następuje token "`=`" i *variable_initializer* ([inicjatory zmiennych](classes.md#variable-initializers)), który daje początkową wartość tego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="cec0a-611">*Typ* pola musi być co najmniej tak samo jak w przypadku samego pola ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cec0a-612">Wartość pola jest uzyskiwana w wyrażeniu przy użyciu *simple_name* ([nazw prostych](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="cec0a-613">Wartość pola, które nie jest tylko do odczytu, jest modyfikowana przy użyciu *przypisania* ([Operatory przypisania](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="cec0a-614">Wartość pola, które nie jest tylko do odczytu, może być zarówno uzyskana, jak i modyfikowana przy użyciu przyrostka przyrostkowego i zmniejszania (operatory[przyrostu i zmniejszania](expressions.md#postfix-increment-and-decrement-operators)) oraz operatory przyrostu i zmniejszania wartości prefiksów ([Operatory przyrostu i](expressions.md#prefix-increment-and-decrement-operators)zmniejszania).</span><span class="sxs-lookup"><span data-stu-id="cec0a-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="cec0a-615">Deklaracja pola, która deklaruje wiele pól, jest równoważna z wieloma deklaracjami pojedynczych pól z tymi samymi atrybutami, modyfikatorami i typem.</span><span class="sxs-lookup"><span data-stu-id="cec0a-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="cec0a-616">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="cec0a-617">odpowiada wyrażeniu</span><span class="sxs-lookup"><span data-stu-id="cec0a-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="cec0a-618">Pola statyczne i wystąpienia</span><span class="sxs-lookup"><span data-stu-id="cec0a-618">Static and instance fields</span></span>

<span data-ttu-id="cec0a-619">Gdy deklaracja pola zawiera modyfikator `static`, pola wprowadzane przez deklarację są ***polami statycznymi***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="cec0a-620">Gdy modyfikator `static` nie jest obecny, pola wprowadzane przez deklarację są ***polami wystąpień***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="cec0a-621">Pola statyczne i wystąpienia są dwa rodzaje zmiennych ([zmienne](variables.md)) obsługiwane przez C#, a czasami są one określane odpowiednio jako ***zmienne statyczne*** i ***zmienne wystąpień***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="cec0a-622">Pole statyczne nie jest częścią określonego wystąpienia; Zamiast tego jest współużytkowany między wszystkimi wystąpieniami typu zamkniętego ([otwarte i zamknięte](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="cec0a-623">Niezależnie od tego, ile wystąpień typu zamkniętej klasy są tworzone, istnieje tylko jedna kopia pola statycznego dla skojarzonej domeny aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="cec0a-624">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-624">For example:</span></span>
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

<span data-ttu-id="cec0a-625">Pole wystąpienia należy do wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="cec0a-626">Każde wystąpienie klasy zawiera oddzielny zestaw wszystkich pól wystąpienia tej klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="cec0a-627">Gdy odwołanie do pola znajduje się w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `E.M`, jeśli `M` jest polem statycznym, `E` musi zwrócić uwagę na typ zawierający `M`i jeśli `M` jest polem wystąpienia, E musi zwrócić uwagę na wystąpienie typu zawierającego `M`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="cec0a-628">Różnice między elementami statycznymi a składowymi wystąpienia są omówione bardziej szczegółowo w [elementach członkowskich static i instance](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="cec0a-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="cec0a-629">Pola tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="cec0a-629">Readonly fields</span></span>

<span data-ttu-id="cec0a-630">Gdy *field_declaration* zawiera modyfikator `readonly`, pola wprowadzone przez deklarację to ***pola tylko do odczytu***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="cec0a-631">Bezpośrednie przypisania do pól tylko do odczytu mogą wystąpić tylko jako część tej deklaracji lub w konstruktorze wystąpienia lub w konstruktorze statycznym w tej samej klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="cec0a-632">(Pole tylko do odczytu można przypisać wiele razy w tych kontekstach). W szczególnych przypadkach przypisania bezpośrednie do pola `readonly` są dozwolone tylko w następujących kontekstach:</span><span class="sxs-lookup"><span data-stu-id="cec0a-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="cec0a-633">W *variable_declarator* , które wprowadza pole (poprzez uwzględnienie *variable_initializer* w deklaracji).</span><span class="sxs-lookup"><span data-stu-id="cec0a-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="cec0a-634">Dla pola wystąpienia w konstruktorach wystąpień klasy, która zawiera deklarację pola; dla pola statycznego w konstruktorze statycznym klasy, która zawiera deklarację pola.</span><span class="sxs-lookup"><span data-stu-id="cec0a-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="cec0a-635">Są to również jedyne konteksty, w których prawidłowe jest przekazanie pola `readonly` jako parametru `out` lub `ref`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="cec0a-636">Próba przypisania do pola `readonly` lub przekazania go jako parametru `out` lub `ref` w dowolnym innym kontekście jest błędem czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="cec0a-637">Używanie statycznych pól tylko do odczytu dla stałych</span><span class="sxs-lookup"><span data-stu-id="cec0a-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="cec0a-638">Pole `static readonly` jest przydatne, gdy wymagana jest Nazwa symboliczna wartości stałej, ale jeśli typ wartości nie jest dozwolony w deklaracji `const` lub nie można obliczyć wartości w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="cec0a-639">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-639">In the example</span></span>
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
<span data-ttu-id="cec0a-640">elementów członkowskich `Black`, `White`, `Red`, `Green`i `Blue` nie można zadeklarować jako elementów członkowskich `const`, ponieważ ich wartości nie mogą być obliczane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="cec0a-641">Jednakże deklarując je `static readonly` ma znacznie taki sam skutek.</span><span class="sxs-lookup"><span data-stu-id="cec0a-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="cec0a-642">Przechowywanie wersji stałych i statycznych pól tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="cec0a-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="cec0a-643">Stałe i tylko do odczytu pola mają różne semantyki wersji binarnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="cec0a-644">Gdy wyrażenie odwołuje się do stałej, wartość stałej jest uzyskiwana w czasie kompilacji, ale gdy wyrażenie odwołuje się do pola tylko do odczytu, wartość pola nie zostanie uzyskana do czasu wykonania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="cec0a-645">Rozważmy aplikację, która składa się z dwóch oddzielnych programów:</span><span class="sxs-lookup"><span data-stu-id="cec0a-645">Consider an application that consists of two separate programs:</span></span>
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

<span data-ttu-id="cec0a-646">Przestrzenie nazw `Program1` i `Program2` oznaczają dwa programy, które są kompilowane osobno.</span><span class="sxs-lookup"><span data-stu-id="cec0a-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="cec0a-647">Ponieważ `Program1.Utils.X` jest zadeklarowana jako statyczne pole tylko do odczytu, wartość wyjściowa instrukcji `Console.WriteLine` nie jest znana w czasie kompilacji, ale jest uzyskiwana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="cec0a-648">W takim przypadku, jeśli wartość `X` zostanie zmieniona i `Program1` zostanie ponownie skompilowana, instrukcja `Console.WriteLine` będzie wyprowadzać nową wartość nawet wtedy, gdy `Program2` nie zostanie ponownie skompilowana.</span><span class="sxs-lookup"><span data-stu-id="cec0a-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="cec0a-649">Jednak `X` była stałą, wartość `X` zostałaby uzyskana w czasie, `Program2` została skompilowana i pozostanie nienaruszona przez zmiany w `Program1` do momentu ponownego skompilowania `Program2`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="cec0a-650">Pola nietrwałe</span><span class="sxs-lookup"><span data-stu-id="cec0a-650">Volatile fields</span></span>

<span data-ttu-id="cec0a-651">Gdy *field_declaration* zawiera modyfikator `volatile`, pola wprowadzone przez tę deklarację są ***polami nietrwałymi***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="cec0a-652">W przypadku pól nietrwałych, techniki optymalizacji, które zmieniają kolejność instrukcji, mogą prowadzić do nieoczekiwanych i nieprzewidzianych wyników w programach wielowątkowych, które uzyskują dostęp do pól bez synchronizacji, takich jak zapewniane przez *lock_statement* ([instrukcja Lock](statements.md#the-lock-statement)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="cec0a-653">Optymalizacje mogą być wykonywane przez kompilator, przez system czasu wykonywania lub przez sprzęt.</span><span class="sxs-lookup"><span data-stu-id="cec0a-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="cec0a-654">W przypadku pól nietrwałych takie zmiany kolejności są ograniczone:</span><span class="sxs-lookup"><span data-stu-id="cec0a-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="cec0a-655">Odczyt pola nietrwałego jest nazywany ***nietrwałym odczytem***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="cec0a-656">Odczyt nietrwały ma "pozyskiwanie semantyki"; oznacza to, że jest to konieczne przed wszelkimi odwołaniami do pamięci, która następuje po niej w sekwencji instrukcji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="cec0a-657">Zapis pola nietrwałego jest nazywany ***zapisem nietrwałym***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="cec0a-658">Zapis nietrwały ma "semantykę wersji"; oznacza to, że ma to miejsce po dowolnych odwołaniach do pamięci przed instrukcją zapisu w sekwencji instrukcji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="cec0a-659">Te ograniczenia zapewniają, że wszystkie wątki będą obserwować nietrwałe zapisy wykonywane przez każdy inny wątek w kolejności, w której zostały wykonane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="cec0a-660">Implementacja zgodna z implementacją nie jest wymagana do zapewnienia pojedynczej kolejności zapisów nietrwałych w postaci zaobserwowanej ze wszystkich wątków wykonania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="cec0a-661">Typ pola nietrwałego musi mieć jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="cec0a-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="cec0a-662">*Reference_type*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-662">A *reference_type*.</span></span>
*  <span data-ttu-id="cec0a-663">Typ `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`lub `System.UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or `System.UIntPtr`.</span></span>
*  <span data-ttu-id="cec0a-664">*Enum_type* ma typ podstawowy wyliczenia `byte`, `sbyte`, `short`, `ushort`, `int`lub `uint`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="cec0a-665">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-665">The example</span></span>
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
<span data-ttu-id="cec0a-666">tworzy dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="cec0a-666">produces the output:</span></span>
```console
result = 143
```

<span data-ttu-id="cec0a-667">W tym przykładzie metoda `Main` uruchamia nowy wątek, w którym jest uruchamiana Metoda `Thread2`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="cec0a-668">Ta metoda przechowuje wartość w polu nietrwałym o nazwie `result`, a następnie przechowuje `true` w `finished`polu trwałym.</span><span class="sxs-lookup"><span data-stu-id="cec0a-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="cec0a-669">Główny wątek czeka na ustawienie pola `finished` na `true`, a następnie odczytuje `result`pola.</span><span class="sxs-lookup"><span data-stu-id="cec0a-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="cec0a-670">Ponieważ `finished` został zadeklarowany `volatile`, główny wątek musi odczytać wartość `143` z `result`pola.</span><span class="sxs-lookup"><span data-stu-id="cec0a-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="cec0a-671">Jeśli pole `finished` nie zostało zadeklarowane `volatile`, będzie dozwolone, aby Magazyn `result` widoczny dla głównego wątku po magazynie do `finished`i dlatego dla wątku głównego odczytać wartość `0` z pola `result`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="cec0a-672">Deklarowanie `finished` jako pola `volatile` uniemożliwia takie niespójność.</span><span class="sxs-lookup"><span data-stu-id="cec0a-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="cec0a-673">Inicjowanie pola</span><span class="sxs-lookup"><span data-stu-id="cec0a-673">Field initialization</span></span>

<span data-ttu-id="cec0a-674">Wartość początkowa pola, niezależnie od tego, czy jest to pole statyczne czy pole wystąpienia, jest wartością domyślną ([wartości domyślne](variables.md#default-values)) typu pola.</span><span class="sxs-lookup"><span data-stu-id="cec0a-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="cec0a-675">Nie jest możliwe przestrzeganie wartości pola przed wystąpieniem domyślnej inicjalizacji, a w takim przypadku pole nigdy nie jest "niezainicjowane".</span><span class="sxs-lookup"><span data-stu-id="cec0a-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="cec0a-676">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-676">The example</span></span>
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
<span data-ttu-id="cec0a-677">tworzy dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="cec0a-677">produces the output</span></span>
```console
b = False, i = 0
```
<span data-ttu-id="cec0a-678">ponieważ `b` i `i` są automatycznie inicjowane do wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="cec0a-679">Inicjatory zmiennych</span><span class="sxs-lookup"><span data-stu-id="cec0a-679">Variable initializers</span></span>

<span data-ttu-id="cec0a-680">Deklaracje pól mogą zawierać *variable_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="cec0a-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="cec0a-681">Dla pól statycznych, inicjatory zmiennych odpowiadają instrukcjom przypisania, które są wykonywane podczas inicjowania klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="cec0a-682">W przypadku pól wystąpienia zmienne inicjatory odpowiadają instrukcjom przypisania, które są wykonywane podczas tworzenia wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="cec0a-683">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-683">The example</span></span>
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
<span data-ttu-id="cec0a-684">tworzy dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="cec0a-684">produces the output</span></span>
```console
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="cec0a-685">Ponieważ przypisanie do `x` występuje, gdy inicjatory pola statycznego są wykonywane, a przypisania do `i` i `s` wystąpią, kiedy inicjator pola wystąpienia zostanie wykonany.</span><span class="sxs-lookup"><span data-stu-id="cec0a-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="cec0a-686">Inicjalizacja wartości domyślnej opisana w ramach [inicjalizacji pola](classes.md#field-initialization) występuje dla wszystkich pól, w tym pól, które mają inicjatory zmiennych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="cec0a-687">W tym przypadku po zainicjowaniu klasy wszystkie pola statyczne w tej klasie są najpierw inicjowane na wartości domyślne, a następnie inicjatory pola statycznego są wykonywane w kolejności tekstowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="cec0a-688">Podobnie, gdy tworzone jest wystąpienie klasy, wszystkie pola wystąpienia w tym wystąpieniu są najpierw inicjowane na wartości domyślne, a następnie inicjatory pola wystąpienia są wykonywane w kolejności tekstowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="cec0a-689">Istnieje możliwość, że pola statyczne z inicjatorami zmiennych mają być przestrzegane w stanie wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="cec0a-690">Jest to jednak zdecydowanie odradzane jako kwestia stylu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="cec0a-691">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-691">The example</span></span>
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
<span data-ttu-id="cec0a-692">wykazuje takie zachowanie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-692">exhibits this behavior.</span></span> <span data-ttu-id="cec0a-693">Pomimo definicji cyklicznych a i b, program jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="cec0a-694">Powoduje to wyjście</span><span class="sxs-lookup"><span data-stu-id="cec0a-694">It results in the output</span></span>
```console
a = 1, b = 2
```
<span data-ttu-id="cec0a-695">ponieważ pola statyczne `a` i `b` są inicjowane do `0` (wartość domyślna dla `int`) przed wykonaniem inicjatorów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="cec0a-696">Gdy inicjator `a` jest uruchomiony, wartość `b` jest równa zero, więc `a` jest inicjowana do `1`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="cec0a-697">Po uruchomieniu inicjatora `b`, wartość `a` jest już `1`, a więc `b` jest inicjowana do `2`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="cec0a-698">Inicjowanie pola statycznego</span><span class="sxs-lookup"><span data-stu-id="cec0a-698">Static field initialization</span></span>

<span data-ttu-id="cec0a-699">Inicjatory zmiennych pól statycznych klasy odpowiadają sekwencji przypisań, które są wykonywane w kolejności tekstowej, w której pojawiają się w deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="cec0a-700">Jeśli w klasie istnieje Konstruktor statyczny ([konstruktory statyczne](classes.md#static-constructors)), wykonywanie inicjatorów pól statycznych odbywa się bezpośrednio przed wykonaniem tego konstruktora statycznego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="cec0a-701">W przeciwnym razie inicjatory pola statycznego są wykonywane w czasie zależnym od implementacji przed pierwszym użyciem pola statycznego tej klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="cec0a-702">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-702">The example</span></span>
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="cec0a-703">może generować dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="cec0a-703">might produce either the output:</span></span>
```console
Init A
Init B
1 1
```
<span data-ttu-id="cec0a-704">lub dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="cec0a-704">or the output:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="cec0a-705">ponieważ wykonywanie inicjatora `X`i inicjatora `Y`może wystąpić w dowolnej kolejności; są one ograniczone tylko przed odwołaniami do tych pól.</span><span class="sxs-lookup"><span data-stu-id="cec0a-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="cec0a-706">Jednak w przykładzie:</span><span class="sxs-lookup"><span data-stu-id="cec0a-706">However, in the example:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="cec0a-707">dane wyjściowe muszą być następujące:</span><span class="sxs-lookup"><span data-stu-id="cec0a-707">the output must be:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="cec0a-708">ponieważ reguły, które są wykonywane przez konstruktory statyczne (zgodnie z definicją w [konstruktorach statycznych](classes.md#static-constructors)), zapewniają, że `B`Konstruktor statyczny (i dlatego, że `B`pola statyczne inicjatorów) muszą zostać uruchomione przed `A`statycznego konstruktora i pól.</span><span class="sxs-lookup"><span data-stu-id="cec0a-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="cec0a-709">Inicjowanie pola wystąpienia</span><span class="sxs-lookup"><span data-stu-id="cec0a-709">Instance field initialization</span></span>

<span data-ttu-id="cec0a-710">Inicjatory zmiennych pola wystąpienia klasy odpowiadają sekwencji przypisań, które są wykonywane natychmiast po wprowadzeniu do dowolnego jednego z konstruktorów wystąpień ([inicjatory konstruktorów](classes.md#constructor-initializers)) tej klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="cec0a-711">Inicjatory zmiennych są wykonywane w kolejności tekstowej, w której pojawiają się w deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="cec0a-712">Proces tworzenia i inicjowania wystąpienia klasy został szczegółowo opisany w [konstruktorach wystąpień](classes.md#instance-constructors).</span><span class="sxs-lookup"><span data-stu-id="cec0a-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="cec0a-713">Inicjator zmiennej dla pola wystąpienia nie może odwoływać się do tworzonego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="cec0a-714">W ten sposób jest to błąd czasu kompilacji, aby odwołać się do `this` w inicjatorze zmiennej, ponieważ jest to błąd czasu kompilacji dla inicjatora zmiennej, który odwołuje się do dowolnego elementu członkowskiego wystąpienia za pomocą *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="cec0a-715">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="cec0a-716">Inicjator zmiennej dla `y` powoduje błąd czasu kompilacji, ponieważ odwołuje się do elementu członkowskiego tworzonego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="cec0a-717">Metody</span><span class="sxs-lookup"><span data-stu-id="cec0a-717">Methods</span></span>

<span data-ttu-id="cec0a-718">***Metoda*** to element członkowski implementujący obliczenia lub akcję, które mogą być wykonywane przez obiekt lub klasę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="cec0a-719">Metody są deklarowane przy użyciu *method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cec0a-719">Methods are declared using *method_declaration*s:</span></span>

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="cec0a-720">*Method_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)) i prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `static` (Metoda[statyczna i wystąpienia](classes.md#static-and-instance-methods)), `virtual` ([metody wirtualne](classes.md#virtual-methods)), `override` ([metody zastępowania](classes.md#override-methods)), `sealed` ([zapieczętowane metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)) i `extern` (metody[zewnętrzne](classes.md#external-methods)) modyfikatory.</span><span class="sxs-lookup"><span data-stu-id="cec0a-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="cec0a-721">Deklaracja ma prawidłową kombinację modyfikatorów, jeśli spełnione są wszystkie następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="cec0a-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="cec0a-722">Deklaracja zawiera prawidłową kombinację modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="cec0a-723">Deklaracja nie zawiera tego samego modyfikatora wiele razy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="cec0a-724">Deklaracja zawiera co najwyżej jeden z następujących modyfikatorów: `static`, `virtual`i `override`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="cec0a-725">Deklaracja zawiera co najwyżej jeden z następujących modyfikatorów: `new` i `override`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="cec0a-726">Jeśli deklaracja zawiera modyfikator `abstract`, to Deklaracja nie zawiera żadnego z następujących modyfikatorów: `static`, `virtual`, `sealed` lub `extern`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="cec0a-727">Jeśli deklaracja zawiera modyfikator `private`, to Deklaracja nie zawiera żadnego z następujących modyfikatorów: `virtual`, `override`lub `abstract`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="cec0a-728">Jeśli deklaracja zawiera modyfikator `sealed`, wówczas deklaracja zawiera również modyfikator `override`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="cec0a-729">Jeśli deklaracja zawiera modyfikator `partial`, wówczas nie zawiera żadnego z następujących modyfikatorów: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`lub `extern`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="cec0a-730">Metoda, która ma modyfikator `async` jest funkcją asynchroniczną i jest zgodna z regułami opisanymi w [funkcjach asynchronicznych](classes.md#async-functions).</span><span class="sxs-lookup"><span data-stu-id="cec0a-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="cec0a-731">*Return_type* deklaracji metody Określa typ wartości obliczanej i zwracanej przez metodę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="cec0a-732">*Return_type* jest `void`, jeśli metoda nie zwraca wartości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="cec0a-733">Jeśli deklaracja zawiera modyfikator `partial`, zwracany typ musi być `void`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="cec0a-734">*MEMBER_NAME* określa nazwę metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="cec0a-735">O ile Metoda nie jest jawną implementacją składowej interfejsu ([jawne implementacje elementu członkowskiego interfejsu](interfaces.md#explicit-interface-member-implementations)), *MEMBER_NAME* jest po prostu *identyfikatorem*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="cec0a-736">W przypadku jawnej implementacji elementu członkowskiego interfejsu *MEMBER_NAME* składa się z *INTERFACE_TYPE* po którym następuje "`.`" i *Identyfikator*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="cec0a-737">Opcjonalne *type_parameter_list* określa parametry typu metody ([parametry typu](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="cec0a-738">Jeśli *type_parameter_list* jest określony, metoda jest ***metodą rodzajową***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="cec0a-739">Jeśli metoda ma modyfikator `extern`, nie można określić *type_parameter_list* .</span><span class="sxs-lookup"><span data-stu-id="cec0a-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="cec0a-740">Opcjonalne *formal_parameter_list* określa parametry metody ([Parametry metody](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="cec0a-741">Opcjonalne *type_parameter_constraints_clause*s określają ograniczenia dotyczące poszczególnych parametrów typu ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) i można je określić tylko w przypadku podania *type_parameter_list* również, a metoda nie ma modyfikatora `override`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="cec0a-742">*Return_type* i każdy z typów, do których odwołuje się *formal_parameter_list* metody, musi być co najmniej tak samo samo jak Metoda ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cec0a-743">*Method_body* jest średnikiem, ***treścią instrukcji*** lub ***treścią wyrażenia***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="cec0a-744">Treść instrukcji składa się z *bloku*, który określa instrukcje do wykonania, gdy metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="cec0a-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="cec0a-745">Treść wyrażenia składa się z `=>` po którym następuje *wyrażenie* i średnik, i oznacza pojedyncze wyrażenie do wykonania, gdy wywoływana jest metoda.</span><span class="sxs-lookup"><span data-stu-id="cec0a-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="cec0a-746">W przypadku metod `abstract` i `extern` *method_body* składa się po prostu z średnika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="cec0a-747">Dla `partial` metod, *method_body* może składać się z średnika, treści bloku lub treści wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="cec0a-748">Dla wszystkich innych metod *method_body* jest treścią bloku lub treścią wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="cec0a-749">Jeśli *method_body* składa się z średnika, wówczas deklaracja nie może zawierać modyfikatora `async`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="cec0a-750">Nazwa, lista parametrów typu i formalna lista parametrów metody definiują sygnaturę ([sygnatury i Przeciążenie](basic-concepts.md#signatures-and-overloading)) metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="cec0a-751">W każdym przypadku podpis metody składa się z jej nazwy, liczby parametrów typu oraz liczby, modyfikatorów i typów jego parametrów formalnych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="cec0a-752">W tym celu wszelkie parametry typu metody, która występuje w typie parametru formalnego, nie są identyfikowane przez jego nazwę, ale według pozycji porządkowej na liście argumentów typu metody. Zwracany typ nie jest częścią podpisu metody ani nie są nazwami parametrów typu lub parametrów formalnych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="cec0a-753">Nazwa metody musi różnić się od nazw wszystkich innych metod, które nie są zadeklarowane w tej samej klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="cec0a-754">Ponadto podpis metody musi różnić się od sygnatur wszystkich innych metod zadeklarowanych w tej samej klasie, a dwie metody zadeklarowane w tej samej klasie mogą nie mieć podpisów, które różnią się wyłącznie `ref` i `out`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="cec0a-755">*Type_parameter*s metody są w zakresie w *method_declaration*i mogą być używane do tworzenia typów w całym zakresie w *return_type*, *method_body*i *type_parameter_constraints_clause*s, ale nie w *atrybutach*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="cec0a-756">Wszystkie parametry formalne i parametry typu muszą mieć różne nazwy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="cec0a-757">Parametry metody</span><span class="sxs-lookup"><span data-stu-id="cec0a-757">Method parameters</span></span>

<span data-ttu-id="cec0a-758">Parametry metody, jeśli istnieje, są deklarowane przez *formal_parameter_list*metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

<span data-ttu-id="cec0a-759">Formalna lista parametrów składa się z jednego lub więcej parametrów oddzielonych przecinkami, których tylko ostatni może być *parameter_array*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="cec0a-760">*Fixed_parameter* składa się z opcjonalnego zestawu *atrybutów* ([atrybutów](attributes.md)), opcjonalnego modyfikatora `ref`, `out` lub `this`, *typu*, *identyfikatora* i opcjonalnego *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="cec0a-761">Każda *fixed_parameter* deklaruje parametr danego typu o podaną nazwę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="cec0a-762">Modyfikator `this` wyznacza metodę jako metodę rozszerzającą i jest dozwolony tylko dla pierwszego parametru metody statycznej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="cec0a-763">Metody rozszerzające są dokładniej opisane w [metodach rozszerzających](classes.md#extension-methods).</span><span class="sxs-lookup"><span data-stu-id="cec0a-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="cec0a-764">*Fixed_parameter* z *default_argument* jest znany jako ***parametr opcjonalny***, podczas gdy *fixed_parameter* bez *default_argument* jest ***wymaganym parametrem***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="cec0a-765">Wymagany parametr nie może występować po opcjonalnym parametrze w *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="cec0a-766">Parametr `ref` lub `out` nie może mieć *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="cec0a-767">*Wyrażenie* w *default_argument* musi mieć jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="cec0a-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="cec0a-768">*constant_expression*</span><span class="sxs-lookup"><span data-stu-id="cec0a-768">a *constant_expression*</span></span>
*  <span data-ttu-id="cec0a-769">wyrażenie `new S()`, gdzie `S` jest typem wartości</span><span class="sxs-lookup"><span data-stu-id="cec0a-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="cec0a-770">wyrażenie `default(S)`, gdzie `S` jest typem wartości</span><span class="sxs-lookup"><span data-stu-id="cec0a-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="cec0a-771">*Wyrażenie* musi być niejawnie konwertowane przez tożsamość lub konwersję dopuszczającą wartości null na typ parametru.</span><span class="sxs-lookup"><span data-stu-id="cec0a-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="cec0a-772">Jeśli parametry opcjonalne występują w deklaracji metody częściowej implementującej ([metody częściowe](classes.md#partial-methods)), Jawna implementacja składowej interfejsu ([jawne implementacje elementu członkowskiego interfejsu](interfaces.md#explicit-interface-member-implementations)) lub w jednoparametrowej deklaracji indeksatora ([indeksatory](classes.md#indexers)) kompilator powinien dać ostrzeżenie, ponieważ te elementy członkowskie nigdy nie mogą być wywoływane w sposób zezwalający na pominięcie argumentów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="cec0a-773">*Parameter_array* składa się z opcjonalnego zestawu *atrybutów* ([atrybutów](attributes.md)), modyfikator `params`, *array_type*i *Identyfikator*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="cec0a-774">Tablica parametrów deklaruje pojedynczy parametr danego typu tablicy o podaną nazwę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="cec0a-775">*Array_type* tablicy parametrów musi być typem tablicy jednowymiarowej ([typy tablicowe](arrays.md#array-types)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="cec0a-776">W wywołaniu metody, tablica parametrów zezwala na określenie pojedynczego argumentu danego typu tablicy lub dopuszcza zero lub więcej argumentów typu elementu tablicy, który ma zostać określony.</span><span class="sxs-lookup"><span data-stu-id="cec0a-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="cec0a-777">Tablice parametrów są szczegółowo opisane w [tablicach parametrów](classes.md#parameter-arrays).</span><span class="sxs-lookup"><span data-stu-id="cec0a-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="cec0a-778">*Parameter_array* może występować po opcjonalnym parametrze, ale nie może mieć wartości domyślnej — pominięcie argumentów dla *parameter_array* zamiast tego spowoduje utworzenie pustej tablicy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="cec0a-779">Poniższy przykład ilustruje różne rodzaje parametrów:</span><span class="sxs-lookup"><span data-stu-id="cec0a-779">The following example illustrates different kinds of parameters:</span></span>
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

<span data-ttu-id="cec0a-780">W *formal_parameter_list* dla `M`, `i` jest wymaganym parametrem ref, `d` jest wymaganym parametrem wartości, `b`, `s`, `o` i `t` są opcjonalnymi parametrami wartości, a `a` jest tablicą parametrów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="cec0a-781">Deklaracja metody tworzy oddzielne miejsce deklaracji dla parametrów, parametrów typu i zmiennych lokalnych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="cec0a-782">Nazwy są wprowadzane do tej przestrzeni deklaracji przez listę parametrów typu i formalną listę parametrów metody i według lokalnych deklaracji zmiennych w *bloku* metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="cec0a-783">Występuje błąd dla dwóch elementów członkowskich przestrzeni deklaracji metody, które mają taką samą nazwę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="cec0a-784">Jest to błąd dla przestrzeni deklaracji metody i przestrzeni lokalnej deklaracji zmiennej obszaru zagnieżdżonej deklaracji, która zawiera elementy o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="cec0a-785">Wywołanie metody ([wywołania metody](expressions.md#method-invocations)) tworzy kopię, specyficzną dla tego wywołania, parametrów formalnych i zmiennych lokalnych metody, a lista argumentów wywołania przypisuje wartości lub odwołania do zmiennych do nowo utworzonych parametrów formalnych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="cec0a-786">W *bloku* metody do parametrów formalnych można odwoływać się ich identyfikatory w wyrażeniach *simple_name* ([proste nazwy](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="cec0a-787">Istnieją cztery rodzaje parametrów formalnych:</span><span class="sxs-lookup"><span data-stu-id="cec0a-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="cec0a-788">Parametry wartości, które są zadeklarowane bez żadnych modyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="cec0a-789">Parametry odwołania, które są zadeklarowane za pomocą modyfikatora `ref`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="cec0a-790">Parametry wyjściowe, które są zadeklarowane za pomocą modyfikatora `out`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="cec0a-791">Tablice parametrów, które są zadeklarowane za pomocą modyfikatora `params`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="cec0a-792">Zgodnie z opisem w [podpisach i przeciążeniu](basic-concepts.md#signatures-and-overloading), modyfikatory `ref` i `out` są częścią podpisu metody, ale modyfikator `params` nie jest.</span><span class="sxs-lookup"><span data-stu-id="cec0a-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="cec0a-793">Parametry wartości</span><span class="sxs-lookup"><span data-stu-id="cec0a-793">Value parameters</span></span>

<span data-ttu-id="cec0a-794">Parametr zadeklarowany bez modyfikatorów jest parametrem wartości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="cec0a-795">Parametr value odnosi się do zmiennej lokalnej, która pobiera jej wartość początkową z odpowiedniego argumentu dostarczonego w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="cec0a-796">Gdy parametr formalny jest parametrem wartości, odpowiadający mu argument w wywołaniu metody musi być wyrażeniem, które jest niejawnie konwertowane ([konwersje niejawne](conversions.md#implicit-conversions)) na typ parametru formalnego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="cec0a-797">Metoda może przypisywać nowe wartości do parametru value.</span><span class="sxs-lookup"><span data-stu-id="cec0a-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="cec0a-798">Takie przypisania mają wpływ tylko na lokalizację magazynu lokalnego reprezentowane przez parametr value — nie mają wpływu na rzeczywisty argument określony w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="cec0a-799">Parametry odwołania</span><span class="sxs-lookup"><span data-stu-id="cec0a-799">Reference parameters</span></span>

<span data-ttu-id="cec0a-800">Parametr zadeklarowany za pomocą modyfikatora `ref` jest parametrem referencyjnym.</span><span class="sxs-lookup"><span data-stu-id="cec0a-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="cec0a-801">W przeciwieństwie do parametru wartości, parametr odwołania nie tworzy nowej lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="cec0a-802">Zamiast tego parametr odwołania reprezentuje tę samą lokalizację magazynu, co zmienna określona jako argument w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="cec0a-803">Gdy parametr formalny jest parametrem referencyjnym, odpowiadający mu argument w wywołaniu metody musi zawierać słowo kluczowe `ref`, a następnie *variable_reference* ([precyzyjne reguły określania przypisania](variables.md#precise-rules-for-determining-definite-assignment), które mają być określone) tego samego typu co parametr formalny.</span><span class="sxs-lookup"><span data-stu-id="cec0a-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="cec0a-804">Zmienna musi być ostatecznie przypisana, zanim będzie mogła zostać przeniesiona jako parametr referencyjny.</span><span class="sxs-lookup"><span data-stu-id="cec0a-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="cec0a-805">W ramach metody parametr odwołania jest zawsze uznawany za ostatecznie przypisany.</span><span class="sxs-lookup"><span data-stu-id="cec0a-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="cec0a-806">Metoda zadeklarowana jako iterator ([Iteratory](classes.md#iterators)) nie może mieć parametrów referencyjnych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="cec0a-807">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-807">The example</span></span>
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
<span data-ttu-id="cec0a-808">tworzy dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="cec0a-808">produces the output</span></span>
```console
i = 2, j = 1
```

<span data-ttu-id="cec0a-809">W przypadku wywołania `Swap` w `Main``x` reprezentuje `i` i `y` reprezentuje `j`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="cec0a-810">W efekcie wywołanie ma wpływ na zamianę wartości `i` i `j`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="cec0a-811">W metodzie, która pobiera parametry referencyjne, istnieje możliwość, że wiele nazw reprezentuje tę samą lokalizację magazynu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="cec0a-812">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-812">In the example</span></span>
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
<span data-ttu-id="cec0a-813">Wywołanie `F` w `G` przekazuje odwołanie do `s` dla `a` i `b`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="cec0a-814">W takim przypadku nazwy `s`, `a`i `b` wszystkie odnoszą się do tej samej lokalizacji przechowywania, a wszystkie trzy przydziały modyfikują pole wystąpienia `s`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="cec0a-815">Parametry wyjściowe</span><span class="sxs-lookup"><span data-stu-id="cec0a-815">Output parameters</span></span>

<span data-ttu-id="cec0a-816">Parametr zadeklarowany za pomocą modyfikatora `out` jest parametrem wyjściowym.</span><span class="sxs-lookup"><span data-stu-id="cec0a-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="cec0a-817">Podobnie jak w przypadku parametru Reference, parametr wyjściowy nie tworzy nowej lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="cec0a-818">Zamiast tego parametr wyjściowy reprezentuje taką samą lokalizację przechowywania jak zmienna określona jako argument w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="cec0a-819">Gdy parametr formalny jest parametrem wyjściowym, odpowiadający mu argument w wywołaniu metody musi zawierać słowo kluczowe `out`, a następnie *variable_reference* ([precyzyjne reguły określania przypisania](variables.md#precise-rules-for-determining-definite-assignment), które mają być określone) tego samego typu co parametr formalny.</span><span class="sxs-lookup"><span data-stu-id="cec0a-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="cec0a-820">Zmienna nie musi być ostatecznie przypisana, zanim będzie mogła zostać przeniesiona jako parametr wyjściowy, ale po wywołaniu, gdzie zmienna została przeniesiona jako parametr wyjściowy, zmienna jest uważana za ostatecznie przypisaną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="cec0a-821">W ramach metody, podobnie jak zmienna lokalna, parametr wyjściowy jest początkowo uznawany za nieprzypisany i musi być ostatecznie przypisany przed użyciem wartości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="cec0a-822">Każdy parametr wyjściowy metody musi być ostatecznie przypisany przed zwróceniem metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="cec0a-823">Metoda zadeklarowana jako metoda częściowa ([metody częściowe](classes.md#partial-methods)) lub iterator ([Iteratory](classes.md#iterators)) nie może mieć parametrów wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="cec0a-824">Parametry wyjściowe są zwykle używane w metodach, które generują wiele wartości zwracanych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="cec0a-825">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-825">For example:</span></span>
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

<span data-ttu-id="cec0a-826">Przykład generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="cec0a-826">The example produces the output:</span></span>
```console
c:\Windows\System\
hello.txt
```

<span data-ttu-id="cec0a-827">Należy pamiętać, że zmienne `dir` i `name` mogą być nieprzypisane przed przekazaniem do `SplitPath`i że są uznawane za ostatecznie przypisane po wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="cec0a-828">Tablice parametrów</span><span class="sxs-lookup"><span data-stu-id="cec0a-828">Parameter arrays</span></span>

<span data-ttu-id="cec0a-829">Parametr zadeklarowany za pomocą modyfikatora `params` jest tablicą parametrów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="cec0a-830">Jeśli lista parametrów formalnych zawiera tablicę parametrów, musi być ostatnim parametrem na liście i musi być typu tablicy jednowymiarowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="cec0a-831">Na przykład typy `string[]` i `string[][]` mogą być używane jako typ tablicy parametrów, ale `string[,]` typu nie może.</span><span class="sxs-lookup"><span data-stu-id="cec0a-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="cec0a-832">Nie można połączyć modyfikatora `params` z modyfikatorami `ref` i `out`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="cec0a-833">Tablica parametrów zezwala na określenie argumentów na jeden z dwóch sposobów w wywołaniu metody:</span><span class="sxs-lookup"><span data-stu-id="cec0a-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="cec0a-834">Argument określony dla tablicy parametrów może być pojedynczym wyrażeniem, które jest niejawnie konwertowane ([konwersje niejawne](conversions.md#implicit-conversions)) na typ tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="cec0a-835">W takim przypadku tablica parametrów działa dokładnie tak, jak parametr value.</span><span class="sxs-lookup"><span data-stu-id="cec0a-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="cec0a-836">Alternatywnie, wywołanie może określać zero lub więcej argumentów tablicy parametrów, gdzie każdy argument jest wyrażeniem, które jest niejawnie konwertowane ([konwersje niejawne](conversions.md#implicit-conversions)) na typ elementu tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="cec0a-837">W takim przypadku wywołanie tworzy wystąpienie typu tablicy parametrów o długości odpowiadającej liczbie argumentów, inicjuje elementy instancji Array z podaną wartością argumentów i używa nowo utworzonego wystąpienia tablicy jako wartości rzeczywistej argument.</span><span class="sxs-lookup"><span data-stu-id="cec0a-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="cec0a-838">Oprócz dopuszczania zmiennej liczby argumentów w wywołaniu, tablica parametrów jest dokładnie równoważna z parametrem wartości ([Parametry wartości](classes.md#value-parameters)) tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="cec0a-839">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-839">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
<span data-ttu-id="cec0a-840">tworzy dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="cec0a-840">produces the output</span></span>
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="cec0a-841">Pierwsze wywołanie `F` po prostu przekazuje tablicę `a` jako parametr wartości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="cec0a-842">Drugie wywołanie `F` automatycznie tworzy cztery elementy `int[]` z podaną wartością elementu i przekazuje to wystąpienie tablicy jako parametr wartości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="cec0a-843">Podobnie trzecie wywołanie `F` powoduje utworzenie elementu zero `int[]` i przekazanie tego wystąpienia jako parametru wartości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="cec0a-844">Drugie i trzecie wywołania są dokładnie równoważne zapisowi:</span><span class="sxs-lookup"><span data-stu-id="cec0a-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="cec0a-845">Podczas rozpoznawania przeciążenia metoda z tablicą parametrów może być stosowana w jego normalnej postaci lub w rozwiniętej formie ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="cec0a-846">Rozwinięta forma metody jest dostępna tylko wtedy, gdy normalna forma metody nie ma zastosowania i tylko wtedy, gdy odpowiednia metoda o tej samej sygnaturze, co rozwinięta forma, nie jest już zadeklarowana w tym samym typie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="cec0a-847">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-847">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
<span data-ttu-id="cec0a-848">tworzy dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="cec0a-848">produces the output</span></span>
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="cec0a-849">W przykładzie dwa z możliwych rozwiniętych formularzy metody z tablicą parametrów są już zawarte w klasie jako metody zwykłe.</span><span class="sxs-lookup"><span data-stu-id="cec0a-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="cec0a-850">Te rozwinięte formularze nie są więc brane pod uwagę podczas rozpoznawania przeciążenia, a pierwsze i trzecie wywołania metody wybierają metody zwykłe.</span><span class="sxs-lookup"><span data-stu-id="cec0a-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="cec0a-851">Gdy Klasa deklaruje metodę z tablicą parametrów, nierzadko jest również zawierać część rozwiniętych formularzy jako zwykłe metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="cec0a-852">Dzięki temu można uniknąć alokacji wystąpienia tablicy, które występuje, gdy zostanie wywołana rozwinięta forma metody z tablicą parametrów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="cec0a-853">Gdy typ tablicy parametrów jest `object[]`, potencjalna niejednoznaczność występuje między normalną formą metody a wykorzystaną formą dla jednego parametru `object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="cec0a-854">Przyczyna niejednoznaczności polega na tym, że `object[]` jest sama niejawnie przekonwertowana na typ `object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="cec0a-855">Niejednoznaczność nie ma jednak żadnego problemu, ponieważ można ją rozwiązać, wstawiając Rzutowanie w razie konieczności.</span><span class="sxs-lookup"><span data-stu-id="cec0a-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="cec0a-856">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-856">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
<span data-ttu-id="cec0a-857">tworzy dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="cec0a-857">produces the output</span></span>
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="cec0a-858">W pierwszym i ostatnim wywołaniu `F`jest stosowana normalna forma `F`, ponieważ istnieje niejawna konwersja z typu argumentu na typ parametru (obie są typu `object[]`).</span><span class="sxs-lookup"><span data-stu-id="cec0a-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="cec0a-859">W rezultacie rozwiązanie przeciążania wybiera normalną postać `F`i argument jest przenoszona jako parametr wartości regularnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="cec0a-860">W drugim i trzecim wywołaniu, normalna forma `F` nie ma zastosowania, ponieważ nie istnieje niejawna konwersja z typu argumentu na typ parametru (typ `object` nie może być niejawnie konwertowany na typ `object[]`).</span><span class="sxs-lookup"><span data-stu-id="cec0a-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="cec0a-861">Jednakże rozwinięta forma `F` ma zastosowanie, więc jest wybierana przez metodę rozpoznawania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="cec0a-862">W związku z tym `object[]` jest tworzony przez wywołanie, a pojedynczy element tablicy jest inicjowany z daną wartością argumentu (która sama jest odwołaniem do `object[]`).</span><span class="sxs-lookup"><span data-stu-id="cec0a-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="cec0a-863">Metody static i instance</span><span class="sxs-lookup"><span data-stu-id="cec0a-863">Static and instance methods</span></span>

<span data-ttu-id="cec0a-864">Gdy deklaracja metody zawiera modyfikator `static`, ta metoda jest określana jako metoda statyczna.</span><span class="sxs-lookup"><span data-stu-id="cec0a-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="cec0a-865">Gdy nie ma modyfikatora `static`, metoda jest uznawana za metodę wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="cec0a-866">Metoda statyczna nie działa w określonym wystąpieniu i jest błędem czasu kompilacji, aby odwołać się do `this` w metodzie statycznej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="cec0a-867">Metoda wystąpienia działa w danym wystąpieniu klasy i można uzyskać do niego dostęp jako `this` ([ten dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="cec0a-868">Gdy metoda jest przywoływana w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `E.M`, jeśli `M` jest metodą statyczną, `E` musi zwrócić uwagę na typ zawierający `M`, a jeśli `M` jest metodą wystąpienia, `E` musi zwrócić uwagę na wystąpienie typu zawierającego `M`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="cec0a-869">Różnice między elementami statycznymi a składowymi wystąpienia są omówione bardziej szczegółowo w [elementach członkowskich static i instance](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="cec0a-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="cec0a-870">Metody wirtualne</span><span class="sxs-lookup"><span data-stu-id="cec0a-870">Virtual methods</span></span>

<span data-ttu-id="cec0a-871">Gdy deklaracja metody wystąpienia zawiera modyfikator `virtual`, ta metoda jest określana jako metoda wirtualna.</span><span class="sxs-lookup"><span data-stu-id="cec0a-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="cec0a-872">Gdy modyfikator `virtual` nie jest obecny, metoda jest uznawana za metodę niewirtualną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="cec0a-873">Implementacja metody niewirtualnej jest niezmienna: implementacja jest taka sama, niezależnie od tego, czy metoda jest wywoływana w wystąpieniu klasy, w której jest zadeklarowana, czy też wystąpieniem klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="cec0a-874">W przeciwieństwie do implementacji metody wirtualnej można zastąpić klasy pochodne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="cec0a-875">Proces zastępowania implementacji dziedziczonej metody wirtualnej jest znany jako ***zastępowanie*** tej metody ([metody zastępowania](classes.md#override-methods)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="cec0a-876">W wywołaniu metody wirtualnej, ***Typ czasu wykonywania*** wystąpienia, dla którego odbywa się wywołanie określa rzeczywistą implementację metody do wywołania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="cec0a-877">W wywołaniu metody niewirtualnej ***Typ czasu kompilacji*** wystąpienia jest czynnikiem decydującym.</span><span class="sxs-lookup"><span data-stu-id="cec0a-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="cec0a-878">W precyzyjnej terminologii, gdy metoda o nazwie `N` jest wywoływana z listą argumentów `A` w wystąpieniu z typem czasu kompilacji `C` i typem czasu wykonywania `R` (gdzie `R` jest `C` lub Klasa pochodna `C`), wywołanie jest przetwarzane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cec0a-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="cec0a-879">Najpierw rozwiązanie przeciążenia jest stosowane do `C`, `N`i `A`w celu wybrania konkretnej metody `M` z zestawu metod zadeklarowanych w i dziedziczonych przez `C`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="cec0a-880">Jest to opisane w [wywołaniu metody](expressions.md#method-invocations).</span><span class="sxs-lookup"><span data-stu-id="cec0a-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="cec0a-881">Następnie Jeśli `M` jest metodą niewirtualną, `M` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="cec0a-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="cec0a-882">W przeciwnym razie `M` jest metodą wirtualną i zostanie wywołana najbardziej pochodna implementacja `M` w odniesieniu do `R`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="cec0a-883">Dla każdej metody wirtualnej zadeklarowanej w lub dziedziczonej przez klasę istnieje ***najbardziej pochodna implementacja*** metody w odniesieniu do tej klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="cec0a-884">Najbardziej pochodna implementacja metody wirtualnej `M` w odniesieniu do klasy `R` jest określana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cec0a-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="cec0a-885">Jeśli `R` zawiera `virtual` deklaracji `M`, to najbardziej pochodna implementacja `M`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="cec0a-886">W przeciwnym razie, jeśli `R` zawiera `override` `M`, to najbardziej pochodna implementacja `M`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="cec0a-887">W przeciwnym razie najbardziej pochodna implementacja `M` w odniesieniu do `R` jest taka sama jak w przypadku najbardziej pochodnej implementacji `M` w odniesieniu do bezpośredniej klasy podstawowej `R`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="cec0a-888">Poniższy przykład ilustruje różnice między metodami wirtualnymi i niewirtualnymi:</span><span class="sxs-lookup"><span data-stu-id="cec0a-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

<span data-ttu-id="cec0a-889">W przykładzie `A` wprowadza metodę niewirtualną `F` i `G`metodę wirtualną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="cec0a-890">Klasa `B` wprowadza nową metodę niewirtualną `F`, ukrywając dziedziczone `F`, a także przesłania metodę dziedziczenia `G`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="cec0a-891">Przykład generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="cec0a-891">The example produces the output:</span></span>
```console
A.F
B.F
B.G
B.G
```

<span data-ttu-id="cec0a-892">Zwróć uwagę, że instrukcja `a.G()` wywołuje `B.G`, a nie `A.G`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="cec0a-893">Wynika to z faktu, że typ czasu wykonywania wystąpienia (czyli `B`), a nie typ czasu kompilacji wystąpienia (czyli `A`), określa rzeczywistą implementację metody do wywołania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="cec0a-894">Ponieważ metody są dozwolone do ukrycia metod dziedziczonych, istnieje możliwość, że Klasa zawiera kilka metod wirtualnych o tej samej sygnaturze.</span><span class="sxs-lookup"><span data-stu-id="cec0a-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="cec0a-895">Nie jest to problem niejednoznaczny, ponieważ wszystkie z nich oprócz metoda pochodna są ukryte.</span><span class="sxs-lookup"><span data-stu-id="cec0a-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="cec0a-896">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-896">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
<span data-ttu-id="cec0a-897">klasy `C` i `D` zawierają dwie metody wirtualne o tej samej sygnaturze: wprowadzoną przez `A` i wprowadzoną przez `C`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="cec0a-898">Metoda wprowadzona przez `C` ukrywa metodę dziedziczoną z `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="cec0a-899">W związku z tym, deklaracja przesłonięcia w `D` przesłania metodę wprowadzoną przez `C`i nie jest możliwe, aby `D` przesłonić metodę wprowadzoną przez `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="cec0a-900">Przykład generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="cec0a-900">The example produces the output:</span></span>
```console
B.F
B.F
D.F
D.F
```

<span data-ttu-id="cec0a-901">Należy zauważyć, że można wywołać ukrytą metodę wirtualną, uzyskując dostęp do wystąpienia `D` za pomocą mniej pochodnego typu, w którym metoda nie jest ukryta.</span><span class="sxs-lookup"><span data-stu-id="cec0a-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="cec0a-902">Metody przesłonięcia</span><span class="sxs-lookup"><span data-stu-id="cec0a-902">Override methods</span></span>

<span data-ttu-id="cec0a-903">Gdy deklaracja metody wystąpienia zawiera modyfikator `override`, metoda jest określana jako ***Metoda przesłonięcia***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="cec0a-904">Metoda przesłonięcia zastępuje dziedziczoną metodę wirtualną z tą samą sygnaturą.</span><span class="sxs-lookup"><span data-stu-id="cec0a-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="cec0a-905">Podczas gdy deklaracja metody wirtualnej wprowadza nową metodę, Deklaracja metody przesłonięcia specjalizacji istniejącej dziedziczonej metody wirtualnej, dostarczając nową implementację tej metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="cec0a-906">Metoda zastąpiona przez deklarację `override` jest znana jako ***zastąpiona metoda podstawowa***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="cec0a-907">Dla metody przesłonięcia `M` zadeklarowanej w klasie `C`, zastąpiona metoda bazowa jest określana przez badanie każdego typu klasy bazowej `C`, rozpoczynając od bezpośredniego typu klasy bazowej `C` i kontynuując każdy kolejny bezpośredni typ klasy bazowej, do momentu w danym typie klasy bazowej, który ma taką samą sygnaturę jak `M` po podstawieniu argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="cec0a-908">Na potrzeby lokalizowania zastąpionej metody bazowej Metoda jest uważana za dostępną, jeśli jest `public`, jeśli jest `protected`, jeśli jest `protected internal`lub jeśli jest `internal` i zadeklarowana w tym samym programie jako `C`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="cec0a-909">Błąd czasu kompilacji występuje, chyba że wszystkie poniższe warunki są spełnione dla deklaracji przesłonięcia:</span><span class="sxs-lookup"><span data-stu-id="cec0a-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="cec0a-910">Zastąpiona metoda bazowa może być zlokalizowana zgodnie z powyższym opisem.</span><span class="sxs-lookup"><span data-stu-id="cec0a-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="cec0a-911">Istnieje dokładnie jedna taka zastąpiona metoda bazowa.</span><span class="sxs-lookup"><span data-stu-id="cec0a-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="cec0a-912">To ograniczenie ma wpływ tylko wtedy, gdy typ klasy bazowej jest typem skonstruowanym, w którym Podstawienie argumentów typu powoduje, że sygnatura dwóch metod jest taka sama.</span><span class="sxs-lookup"><span data-stu-id="cec0a-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="cec0a-913">Zastąpiona metoda bazowa to metoda wirtualna, abstrakcyjna lub zastępowania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="cec0a-914">Inaczej mówiąc, zastąpiona metoda bazowa nie może być statyczna ani niewirtualna.</span><span class="sxs-lookup"><span data-stu-id="cec0a-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="cec0a-915">Zastąpiona metoda bazowa nie jest metodą zapieczętowana.</span><span class="sxs-lookup"><span data-stu-id="cec0a-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="cec0a-916">Metoda override i zastąpiona metoda bazowa mają ten sam typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="cec0a-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="cec0a-917">Deklaracja przesłonięcia i zastąpiona metoda bazowa mają takie same zadeklarowane ułatwienia dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="cec0a-918">Inaczej mówiąc, deklaracja przesłonięcia nie może zmienić dostępności metody wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="cec0a-919">Jednakże jeśli zastąpiona metoda bazowa jest chroniona wewnętrznie i jest zadeklarowana w innym zestawie niż zestaw zawierający metodę przesłonięcia, zadeklarowana dostępność metody override musi być chroniona.</span><span class="sxs-lookup"><span data-stu-id="cec0a-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="cec0a-920">Deklaracja przesłonięcia nie określa klauzul typu-Parameter-Constraints.</span><span class="sxs-lookup"><span data-stu-id="cec0a-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="cec0a-921">Zamiast tego ograniczenia są dziedziczone z przesłoniętej metody podstawowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="cec0a-922">Należy zauważyć, że ograniczenia, które są parametrami typu w zastąpionej metodzie, mogą zostać zastąpione przez argumenty typu w dziedziczonym ograniczeniu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="cec0a-923">Może to prowadzić do ograniczeń, które nie są dozwolone, gdy jawnie określono, takich jak typy wartości lub typy zapieczętowane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="cec0a-924">W poniższym przykładzie pokazano, w jaki sposób zastępujące reguły działają dla klas ogólnych:</span><span class="sxs-lookup"><span data-stu-id="cec0a-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

<span data-ttu-id="cec0a-925">Deklaracja przesłonięcia może uzyskać dostęp do zastąpionej metody bazowej przy użyciu *base_access* ([dostęp podstawowy](expressions.md#base-access)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="cec0a-926">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-926">In the example</span></span>
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
<span data-ttu-id="cec0a-927">Wywołanie `base.PrintFields()` w `B` wywołuje metodę `PrintFields` zadeklarowaną w `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="cec0a-928">*Base_access* wyłącza mechanizm wywoływania wirtualnego i po prostu traktuje metodę podstawową jako metodę niewirtualną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="cec0a-929">Miało to, że w `B` został zapisany `((A)this).PrintFields()`, spowoduje cykliczne wywołanie metody `PrintFields` zadeklarowanej w `B`, a nie zadeklarowanej w `A`, ponieważ `PrintFields` jest wirtualną, a typem czasu wykonywania `((A)this)` jest `B`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="cec0a-930">Tylko poprzez dołączenie modyfikatora `override` może zastąpić inną metodę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="cec0a-931">We wszystkich innych przypadkach Metoda o tej samej sygnaturze, jako dziedziczona metoda po prostu ukrywa metodę dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="cec0a-932">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-932">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
<span data-ttu-id="cec0a-933">Metoda `F` w `B` nie zawiera modyfikatora `override` i w związku z tym nie przesłania metody `F` w `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="cec0a-934">Zamiast tego Metoda `F` w `B` ukrywa metodę w `A`i zostanie zgłoszone ostrzeżenie, ponieważ deklaracja nie zawiera modyfikatora `new`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="cec0a-935">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-935">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
<span data-ttu-id="cec0a-936">Metoda `F` w `B` powoduje ukrycie metody `F` wirtualnej dziedziczonej z `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="cec0a-937">Ponieważ nowy `F` w `B` ma dostęp prywatny, jego zakres zawiera tylko treść klasy `B` i nie rozszerzy się do `C`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="cec0a-938">W związku z tym, deklaracja `F` w `C` może przesłonić `F` dziedziczone z `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="cec0a-939">Zapieczętowane metody</span><span class="sxs-lookup"><span data-stu-id="cec0a-939">Sealed methods</span></span>

<span data-ttu-id="cec0a-940">Gdy deklaracja metody wystąpienia zawiera modyfikator `sealed`, ta metoda jest określana jako ***Metoda zapieczętowana***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="cec0a-941">Jeśli deklaracja metody wystąpienia zawiera modyfikator `sealed`, musi również zawierać modyfikator `override`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="cec0a-942">Użycie modyfikatora `sealed` zapobiega dalszemu zastępowaniu metody przez klasę pochodną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="cec0a-943">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-943">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
<span data-ttu-id="cec0a-944">Klasa `B` udostępnia dwie metody przesłaniania: metodę `F`, która ma modyfikator `sealed` i metodę `G`, która nie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="cec0a-945">Użycie zapieczętowanych `modifier` `B`uniemożliwia `C` dalsze przesłanianie `F`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="cec0a-946">Metody abstrakcyjne</span><span class="sxs-lookup"><span data-stu-id="cec0a-946">Abstract methods</span></span>

<span data-ttu-id="cec0a-947">Gdy deklaracja metody wystąpienia zawiera modyfikator `abstract`, ta metoda jest uznawana za ***metodę abstrakcyjną***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="cec0a-948">Chociaż metoda abstrakcyjna jest niejawnie również metodą wirtualną, nie może mieć modyfikatora `virtual`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="cec0a-949">Deklaracja metody abstrakcyjnej wprowadza nową metodę wirtualną, ale nie zapewnia implementacji tej metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="cec0a-950">Zamiast tego nieabstrakcyjne klasy pochodne są wymagane do zapewnienia własnych implementacji przez zastąpienie tej metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="cec0a-951">Ponieważ metoda abstrakcyjna nie zapewnia rzeczywistej implementacji, *method_body* metody abstrakcyjnej składa się z średnika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="cec0a-952">Deklaracje metody abstrakcyjnej są dozwolone tylko w klasach abstrakcyjnych ([klasach abstrakcyjnych](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="cec0a-953">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-953">In the example</span></span>
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
<span data-ttu-id="cec0a-954">Klasa `Shape` definiuje abstrakcyjne pojęcie obiektu kształtu geometrycznego, który może narysować sam siebie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="cec0a-955">Metoda `Paint` jest abstrakcyjna, ponieważ nie ma żadnej znaczącej implementacji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="cec0a-956">Klasy `Ellipse` i `Box` to konkretne implementacje `Shape`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="cec0a-957">Ponieważ te klasy nie są abstrakcyjne, są one wymagane do przesłonięcia metody `Paint` i zapewnienia rzeczywistej implementacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="cec0a-958">Jest to błąd czasu kompilacji dla *base_access* ([dostęp podstawowy](expressions.md#base-access)), aby odwołać się do metody abstrakcyjnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="cec0a-959">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-959">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
<span data-ttu-id="cec0a-960">zgłoszono błąd czasu kompilacji dla wywołania `base.F()`, ponieważ odwołuje się do metody abstrakcyjnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="cec0a-961">Deklaracja metody abstrakcyjnej może przesłonić metodę wirtualną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="cec0a-962">Dzięki temu Klasa abstrakcyjna może wymusić ponowną implementację metody w klasach pochodnych i powoduje, że oryginalna implementacja metody jest niedostępna.</span><span class="sxs-lookup"><span data-stu-id="cec0a-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="cec0a-963">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-963">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
<span data-ttu-id="cec0a-964">Klasa `A` deklaruje metodę wirtualną, Klasa `B` przesłania tę metodę za pomocą metody abstrakcyjnej, a Klasa `C` przesłania metodę abstrakcyjną w celu zapewnienia własnej implementacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="cec0a-965">Metody zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="cec0a-965">External methods</span></span>

<span data-ttu-id="cec0a-966">Gdy deklaracja metody zawiera modyfikator `extern`, ta metoda jest określana jako ***Metoda zewnętrzna***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="cec0a-967">Metody zewnętrzne są implementowane zewnętrznie, zazwyczaj przy użyciu języka innego C#niż.</span><span class="sxs-lookup"><span data-stu-id="cec0a-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="cec0a-968">Ponieważ Deklaracja metody zewnętrznej nie zapewnia żadnej rzeczywistej implementacji, *method_body* metody zewnętrznej składa się z średnika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="cec0a-969">Metoda zewnętrzna nie może być rodzajowa.</span><span class="sxs-lookup"><span data-stu-id="cec0a-969">An external method may not be generic.</span></span>

<span data-ttu-id="cec0a-970">Modyfikator `extern` jest zwykle używany w połączeniu z atrybutem `DllImport` ([współdziałaniem ze składnikami com i Win32](attributes.md#interoperation-with-com-and-win32-components)), umożliwiając Implementowanie zewnętrznych metod przy użyciu bibliotek DLL (bibliotek dołączanych dynamicznie).</span><span class="sxs-lookup"><span data-stu-id="cec0a-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="cec0a-971">Środowisko wykonawcze może obsługiwać inne mechanizmy, w których można dostarczyć implementacje metod zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="cec0a-972">Gdy metoda zewnętrzna zawiera atrybut `DllImport`, Deklaracja metody musi zawierać również modyfikator `static`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="cec0a-973">Ten przykład ilustruje użycie modyfikatora `extern` i atrybutu `DllImport`:</span><span class="sxs-lookup"><span data-stu-id="cec0a-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a><span data-ttu-id="cec0a-974">Metody częściowe (podsumowanie)</span><span class="sxs-lookup"><span data-stu-id="cec0a-974">Partial methods (recap)</span></span>

<span data-ttu-id="cec0a-975">Gdy deklaracja metody zawiera modyfikator `partial`, ta metoda jest uznawana za ***metodę częściową***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="cec0a-976">Metody częściowe mogą być deklarowane tylko jako elementy członkowskie typów częściowych ([typy częściowe](classes.md#partial-types)) i podlegają wielu ograniczeniom.</span><span class="sxs-lookup"><span data-stu-id="cec0a-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="cec0a-977">Metody częściowe są szczegółowo opisane w [metodach częściowych](classes.md#partial-methods).</span><span class="sxs-lookup"><span data-stu-id="cec0a-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="cec0a-978">Metody rozszerzające</span><span class="sxs-lookup"><span data-stu-id="cec0a-978">Extension methods</span></span>

<span data-ttu-id="cec0a-979">Gdy pierwszy parametr metody zawiera modyfikator `this`, ta metoda jest określana jako ***Metoda rozszerzenia***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="cec0a-980">Metody rozszerzające mogą być deklarowane tylko w nieogólnych, niezagnieżdżonych klasach statycznych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="cec0a-981">Pierwszy parametr metody rozszerzenia nie może mieć modyfikatorów innych niż `this`, a typ parametru nie może być typem wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="cec0a-982">Poniżej znajduje się przykład klasy statycznej, która deklaruje dwie metody rozszerzające:</span><span class="sxs-lookup"><span data-stu-id="cec0a-982">The following is an example of a static class that declares two extension methods:</span></span>
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

<span data-ttu-id="cec0a-983">Metoda rozszerzenia to zwykła metoda statyczna.</span><span class="sxs-lookup"><span data-stu-id="cec0a-983">An extension method is a regular static method.</span></span> <span data-ttu-id="cec0a-984">Ponadto, gdy jej otaczająca Klasa statyczna znajduje się w zakresie, Metoda rozszerzenia może być wywoływana przy użyciu składni wywołania metody wystąpienia ([wywołania metody rozszerzenia](expressions.md#extension-method-invocations)), przy użyciu wyrażenia odbiorcy jako pierwszego argumentu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="cec0a-985">Następujący program używa metod rozszerzających zadeklarowanych powyżej:</span><span class="sxs-lookup"><span data-stu-id="cec0a-985">The following program uses the extension methods declared above:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

<span data-ttu-id="cec0a-986">Metoda `Slice` jest dostępna na `string[]`, a metoda `ToInt32` jest dostępna na `string`, ponieważ zostały zadeklarowane jako metody rozszerzania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="cec0a-987">Znaczenie programu jest takie samo jak w przypadku następujących wywołań metod statycznych:</span><span class="sxs-lookup"><span data-stu-id="cec0a-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a><span data-ttu-id="cec0a-988">Treść metody</span><span class="sxs-lookup"><span data-stu-id="cec0a-988">Method body</span></span>

<span data-ttu-id="cec0a-989">*Method_body* deklaracji metody składa się z treści bloku, treści wyrażenia lub średnika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="cec0a-990">***Typ wyniku*** metody jest `void`, jeśli typem zwracanym jest `void`, lub jeśli metoda jest Async i typem zwracanym jest `System.Threading.Tasks.Task`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="cec0a-991">W przeciwnym razie typ wyniku metody nieasynchronicznej jest jego typem zwracanym, a typ wyniku metody asynchronicznej z typem zwracanym `System.Threading.Tasks.Task<T>` jest `T`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="cec0a-992">Gdy metoda ma `void` typ wyniku i treść bloku, instrukcje `return` ([instrukcja return](statements.md#the-return-statement)) w bloku nie mogą określać wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="cec0a-993">Jeśli wykonywanie bloku metody void kończy się normalnie (to oznacza, że sterowanie przepływem poza końcem treści metody), ta metoda po prostu wraca do bieżącego obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="cec0a-994">Gdy metoda ma `void` wynik i treść wyrażenia, wyrażenie `E` musi być *statement_expression*, a treść jest dokładnie równoważna treści bloku formularza `{ E; }`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="cec0a-995">Gdy metoda ma typ wyniku inny niż void i treści bloku, każda instrukcja `return` w bloku musi określać wyrażenie, które jest niejawnie konwertowane na typ wyniku.</span><span class="sxs-lookup"><span data-stu-id="cec0a-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="cec0a-996">Punkt końcowy treści bloku metody zwracającej wartość nie może być osiągalny.</span><span class="sxs-lookup"><span data-stu-id="cec0a-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="cec0a-997">Innymi słowy, w metodzie zwracającej wartość z treści bloku, sterowanie nie jest dozwolone do przepływu poza końcem treści metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="cec0a-998">Gdy metoda ma typ wyniku inny niż void i treść wyrażenia, wyrażenie musi być niejawnie konwertowane na typ wyniku, a treść jest dokładnie równoważna z treścią bloku formularza `{ return E; }`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="cec0a-999">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-999">In the example</span></span>
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
<span data-ttu-id="cec0a-1000">Metoda `F` zwracająca wartość powoduje błąd czasu kompilacji, ponieważ kontrolka może przepływać poza końcem treści metody.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="cec0a-1001">Metody `G` i `H` są poprawne, ponieważ wszystkie możliwe ścieżki wykonywania kończą się instrukcją Return, która określa wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="cec0a-1002">Metoda `I` jest poprawna, ponieważ jej treść jest równoważna z blokiem instrukcji z tylko jedną instrukcją Return.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="cec0a-1003">Przeciążanie metody</span><span class="sxs-lookup"><span data-stu-id="cec0a-1003">Method overloading</span></span>

<span data-ttu-id="cec0a-1004">Reguły rozpoznawania przeciążenia metody są opisane w [wnioskach o typie](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="cec0a-1005">Właściwości</span><span class="sxs-lookup"><span data-stu-id="cec0a-1005">Properties</span></span>

<span data-ttu-id="cec0a-1006">***Właściwość*** jest członkiem, który zapewnia dostęp do cech obiektu lub klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="cec0a-1007">Przykłady właściwości obejmują długość ciągu znaków, rozmiar czcionki, podpis okna, nazwę klienta i tak dalej...</span><span class="sxs-lookup"><span data-stu-id="cec0a-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="cec0a-1008">Właściwości są naturalnym rozszerzeniem pól — obie są nazwanymi elementami członkowskimi ze skojarzonymi typami, a Składnia służąca do uzyskiwania dostępu do pól i właściwości jest taka sama.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="cec0a-1009">Jednak w przeciwieństwie do pól właściwości nie oznacza lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="cec0a-1010">Zamiast tego właściwości mają metody ***dostępu*** określające instrukcje, które mają być wykonywane, gdy ich wartości są odczytywane lub zapisywane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="cec0a-1011">W ten sposób właściwości zapewniają mechanizm kojarzenia akcji z odczytem i pisaniem atrybutów obiektu; Ponadto umożliwiają one obliczenia takich atrybutów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="cec0a-1012">Właściwości są deklarowane przy użyciu *property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1012">Properties are declared using *property_declaration*s:</span></span>

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

<span data-ttu-id="cec0a-1013">*Property_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)) i prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `static` (Metoda[statyczna i wystąpienia](classes.md#static-and-instance-methods)), `virtual` ([metody wirtualne](classes.md#virtual-methods)), `override` ([metody zastępowania](classes.md#override-methods)), `sealed` ([zapieczętowane metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)) i `extern` (metody[zewnętrzne](classes.md#external-methods)) modyfikatory.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="cec0a-1014">Deklaracje właściwości podlegają tym samym regułom, które są deklaracjami metod ([metodami](classes.md#methods)) w odniesieniu do prawidłowych kombinacji modyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="cec0a-1015">*Typ* deklaracji właściwości określa typ właściwości wprowadzonej przez deklarację, a *MEMBER_NAME* określa nazwę właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="cec0a-1016">Chyba że właściwość jest jawną implementacją składowej interfejsu, *MEMBER_NAME* jest po prostu *identyfikatorem*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="cec0a-1017">W przypadku jawnej implementacji elementu członkowskiego interfejsu ([jawne implementacje elementu członkowskiego interfejsu](interfaces.md#explicit-interface-member-implementations)) *MEMBER_NAME* składa się z *interface_type* po którym następuje "`.`" i *Identyfikator*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="cec0a-1018">*Typ* właściwości musi być co najmniej taki sam jak wartość właściwości ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cec0a-1019">*Property_body* może składać się z ***treści metody dostępu*** lub ***treści wyrażenia***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="cec0a-1020">W treści metody dostępu *accessor_declarations*, która musi być ujęta w tokeny "`{`" i "`}`", zadeklaruj metody dostępu ([Akcesory](classes.md#accessors)) właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="cec0a-1021">Metody dostępu określają instrukcję wykonywalną skojarzoną z odczytem i zapisem właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="cec0a-1022">Treść wyrażenia składająca się z `=>` po którym następuje *wyrażenie* `E`, a średnik jest dokładnie równoważny z treścią instrukcji `{ get { return E; } }`i można go użyć tylko do określenia właściwości metody pobierającej, w której wynik metody pobierającej jest określony przez pojedyncze wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="cec0a-1023">*Property_initializer* można udzielić tylko dla automatycznie zaimplementowanej właściwości ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)) i powoduje inicjalizację pola bazowego takich właściwości z wartością podaną przez *wyrażenie*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="cec0a-1024">Mimo że składnia uzyskiwania dostępu do właściwości jest taka sama jak w przypadku pola, właściwość nie jest sklasyfikowana jako zmienna.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="cec0a-1025">Dlatego nie jest możliwe przekazanie właściwości jako argumentu `ref` lub `out`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="cec0a-1026">Gdy Deklaracja właściwości zawiera modyfikator `extern`, właściwość jest określana jako ***Właściwość zewnętrzna***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="cec0a-1027">Ponieważ Deklaracja właściwości zewnętrznej nie zapewnia żadnej rzeczywistej implementacji, każda z jej *accessor_declarations* składa się z średnika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="cec0a-1028">Właściwości statyczne i wystąpienia</span><span class="sxs-lookup"><span data-stu-id="cec0a-1028">Static and instance properties</span></span>

<span data-ttu-id="cec0a-1029">Gdy Deklaracja właściwości zawiera modyfikator `static`, właściwość jest określana jako ***właściwość statyczna***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="cec0a-1030">Gdy modyfikator `static` nie jest obecny, właściwość jest określana jako ***Właściwość wystąpienia***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="cec0a-1031">Właściwość statyczna nie jest skojarzona z określonym wystąpieniem i jest błędem czasu kompilacji, aby odwołać się do `this` w obiektach dostępu do właściwości statycznej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="cec0a-1032">Właściwość instance jest skojarzona z danym wystąpieniem klasy i można uzyskać do niego dostęp jako `this` ([ten dostęp](expressions.md#this-access)) w obiektach dostępu tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="cec0a-1033">Gdy właściwość jest przywoływana w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `E.M`, jeśli `M` jest właściwością statyczną, `E` musi zwrócić uwagę na typ zawierający `M`, a jeśli `M` jest właściwością wystąpienia, E musi zwrócić uwagę na wystąpienie typu zawierającego `M`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="cec0a-1034">Różnice między elementami statycznymi a składowymi wystąpienia są omówione bardziej szczegółowo w [elementach członkowskich static i instance](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="cec0a-1035">Metod dostępu</span><span class="sxs-lookup"><span data-stu-id="cec0a-1035">Accessors</span></span>

<span data-ttu-id="cec0a-1036">*Accessor_declarations* właściwości określa instrukcje wykonywalne skojarzone z odczytem i zapisem tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="cec0a-1037">Deklaracje metody dostępu składają się z *get_accessor_declaration*, *set_accessor_declaration*lub obu tych elementów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="cec0a-1038">Każda deklaracja metody dostępu składa się z tokenu `get` lub `set` po nim opcjonalne *accessor_modifier* i *accessor_body*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="cec0a-1039">Użycie *accessor_modifier*s podlega następującym ograniczeniom:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="cec0a-1040">Nie można użyć *accessor_modifier* w interfejsie lub w jawnej implementacji elementu członkowskiego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="cec0a-1041">Dla właściwości lub indeksatora, który nie ma modyfikatora `override`, *accessor_modifier* jest dozwolony tylko wtedy, gdy właściwość lub indeksator ma metodę dostępu `get` i `set`, a następnie jest dozwolony tylko w jednym z tych metod dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="cec0a-1042">Dla właściwości lub indeksatora, który zawiera modyfikator `override`, metoda dostępu musi być zgodna z *accessor_modifier*, jeśli istnieje, do zastąpienia metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="cec0a-1043">*Accessor_modifier* musi deklarować dostępność, która jest ściśle bardziej restrykcyjna niż zadeklarowana dostępność właściwości lub indeksatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="cec0a-1044">Aby precyzyjnie:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1044">To be precise:</span></span>
   * <span data-ttu-id="cec0a-1045">Jeśli właściwość lub indeksator ma zadeklarowaną dostępność `public`, *accessor_modifier* może być `protected internal`, `internal`, `protected`lub `private`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="cec0a-1046">Jeśli właściwość lub indeksator ma zadeklarowaną dostępność `protected internal`, *accessor_modifier* może być `internal`, `protected`lub `private`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="cec0a-1047">Jeśli właściwość lub indeksator ma zadeklarowaną dostępność `internal` lub `protected`, *accessor_modifier* musi być `private`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="cec0a-1048">Jeśli właściwość lub indeksator ma zadeklarowaną dostępność `private`, nie można używać *accessor_modifier* .</span><span class="sxs-lookup"><span data-stu-id="cec0a-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="cec0a-1049">Dla właściwości `abstract` i `extern` *accessor_body* dla każdego określonego akcesora jest po prostu średnikiem.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="cec0a-1050">Nieabstrakcyjna Właściwość niebędąca elementem zewnętrznym może *accessor_body* być średnikiem, w takim przypadku jest ***automatycznie implementowaną właściwością*** ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="cec0a-1051">Automatycznie implementowana właściwość musi mieć co najmniej metodę dostępu get.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="cec0a-1052">W przypadku akcesorów dowolnej innej nieabstrakcyjnej właściwości nieextern *accessor_body* jest *blok* , który określa instrukcje, które mają zostać wykonane po wywołaniu odpowiedniej metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="cec0a-1053">Metoda dostępu `get` odpowiada metodzie bez parametrów z wartością zwracaną typu właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="cec0a-1054">Poza elementem docelowym przypisania, gdy właściwość jest przywoływana w wyrażeniu, metoda dostępu `get` właściwości jest wywoływana, aby obliczyć wartość właściwości ([wartości wyrażeń](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="cec0a-1055">Treść metody dostępu `get` musi być zgodna z regułami dotyczącymi metod zwracających wartość opisaną w [treści metod](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="cec0a-1056">W szczególności wszystkie instrukcje `return` w treści metody dostępu `get` muszą określać wyrażenie, które jest niejawnie konwertowane na typ właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="cec0a-1057">Ponadto punkt końcowy metody dostępu `get` nie może być osiągalny.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="cec0a-1058">Metoda dostępu `set` odnosi się do metody z parametrem pojedynczego wartości typu właściwości i `void` typem zwracanym.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="cec0a-1059">Niejawny parametr metody dostępu `set` ma zawsze nazwę `value`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="cec0a-1060">Gdy właściwość jest przywoływana jako element docelowy przypisania ([Operatory przypisania](expressions.md#assignment-operators)), lub jako operand `++` lub `--` ([Operatory przyrostu i zmniejszania](expressions.md#postfix-increment-and-decrement-operators)wartości [prefiksu](expressions.md#prefix-increment-and-decrement-operators)), metoda dostępu `set` jest wywoływana z argumentem (którego wartość jest po prawej stronie przypisania lub operandem operatora `++` lub `--`), który zapewnia nową wartość ([przypisanie proste](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="cec0a-1061">Treść metody dostępu `set` musi być zgodna z regułami dla `void` metod opisanymi w [treści metod](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="cec0a-1062">W szczególności instrukcje `return` w treści metody dostępu `set` nie mogą określać wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="cec0a-1063">Ponieważ metoda dostępu `set` niejawnie ma parametr o nazwie `value`, jest to błąd czasu kompilacji dla zmiennej lokalnej lub deklaracji stałej w metodzie dostępu `set`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="cec0a-1064">W oparciu o obecność lub brak `get` i metod dostępu `set`, właściwość jest sklasyfikowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="cec0a-1065">Właściwość, która zawiera zarówno metodę dostępu `get`, jak i metodę dostępu `set`, jest określana jako właściwość do ***odczytu i zapisu*** .</span><span class="sxs-lookup"><span data-stu-id="cec0a-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="cec0a-1066">Właściwość, która ma tylko metodę dostępu `get`, jest określana jako właściwość ***tylko do odczytu*** .</span><span class="sxs-lookup"><span data-stu-id="cec0a-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="cec0a-1067">Jest to błąd czasu kompilacji dla właściwości tylko do odczytu, aby być elementem docelowym przypisania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="cec0a-1068">Właściwość, która ma tylko metodę dostępu `set`, jest określana jako właściwość ***tylko do zapisu*** .</span><span class="sxs-lookup"><span data-stu-id="cec0a-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="cec0a-1069">Poza elementem docelowym przypisania jest to błąd czasu kompilacji, który odwołuje się do właściwości tylko do zapisu w wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="cec0a-1070">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-1070">In the example</span></span>
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
<span data-ttu-id="cec0a-1071">formant `Button` deklaruje publiczną właściwość `Caption`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="cec0a-1072">Metoda dostępu `get` właściwości `Caption` zwraca ciąg przechowywany w prywatnym polu `caption`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="cec0a-1073">Metoda dostępu `set` sprawdza, czy nowa wartość różni się od bieżącej wartości, a jeśli tak, przechowuje nową wartość i odmaluje formant.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="cec0a-1074">Właściwości często są zgodne ze wzorcem pokazanym powyżej: metoda dostępu `get` po prostu zwraca wartość przechowywaną w polu prywatnym, a metoda dostępu `set` modyfikuje to pole prywatne, a następnie wykonuje wszelkie dodatkowe akcje wymagane do pełnej aktualizacji stanu obiektu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="cec0a-1075">W przypadku powyższej klasy `Button` poniżej przedstawiono przykład użycia właściwości `Caption`:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="cec0a-1076">W tym miejscu metoda dostępu `set` jest wywoływana przez przypisanie wartości do właściwości, a metoda dostępu `get` jest wywoływana przez odwołanie do właściwości w wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="cec0a-1077">Metody dostępu `get` i `set` właściwości nie są odrębnymi składowymi i nie można deklarować metod dostępu dla właściwości oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="cec0a-1078">W związku z tym nie jest możliwe, aby dwie metody dostępu do odczytu i zapisu miały różne ułatwienia dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="cec0a-1079">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-1079">The example</span></span>
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
<span data-ttu-id="cec0a-1080">nie deklaruje pojedynczej właściwości do odczytu i zapisu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="cec0a-1081">Zamiast tego deklaruje dwie właściwości o tej samej nazwie, jeden tylko do odczytu i jeden tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="cec0a-1082">Ponieważ dwa elementy członkowskie zadeklarowane w tej samej klasie nie mogą mieć takiej samej nazwy, przykład powoduje błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="cec0a-1083">Gdy Klasa pochodna deklaruje właściwość o takiej samej nazwie jak dziedziczona właściwość, właściwość pochodna ukrywa dziedziczonej właściwości w odniesieniu do odczytu i zapisu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="cec0a-1084">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-1084">In the example</span></span>
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
<span data-ttu-id="cec0a-1085">Właściwość `P` w `B` ukrywa właściwość `P` w `A` w odniesieniu do odczytu i zapisu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="cec0a-1086">W tym celu w instrukcjach</span><span class="sxs-lookup"><span data-stu-id="cec0a-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="cec0a-1087">przypisanie do `b.P` powoduje zgłoszenie błędu czasu kompilacji, ponieważ właściwość `P` tylko do odczytu w `B` ukrywa właściwość `P` tylko do zapisu w `A`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="cec0a-1088">Należy jednak zauważyć, że rzutowanie może być używane w celu uzyskania dostępu do właściwości Hidden `P`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="cec0a-1089">W przeciwieństwie do pól publicznych, właściwości zapewniają rozdzielenie między wewnętrznym stanem obiektu a jego interfejsem publicznym.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="cec0a-1090">Rozważmy przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1090">Consider the example:</span></span>
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="cec0a-1091">W tym miejscu Klasa `Label` używa dwóch `int` pól, `x` i `y`do przechowywania jej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="cec0a-1092">Lokalizacja jest publicznie ujawniana zarówno jako `X`, jak i Właściwość `Y` i jako właściwość `Location` typu `Point`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="cec0a-1093">Jeśli w przyszłej wersji `Label`będzie bardziej wygodne przechowywanie lokalizacji jako `Point` wewnętrznie, zmiana może zostać wprowadzona bez wpływu na publiczny interfejs klasy:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="cec0a-1094">Miało `x` i `y` zamiast tego zostały `public readonly` pola, nie byłoby możliwe dokonanie takiej zmiany w klasie `Label`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="cec0a-1095">Ujawnienie stanu za poorednictwem właściwości nie musi być mniej wydajne niż bezpośrednie udostępnianie pól.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="cec0a-1096">W szczególności, gdy właściwość nie jest wirtualna i zawiera tylko niewielką ilość kodu, środowisko wykonawcze może zastępować wywołania metod dostępu przy użyciu rzeczywistego kodu metod dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="cec0a-1097">Ten proces jest nazywany ***dekreśleniem***i sprawia, że dostęp do właściwości jest skuteczny jako dostęp do pola, a jeszcze zachowuje zwiększoną elastyczność właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="cec0a-1098">Ponieważ wywoływanie metody dostępu `get` jest koncepcyjnie równoważne odczytywaniu wartości pola, jest uznawany za zły styl programowania dla metod dostępu `get`, aby mieć zauważalne efekty uboczne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="cec0a-1099">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="cec0a-1100">wartość właściwości `Next` zależy od tego, ile razy uzyskano dostęp do właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="cec0a-1101">W efekcie uzyskanie dostępu do właściwości daje zauważalny efekt uboczny, a właściwość powinna zostać zaimplementowana jako metoda.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="cec0a-1102">Konwencja "brak efektów ubocznych" dla metod dostępu `get` nie oznacza, że metody dostępu `get` powinny zawsze być zapisane, aby po prostu zwracały wartości przechowywane w polach.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="cec0a-1103">W rzeczywistości metody dostępu `get` często obliczają wartość właściwości poprzez dostęp do wielu pól lub wywoływanie metod.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="cec0a-1104">Jednak prawidłowo zaprojektowany `get` metoda dostępu nie wykonuje żadnych działań, które powodują zauważalne zmiany stanu obiektu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="cec0a-1105">Właściwości można użyć, aby opóźnić inicjalizację zasobu do momentu, w którym jest ono najpierw przywoływane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="cec0a-1106">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1106">For example:</span></span>
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

<span data-ttu-id="cec0a-1107">Klasa `Console` zawiera trzy właściwości, `In`, `Out`i `Error`, które reprezentują odpowiednio standardowe dane wejściowe, wyjściowe i błędy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="cec0a-1108">Przez udostępnienie tych elementów członkowskich jako właściwości, Klasa `Console` może opóźnić ich inicjalizację do momentu ich faktycznego użycia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="cec0a-1109">Na przykład po pierwszym odwołaniu się do właściwości `Out`, jak w</span><span class="sxs-lookup"><span data-stu-id="cec0a-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="cec0a-1110">zostanie utworzony podstawowy `TextWriter` urządzenia wyjściowego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="cec0a-1111">Jeśli jednak aplikacja nie tworzy żadnych odwołań do właściwości `In` i `Error`, wówczas żadne obiekty nie są tworzone dla tych urządzeń.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="cec0a-1112">Automatycznie implementowane właściwości</span><span class="sxs-lookup"><span data-stu-id="cec0a-1112">Automatically implemented properties</span></span>

<span data-ttu-id="cec0a-1113">Automatycznie implementowana Właściwość (lub ***Właściwość automatyczna*** dla krótkich) jest nieabstrakcyjną właściwością nieextern z tylko średnikami.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="cec0a-1114">Funkcja autowłaściwości musi mieć metodę dostępu get i opcjonalnie może mieć metodę dostępu set.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="cec0a-1115">Gdy właściwość jest określona jako automatycznie implementowana właściwość, ukryte pole zapasowe jest automatycznie dostępne dla właściwości, a metody dostępu są implementowane w celu odczytu i zapisu w tym polu zapasowym.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="cec0a-1116">Jeśli właściwość autoproperty nie ma metody dostępu set, pole zapasowe jest uznawane za `readonly` ([pola tylko do odczytu](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="cec0a-1117">Podobnie jak w przypadku pola `readonly`, metoda pobierająca, która jest jedyną właściwością, może również być przypisana do w treści konstruktora klasy otaczającej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="cec0a-1118">Takie przypisanie przypisuje bezpośrednio do pola zapasowego tylko do odczytu właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="cec0a-1119">Właściwość autoproperty może opcjonalnie mieć *property_initializer*, która jest stosowana bezpośrednio do pola zapasowego jako *variable_initializer* ([inicjatory zmiennych](classes.md#variable-initializers)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="cec0a-1120">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="cec0a-1121">jest równoważne następującej deklaracji:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="cec0a-1122">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="cec0a-1123">jest równoważne następującej deklaracji:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1123">is equivalent to the following declaration:</span></span>
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

<span data-ttu-id="cec0a-1124">Zwróć uwagę, że przypisania do pola tylko do odczytu są dozwolone, ponieważ występują w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="cec0a-1125">Ułatwienia dostępu</span><span class="sxs-lookup"><span data-stu-id="cec0a-1125">Accessibility</span></span>

<span data-ttu-id="cec0a-1126">Jeśli metoda dostępu ma *accessor_modifier*, domena dostępności ([domeny dostępu](basic-concepts.md#accessibility-domains)) metody dostępu jest określana przy użyciu deklarowanej dostępności *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="cec0a-1127">Jeśli metoda dostępu nie ma *accessor_modifier*, domena dostępności metody dostępu jest określana na podstawie zadeklarowanej dostępności właściwości lub indeksatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="cec0a-1128">Obecność *accessor_modifier* nie ma wpływu na wyszukiwanie elementów członkowskich ([operatorów](expressions.md#operators)) ani rozpoznawanie przeciążenia ([rozwiązanie przeciążenia](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="cec0a-1129">Modyfikatory właściwości lub indeksatora zawsze określają, która właściwość lub indeksator jest powiązany, niezależnie od kontekstu dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="cec0a-1130">Po wybraniu określonej właściwości lub indeksatora domeny dostępności konkretnych metod dostępu są używane do określenia, czy to użycie jest prawidłowe:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="cec0a-1131">Jeśli użycie ma charakter wartości ([wartości wyrażeń](expressions.md#values-of-expressions)), metoda dostępu `get` musi istnieć i być dostępna.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="cec0a-1132">Jeśli użycie ma charakter docelowy przypisania prostego ([przypisanie proste](expressions.md#simple-assignment)), metoda dostępu `set` musi istnieć i być dostępna.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="cec0a-1133">Jeśli użycie ma charakter docelowy przypisania złożonego ([przypisanie złożone](expressions.md#compound-assignment)) lub jako obiekt docelowy operatory `++` lub `--` ([elementy członkowskie funkcji](expressions.md#function-members).9, [wyrażenia wywołania](expressions.md#invocation-expressions)), zarówno metody dostępu `get`, jak i akcesor `set` muszą istnieć i być dostępne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="cec0a-1134">W poniższym przykładzie właściwość `A.Text` jest ukryta przez właściwość `B.Text`, nawet w kontekstach, w których wywoływana jest tylko metoda dostępu `set`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="cec0a-1135">Z kolei Właściwość `B.Count` nie jest dostępna dla klasy `M`, więc zamiast niej zostanie użyta Właściwość dostępna `A.Count`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

<span data-ttu-id="cec0a-1136">Metoda dostępu użyta do zaimplementowania interfejsu może nie mieć *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="cec0a-1137">Jeśli do zaimplementowania interfejsu jest używana tylko jedna metoda dostępu, inne metody dostępu można zadeklarować za pomocą *accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="cec0a-1138">Metody dostępu do właściwości Virtual, Sealed, override i abstract</span><span class="sxs-lookup"><span data-stu-id="cec0a-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="cec0a-1139">Deklaracja właściwości `virtual` określa, że metody dostępu do właściwości są wirtualne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="cec0a-1140">Modyfikator `virtual` ma zastosowanie do obu metod dostępu właściwości do odczytu i zapisu — nie jest to możliwe tylko w przypadku jednej metody dostępu do odczytu i zapisu, która ma być wirtualna.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="cec0a-1141">Deklaracja właściwości `abstract` określa, że metody dostępu do właściwości są wirtualne, ale nie zapewniają rzeczywistej implementacji metod dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="cec0a-1142">Zamiast tego nieabstrakcyjne klasy pochodne są wymagane do zapewnienia własnych implementacji dla metod dostępu poprzez Zastępowanie właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="cec0a-1143">Ponieważ metoda dostępu dla deklaracji właściwości abstrakcyjnej nie oferuje rzeczywistej implementacji, jej *accessor_body* po prostu składa się z średnika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="cec0a-1144">Deklaracja właściwości, która zawiera zarówno Modyfikatory `abstract`, jak i `override` określa, że właściwość jest abstrakcyjna i przesłania Właściwość bazową.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="cec0a-1145">Metody dostępu takich właściwości są również abstrakcyjne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="cec0a-1146">Deklaracje właściwości abstrakcyjnych są dozwolone tylko w klasach abstrakcyjnych ([klasach abstrakcyjnych](classes.md#abstract-classes)). Metody dostępu dziedziczonej właściwości wirtualnej można zastąpić w klasie pochodnej przez dołączenie deklaracji właściwości, która określa `override` dyrektywę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="cec0a-1147">Jest to tzw. ***zastępowanie deklaracji właściwości***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="cec0a-1148">Zastępowanie deklaracji właściwości nie deklaruje nowej właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="cec0a-1149">Zamiast tego po prostu wyspecjalizowane są implementacje metod dostępu istniejącej właściwości wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="cec0a-1150">Zastępowanie deklaracji właściwości musi określać dokładnie te same Modyfikatory dostępności, typ i nazwę, co Właściwość dziedziczona.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="cec0a-1151">Jeśli dziedziczona właściwość ma tylko jedną metodę dostępu (tj., jeśli dziedziczona właściwość jest tylko do odczytu lub tylko do zapisu), właściwość zastępująca musi zawierać tylko ten akcesor.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="cec0a-1152">Jeśli dziedziczona Właściwość zawiera obie metody dostępu (tj., jeśli dziedziczona właściwość jest do odczytu i zapisu), zastępujący właściwość może zawierać jedną metodę dostępu lub obu akcesorów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="cec0a-1153">Zastępowanie deklaracji właściwości może zawierać modyfikator `sealed`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="cec0a-1154">Użycie tego modyfikatora uniemożliwia klasie pochodnej bardziej Zastępowanie właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="cec0a-1155">Metody dostępu właściwości zapieczętowanej są również zapieczętowane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="cec0a-1156">Z wyjątkiem różnic w składni deklaracji i wywołania, Virtual, Sealed, override i abstract metody dostępu zachowują się dokładnie tak, jak wirtualne, zapieczętowane, przesłonięcie i abstrakcyjne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="cec0a-1157">Zgodnie z zasadami opisanymi w [metodach wirtualnych](classes.md#virtual-methods), [metodami przesłonięcia](classes.md#override-methods), [metodami zapieczętowanymi](classes.md#sealed-methods)i [metodami abstrakcyjnymi](classes.md#abstract-methods) stosuje się tak, jakby metody dostępu były metodami odpowiedniej formy:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="cec0a-1158">Metoda dostępu `get` odpowiada metodzie bez parametrów z wartością zwracaną typu właściwości i tymi samymi modyfikatorami co Właściwość zawierająca.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="cec0a-1159">Metoda dostępu `set` odnosi się do metody z parametrem pojedynczego wartości typu właściwości, `void` typem zwracanym i tymi samymi modyfikatorami co Właściwość zawierająca.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="cec0a-1160">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-1160">In the example</span></span>
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
<span data-ttu-id="cec0a-1161">`X` jest wirtualną właściwość tylko do odczytu, `Y` jest wirtualną właściwością odczytu i zapisu, a `Z` jest abstrakcyjną właściwością do odczytu i zapisu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="cec0a-1162">Ponieważ `Z` jest abstrakcyjna, `A` zawierającej klasy również musi być zadeklarowany jako abstract.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="cec0a-1163">Poniżej przedstawiono klasę, która dziedziczy z `A`:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1163">A class that derives from `A` is show below:</span></span>
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

<span data-ttu-id="cec0a-1164">W tym miejscu deklaracje `X`, `Y`i `Z` zastępują deklaracje właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="cec0a-1165">Każda deklaracja właściwości dokładnie dopasowuje Modyfikatory dostępności, typ i nazwę odpowiedniej dziedziczonej właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="cec0a-1166">Metoda dostępu `get` `X` i metoda dostępu `set` `Y` używają słowa kluczowego `base`, aby uzyskać dostęp do dziedziczonych akcesorów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="cec0a-1167">Deklaracja `Z` przesłania zarówno abstrakcyjnych metod dostępu, w tym, że nie istnieją żadne oczekujące składowe funkcji abstrakcyjnych w `B`, a `B` może być klasą abstrakcyjną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="cec0a-1168">Gdy właściwość jest zadeklarowana jako `override`, wszystkie zastąpione metody dostępu muszą być dostępne dla zastępujący kod.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="cec0a-1169">Ponadto zadeklarowana dostępność zarówno właściwości, jak i indeksatora, jak i metod dostępu musi być zgodna z przesłoniętą składową i dostępem.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="cec0a-1170">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1170">For example:</span></span>
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a><span data-ttu-id="cec0a-1171">Zdarzenia</span><span class="sxs-lookup"><span data-stu-id="cec0a-1171">Events</span></span>

<span data-ttu-id="cec0a-1172">***Zdarzenie*** jest członkiem, który umożliwia obiektowi lub klasy udostępnianie powiadomień.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="cec0a-1173">Klienci mogą dołączyć kod wykonywalny zdarzeń przez dostarczenie ***obsługi zdarzeń***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="cec0a-1174">Zdarzenia są deklarowane przy użyciu *event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1174">Events are declared using *event_declaration*s:</span></span>

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

<span data-ttu-id="cec0a-1175">*Event_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)) i prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `static` (Metoda[statyczna i wystąpienia](classes.md#static-and-instance-methods)), `virtual` ([metody wirtualne](classes.md#virtual-methods)), `override` ([metody zastępowania](classes.md#override-methods)), `sealed` ([zapieczętowane metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)) i `extern` (metody[zewnętrzne](classes.md#external-methods)) modyfikatory.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="cec0a-1176">Deklaracje zdarzeń podlegają tym samym regułom, które są deklaracjami metod ([metodami](classes.md#methods)) w odniesieniu do prawidłowych kombinacji modyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="cec0a-1177">*Typ* deklaracji zdarzenia musi być *delegate_type* ([typy referencyjne](types.md#reference-types)), a *delegate_type* musi być co najmniej tak samo jak w przypadku samego zdarzenia ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cec0a-1178">Deklaracja zdarzenia może zawierać *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="cec0a-1179">Jednakże jeśli nie, dla zdarzeń niezewnętrznych, kompilator dostarcza je automatycznie ([zdarzenia podobne do pól](classes.md#field-like-events)); dla zdarzeń extern, metody dostępu są udostępniane zewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="cec0a-1180">Deklaracja zdarzenia, która pomija *event_accessor_declarations* definiuje jedno lub więcej zdarzeń — jeden dla każdej *variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="cec0a-1181">Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych przez takie *event_declaration*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="cec0a-1182">Jest to błąd czasu kompilacji dla *event_declaration* , aby uwzględnić zarówno modyfikator `abstract`, jak i rozdzieloną *event_accessor_declarations*nawiasów klamrowych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="cec0a-1183">Gdy deklaracja zdarzenia zawiera modyfikator `extern`, zdarzenie jest określane jako ***zdarzenie zewnętrzne***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="cec0a-1184">Ponieważ deklaracja zdarzenia zewnętrznego nie oferuje rzeczywistej implementacji, jest to błąd, aby uwzględnić zarówno modyfikator `extern`, jak i *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="cec0a-1185">Jest to błąd czasu kompilacji dla *variable_declarator* deklaracji zdarzenia z modyfikatorem `abstract` lub `external` w celu uwzględnienia *variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="cec0a-1186">Zdarzenia można użyć jako lewego operandu operatorów `+=` i `-=` ([przypisanie zdarzenia](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="cec0a-1187">Te operatory służą odpowiednio do dołączania obsługi zdarzeń do lub usuwania programów obsługi zdarzeń ze zdarzenia, a także Modyfikatory dostępu dla zdarzenia kontrolują konteksty, w których takie operacje są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="cec0a-1188">Ponieważ `+=` i `-=` są jedynymi operacjami, które są dozwolone dla zdarzenia poza typem, który deklaruje zdarzenie, kod zewnętrzny może dodawać i usuwać programy obsługi dla zdarzenia, ale nie może w żaden inny sposób uzyskać ani zmodyfikować podstawowej listy programów obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="cec0a-1189">W operacji `x += y` lub `x -= y`, gdy `x` jest zdarzeniem, a odwołanie ma miejsce poza typem, który zawiera deklarację `x`, wynik operacji ma typ `void` (w przeciwieństwie do typu `x`, z wartością `x` po przypisaniu).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="cec0a-1190">Ta zasada zabrania kodowi zewnętrznemu przeanalizować bazowego delegata zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="cec0a-1191">Poniższy przykład pokazuje, jak programy obsługi zdarzeń są dołączone do wystąpień klasy `Button`:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

<span data-ttu-id="cec0a-1192">W tym miejscu Konstruktor wystąpienia `LoginDialog` tworzy dwa wystąpienia `Button` i dołącza obsługę zdarzeń do zdarzeń `Click`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="cec0a-1193">Zdarzenia podobne do pól</span><span class="sxs-lookup"><span data-stu-id="cec0a-1193">Field-like events</span></span>

<span data-ttu-id="cec0a-1194">W tekście programu klasy lub struktury, która zawiera deklarację zdarzenia, można użyć określonych zdarzeń, takich jak pola.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="cec0a-1195">Do użycia w ten sposób, zdarzenie nie może być `abstract` ani `extern`i nie może jawnie zawierać *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="cec0a-1196">Takiego zdarzenia można użyć w dowolnym kontekście, który zezwala na pole.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="cec0a-1197">Pole zawiera delegata ([delegatów](delegates.md)), który odwołuje się do listy programów obsługi zdarzeń, które zostały dodane do zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="cec0a-1198">Jeśli nie dodano żadnych programów obsługi zdarzeń, pole zawiera `null`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="cec0a-1199">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-1199">In the example</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
<span data-ttu-id="cec0a-1200">`Click` jest używany jako pole w klasie `Button`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="cec0a-1201">Jak pokazano w przykładzie, pole może być badane, modyfikowane i używane w wyrażeniach delegatów wywołań.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="cec0a-1202">Metoda `OnClick` w klasie `Button` "wywołuje" `Click` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="cec0a-1203">Pojęcie związane z wywoływaniem zdarzenia jest dokładnie równoważne do wywołania delegata reprezentowanego przez zdarzenie, co oznacza, że nie istnieją żadne specjalne konstrukcje języka do wywoływania zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="cec0a-1204">Należy zauważyć, że wywołanie delegata jest poprzedzone przez sprawdzenie, czy delegat ma wartość różną od null.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="cec0a-1205">Poza deklaracją klasy `Button`, element członkowski `Click` może być używany tylko po lewej stronie operatorów `+=` i `-=`, jak w</span><span class="sxs-lookup"><span data-stu-id="cec0a-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="cec0a-1206">dołącza delegata do listy wywołań zdarzenia `Click` i</span><span class="sxs-lookup"><span data-stu-id="cec0a-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="cec0a-1207">który usuwa delegata z listy wywołań zdarzenia `Click`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="cec0a-1208">Podczas kompilowania zdarzenia przypominającego pole, kompilator automatycznie tworzy magazyn do przechowywania delegata i tworzy metody dostępu dla zdarzenia, które dodaje lub usuwa programy obsługi zdarzeń do pola delegat.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="cec0a-1209">Operacje dodawania i usuwania są bezpieczne dla wątków i mogą (ale nie muszą) być wykonywane podczas utrzymywania blokady ([instrukcja Lock](statements.md#the-lock-statement)) na obiekcie zawierającym zdarzenie wystąpienia lub obiekt typu ([wyrażenia tworzenia obiektów anonimowych](expressions.md#anonymous-object-creation-expressions)) dla zdarzenia statycznego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="cec0a-1210">W rezultacie deklaracja zdarzenia wystąpienia formularza:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="cec0a-1211">zostanie skompilowany do dowolnego elementu równoważnego:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1211">will be compiled to something equivalent to:</span></span>
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
<span data-ttu-id="cec0a-1212">W ramach klasy `X`odwołania do `Ev` po lewej stronie operatorów `+=` i `-=` powodują wywoływanie metod dodawania i usuwania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="cec0a-1213">Wszystkie inne odwołania do `Ev` są kompilowane w celu odwoływania się do pola ukrytego `__Ev` zamiast tego ([dostęp do elementów członkowskich](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="cec0a-1214">Nazwa "`__Ev`" jest arbitralna; pole ukryte może mieć dowolną nazwę lub Brak nazwy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="cec0a-1215">Metody dostępu zdarzeń</span><span class="sxs-lookup"><span data-stu-id="cec0a-1215">Event accessors</span></span>

<span data-ttu-id="cec0a-1216">Deklaracje zdarzeń zwykle pomijają *event_accessor_declarations*, tak jak w powyższym przykładzie `Button`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="cec0a-1217">Jedną z sytuacji, w której należy to zrobić, jest sytuacja, w której koszt magazynowania jednego pola na zdarzenie nie jest akceptowalny.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="cec0a-1218">W takich przypadkach Klasa może zawierać *event_accessor_declarations* i używać mechanizmu prywatnego do przechowywania listy programów obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="cec0a-1219">*Event_accessor_declarations* zdarzenia określa instrukcje wykonywalne skojarzone z dodawaniem i usuwaniem obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="cec0a-1220">Deklaracje metody dostępu składają się z *add_accessor_declaration* i *remove_accessor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="cec0a-1221">Każda deklaracja metody dostępu składa się z tokenu `add` lub `remove`, po którym następuje *blok*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="cec0a-1222">*Blok* skojarzony z *add_accessor_declaration* określa instrukcje do wykonania po dodaniu programu obsługi zdarzeń, a *blok* skojarzony z *remove_accessor_declaration* określa instrukcje do wykonania po usunięciu programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="cec0a-1223">Każda *add_accessor_declaration* i *remove_accessor_declaration* odnosi się do metody z parametrem pojedynczego wartości typu zdarzenia i `void` typem zwracanym.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="cec0a-1224">Niejawny parametr metody dostępu do zdarzenia nosi nazwę `value`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="cec0a-1225">Gdy zdarzenie jest używane w przypisaniu zdarzenia, zostanie użyta odpowiednia metoda dostępu do zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="cec0a-1226">W odróżnieniu od tego, czy operator przypisania jest `+=`, używana jest metoda Add akcesor, a jeśli operator przypisania jest `-=`, zostanie użyta metoda dostępu Remove.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="cec0a-1227">W obu przypadkach jest używany operand z prawej strony operatora przypisania jako argument metody dostępu do zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="cec0a-1228">Blok *add_accessor_declaration* lub *remove_accessor_declaration* musi być zgodny z regułami `void` metodami opisanymi w [treści metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="cec0a-1229">W szczególności instrukcje `return` w takich blokach nie mogą określać wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="cec0a-1230">Ponieważ metoda dostępu do zdarzenia niejawnie ma parametr o nazwie `value`, jest to błąd czasu kompilacji dla zmiennej lokalnej lub stała zadeklarowana w metodzie dostępu do zdarzeń, która ma tę nazwę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="cec0a-1231">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-1231">In the example</span></span>
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
<span data-ttu-id="cec0a-1232">Klasa `Control` implementuje mechanizm wewnętrznego magazynu dla zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="cec0a-1233">Metoda `AddEventHandler` kojarzy wartość delegata z kluczem, Metoda `GetEventHandler` zwraca delegata aktualnie skojarzony z kluczem, a metoda `RemoveEventHandler` usuwa delegata jako procedurę obsługi zdarzeń dla określonego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="cec0a-1234">Zakładając, że podstawowy mechanizm magazynowania jest w taki sposób, że nie ma żadnego kosztu kojarzenia wartości delegata `null` z kluczem, a w związku z tym nieobsłużone zdarzenia nie zużywają magazynu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="cec0a-1235">Zdarzenia statyczne i wystąpienia</span><span class="sxs-lookup"><span data-stu-id="cec0a-1235">Static and instance events</span></span>

<span data-ttu-id="cec0a-1236">Gdy deklaracja zdarzenia zawiera modyfikator `static`, zdarzenie jest określane jako ***zdarzenie statyczne***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="cec0a-1237">Gdy `static` modyfikator nie jest obecny, zdarzenie jest określane jako ***zdarzenie wystąpienia***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="cec0a-1238">Zdarzenie statyczne nie jest skojarzone z określonym wystąpieniem i jest błędem czasu kompilowania, który odwołuje się do `this` w obiektach dostępu do statycznego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="cec0a-1239">Zdarzenie wystąpienia jest skojarzone z danym wystąpieniem klasy i można uzyskać do niego dostęp jako `this` ([ten dostęp](expressions.md#this-access)) w odniesieniu do tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="cec0a-1240">Gdy do zdarzenia odwołuje się *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `E.M`, jeśli `M` jest zdarzeniem statycznym, `E` musi zwrócić uwagę na typ zawierający `M`, a jeśli `M` jest zdarzenie wystąpienia, E musi zwrócić uwagę na wystąpienie typu zawierającego `M`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="cec0a-1241">Różnice między elementami statycznymi a składowymi wystąpienia są omówione bardziej szczegółowo w [elementach członkowskich static i instance](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="cec0a-1242">Wirtualne, zapieczętowane, przesłonięcie i abstrakcyjne metody dostępu do zdarzeń</span><span class="sxs-lookup"><span data-stu-id="cec0a-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="cec0a-1243">Deklaracja zdarzenia `virtual` określa, że metody dostępu tego zdarzenia są wirtualne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="cec0a-1244">Modyfikator `virtual` ma zastosowanie do obu metod dostępu zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="cec0a-1245">Deklaracja zdarzenia `abstract` określa, że metody dostępu do zdarzenia są wirtualne, ale nie zapewniają rzeczywistej implementacji metod dostępu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="cec0a-1246">Zamiast tego nieabstrakcyjne klasy pochodne są wymagane do zapewnienia własnych implementacji dla metod dostępu przez zastąpienie zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="cec0a-1247">Ze względu na to, że deklaracja zdarzenia abstrakcyjnego nie zawiera rzeczywistej implementacji, nie może zawierać *event_accessor_declarations*rozdzielonych nawiasami klamrowymi.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="cec0a-1248">Deklaracja zdarzenia, która zawiera zarówno Modyfikatory `abstract`, jak i `override` określa, że zdarzenie jest abstrakcyjne i przesłania zdarzenie podstawowe.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="cec0a-1249">Metody dostępu takiego zdarzenia są również abstrakcyjne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="cec0a-1250">Deklaracje zdarzeń abstrakcyjnych są dozwolone tylko w klasach abstrakcyjnych ([klasach abstrakcyjnych](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="cec0a-1251">Metody dostępu dziedziczonego zdarzenia wirtualnego można zastąpić w klasie pochodnej przez dołączenie deklaracji zdarzenia, która określa modyfikator `override`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="cec0a-1252">Jest to tzw. ***zastępowanie deklaracji zdarzenia***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="cec0a-1253">Zastępowanie deklaracji zdarzenia nie deklaruje nowego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="cec0a-1254">Zamiast tego po prostu wyspecjalizowane są implementacje metod dostępu istniejącego zdarzenia wirtualnego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="cec0a-1255">Zastępowanie deklaracji zdarzenia musi określać dokładnie te same Modyfikatory dostępności, typ i nazwę, co zastąpione zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="cec0a-1256">Zastępowanie deklaracji zdarzenia może zawierać modyfikator `sealed`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="cec0a-1257">Użycie tego modyfikatora zapobiega dalszemu zastępowaniu zdarzenia przez klasę pochodną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="cec0a-1258">Metody dostępu zapieczętowanego zdarzenia są również zapieczętowane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="cec0a-1259">Jest to błąd czasu kompilacji dla zastępujący deklaracji zdarzenia w celu uwzględnienia modyfikatora `new`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="cec0a-1260">Z wyjątkiem różnic w składni deklaracji i wywołania, Virtual, Sealed, override i abstract metody dostępu zachowują się dokładnie tak, jak wirtualne, zapieczętowane, przesłonięcie i abstrakcyjne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="cec0a-1261">Zgodnie z zasadami opisanymi w [metodach wirtualnych](classes.md#virtual-methods), [metodami przesłonięcia](classes.md#override-methods), [metodami zapieczętowanymi](classes.md#sealed-methods)i [metodami abstrakcyjnymi](classes.md#abstract-methods) stosuje się tak, jakby metody dostępu były metodami odpowiedniego formularza.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="cec0a-1262">Każdy akcesor odnosi się do metody z parametrem jednowartościowym typu zdarzenia, `void` zwracanym typem i tego samego modyfikatora co zawiera zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="cec0a-1263">Indeksatory</span><span class="sxs-lookup"><span data-stu-id="cec0a-1263">Indexers</span></span>

<span data-ttu-id="cec0a-1264">***Indeksator*** jest członkiem, który umożliwia indeksowanie obiektu w taki sam sposób jak tablica.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="cec0a-1265">Indeksatory są deklarowane przy użyciu *indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1265">Indexers are declared using *indexer_declaration*s:</span></span>

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

<span data-ttu-id="cec0a-1266">*Indexer_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)) i prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `virtual` ([metody wirtualne](classes.md#virtual-methods)), `override` ([Zastąp metody](classes.md#override-methods)), `sealed` ([zapieczętowane metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)) i `extern` ([metody zewnętrzne](classes.md#external-methods)) modyfikatory.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="cec0a-1267">Deklaracje indeksatora podlegają tym samym zasadom, które są deklaracjami metod ([metodami](classes.md#methods)) w odniesieniu do prawidłowych kombinacji modyfikatorów, z wyjątkiem tego, że modyfikator static nie jest dozwolony w deklaracji indeksatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="cec0a-1268">Modyfikatory `virtual`, `override`i `abstract` wzajemnie się wykluczają, chyba że w jednym przypadku.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="cec0a-1269">Modyfikatory `abstract` i `override` mogą być używane razem, aby abstrakcyjny indeksator mógł zastąpić wirtualne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="cec0a-1270">*Typ* deklaracji indeksatora określa typ elementu indeksatora wprowadzonego przez deklarację.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="cec0a-1271">Jeśli indeksator *nie jest* jawną implementacją składowej interfejsu, następuje słowo kluczowe `this`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="cec0a-1272">Dla jawnej implementacji elementu członkowskiego interfejsu *Typ* następuje *interface_type*, "`.`" i słowa kluczowego `this`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="cec0a-1273">W przeciwieństwie do innych elementów członkowskich indeksatory nie mają nazw zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="cec0a-1274">*Formal_parameter_list* określa parametry indeksatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="cec0a-1275">Lista parametrów formalnych indeksatora odpowiada metodzie metody ([parametrów metody](classes.md#method-parameters)), z tą różnicą, że musi być określony co najmniej jeden parametr, i że Modyfikatory parametrów `ref` i `out` są niedozwolone.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="cec0a-1276">*Typ* indeksatora i każdy z typów, do których odwołuje się *formal_parameter_list* musi być co najmniej tak samo jak indeksator ([ograniczenia dotyczące dostępności](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cec0a-1277">*Indexer_body* może składać się z ***treści metody dostępu*** lub ***treści wyrażenia***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="cec0a-1278">W treści metody dostępu *accessor_declarations*, która musi być ujęta w tokeny "`{`" i "`}`", zadeklaruj metody dostępu ([Akcesory](classes.md#accessors)) właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="cec0a-1279">Metody dostępu określają instrukcję wykonywalną skojarzoną z odczytem i zapisem właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="cec0a-1280">Treść wyrażenia składająca się z "`=>`", po którym następuje wyrażenie `E` i średnik jest dokładnie równoważne z treścią instrukcji `{ get { return E; } }`i można go użyć tylko do określenia indeksatorów tylko do pobierającego, w których wynik metody pobierającej jest określony przez pojedyncze wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="cec0a-1281">Mimo że składnia uzyskiwania dostępu do elementu indeksatora jest taka sama jak dla elementu tablicy, element indeksatora nie jest klasyfikowany jako zmienna.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="cec0a-1282">Z tego względu nie jest możliwe przekazanie elementu indeksatora jako argumentu `ref` lub `out`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="cec0a-1283">Formalna lista parametrów indeksatora definiuje sygnaturę ([sygnatury i Przeciążenie](basic-concepts.md#signatures-and-overloading)) indeksatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="cec0a-1284">W celu podpisania indeksatora składa się z liczby i typów jego parametrów formalnych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="cec0a-1285">Typ elementu i nazwy parametrów formalnych nie są częścią podpisu indeksatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="cec0a-1286">Sygnatura indeksatora musi różnić się od sygnatur wszystkich innych indeksatorów zadeklarowanych w tej samej klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="cec0a-1287">Indeksatory i właściwości są bardzo podobne do koncepcji, ale różnią się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="cec0a-1288">Właściwość jest identyfikowana za pomocą jej nazwy, podczas gdy indeksator jest identyfikowany przez jego sygnaturę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="cec0a-1289">Dostęp do właściwości uzyskuje się za pomocą *simple_name* ([nazw prostych](expressions.md#simple-names)) *lub member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)), podczas gdy dostęp do elementu indeksatora uzyskuje się za pomocą *element_access* ([dostęp do indeksatora](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="cec0a-1290">Właściwość może być `static` składową, natomiast indeksator jest zawsze składową wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="cec0a-1291">Metoda dostępu `get` właściwość odpowiada metodzie bez parametrów, podczas gdy metoda dostępu `get` indeksatora odpowiada metodzie z tą samą formalną listą parametrów, co indeksator.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="cec0a-1292">Metoda dostępu `set` właściwość odpowiada metodzie z pojedynczym parametrem o nazwie `value`, podczas gdy `set` metoda dostępu indeksatora odpowiada metodzie z tą samą formalną listą parametrów, co indeksator, oraz dodatkowym parametrem o nazwie `value`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="cec0a-1293">Jest to błąd czasu kompilacji dla metody dostępu indeksatora, aby zadeklarować zmienną lokalną o takiej samej nazwie jak parametr indeksatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="cec0a-1294">W deklaracji właściwości zastępują dostęp do właściwości dziedziczonej przy użyciu składni `base.P`, gdzie `P` jest nazwą właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="cec0a-1295">W przypadku przesłaniania deklaracji indeksatora Dziedziczony indeksator jest dostępny przy użyciu składni `base[E]`, gdzie `E` jest rozdzielaną przecinkami listą wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="cec0a-1296">Nie ma koncepcji "automatycznie zaimplementowany indeksator".</span><span class="sxs-lookup"><span data-stu-id="cec0a-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="cec0a-1297">Wystąpił błąd nieabstrakcyjnego, niezewnętrznego indeksatora z dostępem średników.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="cec0a-1298">Oprócz tych różnic wszystkie reguły zdefiniowane w [metodach dostępu](classes.md#accessors) i [automatycznie zaimplementowane właściwości](classes.md#automatically-implemented-properties) mają zastosowanie do metod dostępu indeksatora oraz metod dostępu do właściwości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="cec0a-1299">Gdy deklaracja indeksatora zawiera modyfikator `extern`, indeksator jest uznawany za ***zewnętrzny indeksator***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="cec0a-1300">Ponieważ zewnętrzna deklaracja indeksatora nie zapewnia żadnej rzeczywistej implementacji, każda z jej *accessor_declarations* składa się z średnika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="cec0a-1301">Poniższy przykład deklaruje klasę `BitArray`, która implementuje indeksator do uzyskiwania dostępu do poszczególnych bitów w tablicy bitowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

<span data-ttu-id="cec0a-1302">Wystąpienie klasy `BitArray` zużywa znacznie mniej pamięci niż odpowiadające im `bool[]` (ponieważ każda wartość dawna zajmuje tylko jeden bit zamiast ostatniego z nich), ale umożliwia wykonywanie tych samych operacji co `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="cec0a-1303">W poniższej klasie `CountPrimes` używany jest `BitArray` i klasyczny algorytm "Sit", aby obliczyć liczbę wartości granicznych z przedziału od 1 do:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

<span data-ttu-id="cec0a-1304">Należy zauważyć, że Składnia służąca do uzyskiwania dostępu do elementów `BitArray` jest dokładnie taka sama jak w przypadku `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="cec0a-1305">W poniższym przykładzie przedstawiono klasę siatki 26 \* 10, która ma indeksator z dwoma parametrami.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="cec0a-1306">Pierwszy parametr musi być wielką lub małą literą w zakresie A-Z, a drugi musi być liczbą całkowitą z zakresu 0-9.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a><span data-ttu-id="cec0a-1307">Przeciążanie indeksatora</span><span class="sxs-lookup"><span data-stu-id="cec0a-1307">Indexer overloading</span></span>

<span data-ttu-id="cec0a-1308">Reguły rozpoznawania przeciążenia indeksatora są opisane w [wnioskach o typie](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="cec0a-1309">Operatory</span><span class="sxs-lookup"><span data-stu-id="cec0a-1309">Operators</span></span>

<span data-ttu-id="cec0a-1310">***Operator*** jest członkiem, który definiuje znaczenie operatora wyrażenia, który można zastosować do wystąpień klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="cec0a-1311">Operatory są deklarowane przy użyciu *operator_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1311">Operators are declared using *operator_declaration*s:</span></span>

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="cec0a-1312">Istnieją trzy kategorie operatorów z możliwością przeciążenia: Jednoargumentowe operatory ([operatory jednoargumentowe](classes.md#unary-operators)), operatory binarne ([Operatory binarne](classes.md#binary-operators)) i operatory konwersji ([Operatory konwersji](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="cec0a-1313">*Operator_body* jest średnikiem, ***treścią instrukcji*** lub ***treścią wyrażenia***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="cec0a-1314">Treść instrukcji składa się z *bloku*, który określa instrukcje do wykonania, gdy operator jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="cec0a-1315">*Blok* musi być zgodny z regułami dotyczącymi metod zwracających wartość opisaną w [treści metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="cec0a-1316">Treść wyrażenia składa się z `=>` po którym następuje wyrażenie i średnik, i oznacza pojedyncze wyrażenie, które ma zostać wykonane po wywołaniu operatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="cec0a-1317">Dla operatorów `extern` *operator_body* składa się po prostu z średnika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="cec0a-1318">Dla wszystkich innych operatorów *operator_body* jest treścią bloku lub treścią wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="cec0a-1319">Następujące reguły mają zastosowanie do wszystkich deklaracji operatora:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="cec0a-1320">Deklaracja operatora musi zawierać zarówno modyfikator `public`, jak i `static`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="cec0a-1321">Parametry operatora muszą być parametrami wartości ([Parametry wartości](variables.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="cec0a-1322">Jest to błąd czasu kompilacji dla deklaracji operatora, aby określić parametry `ref` lub `out`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="cec0a-1323">Sygnatura operatora ([operatory jednoargumentowe](classes.md#unary-operators), [Operatory binarne](classes.md#binary-operators), [Operatory konwersji](classes.md#conversion-operators)) musi różnić się od sygnatur wszystkich innych operatorów zadeklarowanych w tej samej klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="cec0a-1324">Wszystkie typy, do których odwołuje się deklaracja operatora, muszą być co najmniej tak samo jak operator ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="cec0a-1325">Jest to błąd dla tego samego modyfikatora, który może występować wiele razy w deklaracji operatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="cec0a-1326">Każda kategoria operatora nakłada dodatkowe ograniczenia, zgodnie z opisem w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="cec0a-1327">Podobnie jak w przypadku innych składowych, operatory zadeklarowane w klasie bazowej są dziedziczone przez klasy pochodne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="cec0a-1328">Ponieważ deklaracje operatora zawsze wymagają klasy lub struktury, w której operator jest zadeklarowany do uczestniczenia w sygnaturze operatora, nie jest możliwe dla operatora zadeklarowanego w klasie pochodnej w celu ukrycia operatora zadeklarowanego w klasie bazowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="cec0a-1329">W związku z tym modyfikator `new` nigdy nie jest wymagany i w związku z tym nigdy nie jest dozwolony w deklaracji operatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="cec0a-1330">Dodatkowe informacje na temat operatorów jednoargumentowych i binarnych można znaleźć w [operatorach](expressions.md#operators).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="cec0a-1331">Dodatkowe informacje na temat operatorów konwersji można znaleźć w [definicjach zdefiniowanych przez użytkownika](conversions.md#user-defined-conversions).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="cec0a-1332">Operatory jednoargumentowe</span><span class="sxs-lookup"><span data-stu-id="cec0a-1332">Unary operators</span></span>

<span data-ttu-id="cec0a-1333">Poniższe reguły dotyczą deklaracji operatora jednoargumentowego, gdzie `T` wskazuje typ wystąpienia klasy lub struktury, która zawiera deklarację operatora:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="cec0a-1334">Operatory jednoargumentowe `+`, `-`, `!`lub `~` muszą przyjmować jeden parametr typu `T` lub `T?` i mogą zwracać dowolny typ.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="cec0a-1335">Operatory jednoargumentowe `++` lub `--` muszą przyjmować jeden parametr typu `T` lub `T?` i muszą zwracać ten sam typ lub typ pochodny.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="cec0a-1336">Operatory jednoargumentowe `true` lub `false` muszą przyjmować jeden parametr typu `T` lub `T?` i muszą zwracać typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="cec0a-1337">Sygnatura jednoargumentowego operatora składa się z tokenu operatora (`+`, `-`, `!`, `~`, `++`, `--`, `true`lub `false`) i typu pojedynczego parametru formalnego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="cec0a-1338">Zwracany typ nie jest częścią podpisu operatora jednoargumentowego ani nie jest nazwą parametru formalnego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="cec0a-1339">Operatory `true` i `false` jednoargumentowe wymagają deklaracji pary.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="cec0a-1340">Błąd czasu kompilacji występuje, gdy Klasa deklaruje jeden z tych operatorów bez deklarowania innych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="cec0a-1341">Operatory `true` i `false` są opisane bardziej szczegółowo w [warunkowych operatory logiczne](expressions.md#user-defined-conditional-logical-operators) i [wyrażeniach](expressions.md#boolean-expressions)logicznych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="cec0a-1342">W poniższym przykładzie przedstawiono implementację i kolejne użycie `operator ++` dla klasy wektorów liczb całkowitych:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

<span data-ttu-id="cec0a-1343">Zwróć uwagę, jak Metoda operatora zwraca wartość wygenerowaną przez dodanie 1 do operandu, podobnie jak Operatory przyrostka i zmniejszanie ([Operatory przyrostka i zmniejszenie](expressions.md#postfix-increment-and-decrement-operators)) oraz operatory przyrostu i zmniejszania prefiksu ([Operatory przyrostka i](expressions.md#prefix-increment-and-decrement-operators)zmniejszenie).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="cec0a-1344">W przeciwieństwie C++do programu w przypadku tej metody nie należy modyfikować wartości operandu bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="cec0a-1345">W rzeczywistości modyfikacja wartości operandu spowoduje naruszenie standardowej semantyki operatora przyrostu przyrostkowego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="cec0a-1346">Operatory binarne</span><span class="sxs-lookup"><span data-stu-id="cec0a-1346">Binary operators</span></span>

<span data-ttu-id="cec0a-1347">Następujące reguły dotyczą deklaracji operatora binarnego, gdzie `T` określa typ wystąpienia klasy lub struktury, która zawiera deklarację operatora:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="cec0a-1348">Binarny operator nie przesunięcia musi przyjmować dwa parametry, co najmniej jeden z nich musi mieć typ `T` lub `T?`i może zwracać dowolny typ.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="cec0a-1349">Binarny operator `<<` lub `>>` musi przyjmować dwa parametry, z których pierwszy musi mieć typ `T` lub `T?`, a drugi, który musi mieć typ `int` lub `int?`, i może zwracać dowolny typ.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="cec0a-1350">Podpis operatora Binary składa się z tokenu operatora (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`lub `<=`) i typów dwóch parametrów formalnych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="cec0a-1351">Typ zwracany i nazwy parametrów formalnych nie są częścią podpisu operatora binarnego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="cec0a-1352">Niektóre operatory binarne wymagają deklaracji pary.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="cec0a-1353">Dla każdej deklaracji jednego z operatorów para, musi istnieć zgodna deklaracja innego operatora pary.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="cec0a-1354">Deklaracje dwóch operatorów są zgodne, gdy mają ten sam typ zwracany i ten sam typ dla każdego parametru.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="cec0a-1355">Następujące operatory wymagają deklaracji pary:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="cec0a-1356">`operator ==` i `operator !=`</span><span class="sxs-lookup"><span data-stu-id="cec0a-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="cec0a-1357">`operator >` i `operator <`</span><span class="sxs-lookup"><span data-stu-id="cec0a-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="cec0a-1358">`operator >=` i `operator <=`</span><span class="sxs-lookup"><span data-stu-id="cec0a-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="cec0a-1359">Operatory konwersji</span><span class="sxs-lookup"><span data-stu-id="cec0a-1359">Conversion operators</span></span>

<span data-ttu-id="cec0a-1360">Deklaracja operatora konwersji wprowadza ***konwersję zdefiniowaną przez użytkownika*** ([konwersje zdefiniowane przez użytkownika](conversions.md#user-defined-conversions)), która rozszerza wstępnie zdefiniowane konwersje niejawne i jawne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="cec0a-1361">Deklaracja operatora konwersji, która zawiera `implicit` słowo kluczowe wprowadza zdefiniowaną przez użytkownika konwersję niejawną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="cec0a-1362">Konwersje niejawne mogą odbywać się w różnych sytuacjach, w tym wywołaniach elementów członkowskich funkcji, wyrażeniach rzutowania i przypisaniach.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="cec0a-1363">Jest to bardziej szczegółowo opisane w [konwersji niejawnych](conversions.md#implicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="cec0a-1364">Deklaracja operatora konwersji, która zawiera `explicit` słowo kluczowe wprowadza zdefiniowaną przez użytkownika konwersję jawną.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="cec0a-1365">Jawne konwersje mogą wystąpić w wyrażeniach rzutowania i są opisane bardziej szczegółowo w przypadku [konwersji jawnych](conversions.md#explicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="cec0a-1366">Operator konwersji wykonuje konwersję z typu źródłowego, wskazywanego przez typ parametru operatora konwersji do typu docelowego, wskazywanego przez zwracany typ operatora konwersji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="cec0a-1367">Dla danego typu źródła `S` i typu docelowego `T`, jeśli `S` lub `T` są typami dopuszczających wartości null, pozwól `S0` i `T0` odnoszą się do ich typów podstawowych, w przeciwnym razie `S0` i `T0` są równe `S` i `T` odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="cec0a-1368">Klasa lub struktura może deklarować konwersję z typu źródłowego `S` do typu docelowego `T` tylko wtedy, gdy spełnione są wszystkie następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="cec0a-1369">`S0` i `T0` są różnymi typami.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="cec0a-1370">`S0` lub `T0` jest klasą lub typem struktury, w której odbywa się deklaracja operatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="cec0a-1371">Ani `S0` ani `T0` jest *interface_typeem*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="cec0a-1372">Oprócz konwersji zdefiniowanych przez użytkownika konwersja nie istnieje z `S` do `T` lub `T` do `S`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="cec0a-1373">Na potrzeby tych zasad wszystkie parametry typu skojarzone z `S` lub `T` są uznawane za unikatowe typy, które nie mają relacji dziedziczenia z innymi typami, i wszelkie ograniczenia dotyczące tych parametrów typu są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="cec0a-1374">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="cec0a-1375">są dozwolone pierwsze dwie deklaracje operatora, ponieważ na [potrzeby .3,](classes.md#indexers)`T` i `int` i `string` są uznawane za unikatowe typy bez relacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="cec0a-1376">Jednak trzeci operator jest błędem, ponieważ `C<T>` jest klasą bazową `D<T>`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="cec0a-1377">Z drugiej reguły należy wykonać konwersję operatora konwersji do lub z klasy lub typu struktury, w którym jest zadeklarowany operator.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="cec0a-1378">Na przykład, istnieje możliwość, że Klasa lub typ struktury `C` definiować konwersję z `C` do `int` oraz z `int` do `C`, ale nie z `int` `bool`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="cec0a-1379">Nie można bezpośrednio zdefiniować wstępnie zdefiniowanej konwersji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="cec0a-1380">W związku z tym operatory konwersji nie mogą konwertować z lub do `object`, ponieważ niejawne i jawne konwersje już istnieją między `object` i innymi typami.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="cec0a-1381">Podobnie ani źródłowe, ani docelowe typy konwersji nie mogą być typu podstawowego drugiego, ponieważ konwersja już istnieje.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="cec0a-1382">Można jednak zadeklarować operatory na typach ogólnych, które dla konkretnych argumentów typu określają konwersje, które już istniały jako wstępnie zdefiniowane konwersje.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="cec0a-1383">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="cec0a-1384">gdy typ `object` jest określony jako argument typu dla `T`, drugi operator deklaruje konwersję, która już istnieje (niejawną, a w związku z tym również jawne, konwersja istnieje z dowolnego typu do typu `object`).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="cec0a-1385">W przypadkach, gdy istnieje wstępnie zdefiniowana konwersja między dwoma typami, wszystkie konwersje zdefiniowane przez użytkownika między tymi typami są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="cec0a-1386">Opracowany</span><span class="sxs-lookup"><span data-stu-id="cec0a-1386">Specifically:</span></span>

*  <span data-ttu-id="cec0a-1387">Jeśli wstępnie zdefiniowana niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z typu `S` do typu `T`, wszystkie konwersje zdefiniowane przez użytkownika (niejawne lub jawne) z `S` do `T` są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="cec0a-1388">Jeśli wstępnie zdefiniowana konwersja jawna ([jawne konwersje](conversions.md#explicit-conversions)) istnieje z typu `S` do typu `T`, wszystkie jawne konwersje zdefiniowane przez użytkownika z `S` do `T` są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="cec0a-1389">Więcej</span><span class="sxs-lookup"><span data-stu-id="cec0a-1389">Furthermore:</span></span>

<span data-ttu-id="cec0a-1390">Jeśli `T` jest typem interfejsu, zdefiniowane przez użytkownika Konwersje niejawne z `S` do `T` są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="cec0a-1391">W przeciwnym razie niejawne konwersje zdefiniowane przez użytkownika z `S` do `T` nadal są brane pod uwagę.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="cec0a-1392">Dla wszystkich typów, ale `object`operatory zadeklarowane przez typ `Convertible<T>` powyżej nie powodują konfliktu ze wstępnie zdefiniowanymi konwersjemi.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="cec0a-1393">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="cec0a-1394">Jednak w przypadku typu `object`wstępnie zdefiniowane konwersje ukrywają konwersje zdefiniowane przez użytkownika we wszystkich przypadkach, ale jeden:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="cec0a-1395">Konwersje zdefiniowane przez użytkownika nie mogą konwertować z lub na *INTERFACE_TYPE*s.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="cec0a-1396">W szczególności to ograniczenie gwarantuje, że podczas konwertowania na *INTERFACE_TYPE*nie występują żadne przekształcenia zdefiniowane przez użytkownika, a konwersja na *INTERFACE_TYPE* powiodła się tylko wtedy, gdy konwertowany obiekt rzeczywiście implementuje określony *INTERFACE_TYPE*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="cec0a-1397">Sygnatura operatora konwersji składa się z typu źródłowego i typu docelowego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="cec0a-1398">(Należy zauważyć, że jest to jedyna forma elementu członkowskiego, dla którego typ zwracany uczestniczy w podpisie). Klasyfikacja `implicit` lub `explicit` operatora konwersji nie jest częścią podpisu operatora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="cec0a-1399">W ten sposób Klasa lub struktura nie może deklarować zarówno `implicit`, jak i operatora konwersji `explicit` z tymi samymi typami źródłowymi i docelowymi.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="cec0a-1400">Ogólnie rzecz biorąc, zdefiniowane przez użytkownika Konwersje niejawne powinny być zaprojektowane tak, aby nigdy nie generować wyjątków i nigdy nie tracić informacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="cec0a-1401">Jeśli zdefiniowana przez użytkownika konwersja może prowadzić do wyjątków (na przykład ponieważ argument źródłowy znajduje się poza zakresem) lub utracisz informacje (np. odrzucanie bitów o wysokim stopniu kolejności), konwersja powinna być zdefiniowana jako konwersja jawna.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="cec0a-1402">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-1402">In the example</span></span>
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
<span data-ttu-id="cec0a-1403">Konwersja z `Digit` na `byte` jest niejawna, ponieważ nigdy nie zgłasza wyjątków lub utraci informacje, ale konwersja z `byte` do `Digit` jest jawna, ponieważ `Digit` może reprezentować tylko podzestaw możliwych wartości `byte`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="cec0a-1404">Konstruktory wystąpień</span><span class="sxs-lookup"><span data-stu-id="cec0a-1404">Instance constructors</span></span>

<span data-ttu-id="cec0a-1405">***Konstruktor wystąpienia*** jest członkiem, który implementuje akcje wymagane do zainicjowania wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="cec0a-1406">Konstruktory wystąpień są deklarowane przy użyciu *constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="cec0a-1407">*Constructor_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)), prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)) oraz modyfikator `extern` ([metody zewnętrzne](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="cec0a-1408">Deklaracja konstruktora nie może zawierać tego samego modyfikatora wiele razy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="cec0a-1409">*Identyfikator* *constructor_declarator* musi mieć nazwę klasy, w której jest zadeklarowany Konstruktor wystąpień.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="cec0a-1410">Jeśli określona jest jakakolwiek inna nazwa, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="cec0a-1411">Opcjonalne *formal_parameter_list* konstruktora wystąpienia podlegają tym samym regułom, co *Formal_parameter_list* metody ([metody](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="cec0a-1412">Formalna lista parametrów definiuje sygnaturę ([sygnatury i Przeciążenie](basic-concepts.md#signatures-and-overloading)) konstruktora wystąpienia i zarządza procesem, zgodnie z którym Rozpoznanie przeciążenia ([wnioskowanie o typie](expressions.md#type-inference)) wybiera określony Konstruktor wystąpienia w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="cec0a-1413">Każdy typ, do którego odwołuje się *formal_parameter_list* konstruktora wystąpienia, musi być co najmniej tak samo samo jak Konstruktor ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="cec0a-1414">Opcjonalny *constructor_initializer* określa inny Konstruktor wystąpienia do wywołania przed wykonaniem instrukcji podanym w *constructor_body* tego konstruktora wystąpień.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="cec0a-1415">Jest to bardziej szczegółowo opisane w [inicjatorach konstruktora](classes.md#constructor-initializers).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="cec0a-1416">Gdy deklaracja konstruktora zawiera modyfikator `extern`, Konstruktor jest nazywany ***konstruktorem zewnętrznym***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="cec0a-1417">Ponieważ deklaracja konstruktora zewnętrznego nie oferuje rzeczywistej implementacji, jej *constructor_body* składa się z średnika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="cec0a-1418">Dla wszystkich innych konstruktorów *constructor_body* składa się z *bloku* , który określa instrukcje inicjowania nowego wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="cec0a-1419">Odpowiada to dokładnie na *blok* metody wystąpienia z typem zwracanym `void` ([treść metody](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="cec0a-1420">Konstruktory wystąpień nie są dziedziczone.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="cec0a-1421">W ten sposób Klasa nie ma konstruktorów wystąpień innych niż zadeklarowane w klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="cec0a-1422">Jeśli Klasa nie zawiera deklaracji konstruktora wystąpienia, zostanie automatycznie udostępniony Konstruktor wystąpienia domyślnego ([konstruktory domyślne](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="cec0a-1423">Konstruktory wystąpień są wywoływane przez *object_creation_expression*s ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)) i *constructor_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="cec0a-1424">Inicjatory konstruktora</span><span class="sxs-lookup"><span data-stu-id="cec0a-1424">Constructor initializers</span></span>

<span data-ttu-id="cec0a-1425">Wszystkie konstruktory wystąpień (z wyjątkiem tych dla klasy `object`) niejawnie zawierają wywołanie innego konstruktora wystąpienia bezpośrednio przed *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="cec0a-1426">Konstruktor niejawnie wywoływany jest określany przez *constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="cec0a-1427">Inicjator konstruktora wystąpień w formularzu `base(argument_list)` lub `base()` powoduje wywołanie konstruktora wystąpienia z bezpośredniej klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="cec0a-1428">Ten konstruktor jest wybierany przy użyciu *argument_list* , jeśli jest obecny, i [reguł rozpoznawania przeciążenia.](expressions.md#overload-resolution)</span><span class="sxs-lookup"><span data-stu-id="cec0a-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="cec0a-1429">Zestaw konstruktorów wystąpień kandydatów składa się ze wszystkich dostępnych konstruktorów wystąpień zawartych w bezpośredniej klasie podstawowej lub konstruktora domyślnego ([konstruktory domyślne](classes.md#default-constructors)), jeśli nie są zadeklarowane konstruktory wystąpień w bezpośredniej klasie podstawowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="cec0a-1430">Jeśli ten zestaw jest pusty lub nie można zidentyfikować jednego najlepszego konstruktora wystąpienia, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="cec0a-1431">Inicjator konstruktora wystąpień w formularzu `this(argument-list)` lub `this()` powoduje wywołanie konstruktora wystąpienia z samej klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="cec0a-1432">Konstruktor jest wybierany przy użyciu *argument_list* , jeśli jest obecny, i reguł [rozdzielczości przeciążenia.](expressions.md#overload-resolution)</span><span class="sxs-lookup"><span data-stu-id="cec0a-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="cec0a-1433">Zestaw konstruktorów wystąpień kandydatów składa się ze wszystkich dostępnych konstruktorów wystąpień zadeklarowanych w samej klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="cec0a-1434">Jeśli ten zestaw jest pusty lub nie można zidentyfikować jednego najlepszego konstruktora wystąpienia, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="cec0a-1435">Jeśli deklaracja konstruktora wystąpienia zawiera inicjator konstruktora, który wywołuje samego konstruktora, wystąpi błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="cec0a-1436">Jeśli Konstruktor wystąpienia nie ma inicjatora konstruktora, niejawnie podany zostanie inicjator konstruktora w formularzu `base()`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="cec0a-1437">W rezultacie deklaracja konstruktora wystąpienia formularza</span><span class="sxs-lookup"><span data-stu-id="cec0a-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="cec0a-1438">jest dokładnie równoważne</span><span class="sxs-lookup"><span data-stu-id="cec0a-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="cec0a-1439">Zakres parametrów przyznanych przez *formal_parameter_list* deklaracji konstruktora wystąpień zawiera inicjator konstruktora tej deklaracji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="cec0a-1440">W ten sposób inicjator konstruktora jest uprawniony do uzyskiwania dostępu do parametrów konstruktora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="cec0a-1441">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1441">For example:</span></span>
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

<span data-ttu-id="cec0a-1442">Inicjator konstruktora wystąpień nie może uzyskać dostępu do tworzonego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="cec0a-1443">W związku z tym jest to błąd czasu kompilacji, aby odwołać się do `this` w wyrażeniu argumentu inicjatora konstruktora, podobnie jak w przypadku błędu czasu kompilacji dla wyrażenia argumentu, aby odwołać się do dowolnego elementu członkowskiego wystąpienia za pomocą *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="cec0a-1444">Inicjatory zmiennych wystąpienia</span><span class="sxs-lookup"><span data-stu-id="cec0a-1444">Instance variable initializers</span></span>

<span data-ttu-id="cec0a-1445">Gdy Konstruktor wystąpienia nie ma inicjatora konstruktora lub ma inicjatora konstruktora `base(...)`, ten Konstruktor niejawnie wykonuje inicjalizacje określone przez *variable_initializer*s pól wystąpienia zadeklarowanych w swojej klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="cec0a-1446">Odnosi się to do sekwencji przypisań, które są wykonywane natychmiast po wpisie do konstruktora i przed niejawnym wywołaniem konstruktora klasy bazowej bezpośredniego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="cec0a-1447">Inicjatory zmiennych są wykonywane w kolejności tekstowej, w której pojawiają się w deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="cec0a-1448">Wykonanie konstruktora</span><span class="sxs-lookup"><span data-stu-id="cec0a-1448">Constructor execution</span></span>

<span data-ttu-id="cec0a-1449">Inicjatory zmiennych są przekształcane w instrukcje przypisania i te instrukcje przypisania są wykonywane przed wywołaniem konstruktora wystąpienia klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="cec0a-1450">Takie porządkowanie gwarantuje, że wszystkie pola wystąpienia są inicjowane przez ich inicjatory zmiennych przed wykonaniem jakichkolwiek instrukcji, które mają dostęp do tego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="cec0a-1451">Zamieszczono przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-1451">Given the example</span></span>
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
<span data-ttu-id="cec0a-1452">gdy `new B()` jest używany do tworzenia wystąpienia `B`, generowane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```console
x = 1, y = 0
```

<span data-ttu-id="cec0a-1453">Wartość `x` jest równa 1, ponieważ inicjator zmiennej jest wykonywany przed wywołaniem konstruktora wystąpienia klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="cec0a-1454">Jednak wartość `y` jest równa 0 (wartość domyślna `int`), ponieważ przypisanie do `y` nie jest wykonywane, dopóki nie zostanie zwrócony Konstruktor klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="cec0a-1455">Warto traktować inicjatory zmiennych wystąpień i inicjatory konstruktora jako instrukcje, które są automatycznie wstawiane przed *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="cec0a-1456">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-1456">The example</span></span>
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
<span data-ttu-id="cec0a-1457">zawiera kilka inicjatorów zmiennych; zawiera również inicjatory konstruktora obu formularzy (`base` i `this`).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="cec0a-1458">Przykład odnosi się do kodu pokazanego poniżej, gdzie każdy komentarz wskazuje automatycznie wstawioną instrukcję (składnia używana dla automatycznie wstawionego konstruktora wywołania jest nieprawidłowa, ale tylko służy do zilustrowania mechanizmu).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a><span data-ttu-id="cec0a-1459">Konstruktory domyślne</span><span class="sxs-lookup"><span data-stu-id="cec0a-1459">Default constructors</span></span>

<span data-ttu-id="cec0a-1460">Jeśli Klasa nie zawiera deklaracji konstruktora wystąpienia, zostanie automatycznie udostępniony Konstruktor wystąpienia domyślnego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="cec0a-1461">Ten konstruktor domyślny po prostu wywołuje konstruktora bez parametrów bezpośredniej klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="cec0a-1462">Jeśli klasa jest abstrakcyjna, zadeklarowana dostępność dla domyślnego konstruktora jest chroniona.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="cec0a-1463">W przeciwnym razie zadeklarowana dostępność dla domyślnego konstruktora jest publiczna.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="cec0a-1464">W ten sposób Konstruktor domyślny ma zawsze postać</span><span class="sxs-lookup"><span data-stu-id="cec0a-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="cec0a-1465">lub</span><span class="sxs-lookup"><span data-stu-id="cec0a-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="cec0a-1466">gdzie `C` jest nazwą klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="cec0a-1467">Jeśli rozwiązanie przeciążenia nie jest w stanie ustalić unikatowego najlepszego kandydata dla inicjatora konstruktora klasy bazowej, wystąpi błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="cec0a-1468">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="cec0a-1469">podano Konstruktor domyślny, ponieważ Klasa nie zawiera deklaracji konstruktora wystąpień.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="cec0a-1470">W tym celu przykład jest dokładnie równoważny z</span><span class="sxs-lookup"><span data-stu-id="cec0a-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="cec0a-1471">Konstruktory prywatne</span><span class="sxs-lookup"><span data-stu-id="cec0a-1471">Private constructors</span></span>

<span data-ttu-id="cec0a-1472">Gdy Klasa `T` deklaruje tylko konstruktorów wystąpień prywatnych, nie jest możliwe w przypadku klas spoza tekstu programu `T`, które pochodzą z `T` lub bezpośrednio tworzą wystąpienia `T`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="cec0a-1473">W takim przypadku, jeśli Klasa zawiera tylko statyczne elementy członkowskie i nie jest przeznaczona do wystąpienia, dodanie pustego konstruktora wystąpienia prywatnego uniemożliwi utworzenie wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="cec0a-1474">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1474">For example:</span></span>
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

<span data-ttu-id="cec0a-1475">Klasy `Trig` są powiązane z metodami i stałymi, ale nie są przeznaczone do tworzenia wystąpień.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="cec0a-1476">W związku z tym deklaruje on jeden pusty Konstruktor wystąpienia prywatnego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="cec0a-1477">Należy zadeklarować co najmniej jeden Konstruktor wystąpienia, aby pominąć automatyczne generowanie domyślnego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="cec0a-1478">Opcjonalne parametry konstruktora wystąpień</span><span class="sxs-lookup"><span data-stu-id="cec0a-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="cec0a-1479">`this(...)`a forma inicjatora konstruktora jest często używana w połączeniu z przeciążeniem w celu zaimplementowania opcjonalnych parametrów konstruktora wystąpień.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="cec0a-1480">w przykładzie</span><span class="sxs-lookup"><span data-stu-id="cec0a-1480">In the example</span></span>
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
<span data-ttu-id="cec0a-1481">pierwsze dwa konstruktory wystąpień jedynie zawierają wartości domyślne dla brakujących argumentów.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="cec0a-1482">Użyj inicjatora konstruktora `this(...)`, aby wywołać trzeci Konstruktor wystąpienia, który faktycznie wykonuje zadania inicjujące nowe wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="cec0a-1483">Efektem jest to, że opcjonalne parametry konstruktora:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="cec0a-1484">Konstruktory statyczne</span><span class="sxs-lookup"><span data-stu-id="cec0a-1484">Static constructors</span></span>

<span data-ttu-id="cec0a-1485">***Statyczny Konstruktor*** jest członkiem, który implementuje akcje wymagane do zainicjowania typu zamkniętej klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="cec0a-1486">Konstruktory statyczne są deklarowane przy użyciu *static_constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="cec0a-1487">*Static_constructor_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)) i modyfikator `extern` ([metody zewnętrzne](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="cec0a-1488">*Identyfikator* *static_constructor_declaration* musi należeć do klasy, w której zadeklarowany jest Konstruktor statyczny.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="cec0a-1489">Jeśli określona jest jakakolwiek inna nazwa, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="cec0a-1490">Gdy deklaracja konstruktora statycznego zawiera modyfikator `extern`, Konstruktor statyczny jest uznawany za ***zewnętrzny Konstruktor statyczny***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="cec0a-1491">Ponieważ deklaracja zewnętrznego konstruktora statycznego nie oferuje rzeczywistej implementacji, jej *static_constructor_body* składa się z średnika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="cec0a-1492">Dla wszystkich innych deklaracji statycznego konstruktora *static_constructor_body* składa się z *bloku* , który określa instrukcje do wykonania w celu zainicjowania klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="cec0a-1493">Odpowiada to dokładnie *method_body* statycznej metody z typem zwracanym `void` ([treść metody](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="cec0a-1494">Konstruktory statyczne nie są dziedziczone i nie mogą być wywoływane bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="cec0a-1495">Statyczny Konstruktor dla typu zamkniętej klasy jest wykonywany co najwyżej raz w danej domenie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="cec0a-1496">Wykonanie konstruktora statycznego jest wyzwalane przez pierwsze z następujących zdarzeń, które mają być wykonywane w domenie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="cec0a-1497">Tworzone jest wystąpienie typu klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="cec0a-1498">Wszystkie statyczne elementy członkowskie typu klasy są przywoływane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="cec0a-1499">Jeśli Klasa zawiera metodę `Main` ([Uruchamianie aplikacji](basic-concepts.md#application-startup)), w której rozpoczyna się wykonywanie, Konstruktor statyczny dla tej klasy jest wykonywany przed wywołaniem metody `Main`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="cec0a-1500">Aby zainicjować nowy typ klasy zamkniętej, najpierw tworzony jest nowy zestaw pól statycznych ([pola statyczne i wystąpienia](classes.md#static-and-instance-fields)) dla danego typu zamkniętego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="cec0a-1501">Każde z pól statycznych jest inicjowane z wartością domyślną ([wartości domyślne](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="cec0a-1502">Następnie elementy statyczne inicjatory pola ([Inicjalizacja pola statycznego](classes.md#static-field-initialization)) są wykonywane dla tych pól statycznych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="cec0a-1503">Na koniec jest wykonywany Konstruktor statyczny.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="cec0a-1504">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-1504">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
<span data-ttu-id="cec0a-1505">musi generować dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1505">must produce the output:</span></span>
```console
Init A
A.F
Init B
B.F
```
<span data-ttu-id="cec0a-1506">ponieważ wykonywanie `A`statycznego konstruktora jest wyzwalane przez wywołanie do `A.F`, a wykonywanie `B`go konstruktora statycznego jest wyzwalane przez wywołanie do `B.F`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="cec0a-1507">Istnieje możliwość skonstruowania zależności cyklicznych, które zezwalają na przestrzeganie pól statycznych z inicjatorami zmiennych w stanie wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="cec0a-1508">Przykład</span><span class="sxs-lookup"><span data-stu-id="cec0a-1508">The example</span></span>
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
<span data-ttu-id="cec0a-1509">tworzy dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="cec0a-1509">produces the output</span></span>
```console
X = 1, Y = 2
```

<span data-ttu-id="cec0a-1510">Aby wykonać metodę `Main`, system najpierw uruchamia inicjator dla `B.Y`, przed konstruktorem statycznym klasy `B`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="cec0a-1511">Inicjator `Y`powoduje uruchomienie `A`nego konstruktora statycznego, ponieważ istnieje odwołanie do wartości `A.X`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="cec0a-1512">Statyczny Konstruktor `A` z kolei będzie kontynuował Obliczanie wartości `X`i w ten sposób Pobiera wartość domyślną `Y`, która jest równa zero.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="cec0a-1513">`A.X` jest w ten sposób inicjowany do 1.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="cec0a-1514">Proces uruchamiania inicjatorów pól statycznych `A`i konstruktora statycznego, a następnie zakończenia, zwracając do obliczenia wartości początkowej `Y`, wynik, który zostanie ustawiony na 2.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="cec0a-1515">Ponieważ statyczny Konstruktor jest wykonywany dokładnie jeden raz dla każdego zamkniętego typu klasy konstruowanej, jest to wygodne miejsce do wymuszania sprawdzania w czasie wykonywania na parametrze typu, który nie może być sprawdzany w czasie kompilacji za pośrednictwem ograniczeń ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="cec0a-1516">Na przykład następujący typ używa konstruktora statycznego, aby wymusić, że argument typu jest wyliczeniem:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a><span data-ttu-id="cec0a-1517">Destruktory</span><span class="sxs-lookup"><span data-stu-id="cec0a-1517">Destructors</span></span>

<span data-ttu-id="cec0a-1518">***Destruktor*** jest członkiem, który implementuje akcje wymagane do zadestruktorowania wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="cec0a-1519">Destruktor jest zadeklarowany przy użyciu *destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1519">A destructor is declared using a *destructor_declaration*:</span></span>

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="cec0a-1520">*Destructor_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="cec0a-1521">*Identyfikator* *destructor_declaration* musi należeć do klasy, w której jest zadeklarowany destruktor.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="cec0a-1522">Jeśli określona jest jakakolwiek inna nazwa, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="cec0a-1523">Gdy deklaracja destruktora zawiera modyfikator `extern`, destruktor jest nazywany ***destruktorem zewnętrznym***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="cec0a-1524">Ponieważ deklaracja zewnętrznego destruktora nie zapewnia rzeczywistej implementacji, jej *destructor_body* składa się z średnika.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="cec0a-1525">Dla wszystkich innych destruktorów *destructor_body* składa się z bloku, który określa instrukcje do wykonania w celu *zablokowania* wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="cec0a-1526">*Destructor_body* odpowiada dokładnie *method_body* metody wystąpienia z typem zwracanym `void` ([treść metody](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="cec0a-1527">Destruktory nie są dziedziczone.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1527">Destructors are not inherited.</span></span> <span data-ttu-id="cec0a-1528">W ten sposób Klasa nie ma destruktorów innych niż ten, który może być zadeklarowany w tej klasie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="cec0a-1529">Ponieważ destruktor jest wymagany do braku parametrów, nie może być przeciążony, więc Klasa może mieć co najwyżej jeden destruktor.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="cec0a-1530">Destruktory są wywoływane automatycznie i nie mogą być wywoływane jawnie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="cec0a-1531">Wystąpienie kwalifikuje się do zniszczenia, gdy nie jest już możliwe do użycia w żadnym kodzie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="cec0a-1532">Wykonywanie destruktora dla wystąpienia może wystąpić w dowolnym momencie, gdy wystąpienie kwalifikuje się do zniszczenia.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="cec0a-1533">Gdy wystąpienie jest destruktorem, destruktory w łańcuchu dziedziczenia tego wystąpienia są wywoływane, w kolejności, od najbardziej pochodnych do najmniej pochodnych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="cec0a-1534">Destruktor może być wykonywany na dowolnym wątku.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="cec0a-1535">Aby uzyskać dalsze Omówienie zasad decydujących o sposobie i sposobach wykonywania destruktora, zobacz [Automatyczne zarządzanie pamięcią](basic-concepts.md#automatic-memory-management).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="cec0a-1536">Dane wyjściowe przykładu</span><span class="sxs-lookup"><span data-stu-id="cec0a-1536">The output of the example</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
<span data-ttu-id="cec0a-1537">is</span><span class="sxs-lookup"><span data-stu-id="cec0a-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="cec0a-1538">ponieważ destruktory w łańcuchu dziedziczenia są wywoływane w kolejności, od najbardziej pochodnych do najmniej pochodnych.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="cec0a-1539">Destruktory są implementowane przez zastąpienie metody wirtualnej `Finalize` w `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="cec0a-1540">C#programy nie mogą przesłonić tej metody lub wywołać jej bezpośrednio (lub przesłonięć).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="cec0a-1541">Na przykład program</span><span class="sxs-lookup"><span data-stu-id="cec0a-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="cec0a-1542">zawiera dwa błędy.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1542">contains two errors.</span></span>

<span data-ttu-id="cec0a-1543">Kompilator zachowuje się tak, jakby ta metoda i przesłonięcia go nie istnieją.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="cec0a-1544">W tym programie:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="cec0a-1545">jest prawidłowy, a pokazana Metoda ukrywa metodę `Finalize` `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="cec0a-1546">Aby zapoznać się z omówieniem zachowania, gdy wyjątek jest zgłaszany przez destruktor, zobacz [jak wyjątki są obsługiwane](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="cec0a-1547">Iteratory</span><span class="sxs-lookup"><span data-stu-id="cec0a-1547">Iterators</span></span>

<span data-ttu-id="cec0a-1548">Element członkowski funkcji ([składowe funkcji](expressions.md#function-members)) zaimplementowany przy użyciu bloku iteratora ([Blocks](statements.md#blocks)) jest nazywany ***iteratorem***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="cec0a-1549">Blok iteratora może być używany jako treść składowej funkcji, tak długo, jak typ zwracany odpowiadającego elementu członkowskiego funkcji jest jednym z interfejsów modułu wyliczającego ([moduły wyliczające](classes.md#enumerator-interfaces)) lub jeden z wyliczalnych interfejsów ([wyliczalne interfejsy](classes.md#enumerable-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="cec0a-1550">Może wystąpić jako *method_body*, *operator_body* lub *accessor_body*, podczas gdy zdarzenia, konstruktory wystąpień, konstruktory statyczne i destruktory nie mogą być implementowane jako Iteratory.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="cec0a-1551">Gdy element członkowski funkcji jest implementowany przy użyciu bloku iteratora, jest to błąd czasu kompilacji dla formalnej listy parametrów elementu członkowskiego funkcji, aby określić parametry `ref` lub `out`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="cec0a-1552">Interfejsy modułu wyliczającego</span><span class="sxs-lookup"><span data-stu-id="cec0a-1552">Enumerator interfaces</span></span>

<span data-ttu-id="cec0a-1553">***Interfejsy modułu wyliczającego*** są interfejsami nieogólnymi `System.Collections.IEnumerator` i wszystkimi wystąpieniami `System.Collections.Generic.IEnumerator<T>`interfejsu ogólnego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="cec0a-1554">Ze względu na zwięzłości w tym rozdziale te interfejsy są przywoływane odpowiednio `IEnumerator` i `IEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="cec0a-1555">Wyliczalne interfejsy</span><span class="sxs-lookup"><span data-stu-id="cec0a-1555">Enumerable interfaces</span></span>

<span data-ttu-id="cec0a-1556">***Wyliczalne interfejsy*** są interfejsem nieogólnym `System.Collections.IEnumerable` i wszystkimi wystąpieniami `System.Collections.Generic.IEnumerable<T>`interfejsu ogólnego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="cec0a-1557">Ze względu na zwięzłości w tym rozdziale te interfejsy są przywoływane odpowiednio `IEnumerable` i `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="cec0a-1558">Typ Yield</span><span class="sxs-lookup"><span data-stu-id="cec0a-1558">Yield type</span></span>

<span data-ttu-id="cec0a-1559">Iterator tworzy sekwencję wartości, tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="cec0a-1560">Ten typ jest nazywany ***typem Yield*** iteratora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="cec0a-1561">Należy `object`typ Yield iteratora zwracającego `IEnumerator` lub `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="cec0a-1562">Należy `T`typ Yield iteratora zwracającego `IEnumerator<T>` lub `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="cec0a-1563">Obiekty modułu wyliczającego</span><span class="sxs-lookup"><span data-stu-id="cec0a-1563">Enumerator objects</span></span>

<span data-ttu-id="cec0a-1564">Gdy element członkowski funkcji zwracający typ interfejsu modułu wyliczającego jest zaimplementowany przy użyciu bloku iteratora, wywołanie elementu członkowskiego funkcji nie powoduje natychmiastowego wykonania kodu w bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="cec0a-1565">Zamiast tego ***obiekt modułu wyliczającego*** jest tworzony i zwracany.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="cec0a-1566">Ten obiekt hermetyzuje kod określony w bloku iteratora i wykonywanie kodu w bloku iteratora występuje, gdy wywoływana jest metoda `MoveNext` obiektu modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="cec0a-1567">Obiekt modułu wyliczającego ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="cec0a-1568">Implementuje `IEnumerator` i `IEnumerator<T>`, gdzie `T` jest typem Yield iteratora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="cec0a-1569">Implementuje `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="cec0a-1570">Jest on inicjowany z kopią wartości argumentów (jeśli istnieją) i wartością wystąpienia przekazaną do elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="cec0a-1571">Ma cztery potencjalne Stany, ***wcześniej***, ***uruchomione***, ***zawieszone***i ***po***nich, a początkowo jest w stanie ***przed*** .</span><span class="sxs-lookup"><span data-stu-id="cec0a-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="cec0a-1572">Obiekt modułu wyliczającego jest zwykle wystąpieniem klasy modułu wyliczającego wygenerowanego przez kompilator, która hermetyzuje kod w bloku iteratora i implementuje interfejsy modułu wyliczającego, ale możliwe są inne metody implementacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="cec0a-1573">Jeśli klasa modułu wyliczającego jest generowana przez kompilator, ta klasa będzie zagnieżdżona, bezpośrednio lub pośrednio, w klasie zawierającej element członkowski funkcji będzie mieć prywatną dostępność i będzie miała nazwę zarezerwowaną dla użycia kompilatora ([identyfikatory](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="cec0a-1574">Obiekt modułu wyliczającego może zaimplementować więcej interfejsów niż określono powyżej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="cec0a-1575">W poniższych sekcjach opisano dokładne zachowanie `MoveNext`, `Current`i `Dispose` członków implementacji interfejsu `IEnumerable` i `IEnumerable<T>` dostarczonych przez obiekt modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="cec0a-1576">Należy zauważyć, że obiekty modułu wyliczającego nie obsługują metody `IEnumerator.Reset`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="cec0a-1577">Wywołanie tej metody powoduje zgłoszenie `System.NotSupportedException`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="cec0a-1578">MoveNext — Metoda</span><span class="sxs-lookup"><span data-stu-id="cec0a-1578">The MoveNext method</span></span>

<span data-ttu-id="cec0a-1579">Metoda `MoveNext` obiektu modułu wyliczającego hermetyzuje kod bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="cec0a-1580">Wywoływanie metody `MoveNext` wykonuje kod w bloku iteratora i ustawia właściwość `Current` obiektu Enumerator odpowiednio do potrzeb.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="cec0a-1581">Precyzyjne działanie wykonywane przez `MoveNext` zależy od stanu obiektu modułu wyliczającego, gdy `MoveNext` jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="cec0a-1582">Jeśli stan obiektu modułu wyliczającego jest ***wcześniejszy***, wywoływanie `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="cec0a-1583">Zmienia stan na ***uruchomiony***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="cec0a-1584">Inicjuje parametry (w tym `this`) bloku iteratora do wartości argumentów i wartości wystąpienia zapisanego podczas inicjowania obiektu modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="cec0a-1585">Wykonuje blok iterator od początku do momentu przerwania wykonywania (zgodnie z poniższym opisem).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="cec0a-1586">Jeśli stan obiektu modułu wyliczającego jest ***uruchomiony***, wynik wywoływania `MoveNext` jest nieokreślony.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="cec0a-1587">Jeśli stan obiektu modułu wyliczającego jest ***zawieszony***, wywoływanie `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="cec0a-1588">Zmienia stan na ***uruchomiony***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="cec0a-1589">Przywraca wartości wszystkich zmiennych lokalnych i parametrów (włącznie z tym) do wartości zapisanych podczas ostatniego wstrzymania wykonywania bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="cec0a-1590">Należy zauważyć, że zawartość wszelkich obiektów, do których odwołuje się te zmienne, mogła ulec zmianie od czasu poprzedniego wywołania elementu MoveNext.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="cec0a-1591">Wznawia wykonywanie bloku iteratora bezpośrednio po instrukcji `yield return`, która spowodowała zawieszenie wykonywania i jest kontynuowane do momentu przerwania wykonywania (zgodnie z poniższym opisem).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="cec0a-1592">Jeśli stan obiektu modułu wyliczającego jest ***po***, wywołanie `MoveNext` zwraca `false`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="cec0a-1593">Gdy `MoveNext` wykonuje blok iterator, wykonywanie może zostać przerwane na cztery sposoby: przez instrukcję `yield return`, przez instrukcję `yield break`, przez napotkanie końca bloku iteratora i wyjątek zgłaszany i propagowany z bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="cec0a-1594">Po napotkaniu instrukcji `yield return` ([instrukcja Yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="cec0a-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="cec0a-1595">Wyrażenie określone w instrukcji jest oceniane, niejawnie konwertowane na typ Yield i przypisane do właściwości `Current` obiektu modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="cec0a-1596">Wykonywanie treści iteratora zostało wstrzymane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="cec0a-1597">Wartości wszystkich lokalnych zmiennych i parametrów (w tym `this`) są zapisywane, podobnie jak lokalizacja tej instrukcji `yield return`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="cec0a-1598">Jeśli instrukcja `yield return` znajduje się w obrębie jednego lub kilku bloków `try`, skojarzone bloki `finally` nie są wykonywane w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="cec0a-1599">Stan obiektu modułu wyliczającego został zmieniony na ***wstrzymany***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="cec0a-1600">Metoda `MoveNext` zwraca `true` do obiektu wywołującego, co wskazuje, że iteracja została pomyślnie zaawansowana do następnej wartości.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="cec0a-1601">Po napotkaniu instrukcji `yield break` ([instrukcja Yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="cec0a-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="cec0a-1602">Jeśli instrukcja `yield break` znajduje się w obrębie jednego lub kilku bloków `try`, skojarzone bloki `finally` są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="cec0a-1603">Stan obiektu modułu wyliczającego jest zmieniany na ***po***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="cec0a-1604">Metoda `MoveNext` zwraca `false` do obiektu wywołującego, wskazując, że iteracja została zakończona.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="cec0a-1605">Gdy zostanie osiągnięty koniec treści iteratora:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="cec0a-1606">Stan obiektu modułu wyliczającego jest zmieniany na ***po***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="cec0a-1607">Metoda `MoveNext` zwraca `false` do obiektu wywołującego, wskazując, że iteracja została zakończona.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="cec0a-1608">Gdy wyjątek jest zgłaszany i propagowany z bloku iteratora:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="cec0a-1609">Odpowiednie bloki `finally` w treści iteratora zostaną wykonane przez propagację wyjątku.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="cec0a-1610">Stan obiektu modułu wyliczającego jest zmieniany na ***po***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="cec0a-1611">Propagacja wyjątku kontynuuje proces wywołujący metody `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="cec0a-1612">Bieżąca Właściwość</span><span class="sxs-lookup"><span data-stu-id="cec0a-1612">The Current property</span></span>

<span data-ttu-id="cec0a-1613">Na właściwości `Current` obiektu modułu wyliczającego wpływają instrukcje `yield return` w bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="cec0a-1614">Gdy obiekt modułu wyliczającego jest w stanie ***wstrzymania*** , wartość `Current` jest wartością ustawioną przez poprzednie wywołanie do `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="cec0a-1615">Gdy obiekt modułu wyliczającego znajduje się w stanie ***sprzed***, ***uruchomiona***lub ***after*** , wynik uzyskania dostępu `Current` nie zostanie określony.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="cec0a-1616">Dla iteratora z typem Yield innym niż `object`, wynik uzyskania dostępu `Current` przez implementację `IEnumerable` obiektu modułu wyliczającego odnosi się do dostępu `Current` przez implementację `IEnumerator<T>` obiektu modułu wyliczającego i rzutowanie wyniku na `object`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="cec0a-1617">Metoda Dispose</span><span class="sxs-lookup"><span data-stu-id="cec0a-1617">The Dispose method</span></span>

<span data-ttu-id="cec0a-1618">Metoda `Dispose` służy do czyszczenia iteracji przez przełączenie obiektu Enumerator do stanu ***po*** .</span><span class="sxs-lookup"><span data-stu-id="cec0a-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="cec0a-1619">Jeśli stan obiektu modułu wyliczającego jest ***wcześniejszy***, wywoływanie `Dispose` zmienia stan na ***po***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="cec0a-1620">Jeśli stan obiektu modułu wyliczającego jest ***uruchomiony***, wynik wywoływania `Dispose` jest nieokreślony.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="cec0a-1621">Jeśli stan obiektu modułu wyliczającego jest ***zawieszony***, wywoływanie `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="cec0a-1622">Zmienia stan na ***uruchomiony***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="cec0a-1623">Wykonuje wszystkie bloki finally tak, jakby Ostatnia wykonana instrukcja `yield return` była instrukcją `yield break`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="cec0a-1624">Jeśli spowoduje to wyjątek, który ma zostać wygenerowany i rozpropagowany z treści iteratora, stan obiektu modułu wyliczającego jest ustawiony na ***po*** , a wyjątek jest propagowany do obiektu wywołującego metody `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="cec0a-1625">Zmienia stan na ***po***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="cec0a-1626">Jeśli stan obiektu modułu wyliczającego jest ***po***, wywołanie `Dispose` nie ma wpływu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="cec0a-1627">Wyliczalne obiekty</span><span class="sxs-lookup"><span data-stu-id="cec0a-1627">Enumerable objects</span></span>

<span data-ttu-id="cec0a-1628">Gdy element członkowski funkcji zwracający wyliczalny typ interfejsu jest implementowany przy użyciu bloku iteratora, wywołanie elementu członkowskiego funkcji nie powoduje natychmiastowego wykonania kodu w bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="cec0a-1629">Zamiast tego zostanie utworzony i zwrócony ***wyliczalny obiekt*** .</span><span class="sxs-lookup"><span data-stu-id="cec0a-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="cec0a-1630">Metoda `GetEnumerator` obiektu wyliczeniowego zwraca obiekt modułu wyliczającego, który hermetyzuje kod określony w bloku iterator i wykonywanie kodu w bloku iteratora występuje, gdy wywoływana jest metoda `MoveNext` obiektu modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="cec0a-1631">Wyliczalny obiekt ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="cec0a-1632">Implementuje `IEnumerable` i `IEnumerable<T>`, gdzie `T` jest typem Yield iteratora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="cec0a-1633">Jest on inicjowany z kopią wartości argumentów (jeśli istnieją) i wartością wystąpienia przekazaną do elementu członkowskiego funkcji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="cec0a-1634">Wyliczalny obiekt jest zwykle wystąpieniem klasy wyliczalnej generowanej przez kompilator, która hermetyzuje kod w bloku iteratora i implementuje wyliczalne interfejsy, ale inne metody implementacji są możliwe.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="cec0a-1635">Jeśli wyliczalna Klasa jest generowana przez kompilator, ta klasa będzie zagnieżdżona, bezpośrednio lub pośrednio, w klasie zawierającej element członkowski funkcji będzie mieć prywatną dostępność i będzie miała nazwę zarezerwowaną dla użycia kompilatora ([identyfikatory](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="cec0a-1636">Wyliczalny obiekt może zaimplementować więcej interfejsów niż określono powyżej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="cec0a-1637">W szczególności, wyliczalny obiekt może również zaimplementować `IEnumerator` i `IEnumerator<T>`, co umożliwia jego pełnienie jako wyliczalny i moduł wyliczający.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="cec0a-1638">W tym typie implementacji przy pierwszym wywołaniu metody `GetEnumerator` obiektu wyliczalnego jest zwracany wyliczalny obiekt.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="cec0a-1639">Kolejne wywołania `GetEnumerator`wyliczalnego obiektu, jeśli takie istnieją, zwracają kopię wyliczalnego obiektu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="cec0a-1640">W ten sposób każdy zwrócony moduł wyliczający ma swój własny stan, a zmiany w jednym wyliczającym nie wpłyną na inne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="cec0a-1641">GetEnumerator — metoda</span><span class="sxs-lookup"><span data-stu-id="cec0a-1641">The GetEnumerator method</span></span>

<span data-ttu-id="cec0a-1642">Obiekt przeliczalny zapewnia implementację metod `GetEnumerator` interfejsów `IEnumerable` i `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="cec0a-1643">Dwie `GetEnumerator` metody współdzielą wspólną implementację, która uzyskuje i zwraca dostępny obiekt modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="cec0a-1644">Obiekt modułu wyliczającego jest inicjowany przy użyciu wartości argumentów i wartości wystąpienia zapisywanych, gdy wyliczalny obiekt został zainicjowany, ale w przeciwnym razie obiekt Enumerator działa zgodnie z opisem w obiekcie [Enumerator](classes.md#enumerator-objects).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="cec0a-1645">Przykład implementacji</span><span class="sxs-lookup"><span data-stu-id="cec0a-1645">Implementation example</span></span>

<span data-ttu-id="cec0a-1646">W tej sekcji opisano możliwe implementacje iteratorów w postaci standardowych C# konstrukcji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="cec0a-1647">Implementacja opisana tutaj jest oparta na tych samych zasadach, które są używane przez C# kompilator firmy Microsoft, ale nie oznacza to, że jest to dozwolone wdrożenie ani tylko jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="cec0a-1648">Następująca Klasa `Stack<T>` implementuje metodę `GetEnumerator` przy użyciu iteratora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="cec0a-1649">Iterator wylicza elementy stosu w kolejności od góry do dołu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

<span data-ttu-id="cec0a-1650">Metodę `GetEnumerator` można przetłumaczyć na wystąpienie klasy modułu wyliczającego wygenerowanego przez kompilator, która hermetyzuje kod w bloku iteratora, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="cec0a-1651">W poprzednim tłumaczeniu kod w bloku iteratora jest przekształcany w komputer stanu i umieszczany w metodzie `MoveNext` klasy Enumerator.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="cec0a-1652">Ponadto zmienna lokalna `i` jest przełączana do pola w obiekcie modułu wyliczającego, dzięki czemu może nadal istnieć między wywołaniami `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="cec0a-1653">Poniższy przykład drukuje prostą tabelę mnożenia liczb całkowitych od 1 do 10.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="cec0a-1654">Metoda `FromTo` w przykładzie zwraca wyliczalny obiekt i jest implementowana przy użyciu iteratora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="cec0a-1655">Metodę `FromTo` można przetłumaczyć na wystąpienie klasy wyliczalnej kompilatora, która hermetyzuje kod w bloku iteratora, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="cec0a-1656">Klasa wyliczalna implementuje zarówno wyliczalne interfejsy, jak i interfejsy modułu wyliczającego, umożliwiając obsługę zarówno jako wyliczalnego, jak i modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="cec0a-1657">Przy pierwszym wywołaniu metody `GetEnumerator` zostanie zwrócony wyliczalny obiekt.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="cec0a-1658">Kolejne wywołania `GetEnumerator`wyliczalnego obiektu, jeśli takie istnieją, zwracają kopię wyliczalnego obiektu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="cec0a-1659">W ten sposób każdy zwrócony moduł wyliczający ma swój własny stan, a zmiany w jednym wyliczającym nie wpłyną na inne.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="cec0a-1660">Metoda `Interlocked.CompareExchange` służy do zapewnienia bezpiecznego wątkowo operacji.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="cec0a-1661">Parametry `from` i `to` są przełączane do pól w klasie wyliczalnej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="cec0a-1662">Ponieważ `from` jest modyfikowany w bloku iteratora, jest wprowadzane dodatkowe pole `__from` do przechowywania wartości początkowej `from` w każdym wyliczeniu.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="cec0a-1663">Metoda `MoveNext` zgłasza `InvalidOperationException`, jeśli jest wywoływana, gdy `__state` jest `0`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="cec0a-1664">Chroni to przed użyciem wyliczalnego obiektu jako obiektu modułu wyliczającego bez uprzedniego wywołania `GetEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="cec0a-1665">Poniższy przykład pokazuje prostą klasę drzewa.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="cec0a-1666">Klasa `Tree<T>` implementuje metodę `GetEnumerator` przy użyciu iteratora.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="cec0a-1667">Iterator wylicza elementy drzewa w kolejności wrostkowe.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="cec0a-1668">Metodę `GetEnumerator` można przetłumaczyć na wystąpienie klasy modułu wyliczającego wygenerowanego przez kompilator, która hermetyzuje kod w bloku iteratora, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="cec0a-1669">Kompilator wygenerował obiekty tymczasowe używany w instrukcjach `foreach`ch zostaje zniesiony do pól `__left` i `__right` obiektu modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="cec0a-1670">Wartość pola `__state` obiektu wyliczającego jest dokładnie aktualizowana, tak aby poprawna `Dispose()` Metoda zostanie wywołana prawidłowo, jeśli wystąpi wyjątek.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="cec0a-1671">Należy zauważyć, że nie jest możliwe zapisanie przetłumaczonego kodu przy użyciu prostych instrukcji `foreach`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="cec0a-1672">Funkcje asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="cec0a-1672">Async functions</span></span>

<span data-ttu-id="cec0a-1673">Metoda ([metody](classes.md#methods)) lub funkcja anonimowa ([wyrażenia funkcji anonimowej](expressions.md#anonymous-function-expressions)) z modyfikatorem `async` jest nazywana ***funkcją asynchroniczną***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="cec0a-1674">Ogólnie rzecz biorąc termin ***Async*** jest używany do opisywania dowolnego rodzaju funkcji, która ma modyfikator `async`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="cec0a-1675">Jest to błąd czasu kompilacji dla formalnej listy parametrów funkcji asynchronicznej, aby określić parametry `ref` lub `out`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="cec0a-1676">*Return_type* metody asynchronicznej musi być albo `void` lub ***typem zadania***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="cec0a-1677">Typy zadań są `System.Threading.Tasks.Task` i typy skonstruowane z `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="cec0a-1678">Ze względu na zwięzłości w tym rozdziale te typy są przywoływane odpowiednio `Task` i `Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="cec0a-1679">Metoda asynchroniczna zwracająca typ zadania jest określana jako zadanie zwracające zadania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="cec0a-1680">Dokładna definicja typów zadań jest definiowana przez implementację, ale z punktu widzenia języka, typ zadania jest w jednym z Stanów nieukończonych, zakończonych powodzeniem lub błędem.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="cec0a-1681">Zadanie z błędami rejestruje odpowiedni wyjątek.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="cec0a-1682">Pomyślnie `Task<T>` rejestruje wynik typu `T`.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="cec0a-1683">Typy zadań są oczekiwane i dlatego mogą być operandami wyrażeń Await ([Expressions](expressions.md#await-expressions)).</span><span class="sxs-lookup"><span data-stu-id="cec0a-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="cec0a-1684">Wywołanie funkcji asynchronicznej ma możliwość zawieszenia oceny przy użyciu wyrażeń Await ([wyrażenie await](expressions.md#await-expressions)) w swojej treści.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="cec0a-1685">Ocenę można później wznowić w punkcie wstrzymania wyrażenia await przy użyciu ***delegata wznawiania***.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="cec0a-1686">Delegat wznawiania jest typu `System.Action`i gdy jest wywoływany, oszacowanie wywołania funkcji asynchronicznej zostanie wznowione z wyrażenia await, gdzie pozostawione.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="cec0a-1687">Bieżącym obiektem ***wywołującym*** wywołania funkcji asynchronicznej jest oryginalny obiekt wywołujący, jeśli wywołanie funkcji nigdy nie zostało zawieszone lub ostatni obiekt wywołujący delegata wznawiania w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="cec0a-1688">Obliczanie funkcji asynchronicznej zwracającej zadania</span><span class="sxs-lookup"><span data-stu-id="cec0a-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="cec0a-1689">Wywołanie funkcji asynchronicznej zwracającej zadanie powoduje wygenerowanie wystąpienia zwracanego typu zadania.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="cec0a-1690">Jest to nazywane ***zadaniem zwrotnym*** funkcji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="cec0a-1691">Zadanie jest początkowo w stanie niepełnym.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="cec0a-1692">Treść funkcji asynchronicznej jest następnie Szacowana do momentu, aż zostanie zawieszona (przez osiągnięcie wyrażenia await) lub przerwania, w którym formant punktu jest zwracany do obiektu wywołującego, wraz z zadaniem zwrotnym.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="cec0a-1693">Gdy treść funkcji asynchronicznej zostanie zakończona, zadanie powrotu zostanie przeniesione z niekompletnego stanu:</span><span class="sxs-lookup"><span data-stu-id="cec0a-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="cec0a-1694">Jeśli treść funkcji kończy się jako wynik osiągnięcia instrukcji return lub końca treści, każda wartość wyniku zostanie zarejestrowana w zadaniu zwrotnym, która jest przesunięta w stan powodzenie.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="cec0a-1695">Jeśli treść funkcji kończy się jako wynik nieprzechwyconego wyjątku ([Instrukcja throw](statements.md#the-throw-statement)), wyjątek jest rejestrowany w zadaniu zwrotnym, które jest umieszczane w stanie awarii.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="cec0a-1696">Obliczanie funkcji asynchronicznej zwracającej wartość pustą</span><span class="sxs-lookup"><span data-stu-id="cec0a-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="cec0a-1697">Jeśli zwracany typ funkcji asynchronicznej jest `void`, Ocena różni się od powyższych w następujący sposób: ponieważ żadne zadanie nie jest zwracane, funkcja zamiast tego komunikuje zakończenie i wyjątki z ***kontekstem synchronizacji***bieżącego wątku.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="cec0a-1698">Dokładna definicja kontekstu synchronizacji jest zależna od implementacji, ale jest reprezentacją "gdzie" bieżącego wątku jest uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="cec0a-1699">Kontekst synchronizacji jest powiadamiany, gdy zostanie rozpoczęta Ocena funkcji asynchronicznej zwracającej wartość void, została zakończona pomyślnie, lub spowoduje zgłoszenie nieprzechwyconego wyjątku.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="cec0a-1700">Pozwala to na śledzenie liczby uruchomionych w nim funkcji asynchronicznych zwracających wartość pustą oraz decydowanie o sposobie propagowania wyjątków.</span><span class="sxs-lookup"><span data-stu-id="cec0a-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
