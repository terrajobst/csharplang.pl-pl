---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485169"
---
# <a name="pattern-matching-for-c-7"></a><span data-ttu-id="303e5-101">Dopasowanie wzorca dla C# 7</span><span class="sxs-lookup"><span data-stu-id="303e5-101">Pattern Matching for C# 7</span></span>

<span data-ttu-id="303e5-102">Rozszerzenia zgodne z wzorcem umożliwiające C# korzystanie z wielu zalet typów danych algebraicznych i dopasowywanie wzorców z języków funkcjonalnych, ale w sposób, który bezproblemowo integruje się z działaniem języka bazowego.</span><span class="sxs-lookup"><span data-stu-id="303e5-102">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="303e5-103">Podstawowe funkcje to: [typy rekordów](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), które są typami, których znaczenie semantyczne jest opisane przez kształt danych; i dopasowywanie do wzorca, które jest nowym formularzem wyrażenia, który umożliwia bardzo zwięzłe dekompozycję tych typów danych.</span><span class="sxs-lookup"><span data-stu-id="303e5-103">The basic features are: [record types](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), which are types whose semantic meaning is described by the shape of the data; and pattern matching, which is a new expression form that enables extremely concise multilevel decomposition of these data types.</span></span> <span data-ttu-id="303e5-104">Elementy tego podejścia są inspirowane powiązanymi funkcjami w językach [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Rozszerzalne Dopasowywanie wzorców za pośrednictwem języka lekkiego") programowania i [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Dopasowywanie obiektów ze wzorcami").</span><span class="sxs-lookup"><span data-stu-id="303e5-104">Elements of this approach are inspired by related features in the programming languages [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Matching Objects With Patterns").</span></span>

## <a name="is-expression"></a><span data-ttu-id="303e5-105">Wyrażenie is</span><span class="sxs-lookup"><span data-stu-id="303e5-105">Is expression</span></span>

<span data-ttu-id="303e5-106">Operator `is` jest rozszerzony w celu przetestowania wyrażenia względem *wzorca*.</span><span class="sxs-lookup"><span data-stu-id="303e5-106">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="303e5-107">Ta forma *relational_expression* jest uzupełnieniem istniejących formularzy w C# specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="303e5-107">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="303e5-108">Jest to błąd czasu kompilacji, jeśli *relational_expression* po lewej stronie tokenu `is` nie wyznaczy wartości lub nie ma typu.</span><span class="sxs-lookup"><span data-stu-id="303e5-108">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="303e5-109">Każdy *Identyfikator* wzorca wprowadza nową zmienną lokalną, która jest *ostatecznie przypisana* po `true` operatora `is` (tj. jest ona *przypisywana w nieskończoność, gdy wartość jest równa true*).</span><span class="sxs-lookup"><span data-stu-id="303e5-109">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="303e5-110">Uwaga: w `is-expression` i *constant_pattern*jest to technicznie niejednoznaczności między *typem* , z których każda może być prawidłową analizą kwalifikowanego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="303e5-110">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="303e5-111">Staramy się powiązać ją z typem w celu zapewnienia zgodności z poprzednimi wersjami języka; tylko wtedy, gdy to się nie powiedzie, firma Microsoft rozwiązuje ten problem, tak jak w przypadku innych kontekstów, do pierwszego znalezionego (który musi być stałą lub typem).</span><span class="sxs-lookup"><span data-stu-id="303e5-111">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="303e5-112">Ta niejednoznaczność jest obecna tylko po prawej stronie wyrażenia `is`.</span><span class="sxs-lookup"><span data-stu-id="303e5-112">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

## <a name="patterns"></a><span data-ttu-id="303e5-113">Wzorce</span><span class="sxs-lookup"><span data-stu-id="303e5-113">Patterns</span></span>

<span data-ttu-id="303e5-114">Wzorce są używane w operatorze `is` i w *switch_statement* do wyrażania kształtu danych, z których dane przychodzące mają być porównywane.</span><span class="sxs-lookup"><span data-stu-id="303e5-114">Patterns are used in the `is` operator and in a *switch_statement* to express the shape of data against which incoming data is to be compared.</span></span> <span data-ttu-id="303e5-115">Wzorce mogą być cykliczne, aby części danych mogły być dopasowane do wzorców podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="303e5-115">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> <span data-ttu-id="303e5-116">Uwaga: w `is-expression` i *constant_pattern*jest to technicznie niejednoznaczności między *typem* , z których każda może być prawidłową analizą kwalifikowanego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="303e5-116">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="303e5-117">Staramy się powiązać ją z typem w celu zapewnienia zgodności z poprzednimi wersjami języka; tylko wtedy, gdy to się nie powiedzie, firma Microsoft rozwiązuje ten problem, tak jak w przypadku innych kontekstów, do pierwszego znalezionego (który musi być stałą lub typem).</span><span class="sxs-lookup"><span data-stu-id="303e5-117">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="303e5-118">Ta niejednoznaczność jest obecna tylko po prawej stronie wyrażenia `is`.</span><span class="sxs-lookup"><span data-stu-id="303e5-118">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="declaration-pattern"></a><span data-ttu-id="303e5-119">Wzorzec deklaracji</span><span class="sxs-lookup"><span data-stu-id="303e5-119">Declaration pattern</span></span>

