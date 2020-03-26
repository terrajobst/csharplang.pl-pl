---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281960"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="f6bab-101">Rejestruje prace w toku</span><span class="sxs-lookup"><span data-stu-id="f6bab-101">Records Work-in-Progress</span></span>

<span data-ttu-id="f6bab-102">W przeciwieństwie do innych rekordów propozycje nie jest to propozycja, ale prace w toku zaprojektowane do rejestrowania jednomyślnych decyzji projektowych dotyczących funkcji Records.</span><span class="sxs-lookup"><span data-stu-id="f6bab-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="f6bab-103">Szczegóły specyfikacji zostaną dodane w razie potrzeby w celu rozwiązania pytań.</span><span class="sxs-lookup"><span data-stu-id="f6bab-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="f6bab-104">Składnia dla rekordu jest proponowana do dodania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f6bab-104">The syntax for a record is proposed to be added as follows:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

<span data-ttu-id="f6bab-105">`attributes` inne niż Terminal zezwoli również na nowy atrybut kontekstowy, `data`.</span><span class="sxs-lookup"><span data-stu-id="f6bab-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="f6bab-106">Klasa (struktura) zadeklarowana z listą parametrów lub modyfikatorem `data` jest nazywana klasą rekordów (strukturą rekordów), z których każdy jest typem rekordu.</span><span class="sxs-lookup"><span data-stu-id="f6bab-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="f6bab-107">Jest to błąd do deklarowania typu rekordu bez zarówno listy parametrów, jak i modyfikatora `data`.</span><span class="sxs-lookup"><span data-stu-id="f6bab-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="f6bab-108">Elementy członkowskie typu rekordu</span><span class="sxs-lookup"><span data-stu-id="f6bab-108">Members of a record type</span></span>

<span data-ttu-id="f6bab-109">Oprócz elementów członkowskich zadeklarowanych w treści klasy, typ rekordu ma następujące dodatkowe elementy członkowskie:</span><span class="sxs-lookup"><span data-stu-id="f6bab-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="f6bab-110">Konstruktor podstawowy</span><span class="sxs-lookup"><span data-stu-id="f6bab-110">Primary Constructor</span></span>

<span data-ttu-id="f6bab-111">Typ rekordu ma Konstruktor publiczny, którego sygnatura odnosi się do parametrów wartości deklaracji typu.</span><span class="sxs-lookup"><span data-stu-id="f6bab-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="f6bab-112">Jest to nazywane konstruktorem podstawowym dla typu i powoduje pominięcie niejawnie zadeklarowanego konstruktora domyślnego.</span><span class="sxs-lookup"><span data-stu-id="f6bab-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="f6bab-113">Wystąpił błąd w przypadku konstruktora podstawowego i konstruktora o tym samym podpisie w klasie.</span><span class="sxs-lookup"><span data-stu-id="f6bab-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="f6bab-114">W czasie wykonywania podstawowy Konstruktor</span><span class="sxs-lookup"><span data-stu-id="f6bab-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="f6bab-115">wykonuje inicjatory pola wystąpienia pojawiające się w treści klasy; a następnie wywołuje konstruktora klasy bazowej bez argumentów.</span><span class="sxs-lookup"><span data-stu-id="f6bab-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="f6bab-116">Inicjuje pola zapasowe generowane przez kompilator dla właściwości odpowiadających parametrom wartości (jeśli te właściwości są dostarczane przez kompilator; zobacz [właściwości z syntezą](#Synthesized Properties))</span><span class="sxs-lookup"><span data-stu-id="f6bab-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="f6bab-117">[] Do zrobienia: Dodaj składnię podstawowego wywołania i specyfikację dotyczącą wybierania konstruktora podstawowego przy użyciu rozdzielczości przeciążenia</span><span class="sxs-lookup"><span data-stu-id="f6bab-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="f6bab-118">Właściwości</span><span class="sxs-lookup"><span data-stu-id="f6bab-118">Properties</span></span>

