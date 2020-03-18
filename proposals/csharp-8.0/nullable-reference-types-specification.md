---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/26/2020
ms.locfileid: "79485211"
---
# <a name="nullable-reference-types-specification"></a><span data-ttu-id="7978c-101">Specyfikacja typów odwołań dopuszczających wartość null</span><span class="sxs-lookup"><span data-stu-id="7978c-101">Nullable Reference Types Specification</span></span>

<span data-ttu-id="7978c-102">Jest to w toku — brakuje kilku części lub są one niekompletne.</span><span class="sxs-lookup"><span data-stu-id="7978c-102">\*\*\* This is a work in progress - several parts are missing or incomplete.</span></span> ***

## <a name="syntax"></a><span data-ttu-id="7978c-103">Składnia</span><span class="sxs-lookup"><span data-stu-id="7978c-103">Syntax</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="7978c-104">Typy referencyjne dopuszczające wartość null</span><span class="sxs-lookup"><span data-stu-id="7978c-104">Nullable reference types</span></span>

<span data-ttu-id="7978c-105">Typy referencyjne dopuszczające wartość null mają tę samą składnię `T?` co krótka forma wartości null, ale nie ma odpowiedniej długiej formy.</span><span class="sxs-lookup"><span data-stu-id="7978c-105">Nullable reference types have the same syntax `T?` as the short form of nullable value types, but do not have a corresponding long form.</span></span>

<span data-ttu-id="7978c-106">Na potrzeby specyfikacji bieżąca `nullable_type` produkcyjna zostanie zmieniona na `nullable_value_type`i zostanie dodana `nullable_reference_type` produkcyjna:</span><span class="sxs-lookup"><span data-stu-id="7978c-106">For the purposes of the specification, the current `nullable_type` production is renamed to `nullable_value_type`, and a `nullable_reference_type` production is added:</span></span>

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

<span data-ttu-id="7978c-107">`non_nullable_reference_type` w `nullable_reference_type` musi być typem odwołania niedopuszczający wartości null (klasą, interfejsem, delegatem lub tablicą) lub parametrem typu, który jest ograniczony jako typ referencyjny niedopuszczający wartości null (za pomocą ograniczenia `class` lub klasy innej niż `object`).</span><span class="sxs-lookup"><span data-stu-id="7978c-107">The `non_nullable_reference_type` in a `nullable_reference_type` must be a non-nullable reference type (class, interface, delegate or array), or a type parameter that is constrained to be a non-nullable reference type (through the `class` constraint, or a class other than `object`).</span></span>

<span data-ttu-id="7978c-108">Typy referencyjne dopuszczające wartość null nie mogą wystąpić w następujących położeniach:</span><span class="sxs-lookup"><span data-stu-id="7978c-108">Nullable reference types cannot occur in the following positions:</span></span>

- <span data-ttu-id="7978c-109">jako klasa bazowa lub interfejs</span><span class="sxs-lookup"><span data-stu-id="7978c-109">as a base class or interface</span></span>
- <span data-ttu-id="7978c-110">jako odbiornik `member_access`</span><span class="sxs-lookup"><span data-stu-id="7978c-110">as the receiver of a `member_access`</span></span>
- <span data-ttu-id="7978c-111">jak `type` w `object_creation_expression`</span><span class="sxs-lookup"><span data-stu-id="7978c-111">as the `type` in an `object_creation_expression`</span></span>
- <span data-ttu-id="7978c-112">jak `delegate_type` w `delegate_creation_expression`</span><span class="sxs-lookup"><span data-stu-id="7978c-112">as the `delegate_type` in a `delegate_creation_expression`</span></span>
- <span data-ttu-id="7978c-113">jak `type` w `is_expression`, `catch_clause` lub `type_pattern`</span><span class="sxs-lookup"><span data-stu-id="7978c-113">as the `type` in an `is_expression`, a `catch_clause` or a `type_pattern`</span></span>
- <span data-ttu-id="7978c-114">jako `interface` w w pełni kwalifikowanej nazwie elementu członkowskiego interfejsu</span><span class="sxs-lookup"><span data-stu-id="7978c-114">as the `interface` in a fully qualified interface member name</span></span>

<span data-ttu-id="7978c-115">Ostrzeżenie jest udzielane na `nullable_reference_type`, gdzie kontekst adnotacji dopuszczający wartość null jest wyłączony.</span><span class="sxs-lookup"><span data-stu-id="7978c-115">A warning is given on a `nullable_reference_type` where the nullable annotation context is disabled.</span></span>

### <a name="nullable-class-constraint"></a><span data-ttu-id="7978c-116">Ograniczenie klasy dopuszczające wartość null</span><span class="sxs-lookup"><span data-stu-id="7978c-116">Nullable class constraint</span></span>

<span data-ttu-id="7978c-117">Ograniczenie `class` ma odpowiednik wartości null `class?`:</span><span class="sxs-lookup"><span data-stu-id="7978c-117">The `class` constraint has a nullable counterpart `class?`:</span></span>

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a><span data-ttu-id="7978c-118">Operator łagodniejszej o wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-118">The null-forgiving operator</span></span>

<span data-ttu-id="7978c-119">Operator `!` po naprawieniu jest nazywany operatorem łagodniejszej o wartości null.</span><span class="sxs-lookup"><span data-stu-id="7978c-119">The post-fix `!` operator is called the null-forgiving operator.</span></span>

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

<span data-ttu-id="7978c-120">`primary_expression` musi być typu referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="7978c-120">The `primary_expression` must be of a reference type.</span></span>  

<span data-ttu-id="7978c-121">Przyrostkowy operator `!` nie ma efektu środowiska uruchomieniowego — oblicza wynik wyrażenia bazowego.</span><span class="sxs-lookup"><span data-stu-id="7978c-121">The postfix `!` operator has no runtime effect - it evaluates to the result of the underlying expression.</span></span> <span data-ttu-id="7978c-122">Jedyną rolą jest zmiana stanu wartości null wyrażenia i ograniczenie ostrzeżeń podawanych w jego użyciu.</span><span class="sxs-lookup"><span data-stu-id="7978c-122">Its only role is to change the null state of the expression, and to limit warnings given on its use.</span></span>

### <a name="nullable-implicitly-typed-local-variables"></a><span data-ttu-id="7978c-123">niejawnie wpisane wartości null zmienne lokalne</span><span class="sxs-lookup"><span data-stu-id="7978c-123">nullable implicitly typed local variables</span></span>

<span data-ttu-id="7978c-124">`var` wnioskuje typ z adnotacjami dla typów referencyjnych.</span><span class="sxs-lookup"><span data-stu-id="7978c-124">`var` infers an annotated type for reference types.</span></span>
<span data-ttu-id="7978c-125">Na przykład, w `var s = "";` `var` jest wywnioskowany jako `string?`.</span><span class="sxs-lookup"><span data-stu-id="7978c-125">For instance, in `var s = "";` the `var` is inferred as `string?`.</span></span>

### <a name="nullable-compiler-directives"></a><span data-ttu-id="7978c-126">Dyrektywy kompilatora dopuszczające wartość null</span><span class="sxs-lookup"><span data-stu-id="7978c-126">Nullable compiler directives</span></span>

<span data-ttu-id="7978c-127">dyrektywy `#nullable` kontrolują adnotacje dopuszczające wartość null i konteksty ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="7978c-127">`#nullable` directives control the nullable annotation and warning contexts.</span></span>

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

<span data-ttu-id="7978c-128">dyrektywy `#pragma warning` są rozwinięte, aby umożliwić zmianę kontekstu ostrzeżeń o wartości null i Zezwalanie na włączenie poszczególnych ostrzeżeń nawet wtedy, gdy są one domyślnie wyłączone:</span><span class="sxs-lookup"><span data-stu-id="7978c-128">`#pragma warning` directives are expanded to allow changing the nullable warning context, and to allow individual warnings to be enabled on even when they're disabled by default:</span></span>

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

<span data-ttu-id="7978c-129">Należy zauważyć, że nowa forma `pragma_warning_body` używa `nullable_action`, a nie `warning_action`.</span><span class="sxs-lookup"><span data-stu-id="7978c-129">Note that the new form of `pragma_warning_body` uses `nullable_action`, not `warning_action`.</span></span>

