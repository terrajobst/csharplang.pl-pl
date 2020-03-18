---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484616"
---
# <a name="nullable-reference-types-in-c"></a><span data-ttu-id="41979-101">Typy odwołań dopuszczających wartość null wC#</span><span class="sxs-lookup"><span data-stu-id="41979-101">Nullable reference types in C#</span></span> #

<span data-ttu-id="41979-102">Celem tej funkcji jest:</span><span class="sxs-lookup"><span data-stu-id="41979-102">The goal of this feature is to:</span></span>

* <span data-ttu-id="41979-103">Zezwól deweloperom na wyrażenie, czy zmienna, parametr lub wynik typu referencyjnego mają mieć wartość null, czy nie.</span><span class="sxs-lookup"><span data-stu-id="41979-103">Allow developers to express whether a variable, parameter or result of a reference type is intended to be null or not.</span></span>
* <span data-ttu-id="41979-104">Podaj ostrzeżenia, gdy takie zmienne, parametry i wyniki nie są używane zgodnie z tym zamiarem.</span><span class="sxs-lookup"><span data-stu-id="41979-104">Provide warnings when such variables, parameters and results are not used according to that intent.</span></span>

## <a name="expression-of-intent"></a><span data-ttu-id="41979-105">Wyrażenie intencji</span><span class="sxs-lookup"><span data-stu-id="41979-105">Expression of intent</span></span>

<span data-ttu-id="41979-106">Język zawiera już składnię `T?` dla typów wartości.</span><span class="sxs-lookup"><span data-stu-id="41979-106">The language already contains the `T?` syntax for value types.</span></span> <span data-ttu-id="41979-107">Poszerzenie tej składni do typów referencyjnych jest proste.</span><span class="sxs-lookup"><span data-stu-id="41979-107">It is straightforward to extend this syntax to reference types.</span></span>

<span data-ttu-id="41979-108">Przyjęto założenie, że intencja niebędącego typem odwołania `T` ma wartość różną od null.</span><span class="sxs-lookup"><span data-stu-id="41979-108">It is assumed that the intent of an unadorned reference type `T` is for it to be non-null.</span></span>

## <a name="checking-of-nullable-references"></a><span data-ttu-id="41979-109">Sprawdzanie odwołań dopuszczających wartość null</span><span class="sxs-lookup"><span data-stu-id="41979-109">Checking of nullable references</span></span>

<span data-ttu-id="41979-110">Analiza przepływu śledzi zmienne odwołania do wartości null.</span><span class="sxs-lookup"><span data-stu-id="41979-110">A flow analysis tracks nullable reference variables.</span></span> <span data-ttu-id="41979-111">Jeśli analiza uzna, że nie będzie ona mieć wartości null (np. po sprawdzeniu lub przypisaniu), ich wartość zostanie uznana za odwołanie o wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="41979-111">Where the analysis deems that they would not be null (e.g. after a check or an assignment), their value will be considered a non-null reference.</span></span>

<span data-ttu-id="41979-112">Odwołanie dopuszczające wartość null może być również jawnie traktowane jako inne niż null z przyrostkowym operatorem `x!` (operator "Dammit"), w przypadku gdy analiza przepływu nie może ustanowić niezerowej sytuacji, którą zna Deweloper.</span><span class="sxs-lookup"><span data-stu-id="41979-112">A nullable reference can also explicitly be treated as non-null with the postfix `x!` operator (the "dammit" operator), for when flow analysis cannot establish a non-null situation that the developer knows is there.</span></span>

<span data-ttu-id="41979-113">W przeciwnym razie zostanie wyświetlone ostrzeżenie, jeśli odwołanie do wartości null jest wywoływane lub jest konwertowane na typ inny niż null.</span><span class="sxs-lookup"><span data-stu-id="41979-113">Otherwise, a warning is given if a nullable reference is dereferenced, or is converted to a non-null type.</span></span>

<span data-ttu-id="41979-114">Podczas konwertowania z `S[]` na `T?[]` i z `S?[]` na `T[]`należy uzyskać ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="41979-114">A warning is given when converting from `S[]` to `T?[]` and from `S?[]` to `T[]`.</span></span>

<span data-ttu-id="41979-115">Ostrzeżenie jest wyrażane podczas konwersji z `C<S>` na `C<T?>`, z wyjątkiem sytuacji, gdy parametr typu jest współvariant (`out`) i podczas konwersji z `C<S?>` na `C<T>`, z wyjątkiem sytuacji, gdy parametr type ma wartość kontrawariantne (`in`).</span><span class="sxs-lookup"><span data-stu-id="41979-115">A warning is given when converting from `C<S>` to `C<T?>` except when the type parameter is covariant (`out`), and when converting from `C<S?>` to `C<T>` except when the type parameter is contravariant (`in`).</span></span>

