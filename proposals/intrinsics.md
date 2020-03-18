---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484567"
---
# <a name="compiler-intrinsics"></a><span data-ttu-id="eb235-101">Funkcje wewnętrzne kompilatora</span><span class="sxs-lookup"><span data-stu-id="eb235-101">Compiler Intrinsics</span></span>

## <a name="summary"></a><span data-ttu-id="eb235-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="eb235-102">Summary</span></span>

<span data-ttu-id="eb235-103">Ta propozycja zawiera konstrukcje języka, które uwidaczniają elementy kodów IL niskiego poziomu, które nie są obecnie dostępne efektywnie lub w ogóle: `ldftn`, `ldvirtftn`, `ldtoken` i `calli`.</span><span class="sxs-lookup"><span data-stu-id="eb235-103">This proposal provides language constructs that expose low level IL opcodes that cannot currently be accessed efficiently, or at all: `ldftn`, `ldvirtftn`, `ldtoken` and `calli`.</span></span> <span data-ttu-id="eb235-104">Te kody operacji niskiego poziomu mogą być ważne w kodzie o wysokiej wydajności, a deweloperzy potrzebują wydajnych metod uzyskiwania dostępu do nich.</span><span class="sxs-lookup"><span data-stu-id="eb235-104">These low level opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="eb235-105">Motywacją</span><span class="sxs-lookup"><span data-stu-id="eb235-105">Motivation</span></span>

<span data-ttu-id="eb235-106">Motywacje i tło tej funkcji opisano w następujących kwestiach (jak jest potencjalną implementacją funkcji):</span><span class="sxs-lookup"><span data-stu-id="eb235-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span> 

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="eb235-107">Ta alternatywna propozycja projektu jest dostępna po przejrzeniu implementacji prototypu oryginalnej oferty przez @msjabby, a także użycia w całej znaczącej bazie kodu.</span><span class="sxs-lookup"><span data-stu-id="eb235-107">This alternate design proposal comes after reviewing a prototype implementation of the original proposal by @msjabby as well as the use throughout a significant code base.</span></span> <span data-ttu-id="eb235-108">Ten projekt został wykonany z istotnymi danymi wejściowymi z @mjsabby, @tmat i @jkotas.</span><span class="sxs-lookup"><span data-stu-id="eb235-108">This design was done with significant input from @mjsabby, @tmat and @jkotas.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="eb235-109">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="eb235-109">Detailed Design</span></span> 

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="eb235-110">Zezwalaj na adres metod docelowych</span><span class="sxs-lookup"><span data-stu-id="eb235-110">Allow address of to target methods</span></span>

<span data-ttu-id="eb235-111">Grupy metod będą teraz dozwolone jako argumenty dla wyrażenia adresu.</span><span class="sxs-lookup"><span data-stu-id="eb235-111">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="eb235-112">Typ takiego wyrażenia zostanie `void*`.</span><span class="sxs-lookup"><span data-stu-id="eb235-112">The type of such an expression will be `void*`.</span></span> 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

<span data-ttu-id="eb235-113">Nie istnieje żadna konwersja delegatów w tym miejscu jedynym mechanizmem filtrowania elementów członkowskich w grupie metod jest dostęp statyczny/wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="eb235-113">Given there is no delegate conversion here the only mechanism for filtering members in the method group is by static / instance access.</span></span> <span data-ttu-id="eb235-114">Jeśli nie można rozróżnić członków, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="eb235-114">If that cannot distinguish the members then a compile time error will occur.</span></span>

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

<span data-ttu-id="eb235-115">Wyrażenie AddressOf w tym kontekście zostanie zaimplementowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="eb235-115">The addressof expression in this context will be implemented in the following manner:</span></span>

- <span data-ttu-id="eb235-116">Ldftn: gdy metoda jest niewirtualna.</span><span class="sxs-lookup"><span data-stu-id="eb235-116">ldftn: when the method is non-virtual.</span></span>
- <span data-ttu-id="eb235-117">ldvirtftn: Kiedy metoda jest wirtualna.</span><span class="sxs-lookup"><span data-stu-id="eb235-117">ldvirtftn: when the method is virtual.</span></span>

