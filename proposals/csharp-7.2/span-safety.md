---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485043"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a><span data-ttu-id="ffa7c-101">Wymuszanie czasu kompilowania bezpieczeństwa dla typów typu referencyjnego</span><span class="sxs-lookup"><span data-stu-id="ffa7c-101">Compile time enforcement of safety for ref-like types</span></span>

## <a name="introduction"></a><span data-ttu-id="ffa7c-102">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="ffa7c-102">Introduction</span></span>

<span data-ttu-id="ffa7c-103">Głównym powodem dodatkowych reguł bezpieczeństwa podczas pracy z typami, takimi jak `Span<T>` i `ReadOnlySpan<T>`, jest to, że takie typy muszą być ograniczone do stosu wykonywania.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-103">The main reason for the additional safety rules when dealing with types like `Span<T>` and `ReadOnlySpan<T>` is that such types must be confined to the execution stack.</span></span>
 
<span data-ttu-id="ffa7c-104">Istnieją dwa powody, dla których `Span<T>` i podobne typy muszą być typami tylko stosu.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-104">There are two reasons why `Span<T>` and similar types must be a stack-only types.</span></span>

1. <span data-ttu-id="ffa7c-105">`Span<T>` jest semantycznie strukturą zawierającą odwołanie i `(ref T data, int length)`zakresem.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-105">`Span<T>` is semantically a struct containing a reference and a range - `(ref T data, int length)`.</span></span> <span data-ttu-id="ffa7c-106">Niezależnie od rzeczywistej implementacji, zapisy do takiej struktury nie są niepodzielne.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-106">Regardless of actual implementation, writes to such struct would not be atomic.</span></span> <span data-ttu-id="ffa7c-107">Współbieżne "rozerwanie" takiej struktury doprowadziłoby do `length` nie pasuje do `data`, powodującego dostęp poza zakresem i naruszenia bezpieczeństwa typów, co ostatecznie może doprowadzić do uszkodzenia sterty GC w pozornie "bezpiecznym" kodzie.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-107">Concurrent "tearing" of such struct would lead to the possibility of `length` not matching the `data`, causing out-of-range accesses and type-safety violations, which ultimately could result in GC heap corruption in seemingly "safe" code.</span></span>
2. <span data-ttu-id="ffa7c-108">Niektóre implementacje `Span<T>` dosłownie zawierają zarządzany wskaźnik w jednym z jego pól.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-108">Some implementations of `Span<T>` literally contain a managed pointer in one of its fields.</span></span> <span data-ttu-id="ffa7c-109">Wskaźniki zarządzane nie są obsługiwane jako pola sterty i kodu, które zarządzają, aby umieścić zarządzany wskaźnik na stercie GC zazwyczaj awarie w czasie JIT.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-109">Managed pointers are not supported as fields of heap objects and code that manages to put a managed pointer on the GC heap typically crashes at JIT time.</span></span>

<span data-ttu-id="ffa7c-110">Wszystkie powyższe problemy są rozwiązywane, jeśli wystąpienia `Span<T>` są ograniczone do istniejących tylko na stosie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-110">All the above problems would be alleviated if instances of `Span<T>` are constrained to exist only on the execution stack.</span></span> 

<span data-ttu-id="ffa7c-111">Dodatkowy problem występuje ze względu na składanie.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-111">An additional problem arises due to composition.</span></span> <span data-ttu-id="ffa7c-112">Zalecane jest utworzenie bardziej złożonych typów danych, które mogłyby osadzić `Span<T>` i `ReadOnlySpan<T>` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-112">It would be generally desirable to build more complex data types that would embed `Span<T>` and `ReadOnlySpan<T>` instances.</span></span> <span data-ttu-id="ffa7c-113">Takie złożone typy muszą być strukturami i współdzielić wszystkie zagrożenia i wymagania dotyczące `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-113">Such composite types would have to be structs and would share all the hazards and requirements of `Span<T>`.</span></span> <span data-ttu-id="ffa7c-114">W związku z tym opisane tutaj reguły bezpieczeństwa powinny być wyświetlane jako mające zastosowanie do całego zakresu **_typów referencyjnych_** .</span><span class="sxs-lookup"><span data-stu-id="ffa7c-114">As a result the safety rules described here should be viewed as applicable to the whole range of **_ref-like types_**.</span></span>

<span data-ttu-id="ffa7c-115">[Specyfikacja języka roboczego](#draft-language-specification) jest zaprojektowana w celu zapewnienia, że wartości typu "ref" są wykonywane tylko na stosie.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-115">The [draft language specification](#draft-language-specification) is intended to ensure that values of a ref-like type occurs only on the stack.</span></span>

## <a name="generalized-ref-like-types-in-source-code"></a><span data-ttu-id="ffa7c-116">Uogólnione typy `ref-like` w kodzie źródłowym</span><span class="sxs-lookup"><span data-stu-id="ffa7c-116">Generalized `ref-like` types in source code</span></span>

<span data-ttu-id="ffa7c-117">struktury `ref-like` są jawnie oznaczone w kodzie źródłowym przy użyciu modyfikatora `ref`:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-117">`ref-like` structs are explicitly marked in the source code using `ref` modifier:</span></span>

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

<span data-ttu-id="ffa7c-118">Wyznaczenie struktury jako ref-like umożliwi strukturze, która ma pola wystąpienia przypominającego odwołania, a także wprowadzi wszystkie wymagania dotyczące typów referencyjnych, które mają zastosowanie do struktury.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-118">Designating a struct as ref-like will allow the struct to have ref-like instance fields and will also make all the requirements of ref-like types applicable to the struct.</span></span> 

## <a name="metadata-representation-or-ref-like-structs"></a><span data-ttu-id="ffa7c-119">Reprezentacja metadanych lub struktury przypominające odwołania</span><span class="sxs-lookup"><span data-stu-id="ffa7c-119">Metadata representation or ref-like structs</span></span>

<span data-ttu-id="ffa7c-120">Struktury typu ref-like zostaną oznaczone atrybutem **System. Runtime. CompilerServices. IsRefLikeAttribute** .</span><span class="sxs-lookup"><span data-stu-id="ffa7c-120">Ref-like structs will be marked with **System.Runtime.CompilerServices.IsRefLikeAttribute** attribute.</span></span>

<span data-ttu-id="ffa7c-121">Ten atrybut zostanie dodany do wspólnych bibliotek podstawowych, takich jak `mscorlib`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-121">The attribute will be added to common base libraries such as `mscorlib`.</span></span> <span data-ttu-id="ffa7c-122">W przypadku, gdy atrybut nie jest dostępny, kompilator generuje wewnętrzne, podobne do innych atrybutów osadzonych na żądanie, takich jak `IsReadOnlyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-122">In a case if the attribute is not available, compiler will generate an internal one similarly to other embedded-on-demand attributes such as `IsReadOnlyAttribute`.</span></span>

