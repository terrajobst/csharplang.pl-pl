---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484539"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="8bc65-101">Wyrażenia `new` z typem docelowym</span><span class="sxs-lookup"><span data-stu-id="8bc65-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="8bc65-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="8bc65-102">[x] Proposed</span></span>
* <span data-ttu-id="8bc65-103">[x] [prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="8bc65-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="8bc65-104">[] Implementacja</span><span class="sxs-lookup"><span data-stu-id="8bc65-104">[ ] Implementation</span></span>
* <span data-ttu-id="8bc65-105">[] — Specyfikacja</span><span class="sxs-lookup"><span data-stu-id="8bc65-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="8bc65-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="8bc65-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="8bc65-107">Nie wymagaj specyfikacji typu dla konstruktorów, gdy typ jest znany.</span><span class="sxs-lookup"><span data-stu-id="8bc65-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="8bc65-108">Motywacją</span><span class="sxs-lookup"><span data-stu-id="8bc65-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="8bc65-109">Zezwalaj na inicjowanie pola bez duplikowania typu.</span><span class="sxs-lookup"><span data-stu-id="8bc65-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
<span data-ttu-id="8bc65-110">Zezwalaj na pominięcie typu, jeśli można go wywnioskować na podstawie użycia.</span><span class="sxs-lookup"><span data-stu-id="8bc65-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
<span data-ttu-id="8bc65-111">Tworzenie wystąpienia obiektu bez wypełniania tekstu.</span><span class="sxs-lookup"><span data-stu-id="8bc65-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a><span data-ttu-id="8bc65-112">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="8bc65-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="8bc65-113">Składnia *object_creation_expression* zostanie zmodyfikowana, aby *Typ* był opcjonalny, gdy nawiasy są obecne.</span><span class="sxs-lookup"><span data-stu-id="8bc65-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="8bc65-114">Jest to wymagane, aby rozwiązać niejednoznaczność za pomocą *anonymous_object_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="8bc65-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
<span data-ttu-id="8bc65-115">`new` z typem docelowym jest konwertowany na dowolny typ.</span><span class="sxs-lookup"><span data-stu-id="8bc65-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="8bc65-116">W związku z tym nie przyczynia się do rozpoznania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="8bc65-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="8bc65-117">Jest to przede wszystkim uniknięcie nieprzewidywalnych zmian.</span><span class="sxs-lookup"><span data-stu-id="8bc65-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="8bc65-118">Lista argumentów i wyrażenia inicjatora będą powiązane po ustaleniu typu.</span><span class="sxs-lookup"><span data-stu-id="8bc65-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="8bc65-119">Typ wyrażenia zostanie wywnioskowany na podstawie typu docelowego, który będzie wymagał jednego z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="8bc65-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="8bc65-120">**Dowolny typ struktury**</span><span class="sxs-lookup"><span data-stu-id="8bc65-120">**Any struct type**</span></span>
- <span data-ttu-id="8bc65-121">**Dowolny typ referencyjny**</span><span class="sxs-lookup"><span data-stu-id="8bc65-121">**Any reference type**</span></span>
- <span data-ttu-id="8bc65-122">**Dowolny parametr typu** z konstruktorem lub ograniczeniem `struct`</span><span class="sxs-lookup"><span data-stu-id="8bc65-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="8bc65-123">z następującymi wyjątkami:</span><span class="sxs-lookup"><span data-stu-id="8bc65-123">with the following exceptions:</span></span>

- <span data-ttu-id="8bc65-124">**Typy wyliczeniowe:** nie wszystkie typy wyliczeniowe zawierają stałą zero, dlatego powinno być wskazane użycie jawnego elementu członkowskiego wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="8bc65-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="8bc65-125">**Typy interfejsów:** jest to funkcja niszowa, dlatego warto jawnie wspominać o typie.</span><span class="sxs-lookup"><span data-stu-id="8bc65-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="8bc65-126">**Typy tablic:** tablice muszą mieć specjalną składnię, aby zapewnić długość.</span><span class="sxs-lookup"><span data-stu-id="8bc65-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="8bc65-127">**Konstruktor domyślny struktury**: Ta reguła określa wszystkie typy pierwotne i większość typów wartości.</span><span class="sxs-lookup"><span data-stu-id="8bc65-127">**Struct default constructor**: this rules out all primitive types and most value types.</span></span> <span data-ttu-id="8bc65-128">Jeśli chcesz użyć wartości domyślnej takich typów, możesz zamiast tego napisać `default`.</span><span class="sxs-lookup"><span data-stu-id="8bc65-128">If you wanted to use the default value of such types you could write `default` instead.</span></span>

<span data-ttu-id="8bc65-129">Wszystkie inne typy, które nie są dozwolone w *object_creation_expression* są również wykluczone, na przykład typy wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="8bc65-129">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

> <span data-ttu-id="8bc65-130">**Problem otwarty:** czy zezwalamy na delegatów i krotek jako typ docelowy?</span><span class="sxs-lookup"><span data-stu-id="8bc65-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="8bc65-131">Powyższe reguły obejmują delegatów (typ referencyjny) i krotek (typ struktury).</span><span class="sxs-lookup"><span data-stu-id="8bc65-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="8bc65-132">Mimo że oba typy są konstrukcyjną, jeśli typ jest wywnioskowany, można już użyć funkcji anonimowej lub literału krotki.</span><span class="sxs-lookup"><span data-stu-id="8bc65-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> <span data-ttu-id="8bc65-133">**Problem otwarty:** czy `throw new()` z `Exception` jako typ docelowy?</span><span class="sxs-lookup"><span data-stu-id="8bc65-133">**Open Issue:** should we allow `throw new()` with `Exception` as the target-type?</span></span>

<span data-ttu-id="8bc65-134">Mamy `throw null` dzisiaj, ale nie `throw default` (chociaż miałoby to ten sam skutek).</span><span class="sxs-lookup"><span data-stu-id="8bc65-134">We have `throw null` today, but not `throw default` (though it would have the same effect).</span></span> <span data-ttu-id="8bc65-135">Z drugiej strony `throw new()` mogą być rzeczywiście przydatne jako skróty `throw new Exception(...)`.</span><span class="sxs-lookup"><span data-stu-id="8bc65-135">On the other hand, `throw new()` could be actually useful as a shorthand for `throw new Exception(...)`.</span></span> <span data-ttu-id="8bc65-136">Należy zauważyć, że jest już dozwolony przez bieżącą specyfikację.</span><span class="sxs-lookup"><span data-stu-id="8bc65-136">Note that it is already allowed by the current specification.</span></span> <span data-ttu-id="8bc65-137">`Exception` jest typem referencyjnym, a Specyfikacja instrukcji throw wskazuje, że wyrażenie jest konwertowane na `Exception`.</span><span class="sxs-lookup"><span data-stu-id="8bc65-137">`Exception` is a reference type, and the specification for the throw statement says that the expression is converted to `Exception`.</span></span>

> <span data-ttu-id="8bc65-138">**Problem otwarty:** czy zezwalamy na użycie `new` typu docelowego z porównaniem zdefiniowanym przez użytkownika i operatorami arytmetycznymi?</span><span class="sxs-lookup"><span data-stu-id="8bc65-138">**Open Issue:** should we allow usages of a target-typed `new` with user-defined comparison and arithmetic operators?</span></span>

<span data-ttu-id="8bc65-139">W celu porównania `default` obsługuje tylko równości (zdefiniowane przez użytkownika i wbudowane) operatory.</span><span class="sxs-lookup"><span data-stu-id="8bc65-139">For comparison, `default` only supports equality (user-defined and built-in) operators.</span></span> <span data-ttu-id="8bc65-140">Czy warto również zapewnić obsługę innych operatorów dla `new()`?</span><span class="sxs-lookup"><span data-stu-id="8bc65-140">Would it make sense to support other operators for `new()` as well?</span></span>

## <a name="drawbacks"></a><span data-ttu-id="8bc65-141">Wady</span><span class="sxs-lookup"><span data-stu-id="8bc65-141">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="8bc65-142">Brak.</span><span class="sxs-lookup"><span data-stu-id="8bc65-142">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="8bc65-143">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="8bc65-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="8bc65-144">Większość reklamacji na temat typów, które są zbyt długie, aby można było duplikować w zainicjowaniu pól, nie jest jedynym typem *argumentów* , możemy wnioskować o argumenty typu, takie jak `new Dictionary(...)` (lub podobne) i wnioskowania argumentów typu lokalnie z argumentów lub inicjatora kolekcji.</span><span class="sxs-lookup"><span data-stu-id="8bc65-144">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="8bc65-145">Masz</span><span class="sxs-lookup"><span data-stu-id="8bc65-145">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="8bc65-146">Czy należy zabronić użycia w drzewach wyrażeń?</span><span class="sxs-lookup"><span data-stu-id="8bc65-146">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="8bc65-147">znaleziono</span><span class="sxs-lookup"><span data-stu-id="8bc65-147">(no)</span></span>
- <span data-ttu-id="8bc65-148">Jak działa funkcja z argumentami `dynamic`?</span><span class="sxs-lookup"><span data-stu-id="8bc65-148">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="8bc65-149">(bez specjalnego traktowania)</span><span class="sxs-lookup"><span data-stu-id="8bc65-149">(no special treatment)</span></span>
- <span data-ttu-id="8bc65-150">Jak technologia IntelliSense powinna współpracować z `new()`?</span><span class="sxs-lookup"><span data-stu-id="8bc65-150">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="8bc65-151">(tylko wtedy, gdy istnieje pojedynczy typ docelowy)</span><span class="sxs-lookup"><span data-stu-id="8bc65-151">(only when there is a single target-type)</span></span>
## <a name="design-meetings"></a><span data-ttu-id="8bc65-152">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="8bc65-152">Design meetings</span></span>

- [<span data-ttu-id="8bc65-153">LDM — 2017-10-18</span><span class="sxs-lookup"><span data-stu-id="8bc65-153">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
