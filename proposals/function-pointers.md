---
ms.openlocfilehash: 8bf3a18dc42e225e64bd3ccda2106aed89b421ed
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108392"
---
# <a name="function-pointers"></a><span data-ttu-id="f6a48-101">Wskaźniki funkcji</span><span class="sxs-lookup"><span data-stu-id="f6a48-101">Function Pointers</span></span>

## <a name="summary"></a><span data-ttu-id="f6a48-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="f6a48-102">Summary</span></span>

<span data-ttu-id="f6a48-103">Ta propozycja zawiera konstrukcje języka, które uwidaczniają opcode kodu IL, które nie są obecnie dostępne efektywnie lub w C# ogóle: `ldftn` i `calli`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-103">This proposal provides language constructs that expose IL opcodes that cannot currently be accessed efficiently, or at all, in C# today: `ldftn` and `calli`.</span></span> <span data-ttu-id="f6a48-104">Te kody kodów IL mogą być ważne w kodzie o wysokiej wydajności, a deweloperzy potrzebują wydajnych metod uzyskiwania dostępu do nich.</span><span class="sxs-lookup"><span data-stu-id="f6a48-104">These IL opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="f6a48-105">Motywacją</span><span class="sxs-lookup"><span data-stu-id="f6a48-105">Motivation</span></span>