<span data-ttu-id="eb235-118">Ograniczenia tej funkcji:</span><span class="sxs-lookup"><span data-stu-id="eb235-118">Restrictions of this feature:</span></span>

- <span data-ttu-id="eb235-119">Metody wystąpień można określić tylko w przypadku użycia wyrażenia wywołania dla wartości</span><span class="sxs-lookup"><span data-stu-id="eb235-119">Instance methods can only be specified when using an invocation expression on a value</span></span>
- <span data-ttu-id="eb235-120">Funkcji lokalnych nie można używać w `&`.</span><span class="sxs-lookup"><span data-stu-id="eb235-120">Local functions cannot be used in `&`.</span></span> <span data-ttu-id="eb235-121">Szczegóły implementacji tych metod celowo nie są określone przez język.</span><span class="sxs-lookup"><span data-stu-id="eb235-121">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="eb235-122">Dotyczy to również tego, czy są to elementy statyczne a wystąpienia, czy dokładnie do których podpisu są emitowane.</span><span class="sxs-lookup"><span data-stu-id="eb235-122">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="handleof"></a><span data-ttu-id="eb235-123">handleof</span><span class="sxs-lookup"><span data-stu-id="eb235-123">handleof</span></span>

<span data-ttu-id="eb235-124">`handleof` kontekstowe słowo kluczowe przetłumaczy pole, element członkowski lub typ na odpowiedni typ `RuntimeHandle` za pomocą instrukcji `ldtoken`.</span><span class="sxs-lookup"><span data-stu-id="eb235-124">The `handleof` contextual keyword will translate a field, member or type into their equivalent `RuntimeHandle` type using the `ldtoken` instruction.</span></span> <span data-ttu-id="eb235-125">Dokładny typ wyrażenia będzie zależeć od rodzaju nazwy w `handleof`:</span><span class="sxs-lookup"><span data-stu-id="eb235-125">The exact type of the expression will depend on the kind of the name in `handleof`:</span></span>

- <span data-ttu-id="eb235-126">pole: `RuntimeFieldHandle`</span><span class="sxs-lookup"><span data-stu-id="eb235-126">field: `RuntimeFieldHandle`</span></span>
- <span data-ttu-id="eb235-127">Typ: `RuntimeTypeHandle`</span><span class="sxs-lookup"><span data-stu-id="eb235-127">type: `RuntimeTypeHandle`</span></span>
- <span data-ttu-id="eb235-128">Metoda: `RuntimeMethodHandle`</span><span class="sxs-lookup"><span data-stu-id="eb235-128">method: `RuntimeMethodHandle`</span></span>

<span data-ttu-id="eb235-129">Argumenty do `handleof` są identyczne z `nameof`.</span><span class="sxs-lookup"><span data-stu-id="eb235-129">The arguments to `handleof` are identical to `nameof`.</span></span> <span data-ttu-id="eb235-130">Musi to być prosta nazwa, kwalifikowana nazwa, dostęp do składowej, dostęp podstawowy do określonego elementu członkowskiego lub ten dostęp do określonego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="eb235-130">It must be a simple name, qualified name, member access, base access with a specified member, or this access with a specified member.</span></span> <span data-ttu-id="eb235-131">Wyrażenie argumentu identyfikuje definicję kodu, ale nigdy nie jest oceniane.</span><span class="sxs-lookup"><span data-stu-id="eb235-131">The argument expression identifies a code definition, but it is never evaluated.</span></span>

