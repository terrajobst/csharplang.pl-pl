---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485064"
---
# <a name="recursive-pattern-matching"></a><span data-ttu-id="e5de2-101">Cykliczne dopasowanie wzorca</span><span class="sxs-lookup"><span data-stu-id="e5de2-101">Recursive Pattern Matching</span></span>

## <a name="summary"></a><span data-ttu-id="e5de2-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="e5de2-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="e5de2-103">Rozszerzenia zgodne z wzorcem umożliwiające C# korzystanie z wielu zalet typów danych algebraicznych i dopasowywanie wzorców z języków funkcjonalnych, ale w sposób, który bezproblemowo integruje się z działaniem języka bazowego.</span><span class="sxs-lookup"><span data-stu-id="e5de2-103">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="e5de2-104">Elementy tego podejścia są inspirowane powiązanymi funkcjami w językach [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Rozszerzalne Dopasowywanie wzorców za pośrednictwem języka lekkiego") programowania i [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Dopasowywanie obiektów ze wzorcami, Strona 273").</span><span class="sxs-lookup"><span data-stu-id="e5de2-104">Elements of this approach are inspired by related features in the programming languages [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Matching Objects With Patterns, page 273").</span></span>

## <a name="detailed-design"></a><span data-ttu-id="e5de2-105">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="e5de2-105">Detailed design</span></span>
[design]: #detailed-design

### <a name="is-expression"></a><span data-ttu-id="e5de2-106">Wyrażenie is</span><span class="sxs-lookup"><span data-stu-id="e5de2-106">Is Expression</span></span>

<span data-ttu-id="e5de2-107">Operator `is` jest rozszerzony w celu przetestowania wyrażenia względem *wzorca*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-107">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="e5de2-108">Ta forma *relational_expression* jest uzupełnieniem istniejących formularzy w C# specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="e5de2-108">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="e5de2-109">Jest to błąd czasu kompilacji, jeśli *relational_expression* po lewej stronie tokenu `is` nie wyznaczy wartości lub nie ma typu.</span><span class="sxs-lookup"><span data-stu-id="e5de2-109">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="e5de2-110">Każdy *Identyfikator* wzorca wprowadza nową zmienną lokalną, która jest *ostatecznie przypisana* po `true` operatora `is` (tj. jest ona *przypisywana w nieskończoność, gdy wartość jest równa true*).</span><span class="sxs-lookup"><span data-stu-id="e5de2-110">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="e5de2-111">Uwaga: w `is-expression` i *constant_pattern*jest to technicznie niejednoznaczności między *typem* , z których każda może być prawidłową analizą kwalifikowanego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="e5de2-111">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="e5de2-112">Staramy się powiązać ją z typem w celu zapewnienia zgodności z poprzednimi wersjami języka; tylko wtedy, gdy to się nie powiedzie, firma Microsoft rozwiązuje ten problem, gdy wykonamy wyrażenie w innych kontekstach, do pierwszego znalezionego (który musi być stałą lub typem).</span><span class="sxs-lookup"><span data-stu-id="e5de2-112">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do an expression in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="e5de2-113">Ta niejednoznaczność jest obecna tylko po prawej stronie wyrażenia `is`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-113">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="patterns"></a><span data-ttu-id="e5de2-114">Wzorce</span><span class="sxs-lookup"><span data-stu-id="e5de2-114">Patterns</span></span>

<span data-ttu-id="e5de2-115">Wzorce są używane w operatorze *is_pattern* , w *switch_statement*i w *switch_expression* do wyrażania kształtu danych, z których dane przychodzące (wywoływane przez nas wartość wejściową) mają być porównywane.</span><span class="sxs-lookup"><span data-stu-id="e5de2-115">Patterns are used in the *is_pattern* operator, in a *switch_statement*, and in a *switch_expression* to express the shape of data against which incoming data  (which we call the input value) is to be compared.</span></span> <span data-ttu-id="e5de2-116">Wzorce mogą być cykliczne, aby części danych mogły być dopasowane do wzorców podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="e5de2-116">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a><span data-ttu-id="e5de2-117">Wzorzec deklaracji</span><span class="sxs-lookup"><span data-stu-id="e5de2-117">Declaration Pattern</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="e5de2-118">*Declaration_pattern* obu testuje, że wyrażenie jest danego typu i rzutuje je na ten typ, jeśli test zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="e5de2-118">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="e5de2-119">Może to spowodować, że zmienna lokalna danego typu nazywana przez dany identyfikator, jeśli oznaczenie jest *single_variable_designation*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-119">This may introduce a local variable of the given type named by the given identifier, if the designation is a *single_variable_designation*.</span></span> <span data-ttu-id="e5de2-120">Ta zmienna lokalna jest *ostatecznie przypisana* , gdy wynik operacji dopasowania do wzorca jest `true`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-120">That local variable is *definitely assigned* when the result of the pattern-matching operation is `true`.</span></span>