<span data-ttu-id="ffa7c-123">Zostanie podjęta dodatkowa miara, która uniemożliwia korzystanie z struktur typu referencyjnego w kompilatorach, które nie znają reguł bezpieczeństwa (obejmuje C# to kompilatory przed tą, w której ta funkcja jest zaimplementowana).</span><span class="sxs-lookup"><span data-stu-id="ffa7c-123">An additional measure will be taken to prevent the use of ref-like structs in compilers not familiar with the safety rules (this includes C# compilers prior to the one in which this feature is implemented).</span></span> 

<span data-ttu-id="ffa7c-124">Bez innych dobrych alternatyw, które pracują w starych kompilatorach bez obsługi, atrybut `Obsolete` ze znanym ciągiem zostanie dodany do wszystkich struktur typu ref.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-124">Having no other good alternatives that work in old compilers without servicing, an `Obsolete` attribute with a known string will be added to all ref-like structs.</span></span> <span data-ttu-id="ffa7c-125">Kompilatory, które wiedzą, jak używać typów odwołań typu REF, zignorują tę konkretną formę `Obsolete`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-125">Compilers that know how to use ref-like types will ignore this particular form of `Obsolete`.</span></span>

<span data-ttu-id="ffa7c-126">Typowa reprezentacja metadanych:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-126">A typical metadata representation:</span></span>

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

<span data-ttu-id="ffa7c-127">Uwaga: nie jest to cel, aby uczynić go tak, aby wszelkie użycie typów odwołań typu ref na starych kompilatorów kończyło się niepowodzeniem 100%.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-127">NOTE: it is not the goal to make it so that any use of ref-like types on old compilers fails 100%.</span></span> <span data-ttu-id="ffa7c-128">Jest to trudne do osiągnięcia i nie jest absolutnie konieczne.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-128">That is hard to achieve and is not strictly necessary.</span></span> <span data-ttu-id="ffa7c-129">Na przykład zawsze można uzyskać dostęp do `Obsolete` przy użyciu kodu dynamicznego lub, na przykład, utworzyć tablicę typów typu referencyjnego za pomocą odbicia.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-129">For example there would always be a way to get around the `Obsolete` using dynamic code or, for example, creating an array of ref-like types through reflection.</span></span>

<span data-ttu-id="ffa7c-130">W szczególności, jeśli użytkownik chce w rzeczywistości umieścić atrybut `Obsolete` lub `Deprecated` w typie podobnym do, nie zostanie wybrana opcja inna niż nie emituje wstępnie zdefiniowanego, ponieważ atrybut `Obsolete` nie może zostać zastosowany więcej niż raz.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-130">In particular, if user wants to actually put an `Obsolete` or `Deprecated` attribute on a ref-like type, we will have no choice other than not emitting the predefined one since `Obsolete` attribute cannot be applied more than once..</span></span>  

## <a name="examples"></a><span data-ttu-id="ffa7c-131">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-131">Examples:</span></span>

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a><span data-ttu-id="ffa7c-132">Specyfikacja języka wersji roboczej</span><span class="sxs-lookup"><span data-stu-id="ffa7c-132">Draft language specification</span></span>

<span data-ttu-id="ffa7c-133">Poniżej opisano zestaw reguł bezpieczeństwa dla typów referencyjnych (`ref struct`s), aby upewnić się, że wartości tych typów występują tylko na stosie.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-133">Below we describe a set of safety rules for ref-like types (`ref struct`s) to ensure that values of these types occur only on the stack.</span></span> <span data-ttu-id="ffa7c-134">Inny, prostszy zestaw reguł bezpieczeństwa byłby możliwy, jeśli nie można przekazywać elementów lokalnych przez odwołanie.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-134">A different, simpler set of safety rules would be possible if locals cannot be passed by reference.</span></span> <span data-ttu-id="ffa7c-135">Ta specyfikacja umożliwia również bezpieczne ponowne przypisanie odwołań lokalnych.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-135">This specification would also permit the safe reassignment of ref locals.</span></span>

### <a name="overview"></a><span data-ttu-id="ffa7c-136">Omówienie</span><span class="sxs-lookup"><span data-stu-id="ffa7c-136">Overview</span></span>

<span data-ttu-id="ffa7c-137">Jest on kojarzony z każdym wyrażeniem w czasie kompilacji, koncepcji zakresu, w którym wyrażenie jest dozwolone, do którego jest dozwolony, "bezpieczne do ucieczki".</span><span class="sxs-lookup"><span data-stu-id="ffa7c-137">We associate with each expression at compile-time the concept of what scope that expression is permitted to escape to, "safe-to-escape".</span></span> <span data-ttu-id="ffa7c-138">Podobnie w przypadku każdej lvalue utrzymujemy koncepcję zakresu, do którego odnosi się odwołanie, do "ref-Safe-to-Escape".</span><span class="sxs-lookup"><span data-stu-id="ffa7c-138">Similarly, for each lvalue we maintain a concept of what scope a reference to it is permitted to escape to, "ref-safe-to-escape".</span></span> <span data-ttu-id="ffa7c-139">Dla danego wyrażenia lvalue mogą się one różnić.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-139">For a given lvalue expression, these may be different.</span></span>

<span data-ttu-id="ffa7c-140">Są one analogiczne do "bezpiecznej do zwrócenia" funkcji ref locales, ale jest bardziej szczegółowy.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-140">These are analogous to the "safe to return" of the ref locals feature, but it is more fine-grained.</span></span> <span data-ttu-id="ffa7c-141">Gdzie wyrażenie "Safe-to-Return" jest rejestrowane tylko w przypadku, gdy (lub nie) może to zmienić metodę otaczającą jako całość, rekordy bezpiecznych do ucieczki, w których zakres może przekroczyć (w jakim zakresie może nie ucieczki).</span><span class="sxs-lookup"><span data-stu-id="ffa7c-141">Where the "safe-to-return" of an expression records only whether (or not) it may escape the enclosing method as a whole, the safe-to-escape records which scope it may escape to (which scope it may not escape beyond).</span></span> <span data-ttu-id="ffa7c-142">Podstawowy mechanizm bezpieczeństwa jest wymuszany w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-142">The basic safety mechanism is enforced as follows.</span></span> <span data-ttu-id="ffa7c-143">Przypisanie z wyrażenia E1 z zakresem bezpiecznego do ucieczki S1 do (lvalue) Expression E2 z zakresem "bezpieczny do-ucieczk" S2, występuje błąd, jeśli S2 jest szerszym zakresem niż S1.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-143">Given an assignment from an expression E1 with a safe-to-escape scope S1, to an (lvalue) expression E2 with safe-to-escape scope S2, it is an error if S2 is a wider scope than S1.</span></span> <span data-ttu-id="ffa7c-144">Przez konstrukcję dwa zakresy S1 i S2 znajdują się w relacji zagnieżdżania, ponieważ wyrażenie prawne zawsze jest bezpieczne do zwrócenia z jakiegoś zakresu otaczającego wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-144">By construction, the two scopes S1 and S2 are in a nesting relationship, because a legal expression is always safe-to-return from some scope enclosing the expression.</span></span>

<span data-ttu-id="ffa7c-145">W odpowiednim czasie, na potrzeby analizy, do obsługi tylko dwóch zakresów — zewnętrznego względem metody i zakresu najwyższego poziomu metody.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-145">For the time being it is sufficient, for the purpose of the analysis, to support just two scopes - external to the method, and top-level scope of the method.</span></span> <span data-ttu-id="ffa7c-146">Wynika to z faktu, że nie można tworzyć wartości typu referencyjnego z zakresami wewnętrznymi, a odwołania lokalne nie obsługują ponownego przypisywania.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-146">That is because ref-like values with inner scopes cannot be created and ref locals do not support re-assignment.</span></span> <span data-ttu-id="ffa7c-147">Reguły te mogą jednak obsługiwać więcej niż dwa poziomy zakresu.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-147">The rules, however, can support more than two scope levels.</span></span>

<span data-ttu-id="ffa7c-148">Precyzyjne reguły obliczania stanu *bezpiecznego i powrotu* wyrażenia oraz reguły regulujące warunki prawne wyrażeń, należy stosować.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-148">The precise rules for computing the *safe-to-return* status of an expression, and the rules governing the legality of expressions, follow.</span></span>

### <a name="ref-safe-to-escape"></a><span data-ttu-id="ffa7c-149">odwołanie-bezpieczne-do-ucieczki</span><span class="sxs-lookup"><span data-stu-id="ffa7c-149">ref-safe-to-escape</span></span>

<span data-ttu-id="ffa7c-150">Wartość *ref-Safe-to-Escape* jest zakresem zawierającym wyrażenie lvalue, do którego jest bezpieczna w przypadku odwołania do lvalue.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-150">The *ref-safe-to-escape* is a scope, enclosing an lvalue expression, to which it is safe for a ref to the lvalue to escape to.</span></span> <span data-ttu-id="ffa7c-151">Jeśli zakresem jest cała Metoda, mówimy, że odwołanie do lvalue jest *bezpieczne do zwrócenia* z metody.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-151">If that scope is the entire method, we say that a ref to the lvalue is *safe to return* from the method.</span></span>

### <a name="safe-to-escape"></a><span data-ttu-id="ffa7c-152">bezpieczna do wyjścia</span><span class="sxs-lookup"><span data-stu-id="ffa7c-152">safe-to-escape</span></span>

<span data-ttu-id="ffa7c-153">*Bezpieczny do ucieczki* jest zakresem zawierającym wyrażenie, które jest bezpieczne dla wartości, do której ma zostać zmieniony.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-153">The *safe-to-escape* is a scope, enclosing an expression, to which it is safe for the value to escape to.</span></span> <span data-ttu-id="ffa7c-154">Jeśli zakresem jest cała Metoda, Załóżmy, że wartość jest *bezpieczna do zwrócenia* z metody.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-154">If that scope is the entire method, we say that a the value is *safe to return* from the method.</span></span>

<span data-ttu-id="ffa7c-155">Wyrażenie, którego typ nie jest typem `ref struct`, jest *bezpieczne do zwrócenia* z całej otaczającej metody.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-155">An expression whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span> <span data-ttu-id="ffa7c-156">W przeciwnym razie odwołujemy się do poniższych reguł.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-156">Otherwise we refer to the rules below.</span></span>

#### <a name="parameters"></a><span data-ttu-id="ffa7c-157">Parametry</span><span class="sxs-lookup"><span data-stu-id="ffa7c-157">Parameters</span></span>

<span data-ttu-id="ffa7c-158">Lvalue wyznaczania parametru formalnego to *ref-Safe-to-Escape* (poprzez odwołanie) w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-158">An lvalue designating a formal parameter is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="ffa7c-159">Jeśli parametr jest parametrem `ref`, `out`lub `in`, jest to metoda *ref-Safe-to-ucieczki* z całej metody (np. przez instrukcję `return ref`); przypadku</span><span class="sxs-lookup"><span data-stu-id="ffa7c-159">If the parameter is a `ref`, `out`, or `in` parameter, it is *ref-safe-to-escape* from the entire method (e.g. by a `return ref` statement); otherwise</span></span>
- <span data-ttu-id="ffa7c-160">Jeśli parametr jest `this`m parametrem typu struktury, jest to *odwołanie-bezpieczne-do-ucieczki* do zakresu najwyższego poziomu metody (ale nie z całej metody); [Przykład](#struct-this-escape)</span><span class="sxs-lookup"><span data-stu-id="ffa7c-160">If the parameter is the `this` parameter of a struct type, it is *ref-safe-to-escape* to the top-level scope of the method (but not from the entire method itself); [Sample](#struct-this-escape)</span></span>
- <span data-ttu-id="ffa7c-161">W przeciwnym razie parametr jest parametrem wartości i jest *typu ref-Safe-to-Escape* do zakresu najwyższego poziomu metody (ale nie z samej metody).</span><span class="sxs-lookup"><span data-stu-id="ffa7c-161">Otherwise the parameter is a value parameter, and it is *ref-safe-to-escape* to the top-level scope of the method (but not from the method itself).</span></span>

<span data-ttu-id="ffa7c-162">Wyrażenie, które jest rvalue, wyznaczające użycie parametru formalnego, jest *bezpieczne do ucieczki* (według wartości) z całej metody (np. przez instrukcję `return`).</span><span class="sxs-lookup"><span data-stu-id="ffa7c-162">An expression that is an rvalue designating the use of a formal parameter is *safe-to-escape* (by value) from the entire method (e.g. by a `return` statement).</span></span> <span data-ttu-id="ffa7c-163">Dotyczy to również parametru `this`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-163">This applies to the `this` parameter as well.</span></span>

#### <a name="locals"></a><span data-ttu-id="ffa7c-164">Zmienne lokalne</span><span class="sxs-lookup"><span data-stu-id="ffa7c-164">Locals</span></span>

<span data-ttu-id="ffa7c-165">Lvalue wyznaczający zmienną lokalną to *ref-Safe-to-Escape* (poprzez odwołanie) w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-165">An lvalue designating a local variable is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="ffa7c-166">Jeśli zmienna jest zmienną `ref`, jej wartość *ref-Safe-to-ucieczki* jest pobierana z wyrażenia *ref-Safe-* to-ucieczki w jego wyrażeniu inicjowania; przypadku</span><span class="sxs-lookup"><span data-stu-id="ffa7c-166">If the variable is a `ref` variable, then its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of its initializing expression; otherwise</span></span>
- <span data-ttu-id="ffa7c-167">Zmienna ma wartość *ref-Safe-to-ucieczki* zakresu, w którym został zadeklarowany.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-167">The variable is *ref-safe-to-escape* the scope in which it was declared.</span></span>

<span data-ttu-id="ffa7c-168">Wyrażenie, które jest rvalue, wyznaczające użycie zmiennej lokalnej jest *bezpieczne do ucieczki* (według wartości) w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-168">An expression that is an rvalue designating the use of a local variable is *safe-to-escape* (by value) as follows:</span></span>
- <span data-ttu-id="ffa7c-169">Jednak Powyższa reguła ogólna, której typ nie jest typem `ref struct`, jest *bezpieczna do zwrócenia* z całej otaczającej metody.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-169">But the general rule above, a local whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="ffa7c-170">Jeśli zmienna jest zmienną iteracji pętli `foreach`, zakres *bezpiecznego do ucieczki* zmiennej jest taka sama jak wartość " *bezpieczna do ucieczki* " `foreach` wyrażeniu pętli.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-170">If the variable is an iteration variable of a `foreach` loop, then the variable's *safe-to-escape* scope is the same as the *safe-to-escape* of the `foreach` loop's expression.</span></span>
- <span data-ttu-id="ffa7c-171">Lokalny typ `ref struct` i niezainicjowany w punkcie deklaracji jest *bezpieczny-do-Return* z całej otaczającej metody.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-171">A local of `ref struct` type and uninitialized at the point of declaration is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="ffa7c-172">W przeciwnym razie typ zmiennej jest typu `ref struct`, a deklaracja zmiennej wymaga inicjatora.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-172">Otherwise the variable's type is a `ref struct` type, and the variable's declaration requires an initializer.</span></span> <span data-ttu-id="ffa7c-173">Zakres *bezpiecznej do ucieczki* zmiennej jest taki sam jak *bezpieczny dla* niego inicjatora.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-173">The variable's *safe-to-escape* scope is the same as the *safe-to-escape* of its initializer.</span></span>

#### <a name="field-reference"></a><span data-ttu-id="ffa7c-174">Odwołanie do pola</span><span class="sxs-lookup"><span data-stu-id="ffa7c-174">Field reference</span></span>

<span data-ttu-id="ffa7c-175">Lvalue wyznaczający odwołanie do pola, `e.F`, to *ref-Safe-to-Escape* (poprzez odwołanie) w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-175">An lvalue designating a reference to a field, `e.F`, is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="ffa7c-176">Jeśli `e` ma typ referencyjny, jest to metoda *ref-Safe-to-Escape* z całej metody; przypadku</span><span class="sxs-lookup"><span data-stu-id="ffa7c-176">If `e` is of a reference type, it is *ref-safe-to-escape* from the entire method; otherwise</span></span>
- <span data-ttu-id="ffa7c-177">Jeśli `e` ma typ wartości, jego wartość *ref-Safe-to-Escape* jest pobierana z `e`*ref-Safe-to* -ucieczki.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-177">If `e` is of a value type, its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of `e`.</span></span>

<span data-ttu-id="ffa7c-178">Rvalue wyznaczający odwołanie do pola, `e.F`, ma zakres *bezpieczny-do-ucieczki* , który jest taki sam jak *bezpieczny* dla `e`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-178">An rvalue designating a reference to a field, `e.F`, has a *safe-to-escape* scope that is the same as the *safe-to-escape* of `e`.</span></span>

#### <a name="operators-including-"></a><span data-ttu-id="ffa7c-179">Operatory, w tym `?:`</span><span class="sxs-lookup"><span data-stu-id="ffa7c-179">Operators including `?:`</span></span>

<span data-ttu-id="ffa7c-180">Aplikacja operatora zdefiniowanego przez użytkownika jest traktowana jako wywołanie metody.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-180">The application of a user-defined operator is treated as a method invocation.</span></span>

<span data-ttu-id="ffa7c-181">Dla operatora, który daje element rvalue, taki jak `e1 + e2` lub `c ? e1 : e2`, *bezpieczna do ucieczki* wyniku jest najwęższym zakresem od *bezpiecznych do ucieczki* operandów operatora.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-181">For an operator that yields an rvalue, such as `e1 + e2` or `c ? e1 : e2`, the *safe-to-escape* of the result is the narrowest scope among the *safe-to-escape* of the operands of the operator.</span></span>  <span data-ttu-id="ffa7c-182">W wyniku tego dla operatora jednoargumentowego, który daje rvalue, taki jak `+e`, *bezpieczna do ucieczki* wyniku jest *bezpieczna do ucieczki* operandu.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-182">As a consequence, for a unary operator that yields an rvalue, such as `+e`, the *safe-to-escape* of the result is the *safe-to-escape* of the operand.</span></span>

<span data-ttu-id="ffa7c-183">Dla operatora, który daje lvalue, taki jak `c ? ref e1 : ref e2`</span><span class="sxs-lookup"><span data-stu-id="ffa7c-183">For an operator that yields an lvalue, such as `c ? ref e1 : ref e2`</span></span>
- <span data-ttu-id="ffa7c-184">wartość *ref-Safe-to-Escape* wyniku to najwęższy zakres między *odwołaniami typu ref-Safe-do-ucieczki* operatora.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-184">the *ref-safe-to-escape* of the result is the narrowest scope among the *ref-safe-to-escape* of the operands of the operator.</span></span>
- <span data-ttu-id="ffa7c-185">*bezpieczne do ucieczki* operandy muszą zgadzać się, a to jest *bezpieczna do ucieczki* wyników lvalue.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-185">the *safe-to-escape* of the operands must agree, and that is the *safe-to-escape* of the resulting lvalue.</span></span>

#### <a name="method-invocation"></a><span data-ttu-id="ffa7c-186">Wywołanie metody</span><span class="sxs-lookup"><span data-stu-id="ffa7c-186">Method invocation</span></span>

<span data-ttu-id="ffa7c-187">Lvalue wynikający z wywołania metody zwracającej odwołanie `e1.M(e2, ...)` ma wartość *ref-Safe-to-ucieczki* najmniejszy z następujących zakresów:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-187">An lvalue resulting from a ref-returning method invocation `e1.M(e2, ...)` is *ref-safe-to-escape* the smallest of the following scopes:</span></span>
- <span data-ttu-id="ffa7c-188">Cała zaotaczająca Metoda</span><span class="sxs-lookup"><span data-stu-id="ffa7c-188">The entire enclosing method</span></span>
- <span data-ttu-id="ffa7c-189">*odwołania* do wszystkich argumentów `ref` i `out` (z wyłączeniem odbiorcy)</span><span class="sxs-lookup"><span data-stu-id="ffa7c-189">the *ref-safe-to-escape* of all `ref` and `out` argument expressions (excluding the receiver)</span></span>
- <span data-ttu-id="ffa7c-190">Dla każdego `in` parametru metody, jeśli istnieje odpowiednie wyrażenie, które jest lvalue, jego *ref-Safe-to-Escape*, w przeciwnym razie najbliższy zakres otaczający</span><span class="sxs-lookup"><span data-stu-id="ffa7c-190">For each `in` parameter of the method, if there is a corresponding expression that is an lvalue, its *ref-safe-to-escape*, otherwise the nearest enclosing scope</span></span>
- <span data-ttu-id="ffa7c-191">*bezpieczne* dla wszystkich wyrażeń argumentów (w tym odbiorcy)</span><span class="sxs-lookup"><span data-stu-id="ffa7c-191">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

> <span data-ttu-id="ffa7c-192">Uwaga: ostatni punktor jest niezbędny do obsługi kodu, takiego jak</span><span class="sxs-lookup"><span data-stu-id="ffa7c-192">Note: the last bullet is necessary to handle code such as</span></span>
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> <span data-ttu-id="ffa7c-193">lub</span><span class="sxs-lookup"><span data-stu-id="ffa7c-193">or</span></span>
> ```csharp
> return ref M(sp, 0);
> ```

<span data-ttu-id="ffa7c-194">Rvalue wynikający z wywołania metody `e1.M(e2, ...)` jest *bezpieczna do ucieczki* z najmniejszego z następujących zakresów:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-194">An rvalue resulting from a method invocation `e1.M(e2, ...)` is *safe-to-escape* from the smallest of the following scopes:</span></span>
- <span data-ttu-id="ffa7c-195">Cała zaotaczająca Metoda</span><span class="sxs-lookup"><span data-stu-id="ffa7c-195">The entire enclosing method</span></span>
- <span data-ttu-id="ffa7c-196">*bezpieczne* dla wszystkich wyrażeń argumentów (w tym odbiorcy)</span><span class="sxs-lookup"><span data-stu-id="ffa7c-196">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

#### <a name="an-rvalue"></a><span data-ttu-id="ffa7c-197">Rvalue</span><span class="sxs-lookup"><span data-stu-id="ffa7c-197">An Rvalue</span></span>
<span data-ttu-id="ffa7c-198">Rvalue ma wartość *ref-Safe-to-ucieczki* z najbliższego zakresu otaczającego.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-198">An rvalue is *ref-safe-to-escape* from the nearest enclosing scope.</span></span> <span data-ttu-id="ffa7c-199">Dzieje się tak na przykład w wywołaniu, takim jak `M(ref d.Length)`, gdzie `d` jest typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-199">This occurs for example in an invocation such as `M(ref d.Length)` where `d` is of type `dynamic`.</span></span> <span data-ttu-id="ffa7c-200">Jest ona również spójna z (i prawdopodobnie subsumes) z obsługą argumentów odpowiadających parametrom `in`. \*</span><span class="sxs-lookup"><span data-stu-id="ffa7c-200">It is also consistent with (and perhaps subsumes) our handling of arguments corresponding to `in` parameters.\*</span></span>

#### <a name="property-invocations"></a><span data-ttu-id="ffa7c-201">Wywołania właściwości</span><span class="sxs-lookup"><span data-stu-id="ffa7c-201">Property invocations</span></span>

<span data-ttu-id="ffa7c-202">Wywołanie właściwości (`get` lub `set`) traktowane jako wywołanie metody źródłowej metody przez powyższe reguły.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-202">A property invocation (either `get` or `set`) it treated as a method invocation of the underlying method by the above rules.</span></span>

#### `stackalloc`

<span data-ttu-id="ffa7c-203">Wyrażenie stackalloc jest rvalue, który jest *bezpieczny-do-ucieczki* do zakresu najwyższego poziomu metody (ale nie z całej metody).</span><span class="sxs-lookup"><span data-stu-id="ffa7c-203">A stackalloc expression is an rvalue that is *safe-to-escape* to the top-level scope of the method (but not from the entire method itself).</span></span>

#### <a name="constructor-invocations"></a><span data-ttu-id="ffa7c-204">Wywołania konstruktora</span><span class="sxs-lookup"><span data-stu-id="ffa7c-204">Constructor invocations</span></span>

<span data-ttu-id="ffa7c-205">Wyrażenie `new`, które wywołuje Konstruktor, przestrzega te same reguły jak wywołanie metody, które jest uważane za typ, który jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-205">A `new` expression that invokes a constructor obeys the same rules as a method invocation that is considered to return the type being constructed.</span></span>

<span data-ttu-id="ffa7c-206">Dodatkowo *bezpieczne do ucieczki* nie jest szersze niż *najmniejszy ze wszystkich* argumentów/operandów wyrażeń inicjatora obiektów, rekursywnie, Jeśli inicjator jest obecny.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-206">In addition *safe-to-escape* is no wider than the smallest of the *safe-to-escape* of all arguments/operands of the object initializer expressions, recursively, if initializer is present.</span></span> 

#### <a name="span-constructor"></a><span data-ttu-id="ffa7c-207">Konstruktor zakresu</span><span class="sxs-lookup"><span data-stu-id="ffa7c-207">Span constructor</span></span>
<span data-ttu-id="ffa7c-208">Język opiera się na `Span<T>` nie ma konstruktora następującej formy:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-208">The language relies on `Span<T>` not having a constructor of the following form:</span></span>

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

<span data-ttu-id="ffa7c-209">Taki Konstruktor sprawia, że `Span<T>`, które są używane jako pola, które można odróżnić od pola `ref`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-209">Such a constructor makes `Span<T>` which are used as fields indistinguishable from a `ref` field.</span></span> <span data-ttu-id="ffa7c-210">Reguły bezpieczeństwa opisane w tym dokumencie zależą od `ref` pól, które nie są prawidłową konstrukcją C#w programie, lub .NET.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-210">The safety rules described in this document depend on `ref` fields not being a valid construct in C#, or .NET.</span></span>

#### <a name="default-expressions"></a><span data-ttu-id="ffa7c-211">wyrażenia `default`</span><span class="sxs-lookup"><span data-stu-id="ffa7c-211">`default` expressions</span></span>

<span data-ttu-id="ffa7c-212">Wyrażenie `default` jest *bezpieczne-do-ucieczki* z całej otaczającej metody.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-212">A `default` expression is *safe-to-escape* from the entire enclosing method.</span></span>

## <a name="language-constraints"></a><span data-ttu-id="ffa7c-213">Ograniczenia języka</span><span class="sxs-lookup"><span data-stu-id="ffa7c-213">Language Constraints</span></span>

<span data-ttu-id="ffa7c-214">Chcemy upewnić się, że żadna `ref` zmienna lokalna i żadna zmienna typu `ref struct` nie odwołuje się do pamięci stosu lub zmiennych, które nie są już aktywne.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-214">We wish to ensure that no `ref` local variable, and no variable of `ref struct` type, refers to stack memory or variables that are no longer alive.</span></span> <span data-ttu-id="ffa7c-215">W związku z tym mamy następujące ograniczenia dotyczące języka:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-215">We therefore have the following language constraints:</span></span>

- <span data-ttu-id="ffa7c-216">Ani parametru ref, ani lokalnego nie, ani parametru ani lokalnego typu `ref struct` nie można znieść do funkcji lambda ani lokalnej.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-216">Neither a ref parameter, nor a ref local, nor a parameter or local of a `ref struct` type can be lifted into a lambda or local function.</span></span>

- <span data-ttu-id="ffa7c-217">Ani parametr ref, ani parametr typu `ref struct` nie może być argumentem metody iteratora ani metody `async`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-217">Neither a ref parameter nor a parameter of a `ref struct` type may be an argument on an iterator method or an `async` method.</span></span>

- <span data-ttu-id="ffa7c-218">Ani nie lokalnego ani lokalnego typu `ref struct` nie mogą znajdować się w zakresie w punkcie instrukcji `yield return` lub wyrażenia `await`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-218">Neither a ref local, nor a local of a `ref struct` type may be in scope at the point of a `yield return` statement or an `await` expression.</span></span>

- <span data-ttu-id="ffa7c-219">Typ `ref struct` nie może być używany jako argument typu lub jako typ elementu w typie krotki.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-219">A `ref struct` type may not be used as a type argument, or as an element type in a tuple type.</span></span>

- <span data-ttu-id="ffa7c-220">Typ `ref struct` nie może być zadeklarowanym typem pola, z tą różnicą, że może to być zadeklarowany typ pola wystąpienia innego `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-220">A `ref struct` type may not be the declared type of a field, except that it may be the declared type of an instance field of another `ref struct`.</span></span>

- <span data-ttu-id="ffa7c-221">Typ `ref struct` nie może być typem elementu tablicy.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-221">A `ref struct` type may not be the element type of an array.</span></span>

- <span data-ttu-id="ffa7c-222">Wartość typu `ref struct` nie może być opakowana:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-222">A value of a `ref struct` type may not be boxed:</span></span>
  - <span data-ttu-id="ffa7c-223">Nie istnieje konwersja z typu `ref struct` do typu `object` lub `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-223">There is no conversion from a `ref struct` type to the type `object` or the type `System.ValueType`.</span></span>
  - <span data-ttu-id="ffa7c-224">Nie można zadeklarować typu `ref struct` w celu zaimplementowania dowolnego interfejsu</span><span class="sxs-lookup"><span data-stu-id="ffa7c-224">A `ref struct` type may not be declared to implement any interface</span></span>
  - <span data-ttu-id="ffa7c-225">Żadna metoda wystąpienia nie została zadeklarowana w `object` lub w `System.ValueType`, ale nie została zastąpiona w `ref struct` typie może być wywoływana z odbiornikiem tego typu `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-225">No instance method declared in `object` or in `System.ValueType` but not overridden in a `ref struct` type may be called with a receiver of that `ref struct` type.</span></span>
  - <span data-ttu-id="ffa7c-226">Żadna metoda wystąpienia typu `ref struct` nie może zostać przechwycona przez konwersję metody na typ delegata.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-226">No instance method of a `ref struct` type may be captured by method conversion to a delegate type.</span></span>

- <span data-ttu-id="ffa7c-227">W przypadku ponownego przypisania odwołań `ref e1 = ref e2`*odwołanie typu "bezpieczny-do-ucieczki"* `e2` musi być co najmniej tak szerokie jak *"ref-Safe-to-Escape* " `e1`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-227">For a ref reassignment `ref e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="ffa7c-228">Dla instrukcji return ref `return ref e1`, Metoda *ref-Safe-to-ucieczk* `e1` musi być typu *ref-Safe-to-Escape* z całej metody.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-228">For a ref return statement `return ref e1`, the *ref-safe-to-escape* of `e1` must be *ref-safe-to-escape* from the entire method.</span></span> <span data-ttu-id="ffa7c-229">(Do zrobienia: potrzebna jest również reguła, która `e1` musi być *bezpieczna do ucieczki* z całej metody lub czy jest nadmiarowa?)</span><span class="sxs-lookup"><span data-stu-id="ffa7c-229">(TODO: Do we also need a rule that `e1` must be *safe-to-escape* from the entire method, or is that redundant?)</span></span>

- <span data-ttu-id="ffa7c-230">W przypadku instrukcji return `return e1`, *bezpieczna do ucieczki* `e1` musi być *bezpieczna do ucieczki* z całej metody.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-230">For a return statement `return e1`, the *safe-to-escape* of `e1` must be *safe-to-escape* from the entire method.</span></span>

- <span data-ttu-id="ffa7c-231">W przypadku przypisania `e1 = e2`, jeśli typ `e1` jest typem `ref struct`, wówczas *bezpieczna do ucieczki* `e2` musi być co najmniej tak szeroki zakres *jak w przypadku* `e1`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-231">For an assignment `e1 = e2`, if the type of `e1` is a `ref struct` type, then the *safe-to-escape* of `e2` must be at least as wide a scope as the *safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="ffa7c-232">W przypadku wywołania metody, jeśli istnieje argument `ref` lub `out` typu `ref struct` (włącznie z odbiornikiem), z *bezpiecznym do-ucieczk* E1, a żaden argument (w tym odbiorca) może mieć węższy *bezpieczny do ucieczki* niż E1.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-232">For a method invocation if there is a `ref` or `out` argument of a `ref struct` type (including the receiver), with *safe-to-escape* E1, then no argument (including the receiver) may have a narrower *safe-to-escape* than E1.</span></span> [<span data-ttu-id="ffa7c-233">Przykład</span><span class="sxs-lookup"><span data-stu-id="ffa7c-233">Sample</span></span>](#method-arguments-must-match)

- <span data-ttu-id="ffa7c-234">Funkcja lokalna lub funkcja anonimowa nie może odwoływać się do lokalnego lub parametru typu `ref struct` zadeklarowanego w otaczającym zakresie.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-234">A local function or anonymous function may not refer to a local or parameter of `ref struct` type declared in an enclosing scope.</span></span>

> <span data-ttu-id="ffa7c-235">***Problem otwarty:*** Potrzebujemy pewnej reguły, która pozwala na wygenerowanie błędu, gdy konieczne jest przelanie wartości stosu typu `ref struct` w wyrażeniu await, na przykład w kodzie</span><span class="sxs-lookup"><span data-stu-id="ffa7c-235">***Open Issue:*** We need some rule that permits us to produce an error when needing to spill a stack value of a `ref struct` type at an await expression, for example in the code</span></span>
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a><span data-ttu-id="ffa7c-236">Wyjaśnienia</span><span class="sxs-lookup"><span data-stu-id="ffa7c-236">Explanations</span></span>
<span data-ttu-id="ffa7c-237">Te wyjaśnienia i przykłady ułatwiają wyjaśnienie, dlaczego istnieje wiele reguł bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="ffa7c-237">These explanations and samples help explain why many of the safety rules above exist</span></span>

### <a name="method-arguments-must-match"></a><span data-ttu-id="ffa7c-238">Argumenty metody muszą być zgodne</span><span class="sxs-lookup"><span data-stu-id="ffa7c-238">Method Arguments Must Match</span></span>
<span data-ttu-id="ffa7c-239">Podczas wywoływania metody, w której występuje `out`, `ref` parametr, który jest `ref struct`, w tym odbiorcę, wszystkie `ref struct` muszą mieć ten sam okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-239">When invoking a method where there is an `out`, `ref` parameter that is a `ref struct` including the receiver then all of the `ref struct` need to have the same lifetime.</span></span> <span data-ttu-id="ffa7c-240">Jest to konieczne, C# ponieważ należy podjąć wszystkie decyzje dotyczące bezpieczeństwa okresu istnienia na podstawie informacji dostępnych w podpisie metody oraz okresu istnienia wartości w lokacji wywołania.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-240">This is necessary because C# must make all of it's decisions around lifetime safety based on the information available in the signature of the method and the lifetime of the values at the call site.</span></span> 

<span data-ttu-id="ffa7c-241">Jeśli istnieją `ref` parametry, które są `ref struct`, possiblity może zamienić ich zawartość.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-241">When there are `ref` parameters that are `ref struct` then there is the possiblity they could swap around their contents.</span></span> <span data-ttu-id="ffa7c-242">W związku z tym należy upewnić się, że wszystkie te **potencjalne** zamiany są zgodne.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-242">Hence at the call site we must ensure all of these **potential** swaps are compatible.</span></span> <span data-ttu-id="ffa7c-243">Jeśli język nie wymusił, że umożliwi to niewłaściwy kod, taki jak poniższy.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-243">If the language didn't enforce that then it will allow for bad code like the following.</span></span>

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

<span data-ttu-id="ffa7c-244">Ograniczenie na odbiorniku jest konieczne, ponieważ chociaż żadna z jej zawartości nie jest bezpieczna w trybie ref-do-ucieczki, może przechowywać podane wartości.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-244">The restriction on the receiver is necessary because while none of its contents are ref-safe-to-escape it can store the provided values.</span></span> <span data-ttu-id="ffa7c-245">Oznacza to, że w przypadku niezgodnych okresów istnienia można utworzyć otwór bezpieczeństwa typu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-245">This means with mismatched lifetimes you could create a type safety hole in the following way:</span></span>

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a><span data-ttu-id="ffa7c-246">Struktura ta ucieczki</span><span class="sxs-lookup"><span data-stu-id="ffa7c-246">Struct This Escape</span></span>
<span data-ttu-id="ffa7c-247">Gdy chodzi o zakres reguł bezpieczeństwa, wartość `this` w składowej wystąpienia jest modelowana jako parametr do elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-247">When it comes to span safety rules the `this` value in an instance member is modeled as a parameter to the member.</span></span> <span data-ttu-id="ffa7c-248">Teraz dla `struct` typ `this` jest w rzeczywistości `ref S`, gdzie w `class` jest po prostu `S` (dla elementów członkowskich `class / struct` o nazwie S).</span><span class="sxs-lookup"><span data-stu-id="ffa7c-248">Now for a `struct` the type of `this` is actually `ref S` where in a `class` it's simply `S` (for members of a `class / struct` named S).</span></span> 

<span data-ttu-id="ffa7c-249">Jeszcze `this` ma inne reguły ucieczki niż inne parametry `ref`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-249">Yet `this` has different escaping rules than other `ref` parameters.</span></span> <span data-ttu-id="ffa7c-250">W przeciwnym razie nie jest bezpieczna w trybie ref-do-ucieczki, a inne parametry są:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-250">Specifically it is not ref-safe-to-escape while other parameters are:</span></span>

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

<span data-ttu-id="ffa7c-251">Przyczyna tego ograniczenia jest w rzeczywistości niewielka, aby można było wykonać `struct`go wywołania elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-251">The reason for this restriction actually has little to do with `struct` member invocation.</span></span> <span data-ttu-id="ffa7c-252">Istnieją pewne reguły, które należy wykonać w odniesieniu do elementu członkowskiego, na którym znajdują się `struct` członkowie, gdzie odbiorca jest rvalue.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-252">There are some rules that need to be worked out with respect to member invocation on `struct` members where the receiver is an rvalue.</span></span> <span data-ttu-id="ffa7c-253">Jednak jest to bardzo rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-253">But that is very approachable.</span></span> 

<span data-ttu-id="ffa7c-254">Przyczyną tego ograniczenia jest faktyczne wywołanie interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-254">The reason for this restriction is actually about interface invocation.</span></span> <span data-ttu-id="ffa7c-255">W przeciwnym razie jest to możliwe niezależnie od tego, czy Poniższa próba lub nie powinna zostać skompilowana;</span><span class="sxs-lookup"><span data-stu-id="ffa7c-255">Specifically it comes down to whether or not the following sample should or should not compile;</span></span>

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

<span data-ttu-id="ffa7c-256">Rozważ przypadek, w którym `T` jest tworzona jako `struct`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-256">Consider the case where `T` is instantiated as a `struct`.</span></span> <span data-ttu-id="ffa7c-257">Jeśli parametr `this` ma wartość Ref-Safe-to-Escape, zwracany `p.Get` może wskazywać na stos (w odróżnieniu od typu wystąpienia `T`).</span><span class="sxs-lookup"><span data-stu-id="ffa7c-257">If the `this` parameter is ref-safe-to-escape then the return of `p.Get` could point to the stack (specifically it could be a field inside of the instantiated type of `T`).</span></span> <span data-ttu-id="ffa7c-258">Oznacza to, że język nie może zezwolić na kompilację tego przykładu, ponieważ może on zwrócić `ref` do lokalizacji stosu.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-258">That means the language could not allow this sample to compile as it could be returning a `ref` to a stack location.</span></span> <span data-ttu-id="ffa7c-259">Z drugiej strony, jeśli `this` nie jest bezpieczny-do-ucieczki, `p.Get` nie może odwoływać się do stosu, co z tego powodu jest bezpieczne do zwrócenia.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-259">On the other hand if `this` is not ref-safe-to-escape then `p.Get` cannot refer to the stack and hence it's safe to return.</span></span> 

<span data-ttu-id="ffa7c-260">Dzieje się tak dlatego, że ucieczka `this` w `struct` jest naprawdę wszystkie interfejsy.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-260">This is why the escapability of `this` in a `struct` is really all about interfaces.</span></span> <span data-ttu-id="ffa7c-261">Może ona być bezwzględnie niezawodna, ale jej działanie jest wyłączone.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-261">It can absolutely be made to work but it has a trade off.</span></span> <span data-ttu-id="ffa7c-262">Projekt ostatecznie zatrzymał się w celu zwiększenia elastyczności interfejsów.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-262">The design eventually came down in favor of making interfaces more flexible.</span></span> 

<span data-ttu-id="ffa7c-263">Istnieje możliwość, że firma Microsoft może w przyszłości złagodzić ten problem.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-263">There is potential for us to relax this in the future though.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="ffa7c-264">Zagadnienia w przyszłości</span><span class="sxs-lookup"><span data-stu-id="ffa7c-264">Future Considerations</span></span>

### <a name="length-one-spant-over-ref-values"></a><span data-ttu-id="ffa7c-265">Długość jednego zakresu\<T > za pośrednictwem wartości ref</span><span class="sxs-lookup"><span data-stu-id="ffa7c-265">Length one Span\<T> over ref values</span></span>
<span data-ttu-id="ffa7c-266">Chociaż obecnie nie jest to dozwolone, istnieją przypadki, w których tworzenie długości jednego `Span<T>` wystąpienia przez wartość byłoby korzystne:</span><span class="sxs-lookup"><span data-stu-id="ffa7c-266">Though not legal today there are cases where creating a length one `Span<T>` instance over a value would be beneficial:</span></span>

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

<span data-ttu-id="ffa7c-267">Ta funkcja uzyskuje bardziej atrakcyjny sposób, jeśli znosimy ograniczenia dotyczące [buforów o stałym rozmiarze](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) , ponieważ umożliwi to `Span<T>` wystąpienia o większej długości.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-267">This feature gets more compelling if we lift the restrictions on [fixed sized buffers](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) as it would allow for `Span<T>` instances of even greater length.</span></span> 

<span data-ttu-id="ffa7c-268">Jeśli kiedykolwiek chcesz przejść do tej ścieżki, język może obsłużyć to przez zapewnienie, że takie `Span<T>` wystąpienia były tylko w dół.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-268">If there is ever a need to go down this path then the language could accommodate this by ensuring such `Span<T>` instances were downward facing only.</span></span> <span data-ttu-id="ffa7c-269">To są tylko *bezpieczne do wyjścia* do zakresu, w którym zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-269">That is they were only ever *safe-to-escape* to the scope in which they were created.</span></span> <span data-ttu-id="ffa7c-270">Dzięki temu język nigdy nie musiał rozważać `ref` wartość ucieczki metody za pomocą `ref struct` zwracają lub pola `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-270">This ensure the language never had to consider a `ref` value escaping a method via a `ref struct` return or field of `ref struct`.</span></span> <span data-ttu-id="ffa7c-271">Może to spowodować również konieczność wprowadzenia dalszych zmian w celu rozpoznania takich konstruktorów jako przechwycenia `ref` parametru w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="ffa7c-271">This would likely also require further changes to recognize such constructors as capturing a `ref` parameter in this way though.</span></span>