<span data-ttu-id="41979-116">Ostrzeżenie jest udzielane na `C<T?>`, jeśli parametr typu ma ograniczenia inne niż null.</span><span class="sxs-lookup"><span data-stu-id="41979-116">A warning is given on `C<T?>` if the type parameter has non-null constraints.</span></span> 

## <a name="checking-of-non-null-references"></a><span data-ttu-id="41979-117">Sprawdzanie odwołań o wartości innej niż null</span><span class="sxs-lookup"><span data-stu-id="41979-117">Checking of non-null references</span></span>

<span data-ttu-id="41979-118">Ostrzeżenie jest wyrażane, Jeśli literał o wartości null jest przypisany do zmiennej innej niż null lub przekazany jako parametr inny niż null.</span><span class="sxs-lookup"><span data-stu-id="41979-118">A warning is given if a null literal is assigned to a non-null variable or passed as a non-null parameter.</span></span>

<span data-ttu-id="41979-119">Ostrzeżenie jest również udzielane, jeśli Konstruktor nie inicjuje jawnie pól odwołania o wartości innej niż null.</span><span class="sxs-lookup"><span data-stu-id="41979-119">A warning is also given if a constructor does not explicitly initialize non-null reference fields.</span></span>

<span data-ttu-id="41979-120">Nie można prawidłowo śledzić, że wszystkie elementy tablicy odwołań o wartości innej niż null są inicjowane.</span><span class="sxs-lookup"><span data-stu-id="41979-120">We cannot adequately track that all elements of an array of non-null references are initialized.</span></span> <span data-ttu-id="41979-121">Można jednak wydać ostrzeżenie, jeśli nie zostanie przypisany żaden element nowo utworzonej tablicy, zanim tablica zostanie odczytana lub przeniesiona.</span><span class="sxs-lookup"><span data-stu-id="41979-121">However, we could issue a warning if no element of a newly created array is assigned to before the array is read from or passed on.</span></span> <span data-ttu-id="41979-122">To może obsłużyć typowy przypadek bez zbyt dużej szumu.</span><span class="sxs-lookup"><span data-stu-id="41979-122">That might handle the common case without being too noisy.</span></span>

<span data-ttu-id="41979-123">Musimy zdecydować, czy `default(T)` generuje ostrzeżenie, czy jest po prostu traktowany jak typ `T?`.</span><span class="sxs-lookup"><span data-stu-id="41979-123">We need to decide whether `default(T)` generates a warning, or is simply treated as being of the type `T?`.</span></span>

## <a name="metadata-representation"></a><span data-ttu-id="41979-124">Reprezentacja metadanych</span><span class="sxs-lookup"><span data-stu-id="41979-124">Metadata representation</span></span>

<span data-ttu-id="41979-125">Parametry definiowania wartości null powinny być reprezentowane w metadanych jako atrybuty.</span><span class="sxs-lookup"><span data-stu-id="41979-125">Nullability adornments should be represented in metadata as attributes.</span></span> <span data-ttu-id="41979-126">Oznacza to, że kompilatory niskiego poziomu zignorują je.</span><span class="sxs-lookup"><span data-stu-id="41979-126">This means that downlevel compilers will ignore them.</span></span>

<span data-ttu-id="41979-127">Musimy zdecydować, czy dołączane są tylko adnotacje dopuszczające wartość null, lub też wskazuje, czy w zestawie nie ma wartości "on".</span><span class="sxs-lookup"><span data-stu-id="41979-127">We need to decide if only nullable annotations are included, or there's also some indication of whether non-null was "on" in the assembly.</span></span>

## <a name="generics"></a><span data-ttu-id="41979-128">Typy ogólne</span><span class="sxs-lookup"><span data-stu-id="41979-128">Generics</span></span>

<span data-ttu-id="41979-129">Jeśli parametr typu `T` ma ograniczenia, które nie dopuszcza wartości null, jest traktowany jako niedopuszczający wartości null w swoim zakresie.</span><span class="sxs-lookup"><span data-stu-id="41979-129">If a type parameter `T` has non-nullable constraints, it is treated as non-nullable within its scope.</span></span>

