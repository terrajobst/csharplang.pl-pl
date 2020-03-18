---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484490"
---
# <a name="suppress-emitting-of-localsinit-flag"></a><span data-ttu-id="9e371-101">Pomiń emitowanie flagi `localsinit`.</span><span class="sxs-lookup"><span data-stu-id="9e371-101">Suppress emitting of `localsinit` flag.</span></span>

* <span data-ttu-id="9e371-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="9e371-102">[x] Proposed</span></span>
* <span data-ttu-id="9e371-103">[] Prototyp: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="9e371-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="9e371-104">[] Implementacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="9e371-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="9e371-105">[] Specyfikacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="9e371-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="9e371-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="9e371-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="9e371-107">Zezwalaj na pomijanie emitowania flagi `localsinit` za pośrednictwem atrybutu `SkipLocalsInitAttribute`.</span><span class="sxs-lookup"><span data-stu-id="9e371-107">Allow suppressing emit of `localsinit` flag via `SkipLocalsInitAttribute` attribute.</span></span> 

## <a name="motivation"></a><span data-ttu-id="9e371-108">Motywacją</span><span class="sxs-lookup"><span data-stu-id="9e371-108">Motivation</span></span>
[motivation]: #motivation


### <a name="background"></a><span data-ttu-id="9e371-109">Tło</span><span class="sxs-lookup"><span data-stu-id="9e371-109">Background</span></span>
<span data-ttu-id="9e371-110">Dla specyfikacji CLR zmienne lokalne, które nie zawierają odwołań, nie są inicjowane do określonej wartości przez maszynę wirtualną/JIT.</span><span class="sxs-lookup"><span data-stu-id="9e371-110">Per CLR spec local variables that do not contain references are not initialized to a particular value by the VM/JIT.</span></span> <span data-ttu-id="9e371-111">Odczyt z takich zmiennych bez inicjalizacji jest bezpieczny dla typów, ale w przeciwnym razie zachowanie jest niezdefiniowane i specyficzne dla implementacji.</span><span class="sxs-lookup"><span data-stu-id="9e371-111">Reading from such variables without initialization is type-safe, but otherwise the behavior is undefined and implementation specific.</span></span> <span data-ttu-id="9e371-112">Zwykle niezainicjowane elementy lokalne zawierają wszelkie wartości pozostawione w pamięci, która jest teraz zajęta przez ramkę stosu.</span><span class="sxs-lookup"><span data-stu-id="9e371-112">Typically uninitialized locals contain whatever values were left in the memory that is now occupied by the stack frame.</span></span> <span data-ttu-id="9e371-113">Może to prowadzić do niedeterministycznych zachowań i trudno odtworzyć błędy.</span><span class="sxs-lookup"><span data-stu-id="9e371-113">That could lead to nondeterministic behavior and hard to reproduce bugs.</span></span> 