<span data-ttu-id="e5de2-121">Semantyka środowiska uruchomieniowego tego wyrażenia sprawdza typ środowiska uruchomieniowego operandu *relational_expression* po lewej stronie względem *typu* we wzorcu.</span><span class="sxs-lookup"><span data-stu-id="e5de2-121">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span>  <span data-ttu-id="e5de2-122">Jeśli jest to typ środowiska uruchomieniowego (lub niektóre podtypu), a nie `null`, wynik `is operator` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-122">If it is of that runtime type (or some subtype) and not `null`, the result of the `is operator` is `true`.</span></span>

<span data-ttu-id="e5de2-123">Niektóre kombinacje typu statycznego po lewej stronie i danego typu są uznawane za niezgodne i powodują błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e5de2-123">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="e5de2-124">Wartość typu statycznego `E` jest określana jako *zgodna ze wzorcem* z typem `T`, jeśli istnieje konwersja tożsamości, niejawna konwersja odwołania, konwersja z opakowania, jawna konwersja odwołania lub konwersja rozpakowywania z `E` do `T`lub jeśli jeden z tych typów jest typem otwartym.</span><span class="sxs-lookup"><span data-stu-id="e5de2-124">A value of static type `E` is said to be *pattern-compatible* with a type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, or if one of those types is an open type.</span></span> <span data-ttu-id="e5de2-125">Jest to błąd czasu kompilacji, jeśli dane wejściowe typu `E` nie są zgodne ze *wzorcem* z *typem* w wzorcu typów, z którym jest on dopasowany.</span><span class="sxs-lookup"><span data-stu-id="e5de2-125">It is a compile-time error if an input of type `E` is not *pattern-compatible* with the *type* in a type pattern that it is matched with.</span></span>

<span data-ttu-id="e5de2-126">Wzorzec typu jest przydatny do wykonywania testów typu w czasie wykonywania dla typów referencyjnych i zastępuje idiom</span><span class="sxs-lookup"><span data-stu-id="e5de2-126">The type pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

<span data-ttu-id="e5de2-127">O nieco bardziej zwięzły</span><span class="sxs-lookup"><span data-stu-id="e5de2-127">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v
```

<span data-ttu-id="e5de2-128">Jeśli *Typ* jest typem wartości null, występuje błąd.</span><span class="sxs-lookup"><span data-stu-id="e5de2-128">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="e5de2-129">Wzorzec typu może służyć do testowania wartości typów dopuszczających wartość null: wartość typu `Nullable<T>` (lub opakowany `T`) pasuje do wzorca typu, `T2 id` Jeśli wartość jest inna niż null, a typ `T2` to `T`lub jakiś typ podstawowy lub interfejs `T`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-129">The type pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="e5de2-130">Na przykład w fragmencie kodu</span><span class="sxs-lookup"><span data-stu-id="e5de2-130">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v
```

<span data-ttu-id="e5de2-131">Warunek instrukcji `if` jest `true` w czasie wykonywania, a zmienna `v` przechowuje wartość `3` typu `int` wewnątrz bloku.</span><span class="sxs-lookup"><span data-stu-id="e5de2-131">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span> <span data-ttu-id="e5de2-132">Po bloku Zmienna `v` jest w zakresie, ale nie jest ostatecznie przypisana.</span><span class="sxs-lookup"><span data-stu-id="e5de2-132">After the block the variable `v` is in scope but not definitely assigned.</span></span>

#### <a name="constant-pattern"></a><span data-ttu-id="e5de2-133">Wzorzec stałej</span><span class="sxs-lookup"><span data-stu-id="e5de2-133">Constant Pattern</span></span>

```antlr
constant_pattern
    : constant_expression
    ;
```

<span data-ttu-id="e5de2-134">Stały wzorzec testuje wartość wyrażenia w stosunku do wartości stałej.</span><span class="sxs-lookup"><span data-stu-id="e5de2-134">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="e5de2-135">Stała może być dowolnym wyrażeniem stałym, takim jak literał, nazwa zadeklarowanej zmiennej `const` lub stała wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="e5de2-135">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant.</span></span> <span data-ttu-id="e5de2-136">Gdy wartość wejściowa nie jest typu otwartego, wyrażenie stałe jest niejawnie konwertowane na typ dopasowanego wyrażenia; Jeśli typ wartości wejściowej nie jest zgodny ze *wzorcem* z typem wyrażenia stałego, Operacja dopasowania do wzorca jest błędem.</span><span class="sxs-lookup"><span data-stu-id="e5de2-136">When the input value is not an open type, the constant expression is implicitly converted to the type of the matched expression; if the type of the input value is not *pattern-compatible* with the type of the constant expression, the pattern-matching operation is an error.</span></span>

