---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484623"
---
# <a name="unmanaged-type-constraint"></a><span data-ttu-id="e3886-101">Niezarządzany typ ograniczenia</span><span class="sxs-lookup"><span data-stu-id="e3886-101">Unmanaged type constraint</span></span>

## <a name="summary"></a><span data-ttu-id="e3886-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="e3886-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="e3886-103">Funkcja ograniczeń niezarządzanych umożliwi wymuszanie języka dla klasy typów znanych jako "typy niezarządzane" w specyfikacji C# językowej.  Jest to zdefiniowane w sekcji 18,2 jako typ, który nie jest typem referencyjnym i nie zawiera pól typu odwołania na żadnym poziomie zagnieżdżania.</span><span class="sxs-lookup"><span data-stu-id="e3886-103">The unmanaged constraint feature will give language enforcement to the class of types known as "unmanaged types" in the C# language spec.  This is defined in section 18.2 as a type which is not a reference type and doesn't contain reference type fields at any level of nesting.</span></span>  

## <a name="motivation"></a><span data-ttu-id="e3886-104">Motywacją</span><span class="sxs-lookup"><span data-stu-id="e3886-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="e3886-105">Główną motywacją jest ułatwienie tworzenia kodu międzyoperacyjności niskiego poziomu w C#programie.</span><span class="sxs-lookup"><span data-stu-id="e3886-105">The primary motivation is to make it easier to author low level interop code in C#.</span></span> <span data-ttu-id="e3886-106">Typy niezarządzane są jednym z podstawowych bloków konstrukcyjnych dla kodu międzyoperacyjności, ale brak obsługi w typach ogólnych uniemożliwia tworzenie procedur wielokrotnego użytku dla wszystkich typów niezarządzanych.</span><span class="sxs-lookup"><span data-stu-id="e3886-106">Unmanaged types are one of the core building blocks for interop code, yet the lack of support in generics makes it impossible to create re-usable routines across all unmanaged types.</span></span> <span data-ttu-id="e3886-107">Zamiast tego deweloperzy są zmuszeni do tworzenia tego samego kodu kliszy kotłowej dla każdego typu niezarządzanego w swojej bibliotece:</span><span class="sxs-lookup"><span data-stu-id="e3886-107">Instead developers are forced to author the same boiler plate code for every unmanaged type in their library:</span></span>

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

<span data-ttu-id="e3886-108">Aby włączyć ten typ scenariusza, język będzie wprowadzał nowe ograniczenie: niezarządzane:</span><span class="sxs-lookup"><span data-stu-id="e3886-108">To enable this type of scenario the language will be introducing a new constraint: unmanaged:</span></span>

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

<span data-ttu-id="e3886-109">To ograniczenie może zostać spełnione tylko przez typy, które pasują do definicji typu niezarządzanego w specyfikacji C# języka. Innym sposobem na to, że typ spełnia niezarządzane ograniczenie Jeśli, może być również używany jako wskaźnik.</span><span class="sxs-lookup"><span data-stu-id="e3886-109">This constraint can only be met by types which fit into the unmanaged type definition in the C# language spec. Another way of looking at it is that a type satisfies the unmanaged constraint iff it can also be used as a pointer.</span></span> 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

<span data-ttu-id="e3886-110">Parametry typu z niezarządzanym ograniczeniem mogą używać wszystkich funkcji dostępnych dla typów niezarządzanych: wskaźniki, stałe itp...</span><span class="sxs-lookup"><span data-stu-id="e3886-110">Type parameters with the unmanaged constraint can use all the features available to unmanaged types: pointers, fixed, etc ...</span></span> 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

<span data-ttu-id="e3886-111">To ograniczenie umożliwia również wydajne konwersje między danymi strukturalnymi i strumieniami bajtów.</span><span class="sxs-lookup"><span data-stu-id="e3886-111">This constraint will also make it possible to have efficient conversions between structured data and streams of bytes.</span></span> <span data-ttu-id="e3886-112">Jest to operacja, która jest wspólna w stosach sieci i warstwach serializacji:</span><span class="sxs-lookup"><span data-stu-id="e3886-112">This is an operation that is common in networking stacks and serialization layers:</span></span>

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