<span data-ttu-id="303e5-120">*Declaration_pattern* obu testuje, że wyrażenie jest danego typu i rzutuje je na ten typ, jeśli test zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="303e5-120">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="303e5-121">Jeśli *simple_designation* jest identyfikatorem, wprowadza zmienną lokalną danego typu o podanym identyfikatorze.</span><span class="sxs-lookup"><span data-stu-id="303e5-121">If the *simple_designation* is an identifier, it introduces a local variable of the given type named by the given identifier.</span></span> <span data-ttu-id="303e5-122">Ta zmienna lokalna jest *ostatecznie przypisana* , gdy wynik operacji dopasowania do wzorca ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="303e5-122">That local variable is *definitely assigned* when the result of the pattern-matching operation is true.</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="303e5-123">Semantyka środowiska uruchomieniowego tego wyrażenia sprawdza typ środowiska uruchomieniowego operandu *relational_expression* po lewej stronie względem *typu* we wzorcu.</span><span class="sxs-lookup"><span data-stu-id="303e5-123">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span> <span data-ttu-id="303e5-124">W przypadku tego typu środowiska uruchomieniowego (lub niektórych podtypów) wynik `is operator` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="303e5-124">If it is of that runtime type (or some subtype), the result of the `is operator` is `true`.</span></span> <span data-ttu-id="303e5-125">Deklaruje nową zmienną lokalną o nazwie, która ma przypisaną wartość operandu po lewej *stronie, gdy* wynik jest `true`.</span><span class="sxs-lookup"><span data-stu-id="303e5-125">It declares a new local variable named by the *identifier* that is assigned the value of the left-hand operand when the result is `true`.</span></span>

<span data-ttu-id="303e5-126">Niektóre kombinacje typu statycznego po lewej stronie i danego typu są uznawane za niezgodne i powodują błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="303e5-126">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="303e5-127">Wartość typu statycznego `E` jest uznawana za *wzorzec zgodną* z typem `T`, jeśli istnieje konwersja tożsamości, niejawna konwersja odwołania, konwersja z opakowania, jawna konwersja odwołania lub konwersja rozpakowywanie z `E` do `T`.</span><span class="sxs-lookup"><span data-stu-id="303e5-127">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`.</span></span> <span data-ttu-id="303e5-128">Jest to błąd czasu kompilacji, jeśli wyrażenie typu `E` nie jest zgodne ze wzorcem typu w wzorcu typów, z którym jest on dopasowany.</span><span class="sxs-lookup"><span data-stu-id="303e5-128">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

> <span data-ttu-id="303e5-129">Uwaga: [w C# 7,1 rozszerzającmy to](../csharp-7.1/generics-pattern-match.md) , aby umożliwić operacji dopasowania do wzorca, jeśli typ danych wejściowych lub typ `T` jest typem otwartym.</span><span class="sxs-lookup"><span data-stu-id="303e5-129">Note: [In C# 7.1 we extend this](../csharp-7.1/generics-pattern-match.md) to permit a pattern-matching operation if either the input type or the type `T` is an open type.</span></span> <span data-ttu-id="303e5-130">Ten akapit jest zastępowany przez następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="303e5-130">This paragraph is replaced by the following:</span></span>
> 
> <span data-ttu-id="303e5-131">Niektóre kombinacje typu statycznego po lewej stronie i danego typu są uznawane za niezgodne i powodują błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="303e5-131">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="303e5-132">Wartość typu statycznego `E` jest uznawana za *wzorzec zgodną* z typem `T`, jeśli istnieje konwersja tożsamości, niejawna konwersja odwołania, konwersja z opakowania, jawna konwersja odwołania lub konwersja rozpakowywania z `E` do `T`**lub jeśli zarówno `E`, jak i `T` jest typem otwartym**.</span><span class="sxs-lookup"><span data-stu-id="303e5-132">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, **or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="303e5-133">Jest to błąd czasu kompilacji, jeśli wyrażenie typu `E` nie jest zgodne ze wzorcem typu w wzorcu typów, z którym jest on dopasowany.</span><span class="sxs-lookup"><span data-stu-id="303e5-133">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

<span data-ttu-id="303e5-134">Wzorzec deklaracji jest przydatny do wykonywania testów typu w czasie wykonywania typów referencyjnych i zastępuje idiom</span><span class="sxs-lookup"><span data-stu-id="303e5-134">The declaration pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

<span data-ttu-id="303e5-135">O nieco bardziej zwięzły</span><span class="sxs-lookup"><span data-stu-id="303e5-135">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v }
```

<span data-ttu-id="303e5-136">Jeśli *Typ* jest typem wartości null, występuje błąd.</span><span class="sxs-lookup"><span data-stu-id="303e5-136">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="303e5-137">Wzorzec deklaracji może służyć do testowania wartości typów dopuszczających wartość null: wartość typu `Nullable<T>` (lub opakowany `T`) pasuje do wzorca typu, `T2 id` Jeśli wartość jest inna niż null, a typ `T2` to `T`lub jakiś typ podstawowy lub interfejs `T`.</span><span class="sxs-lookup"><span data-stu-id="303e5-137">The declaration pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="303e5-138">Na przykład w fragmencie kodu</span><span class="sxs-lookup"><span data-stu-id="303e5-138">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

<span data-ttu-id="303e5-139">Warunek instrukcji `if` jest `true` w czasie wykonywania, a zmienna `v` przechowuje wartość `3` typu `int` wewnątrz bloku.</span><span class="sxs-lookup"><span data-stu-id="303e5-139">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span>

### <a name="constant-pattern"></a><span data-ttu-id="303e5-140">Wzorzec stałej</span><span class="sxs-lookup"><span data-stu-id="303e5-140">Constant pattern</span></span>

```antlr
constant_pattern
    : shift_expression
    ;
```

<span data-ttu-id="303e5-141">Stały wzorzec testuje wartość wyrażenia w stosunku do wartości stałej.</span><span class="sxs-lookup"><span data-stu-id="303e5-141">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="303e5-142">Stała może być dowolnym wyrażeniem stałym, takim jak literał, nazwa zadeklarowanej zmiennej `const` lub stała wyliczenia lub wyrażenie `typeof`.</span><span class="sxs-lookup"><span data-stu-id="303e5-142">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant, or a `typeof` expression.</span></span>

<span data-ttu-id="303e5-143">Jeśli zarówno *e* , jak i *c* są typami całkowitymi, wzorzec jest uznawany za dopasowany, jeśli wynik wyrażenia `e == c` jest `true`.</span><span class="sxs-lookup"><span data-stu-id="303e5-143">If both *e* and *c* are of integral types, the pattern is considered matched if the result of the expression `e == c` is `true`.</span></span>

