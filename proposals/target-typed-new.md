---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281973"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="0678f-101">Wyrażenia `new` z typem docelowym</span><span class="sxs-lookup"><span data-stu-id="0678f-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="0678f-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="0678f-102">[x] Proposed</span></span>
* <span data-ttu-id="0678f-103">[x] [prototyp](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="0678f-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="0678f-104">[] Implementacja</span><span class="sxs-lookup"><span data-stu-id="0678f-104">[ ] Implementation</span></span>
* <span data-ttu-id="0678f-105">[] — Specyfikacja</span><span class="sxs-lookup"><span data-stu-id="0678f-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="0678f-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="0678f-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="0678f-107">Nie wymagaj specyfikacji typu dla konstruktorów, gdy typ jest znany.</span><span class="sxs-lookup"><span data-stu-id="0678f-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="0678f-108">Motywacją</span><span class="sxs-lookup"><span data-stu-id="0678f-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="0678f-109">Zezwalaj na inicjowanie pola bez duplikowania typu.</span><span class="sxs-lookup"><span data-stu-id="0678f-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="0678f-110">Zezwalaj na pominięcie typu, jeśli można go wywnioskować na podstawie użycia.</span><span class="sxs-lookup"><span data-stu-id="0678f-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="0678f-111">Tworzenie wystąpienia obiektu bez wypełniania tekstu.</span><span class="sxs-lookup"><span data-stu-id="0678f-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="specification"></a><span data-ttu-id="0678f-112">Specyfikacja</span><span class="sxs-lookup"><span data-stu-id="0678f-112">Specification</span></span>
[design]: #detailed-design

<span data-ttu-id="0678f-113">Nowy formularz składni, *target_typed_new* *object_creation_expression* zostanie zaakceptowany, gdy *Typ* jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="0678f-113">A new syntactic form, *target_typed_new* of the *object_creation_expression* is accepted in which the *type* is optional.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

<span data-ttu-id="0678f-114">Wyrażenie *target_typed_new* nie ma typu.</span><span class="sxs-lookup"><span data-stu-id="0678f-114">A *target_typed_new* expression does not have a type.</span></span> <span data-ttu-id="0678f-115">Istnieje jednak Nowa *Konwersja tworzenia obiektów* , która jest niejawną konwersją z wyrażenia, która istnieje w *target_typed_new* do każdego typu.</span><span class="sxs-lookup"><span data-stu-id="0678f-115">However, there is a new *object creation conversion* that is an implicit conversion from expression, that exists from a *target_typed_new* to every type.</span></span>

<span data-ttu-id="0678f-116">Uwzględniając typ docelowy `T`, typ `T0` jest `T`typ podstawowy, jeśli `T` jest wystąpieniem `System.Nullable`.</span><span class="sxs-lookup"><span data-stu-id="0678f-116">Given a target type `T`, the type `T0` is `T`'s underlying type if `T` is an instance of `System.Nullable`.</span></span> <span data-ttu-id="0678f-117">W przeciwnym razie `T0` jest `T`.</span><span class="sxs-lookup"><span data-stu-id="0678f-117">Otherwise `T0` is `T`.</span></span> <span data-ttu-id="0678f-118">Znaczenie wyrażenia *target_typed_new* , które jest konwertowane na typ `T` jest takie samo, jak znaczenie odpowiadającego *object_creation_expression* , który określa `T0` jako typ.</span><span class="sxs-lookup"><span data-stu-id="0678f-118">The meaning of a *target_typed_new* expression that is converted to the type `T` is the same as the meaning of a corresponding *object_creation_expression* that specifies `T0` as the type.</span></span>

<span data-ttu-id="0678f-119">Jest to błąd czasu kompilacji, jeśli *target_typed_new* jest używany jako operand operatora jednoargumentowego lub binarnego lub jeśli jest używany, gdzie nie podlega *konwersji tworzenia obiektów*.</span><span class="sxs-lookup"><span data-stu-id="0678f-119">It is a compile-time error if a *target_typed_new* is used as an operand of a unary or binary operator, or if it is used where it is not subject to an *object creation conversion*.</span></span>

> <span data-ttu-id="0678f-120">**Problem otwarty:** czy zezwalamy na delegatów i krotek jako typ docelowy?</span><span class="sxs-lookup"><span data-stu-id="0678f-120">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="0678f-121">Powyższe reguły obejmują delegatów (typ referencyjny) i krotek (typ struktury).</span><span class="sxs-lookup"><span data-stu-id="0678f-121">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="0678f-122">Mimo że oba typy są konstrukcyjną, jeśli typ jest wywnioskowany, można już użyć funkcji anonimowej lub literału krotki.</span><span class="sxs-lookup"><span data-stu-id="0678f-122">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="0678f-123">Różne</span><span class="sxs-lookup"><span data-stu-id="0678f-123">Miscellaneous</span></span>

<span data-ttu-id="0678f-124">Poniżej przedstawiono konsekwencje specyfikacji:</span><span class="sxs-lookup"><span data-stu-id="0678f-124">The following are consequences of the specification:</span></span>

- <span data-ttu-id="0678f-125">dozwolony jest `throw new()` (typ docelowy to `System.Exception`)</span><span class="sxs-lookup"><span data-stu-id="0678f-125">`throw new()` is allowed (the target type is `System.Exception`)</span></span>
- <span data-ttu-id="0678f-126">`new` z typem docelowym nie jest dozwolony w przypadku operatorów binarnych.</span><span class="sxs-lookup"><span data-stu-id="0678f-126">Target-typed `new` is not allowed with binary operators.</span></span>
- <span data-ttu-id="0678f-127">Nie jest dozwolone, gdy nie ma żadnego typu docelowego: operatory jednoargumentowe, kolekcja `foreach`, w `using`, w trakcie dekonstrukcji, w wyrażeniu `await`, jako właściwość typu anonimowego (`new { Prop = new() }`), w instrukcji `lock`, w `sizeof`, w instrukcji `fixed`, w dostępie do elementu członkowskiego (`new().field`), w wyniku operacji dynamicznie wysyłanej (`someDynamic.Method(new())`) w zapytaniu LINQ jako operand operatora `is` jako lewy argument operacji operatora `??` ,  ...</span><span class="sxs-lookup"><span data-stu-id="0678f-127">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>
- <span data-ttu-id="0678f-128">Jest on również niedozwolony jako `ref`.</span><span class="sxs-lookup"><span data-stu-id="0678f-128">It is also disallowed as a `ref`.</span></span>
- <span data-ttu-id="0678f-129">Następujące rodzaje typów nie są dozwolone jako elementy docelowe konwersji</span><span class="sxs-lookup"><span data-stu-id="0678f-129">The following kinds of types are not permitted as targets of the conversion</span></span>
  - <span data-ttu-id="0678f-130">**Typy wyliczeniowe:** `new()` będzie działać (jak `new Enum()` działa, aby nadać wartość domyślną), ale `new(1)` nie będzie działać, ponieważ typy wyliczeniowe nie mają konstruktora.</span><span class="sxs-lookup"><span data-stu-id="0678f-130">**Enum types:** `new()` will work (as `new Enum()` works to give the default value), but `new(1)` will not work as enum types do not have a constructor.</span></span>
  - <span data-ttu-id="0678f-131">**Typy interfejsów:** To działanie będzie takie samo jak odpowiednie wyrażenie tworzenia dla typów COM.</span><span class="sxs-lookup"><span data-stu-id="0678f-131">**Interface types:** This would work the same as the corresponding creation expression for COM types.</span></span>
  - <span data-ttu-id="0678f-132">**Typy tablic:** tablice muszą mieć specjalną składnię, aby zapewnić długość.</span><span class="sxs-lookup"><span data-stu-id="0678f-132">**Array types:** arrays need a special syntax to provide the length.</span></span>    
  - <span data-ttu-id="0678f-133">**dynamiczne:** nie zezwalamy na `new dynamic()`, więc nie zezwalamy na `new()` z `dynamic` jako typ docelowy.</span><span class="sxs-lookup"><span data-stu-id="0678f-133">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>
  - <span data-ttu-id="0678f-134">**krotki:** Mają one takie samo znaczenie jak tworzenie obiektów przy użyciu typu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="0678f-134">**tuples:** These have the same meaning as an object creation using the underlying type.</span></span>
  - <span data-ttu-id="0678f-135">Wszystkie inne typy, które nie są dozwolone w *object_creation_expression* są również wykluczone, na przykład typy wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="0678f-135">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>   

## <a name="drawbacks"></a><span data-ttu-id="0678f-136">Wady</span><span class="sxs-lookup"><span data-stu-id="0678f-136">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="0678f-137">Istnieją pewne wątpliwości dotyczące `new` tworzenia nowych kategorii znaczących zmian, które zostały wprowadzone, ale istnieją już z `null` i `default`i nie było to znacznego problemu.</span><span class="sxs-lookup"><span data-stu-id="0678f-137">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="0678f-138">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="0678f-138">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="0678f-139">Większość reklamacji na temat typów, które są zbyt długie, aby można było duplikować w zainicjowaniu pól, nie jest jedynym typem *argumentów* , możemy wnioskować o argumenty typu, takie jak `new Dictionary(...)` (lub podobne) i wnioskowania argumentów typu lokalnie z argumentów lub inicjatora kolekcji.</span><span class="sxs-lookup"><span data-stu-id="0678f-139">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="0678f-140">Masz</span><span class="sxs-lookup"><span data-stu-id="0678f-140">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="0678f-141">Czy należy zabronić użycia w drzewach wyrażeń?</span><span class="sxs-lookup"><span data-stu-id="0678f-141">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="0678f-142">znaleziono</span><span class="sxs-lookup"><span data-stu-id="0678f-142">(no)</span></span>
- <span data-ttu-id="0678f-143">Jak działa funkcja z argumentami `dynamic`?</span><span class="sxs-lookup"><span data-stu-id="0678f-143">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="0678f-144">(bez specjalnego traktowania)</span><span class="sxs-lookup"><span data-stu-id="0678f-144">(no special treatment)</span></span>
- <span data-ttu-id="0678f-145">Jak technologia IntelliSense powinna współpracować z `new()`?</span><span class="sxs-lookup"><span data-stu-id="0678f-145">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="0678f-146">(tylko wtedy, gdy istnieje pojedynczy typ docelowy)</span><span class="sxs-lookup"><span data-stu-id="0678f-146">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="0678f-147">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="0678f-147">Design meetings</span></span>

- [<span data-ttu-id="0678f-148">LDM — 2017-10-18</span><span class="sxs-lookup"><span data-stu-id="0678f-148">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="0678f-149">LDM — 2018-05-21</span><span class="sxs-lookup"><span data-stu-id="0678f-149">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="0678f-150">LDM — 2018-06-25</span><span class="sxs-lookup"><span data-stu-id="0678f-150">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="0678f-151">LDM — 2018-08-22</span><span class="sxs-lookup"><span data-stu-id="0678f-151">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="0678f-152">LDM — 2018-10-17</span><span class="sxs-lookup"><span data-stu-id="0678f-152">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
- [<span data-ttu-id="0678f-153">LDM — 2020-03-25</span><span class="sxs-lookup"><span data-stu-id="0678f-153">LDM-2020-03-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