<span data-ttu-id="f6a48-106">Motywacje i tło tej funkcji opisano w następujących kwestiach (jak jest potencjalną implementacją funkcji):</span><span class="sxs-lookup"><span data-stu-id="f6a48-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span>

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="f6a48-107">Jest to alternatywna propozycja projektu dla elementów [wewnętrznych kompilatora](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span><span class="sxs-lookup"><span data-stu-id="f6a48-107">This is an alternate design proposal to [compiler intrinsics](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="f6a48-108">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="f6a48-108">Detailed Design</span></span>

### <a name="function-pointers"></a><span data-ttu-id="f6a48-109">Wskaźniki funkcji</span><span class="sxs-lookup"><span data-stu-id="f6a48-109">Function pointers</span></span>

<span data-ttu-id="f6a48-110">Język będzie zezwalać na deklarację wskaźników funkcji przy użyciu składni `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-110">The language will allow for the declaration of function pointers using the `delegate*` syntax.</span></span> <span data-ttu-id="f6a48-111">Pełna składnia została szczegółowo opisana w następnej sekcji, ale jest to podobne do składni używanej przez `Func` i `Action` deklaracji typu.</span><span class="sxs-lookup"><span data-stu-id="f6a48-111">The full syntax is described in detail in the next section but it is meant to resemble the syntax used by `Func` and `Action` type declarations.</span></span>

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

<span data-ttu-id="f6a48-112">Te typy są reprezentowane za pomocą typu wskaźnika funkcji, jak opisano w ECMA-335.</span><span class="sxs-lookup"><span data-stu-id="f6a48-112">These types are represented using the function pointer type as outlined in ECMA-335.</span></span> <span data-ttu-id="f6a48-113">Oznacza to, że wywołanie `delegate*` będzie używać `calli`, gdzie wywołanie `delegate` będzie używać `callvirt` na `Invoke` metodzie.</span><span class="sxs-lookup"><span data-stu-id="f6a48-113">This means invocation of a `delegate*` will use `calli` where invocation of a `delegate` will use `callvirt` on the `Invoke` method.</span></span>
<span data-ttu-id="f6a48-114">Syntaktycznie chociaż wywołanie jest identyczne dla obu konstrukcji.</span><span class="sxs-lookup"><span data-stu-id="f6a48-114">Syntactically though invocation is identical for both constructs.</span></span>

<span data-ttu-id="f6a48-115">Definicja ECMA-335 wskaźników metody obejmuje konwencję wywoływania jako część podpisu typu (sekcja 7,1).</span><span class="sxs-lookup"><span data-stu-id="f6a48-115">The ECMA-335 definition of method pointers includes the calling convention as part of the type signature (section 7.1).</span></span>
<span data-ttu-id="f6a48-116">Domyślna konwencja wywoływania zostanie `managed`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-116">The default calling convention will be `managed`.</span></span> <span data-ttu-id="f6a48-117">Alternatywne formularze można określić przez dodanie odpowiedniego modyfikatora po składni `delegate*`: `managed`, `cdecl`, `stdcall`, `thiscall`lub `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-117">Alternate forms can be specified by adding the appropriate modifier after the `delegate*` syntax: `managed`, `cdecl`, `stdcall`, `thiscall`, or `unmanaged`.</span></span> <span data-ttu-id="f6a48-118">Przykład:</span><span class="sxs-lookup"><span data-stu-id="f6a48-118">Example:</span></span>

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

<span data-ttu-id="f6a48-119">Konwersje między typami `delegate*` są wykonywane na podstawie ich podpisu, w tym konwencji wywoływania.</span><span class="sxs-lookup"><span data-stu-id="f6a48-119">Conversions between `delegate*` types is done based on their signature including the calling convention.</span></span>

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

<span data-ttu-id="f6a48-120">Typ `delegate*` jest typem wskaźnika, który oznacza, że ma wszystkie możliwości i ograniczenia standardowego typu wskaźnika:</span><span class="sxs-lookup"><span data-stu-id="f6a48-120">A `delegate*` type is a pointer type which means it has all of the capabilities and restrictions of a standard pointer type:</span></span>

- <span data-ttu-id="f6a48-121">Prawidłowy tylko w kontekście `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-121">Only valid in an `unsafe` context.</span></span>
- <span data-ttu-id="f6a48-122">Metody, które zawierają parametr `delegate*` lub typu zwracanego można wywołać tylko z kontekstu `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-122">Methods which contain a `delegate*` parameter or return type can only be called from an `unsafe` context.</span></span>
- <span data-ttu-id="f6a48-123">Nie można przekonwertować na `object`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-123">Cannot be converted to `object`.</span></span>
- <span data-ttu-id="f6a48-124">Nie można użyć jako argumentu ogólnego.</span><span class="sxs-lookup"><span data-stu-id="f6a48-124">Cannot be used as a generic argument.</span></span>
- <span data-ttu-id="f6a48-125">Może niejawnie skonwertować `delegate*` na `void*`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-125">Can implicitly convert `delegate*` to `void*`.</span></span>
- <span data-ttu-id="f6a48-126">Można jawnie skonwertować `void*` na `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-126">Can explicitly convert from `void*` to `delegate*`.</span></span>

<span data-ttu-id="f6a48-127">Dotyczących</span><span class="sxs-lookup"><span data-stu-id="f6a48-127">Restrictions:</span></span>

- <span data-ttu-id="f6a48-128">Nie można zastosować atrybutów niestandardowych do `delegate*` ani żadnego z jej elementów.</span><span class="sxs-lookup"><span data-stu-id="f6a48-128">Custom attributes cannot be applied to a `delegate*` or any of its elements.</span></span>
- <span data-ttu-id="f6a48-129">Nie można oznaczyć `delegate*` parametru jako `params`</span><span class="sxs-lookup"><span data-stu-id="f6a48-129">A `delegate*` parameter cannot be marked as `params`</span></span>
- <span data-ttu-id="f6a48-130">Typ `delegate*` ma wszystkie ograniczenia normalnego typu wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="f6a48-130">A `delegate*` type has all of the restrictions of a normal pointer type.</span></span>

### <a name="function-pointer-syntax"></a><span data-ttu-id="f6a48-131">Składnia wskaźnika funkcji</span><span class="sxs-lookup"><span data-stu-id="f6a48-131">Function pointer syntax</span></span>

<span data-ttu-id="f6a48-132">Składnia pełnego wskaźnika funkcji jest reprezentowana przez następującą gramatykę:</span><span class="sxs-lookup"><span data-stu-id="f6a48-132">The full function pointer syntax is represented by the following grammar:</span></span>

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

<span data-ttu-id="f6a48-133">Konwencja wywoływania `unmanaged` reprezentuje domyślną konwencję wywoływania dla kodu natywnego na bieżącej platformie i jest zaszyfrowana jako WinAPI.</span><span class="sxs-lookup"><span data-stu-id="f6a48-133">The `unmanaged` calling convention represents the default calling convention for native code on the current platform, and is encoded as winapi.</span></span>
<span data-ttu-id="f6a48-134">Wszystkie `calling_convention`s są kontekstowymi słowami kluczowymi, gdy są poprzedzone przez `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-134">All `calling_convention`s are contextual keywords when preceded by a `delegate*`.</span></span>

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a><span data-ttu-id="f6a48-135">Konwersje wskaźników funkcji</span><span class="sxs-lookup"><span data-stu-id="f6a48-135">Function pointer conversions</span></span>

<span data-ttu-id="f6a48-136">W niebezpiecznym kontekście zestaw dostępnych konwersji niejawnych (konwersje niejawne) jest rozszerzony w celu uwzględnienia następujących niejawnych konwersji wskaźnika:</span><span class="sxs-lookup"><span data-stu-id="f6a48-136">In an unsafe context, the set of available implicit conversions (Implicit conversions) is extended to include the following implicit pointer conversions:</span></span>
- [<span data-ttu-id="f6a48-137">_Istniejące konwersje_</span><span class="sxs-lookup"><span data-stu-id="f6a48-137">_Existing conversions_</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- <span data-ttu-id="f6a48-138">Od _funcptr typu\__ `F0` do innego _funcptr\_typu_ `F1`, pod warunkiem że spełnione są wszystkie z następujących warunków:</span><span class="sxs-lookup"><span data-stu-id="f6a48-138">From _funcptr\_type_ `F0` to another _funcptr\_type_ `F1`, provided all of the following are true:</span></span>
    - <span data-ttu-id="f6a48-139">`F0` i `F1` mają tę samą liczbę parametrów, a każdy `D0n` parametru w `F0` ma takie same `ref`, `out`, lub `in` modyfikatory, co odpowiadający mu parametr `D1n` w `F1`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-139">`F0` and `F1` have the same number of parameters, and each parameter `D0n` in `F0` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter `D1n` in `F1`.</span></span>
    - <span data-ttu-id="f6a48-140">Dla każdego parametru wartości (parametru bez modyfikatora `ref`, `out`lub `in`), konwersja tożsamości, niejawna konwersja odwołania lub niejawna konwersja wskaźnika istnieje z typu parametru w `F0` do odpowiedniego typu parametru w `F1`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-140">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `F0` to the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="f6a48-141">Dla każdego `ref`, `out`lub parametru `in` typ parametru w `F0` jest taki sam jak odpowiedni typ parametru w `F1`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-141">For each `ref`, `out`, or `in` parameter, the parameter type in `F0` is the same as the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="f6a48-142">Jeśli zwracanym typem jest przez wartość (No `ref` lub `ref readonly`), tożsamość, odwołanie niejawne lub niejawny wskaźnik nie istnieje z typu zwracanego `F1` do zwracanego typu `F0`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-142">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F1` to the return type of `F0`.</span></span>
    - <span data-ttu-id="f6a48-143">Jeśli zwracanym typem jest przez odwołanie (`ref` lub `ref readonly`), typ zwracany i Modyfikatory `ref` `F1` są takie same jak typ zwracany i `ref` Modyfikatory `F0`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-143">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F1` are the same as the return type and `ref` modifiers of `F0`.</span></span>
    - <span data-ttu-id="f6a48-144">Konwencja wywoływania `F0` jest taka sama jak Konwencja wywoływania `F1`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-144">The calling convention of `F0` is the same as the calling convention of `F1`.</span></span>

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="f6a48-145">Zezwalaj na adres dla metod docelowych</span><span class="sxs-lookup"><span data-stu-id="f6a48-145">Allow address-of to target methods</span></span>

<span data-ttu-id="f6a48-146">Grupy metod będą teraz dozwolone jako argumenty dla wyrażenia adresu.</span><span class="sxs-lookup"><span data-stu-id="f6a48-146">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="f6a48-147">Typ takiego wyrażenia będzie `delegate*`, który ma odpowiednik sygnatury metody docelowej i zarządzanej konwencji wywoływania:</span><span class="sxs-lookup"><span data-stu-id="f6a48-147">The type of such an expression will be a `delegate*` which has the equivalent signature of the target method and a managed calling convention:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

<span data-ttu-id="f6a48-148">W niebezpiecznym kontekście Metoda `M` jest zgodna z typem wskaźnika funkcji `F`, jeśli spełnione są wszystkie następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="f6a48-148">In an unsafe context, a method `M` is compatible with a function pointer type `F` if all of the following are true:</span></span>
- <span data-ttu-id="f6a48-149">`M` i `F` mają tę samą liczbę parametrów, a każdy parametr w `D` ma takie same Modyfikatory `ref`, `out`lub `in` jako odpowiedni parametr w `F`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-149">`M` and `F` have the same number of parameters, and each parameter in `D` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter in `F`.</span></span>
- <span data-ttu-id="f6a48-150">Dla każdego parametru wartości (parametru bez modyfikatora `ref`, `out`lub `in`), konwersja tożsamości, niejawna konwersja odwołania lub niejawna konwersja wskaźnika istnieje z typu parametru w `M` do odpowiedniego typu parametru w `F`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-150">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `M` to the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="f6a48-151">Dla każdego `ref`, `out`lub parametru `in` typ parametru w `M` jest taki sam jak odpowiedni typ parametru w `F`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-151">For each `ref`, `out`, or `in` parameter, the parameter type in `M` is the same as the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="f6a48-152">Jeśli zwracanym typem jest przez wartość (No `ref` lub `ref readonly`), tożsamość, odwołanie niejawne lub niejawny wskaźnik nie istnieje z typu zwracanego `F` do zwracanego typu `M`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-152">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F` to the return type of `M`.</span></span>
- <span data-ttu-id="f6a48-153">Jeśli zwracanym typem jest przez odwołanie (`ref` lub `ref readonly`), typ zwracany i Modyfikatory `ref` `F` są takie same jak typ zwracany i `ref` Modyfikatory `M`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-153">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F` are the same as the return type and `ref` modifiers of `M`.</span></span>
- <span data-ttu-id="f6a48-154">Konwencja wywoływania `M` jest taka sama jak Konwencja wywoływania `F`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-154">The calling convention of `M` is the same as the calling convention of `F`.</span></span>
- <span data-ttu-id="f6a48-155">`M` jest metodą statyczną.</span><span class="sxs-lookup"><span data-stu-id="f6a48-155">`M` is a static method.</span></span>

<span data-ttu-id="f6a48-156">W niebezpiecznym kontekście istnieje niejawna konwersja z wyrażenia address-of, którego obiektem docelowym jest grupa metod `E` do zgodnego typu wskaźnika funkcji `F` Jeśli `E` zawiera co najmniej jedną metodę, która jest stosowana w jego normalnej postaci do listy argumentów skonstruowanych przy użyciu typów parametrów i modyfikatorów `F`, zgodnie z opisem w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="f6a48-156">In an unsafe context, an implicit conversion exists from an address-of expression whose target is a method group `E` to a compatible function pointer type `F` if `E` contains at least one method that is applicable in its normal form to an argument list constructed by use of the parameter types and modifiers of `F`, as described in the following.</span></span>
- <span data-ttu-id="f6a48-157">Wybrano jedną metodę `M` odpowiadającą wywołaniu metody formularza `E(A)` z następującymi modyfikacjami:</span><span class="sxs-lookup"><span data-stu-id="f6a48-157">A single method `M` is selected corresponding to a method invocation of the form `E(A)` with the following modifications:</span></span>
   - <span data-ttu-id="f6a48-158">Lista argumentów `A` jest listą wyrażeń, z których każda została sklasyfikowana jako zmienna i z typem i modyfikatorem (`ref`, `out`lub `in`) odpowiedniego _formalnego\_parametru\_listę_ `D`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-158">The arguments list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref`, `out`, or `in`) of the corresponding _formal\_parameter\_list_ of `D`.</span></span>
   - <span data-ttu-id="f6a48-159">Metody kandydata to tylko te metody, które mają zastosowanie w ich normalnej postaci, nie są stosowane w ich rozwiniętej postaci.</span><span class="sxs-lookup"><span data-stu-id="f6a48-159">The candidate methods are only those methods that are applicable in their normal form, not those applicable in their expanded form.</span></span>
   - <span data-ttu-id="f6a48-160">Metody kandydata to tylko te metody, które są statyczne.</span><span class="sxs-lookup"><span data-stu-id="f6a48-160">The candidate methods are only those methods that are static.</span></span>
- <span data-ttu-id="f6a48-161">Jeśli algorytm wywołań metod wywołuje błąd, wystąpi błąd czasu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="f6a48-161">If the algorithm of Method invocations produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="f6a48-162">W przeciwnym razie algorytm tworzy jedną najlepszą metodę `M` ma taką samą liczbę parametrów jak `F`, a konwersja jest uznawana za istnieje.</span><span class="sxs-lookup"><span data-stu-id="f6a48-162">Otherwise, the algorithm produces a single best method `M` having the same number of parameters as `F` and the conversion is considered to exist.</span></span>
- <span data-ttu-id="f6a48-163">Wybrana metoda `M` musi być zgodna (zgodnie z definicją powyżej) z typem wskaźnika funkcji `F`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-163">The selected method `M` must be compatible (as defined above) with the function pointer type `F`.</span></span> <span data-ttu-id="f6a48-164">W przeciwnym razie wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="f6a48-164">Otherwise, a compile-time error occurs.</span></span>
- <span data-ttu-id="f6a48-165">Wynikiem konwersji jest wskaźnik funkcji typu `F`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-165">The result of the conversion is a function pointer of type `F`.</span></span>

<span data-ttu-id="f6a48-166">Niejawna konwersja istnieje z wyrażenia address-of, którego Target jest grupą metod `E` do `void*`, jeśli istnieje tylko jedna metoda statyczna `M` w `E`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-166">An implicit conversion exists from an address-of expression whose target is a method group `E` to `void*` if there is only one static method `M` in `E`.</span></span>
<span data-ttu-id="f6a48-167">Jeśli istnieje jedna metoda statyczna, należy `M`jedną najlepszą metodę z `E`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-167">If there is one static method, then the single best method from `E` is `M`.</span></span>
<span data-ttu-id="f6a48-168">W przeciwnym razie wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="f6a48-168">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="f6a48-169">Oznacza to, że deweloperzy mogą zależeć od reguł rozpoznawania przeciążenia, aby współdziałać z operatorem address-of:</span><span class="sxs-lookup"><span data-stu-id="f6a48-169">This means developers can depend on overload resolution rules to work in conjunction with the address-of operator:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

<span data-ttu-id="f6a48-170">Operator address-of zostanie zaimplementowany przy użyciu instrukcji `ldftn`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-170">The address-of operator will be implemented using the `ldftn` instruction.</span></span>

<span data-ttu-id="f6a48-171">Ograniczenia tej funkcji:</span><span class="sxs-lookup"><span data-stu-id="f6a48-171">Restrictions of this feature:</span></span>

- <span data-ttu-id="f6a48-172">Dotyczy tylko metod oznaczonych jako `static`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-172">Only applies to methods marked as `static`.</span></span>
- <span data-ttu-id="f6a48-173">W `&`nie można używać funkcji lokalnych innych niż`static`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-173">Non-`static` local functions cannot be used in `&`.</span></span> <span data-ttu-id="f6a48-174">Szczegóły implementacji tych metod celowo nie są określone przez język.</span><span class="sxs-lookup"><span data-stu-id="f6a48-174">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="f6a48-175">Dotyczy to również tego, czy są to elementy statyczne a wystąpienia, czy dokładnie do których podpisu są emitowane.</span><span class="sxs-lookup"><span data-stu-id="f6a48-175">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="better-function-member"></a><span data-ttu-id="f6a48-176">Lepsza składowa funkcji</span><span class="sxs-lookup"><span data-stu-id="f6a48-176">Better function member</span></span>

<span data-ttu-id="f6a48-177">Lepsza Specyfikacja elementu członkowskiego funkcji zostanie zmieniona w taki sposób, aby obejmowała następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="f6a48-177">The better function member specification will be changed to include the following line:</span></span>

> <span data-ttu-id="f6a48-178">`delegate*` jest bardziej szczegółowy niż `void*`</span><span class="sxs-lookup"><span data-stu-id="f6a48-178">A `delegate*` is more specific than `void*`</span></span>

<span data-ttu-id="f6a48-179">Oznacza to, że można przeciążać `void*` i `delegate*` i nadal korzystać z operatora address-of.</span><span class="sxs-lookup"><span data-stu-id="f6a48-179">This means that it is possible to overload on `void*` and a `delegate*` and still sensibly use the address-of operator.</span></span>

## <a name="open-issues"></a><span data-ttu-id="f6a48-180">Otwarte problemy</span><span class="sxs-lookup"><span data-stu-id="f6a48-180">Open Issues</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="f6a48-181">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="f6a48-181">NativeCallableAttribute</span></span>

<span data-ttu-id="f6a48-182">Jest to atrybut używany przez środowisko CLR, aby uniknąć zarządzania prologuą natywną podczas wywoływania.</span><span class="sxs-lookup"><span data-stu-id="f6a48-182">This is an attribute used by the CLR to avoid the managed to native prologue when invoking.</span></span> <span data-ttu-id="f6a48-183">Metody oznaczone przez ten atrybut są wywoływane tylko z kodu natywnego, niezarządzanego (nie można wywoływać metod, utworzyć delegata itp.).</span><span class="sxs-lookup"><span data-stu-id="f6a48-183">Methods marked by this attribute are only callable from native code, not managed (can’t call methods, create a delegate, etc …).</span></span> <span data-ttu-id="f6a48-184">Atrybut nie jest specjalny dla mscorlib; środowisko uruchomieniowe będzie traktować dowolny atrybut o tej samej nazwie z tą samą semantyką.</span><span class="sxs-lookup"><span data-stu-id="f6a48-184">The attribute is not special to mscorlib; the runtime will treat any attribute with this name with the same semantics.</span></span>

<span data-ttu-id="f6a48-185">Istnieje możliwość, że środowisko uruchomieniowe i język współdziałają ze sobą w celu zapewnienia pełnej obsługi.</span><span class="sxs-lookup"><span data-stu-id="f6a48-185">It's possible for the runtime and language to work together to fully support this.</span></span> <span data-ttu-id="f6a48-186">Język może zdecydować się na traktowanie `static` członków z atrybutem `NativeCallable` jako `delegate*` z określoną konwencją wywoływania.</span><span class="sxs-lookup"><span data-stu-id="f6a48-186">The language could choose to treat address-of `static` members with a `NativeCallable` attribute as a `delegate*` with the specified calling convention.</span></span>

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

<span data-ttu-id="f6a48-187">Ponadto język może również chcieć:</span><span class="sxs-lookup"><span data-stu-id="f6a48-187">Additionally the language would likely also want to:</span></span>

- <span data-ttu-id="f6a48-188">Oflaguj wszystkie wywołania zarządzane do metody oznaczonej `NativeCallable` jako błąd.</span><span class="sxs-lookup"><span data-stu-id="f6a48-188">Flag any managed calls to a method tagged with `NativeCallable` as an error.</span></span> <span data-ttu-id="f6a48-189">Nie można wywołać funkcji z kodu zarządzanego kompilator powinien uniemożliwić deweloperom podejmowanie próby takiego wywołania.</span><span class="sxs-lookup"><span data-stu-id="f6a48-189">Given the function can't be invoked from managed code the compiler should prevent developers from attempting such an invocation.</span></span>
- <span data-ttu-id="f6a48-190">Zapobiegaj konwersji grup metod na `delegate`, gdy metoda jest otagowana przy użyciu `NativeCallable`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-190">Prevent method group conversions to `delegate` when the method is tagged with `NativeCallable`.</span></span>

<span data-ttu-id="f6a48-191">Nie jest to konieczne do obsługi `NativeCallable`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-191">This is not necessary to support `NativeCallable` though.</span></span> <span data-ttu-id="f6a48-192">Kompilator może obsługiwać atrybut `NativeCallable` jako używa istniejącej składni.</span><span class="sxs-lookup"><span data-stu-id="f6a48-192">The compiler can support the `NativeCallable` attribute as is using the existing syntax.</span></span> <span data-ttu-id="f6a48-193">Program powinien po prostu wykonać rzutowanie na `void*` przed rzutowanie na poprawną sygnaturę `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-193">The program would simply need to cast to `void*` before casting to the correct `delegate*` signature.</span></span> <span data-ttu-id="f6a48-194">To nie będzie gorsze niż obecnie pomoc techniczna.</span><span class="sxs-lookup"><span data-stu-id="f6a48-194">That would be no worse than the support today.</span></span>

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a><span data-ttu-id="f6a48-195">Rozszerzalny zestaw niezarządzanych konwencji wywoływania</span><span class="sxs-lookup"><span data-stu-id="f6a48-195">Extensible set of unmanaged calling conventions</span></span>

<span data-ttu-id="f6a48-196">Zestaw niezarządzanych konwencji wywoływania obsługiwanych przez bieżące kodowania ECMA-335 jest nieaktualny.</span><span class="sxs-lookup"><span data-stu-id="f6a48-196">The set of unmanaged calling conventions supported by the current ECMA-335 encodings is outdated.</span></span> <span data-ttu-id="f6a48-197">Zaobserwowano prośby o dodanie obsługi dla bardziej niezarządzanych konwencji wywoływania, na przykład:</span><span class="sxs-lookup"><span data-stu-id="f6a48-197">We have seen requests to add support for more unmanaged calling conventions, for example:</span></span>

- <span data-ttu-id="f6a48-198">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span><span class="sxs-lookup"><span data-stu-id="f6a48-198">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span></span>
- <span data-ttu-id="f6a48-199">StdCall z jawnym tym https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span><span class="sxs-lookup"><span data-stu-id="f6a48-199">StdCall with explicit this https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span></span>

<span data-ttu-id="f6a48-200">Projekt tej funkcji powinien zezwalać na rozszerzanie zestawu niezarządzanych konwencji wywoływania zgodnie z wymaganiami w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="f6a48-200">The design of this feature should allow extending the set of unmanaged calling conventions as needed in future.</span></span> <span data-ttu-id="f6a48-201">Problemy obejmują ograniczoną ilość miejsca na potrzeby kodowania konwencji wywoływania (12 z 16 wartości są pobierane w `IMAGE_CEE_CS_CALLCONV_MASK`) i liczbę miejsc, które muszą być dotknięcia, aby dodać nową konwencję wywoływania.</span><span class="sxs-lookup"><span data-stu-id="f6a48-201">The problems include limited space for encoding calling conventions (12 out of 16 values are taken in `IMAGE_CEE_CS_CALLCONV_MASK`) and number of places that need to be touched in order to add a new calling convention.</span></span> <span data-ttu-id="f6a48-202">Potencjalnym rozwiązaniem jest wprowadzenie nowego kodowania, które reprezentuje konwencję wywoływania przy użyciu [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) wyliczeniem.</span><span class="sxs-lookup"><span data-stu-id="f6a48-202">A potential solution is to introduce a new encoding that represents the calling convention using [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum.</span></span>

<span data-ttu-id="f6a48-203">W przypadku elementu Reference https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h ma listę konwencji wywoływania obsługiwanych przez LLVM.</span><span class="sxs-lookup"><span data-stu-id="f6a48-203">For reference, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h has the list of calling conventions supported by LLVM.</span></span> <span data-ttu-id="f6a48-204">Chociaż jest mało prawdopodobne, że platforma .NET będzie musiała obsługiwać wszystkie te elementy, pokazuje, że przestrzeń konwencji wywoływania jest bardzo głęboka.</span><span class="sxs-lookup"><span data-stu-id="f6a48-204">While it is unlikely that .NET will ever need to support all of them, it demonstrates that the space of calling conventions is very rich.</span></span>

## <a name="considerations"></a><span data-ttu-id="f6a48-205">Zagadnienia do rozważenia</span><span class="sxs-lookup"><span data-stu-id="f6a48-205">Considerations</span></span>

### <a name="allow-instance-methods"></a><span data-ttu-id="f6a48-206">Zezwalaj na metody wystąpień</span><span class="sxs-lookup"><span data-stu-id="f6a48-206">Allow instance methods</span></span>

<span data-ttu-id="f6a48-207">Propozycja może zostać rozszerzona w celu obsługi metod wystąpień przez skorzystanie z konwencji wywoływania interfejsu wiersza polecenia `EXPLICITTHIS` (o C# nazwie `instance` w kodzie).</span><span class="sxs-lookup"><span data-stu-id="f6a48-207">The proposal could be extended to support instance methods by taking advantage of the `EXPLICITTHIS` CLI calling convention (named `instance` in C# code).</span></span> <span data-ttu-id="f6a48-208">Ta forma wskaźników funkcji interfejsu wiersza polecenia umieszcza parametr `this` jako jawny pierwszy parametr składni wskaźnika funkcji.</span><span class="sxs-lookup"><span data-stu-id="f6a48-208">This form of CLI function pointers puts the `this` parameter as an explicit first parameter of the function pointer syntax.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

<span data-ttu-id="f6a48-209">Jest to dźwięk, ale dodaje pewną skomplikowanie do propozycji.</span><span class="sxs-lookup"><span data-stu-id="f6a48-209">This is sound but adds some complication to the proposal.</span></span> <span data-ttu-id="f6a48-210">Szczególnie ze względu na to, że wskaźniki funkcji, które różnią się konwencją wywoływania `instance` i `managed` byłyby niezgodne, mimo że oba przypadki C# są używane do wywoływania metod zarządzanych z tą samą sygnaturą.</span><span class="sxs-lookup"><span data-stu-id="f6a48-210">Particularly because function pointers which differed by the calling convention `instance` and `managed` would be incompatible even though both cases are used to invoke managed methods with the same C# signature.</span></span> <span data-ttu-id="f6a48-211">Również w każdym przypadku należy wziąć pod uwagę, że jest to wartość cenna, aby istniała prosta obejście: Użyj funkcji lokalnej `static`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-211">Also in every case considered where this would be valuable to have there was a simple work around: use a `static` local function.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a><span data-ttu-id="f6a48-212">Nie wymagaj niebezpiecznego w deklaracji</span><span class="sxs-lookup"><span data-stu-id="f6a48-212">Don't require unsafe at declaration</span></span>

<span data-ttu-id="f6a48-213">Zamiast żądać `unsafe` przy każdym użyciu `delegate*`, wystarczy tylko wtedy, gdy grupa metod jest konwertowana na `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-213">Instead of requiring `unsafe` at every use of a `delegate*`, only require it at the point where a method group is converted to a `delegate*`.</span></span> <span data-ttu-id="f6a48-214">Jest to miejsce, w którym podstawowe problemy z bezpieczeństwem są odtwarzane (wiedząc, że nie można zwolnić zawierającego go zestawu, gdy wartość jest aktywna).</span><span class="sxs-lookup"><span data-stu-id="f6a48-214">This is where the core safety issues come into play (knowing that the containing assembly cannot be unloaded while the value is alive).</span></span> <span data-ttu-id="f6a48-215">Wymaganie `unsafe` w innych lokalizacjach może być postrzegane jako nadmierne.</span><span class="sxs-lookup"><span data-stu-id="f6a48-215">Requiring `unsafe` on the other locations can be seen as excessive.</span></span>

<span data-ttu-id="f6a48-216">Jest to sposób, w jaki projekt był pierwotnie przewidziany.</span><span class="sxs-lookup"><span data-stu-id="f6a48-216">This is how the design was originally intended.</span></span> <span data-ttu-id="f6a48-217">Jednak reguły języka powstają bardzo niewygodna.</span><span class="sxs-lookup"><span data-stu-id="f6a48-217">But the resulting language rules felt very awkward.</span></span> <span data-ttu-id="f6a48-218">Nie można ukryć faktu, że jest to wartość wskaźnika i jest ono utrzymywane przez nawet bez słowa kluczowego `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-218">It's impossible to hide the fact that this is a pointer value and it kept peeking through even without the `unsafe` keyword.</span></span> <span data-ttu-id="f6a48-219">Na przykład konwersja do `object` nie może być dozwolona, nie może być elementem członkowskim `class`itd... C# Projekt jest wymagany do `unsafe` dla wszystkich używanych wskaźników i dlatego ten projekt następuje poniżej.</span><span class="sxs-lookup"><span data-stu-id="f6a48-219">For example the conversion to `object` can't be allowed, it can't be a member of a `class`, etc ... The C# design is to require `unsafe` for all pointer uses and hence this design follows that.</span></span>

<span data-ttu-id="f6a48-220">Deweloperzy nadal będą mogli przedstawiać _bezpieczną_ otokę na podstawie `delegate*` wartości w taki sam sposób, jak w przypadku normalnych typów wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="f6a48-220">Developers will still be capable of presenting a _safe_ wrapper on top of `delegate*` values the same way that they do for normal pointer types today.</span></span> <span data-ttu-id="f6a48-221">Należy wziąć pod uwagę:</span><span class="sxs-lookup"><span data-stu-id="f6a48-221">Consider:</span></span>

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a><span data-ttu-id="f6a48-222">Korzystanie z delegatów</span><span class="sxs-lookup"><span data-stu-id="f6a48-222">Using delegates</span></span>

<span data-ttu-id="f6a48-223">Zamiast używać nowego elementu składni `delegate*`, po prostu Użyj istniejących typów `delegate` z `*` następującym po typie:</span><span class="sxs-lookup"><span data-stu-id="f6a48-223">Instead of using a new syntax element, `delegate*`, simply use existing `delegate` types with a `*` following the type:</span></span>

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

<span data-ttu-id="f6a48-224">Obsługa konwencji wywoływania może odbywać się poprzez dodawanie adnotacji do typów `delegate` z atrybutem określającym `CallingConvention` wartość.</span><span class="sxs-lookup"><span data-stu-id="f6a48-224">Handling calling convention can be done by annotating the `delegate` types with an attribute that specifies a `CallingConvention` value.</span></span> <span data-ttu-id="f6a48-225">Brak atrybutu oznacza zarządzaną konwencję wywoływania.</span><span class="sxs-lookup"><span data-stu-id="f6a48-225">The lack of an attribute would signify the managed calling convention.</span></span>

<span data-ttu-id="f6a48-226">Kodowanie tego w IL jest problematyczne.</span><span class="sxs-lookup"><span data-stu-id="f6a48-226">Encoding this in IL is problematic.</span></span> <span data-ttu-id="f6a48-227">Wartość bazowa musi być reprezentowana jako wskaźnik jeszcze:</span><span class="sxs-lookup"><span data-stu-id="f6a48-227">The underlying value needs to be represented as a pointer yet it also must:</span></span>

1. <span data-ttu-id="f6a48-228">Mieć unikatowy typ, aby zezwalać na przeciążenia z różnymi typami wskaźników funkcji.</span><span class="sxs-lookup"><span data-stu-id="f6a48-228">Have a unique type to allow for overloads with different function pointer types.</span></span>
1. <span data-ttu-id="f6a48-229">Być równoważne do celów OHI między granicami zestawu.</span><span class="sxs-lookup"><span data-stu-id="f6a48-229">Be equivalent for OHI purposes across assembly boundaries.</span></span>

<span data-ttu-id="f6a48-230">Ostatnim punktem jest szczególnie problematyczne.</span><span class="sxs-lookup"><span data-stu-id="f6a48-230">The last point is particularly problematic.</span></span> <span data-ttu-id="f6a48-231">Oznacza to, że każdy zestaw, który używa `Func<int>*` musi zakodować odpowiedni typ w metadanych, mimo że `Func<int>*` jest zdefiniowany w zestawie, chociaż nie kontroluje.</span><span class="sxs-lookup"><span data-stu-id="f6a48-231">This mean that every assembly which uses `Func<int>*` must encode an equivalent type in metadata even though `Func<int>*` is defined in an assembly though don't control.</span></span>
<span data-ttu-id="f6a48-232">Dodatkowo każdy inny typ, który jest zdefiniowany przy użyciu nazwy `System.Func<T>` w zestawie, który nie jest typem mscorlib, musi być inny niż wersja zdefiniowana w bibliotece Mscorlib.</span><span class="sxs-lookup"><span data-stu-id="f6a48-232">Additionally any other type which is defined with the name `System.Func<T>` in an assembly that is not mscorlib must be different than the version defined in mscorlib.</span></span>

<span data-ttu-id="f6a48-233">Jedna z opcji, która została zbadana, obemituje taki wskaźnik jako `mod_req(Func<int>) void*`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-233">One option that was explored was emitting such a pointer as `mod_req(Func<int>) void*`.</span></span> <span data-ttu-id="f6a48-234">To nie działa, ponieważ `mod_req` nie można powiązać z `TypeSpec` i dlatego nie może kierować ogólnych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="f6a48-234">This doesn't work though as a `mod_req` cannot bind to a `TypeSpec` and hence cannot target generic instantiations.</span></span>

### <a name="named-function-pointers"></a><span data-ttu-id="f6a48-235">Wskaźniki nazwanych funkcji</span><span class="sxs-lookup"><span data-stu-id="f6a48-235">Named function pointers</span></span>

<span data-ttu-id="f6a48-236">Składnia wskaźnika funkcji może być nieskomplikowana, szczególnie w złożonych przypadkach, takich jak zagnieżdżone wskaźniki funkcji.</span><span class="sxs-lookup"><span data-stu-id="f6a48-236">The function pointer syntax can be cumbersome, particularly in complex cases like nested function pointers.</span></span> <span data-ttu-id="f6a48-237">Zamiast przypisywania przez deweloperów podpisu za każdym razem, gdy język może zezwalać na nazwane deklaracje wskaźników funkcji w ramach `delegate`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-237">Rather than have developers type out the signature every time the language could allow for named declarations of function pointers as is done with `delegate`.</span></span>

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

<span data-ttu-id="f6a48-238">Częścią problemu jest podstawowy interfejs wiersza polecenia, którego nazwy nie są w związku z tym, że jest C# to całkowicie wynalazk i wymaga to, aby można było włączyć obsługę metadanych.</span><span class="sxs-lookup"><span data-stu-id="f6a48-238">Part of the problem here is the underlying CLI primitive doesn't have names hence this would be purely a C# invention and require a bit of metadata work to enable.</span></span> <span data-ttu-id="f6a48-239">Jest to doable, ale jest istotny dla pracy.</span><span class="sxs-lookup"><span data-stu-id="f6a48-239">That is doable but is a significant about of work.</span></span> <span data-ttu-id="f6a48-240">Zasadniczo wymaga C# posiadania pomocnika do tabeli typu def dla tych nazw.</span><span class="sxs-lookup"><span data-stu-id="f6a48-240">It essentially requires C# to have a companion to the type def table purely for these names.</span></span>

<span data-ttu-id="f6a48-241">Ponadto po zbadaniu argumentów dla nazwanych wskaźników funkcji można było zastosować równie dobrze do wielu innych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="f6a48-241">Also when the arguments for named function pointers were examined we found they could apply equally well to a number of other scenarios.</span></span> <span data-ttu-id="f6a48-242">Na przykład byłoby to wygodne do deklarowania nazwanych krotek, aby zmniejszyć konieczność wpisania pełnego podpisu we wszystkich przypadkach.</span><span class="sxs-lookup"><span data-stu-id="f6a48-242">For example it would be just as convenient to declare named tuples to reduce the need to type out the full signature in all cases.</span></span>

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

<span data-ttu-id="f6a48-243">Po omówieniu zdecydowała się nie zezwalać na nazwaną deklarację typów `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-243">After discussion we decided to not allow named declaration of `delegate*` types.</span></span> <span data-ttu-id="f6a48-244">Jeśli okaże się, że jest to istotne, na podstawie informacji zwrotnych dotyczących użycia klienta sprawdzimy rozwiązanie nazewnictwa, które działa w przypadku wskaźników funkcji, krotek, typów ogólnych itp... Jest to prawdopodobnie podobne w formularzu do innych sugestii, takich jak Pełna obsługa `typedef` w języku.</span><span class="sxs-lookup"><span data-stu-id="f6a48-244">If we find there is significant need for this based on customer usage feedback then we will investigate a naming solution that works for function pointers, tuples, generics, etc ... This is likely to be similar in form to other suggestions like full `typedef` support in the language.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="f6a48-245">Zagadnienia w przyszłości</span><span class="sxs-lookup"><span data-stu-id="f6a48-245">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="f6a48-246">statyczne funkcje lokalne</span><span class="sxs-lookup"><span data-stu-id="f6a48-246">static local functions</span></span>

<span data-ttu-id="f6a48-247">Odnosi się do [propozycji](https://github.com/dotnet/csharplang/issues/1565) , aby zezwolić na modyfikator `static` w funkcjach lokalnych.</span><span class="sxs-lookup"><span data-stu-id="f6a48-247">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="f6a48-248">Takie funkcje byłyby gwarantowane jako `static` i z dokładnym podpisem określonym w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="f6a48-248">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="f6a48-249">Taka funkcja powinna być prawidłowym argumentem, aby `&`, ponieważ nie zawiera ona żadnych problemów z funkcją lokalną dzisiaj</span><span class="sxs-lookup"><span data-stu-id="f6a48-249">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today</span></span>

### <a name="static-delegates"></a><span data-ttu-id="f6a48-250">elementy delegowane statyczne</span><span class="sxs-lookup"><span data-stu-id="f6a48-250">static delegates</span></span>

<span data-ttu-id="f6a48-251">Odnosi się to do [propozycji](https://github.com/dotnet/csharplang/issues/302) zezwalania dla deklaracji typów `delegate`, które mogą odwoływać się tylko do `static` członków.</span><span class="sxs-lookup"><span data-stu-id="f6a48-251">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/302) to allow for the declaration of `delegate` types which can only refer to `static` members.</span></span> <span data-ttu-id="f6a48-252">Zalety takich `delegate` wystąpień może być bezpłatna i lepsza w scenariuszach z uwzględnieniem wydajności.</span><span class="sxs-lookup"><span data-stu-id="f6a48-252">The advantage being that such `delegate` instances can be allocation free and better in performance sensitive scenarios.</span></span>

<span data-ttu-id="f6a48-253">Jeśli funkcja wskaźnika funkcji jest zaimplementowana, `static delegate` propozycja prawdopodobnie zostanie zamknięta. Proponowana zaletą tej funkcji jest bezpłatna alokacja.</span><span class="sxs-lookup"><span data-stu-id="f6a48-253">If the function pointer feature is implemented the `static delegate` proposal will likely be closed out. The proposed advantage of that feature is the allocation free nature.</span></span> <span data-ttu-id="f6a48-254">Stwierdzono niedawne badania, które nie są możliwe do osiągnięcia ze względu na zwolnienie zestawu.</span><span class="sxs-lookup"><span data-stu-id="f6a48-254">However recent investigations have found that is not possible to achieve due to assembly unloading.</span></span> <span data-ttu-id="f6a48-255">Musi istnieć silne dojście z `static delegate` do metody, do której się odwołuje, aby zapobiec wyładowaniu zestawu z tego elementu.</span><span class="sxs-lookup"><span data-stu-id="f6a48-255">There must be a strong handle from the `static delegate` to the method it refers to in order to keep the assembly from being unloaded out from under it.</span></span>

<span data-ttu-id="f6a48-256">Aby zapewnić, że każde wystąpienie `static delegate` będzie wymagane do przydzielenia nowego uchwytu, który uruchamia licznik w celach oferty.</span><span class="sxs-lookup"><span data-stu-id="f6a48-256">To maintain every `static delegate` instance would be required to allocate a new handle which runs counter to the goals of the proposal.</span></span> <span data-ttu-id="f6a48-257">Istnieją pewne projekty, w których alokacja może być amortyzowana do pojedynczej alokacji dla każdej lokacji wywołania, ale jest to nieskomplikowana złożona i niepozornie zawarta.</span><span class="sxs-lookup"><span data-stu-id="f6a48-257">There were some designs where the allocation could be amortized to a single allocation per call-site but that was a bit complex and didn't seem worth the trade off.</span></span>

<span data-ttu-id="f6a48-258">Oznacza to, że deweloperzy muszą przede wszystkim podejmować decyzje o następujących możliwościach:</span><span class="sxs-lookup"><span data-stu-id="f6a48-258">That means developers essentially have to decide between the following trade offs:</span></span>

1. <span data-ttu-id="f6a48-259">Bezpieczeństwo przed rozładunkiem zestawu: wymaga to alokacji, dlatego `delegate` jest już wystarczającą opcją.</span><span class="sxs-lookup"><span data-stu-id="f6a48-259">Safety in the face of assembly unloading: this requires allocations and hence `delegate` is already a sufficient option.</span></span>
1. <span data-ttu-id="f6a48-260">Brak zabezpieczeń przed rozładunkiem zestawu: Użyj `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="f6a48-260">No safety in face of assembly unloading: use a `delegate*`.</span></span> <span data-ttu-id="f6a48-261">Może to być opakowany w `struct`, aby zezwolić na użycie poza kontekstem `unsafe` w pozostałej części kodu.</span><span class="sxs-lookup"><span data-stu-id="f6a48-261">This can be wrapped in a `struct` to allow usage outside an `unsafe` context in the rest of the code.</span></span>