## <a name="nullable-contexts"></a><span data-ttu-id="7978c-130">Konteksty dopuszczające wartość null</span><span class="sxs-lookup"><span data-stu-id="7978c-130">Nullable contexts</span></span>

<span data-ttu-id="7978c-131">Każdy wiersz kodu źródłowego ma *kontekst adnotacji dopuszczający wartości null* i *kontekst ostrzegawczy do wartości null*.</span><span class="sxs-lookup"><span data-stu-id="7978c-131">Every line of source code has a *nullable annotation context* and a *nullable warning context*.</span></span> <span data-ttu-id="7978c-132">Kontrolują, czy adnotacje dopuszczające wartość null mają wpływ, oraz czy są podane ostrzeżenia o wartości null.</span><span class="sxs-lookup"><span data-stu-id="7978c-132">These control whether nullable annotations have effect, and whether nullability warnings are given.</span></span> <span data-ttu-id="7978c-133">Kontekst adnotacji danego wiersza jest *wyłączony* lub *włączony*.</span><span class="sxs-lookup"><span data-stu-id="7978c-133">The annotation context of a given line is either *disabled* or *enabled*.</span></span> <span data-ttu-id="7978c-134">Kontekst ostrzeżenia danego wiersza jest *wyłączony* lub *włączony*.</span><span class="sxs-lookup"><span data-stu-id="7978c-134">The warning context of a given line is either *disabled* or *enabled*.</span></span>