<span data-ttu-id="e5de2-137">Wzorzec *c* jest uznawany za zgodny ze przekonwertowaną wartością wejściową *e* , jeśli `object.Equals(c, e)` zwróci `true`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-137">The pattern *c* is considered matching the converted input value *e* if `object.Equals(c, e)` would return `true`.</span></span>

<span data-ttu-id="e5de2-138">Oczekujemy, że `e is null` jako najbardziej typowym sposobem na przetestowanie `null` w nowo zapisanym kodzie, ponieważ nie można wywoływać `operator==`zdefiniowanej przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e5de2-138">We expect to see `e is null` as the most common way to test for `null` in newly written code, as it cannot invoke a user-defined `operator==`.</span></span>

#### <a name="var-pattern"></a><span data-ttu-id="e5de2-139">Wzorzec wariancji</span><span class="sxs-lookup"><span data-stu-id="e5de2-139">Var Pattern</span></span>

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

<span data-ttu-id="e5de2-140">Jeśli *oznaczenie* jest *simple_designation*, wyrażenie *e* pasuje do wzorca.</span><span class="sxs-lookup"><span data-stu-id="e5de2-140">If the *designation* is a *simple_designation*, an expression *e* matches the pattern.</span></span> <span data-ttu-id="e5de2-141">Inaczej mówiąc, dopasowanie do *wzorca var* zawsze powiedzie się z *simple_designation*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-141">In other words, a match to a *var pattern* always succeeds with a *simple_designation*.</span></span> <span data-ttu-id="e5de2-142">Jeśli *simple_designation* jest *single_variable_designation*, wartość *e* jest związana z nowo wprowadzoną zmienną lokalną.</span><span class="sxs-lookup"><span data-stu-id="e5de2-142">If the *simple_designation* is a *single_variable_designation*, the value of *e* is bounds to a newly introduced local variable.</span></span> <span data-ttu-id="e5de2-143">Typem zmiennej lokalnej jest typ statyczny *e*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-143">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="e5de2-144">Jeśli *oznaczenie* jest *tuple_designation*, wzorzec jest odpowiednikiem *positional_pattern* formularza `(var` *oznaczenie*,... `)`, gdzie *oznaczenie*s znajduje się w *tuple_designation*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-144">If the *designation* is a *tuple_designation*, then the pattern is equivalent to a *positional_pattern* of the form `(var` *designation*, ... `)` where the *designation*s are those found within the *tuple_designation*.</span></span>  <span data-ttu-id="e5de2-145">Na przykład wzorzec `var (x, (y, z))` jest równoznaczny z `(var x, (var y, var z))`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-145">For example, the pattern `var (x, (y, z))` is equivalent to `(var x, (var y, var z))`.</span></span>

<span data-ttu-id="e5de2-146">Jeśli nazwa `var` powiązania z typem, występuje błąd.</span><span class="sxs-lookup"><span data-stu-id="e5de2-146">It is an error if the name `var` binds to a type.</span></span>

#### <a name="discard-pattern"></a><span data-ttu-id="e5de2-147">Odrzuć wzorzec</span><span class="sxs-lookup"><span data-stu-id="e5de2-147">Discard Pattern</span></span>

```antlr
discard_pattern
    : '_'
    ;
```

<span data-ttu-id="e5de2-148">Wyrażenie *e* pasuje do wzorca `_` zawsze.</span><span class="sxs-lookup"><span data-stu-id="e5de2-148">An expression *e* matches the pattern `_` always.</span></span> <span data-ttu-id="e5de2-149">Innymi słowy każde wyrażenie pasuje do wzorca odrzucania.</span><span class="sxs-lookup"><span data-stu-id="e5de2-149">In other words, every expression matches the discard pattern.</span></span>

<span data-ttu-id="e5de2-150">Nie można użyć wzorca odrzucania jako wzorca *is_pattern_expression*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-150">A discard pattern may not be used as the pattern of an *is_pattern_expression*.</span></span>

#### <a name="positional-pattern"></a><span data-ttu-id="e5de2-151">Wzorzec pozycyjny</span><span class="sxs-lookup"><span data-stu-id="e5de2-151">Positional Pattern</span></span>