<span data-ttu-id="e3886-113">Takie procedury są korzystne, ponieważ są provably bezpieczne w czasie kompilacji i w bezpłatnej alokacji.</span><span class="sxs-lookup"><span data-stu-id="e3886-113">Such routines are advantageous because they are provably safe at compile time and allocation free.</span></span>  <span data-ttu-id="e3886-114">Autorzy międzyoperacyjni dzisiaj nie mogą wykonać tej czynności (nawet jeśli jest to warstwa, w której wydajność jest krytyczna).</span><span class="sxs-lookup"><span data-stu-id="e3886-114">Interop authors today can not do this (even though it's at a layer where perf is critical).</span></span>  <span data-ttu-id="e3886-115">Zamiast tego muszą polegać na przydzielaniu procedur, które mają kosztowne testy środowiska uruchomieniowego, aby sprawdzić, czy wartości są prawidłowo niezarządzane.</span><span class="sxs-lookup"><span data-stu-id="e3886-115">Instead they need to rely on allocating routines that have expensive runtime checks to verify values are correctly unmanaged.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="e3886-116">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="e3886-116">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="e3886-117">Język wprowadzi nowe ograniczenie o nazwie `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="e3886-117">The language will introduce a new constraint named `unmanaged`.</span></span> <span data-ttu-id="e3886-118">W celu spełnienia tego ograniczenia typ musi być strukturą, a wszystkie pola typu muszą należeć do jednej z następujących kategorii:</span><span class="sxs-lookup"><span data-stu-id="e3886-118">In order to satisfy this constraint a type must be a struct and all the fields of the type must fall into one of the following categories:</span></span>

- <span data-ttu-id="e3886-119">Mają typ `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` lub `UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="e3886-119">Have the type `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` or `UIntPtr`.</span></span>
- <span data-ttu-id="e3886-120">Być dowolnego typu `enum`.</span><span class="sxs-lookup"><span data-stu-id="e3886-120">Be any `enum` type.</span></span>
- <span data-ttu-id="e3886-121">Być typem wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="e3886-121">Be a pointer type.</span></span>
- <span data-ttu-id="e3886-122">Być strukturą zdefiniowaną przez użytkownika, która satsifies ograniczenie `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="e3886-122">Be a user defined struct that satsifies the `unmanaged` constraint.</span></span>

<span data-ttu-id="e3886-123">Pola wystąpienia wygenerowanego przez kompilator, takie jak te, automatycznie implementowane właściwości, muszą również spełniać te ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="e3886-123">Compiler generated instance fields, such as those backing auto-implemented properties, must also meet these constraints.</span></span> 

<span data-ttu-id="e3886-124">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e3886-124">For example:</span></span>

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

<span data-ttu-id="e3886-125">Nie można łączyć ograniczenia `unmanaged` z `struct`, `class` lub `new()`.</span><span class="sxs-lookup"><span data-stu-id="e3886-125">The `unmanaged` constraint cannot be combined with `struct`, `class` or `new()`.</span></span> <span data-ttu-id="e3886-126">To ograniczenie wynika z faktu, że `unmanaged` implikuje `struct` w związku z tym inne ograniczenia nie mają sensu.</span><span class="sxs-lookup"><span data-stu-id="e3886-126">This restriction derives from the fact that `unmanaged` implies `struct` hence the other constraints do not make sense.</span></span>

<span data-ttu-id="e3886-127">Ograniczenie `unmanaged` nie jest wymuszane przez środowisko CLR tylko przez język.</span><span class="sxs-lookup"><span data-stu-id="e3886-127">The `unmanaged` constraint is not enforced by CLR, only by the language.</span></span> <span data-ttu-id="e3886-128">Aby zapobiec nieprawidłowym użyciu w innych językach, metody, które mają to ograniczenie, będą chronione przez mod-REQ. Uniemożliwi to innym językom używanie argumentów typu, które nie są typami niezarządzanymi.</span><span class="sxs-lookup"><span data-stu-id="e3886-128">To prevent mis-use by other languages, methods which have this constraint will be protected by a mod-req. This will prevent other languages from using type arguments which are not unmanaged types.</span></span>

<span data-ttu-id="e3886-129">Token `unmanaged` w ograniczeniu nie jest słowem kluczowym ani kontekstowym słowem kluczowym.</span><span class="sxs-lookup"><span data-stu-id="e3886-129">The token `unmanaged` in the constraint is not a keyword, nor a contextual keyword.</span></span> <span data-ttu-id="e3886-130">Zamiast tego przypomina `var`, że jest on oceniany w tej lokalizacji i będzie:</span><span class="sxs-lookup"><span data-stu-id="e3886-130">Instead it is like `var` in that it is evaluated at that location and will either:</span></span>

- <span data-ttu-id="e3886-131">Powiąż ze zdefiniowanym przez użytkownika lub typem przywoływanym o nazwie `unmanaged`: będzie on traktowany tak samo, jak wszystkie inne nazwane typy ograniczenia są traktowane.</span><span class="sxs-lookup"><span data-stu-id="e3886-131">Bind to user defined or referenced type named `unmanaged`: This will be treated just as any other named type constraint is treated.</span></span> 
- <span data-ttu-id="e3886-132">Powiąż z typem brak: ten element będzie interpretowany jako ograniczenie `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="e3886-132">Bind to no type: This will be interpreted as the `unmanaged` constraint.</span></span>

<span data-ttu-id="e3886-133">W przypadku typu o nazwie `unmanaged` i jest on dostępny bez kwalifikacji w bieżącym kontekście, nie będzie można użyć ograniczenia `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="e3886-133">In the case there is a type named `unmanaged` and it is available without qualification in the current context, then there will be no way to use the `unmanaged` constraint.</span></span> <span data-ttu-id="e3886-134">Jest to równoległe reguły dotyczące `var` i typów zdefiniowanych przez użytkownika o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="e3886-134">This parallels the rules surrounding the feature `var` and user defined types of the same name.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="e3886-135">Wady</span><span class="sxs-lookup"><span data-stu-id="e3886-135">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="e3886-136">Podstawową wadą tej funkcji jest zapewnienie niewielkiej liczby deweloperów: zazwyczaj autorów lub struktur bibliotek o niskim poziomie.</span><span class="sxs-lookup"><span data-stu-id="e3886-136">The primary drawback of this feature is that it serves a small number of developers: typically low level library authors or frameworks.</span></span>  <span data-ttu-id="e3886-137">W związku z tym spędzamy cenny czas pracy w niewielkiej liczbie deweloperów.</span><span class="sxs-lookup"><span data-stu-id="e3886-137">Hence it's spending precious language time for a small number of developers.</span></span> 

<span data-ttu-id="e3886-138">Te struktury są jednak często podstawą dla większości aplikacji platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="e3886-138">Yet these frameworks are often the basis for the majority of .NET applications out there.</span></span>  <span data-ttu-id="e3886-139">W związku z tym, że usługa WINS wydajności/poprawność na tym poziomie może mieć efekt Ripple w ekosystemie platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="e3886-139">Hence performance / correctness wins at this level can have a ripple effect on the .NET ecosystem.</span></span>  <span data-ttu-id="e3886-140">To sprawia, że funkcja rozważa się nawet z ograniczonymi odbiorcami.</span><span class="sxs-lookup"><span data-stu-id="e3886-140">This makes the feature worth considering even with the limited audience.</span></span>

## <a name="alternatives"></a><span data-ttu-id="e3886-141">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="e3886-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="e3886-142">Istnieje kilka rozwiązań alternatywnych, które należy wziąć pod uwagę:</span><span class="sxs-lookup"><span data-stu-id="e3886-142">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="e3886-143">Stan quo: funkcja nie jest uzasadniona ze względu na własne znaczenie i deweloperzy nadal używają niejawnego wyboru.</span><span class="sxs-lookup"><span data-stu-id="e3886-143">The status quo:  The feature is not justified on its own merits and developers continue to use the implicit opt in behavior.</span></span>

## <a name="questions"></a><span data-ttu-id="e3886-144">Masz</span><span class="sxs-lookup"><span data-stu-id="e3886-144">Questions</span></span>
[quesions]: #questions

### <a name="metadata-representation"></a><span data-ttu-id="e3886-145">Reprezentacja metadanych</span><span class="sxs-lookup"><span data-stu-id="e3886-145">Metadata Representation</span></span>

<span data-ttu-id="e3886-146">F# Język koduje ograniczenie w pliku podpisu, co oznacza C# , że nie można użyć ich reprezentacji.</span><span class="sxs-lookup"><span data-stu-id="e3886-146">The F# language encodes the constraint in the signature file which means C# cannot re-use their representation.</span></span> <span data-ttu-id="e3886-147">Należy wybrać nowy atrybut dla tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="e3886-147">A new attribute will need to be chosen for this constraint.</span></span> <span data-ttu-id="e3886-148">Ponadto metoda, która ma to ograniczenie, musi być chroniona przez mod-REQ.</span><span class="sxs-lookup"><span data-stu-id="e3886-148">Additionally a method which has this constraint must be protected by a mod-req.</span></span>

### <a name="blittable-vs-unmanaged"></a><span data-ttu-id="e3886-149">Danych kopiowalnych a niezarządzana</span><span class="sxs-lookup"><span data-stu-id="e3886-149">Blittable vs. Unmanaged</span></span>
<span data-ttu-id="e3886-150">F# Język ma bardzo [podobną funkcję](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) , która używa niezarządzanego słowa kluczowego.</span><span class="sxs-lookup"><span data-stu-id="e3886-150">The F# language has a very [similar feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) which uses the keyword unmanaged.</span></span> <span data-ttu-id="e3886-151">Nazwa danych kopiowalnych jest używana w Midori.</span><span class="sxs-lookup"><span data-stu-id="e3886-151">The blittable name comes from the use in Midori.</span></span>  <span data-ttu-id="e3886-152">Warto chcieć zajrzeć do pierwszeństwa w tym miejscu i zamiast tego użyć niezarządzanego.</span><span class="sxs-lookup"><span data-stu-id="e3886-152">May want to look to precedence here and use unmanaged instead.</span></span> 

<span data-ttu-id="e3886-153">**Rozwiązanie** Język zdecyduje się użyć niezarządzanego</span><span class="sxs-lookup"><span data-stu-id="e3886-153">**Resolution** The language decide to use unmanaged</span></span> 

### <a name="verifier"></a><span data-ttu-id="e3886-154">Weryfikatora</span><span class="sxs-lookup"><span data-stu-id="e3886-154">Verifier</span></span>

<span data-ttu-id="e3886-155">Czy należy zaktualizować weryfikator/środowisko uruchomieniowe, aby zrozumieć użycie wskaźników do parametrów typu ogólnego?</span><span class="sxs-lookup"><span data-stu-id="e3886-155">Does the verifier / runtime need to be updated to understand the use of pointers to generic type parameters?</span></span>  <span data-ttu-id="e3886-156">Lub może być po prostu działała bez zmian?</span><span class="sxs-lookup"><span data-stu-id="e3886-156">Or can it simply work as is without changes?</span></span>

<span data-ttu-id="e3886-157">**Rozwiązanie** Nie trzeba zmieniać żadnych zmian.</span><span class="sxs-lookup"><span data-stu-id="e3886-157">**Resolution** No changes needed.</span></span> <span data-ttu-id="e3886-158">Wszystkie typy wskaźników są po prostu niemożliwym do zweryfikowania.</span><span class="sxs-lookup"><span data-stu-id="e3886-158">All pointer types are simply unverifiable.</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="e3886-159">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="e3886-159">Design meetings</span></span>

<span data-ttu-id="e3886-160">Nie dotyczy</span><span class="sxs-lookup"><span data-stu-id="e3886-160">n/a</span></span>