<span data-ttu-id="9e371-114">Istnieją dwa sposoby "przypisywania" zmiennej lokalnej:</span><span class="sxs-lookup"><span data-stu-id="9e371-114">There are two ways to "assign" a local variable:</span></span> 
- <span data-ttu-id="9e371-115">przechowując wartość lub</span><span class="sxs-lookup"><span data-stu-id="9e371-115">by storing a value or</span></span> 
- <span data-ttu-id="9e371-116">określając flagę `localsinit`, która wymusza, że wszystkie elementy, które są przydzielone, tworzą pulę pamięci lokalnej, która ma zostać zainicjowana zero: dotyczy to zarówno zmiennych lokalnych, jak i danych `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="9e371-116">by specifying `localsinit` flag which forces everything that is allocated form the local memory pool to be zero-initialized NOTE: this includes both local variables and `stackalloc` data.</span></span>    

<span data-ttu-id="9e371-117">Użycie niezainicjowanych danych jest niezalecane i nie jest dozwolone w kodzie możliwym do zweryfikowania.</span><span class="sxs-lookup"><span data-stu-id="9e371-117">Use of uninitialized data is discouraged and is not allowed in verifiable code.</span></span> <span data-ttu-id="9e371-118">Mimo że można udowodnić, że przez metodę analizy przepływu można określić, że algorytm weryfikacji jest ostrożny i po prostu wymaga, aby `localsinit` został ustawiony.</span><span class="sxs-lookup"><span data-stu-id="9e371-118">While it might be possible to prove that by the means of flow analysis, it is permitted for the verification algorithm to be conservative and simply require that `localsinit` is set.</span></span>

<span data-ttu-id="9e371-119">C# Kompilator historyczny emituje flagę `localsinit` na wszystkich metodach, które deklarują elementy lokalne.</span><span class="sxs-lookup"><span data-stu-id="9e371-119">Historically C# compiler emits `localsinit` flag on all methods that declare locals.</span></span>

<span data-ttu-id="9e371-120">C# Program korzysta z analizy o określonym przypisaniu, która jest bardziej rygorystyczna niż wymagana Specyfikacja środowiskaC# CLR (wymaga również określenia zakresu wartości lokalnych), dlatego nie gwarantuje, że kod wynikający zostanie formalnie zweryfikowany:</span><span class="sxs-lookup"><span data-stu-id="9e371-120">While C# employs definite-assignment analysis which is more strict than what CLR spec would require (C# also needs to consider scoping of locals), it is not strictly guaranteed that the resulting code would be formally verifiable:</span></span>
- <span data-ttu-id="9e371-121">Środowisko CLR C# i reguły mogą nie wyrażać zgody na to, czy przekazanie argumentu lokalnego jako `out` jest `use`.</span><span class="sxs-lookup"><span data-stu-id="9e371-121">CLR and C# rules may not agree on whether passing a local as `out` argument is a `use`.</span></span>
- <span data-ttu-id="9e371-122">Środowiska CLR C# i reguły mogą nie zgadzać się na traktowanie gałęzi warunkowych, gdy są znane warunki (Propagacja stała).</span><span class="sxs-lookup"><span data-stu-id="9e371-122">CLR and C# rules may not agree on treatment of conditional branches when conditions are known (constant propagation).</span></span>
- <span data-ttu-id="9e371-123">Środowisko CLR może również po prostu wymagać `localinits`, ponieważ jest to dozwolone.</span><span class="sxs-lookup"><span data-stu-id="9e371-123">CLR could as well simply require `localinits`, since that is permitted.</span></span>  

### <a name="problem"></a><span data-ttu-id="9e371-124">Problem</span><span class="sxs-lookup"><span data-stu-id="9e371-124">Problem</span></span>
<span data-ttu-id="9e371-125">W przypadku aplikacji o wysokiej wydajności koszt wymuszonej inicjacji zero może być zauważalny.</span><span class="sxs-lookup"><span data-stu-id="9e371-125">In high-performance application the cost of forced zero-initialization could be noticeable.</span></span> <span data-ttu-id="9e371-126">Jest to szczególnie zauważalne, gdy `stackalloc` jest używany.</span><span class="sxs-lookup"><span data-stu-id="9e371-126">It is particularly noticeable when `stackalloc` is used.</span></span>

<span data-ttu-id="9e371-127">W niektórych przypadkach JIT może Elide początkowej zero inicjacji pojedynczych lokalnych, gdy takie inicjowanie jest "zabite" przez kolejne przydziały.</span><span class="sxs-lookup"><span data-stu-id="9e371-127">In some cases JIT can elide initial zero-initialization of individual locals when such initialization is "killed" by subsequent assignments.</span></span> <span data-ttu-id="9e371-128">Nie wszystkie JITs to i że Optymalizacja ma limity.</span><span class="sxs-lookup"><span data-stu-id="9e371-128">Not all JITs do this and such optimization has limits.</span></span> <span data-ttu-id="9e371-129">Nie pomaga w `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="9e371-129">It does not help with `stackalloc`.</span></span>

<span data-ttu-id="9e371-130">Aby zilustrować, że problem występuje w rzeczywistości, występuje znana usterka, w której metoda nie zawiera żadnych `IL` lokalnych nie ma flagi `localsinit`.</span><span class="sxs-lookup"><span data-stu-id="9e371-130">To illustrate that the problem is real - there is a known bug where a method not containing any `IL` locals would not have `localsinit` flag.</span></span> <span data-ttu-id="9e371-131">Usterka jest już wykorzystywana przez użytkowników przez umieszczenie `stackalloc` w takich metodach — celowo aby uniknąć kosztów inicjacji.</span><span class="sxs-lookup"><span data-stu-id="9e371-131">The bug is already being exploited by users by putting `stackalloc` into such methods - intentionally to avoid initialization costs.</span></span> <span data-ttu-id="9e371-132">Jest to pomimo faktu, że brak `IL` lokalnych jest niestabilną metryką i może się różnić w zależności od zmian w strategii codegen.</span><span class="sxs-lookup"><span data-stu-id="9e371-132">That is despite the fact that absence of `IL` locals is an unstable metric and may vary depending on changes in codegen strategy.</span></span> <span data-ttu-id="9e371-133">Usterka powinna zostać naprawiona, a użytkownicy powinni uzyskać bardziej udokumentowaną i niezawodną metodę pomijania flagi.</span><span class="sxs-lookup"><span data-stu-id="9e371-133">The bug should be fixed and users should get a more documented and reliable way of suppressing the flag.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="9e371-134">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="9e371-134">Detailed design</span></span>