<span data-ttu-id="eb235-132">Wyrażenie `handleof` jest oceniane w czasie wykonywania i ma zwracany typ `RuntimeHandle`.</span><span class="sxs-lookup"><span data-stu-id="eb235-132">The `handleof` expression is evaluated at runtime and has a return type of `RuntimeHandle`.</span></span> <span data-ttu-id="eb235-133">Można to zrobić w bezpiecznym kodzie, a także w przypadku niebezpiecznego.</span><span class="sxs-lookup"><span data-stu-id="eb235-133">This can be executed in safe code as well as unsafe.</span></span> 

``` 
RuntimeHandle stringHandle = handleof(string);
```

<span data-ttu-id="eb235-134">Ograniczenia tej funkcji:</span><span class="sxs-lookup"><span data-stu-id="eb235-134">Restrictions of this feature:</span></span>

- <span data-ttu-id="eb235-135">Właściwości nie można używać w wyrażeniu `handleof`.</span><span class="sxs-lookup"><span data-stu-id="eb235-135">Properties cannot be used in a `handleof` expression.</span></span>
- <span data-ttu-id="eb235-136">Nie można użyć wyrażenia `handleof`, jeśli istnieje nazwa `handleof` istniejąca w zakresie.</span><span class="sxs-lookup"><span data-stu-id="eb235-136">The `handleof` expression cannot be used when there is an existing `handleof` name in scope.</span></span> <span data-ttu-id="eb235-137">Na przykład typ, przestrzeń nazw itp...</span><span class="sxs-lookup"><span data-stu-id="eb235-137">For example a type, namespace, etc ...</span></span>

### <a name="calli"></a><span data-ttu-id="eb235-138">Calli</span><span class="sxs-lookup"><span data-stu-id="eb235-138">calli</span></span>

<span data-ttu-id="eb235-139">Kompilator doda obsługę nowego typu funkcji `extern`, która efektywnie przekształci się w instrukcję `.calli`.</span><span class="sxs-lookup"><span data-stu-id="eb235-139">The compiler will add support for a new type of `extern` function that efficiently translates into a `.calli` instruction.</span></span> <span data-ttu-id="eb235-140">Atrybut extern zostanie oznaczony atrybutem następującego kształtu:</span><span class="sxs-lookup"><span data-stu-id="eb235-140">The extern attribute will be marked with an attribute of the following shape:</span></span>

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

<span data-ttu-id="eb235-141">Dzięki temu deweloperzy mogą definiować metody w następującej postaci:</span><span class="sxs-lookup"><span data-stu-id="eb235-141">This allows developers to define methods in the following form:</span></span>

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

<span data-ttu-id="eb235-142">Ograniczenia dotyczące metody, która ma zastosowany atrybut `CallIndirect`:</span><span class="sxs-lookup"><span data-stu-id="eb235-142">Restrictions on the method which has the `CallIndirect` attribute applied:</span></span>

- <span data-ttu-id="eb235-143">Nie może mieć atrybutu `DllImport`.</span><span class="sxs-lookup"><span data-stu-id="eb235-143">Cannot have a `DllImport` attribute.</span></span>
- <span data-ttu-id="eb235-144">Nie może być ogólny.</span><span class="sxs-lookup"><span data-stu-id="eb235-144">Cannot be generic.</span></span>

## <a name="open-issues"></a><span data-ttu-id="eb235-145">Otwarte problemy</span><span class="sxs-lookup"><span data-stu-id="eb235-145">Open Issues</span></span>

### <a name="callingconvention"></a><span data-ttu-id="eb235-146">CallingConvention</span><span class="sxs-lookup"><span data-stu-id="eb235-146">CallingConvention</span></span>

<span data-ttu-id="eb235-147">`CallIndirectAttribute` zaprojektowana używa `CallingConvention` Wyliczenie, które nie ma wpisu dla zarządzanych konwencji wywoływania.</span><span class="sxs-lookup"><span data-stu-id="eb235-147">The `CallIndirectAttribute` as designed uses the `CallingConvention` enum which lacks an entry for managed calling conventions.</span></span> <span data-ttu-id="eb235-148">Wyliczenie należy rozszerzyć w celu uwzględnienia tej konwencji wywoływania lub atrybutu musi mieć inne podejście.</span><span class="sxs-lookup"><span data-stu-id="eb235-148">The enum either needs to be extended to include this calling convention or the attribute needs to take a different approach.</span></span>