<span data-ttu-id="303e5-144">W przeciwnym razie wzorzec jest uznawany za pasujący, jeśli `object.Equals(e, c)` zwraca `true`.</span><span class="sxs-lookup"><span data-stu-id="303e5-144">Otherwise the pattern is considered matching if `object.Equals(e, c)` returns `true`.</span></span> <span data-ttu-id="303e5-145">W takim przypadku jest to błąd czasu kompilacji, jeśli typ statyczny *e* nie jest zgodny ze *wzorcem* typu stałej.</span><span class="sxs-lookup"><span data-stu-id="303e5-145">In this case it is a compile-time error if the static type of *e* is not *pattern compatible* with the type of the constant.</span></span>

### <a name="var-pattern"></a><span data-ttu-id="303e5-146">wzorzec wariancji</span><span class="sxs-lookup"><span data-stu-id="303e5-146">Var pattern</span></span>

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

<span data-ttu-id="303e5-147">Wyrażenie *e* pasuje do *var_pattern* zawsze.</span><span class="sxs-lookup"><span data-stu-id="303e5-147">An expression *e* matches a *var_pattern* always.</span></span> <span data-ttu-id="303e5-148">Inaczej mówiąc, dopasowanie do *wzorca var* zawsze powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="303e5-148">In other words, a match to a *var pattern* always succeeds.</span></span> <span data-ttu-id="303e5-149">Jeśli *simple_designation* jest identyfikatorem, wówczas w czasie wykonywania wartość *e* jest powiązana z nowo wprowadzoną zmienną lokalną.</span><span class="sxs-lookup"><span data-stu-id="303e5-149">If the *simple_designation* is an identifier, then at runtime the value of *e* is bound to a newly introduced local variable.</span></span> <span data-ttu-id="303e5-150">Typem zmiennej lokalnej jest typ statyczny *e*.</span><span class="sxs-lookup"><span data-stu-id="303e5-150">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="303e5-151">Jeśli nazwa `var` powiązania z typem, występuje błąd.</span><span class="sxs-lookup"><span data-stu-id="303e5-151">It is an error if the name `var` binds to a type.</span></span>

## <a name="switch-statement"></a><span data-ttu-id="303e5-152">Switch, instrukcja</span><span class="sxs-lookup"><span data-stu-id="303e5-152">Switch statement</span></span>

<span data-ttu-id="303e5-153">Instrukcja `switch` jest rozszerzona, aby wybrać do wykonania pierwszy blok ze skojarzonym wzorcem zgodnym z *wyrażeniem Switch*.</span><span class="sxs-lookup"><span data-stu-id="303e5-153">The `switch` statement is extended to select for execution the first block having an associated pattern that matches the *switch expression*.</span></span>

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

<span data-ttu-id="303e5-154">Kolejność dopasowywania wzorców nie jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="303e5-154">The order in which patterns are matched is not defined.</span></span> <span data-ttu-id="303e5-155">Kompilator może dopasować wzorce poza kolejnością i ponownie wykorzystać wyniki już dopasowanych wzorców w celu obliczenia wyniku dopasowania innych wzorców.</span><span class="sxs-lookup"><span data-stu-id="303e5-155">A compiler is permitted to match patterns out of order, and to reuse the results of already matched patterns to compute the result of matching of other patterns.</span></span>

<span data-ttu-id="303e5-156">Jeśli jest obecna wartość *Case-Guard* , jej wyrażenie jest typu `bool`.</span><span class="sxs-lookup"><span data-stu-id="303e5-156">If a *case-guard* is present, its expression is of type `bool`.</span></span> <span data-ttu-id="303e5-157">Jest on oceniany jako dodatkowy warunek, który musi być spełniony dla przypadku, aby można go było uznać za spełniony.</span><span class="sxs-lookup"><span data-stu-id="303e5-157">It is evaluated as an additional condition that must be satisfied for the case to be considered satisfied.</span></span>

<span data-ttu-id="303e5-158">Jeśli *switch_label* nie ma wpływu na środowisko uruchomieniowe, występuje błąd, ponieważ jego wzorzec jest subsumed przez poprzednie przypadki.</span><span class="sxs-lookup"><span data-stu-id="303e5-158">It is an error if a *switch_label* can have no effect at runtime because its pattern is subsumed by previous cases.</span></span> <span data-ttu-id="303e5-159">[Do zrobienia: mamy dokładniejsze informacje o technikach, których kompilator musi użyć do osiągnięcia tego orzeczenia.]</span><span class="sxs-lookup"><span data-stu-id="303e5-159">[TODO: We should be more precise about the techniques the compiler is required to use to reach this judgment.]</span></span>

<span data-ttu-id="303e5-160">Zmienna wzorca zadeklarowana w *switch_label* jest ostatecznie przypisana w jej bloku Case, jeśli i tylko wtedy, gdy ten blok case zawiera dokładnie jeden *switch_label*.</span><span class="sxs-lookup"><span data-stu-id="303e5-160">A pattern variable declared in a *switch_label* is definitely assigned in its case block if and only if that case block contains precisely one *switch_label*.</span></span>

<span data-ttu-id="303e5-161">[Do zrobienia: należy określić, kiedy *blok przełącznika* jest osiągalny.]</span><span class="sxs-lookup"><span data-stu-id="303e5-161">[TODO: We should specify when a *switch block* is reachable.]</span></span>

### <a name="scope-of-pattern-variables"></a><span data-ttu-id="303e5-162">Zakres zmiennych wzorca</span><span class="sxs-lookup"><span data-stu-id="303e5-162">Scope of pattern variables</span></span>

<span data-ttu-id="303e5-163">Zakres zmiennej zadeklarowanej we wzorcu jest następujący:</span><span class="sxs-lookup"><span data-stu-id="303e5-163">The scope of a variable declared in a pattern is as follows:</span></span>