<span data-ttu-id="e5de2-152">Wzorzec pozycyjny sprawdza, czy wartość wejściowa nie jest `null`, wywołuje odpowiednią metodę `Deconstruct` i wykonuje dalsze Dopasowywanie wzorców do wartości wyników.</span><span class="sxs-lookup"><span data-stu-id="e5de2-152">A positional pattern checks that the input value is not `null`, invokes an appropriate `Deconstruct` method, and performs further pattern matching on the resulting values.</span></span>  <span data-ttu-id="e5de2-153">Obsługuje również składnię wzorca przypominającą krotkę (bez podanego typu), gdy typ wartości wejściowej jest taki sam jak typ zawierający `Deconstruct`, lub jeśli typ wartości wejściowej jest typem krotki, lub jeśli typ wartości wejściowej to `object` lub `ITuple` i typ środowiska uruchomieniowego w wyrażeniu implementuje `ITuple`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-153">It also supports a tuple-like pattern syntax (without the type being provided) when the type of the input value is the same as the type containing `Deconstruct`, or if the type of the input value is a tuple type, or if the type of the input value is `object` or `ITuple` and the runtime type of the expression implements `ITuple`.</span></span>

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

<span data-ttu-id="e5de2-154">Jeśli *Typ* zostanie pominięty, przyjmujemy, że jest to typ statyczny wartości wejściowej.</span><span class="sxs-lookup"><span data-stu-id="e5de2-154">If the *type* is omitted, we take it to be the static type of the input value.</span></span>

<span data-ttu-id="e5de2-155">Uwzględniając dopasowanie wartości wejściowej do *typu* wzorca `(` *subpattern_list* `)`, metoda jest wybierana przez wyszukiwanie w *typie* dla dostępnych deklaracji `Deconstruct` i wybranie jednej z nich przy użyciu tych samych reguł dla deklaracji dekonstrukcji.</span><span class="sxs-lookup"><span data-stu-id="e5de2-155">Given a match of an input value to the pattern *type* `(` *subpattern_list* `)`, a method is selected by searching in *type* for accessible declarations of `Deconstruct` and selecting one among them using the same rules as for the deconstruction declaration.</span></span>

<span data-ttu-id="e5de2-156">Jeśli *positional_pattern* pomija typ, ma jeden *podwzorzec* bez *identyfikatora*, nie ma *property_subpattern* i nie *simple_designation*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-156">It is an error if a *positional_pattern* omits the type, has a single *subpattern* without an *identifier*, has no *property_subpattern* and has no *simple_designation*.</span></span> <span data-ttu-id="e5de2-157">Powoduje to odróżnienie między *constant_pattern* w nawiasach i *positional_pattern*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-157">This disambiguates between a *constant_pattern* that is parenthesized and a *positional_pattern*.</span></span>

<span data-ttu-id="e5de2-158">W celu wyodrębnienia wartości, aby były zgodne ze wzorcami na liście,</span><span class="sxs-lookup"><span data-stu-id="e5de2-158">In order to extract the values to match against the patterns in the list,</span></span>
- <span data-ttu-id="e5de2-159">Jeśli *Typ* został pominięty, a typ wartości wejściowej jest typem krotki, liczba wzorców jest wymagana tak samo jak Kardynalność krotki.</span><span class="sxs-lookup"><span data-stu-id="e5de2-159">If *type* was omitted and the input value's type is a tuple type, then the number of subpatterns is required to be the same as the cardinality of the tuple.</span></span> <span data-ttu-id="e5de2-160">Każdy element krotki jest dopasowywany do odpowiadającego mu *podwzorca*, a dopasowanie powiedzie się, jeśli wszystkie z nich zakończyły się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="e5de2-160">Each tuple element is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="e5de2-161">Jeśli którykolwiek z *wzorców* ma *Identyfikator*, to musi należeć do nazwy elementu krotki w odpowiedniej pozycji w typie krotki.</span><span class="sxs-lookup"><span data-stu-id="e5de2-161">If any *subpattern* has an *identifier*, then that must name a tuple element at the corresponding position in the tuple type.</span></span>
- <span data-ttu-id="e5de2-162">W przeciwnym razie, Jeśli odpowiednia `Deconstruct` istnieje jako element członkowski *typu*, jest to błąd czasu kompilacji, jeśli typ wartości wejściowej nie jest *zgodny ze wzorcem* *typu*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-162">Otherwise, if a suitable `Deconstruct` exists as a member of *type*, it is a compile-time error if the type of the input value is not *pattern-compatible* with *type*.</span></span> <span data-ttu-id="e5de2-163">W czasie wykonywania wartość wejściowa jest testowana względem *typu*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-163">At runtime the input value is tested against *type*.</span></span> <span data-ttu-id="e5de2-164">Jeśli to się nie powiedzie, dopasowanie do wzorca pozycyjnego zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="e5de2-164">If this fails then the positional pattern match fails.</span></span> <span data-ttu-id="e5de2-165">Jeśli to się powiedzie, wartość wejściowa jest konwertowana na ten typ, a `Deconstruct` jest wywoływana z użyciem świeżych zmiennych generowanych przez kompilator w celu uzyskania parametrów `out`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-165">If it succeeds,  the input value is converted to this type and `Deconstruct` is invoked with fresh compiler-generated variables to receive the `out` parameters.</span></span> <span data-ttu-id="e5de2-166">Każda odebrana wartość jest dopasowywana do odpowiadającego jej *wzorca*, a dopasowanie powiedzie się, jeśli wszystkie z nich zakończyły się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="e5de2-166">Each value that was received is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="e5de2-167">Jeśli którykolwiek z *wzorców* ma *Identyfikator*, należy nazwać parametr w odpowiedniej pozycji `Deconstruct`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-167">If any *subpattern* has an *identifier*, then that must name a parameter at the corresponding position of `Deconstruct`.</span></span>
- <span data-ttu-id="e5de2-168">W przeciwnym razie, jeśli *Typ* został pominięty, a wartość wejściowa jest typu `object` lub `ITuple` lub jakiś typ, który można przekonwertować na `ITuple` przez niejawną konwersję odwołania, a żaden *Identyfikator* nie pojawia się w podwzorcach, zgadzamy się przy użyciu `ITuple`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-168">Otherwise if *type* was omitted, and the input value is of type `object` or `ITuple` or some type that can be converted to `ITuple` by an implicit reference conversion, and no *identifier* appears among the subpatterns, then we match using `ITuple`.</span></span>
- <span data-ttu-id="e5de2-169">W przeciwnym razie wzorzec jest błędem czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="e5de2-169">Otherwise the pattern is a compile-time error.</span></span>