<span data-ttu-id="41979-130">Jeśli parametr typu jest nieograniczony lub ma tylko ograniczenia dopuszczające wartość null, sytuacja jest nieco bardziej złożona: oznacza to, że odpowiedni argument typu może być dopuszczany do wartości null lub nie dopuszczający *wartości null.*</span><span class="sxs-lookup"><span data-stu-id="41979-130">If a type parameter is unconstrained or has only nullable constraints, the situation is a little more complex: this means that the corresponding type argument could be *either* nullable or non-nullable.</span></span> <span data-ttu-id="41979-131">Bezpiecznym zadaniem do wykonania w tej sytuacji jest traktowanie parametru typu jako wartości null i niedopuszczający *wartości null,* co daje ostrzeżenia w przypadku naruszenia obu tych warunków.</span><span class="sxs-lookup"><span data-stu-id="41979-131">The safe thing to do in that situation is to treat the type parameter as *both* nullable and non-nullable, giving warnings when either is violated.</span></span> 

<span data-ttu-id="41979-132">Warto rozważać, czy jawne ograniczenia odwołań do wartości null powinny być dozwolone.</span><span class="sxs-lookup"><span data-stu-id="41979-132">It is worth considering whether explicit nullable reference constraints should be allowed.</span></span> <span data-ttu-id="41979-133">Należy jednak pamiętać, że nie można uniknąć, że typy odwołań dopuszczających wartość null nie są *jawnie* określone w pewnych przypadkach (dziedziczone ograniczenia).</span><span class="sxs-lookup"><span data-stu-id="41979-133">Note, however, that we cannot avoid having nullable reference types *implicitly* be constraints in certain cases (inherited constraints).</span></span>

<span data-ttu-id="41979-134">Ograniczenie `class` jest inne niż null.</span><span class="sxs-lookup"><span data-stu-id="41979-134">The `class` constraint is non-null.</span></span> <span data-ttu-id="41979-135">Możemy rozważyć, czy `class?` powinien być prawidłowym ograniczeniem null, co oznacza, że typ odwołania dopuszcza wartość null.</span><span class="sxs-lookup"><span data-stu-id="41979-135">We can consider whether `class?` should be a valid nullable constraint denoting "nullable reference type".</span></span>

## <a name="type-inference"></a><span data-ttu-id="41979-136">Wnioskowanie o typie</span><span class="sxs-lookup"><span data-stu-id="41979-136">Type inference</span></span>

<span data-ttu-id="41979-137">W przypadku wnioskowania o typie, jeśli typ współtworzenia ma typ referencyjny dopuszczający wartość null, Typ otrzymany powinien dopuszczać wartość null.</span><span class="sxs-lookup"><span data-stu-id="41979-137">In type inference, if a contributing type is a nullable reference type, the resulting type should be nullable.</span></span> <span data-ttu-id="41979-138">Innymi słowy, wartość zerowa jest propagowana.</span><span class="sxs-lookup"><span data-stu-id="41979-138">In other words, nullness is propagated.</span></span>

<span data-ttu-id="41979-139">Należy rozważyć, czy literał `null` jako wyrażenie uczestniczące ma wchodzić w skład wartości null.</span><span class="sxs-lookup"><span data-stu-id="41979-139">We should consider whether the `null` literal as a participating expression should contribute nullness.</span></span> <span data-ttu-id="41979-140">Nie jest to dzisiaj: dla typów wartości, które prowadzi do błędu, natomiast w przypadku typów referencyjnych wartość null została pomyślnie konwertowana na typ zwykły.</span><span class="sxs-lookup"><span data-stu-id="41979-140">It doesn't today: for value types it leads to an error, whereas for reference types the null successfully converts to the plain type.</span></span> 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a><span data-ttu-id="41979-141">Zmiany powodujące niezgodność</span><span class="sxs-lookup"><span data-stu-id="41979-141">Breaking changes</span></span>

<span data-ttu-id="41979-142">Ostrzeżenia o wartościach innych niż null są oczywistymi istotnymi zmianami w istniejącym kodzie i powinny towarzyszyć mechanizmem akceptacji.</span><span class="sxs-lookup"><span data-stu-id="41979-142">Non-null warnings are an obvious breaking change on existing code, and should be accompanied with an opt-in mechanism.</span></span>

<span data-ttu-id="41979-143">Mniej oczywiście ostrzeżenia dotyczące typów dopuszczających wartości null (jak opisano powyżej) są istotną zmianą istniejącego kodu w niektórych scenariuszach, w których wartość null jest niejawna:</span><span class="sxs-lookup"><span data-stu-id="41979-143">Less obviously, warnings from nullable types (as described above) are a breaking change on existing code in certain scenarios where the nullability is implicit:</span></span>