<span data-ttu-id="9e371-135">Zezwól na określanie `System.Runtime.CompilerServices.SkipLocalsInitAttribute` jako sposobu, aby wypowiedzieć kompilatorowi, że nie emituje `localsinit` flagę.</span><span class="sxs-lookup"><span data-stu-id="9e371-135">Allow specifying `System.Runtime.CompilerServices.SkipLocalsInitAttribute` as a way to tell the compiler to not emit `localsinit` flag.</span></span>
 
<span data-ttu-id="9e371-136">Wynikiem tego jest to, że wartości lokalne nie mogą być inicjowane przez kompilator JIT, który jest w większości przypadków niezauważalny w C#.</span><span class="sxs-lookup"><span data-stu-id="9e371-136">The end result of this will be that the locals may not be zero-initialized by the JIT, which is in most cases unobservable in C#.</span></span>  
<span data-ttu-id="9e371-137">Oprócz tego `stackalloc` dane nie będą inicjowane od zera.</span><span class="sxs-lookup"><span data-stu-id="9e371-137">In addition to that `stackalloc` data will not be zero-initialized.</span></span> <span data-ttu-id="9e371-138">Jest to w nieskończoność zauważalne, ale jest również najbardziej umotywowany scenariusz.</span><span class="sxs-lookup"><span data-stu-id="9e371-138">That is definitely observable, but also is the most motivating scenario.</span></span>

<span data-ttu-id="9e371-139">Dozwolone i rozpoznawane elementy docelowe atrybutów to: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`.</span><span class="sxs-lookup"><span data-stu-id="9e371-139">Permitted and recognized attribute targets are: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`.</span></span> <span data-ttu-id="9e371-140">Jednak kompilator nie wymaga, aby atrybut został zdefiniowany z wymienionymi obiektami docelowymi, a w tym przypadku zestaw atrybutu jest zdefiniowany.</span><span class="sxs-lookup"><span data-stu-id="9e371-140">However compiler will not require that attribute is defined with the listed targets nor it will care in which assembly the attribute is defined.</span></span> 

<span data-ttu-id="9e371-141">Gdy atrybut jest określony w kontenerze (`class`, `module`, zawierający metodę dla zagnieżdżonej metody,...), flaga ma wpływ na wszystkie metody zawarte w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="9e371-141">When attribute is specified on a container (`class`, `module`, containing method for a nested method, ...), the flag affects all methods contained within the container.</span></span>

<span data-ttu-id="9e371-142">Metody z syntezą "dziedziczą" flagę z kontenera logicznego/właściciela.</span><span class="sxs-lookup"><span data-stu-id="9e371-142">Synthesized methods "inherit" the flag from the logical container/owner.</span></span> 

<span data-ttu-id="9e371-143">Flaga ma wpływ tylko na strategię codegen dla rzeczywistych treści metod.</span><span class="sxs-lookup"><span data-stu-id="9e371-143">The flag affects only codegen strategy for actual method bodies.</span></span> <span data-ttu-id="9e371-144">I.E.</span><span class="sxs-lookup"><span data-stu-id="9e371-144">I.E.</span></span> <span data-ttu-id="9e371-145">Flaga nie ma wpływu na metody abstrakcyjne i nie jest propagowana do zastępowania/implementowania metod.</span><span class="sxs-lookup"><span data-stu-id="9e371-145">the flag has no effect on abstract methods and is not propagated to overriding/implementing methods.</span></span>

