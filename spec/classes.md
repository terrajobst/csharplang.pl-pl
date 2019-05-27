---
ms.openlocfilehash: 917e2f1e196013f85eefbda21fb3d717cc681084
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193911"
---
# <a name="classes"></a><span data-ttu-id="c29a4-101">Klasy</span><span class="sxs-lookup"><span data-stu-id="c29a4-101">Classes</span></span>

<span data-ttu-id="c29a4-102">Klasa jest strukturą danych, który może zawierać elementy członkowskie (stałe i pola), funkcji elementów członkowskich danych (metody, właściwości, zdarzenia, indeksatory, operatory, konstruktory wystąpień, destruktory i konstruktorów statycznych) i zagnieżdżone typy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="c29a4-103">Typy klas obsługuje dziedziczenie, mechanizm, według której rozszerzać i specialize klasę bazową klasę pochodną.</span><span class="sxs-lookup"><span data-stu-id="c29a4-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="c29a4-104">Deklaracje klas</span><span class="sxs-lookup"><span data-stu-id="c29a4-104">Class declarations</span></span>

<span data-ttu-id="c29a4-105">A *class_declaration* jest *type_declaration* ([wpisz deklaracje](namespaces.md#type-declarations)) który deklaruje nową klasę.</span><span class="sxs-lookup"><span data-stu-id="c29a4-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="c29a4-106">A *class_declaration* zawiera opcjonalny zestaw *atrybuty* ([atrybuty](attributes.md)), a następnie opcjonalny zestaw *class_modifier*s ([klasy Modyfikatory](classes.md#class-modifiers)), a następnie opcjonalnie `partial` modyfikator, następuje słowa kluczowego `class` i *identyfikator* nazw, klasy, a następnie Opcjonalnie *type_parameter_list* ([parametry typu](classes.md#type-parameters)), a następnie opcjonalnie *class_base* specyfikacji ([klasy podstawowej Specyfikacja](classes.md#class-base-specification)), a następnie opcjonalny zestaw *type_parameter_constraints_clause*s ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), a następnie *class_body*  ([Klasy treści](classes.md#class-body)), opcjonalnie następuje średnik.</span><span class="sxs-lookup"><span data-stu-id="c29a4-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="c29a4-107">Deklaracja klasy nie może dostarczyć *type_parameter_constraints_clause*s, chyba że zawiera także *type_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="c29a4-108">Deklaracja klasy, która dostarcza *type_parameter_list* jest ***deklaracji klasy ogólnej***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="c29a4-109">Ponadto każda klasa zagnieżdżona w deklaracji klasy ogólnej lub deklaracja struktury ogólnej jest deklaracji klasy ogólnej, ponieważ parametrów typu dla typu zawierającego należy podać w celu utworzenia skonstruowanego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="c29a4-110">Modyfikatorów klasy pamięci</span><span class="sxs-lookup"><span data-stu-id="c29a4-110">Class modifiers</span></span>

<span data-ttu-id="c29a4-111">A *class_declaration* może opcjonalnie obejmować sekwencję modyfikatorów klasy pamięci:</span><span class="sxs-lookup"><span data-stu-id="c29a4-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

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

<span data-ttu-id="c29a4-112">Jest to błąd czasu kompilacji dla tego samego modyfikator pojawią się wiele razy w deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="c29a4-113">`new` Modyfikator jest dozwolona dla klas zagnieżdżonych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="c29a4-114">Określa, że klasa ukrywa dziedziczonej składowej o tej samej nazwie, zgodnie z opisem w [nowy modyfikator](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="c29a4-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="c29a4-115">Jest to błąd czasu kompilacji dla `new` modyfikatora występować w deklaracji klasy, który nie jest deklaracją klasy zagnieżdżonej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="c29a4-116">`public`, `protected`, `internal`, I `private` Modyfikatory kontrolować ułatwień dostępu klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="c29a4-117">W zależności od kontekstu, w którym występuje deklaracja klasy, niektóre z tych modyfikatorów może nie być dozwolone ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="c29a4-118">`abstract`, `sealed` i `static` Modyfikatory zostały omówione w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="c29a4-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="c29a4-119">Klasy abstrakcyjne</span><span class="sxs-lookup"><span data-stu-id="c29a4-119">Abstract classes</span></span>

<span data-ttu-id="c29a4-120">`abstract` Modyfikator jest używany do wskazania, że klasa jest niekompletne i czy mają być używane tylko jako klasa bazowa.</span><span class="sxs-lookup"><span data-stu-id="c29a4-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="c29a4-121">Klasa abstrakcyjna różni się od klasy nieabstrakcyjnej w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c29a4-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="c29a4-122">Klasa abstrakcyjna nie można bezpośrednio utworzyć wystąpienia i jest to błąd kompilacji, aby użyć `new` operator dla klasy abstrakcyjnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="c29a4-123">Mimo że możliwe jest zapewnienie zmiennych i wartości, których typy kompilacji są abstrakcyjne, takich zmiennych i wartości niekoniecznie będzie `null` lub zawierają odwołania do wystąpienia klas pochodzących od typy abstrakcyjne klasy nieabstrakcyjnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="c29a4-124">Klasy abstrakcyjnej jest dozwolone (ale nie wymagane) zawierać abstrakcyjne składowe.</span><span class="sxs-lookup"><span data-stu-id="c29a4-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="c29a4-125">Klasa abstrakcyjna nie może być zapieczętowany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="c29a4-126">Gdy klasy nieabstrakcyjnej pochodzi z klasy abstrakcyjnej, nieabstrakcyjnej klasie musi zawierać rzeczywistej implementacji wszystkich dziedziczonych członków abstrakcyjnych, zastępując tych członków abstrakcyjnych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="c29a4-127">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-127">In the example</span></span>
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
<span data-ttu-id="c29a4-128">Klasa abstrakcyjna `A` wprowadza metodę abstrakcyjną `F`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="c29a4-129">Klasy `B` wprowadzono kolejną metodę `G`, ale ponieważ nie zapewnia to implementacja `F`, `B` musi również być zadeklarowany jako abstrakcyjny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="c29a4-130">Klasa `C` zastępuje `F` i zawiera rzeczywiste wdrożenie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="c29a4-131">Ponieważ nie abstrakcyjne składowe w `C`, `C` jest dozwolone (ale nie muszą) być nieabstrakcyjnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="c29a4-132">Zapieczętowane klasy</span><span class="sxs-lookup"><span data-stu-id="c29a4-132">Sealed classes</span></span>

<span data-ttu-id="c29a4-133">`sealed` Modyfikator jest używany do uniemożliwiają wyprowadzanie z klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="c29a4-134">Błąd kompilacji występuje, jeśli określono klasy zapieczętowanej jako klasa bazowa innej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="c29a4-135">Klasa zapieczętowana nie może być klasą abstrakcyjną.</span><span class="sxs-lookup"><span data-stu-id="c29a4-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="c29a4-136">`sealed` Modyfikator jest używany głównie do uniemożliwiają wyprowadzanie niezamierzone, ale umożliwia również pewne optymalizacje w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="c29a4-137">W szczególności ponieważ wiadomo, że klasa zapieczętowana nigdy nie ma żadnych klas pochodnych, istnieje możliwość przekształcania funkcja wirtualna elementu członkowskiego wywołań w wystąpieniach klasy zapieczętowanej niewirtualną wywołania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="c29a4-138">Klasy statyczne</span><span class="sxs-lookup"><span data-stu-id="c29a4-138">Static classes</span></span>

<span data-ttu-id="c29a4-139">`static` Modyfikator jest używany do oznaczania klasy został zadeklarowany jako ***klasy statycznej***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="c29a4-140">Klasa statyczna, nie można utworzyć wystąpienia nie może być używany jako typ i może zawierać tylko statyczne elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="c29a4-141">Tylko statyczne klasy może zawierać deklaracje metody rozszerzenia ([metody rozszerzenia](classes.md#extension-methods)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="c29a4-142">Deklaracja klasy statycznej podlega następującym ograniczeniom:</span><span class="sxs-lookup"><span data-stu-id="c29a4-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="c29a4-143">Klasa statyczna nie może zawierać `sealed` lub `abstract` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="c29a4-144">Należy jednak pamiętać, że ponieważ klasy statycznej nie można utworzyć wystąpienia lub pochodną, działa tak, jakby był zapieczętowany i abstrakcyjny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="c29a4-145">Klasa statyczna nie może zawierać *class_base* specyfikacji ([klasy podstawowej specyfikacji](classes.md#class-base-specification)) i nie można jawnie określić klasę bazową lub listy implementowanych interfejsów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="c29a4-146">Klasa statyczna niejawnie dziedziczy z typu `object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="c29a4-147">Klasa statyczna może zawierać tylko statyczne elementy członkowskie ([statyczna i wystąpienia elementów członkowskich](classes.md#static-and-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="c29a4-148">Należy pamiętać, że stałe i zagnieżdżone typy są klasyfikowane jako statyczne elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="c29a4-149">Klasa statyczna nie może mieć elementów członkowskich z `protected` lub `protected internal` zadeklarowana ułatwień dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="c29a4-150">Jest to błąd czasu kompilacji narusza dowolne z tych ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="c29a4-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="c29a4-151">Klasa statyczna nie ma konstruktorów wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-151">A static class has no instance constructors.</span></span> <span data-ttu-id="c29a4-152">Nie jest możliwe do deklarowania konstruktora wystąpienia w klasie statycznej i Brak domyślnego konstruktora wystąpienia ([domyślne konstruktory](classes.md#default-constructors)) znajduje się na klasie statycznej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="c29a4-153">Elementy członkowskie klasy statyczne nie są automatycznie statyczne i deklaracji elementu członkowskiego musi jawnie dołączyć znak `static` modyfikator (z wyjątkiem i zagnieżdżone typy stałe).</span><span class="sxs-lookup"><span data-stu-id="c29a4-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="c29a4-154">Gdy klasa jest zagnieżdżony w klasie statycznej zewnętrzne, zagnieżdżona klasa nie jest Klasa statyczna, chyba że jawnie zawierającym `static` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="c29a4-155">__Odwoływanie się do typów klas statycznych__</span><span class="sxs-lookup"><span data-stu-id="c29a4-155">__Referencing static class types__</span></span>

<span data-ttu-id="c29a4-156">A *namespace_or_type_name* ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)) jest dozwolona w celu odwołania klasy statycznej, jeśli</span><span class="sxs-lookup"><span data-stu-id="c29a4-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="c29a4-157">*Namespace_or_type_name* jest `T` w *namespace_or_type_name* formularza `T.I`, lub</span><span class="sxs-lookup"><span data-stu-id="c29a4-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="c29a4-158">*Namespace_or_type_name* jest `T` w *typeof_expression* ([listy argumentów](expressions.md#argument-lists)1) w postaci `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="c29a4-159">A *primary_expression* ([funkcji elementów członkowskich](expressions.md#function-members)) jest dozwolona w celu odwołania klasy statycznej, jeśli</span><span class="sxs-lookup"><span data-stu-id="c29a4-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="c29a4-160">*Primary_expression* jest `E` w *member_access* ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) w postaci `E.I`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="c29a4-161">Błąd kompilacji, aby odwoływać się do klasy statycznej jest w innym kontekście.</span><span class="sxs-lookup"><span data-stu-id="c29a4-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="c29a4-162">Na przykład, występuje błąd dla klasy statycznej do użycia jako klasę bazową składowych typu ([zagnieżdżone typy](classes.md#nested-types)) członkiem, argument typu ogólnego lub ograniczenia parametru typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="c29a4-163">Podobnie, w typu tablicowego, typu wskaźnika, w którym nie można użyć klasy statycznej `new` wyrażenia, wyrażenia rzutowania `is` wyrażenie `as` wyrażenie `sizeof` wyrażenie lub wyrażenie wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="c29a4-164">Modyfikatora "Partial"</span><span class="sxs-lookup"><span data-stu-id="c29a4-164">Partial modifier</span></span>

<span data-ttu-id="c29a4-165">`partial` Modyfikator jest używany w celu wskazania, że *class_declaration* jest deklaracją typu częściowego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="c29a4-166">Łączenie wielu deklaracjach częściowej typu o takiej samej nazwie w deklaracji otaczającego przestrzeń nazw lub typ do postaci jednego typu deklaracji, zgodnie z regułami określone w [typów częściowych](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="c29a4-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="c29a4-167">Po deklaracji klasy rozłożone w odrębnych segmentach tekstu programu może być przydatne, jeśli te segmenty nie są tworzone lub przechowywane w różnych kontekstach.</span><span class="sxs-lookup"><span data-stu-id="c29a4-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="c29a4-168">Na przykład jedną część deklaracji klasy może być maszyny generowania, natomiast druga ręcznie utworzone.</span><span class="sxs-lookup"><span data-stu-id="c29a4-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="c29a4-169">Rozdzielenie tekstową dwa uniemożliwia aktualizacje za pomocą jednej z powodujące konflikt z elementem aktualizacji przez inne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="c29a4-170">Parametry typu</span><span class="sxs-lookup"><span data-stu-id="c29a4-170">Type parameters</span></span>

<span data-ttu-id="c29a4-171">Parametr typu jest prostym identyfikatorem, który oznacza symbol zastępczy dla argumentu typu dostarczony w celu utworzenia skonstruowanego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="c29a4-172">Parametr typu jest posiadanie symbol zastępczy dla typu, które zostaną udostępnione później.</span><span class="sxs-lookup"><span data-stu-id="c29a4-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="c29a4-173">Przez kontrast, argument typu ([argumentów typu](types.md#type-arguments)) to rzeczywisty typ, który zostanie zastąpiony dla parametru typu, po utworzeniu skonstruowanego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

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

<span data-ttu-id="c29a4-174">Każdy parametr typu w deklaracji klasy definiuje nazwę w obszarze deklaracji ([deklaracje](basic-concepts.md#declarations)) dla tej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="c29a4-175">W związku z tym nie może on mieć taką samą nazwę jak inny parametr typu lub składowa zadeklarowana w tej klasie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="c29a4-176">Parametr typu nie może mieć taką samą nazwę jak samego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="c29a4-177">Klasy podstawowej specyfikacji</span><span class="sxs-lookup"><span data-stu-id="c29a4-177">Class base specification</span></span>

<span data-ttu-id="c29a4-178">Deklaracja klasy może zawierać *class_base* specyfikacja definiuje bezpośrednia klasa podstawowa klasy i interfejsy ([interfejsów](interfaces.md)) bezpośrednio zaimplementowany przez klasę.</span><span class="sxs-lookup"><span data-stu-id="c29a4-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

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

<span data-ttu-id="c29a4-179">Klasa bazowa, określonym w deklaracji klasy może być typem klasy skonstruowany ([skonstruowany typy](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="c29a4-180">Klasy bazowej nie może być parametrem typu samodzielnie, chociaż może pociągać za sobą parametry typu, które znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="c29a4-181">Klas podstawowych</span><span class="sxs-lookup"><span data-stu-id="c29a4-181">Base classes</span></span>

<span data-ttu-id="c29a4-182">Gdy *class_type* znajduje się w *class_base*, określa on bezpośredniej klasie bazowej deklarowanej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="c29a4-183">Jeśli nie ma w deklaracji klasy *class_base*, lub jeśli *class_base* listy tylko typy interfejsów, bezpośrednie klasy bazowej zakłada się, że `object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="c29a4-184">Klasa dziedziczy członków jego bezpośrednia klasa bazowa, zgodnie z opisem w [dziedziczenia](classes.md#inheritance).</span><span class="sxs-lookup"><span data-stu-id="c29a4-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="c29a4-185">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="c29a4-186">Klasa `A` bezpośrednie klasy bazowej jest nazywany `B`, i `B` jest nazywany mogą pochodzić z `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="c29a4-187">Ponieważ `A` jest nie zostały jawnie określić bezpośrednią klasą bazową, jego bezpośrednia klasa bazowa jest niejawnie `object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="c29a4-188">Dla typu klasy skonstruowany, jeśli klasa podstawowa jest określona w deklaracji klasy ogólnej klasy bazowej skonstruowanego typu uzyskuje się przez podstawianie dla każdego *type_parameter* w deklaracji klasy bazowej, odpowiedni *type_argument* skonstruowanego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="c29a4-189">Biorąc pod uwagę deklaracji klasy ogólnej</span><span class="sxs-lookup"><span data-stu-id="c29a4-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="c29a4-190">Klasa bazowa skonstruowanego typu `G<int>` będzie `B<string,int[]>`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="c29a4-191">Bezpośrednie klasy bazowej typu klasy musi być co najmniej tak samo dostępna jak jej typ ([domeny dostępności](basic-concepts.md#accessibility-domains)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="c29a4-192">Na przykład, jest to błąd czasu kompilacji dla `public` pochodzi od klasy `private` lub `internal` klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="c29a4-193">Bezpośrednie klasy bazowej typu klasy nie może być dowolny z następujących typów: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, lub `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="c29a4-194">Ponadto nie można użyć deklaracji klasy ogólnej `System.Attribute` jako bezpośredniej lub pośredniej klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="c29a4-195">Podczas określania znaczenie specyfikacji bezpośrednią klasą bazową `A` klasy `B`, bezpośrednie klasy bazowej `B` tymczasowo zakłada się, że `object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="c29a4-196">Intuicyjnie daje to gwarancję, że znaczenie Specyfikacja klasy bazowej nie można rekursywnie zależeć od siebie samej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="c29a4-197">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c29a4-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="c29a4-198">Wystąpił błąd od w specyfikacji klasy bazowej `A<C.B>` bezpośrednia klasa podstawowa `C` jest uważany za `object`i dlatego (przez reguły [nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)) `C` nie jest uznawany za umieścić elementu członkowskiego `B`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="c29a4-199">Klasy bazowe typu klasy są bezpośredniej klasy podstawowej i jej klasy bazowe.</span><span class="sxs-lookup"><span data-stu-id="c29a4-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="c29a4-200">Innymi słowy zestaw klas bazowych jest przechodnia zamknięcia relacja bezpośrednia klasa bazowa.</span><span class="sxs-lookup"><span data-stu-id="c29a4-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="c29a4-201">Odwołujące się do przykładu powyżej klasy bazowe `B` są `A` i `object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="c29a4-202">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="c29a4-203">klasy bazowe `D<int>` są `C<int[]>`, `B<IComparable<int[]>>`, `A`, i `object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="c29a4-204">Z wyjątkiem klasy `object`, co typ klasy ma dokładnie jednego bezpośrednią klasą bazową.</span><span class="sxs-lookup"><span data-stu-id="c29a4-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="c29a4-205">`object` Klasa nie ma bezpośredniej klasy bazowej i jest ultimate klasa bazowa innych klas.</span><span class="sxs-lookup"><span data-stu-id="c29a4-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="c29a4-206">Gdy klasa `B` pochodzi z klasy `A`, jest to błąd czasu kompilacji dla `A` zależą `B`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="c29a4-207">Klasa ***zależy od bezpośrednio*** jego bezpośrednia klasa bazowa (jeśli istnieje) i ***zależy od bezpośrednio*** klasy, w którym od razu zagnieżdżenia (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="c29a4-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="c29a4-208">Biorąc pod uwagę tę definicję, kompletny zestaw klas, od których zależy od klasy jest zamknięcie zwrotnej i przechodni ***zależy od bezpośrednio*** relacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="c29a4-209">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="c29a4-210">jest błędne, ponieważ zależy od samej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="c29a4-211">Podobnie w tym przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="c29a4-212">jest w wyniku błędu, ponieważ klasy rekurencyjnie zależą od siebie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="c29a4-213">Na koniec przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="c29a4-214">powoduje błąd w czasie kompilacji, ponieważ `A` zależy od `B.C` (jego bezpośrednia klasa bazowa), która jest zależna od `B` (jego natychmiastowo otaczającą klasę), która rekurencyjnie jest zależna od `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="c29a4-215">Należy pamiętać, że klasa nie zależy od klas, które są w nim zagnieżdżony.</span><span class="sxs-lookup"><span data-stu-id="c29a4-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="c29a4-216">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="c29a4-217">`B` zależy od `A` (ponieważ `A` jest jego bezpośrednia klasa bazowa i jego natychmiastowo otaczającą klasę), ale `A` nie zależy od `B` (ponieważ `B` nie jest klasą bazową ani otaczającej klasy `A` ).</span><span class="sxs-lookup"><span data-stu-id="c29a4-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="c29a4-218">W efekcie przykładu jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-218">Thus, the example is valid.</span></span>

<span data-ttu-id="c29a4-219">Nie jest możliwe dziedziczyć `sealed` klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="c29a4-220">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="c29a4-221">Klasa `B` jest błąd, ponieważ próbuje on pochodzić od `sealed` klasy `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="c29a4-222">Implementacje interfejsu</span><span class="sxs-lookup"><span data-stu-id="c29a4-222">Interface implementations</span></span>

<span data-ttu-id="c29a4-223">A *class_base* specyfikacji może zawierać listę typów interfejsów, w których przypadku klasy jest nazywany bezpośrednio zaimplementować typy danego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="c29a4-224">Implementacje interfejsu są omówione w dalszych [implementacje interfejsu](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="c29a4-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="c29a4-225">Ograniczenia parametru typu</span><span class="sxs-lookup"><span data-stu-id="c29a4-225">Type parameter constraints</span></span>

<span data-ttu-id="c29a4-226">Ogólny deklaracje typu i metody można opcjonalnie określić ograniczenia parametru typu, umieszczając *type_parameter_constraints_clause*s.</span><span class="sxs-lookup"><span data-stu-id="c29a4-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

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

<span data-ttu-id="c29a4-227">Każdy *type_parameter_constraints_clause* składa się z tokenem `where`, następuje nazwa parametru typu, a następnie dwukropek i listy ograniczeń dla tego parametru typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="c29a4-228">Może być co najwyżej jeden `where` klauzulę dla każdego parametru typu i `where` klauzule mogą być wyświetlane w dowolnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="c29a4-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="c29a4-229">Podobnie jak `get` i `set` tokenów w metody dostępu właściwości `where` token nie jest słowem kluczowym.</span><span class="sxs-lookup"><span data-stu-id="c29a4-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="c29a4-230">Lista ograniczenia podany w `where` klauzuli może zawierać dowolne z następujących składników w następującej kolejności: jednego ograniczenia podstawowego, co najmniej jedno ograniczenie dodatkowej i ograniczenie konstruktora `new()`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="c29a4-231">Ograniczenia podstawowy może być typem klasy lub ***odwoływać ograniczenie typu*** `class` lub ***wartość ograniczenia typu*** `struct`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="c29a4-232">Może być ograniczenie dodatkowej *type_parameter* lub *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="c29a4-233">Ograniczenie typu odwołania określa, że argument typu, używany dla parametru typu musi być typem referencyjnym.</span><span class="sxs-lookup"><span data-stu-id="c29a4-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="c29a4-234">Wszystkie typy klas, typy interfejsów, typy delegatów, typy tablic i parametry typu, znane jako typu odwołania (zdefiniowany poniżej) spełnić tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="c29a4-235">Ograniczenie typu wartość określa, że argument typu, używany dla parametru typu musi być typem wartości niedopuszczającym wartości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="c29a4-236">Wszystkie typy nieprzyjmujące struktury, typach wyliczeniowych i parametry typu mające wartość ograniczenia typu spełnia tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="c29a4-237">Należy pamiętać, że chociaż sklasyfikowane jako typ wartości — Typ dopuszczający wartość null ([typów dopuszczających wartości zerowe](types.md#nullable-types)) nie spełnia ograniczenia typu wartości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="c29a4-238">Parametr typu o ograniczenie typu wartości nie może mieć również *constructor_constraint*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="c29a4-239">Typy wskaźników nigdy nie mogą mieć argumentów typu i nie są wliczane do zaspokojenia albo odwołanie do typu lub wartości typu ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="c29a4-240">W przypadku typu klasy, typ interfejsu lub parametrem typu ograniczenia typu określa minimalne "typ podstawowy", co argument typu, używany dla tego parametru typu muszą obsługiwać.</span><span class="sxs-lookup"><span data-stu-id="c29a4-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="c29a4-241">Zawsze, gdy jest używany skonstruowanego typu lub metody rodzajowej, argument typu jest sprawdzana względem ograniczenia dotyczące parametrów typu w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="c29a4-242">Podany argument typu musi spełniać warunki opisane w [spełniających ograniczenia](types.md#satisfying-constraints).</span><span class="sxs-lookup"><span data-stu-id="c29a4-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="c29a4-243">A *class_type* ograniczenie musi spełniać następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="c29a4-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="c29a4-244">Typ musi być typem klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-244">The type must be a class type.</span></span>
*  <span data-ttu-id="c29a4-245">Typ nie może być `sealed`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="c29a4-246">Typ nie może być jednym z następujących typów: `System.Array`, `System.Delegate`, `System.Enum`, lub `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="c29a4-247">Typ nie może być `object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-247">The type must not be `object`.</span></span> <span data-ttu-id="c29a4-248">Ponieważ wszystkie typy wyprowadzono z klasy `object`, ograniczenia typu miałaby żadnego efektu, jeśli jego były dozwolone.</span><span class="sxs-lookup"><span data-stu-id="c29a4-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="c29a4-249">Co najwyżej jedno ograniczenie dla danego typu parametru może być typem klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="c29a4-250">Typ określony jako *interface_type* ograniczenie musi spełniać następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="c29a4-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="c29a4-251">Typ musi być typem interfejsu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="c29a4-252">Typ nie może być określony więcej niż jeden raz w danym `where` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="c29a4-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="c29a4-253">W obu przypadkach ograniczenie może obejmować dowolny z parametrów typu skojarzony typ lub metoda deklaracji jako część skonstruowanego typu i może obejmować typ został zadeklarowany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="c29a4-254">Dowolny typ klasy lub interfejsu, określony jako ograniczenia parametru typu muszą być co najmniej tak samo dostępna ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)) jako typu ogólnego lub metody został zadeklarowany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="c29a4-255">Typ określony jako *type_parameter* ograniczenie musi spełniać następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="c29a4-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="c29a4-256">Typ musi być parametrem typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="c29a4-257">Typ nie może być określony więcej niż jeden raz w danym `where` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="c29a4-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="c29a4-258">Ponadto musi istnieć żadne cykle w wykresie zależności parametrów typu, w którym zależność jest relacja przechodnia zdefiniowane przez:</span><span class="sxs-lookup"><span data-stu-id="c29a4-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="c29a4-259">Jeśli parametr typu `T` jest używany jako ograniczenie parametru typu `S` następnie `S` ***zależy od*** `T`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="c29a4-260">Jeśli parametr typu `S` zależy od parametru typu `T` i `T` zależy od parametru typu `U` następnie `S` ***zależy od*** `U`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="c29a4-261">Biorąc pod uwagę tej relacji, jest to błąd czasu kompilacji, dla parametru typu była zależna od siebie samej (bezpośrednio lub pośrednio).</span><span class="sxs-lookup"><span data-stu-id="c29a4-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="c29a4-262">Wszelkich ograniczeń musi być takie same dla wszystkich parametrów typu zależne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="c29a4-263">Jeśli parametr typu `S` zależy od typu parametru `T` następnie:</span><span class="sxs-lookup"><span data-stu-id="c29a4-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="c29a4-264">`T` nie może mieć wartość typu ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="c29a4-265">W przeciwnym razie `T` skutecznie jest zapieczętowany tak `S` będą musieli się tego samego typu co `T`, eliminując konieczność łączenia się dwa parametry typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="c29a4-266">Jeśli `S` następnie posiada wartość ograniczenia typu `T` nie może mieć *class_type* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="c29a4-267">Jeśli `S` ma *class_type* ograniczenie `A` i `T` ma *class_type* ograniczenie `B` musi być konwersję tożsamości lub niejawne Konwersja odwołania `A` do `B` lub niejawna konwersja odwołania z `B` do `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="c29a4-268">Jeśli `S` zależy również od parametru typu `U` i `U` ma *class_type* ograniczenie `A` i `T` ma *class_type* ograniczenie `B` musi być konwersji tożsamości lub niejawna konwersja odwołania z `A` do `B` lub niejawna konwersja odwołania z `B` do `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="c29a4-269">Jest on prawidłowy dla `S` ma ograniczenia typu wartości i `T` ma ograniczenia typu odwołania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="c29a4-270">Skutecznie ogranicza `T` do typów `System.Object`, `System.ValueType`, `System.Enum`i dowolny typ interfejsu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="c29a4-271">Jeśli `where` klauzula dla parametru typu zawiera ograniczenie konstruktora (która ma postać `new()`), można użyć `new` operatora do utworzenia wystąpienia typu ([tworzenia wyrażeniaobiektów](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="c29a4-272">Dowolny typ argumentu dla parametru typu o ograniczenie konstruktora musi mieć publicznego konstruktora bez parametrów (ten konstruktor niejawnie istnieje, dla każdego typu wartości) lub być parametrem typu ograniczenia typu wartości lub ograniczenie konstruktora (patrz [Ograniczenia parametru typu](classes.md#type-parameter-constraints) Aby uzyskać szczegółowe informacje).</span><span class="sxs-lookup"><span data-stu-id="c29a4-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="c29a4-273">Poniżej przedstawiono przykłady ograniczeń:</span><span class="sxs-lookup"><span data-stu-id="c29a4-273">The following are examples of constraints:</span></span>
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

<span data-ttu-id="c29a4-274">Poniższy przykład jest błąd, ponieważ powoduje ona zależność cykliczną w wykresie zależności parametrów typu:</span><span class="sxs-lookup"><span data-stu-id="c29a4-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="c29a4-275">W poniższych przykładach pokazano też inne nieprawidłowe sytuacje:</span><span class="sxs-lookup"><span data-stu-id="c29a4-275">The following examples illustrate additional invalid situations:</span></span>
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

<span data-ttu-id="c29a4-276">***Skuteczne klasy bazowej*** parametru typu `T` jest zdefiniowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c29a4-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="c29a4-277">Jeśli `T` nie ograniczeń podstawowego lub ograniczenia parametru typu, klasą bazową skuteczne `object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="c29a4-278">Jeśli `T` posiada wartość ograniczenia typ jest klasą bazową skuteczne `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="c29a4-279">Jeśli `T` ma *class_type* ograniczenie `C` , ale nie *type_parameter* jest klasą bazową obowiązywać ograniczenia, `C`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="c29a4-280">Jeśli `T` nie ma przypisanego *class_type* ograniczenia, ale ma co najmniej jeden *type_parameter* ograniczenia, klasą bazową skuteczne jest najbardziej encompassed typu ([zniesione konwersji operatory](conversions.md#lifted-conversion-operators)) w zestawie skuteczne klasy bazowe jego *type_parameter* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="c29a4-281">Zasady spójności upewnij się, że istnieje najbardziej encompassed typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="c29a4-282">Jeśli `T` znajdują się oba *class_type* ograniczenia i co najmniej jeden *type_parameter* ograniczenia, klasą bazową skuteczne jest najbardziej encompassed typu ([zniesione konwersji operatory](conversions.md#lifted-conversion-operators)) w zestawie składający się z *class_type* ograniczenie `T` i skuteczne klasy bazowe jego *type_parameter* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="c29a4-283">Zasady spójności upewnij się, że istnieje najbardziej encompassed typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="c29a4-284">Jeśli `T` ma ograniczenia typu odwołania, ale nie *class_type* jest klasą bazową obowiązywać ograniczenia, `object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="c29a4-285">Na potrzeby tych zasad, jeśli T ma ograniczenie `V` czyli *value_type*, zamiast tego użyj specyficzny typ podstawowy elementu `V` czyli *class_type*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="c29a4-286">Nigdy nie może się zdarzyć w ograniczeniu jawnie danego, ale może wystąpić, gdy ograniczenia metody ogólnej niejawnie są dziedziczone przez nadrzędne deklaracji metody lub jawnych implementacji metody interfejsu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="c29a4-287">Te reguły upewnij się, że od klasy bazowej zawsze *class_type*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="c29a4-288">***Zestaw skuteczne interfejsu*** parametru typu `T` jest zdefiniowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c29a4-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="c29a4-289">Jeśli `T` nie ma przypisanego *secondary_constraints*, jego zestaw skuteczne interfejsu jest pusty.</span><span class="sxs-lookup"><span data-stu-id="c29a4-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="c29a4-290">Jeśli `T` ma *interface_type* ograniczeń, ale nie *type_parameter* ograniczenia, jego zestawu skuteczne interfejsu jest jej zestaw *interface_type* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="c29a4-291">Jeśli `T` nie ma przypisanego *interface_type* ale ma ograniczenia *type_parameter* ograniczenia, jego zestaw skuteczne interfejsu jest złożenie zestawy skuteczne interfejsu jego *type_ Parametr* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="c29a4-292">Jeśli `T` znajdują się oba *interface_type* ograniczeń i *type_parameter* ograniczenia, jego zestaw skuteczne interfejsu jest złożenie swój zestaw *interface_type* ograniczenia i zestawy skuteczne interfejsu jego *type_parameter* ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="c29a4-293">Parametr typu jest ***znane jako typ referencyjny*** jeśli ma ograniczenia typu odwołania lub nie jest klasą bazową skuteczne `object` lub `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="c29a4-294">Wartości typu typu parametru ograniczonego typu może służyć do dostępu do składowych wystąpienia też dorozumianych przez ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="c29a4-295">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-295">In the example</span></span>
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
<span data-ttu-id="c29a4-296">metody `IPrintable` może być wywoływany bezpośrednio na `x` ponieważ `T` jest ograniczony do zaimplementowania zawsze `IPrintable`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="c29a4-297">Treści klasy</span><span class="sxs-lookup"><span data-stu-id="c29a4-297">Class body</span></span>

<span data-ttu-id="c29a4-298">*Class_body* klasy definiuje elementy członkowskie tej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="c29a4-299">Typy częściowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-299">Partial types</span></span>

<span data-ttu-id="c29a4-300">Deklaracja typu mogą być dzielone między wieloma ***deklaracje typu częściowego***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="c29a4-301">Deklaracja typu jest tworzona z jego części postępując zgodnie z zasadami określonymi w tej sekcji — jest ona traktowana jako pojedyncza deklaracja w pozostałym czasie przetwarzania kompilacji i czasu wykonywania programu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="c29a4-302">A *class_declaration*, *struct_declaration* lub *interface_declaration* reprezentuje deklarację typu częściowego, jeśli zawiera on `partial` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="c29a4-303">`partial` nie jest słowem kluczowym i tylko działa jako modyfikatory, jeśli zostanie wyświetlona bezpośrednio przed słowa kluczowe `class`, `struct` lub `interface` w deklaracji typu lub przed typem `void` w deklaracji metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="c29a4-304">W innych kontekstach może służyć jako identyfikator normalny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="c29a4-305">Każda część deklaracji typu częściowego musi zawierać `partial` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="c29a4-306">On musi mieć taką samą nazwę i być zadeklarowana w tej samej przestrzeni nazw lub deklaracja typu jako inne części.</span><span class="sxs-lookup"><span data-stu-id="c29a4-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="c29a4-307">`partial` Modyfikator wskazuje dodatkowe elementy deklaracji typu może istnieć w innym miejscu, że istnienie takich dodatkowe elementy nie jest to wymagane, jest on prawidłowy dla typu przy użyciu jednej deklaracji, aby uwzględnić `partial` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="c29a4-308">Wszystkie części typu częściowego muszą być skompilowane ze sobą tak, aby części mogą być scalone w czasie kompilacji w jedną deklarację typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="c29a4-309">Typy częściowe wyraźnie zezwala na już skompilowane typy można rozszerzyć.</span><span class="sxs-lookup"><span data-stu-id="c29a4-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="c29a4-310">Zagnieżdżone typy mogą być zadeklarowane w wiele części za pomocą `partial` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="c29a4-311">Zazwyczaj typ zawierający jest zadeklarowany za pomocą `partial` jak dobrze, a każda część typu zagnieżdżonego jest zadeklarowana w innej części typu zawierającego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="c29a4-312">`partial` Modyfikator nie jest dozwolone w deklaracjach typu delegata lub typu wyliczeniowego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="c29a4-313">Atrybuty</span><span class="sxs-lookup"><span data-stu-id="c29a4-313">Attributes</span></span>

<span data-ttu-id="c29a4-314">Atrybuty typu częściowego są określane przez połączenie w nieokreślonej kolejności atrybutów każdej części.</span><span class="sxs-lookup"><span data-stu-id="c29a4-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="c29a4-315">Jeśli atrybut znajduje się na wiele części, jest równoznaczne z użyciem atrybutu wielokrotnie w typie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="c29a4-316">Na przykład dwie części:</span><span class="sxs-lookup"><span data-stu-id="c29a4-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="c29a4-317">są równoważne z deklaracji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="c29a4-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="c29a4-318">Atrybuty w parametrach typu łączyć się w podobny sposób.</span><span class="sxs-lookup"><span data-stu-id="c29a4-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="c29a4-319">Modyfikatory</span><span class="sxs-lookup"><span data-stu-id="c29a4-319">Modifiers</span></span>

<span data-ttu-id="c29a4-320">Gdy deklarację typu częściowego obejmuje specyfikacji ułatwień dostępu ( `public`, `protected`, `internal`, i `private` Modyfikatory) musisz zaakceptować z innymi częściami obejmujących specyfikacji ułatwień dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="c29a4-321">Jeśli żadna część typu częściowego, zawiera specyfikację ułatwień dostępu, typ podano dostępności odpowiednią wartość domyślną ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="c29a4-322">Jeśli zawierają co najmniej jeden częściowe deklaracje typu zagnieżdżonego `new` modyfikator, ostrzeżenie nie jest zgłaszany, gdy typ zagnieżdżony ukrywa dziedziczonej składowej ([ukrywanie poprzez dziedziczenie](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="c29a4-323">Jeśli co najmniej jeden częściowe deklaracje klasy zawierają `abstract` , modyfikator klasy jest uznawany za abstrakcyjny ([klasy abstrakcyjne](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="c29a4-324">W przeciwnym razie klasy jest traktowany jako nieabstrakcyjne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="c29a4-325">Jeśli zawierają co najmniej jeden częściowe deklaracje klasy `sealed` , modyfikator klasy jest uznawany za zapieczętowany ([zapieczętowanych klas](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="c29a4-326">W przeciwnym razie jest uznawany za klasy niezapieczętowany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="c29a4-327">Należy pamiętać, że klasa nie może być abstrakcyjny i zapieczętowany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="c29a4-328">Gdy `unsafe` modyfikator jest używany w deklaracji typu częściowego, tylko tej określonej części jest uznawany za niebezpiecznym kontekście ([niebezpieczny kontekst](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="c29a4-329">Parametry typu i ograniczenia</span><span class="sxs-lookup"><span data-stu-id="c29a4-329">Type parameters and constraints</span></span>

<span data-ttu-id="c29a4-330">Jeśli typ ogólny jest zadeklarowany w wielu części, każda część musi podać parametry typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="c29a4-331">Każda część musi mieć taką samą liczbę parametrów typu i taką samą nazwę dla każdego parametru typu, w kolejności.</span><span class="sxs-lookup"><span data-stu-id="c29a4-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="c29a4-332">Po deklaracji typu rodzajowego częściowego obejmuje ograniczenia (`where` klauzule), ograniczenia należy uzgodnić z innymi częściami obejmujących ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="c29a4-333">Każda część obejmującą ograniczenia muszą mieć ograniczenia dla tego samego zestawu parametrów typu i dla każdego parametru typu zestawy główne, pomocniczej i ograniczenia konstruktora musi być równoważne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="c29a4-334">Dwa zestawy warunków ograniczających są równoważne, jeśli zawierają one te same elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="c29a4-335">Jeśli żadna część typu rodzajowego częściowe określa ograniczenia parametru typu, parametry są traktowane jako typu nieograniczone.</span><span class="sxs-lookup"><span data-stu-id="c29a4-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="c29a4-336">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-336">The example</span></span>
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
<span data-ttu-id="c29a4-337">jest poprawna, ponieważ te części, które obejmują skutecznie ograniczenia (dwie pierwsze) należy określić sam zestaw podstawowego, pomocniczego i ograniczenia konstruktora dla tego samego zestawu parametrów typu odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="c29a4-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="c29a4-338">Klasa bazowa</span><span class="sxs-lookup"><span data-stu-id="c29a4-338">Base class</span></span>

<span data-ttu-id="c29a4-339">Deklaracji klasy częściowej tym Specyfikacja klasy bazowej muszą wyrazić zgodę z innymi częściami obejmujących Specyfikacja klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="c29a4-340">Jeśli żadna z części klasy częściowej zawiera specyfikacji klasa bazowa, klasy bazowej staje się `System.Object` ([klas bazowych](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="c29a4-341">Interfejsy podstawowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-341">Base interfaces</span></span>

<span data-ttu-id="c29a4-342">Zbiór interfejsy podstawowe w odniesieniu do typu zadeklarowany w wielu częściach jest złożenie interfejsy podstawowe określone w każdej części.</span><span class="sxs-lookup"><span data-stu-id="c29a4-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="c29a4-343">Określonego interfejs podstawowy tylko nazwę raz w każdej części, ale jest dozwolony w przypadku wielu składa się z części nazwy tego samego bazowych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="c29a4-344">Musi istnieć tylko jedna implementacja członków dowolnej podanej interfejs podstawowy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="c29a4-345">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="c29a4-346">Zbiór interfejsy podstawowe klasy `C` jest `IA`, `IB`, i `IC`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="c29a4-347">Zwykle każda część zapewnia implementacja interfejsy zadeklarowanych w tej części; jednak nie jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="c29a4-348">Części mogą dostarczać implementację interfejsu zadeklarowanych w innej części.</span><span class="sxs-lookup"><span data-stu-id="c29a4-348">A part may provide the implementation for an interface declared on a different part:</span></span>
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

### <a name="members"></a><span data-ttu-id="c29a4-349">Elementy członkowskie</span><span class="sxs-lookup"><span data-stu-id="c29a4-349">Members</span></span>

<span data-ttu-id="c29a4-350">Z wyjątkiem metody częściowe ([metod częściowych](classes.md#partial-methods)), zestaw elementów członkowskich typu zadeklarowany w wielu częściach to po prostu związek zestaw elementów członkowskich zadeklarowanych w każdej części.</span><span class="sxs-lookup"><span data-stu-id="c29a4-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="c29a4-351">Treści wszystkich części deklaracji typu współużytkują ten sam obszar deklaracji ([deklaracje](basic-concepts.md#declarations)) oraz zakres każdego członka ([zakresy](basic-concepts.md#scopes)) rozszerza jednostkom wszystkie części.</span><span class="sxs-lookup"><span data-stu-id="c29a4-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="c29a4-352">Domena dostępności członka zawsze zawiera wszystkie elementy typu otaczającego; `private` składowa zadeklarowana w jednej części jest ogólnie dostępne od innego składnika.</span><span class="sxs-lookup"><span data-stu-id="c29a4-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="c29a4-353">Jest błąd kompilacji, aby zadeklarować ten sam element członkowski w więcej niż jedną część typu, chyba że ten element jest typu z `partial` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

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

<span data-ttu-id="c29a4-354">Kolejność elementów członkowskich w ramach danego typu jest rzadko istotny dla kodu C#, ale mogą być istotne w przypadku komunikowanie się z innych języków i środowisk.</span><span class="sxs-lookup"><span data-stu-id="c29a4-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="c29a4-355">W takich przypadkach kolejność elementów członkowskich w ramach zadeklarowany w wielu częściach typu jest niezdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="c29a4-356">Metody częściowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-356">Partial methods</span></span>

<span data-ttu-id="c29a4-357">Metody częściowe mogą być definiowane w jednej części deklaracji typu i zaimplementowane w innym.</span><span class="sxs-lookup"><span data-stu-id="c29a4-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="c29a4-358">Implementacja jest opcjonalna. Jeśli żadna część implementuje metody częściowej, deklaracji metody częściowej i wszystkie wywołania do niego są usuwane z deklaracji typu wynikające z kombinacji części.</span><span class="sxs-lookup"><span data-stu-id="c29a4-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="c29a4-359">Metod częściowych nie można zdefiniować modyfikatory dostępu, ale są niejawną kolekcją `private`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="c29a4-360">Ich zwracanym typem musi być `void`, i ich parametrów nie może mieć `out` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="c29a4-361">Identyfikator `partial` jest rozpoznawana jako specjalne — słowo kluczowe w deklaracji metody, tylko wtedy, gdy znajdzie się on bezpośrednio przed `void` typu; w przeciwnym razie może służyć jako identyfikator normalny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="c29a4-362">Metoda częściowa nie może jawnie implementować metody interfejsu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="c29a4-363">Istnieją dwa rodzaje częściowe deklaracje metody: Jeśli treść deklaracji metody średnikiem, deklaracja jest nazywany ***Definiowanie deklaracji metody częściowej***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="c29a4-364">Jeśli treść jest podawana jako *bloku*, deklaracja jest nazywany ***Implementowanie deklaracji metody częściowej***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="c29a4-365">W części deklaracji typu może istnieć tylko jeden Definiowanie deklaracji metody częściowej za pomocą podanego podpisu i może istnieć tylko jedna implementacja deklaracji metody częściowej za pomocą podanego podpisu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="c29a4-366">Jeśli podano deklaracji implementującej metody częściowej, odpowiedni Definiowanie deklaracji metody częściowej musi istnieć i deklaracji musi odpowiadać jako określona poniżej:</span><span class="sxs-lookup"><span data-stu-id="c29a4-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="c29a4-367">Deklaracje muszą mieć ten sam Modyfikatory (ale nie musi to być w tej samej kolejności), nazwę metody, liczbę parametrów typu i liczby parametrów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="c29a4-368">Odpowiednich parametrów w deklaracjach muszą mieć ten sam Modyfikatory (ale nie musi to być w tej samej kolejności) i tymi samymi typami (modulo różnice w nazwach parametrów typu).</span><span class="sxs-lookup"><span data-stu-id="c29a4-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="c29a4-369">Odpowiednich parametrów typu w deklaracji musi mieć tego samego ograniczenia (modulo różnice w nazwach parametrów typu).</span><span class="sxs-lookup"><span data-stu-id="c29a4-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="c29a4-370">Deklaracji implementującej metody częściowej może znajdować się w tej samej części jako odpowiednie definiujące deklaracji metody częściowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="c29a4-371">Tylko definiowanie metody częściowej uczestniczy w przeciążeniu rozdzielczości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="c29a4-372">W związku z tym czy podano deklaracji implementującej, wyrażenia wywołania mogą prowadzić do wywołania metody częściowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="c29a4-373">Ponieważ metoda częściowa zawsze zwraca `void`, takie wyrażenia wywołania będą zawsze instrukcje wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="c29a4-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="c29a4-374">Ponadto ponieważ metoda częściowa jest niejawnie `private`, tych instrukcji będą zawsze wykonywane w ramach jednej części deklaracji typu, w którym zadeklarowano metody częściowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="c29a4-375">Jeśli żadna część deklaracji typu częściowego zawiera deklaracji implementującej dla danej metody częściowej, każda instrukcja wyrażenia wywołanie go po prostu zostanie usunięty z deklaracji typu połączone.</span><span class="sxs-lookup"><span data-stu-id="c29a4-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="c29a4-376">Ten sposób wywołania wyrażenie, tym żadnych składowych wyrażeń, nie ma znaczenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="c29a4-377">Sama metoda częściowa zostaną również usunięte i nie będzie należeć do deklaracji typu połączone.</span><span class="sxs-lookup"><span data-stu-id="c29a4-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="c29a4-378">Jeśli deklaracji implementującej istnieje dla danej metody częściowej, wywołań metod częściowych zostaną zachowane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="c29a4-379">Metoda częściowa powoduje powstanie deklaracji metody, podobne do deklaracji implementującej metody częściowej z wyjątkiem następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c29a4-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="c29a4-380">`partial` Modyfikator nie jest dołączony</span><span class="sxs-lookup"><span data-stu-id="c29a4-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="c29a4-381">Atrybuty w deklaracji metody wynikowe są połączone atrybutów definiowania i deklaracji implementującej metody częściowej w nieokreślonej kolejności.</span><span class="sxs-lookup"><span data-stu-id="c29a4-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="c29a4-382">Duplikaty nie zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="c29a4-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="c29a4-383">Atrybuty w parametrach wynikowy deklaracji metody są połączone atrybuty odpowiednich parametrów definiowania i deklaracji implementującej metody częściowej w nieokreślonej kolejności.</span><span class="sxs-lookup"><span data-stu-id="c29a4-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="c29a4-384">Duplikaty nie zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="c29a4-384">Duplicates are not removed.</span></span>

<span data-ttu-id="c29a4-385">Jeśli dla metody częściowej M jest deklaracją definiującą, ale nie deklaracji implementującej, obowiązują następujące ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="c29a4-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="c29a4-386">Jest to błąd czasu kompilacji do utworzenia delegata do metody ([delegować Tworzenie wyrażenia](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="c29a4-387">Jest to błąd kompilacji, aby odwołać się do `M` wewnątrz funkcja anonimowa, która jest konwertowana na typ drzewa wyrażeń ([oceny funkcja anonimowa konwersji na typy drzewa wyrażenie](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="c29a4-388">Wyrażenia, które pojawiają się jako część wywołania `M` nie wpływają na stan asercję określonego przypisania ([asercję określonego przypisania](variables.md#definite-assignment)), co potencjalnie może prowadzić do błędów kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="c29a4-389">`M` nie może być punktem wejścia dla aplikacji ([uruchamiania aplikacji](basic-concepts.md#application-startup)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="c29a4-390">Metody częściowe są przydatne do umożliwiając jedną część deklaracji typu umożliwia dostosowywanie zachowania innej części, np. taki, który jest generowany przez narzędzie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="c29a4-391">Rozważmy następującą deklarację klasy częściowej:</span><span class="sxs-lookup"><span data-stu-id="c29a4-391">Consider the following partial class declaration:</span></span>
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

<span data-ttu-id="c29a4-392">Jeśli ta klasa jest skompilowana bez żadnych innych części, definiującą deklaracje metody częściowej i ich wywołania zostaną usunięte, a wynikowy deklaracji klasy połączone będzie odpowiednikiem następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c29a4-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
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

<span data-ttu-id="c29a4-393">Załóżmy, że innej części zostanie podany, jednak zapewniającą deklaracji implementującej metody częściowe:</span><span class="sxs-lookup"><span data-stu-id="c29a4-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
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

<span data-ttu-id="c29a4-394">Następnie wynikowy deklaracji klasy połączone będzie odpowiednikiem następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c29a4-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
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

### <a name="name-binding"></a><span data-ttu-id="c29a4-395">Powiązania nazwy</span><span class="sxs-lookup"><span data-stu-id="c29a4-395">Name binding</span></span>

<span data-ttu-id="c29a4-396">Mimo że każda część rozszerzalnego typu musi być zadeklarowany w tej samej przestrzeni nazw, części są zwykle pisane w obrębie deklaracji innej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="c29a4-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="c29a4-397">W efekcie różnych `using` dyrektywy ([dyrektywy Using](namespaces.md#using-directives)) może być stosowany w przypadku każdej części.</span><span class="sxs-lookup"><span data-stu-id="c29a4-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="c29a4-398">Interpretując nazwy proste ([wnioskowanie o typie](expressions.md#type-inference)) w ramach jednej strony, tylko `using` dyrektywy deklarację przestrzeni nazw, otaczający tej części są traktowane jako.</span><span class="sxs-lookup"><span data-stu-id="c29a4-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="c29a4-399">Może to spowodować, że ten sam identyfikator mających różne znaczenie w różnych częściach:</span><span class="sxs-lookup"><span data-stu-id="c29a4-399">This may result in the same identifier having different meanings in different parts:</span></span>
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

## <a name="class-members"></a><span data-ttu-id="c29a4-400">Elementy członkowskie klasy</span><span class="sxs-lookup"><span data-stu-id="c29a4-400">Class members</span></span>

<span data-ttu-id="c29a4-401">Elementy członkowskie klasy składają się z elementów członkowskich, wynikające z jego *class_member_declaration*s i elementy członkowskie odziedziczone po bezpośredniej klasie bazowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

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

<span data-ttu-id="c29a4-402">Elementy członkowskie typu klasy są podzielone na następujące kategorie:</span><span class="sxs-lookup"><span data-stu-id="c29a4-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="c29a4-403">Stałe, które reprezentują stałe wartości skojarzone z klasą ([stałe](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="c29a4-404">Pola, które są zmiennymi klasy ([pola](classes.md#fields)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="c29a4-405">Metody, które implementują obliczeń i akcji, które mogą być wykonywane przez klasę ([metody](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="c29a4-406">Właściwości, które definiują o nazwie właściwości i akcje związane z odczytywaniem i zapisywaniem tych cech ([właściwości](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="c29a4-407">Zdarzenia, które definiują powiadomień, które mogą być generowane przez klasę ([zdarzenia](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="c29a4-408">Indeksatory, umożliwiające wystąpień klasy, która ma zostać pomyślnie zindeksowane w taki sam sposób (składniowo) jako tablice ([indeksatory](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="c29a4-409">Operatory, które określają Operatory wyrażeń, które mogą być stosowane do wystąpień klasy ([operatory](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="c29a4-410">Wystąpienie konstruktorów implementować akcje wymagane to inicjalizacji wystąpień klasy ([wystąpienia konstruktory](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="c29a4-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="c29a4-411">Destruktory, implementujących czynności, które należy wykonać, zanim wystąpienia klasy trwale zostaną odrzucone ([destruktory](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="c29a4-412">Konstruktory statyczne, implementujących działania wymagane w celu zainicjowania samej klasy ([konstruktorów statycznych](classes.md#static-constructors)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="c29a4-413">Typy, które reprezentują typy, które są lokalne w klasie ([zagnieżdżone typy](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="c29a4-414">Elementy członkowskie, które mogą zawierać kod wykonywalny są nazywane zbiorczo *funkcji elementów członkowskich* o typie danej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="c29a4-415">Funkcja elementów członkowskich typu klasy są metody, właściwości, zdarzenia, indeksatory, operatory, konstruktory wystąpień, destruktory i konstruktorów statycznych tego typu klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="c29a4-416">A *class_declaration* tworzy nowe miejsce do deklaracji ([deklaracje](basic-concepts.md#declarations)), a *class_member_declaration*s natychmiast zawartych *klasy _declaration* wprowadzenia nowych członków do tej deklaracji przestrzeni.</span><span class="sxs-lookup"><span data-stu-id="c29a4-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="c29a4-417">Następujące reguły mają zastosowanie do *class_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c29a4-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="c29a4-418">Konstruktory wystąpień, destruktory i konstruktorów statycznych musi mieć taką samą nazwę jako natychmiastowo otaczającą klasę.</span><span class="sxs-lookup"><span data-stu-id="c29a4-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="c29a4-419">Wszystkie inne elementy członkowskie muszą mieć nazwy, które różnią się od nazwy natychmiastowo otaczającą klasę.</span><span class="sxs-lookup"><span data-stu-id="c29a4-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="c29a4-420">Nazwa — stała, pola, właściwości, zdarzenia lub typ musi się różnić od nazw innych składowych zadeklarowanych w tej samej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="c29a4-421">Nazwa metody muszą różnić się od nazw wszystkich innych niż metod zadeklarowanych w tej samej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="c29a4-422">Ponadto, podpis ([podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading)) z metody muszą różnić się od podpisy wszystkich pozostałych metod zadeklarowanych w tej samej klasie i dwie metody zadeklarowanej w tej samej klasy nie może mieć podpisów, które różnią się jedynie przez `ref` i `out`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="c29a4-423">Podpis konstruktora wystąpienia muszą różnić się od podpisy wszystkie inne konstruktory wystąpień zadeklarowane w tej samej klasy, a dwa konstruktory zadeklarowane w tej samej klasy nie może mieć podpisów, które różnią się jedynie przez `ref` i `out`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="c29a4-424">Podpis indeksatora musi się różnić od podpisy innymi indeksatorami zadeklarowane w tej samej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="c29a4-425">Podpis operatora musi się różnić od podpisy innych operatorów zadeklarowane w tej samej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="c29a4-426">Dziedziczone składowe typu klasy ([dziedziczenia](classes.md#inheritance)) nie są częścią przestrzeni deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="c29a4-427">W związku z tym Klasa pochodna może zadeklarować członka o tej samej nazwie lub podpisie, jako dziedziczonej składowej (co w efekcie ukrywa dziedziczoną składową).</span><span class="sxs-lookup"><span data-stu-id="c29a4-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="c29a4-428">Typ wystąpienia</span><span class="sxs-lookup"><span data-stu-id="c29a4-428">The instance type</span></span>

<span data-ttu-id="c29a4-429">Każdy deklaracji klasy jest skojarzony typ powiązanej ([powiązane i wskazanych typów](types.md#bound-and-unbound-types)), ***typu wystąpienia***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="c29a4-430">Dla deklaracji klasy ogólnej, typu wystąpienia jest tworzona przez tworzenie skonstruowanego typu ([skonstruowany typy](types.md#constructed-types)) z deklaracji typu z każdą z podanego typu argumentów jest odpowiedni parametr typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="c29a4-431">Ponieważ typ wystąpienia używa parametrów typu, jego można używać tylko gdy parametry typu są w zakresie; oznacza to wewnątrz deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="c29a4-432">Typ wystąpienia jest typem `this` dla kodu napisanego w obrębie deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="c29a4-433">Dla klas nieogólnego Typ wystąpienia jest po prostu deklarowanej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="c29a4-434">Poniżej przedstawiono kilka deklaracji klasy, wraz z ich typów wystąpień:</span><span class="sxs-lookup"><span data-stu-id="c29a4-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="c29a4-435">Członkowie typy utworzone</span><span class="sxs-lookup"><span data-stu-id="c29a4-435">Members of constructed types</span></span>

<span data-ttu-id="c29a4-436">Dziedziczone elementy członkowskie skonstruowanego typu są uzyskiwane poprzez zastąpienie dla każdego *type_parameter* w deklaracji elementu członkowskiego, odpowiedni *type_argument* skonstruowanego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="c29a4-437">Proces podstawienia opiera się na znaczenia semantycznego deklaracje typu i nie jest po prostu tekstową podstawienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="c29a4-438">Na przykład biorąc pod uwagę deklaracji klasy ogólnej</span><span class="sxs-lookup"><span data-stu-id="c29a4-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="c29a4-439">skonstruowany typ `Gen<int[],IComparable<string>>` ma następujące składowe:</span><span class="sxs-lookup"><span data-stu-id="c29a4-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="c29a4-440">Typ elementu członkowskiego `a` w deklaracji klasy ogólnej `Gen` jest "dwuwymiarową tablicę `T`", więc typ elementu członkowskiego `a` w skonstruowanego typu powyżej jest "dwuwymiarową tablicę jednowymiarową tablicę `int`", lub `int[,][]`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="c29a4-441">W ramach wystąpienia funkcji członków, typ `this` jest typem wystąpienia ([typu wystąpienia](classes.md#the-instance-type)) zawierający deklaracji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="c29a4-442">Wszystkie elementy członkowskie klasy generycznej można używać parametrów typu w otaczającej klasy, bezpośrednio lub jako część skonstruowanego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="c29a4-443">Po zamknięciu określonego skonstruowany typ ([otwarte i zamknięte typy](types.md#open-and-closed-types)) jest używany w czasie wykonywania, każde użycie parametru typu jest zastępowany rzeczywisty typ argumentu dostarczane do skonstruowanego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="c29a4-444">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c29a4-444">For example:</span></span>
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

### <a name="inheritance"></a><span data-ttu-id="c29a4-445">Dziedziczenie</span><span class="sxs-lookup"><span data-stu-id="c29a4-445">Inheritance</span></span>

<span data-ttu-id="c29a4-446">Klasa ***dziedziczy*** członkowie jej typ bezpośredniej klasie bazowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="c29a4-447">Dziedziczenie oznacza, że klasa niejawnie zawiera wszystkie elementy członkowskie typu bezpośrednią klasą bazową, z wyjątkiem wystąpienia konstruktory, destruktory i konstruktory statyczne klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="c29a4-448">Niektórych ważnych aspektów dziedziczenia są następujące:</span><span class="sxs-lookup"><span data-stu-id="c29a4-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="c29a4-449">Dziedziczenie jest przechodnia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-449">Inheritance is transitive.</span></span> <span data-ttu-id="c29a4-450">Jeśli `C` jest tworzony na podstawie `B`, i `B` jest tworzony na podstawie `A`, następnie `C` dziedziczy elementów członkowskich zadeklarowanych w `B` oraz elementów członkowskich zadeklarowanych w `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="c29a4-451">Klasa pochodna rozszerza jego bezpośrednia klasa bazowa.</span><span class="sxs-lookup"><span data-stu-id="c29a4-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="c29a4-452">Klasy pochodne mogą dodawać nowych członków do tych, które dziedziczy, ale go nie można usunąć definicji dziedziczonego członka.</span><span class="sxs-lookup"><span data-stu-id="c29a4-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="c29a4-453">Konstruktory wystąpień, destruktory i konstruktory statyczne nie są dziedziczone, ale wszystkie inne elementy członkowskie są, niezależnie od ich deklarowana dostępność metody ([dostęp do elementu członkowskiego](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="c29a4-454">Jednak w zależności od ich deklarowana dostępność metody dziedziczone elementy Członkowskie mogą być niedostępne w klasie pochodnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="c29a4-455">Klasa pochodna może ***Ukryj*** ([ukrywanie poprzez dziedziczenie](basic-concepts.md#hiding-through-inheritance)) odziedziczone składowe przez zadeklarowanie nowych elementów członkowskich z tej samej nazwie lub podpisie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="c29a4-456">Pamiętaj jednak, ukrywać dziedziczonego członka nie powoduje usunięcia tego elementu członkowskiego — to jedynie sprawia, że ten element członkowski dostępna bezpośrednio za pośrednictwem klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="c29a4-457">Wystąpienie klasy zawiera zbiór wszystkie pola wystąpienia zadeklarowana w klasie i jej klasy bazowe i niejawną konwersję ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) istnieje typ klasy pochodnej dowolny z jej typów klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="c29a4-458">W związku z tym odwołanie do wystąpienia klasy pochodnej niektóre mogą być traktowane jako odwołanie do wystąpienia któregokolwiek z jej klas podstawowych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="c29a4-459">Klasę można zadeklarować wirtualne metody, właściwości i indeksatorów i zastępować implementację z tych funkcji składowych klas pochodnych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="c29a4-460">Dzięki temu klasy do następującej liczby etapów stwierdzono zachowanie polimorficzne, w którym akcje wykonywane przez wywołanie funkcji Członkowskich różni się zależnie od typu run-time wystąpienia za pomocą którego ten element członkowski funkcja jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="c29a4-461">Dziedziczonego elementu członkowskiego typu klasy skonstruowane są członkami natychmiastowego klasy podstawowej typu ([klas bazowych](classes.md#base-classes)), który znajduje się poprzez zastąpienie argumentów typu skonstruowanego typu dla każdego wystąpienia danego typu Parametry w *class_base* specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="c29a4-462">Te elementy członkowskie z kolei są przekształcane wstawiając dla każdego *type_parameter* w deklaracji elementu członkowskiego, odpowiedni *type_argument* z *class_base* Specyfikacja.</span><span class="sxs-lookup"><span data-stu-id="c29a4-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

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

<span data-ttu-id="c29a4-463">W powyższym przykładzie skonstruowanego typu `D<int>` ma składową dziedziczone `public int G(string s)` uzyskane poprzez zastąpienie argumentów typu `int` dla parametru typu `T`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="c29a4-464">`D<int>` ma również odziedziczonej składowej przed deklaracją klasy `B`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="c29a4-465">Ta dziedziczonego elementu członkowskiego jest określana przez pierwszy określająca typ klasy bazowej `B<int[]>` z `D<int>` wstawiając `int` dla `T` w specyfikacji klasy bazowej `B<T[]>`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="c29a4-466">Następnie jako argument typu `B`, `int[]` zostanie zastąpiony `U` w `public U F(long index)`, którego można dziedziczonego elementu członkowskiego `public int[] F(long index)`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="c29a4-467">New — modyfikator</span><span class="sxs-lookup"><span data-stu-id="c29a4-467">The new modifier</span></span>

<span data-ttu-id="c29a4-468">A *class_member_declaration* może zadeklarować członka o tej samej nazwie lub podpisie, jako dziedziczonego członka.</span><span class="sxs-lookup"><span data-stu-id="c29a4-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="c29a4-469">W takiej sytuacji składowej klasy pochodnej jest nazywany ***Ukryj*** składowej klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="c29a4-470">Ukrywanie dziedziczonej składowej nie jest uważany za błąd, ale spowodować, że kompilator generuje ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="c29a4-471">Aby pominąć to ostrzeżenie, może zawierać deklaracji składowej klasy pochodnej `new` modyfikator, aby wskazać, że składowa pochodnej jest przeznaczona do Ukryj składową bazową.</span><span class="sxs-lookup"><span data-stu-id="c29a4-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="c29a4-472">W tym temacie omówiono w dalszej [ukrywanie poprzez dziedziczenie](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="c29a4-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="c29a4-473">Jeśli `new` modyfikator znajduje się w deklaracji, która nie ukrywa dziedziczonego członka, w tym celu jest wyświetlane ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="c29a4-474">To ostrzeżenie jest pominięty, usuwając `new` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="c29a4-475">Modyfikatory dostępu</span><span class="sxs-lookup"><span data-stu-id="c29a4-475">Access modifiers</span></span>

<span data-ttu-id="c29a4-476">A *class_member_declaration* może mieć dowolną pięć rodzajów deklarowana dostępność metody ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` , lub `private`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="c29a4-477">Z wyjątkiem `protected internal` kombinacji, jest to błąd czasu kompilacji, można określić więcej niż jeden modyfikator dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="c29a4-478">Gdy *class_member_declaration* nie zawiera żadnych modyfikatory dostępu `private` zakłada, że.</span><span class="sxs-lookup"><span data-stu-id="c29a4-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="c29a4-479">Typy składowych</span><span class="sxs-lookup"><span data-stu-id="c29a4-479">Constituent types</span></span>

<span data-ttu-id="c29a4-480">Typy, które są używane w deklaracji elementu członkowskiego są nazywane składowych typów tego członka.</span><span class="sxs-lookup"><span data-stu-id="c29a4-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="c29a4-481">Możliwe typy składowych to typ stałej, pola, właściwości, zdarzenia lub indeksator, zwracany typ metody lub operatora i typy parametrów metody, indeksator, operator lub konstruktora wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="c29a4-482">Typy składowych elementu członkowskiego musi być co najmniej tak samo dostępna jak ten sam element członkowski ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="c29a4-483">Statyczna i wystąpienia elementów członkowskich</span><span class="sxs-lookup"><span data-stu-id="c29a4-483">Static and instance members</span></span>

<span data-ttu-id="c29a4-484">Elementy członkowskie klasy są albo ***statyczne elementy członkowskie*** lub ***wystąpieniami elementów członkowskich***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="c29a4-485">Ogólnie rzecz biorąc warto traktować statyczne elementy członkowskie jako należące do typów klas i składowych wystąpienia jako należące do obiektów (wystąpienia typów klas)</span><span class="sxs-lookup"><span data-stu-id="c29a4-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="c29a4-486">Kiedy zawiera deklarację pola, metody, właściwości, zdarzenia, operator lub Konstruktor `static` modyfikator, deklaruje składową statyczną.</span><span class="sxs-lookup"><span data-stu-id="c29a4-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="c29a4-487">Ponadto deklaracji stałą lub typ niejawnie deklaruje statyczny element członkowski.</span><span class="sxs-lookup"><span data-stu-id="c29a4-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="c29a4-488">Statyczne elementy członkowskie mają następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="c29a4-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="c29a4-489">Gdy statyczny element członkowski `M` mowa w punkcie *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `E.M`, `E` musi oznaczają typu zawierającego `M`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="c29a4-490">Jest to błąd czasu kompilacji dla `E` do oznaczenia wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="c29a4-491">Pole statyczne identyfikuje dokładnie jednej lokalizacji magazynu, być współużytkowane przez wszystkie wystąpienia typu danej klasy zamknięte.</span><span class="sxs-lookup"><span data-stu-id="c29a4-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="c29a4-492">Niezależnie od tego, ile wystąpień typu danej klasy zamknięte są tworzone istnieje tylko kiedykolwiek jedną kopię pole statyczne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="c29a4-493">Funkcja statyczna elementu członkowskiego (metody, właściwości, zdarzenia, operator lub konstruktora) nie będzie działać na określonym wystąpieniu i jest to błąd kompilacji, aby odwołać się do `this` w składowej funkcji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="c29a4-494">Jeśli deklaracja pola, metody, właściwości, zdarzenia, indeksator, Konstruktor lub destruktor nie obejmuje `static` modyfikator, deklaruje składową wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="c29a4-495">(Elementu członkowskiego wystąpienia jest czasami nazywane niestatycznego elementu członkowskiego). Elementy członkowskie wystąpień mają następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="c29a4-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="c29a4-496">Gdy elementu członkowskiego wystąpienia `M` mowa w punkcie *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `E.M`, `E` musi oznaczają wystąpienia typu zawierającego `M`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="c29a4-497">Jest to błąd czasu powiązania dla `E` określający typ.</span><span class="sxs-lookup"><span data-stu-id="c29a4-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="c29a4-498">Każde wystąpienie klasy zawiera oddzielny zestaw wszystkie pola wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="c29a4-499">Funkcja elementu członkowskiego wystąpienia (metoda, właściwość, indeksator, wystąpienie konstruktora lub destruktora) działa na danego wystąpienia klasy, a to wystąpienie może być dostępna jako `this` ([dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="c29a4-500">W poniższym przykładzie pokazano reguły do uzyskiwania dostępu do statycznych i elementy członkowskie wystąpień:</span><span class="sxs-lookup"><span data-stu-id="c29a4-500">The following example illustrates the rules for accessing static and instance members:</span></span>
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

<span data-ttu-id="c29a4-501">`F` Metoda pokazuje, że w składową wystąpienia funkcji *simple_name* ([proste nazwy](expressions.md#simple-names)) może służyć do dostępu do składowych wystąpienia i statyczne elementy członkowskie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="c29a4-502">`G` Metoda wskazuje, że w statycznej funkcji składowej, jest to błąd czasu kompilacji do dostępu do członka wystąpienia, za pośrednictwem *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="c29a4-503">`Main` Metoda ma pokazać, że w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)), elementy członkowskie wystąpień muszą być dostępne za pośrednictwem wystąpień i statyczne elementy członkowskie muszą być dostępne za pośrednictwem typów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="c29a4-504">Zagnieżdżone typy</span><span class="sxs-lookup"><span data-stu-id="c29a4-504">Nested types</span></span>

<span data-ttu-id="c29a4-505">Typem zadeklarowanym w deklaracji klasy lub struktury jest wywoływana ***zagnieżdżony typ***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="c29a4-506">Typ, który jest zadeklarowany w jednostki kompilacji lub przestrzeni nazw jest wywoływana ***typu niezagnieżdżonego***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="c29a4-507">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-507">In the example</span></span>
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
<span data-ttu-id="c29a4-508">Klasa `B` jest typem zagnieżdżonym, ponieważ jest on zadeklarowany w klasie `A`, a klasa `A` jest typu niezagnieżdżonego, ponieważ jest on zadeklarowany w ramach jednostki kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="c29a4-509">W pełni kwalifikowana nazwa</span><span class="sxs-lookup"><span data-stu-id="c29a4-509">Fully qualified name</span></span>

<span data-ttu-id="c29a4-510">W pełni kwalifikowana nazwa ([w pełni kwalifikowane nazwy](basic-concepts.md#fully-qualified-names)) jest typem zagnieżdżonym `S.N` gdzie `S` jest w pełni kwalifikowaną nazwę typu, który typ `N` jest zadeklarowana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="c29a4-511">Deklarowana dostępność metody</span><span class="sxs-lookup"><span data-stu-id="c29a4-511">Declared accessibility</span></span>

<span data-ttu-id="c29a4-512">Typy zagnieżdżone nie mogą mieć `public` lub `internal` zadeklarowany ułatwień dostępu i mieć `internal` zadeklarowana dostępność domyślnie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="c29a4-513">Zagnieżdżone typy mogą być deklarowana dostępność metody w takiej formie, oraz jeden lub więcej dodatkowych formularzy deklarowana dostępność metody w zależności od tego, czy typ zawierający klasy lub struktury:</span><span class="sxs-lookup"><span data-stu-id="c29a4-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="c29a4-514">Typ zagnieżdżony, która jest zadeklarowana w klasie może być spowodowany pięć form deklarowana dostępność metody (`public`, `protected internal`, `protected`, `internal`, lub `private`) i, podobnie jak inne elementy członkowskie klasy, wartością domyślną jest `private` zadeklarowana ułatwienia dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="c29a4-515">Typ zagnieżdżony, która jest zadeklarowana w strukturze może mieć jedną z trzech rodzajów deklarowana dostępność metody (`public`, `internal`, lub `private`) i, podobnie jak inne elementy członkowskie struktury, wartością domyślną jest `private` zadeklarowana ułatwień dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="c29a4-516">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-516">The example</span></span>
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
<span data-ttu-id="c29a4-517">deklaruje klasę zagnieżdżoną prywatnej `Node`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="c29a4-518">Ukrywanie</span><span class="sxs-lookup"><span data-stu-id="c29a4-518">Hiding</span></span>

<span data-ttu-id="c29a4-519">Typ zagnieżdżony może ukryć ([ukrywanie nazwy](basic-concepts.md#name-hiding)) podstawową składową.</span><span class="sxs-lookup"><span data-stu-id="c29a4-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="c29a4-520">`new` Modyfikator jest dozwolone w deklaracjach typu zagnieżdżonego, dzięki czemu mogą być jawnie wyrażone ukrywanie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="c29a4-521">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-521">The example</span></span>
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
<span data-ttu-id="c29a4-522">zawiera klasę zagnieżdżoną `M` , ukrywa metodę `M` zdefiniowane w `Base`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="c29a4-523">Ten dostęp</span><span class="sxs-lookup"><span data-stu-id="c29a4-523">this access</span></span>

<span data-ttu-id="c29a4-524">Typ zagnieżdżony, a jej typ zawierający nie mają specjalne relacji w odniesieniu do *this_access* ([dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="c29a4-525">W szczególności `this` w ramach typu zagnieżdżonego nie można używać do odwoływania się do składowych wystąpienia typu zawierającego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="c29a4-526">W przypadkach, gdzie typ zagnieżdżony musi mieć dostęp do wystąpienia członkowie jej typ zawierający, można podać dostępu, zapewniając `this` dla wystąpienia typu zawierającego jako argument konstruktora dla typu zagnieżdżonego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="c29a4-527">Poniższy przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-527">The following example</span></span>
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
<span data-ttu-id="c29a4-528">pokazano tej techniki.</span><span class="sxs-lookup"><span data-stu-id="c29a4-528">shows this technique.</span></span> <span data-ttu-id="c29a4-529">Wystąpienie `C` tworzy wystąpienie `Nested` i przekazuje swój własny `this` do `Nested`przez konstruktora w celu zapewnienia późniejszego dostępu do `C`firmy wystąpieniami elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="c29a4-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="c29a4-530">Dostęp do prywatnych i chronionych członków typu zawierającego</span><span class="sxs-lookup"><span data-stu-id="c29a4-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="c29a4-531">Zagnieżdżony typ ma dostęp do wszystkich elementów członkowskich, które są dostępne dla zawierającego ją typu, w tym elementy członkowskie typu zawierającego, których `private` i `protected` zadeklarowana ułatwień dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="c29a4-532">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-532">The example</span></span>
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
<span data-ttu-id="c29a4-533">Pokazuje klasy `C` zawiera klasę zagnieżdżoną `Nested`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="c29a4-534">W ramach `Nested`, Metoda `G` statyczna metoda `F` zdefiniowane w `C`, i `F` prywatne zadeklarował ułatwień dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="c29a4-535">Zagnieżdżony typ również dostępu do chronionych składowych zdefiniowanych w typie podstawowym jej typ zawierający.</span><span class="sxs-lookup"><span data-stu-id="c29a4-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="c29a4-536">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-536">In the example</span></span>
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
<span data-ttu-id="c29a4-537">Klasa zagnieżdżona `Derived.Nested` uzyskuje dostęp do chronionych metoda `F` zdefiniowane w `Derived`firmy podstawowej klasy, `Base`, wywołanie za pośrednictwem wystąpienia `Derived`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="c29a4-538">Zagnieżdżone typy klas ogólnych</span><span class="sxs-lookup"><span data-stu-id="c29a4-538">Nested types in generic classes</span></span>

<span data-ttu-id="c29a4-539">Deklaracji klasy ogólnej może zawierać deklaracje typu zagnieżdżonego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="c29a4-540">Parametry typu otaczającej klasy może służyć w obrębie zagnieżdżonych typów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="c29a4-541">Deklaracji typu zagnieżdżonego może zawierać parametrów typu dodatkowe, które mają zastosowanie tylko do typu zagnieżdżonego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="c29a4-542">Każdy zawartych w deklaracji klasy ogólnej deklaracji typu jest niejawnie deklaracją typu ogólnego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="c29a4-543">Podczas pisania odwołanie do typu zagnieżdżone wewnątrz typu rodzajowego, musi mieć nazwę zawierającą skonstruowanego typu, w tym argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="c29a4-544">Jednak z w ramach zewnętrznej klasy typu zagnieżdżonego można używać bez kwalifikacji; Typ wystąpienia klasy zewnętrznej niejawnie służy przy konstruowaniu zagnieżdżonych typów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="c29a4-545">Poniższy przykład pokazuje trzy różne sposoby poprawne do odwoływania się do skonstruowanego typu utworzone na podstawie `Inner`; pierwsze dwa są równoważne:</span><span class="sxs-lookup"><span data-stu-id="c29a4-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
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

<span data-ttu-id="c29a4-546">Mimo że to jest źle programowania stylu, parametr typu w typie zagnieżdżonym można ukryć członka lub parametr zostało zadeklarowane w typie zewnętrznego typu:</span><span class="sxs-lookup"><span data-stu-id="c29a4-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="c29a4-547">Nazw zastrzeżoną składową</span><span class="sxs-lookup"><span data-stu-id="c29a4-547">Reserved member names</span></span>

<span data-ttu-id="c29a4-548">W celu ułatwienia podstawowych języka C# środowiska wykonawczego wykonania, dla każdej deklaracji elementu członkowskiego źródła, właściwości, zdarzenia lub indeksatora implementacji należy zarezerwować dwóch podpisów metody oparte na rodzaju deklaracji składowej, nazwy i typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="c29a4-549">Jest to błąd czasu kompilacji dla programu zadeklarować członka, którego podpis pasuje do jednej z tych zarezerwowanych podpisów, nawet wtedy, gdy podstawowej implementacji czasu wykonywania nie oznacza, że użycie te zastrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="c29a4-550">Nazw zastrzeżonych nie wprowadzają deklaracji, w związku z tym nie uczestniczą w wyszukanie członka.</span><span class="sxs-lookup"><span data-stu-id="c29a4-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="c29a4-551">Jednak deklarację skojarzonej zarezerwowanych metoda podpisów uczestniczą w dziedziczeniu ([dziedziczenia](classes.md#inheritance)) i może być ukryty za pomocą `new` modyfikator ([nowy modyfikator](classes.md#the-new-modifier)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="c29a4-552">Rezerwacja te nazwy służy trzy funkcje:</span><span class="sxs-lookup"><span data-stu-id="c29a4-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="c29a4-553">Aby zezwolić na podstawowej implementacji użyć zwykłej identyfikatora jako nazwę metody get lub dostęp do funkcji języka C#.</span><span class="sxs-lookup"><span data-stu-id="c29a4-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="c29a4-554">Aby zezwolić na inne języki współpracować przy użyciu zwykłej identyfikatora jako nazwę metody get lub Ustaw dostęp do funkcji języka C#.</span><span class="sxs-lookup"><span data-stu-id="c29a4-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="c29a4-555">Aby mieć pewność, że źródła akceptowane przez kompilator odpowiadające jeden jest akceptowana przez inną, wprowadzając szczegóły zastrzeżoną składową nazwy spójne we wszystkich implementacji języka C#.</span><span class="sxs-lookup"><span data-stu-id="c29a4-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="c29a4-556">Deklaracja destruktora ([destruktory](classes.md#destructors)) również powoduje, że podpis mają zostać zarezerwowane ([nazwy elementów członkowskich zarezerwowane dla destruktory](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="c29a4-557">Zastrzeżone nazwy elementów członkowskich dla właściwości</span><span class="sxs-lookup"><span data-stu-id="c29a4-557">Member names reserved for properties</span></span>

<span data-ttu-id="c29a4-558">Dla właściwości `P` ([właściwości](classes.md#properties)) typu `T`, następujące podpisy są zarezerwowane:</span><span class="sxs-lookup"><span data-stu-id="c29a4-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="c29a4-559">Zarówno podpisy są zarezerwowane, nawet jeśli właściwość jest tylko do odczytu lub tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="c29a4-560">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-560">In the example</span></span>
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
<span data-ttu-id="c29a4-561">Klasa `A` definiuje właściwość tylko do odczytu `P`, dlatego zarezerwowanie podpisów dla `get_P` i `set_P` metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="c29a4-562">Klasa `B` pochodzi od klasy `A` i ukrywa obu tych zarezerwowanych podpisów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="c29a4-563">Przykład generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="c29a4-563">The example produces the output:</span></span>
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="c29a4-564">Zastrzeżone nazwy elementów członkowskich dla zdarzeń</span><span class="sxs-lookup"><span data-stu-id="c29a4-564">Member names reserved for events</span></span>

<span data-ttu-id="c29a4-565">Zdarzenia `E` ([zdarzenia](classes.md#events)) typu delegata `T`, następujące podpisy są zarezerwowane:</span><span class="sxs-lookup"><span data-stu-id="c29a4-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="c29a4-566">Zastrzeżone nazwy elementów członkowskich dla indeksatorów</span><span class="sxs-lookup"><span data-stu-id="c29a4-566">Member names reserved for indexers</span></span>

<span data-ttu-id="c29a4-567">Dla indeksatora ([indeksatory](classes.md#indexers)) typu `T` z listą parametrów `L`, następujące podpisy są zarezerwowane:</span><span class="sxs-lookup"><span data-stu-id="c29a4-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="c29a4-568">Zarówno podpisy są zarezerwowane, nawet jeśli indeksatora jest tylko do odczytu lub tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="c29a4-569">Ponadto nazwa elementu członkowskiego `Item` jest zarezerwowany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="c29a4-570">Zastrzeżone nazwy elementów członkowskich dla destruktorów</span><span class="sxs-lookup"><span data-stu-id="c29a4-570">Member names reserved for destructors</span></span>

<span data-ttu-id="c29a4-571">Klasy zawierające destruktora ([destruktory](classes.md#destructors)), następujący podpis jest zastrzeżona:</span><span class="sxs-lookup"><span data-stu-id="c29a4-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="c29a4-572">Stałe</span><span class="sxs-lookup"><span data-stu-id="c29a4-572">Constants</span></span>

<span data-ttu-id="c29a4-573">A ***stałej*** jest składową klasy, która reprezentuje wartość stałą: wartość, która może zostać obliczony w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="c29a4-574">A *constant_declaration* wprowadza co najmniej jedną stałą danego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

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

<span data-ttu-id="c29a4-575">A *constant_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)), `new` modyfikator ([nowy modyfikator](classes.md#the-new-modifier)), Nieprawidłowa kombinacja modyfikatory dostępu czterech i ([modyfikatorach dostępu](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="c29a4-576">Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych za *constant_declaration*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="c29a4-577">Mimo że stałe są traktowane jako elementy statyczne, *constant_declaration* nie wymaga ani nie zezwala na `static` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="c29a4-578">Jest to błąd dla tego samego modyfikator pojawią się wiele razy w deklaracji stałej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="c29a4-579">*Typu* z *constant_declaration* Określa typ elementów członkowskich wprowadzonych przez tę deklarację.</span><span class="sxs-lookup"><span data-stu-id="c29a4-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="c29a4-580">Typ następuje lista *constant_declarator*s, z których każdy wprowadza nowy element członkowski.</span><span class="sxs-lookup"><span data-stu-id="c29a4-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="c29a4-581">A *constant_declarator* składa się z *identyfikator* , nazwy elementu członkowskiego, a następnie "`=`" token, a następnie *constant_expression* ([ Wyrażenia stałe](expressions.md#constant-expressions)) daje wartość elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="c29a4-582">*Typu* określone w stałą deklaracja musi być `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, *enum_type*, lub *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="c29a4-583">Każdy *constant_expression* należy przekazać wartości typu docelowego, lub typu, który można przekonwertować na typ docelowy niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="c29a4-584">*Typu* stałej musi być co najmniej tak samo dostępna jak stała się ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c29a4-585">Wartość stałej jest uzyskiwana przy użyciu wyrażenia *simple_name* ([proste nazwy](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="c29a4-586">Stała się uczestniczyć w *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="c29a4-587">W efekcie stałą mogą być używane w taki sposób, w konstrukcji, która wymaga *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="c29a4-588">Przykładami takich konstrukcji `case` etykiet, `goto case` instrukcji `enum` deklaracji elementu członkowskiego, atrybutów i innych deklaracji stałej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="c29a4-589">Zgodnie z opisem w [wyrażeń stałych](expressions.md#constant-expressions), *constant_expression* jest wyrażeniem, które mogą zostać w pełni oszacowane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="c29a4-590">Ponieważ jedynym sposobem, aby utworzyć inną niż null wartości *reference_type* innych niż `string` polega na zastosowaniu `new` operatora, a ponieważ `new` operator nie jest dozwolona w *constant_ wyrażenie*, jedyną możliwą wartością dla stałych typu *reference_type*s innych niż `string` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="c29a4-591">Gdy potrzeby Symboliczna nazwa wartości stałej, ale gdy typ wartości nie jest dozwolone w deklaracji stałej lub gdy nie można obliczyć wartości w czasie kompilacji przez *constant_expression*, `readonly` pola () [Pola tylko do odczytu](classes.md#readonly-fields)) mogą być używane zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="c29a4-592">Deklaracji stałej, która deklaruje kilka stałych jest odpowiednikiem w wielu deklaracjach stałe jednego z atrybutami, Modyfikatory i typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="c29a4-593">Na przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="c29a4-594">odpowiada wyrażeniu</span><span class="sxs-lookup"><span data-stu-id="c29a4-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="c29a4-595">Stałe są dozwolone zależą inne stałe w tym samym programie, tak długo, jak zależności nie mają charakteru cykliczne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="c29a4-596">Kompilator automatycznie rozmieszcza można obliczyć wartości stałej deklaracje w odpowiedniej kolejności.</span><span class="sxs-lookup"><span data-stu-id="c29a4-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="c29a4-597">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-597">In the example</span></span>
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
<span data-ttu-id="c29a4-598">Kompilator najpierw ocenia `A.Y`, ocenia następnie `B.Z`i na końcu ocenia `A.X`, produkcji wartości `10`, `11`, i `12`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="c29a4-599">Deklaracji stałej może zależeć od stałe z innych programów, ale takie zależności są możliwe tylko w jednym kierunku.</span><span class="sxs-lookup"><span data-stu-id="c29a4-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="c29a4-600">Odwołujące się do w powyższym przykładzie, jeśli `A` i `B` zostały zgłoszone w oddzielnych programach, możliwe będzie `A.X` zależą `B.Z`, ale `B.Z` może następnie równocześnie nie zależą od `A.Y`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="c29a4-601">Pola</span><span class="sxs-lookup"><span data-stu-id="c29a4-601">Fields</span></span>

<span data-ttu-id="c29a4-602">A ***pola*** jest elementem członkowskim, który reprezentuje zmienną związaną z obiektem lub klasą.</span><span class="sxs-lookup"><span data-stu-id="c29a4-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="c29a4-603">A *field_declaration* wprowadza co najmniej jedno pole danego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

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

<span data-ttu-id="c29a4-604">A *field_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)), `new` modyfikator ([nowy modyfikator](classes.md#the-new-modifier)), Nieprawidłowa kombinacja modyfikatory dostępu cztery ([modyfikatorach dostępu](classes.md#access-modifiers)) i `static` modyfikator ([pola statyczne i wystąpienia](classes.md#static-and-instance-fields)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="c29a4-605">Ponadto *field_declaration* mogą obejmować `readonly` modyfikator ([pola tylko do odczytu](classes.md#readonly-fields)) lub `volatile` modyfikator ([pola nietrwałego](classes.md#volatile-fields)), ale nie oba.</span><span class="sxs-lookup"><span data-stu-id="c29a4-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="c29a4-606">Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych za *field_declaration*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="c29a4-607">Jest to błąd dla tego samego modyfikator pojawią się wiele razy w deklarację pola.</span><span class="sxs-lookup"><span data-stu-id="c29a4-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="c29a4-608">*Typu* z *field_declaration* Określa typ elementów członkowskich wprowadzonych przez tę deklarację.</span><span class="sxs-lookup"><span data-stu-id="c29a4-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="c29a4-609">Typ następuje lista *variable_declarator*s, z których każdy wprowadza nowy element członkowski.</span><span class="sxs-lookup"><span data-stu-id="c29a4-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="c29a4-610">A *variable_declarator* składa się z *identyfikator* , nazwy tego elementu członkowskiego, opcjonalnie następuje "`=`" token i *variable_initializer* ([ Inicjatory zmiennej](classes.md#variable-initializers)) daje wartość początkową tego członka.</span><span class="sxs-lookup"><span data-stu-id="c29a4-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="c29a4-611">*Typu* pola musi być co najmniej tak samo dostępna jak samo pole ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c29a4-612">Wartość pola są uzyskiwane przy użyciu wyrażenia *simple_name* ([proste nazwy](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="c29a4-613">Wartość pola — tylko do odczytu jest modyfikowane przy użyciu *przypisania* ([operatory przypisania](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="c29a4-614">Wartość pola — tylko do odczytu mogą być uzyskane i modyfikować za pomocą przyrostka inkrementacji i operatory dekrementacji ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators)) przedrostkowe i operatory dekrementacji ([prefiksu operatory inkrementacji i dekrementacji](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="c29a4-615">Deklaracja pola, która deklaruje wiele pól jest odpowiednikiem w wielu deklaracjach pojedynczego pola z atrybutami, Modyfikatory i typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="c29a4-616">Na przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="c29a4-617">odpowiada wyrażeniu</span><span class="sxs-lookup"><span data-stu-id="c29a4-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="c29a4-618">Pola statyczne i wystąpienia</span><span class="sxs-lookup"><span data-stu-id="c29a4-618">Static and instance fields</span></span>

<span data-ttu-id="c29a4-619">Jeśli deklaracja pola zawiera `static` są pól wprowadzonych przez tę deklarację, modyfikator ***pola statyczne***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="c29a4-620">Gdy nie `static` modyfikator, pól wprowadzonych przez tę deklarację ***wystąpienia pól***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="c29a4-621">Pola statyczne i pola wystąpienia są dwa rodzaje kilka zmiennych ([zmienne](variables.md)) obsługiwane przez C# i w czasie ich są określane jako ***statyczne zmienne*** i ***zmienne wystąpienia*** , odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="c29a4-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="c29a4-622">Statyczne pole nie jest częścią konkretnego wystąpienia; Zamiast tego należy go jest współużytkowany przez wszystkie wystąpienia typu zamknięte ([otwarte i zamknięte typy](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="c29a4-623">Niezależnie od tego, ile wystąpień typu zamkniętej klasy są tworzone istnieje tylko kiedykolwiek jedną kopię pole statyczne w domenie skojarzonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="c29a4-624">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c29a4-624">For example:</span></span>
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

<span data-ttu-id="c29a4-625">Pola wystąpienia należy do wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="c29a4-626">W szczególności każde wystąpienie klasy zawiera oddzielny zestaw wszystkie pola wystąpienia tej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="c29a4-627">Gdy pole jest przywoływany w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `E.M`, jeśli `M` jest polem statyczne `E` musi oznaczają typu zawierającego `M` Jeśli `M` jest pole wystąpienia, E musi oznaczają wystąpienia typu zawierającego `M`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="c29a4-628">Różnice statyczne elementy członkowskie wystąpień zostały one omówione w dalszych [statyczna i wystąpienia elementów członkowskich](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="c29a4-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="c29a4-629">Pola tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="c29a4-629">Readonly fields</span></span>

<span data-ttu-id="c29a4-630">Gdy *field_declaration* obejmuje `readonly` są pól wprowadzonych przez tę deklarację, modyfikator ***pola tylko do odczytu***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="c29a4-631">Bezpośrednie przypisania do pól tylko do odczytu mogą występować wyłącznie jako część tej deklaracji lub w konstruktorze wystąpienia lub statycznego konstruktora w tej samej klasie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="c29a4-632">(Pola tylko do odczytu może być przypisany do wielokrotnie w tych kontekstach.) W szczególności, bezpośrednie przypisania do `readonly` pola są dozwolone tylko w następujących kontekstów się:</span><span class="sxs-lookup"><span data-stu-id="c29a4-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="c29a4-633">W *variable_declarator* powoduje to wprowadzenie pola (umieszczając *variable_initializer* w deklaracji).</span><span class="sxs-lookup"><span data-stu-id="c29a4-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="c29a4-634">Pola wystąpienia w konstruktory wystąpień klasy, który zawiera deklarację pola; statycznego pola w konstruktorze statycznym klasy, który zawiera deklarację pola.</span><span class="sxs-lookup"><span data-stu-id="c29a4-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="c29a4-635">Jest on również tylko kontekstów, w których jest on prawidłowy do przekazania `readonly` pola jako `out` lub `ref` parametru.</span><span class="sxs-lookup"><span data-stu-id="c29a4-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="c29a4-636">Podjęto próbę przypisania do `readonly` pola lub przekazać je jako `out` lub `ref` parametr w innym kontekście jest to błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="c29a4-637">Za pomocą pola statycznego tylko do odczytu dla stałych</span><span class="sxs-lookup"><span data-stu-id="c29a4-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="c29a4-638">A `static readonly` pola jest przydatne, jeśli pożądane jest Symboliczna nazwa wartości stałej, ale gdy typ wartości nie jest dozwolona w `const` deklaracji, lub gdy w czasie kompilacji, w którym nie można obliczyć wartości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="c29a4-639">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-639">In the example</span></span>
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
<span data-ttu-id="c29a4-640">`Black`, `White`, `Red`, `Green`, i `Blue` elementów członkowskich nie można zadeklarować jako `const` elementów członkowskich, ponieważ nie można obliczyć wartości w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="c29a4-641">Jednak ich zgłaszania `static readonly` zamiast tego ma znacznie taki sam skutek.</span><span class="sxs-lookup"><span data-stu-id="c29a4-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="c29a4-642">Przechowywanie wersji, stałe i pola statycznego tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="c29a4-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="c29a4-643">Stałe i pola tylko do odczytu mają semantykę różnych wersji binarnego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="c29a4-644">Gdy wyrażenie odwołuje się do stałej, wartość stałej są uzyskiwane w czasie kompilacji, ale gdy wyrażenie odwołuje się do pola tylko do odczytu, wartość pola nie uzyskano czasu wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="c29a4-645">Rozważmy aplikację, która składa się z dwóch oddzielnych programach:</span><span class="sxs-lookup"><span data-stu-id="c29a4-645">Consider an application that consists of two separate programs:</span></span>
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

<span data-ttu-id="c29a4-646">`Program1` i `Program2` przestrzenie nazw oznaczają dwa programy, które są kompilowane osobno.</span><span class="sxs-lookup"><span data-stu-id="c29a4-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="c29a4-647">Ponieważ `Program1.Utils.X` jest zadeklarowany jako pola statycznego tylko do odczytu, wartość danych wyjściowych przez `Console.WriteLine` instrukcja nie jest znany w czasie kompilacji, ale raczej są uzyskiwane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="c29a4-648">W związku z tym jeśli wartość `X` zostanie zmieniony i `Program1` jest ponownie kompilowany, `Console.WriteLine` instrukcji zwróci nowy nawet jeśli wartość `Program2` nie jest ponownie kompilowana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="c29a4-649">Jednak miał `X` zostały stałą wartość `X` będą uzyskiwane w czasie `Program2` został skompilowany i spowoduje wpływu zmian w `Program1` aż `Program2` jest ponownie kompilowana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="c29a4-650">Pola nietrwałego</span><span class="sxs-lookup"><span data-stu-id="c29a4-650">Volatile fields</span></span>

<span data-ttu-id="c29a4-651">Gdy *field_declaration* obejmuje `volatile` są pól wprowadzonych przez deklaracja, modyfikator ***pola nietrwałego***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="c29a4-652">Dla pól typu nieulotnego technik optymalizacji, które zmiana kolejności instrukcji może prowadzić do nieoczekiwanych i nieprzewidywalne wyniki w programach wielowątkowych, które uzyskują dostęp do pól bez synchronizacji, takim jak udostępniany przez *lock_statement*  ([Instrukcję lock](statements.md#the-lock-statement)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="c29a4-653">Optymalizacje te można wykonać przez kompilator, działającego systemu lub sprzętu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="c29a4-654">Do pola nietrwałego takich ponownego zamawiania optymalizacje są ograniczone:</span><span class="sxs-lookup"><span data-stu-id="c29a4-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="c29a4-655">Odczyt pola nietrwałego jest nazywany ***volatile odczytu***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="c29a4-656">Volatile odczytu ma "nabyć semantykę"; oznacza to, że zagwarantowane jest wystąpić przed wszystkie odwołania do pamięci, które występują po nim w sekwencji instrukcji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="c29a4-657">Zapis pola nietrwałego jest nazywany ***volatile zapisu***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="c29a4-658">Volatile zapisu ma "release semantykę"; oznacza to, że jest gwarantowane do wykonania po wszelkie odwołania do pamięci, przed instrukcją zapisu w kolejności instrukcji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="c29a4-659">Te ograniczenia upewnij się, że wszystkie wątki będzie przestrzegać volatile zapisów wykonywane przez inny wątek, w kolejności, w którym zostały wykonane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="c29a4-660">Odpowiadające wdrożenia nie jest wymagany zapewnienie pojedynczego całkowita kolejność volatile zapisów wyświetlanego ze wszystkich wątków wykonania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="c29a4-661">Typ pola nietrwałego musi być jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c29a4-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="c29a4-662">A *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-662">A *reference_type*.</span></span>
*  <span data-ttu-id="c29a4-663">Typ `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, lub `System.UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or `System.UIntPtr`.</span></span>
*  <span data-ttu-id="c29a4-664">*Enum_type* wyliczenia podstawowego typu `byte`, `sbyte`, `short`, `ushort`, `int`, lub `uint`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="c29a4-665">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-665">The example</span></span>
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
<span data-ttu-id="c29a4-666">generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="c29a4-666">produces the output:</span></span>
```
result = 143
```

<span data-ttu-id="c29a4-667">W tym przykładzie metoda `Main` uruchamia nowy wątek, który uruchamia metody `Thread2`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="c29a4-668">Ta metoda przechowuje wartość w polu trwałej o nazwie `result`, następnie przechowuje `true` w pola nietrwałego `finished`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="c29a4-669">Główny wątek czeka pole `finished` należy ustawić `true`, następnie odczytuje pola `result`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="c29a4-670">Ponieważ `finished` została zadeklarowana `volatile`, wątek główny należy odczytać wartości `143` pola `result`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="c29a4-671">Jeśli pole `finished` nie została zadeklarowana `volatile`, a następnie będzie dozwolony dla magazynu do `result` mają być widoczne dla głównego wątku po sklep `finished`i dlatego dla głównego wątku, który ma zostać odczytana wartość `0` z pole `result`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="c29a4-672">Deklarowanie `finished` jako `volatile` pola uniemożliwia takich niezgodności.</span><span class="sxs-lookup"><span data-stu-id="c29a4-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="c29a4-673">Inicjowanie pola</span><span class="sxs-lookup"><span data-stu-id="c29a4-673">Field initialization</span></span>

<span data-ttu-id="c29a4-674">Początkowa wartość pola, czy jest to pole statyczne lub pole wystąpienia, jest wartością domyślną ([wartości domyślne](variables.md#default-values)) z typem pola.</span><span class="sxs-lookup"><span data-stu-id="c29a4-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="c29a4-675">Nie jest możliwe obserwowania wartości pola, zanim wystąpił ten proces inicjowania domyślnego i tym samym polem nigdy nie jest "niezainicjowanej".</span><span class="sxs-lookup"><span data-stu-id="c29a4-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="c29a4-676">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-676">The example</span></span>
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
<span data-ttu-id="c29a4-677">generuje dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-677">produces the output</span></span>
```
b = False, i = 0
```
<span data-ttu-id="c29a4-678">Ponieważ `b` i `i` są zarówno automatycznie zainicjowany do wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="c29a4-679">Inicjatory zmiennej</span><span class="sxs-lookup"><span data-stu-id="c29a4-679">Variable initializers</span></span>

<span data-ttu-id="c29a4-680">Może zawierać deklaracji pól *variable_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="c29a4-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="c29a4-681">W przypadku pola statyczne inicjatory zmiennej odpowiadają instrukcji przypisania, które są wykonywane podczas inicjowania klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="c29a4-682">Na przykład pola, inicjatory zmiennej odpowiadają instrukcji przypisania, które są wykonywane, gdy tworzone jest wystąpienie klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="c29a4-683">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-683">The example</span></span>
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
<span data-ttu-id="c29a4-684">generuje dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-684">produces the output</span></span>
```
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="c29a4-685">ponieważ przypisania do `x` występuje podczas wykonywania inicjatorów pola statyczne i przypisania do `i` i `s` występują, gdy wykonywanie inicjatorów pola wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="c29a4-686">Inicjowanie wartości domyślne, które są opisane w [inicjowania pola](classes.md#field-initialization) pojawia się dla wszystkich pól, takich jak pola, które mają zmienną inicjatory.</span><span class="sxs-lookup"><span data-stu-id="c29a4-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="c29a4-687">W związku z tym podczas inicjowania klasy wszystkie pola statyczne w tej klasie najpierw są inicjowane do wartości domyślnych, a następnie inicjatorów pola statyczne są wykonywane w kolejności tekstową.</span><span class="sxs-lookup"><span data-stu-id="c29a4-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="c29a4-688">Podobnie po utworzeniu wystąpienia klasy, wszystkie pola wystąpienia w tym wystąpieniu najpierw są inicjowane do wartości domyślnych, a następnie inicjatorów pola wystąpienia są wykonywane w kolejności tekstową.</span><span class="sxs-lookup"><span data-stu-id="c29a4-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="c29a4-689">Jest możliwe w przypadku pól statycznych zmiennych inicjatorów należy przestrzegać w ich stanie. wartość domyślna.</span><span class="sxs-lookup"><span data-stu-id="c29a4-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="c29a4-690">Jest to jednak zalecane jako stylu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="c29a4-691">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-691">The example</span></span>
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
<span data-ttu-id="c29a4-692">wykazuje to zachowanie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-692">exhibits this behavior.</span></span> <span data-ttu-id="c29a4-693">Pomimo cykliczne definicje i b, program jest prawidłowy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="c29a4-694">Powoduje to dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-694">It results in the output</span></span>
```
a = 1, b = 2
```
<span data-ttu-id="c29a4-695">ponieważ pola statyczne `a` i `b` są inicjowane na wartość `0` (wartość domyślna dla `int`) przed ich inicjatory są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="c29a4-696">Podczas inicjator dla `a` uruchamia wartość `b` wynosi zero, a tym samym `a` jest inicjowany do `1`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="c29a4-697">Podczas inicjator dla `b` uruchamia wartość `a` jest już `1`, a tym samym `b` jest inicjowany do `2`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="c29a4-698">Inicjowanie pole statyczne</span><span class="sxs-lookup"><span data-stu-id="c29a4-698">Static field initialization</span></span>

<span data-ttu-id="c29a4-699">Pole statyczne inicjatory zmiennej klasy odpowiadają sekwencję przypisania, które zostały wykonane w porządku tekstowych, w jakiej występują w deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="c29a4-700">Jeśli Konstruktor statyczny ([konstruktorów statycznych](classes.md#static-constructors)) istnieje w klasie inicjatorów pola statycznego jest wykonywany natychmiast, przed wykonaniem tego Konstruktor statyczny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="c29a4-701">W przeciwnym razie inicjatorów pola statyczne są wykonywane w czasie zależne od implementacji przed pierwszym użyciu pole statyczne tej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="c29a4-702">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-702">The example</span></span>
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
<span data-ttu-id="c29a4-703">może dawać obu dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="c29a4-703">might produce either the output:</span></span>
```
Init A
Init B
1 1
```
<span data-ttu-id="c29a4-704">lub dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="c29a4-704">or the output:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="c29a4-705">ponieważ wykonanie `X`firmy inicjatora i `Y`w inicjatorze mogą wystąpić w obu kolejności; są one tylko ograniczone występuje przed odwołania do tych pól.</span><span class="sxs-lookup"><span data-stu-id="c29a4-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="c29a4-706">Jednak w przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c29a4-706">However, in the example:</span></span>
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
<span data-ttu-id="c29a4-707">dane wyjściowe muszą być:</span><span class="sxs-lookup"><span data-stu-id="c29a4-707">the output must be:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="c29a4-708">ponieważ zasady podczas wykonywania konstruktory statyczne (zgodnie z definicją w [konstruktory statyczne](classes.md#static-constructors)) stanowią, że `B`firmy Konstruktor statyczny (i w związku z tym `B`firmy pole statyczne inicjatory) musi zostać uruchomiony przed `A`w konstruktorze statycznym i inicjatorów pola.</span><span class="sxs-lookup"><span data-stu-id="c29a4-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="c29a4-709">Inicjowanie pola wystąpienia</span><span class="sxs-lookup"><span data-stu-id="c29a4-709">Instance field initialization</span></span>

<span data-ttu-id="c29a4-710">Inicjatorów pola zmiennej wystąpienia klasy odpowiadają sekwencję przypisania, które są wykonywane od razu po wejściu do jednej z konstruktorów wystąpienia ([inicjatory konstruktora](classes.md#constructor-initializers)) dla tej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="c29a4-711">Inicjatory zmiennej są wykonywane w kolejności tekstowych, w jakiej występują w deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="c29a4-712">Proces tworzenia i inicjowania wystąpienia klasy jest dokładniejszym opisem zawartym w [wystąpienia konstruktory](classes.md#instance-constructors).</span><span class="sxs-lookup"><span data-stu-id="c29a4-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="c29a4-713">Inicjator zmiennej dla pola wystąpienia nie może odwoływać się wystąpienia tworzona.</span><span class="sxs-lookup"><span data-stu-id="c29a4-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="c29a4-714">Dlatego jest to błąd czasu kompilacji, aby odwołać się do `this` w inicjatorze zmiennych w postaci, w jakiej jest błąd w czasie kompilacji dla inicjatorze zmiennej można odwoływać się do dowolnego elementu członkowskiego wystąpienia za pośrednictwem *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="c29a4-715">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="c29a4-716">Inicjator zmiennej `y` powoduje błąd w czasie kompilacji, ponieważ widok odwołuje się członkiem wystąpienia tworzona.</span><span class="sxs-lookup"><span data-stu-id="c29a4-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="c29a4-717">Metody</span><span class="sxs-lookup"><span data-stu-id="c29a4-717">Methods</span></span>

<span data-ttu-id="c29a4-718">A ***metoda*** jest elementem członkowskim, który implementuje obliczenia lub akcji, które mogą być wykonywane przez obiekt lub klasa.</span><span class="sxs-lookup"><span data-stu-id="c29a4-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="c29a4-719">Metody są deklarowane przy użyciu *method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c29a4-719">Methods are declared using *method_declaration*s:</span></span>

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

<span data-ttu-id="c29a4-720">A *method_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)) i prawidłową kombinację modyfikatory dostępu cztery ([modyfikatorach dostępu ](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `static` ([metody statyczne i wystąpienia](classes.md#static-and-instance-methods)), `virtual` ([metody wirtualnej](classes.md#virtual-methods)), `override` ([Przesłaniaj metody](classes.md#override-methods)), `sealed` ([zapieczętowany metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)), a `extern` ([Metody zewnętrznej](classes.md#external-methods)) modyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="c29a4-721">Deklaracja ma prawidłową kombinację modyfikatorów, jeśli spełnione są wszystkie z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c29a4-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="c29a4-722">Deklaracja zawiera prawidłową kombinację modyfikatory dostępu ([modyfikatorach dostępu](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="c29a4-723">Deklaracja nie ma tego samego modyfikator wiele razy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="c29a4-724">Deklaracja zawiera co najwyżej jeden z następujących modyfikatorów: `static`, `virtual`, i `override`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="c29a4-725">Deklaracja zawiera co najwyżej jeden z następujących modyfikatorów: `new` i `override`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="c29a4-726">Jeśli deklaracja zawiera `abstract` modyfikator, a następnie deklaracji nie zawiera żadnego z następujących modyfikatorów: `static`, `virtual`, `sealed` lub `extern`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="c29a4-727">Jeśli deklaracja zawiera `private` modyfikator, a następnie deklaracji nie zawiera żadnego z następujących modyfikatorów: `virtual`, `override`, lub `abstract`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="c29a4-728">Jeśli deklaracja zawiera `sealed` modyfikator, a następnie deklaracji obejmuje również `override` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="c29a4-729">Jeśli deklaracja zawiera `partial` modyfikator, a następnie go nie ma żadnego z następujących modyfikatorów: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`, lub `extern`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="c29a4-730">Metody, która ma `async` modyfikator jest funkcji asynchronicznej i zgodna z zasadami opisanymi w [funkcje asynchroniczne](classes.md#async-functions).</span><span class="sxs-lookup"><span data-stu-id="c29a4-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="c29a4-731">*Typ_wyniku* metody deklaracja określa typ wartości, obliczana i zwracany przez metodę.</span><span class="sxs-lookup"><span data-stu-id="c29a4-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="c29a4-732">*Typ_wyniku* jest `void` Jeśli metoda nie zwraca wartości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="c29a4-733">Jeśli deklaracja zawiera `partial` modyfikator, a następnie zwracany typ musi być `void`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="c29a4-734">*Member_name* Określa nazwę metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="c29a4-735">Jeśli metoda nie jest jawną implementacją członków ([implementacji elementu członkowskiego interfejsu jawnego](interfaces.md#explicit-interface-member-implementations)), *member_name* jest po prostu *identyfikator*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="c29a4-736">Dla implementacja interfejsu jawnego członka *member_name* składa się z *interface_type* następuje "`.`" i *identyfikator*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="c29a4-737">Opcjonalny *type_parameter_list* określa parametry typu metody ([parametry typu](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="c29a4-738">Jeśli *type_parameter_list* jest określona, ta metoda jest ***metody ogólnej***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="c29a4-739">Jeśli metoda ma `extern` , modyfikator *type_parameter_list* nie może być określony.</span><span class="sxs-lookup"><span data-stu-id="c29a4-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="c29a4-740">Opcjonalny *formal_parameter_list* określa parametry metody ([parametry metody](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="c29a4-741">Opcjonalny *type_parameter_constraints_clause*s określić ograniczenia dotyczące parametrów typu poszczególnych ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) i może być określona tylko, jeśli *type_parameter_ Lista* jest również podana, i nie ma metody `override` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="c29a4-742">*Typ_wyniku* , a każdy z typów, do którego odwołuje się *formal_parameter_list* metody musi być co najmniej tak samo dostępna jak metoda, sama ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c29a4-743">*Method_body* jest albo średnik ***treść instrukcji*** lub ***treści wyrażenia***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="c29a4-744">Treść instrukcji składa się z *bloku*, która określa instrukcji do wykonania, gdy metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="c29a4-745">Treść wyrażenia składa się z `=>` następuje *wyrażenie* i średnikiem oraz oznacza pojedynczego wyrażenia do wykonania, gdy metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="c29a4-746">Aby uzyskać `abstract` i `extern` metod, *method_body* po prostu składa się z średnikiem.</span><span class="sxs-lookup"><span data-stu-id="c29a4-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="c29a4-747">Aby uzyskać `partial` metody *method_body* może składać się ze średnikiem, treści bloku lub treści wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="c29a4-748">Inne metody *method_body* treści bloku lub treści wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="c29a4-749">Jeśli *method_body* składa się średnikiem, a następnie deklaracja nie może zawierać `async` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="c29a4-750">Nazwa, lista parametrów typu i formalnej listy parametrów metody definiowania podpis ([podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading)) metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="c29a4-751">W szczególności podpis metody składa się z jego nazwę, liczba parametrów typu i liczbę, Modyfikatory i typy parametrów formalnych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="c29a4-752">Do tych celów wszelkie parametr typu metody, która występuje w typie formalny parametr jest identyfikowany nie według jego nazwy, ale za pomocą jego numer porządkowy pozycji na liście argumentów typu metody. Zwracany typ nie jest częścią podpis metody nie są nazwy parametrów typu lub parametrów formalnych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="c29a4-753">Nazwa metody muszą różnić się od nazw wszystkich innych niż metod zadeklarowanych w tej samej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="c29a4-754">Ponadto podpis metody muszą różnić się od podpisy wszystkich pozostałych metod zadeklarowanych w tej samej klasie i dwie metody zadeklarowanej w tej samej klasy nie może mieć podpisów, które różnią się jedynie przez `ref` i `out`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="c29a4-755">Metoda *type_parameter*s znajdują się w zakresie *method_declaration*i może służyć do typów formularza, w tym zakresie w *Typ_wyniku*, *method_body*, i *type_parameter_constraints_clause*s, ale nie *atrybuty*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="c29a4-756">Wszystkie parametry formalne i typ parametrów muszą mieć różne nazwy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="c29a4-757">Parametry metody</span><span class="sxs-lookup"><span data-stu-id="c29a4-757">Method parameters</span></span>

<span data-ttu-id="c29a4-758">Parametry metody, jeśli są zadeklarowane za pomocą metody *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

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

<span data-ttu-id="c29a4-759">Na liście parametrów formalnych składa się z jednego lub więcej rozdzielonych przecinkami parametrów, z których mogą być tylko ostatnie *parameter_array*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="c29a4-760">A *fixed_parameter* zawiera opcjonalny zestaw *atrybuty* ([atrybuty](attributes.md)), opcjonalny `ref`, `out` lub `this` , modyfikator *typu*, *identyfikator* oraz opcjonalny *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="c29a4-761">Każdy *fixed_parameter* deklaruje parametr danego typu o podanej nazwie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="c29a4-762">`this` Modyfikator określa metodę jako metodę rozszerzenia i jest dozwolona tylko w pierwszym parametrze metody statycznej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="c29a4-763">Metody rozszerzenia są opisane w [metody rozszerzenia](classes.md#extension-methods).</span><span class="sxs-lookup"><span data-stu-id="c29a4-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="c29a4-764">A *fixed_parameter* z *default_argument* jest znany jako ***opcjonalny parametr***, podczas gdy *fixed_parameter* bez *default_argument* jest ***wymaganego parametru***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="c29a4-765">Wymagany parametr nie może występować po opcjonalnym parametrze w *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="c29a4-766">A `ref` lub `out` parametr nie może mieć *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="c29a4-767">*Wyrażenie* w *default_argument* musi mieć jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c29a4-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="c29a4-768">a *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="c29a4-768">a *constant_expression*</span></span>
*  <span data-ttu-id="c29a4-769">wyrażenie w formie `new S()` gdzie `S` jest typem wartości</span><span class="sxs-lookup"><span data-stu-id="c29a4-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="c29a4-770">wyrażenie w formie `default(S)` gdzie `S` jest typem wartości</span><span class="sxs-lookup"><span data-stu-id="c29a4-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="c29a4-771">*Wyrażenie* musi umożliwiać niejawną konwersję przez tożsamość lub dopuszczające wartość null konwersji na typ parametru.</span><span class="sxs-lookup"><span data-stu-id="c29a4-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="c29a4-772">Jeśli następujące parametry opcjonalne występować w deklaracji implementującej metody częściowej ([metod częściowych](classes.md#partial-methods)), jawną implementacją członków ([implementacji elementu członkowskiego interfejsu jawnego](interfaces.md#explicit-interface-member-implementations)) lub w Deklaracja pojedynczego parametru indeksatora ([indeksatory](classes.md#indexers)) kompilator powinien zawierać ostrzeżenie, ponieważ te elementy członkowskie nigdy nie może być wywoływany w sposób, który pozwala na argumenty, które można pominąć.</span><span class="sxs-lookup"><span data-stu-id="c29a4-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="c29a4-773">A *parameter_array* zawiera opcjonalny zestaw *atrybuty* ([atrybuty](attributes.md)), `params` , modyfikator *array_type*, i *identyfikator*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="c29a4-774">Tablica parametrów deklaruje jeden parametr typu danej tablicy o podanej nazwie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="c29a4-775">*Array_type* parametru tablicy musi być typem tablicy jednowymiarowej ([tablicy typów](arrays.md#array-types)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="c29a4-776">W wywołaniu metody tablicy parametrów zezwala na pojedynczy argument typu danej tablicy, należy określić lub pozwala na zero lub więcej argumentów typu elementu tablicy, należy określić.</span><span class="sxs-lookup"><span data-stu-id="c29a4-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="c29a4-777">Parameter — tablice są dokładniejszym opisem zawartym w [Parameter — tablice](classes.md#parameter-arrays).</span><span class="sxs-lookup"><span data-stu-id="c29a4-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="c29a4-778">A *parameter_array* może występować po opcjonalnym parametrze, ale nie może mieć wartości domyślnej — pominięcie argumentów *parameter_array* zamiast tego spowoduje utworzenie pustej tablicy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="c29a4-779">Poniższy przykład ilustruje różne rodzaje parametrów:</span><span class="sxs-lookup"><span data-stu-id="c29a4-779">The following example illustrates different kinds of parameters:</span></span>
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

<span data-ttu-id="c29a4-780">W *formal_parameter_list* dla `M`, `i` jest parametrem ref wymagane `d` jest parametrem wymaganej wartości `b`, `s`, `o` i `t` Opcjonalna wartość parametrów i `a` jest tablicą parametrów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="c29a4-781">Deklaracja metody tworzy miejsce na oddzielnych deklaracji parametrów, typów parametrów i zmiennych lokalnych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="c29a4-782">Nazwy są wprowadzane do tego obszaru deklaracji, lista parametrów typu i formalnej listy parametrów metody oraz lokalne deklaracje zmiennych w *bloku* metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="c29a4-783">Jest to błąd, dla dwóch członków miejsca deklaracji metody mieć taką samą nazwę.</span><span class="sxs-lookup"><span data-stu-id="c29a4-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="c29a4-784">Jest to błąd dla deklaracji metody i miejsca deklaracji zmiennej lokalnej w deklaracji zagnieżdżonej przestrzeni ma zawierać elementy o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="c29a4-785">Wywołanie metody ([wywołań metody opisywanego](expressions.md#method-invocations)) tworzy kopię, specyficzne dla tego wywołania, parametry formalne i zmienne lokalne, metody i listą argumentów wywołania przypisuje wartości lub zmiennej odwołania do nowo utworzona parametrów formalnych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="c29a4-786">W ramach *bloku* metody parametrów formalnych mogą być przywoływane przez ich identyfikatorów w *simple_name* wyrażenia ([proste nazwy](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="c29a4-787">Istnieją cztery rodzaje parametrów formalnych:</span><span class="sxs-lookup"><span data-stu-id="c29a4-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="c29a4-788">Wartości parametrów, które są zadeklarowane bez żadnych modyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="c29a4-789">Parametry, które są zadeklarowane za pomocą odwołania `ref` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="c29a4-790">Parametry wyjściowe, które są zadeklarowane za pomocą `out` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="c29a4-791">Parameter — tablice, które są zadeklarowane za pomocą `params` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="c29a4-792">Zgodnie z opisem w [podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading), `ref` i `out` Modyfikatory stanowią część podpisu metody, ale `params` modyfikator nie jest.</span><span class="sxs-lookup"><span data-stu-id="c29a4-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="c29a4-793">Wartości parametrów</span><span class="sxs-lookup"><span data-stu-id="c29a4-793">Value parameters</span></span>

<span data-ttu-id="c29a4-794">Parametr zadeklarowane za pomocą modyfikatorów nie jest parametrem wartości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="c29a4-795">Wartość parametru odnosi się do zmiennej lokalnej, która pobiera jej wartość początkową z odpowiadający argument podany w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="c29a4-796">Formalny parametr po parametru wartości odnośnego argumentu w wywołaniu metody musi być wyrażeniem, który jest niejawnie konwertowany ([niejawne konwersje](conversions.md#implicit-conversions)) do typu formalnego parametru.</span><span class="sxs-lookup"><span data-stu-id="c29a4-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="c29a4-797">Metoda może przypisać nowe wartości do parametru wartości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="c29a4-798">Takie przypisania wpływ tylko na lokalizację magazynu lokalnego reprezentowanego przez parametr wartość — nie mają wpływu na rzeczywisty argument podany w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="c29a4-799">Parametry odwołania</span><span class="sxs-lookup"><span data-stu-id="c29a4-799">Reference parameters</span></span>

<span data-ttu-id="c29a4-800">Parametr zadeklarowana za pomocą `ref` modyfikator jest parametr przekazany przez odwołanie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="c29a4-801">W odróżnieniu od wartość parametru parametr przekazany przez odwołanie, nie powoduje utworzenia nowej lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="c29a4-802">Zamiast tego parametru odwołania reprezentuje tę samą lokalizację magazynu zmienna, podane jako argument w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="c29a4-803">Gdy parametr formalny jest parametr przekazany przez odwołanie, odpowiadający argument w wywołaniu metody musi zawierać słowa kluczowego `ref` następuje *variable_reference* ([dokładne zasady ustalania asercja określonego przydziału](variables.md#precise-rules-for-determining-definite-assignment)) z tego samego typu co parametr formalny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="c29a4-804">Zmienna musi być zdecydowanie przypisana, zanim może być przekazywany jako parametr przekazany przez odwołanie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="c29a4-805">W metodzie parametr przekazany przez odwołanie jest zawsze traktowany jako zdecydowanie przypisany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="c29a4-806">Metoda zadeklarowany jako iterator ([Iteratory](classes.md#iterators)) nie może mieć parametrów odwołania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="c29a4-807">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-807">The example</span></span>
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
<span data-ttu-id="c29a4-808">generuje dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-808">produces the output</span></span>
```
i = 2, j = 1
```

<span data-ttu-id="c29a4-809">Dla wywołania elementu `Swap` w `Main`, `x` reprezentuje `i` i `y` reprezentuje `j`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="c29a4-810">W związku z tym, wywołanie powoduje zamianę wartości `i` i `j`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="c29a4-811">W metodzie, która pobiera parametry odwołania, istnieje możliwość, że wiele nazw do reprezentowania tej samej lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="c29a4-812">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-812">In the example</span></span>
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
<span data-ttu-id="c29a4-813">wywołanie `F` w `G` odwołuje się do `s` dla obu `a` i `b`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="c29a4-814">W związku z tym, dla tego wywołania, nazwy `s`, `a`, i `b` wszystkie odnoszą się do tej samej lokalizacji magazynu, a następnie wszystkie przypisania trzy Zmodyfikuj pole wystąpienia `s`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="c29a4-815">Parametry wyjściowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-815">Output parameters</span></span>

<span data-ttu-id="c29a4-816">Parametr zadeklarowana za pomocą `out` modyfikator jest parametrem wyjściowym.</span><span class="sxs-lookup"><span data-stu-id="c29a4-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="c29a4-817">Podobnie jak parametr przekazany przez odwołanie, parametr wyjściowy nie powoduje utworzenia nowej lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="c29a4-818">Zamiast tego parametru output reprezentuje tę samą lokalizację magazynu zmienna, podane jako argument w wywołaniu metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="c29a4-819">Gdy parametr formalny jest parametr wyjściowy, odpowiadający argument w wywołaniu metody musi składać się z słowa kluczowego `out` następuje *variable_reference* ([dokładne zasady ustalania asercja określonego przydziału](variables.md#precise-rules-for-determining-definite-assignment)) z tego samego typu co parametr formalny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="c29a4-820">Zmienna musi nie być zdecydowanie przypisana może być przekazywany jako parametr wyjściowy, ale następujące wywołania, gdy zmienna została przekazana jako parametr wyjściowy, zmienna jest uznawany za zdecydowanie przypisany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="c29a4-821">W metodzie, podobnie jak w przypadku lokalnej zmiennej, parametr wyjściowy jest początkowo uznawany za nieprzypisanych i musi być zdecydowanie przypisana, zanim jego wartość jest używana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="c29a4-822">Każdy parametr danych wyjściowych metody musi być zdecydowanie przypisana, przed powrotem z metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="c29a4-823">Metody zadeklarowane jako metody częściowej ([metod częściowych](classes.md#partial-methods)) lub iteratora ([Iteratory](classes.md#iterators)) nie można wyprowadzić parametrów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="c29a4-824">Parametry wyjściowe są zwykle używane w metodach, które wywołują z wieloma wartościami zwracanymi.</span><span class="sxs-lookup"><span data-stu-id="c29a4-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="c29a4-825">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c29a4-825">For example:</span></span>
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

<span data-ttu-id="c29a4-826">Przykład generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="c29a4-826">The example produces the output:</span></span>
```
c:\Windows\System\
hello.txt
```

<span data-ttu-id="c29a4-827">Należy pamiętać, że `dir` i `name` zmienne mogą być nieprzypisane przed przekazaniem ich do `SplitPath`, i że są uwzględniane zdecydowanie przypisany następujący po wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="c29a4-828">Parameter — tablice</span><span class="sxs-lookup"><span data-stu-id="c29a4-828">Parameter arrays</span></span>

<span data-ttu-id="c29a4-829">Parametr zadeklarowana za pomocą `params` modyfikator jest tablicą parametrów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="c29a4-830">Jeśli formalnej listy parametrów zawiera tablicy parametrów, musi być ostatnim parametrem na liście, a musi być typu tablicy jednowymiarowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="c29a4-831">Na przykład typy `string[]` i `string[][]` może być używany jako typ tablicy parametrów, ale typ `string[,]` nie jest możliwe.</span><span class="sxs-lookup"><span data-stu-id="c29a4-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="c29a4-832">Nie jest możliwe połączyć `params` modyfikator z modyfikatorami `ref` i `out`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="c29a4-833">Tablica parametrów zezwala na argumenty, które można określić w jeden z dwóch sposobów, w wywołaniu metody:</span><span class="sxs-lookup"><span data-stu-id="c29a4-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="c29a4-834">Argument podany dla tablicy parametrów może być wyrażeniem pojedynczym, który jest niejawnie konwertowany ([niejawne konwersje](conversions.md#implicit-conversions)) na typ tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="c29a4-835">W tym przypadku tablicy parametrów funkcjonuje dokładnie parametru wartości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="c29a4-836">Alternatywnie określić wywołanie zero lub więcej argumentów dla tablicy parametrów, w którym każdy argument jest wyrażeniem, które jest niejawnie konwertowany ([niejawne konwersje](conversions.md#implicit-conversions)) na typ elementu tablicy parametrów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="c29a4-837">W takim przypadku wywołanie tworzy wystąpienie typu parametru tablicy o długości odpowiadającej liczbę argumentów, inicjuje elementy wystąpienia tablicy wartościami podany argument i używa wystąpienia nowo utworzona tablica jako rzeczywisty argument.</span><span class="sxs-lookup"><span data-stu-id="c29a4-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="c29a4-838">Z wyjątkiem umożliwiając zmienną liczbę argumentów w wywołaniu, tablicy parametrów odpowiada dokładnie parametru wartości ([wartości parametrów](classes.md#value-parameters)) tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="c29a4-839">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-839">The example</span></span>
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
<span data-ttu-id="c29a4-840">generuje dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-840">produces the output</span></span>
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="c29a4-841">Pierwsze wywołanie `F` po prostu przekazuje tablicy `a` jako parametru wartości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="c29a4-842">Drugie wywołanie `F` automatycznie tworzy element czterech `int[]` z wartości danego elementu i przebiegi, które wystąpienie jako parametru wartości w tablicy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="c29a4-843">Podobnie, trzeci wywołanie `F` tworzy zero element `int[]` i przekazanie danego wystąpienia jako parametru wartości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="c29a4-844">Drugi i trzeci wywołania są dokładnie odpowiednikiem pisania:</span><span class="sxs-lookup"><span data-stu-id="c29a4-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="c29a4-845">Podczas rozpoznawania przeciążenia metody z tablicą parametrów mogą być stosowane, w postaci normalnym lub w postaci rozwiniętej ([dotyczy funkcja składowa](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="c29a4-846">Rozwinięty formularza metody jest dostępna tylko wtedy, gdy jest to normalne formularza metody nie ma zastosowania i tylko wtedy, gdy odpowiedniej metody o tej samej sygnaturze, rozwinięty formularza nie jest już zadeklarowany w tym samym typie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="c29a4-847">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-847">The example</span></span>
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
<span data-ttu-id="c29a4-848">generuje dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-848">produces the output</span></span>
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="c29a4-849">W tym przykładzie dwa możliwe formy rozwiniętej metody z tablicą parametrów znajdują się już w klasie jako metody regularnego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="c29a4-850">Te formularze rozwinięty w związku z tym nie są uwzględniane podczas rozpoznawania przeciążenia i wywołań metod pierwszy i trzeci związku z tym Wybierz metody regularnego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="c29a4-851">Gdy klasa deklaruje metodę z tablicą parametrów, nie jest niczym niezwykłym również obejmować niektóre spośród rozwiniętej forms jako metody regularnego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="c29a4-852">W ten sposób można uniknąć alokacji tablicy wystąpienie, które występuje, gdy w formie metody z tablicą parametrów jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="c29a4-853">Jeśli typ tablicy parametrów to `object[]`, potencjalną niejednoznacznością pojawia się między normalnym metody expended w formularzach i dla pojedynczego `object` parametru.</span><span class="sxs-lookup"><span data-stu-id="c29a4-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="c29a4-854">Przyczyny niejasności jest fakt, że `object[]` sama jest niejawnie konwertowany na typ `object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="c29a4-855">Niejednoznaczności przedstawia nie ma problemu, ponieważ może zostać rozpoznana przez wstawienie rzutowania, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="c29a4-856">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-856">The example</span></span>
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
<span data-ttu-id="c29a4-857">generuje dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-857">produces the output</span></span>
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="c29a4-858">W pierwszym i ostatnim wywołań `F`, normalne formie `F` ma zastosowanie, ponieważ istnieje niejawna konwersja z typu argumentu typu parametru (oba są typu `object[]`).</span><span class="sxs-lookup"><span data-stu-id="c29a4-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="c29a4-859">W związku z tym, funkcja rozpoznawania przeciążeń wybiera normalne formie `F`, a argument jest przekazywany jako parametr wartość regularne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="c29a4-860">W drugi i trzeci wywołań, normalne formie `F` nie ma zastosowania, ponieważ nie istnieje niejawna konwersja z typu argumentu z typem parametru (typ `object` nie może być niejawnie konwertowany na typ `object[]`).</span><span class="sxs-lookup"><span data-stu-id="c29a4-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="c29a4-861">Jednak rozszerzonej postaci `F` jest stosowana, dzięki czemu jest ono zaznaczone w przeciążeniu rozdzielczości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="c29a4-862">W wyniku element jeden `object[]` jest tworzony przez wywołanie, a pojedynczy element tablicy jest inicjowany z wartością podany argument (która sama jest odwołaniem do `object[]`).</span><span class="sxs-lookup"><span data-stu-id="c29a4-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="c29a4-863">Metody statyczne i wystąpienia</span><span class="sxs-lookup"><span data-stu-id="c29a4-863">Static and instance methods</span></span>

<span data-ttu-id="c29a4-864">Po deklaracji metody zawiera `static` modyfikator, że metoda jest określany jako metoda statyczna.</span><span class="sxs-lookup"><span data-stu-id="c29a4-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="c29a4-865">Gdy nie `static` modyfikator, metoda jest określany jako metodę wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="c29a4-866">Metoda statyczna nie będzie działać na określonym wystąpieniu i jest to błąd kompilacji, aby odwołać się do `this` w metodzie statycznej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="c29a4-867">Metoda wystąpienia działa danego wystąpienia klasy, i to wystąpienie może być dostępna jako `this` ([dostęp](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="c29a4-868">Gdy odwołuje się do metody *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `E.M`, jeśli `M` jest statycznej metody `E` musi oznaczają typu zawierającego `M`i jeśli `M` jest metodą wystąpienia `E` musi oznaczają wystąpienia typu zawierającego `M`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="c29a4-869">Różnice statyczne elementy członkowskie wystąpień zostały one omówione w dalszych [statyczna i wystąpienia elementów członkowskich](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="c29a4-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="c29a4-870">Metody wirtualne</span><span class="sxs-lookup"><span data-stu-id="c29a4-870">Virtual methods</span></span>

<span data-ttu-id="c29a4-871">Po deklaracji metody wystąpienia obejmuje `virtual` modyfikator, że metoda jest określany jako metodę wirtualną.</span><span class="sxs-lookup"><span data-stu-id="c29a4-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="c29a4-872">Gdy nie `virtual` modyfikator, metoda jest określany jako metody niewirtualnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="c29a4-873">Implementacja metody niewirtualnej jest niezmienny: Implementacja jest taki sam, czy metoda jest wywoływana w wystąpieniu klasy, w którym jest zdeklarowana lub wystąpienie klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="c29a4-874">Z kolei implementację metody wirtualnej może zostać zastąpiony przez klasy pochodne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="c29a4-875">Proces zastępowanie implementacji dziedziczoną metodę wirtualną jest znany jako ***zastępowanie*** tej metody ([Przesłaniaj metody](classes.md#override-methods)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="c29a4-876">W wywołaniu metody wirtualnej ***typu run-time*** wystąpienia, dla którego tego wywołania ma miejsce określa rzeczywista implementacja metody do wywołania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="c29a4-877">W wywołaniu metody niewirtualnej ***typów w czasie kompilacji*** wystąpienia jest czynnikiem decydującym.</span><span class="sxs-lookup"><span data-stu-id="c29a4-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="c29a4-878">W dokładne warunki, kiedy metodę o nazwie `N` jest wywoływana z listą argumentów `A` w wystąpieniu typu kompilacji `C` i typu run-time `R` (gdzie `R` jest `C` lub klasa pochodnej z `C`), wywołanie jest przetwarzana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c29a4-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="c29a4-879">Po pierwsze, funkcja rozpoznawania przeciążeń jest stosowany do `C`, `N`, i `A`, aby wybrać określonej metody `M` z zestawu metod zadeklarowanych w i dziedziczone przez `C`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="c29a4-880">Jest to opisane w [wywołań metody opisywanego](expressions.md#method-invocations).</span><span class="sxs-lookup"><span data-stu-id="c29a4-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="c29a4-881">Następnie, jeśli `M` jest metodą niewirtualną `M` zostanie wywołana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="c29a4-882">W przeciwnym razie `M` jest metodą wirtualną i wykonania najbardziej pochodnej `M` w odniesieniu do `R` zostanie wywołana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="c29a4-883">Dla każdej wirtualnej metody zadeklarowanej w lub dziedziczone przez klasy, istnieje ***najbardziej pochodnego implementacji*** metody w odniesieniu do tej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="c29a4-884">Najbardziej pochodnej metody wirtualnej `M` względem klasy `R` jest określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c29a4-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="c29a4-885">Jeśli `R` zawiera wprowadzenie do `virtual` deklaracji `M`, ten parametr jest wykonania najbardziej pochodnej `M`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="c29a4-886">W przeciwnym razie, jeśli `R` zawiera `override` z `M`, ten parametr jest wykonania najbardziej pochodnej `M`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="c29a4-887">W przeciwnym razie najbardziej pochodnej implementacji `M` w odniesieniu do `R` jest taka sama jak wykonania najbardziej pochodnej `M` względem bezpośrednia klasa podstawowa `R`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="c29a4-888">Poniższy przykład ilustruje różnice między metodami wirtualnej i niewirtualnej:</span><span class="sxs-lookup"><span data-stu-id="c29a4-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
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

<span data-ttu-id="c29a4-889">W tym przykładzie `A` wprowadza metody niewirtualnej `F` i metodę wirtualną `G`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="c29a4-890">Klasa `B` wprowadzono nowe metody niewirtualnej `F`, dlatego ukrywanie odziedziczonych `F`i zastępuje również dziedziczonej metody `G`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="c29a4-891">Przykład generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="c29a4-891">The example produces the output:</span></span>
```
A.F
B.F
B.G
B.G
```

<span data-ttu-id="c29a4-892">Należy zauważyć, że instrukcja `a.G()` wywołuje `B.G`, a nie `A.G`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="c29a4-893">Jest to spowodowane środowiska wykonawczego typu wystąpienia (czyli `B`), nie kompilacji Typ wystąpienia (czyli `A`), określa rzeczywista implementacja metody do wywołania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="c29a4-894">Ponieważ metody mogą ukryć metody dziedziczone, jest możliwe, klasa może zawierać kilka metod wirtualnych za pomocą tego samego podpisu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="c29a4-895">To nie przedstawi wystąpił problem niejednoznaczności, ponieważ wszystkie oprócz najbardziej pochodna metoda są ukryte.</span><span class="sxs-lookup"><span data-stu-id="c29a4-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="c29a4-896">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-896">In the example</span></span>
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
<span data-ttu-id="c29a4-897">`C` i `D` klasy zawierają dwie metody wirtualnej z tym samym podpisie: Jeden wprowadzone przez `A` a wynikające z `C`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="c29a4-898">Metoda wprowadzona przez `C` ukrywa metodę odziedziczone `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="c29a4-899">W związku z tym, deklaracji zastąpienie `D` zastępuje metodę wynikające z `C`, i nie jest możliwe dla `D` Aby przesłonić metodę wynikające z `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="c29a4-900">Przykład generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="c29a4-900">The example produces the output:</span></span>
```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="c29a4-901">Należy pamiętać, że jest możliwe do wywołania metody wirtualnej ukryte, uzyskując dostęp do wystąpienia `D` za pośrednictwem mniej pochodne typu, w którym metoda nie jest ukryty.</span><span class="sxs-lookup"><span data-stu-id="c29a4-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="c29a4-902">Przesłaniaj metody</span><span class="sxs-lookup"><span data-stu-id="c29a4-902">Override methods</span></span>

<span data-ttu-id="c29a4-903">Po deklaracji metody wystąpienia obejmuje `override` modyfikator, metoda jest nazywany ***przesłonić metodę***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="c29a4-904">To metoda przesłonięcia zastępuje dziedziczoną metodę wirtualną przy użyciu tego samego podpisu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="c29a4-905">Dlatego deklaracja metody wirtualnej wprowadzono nowe metody, deklaracji metody zastąpienie specjalizuje się istniejących dziedziczoną metodę wirtualną, podając nową metodę implementacji tej metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="c29a4-906">Metoda zastąpione `override` deklaracji jest znany jako ***przesłonięcia metody podstawowej***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="c29a4-907">Zastąpienie metody `M` zadeklarowana w klasie `C`, zastąpiona metoda podstawowa jest określana przez badanie każdego typu klasy bazowej `C`, począwszy od rodzaju bezpośrednią klasą bazową `C` i kontynuowanie z każdym kolejnych Typ bezpośrednią klasą bazową, dopóki w typie danej klasy bazowej co najmniej jeden z dostępnych metod jest znajduje się który ma taką samą sygnaturę jak `M` po zastąpienie argumentów typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="c29a4-908">Potrzeby lokalizowanie przesłonięte metody podstawowej metody jest traktowane jako niedostępny w `public`, jeśli jest on `protected`, jeśli jest on `protected internal`, lub jeśli jest `internal` i zadeklarowany w tym samym programie jako `C`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="c29a4-909">Błąd kompilacji występuje, o ile spełnione dla deklaracji zastąpienie są wszystkie z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c29a4-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="c29a4-910">Zastąpione metody podstawowej, można znaleźć, zgodnie z powyższym opisem.</span><span class="sxs-lookup"><span data-stu-id="c29a4-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="c29a4-911">Ma dokładnie jedno takie podstawowy przeciążonej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="c29a4-912">To ograniczenie obowiązuje tylko wtedy, gdy typ klasy bazowej jest skonstruowanego typu, w którym zastąpienie argumentów typu sprawia, że podpis metody dwa takie same.</span><span class="sxs-lookup"><span data-stu-id="c29a4-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="c29a4-913">Zastąpione metody podstawowej jest abstrakcyjny wirtualne, lub metoda musi zostać zastąpiona.</span><span class="sxs-lookup"><span data-stu-id="c29a4-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="c29a4-914">Innymi słowy przesłonięte metody podstawowej, nie może być statyczne lub -virtual.</span><span class="sxs-lookup"><span data-stu-id="c29a4-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="c29a4-915">Zastąpione metody podstawowej nie jest metodą zapieczętowany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="c29a4-916">Metoda przesłaniająca i zastąpione metody podstawowej mają taki sam typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="c29a4-917">Deklaracja zastąpienia i przesłonięte metody podstawowej mają ten sam deklarowana dostępność metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="c29a4-918">Innymi słowy deklaracja override nie można zmienić dostępność metody wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="c29a4-919">Jednak jeśli zastąpiona metoda podstawowa jest chronionych wewnętrznych, a zostanie ona zadeklarowana w innym zestawie, od deklarowanej zestawu zawierającego metodę przesłaniającą, a następnie metodę przesłaniającą dostępności muszą być chronione.</span><span class="sxs-lookup"><span data-stu-id="c29a4-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="c29a4-920">Deklaracja zastąpienie nie określono typu — parametr ograniczenia — klauzule.</span><span class="sxs-lookup"><span data-stu-id="c29a4-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="c29a4-921">Zamiast tego ograniczenia są dziedziczone z przesłonięte metody podstawowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="c29a4-922">Należy pamiętać, że ograniczenia, które są parametrów typu w metodzie zgodnym z przesłoniętą mogą być zastąpione przez argumenty typu w ograniczeniu dziedziczone.</span><span class="sxs-lookup"><span data-stu-id="c29a4-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="c29a4-923">Może to prowadzić do ograniczenia, które są niedozwolone, gdy określony jawnie, takie jak typy wartości lub typach zapieczętowanych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="c29a4-924">W poniższym przykładzie pokazano, jak działają zasady nadrzędne dla klas ogólnych:</span><span class="sxs-lookup"><span data-stu-id="c29a4-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
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

<span data-ttu-id="c29a4-925">Deklaracja zastąpienie mogą uzyskiwać dostęp do przesłonięte metody podstawowej, za pomocą *base_access* ([podstawowa dostępu](expressions.md#base-access)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="c29a4-926">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-926">In the example</span></span>
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
<span data-ttu-id="c29a4-927">`base.PrintFields()` wywołania w `B` wywołuje `PrintFields` metody zadeklarowane w `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="c29a4-928">A *base_access* wyłącza mechanizm wywołania wirtualnego i po prostu traktuje metody podstawowej jako metody niewirtualnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="c29a4-929">Było wywołanie `B` zostały napisane `((A)this).PrintFields()`, jak rekursywnie wywołania `PrintFields` metody zadeklarowane w `B`, nie jest zadeklarowana w `A`, ponieważ `PrintFields` jest wirtualny i typów w czasie wykonywania `((A)this)` jest `B`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="c29a4-930">Tylko Jeśli dołączysz `override` może modyfikator metody zastępują innej metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="c29a4-931">We wszystkich innych przypadkach metody z taką samą sygnaturę jak dziedziczonej metody po prostu ukrywa dziedziczonej metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="c29a4-932">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-932">In the example</span></span>
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
<span data-ttu-id="c29a4-933">`F` method in Class metoda `B` nie obejmuje `override` modyfikator i dlatego nie powoduje zastąpienia `F` method in Class metoda `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="c29a4-934">Zamiast `F` method in Class metoda `B` ukrywa metodę w `A`, i ostrzeżenie jest zgłaszane, ponieważ nie zawiera deklarację `new` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="c29a4-935">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-935">In the example</span></span>
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
<span data-ttu-id="c29a4-936">`F` method in Class metoda `B` ukrywa wirtualnego `F` metody dziedziczone z `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="c29a4-937">Ponieważ nowe `F` w `B` ma dostęp do prywatnych, jej zakres obejmuje tylko treści klasy `B` i nie obejmuje `C`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="c29a4-938">Dlatego deklaracja `F` w `C` jest dozwolone, Zastąp `F` odziedziczone `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="c29a4-939">Metody zapieczętowane</span><span class="sxs-lookup"><span data-stu-id="c29a4-939">Sealed methods</span></span>

<span data-ttu-id="c29a4-940">Po deklaracji metody wystąpienia obejmuje `sealed` modyfikator, że metoda jest nazywany ***zapieczętowany metoda***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="c29a4-941">Jeśli zawiera deklarację metody wystąpienia `sealed` modyfikator, musi również zawierać `override` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="c29a4-942">Korzystanie z `sealed` modyfikator uniemożliwia dalsze zastąpienie metody klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="c29a4-943">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-943">In the example</span></span>
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
<span data-ttu-id="c29a4-944">Klasa `B` zawiera dwa Przesłaniaj metody: `F` metody, która ma `sealed` modyfikator i `G` metody, która nie ma.</span><span class="sxs-lookup"><span data-stu-id="c29a4-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="c29a4-945">`B`używania zapieczętowanego `modifier` zapobiega `C` możliwość dalszego przesłaniania `F`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="c29a4-946">Metody abstrakcyjne</span><span class="sxs-lookup"><span data-stu-id="c29a4-946">Abstract methods</span></span>

<span data-ttu-id="c29a4-947">Po deklaracji metody wystąpienia obejmuje `abstract` modyfikator, że metoda jest nazywany ***metody abstrakcyjnej***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="c29a4-948">Mimo że metodę abstrakcyjną niejawnie jest również metodę wirtualną, nie może on zawierać modyfikator `virtual`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="c29a4-949">Deklaracja metody abstrakcyjnej wprowadzono nowe metody wirtualnej, ale nie zapewnia implementacja tej metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="c29a4-950">Zamiast tego nieabstrakcyjnej klasy pochodne są wymagane do zapewnienia ich własnych implementacji przez zastąpienie tej metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="c29a4-951">Ponieważ metodę abstrakcyjną nie zapewnia żadnych rzeczywistą implementację *method_body* metody abstrakcyjnej po prostu składa się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="c29a4-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="c29a4-952">Deklaracje metody abstrakcyjne są dozwolone tylko w klasy abstrakcyjne ([klasy abstrakcyjne](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="c29a4-953">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-953">In the example</span></span>
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
<span data-ttu-id="c29a4-954">`Shape` klasy definiuje abstrakcyjny pojęcie obiekt kształt geometryczny, za pomocą którego można malować sam.</span><span class="sxs-lookup"><span data-stu-id="c29a4-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="c29a4-955">`Paint` Metoda jest abstrakcyjna, ponieważ nie istnieje żadne istotne Domyślna implementacja.</span><span class="sxs-lookup"><span data-stu-id="c29a4-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="c29a4-956">`Ellipse` i `Box` klasy są konkretne `Shape` implementacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="c29a4-957">Ponieważ te klasy nieabstrakcyjnej, są wymagane do zastąpienia `Paint` metody i Podaj rzeczywistą implementację.</span><span class="sxs-lookup"><span data-stu-id="c29a4-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="c29a4-958">Jest to błąd czasu kompilacji dla *base_access* ([podstawowa dostępu](expressions.md#base-access)) można odwoływać się do metody abstrakcyjnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="c29a4-959">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-959">In the example</span></span>
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
<span data-ttu-id="c29a4-960">Błąd kompilacji jest zgłaszany, `base.F()` wywołania ponieważ widok odwołuje się metodę abstrakcyjną.</span><span class="sxs-lookup"><span data-stu-id="c29a4-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="c29a4-961">Deklaracji metoda abstrakcyjna może zastąpić metodę wirtualną.</span><span class="sxs-lookup"><span data-stu-id="c29a4-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="c29a4-962">Zapewnia to klasy abstrakcyjnej wymusić ponowną implementację metody w klasach pochodnych i powoduje, że oryginalnej implementacji metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="c29a4-963">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-963">In the example</span></span>
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
<span data-ttu-id="c29a4-964">Klasa `A` deklaruje metodę wirtualną klasy `B` przesłonięcia tej metody za pomocą metody abstrakcyjnej, a klasa `C` zastępuje metodę abstrakcyjną, aby podać własną implementację.</span><span class="sxs-lookup"><span data-stu-id="c29a4-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="c29a4-965">Metody zewnętrznej</span><span class="sxs-lookup"><span data-stu-id="c29a4-965">External methods</span></span>

<span data-ttu-id="c29a4-966">Po deklaracji metody zawiera `extern` modyfikator, że metoda jest nazywany ***metody zewnętrznej***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="c29a4-967">Metody zewnętrzne są implementowane zewnętrznie, zwykle korzystając z języka innego niż C#.</span><span class="sxs-lookup"><span data-stu-id="c29a4-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="c29a4-968">Ponieważ deklaracja metody zewnętrznej zapewnia nie rzeczywistego wykonania *method_body* metody zewnętrznej po prostu składa się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="c29a4-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="c29a4-969">Metody zewnętrznej nie może być ogólny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-969">An external method may not be generic.</span></span>

<span data-ttu-id="c29a4-970">`extern` Modyfikator jest zwykle używany w połączeniu z `DllImport` atrybutu ([współdziałanie ze składnikami COM i Win32](attributes.md#interoperation-with-com-and-win32-components)), dzięki czemu zewnętrznych metody implementowane przez biblioteki dll (biblioteki dll).</span><span class="sxs-lookup"><span data-stu-id="c29a4-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="c29a4-971">Środowisko wykonawcze może obsługiwać inne mechanizmy, według której można podać implementacje metod zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="c29a4-972">Kiedy zawiera metody zewnętrznej `DllImport` atrybutu deklaracji metody należy również uwzględnić `static` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="c29a4-973">W tym przykładzie pokazano użycie `extern` modyfikator i `DllImport` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="c29a4-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
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

### <a name="partial-methods-recap"></a><span data-ttu-id="c29a4-974">Metody częściowe (podsumowanie)</span><span class="sxs-lookup"><span data-stu-id="c29a4-974">Partial methods (recap)</span></span>

<span data-ttu-id="c29a4-975">Po deklaracji metody zawiera `partial` modyfikator, że metoda jest nazywany ***metody częściowej***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="c29a4-976">Metody częściowe mogą być deklarowane tylko jako członkowie typów częściowych ([typów częściowych](classes.md#partial-types)) i podlegają kilku ograniczeniom.</span><span class="sxs-lookup"><span data-stu-id="c29a4-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="c29a4-977">Metody częściowe są opisane w [metod częściowych](classes.md#partial-methods).</span><span class="sxs-lookup"><span data-stu-id="c29a4-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="c29a4-978">Metody rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="c29a4-978">Extension methods</span></span>

<span data-ttu-id="c29a4-979">Gdy pierwszy parametr metody zawiera `this` modyfikator, że metoda jest nazywany ***— metoda rozszerzenia***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="c29a4-980">Metody rozszerzenia mogą być deklarowane tylko w nieogólnej, -nested klas statycznych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="c29a4-981">Pierwszy parametr metody rozszerzenia może mieć inny niż brak modyfikatorów `this`, a typ parametru nie może być typem wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="c29a4-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="c29a4-982">Oto przykład klasy statycznej, która deklaruje dwie metody rozszerzające:</span><span class="sxs-lookup"><span data-stu-id="c29a4-982">The following is an example of a static class that declares two extension methods:</span></span>
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

<span data-ttu-id="c29a4-983">Metoda rozszerzenia jest regularne metody statycznej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-983">An extension method is a regular static method.</span></span> <span data-ttu-id="c29a4-984">Ponadto w przypadku, gdy jego otaczającej klasy statycznej znajduje się w zakresie, metoda rozszerzenia może być wywoływany przy użyciu składni wywołania metody wystąpienia ([wywołań metod rozszerzenia](expressions.md#extension-method-invocations)), za pomocą wyrażenia odbiornika jako pierwszy argument.</span><span class="sxs-lookup"><span data-stu-id="c29a4-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="c29a4-985">Następujący program używa metody rozszerzenia zadeklarowanej powyżej:</span><span class="sxs-lookup"><span data-stu-id="c29a4-985">The following program uses the extension methods declared above:</span></span>
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

<span data-ttu-id="c29a4-986">`Slice` Metoda jest dostępna na `string[]`i `ToInt32` metoda jest dostępna na `string`, ponieważ zostały zadeklarowane jako metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="c29a4-987">Znaczenie program jest taka sama jak poniższe, przy użyciu wywołania zwykłej statycznej metody:</span><span class="sxs-lookup"><span data-stu-id="c29a4-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
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

### <a name="method-body"></a><span data-ttu-id="c29a4-988">Treść metody</span><span class="sxs-lookup"><span data-stu-id="c29a4-988">Method body</span></span>

<span data-ttu-id="c29a4-989">*Method_body* metody deklaracji składa się z treści bloku, treści wyrażenia lub średnikami.</span><span class="sxs-lookup"><span data-stu-id="c29a4-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="c29a4-990">***Wyniku typu*** metody jest `void` Jeśli typ zwracany jest `void`, lub jeśli metoda jest asynchroniczne, a typem zwracanym jest `System.Threading.Tasks.Task`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="c29a4-991">W przeciwnym razie typ wyniku metody async nie jest typem zwracanym, a typ wyniku metody asynchronicznej, z typem zwracanym `System.Threading.Tasks.Task<T>` jest `T`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="c29a4-992">Gdy metoda ma `void` wyniku typu i treści bloku `return` instrukcji ([instrukcji return](statements.md#the-return-statement)) w bloku nie można określić wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="c29a4-993">Jeśli wykonanie bloku to metoda void zakończy się normalnie (czyli kontrolę nad przepływów poza koniec treści metody), że metoda po prostu wraca do bieżącego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="c29a4-994">Gdy metoda ma `void` wynik i treści wyrażenia wyrażenie `E` musi być *statement_expression*, oraz treść jest równoznaczny z treści bloku, w postaci `{ E; }`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="c29a4-995">Gdy metoda ma typ inny niż void wynik i blok treści, każdy `return` instrukcji w bloku, należy określić wyrażenie, które jest niejawnie konwertowany na typ wyniku.</span><span class="sxs-lookup"><span data-stu-id="c29a4-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="c29a4-996">Punkt końcowy treści bloku metody, zwracając wartość nie może być dostępny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="c29a4-997">Innymi słowy w metodzie zwracanie wartości z treści bloku, formant nie jest dozwolona przepływ poza koniec treści metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="c29a4-998">Metoda ma typ inny niż void wynik i treści wyrażenia, wyrażenie musi być niejawnie konwertowane na typ wyniku, gdy treść jest równoznaczny z treści bloku, w postaci `{ return E; }`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="c29a4-999">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-999">In the example</span></span>
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
<span data-ttu-id="c29a4-1000">Zwraca wartość `F` metoda nie powoduje błąd w czasie kompilacji, ponieważ może przepływ sterowania poza koniec treści metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="c29a4-1001">`G` i `H` metody są poprawne, dlatego wszystkie możliwe wykonania ścieżki w instrukcji return, która określa wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="c29a4-1002">`I` Metody jest poprawna, ponieważ jego treść jest odpowiednikiem blok instrukcji przy użyciu tylko jednej instrukcji return w nim.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="c29a4-1003">Przeciążenie metody</span><span class="sxs-lookup"><span data-stu-id="c29a4-1003">Method overloading</span></span>

<span data-ttu-id="c29a4-1004">Zasad rozpoznawania przeciążenia metody są opisane w [wnioskowanie o typie](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="c29a4-1005">Właściwości</span><span class="sxs-lookup"><span data-stu-id="c29a4-1005">Properties</span></span>

<span data-ttu-id="c29a4-1006">A ***właściwość*** jest elementem członkowskim, który zapewnia dostęp do właściwości obiektu lub klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="c29a4-1007">Przykłady właściwości obejmują długość ciągu, rozmiar czcionki, podpis okna, nazwę klienta i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="c29a4-1008">Właściwości to naturalne rozszerzenie pola — nazwanych elementów członkowskich skojarzonych typów i składnia do uzyskiwania dostępu do pola i właściwości jest taka sama.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="c29a4-1009">Jednak w przeciwieństwie do pola, właściwości określa lokalizacji przechowywania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="c29a4-1010">Właściwości mają ***Akcesory*** określające instrukcji do wykonania, gdy ich wartości są odczytu lub zapisu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="c29a4-1011">Właściwości związku z tym mechanizm kojarzenia akcji z odczytu i zapisu obiektu właściwości. Ponadto pozwalają takie atrybuty, które ma zostać obliczony.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="c29a4-1012">Właściwości są deklarowane przy użyciu *property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1012">Properties are declared using *property_declaration*s:</span></span>

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

<span data-ttu-id="c29a4-1013">A *property_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)) i prawidłową kombinację modyfikatory dostępu cztery ([modyfikatorach dostępu ](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `static` ([metody statyczne i wystąpienia](classes.md#static-and-instance-methods)), `virtual` ([metody wirtualnej](classes.md#virtual-methods)), `override` ([Przesłaniaj metody](classes.md#override-methods)), `sealed` ([zapieczętowany metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)), a `extern` ([Metody zewnętrznej](classes.md#external-methods)) modyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="c29a4-1014">Deklaracje właściwości podlegają te same reguły jako deklaracje metody ([metody](classes.md#methods)) w odniesieniu do ważnych kombinacji modyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="c29a4-1015">*Typu* właściwości deklaracja określa typ właściwości wprowadzonych przez tę deklarację i *member_name* Określa nazwę właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="c29a4-1016">Chyba że właściwość jest jawną implementacją członków, *member_name* jest po prostu *identyfikator*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="c29a4-1017">Aby uzyskać jawną implementacją członków ([implementacji elementu członkowskiego interfejsu jawnego](interfaces.md#explicit-interface-member-implementations)), *member_name* składa się z *interface_type* następuje " `.`"i *identyfikator*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="c29a4-1018">*Typu* właściwości musi być co najmniej tak samo dostępna jak samej właściwości ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c29a4-1019">A *property_body* może albo składają się z ***treści metody dostępu*** lub ***treści wyrażenia***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="c29a4-1020">W treści metody dostępu *accessor_declarations*, musi być ujęta w "`{`"i"`}`" tokenów, Zadeklaruj metody dostępu ([Akcesory](classes.md#accessors)) właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="c29a4-1021">Metody dostępu Określ instrukcji wykonywalnych powiązane z odczytywaniem i zapisywania właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="c29a4-1022">Treść wyrażenia składające się z `=>` następuje *wyrażenie* `E` , a dokładnie odpowiada na treść instrukcji średnikiem `{ get { return E; } }`i w związku z tym należy używać tylko do określenia tylko metod pobierających właściwości, których wynikiem metody pobierającej jest nadawana przez jedno wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="c29a4-1023">A *property_initializer* mogą być przydzielane tylko dla automatycznie implementowanej właściwości ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)) i powoduje, że Inicjalizacja odpowiedniego pola takich właściwości z wartością podane przez *wyrażenie*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="c29a4-1024">Mimo że składnia służąca do uzyskiwania dostępu do właściwości jest taka sama jak dla pola, właściwości nie jest sklasyfikowany jako zmienną.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="c29a4-1025">W związku z tym, nie jest możliwe do przekazania właściwość jako `ref` lub `out` argumentu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="c29a4-1026">Jeśli deklaracja właściwości zawiera `extern` modyfikator, właściwość jest nazywany ***właściwość external***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="c29a4-1027">Ponieważ deklaracja właściwości zewnętrznych zapewnia nie rzeczywistą implementację każdego z jego *accessor_declarations* składa się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="c29a4-1028">Właściwości statyczne i wystąpienia</span><span class="sxs-lookup"><span data-stu-id="c29a4-1028">Static and instance properties</span></span>

<span data-ttu-id="c29a4-1029">Jeśli deklaracja właściwości zawiera `static` modyfikator, właściwość jest nazywany ***właściwość statyczna***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="c29a4-1030">Gdy nie `static` modyfikator, jest określany jako właściwość ***wystąpienia właściwości***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="c29a4-1031">Właściwość statyczna nie jest skojarzony z określonym wystąpieniem i istnieje błąd w czasie kompilacji do odwoływania się do `this` w metodach dostępu właściwości statycznej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="c29a4-1032">Właściwość wystąpienia jest skojarzona z danego wystąpienia klasy, a to wystąpienie może być dostępna jako `this` ([dostęp](expressions.md#this-access)) w metodach dostępu właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="c29a4-1033">Gdy właściwość mowa w punkcie *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `E.M`, jeśli `M` jest właściwość statyczna `E` musi oznaczają typu zawierającego `M`i jeśli `M` jest właściwością wystąpienia E musi oznaczają wystąpienia typu zawierającego `M`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="c29a4-1034">Różnice statyczne elementy członkowskie wystąpień zostały one omówione w dalszych [statyczna i wystąpienia elementów członkowskich](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="c29a4-1035">Metod dostępu</span><span class="sxs-lookup"><span data-stu-id="c29a4-1035">Accessors</span></span>

<span data-ttu-id="c29a4-1036">*Accessor_declarations* właściwość Podaj instrukcje wykonywalne skojarzone z odczytywaniem i zapisywaniem tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

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

<span data-ttu-id="c29a4-1037">Deklaracje metody dostępu składają się z *get_accessor_declaration*, *set_accessor_declaration*, lub obu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="c29a4-1038">Każda deklaracja dostępu składa się z tokenem `get` lub `set` następuje opcjonalny *accessor_modifier* i *accessor_body*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="c29a4-1039">Korzystanie z *accessor_modifier*s podlega następującym ograniczeniom:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="c29a4-1040">*Accessor_modifier* nie można używać w interfejsie lub jawną implementacją członków.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="c29a4-1041">Dla właściwości lub indeksatora, który nie ma `override` , modyfikator *accessor_modifier* jest dozwolona tylko wtedy, gdy właściwość lub indeksator zarówno `get` i `set` metody dostępu i następnie jest dozwolona tylko w jednym z tych metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="c29a4-1042">Dla właściwości lub indeksatora, który zawiera `override` , modyfikator dostępu musi odpowiadać *accessor_modifier*(jeśli istnieje) zastąpieniu metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="c29a4-1043">*Accessor_modifier* musi deklarować ułatwień dostępu, który jest ściśle bardziej restrykcyjny niż deklarowana dostępność metody, właściwości lub indeksatora, sam.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="c29a4-1044">Aby była precyzyjna:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1044">To be precise:</span></span>
   * <span data-ttu-id="c29a4-1045">Jeśli właściwość lub indeksator ma deklarowana dostępność metody `public`, *accessor_modifier* może być `protected internal`, `internal`, `protected`, lub `private`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="c29a4-1046">Jeśli właściwość lub indeksator ma deklarowana dostępność metody `protected internal`, *accessor_modifier* może być `internal`, `protected`, lub `private`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="c29a4-1047">Jeśli właściwość lub indeksator ma deklarowana dostępność metody `internal` lub `protected`, *accessor_modifier* musi być `private`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="c29a4-1048">Jeśli właściwość lub indeksator ma deklarowana dostępność metody `private`, nie *accessor_modifier* mogą być używane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="c29a4-1049">Aby uzyskać `abstract` i `extern` właściwości *accessor_body* dla każdego dostępu określony jest po prostu średnikiem.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="c29a4-1050">Właściwość nieabstrakcyjnej, bez extern może mieć każdy *accessor_body* się średnikiem, w którym to przypadku jest ***automatycznie implementowana właściwość*** ([automatycznie implementowane właściwości ](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="c29a4-1051">Automatycznie implementowanej właściwości musi mieć co najmniej jeden metody dostępu get.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="c29a4-1052">Dla metod dostępu wszystkich innych nieabstrakcyjnej, bez extern właściwości *accessor_body* jest *bloku* określający instrukcji do wykonania, gdy zostanie wywołana odpowiedniej metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="c29a4-1053">A `get` akcesor odnosi się do metody bez parametrów, z wartością zwracaną z typem właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="c29a4-1054">Z wyjątkiem elementem docelowym przypisania, gdy właściwość jest przywoływany w wyrażeniu, `get` metody dostępu właściwości jest wywoływana w celu obliczenia wartości właściwości ([wartości wyrażeń](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="c29a4-1055">Treść `get` metody dostępu muszą być zgodne z regułami dotyczącymi zwracanie wartości metod opisanych w [treści metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="c29a4-1056">W szczególności wszystkich `return` instrukcji w treści `get` dostępu należy określić wyrażenie, które jest niejawnie konwertowany na typ właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="c29a4-1057">Ponadto punkt końcowy `get` akcesor nie może być dostępny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="c29a4-1058">A `set` akcesor odnosi się do metody z parametrem typu właściwości pojedynczej wartości i `void` typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="c29a4-1059">Niejawny parametr `set` zawsze nosi nazwę metody dostępu `value`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="c29a4-1060">Gdy właściwość jest określany jako elementem docelowym przypisania ([operatory przypisania](expressions.md#assignment-operators)), lub jako argument operacji `++` lub `--` ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators), [ Przedrostkowe i operatory dekrementacji](expressions.md#prefix-increment-and-decrement-operators)), `set` akcesor zostanie wywołana z nieprawidłowym argumentem (której wartość jest to, że po prawej stronie przypisania lub operand `++` lub `--` operator), udostępnia nową wartość ([przypisanie proste](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="c29a4-1061">Treść `set` metody dostępu muszą być zgodne z regułami dotyczącymi `void` metod opisanych w [treści metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="c29a4-1062">W szczególności `return` instrukcji w `set` treści metody dostępu nie można określić wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="c29a4-1063">Ponieważ `set` akcesor niejawnie określono parametr o nazwie `value`, jest to błąd czasu kompilacji lokalnej deklaracji zmiennej czy stałej w `set` nazwy metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="c29a4-1064">Na podstawie obecności lub braku `get` i `set` metod dostępu, a właściwość jest klasyfikowany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="c29a4-1065">Właściwość, która obejmuje zarówno `get` metody dostępu i `set` metody dostępu jest określany jako ***odczytu i zapisu*** właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="c29a4-1066">Właściwość, która ma tylko `get` metody dostępu jest określany jako ***tylko do odczytu*** właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="c29a4-1067">Jest to błąd czasu kompilacji, dla właściwości tylko do odczytu, aby być elementem docelowym przypisania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="c29a4-1068">Właściwość, która ma tylko `set` metody dostępu jest określany jako ***tylko do zapisu*** właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="c29a4-1069">Z wyjątkiem jako element docelowy przypisania, jest błąd kompilacji, aby odwoływać się do właściwości tylko do zapisu w wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="c29a4-1070">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1070">In the example</span></span>
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
<span data-ttu-id="c29a4-1071">`Button` kontroli deklaruje publiczny `Caption` właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="c29a4-1072">`get` Akcesor `Caption` właściwość zwraca ciąg, przechowywane w prywatnych `caption` pola.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="c29a4-1073">`set` Akcesor sprawdza, czy nowa wartość jest inna niż bieżąca wartość, a jeśli tak, są przechowywane nową wartość i odświeża formantu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="c29a4-1074">Właściwości często oparte na wzorcu powyżej: `get` Dostępu po prostu zwraca wartość przechowywaną w pola prywatnego, a `set` akcesor modyfikuje tego pola prywatnego, a następnie wykonuje żadnych dodatkowych akcji, wymagane do pełnej aktualizacji stanu obiektu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="c29a4-1075">Biorąc pod uwagę `Button` klasy powyżej, Oto przykład użycia `Caption` właściwości:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="c29a4-1076">W tym miejscu `set` metody dostępu jest wywoływany przez przypisywanie wartości do właściwości, a `get` metody dostępu jest wywoływany przez odwołanie do właściwości w wyrażeniu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="c29a4-1077">`get` i `set` metod dostępu właściwości nie są różne elementy członkowskie i nie jest możliwe do deklarowania metod dostępu właściwości oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="c29a4-1078">W efekcie nie jest możliwe dla dwóch metod dostępu właściwości odczytu / zapisu zapewnienie różnych ułatwień dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="c29a4-1079">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-1079">The example</span></span>
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
<span data-ttu-id="c29a4-1080">nie deklaruje pojedynczej właściwości odczytu / zapisu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="c29a4-1081">Przeciwnie, deklaruje dwie właściwości o takiej samej nazwie, tylko do odczytu, a drugi jako tylko do zapisu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="c29a4-1082">Ponieważ dwóch elementów członkowskich zadeklarowanych w tej samej klasy nie może mieć takiej samej nazwie, przykład powoduje, że występuje błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="c29a4-1083">Gdy klasa pochodna deklaruje właściwości przez taką samą nazwę jak właściwość dziedziczona, właściwość dziedziczona ukrywa właściwość dziedziczona względem Odczyt i zapis.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="c29a4-1084">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1084">In the example</span></span>
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
<span data-ttu-id="c29a4-1085">`P` właściwość `B` ukrywa `P` właściwość `A` względem Odczyt i zapis.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="c29a4-1086">W efekcie w instrukcjach</span><span class="sxs-lookup"><span data-stu-id="c29a4-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="c29a4-1087">Przypisanie do `b.P` powoduje błąd kompilacji, należy podać, ponieważ tylko do odczytu `P` właściwość `B` ukrywa tylko do zapisu `P` właściwość `A`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="c29a4-1088">Należy jednak pamiętać, że rzutowania może służyć do dostępu do ukrytych `P` właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="c29a4-1089">W przeciwieństwie do pola publiczne właściwości zapewnia oddzielenie stan wewnętrzny obiekt i jego interfejsu publicznego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="c29a4-1090">Rozważmy na przykład:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1090">Consider the example:</span></span>
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

<span data-ttu-id="c29a4-1091">W tym miejscu `Label` klasa korzysta z dwóch `int` pól `x` i `y`, do jego lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="c29a4-1092">Lokalizacja jest publicznie widoczne jako `X` i `Y` właściwości i `Location` właściwości typu `Point`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="c29a4-1093">Jeśli w przyszłych wersjach programu `Label`, staje się bardziej wygodne do przechowywania lokalizacji jako `Point` wewnętrznie, można wprowadzić zmiany bez wywierania wpływu na interfejs publiczny klasy:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
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

<span data-ttu-id="c29a4-1094">Gdyby `x` i `y` zamiast tego zostało `public readonly` pól, byłoby niemożliwe wprowadzić zmiany do `Label` klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="c29a4-1095">Udostępnianie stanu przy użyciu właściwości nie jest zawsze wszelkie mniej wydajne niż udostępnianie pól bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="c29a4-1096">W szczególności gdy właściwość jest niewirtualną i zawiera tylko niewielką ilość kodu, środowisko wykonawcze może zastąpić wywołania do metod dostępu, jak rzeczywisty kod metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="c29a4-1097">Ten proces jest nazywany ***inlining***, i sprawia, że dostęp do właściwości wydajne niż dostęp do pola, ale zachowuje zwiększona elastyczność właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="c29a4-1098">Ponieważ wywoływanie `get` metody dostępu jest równoważny odczytywania wartości pola, jest uznawane za nieprawidłowe stylu programowania dla `get` metod dostępu ma dostrzegalnych skutki uboczne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="c29a4-1099">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="c29a4-1100">Wartość `Next` właściwości zależy od liczby właściwości wcześniej były używane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="c29a4-1101">Dlatego uzyskiwanie dostępu do właściwości tworzy zauważalny efekt i właściwości powinny być zrealizowane jako metody zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="c29a4-1102">Konwencja "efektów ubocznych" dla `get` akcesory nie oznaczają, że `get` metod dostępu powinny być zawsze zapisywane po prostu zwrócenia wartości przechowywane w polach.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="c29a4-1103">W rzeczywistości `get` metod dostępu często obliczenia wartości właściwości, uzyskiwanie dostępu do wielu pól lub wywoływania metod.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="c29a4-1104">Jednak prawidłowo zaprojektowana `get` akcesor nie wykonuje żadnych akcji, które powodują zauważalne zmiany w stan obiektu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="c29a4-1105">Właściwości można opóźnić zainicjowanie zasobu, aż do chwili, który odwołuje się najpierw do.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="c29a4-1106">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1106">For example:</span></span>
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

<span data-ttu-id="c29a4-1107">`Console` Klasa zawiera trzy właściwości `In`, `Out`, i `Error`, reprezentujące odpowiednio standardowe dane wejściowe, dane wyjściowe i urządzenia z błędami.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="c29a4-1108">Dzięki uwidocznieniu działania tych elementów członkowskich jako właściwości, `Console` klasy można opóźnić ich inicjowania, dopóki nie są rzeczywiście używane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="c29a4-1109">Na przykład, po pierwszym odwołujące się do `Out` właściwości, jak</span><span class="sxs-lookup"><span data-stu-id="c29a4-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="c29a4-1110">podstawowe `TextWriter` dla tworzenia urządzenia w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="c29a4-1111">Ale jeśli aplikacja nie ma żadnego odwołania do `In` i `Error` właściwości, a następnie żadne obiekty nie są tworzone dla tych urządzeń.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="c29a4-1112">Automatycznie zaimplementowane właściwości</span><span class="sxs-lookup"><span data-stu-id="c29a4-1112">Automatically implemented properties</span></span>

<span data-ttu-id="c29a4-1113">Automatycznie implementowanej właściwości (lub ***właściwości automatycznej*** w skrócie), jest właściwością nieabstrakcyjnej niż zewnętrzny z organami dostępu tylko do średnikami.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="c29a4-1114">Właściwości automatyczne musi mieć metodę dostępu get i opcjonalnie może mieć dostępu set.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="c29a4-1115">Jeśli właściwość jest określony jako automatycznie implementowanej właściwości, polem zapasowym ukryte są automatycznie dostępne dla właściwości i metod dostępu są wdrożone w celu odczytu i zapisu do tego pola pomocniczego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="c29a4-1116">Jeśli właściwość automatyczną nie metody dostępu set, uznaje się do pola pomocniczego `readonly` ([pola tylko do odczytu](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="c29a4-1117">Podobnie jak w przypadku `readonly` pole, tylko metod pobierających auto właściwością również można przypisać do treści konstruktora otaczającej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="c29a4-1118">Takie przypisania przypisuje bezpośrednio do pola pomocniczego tylko do odczytu właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="c29a4-1119">Auto właściwością opcjonalnie może mieć *property_initializer*, których są stosowane bezpośrednio do pola pomocniczego jako *variable_initializer* ([zmiennej inicjatory](classes.md#variable-initializers)) .</span><span class="sxs-lookup"><span data-stu-id="c29a4-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="c29a4-1120">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="c29a4-1121">jest równoważny z następującą deklarację:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="c29a4-1122">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="c29a4-1123">jest równoważny z następującą deklarację:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1123">is equivalent to the following declaration:</span></span>
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

<span data-ttu-id="c29a4-1124">Zwróć uwagę, przypisania do pola tylko do odczytu czy prawne, ponieważ występują one w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="c29a4-1125">Ułatwienia dostępu</span><span class="sxs-lookup"><span data-stu-id="c29a4-1125">Accessibility</span></span>

<span data-ttu-id="c29a4-1126">Jeśli ma metody dostępu *accessor_modifier*, domena dostępności ([domeny dostępności](basic-concepts.md#accessibility-domains)) metody dostępu jest określany za pomocą deklarowana dostępność metody *accessor_modifier* .</span><span class="sxs-lookup"><span data-stu-id="c29a4-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="c29a4-1127">Jeśli nie ma metody dostępu *accessor_modifier*, domena dostępności metody dostępu jest określana na podstawie deklarowana dostępność metody, właściwości lub indeksatora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="c29a4-1128">Obecność *accessor_modifier* nigdy nie wpływa na wyszukanie członka ([operatory](expressions.md#operators)) lub Rozpoznanie przeciążenia ([Rozpoznanie przeciążenia](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="c29a4-1129">Modyfikatory na właściwość lub indeksator zawsze określić, które właściwości lub indeksatora jest powiązana, niezależnie od tego, w kontekście dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="c29a4-1130">Po dokonaniu wyboru określoną właściwość lub indeksator, domeny dostępności określonej metody dostępu związane są używane do określenia, czy to użycie jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="c29a4-1131">Jeśli użycie wynosi jako wartość ([wartości wyrażeń](expressions.md#values-of-expressions)), `get` dostępu musi istnieć i być dostępne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="c29a4-1132">W przypadku użycia jako obiekt docelowy przypisanie proste ([przypisanie proste](expressions.md#simple-assignment)), `set` dostępu musi istnieć i być dostępne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="c29a4-1133">Jeśli użycie wynosi jako obiekt docelowy przypisania złożonego ([przydział złożony](expressions.md#compound-assignment)), lub jako obiekt docelowy `++` lub `--` operatorów ([funkcji elementów członkowskich](expressions.md#function-members).9, [ Wyrażenia wywołania](expressions.md#invocation-expressions)), zarówno `get` metody dostępu i `set` dostępu musi istnieć i być dostępne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="c29a4-1134">W poniższym przykładzie właściwość `A.Text` jest ukryty przez właściwość `B.Text`, nawet w sytuacjach, w przypadku, gdy tylko `set` nosi nazwę metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="c29a4-1135">Z drugiej strony, właściwość `B.Count` nie jest dostępna dla klasy `M`, więc właściwości dostępnych `A.Count` zamian jest używana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

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

<span data-ttu-id="c29a4-1136">Metody dostępu, który jest używany do implementowania interfejsu nie może mieć *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="c29a4-1137">Jeśli tylko jedną metodę dostępu jest używana do zaimplementowania interfejsu, inne metody dostępu może być zadeklarowana z *accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="c29a4-1138">Virtual, zapieczętowany, przesłonięć i metod dostępu do właściwości abstrakcyjnej</span><span class="sxs-lookup"><span data-stu-id="c29a4-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="c29a4-1139">A `virtual` deklaracja właściwości określa, że metody dostępu właściwości wirtualnego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="c29a4-1140">`virtual` Modyfikator ma zastosowanie do obu metod dostępu właściwości odczytu / zapisu — nie jest możliwe tylko jednej metody dostępu właściwości odczytu / zapisu być wirtualne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="c29a4-1141">`abstract` Deklaracja właściwości określa wirtualne metody dostępu właściwości, ale nie zapewnia rzeczywistego implementację metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="c29a4-1142">Zamiast tego pochodnej klasy nieabstrakcyjnej wymaganych zapewniali własną implementację dla metod dostępu przez zastąpienie właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="c29a4-1143">Ponieważ metody dostępu dla deklaracji właściwość abstrakcyjną zapewnia nie rzeczywistego wykonania jego *accessor_body* po prostu składa się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="c29a4-1144">Deklaracja właściwości, która obejmuje zarówno `abstract` i `override` Modyfikatory Określa, czy właściwość jest abstrakcyjna i przesłania właściwość podstawowy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="c29a4-1145">Metod dostępu do tych właściwości są również abstrakcyjne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="c29a4-1146">Deklaracje właściwości abstrakcyjne są dozwolone tylko w klasy abstrakcyjne ([klasy abstrakcyjne](classes.md#abstract-classes)). Metod dostępu właściwości wirtualnego odziedziczonej można zastąpić w klasie pochodnej przy tym deklaracja właściwości, która określa `override` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="c29a4-1147">Jest to nazywane ***zastępowanie deklaracja właściwości***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="c29a4-1148">Przesłanianie deklaracja właściwości nie deklaruje nową właściwość.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="c29a4-1149">Zamiast tego należy ją po prostu specjalizuje się implementacje metod dostępu do istniejącej właściwości wirtualnego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="c29a4-1150">Przesłanianie deklaracja właściwości, należy określić dokładnie tych samych modyfikatory dostępności, typ i nazwę jako właściwość dziedziczona.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="c29a4-1151">Jeśli jest dziedziczone właściwość tylko jednej metody dostępu (tj. Jeśli właściwość dziedziczona, jest tylko do odczytu lub tylko do zapisu), nadrzędnych właściwości musi zawierać tylko tej metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="c29a4-1152">Jeśli właściwość dziedziczona obejmuje obu metod dostępu (czyli jeśli właściwość dziedziczona odczytu / zapisu), nadrzędnych właściwości mogą obejmować dostępu jednego lub obu metod dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="c29a4-1153">Przesłanianie deklaracja właściwości może obejmować `sealed` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="c29a4-1154">Użyj ten modyfikator uniemożliwia dalsze zastępowanie właściwości klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="c29a4-1155">Metody dostępu właściwości zapieczętowane również są zamknięte.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="c29a4-1156">Z wyjątkiem różnic w deklaracji i wywołanie składni, zastąpienie wirtualnego, sealed i metody abstrakcyjne dostępu zachowują się tak samo jak wirtualny, sealed, przesłonięć i metody abstrakcyjne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="c29a4-1157">W szczególności opisano zasady w [metod wirtualnych](classes.md#virtual-methods), [Przesłaniaj metody](classes.md#override-methods), [zapieczętowany metody](classes.md#sealed-methods), i [metody abstrakcyjne](classes.md#abstract-methods) zastosować tak, jakby metod dostępu zostały metody odpowiedniego formularza:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="c29a4-1158">A `get` akcesor odnosi się do metody bez parametrów, zwracając wartość, typ właściwości i tym samym Modyfikatory jako zawierającą je właściwość.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="c29a4-1159">A `set` akcesor odnosi się do metody z parametrem typu właściwości pojedynczej wartości `void` zwracać typ i ten sam Modyfikatory jako zawierającą je właściwość.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="c29a4-1160">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1160">In the example</span></span>
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
<span data-ttu-id="c29a4-1161">`X` jest wirtualny właściwość tylko do odczytu, `Y` jest właściwością odczytu i zapisu wirtualnego i `Z` jest abstrakcyjny właściwości odczytu / zapisu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="c29a4-1162">Ponieważ `Z` jest abstrakcyjny, klasa zawierająca `A` musi również być zadeklarowany jako abstrakcyjny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="c29a4-1163">Klasa, która pochodzi od klasy `A` jest wyświetlane poniżej:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1163">A class that derives from `A` is show below:</span></span>
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

<span data-ttu-id="c29a4-1164">Tutaj, deklaracje `X`, `Y`, i `Z` są zastępowanie deklaracje właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="c29a4-1165">Każdy deklaracja właściwości dokładnie odpowiada modyfikatory dostępności, typ i nazwę odpowiedniej właściwości dziedziczone.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="c29a4-1166">`get` Akcesor `X` i `set` akcesor `Y` użyj `base` — słowo kluczowe do dostępu do dziedziczonej metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="c29a4-1167">Deklaracja `Z` zastępuje obu abstrakcyjne metod dostępu — w ten sposób, Brak członków oczekujących abstrakcyjna funkcja w `B`, i `B` może być klasą nieabstrakcyjną.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="c29a4-1168">Jeśli właściwość jest zadeklarowany jako `override`, wszelkich przesłoniętych metod dostępu muszą być dostępne dla kodu nadrzędnych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="c29a4-1169">Ponadto deklarowana dostępność metody, właściwości lub indeksatora, sama i metod dostępu, musi być zgodna z zgodnym z przesłoniętą składową i metod dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="c29a4-1170">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1170">For example:</span></span>
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

## <a name="events"></a><span data-ttu-id="c29a4-1171">Zdarzenia</span><span class="sxs-lookup"><span data-stu-id="c29a4-1171">Events</span></span>

<span data-ttu-id="c29a4-1172">***Zdarzeń*** jest elementem członkowskim, umożliwiająca obiektem lub klasą do udostępniania powiadomień.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="c29a4-1173">Klientów można dołączyć kod wykonywalny dla zdarzeń, podając ***procedury obsługi zdarzeń***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="c29a4-1174">Zdarzenia są deklarowane przy użyciu *event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1174">Events are declared using *event_declaration*s:</span></span>

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

<span data-ttu-id="c29a4-1175">*Event_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)) i prawidłową kombinację modyfikatory dostępu cztery ([modyfikatorach dostępu ](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `static` ([metody statyczne i wystąpienia](classes.md#static-and-instance-methods)), `virtual` ([metody wirtualnej](classes.md#virtual-methods)), `override` ([Przesłaniaj metody](classes.md#override-methods)), `sealed` ([zapieczętowany metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)), a `extern` ([Metody zewnętrznej](classes.md#external-methods)) modyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="c29a4-1176">Deklaracje zdarzeń jest zależna od te same reguły jako deklaracje metody ([metody](classes.md#methods)) w odniesieniu do ważnych kombinacji modyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="c29a4-1177">*Typu* zdarzenia deklaracja musi być *delegate_type* ([typy odwołań](types.md#reference-types)) oraz że *delegate_type* musi wynosić co najmniej jako dostępne jako samego zdarzenia ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c29a4-1178">Może zawierać deklaracji zdarzenia *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="c29a4-1179">Jednakże, jeśli nie, dla innych extern nieabstrakcyjnej zdarzeń, kompilator dostarcza je automatycznie ([zdarzenia podobne do pól](classes.md#field-like-events)); w przypadku zdarzeń extern metody dostępu są udostępniane zewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="c29a4-1180">Deklaracja zdarzenia, które pomija *event_accessor_declarations* definiuje jedno lub więcej zdarzeń — jednej dla każdego z *variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="c29a4-1181">Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych za takie *event_declaration*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="c29a4-1182">Jest to błąd czasu kompilacji dla *event_declaration* oba `abstract` modyfikator i rozdzielany nawias klamrowy *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="c29a4-1183">Gdy deklaracji zdarzenia zawiera `extern` modyfikator, zdarzenie jest nazywany ***zewnętrznego zdarzenia***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="c29a4-1184">Ponieważ deklaracja zewnętrznego zdarzenia zapewnia nie rzeczywiste wdrożenie, występuje błąd dla niego oba `extern` modyfikator i *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="c29a4-1185">Jest to błąd czasu kompilacji dla *variable_declarator* deklaracji zdarzeń za pomocą `abstract` lub `external` modyfikator obejmujący *variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="c29a4-1186">Zdarzenia mogą być używane jako lewostronny operand `+=` i `-=` operatorów ([przypisanie zdarzenia](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="c29a4-1187">Te operatory są używane, odpowiednio, można dołączyć programów obsługi zdarzeń do lub usuwanie programów obsługi zdarzeń zdarzeń i modyfikatory dostępu zdarzenia kontrola kontekstów, w których operacje takie są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="c29a4-1188">Ponieważ `+=` i `-=` są tylko operacje, które są dozwolone w zdarzenie poza typ, który deklaruje zdarzenie, kod zewnętrzny można dodać i usunąć programy obsługi zdarzeń, ale nie w jakikolwiek inny sposób uzyskać lub zmodyfikować bazowego listę zdarzeń procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="c29a4-1189">W przypadku operacji formularza `x += y` lub `x -= y`, gdy `x` jest zdarzeniem i odwołania, ma miejsce poza typ, który zawiera deklarację `x`, wynik operacji ma typ `void` (w przeciwieństwie do konieczności Typ `x`, z wartością `x` po przypisaniu).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="c29a4-1190">Ta zasada uniemożliwia kod zewnętrzny pośrednio badanie podstawowego delegata zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="c29a4-1191">W poniższym przykładzie pokazano, jak procedury obsługi zdarzeń są dołączone do wystąpień `Button` klasy:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
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

<span data-ttu-id="c29a4-1192">W tym miejscu `LoginDialog` konstruktora wystąpienia tworzy dwa `Button` wystąpień i dołącza programów obsługi zdarzeń do `Click` zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="c29a4-1193">Zdarzenia podobne do pól</span><span class="sxs-lookup"><span data-stu-id="c29a4-1193">Field-like events</span></span>

<span data-ttu-id="c29a4-1194">Tekstu program klasy lub struktury, który zawiera deklarację zdarzenia niektóre zdarzenia mogą być używane jak pola.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="c29a4-1195">Ma być używana w ten sposób, nie może być zdarzenie `abstract` lub `extern`i nie może zawierać jawne *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="c29a4-1196">Takie zdarzenie może służyć w dowolnym kontekście, który zezwala na pola.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="c29a4-1197">Pole zawiera delegata ([delegatów](delegates.md)) które odwołuje się do listy programów obsługi zdarzeń, które zostały dodane do zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="c29a4-1198">Jeśli zostały dodane nie procedury obsługi zdarzeń, to pole zawiera `null`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="c29a4-1199">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1199">In the example</span></span>
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
<span data-ttu-id="c29a4-1200">`Click` Służy jako pole w ramach `Button` klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="c29a4-1201">Jak w przykładzie pokazano, pole można zbadać, modyfikacji i używać w wyrażeniach wywołanie delegata.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="c29a4-1202">`OnClick` Method in Class metoda `Button` klasy "wywołuje" `Click` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="c29a4-1203">Pojęcie podnoszenie zdarzenia odpowiada dokładnie wywoływania delegata reprezentowanej przez zdarzenie — tak więc nie istnieją żadne konstrukcje specjalny język przeznaczony dla podnoszonego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="c29a4-1204">Należy pamiętać, że wywołanie delegata jest poprzedzony wyboru, które zapewnia, że delegat jest różna od null.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="c29a4-1205">Poza deklaracją elementu `Button` klasy `Click` element członkowski należy używać tylko w lewej części `+=` i `-=` operatorów, jak</span><span class="sxs-lookup"><span data-stu-id="c29a4-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="c29a4-1206">która dołącza do listy wywołanie delegata `Click` zdarzenia i</span><span class="sxs-lookup"><span data-stu-id="c29a4-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="c29a4-1207">co spowoduje usunięcie delegata z listy wywołania `Click` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="c29a4-1208">Podczas kompilowania kodu zdarzenia podobne do pól, kompilator automatycznie tworzy magazynu do przechowywania delegata oraz metody dostępu dla zdarzenia, które dodają lub usuń programy obsługi zdarzeń do pola delegata.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="c29a4-1209">Operacje dodawania i usuwania są bezpieczne wątkowo i może (ale nie jest wymagane) się podczas pracy z blokadą ([instrukcję lock](statements.md#the-lock-statement)) zawierającego go obiektu zdarzenia wystąpienia lub obiekt typu ([anonimowe Tworzenie wyrażenia obiektów](expressions.md#anonymous-object-creation-expressions)) dla zdarzeń statycznych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="c29a4-1210">W efekcie deklarację wystąpienia zdarzenia w postaci:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="c29a4-1211">będzie kompilowana do czegoś równoważne:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1211">will be compiled to something equivalent to:</span></span>
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
<span data-ttu-id="c29a4-1212">W ramach klasy `X`, odwołuje się do `Ev` w lewej części `+=` i `-=` operatory powodują dodawanie i usuwanie metod dostępu do wywołania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="c29a4-1213">Wszystkie odwołania do `Ev` są kompilowane do odwoływać się do pola ukrytego `__Ev` zamiast ([dostęp do elementu członkowskiego](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="c29a4-1214">Nazwa "`__Ev`" dowolnej; ukryte pole może mieć dowolną nazwę lub bez nazwy w ogóle.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="c29a4-1215">Metod dostępu zdarzeń</span><span class="sxs-lookup"><span data-stu-id="c29a4-1215">Event accessors</span></span>

<span data-ttu-id="c29a4-1216">Deklaracje zdarzeń zwykle pominąć *event_accessor_declarations*, jak w `Button` w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="c29a4-1217">Jedna sytuacja może więc obejmuje przypadek, w której koszt magazynowania jednego pola przypadającego na zdarzenia nie są akceptowane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="c29a4-1218">W takich przypadkach mogą zawierać klasę *event_accessor_declarations* i używany był mechanizm prywatnych do przechowywania listy programów obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="c29a4-1219">*Event_accessor_declarations* zdarzenie Podaj instrukcje wykonywalne skojarzonych z Dodawanie i usuwanie programów obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="c29a4-1220">Deklaracje metody dostępu składają się z *add_accessor_declaration* i *remove_accessor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="c29a4-1221">Każda deklaracja dostępu składa się z tokenem `add` lub `remove` następuje *bloku*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="c29a4-1222">*Bloku* skojarzone z *add_accessor_declaration* Określa instrukcje Aby wykonać po dodaniu programu obsługi zdarzeń i *bloku* skojarzone z *remove_accessor_declaration* Określa instrukcje wykonywania, gdy program obsługi zdarzeń jest usuwana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="c29a4-1223">Każdy *add_accessor_declaration* i *remove_accessor_declaration* odnosi się do metody przy użyciu pojedynczej wartości parametru typu zdarzenia i `void` typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="c29a4-1224">Niejawny parametr metody dostępu zdarzeń ma nazwę `value`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="c29a4-1225">Gdy zdarzenie jest używany w przypisaniu zdarzenia, metody dostępu do odpowiednich zdarzeń jest używany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="c29a4-1226">W szczególności jeśli operator przypisania jest `+=` użyta metody dostępu add i operator przypisania jest `-=` , a następnie usunięcie akcesora jest używany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="c29a4-1227">W obu przypadkach prawostronny operand operatora przypisania służy jako argument do metody dostępu zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="c29a4-1228">Blok *add_accessor_declaration* lub *remove_accessor_declaration* musi być zgodna z regułami dotyczącymi `void` metod opisanych w [treści metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="c29a4-1229">W szczególności `return` instrukcje w takich bloku nie można określić wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="c29a4-1230">Ponieważ metoda dostępu do zdarzeń niejawnie ma parametr o nazwie `value`, jest to błąd czasu kompilacji dla lokalnej zmiennej czy stałej zadeklarowanych w metody dostępu zdarzeń do nazwy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="c29a4-1231">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1231">In the example</span></span>
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
<span data-ttu-id="c29a4-1232">`Control` klasa implementuje mechanizm wewnętrzny magazyn zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="c29a4-1233">`AddEventHandler` Metoda kojarzy wartość delegata z kluczem, `GetEventHandler` metoda zwraca delegata obecnie skojarzony z kluczem, a `RemoveEventHandler` metoda usuwa obiekt delegowany jako program obsługi zdarzeń dla określonego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="c29a4-1234">Prawdopodobnie, podstawowego mechanizmu przechowywania zaprojektowano w taki sposób, że nie ma żadnych kosztów kojarzenia `null` delegować wartość za pomocą klucza i związku z tym nieobsługiwany zdarzenia używanie Brak magazynu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="c29a4-1235">Zdarzenia statyczne i wystąpienia</span><span class="sxs-lookup"><span data-stu-id="c29a4-1235">Static and instance events</span></span>

<span data-ttu-id="c29a4-1236">Jeśli deklaracja zdarzenia zawiera `static` modyfikator, zdarzenie jest nazywany ***zdarzeń statycznych***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="c29a4-1237">Gdy nie `static` modyfikator, zdarzenie jest nazywany ***wystąpienie zdarzenia***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="c29a4-1238">Zdarzeń statycznych nie jest skojarzony z określonym wystąpieniem i istnieje błąd w czasie kompilacji do odwoływania się do `this` w metod dostępu zdarzeń statycznych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="c29a4-1239">Wystąpienie zdarzenia jest skojarzona z danego wystąpienia klasy, a to wystąpienie może być dostępna jako `this` ([dostęp](expressions.md#this-access)) w metodach dostępu tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="c29a4-1240">Gdy zdarzenie jest przywoływany w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `E.M`, jeśli `M` jest zdarzeniem statyczne `E` musi oznaczają typu zawierającego `M`i jeśli `M` to zdarzenie wystąpienia E musi oznaczają wystąpienia typu zawierającego `M`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="c29a4-1241">Różnice statyczne elementy członkowskie wystąpień zostały one omówione w dalszych [statyczna i wystąpienia elementów członkowskich](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="c29a4-1242">Virtual, zapieczętowany, przesłonięć i metody dostępu zdarzenia abstrakcyjnego</span><span class="sxs-lookup"><span data-stu-id="c29a4-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="c29a4-1243">A `virtual` deklarację zdarzenia, która określa, że metod dostępu do tego zdarzenia jest wirtualny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="c29a4-1244">`virtual` Modyfikator ma zastosowanie do obu metod dostępu zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="c29a4-1245">`abstract` Deklaracja zdarzenia określa wirtualne metody dostępu zdarzenia, ale nie zapewnia rzeczywistego implementację metody dostępu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="c29a4-1246">Zamiast tego pochodnej klasy nieabstrakcyjnej wymaganych zapewniali własną implementację dla metod dostępu przez zastąpienie zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="c29a4-1247">Ponieważ deklaracja zdarzenia abstrakcyjnego zapewnia nie rzeczywistego wykonania, nie może dostarczyć rozdzielonych nawias klamrowy *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="c29a4-1248">Deklaracja zdarzenia, która obejmuje zarówno `abstract` i `override` Modyfikatory Określa, że zdarzenie jest abstrakcyjna i zastępuje podstawowego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="c29a4-1249">Metody dostępu takiego zdarzenia są także abstrakcyjne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="c29a4-1250">Deklaracje abstrakcyjne zdarzeń są dozwolone tylko w klasy abstrakcyjne ([klasy abstrakcyjne](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="c29a4-1251">Metod dostępu dziedziczone wirtualne wydarzenie może zostać przesłonięta w klasie pochodnej przez łącznie z deklaracji zdarzeń, który określa `override` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="c29a4-1252">Jest to nazywane ***zastępowanie deklaracja zdarzenia***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="c29a4-1253">Przesłanianie deklaracja zdarzenia nie deklaruje nowe zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="c29a4-1254">Zamiast tego należy ją po prostu specjalizuje się implementacje metod dostępu do zdarzenia istniejącego wirtualnego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="c29a4-1255">Przesłanianie deklaracja zdarzenia należy określić dokładnie tych samych modyfikatory dostępności, typ i nazwę jako przesłoniętej zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="c29a4-1256">Przesłanianie deklaracja zdarzenia mogą obejmować `sealed` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="c29a4-1257">Użyj ten modyfikator uniemożliwia dalsze zastępowanie zdarzenia klasę pochodną.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="c29a4-1258">Metod dostępu zdarzeń zapieczętowanego również są zamknięte.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="c29a4-1259">Jest to błąd czasu kompilacji nadrzędnych deklaracji zdarzeń uwzględnić `new` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="c29a4-1260">Z wyjątkiem różnic w deklaracji i wywołanie składni, zastąpienie wirtualnego, sealed i metody abstrakcyjne dostępu zachowują się tak samo jak wirtualny, sealed, przesłonięć i metody abstrakcyjne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="c29a4-1261">W szczególności opisano zasady w [metod wirtualnych](classes.md#virtual-methods), [Przesłaniaj metody](classes.md#override-methods), [zapieczętowany metody](classes.md#sealed-methods), i [metody abstrakcyjne](classes.md#abstract-methods) zastosować tak, jakby metod dostępu zostały metody odpowiedniej postaci.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="c29a4-1262">Każdej metody dostępu odnosi się do metody przy użyciu pojedynczej wartości parametru typu zdarzenia `void` zwracać typ i ten sam Modyfikatory jako zdarzenia zawierającego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="c29a4-1263">Indeksatory</span><span class="sxs-lookup"><span data-stu-id="c29a4-1263">Indexers</span></span>

<span data-ttu-id="c29a4-1264">***Indeksatora*** jest element członkowski, który umożliwia to obiektowi zostać pomyślnie zindeksowane w taki sam sposób jak tablica.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="c29a4-1265">Indeksatory są deklarowane przy użyciu *indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1265">Indexers are declared using *indexer_declaration*s:</span></span>

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

<span data-ttu-id="c29a4-1266">*Indexer_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)) i prawidłową kombinację modyfikatory dostępu cztery ([modyfikatorach dostępu ](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `virtual` ([metody wirtualnej](classes.md#virtual-methods)), `override` ([Przesłaniaj metody](classes.md#override-methods) ), `sealed` ([Zapieczętowany metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)), i `extern` ([metody zewnętrznej](classes.md#external-methods)) modyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="c29a4-1267">Deklaracje indeksatora podlegają te same reguły jako deklaracje metody ([metody](classes.md#methods)) w odniesieniu do ważnych kombinacji modyfikatorów, z jednym wyjątkiem jest, że modyfikator statyczny jest niedozwolony w deklaracji indeksatora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="c29a4-1268">Modyfikatorów `virtual`, `override`, i `abstract` wykluczają się wzajemnie z wyjątkiem w przypadku jednego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="c29a4-1269">`abstract` i `override` Modyfikatory mogą być używane razem, tak aby indeksator abstrakcyjna może zastąpić jeden wirtualny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="c29a4-1270">*Typu* indeksatora deklaracja określa typ elementu indeksatora wprowadzonych przez tę deklarację.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="c29a4-1271">Chyba że indeksatora jest jawną implementacją członków, *typu* następuje słowa kluczowego `this`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="c29a4-1272">Dla implementacja interfejsu jawnego członka *typu* następuje *interface_type*, "`.`", a słowa kluczowego `this`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="c29a4-1273">W przeciwieństwie do innych członków indeksatory nie ma nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="c29a4-1274">*Formal_parameter_list* określa parametry indeksatora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="c29a4-1275">Lista parametrów formalnych indeksatora odnosi się do metody ([parametry metody](classes.md#method-parameters)), z tą różnicą, że co najmniej jeden parametr musi być określony i że `ref` i `out` Modyfikatory parametrów nie są dozwolone. .</span><span class="sxs-lookup"><span data-stu-id="c29a4-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="c29a4-1276">*Typu* indeksatora i każdy z typów, do którego odwołuje się *formal_parameter_list* musi być co najmniej tak samo dostępna jak indeksatora, sama ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c29a4-1277">*Indexer_body* może albo składają się z ***treści metody dostępu*** lub ***treści wyrażenia***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="c29a4-1278">W treści metody dostępu *accessor_declarations*, musi być ujęta w "`{`"i"`}`" tokenów, Zadeklaruj metody dostępu ([Akcesory](classes.md#accessors)) właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="c29a4-1279">Metody dostępu Określ instrukcji wykonywalnych powiązane z odczytywaniem i zapisywania właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="c29a4-1280">Treść wyrażenia składające się z "`=>`" występować wyrażenie `E` , a dokładnie odpowiada na treść instrukcji średnikiem `{ get { return E; } }`i w związku z tym należy używać tylko do określenia tylko metod pobierających indeksatory gdzie jest wynikiem metody pobierającej podane przez jedno wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="c29a4-1281">Mimo że składnia służąca do uzyskiwania dostępu do elementu indeksatora jest taka sama jak dla elementu tablicy, element indeksator nie jest sklasyfikowany jako zmienną.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="c29a4-1282">W związku z tym, nie jest możliwe do przekazania element indeksatora `ref` lub `out` argumentu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="c29a4-1283">Lista parametrów formalnych indeksatora definiuje podpis ([podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading)) indeksatora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="c29a4-1284">W szczególności podpis indeksatora składa się z liczbę i typy parametrów formalnych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="c29a4-1285">Typ elementu oraz nazwy parametrów formalnych nie są częścią podpisu indeksatora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="c29a4-1286">Podpis indeksatora musi się różnić od podpisy innymi indeksatorami zadeklarowane w tej samej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="c29a4-1287">Właściwości i indeksatorów są bardzo podobne, ale różnią się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="c29a4-1288">Właściwość jest identyfikowany przez jego nazwę, natomiast indeksatora jest identyfikowane za pomocą jego podpisu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="c29a4-1289">Właściwość jest dostępny za pośrednictwem *simple_name* ([proste nazwy](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)), podczas gdy indeksatora element jest dostępny za pośrednictwem *element_access* ([dostęp indeksatora](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="c29a4-1290">Właściwość może być `static` elementu członkowskiego, indeksator zawsze jest składową wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="c29a4-1291">A `get` metody dostępu właściwości odnosi się do metody bez parametrów, natomiast `get` akcesor indeksatora odnosi się do metody o tej samej listy parametrów formalnych jako indeksatora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="c29a4-1292">A `set` metody dostępu właściwości odnosi się do metody z pojedynczym parametrem o nazwie `value`, podczas gdy `set` akcesor indeksatora odnosi się do metody o tej samej listy parametrów formalnych jako indeksatora plus dodatkowy parametr o nazwie `value`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="c29a4-1293">Jest to błąd czasu kompilacji dla metody dostępu indeksatora zadeklarować zmienną lokalną o takiej samej nazwie jako parametru indeksatora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="c29a4-1294">W deklaracji właściwości nadrzędnych właściwość dziedziczona odbywa się przy użyciu składni `base.P`, gdzie `P` jest nazwą właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="c29a4-1295">W deklaracji nadrzędnych indeksatora dziedziczone indeksatora odbywa się przy użyciu składni `base[E]`, gdzie `E` jest rozdzielana przecinkami lista wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="c29a4-1296">Nie obowiązuje koncepcja "indeksatora automatycznie implementowanej".</span><span class="sxs-lookup"><span data-stu-id="c29a4-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="c29a4-1297">Jest błędem nieabstrakcyjnej, inne niż zewnętrzne indeksatora przy użyciu metod dostępu średnikami.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="c29a4-1298">Oprócz tych różnic wszystkie reguły zdefiniowane w [Akcesory](classes.md#accessors) i [automatycznie implementowane właściwości](classes.md#automatically-implemented-properties) dotyczy indeksatora metod dostępu również do metod dostępu do właściwości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="c29a4-1299">Jeśli deklaracja indeksator zawiera `extern` modyfikator, indeksatora jest nazywany ***zewnętrznych indeksatora***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="c29a4-1300">Ponieważ deklaracja zewnętrznych indeksatora zapewnia nie rzeczywistą implementację każdego z jego *accessor_declarations* składa się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="c29a4-1301">W poniższym przykładzie deklarujemy `BitArray` klasę, która implementuje indeksator do uzyskiwania dostępu do poszczególnych bitów w tablicy bitowych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
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

<span data-ttu-id="c29a4-1302">Wystąpienie `BitArray` klasy zużywa znacznie mniej pamięci niż odpowiadające `bool[]` (ponieważ każda wartość tego pierwszego zajmuje tylko jednego bitu zamiast jego 's jednobajtowego), ale pozwala na tych samych operacji co `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="c29a4-1303">Następujące `CountPrimes` klasy używa `BitArray` i klasycznego algorytmu "wagi" Aby obliczyć liczbę blokad między 1 a maksymalną danego:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
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

<span data-ttu-id="c29a4-1304">Należy pamiętać, że składnia służąca do uzyskiwania dostępu do elementów `BitArray` jest dokładnie taka sama, jak w przypadku `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="c29a4-1305">Poniższy przykład przedstawia klasę siatki 26 \* 10, która ma indeksatora z dwoma parametrami.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="c29a4-1306">Pierwszy parametr musi być wielkich lub małą literę z zakresu A-Z, a drugi musi być liczbą całkowitą z zakresu od 0 do 9.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

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

### <a name="indexer-overloading"></a><span data-ttu-id="c29a4-1307">Przeciążanie indeksatora</span><span class="sxs-lookup"><span data-stu-id="c29a4-1307">Indexer overloading</span></span>

<span data-ttu-id="c29a4-1308">Zasad rozpoznawania przeciążenia indeksatora są opisane w [wnioskowanie o typie](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="c29a4-1309">Operatory</span><span class="sxs-lookup"><span data-stu-id="c29a4-1309">Operators</span></span>

<span data-ttu-id="c29a4-1310">***Operator*** jest element członkowski, który definiuje znaczenie operator wyrażenia, które mogą być stosowane do wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="c29a4-1311">Operatory są deklarowane przy użyciu *operator_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1311">Operators are declared using *operator_declaration*s:</span></span>

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

<span data-ttu-id="c29a4-1312">Istnieją trzy kategorie operatory z możliwością przeciążenia: Operatory jednoargumentowe ([operatorów Jednoargumentowych](classes.md#unary-operators)), operatory binarne ([operatory dwuargumentowe](classes.md#binary-operators)) i operatorów konwersji ([operatory konwersji](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="c29a4-1313">*Operator_body* jest albo średnik ***treść instrukcji*** lub ***treści wyrażenia***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="c29a4-1314">Treść instrukcji składa się z *bloku*, która określa instrukcje wykonywania, gdy operator jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="c29a4-1315">*Bloku* musi być zgodna z regułami dotyczącymi zwracanie wartości metod opisanych w [treści metody](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="c29a4-1316">Treść wyrażenia składa się z `=>` następuje wyrażenia i średnika i oznacza pojedynczego wyrażenia do wykonania, gdy operator jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="c29a4-1317">Aby uzyskać `extern` operatorów, *operator_body* po prostu składa się z średnikiem.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="c29a4-1318">Dla wszystkich innych operatorów *operator_body* treści bloku lub treści wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="c29a4-1319">Następujące reguły dotyczą wszystkie deklaracje operatora:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="c29a4-1320">Deklaracja operatora musi zawierać zarówno `public` i `static` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="c29a4-1321">Parametry operatora musi mieć wartość parametrów ([wartości parametrów](variables.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="c29a4-1322">Jest to błąd czasu kompilacji dla Deklaracja operatora określić `ref` lub `out` parametrów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="c29a4-1323">Podpis operatora ([operatorów Jednoargumentowych](classes.md#unary-operators), [operatory dwuargumentowe](classes.md#binary-operators), [operatory konwersji](classes.md#conversion-operators)) musi się różnić od podpisy innych operatorów zadeklarowane w tej samej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="c29a4-1324">Wszystkie typy, do którego odwołuje się Deklaracja operatora musi być co najmniej tak samo dostępna jak operator, sama ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="c29a4-1325">Jest błąd dla tego samego modyfikator pojawią się wiele razy w Deklaracja operatora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="c29a4-1326">Każda kategoria operatora nakłada dodatkowe ograniczenia, zgodnie z opisem w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="c29a4-1327">Podobnie jak inne elementy członkowskie operatory zadeklarowana w klasie bazowej są dziedziczone przez klasy pochodne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="c29a4-1328">Ponieważ deklaracje operatora zawsze należy wymagać klasy lub struktury, w której operator jest zadeklarowany do wzięcia udziału w podpisie operatora, nie jest możliwe operatora zadeklarowana w klasie pochodnej spowoduje ukrycie operatora zadeklarowana w klasie bazowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="c29a4-1329">W efekcie `new` modyfikator jest nigdy nie wymagane i w związku z tym nigdy nie dozwolone, w deklaracji operatora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="c29a4-1330">Dodatkowe informacje na temat operatory jednoargumentowe i dane binarne znajdują się w [operatory](expressions.md#operators).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="c29a4-1331">Dodatkowe informacje na temat operatory konwersji można znaleźć w [zdefiniowane przez użytkownika konwersje](conversions.md#user-defined-conversions).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="c29a4-1332">Operatory jednoargumentowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-1332">Unary operators</span></span>

<span data-ttu-id="c29a4-1333">Obowiązują następujące reguły deklaracje operatora jednoargumentowego, gdy `T` wskazuje typ wystąpienia klasy lub struktury, który zawiera deklarację operatora:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="c29a4-1334">Jednoargumentowy `+`, `-`, `!`, lub `~` operatora musi mieć jeden parametr typu `T` lub `T?` i może zwrócić dowolnego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="c29a4-1335">Jednoargumentowy `++` lub `--` operatora musi mieć jeden parametr typu `T` lub `T?` i musi zwrócić się, że tego samego typu lub pochodzić od niego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="c29a4-1336">Jednoargumentowy `true` lub `false` operatora musi mieć jeden parametr typu `T` lub `T?` i musi zwracać typ `bool`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="c29a4-1337">Podpis operatora jednoargumentowego składa się z token operatora (`+`, `-`, `!`, `~`, `++`, `--`, `true`, lub `false`) i typ pojedynczy parametr formalny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="c29a4-1338">Zwracany typ nie jest częścią podpisu operatora jednoargumentowego ani nie jest nazwą parametru formalnego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="c29a4-1339">`true` i `false` operatory jednoargumentowe wymagają pair-wise deklaracji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="c29a4-1340">Błąd kompilacji występuje, jeśli klasa deklaruje jeden z tych operatorów bez także zadeklarowanie drugiego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="c29a4-1341">`true` i `false` operatory są dokładniejszym opisem zawartym w [zdefiniowanych przez użytkownika operatorów logicznych warunkowych](expressions.md#user-defined-conditional-logical-operators) i [wyrażeń logicznych](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="c29a4-1342">W poniższym przykładzie pokazano implementację i kolejne użycie `operator ++` klasy wektor liczbą całkowitą:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
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

<span data-ttu-id="c29a4-1343">Należy zauważyć, jak metoda operator zwraca wartość produkowane przez dodanie 1 do argumentu operacji, podobnie jak przyrostka inkrementacji i dekrementacji operatorów ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators)) oraz prefiksów inkrementacji i dekrementacji operatory ([przedrostkowe i operatory dekrementacji](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="c29a4-1344">W przeciwieństwie do języka C++, ta metoda musi Modyfikuje wartości swojego operandu bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="c29a4-1345">W rzeczywistości zmieniając wartość operandu naruszyłoby semantyce przyrostkowego operatora inkrementacyjnego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="c29a4-1346">Operatory binarne</span><span class="sxs-lookup"><span data-stu-id="c29a4-1346">Binary operators</span></span>

<span data-ttu-id="c29a4-1347">Obowiązują następujące reguły deklaracje operatora binarnego, gdzie `T` wskazuje typ wystąpienia klasy lub struktury, który zawiera deklarację operatora:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="c29a4-1348">Operator binarny-shift, należy wykonać dwa parametry, z których co najmniej jeden z nich musi mieć typ `T` lub `T?`i może zwrócić dowolnego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="c29a4-1349">Plik binarny `<<` lub `>>` operator przyjmuje dwa parametry, z których pierwszy musi mieć typ `T` lub `T?` i drugi z nich musi mieć typ `int` lub `int?`i może zwrócić dowolnego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="c29a4-1350">Podpis operatora binarnego składa się z token operatora (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, lub `<=`) i typy dwóch parametrów formalnych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="c29a4-1351">Zwracany typ i nazwy parametrów formalnych nie są częścią podpisu operatora binarnego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="c29a4-1352">Niektóre operatory binarne wymagają pair-wise deklaracji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="c29a4-1353">Dla każdej deklaracji albo operator parę musi być zgodna Deklaracja operatora pary.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="c29a4-1354">Dwie deklaracje operatora dopasowania, gdy mają taki sam zwracany typ i ten sam typ, dla każdego parametru.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="c29a4-1355">Następujące operatory wymagają pair-wise deklaracji:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="c29a4-1356">`operator ==` i `operator !=`</span><span class="sxs-lookup"><span data-stu-id="c29a4-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="c29a4-1357">`operator >` i `operator <`</span><span class="sxs-lookup"><span data-stu-id="c29a4-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="c29a4-1358">`operator >=` i `operator <=`</span><span class="sxs-lookup"><span data-stu-id="c29a4-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="c29a4-1359">Operatory konwersji</span><span class="sxs-lookup"><span data-stu-id="c29a4-1359">Conversion operators</span></span>

<span data-ttu-id="c29a4-1360">Wprowadza Deklaracja operatora konwersji ***konwersja zdefiniowana przez użytkownika*** ([zdefiniowane przez użytkownika konwersje](conversions.md#user-defined-conversions)) który wspomaga wstępnie zdefiniowanych Konwersje jawne i niejawne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="c29a4-1361">Deklaracja operatora konwersji, która obejmuje `implicit` — słowo kluczowe wprowadza niejawnej konwersji zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="c29a4-1362">Niejawne konwersje mogą występować w wielu różnych sytuacjach, takich jak funkcja elementu członkowskiego wywołania, wyrażenia cast i przypisania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="c29a4-1363">Jest to dokładniejszym opisem zawartym w [niejawne konwersje](conversions.md#implicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="c29a4-1364">Deklaracja operatora konwersji, która obejmuje `explicit` — słowo kluczowe wprowadza zdefiniowanych przez użytkownika konwersji jawnej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="c29a4-1365">Konwersje jawne mogą występować w wyrażenia cast i są dokładniejszym opisem zawartym w [jawne konwersje](conversions.md#explicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="c29a4-1366">Konwertuje operatora konwersji z typu źródłowego, wskazywanym przez typ parametru operatora konwersji na typ docelowy, wskazane przez zwracany typ operatora konwersji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="c29a4-1367">Dla typu danego źródła `S` i docelowy typ `T`, jeśli `S` lub `T` są typy dopuszczające wartości zerowe umożliwiają `S0` i `T0` odnoszą się do ich typów podstawowych, w przeciwnym razie `S0` i `T0` są równa `S` i `T` odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="c29a4-1368">Klasy lub struktury może zadeklarować konwersji z typu źródłowego `S` z typem docelowym `T` tylko wtedy, gdy spełnione są wszystkie z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="c29a4-1369">`S0` i `T0` są różnych typów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="c29a4-1370">Albo `S0` lub `T0` jest typem klasy lub struktury, w którym odbywa się Deklaracja operatora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="c29a4-1371">Ani `S0` ani `T0` jest *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="c29a4-1372">Z wyjątkiem konwersje zdefiniowane przez użytkownika konwersja nie istnieje z `S` do `T` lub `T` do `S`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="c29a4-1373">Dla celów tych reguł, dowolny typ parametrów skojarzonych z `S` lub `T` są traktowane jako unikatowe typy, które mają nie relacji dziedziczenia z innych typów i wszelkie ograniczenia typu, te parametry są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="c29a4-1374">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="c29a4-1375">pierwsze deklaracje operatora dwa są dozwolone, ponieważ na potrzeby [indeksatory](classes.md#indexers).3, `T` i `int` i `string` odpowiednio są traktowane jako unikatowe typy z nie relacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="c29a4-1376">Jednak Trzeci operator jest błąd, ponieważ `C<T>` jest klasa bazowa `D<T>`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="c29a4-1377">Z drugą regułę, że wynika, że operator konwersji należy przekonwertować do lub z typu klasy lub struktury, w której operator jest zadeklarowany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="c29a4-1378">Na przykład możliwe dla typu klasy lub struktury jest `C` do definiowania konwersja `C` do `int` i `int` do `C`, ale nie z `int` do `bool`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="c29a4-1379">Nie jest możliwe do przedefiniowania bezpośrednio wstępnie zdefiniowanych konwersji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="c29a4-1380">W związku z tym, operatory konwersji nie są dozwolone do konwertowania z lub `object` ponieważ Konwersje jawne i niejawne już istnieją między `object` i innych typów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="c29a4-1381">Podobnie źródło ani typy docelowy konwersji nie może być typ podstawowy, z drugiej strony, ponieważ konwersja następnie będzie już istnieje.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="c29a4-1382">Jednak jest możliwe do deklarowania operatorów w typach ogólnych określającymi, argumenty określonego typu, konwersje, które już istnieją wstępnie zdefiniowane konwersji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="c29a4-1383">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="c29a4-1384">Podczas wpisywania `object` jest określony jako typ argumentu dla `T`, drugi operator deklaruje konwersji, która już istnieje (niejawny i w związku z tym również jawnie, istnieje konwersja z każdym do typu `object`).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="c29a4-1385">W przypadkach, gdy istnieje wstępnie zdefiniowanych konwersji między dwoma typami zdefiniowane przez użytkownika konwersje między typami te są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="c29a4-1386">W szczególności:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1386">Specifically:</span></span>

*  <span data-ttu-id="c29a4-1387">Jeśli wstępnie zdefiniowane niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z typu `S` na typ `T`, wszystkie konwersje zdefiniowane przez użytkownika (niejawny lub jawny) z `S` do `T` są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="c29a4-1388">Jeśli wstępnie zdefiniowane jawna konwersja ([jawne konwersje](conversions.md#explicit-conversions)) istnieje z typu `S` na typ `T`, zdefiniowane przez użytkownika jawnych konwersji z `S` do `T` są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="c29a4-1389">Ponadto:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1389">Furthermore:</span></span>

<span data-ttu-id="c29a4-1390">Jeśli `T` jest typem interfejsu użytkownika niejawne konwersje z elementu `S` do `T` są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="c29a4-1391">W przeciwnym razie wartość zdefiniowana przez użytkownika niejawne konwersje z elementu `S` do `T` są nadal uważane za.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="c29a4-1392">Dla wszystkich typów, ale `object`, operatory zadeklarowana przez `Convertible<T>` powyżej typ nie wchodzą w konflikt z wstępnie zdefiniowanych konwersji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="c29a4-1393">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="c29a4-1394">Jednak w przypadku typu `object`, wstępnie zdefiniowanych konwersje Ukryj konwersje zdefiniowane przez użytkownika w wszystkich przypadków, ale jedna:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="c29a4-1395">Konwersje zdefiniowane przez użytkownika nie są dozwolone do konwersji z lub do *interface_type*s.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="c29a4-1396">W szczególności, to ograniczenie gwarantuje, że nie transformacji zdefiniowanych przez użytkownika wystąpić podczas konwertowania na *interface_type*oraz że konwersję *interface_type* powiedzie się tylko wtedy, gdy obiekt faktycznie przekształcany implementuje określony *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="c29a4-1397">Podpis operatora konwersji składa się z typu źródłowego i typ docelowy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="c29a4-1398">(Zwróć uwagę, że jest to tylko formularz, dla którego zwracany typ uczestniczy w podpisie elementu członkowskiego). `implicit` Lub `explicit` klasyfikacji operatora konwersji nie jest częścią podpisu operatora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="c29a4-1399">W związku z tym, klasie lub strukturze nie można zadeklarować zarówno `implicit` i `explicit` operatora konwersji z tymi samymi typami źródłowym i docelowym.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="c29a4-1400">Ogólnie rzecz biorąc zdefiniowane przez użytkownika konwersje niejawne powinny zostać tak zaprojektowane, aby nigdy nie zgłaszają wyjątków i nigdy nie utracą informacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="c29a4-1401">Jeśli konwersja zdefiniowana przez użytkownika może powodować powstanie wyjątków (na przykład, ponieważ źródłowy jest poza zakresem) lub utraty informacji (takich jak odrzucanie bardziej znaczących bitów), a następnie tej konwersji, powinien być zdefiniowany jako jawnej konwersji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="c29a4-1402">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1402">In the example</span></span>
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
<span data-ttu-id="c29a4-1403">Konwersja z `Digit` do `byte` jest niejawny, ponieważ nigdy nie zgłasza wyjątki lub traci informacji, ale konwersja z `byte` do `Digit` jest jawne od `Digit` może być reprezentowana przez podzbiór możliwe wartości `byte`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="c29a4-1404">Konstruktory wystąpień</span><span class="sxs-lookup"><span data-stu-id="c29a4-1404">Instance constructors</span></span>

<span data-ttu-id="c29a4-1405">***Konstruktora wystąpienia*** jest elementem członkowskim implementujący działania wymagane w celu zainicjowania wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="c29a4-1406">Konstruktory wystąpień są deklarowane przy użyciu *constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

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

<span data-ttu-id="c29a4-1407">A *constructor_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)), prawidłowej kombinacji modyfikatory dostępu cztery ([modyfikatorach dostępu ](classes.md#access-modifiers)), a `extern` ([metody zewnętrznej](classes.md#external-methods)) modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="c29a4-1408">Deklaracja konstruktora jest niedozwolona w zawierają ten sam modyfikator wiele razy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="c29a4-1409">*Identyfikator* z *constructor_declarator* należy nadać nazwę klasy, w którym zadeklarowany jest konstruktora wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="c29a4-1410">Jeśli jakakolwiek inna nazwa jest określona, występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="c29a4-1411">Opcjonalny *formal_parameter_list* wystąpienia Konstruktor podlega te same reguły jako *formal_parameter_list* metody ([metody](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="c29a4-1412">Na liście parametrów formalnych definiuje podpis ([podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading)) konstruktora wystąpienia i kontroluje proces, według której rozpoznanie przeciążenia ([wnioskowanie o typie](expressions.md#type-inference)) wybiera określonego Konstruktor wystąpienia w wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="c29a4-1413">Każdy z typów, do którego odwołuje się *formal_parameter_list* wystąpienie konstruktora musi być co najmniej tak samo dostępna jak sam Konstruktor ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="c29a4-1414">Opcjonalny *constructor_initializer* określa innego konstruktora wystąpienia do wywołania przed wykonaniem instrukcji podanych w *constructor_body* tego konstruktora wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="c29a4-1415">Jest to dokładniejszym opisem zawartym w [inicjatory konstruktora](classes.md#constructor-initializers).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="c29a4-1416">Jeśli deklaracja konstruktora zawiera `extern` modyfikator, Konstruktor jest określany jako ***zewnętrznych Konstruktor***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="c29a4-1417">Ponieważ deklaracja konstruktora zewnętrznych zapewnia nie rzeczywistego wykonania jego *constructor_body* składa się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="c29a4-1418">Dla innych konstruktorów *constructor_body* składa się z *bloku* określający instrukcje inicjuje nowe wystąpienie klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="c29a4-1419">Dokładnie odpowiada *bloku* z metody wystąpienia mającej `void` zwracany typ ([treści metody](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="c29a4-1420">Konstruktory wystąpień nie są dziedziczone.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="c29a4-1421">W związku z tym klasa nie ma konstruktorów wystąpienia niż rzeczywiście zadeklarowana w klasie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="c29a4-1422">Jeśli klasa nie zawiera żadnych deklaracji konstruktora wystąpienia, domyślnego konstruktora wystąpienia jest dostarczana automatycznie ([domyślne konstruktory](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="c29a4-1423">Konstruktory wystąpień są wywoływane przez *object_creation_expression*s ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)) i *constructor_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="c29a4-1424">Inicjatory konstruktora</span><span class="sxs-lookup"><span data-stu-id="c29a4-1424">Constructor initializers</span></span>

<span data-ttu-id="c29a4-1425">Wszystkie wystąpienia konstruktory (z wyjątkiem tych, dla klasy `object`) niejawnie obejmują bezpośrednio przed wywołaniem innego konstruktora wystąpienia *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="c29a4-1426">Konstruktor do niejawnie wywołania jest określana przez *constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="c29a4-1427">Inicjatora konstruktora wystąpienia formularza `base(argument_list)` lub `base()` powoduje, że konstruktora wystąpień z bezpośredniej klasy bazowej do wywołania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="c29a4-1428">Ten konstruktor jest zaznaczone, przy użyciu *argument_list* Jeśli obecny i zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="c29a4-1429">Zestaw konstruktory wystąpień Release candidate składa się z wszystkie konstruktory dostępne wystąpienia zawarte w bezpośredniej klasie bazowej lub domyślnego konstruktora ([domyślne konstruktory](classes.md#default-constructors)), jeśli nie konstruktory wystąpień są deklarowane w Bezpośrednia klasa bazowa.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="c29a4-1430">Jeśli ten zestaw jest pusty lub nie można zidentyfikować pojedynczy najlepsze konstruktora wystąpienia, występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="c29a4-1431">Inicjatora konstruktora wystąpienia formularza `this(argument-list)` lub `this()` powoduje, że konstruktora wystąpień z klasy, do wywołania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="c29a4-1432">Konstruktor jest zaznaczone, przy użyciu *argument_list* Jeśli obecny i zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="c29a4-1433">Zestaw konstruktory wystąpień Release candidate składa się z wszystkie konstruktory dostępne wystąpienia zadeklarowanych w samej klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="c29a4-1434">Jeśli ten zestaw jest pusty lub nie można zidentyfikować pojedynczy najlepsze konstruktora wystąpienia, występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="c29a4-1435">Jeśli deklaracja konstruktora wystąpienia zawiera inicjatora konstruktora, który wywołuje sam Konstruktor, występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="c29a4-1436">Jeśli konstruktora wystąpienia nie ma konstruktora inicjatora, inicjatora konstruktora formularza `base()` niejawnie są dostarczane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="c29a4-1437">W związku z tym Konstruktor deklarację wystąpienia formularza</span><span class="sxs-lookup"><span data-stu-id="c29a4-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="c29a4-1438">odpowiada dokładnie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="c29a4-1439">Zakres parametrów podanych przez *formal_parameter_list* deklaracja konstruktora wystąpień obejmuje inicjatora konstruktora w tej deklaracji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="c29a4-1440">W związku z tym dostęp do parametrów konstruktora może inicjatora konstruktora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="c29a4-1441">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1441">For example:</span></span>
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

<span data-ttu-id="c29a4-1442">Inicjatora konstruktora wystąpienia nie może uzyskać dostępu jest utworzone wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="c29a4-1443">Dlatego jest to błąd czasu kompilacji, aby odwołać się do `this` w wyrażeniu argumentu inicjatora konstruktora, co jest błąd kompilacji, wyrażenie argumentu dowolny członek wystąpienia przez odwołanie do *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="c29a4-1444">Inicjatory zmiennej wystąpienia</span><span class="sxs-lookup"><span data-stu-id="c29a4-1444">Instance variable initializers</span></span>

<span data-ttu-id="c29a4-1445">Podczas konstruktora wystąpienia nie ma konstruktora inicjatora lub ma on postać inicjatora konstruktora `base(...)`, ten konstruktor wykonuje niejawnie inicjowania określonej przez *variable_initializer*s pola wystąpienia są zadeklarowane w swojej klasie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="c29a4-1446">Odpowiada to sekwencja przypisania, które są wykonywane od razu po wejściu do konstruktora i przed niejawne wywołanie konstruktora bezpośrednią klasą bazową.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="c29a4-1447">Inicjatory zmiennej są wykonywane w kolejności tekstowych, w jakiej występują w deklaracji klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="c29a4-1448">Wykonywanie konstruktora</span><span class="sxs-lookup"><span data-stu-id="c29a4-1448">Constructor execution</span></span>

<span data-ttu-id="c29a4-1449">Inicjatory zmiennych są przekształcane w instrukcji przypisania, a te instrukcje przypisania są wykonywane przed wywołaniem konstruktora wystąpienia klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="c29a4-1450">Ta kolejność gwarantuje, że wszystkie pola wystąpienia są inicjowane przez ich zmiennej inicjatory, przed wykonaniem dowolnej instrukcji, które mają dostęp do tego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="c29a4-1451">Podane w przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1451">Given the example</span></span>
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
<span data-ttu-id="c29a4-1452">Gdy `new B()` służy do tworzenia instancji `B`, są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```
x = 1, y = 0
```

<span data-ttu-id="c29a4-1453">Wartość `x` wynosi 1, ponieważ inicjator zmiennej jest wykonywany przed wywołaniem konstruktora wystąpienia klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="c29a4-1454">Jednak wartość `y` ma wartość 0 (wartość domyślna `int`) ponieważ przypisanie do `y` nie jest wykonywana do czasu, po powrocie konstruktora klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="c29a4-1455">Warto traktować jako stwierdzenia, które są automatycznie wstawiany przed inicjatorów zmiennej wystąpienia i inicjatory konstruktora *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="c29a4-1456">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-1456">The example</span></span>
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
<span data-ttu-id="c29a4-1457">zawiera kilka zmiennych inicjatory; zawiera ona także inicjatory konstruktora obu Form (`base` i `this`).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="c29a4-1458">Przykład odnosi się do kodu pokazanego poniżej, gdzie każdy komentarz wskazuje automatycznie wstawiono — instrukcja (składnia używana do wywołania automatycznie wstawiono Konstruktor nie jest prawidłowy, ale służy jedynie do ilustrowania mechanizmu).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

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

### <a name="default-constructors"></a><span data-ttu-id="c29a4-1459">Konstruktory domyślne</span><span class="sxs-lookup"><span data-stu-id="c29a4-1459">Default constructors</span></span>

<span data-ttu-id="c29a4-1460">Jeśli klasa nie zawiera żadnych deklaracji konstruktora wystąpienia, domyślnego konstruktora wystąpienia jest dostarczana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="c29a4-1461">Ten konstruktor domyślny po prostu wywołuje konstruktora bez parametrów bezpośrednie klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="c29a4-1462">Jeśli klasa jest abstrakcyjna deklarowana dostępność metody dla domyślnego konstruktora jest chroniona.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="c29a4-1463">W przeciwnym razie deklarowana dostępność metody dla domyślny konstruktor jest publiczny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="c29a4-1464">W związku z tym domyślny konstruktor jest zawsze formularza</span><span class="sxs-lookup"><span data-stu-id="c29a4-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="c29a4-1465">lub</span><span class="sxs-lookup"><span data-stu-id="c29a4-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="c29a4-1466">gdzie `C` jest nazwą klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="c29a4-1467">Jeśli funkcja rozpoznawania przeciążeń nie potrafi określić unikatowy najlepsze kandydatem do inicjatora konstruktora klasy bazowej, a następnie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="c29a4-1468">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="c29a4-1469">domyślny konstruktor znajduje się ponieważ klasy nie zawiera żadnych deklaracjach konstruktorów wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="c29a4-1470">W efekcie przykładzie odpowiada dokładnie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="c29a4-1471">Konstruktory prywatne</span><span class="sxs-lookup"><span data-stu-id="c29a4-1471">Private constructors</span></span>

<span data-ttu-id="c29a4-1472">Gdy klasa `T` deklaruje tylko konstruktory wystąpień prywatnych, nie jest możliwe dla klas spoza i tekstu programu `T` wyprowadzenia z `T` lub bezpośrednio utworzyć wystąpienia elementu `T`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="c29a4-1473">W związku z tym Jeśli klasa zawiera tylko statyczne elementy członkowskie i nie jest przeznaczony do użycia, dodanie pustego wystąpienia prywatnych konstruktora uniemożliwi podczas tworzenia wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="c29a4-1474">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1474">For example:</span></span>
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

<span data-ttu-id="c29a4-1475">`Trig` Klasa grup powiązanych metod i stałe, ale nie jest przeznaczony do użycia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="c29a4-1476">W związku z tym deklaruje Konstruktor pusty prywatnej SIS.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="c29a4-1477">Co najmniej jedno wystąpienie konstruktora musi być zadeklarowany Pomija automatyczne generowanie konstruktora domyślnego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="c29a4-1478">Parametry konstruktora wystąpienia opcjonalne</span><span class="sxs-lookup"><span data-stu-id="c29a4-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="c29a4-1479">`this(...)` Formularz inicjatora konstruktora jest najczęściej używany w połączeniu z przeciążenia do zaimplementowania wystąpienia opcjonalne parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="c29a4-1480">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="c29a4-1480">In the example</span></span>
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
<span data-ttu-id="c29a4-1481">pierwsze konstruktory dwóch wystąpień jedynie podać wartości domyślnej brakujące argumenty.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="c29a4-1482">Obaj użytkownicy za pomocą `this(...)` inicjatora konstruktora, aby wywołać konstruktora wystąpienia trzeci faktycznie działa inicjowania nowego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="c29a4-1483">Efekt jest to, że Konstruktor opcjonalne parametry:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="c29a4-1484">Konstruktory statyczne</span><span class="sxs-lookup"><span data-stu-id="c29a4-1484">Static constructors</span></span>

<span data-ttu-id="c29a4-1485">A ***statyczny Konstruktor*** jest elementem członkowskim implementujący działania wymagane w celu zainicjowania typu klasy zamknięte.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="c29a4-1486">Konstruktory statyczne są deklarowane przy użyciu *static_constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

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

<span data-ttu-id="c29a4-1487">A *static_constructor_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)) i `extern` modyfikator ([metody zewnętrznej](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="c29a4-1488">*Identyfikator* z *static_constructor_declaration* należy nadać nazwę klasy, w którym zadeklarowany jest konstruktor statyczny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="c29a4-1489">Jeśli jakakolwiek inna nazwa jest określona, występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="c29a4-1490">Jeśli deklaracja konstruktora statycznego obejmuje `extern` modyfikator, statyczny Konstruktor jest nazywany ***zewnętrznych Konstruktor statyczny***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="c29a4-1491">Ponieważ deklaracja zewnętrznych konstruktora statycznego zapewnia nie rzeczywistego wykonania jego *static_constructor_body* składa się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="c29a4-1492">Dla wszystkich innych deklaracji statyczny Konstruktor *static_constructor_body* składa się z *bloku* określający instrukcji do wykonania w celu zainicjowania klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="c29a4-1493">Dokładnie odpowiada *method_body* metody statycznej z `void` zwracany typ ([treści metody](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="c29a4-1494">Konstruktory statyczne nie są dziedziczone i nie można wywołać bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="c29a4-1495">Konstruktor statyczny dla typu klasy zamkniętego co najwyżej raz wykonuje w domenie danej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="c29a4-1496">Wykonanie statyczny Konstruktor jest wyzwalany przez pierwszy z następujących zdarzeń w domenie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="c29a4-1497">Tworzone jest wystąpienie typu klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="c29a4-1498">Dowolny ze statycznych składowych typu klasy do których istnieją odwołania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="c29a4-1499">Jeśli klasa zawiera `Main` — metoda ([uruchamiania aplikacji](basic-concepts.md#application-startup)), w którym zaczyna się, statyczny Konstruktor dla tej klasy jest wykonywany przed `Main` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="c29a4-1500">Można zainicjować nowego typu klasy zamknięte, najpierw nowy zestaw pól statycznych ([pola statyczne i wystąpienia](classes.md#static-and-instance-fields)) dla tego konkretnego typu zamknięte jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="c29a4-1501">Każde z pól statycznych jest inicjowany do wartości domyślnej ([wartości domyślne](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="c29a4-1502">Następnie, inicjatory pola statycznego ([pole statyczne inicjowanie](classes.md#static-field-initialization)) są wykonywane dla tych pól statycznych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="c29a4-1503">Na koniec Konstruktor statyczny jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="c29a4-1504">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-1504">The example</span></span>
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
<span data-ttu-id="c29a4-1505">należy wygenerować dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1505">must produce the output:</span></span>
```
Init A
A.F
Init B
B.F
```
<span data-ttu-id="c29a4-1506">ponieważ wykonanie `A`firmy Konstruktor statyczny jest wyzwalany przez wywołanie `A.F`i wykonywania `B`firmy Konstruktor statyczny jest wyzwalany przez wywołanie `B.F`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="c29a4-1507">Istnieje możliwość konstruowania zależności cykliczne, zezwalających na pola statyczne z inicjatorami zmiennej należy przestrzegać w ich stanie. wartość domyślna.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="c29a4-1508">Przykład</span><span class="sxs-lookup"><span data-stu-id="c29a4-1508">The example</span></span>
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
<span data-ttu-id="c29a4-1509">generuje dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="c29a4-1509">produces the output</span></span>
```
X = 1, Y = 2
```

<span data-ttu-id="c29a4-1510">Do wykonania `Main` metody pierwszego uruchomienia systemu inicjator dla `B.Y`wcześniejszej klasy `B`firmy Konstruktor statyczny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="c29a4-1511">`Y`w inicjatorze powoduje, że `A`firmy statyczny Konstruktor można uruchomić, ponieważ wartość `A.X` odwołuje się do.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="c29a4-1512">Statyczny Konstruktor `A` z kolei przechodzi do obliczenia wartości `X`i przeprowadzeniu tak pobiera wartość domyślną `Y`, który wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="c29a4-1513">`A.X` ten sposób jest inicjowany do 1.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="c29a4-1514">Proces uruchamiania `A`inicjatorów pola statyczne i Konstruktor statyczny, a następnie zakończeniu i powrocie do obliczania wartości początkowej `Y`, której wynikiem staje się 2.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="c29a4-1515">Ponieważ Konstruktor statyczny jest wykonywane tylko raz dla każdego zamknięty typ klasy skonstruowany, jest wygodnym miejscem do wymuszania dla parametru typu, który nie można sprawdzić w czasie kompilacji za pomocą ograniczenia sprawdzania w trakcie wykonywania ([parametr typu ograniczenia](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="c29a4-1516">Na przykład następujący typ używa statyczny Konstruktor do wymuszania, że argument typu jest wyliczeniem:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
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

## <a name="destructors"></a><span data-ttu-id="c29a4-1517">Destruktory</span><span class="sxs-lookup"><span data-stu-id="c29a4-1517">Destructors</span></span>

<span data-ttu-id="c29a4-1518">A ***destruktor*** jest elementem członkowskim implementujący działania wymagane do niszczenia wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="c29a4-1519">Destruktor jest zadeklarowany za pomocą *destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1519">A destructor is declared using a *destructor_declaration*:</span></span>

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

<span data-ttu-id="c29a4-1520">A *destructor_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="c29a4-1521">*Identyfikator* z *destructor_declaration* należy nadać nazwę klasy, w którym zadeklarowany jest destruktor.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="c29a4-1522">Jeśli jakakolwiek inna nazwa jest określona, występuje błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="c29a4-1523">Gdy obejmuje deklaracja destruktora `extern` modyfikator, destruktor jest nazywany ***zewnętrznych — destruktor***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="c29a4-1524">Ponieważ deklaracja destruktora zewnętrznych zapewnia nie rzeczywistego wykonania jego *destructor_body* składa się średnikiem.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="c29a4-1525">Dla innych destruktory *destructor_body* składa się z *bloku* określający instrukcji do wykonania w celu niszczenia wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="c29a4-1526">A *destructor_body* odpowiadają dokładnie *method_body* z metody wystąpienia mającej `void` zwracany typ ([treści metody](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="c29a4-1527">Destruktory nie są dziedziczone.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1527">Destructors are not inherited.</span></span> <span data-ttu-id="c29a4-1528">W związku z tym klasa ma żadne destruktory innym niż ten, który może być zdeklarowany przez tę klasę.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="c29a4-1529">Ponieważ destruktora musi mieć żadnych parametrów, nie może być przeciążona, więc klasa może mieć co najwyżej jeden destruktor.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="c29a4-1530">Destruktory są wywoływane automatycznie i nie można jawnie wywołać.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="c29a4-1531">Wystąpienia staje się kwalifikuje się do zniszczenia, gdy nie jest już możliwe dla jakiegokolwiek kodu użyć tego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="c29a4-1532">Wykonanie destruktora dla tego wystąpienia może występować w dowolnym momencie, gdy wystąpienie stanie się kwalifikuje się do zniszczenia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="c29a4-1533">Gdy wystąpienie jest usuwane, destruktory w łańcuch dziedziczenia tego wystąpienia są wywoływane w kolejności od najbardziej pochodnego na co najmniej pochodnych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="c29a4-1534">Destruktora może być wykonywane na żadnym z wątków.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="c29a4-1535">Aby zapoznać się z reguł, które określają, kiedy i jak jest wykonywane destruktora, zobacz [automatyczne zarządzanie pamięcią](basic-concepts.md#automatic-memory-management).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="c29a4-1536">Dane wyjściowe z przykładu</span><span class="sxs-lookup"><span data-stu-id="c29a4-1536">The output of the example</span></span>
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
<span data-ttu-id="c29a4-1537">is</span><span class="sxs-lookup"><span data-stu-id="c29a4-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="c29a4-1538">ponieważ destruktory w łańcuchu dziedziczenia są wywoływane w kolejności od najbardziej pochodnego na co najmniej pochodnych.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="c29a4-1539">Destruktory są implementowane przez zastąpienie metody wirtualnej `Finalize` na `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="c29a4-1540">C# programy nie mogą przesłaniać tę metodę, lub zadzwoń ona (lub zastąpienia jej) bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="c29a4-1541">Na przykład program</span><span class="sxs-lookup"><span data-stu-id="c29a4-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="c29a4-1542">zawiera błędy, dwie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1542">contains two errors.</span></span>

<span data-ttu-id="c29a4-1543">Kompilator zachowuje się tak, jakby tę metodę i zastąpienia, nie istnieją w ogóle.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="c29a4-1544">W związku z tym ten program:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="c29a4-1545">jest prawidłowy, a metoda ukrywa `System.Object`firmy `Finalize` metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="c29a4-1546">Aby uzyskać informacje dotyczące zachowania gdy wyjątek został pominięty przez destruktora, zobacz [sposób obsługi wyjątków](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="c29a4-1547">Iteratory</span><span class="sxs-lookup"><span data-stu-id="c29a4-1547">Iterators</span></span>

<span data-ttu-id="c29a4-1548">Element członkowski funkcji ([funkcji elementów członkowskich](expressions.md#function-members)) implementowane przy użyciu blokiem iteratora ([bloki](statements.md#blocks)) nosi nazwę ***iteratora***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="c29a4-1549">Blokiem iteratora może być używana jako treść funkcji elementu członkowskiego, tak długo, jak zwracany typ odpowiedni element członkowski funkcji jest jednym z interfejsy modułu wyliczającego ([interfejsy modułu wyliczającego](classes.md#enumerator-interfaces)) lub jeden z wyliczalne interfejsy ([Wyliczalne interfejsy](classes.md#enumerable-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="c29a4-1550">Może wystąpić jako *method_body*, *operator_body* lub *accessor_body*, zdarzenia, konstruktory wystąpień, konstruktorów statycznych i destruktory nie może być zaimplementowane jako Iteratory.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="c29a4-1551">Funkcja składowa jest implementowana przy użyciu blokiem iteratora, jest to błąd czasu kompilacji, na liście parametrów formalnych element członkowski funkcji, aby określić dowolne `ref` lub `out` parametrów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="c29a4-1552">Interfejsy modułu wyliczającego</span><span class="sxs-lookup"><span data-stu-id="c29a4-1552">Enumerator interfaces</span></span>

<span data-ttu-id="c29a4-1553">***Interfejsy modułu wyliczającego*** są interfejsu nieogólnego `System.Collections.IEnumerator` i wszystkich wystąpień elementu ogólny interfejs `System.Collections.Generic.IEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="c29a4-1554">W celu skrócenia programu, w tym rozdziale te interfejsy są określone jako `IEnumerator` i `IEnumerator<T>`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="c29a4-1555">Wyliczalne interfejsy</span><span class="sxs-lookup"><span data-stu-id="c29a4-1555">Enumerable interfaces</span></span>

<span data-ttu-id="c29a4-1556">***Wyliczalne interfejsy*** są interfejsu nieogólnego `System.Collections.IEnumerable` i wszystkich wystąpień elementu ogólny interfejs `System.Collections.Generic.IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="c29a4-1557">W celu skrócenia programu, w tym rozdziale te interfejsy są określone jako `IEnumerable` i `IEnumerable<T>`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="c29a4-1558">Typ YIELD</span><span class="sxs-lookup"><span data-stu-id="c29a4-1558">Yield type</span></span>

<span data-ttu-id="c29a4-1559">Iterator wytwarza sekwencję wartości, wszystkie tego samego typu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="c29a4-1560">Tego typu jest nazywana ***uzyskanie typu*** iteratora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="c29a4-1561">Typ iteratora zwracającego yield `IEnumerator` lub `IEnumerable` jest `object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="c29a4-1562">Typ iteratora zwracającego yield `IEnumerator<T>` lub `IEnumerable<T>` jest `T`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="c29a4-1563">Moduł wyliczający obiektów</span><span class="sxs-lookup"><span data-stu-id="c29a4-1563">Enumerator objects</span></span>

<span data-ttu-id="c29a4-1564">Gdy element członkowski funkcji zwraca moduł wyliczający typu interfejsu, jest implementowana przy użyciu blokiem iteratora, wywołanie funkcji elementu członkowskiego nie natychmiast wykonuje kod w bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="c29a4-1565">Zamiast tego ***enumerator — obiekt*** jest tworzony i zwracany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="c29a4-1566">Hermetyzuje kod określony w bloku iteratora i wykonywanie kodu w bloku iteratora występuje, gdy obiekt modułu wyliczającego `MoveNext` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="c29a4-1567">Obiekt modułu wyliczającego ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="c29a4-1568">Implementuje `IEnumerator` i `IEnumerator<T>`, gdzie `T` jest typem yield iteratora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="c29a4-1569">Implementuje `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="c29a4-1570">(Jeśli istnieje) jest inicjowany z kopią wartości argumentu, a wystąpienia wartość przekazana do funkcji elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="c29a4-1571">Ma cztery potencjalnych stanów, ***przed***, ***systemem***, ***zawieszone***, i ***po***i jest początkowo ***przed***  stanu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="c29a4-1572">Obiekt modułu wyliczającego zazwyczaj to wystąpienie klasy generowane przez kompilator modułu wyliczającego, która hermetyzuje kod w bloku iteratora i implementuje interfejsy modułu wyliczającego, ale możliwe są inne metody wdrażania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="c29a4-1573">Jeśli klasy modułu wyliczającego jest generowany przez kompilator, będzie można zagnieżdżać tej klasy, bezpośrednio lub pośrednio, klasy zawierającej funkcję elementu członkowskiego, będzie mieć ułatwień dostępu prywatnego i będzie mieć nazwę zarezerwowany do użytku kompilatora ([identyfikatorów ](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="c29a4-1574">Obiekt modułu wyliczającego mogą implementować interfejsów więcej niż te wymienione powyżej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="c29a4-1575">W poniższych sekcjach opisano dokładne zachowanie `MoveNext`, `Current`, i `Dispose` członkowie `IEnumerable` i `IEnumerable<T>` dostarczane przez obiekt modułu wyliczającego implementacji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="c29a4-1576">Pamiętaj, że moduł wyliczający obiektów nie obsługuje `IEnumerator.Reset` metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="c29a4-1577">Wywołanie tej metody powoduje, że `System.NotSupportedException` zostanie wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="c29a4-1578">Metoda MoveNext</span><span class="sxs-lookup"><span data-stu-id="c29a4-1578">The MoveNext method</span></span>

<span data-ttu-id="c29a4-1579">`MoveNext` Metoda obiekt modułu wyliczającego hermetyzuje kod blokiem iteratora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="c29a4-1580">Wywoływanie `MoveNext` metoda wykonuje kod w bloku iteratora i zestawach `Current` właściwości obiektu modułu wyliczającego, zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="c29a4-1581">Dokładne akcję wykonywaną przez `MoveNext` zależy od stanu obiekt modułu wyliczającego podczas `MoveNext` jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="c29a4-1582">Jeśli stan obiektu modułu wyliczającego jest ***przed***, powinny być przekazywane wywołującemu `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="c29a4-1583">Zmienia stan na ***systemem***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="c29a4-1584">Inicjuje parametry (w tym `this`) bloku iteratora wartości argumentów i wartości wystąpienia zapisywana, gdy obiekt modułu wyliczającego został zainicjowany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="c29a4-1585">Wykonuje blok iterator od samego początku, dopóki nie przerywa wykonywania (opisanych poniżej).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="c29a4-1586">Jeśli stan obiektu modułu wyliczającego jest ***systemem***, wynik wywoływania `MoveNext` jest nieokreślona.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="c29a4-1587">Jeśli stan obiektu modułu wyliczającego jest ***zawieszone***, powinny być przekazywane wywołującemu `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="c29a4-1588">Zmienia stan na ***systemem***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="c29a4-1589">Przywraca wartości wszystkich zmiennych lokalnych i parametrów (w tym to) wartości zapisywana, gdy wykonanie bloku iteratora ostatnio zostało wstrzymane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="c29a4-1590">Należy pamiętać, że zawartość wszystkie obiekty, które odwołują się te zmienne mogą zmieniły się od poprzedniego wywołania MoveNext.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="c29a4-1591">Wznawia działanie natychmiast po bloku iteratora `yield return` instrukcję, która spowodowała zawieszenie wykonywania i jest kontynuowane, dopóki nie przerywa wykonywania (opisanych poniżej).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="c29a4-1592">Jeśli stan obiektu modułu wyliczającego jest ***po***, powinny być przekazywane wywołującemu `MoveNext` zwraca `false`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="c29a4-1593">Gdy `MoveNext` wykonuje bloku iteratora, może zostać przerwane wykonywania na cztery sposoby: Przez `yield return` instrukcji, `yield break` instrukcji przez napotkania koniec bloku iteratora i wyjątek jest generowany i propagowane z bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="c29a4-1594">Gdy `yield return` napotkania instrukcji ([instrukcji yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="c29a4-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="c29a4-1595">Wyrażenie w instrukcji jest obliczane, niejawnie konwertowany na typ produktywności i przypisane do `Current` właściwości obiektu modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="c29a4-1596">Wykonywanie treści iteratora jest zawieszone.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="c29a4-1597">Wartości wszystkich zmiennych lokalnych i parametrów (w tym `this`) zostaną zapisane, ponieważ jest to lokalizacja `yield return` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="c29a4-1598">Jeśli `yield return` Instrukcja znajduje się w co najmniej jeden `try` blokuje skojarzonego `finally` bloki nie są wykonywane w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="c29a4-1599">Stan modułu wyliczającego obiektu jest zmieniany na ***zawieszone***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="c29a4-1600">`MoveNext` Metoda zwraca `true` do obiektu wywołującego, wskazujący, że iteracji pomyślnie zaawansowane do następnej wartości.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="c29a4-1601">Gdy `yield break` napotkania instrukcji ([instrukcji yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="c29a4-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="c29a4-1602">Jeśli `yield break` Instrukcja znajduje się w co najmniej jeden `try` blokuje skojarzonego `finally` bloki są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="c29a4-1603">Stan modułu wyliczającego obiektu jest zmieniany na ***po***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="c29a4-1604">`MoveNext` Metoda zwraca `false` do obiektu wywołującego, wskazujący, że iteracji została zakończona.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="c29a4-1605">Po napotkaniu końcu treści iteratora:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="c29a4-1606">Stan modułu wyliczającego obiektu jest zmieniany na ***po***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="c29a4-1607">`MoveNext` Metoda zwraca `false` do obiektu wywołującego, wskazujący, że iteracji została zakończona.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="c29a4-1608">Gdy wyjątek jest zgłaszany i propagowane z bloku iteratora:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="c29a4-1609">Odpowiednie `finally` bloków w treści iteratora będą wykonywane przez propagacji wyjątku.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="c29a4-1610">Stan modułu wyliczającego obiektu jest zmieniany na ***po***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="c29a4-1611">Propagacja wyjątków w dalszym ciągu obiektu wywołującego `MoveNext` metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="c29a4-1612">Właściwość Current</span><span class="sxs-lookup"><span data-stu-id="c29a4-1612">The Current property</span></span>

<span data-ttu-id="c29a4-1613">Obiekt modułu wyliczającego `Current` właściwości jest zależna od `yield return` instrukcje w bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="c29a4-1614">Gdy obiekt modułu wyliczającego jest w ***zawieszone*** stanu wartości `Current` jest wartość ustawioną przy użyciu poprzedniego wywołania `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="c29a4-1615">Gdy obiekt modułu wyliczającego jest w ***przed***, ***systemem***, lub ***po*** stany wynik uzyskiwania dostępu do `Current` jest nieokreślona.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="c29a4-1616">Dla iteratora instrukcję YIELD typu innego niż `object`, wynikiem uzyskiwania dostępu do `Current` przez obiekt modułu wyliczającego `IEnumerable` implementacji odnosi się do uzyskiwania dostępu do `Current` przez obiekt modułu wyliczającego `IEnumerator<T>` implementacji i wynik rzutowania `object`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="c29a4-1617">Metoda Dispose</span><span class="sxs-lookup"><span data-stu-id="c29a4-1617">The Dispose method</span></span>

<span data-ttu-id="c29a4-1618">`Dispose` Metoda jest używana, aby wyczyścić iteracji, przenosząc obiekt modułu wyliczającego ***po*** stanu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="c29a4-1619">Jeśli stan obiektu modułu wyliczającego jest ***przed***, powinny być przekazywane wywołującemu `Dispose` zmienia stan na ***po***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="c29a4-1620">Jeśli stan obiektu modułu wyliczającego jest ***systemem***, wynik wywoływania `Dispose` jest nieokreślona.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="c29a4-1621">Jeśli stan obiektu modułu wyliczającego jest ***zawieszone***, powinny być przekazywane wywołującemu `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="c29a4-1622">Zmienia stan na ***systemem***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="c29a4-1623">Wykonują wszelkie na koniec bloki tak, jakby ostatnio wykonana `yield return` instrukcji zostały `yield break` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="c29a4-1624">Jeśli powoduje to, że wyjątek zgłoszony i propagowane poza treści iteratora, stan obiektu modułu wyliczającego jest ustawiony na ***po*** i wyjątek jest propagowany do obiektu wywołującego `Dispose` metody.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="c29a4-1625">Zmienia stan na ***po***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="c29a4-1626">Jeśli stan obiektu modułu wyliczającego jest ***po***, powinny być przekazywane wywołującemu `Dispose` nie ma wpływu.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="c29a4-1627">Wyliczalne obiektów</span><span class="sxs-lookup"><span data-stu-id="c29a4-1627">Enumerable objects</span></span>

<span data-ttu-id="c29a4-1628">Gdy element członkowski funkcji zwracająca typ wyliczalny interfejs jest implementowany przy użyciu blokiem iteratora, wywołanie funkcji elementu członkowskiego nie natychmiast wykonuje kod w bloku iteratora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="c29a4-1629">Zamiast tego ***wyliczanym obiektem*** jest tworzony i zwracany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="c29a4-1630">Obiekt wyliczalny `GetEnumerator` metoda zwraca obiekt modułu wyliczającego, który hermetyzuje kod określony w bloku iteratora i wykonywanie kodu w bloku iteratora występuje, gdy obiekt modułu wyliczającego `MoveNext` metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="c29a4-1631">Obiekt wyliczalny ma następującą charakterystykę:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="c29a4-1632">Implementuje `IEnumerable` i `IEnumerable<T>`, gdzie `T` jest typem yield iteratora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="c29a4-1633">(Jeśli istnieje) jest inicjowany z kopią wartości argumentu, a wystąpienia wartość przekazana do funkcji elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="c29a4-1634">Obiekt wyliczalny zazwyczaj to wystąpienie klasy generowane przez kompilator wyliczalny hermetyzuje kod w bloku iteratora, który implementuje wyliczalne interfejsy, ale możliwe są inne metody wdrażania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="c29a4-1635">Jeśli wyliczalny klasy jest generowany przez kompilator, tej klasy będzie można zagnieżdżać, bezpośrednio lub pośrednio w klasie zawierający element członkowski funkcji, będzie mieć ułatwień dostępu prywatnego i będzie mieć nazwę zarezerwowany do użytku kompilatora ([identyfikatorów ](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="c29a4-1636">Obiekt wyliczalny mogą implementować interfejsów więcej niż te wymienione powyżej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="c29a4-1637">W szczególności może również zastosować obiekt wyliczalny `IEnumerator` i `IEnumerator<T>`, dzięki czemu może służyć jako element wyliczalny i moduł wyliczający.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="c29a4-1638">W tym typie wdrożenia, po raz pierwszy obiekt wyliczalny `GetEnumerator` wywołaniu metody z zwracany jest sam obiekt wyliczalny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="c29a4-1639">Kolejne wywołania obiekt wyliczalny `GetEnumerator`, jeśli istnieje, zwraca kopię obiektu wyliczalny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="c29a4-1640">W związku z tym każde zwracane moduł wyliczający ma swój własny stan i zmiany w jeden moduł wyliczający nie wpływają na inny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="c29a4-1641">GetEnumerator — metoda</span><span class="sxs-lookup"><span data-stu-id="c29a4-1641">The GetEnumerator method</span></span>

<span data-ttu-id="c29a4-1642">Obiekt wyliczalny oferuje implementację `GetEnumerator` metody `IEnumerable` i `IEnumerable<T>` interfejsów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="c29a4-1643">Dwa `GetEnumerator` metody udostępniania typową implementację uzyskuje i zwraca obiekt modułu wyliczającego dostępne.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="c29a4-1644">Obiekt modułu wyliczającego jest inicjowany za pomocą wartości argumentów i wystąpienia zapisywana, gdy obiekt wyliczalny został zainicjowany, ale w przeciwnym razie wartość funkcji obiekt modułu wyliczającego, zgodnie z opisem w [obiektów modułu wyliczającego](classes.md#enumerator-objects).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="c29a4-1645">Przykład wdrożenia</span><span class="sxs-lookup"><span data-stu-id="c29a4-1645">Implementation example</span></span>

<span data-ttu-id="c29a4-1646">W tej sekcji opisano możliwą implementację Iteratory pod względem konstrukcje zgodność ze starszymi wersjami języka C#.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="c29a4-1647">Implementacja opisane w tym miejscu opiera się na te same zasady, które są używane przez kompilator Microsoft C#, ale w żadnym wypadku nie jest obowiązkowego implementacji lub możliwe tylko jeden.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="c29a4-1648">Następujące `Stack<T>` klasy implementuje jego `GetEnumerator` metody za pomocą iteratora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="c29a4-1649">Iterator który wylicza elementy stosu w od góry do dołu zamówienia.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

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

<span data-ttu-id="c29a4-1650">`GetEnumerator` Metody mogą być tłumaczone do wystąpienia klasy generowane przez kompilator modułu wyliczającego, która hermetyzuje kod w bloku iteratora, jak pokazano w następującym.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="c29a4-1651">W poprzednim translacji, kod w bloku iteratora jest przekształcane w automacie stanów i umieszczane w `MoveNext` metody klasy modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="c29a4-1652">Ponadto zmienna lokalna `i` jest przekształcane w pola w obiekcie moduł wyliczający, dzięki czemu może on nadal istnieją w wywołań `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="c29a4-1653">Poniższy przykład drukuje prostej tabeli mnożenie liczb całkowitych od 1 do 10.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="c29a4-1654">`FromTo` Metody w przykładzie zwraca obiekt wyliczeniowy i jest implementowane za pomocą iteratora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

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

<span data-ttu-id="c29a4-1655">`FromTo` Metoda może można przetłumaczyć egzemplarzem generowanych przez kompilator wyliczalny klasę, która hermetyzuje kod w bloku iteratora, jak pokazano w następującym.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="c29a4-1656">Wyliczalne klasa implementuje wyliczalne interfejsy i interfejsy modułu wyliczającego, dzięki czemu może służyć jako element wyliczalny i moduł wyliczający.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="c29a4-1657">Po raz pierwszy `GetEnumerator` wywołaniu metody z zwracany jest sam obiekt wyliczalny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="c29a4-1658">Kolejne wywołania obiekt wyliczalny `GetEnumerator`, jeśli istnieje, zwraca kopię obiektu wyliczalny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="c29a4-1659">W związku z tym każde zwracane moduł wyliczający ma swój własny stan i zmiany w jeden moduł wyliczający nie wpływają na inny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="c29a4-1660">`Interlocked.CompareExchange` Metoda służy do zapewnienia działania metodą o bezpiecznych wątkach.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="c29a4-1661">`from` i `to` parametry są przekształcane w pola w klasie wyliczalny.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="c29a4-1662">Ponieważ `from` zostanie zmodyfikowany w bloku iteratora, dodatkowy `__from` pola został wprowadzony do przechowywania wartości początkowej, umożliwiającej `from` w każdy moduł wyliczający.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="c29a4-1663">`MoveNext` Metoda zgłasza wyjątek `InvalidOperationException` Jeśli jest ona wywoływana, gdy `__state` jest `0`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="c29a4-1664">Chroni to przed użycie wyliczanym obiektem jako obiekt modułu wyliczającego bez wywoływania pierwszy `GetEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="c29a4-1665">Poniższy przykład przedstawia klasę proste drzewa.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="c29a4-1666">`Tree<T>` Klasy implementuje jego `GetEnumerator` metody za pomocą iteratora.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="c29a4-1667">Iterator który wylicza elementy drzewa w kolejności wrostkowe.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

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

<span data-ttu-id="c29a4-1668">`GetEnumerator` Metody mogą być tłumaczone do wystąpienia klasy generowane przez kompilator modułu wyliczającego, która hermetyzuje kod w bloku iteratora, jak pokazano w następującym.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="c29a4-1669">Obiekty tymczasowe wygenerowanego przez kompilator, używane w `foreach` instrukcje są podniesione do `__left` i `__right` pola obiekt modułu wyliczającego.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="c29a4-1670">`__state` Pola obiektu modułu wyliczającego dokładnie zostanie zaktualizowany, aby poprawny `Dispose()` metoda zostanie wywołana poprawnie, jeśli wyjątek jest zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="c29a4-1671">Należy pamiętać, że nie jest możliwość napisania przetłumaczony kod za pomocą prostego `foreach` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="c29a4-1672">Funkcje asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="c29a4-1672">Async functions</span></span>

<span data-ttu-id="c29a4-1673">Metody ([metody](classes.md#methods)) lub funkcja anonimowa ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)) za pomocą `async` nosi nazwę modyfikator ***funkcji asynchronicznej***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="c29a4-1674">Ogólnie rzecz biorąc określenie ***async*** służy do opisywania dowolnych funkcji, która ma `async` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="c29a4-1675">Jest to błąd czasu kompilacji, na liście parametrów formalnych funkcji asynchronicznej, aby określić dowolne `ref` lub `out` parametrów.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="c29a4-1676">*Typ_wyniku* asynchroniczna metoda musi być albo `void` lub ***typ zadania***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="c29a4-1677">Typy zadań `System.Threading.Tasks.Task` i typy złożone z `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="c29a4-1678">W celu skrócenia programu, w tym rozdziale te typy są określone jako `Task` i `Task<T>`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="c29a4-1679">Metoda asynchroniczna zwracająca typ zadania jest nazywany zwracającą zadanie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="c29a4-1680">Dokładne definicji typów zadań jest definiowany przez implementację, ale z punktu widzenia języka typ zadania jest w jednym z stanów ukończona, zakończyło się pomyślnie lub wystąpił błąd.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="c29a4-1681">Zadanie uszkodzoną rejestruje odpowiednich wyjątków.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="c29a4-1682">Zakończono pomyślnie `Task<T>` rejestruje wynik o typie `T`.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="c29a4-1683">Typy zadań są oczekujący i dlatego może być argumenty operacji wyrażeń await ([wyrażeń Await](expressions.md#await-expressions)).</span><span class="sxs-lookup"><span data-stu-id="c29a4-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="c29a4-1684">Asynchroniczne wywołanie funkcji ma możliwość wstrzymywania oceny przez wyrażeń await ([wyrażeń Await](expressions.md#await-expressions)) w jej treści.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="c29a4-1685">Ocena może zostać wznowione później w punkcie zawieszenia wyrażenie poprzez await ***delegata wznowienie***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="c29a4-1686">Typ jest delegatem wznowienie `System.Action`, i gdy jest wywoływana, oceny wywołania funkcji asynchronicznej zostanie wznowiona z wyrażenia oczekującego tam, gdzie ją przerwaliśmy.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="c29a4-1687">***Bieżący obiekt wywołujący*** funkcji asynchronicznej wywołanie jest oryginalny obiekt wywołujący, jeśli wywołania funkcji nigdy nie zostało zawieszone lub najnowszych wywołujący delegata wznowienie inaczej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="c29a4-1688">Oceny przez funkcję zwracającą zadanie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="c29a4-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="c29a4-1689">Wywołanie funkcji asynchronicznej zwracania zadania powoduje wystąpienie typu zwracanego zadania do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="c29a4-1690">Jest to nazywane ***Zwróć typ task*** funkcji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="c29a4-1691">Zadanie początkowo jest niekompletna.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="c29a4-1692">Treść funkcji asynchronicznej jest następnie oceniany aż do jego albo została zawieszona (przez osiągnięcia wyrażenie await) lub kończy się, jaką kontroli punktu jest zwracany do obiektu wywołującego, wraz z Zwróć typ task.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="c29a4-1693">Po kończy treść funkcji asynchronicznej, zwracane zadanie zostaje przeniesiony poza stanie niepełnym:</span><span class="sxs-lookup"><span data-stu-id="c29a4-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="c29a4-1694">Jeśli treść funkcji zakończy się w wyniku dotrzeć do instrukcji return lub na końcu treści, każda wartość wyniku jest rejestrowana w zwracanego zadania, które są umieszczane w stanie sukces.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="c29a4-1695">Jeśli w wyniku nieprzechwycony wyjątek kończy się treści funkcji ([instrukcji "throw"](statements.md#the-throw-statement)) wyjątek jest rejestrowana w zwracanego zadania, które są umieszczane w nieprawidłowym stanie.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="c29a4-1696">Obliczanie funkcji asynchronicznej zwraca void</span><span class="sxs-lookup"><span data-stu-id="c29a4-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="c29a4-1697">Jeśli jest zwracany typ funkcji asynchronicznej `void`, oceny, różni się od powyższego w następujący sposób: Ponieważ zadanie nie zostanie zwrócone, funkcja zamiast komunikuje się uzupełniania i wyjątków w bieżącym wątku ***kontekst synchronizacji***.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="c29a4-1698">Dokładne definicji kontekst synchronizacji jest zależna od implementacji, ale jest to reprezentacja elementu "gdzie" bieżący wątek jest uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="c29a4-1699">Kontekst synchronizacji to powiadomienie, gdy oceny funkcji asynchronicznej zwraca void rozpocznie się zakończy się pomyślnie i powoduje, że nieprzechwycony wyjątek zostanie wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="c29a4-1700">Dzięki temu kontekst, aby śledzić liczbę zwracającą typ void async funkcje są uruchomione w nim i zdecydować, jak Propagacja wyjątków pochodzących z nich.</span><span class="sxs-lookup"><span data-stu-id="c29a4-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