* <span data-ttu-id="41979-144">Parametry typu nieograniczonego będą traktowane jako niejawnie dopuszczające wartość null, więc przypisanie ich do `object` lub dostępu np. `ToString` zwróci ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="41979-144">Unconstrained type parameters will be treated as implicitly nullable, so assigning them to `object` or accessing e.g. `ToString` will yield warnings.</span></span>
* <span data-ttu-id="41979-145">Jeśli wnioskowanie o typie wnioskuje o wartości null z wyrażeń `null`, wówczas istniejący kod czasami będzie zwracać wartości null zamiast typów niedopuszczających wartości null, co może prowadzić do nowych ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="41979-145">if type inference infers nullness from `null` expressions, then existing code will sometimes yield nullable rather than non-nullable types, which can lead to new warnings.</span></span>

<span data-ttu-id="41979-146">Dlatego ostrzeżenia null muszą być również opcjonalne</span><span class="sxs-lookup"><span data-stu-id="41979-146">So nullable warnings also need to be optional</span></span>

<span data-ttu-id="41979-147">Na koniec Dodawanie adnotacji do istniejącego interfejsu API będzie stanowić istotną zmianę dla użytkowników, którzy wybrały ostrzeżenia, podczas uaktualniania biblioteki.</span><span class="sxs-lookup"><span data-stu-id="41979-147">Finally, adding annotations to an existing API will be a breaking change to users who have opted in to warnings, when they upgrade the library.</span></span> <span data-ttu-id="41979-148">Jest to również konieczne, aby można było wybrać lub wycofać. "Chcę rozwiązać usterkę, ale nie jestem gotowy do pracy z nowymi adnotacjami"</span><span class="sxs-lookup"><span data-stu-id="41979-148">This, too, merits the ability to opt in or out. "I want the bug fixes, but I am not ready to deal with their new annotations"</span></span>

<span data-ttu-id="41979-149">Podsumowując, musisz mieć możliwość wyboru z:</span><span class="sxs-lookup"><span data-stu-id="41979-149">In summary, you need to be able to opt in/out of:</span></span>
* <span data-ttu-id="41979-150">Ostrzeżenia dopuszczające wartość null</span><span class="sxs-lookup"><span data-stu-id="41979-150">Nullable warnings</span></span>
* <span data-ttu-id="41979-151">Ostrzeżenia o wartościach innych niż null</span><span class="sxs-lookup"><span data-stu-id="41979-151">Non-null warnings</span></span>
* <span data-ttu-id="41979-152">Ostrzeżenia z adnotacji w innych plikach</span><span class="sxs-lookup"><span data-stu-id="41979-152">Warnings from annotations in other files</span></span>

<span data-ttu-id="41979-153">Stopień szczegółowości wyboru zaproponuje model podobny do analizatora, gdzie swaths kodu może być dołączany do dyrektywy pragmy, a poziomy ważności mogą być wybierane przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="41979-153">The granularity of the opt-in suggests an analyzer-like model, where swaths of code can opt in and out with pragmas and severity levels can be chosen by the user.</span></span> <span data-ttu-id="41979-154">Ponadto opcje poszczególnych bibliotek ("Ignoruj adnotacje z JSON.NET, dopóki nie zajdzie do rozpatrzenia") mogą być można wyrazić elementu w kodzie jako atrybuty.</span><span class="sxs-lookup"><span data-stu-id="41979-154">Additionally, per-library options ("ignore the annotations from JSON.NET until I'm ready to deal with the fall out") may be expressible in code as attributes.</span></span>

<span data-ttu-id="41979-155">Projekt doświadczenia z uczestnictwa/przejścia jest decydujący dla sukcesu i użyteczności tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="41979-155">The design of the opt-in/transition experience is crucial to the success and usefulness of this feature.</span></span> <span data-ttu-id="41979-156">Musimy upewnić się, że:</span><span class="sxs-lookup"><span data-stu-id="41979-156">We need to make sure that:</span></span>

* <span data-ttu-id="41979-157">Użytkownicy mogą stopniowo przyjmować sprawdzanie wartości null, gdy chcą</span><span class="sxs-lookup"><span data-stu-id="41979-157">Users can adopt nullability checking gradually as they want to</span></span>
* <span data-ttu-id="41979-158">Autorzy biblioteki mogą dodawać adnotacje dotyczące wartości null bez obaw klientów.</span><span class="sxs-lookup"><span data-stu-id="41979-158">Library authors can add nullability annotations without fear of breaking customers</span></span>
* <span data-ttu-id="41979-159">Pomimo tego nie ma sensu "Configuration okropnej"</span><span class="sxs-lookup"><span data-stu-id="41979-159">Despite these, there is not a sense of "configuration nightmare"</span></span>