<span data-ttu-id="e5de2-170">Kolejność, w której podwzorce są dopasowane w czasie wykonywania, nie jest określona, a dopasowanie nie może próbować dopasować wszystkich wzorców.</span><span class="sxs-lookup"><span data-stu-id="e5de2-170">The order in which subpatterns are matched at runtime is unspecified, and a failed match may not attempt to match all subpatterns.</span></span>

##### <a name="example"></a><span data-ttu-id="e5de2-171">Przykład</span><span class="sxs-lookup"><span data-stu-id="e5de2-171">Example</span></span>

<span data-ttu-id="e5de2-172">Ten przykład używa wielu funkcji opisanych w tej specyfikacji</span><span class="sxs-lookup"><span data-stu-id="e5de2-172">This example uses many of the features described in this specification</span></span>

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a><span data-ttu-id="e5de2-173">Wzorzec właściwości</span><span class="sxs-lookup"><span data-stu-id="e5de2-173">Property Pattern</span></span>

<span data-ttu-id="e5de2-174">Wzorzec właściwości sprawdza, czy wartość wejściowa nie jest `null` i rekursywnie dopasowuje wartości wyodrębnione przy użyciu dostępnych właściwości lub pól.</span><span class="sxs-lookup"><span data-stu-id="e5de2-174">A property pattern checks that the input value is not `null` and recursively matches values extracted by the use of accessible properties or fields.</span></span>

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

<span data-ttu-id="e5de2-175">Jeśli którykolwiek z _wzorców_ _property_pattern_ nie zawiera _identyfikatora_ (musi to być drugi formularz, który ma _Identyfikator_), występuje błąd.</span><span class="sxs-lookup"><span data-stu-id="e5de2-175">It is an error if any _subpattern_ of a _property_pattern_ does not contain an _identifier_ (it must be of the second form, which has an _identifier_).</span></span>  <span data-ttu-id="e5de2-176">Końcowy przecinek po ostatnim przedwzorcu jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="e5de2-176">A trailing comma after the last subpattern is optional.</span></span>

<span data-ttu-id="e5de2-177">Należy zauważyć, że wzorzec sprawdzania wartości null jest spoza prostego wzorca właściwości.</span><span class="sxs-lookup"><span data-stu-id="e5de2-177">Note that a null-checking pattern falls out of a trivial property pattern.</span></span> <span data-ttu-id="e5de2-178">Aby sprawdzić, czy ciąg `s` ma wartość różną od null, można napisać dowolną z następujących form</span><span class="sxs-lookup"><span data-stu-id="e5de2-178">To check if the string `s` is non-null, you can write any of the following forms</span></span>

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