<span data-ttu-id="7978c-135">Oba konteksty można określić na poziomie projektu (poza kodem C# źródłowym) lub w dowolnym miejscu w pliku źródłowym za pośrednictwem dyrektyw przedprocesora `#nullable`.</span><span class="sxs-lookup"><span data-stu-id="7978c-135">Both contexts can be specified at the project level (outside of C# source code), or anywhere within a source file via `#nullable` pre-processor directives.</span></span> <span data-ttu-id="7978c-136">Jeśli nie zostaną podane żadne ustawienia poziomu projektu, domyślnie dla obu kontekstów należy *wyłączyć*.</span><span class="sxs-lookup"><span data-stu-id="7978c-136">If no project level settings are provided the default is for both contexts to be *disabled*.</span></span>

<span data-ttu-id="7978c-137">Dyrektywa `#nullable` kontroluje kontekst adnotacji i ostrzeżeń w tekście źródłowym i ma pierwszeństwo przed ustawieniami poziomu projektu.</span><span class="sxs-lookup"><span data-stu-id="7978c-137">The `#nullable` directive controls the annotation and warning contexts within the source text, and take precedence over the project-level settings.</span></span>

<span data-ttu-id="7978c-138">Dyrektywa ustawia konteksty, które kontroluje dla kolejnych wierszy kodu, do momentu przesłania przez inną dyrektywę lub do końca pliku źródłowego.</span><span class="sxs-lookup"><span data-stu-id="7978c-138">A directive sets the context(s) it controls for subsequent lines of code, until another directive overrides it, or until the end of the source file.</span></span>

<span data-ttu-id="7978c-139">Efekty dyrektyw są następujące:</span><span class="sxs-lookup"><span data-stu-id="7978c-139">The effect of the directives is as follows:</span></span>

- <span data-ttu-id="7978c-140">`#nullable disable`: określa, że adnotacja dopuszczająca wartość null i konteksty ostrzeżeń mają być *wyłączone*</span><span class="sxs-lookup"><span data-stu-id="7978c-140">`#nullable disable`: Sets the nullable annotation and warning contexts to *disabled*</span></span>
- <span data-ttu-id="7978c-141">`#nullable enable`: ustawia do *włączenia* adnotację z dopuszczaniem wartości null i kontekstów ostrzeżeń</span><span class="sxs-lookup"><span data-stu-id="7978c-141">`#nullable enable`: Sets the nullable annotation and warning contexts to *enabled*</span></span>
- <span data-ttu-id="7978c-142">`#nullable restore`: przywraca ustawienia projektu adnotacji z dopuszczaniem wartości null i ostrzeżeń</span><span class="sxs-lookup"><span data-stu-id="7978c-142">`#nullable restore`: Restores the nullable annotation and warning contexts to project settings</span></span>
- <span data-ttu-id="7978c-143">`#nullable disable annotations`: ustawia kontekst adnotacji dopuszczającej wartość null na *wyłączony*</span><span class="sxs-lookup"><span data-stu-id="7978c-143">`#nullable disable annotations`: Sets the nullable annotation context to *disabled*</span></span>
- <span data-ttu-id="7978c-144">`#nullable enable annotations`: ustawia kontekst adnotacji dopuszczający *enabled* wartość null</span><span class="sxs-lookup"><span data-stu-id="7978c-144">`#nullable enable annotations`: Sets the nullable annotation context to *enabled*</span></span>
- <span data-ttu-id="7978c-145">`#nullable restore annotations`: przywraca ustawienia projektu dla kontekstu adnotacji wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-145">`#nullable restore annotations`: Restores the nullable annotation context to project settings</span></span>
- <span data-ttu-id="7978c-146">`#nullable disable warnings`: ustawia dla *wyłączonego* kontekstu ostrzeżeń wartość null</span><span class="sxs-lookup"><span data-stu-id="7978c-146">`#nullable disable warnings`: Sets the nullable warning context to *disabled*</span></span>
- <span data-ttu-id="7978c-147">`#nullable enable warnings`: ustawia kontekst ostrzegawczy dopuszczający *enabled* wartość null</span><span class="sxs-lookup"><span data-stu-id="7978c-147">`#nullable enable warnings`: Sets the nullable warning context to *enabled*</span></span>
- <span data-ttu-id="7978c-148">`#nullable restore warnings`: przywraca ustawienia projektu dla kontekstu ostrzeżenia wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-148">`#nullable restore warnings`: Restores the nullable warning context to project settings</span></span>

## <a name="nullability-of-types"></a><span data-ttu-id="7978c-149">Wartość zerowa typów</span><span class="sxs-lookup"><span data-stu-id="7978c-149">Nullability of types</span></span>

<span data-ttu-id="7978c-150">Dany typ może mieć jedną z czterech nullabilities: *Oblivious* *, null,* *nullable* i *Unknown*.</span><span class="sxs-lookup"><span data-stu-id="7978c-150">A given type can have one of four nullabilities: *Oblivious*, *nonnullable*, *nullable* and *unknown*.</span></span> 

<span data-ttu-id="7978c-151">*Niedopuszczające wartości null* i *nieznane* typy mogą powodować ostrzeżenia, jeśli do nich zostanie przypisana potencjalna wartość `null`.</span><span class="sxs-lookup"><span data-stu-id="7978c-151">*Nonnullable* and *unknown* types may cause warnings if a potential `null` value is assigned to them.</span></span> <span data-ttu-id="7978c-152">Typy *Oblivious* i *nullable dopuszczają* jednak wartość "*null-przypisanie*" i mogą mieć przypisane wartości `null` bez ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="7978c-152">*Oblivious* and *nullable* types, however, are "*null-assignable*" and can have `null` values assigned to them without warnings.</span></span> 

<span data-ttu-id="7978c-153">Typy *Oblivious* i *niedopuszczające wartości null* mogą zostać wyznaczone lub przypisane bez ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="7978c-153">*Oblivious* and *nonnullable* types can be dereferenced or assigned without warnings.</span></span> <span data-ttu-id="7978c-154">Wartości *dopuszczające wartość null* i *nieznane* typy, jednak są "o*wartości null*" i mogą spowodować ostrzeżenia w przypadku wyróżnienia lub przypisania bez prawidłowej kontroli wartości null.</span><span class="sxs-lookup"><span data-stu-id="7978c-154">Values of *nullable* and *unknown* types, however, are "*null-yielding*" and may cause warnings when dereferenced or assigned without proper null checking.</span></span> 

<span data-ttu-id="7978c-155">*Domyślny stan zerowy* typu zwracanego wartością null to "może mieć wartość null".</span><span class="sxs-lookup"><span data-stu-id="7978c-155">The *default null state* of a null-yielding type is "maybe null".</span></span> <span data-ttu-id="7978c-156">Domyślny stan o wartości null dla niezerowego typu zwracanego to "not null".</span><span class="sxs-lookup"><span data-stu-id="7978c-156">The default null state of a non-null-yielding type is "not null".</span></span>

<span data-ttu-id="7978c-157">Rodzaj typu i kontekst adnotacji dopuszczający wartość null, występujący w ustalaniu jego wartości zerowej:</span><span class="sxs-lookup"><span data-stu-id="7978c-157">The kind of type and the nullable annotation context it occurs in determine its nullability:</span></span>

- <span data-ttu-id="7978c-158">Typ wartości niezerowej `S` jest zawsze *niezerowy*</span><span class="sxs-lookup"><span data-stu-id="7978c-158">A nonnullable value type `S` is always *nonnullable*</span></span>
- <span data-ttu-id="7978c-159">Typ wartości null `S?` jest zawsze *dopuszczany do wartości null*</span><span class="sxs-lookup"><span data-stu-id="7978c-159">A nullable value type `S?` is always *nullable*</span></span>
- <span data-ttu-id="7978c-160">Typ referencyjny bez adnotacji `C` w *wyłączonym* kontekście adnotacji to *Oblivious*</span><span class="sxs-lookup"><span data-stu-id="7978c-160">An unannotated reference type `C` in a *disabled* annotation context is *oblivious*</span></span>
- <span data-ttu-id="7978c-161">Typ referencyjny bez adnotacji `C` w *włączonym* kontekście adnotacji jest *niezerowy*</span><span class="sxs-lookup"><span data-stu-id="7978c-161">An unannotated reference type `C` in an *enabled* annotation context is *nonnullable*</span></span>
- <span data-ttu-id="7978c-162">Typ referencyjny dopuszczający wartość null `C?` *dopuszcza wartość null* (ale ostrzeżenie może być wynikiem *wyłączonego* kontekstu adnotacji)</span><span class="sxs-lookup"><span data-stu-id="7978c-162">A nullable reference type `C?` is *nullable* (but a warning may be yielded in a *disabled* annotation context)</span></span>

<span data-ttu-id="7978c-163">Parametry typu dodatkowo przyjmują ograniczenia do konta:</span><span class="sxs-lookup"><span data-stu-id="7978c-163">Type parameters additionally take their constraints into account:</span></span>

- <span data-ttu-id="7978c-164">Parametr typu `T`, gdzie wszystkie ograniczenia (jeśli istnieją) mają typy zwracające wartość null (*nullable* i *Unknown*) lub ograniczenie `class?` jest *nieznane*</span><span class="sxs-lookup"><span data-stu-id="7978c-164">A type parameter `T` where all constraints (if any) are either null-yielding types (*nullable* and *unknown*) or the `class?` constraint is *unknown*</span></span>
- <span data-ttu-id="7978c-165">Parametr typu `T`, gdzie co najmniej jedno ograniczenie ma wartość *Oblivious* lub nie może *mieć wartości null* albo jeden z `struct` lub `class` ograniczeniami jest</span><span class="sxs-lookup"><span data-stu-id="7978c-165">A type parameter `T` where at least one constraint is either *oblivious* or *nonnullable* or one of the `struct` or `class` constraints is</span></span>
    - <span data-ttu-id="7978c-166">*Oblivious* w *wyłączonym* kontekście adnotacji</span><span class="sxs-lookup"><span data-stu-id="7978c-166">*oblivious* in a *disabled* annotation context</span></span>
    - <span data-ttu-id="7978c-167">Brak możliwości *null* w *włączonym* kontekście adnotacji</span><span class="sxs-lookup"><span data-stu-id="7978c-167">*nonnullable* in an *enabled* annotation context</span></span>
- <span data-ttu-id="7978c-168">Parametr typu dopuszczającego wartość null `T?`, gdzie co najmniej jeden z ograniczeń `T`jest *Oblivious* lub ma *wartość null* lub jeden z ograniczeń `struct` lub `class`, jest</span><span class="sxs-lookup"><span data-stu-id="7978c-168">A nullable type parameter `T?` where at least one of `T`'s constraints is *oblivious* or *nonnullable* or one of the `struct` or `class` constraints, is</span></span>
    - <span data-ttu-id="7978c-169">*dopuszczanie wartości null* w *wyłączonym* kontekście adnotacji (ale jest wyświetlane ostrzeżenie)</span><span class="sxs-lookup"><span data-stu-id="7978c-169">*nullable* in a *disabled* annotation context (but a warning is yielded)</span></span>
    - <span data-ttu-id="7978c-170">*dopuszczanie wartości null* w kontekście *włączonej* adnotacji</span><span class="sxs-lookup"><span data-stu-id="7978c-170">*nullable* in an *enabled* annotation context</span></span>

<span data-ttu-id="7978c-171">Dla parametru typu `T``T?` jest dozwolone tylko wtedy, gdy `T` jest znany typ wartości lub znany jako typ referencyjny.</span><span class="sxs-lookup"><span data-stu-id="7978c-171">For a type parameter `T`, `T?` is only allowed if `T` is known to be a value type or known to be a reference type.</span></span>

### <a name="oblivious-vs-nonnullable"></a><span data-ttu-id="7978c-172">Oblivious vs o wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-172">Oblivious vs nonnullable</span></span>

<span data-ttu-id="7978c-173">`type` jest uznawana za występuje w danym kontekście adnotacji, gdy ostatni token typu znajduje się w tym kontekście.</span><span class="sxs-lookup"><span data-stu-id="7978c-173">A `type` is deemed to occur in a given annotation context when the last token of the type is within that context.</span></span>

<span data-ttu-id="7978c-174">Czy dany typ referencyjny `C` w kodzie źródłowym jest interpretowany jako Oblivious lub niedopuszczający wartości null, zależy od kontekstu adnotacji tego kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="7978c-174">Whether a given reference type `C` in source code is interpreted as oblivious or nonnullable depends on the annotation context of that source code.</span></span> <span data-ttu-id="7978c-175">Jednak po ustanowieniu jest uważany za część tego typu i "podróżuje", np. podczas podstawiania argumentów typu ogólnego.</span><span class="sxs-lookup"><span data-stu-id="7978c-175">But once established, it is considered part of that type, and "travels with it" e.g. during substitution of generic type arguments.</span></span> <span data-ttu-id="7978c-176">Jest tak, jakby Adnotacja, taka jak `?` w typie, ale niewidoczna.</span><span class="sxs-lookup"><span data-stu-id="7978c-176">It is as if there is an annotation like `?` on the type, but invisible.</span></span>

## <a name="constraints"></a><span data-ttu-id="7978c-177">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="7978c-177">Constraints</span></span>

<span data-ttu-id="7978c-178">Typów referencyjnych dopuszczających wartość null można używać jako ograniczeń ogólnych.</span><span class="sxs-lookup"><span data-stu-id="7978c-178">Nullable reference types can be used as generic constraints.</span></span> <span data-ttu-id="7978c-179">Ponadto `object` jest teraz prawidłowym ograniczeniem.</span><span class="sxs-lookup"><span data-stu-id="7978c-179">Furthermore `object` is now valid as an explicit constraint.</span></span> <span data-ttu-id="7978c-180">Brak ograniczenia jest teraz równoznaczne z ograniczeniem `object?` (zamiast `object`), ale (w przeciwieństwie do `object` przed) `object?` nie jest zakazane jako jawne ograniczenie.</span><span class="sxs-lookup"><span data-stu-id="7978c-180">Absence of a constraint is now equivalent to an `object?` constraint (instead of `object`), but (unlike `object` before) `object?` is not prohibited as an explicit constraint.</span></span>

<span data-ttu-id="7978c-181">`class?` jest nowym ograniczeniem wskazującym "możliwy do dołączenia typ referencyjny", podczas gdy `class` oznacza "niezerowy typ odwołania".</span><span class="sxs-lookup"><span data-stu-id="7978c-181">`class?` is a new constraint denoting "possibly nullable reference type", whereas `class` denotes "nonnullable reference type".</span></span>

<span data-ttu-id="7978c-182">Wartość null argumentu typu lub ograniczenia nie ma wpływu na to, czy typ spełnia ograniczenie, chyba że jest to już dziś, (typy wartości null nie spełniają ograniczenia `struct`).</span><span class="sxs-lookup"><span data-stu-id="7978c-182">The nullability of a type argument or of a constraint does not impact whether the type satisfies the constraint, except where that is already the case today (nullable value types do not satisfy the `struct` constraint).</span></span> <span data-ttu-id="7978c-183">Jeśli jednak argument typu nie spełnia wymagań dotyczących wartości NULL ograniczenia, może zostać wyświetlone ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="7978c-183">However, if the type argument does not satisfy the nullability requirements of the constraint, a warning may be given.</span></span>

## <a name="null-state-and-null-tracking"></a><span data-ttu-id="7978c-184">Stan o wartości null i śledzenie wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-184">Null state and null tracking</span></span>

<span data-ttu-id="7978c-185">Każde wyrażenie w podanej lokalizacji źródłowej ma *stan null*, który wskazuje, że istnieje prawdopodobieństwo, że może on zostać obliczony na wartość null.</span><span class="sxs-lookup"><span data-stu-id="7978c-185">Every expression in a given source location has a *null state*, which indicated whether it is believed to potentially evaluate to null.</span></span> <span data-ttu-id="7978c-186">Stan o wartości null ma wartość "not null" lub "może mieć wartość null".</span><span class="sxs-lookup"><span data-stu-id="7978c-186">The null state is either "not null" or "maybe null".</span></span> <span data-ttu-id="7978c-187">Stan null służy do określenia, czy należy podać ostrzeżenie dotyczące konwersji i dereferencji z niebezpiecznymi wartościami null.</span><span class="sxs-lookup"><span data-stu-id="7978c-187">The null state is used to determine whether a warning should be given about null-unsafe conversions and dereferences.</span></span>

### <a name="null-tracking-for-variables"></a><span data-ttu-id="7978c-188">Śledzenie wartości null dla zmiennych</span><span class="sxs-lookup"><span data-stu-id="7978c-188">Null tracking for variables</span></span>

<span data-ttu-id="7978c-189">W przypadku niektórych wyrażeń wskazujących zmienne lub właściwości stan o wartości null jest śledzony między wystąpieniami, na podstawie przypisań do nich, testów wykonywanych na nich i przepływów sterowania między nimi.</span><span class="sxs-lookup"><span data-stu-id="7978c-189">For certain expressions denoting variables or properties, the null state is tracked between occurrences, based on assignments to them, tests performed on them and the control flow between them.</span></span> <span data-ttu-id="7978c-190">Jest to podobne do tego, jak określone przypisanie jest śledzone dla zmiennych.</span><span class="sxs-lookup"><span data-stu-id="7978c-190">This is similar to how definite assignment is tracked for variables.</span></span> <span data-ttu-id="7978c-191">Śledzone wyrażenia są tymi, które są następujące:</span><span class="sxs-lookup"><span data-stu-id="7978c-191">The tracked expressions are the ones of the following form:</span></span>

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

<span data-ttu-id="7978c-192">Gdzie są identyfikatory pól lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="7978c-192">Where the identifiers denote fields or properties.</span></span>

<span data-ttu-id="7978c-193">***Opisz przejścia stanu o wartości null podobne do określonego przypisania***</span><span class="sxs-lookup"><span data-stu-id="7978c-193">***Describe null state transitions similar to definite assignment***</span></span>

### <a name="null-state-for-expressions"></a><span data-ttu-id="7978c-194">Stan zerowy dla wyrażeń</span><span class="sxs-lookup"><span data-stu-id="7978c-194">Null state for expressions</span></span>

<span data-ttu-id="7978c-195">Stan o wartości null wyrażenia jest pochodną jego formy i typu oraz od wartości null zmiennych, w których to dotyczy.</span><span class="sxs-lookup"><span data-stu-id="7978c-195">The null state of an expression is derived from its form and type, and from the null state of variables involved in it.</span></span>

### <a name="literals"></a><span data-ttu-id="7978c-196">Literały</span><span class="sxs-lookup"><span data-stu-id="7978c-196">Literals</span></span>

<span data-ttu-id="7978c-197">Stan null literału `null` ma wartość "null".</span><span class="sxs-lookup"><span data-stu-id="7978c-197">The null state of a `null` literal is "maybe null".</span></span> <span data-ttu-id="7978c-198">Stan null literału `default`, który jest konwertowany na typ, który nie jest typem wartości o wartości null, jest "może mieć wartość null".</span><span class="sxs-lookup"><span data-stu-id="7978c-198">The null state of a `default` literal that is being converted to a type that is known not to be a nonnullable value type is "maybe null".</span></span> <span data-ttu-id="7978c-199">Stan null dowolnego innego literału to "not null".</span><span class="sxs-lookup"><span data-stu-id="7978c-199">The null state of any other literal is "not null".</span></span>

### <a name="simple-names"></a><span data-ttu-id="7978c-200">Proste nazwy</span><span class="sxs-lookup"><span data-stu-id="7978c-200">Simple names</span></span>

<span data-ttu-id="7978c-201">Jeśli `simple_name` nie jest sklasyfikowana jako wartość, jej stanem null jest "not null".</span><span class="sxs-lookup"><span data-stu-id="7978c-201">If a `simple_name` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="7978c-202">W przeciwnym razie jest to śledzone wyrażenie, a jego stan null jest jego śledzony stan o wartości null w tej lokalizacji źródłowej.</span><span class="sxs-lookup"><span data-stu-id="7978c-202">Otherwise it is a tracked expression, and its null state is its tracked null state at this source location.</span></span>

### <a name="member-access"></a><span data-ttu-id="7978c-203">Dostęp do elementu członkowskiego</span><span class="sxs-lookup"><span data-stu-id="7978c-203">Member access</span></span>

<span data-ttu-id="7978c-204">Jeśli `member_access` nie jest sklasyfikowana jako wartość, jej stanem null jest "not null".</span><span class="sxs-lookup"><span data-stu-id="7978c-204">If a `member_access` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="7978c-205">W przeciwnym razie, jeśli jest to śledzone wyrażenie, jego stan o wartości null jest jego śledzonym stanem null w tej lokalizacji źródłowej.</span><span class="sxs-lookup"><span data-stu-id="7978c-205">Otherwise, if it is a tracked expression, its null state is its tracked null state at this source location.</span></span> <span data-ttu-id="7978c-206">W przeciwnym razie stan null jest domyślnym stanem null jego typu.</span><span class="sxs-lookup"><span data-stu-id="7978c-206">Otherwise its null state is the default null state of its type.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="7978c-207">Wyrażenia wywołania</span><span class="sxs-lookup"><span data-stu-id="7978c-207">Invocation expressions</span></span>

<span data-ttu-id="7978c-208">Jeśli `invocation_expression` wywołuje składową, która jest zadeklarowana z co najmniej jednym atrybutem dla specjalnego zachowania null, stan null jest określany przez te atrybuty.</span><span class="sxs-lookup"><span data-stu-id="7978c-208">If an `invocation_expression` invokes a member that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="7978c-209">W przeciwnym razie stan null wyrażenia jest domyślnym stanem null jego typu.</span><span class="sxs-lookup"><span data-stu-id="7978c-209">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="element-access"></a><span data-ttu-id="7978c-210">Dostęp do elementu</span><span class="sxs-lookup"><span data-stu-id="7978c-210">Element access</span></span>

<span data-ttu-id="7978c-211">Jeśli `element_access` wywoła indeksator, który jest zadeklarowany z co najmniej jednym atrybutem dla specjalnego zachowania null, stan null jest określany przez te atrybuty.</span><span class="sxs-lookup"><span data-stu-id="7978c-211">If an `element_access` invokes an indexer that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="7978c-212">W przeciwnym razie stan null wyrażenia jest domyślnym stanem null jego typu.</span><span class="sxs-lookup"><span data-stu-id="7978c-212">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="base-access"></a><span data-ttu-id="7978c-213">Dostęp podstawowy</span><span class="sxs-lookup"><span data-stu-id="7978c-213">Base access</span></span>

<span data-ttu-id="7978c-214">Jeśli `B` oznacza typ podstawowy typu otaczającego, `base.I` ma taki sam stan null jak `((B)this).I`, a `base[E]` ma ten sam stan null jak `((B)this)[E]`.</span><span class="sxs-lookup"><span data-stu-id="7978c-214">If `B` denotes the base type of the enclosing type, `base.I` has the same null state as `((B)this).I` and `base[E]` has the same null state as `((B)this)[E]`.</span></span>

### <a name="default-expressions"></a><span data-ttu-id="7978c-215">Wyrażenia domyślne</span><span class="sxs-lookup"><span data-stu-id="7978c-215">Default expressions</span></span>

<span data-ttu-id="7978c-216">`default(T)` ma stan null "non-null", jeśli `T` jest znany jako typ wartości o wartości null.</span><span class="sxs-lookup"><span data-stu-id="7978c-216">`default(T)` has the null state "non-null" if `T` is known to be a nonnullable value type.</span></span> <span data-ttu-id="7978c-217">W przeciwnym razie ma stan null "może mieć wartość null".</span><span class="sxs-lookup"><span data-stu-id="7978c-217">Otherwise it has the null state "maybe null".</span></span>

### <a name="null-conditional-expressions"></a><span data-ttu-id="7978c-218">Wyrażenia warunkowe o wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-218">Null-conditional expressions</span></span>

<span data-ttu-id="7978c-219">`null_conditional_expression` ma stan null "może mieć wartość null".</span><span class="sxs-lookup"><span data-stu-id="7978c-219">A `null_conditional_expression` has the null state "maybe null".</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="7978c-220">Wyrażenia rzutowania</span><span class="sxs-lookup"><span data-stu-id="7978c-220">Cast expressions</span></span>

<span data-ttu-id="7978c-221">Jeśli wyrażenie rzutowania `(T)E` wywołuje konwersję zdefiniowaną przez użytkownika, stan null wyrażenia jest domyślnym stanem null dla tego typu.</span><span class="sxs-lookup"><span data-stu-id="7978c-221">If a cast expression `(T)E` invokes a user-defined conversion, then the null state of the expression is the default null state for its type.</span></span> <span data-ttu-id="7978c-222">W przeciwnym razie, jeśli `T` ma wartość null (*nullable* lub *Unknown*), oznacza to, że wartość zerowa to "może mieć wartość null".</span><span class="sxs-lookup"><span data-stu-id="7978c-222">Otherwise, if `T` is null-yielding (*nullable* or *unknown*) then the null state is "maybe null".</span></span> <span data-ttu-id="7978c-223">W przeciwnym razie stan null jest taki sam jak stan null `E`.</span><span class="sxs-lookup"><span data-stu-id="7978c-223">Otherwise the null state is the same as the null state of `E`.</span></span>

### <a name="await-expressions"></a><span data-ttu-id="7978c-224">Wyrażenia await</span><span class="sxs-lookup"><span data-stu-id="7978c-224">Await expressions</span></span>

<span data-ttu-id="7978c-225">Stan null `await E` jest domyślnym stanem null jego typu.</span><span class="sxs-lookup"><span data-stu-id="7978c-225">The null state of `await E` is the default null state of its type.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="7978c-226">Operator `as`</span><span class="sxs-lookup"><span data-stu-id="7978c-226">The `as` operator</span></span>

<span data-ttu-id="7978c-227">Wyrażenie `as` ma stan null "może mieć wartość null".</span><span class="sxs-lookup"><span data-stu-id="7978c-227">An `as` expression has the null state "maybe null".</span></span>

### <a name="the-null-coalescing-operator"></a><span data-ttu-id="7978c-228">Operator łączenia wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-228">The null-coalescing operator</span></span>

<span data-ttu-id="7978c-229">`E1 ?? E2` ma ten sam stan zerowy co `E2`</span><span class="sxs-lookup"><span data-stu-id="7978c-229">`E1 ?? E2` has the same null state as `E2`</span></span>

### <a name="the-conditional-operator"></a><span data-ttu-id="7978c-230">Operator warunkowy</span><span class="sxs-lookup"><span data-stu-id="7978c-230">The conditional operator</span></span>

<span data-ttu-id="7978c-231">Stan o wartości null `E1 ? E2 : E3` ma wartość "not null", jeśli stan null obu `E2` i `E3` to "not null".</span><span class="sxs-lookup"><span data-stu-id="7978c-231">The null state of `E1 ? E2 : E3` is "not null" if the null state of both `E2` and `E3` are "not null".</span></span> <span data-ttu-id="7978c-232">W przeciwnym razie jest to "może mieć wartość null".</span><span class="sxs-lookup"><span data-stu-id="7978c-232">Otherwise it is "maybe null".</span></span>

### <a name="query-expressions"></a><span data-ttu-id="7978c-233">Wyrażenia zapytań</span><span class="sxs-lookup"><span data-stu-id="7978c-233">Query expressions</span></span>

<span data-ttu-id="7978c-234">Stan null wyrażenia zapytania jest domyślnym stanem null jego typu.</span><span class="sxs-lookup"><span data-stu-id="7978c-234">The null state of a query expression is the default null state of its type.</span></span>

### <a name="assignment-operators"></a><span data-ttu-id="7978c-235">Operatory przypisania</span><span class="sxs-lookup"><span data-stu-id="7978c-235">Assignment operators</span></span>

<span data-ttu-id="7978c-236">`E1 = E2` i `E1 op= E2` mają taki sam stan null jak `E2` po zastosowaniu niejawnych konwersji.</span><span class="sxs-lookup"><span data-stu-id="7978c-236">`E1 = E2` and `E1 op= E2` have the same null state as `E2` after any implicit conversions have been applied.</span></span>

### <a name="unary-and-binary-operators"></a><span data-ttu-id="7978c-237">Operatory jednoargumentowe i binarne</span><span class="sxs-lookup"><span data-stu-id="7978c-237">Unary and binary operators</span></span>

<span data-ttu-id="7978c-238">Jeśli operator jednoargumentowy lub dwuargumentowy wywołuje zdefiniowany przez użytkownika operator, który jest zadeklarowany z co najmniej jednym atrybutem dla specjalnego zachowania null, stan null jest określany przez te atrybuty.</span><span class="sxs-lookup"><span data-stu-id="7978c-238">If a unary or binary operator invokes an user-defined operator that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="7978c-239">W przeciwnym razie stan null wyrażenia jest domyślnym stanem null jego typu.</span><span class="sxs-lookup"><span data-stu-id="7978c-239">Otherwise the null state of the expression is the default null state of its type.</span></span>

<span data-ttu-id="7978c-240">***Coś specjalnego do wykonywania w przypadku `+` binarnych za pośrednictwem ciągów i delegatów?***</span><span class="sxs-lookup"><span data-stu-id="7978c-240">***Something special to do for binary `+` over strings and delegates?***</span></span>

### <a name="expressions-that-propagate-null-state"></a><span data-ttu-id="7978c-241">Wyrażenia propagowania stanu o wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-241">Expressions that propagate null state</span></span>

<span data-ttu-id="7978c-242">`(E)`, `checked(E)` i `unchecked(E)` wszystkie mają taki sam stan null jak `E`.</span><span class="sxs-lookup"><span data-stu-id="7978c-242">`(E)`, `checked(E)` and `unchecked(E)` all have the same null state as `E`.</span></span>

### <a name="expressions-that-are-never-null"></a><span data-ttu-id="7978c-243">Wyrażenia, które nigdy nie mają wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-243">Expressions that are never null</span></span>

<span data-ttu-id="7978c-244">Stan zerowy następujących formularzy wyrażeń ma zawsze wartość "not null":</span><span class="sxs-lookup"><span data-stu-id="7978c-244">The null state of the following expression forms is always "not null":</span></span>

- <span data-ttu-id="7978c-245">dostęp `this`</span><span class="sxs-lookup"><span data-stu-id="7978c-245">`this` access</span></span>
- <span data-ttu-id="7978c-246">ciągi interpolowane</span><span class="sxs-lookup"><span data-stu-id="7978c-246">interpolated strings</span></span>
- <span data-ttu-id="7978c-247">wyrażenia `new` (obiekt, delegat, obiekt anonimowy i wyrażenia tworzenia tablicy)</span><span class="sxs-lookup"><span data-stu-id="7978c-247">`new` expressions (object, delegate, anonymous object and array creation expressions)</span></span>
- <span data-ttu-id="7978c-248">wyrażenia `typeof`</span><span class="sxs-lookup"><span data-stu-id="7978c-248">`typeof` expressions</span></span>
- <span data-ttu-id="7978c-249">wyrażenia `nameof`</span><span class="sxs-lookup"><span data-stu-id="7978c-249">`nameof` expressions</span></span>
- <span data-ttu-id="7978c-250">funkcje anonimowe (anonimowe metody i wyrażenia lambda)</span><span class="sxs-lookup"><span data-stu-id="7978c-250">anonymous functions (anonymous methods and lambda expressions)</span></span>
- <span data-ttu-id="7978c-251">wyrażenia łagodniejszej o wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-251">null-forgiving expressions</span></span>
- <span data-ttu-id="7978c-252">wyrażenia `is`</span><span class="sxs-lookup"><span data-stu-id="7978c-252">`is` expressions</span></span>

## <a name="type-inference"></a><span data-ttu-id="7978c-253">Wnioskowanie o typie</span><span class="sxs-lookup"><span data-stu-id="7978c-253">Type inference</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="7978c-254">Wnioskowanie o typie dla `var`</span><span class="sxs-lookup"><span data-stu-id="7978c-254">Type inference for `var`</span></span>

<span data-ttu-id="7978c-255">Typ wywnioskowany dla zmiennych lokalnych zadeklarowanych za pomocą `var` jest informowany o stanie null wyrażenia inicjowania.</span><span class="sxs-lookup"><span data-stu-id="7978c-255">The type inferred for local variables declared with `var` is informed by the null state of the initializing expression.</span></span>

```csharp
var x = E;
```

<span data-ttu-id="7978c-256">Jeśli typ `E` jest typem referencyjnym dopuszczającym wartość null `C?` a stan null `E` ma wartość "not null", typ wywnioskowany dla `x` to `C`.</span><span class="sxs-lookup"><span data-stu-id="7978c-256">If the type of `E` is a nullable reference type `C?` and the null state of `E` is "not null" then the type inferred for `x` is `C`.</span></span> <span data-ttu-id="7978c-257">W przeciwnym razie typ wnioskowany jest typem `E`.</span><span class="sxs-lookup"><span data-stu-id="7978c-257">Otherwise, the inferred type is the type of `E`.</span></span>

<span data-ttu-id="7978c-258">Wartość null typu wywnioskowanego dla `x` jest określana zgodnie z opisem powyżej, na podstawie kontekstu adnotacji `var`, podobnie jak w przypadku, gdy typ został jawnie określony w tym położeniu.</span><span class="sxs-lookup"><span data-stu-id="7978c-258">The nullability of the type inferred for `x` is determined as described above, based on the annotation context of the `var`, just as if the type had been given explicitly in that position.</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="7978c-259">Wnioskowanie o typie dla `var?`</span><span class="sxs-lookup"><span data-stu-id="7978c-259">Type inference for `var?`</span></span>

<span data-ttu-id="7978c-260">Typ wywnioskowany dla zmiennych lokalnych zadeklarowanych za pomocą `var?` jest niezależny od stanu null wyrażenia inicjowania.</span><span class="sxs-lookup"><span data-stu-id="7978c-260">The type inferred for local variables declared with `var?` is independent of the null state of the initializing expression.</span></span>

```csharp
var? x = E;
```

<span data-ttu-id="7978c-261">Jeśli typ `T` `E` jest typem wartości null lub typem referencyjnym dopuszczającym wartość null, wówczas typ, który został wywnioskowany dla `x` jest `T`.</span><span class="sxs-lookup"><span data-stu-id="7978c-261">If the type `T` of `E` is a nullable value type or a nullable reference type then the type inferred for `x` is `T`.</span></span> <span data-ttu-id="7978c-262">W przeciwnym razie, jeśli `T` jest typem wartości o wartości null `S` wywnioskowanym typem jest `S?`.</span><span class="sxs-lookup"><span data-stu-id="7978c-262">Otherwise, if `T` is a nonnullable value type `S` the type inferred is `S?`.</span></span> <span data-ttu-id="7978c-263">W przeciwnym razie, jeśli `T` jest typem odwołania o wartości null, `C` wnioskowany typ jest `C?`.</span><span class="sxs-lookup"><span data-stu-id="7978c-263">Otherwise, if `T` is a nonnullable reference type `C` the type inferred is `C?`.</span></span> <span data-ttu-id="7978c-264">W przeciwnym razie deklaracja jest niedozwolona.</span><span class="sxs-lookup"><span data-stu-id="7978c-264">Otherwise, the declaration is illegal.</span></span>

<span data-ttu-id="7978c-265">Wartość null typu wywnioskowanego dla `x` zawsze *dopuszcza wartość null*.</span><span class="sxs-lookup"><span data-stu-id="7978c-265">The nullability of the type inferred for `x` is always *nullable*.</span></span>

### <a name="generic-type-inference"></a><span data-ttu-id="7978c-266">Wnioskowanie o typie ogólnym</span><span class="sxs-lookup"><span data-stu-id="7978c-266">Generic type inference</span></span>

<span data-ttu-id="7978c-267">Wnioskowanie o typie ogólnym jest rozszerzane, aby ułatwić podjęcie decyzji, czy wywnioskowane typy odwołań powinny mieć wartość null, czy nie.</span><span class="sxs-lookup"><span data-stu-id="7978c-267">Generic type inference is enhanced to help decide whether inferred reference types should be nullable or not.</span></span> <span data-ttu-id="7978c-268">Jest to najlepszy nakład pracy, a nie w i sama sama z nich, ale może prowadzić do dopuszczających wartość null ostrzeżeń, gdy wywnioskowane typy wybranych przeciążeń są stosowane do argumentów.</span><span class="sxs-lookup"><span data-stu-id="7978c-268">This is a best effort, and does not in and of itself yield warnings, but may lead to nullable warnings when the inferred types of the selected overload are applied to the arguments.</span></span>

<span data-ttu-id="7978c-269">Wnioskowanie o typie nie polega na kontekście adnotacji typów przychodzących.</span><span class="sxs-lookup"><span data-stu-id="7978c-269">The type inference does not rely on the annotation context of incoming types.</span></span> <span data-ttu-id="7978c-270">Zamiast tego zostanie wywnioskowany `type`, który uzyskuje własny kontekst adnotacji, w którym "byłby", jeśli został on jawnie wyrażony.</span><span class="sxs-lookup"><span data-stu-id="7978c-270">Instead a `type` is inferred which acquires its own annotation context from where it "would have been" if it had been expressed explicitly.</span></span> <span data-ttu-id="7978c-271">Jest to podkreślenie roli typu wnioskowania jako wygoda do tego, co można napisać samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="7978c-271">This underscores the role of type inference as a convenience for what you could have written yourself.</span></span>

<span data-ttu-id="7978c-272">Dokładniej, kontekst adnotacji dla argumentu wywnioskowanego typu jest kontekstem tokenu, który miał być poprzedzony listą parametrów typu `<...>`. tj. nazwa wywoływanej metody ogólnej.</span><span class="sxs-lookup"><span data-stu-id="7978c-272">More precisely, the annotation context for an inferred type argument is the context of the token that would have been followed by the `<...>` type parameter list, had there been one; i.e. the name of the generic method being called.</span></span> <span data-ttu-id="7978c-273">Dla wyrażeń zapytania, które są tłumaczone na takie wywołania, kontekst jest pobierany z początkowego słowa kluczowego kontekstowego klauzuli zapytania, z którego jest generowane wywołanie.</span><span class="sxs-lookup"><span data-stu-id="7978c-273">For query expressions that translate to such calls, the context is taken from the initial contextual keyword of the query clause from which the call is generated.</span></span>

### <a name="the-first-phase"></a><span data-ttu-id="7978c-274">Pierwsza faza</span><span class="sxs-lookup"><span data-stu-id="7978c-274">The first phase</span></span>

<span data-ttu-id="7978c-275">Typy odwołań dopuszczające wartość null są przepływem do granic z wyrażeń początkowych, jak opisano poniżej.</span><span class="sxs-lookup"><span data-stu-id="7978c-275">Nullable reference types flow into the bounds from the initial expressions, as described below.</span></span> <span data-ttu-id="7978c-276">Ponadto wprowadzono dwa nowe rodzaje granic, mianowicie `null` i `default`.</span><span class="sxs-lookup"><span data-stu-id="7978c-276">In addition, two new kinds of bounds, namely `null` and `default` are introduced.</span></span> <span data-ttu-id="7978c-277">Ich celem jest przechodzenie między wystąpieniami `null` lub `default` w wyrażeniach wejściowych, które mogą spowodować, że typ wnioskowany będzie dopuszczać wartości null, nawet wtedy, gdy nie byłoby w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="7978c-277">Their purpose is to carry through occurrences of `null` or `default` in the input expressions, which may cause an inferred type to be nullable, even when it otherwise wouldn't.</span></span> <span data-ttu-id="7978c-278">Działa to nawet w przypadku typów *wartości* null, które są rozszerzone w celu wybrania "wartości null" w procesie wnioskowania.</span><span class="sxs-lookup"><span data-stu-id="7978c-278">This works even for nullable *value* types, which are enhanced to pick up "nullness" in the inference process.</span></span>

<span data-ttu-id="7978c-279">Określenie, które granice dodać w pierwszej fazie, zostało ulepszone w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="7978c-279">The determination of what bounds to add in the first phase are enhanced as follows:</span></span>

<span data-ttu-id="7978c-280">Jeśli argument `Ei` ma typ referencyjny, `U` typ używany do wnioskowania zależy od stanu null `Ei` oraz jego zadeklarowanego typu:</span><span class="sxs-lookup"><span data-stu-id="7978c-280">If an argument `Ei` has a reference type, the type `U` used for inference depends on the null state of `Ei` as well as its declared type:</span></span>
- <span data-ttu-id="7978c-281">Jeśli zadeklarowany typ to typ referencyjny niebędący wartością null `U0` lub typ referencyjny dopuszczający wartość null `U0?`</span><span class="sxs-lookup"><span data-stu-id="7978c-281">If the declared type is a nonnullable reference type `U0` or a nullable reference type `U0?` then</span></span>
    - <span data-ttu-id="7978c-282">Jeśli stan null `Ei` ma wartość "not null", wówczas `U` jest `U0`</span><span class="sxs-lookup"><span data-stu-id="7978c-282">if the null state of `Ei` is "not null" then `U` is `U0`</span></span>
    - <span data-ttu-id="7978c-283">Jeśli stan null `Ei` ma wartość "null", wówczas `U` jest `U0?`</span><span class="sxs-lookup"><span data-stu-id="7978c-283">if the null state of `Ei` is "maybe null" then `U` is `U0?`</span></span>
- <span data-ttu-id="7978c-284">W przeciwnym razie, jeśli `Ei` ma zadeklarowany typ, `U` jest tego typu</span><span class="sxs-lookup"><span data-stu-id="7978c-284">Otherwise if `Ei` has a declared type, `U` is that type</span></span>
- <span data-ttu-id="7978c-285">W przeciwnym razie, jeśli `Ei` jest `null`, `U` jest to granica specjalna `null`</span><span class="sxs-lookup"><span data-stu-id="7978c-285">Otherwise if `Ei` is `null` then `U` is the special bound `null`</span></span>
- <span data-ttu-id="7978c-286">W przeciwnym razie, jeśli `Ei` jest `default`, `U` jest to granica specjalna `default`</span><span class="sxs-lookup"><span data-stu-id="7978c-286">Otherwise if `Ei` is `default` then `U` is the special bound `default`</span></span>
- <span data-ttu-id="7978c-287">W przeciwnym razie nie wprowadzono wnioskowania.</span><span class="sxs-lookup"><span data-stu-id="7978c-287">Otherwise no inference is made.</span></span>

### <a name="exact-upper-bound-and-lower-bound-inferences"></a><span data-ttu-id="7978c-288">Dokładne, górne i dolne ograniczenia wnioskowania</span><span class="sxs-lookup"><span data-stu-id="7978c-288">Exact, upper-bound and lower-bound inferences</span></span>

<span data-ttu-id="7978c-289">W wnioskach *z* typu `U` *do* typu `V`, jeśli `V` jest typem referencyjnym null `V0?`, wówczas `V0` jest używany zamiast `V` w poniższych klauzulach.</span><span class="sxs-lookup"><span data-stu-id="7978c-289">In inferences *from* the type `U` *to* the type `V`, if `V` is a nullable reference type `V0?`, then `V0` is used instead of `V` in the following clauses.</span></span>
- <span data-ttu-id="7978c-290">Jeśli `V` jest jedną z nieustalonych zmiennych typu, `U` zostanie dodana jako dokładne, górne lub dolne powiązana jak wcześniej</span><span class="sxs-lookup"><span data-stu-id="7978c-290">If `V` is one of the unfixed type variables, `U` is added as an exact, upper or lower bound as before</span></span>
- <span data-ttu-id="7978c-291">W przeciwnym razie, jeśli `U` jest `null` lub `default`, nie zostaną wykonane żadne wnioskowanie</span><span class="sxs-lookup"><span data-stu-id="7978c-291">Otherwise, if `U` is `null` or `default`, no inference is made</span></span>
- <span data-ttu-id="7978c-292">W przeciwnym razie, jeśli `U` jest typem referencyjnym dopuszczającym wartość null `U0?`, wówczas `U0` jest używany zamiast `U` w kolejnych klauzulach.</span><span class="sxs-lookup"><span data-stu-id="7978c-292">Otherwise, if `U` is a nullable reference type `U0?`, then `U0` is used instead of `U` in the subsequent clauses.</span></span>

<span data-ttu-id="7978c-293">Jest to, że wartość null odnosząca się bezpośrednio do jednej z nieustalonych zmiennych typu jest zachowywana w granicach.</span><span class="sxs-lookup"><span data-stu-id="7978c-293">The essence is that nullability that pertains directly to one of the unfixed type variables is preserved into its bounds.</span></span> <span data-ttu-id="7978c-294">W odniesieniu do wniosków, które powtarzają się w przypadku typów źródłowych i docelowych, z drugiej strony, wartość null jest ignorowana.</span><span class="sxs-lookup"><span data-stu-id="7978c-294">For the inferences that recurse further into the source and target types, on the other hand, nullability is ignored.</span></span> <span data-ttu-id="7978c-295">Może być niezgodny, ale jeśli nie, ostrzeżenie zostanie wystawione później w przypadku wybrania i zastosowania przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="7978c-295">It may or may not match, but if it doesn't, a warning will be issued later if the overload is chosen and applied.</span></span>

### <a name="fixing"></a><span data-ttu-id="7978c-296">Mocowanie</span><span class="sxs-lookup"><span data-stu-id="7978c-296">Fixing</span></span>

<span data-ttu-id="7978c-297">Obecnie nie jest to dobre zadanie opisywania, co się dzieje, gdy wiele granic jest do siebie konwertowane tożsamości, ale różnią się.</span><span class="sxs-lookup"><span data-stu-id="7978c-297">The spec currently does not do a good job of describing what happens when multiple bounds are identity convertible to each other, but are different.</span></span> <span data-ttu-id="7978c-298">Może się to zdarzyć między `object` i `dynamic`, między typami krotek, które różnią się tylko nazwami elementów, między typami konstruowanymi, a teraz między `C` i `C?` dla typów referencyjnych.</span><span class="sxs-lookup"><span data-stu-id="7978c-298">This may happen between `object` and `dynamic`, between tuple types that differ only in element names, between types constructed thereof and now also between `C` and `C?` for reference types.</span></span>

<span data-ttu-id="7978c-299">Ponadto musimy propagować "wartość null" z wyrażeń wejściowych do typu wyniku.</span><span class="sxs-lookup"><span data-stu-id="7978c-299">In addition we need to propagate "nullness" from the input expressions to the result type.</span></span> 

<span data-ttu-id="7978c-300">Aby obsłużyć te etapy, możemy dodać więcej faz, które są teraz:</span><span class="sxs-lookup"><span data-stu-id="7978c-300">To handle these we add more phases to fixing, which is now:</span></span>

1. <span data-ttu-id="7978c-301">Zbierz wszystkie typy we wszystkich granicach jako kandydaci, usuwając `?` ze wszystkich, które są dopuszczającymi wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-301">Gather all the types in all the bounds as candidates, removing `?` from all that are nullable reference types</span></span>
2. <span data-ttu-id="7978c-302">Eliminowanie kandydatów na podstawie wymagań ścisłych, dolnych i górnych granic (utrzymywanie granic `null` i `default`)</span><span class="sxs-lookup"><span data-stu-id="7978c-302">Eliminate candidates based on requirements of exact, lower and upper bounds (keeping `null` and `default` bounds)</span></span>
3. <span data-ttu-id="7978c-303">Eliminowanie kandydatów, które nie mają niejawnej konwersji na wszystkich innych kandydatów</span><span class="sxs-lookup"><span data-stu-id="7978c-303">Eliminate candidates that do not have an implicit conversion to all the other candidates</span></span>
4. <span data-ttu-id="7978c-304">Jeśli pozostałe kandydaci nie wszystkie mają do siebie konwersje tożsamości, wówczas wnioskowanie o typie kończy się niepowodzeniem</span><span class="sxs-lookup"><span data-stu-id="7978c-304">If the remaining candidates do not all have identity conversions to one another, then type inference fails</span></span>
5. <span data-ttu-id="7978c-305">*Scal* pozostałe kandydaci zgodnie z poniższym opisem</span><span class="sxs-lookup"><span data-stu-id="7978c-305">*Merge* the remaining candidates as described below</span></span>
6. <span data-ttu-id="7978c-306">Jeśli wynikiem jest typ referencyjny lub typ wartości, która ma wartość null, a *wszystkie* dokładne granice lub *dowolne* dolne granice są typami wartości null, typem referencyjnym dopuszczającym wartość null, `null` lub `default`, wówczas `?` jest dodawane do uzyskanego kandydata, co oznacza, że jest to typ wartości null lub typ referencyjny.</span><span class="sxs-lookup"><span data-stu-id="7978c-306">If the resulting candidate is a reference type or a nonnullable value type and *all* of the exact bounds or *any* of the lower bounds are nullable value types, nullable reference types, `null` or `default`, then `?` is added to the resulting candidate, making it a nullable value type or reference type.</span></span>

<span data-ttu-id="7978c-307">*Scalanie* jest opisane między dwoma typami kandydatów.</span><span class="sxs-lookup"><span data-stu-id="7978c-307">*Merging* is described between two candidate types.</span></span> <span data-ttu-id="7978c-308">Jest on przechodni i komutatywna, więc kandydaci mogą być scalane w dowolnej kolejności z tym samym ostatecznym wynikiem.</span><span class="sxs-lookup"><span data-stu-id="7978c-308">It is transitive and commutative, so the candidates can be merged in any order with the same ultimate result.</span></span> <span data-ttu-id="7978c-309">Ta wartość jest niezdefiniowana, jeśli dwa typy kandydatów nie są do siebie konwertowane.</span><span class="sxs-lookup"><span data-stu-id="7978c-309">It is undefined if the two candidate types are not identity convertible to each other.</span></span>

<span data-ttu-id="7978c-310">Funkcja *merge* przyjmuje dwa typy kandydujące i kierunek ( *+* lub *-* ):</span><span class="sxs-lookup"><span data-stu-id="7978c-310">The *Merge* function takes two candidate types and a direction (*+* or *-*):</span></span>

- <span data-ttu-id="7978c-311">*Scal*(`T`, `T`, *d*) = t</span><span class="sxs-lookup"><span data-stu-id="7978c-311">*Merge*(`T`, `T`, *d*) = T</span></span>
- <span data-ttu-id="7978c-312">*Scal*(`S`, `T?`, *+* ) = *merge*(`S?`, `T`, *+)* = *merge*(`S`, `T`, *+* )`?`</span><span class="sxs-lookup"><span data-stu-id="7978c-312">*Merge*(`S`, `T?`, *+*) = *Merge*(`S?`, `T`, *+*) = *Merge*(`S`, `T`, *+*)`?`</span></span>
- <span data-ttu-id="7978c-313">*Scal*(`S`, `T?`, *-* ) = *merge*(`S?`, *`T`,-)* = *merge*(`S`, `T`, *-* )</span><span class="sxs-lookup"><span data-stu-id="7978c-313">*Merge*(`S`, `T?`, *-*) = *Merge*(`S?`, `T`, *-*) = *Merge*(`S`, `T`, *-*)</span></span>
- <span data-ttu-id="7978c-314">*Scal*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+* ) = `C<`*scalanie*(`S1`, `T1`, *D1*)`,...,`*scalanie*(`Sn`, `Tn`, *DN*)`>`, *gdzie*</span><span class="sxs-lookup"><span data-stu-id="7978c-314">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="7978c-315">`di` =  *+* , jeśli `i`parametr typu `C<...>` jest współwariantem</span><span class="sxs-lookup"><span data-stu-id="7978c-315">`di` = *+* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="7978c-316">`di` =  *-* , jeśli `i`parametr typu `C<...>` jest antylub niezmienny</span><span class="sxs-lookup"><span data-stu-id="7978c-316">`di` = *-* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="7978c-317">*Scal*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-* ) = `C<`*scalanie*(`S1`, `T1`, *D1*)`,...,`*scalanie*(`Sn`, `Tn`, *DN*)`>`, *gdzie*</span><span class="sxs-lookup"><span data-stu-id="7978c-317">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="7978c-318">`di` =  *-* , jeśli `i`parametr typu `C<...>` jest współwariantem</span><span class="sxs-lookup"><span data-stu-id="7978c-318">`di` = *-* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="7978c-319">`di` =  *+* , jeśli `i`parametr typu `C<...>` jest antylub niezmienny</span><span class="sxs-lookup"><span data-stu-id="7978c-319">`di` = *+* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="7978c-320">*Scal*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*merge*(`S1`, `T1`, *d*)`n1,...,`*scalanie*(`Sn`, `Tn`, *d*) `nn)`, *gdzie*</span><span class="sxs-lookup"><span data-stu-id="7978c-320">*Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*Merge*(`S1`, `T1`, *d*)`n1,...,`*Merge*(`Sn`, `Tn`, *d*) `nn)`, *where*</span></span>
    - <span data-ttu-id="7978c-321">`ni` jest nieobecny, jeśli `si` i `ti` się różnić lub jeśli oba nie są obecne</span><span class="sxs-lookup"><span data-stu-id="7978c-321">`ni` is absent if `si` and `ti` differ, or if both are absent</span></span>
    - <span data-ttu-id="7978c-322">`ni` jest `si`, jeśli `si` i `ti` są takie same</span><span class="sxs-lookup"><span data-stu-id="7978c-322">`ni` is `si` if `si` and `ti` are the same</span></span>