## <a name="tweaks"></a><span data-ttu-id="41979-160">Ulepszenia</span><span class="sxs-lookup"><span data-stu-id="41979-160">Tweaks</span></span>

<span data-ttu-id="41979-161">Możemy rozważyć nieużywanie adnotacji `?` lokalnych, ale po prostu zapoznaj się z tym, czy są one używane zgodnie z przypisanymi do nich informacjami.</span><span class="sxs-lookup"><span data-stu-id="41979-161">We could consider not using the `?` annotations on locals, but just observing whether they are used in accordance with what gets assigned to them.</span></span> <span data-ttu-id="41979-162">Nie podoba mi się to; Myślę, że powinna być jednolicie poinformowania użytkowników.</span><span class="sxs-lookup"><span data-stu-id="41979-162">I don't favor this; I think we should uniformly let people express their intent.</span></span>

<span data-ttu-id="41979-163">Możemy uwzględnić skróconą `T! x` dla parametrów, która automatycznie generuje sprawdzenie wartości null środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="41979-163">We could consider a shorthand `T! x` on parameters, that auto-generates a runtime null check.</span></span>

<span data-ttu-id="41979-164">Niektóre wzorce dla typów ogólnych, takie jak `FirstOrDefault` lub `TryGet`, mają nieco brzmienia zachowanie przy użyciu argumentów typu niedopuszczających wartości null, ponieważ jawnie zwracają wartości domyślne w niektórych sytuacjach.</span><span class="sxs-lookup"><span data-stu-id="41979-164">Certain patterns on generic types, such as `FirstOrDefault` or `TryGet`, have slightly weird behavior with non-nullable type arguments, because they explicitly yield default values in certain situations.</span></span> <span data-ttu-id="41979-165">Możemy spróbować Nuance system typów w celu lepszego dopasowania.</span><span class="sxs-lookup"><span data-stu-id="41979-165">We could try to nuance the type system to accommodate these better.</span></span> <span data-ttu-id="41979-166">Na przykład możemy zezwolić na `?` w przypadku parametrów typu nieograniczonego, mimo że argument typu może już mieć wartość null.</span><span class="sxs-lookup"><span data-stu-id="41979-166">For instance, we could allow `?` on unconstrained type parameters, even though the type argument could already be nullable.</span></span> <span data-ttu-id="41979-167">Mam wątpliwości, że jest to możliwe, i prowadzi do Weirdness związanych z interakcją z typami *wartości* null.</span><span class="sxs-lookup"><span data-stu-id="41979-167">I doubt that it is worth it, and it leads to weirdness related to interaction with nullable *value* types.</span></span> 

## <a name="nullable-value-types"></a><span data-ttu-id="41979-168">Typy wartości dopuszczające wartość null</span><span class="sxs-lookup"><span data-stu-id="41979-168">Nullable value types</span></span>

<span data-ttu-id="41979-169">Możemy rozważyć przyjęcie niektórych powyższych semantyki dla typów wartości null.</span><span class="sxs-lookup"><span data-stu-id="41979-169">We could consider adopting some of the above semantics for nullable value types as well.</span></span>

<span data-ttu-id="41979-170">Wspomniano już wnioskowanie o typie, w którym możemy wnioskować `int?` od `(7, null)`, zamiast tylko wydawania błędu.</span><span class="sxs-lookup"><span data-stu-id="41979-170">We already mentioned type inference, where we could infer `int?` from `(7, null)`, instead of just giving an error.</span></span>

<span data-ttu-id="41979-171">Kolejną możliwością jest zastosowanie analizy przepływu do wartości null.</span><span class="sxs-lookup"><span data-stu-id="41979-171">Another opportunity is to apply the flow analysis to nullable value types.</span></span> <span data-ttu-id="41979-172">Jeśli są uznawane za niemające wartości null, możemy faktycznie zezwolić na użycie jako typu niedopuszczający wartości null w określonych sposobach (np. dostęp do elementów członkowskich).</span><span class="sxs-lookup"><span data-stu-id="41979-172">When they are deemed non-null, we could actually allow using as the non-nullable type in certain ways (e.g. member access).</span></span> <span data-ttu-id="41979-173">Należy zachować ostrożność, aby można było sprawdzić, czy elementy, które są *już* wykonywane na wartości typu null, będą preferowane ze względu na zgodność z poprzednimi przyczynami.</span><span class="sxs-lookup"><span data-stu-id="41979-173">We just have to be careful that the things that you can *already* do on a nullable value type will be preferred, for back compat reasons.</span></span>