<span data-ttu-id="e5de2-179">Nadano dopasowanie wyrażenia *e* do *typu* wzorca `{` *property_pattern_list* `}`, jest to błąd czasu kompilacji, jeśli wyrażenie *e* nie jest *zgodne ze wzorcem* typu *t* oznaczonego przez *Typ*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-179">Given a match of an expression *e* to the pattern *type* `{` *property_pattern_list* `}`, it is a compile-time error if the expression *e* is not *pattern-compatible* with the type *T* designated by *type*.</span></span> <span data-ttu-id="e5de2-180">Jeśli typ nie jest obecny, przyjmujemy, że jest to typ statyczny *e*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-180">If the type is absent, we take it to be the static type of *e*.</span></span> <span data-ttu-id="e5de2-181">Jeśli *Identyfikator* jest obecny, deklaruje zmienną *wzorca typu Type.*</span><span class="sxs-lookup"><span data-stu-id="e5de2-181">If the *identifier* is present, it declares a pattern variable of type *type*.</span></span> <span data-ttu-id="e5de2-182">Każdy z identyfikatorów pojawiających się po lewej stronie *property_pattern_list* musi wyznaczyć dostępną właściwość lub pole z *T*. Jeśli *simple_designation* *property_pattern* jest obecny, definiuje zmienną wzorca typu *T*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-182">Each of the identifiers appearing on the left-hand-side of its *property_pattern_list* must designate an accessible readable property or field of *T*. If the *simple_designation* of the *property_pattern* is present, it defines a pattern variable of type *T*.</span></span>

<span data-ttu-id="e5de2-183">W czasie wykonywania wyrażenie jest testowane względem *T*. Jeśli to się nie powiedzie, dopasowanie wzorca właściwości zakończy się niepowodzeniem, a wynik jest `false`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-183">At runtime, the expression is tested against *T*. If this fails then the property pattern match fails and the result is `false`.</span></span> <span data-ttu-id="e5de2-184">Jeśli to się powiedzie, wszystkie *property_subpattern* pola lub właściwości są odczytywane i ich wartość pasuje do odpowiadającego jej wzorca.</span><span class="sxs-lookup"><span data-stu-id="e5de2-184">If it succeeds, then each *property_subpattern* field or property is read and its value matched against its corresponding pattern.</span></span> <span data-ttu-id="e5de2-185">Wynik całego dopasowania jest `false` tylko wtedy, gdy wynik któregokolwiek z tych elementów jest `false`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-185">The result of the whole match is `false` only if the result of any of these is `false`.</span></span> <span data-ttu-id="e5de2-186">Kolejność, w której są dopasowywane wzorce, nie jest określona, a dopasowanie nie może być zgodne ze wszystkimi podwzorcami w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e5de2-186">The order in which subpatterns are matched is not specified, and a failed match may not match all subpatterns at runtime.</span></span> <span data-ttu-id="e5de2-187">Jeśli dopasowanie się powiedzie, a *simple_designation* *property_pattern* jest *Single_variable_designation*, definiuje zmienną typu *T* , która ma przypisaną dopasowaną wartość.</span><span class="sxs-lookup"><span data-stu-id="e5de2-187">If the match succeeds and the *simple_designation* of the *property_pattern* is a *single_variable_designation*, it defines a variable of type *T* that is assigned the matched value.</span></span>

> <span data-ttu-id="e5de2-188">Uwaga: wzorzec właściwości może służyć do dopasowania do wzorca przy użyciu typów anonimowych.</span><span class="sxs-lookup"><span data-stu-id="e5de2-188">Note: The property pattern can be used to pattern-match with anonymous types.</span></span>

##### <a name="example"></a><span data-ttu-id="e5de2-189">Przykład</span><span class="sxs-lookup"><span data-stu-id="e5de2-189">Example</span></span>

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a><span data-ttu-id="e5de2-190">Wyrażenie Switch</span><span class="sxs-lookup"><span data-stu-id="e5de2-190">Switch Expression</span></span>

<span data-ttu-id="e5de2-191">*Switch_expression* jest dodawany do obsługi semantyki podobnej do `switch`dla kontekstu wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="e5de2-191">A *switch_expression* is added to support `switch`-like semantics for an expression context.</span></span>

<span data-ttu-id="e5de2-192">Składnia C# języka jest rozszerzana o następujące produkcje składniowe:</span><span class="sxs-lookup"><span data-stu-id="e5de2-192">The C# language syntax is augmented with the following syntactic productions:</span></span>

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

<span data-ttu-id="e5de2-193">*Switch_expression* nie jest dozwolony jako *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-193">The *switch_expression* is not permitted as an *expression_statement*.</span></span>

> <span data-ttu-id="e5de2-194">Przeglądamy ten złagodzeniu wymagań dotyczących w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="e5de2-194">We are looking at relaxing this in a future revision.</span></span>