- <span data-ttu-id="303e5-164">Jeśli wzorzec jest etykietą przypadku, wówczas zakres zmiennej jest *blokiem przypadku*.</span><span class="sxs-lookup"><span data-stu-id="303e5-164">If the pattern is a case label, then the scope of the variable is the *case block*.</span></span>

<span data-ttu-id="303e5-165">W przeciwnym razie zmienna jest zadeklarowana w wyrażeniu *is_pattern* , a jej zakres jest oparty na konstrukcji bezpośrednio otaczającej wyrażenie zawierające wyrażenie *is_pattern* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="303e5-165">Otherwise the variable is declared in an *is_pattern* expression, and its scope is based on the construct immediately enclosing the expression containing the *is_pattern* expression as follows:</span></span>

- <span data-ttu-id="303e5-166">Jeśli wyrażenie znajduje się w wyrażeniu lambda, jego zakres jest treścią wyrażenia lambda.</span><span class="sxs-lookup"><span data-stu-id="303e5-166">If the expression is in an expression-bodied lambda, its scope is the body of the lambda.</span></span>
- <span data-ttu-id="303e5-167">Jeśli wyrażenie znajduje się w metodzie lub właściwości będącej częścią wyrażenia, jej zakres jest treścią metody lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="303e5-167">If the expression is in an expression-bodied method or property, its scope is the body of the method or property.</span></span>
- <span data-ttu-id="303e5-168">Jeśli wyrażenie znajduje się w klauzuli `when` klauzuli `catch`, jej zakresem jest `catch` klauzula.</span><span class="sxs-lookup"><span data-stu-id="303e5-168">If the expression is in a `when` clause of a `catch` clause, its scope is that `catch` clause.</span></span>
- <span data-ttu-id="303e5-169">Jeśli wyrażenie znajduje się w *iteration_statement*, jego zakres jest tylko tą instrukcją.</span><span class="sxs-lookup"><span data-stu-id="303e5-169">If the expression is in an *iteration_statement*, its scope is just that statement.</span></span>
- <span data-ttu-id="303e5-170">W przeciwnym razie, jeśli wyrażenie znajduje się w innym formularzu instrukcji, jego zakres jest zakresem zawierającym instrukcję.</span><span class="sxs-lookup"><span data-stu-id="303e5-170">Otherwise if the expression is in some other statement form, its scope is the scope containing the statement.</span></span>

<span data-ttu-id="303e5-171">Na potrzeby określenia zakresu *embedded_statement* jest uznawana za w swoim własnym zakresie.</span><span class="sxs-lookup"><span data-stu-id="303e5-171">For the purpose of determining the scope, an *embedded_statement* is considered to be in its own scope.</span></span> <span data-ttu-id="303e5-172">Na przykład Gramatyka dla *if_statement* jest</span><span class="sxs-lookup"><span data-stu-id="303e5-172">For example, the grammar for an *if_statement* is</span></span>

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="303e5-173">Dlatego jeśli kontrolowana instrukcja *if_statement* deklaruje zmienną wzorca, jej zakres jest ograniczony do tego *embedded_statement*:</span><span class="sxs-lookup"><span data-stu-id="303e5-173">So if the controlled statement of an *if_statement* declares a pattern variable, its scope is restricted to that *embedded_statement*:</span></span>

```csharp
if (x) M(y is var z);
```

<span data-ttu-id="303e5-174">W takim przypadku zakres `z` jest osadzoną instrukcją `M(y is var z);`.</span><span class="sxs-lookup"><span data-stu-id="303e5-174">In this case the scope of `z` is the embedded statement `M(y is var z);`.</span></span>