## <a name="considerations"></a><span data-ttu-id="eb235-149">Zagadnienia do rozważenia</span><span class="sxs-lookup"><span data-stu-id="eb235-149">Considerations</span></span>

### <a name="disambiguating-method-groups"></a><span data-ttu-id="eb235-150">Niejednoznaczność grup metod</span><span class="sxs-lookup"><span data-stu-id="eb235-150">Disambiguating method groups</span></span>

<span data-ttu-id="eb235-151">Istnieje kilka dyskusji na temat funkcji, które mogłyby ułatwić odróżnienie grup metod, które przechodzą do wyrażenia adresu.</span><span class="sxs-lookup"><span data-stu-id="eb235-151">There was some discussion around features that would make it easier to disambiguate method groups passed to an address-of expression.</span></span> <span data-ttu-id="eb235-152">Na przykład dodawanie elementów podpisu do składni:</span><span class="sxs-lookup"><span data-stu-id="eb235-152">For instance potentially adding signature elements to the syntax:</span></span>

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

<span data-ttu-id="eb235-153">Zostało odrzucone, ponieważ nie można było nawiązać istotnego przypadku, a w tym miejscu nie można envisioned prostej składni.</span><span class="sxs-lookup"><span data-stu-id="eb235-153">This was rejected because a compelling case could not be made nor could a simple syntax be envisioned here.</span></span> <span data-ttu-id="eb235-154">Istnieje również stosunkowo proste obejście: proste Definiowanie innej metody, która jest niejednoznaczna i używa C# kodu do wywołania żądanej funkcji.</span><span class="sxs-lookup"><span data-stu-id="eb235-154">Also there is a fairly straight forward work around: simple define another method that is unambiguous and uses C# code to call into the desired function.</span></span> 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

<span data-ttu-id="eb235-155">Jest to jeszcze prostsze, jeśli `static` funkcje lokalne wprowadzają język.</span><span class="sxs-lookup"><span data-stu-id="eb235-155">This becomes even simpler if `static` local functions enter the language.</span></span> <span data-ttu-id="eb235-156">Następnie obejście można zdefiniować w tej samej funkcji, która używała niejednoznacznego adresu operacji:</span><span class="sxs-lookup"><span data-stu-id="eb235-156">Then the work around could be defined in the same function that used the ambiguous address-of operation:</span></span>

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a><span data-ttu-id="eb235-157">LoadTypeTokenInt32</span><span class="sxs-lookup"><span data-stu-id="eb235-157">LoadTypeTokenInt32</span></span>

<span data-ttu-id="eb235-158">Oryginalna propozycja umożliwiająca ładowanie tokenów metadanych jako wartości `int` w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="eb235-158">The original proposal allowed for metadata tokens to be loaded as `int` values at compile time.</span></span> <span data-ttu-id="eb235-159">Zasadniczo mają `tokenof`, które mają te same argumenty co `handleof`, ale są oceniane w czasie kompilacji do stałej `int`.</span><span class="sxs-lookup"><span data-stu-id="eb235-159">Essentially have `tokenof` that has the same arguments as `handleof` but is evaluated at compile time to an `int` constant.</span></span> 

<span data-ttu-id="eb235-160">Zostało to odrzucone, ponieważ powoduje znaczący problem dotyczący ponownego zapisu IL (z którego korzysta program .NET).</span><span class="sxs-lookup"><span data-stu-id="eb235-160">This was rejected as it causes significant problem for IL rewrites (of which .NET has many).</span></span> <span data-ttu-id="eb235-161">Takie odpisanie często manipuluje tabelami metadanych w sposób, który może unieważniać te wartości.</span><span class="sxs-lookup"><span data-stu-id="eb235-161">Such rewriters often manipulate the metadata tables in a way that could invalidate these values.</span></span> <span data-ttu-id="eb235-162">Nie ma żadnego rozsądnego sposobu na aktualizację tych wartości, gdy są one przechowywane jako proste wartości `int`.</span><span class="sxs-lookup"><span data-stu-id="eb235-162">There is no reasonable way for such rewriters to update these values when they are stored as simple `int` values.</span></span>