<span data-ttu-id="f6bab-119">Dla każdego parametru rekordu deklaracji typu rekordu istnieje odpowiedni publiczny element członkowski właściwości, którego nazwa i typ są pobierane z deklaracji parametru wartości.</span><span class="sxs-lookup"><span data-stu-id="f6bab-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="f6bab-120">Jeśli żadna konkretna Właściwość (tj. non-abstract) z metodą dostępu get i z tą nazwą i typem jest jawnie zadeklarowana lub dziedziczona, jest generowana przez kompilator w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f6bab-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="f6bab-121">Dla struktury rekordu lub klasy rekordu:</span><span class="sxs-lookup"><span data-stu-id="f6bab-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="f6bab-122">Zostanie utworzona publiczna właściwość "Get-Only".</span><span class="sxs-lookup"><span data-stu-id="f6bab-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="f6bab-123">Jej wartość jest inicjowana podczas konstruowania z wartością odpowiadającego parametru podstawowego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="f6bab-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="f6bab-124">Każdy "dopasowany" Dziedziczony Właściwość abstract get jest zastępowany.</span><span class="sxs-lookup"><span data-stu-id="f6bab-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="f6bab-125">Równość elementów członkowskich</span><span class="sxs-lookup"><span data-stu-id="f6bab-125">Equality members</span></span>

<span data-ttu-id="f6bab-126">Typy rekordów generują wdrożone implementacje dla następujących metod:</span><span class="sxs-lookup"><span data-stu-id="f6bab-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="f6bab-127">`object.GetHashCode()` przesłonięcia, chyba że jest zapieczętowany lub podany przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="f6bab-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="f6bab-128">`object.Equals(object)` przesłonięcia, chyba że jest zapieczętowany lub podany przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="f6bab-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="f6bab-129">Metoda `T Equals(T)`, gdzie `T` jest bieżącym typem</span><span class="sxs-lookup"><span data-stu-id="f6bab-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="f6bab-130">`T Equals(T)` jest określony w celu przeprowadzenia równości wartości, porównując właściwość z tą samą nazwą jak każdy parametr konstruktora podstawowego z odpowiednią właściwością innego typu.</span><span class="sxs-lookup"><span data-stu-id="f6bab-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="f6bab-131">`object.Equals` wykonuje odpowiednik</span><span class="sxs-lookup"><span data-stu-id="f6bab-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a><span data-ttu-id="f6bab-132">wyrażenie `with`</span><span class="sxs-lookup"><span data-stu-id="f6bab-132">`with` expression</span></span>

<span data-ttu-id="f6bab-133">Wyrażenie `with` jest nowym wyrażeniem używającym następującej składni.</span><span class="sxs-lookup"><span data-stu-id="f6bab-133">A `with` expression is a new expression using the following syntax.</span></span>

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

<span data-ttu-id="f6bab-134">Wyrażenie `with` umożliwia "mutację nieniszczącą", która jest przeznaczona do tworzenia kopii wyrażenia odbiorcy z modyfikacjami właściwości wymienionymi w `anonymous_object_initializer`.</span><span class="sxs-lookup"><span data-stu-id="f6bab-134">A `with` expression allows for "non-destructive mutation", designed to produce a copy of the receiver expression with modifications to properties listed in the `anonymous_object_initializer`.</span></span>

<span data-ttu-id="f6bab-135">Prawidłowe wyrażenie `with` ma odbiorcę z typem innym niż void.</span><span class="sxs-lookup"><span data-stu-id="f6bab-135">A valid `with` expression has a receiver with a non-void type.</span></span> <span data-ttu-id="f6bab-136">Typ odbiornika musi zawierać dostępną metodę wystąpienia o nazwie `With` z odpowiednimi parametrami i typem zwracanym.</span><span class="sxs-lookup"><span data-stu-id="f6bab-136">The receiver type must contain an accessible instance method called `With` with the appropriate parameters and return type.</span></span> <span data-ttu-id="f6bab-137">Jeśli istnieje wiele metod `With` zastąpień, występuje błąd.</span><span class="sxs-lookup"><span data-stu-id="f6bab-137">It is an error if there are multiple non-override `With` methods.</span></span> <span data-ttu-id="f6bab-138">Jeśli istnieje wiele `With` zastąpień, musi istnieć Metoda `With` niezastępująca, która jest metodą docelową.</span><span class="sxs-lookup"><span data-stu-id="f6bab-138">If there are multiple `With` overrides, there must be a non-override `With` method, which is the target method.</span></span> <span data-ttu-id="f6bab-139">W przeciwnym razie należy mieć dokładnie jedną metodę `With`.</span><span class="sxs-lookup"><span data-stu-id="f6bab-139">Otherwise, there must be exactly one `With` method.</span></span>