<span data-ttu-id="e5de2-195">Typ *switch_expression* jest [*najlepszym typowym typem*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) wyrażeń występujących po prawej stronie tokenów `=>` *switch_expression_arm*s, jeśli taki typ istnieje, a wyrażenie w każdym ramieniu wyrażenia Switch może być niejawnie konwertowane na ten typ.</span><span class="sxs-lookup"><span data-stu-id="e5de2-195">The type of the *switch_expression* is the [*best common type*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) of the expressions appearing to the right of the `=>` tokens of the *switch_expression_arm*s if such a type exists and the expression in every arm of the switch expression can be implicitly converted to that type.</span></span>  <span data-ttu-id="e5de2-196">Ponadto dodamy nową *konwersję wyrażenia Switch*, która jest wstępnie zdefiniowaną niejawną konwersją z wyrażenia Switch do każdego typu `T`, dla którego istnieje niejawna konwersja z wyrażenia ramienia do `T`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-196">In addition, we add a new *switch expression conversion*, which is a predefined implicit conversion from a switch expression to every type `T` for which there exists an implicit conversion from each arm's expression to `T`.</span></span>

<span data-ttu-id="e5de2-197">Jeśli jakiś wzorzec *switch_expression_arm*nie może wpływać na wynik, ponieważ niektóre poprzednie wzorce i zabezpieczenia będą zawsze zgodne, występuje błąd.</span><span class="sxs-lookup"><span data-stu-id="e5de2-197">It is an error if some *switch_expression_arm*'s pattern cannot affect the result because some previous pattern and guard will always match.</span></span>

<span data-ttu-id="e5de2-198">Wyrażenie Switch jest uznawane za *wyczerpujące* , jeśli niektóre ARM wyrażenia Switch obsługuje każdą wartość danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="e5de2-198">A switch expression is said to be *exhaustive* if some arm of the switch expression handles every value of its input.</span></span>  <span data-ttu-id="e5de2-199">Kompilator tworzy ostrzeżenie, jeśli wyrażenie Switch nie jest *wyczerpujące*.</span><span class="sxs-lookup"><span data-stu-id="e5de2-199">The compiler shall produce a warning if a switch expression is not *exhaustive*.</span></span>

<span data-ttu-id="e5de2-200">W czasie wykonywania, wynik *switch_expression* jest wartością *wyrażenia* pierwszego *switch_expression_arm* , dla którego wyrażenie po lewej stronie *switch_expression* dopasowuje wzorzec *switch_expression_arm*i dla którego występuje *case_guard* *switch_expression_arm, a*jeśli jest obecny, oblicza się w `true`.</span><span class="sxs-lookup"><span data-stu-id="e5de2-200">At runtime, the result of the *switch_expression* is the value of the *expression* of the first *switch_expression_arm* for which the expression on the left-hand-side of the *switch_expression* matches the *switch_expression_arm*'s pattern, and for which the *case_guard* of the *switch_expression_arm*, if present, evaluates to `true`.</span></span> <span data-ttu-id="e5de2-201">Jeśli nie ma takich *switch_expression_arm*, *switch_expression* zgłasza wystąpienie `System.Runtime.CompilerServices.SwitchExpressionException`wyjątków.</span><span class="sxs-lookup"><span data-stu-id="e5de2-201">If there is no such *switch_expression_arm*, the *switch_expression* throws an instance of the exception `System.Runtime.CompilerServices.SwitchExpressionException`.</span></span>

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a><span data-ttu-id="e5de2-202">Opcjonalna parens podczas przełączania do literału krotki</span><span class="sxs-lookup"><span data-stu-id="e5de2-202">Optional parens when switching on a tuple literal</span></span>

<span data-ttu-id="e5de2-203">Aby można było przełączać się do literału krotki przy użyciu *switch_statement*, należy napisać, co wydaje się być nadmiarowe parens</span><span class="sxs-lookup"><span data-stu-id="e5de2-203">In order to switch on a tuple literal using the *switch_statement*, you have to write what appear to be redundant parens</span></span>

```csharp
switch ((a, b))
{
```

<span data-ttu-id="e5de2-204">Aby zezwolić</span><span class="sxs-lookup"><span data-stu-id="e5de2-204">To permit</span></span>

```csharp
switch (a, b)
{
```

<span data-ttu-id="e5de2-205">nawiasy instrukcji switch są opcjonalne, gdy wyrażenie, które jest włączane, jest literałem krotki.</span><span class="sxs-lookup"><span data-stu-id="e5de2-205">the parentheses of the switch statement are optional when the expression being switched on is a tuple literal.</span></span>