- <span data-ttu-id="7978c-323">*Scal*(`object`, `dynamic`) = *merge*(`dynamic`, `object`) = `dynamic`</span><span class="sxs-lookup"><span data-stu-id="7978c-323">*Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`</span></span>

## <a name="warnings"></a><span data-ttu-id="7978c-324">Ostrzeżenia</span><span class="sxs-lookup"><span data-stu-id="7978c-324">Warnings</span></span>

### <a name="potential-null-assignment"></a><span data-ttu-id="7978c-325">Potencjalne przypisanie o wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-325">Potential null assignment</span></span>

### <a name="potential-null-dereference"></a><span data-ttu-id="7978c-326">Potencjalna odwołująca się wartość null</span><span class="sxs-lookup"><span data-stu-id="7978c-326">Potential null dereference</span></span>

### <a name="constraint-nullability-mismatch"></a><span data-ttu-id="7978c-327">Niezgodność wartości NULL ograniczenia</span><span class="sxs-lookup"><span data-stu-id="7978c-327">Constraint nullability mismatch</span></span>

### <a name="nullable-types-in-disabled-annotation-context"></a><span data-ttu-id="7978c-328">Typy dopuszczające wartości null w wyłączonym kontekście adnotacji</span><span class="sxs-lookup"><span data-stu-id="7978c-328">Nullable types in disabled annotation context</span></span>

## <a name="attributes-for-special-null-behavior"></a><span data-ttu-id="7978c-329">Atrybuty dla specjalnego zachowania o wartości null</span><span class="sxs-lookup"><span data-stu-id="7978c-329">Attributes for special null behavior</span></span>