<span data-ttu-id="eb235-163">Podstawowym pomysłem posiadania nieprzezroczystego uchwytu dla wpisów metadanych będzie dalsza Eksploracja przez zespół środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="eb235-163">The underlying idea of having an opaque handle for metadata entries will continue to be explored by the runtime team.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="eb235-164">Zagadnienia w przyszłości</span><span class="sxs-lookup"><span data-stu-id="eb235-164">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="eb235-165">statyczne funkcje lokalne</span><span class="sxs-lookup"><span data-stu-id="eb235-165">static local functions</span></span>

<span data-ttu-id="eb235-166">Odnosi się do [propozycji](https://github.com/dotnet/csharplang/issues/1565) , aby zezwolić na modyfikator `static` w funkcjach lokalnych.</span><span class="sxs-lookup"><span data-stu-id="eb235-166">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="eb235-167">Takie funkcje byłyby gwarantowane jako `static` i z dokładnym podpisem określonym w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="eb235-167">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="eb235-168">Taka funkcja powinna być prawidłowym argumentem, aby `&`, ponieważ nie zawiera on żadnych problemów lokalnych.</span><span class="sxs-lookup"><span data-stu-id="eb235-168">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today.</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="eb235-169">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="eb235-169">NativeCallableAttribute</span></span>

<span data-ttu-id="eb235-170">Środowisko CLR zawiera funkcję, która umożliwia emitowanie zarządzanych metod w taki sposób, że są one bezpośrednio wywoływane z kodu natywnego.</span><span class="sxs-lookup"><span data-stu-id="eb235-170">The CLR has a feature that allows for managed methods to be emitted in such a way that they are directly callable from native code.</span></span> <span data-ttu-id="eb235-171">W tym celu należy dodać `NativeCallableAttribute` do metod.</span><span class="sxs-lookup"><span data-stu-id="eb235-171">This is done by adding the `NativeCallableAttribute` to methods.</span></span> <span data-ttu-id="eb235-172">Taka metoda jest wywoływana tylko z kodu natywnego i dlatego musi zawierać tylko typy danych kopiowalnych w podpisie.</span><span class="sxs-lookup"><span data-stu-id="eb235-172">Such a method is only callable from native code and hence must contain only blittable types in the signature.</span></span> <span data-ttu-id="eb235-173">Wywoływanie z kodu zarządzanego powoduje błąd w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="eb235-173">Calling from managed code results in a runtime error.</span></span> 

<span data-ttu-id="eb235-174">Ta funkcja będzie dobrze wzórem z tą propozycją, ponieważ będzie to możliwe:</span><span class="sxs-lookup"><span data-stu-id="eb235-174">This feature would pattern well with this proposal as it would allow:</span></span>

- <span data-ttu-id="eb235-175">Przekazywanie funkcji zdefiniowanej w kodzie zarządzanym do kodu natywnego jako wskaźnika funkcji (za pośrednictwem adresu) bez dodatkowych kosztów w kodzie zarządzanym i natywnym.</span><span class="sxs-lookup"><span data-stu-id="eb235-175">Passing a function defined in managed code to native code as a function pointer (via address-of) with no overhead in managed or native code.</span></span> 
- <span data-ttu-id="eb235-176">Środowisko uruchomieniowe może wprowadzić błędy witryny dla takich funkcji w kodzie zarządzanym, aby uniemożliwić ich wywoływanie w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="eb235-176">Runtime can introduce use site errors for such functions in managed code to prevent them from being invoked at compile time.</span></span>