<span data-ttu-id="9e371-146">Jest to jawnie **_Funkcja kompilatora_** , a **_nie funkcja języka_** .</span><span class="sxs-lookup"><span data-stu-id="9e371-146">This is explicitly a **_compiler feature_** and **_not a language feature_**.</span></span>  
<span data-ttu-id="9e371-147">Podobnie jak w przypadku przełączników wiersza polecenia kompilatora funkcja kontroluje szczegóły implementacji konkretnej strategii codegen i nie musi być wymagana przez C# specyfikację.</span><span class="sxs-lookup"><span data-stu-id="9e371-147">Similarly to compiler command line switches the feature controls implementation details of a particular codegen strategy and does not need to be required by the C# spec.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="9e371-148">Wady</span><span class="sxs-lookup"><span data-stu-id="9e371-148">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="9e371-149">Stare/inne kompilatory mogą nie przestrzegać atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9e371-149">Old/other compilers may not honor the attribute.</span></span>
<span data-ttu-id="9e371-150">Ignorowanie atrybutu jest zgodne.</span><span class="sxs-lookup"><span data-stu-id="9e371-150">Ignoring the attribute is compatible behavior.</span></span> <span data-ttu-id="9e371-151">Może to spowodować niewielkie trafienie wydajności.</span><span class="sxs-lookup"><span data-stu-id="9e371-151">Only may result in a slight perf hit.</span></span>

- <span data-ttu-id="9e371-152">Flaga `localinits` nie może wyzwolić błędów weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="9e371-152">The code without `localinits` flag may trigger verification failures.</span></span>
<span data-ttu-id="9e371-153">Użytkownicy, którzy pytają o tę funkcję, zwykle nie są zainteresowani weryfikacją.</span><span class="sxs-lookup"><span data-stu-id="9e371-153">Users that ask for this feature are generally unconcerned with verifiability.</span></span> 
 
- <span data-ttu-id="9e371-154">Zastosowanie atrybutu na wyższym poziomie niż w przypadku pojedynczej metody ma wpływ nielokalny, który jest zauważalny w przypadku użycia `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="9e371-154">Applying the attribute at higher levels than an individual method has nonlocal effect, which is observable when `stackalloc` is used.</span></span> <span data-ttu-id="9e371-155">Jest to jednak najbardziej żądany scenariusz.</span><span class="sxs-lookup"><span data-stu-id="9e371-155">Yet, this is the most requested scenario.</span></span>

## <a name="alternatives"></a><span data-ttu-id="9e371-156">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="9e371-156">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="9e371-157">Pomiń flagę `localinits`, gdy metoda jest zadeklarowana w kontekście `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="9e371-157">omit `localinits` flag when method is declared in `unsafe` context.</span></span> <span data-ttu-id="9e371-158">Może to spowodować, że zachowanie dyskretne i niebezpieczne zmieni się z deterministyczne na niejednoznaczne w przypadku `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="9e371-158">That could cause silent and dangerous behavior change from deterministic to nondeterministic in a case of `stackalloc`.</span></span>

- <span data-ttu-id="9e371-159">Pomiń flagę `localinits`.</span><span class="sxs-lookup"><span data-stu-id="9e371-159">omit `localinits` flag always.</span></span>
<span data-ttu-id="9e371-160">Nawet nierówne.</span><span class="sxs-lookup"><span data-stu-id="9e371-160">Even worse than above.</span></span>

- <span data-ttu-id="9e371-161">Pomiń flagę `localinits`, chyba że `stackalloc` jest używana w treści metody.</span><span class="sxs-lookup"><span data-stu-id="9e371-161">omit `localinits` flag unless `stackalloc` is used in the method body.</span></span>
<span data-ttu-id="9e371-162">Nie dotyczy najbardziej żądanego scenariusza i może wyłączyć kod niemożliwy do zweryfikowania bez opcji przywrócenia tej z powrotem.</span><span class="sxs-lookup"><span data-stu-id="9e371-162">Does not address the most requested scenario and may turn code unverifiable with no option to revert that back.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="9e371-163">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="9e371-163">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="9e371-164">Czy atrybut ma być rzeczywiście emitowany do metadanych?</span><span class="sxs-lookup"><span data-stu-id="9e371-164">Should the attribute be actually emitted to metadata?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="9e371-165">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="9e371-165">Design meetings</span></span>

<span data-ttu-id="9e371-166">Jeszcze nie.</span><span class="sxs-lookup"><span data-stu-id="9e371-166">None yet.</span></span> 