### <a name="order-of-evaluation-in-pattern-matching"></a><span data-ttu-id="e5de2-206">Kolejność obliczeń w dopasowaniu do wzorca</span><span class="sxs-lookup"><span data-stu-id="e5de2-206">Order of evaluation in pattern-matching</span></span>

<span data-ttu-id="e5de2-207">Zapewnianie elastyczności kompilatora w przypadku zmiany kolejności operacji wykonywanych podczas dopasowywania do wzorca może pozwolić na elastyczność, która może być używana do poprawy efektywności dopasowania do wzorca.</span><span class="sxs-lookup"><span data-stu-id="e5de2-207">Giving the compiler flexibility in reordering the operations executed during pattern-matching can permit flexibility that can be used to improve the efficiency of pattern-matching.</span></span> <span data-ttu-id="e5de2-208">Wymaganie (niewymuszone) byłoby, że właściwości, do których uzyskuje się dostęp we wzorcu, a metody dekonstrukcji muszą mieć wartość "czysty" (wolny, idempotentne, itp.).</span><span class="sxs-lookup"><span data-stu-id="e5de2-208">The (unenforced) requirement would be that properties accessed in a pattern, and the Deconstruct methods, are required to be "pure" (side-effect free, idempotent, etc).</span></span> <span data-ttu-id="e5de2-209">Nie oznacza to, że możemy dodać czystość jako koncepcję języka, tylko dzięki czemu możemy zapewnić elastyczność kompilatora w przypadku operacji zmiany kolejności.</span><span class="sxs-lookup"><span data-stu-id="e5de2-209">That doesn't mean that we would add purity as a language concept, only that we would allow the compiler flexibility in reordering operations.</span></span>

<span data-ttu-id="e5de2-210">**Rozwiązanie 2018-04-04 LDM**: Confirmed: kompilator może zmieniać kolejność wywołań `Deconstruct`, dostępu do właściwości i wywołań metod w `ITuple`i może założyć, że zwracane wartości są takie same w przypadku wielu wywołań.</span><span class="sxs-lookup"><span data-stu-id="e5de2-210">**Resolution 2018-04-04 LDM**: confirmed: the compiler is permitted to reorder calls to `Deconstruct`, property accesses, and invocations of methods in `ITuple`, and may assume that returned values are the same from multiple calls.</span></span> <span data-ttu-id="e5de2-211">Kompilator nie powinien wywoływać funkcji, które nie mają wpływu na wynik. przed wprowadzeniem jakichkolwiek zmian w kolejności oceny wygenerowanej przez kompilator w przyszłości będzie bardzo uważnie.</span><span class="sxs-lookup"><span data-stu-id="e5de2-211">The compiler should not invoke functions that cannot affect the result, and we will be very careful before making any changes to the compiler-generated order of evaluation in the future.</span></span>

### <a name="some-possible-optimizations"></a><span data-ttu-id="e5de2-212">Niektóre możliwe optymalizacje</span><span class="sxs-lookup"><span data-stu-id="e5de2-212">Some Possible Optimizations</span></span>

<span data-ttu-id="e5de2-213">Kompilacja dopasowania do wzorca może korzystać ze wspólnych części wzorców.</span><span class="sxs-lookup"><span data-stu-id="e5de2-213">The compilation of pattern matching can take advantage of common parts of patterns.</span></span> <span data-ttu-id="e5de2-214">Na przykład, jeśli test typu najwyższego poziomu dwóch kolejnych wzorców w *switch_statement* jest tego samego typu, wygenerowany kod może pominąć test typu dla drugiego wzorca.</span><span class="sxs-lookup"><span data-stu-id="e5de2-214">For example, if the top-level type test of two successive patterns in a *switch_statement* is the same type, the generated code can skip the type test for the second pattern.</span></span>

<span data-ttu-id="e5de2-215">Jeśli niektóre wzorce są liczbami całkowitymi lub ciągami, kompilator może wygenerować ten sam rodzaj kodu, który generuje dla instrukcji switch we wcześniejszych wersjach języka.</span><span class="sxs-lookup"><span data-stu-id="e5de2-215">When some of the patterns are integers or strings, the compiler can generate the same kind of code it generates for a switch-statement in earlier versions of the language.</span></span>

<span data-ttu-id="e5de2-216">Aby uzyskać więcej informacji na temat tego rodzaju optymalizacji, zobacz [[Scott i Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "Kiedy jest uwzględniana zgodność z algorytmami heurystycznego kompilowania?").</span><span class="sxs-lookup"><span data-stu-id="e5de2-216">For more on these kinds of optimizations, see [[Scott and Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "When Do Match-Compilation Heuristics Matter?").</span></span>
