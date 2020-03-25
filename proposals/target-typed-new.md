---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217206"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="92ade-101">Wyrażenia `new` z typem docelowym</span><span class="sxs-lookup"><span data-stu-id="92ade-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="92ade-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="92ade-102">[x] Proposed</span></span>
* <span data-ttu-id="92ade-103">[x] [prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="92ade-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="92ade-104">[] Implementacja</span><span class="sxs-lookup"><span data-stu-id="92ade-104">[ ] Implementation</span></span>
* <span data-ttu-id="92ade-105">[] — Specyfikacja</span><span class="sxs-lookup"><span data-stu-id="92ade-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="92ade-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="92ade-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="92ade-107">Nie wymagaj specyfikacji typu dla konstruktorów, gdy typ jest znany.</span><span class="sxs-lookup"><span data-stu-id="92ade-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="92ade-108">Motywacją</span><span class="sxs-lookup"><span data-stu-id="92ade-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="92ade-109">Zezwalaj na inicjowanie pola bez duplikowania typu.</span><span class="sxs-lookup"><span data-stu-id="92ade-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="92ade-110">Zezwalaj na pominięcie typu, jeśli można go wywnioskować na podstawie użycia.</span><span class="sxs-lookup"><span data-stu-id="92ade-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="92ade-111">Tworzenie wystąpienia obiektu bez wypełniania tekstu.</span><span class="sxs-lookup"><span data-stu-id="92ade-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="92ade-112">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="92ade-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="92ade-113">Składnia *object_creation_expression* zostanie zmodyfikowana, aby *Typ* był opcjonalny, gdy nawiasy są obecne.</span><span class="sxs-lookup"><span data-stu-id="92ade-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="92ade-114">Jest to wymagane, aby rozwiązać niejednoznaczność za pomocą *anonymous_object_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="92ade-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="92ade-115">`new` z typem docelowym jest konwertowany na dowolny typ.</span><span class="sxs-lookup"><span data-stu-id="92ade-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="92ade-116">W związku z tym nie przyczynia się do rozpoznania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="92ade-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="92ade-117">Jest to przede wszystkim uniknięcie nieprzewidywalnych zmian.</span><span class="sxs-lookup"><span data-stu-id="92ade-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="92ade-118">Lista argumentów i wyrażenia inicjatora będą powiązane po ustaleniu typu.</span><span class="sxs-lookup"><span data-stu-id="92ade-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="92ade-119">Typ wyrażenia zostanie wywnioskowany na podstawie typu docelowego, który będzie wymagał jednego z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="92ade-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="92ade-120">**Dowolny typ struktury** (w tym typy krotek)</span><span class="sxs-lookup"><span data-stu-id="92ade-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="92ade-121">**Dowolny typ odwołania** (w tym typy delegatów)</span><span class="sxs-lookup"><span data-stu-id="92ade-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="92ade-122">**Dowolny parametr typu** z konstruktorem lub ograniczeniem `struct`</span><span class="sxs-lookup"><span data-stu-id="92ade-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="92ade-123">z następującymi wyjątkami:</span><span class="sxs-lookup"><span data-stu-id="92ade-123">with the following exceptions:</span></span>

- <span data-ttu-id="92ade-124">**Typy wyliczeniowe:** nie wszystkie typy wyliczeniowe zawierają stałą zero, dlatego powinno być wskazane użycie jawnego elementu członkowskiego wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="92ade-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="92ade-125">**Typy interfejsów:** jest to funkcja niszowa, dlatego warto jawnie wspominać o typie.</span><span class="sxs-lookup"><span data-stu-id="92ade-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="92ade-126">**Typy tablic:** tablice muszą mieć specjalną składnię, aby zapewnić długość.</span><span class="sxs-lookup"><span data-stu-id="92ade-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="92ade-127">**dynamiczne:** nie zezwalamy na `new dynamic()`, więc nie zezwalamy na `new()` z `dynamic` jako typ docelowy.</span><span class="sxs-lookup"><span data-stu-id="92ade-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="92ade-128">Wszystkie inne typy, które nie są dozwolone w *object_creation_expression* są również wykluczone, na przykład typy wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="92ade-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="92ade-129">Gdy typ docelowy jest typem wartości null, `new` wpisany przez element docelowy zostanie przekonwertowany na typ podstawowy zamiast typu dopuszczającego wartość null.</span><span class="sxs-lookup"><span data-stu-id="92ade-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="92ade-130">**Problem otwarty:** czy zezwalamy na delegatów i krotek jako typ docelowy?</span><span class="sxs-lookup"><span data-stu-id="92ade-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="92ade-131">Powyższe reguły obejmują delegatów (typ referencyjny) i krotek (typ struktury).</span><span class="sxs-lookup"><span data-stu-id="92ade-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="92ade-132">Mimo że oba typy są konstrukcyjną, jeśli typ jest wywnioskowany, można już użyć funkcji anonimowej lub literału krotki.</span><span class="sxs-lookup"><span data-stu-id="92ade-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="92ade-133">Różne</span><span class="sxs-lookup"><span data-stu-id="92ade-133">Miscellaneous</span></span>

<span data-ttu-id="92ade-134">`throw new()` jest niedozwolona.</span><span class="sxs-lookup"><span data-stu-id="92ade-134">`throw new()` is disallowed.</span></span>

<span data-ttu-id="92ade-135">`new` z typem docelowym nie jest dozwolony w przypadku operatorów binarnych.</span><span class="sxs-lookup"><span data-stu-id="92ade-135">Target-typed `new` is not allowed with binary operators.</span></span>

<span data-ttu-id="92ade-136">Nie jest dozwolone, gdy nie ma żadnego typu docelowego: operatory jednoargumentowe, kolekcja `foreach`, w `using`, w trakcie dekonstrukcji, w wyrażeniu `await`, jako właściwość typu anonimowego (`new { Prop = new() }`), w instrukcji `lock`, w `sizeof`, w instrukcji `fixed`, w dostępie do elementu członkowskiego (`new().field`), w wyniku operacji dynamicznie wysyłanej (`someDynamic.Method(new())`) w zapytaniu LINQ jako operand operatora `is` jako lewy argument operacji operatora `??` ,  ...</span><span class="sxs-lookup"><span data-stu-id="92ade-136">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>

<span data-ttu-id="92ade-137">Jest on również niedozwolony jako `ref`.</span><span class="sxs-lookup"><span data-stu-id="92ade-137">It is also disallowed as a `ref`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="92ade-138">Wady</span><span class="sxs-lookup"><span data-stu-id="92ade-138">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="92ade-139">Istnieją pewne wątpliwości dotyczące `new` tworzenia nowych kategorii znaczących zmian, które zostały wprowadzone, ale istnieją już z `null` i `default`i nie było to znacznego problemu.</span><span class="sxs-lookup"><span data-stu-id="92ade-139">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="92ade-140">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="92ade-140">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="92ade-141">Większość reklamacji na temat typów, które są zbyt długie, aby można było duplikować w zainicjowaniu pól, nie jest jedynym typem *argumentów* , możemy wnioskować o argumenty typu, takie jak `new Dictionary(...)` (lub podobne) i wnioskowania argumentów typu lokalnie z argumentów lub inicjatora kolekcji.</span><span class="sxs-lookup"><span data-stu-id="92ade-141">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="92ade-142">Masz</span><span class="sxs-lookup"><span data-stu-id="92ade-142">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="92ade-143">Czy należy zabronić użycia w drzewach wyrażeń?</span><span class="sxs-lookup"><span data-stu-id="92ade-143">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="92ade-144">znaleziono</span><span class="sxs-lookup"><span data-stu-id="92ade-144">(no)</span></span>
- <span data-ttu-id="92ade-145">Jak działa funkcja z argumentami `dynamic`?</span><span class="sxs-lookup"><span data-stu-id="92ade-145">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="92ade-146">(bez specjalnego traktowania)</span><span class="sxs-lookup"><span data-stu-id="92ade-146">(no special treatment)</span></span>
- <span data-ttu-id="92ade-147">Jak technologia IntelliSense powinna współpracować z `new()`?</span><span class="sxs-lookup"><span data-stu-id="92ade-147">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="92ade-148">(tylko wtedy, gdy istnieje pojedynczy typ docelowy)</span><span class="sxs-lookup"><span data-stu-id="92ade-148">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="92ade-149">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="92ade-149">Design meetings</span></span>

- [<span data-ttu-id="92ade-150">LDM — 2017-10-18</span><span class="sxs-lookup"><span data-stu-id="92ade-150">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="92ade-151">LDM — 2018-05-21</span><span class="sxs-lookup"><span data-stu-id="92ade-151">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="92ade-152">LDM — 2018-06-25</span><span class="sxs-lookup"><span data-stu-id="92ade-152">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="92ade-153">LDM — 2018-08-22</span><span class="sxs-lookup"><span data-stu-id="92ade-153">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="92ade-154">LDM — 2018-10-17</span><span class="sxs-lookup"><span data-stu-id="92ade-154">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