<span data-ttu-id="f6bab-140">Po prawej stronie wyrażenia `with` jest `anonymous_object_initializer` z sekwencją przypisań z polem lub właściwością odbiornika po lewej stronie przypisania, a dowolnym wyrażeniem po prawej stronie, które jest niejawnie konwertowane na typ lewej strony...</span><span class="sxs-lookup"><span data-stu-id="f6bab-140">On the right hand side of the `with` expression is an `anonymous_object_initializer` with a sequence of assignments with a field or property of the receiver on the left-hand side of the assignment, and an arbitrary expression on the right-hand side which is implicitly convertible to the type of the left-hand side.</span></span>

<span data-ttu-id="f6bab-141">Uwzględniając docelową metodę `With`, zwracany typ musi być typem wyrażenia odbiorcy lub typem podstawowym.</span><span class="sxs-lookup"><span data-stu-id="f6bab-141">Given a target `With` method, the return type must be the type of the receiver expression type, or a base type thereof.</span></span> <span data-ttu-id="f6bab-142">Dla każdego parametru metody `With` musi istnieć dostępne odpowiednie pole wystąpienia lub właściwość możliwa do odczytu w typie odbiornika o tej samej nazwie i typie.</span><span class="sxs-lookup"><span data-stu-id="f6bab-142">For each parameter of the `With` method, there must be an accessible corresponding instance field or readable property on the receiver type with the same name and the same type.</span></span> <span data-ttu-id="f6bab-143">Każda właściwość lub pole znajdujące się po prawej stronie wyrażenia with musi również odpowiadać parametrowi o tej samej nazwie w metodzie `With`.</span><span class="sxs-lookup"><span data-stu-id="f6bab-143">Each property or field in the right-hand side of the With expression must also correspond to a parameter of the same name in the `With` method.</span></span>

<span data-ttu-id="f6bab-144">Mając prawidłową metodę `With`, Obliczanie wyrażenia `with` jest równoznaczne z wywołaniem metody `With` z wyrażeniami w `anonymous_object_initializer` zastępują dla parametru o tej samej nazwie, co właściwość po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="f6bab-144">Given a valid `With` method, the evaluation of a `with` expression is equivalent to calling the `With` method with the expressions in the `anonymous_object_initializer` substituted for the parameter of the same name as the property on the left hand side.</span></span> <span data-ttu-id="f6bab-145">Jeśli nie ma żadnej pasującej właściwości dla danego parametru w `anonymous_object_initializer`, argument jest oszacowaniem pola lub właściwości o tej samej nazwie w odbiorniku.</span><span class="sxs-lookup"><span data-stu-id="f6bab-145">If there is no matching property for a given parameter in the `anonymous_object_initializer`, the argument is the evaluation of the field or property of the same name on the receiver.</span></span>

<span data-ttu-id="f6bab-146">Kolejność oceny efektów ubocznych jest następująca, a każde wyrażenie jest oceniane dokładnie raz:</span><span class="sxs-lookup"><span data-stu-id="f6bab-146">The order of evaluation of side effects is as follows, with each expression evaluated exactly once:</span></span>

1. <span data-ttu-id="f6bab-147">Wyrażenie odbiorcy</span><span class="sxs-lookup"><span data-stu-id="f6bab-147">Receiver expression</span></span>

2. <span data-ttu-id="f6bab-148">Wyrażenia w `anonymous_object_initializer`w kolejności leksykalnej</span><span class="sxs-lookup"><span data-stu-id="f6bab-148">Expressions in the `anonymous_object_initializer`, in lexical order</span></span>

3. <span data-ttu-id="f6bab-149">Obliczanie wszelkich właściwości pasujących do parametrów metody `With` w kolejności definicji parametrów metody `With`.</span><span class="sxs-lookup"><span data-stu-id="f6bab-149">The evaluation of any properties matching the `With` method parameters, in order of definition of the `With` method parameters.</span></span>