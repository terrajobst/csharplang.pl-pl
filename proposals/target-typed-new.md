---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108375"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="5ae2d-101">Wyrażenia `new` z typem docelowym</span><span class="sxs-lookup"><span data-stu-id="5ae2d-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="5ae2d-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="5ae2d-102">[x] Proposed</span></span>
* <span data-ttu-id="5ae2d-103">[x] [prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="5ae2d-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="5ae2d-104">[] Implementacja</span><span class="sxs-lookup"><span data-stu-id="5ae2d-104">[ ] Implementation</span></span>
* <span data-ttu-id="5ae2d-105">[] — Specyfikacja</span><span class="sxs-lookup"><span data-stu-id="5ae2d-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="5ae2d-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="5ae2d-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="5ae2d-107">Nie wymagaj specyfikacji typu dla konstruktorów, gdy typ jest znany.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="5ae2d-108">Motywacją</span><span class="sxs-lookup"><span data-stu-id="5ae2d-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="5ae2d-109">Zezwalaj na inicjowanie pola bez duplikowania typu.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="5ae2d-110">Zezwalaj na pominięcie typu, jeśli można go wywnioskować na podstawie użycia.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="5ae2d-111">Tworzenie wystąpienia obiektu bez wypełniania tekstu.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="5ae2d-112">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="5ae2d-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="5ae2d-113">Składnia *object_creation_expression* zostanie zmodyfikowana, aby *Typ* był opcjonalny, gdy nawiasy są obecne.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="5ae2d-114">Jest to wymagane, aby rozwiązać niejednoznaczność za pomocą *anonymous_object_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="5ae2d-115">`new` z typem docelowym jest konwertowany na dowolny typ.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="5ae2d-116">W związku z tym nie przyczynia się do rozpoznania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="5ae2d-117">Jest to przede wszystkim uniknięcie nieprzewidywalnych zmian.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="5ae2d-118">Lista argumentów i wyrażenia inicjatora będą powiązane po ustaleniu typu.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="5ae2d-119">Typ wyrażenia zostanie wywnioskowany na podstawie typu docelowego, który będzie wymagał jednego z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="5ae2d-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="5ae2d-120">**Dowolny typ struktury** (w tym typy krotek)</span><span class="sxs-lookup"><span data-stu-id="5ae2d-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="5ae2d-121">**Dowolny typ odwołania** (w tym typy delegatów)</span><span class="sxs-lookup"><span data-stu-id="5ae2d-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="5ae2d-122">**Dowolny parametr typu** z konstruktorem lub ograniczeniem `struct`</span><span class="sxs-lookup"><span data-stu-id="5ae2d-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="5ae2d-123">z następującymi wyjątkami:</span><span class="sxs-lookup"><span data-stu-id="5ae2d-123">with the following exceptions:</span></span>

- <span data-ttu-id="5ae2d-124">**Typy wyliczeniowe:** nie wszystkie typy wyliczeniowe zawierają stałą zero, dlatego powinno być wskazane użycie jawnego elementu członkowskiego wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="5ae2d-125">**Typy interfejsów:** jest to funkcja niszowa, dlatego warto jawnie wspominać o typie.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="5ae2d-126">**Typy tablic:** tablice muszą mieć specjalną składnię, aby zapewnić długość.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="5ae2d-127">**dynamiczne:** nie zezwalamy na `new dynamic()`, więc nie zezwalamy na `new()` z `dynamic` jako typ docelowy.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="5ae2d-128">Wszystkie inne typy, które nie są dozwolone w *object_creation_expression* są również wykluczone, na przykład typy wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="5ae2d-129">Gdy typ docelowy jest typem wartości null, `new` wpisany przez element docelowy zostanie przekonwertowany na typ podstawowy zamiast typu dopuszczającego wartość null.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="5ae2d-130">**Problem otwarty:** czy zezwalamy na delegatów i krotek jako typ docelowy?</span><span class="sxs-lookup"><span data-stu-id="5ae2d-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="5ae2d-131">Powyższe reguły obejmują delegatów (typ referencyjny) i krotek (typ struktury).</span><span class="sxs-lookup"><span data-stu-id="5ae2d-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="5ae2d-132">Mimo że oba typy są konstrukcyjną, jeśli typ jest wywnioskowany, można już użyć funkcji anonimowej lub literału krotki.</span><span class="sxs-lookup"><span data-stu-id="5ae2d-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
