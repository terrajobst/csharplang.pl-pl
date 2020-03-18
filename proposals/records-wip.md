---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2020
ms.locfileid: "79485239"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="048c1-101">Rejestruje prace w toku</span><span class="sxs-lookup"><span data-stu-id="048c1-101">Records Work-in-Progress</span></span>

<span data-ttu-id="048c1-102">W przeciwieństwie do innych rekordów propozycje nie jest to propozycja, ale prace w toku zaprojektowane do rejestrowania jednomyślnych decyzji projektowych dotyczących funkcji Records.</span><span class="sxs-lookup"><span data-stu-id="048c1-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="048c1-103">Szczegóły specyfikacji zostaną dodane w razie potrzeby w celu rozwiązania pytań.</span><span class="sxs-lookup"><span data-stu-id="048c1-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="048c1-104">Składnia dla rekordu jest proponowana do dodania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="048c1-104">The syntax for a record is proposed to be added as follows:</span></span>

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

<span data-ttu-id="048c1-105">`attributes` inne niż Terminal zezwoli również na nowy atrybut kontekstowy, `data`.</span><span class="sxs-lookup"><span data-stu-id="048c1-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="048c1-106">Klasa (struktura) zadeklarowana z listą parametrów lub modyfikatorem `data` jest nazywana klasą rekordów (strukturą rekordów), z których każdy jest typem rekordu.</span><span class="sxs-lookup"><span data-stu-id="048c1-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="048c1-107">Jest to błąd do deklarowania typu rekordu bez zarówno listy parametrów, jak i modyfikatora `data`.</span><span class="sxs-lookup"><span data-stu-id="048c1-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="048c1-108">Elementy członkowskie typu rekordu</span><span class="sxs-lookup"><span data-stu-id="048c1-108">Members of a record type</span></span>

<span data-ttu-id="048c1-109">Oprócz elementów członkowskich zadeklarowanych w treści klasy, typ rekordu ma następujące dodatkowe elementy członkowskie:</span><span class="sxs-lookup"><span data-stu-id="048c1-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="048c1-110">Konstruktor podstawowy</span><span class="sxs-lookup"><span data-stu-id="048c1-110">Primary Constructor</span></span>

<span data-ttu-id="048c1-111">Typ rekordu ma Konstruktor publiczny, którego sygnatura odnosi się do parametrów wartości deklaracji typu.</span><span class="sxs-lookup"><span data-stu-id="048c1-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="048c1-112">Jest to nazywane konstruktorem podstawowym dla typu i powoduje pominięcie niejawnie zadeklarowanego konstruktora domyślnego.</span><span class="sxs-lookup"><span data-stu-id="048c1-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="048c1-113">Wystąpił błąd w przypadku konstruktora podstawowego i konstruktora o tym samym podpisie w klasie.</span><span class="sxs-lookup"><span data-stu-id="048c1-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="048c1-114">W czasie wykonywania podstawowy Konstruktor</span><span class="sxs-lookup"><span data-stu-id="048c1-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="048c1-115">wykonuje inicjatory pola wystąpienia pojawiające się w treści klasy; a następnie wywołuje konstruktora klasy bazowej bez argumentów.</span><span class="sxs-lookup"><span data-stu-id="048c1-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="048c1-116">Inicjuje pola zapasowe generowane przez kompilator dla właściwości odpowiadających parametrom wartości (jeśli te właściwości są dostarczane przez kompilator; zobacz [właściwości z syntezą](#Synthesized Properties))</span><span class="sxs-lookup"><span data-stu-id="048c1-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="048c1-117">[] Do zrobienia: Dodaj składnię podstawowego wywołania i specyfikację dotyczącą wybierania konstruktora podstawowego przy użyciu rozdzielczości przeciążenia</span><span class="sxs-lookup"><span data-stu-id="048c1-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="048c1-118">Właściwości</span><span class="sxs-lookup"><span data-stu-id="048c1-118">Properties</span></span>

<span data-ttu-id="048c1-119">Dla każdego parametru rekordu deklaracji typu rekordu istnieje odpowiedni publiczny element członkowski właściwości, którego nazwa i typ są pobierane z deklaracji parametru wartości.</span><span class="sxs-lookup"><span data-stu-id="048c1-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="048c1-120">Jeśli żadna konkretna Właściwość (tj. non-abstract) z metodą dostępu get i z tą nazwą i typem jest jawnie zadeklarowana lub dziedziczona, jest generowana przez kompilator w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="048c1-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="048c1-121">Dla struktury rekordu lub klasy rekordu:</span><span class="sxs-lookup"><span data-stu-id="048c1-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="048c1-122">Zostanie utworzona publiczna właściwość "Get-Only".</span><span class="sxs-lookup"><span data-stu-id="048c1-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="048c1-123">Jej wartość jest inicjowana podczas konstruowania z wartością odpowiadającego parametru podstawowego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="048c1-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="048c1-124">Każdy "dopasowany" Dziedziczony Właściwość abstract get jest zastępowany.</span><span class="sxs-lookup"><span data-stu-id="048c1-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="048c1-125">Równość elementów członkowskich</span><span class="sxs-lookup"><span data-stu-id="048c1-125">Equality members</span></span>

<span data-ttu-id="048c1-126">Typy rekordów generują wdrożone implementacje dla następujących metod:</span><span class="sxs-lookup"><span data-stu-id="048c1-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="048c1-127">`object.GetHashCode()` przesłonięcia, chyba że jest zapieczętowany lub podany przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="048c1-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="048c1-128">`object.Equals(object)` przesłonięcia, chyba że jest zapieczętowany lub podany przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="048c1-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="048c1-129">Metoda `T Equals(T)`, gdzie `T` jest bieżącym typem</span><span class="sxs-lookup"><span data-stu-id="048c1-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="048c1-130">`T Equals(T)` jest określony w celu przeprowadzenia równości wartości, porównując właściwość z tą samą nazwą jak każdy parametr konstruktora podstawowego z odpowiednią właściwością innego typu.</span><span class="sxs-lookup"><span data-stu-id="048c1-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="048c1-131">`object.Equals` wykonuje odpowiednik</span><span class="sxs-lookup"><span data-stu-id="048c1-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```