<span data-ttu-id="303e5-175">Inne przypadki są błędami z innych powodów (np. w wartości domyślnej parametru lub atrybutu, co jest spowodowane błędem, ponieważ te konteksty wymagają wyrażenia stałego).</span><span class="sxs-lookup"><span data-stu-id="303e5-175">Other cases are errors for other reasons (e.g. in a parameter's default value or an attribute, both of which are an error because those contexts require a constant expression).</span></span>

> <span data-ttu-id="303e5-176">[W C# 7,3 dodaliśmy następujące konteksty](../csharp-7.3/expression-variables-in-initializers.md) , w których można zadeklarować zmienną wzorca:</span><span class="sxs-lookup"><span data-stu-id="303e5-176">[In C# 7.3 we added the following contexts](../csharp-7.3/expression-variables-in-initializers.md) in which a pattern variable may be declared:</span></span>
> - <span data-ttu-id="303e5-177">Jeśli wyrażenie znajduje się w *inicjatorze konstruktora*, jego zakres jest *inicjatorem konstruktora* i treścią konstruktora.</span><span class="sxs-lookup"><span data-stu-id="303e5-177">If the expression is in a *constructor initializer*, its scope is the *constructor initializer* and the constructor's body.</span></span>
> - <span data-ttu-id="303e5-178">Jeśli wyrażenie znajduje się w inicjatorze pola, jego zakres jest *equals_value_clause* , w którym występuje.</span><span class="sxs-lookup"><span data-stu-id="303e5-178">If the expression is in a field initializer, its scope is the *equals_value_clause* in which it appears.</span></span>
> - <span data-ttu-id="303e5-179">Jeśli wyrażenie znajduje się w klauzuli zapytania, która jest określona do przetłumaczenia na treść wyrażenia lambda, jego zakresem jest tylko to wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="303e5-179">If the expression is in a query clause that is specified to be translated into the body of a lambda, its scope is just that expression.</span></span>

## <a name="changes-to-syntactic-disambiguation"></a><span data-ttu-id="303e5-180">Zmiany dotyczące uściślania składni</span><span class="sxs-lookup"><span data-stu-id="303e5-180">Changes to syntactic disambiguation</span></span>

<span data-ttu-id="303e5-181">Istnieją sytuacje, w których C# Gramatyka jest niejednoznaczna, a Specyfikacja języka informuje, jak rozwiązać te niejasności:</span><span class="sxs-lookup"><span data-stu-id="303e5-181">There are situations involving generics where the C# grammar is ambiguous, and the language spec says how to resolve those ambiguities:</span></span>

> #### <a name="7652-grammar-ambiguities"></a><span data-ttu-id="303e5-182">7.6.5.2 gramatyki niejasności</span><span class="sxs-lookup"><span data-stu-id="303e5-182">7.6.5.2 Grammar ambiguities</span></span>
> <span data-ttu-id="303e5-183">Produkcje dla *prostej nazwy* (§ 7.6.3) i *dostęp do elementów członkowskich* (§ 7.6.5) mogą dać podstawę niejasności w gramatyce wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="303e5-183">The productions for *simple-name* (§7.6.3) and *member-access* (§7.6.5) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="303e5-184">Na przykład, instrukcja:</span><span class="sxs-lookup"><span data-stu-id="303e5-184">For example, the statement:</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="303e5-185">może być interpretowany jako wywołanie `F` z dwoma argumentami, `G < A` i `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="303e5-185">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="303e5-186">Alternatywnie, można go interpretować jako wywołanie do `F` z jednym argumentem, który jest wywołaniem metody generycznej `G` z dwoma argumentami typu i jednym argumentem regularnym.</span><span class="sxs-lookup"><span data-stu-id="303e5-186">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

> <span data-ttu-id="303e5-187">Jeśli sekwencja tokenów może być analizowana (w kontekście) jako *prosta nazwa* (§ 7.6.3), *dostęp do elementu członkowskiego* (§ 7.6.5) lub *wskaźnik — dostęp do elementów członkowskich* (§ 18.5.2) kończący się na *listą argumentów typu* (§ 4.4.1), token bezpośrednio po zamykającym tokenie `>` zostanie sprawdzony.</span><span class="sxs-lookup"><span data-stu-id="303e5-187">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="303e5-188">Jeśli jest to jedna z</span><span class="sxs-lookup"><span data-stu-id="303e5-188">If it is one of</span></span>
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> <span data-ttu-id="303e5-189">następnie *Typ argumentu-list* jest zachowywany jako część *prostej nazwy*, *dostęp do składowej* lub wskaźnika — dostęp do elementu *Członkowskiego* i wszelkie inne możliwe analizy sekwencji tokenów są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="303e5-189">then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="303e5-190">W przeciwnym razie *Lista argumentów typu* nie jest uważana za część *prostej nazwy*, *dostęp do składowej* lub > *wskaźnik — dostęp do elementu członkowskiego*, nawet jeśli nie istnieje inna możliwa analiza sekwencji tokenów.</span><span class="sxs-lookup"><span data-stu-id="303e5-190">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or > *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="303e5-191">Należy pamiętać, że te reguły nie są stosowane podczas analizowania *listy argumentów typu* w *przestrzeni nazw lub-type-name* (§ 3,8).</span><span class="sxs-lookup"><span data-stu-id="303e5-191">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span> <span data-ttu-id="303e5-192">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="303e5-192">The statement</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="303e5-193">zgodnie z tą regułą, należy interpretować jako wywołanie `F` z jednym argumentem, który jest wywołaniem metody generycznej `G` z dwoma argumentami typu i jednym argumentem regularnym.</span><span class="sxs-lookup"><span data-stu-id="303e5-193">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="303e5-194">Instrukcje</span><span class="sxs-lookup"><span data-stu-id="303e5-194">The statements</span></span>
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> <span data-ttu-id="303e5-195">Każdy z nich będzie interpretowany jako wywołanie `F` z dwoma argumentami.</span><span class="sxs-lookup"><span data-stu-id="303e5-195">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="303e5-196">Instrukcja</span><span class="sxs-lookup"><span data-stu-id="303e5-196">The statement</span></span>
> ```csharp
> x = F < A > +y;
> ```
> <span data-ttu-id="303e5-197">będzie interpretowany jako operator mniejszości, operator większości i operator jednoargumentowy, tak jakby instrukcja była zapisywana `x = (F < A) > (+y)`, a nie jako *Nazwa prosta* z *listą argumentów typu* , po której następuje operator binarny Plus.</span><span class="sxs-lookup"><span data-stu-id="303e5-197">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple-name* with a *type-argument-list* followed by a binary plus operator.</span></span> <span data-ttu-id="303e5-198">W instrukcji</span><span class="sxs-lookup"><span data-stu-id="303e5-198">In the statement</span></span>
> ```csharp
> x = y is C<T> + z;
> ```
> <span data-ttu-id="303e5-199">tokeny `C<T>` są interpretowane jako *przestrzeń nazw lub-type-name* z *listą argumentów typu*.</span><span class="sxs-lookup"><span data-stu-id="303e5-199">the tokens `C<T>` are interpreted as a *namespace-or-type-name* with a *type-argument-list*.</span></span>

<span data-ttu-id="303e5-200">W programie C# wprowadzono wiele zmian, które nie są już wystarczające, aby obsłużyć złożoność języka.</span><span class="sxs-lookup"><span data-stu-id="303e5-200">There are a number of changes being introduced in C# 7 that make these disambiguation rules no longer sufficient to handle the complexity of the language.</span></span>

### <a name="out-variable-declarations"></a><span data-ttu-id="303e5-201">Deklaracje zmiennych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="303e5-201">Out variable declarations</span></span>

<span data-ttu-id="303e5-202">Teraz można zadeklarować zmienną w argumencie out:</span><span class="sxs-lookup"><span data-stu-id="303e5-202">It is now possible to declare a variable in an out argument:</span></span>

```csharp
M(out Type name);
```

<span data-ttu-id="303e5-203">Jednak typ może być ogólny:</span><span class="sxs-lookup"><span data-stu-id="303e5-203">However, the type may be generic:</span></span> 

```csharp
M(out A<B> name);
```

<span data-ttu-id="303e5-204">Ponieważ Gramatyka języka dla argumentu używa *wyrażenia*, ten kontekst podlega regule uściślania.</span><span class="sxs-lookup"><span data-stu-id="303e5-204">Since the language grammar for the argument uses *expression*, this context is subject to the disambiguation rule.</span></span> <span data-ttu-id="303e5-205">W tym przypadku `>` zamykające następuje *Identyfikator*, który nie jest jednym z tokenów, które zezwala na traktowanie jako *listy argumentów typu*.</span><span class="sxs-lookup"><span data-stu-id="303e5-205">In this case the closing `>` is followed by an *identifier*, which is not one of the tokens that permits it to be treated as a *type-argument-list*.</span></span> <span data-ttu-id="303e5-206">W związku z tym proponuję **dodać *Identyfikator* do zestawu tokenów, które wyzwalają Uściślanie do *listy argumentów typu*.**</span><span class="sxs-lookup"><span data-stu-id="303e5-206">I therefore propose to **add *identifier* to the set of tokens that triggers the disambiguation to a *type-argument-list*.**</span></span>

### <a name="tuples-and-deconstruction-declarations"></a><span data-ttu-id="303e5-207">Kolekcje i deklaracje dekonstrukcji</span><span class="sxs-lookup"><span data-stu-id="303e5-207">Tuples and deconstruction declarations</span></span>

<span data-ttu-id="303e5-208">Literał krotki działa dokładnie na ten sam problem.</span><span class="sxs-lookup"><span data-stu-id="303e5-208">A tuple literal runs into exactly the same issue.</span></span> <span data-ttu-id="303e5-209">Rozważ wyrażenie krotki</span><span class="sxs-lookup"><span data-stu-id="303e5-209">Consider the tuple expression</span></span>

```csharp
(A < B, C > D, E < F, G > H)
```

<span data-ttu-id="303e5-210">W ramach starych C# 6 reguł analizowania listy argumentów, będzie ona analizowana jako krotka z czterema elementami, rozpoczynając od `A < B` jako pierwszy.</span><span class="sxs-lookup"><span data-stu-id="303e5-210">Under the old C# 6 rules for parsing an argument list, this would parse as a tuple with four elements, starting with `A < B` as the first.</span></span> <span data-ttu-id="303e5-211">Jeśli jednak ta opcja pojawia się po lewej stronie dekonstrukcji, chcemy, aby takie Uściślanie zostało wyzwolone przez token *identyfikatora* , jak opisano powyżej:</span><span class="sxs-lookup"><span data-stu-id="303e5-211">However, when this appears on the left of a deconstruction, we want the disambiguation triggered by the *identifier* token as described above:</span></span>

```csharp
(A<B,C> D, E<F,G> H) = e;
```

<span data-ttu-id="303e5-212">Jest to Deklaracja dekonstrukcja, która deklaruje dwie zmienne, z których pierwszy jest typu `A<B,C>` i nazwanych `D`.</span><span class="sxs-lookup"><span data-stu-id="303e5-212">This is a deconstruction declaration which declares two variables, the first of which is of type `A<B,C>` and named `D`.</span></span> <span data-ttu-id="303e5-213">Innymi słowy, literał krotki zawiera dwa wyrażenia, z których każdy jest wyrażeniem deklaracji.</span><span class="sxs-lookup"><span data-stu-id="303e5-213">In other words, the tuple literal contains two expressions, each of which is a declaration expression.</span></span>

<span data-ttu-id="303e5-214">W celu uproszczenia specyfikacji i kompilatora zaproponuję przeanalizowanie tego literału krotki jako spójnej kolekcji w dowolnym miejscu (niezależnie od tego, czy jest ona wyświetlana po lewej stronie przypisania).</span><span class="sxs-lookup"><span data-stu-id="303e5-214">For simplicity of the specification and compiler, I propose that this tuple literal be parsed as a two-element tuple wherever it appears (whether or not it appears on the left-hand-side of an assignment).</span></span> <span data-ttu-id="303e5-215">Jest to naturalny wynik uściślania opisany w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="303e5-215">That would be a natural result of the disambiguation described in the previous section.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="303e5-216">Dopasowanie do wzorca</span><span class="sxs-lookup"><span data-stu-id="303e5-216">Pattern-matching</span></span>

<span data-ttu-id="303e5-217">Dopasowanie wzorca wprowadza nowy kontekst, w którym występuje niejednoznaczność typu wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="303e5-217">Pattern matching introduces a new context where the expression-type ambiguity arises.</span></span> <span data-ttu-id="303e5-218">Wcześniej po prawej stronie operatora `is` było typu.</span><span class="sxs-lookup"><span data-stu-id="303e5-218">Previously the right-hand-side of an `is` operator was a type.</span></span> <span data-ttu-id="303e5-219">Teraz może być typem lub wyrażeniem, a jeśli jest to typ, po którym może następować identyfikator.</span><span class="sxs-lookup"><span data-stu-id="303e5-219">Now it can be a type or expression, and if it is a type it may be followed by an identifier.</span></span> <span data-ttu-id="303e5-220">Pozwala to na techniczne zmianę znaczenia istniejącego kodu:</span><span class="sxs-lookup"><span data-stu-id="303e5-220">This can, technically, change the meaning of existing code:</span></span>

```csharp
var x = e is T < A > B;
```

<span data-ttu-id="303e5-221">Można to przeanalizować w ramach reguł języka C# 6 jako</span><span class="sxs-lookup"><span data-stu-id="303e5-221">This could be parsed under C#6 rules as</span></span>

```csharp
var x = ((e is T) < A) > B;
```

<span data-ttu-id="303e5-222">ale zgodnie z regułami języka C# 7 (z powyższymi uściślaniemi) zostałaby przeanalizowana jako</span><span class="sxs-lookup"><span data-stu-id="303e5-222">but under under C#7 rules (with the disambiguation proposed above) would be parsed as</span></span>

```csharp
var x = e is T<A> B;
```

<span data-ttu-id="303e5-223">który deklaruje zmienną `B` typu `T<A>`.</span><span class="sxs-lookup"><span data-stu-id="303e5-223">which declares a variable `B` of type `T<A>`.</span></span> <span data-ttu-id="303e5-224">Na szczęście kompilatory natywne i Roslyn mają usterkę, która daje w wyniku błąd składniowy w kodzie C# 6.</span><span class="sxs-lookup"><span data-stu-id="303e5-224">Fortunately, the native and Roslyn compilers have a bug whereby they give a syntax error on the C#6 code.</span></span> <span data-ttu-id="303e5-225">W związku z tym ta konkretna zmiana nie jest problemem.</span><span class="sxs-lookup"><span data-stu-id="303e5-225">Therefore this particular breaking change is not a concern.</span></span>

<span data-ttu-id="303e5-226">Dopasowanie do wzorca wprowadza dodatkowe tokeny, które powinny mieć na celu rozwiązanie niejednoznaczności w kierunku wybierania typu.</span><span class="sxs-lookup"><span data-stu-id="303e5-226">Pattern-matching introduces additional tokens that should drive the ambiguity resolution toward selecting a type.</span></span> <span data-ttu-id="303e5-227">Poniższe przykłady istniejącego prawidłowego kodu w języku C# 6 byłyby przerwane bez dodatkowych reguł uściślania:</span><span class="sxs-lookup"><span data-stu-id="303e5-227">The following examples of existing valid C#6 code would be broken without additional disambiguation rules:</span></span>

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a><span data-ttu-id="303e5-228">Proponowana zmiana reguły uściślania</span><span class="sxs-lookup"><span data-stu-id="303e5-228">Proposed change to the disambiguation rule</span></span>

<span data-ttu-id="303e5-229">Proponuję zmianę specyfikacji, aby zmienić listę niejednoznacznych tokenów z</span><span class="sxs-lookup"><span data-stu-id="303e5-229">I propose to revise the specification to change the list of disambiguating tokens from</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

<span data-ttu-id="303e5-230">na</span><span class="sxs-lookup"><span data-stu-id="303e5-230">to</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

<span data-ttu-id="303e5-231">A w niektórych kontekstach traktujemy *Identyfikator* jako niejednoznaczny token.</span><span class="sxs-lookup"><span data-stu-id="303e5-231">And, in certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="303e5-232">Te konteksty są, w których kolejność niejednoznacznych tokenów jest bezpośrednio poprzedzona przez jedno ze słów kluczowych `is`, `case`lub `out`lub występuje podczas analizowania pierwszego elementu literału krotki (w którym przypadku tokeny są poprzedzone `(` lub `:`, a następnie identyfikatorem następuje `,`) lub po kolejnym elemencie literału krotki.</span><span class="sxs-lookup"><span data-stu-id="303e5-232">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case`, or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>

### <a name="modified-disambiguation-rule"></a><span data-ttu-id="303e5-233">Zmodyfikowano regułę uściślania</span><span class="sxs-lookup"><span data-stu-id="303e5-233">Modified disambiguation rule</span></span>

<span data-ttu-id="303e5-234">Zmieniona reguła uściślania będzie wyglądać podobnie do tego</span><span class="sxs-lookup"><span data-stu-id="303e5-234">The revised disambiguation rule would be something like this</span></span>

> <span data-ttu-id="303e5-235">Jeśli sekwencja tokenów może być analizowana (w kontekście) jako *prosta nazwa* (§ 7.6.3), *dostęp do elementu członkowskiego* (§ 7.6.5) lub *wskaźnik — dostęp do elementów członkowskich* (§ 18.5.2) kończący się na *listą argumentów typu* (§ 4.4.1), token bezpośrednio po zamykającym tokenie `>` jest sprawdzany, aby sprawdzić, czy jest</span><span class="sxs-lookup"><span data-stu-id="303e5-235">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined, to see if it is</span></span>
> - <span data-ttu-id="303e5-236">Jeden z `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; oraz</span><span class="sxs-lookup"><span data-stu-id="303e5-236">One of `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; or</span></span>
> - <span data-ttu-id="303e5-237">Jeden z operatorów relacyjnych `<  >  <=  >=  is as`; oraz</span><span class="sxs-lookup"><span data-stu-id="303e5-237">One of the relational operators `<  >  <=  >=  is as`; or</span></span>
> - <span data-ttu-id="303e5-238">Słowo kluczowe zapytania kontekstowego pojawia się wewnątrz wyrażenia zapytania; oraz</span><span class="sxs-lookup"><span data-stu-id="303e5-238">A contextual query keyword appearing inside a query expression; or</span></span>
> - <span data-ttu-id="303e5-239">W niektórych kontekstach traktujemy *Identyfikator* jako niejednoznaczny token.</span><span class="sxs-lookup"><span data-stu-id="303e5-239">In certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="303e5-240">Te konteksty są, w których kolejność niejednoznacznych tokenów jest bezpośrednio poprzedzona przez jedno z słów kluczowych `is`, `case` lub `out`lub występuje podczas analizowania pierwszego elementu literału krotki (w którym przypadku tokeny są poprzedzone przez `(` lub `:`, a następnie identyfikatorem następuje `,`) lub kolejnego elementu literału krotki.</span><span class="sxs-lookup"><span data-stu-id="303e5-240">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case` or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>
> 
> <span data-ttu-id="303e5-241">Jeśli Poniższy token znajduje się na tej liście lub identyfikator w tym kontekście, a następnie *Lista argumentów typu* jest zachowywana w ramach *prostej nazwy*, *dostęp do elementu członkowskiego* lub *wskaźnika — dostęp do elementu członkowskiego* i wszelkie inne możliwe analizy sekwencji tokenów są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="303e5-241">If the following token is among this list, or an identifier in such a context, then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or  *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span>  <span data-ttu-id="303e5-242">W przeciwnym razie *Lista argumentów typu* nie jest uważana za część *prostej nazwy*, *dostępu do składowej* lub *wskaźnika do elementu członkowskiego*, nawet jeśli nie istnieje inna możliwa analiza sekwencji tokenów.</span><span class="sxs-lookup"><span data-stu-id="303e5-242">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="303e5-243">Należy pamiętać, że te reguły nie są stosowane podczas analizowania *listy argumentów typu* w *przestrzeni nazw lub-type-name* (§ 3,8).</span><span class="sxs-lookup"><span data-stu-id="303e5-243">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span>

### <a name="breaking-changes-due-to-this-proposal"></a><span data-ttu-id="303e5-244">Przerywanie zmian spowodowanych tą propozycją</span><span class="sxs-lookup"><span data-stu-id="303e5-244">Breaking changes due to this proposal</span></span>

<span data-ttu-id="303e5-245">Nie są znane żadne istotne zmiany spowodowane tym proponowaną regułą uściślania.</span><span class="sxs-lookup"><span data-stu-id="303e5-245">No breaking changes are known due to this proposed disambiguation rule.</span></span>

### <a name="interesting-examples"></a><span data-ttu-id="303e5-246">Interesujące przykłady</span><span class="sxs-lookup"><span data-stu-id="303e5-246">Interesting examples</span></span>

<span data-ttu-id="303e5-247">Poniżej przedstawiono interesujące wyniki tych reguł uściślania:</span><span class="sxs-lookup"><span data-stu-id="303e5-247">Here are some interesting results of these disambiguation rules:</span></span>

<span data-ttu-id="303e5-248">Wyrażenie `(A < B, C > D)` jest krotką zawierającą dwa elementy, każde porównanie.</span><span class="sxs-lookup"><span data-stu-id="303e5-248">The expression `(A < B, C > D)` is a tuple with two elements, each a comparison.</span></span>

<span data-ttu-id="303e5-249">Wyrażenie `(A<B,C> D, E)` jest krotką z dwoma elementami, z których pierwszy jest wyrażeniem deklaracji.</span><span class="sxs-lookup"><span data-stu-id="303e5-249">The expression `(A<B,C> D, E)` is a tuple with two elements, the first of which is a declaration expression.</span></span>

<span data-ttu-id="303e5-250">Wywołanie `M(A < B, C > D, E)` ma trzy argumenty.</span><span class="sxs-lookup"><span data-stu-id="303e5-250">The invocation `M(A < B, C > D, E)` has three arguments.</span></span>

<span data-ttu-id="303e5-251">`M(out A<B,C> D, E)` wywołania ma dwa argumenty, z których pierwszy jest deklaracją `out`.</span><span class="sxs-lookup"><span data-stu-id="303e5-251">The invocation `M(out A<B,C> D, E)` has two arguments, the first of which is an `out` declaration.</span></span>

<span data-ttu-id="303e5-252">Wyrażenie `e is A<B> C` używa wyrażenia deklaracji.</span><span class="sxs-lookup"><span data-stu-id="303e5-252">The expression `e is A<B> C` uses a declaration expression.</span></span>

<span data-ttu-id="303e5-253">Etykieta przypadku `case A<B> C:` używa wyrażenia deklaracji.</span><span class="sxs-lookup"><span data-stu-id="303e5-253">The case label `case A<B> C:` uses a declaration expression.</span></span>

## <a name="some-examples-of-pattern-matching"></a><span data-ttu-id="303e5-254">Przykłady dopasowania do wzorca</span><span class="sxs-lookup"><span data-stu-id="303e5-254">Some examples of pattern matching</span></span>

### <a name="is-as"></a><span data-ttu-id="303e5-255">Jest jako</span><span class="sxs-lookup"><span data-stu-id="303e5-255">Is-As</span></span>

<span data-ttu-id="303e5-256">Możemy zastąpić idiom</span><span class="sxs-lookup"><span data-stu-id="303e5-256">We can replace the idiom</span></span>

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

<span data-ttu-id="303e5-257">Z nieco bardziej zwięzłe i bezpośrednie</span><span class="sxs-lookup"><span data-stu-id="303e5-257">With the slightly more concise and direct</span></span>

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a><span data-ttu-id="303e5-258">Testowanie dopuszczające wartość null</span><span class="sxs-lookup"><span data-stu-id="303e5-258">Testing nullable</span></span>

<span data-ttu-id="303e5-259">Możemy zastąpić idiom</span><span class="sxs-lookup"><span data-stu-id="303e5-259">We can replace the idiom</span></span>

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

<span data-ttu-id="303e5-260">Z nieco bardziej zwięzłe i bezpośrednie</span><span class="sxs-lookup"><span data-stu-id="303e5-260">With the slightly more concise and direct</span></span>

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a><span data-ttu-id="303e5-261">Uproszczenie arytmetyczne</span><span class="sxs-lookup"><span data-stu-id="303e5-261">Arithmetic simplification</span></span>

<span data-ttu-id="303e5-262">Załóżmy, że definiujemy zestaw typów cyklicznych do reprezentowania wyrażeń (na oddzielną propozycję):</span><span class="sxs-lookup"><span data-stu-id="303e5-262">Suppose we define a set of recursive types to represent expressions (per a separate proposal):</span></span>

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

<span data-ttu-id="303e5-263">Teraz możemy zdefiniować funkcję, która będzie obliczać (niezredukowany) pochodną wyrażenia:</span><span class="sxs-lookup"><span data-stu-id="303e5-263">Now we can define a function to compute the (unreduced) derivative of an expression:</span></span>

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

<span data-ttu-id="303e5-264">Uproszczenie wyrażenia demonstruje wzorce pozycyjne:</span><span class="sxs-lookup"><span data-stu-id="303e5-264">An expression simplifier demonstrates positional patterns:</span></span>

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
