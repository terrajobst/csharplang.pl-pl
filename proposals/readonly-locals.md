---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484483"
---
# <a name="readonly-locals-and-parameters"></a><span data-ttu-id="0e92c-101">locale i parametry w trybie tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="0e92c-101">readonly locals and parameters</span></span>

* <span data-ttu-id="0e92c-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="0e92c-102">[x] Proposed</span></span>
* <span data-ttu-id="0e92c-103">[] Prototyp</span><span class="sxs-lookup"><span data-stu-id="0e92c-103">[ ] Prototype</span></span>
* <span data-ttu-id="0e92c-104">[] Implementacja</span><span class="sxs-lookup"><span data-stu-id="0e92c-104">[ ] Implementation</span></span>
* <span data-ttu-id="0e92c-105">[] — Specyfikacja</span><span class="sxs-lookup"><span data-stu-id="0e92c-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="0e92c-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="0e92c-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="0e92c-107">Zezwalaj na dodawanie adnotacji do elementów lokalnych i parametrów jako tylko do odczytu w celu zapobieżenia przeniesieniu płytki do tych zmiennych lokalnych i parametrów.</span><span class="sxs-lookup"><span data-stu-id="0e92c-107">Allow locals and parameters to be annotated as readonly in order to prevent shallow mutation of those locals and parameters.</span></span>

## <a name="motivation"></a><span data-ttu-id="0e92c-108">Motywacją</span><span class="sxs-lookup"><span data-stu-id="0e92c-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="0e92c-109">Obecnie słowo kluczowe `readonly` można zastosować do pól; pozwala to upewnić się, że w czasie konstruowania pola można pisać tylko podczas konstrukcji (konstrukcja statyczna w przypadku pola statycznego lub konstruowania wystąpienia w przypadku pola wystąpienia), które ułatwia deweloperom uniknięcie błędów przez przypadkowe zastąpienie stanu, którego nie należy modyfikować.</span><span class="sxs-lookup"><span data-stu-id="0e92c-109">Today, the `readonly` keyword can be applied to fields; this has the effect of ensuring that a field can only be written to during construction (static construction in the case of a static field, or instance construction in the case of an instance field), which helps developers avoid mistakes by accidentally overwriting state which should not be modified.</span></span> <span data-ttu-id="0e92c-110">Ale pola nie są jedynymi miejscami, w których deweloperzy chcą mieć pewność, że wartości nie są mutacje.</span><span class="sxs-lookup"><span data-stu-id="0e92c-110">But fields aren't the only places developers want to ensure that values aren't mutated.</span></span> <span data-ttu-id="0e92c-111">W szczególności często istnieje możliwość utworzenia zmiennej lokalnej do przechowywania stanu tymczasowego i przypadkowego aktualizowania stanu tymczasowego może spowodować błędne obliczenia i inne błędy, zwłaszcza wtedy, gdy takie "lokalne" są przechwytywane w wyrażeniach lambda, w których momencie zostały zniesione do pól, ale obecnie nie jest to możliwe, aby oznaczyć takie zniesione pola jako `readonly`.</span><span class="sxs-lookup"><span data-stu-id="0e92c-111">In particular, it's common to create a local variable to store temporary state, and accidentally updating that temporary state can result in erroneous calculations and other such bugs, especially when such "locals" are captured in lambdas, at which point they are lifted to fields, but there's no way today to mark such lifted fields as `readonly`.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="0e92c-112">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="0e92c-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="0e92c-113">Elementy lokalne będą mieć adnotację jako `readonly`, a kompilator gwarantuje, że są one ustawione tylko w czasie deklaracji (niektóre elementy lokalne w programie C# są już niejawnie do odczytu, takie jak Zmienna iteracji w pętli "foreach" lub użyta zmienna w bloku "Using", ale obecnie Deweloper nie ma możliwości oznaczania innych elementów lokalnych jako `readonly`).</span><span class="sxs-lookup"><span data-stu-id="0e92c-113">Locals will be annotatable as `readonly` as well, with the compiler ensuring that they're only set at the time of declaration (certain locals in C# are already implicitly readonly, such as the iteration variable in a 'foreach' loop or the used variable in a 'using' block, but currently a developer has no ability to mark other locals as `readonly`).</span></span> <span data-ttu-id="0e92c-114">Takie `readonly` lokalne muszą mieć inicjatora:</span><span class="sxs-lookup"><span data-stu-id="0e92c-114">Such `readonly` locals must have an initializer:</span></span>

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="0e92c-115">I jako skrót dla `readonly var`, można użyć istniejącego kontekstowego słowa kluczowego `let`, np.</span><span class="sxs-lookup"><span data-stu-id="0e92c-115">And as shorthand for `readonly var`, the existing contextual keyword `let` may be used, e.g.</span></span>

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="0e92c-116">Nie istnieją żadne specjalne ograniczenia dotyczące tego, co może być inicjatorem, i mogą być obecnie ważne jako inicjator dla elementów lokalnych, np.</span><span class="sxs-lookup"><span data-stu-id="0e92c-116">There are no special constraints for what the initializer can be, and can be anything currently valid as an initializer for locals, e.g.</span></span>

```csharp
readonly T data = arg1 ?? arg2;
```

<span data-ttu-id="0e92c-117">`readonly` w lokalnych jest szczególnie cenny podczas pracy z wyrażeniami lambda i zamknięciami.</span><span class="sxs-lookup"><span data-stu-id="0e92c-117">`readonly` on locals is particularly valuable when working with lambdas and closures.</span></span> <span data-ttu-id="0e92c-118">Gdy metoda anonimowa lub lambda uzyskuje dostęp do stanu lokalnego z otaczającego zakresu, ten stan jest przechwytywany do zamknięcia przez kompilator, który jest reprezentowany przez "Klasa wyświetlania".</span><span class="sxs-lookup"><span data-stu-id="0e92c-118">When an anonymous method or lambda accesses local state from the enclosing scope, that state is captured into a closure by the compiler, which is represented by a "display class."</span></span>  <span data-ttu-id="0e92c-119">Każda wartość "lokalna", która jest przechwytywana, to pole w tej klasie, ale ponieważ kompilator generuje to pole w Twoim imieniu, nie ma możliwości dodawania adnotacji jako `readonly`, aby zapobiec błędnemu zapisywaniu wyrażenia lambda do "lokalnego" (w cudzysłowie, ponieważ nie jest to lokalne, co najmniej w wyniku MSIL).</span><span class="sxs-lookup"><span data-stu-id="0e92c-119">Each "local" that's captured is a field in this class, yet because the compiler is generating this field on your behalf, you have no opportunity to annotate it as `readonly` in order to prevent the lambda from erroneously writing to the "local" (in quotes because it's really not a local, at least not in the resulting MSIL).</span></span> <span data-ttu-id="0e92c-120">W przypadku `readonly` lokalnych kompilator może uniemożliwić zapis lambda z zapisu do lokalnego, który jest szczególnie cenny w scenariuszach obejmujących wielowątkowość, w przypadku których błędny zapis może spowodować niebezpieczną, ale rzadki i trudno znaleźć błąd współbieżności.</span><span class="sxs-lookup"><span data-stu-id="0e92c-120">With `readonly` locals, the compiler can prevent the lambda from writing to local, which is particularly valuable in scenarios involving multithreading where an erroneous write could result in a dangerous but rare and hard-to-find concurrency bug.</span></span>

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

<span data-ttu-id="0e92c-121">Jako specjalną formę lokalnego, parametry zostaną również opatrzone adnotacją jako `readonly`.</span><span class="sxs-lookup"><span data-stu-id="0e92c-121">As a special form of local, parameters will also be annotatable as `readonly`.</span></span> <span data-ttu-id="0e92c-122">Nie będzie to miało wpływu na to, co obiekt wywołujący metody jest w stanie przekazać do parametru (tak jak nie ma żadnego ograniczenia dotyczącego wartości, które mogą być przechowywane w `readonly` polu), ale podobnie jak w przypadku dowolnego `readonly` lokalnego, kompilator zabroni kodu przed zapisem do parametru po deklaracji, co oznacza, że treść metody jest zabronione przed zapisem do parametru.</span><span class="sxs-lookup"><span data-stu-id="0e92c-122">This would have no effect on what the caller of the method is able to pass to the parameter (just as there's no constraint on what values may be stored into a `readonly` field), but as with any `readonly` local, the compiler would prohibit code from writing to the parameter after declaration, which means the body of the method is prohibited from writing to the parameter.</span></span>

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

<span data-ttu-id="0e92c-123">`readonly` parametry nie wpływają na sygnaturę/metadane emitowane przez kompilator dla tej metody i po prostu wpływają na sposób, w jaki kompilator obsługuje kompilację treści metody.</span><span class="sxs-lookup"><span data-stu-id="0e92c-123">`readonly` parameters do not affect the signature/metadata emitted by the compiler for that method, and simply affect how the compiler handles the compilation of the method's body.</span></span> <span data-ttu-id="0e92c-124">W tym celu na przykład podstawowa metoda wirtualna może mieć parametr `readonly` i ten parametr może być zapisywalny w przesłonięciu.</span><span class="sxs-lookup"><span data-stu-id="0e92c-124">Thus, for example, a base virtual method could have a `readonly` parameter, and that parameter could be writable in an override.</span></span>

<span data-ttu-id="0e92c-125">Podobnie jak w przypadku pól, `readonly` dla ustawień lokalnych i parametrów ma wpływ na lokalizację przechowywania, ale nie ma przechodniego wpływu na Graf obiektu.</span><span class="sxs-lookup"><span data-stu-id="0e92c-125">As with fields, `readonly` for locals and parameters is shallow, affecting the storage location but not transitively affecting the object graph.</span></span> <span data-ttu-id="0e92c-126">Jednak, podobnie jak w przypadku pól, wywoływanie metody na `readonly` struktura lokalna/parametr w rzeczywistości utworzy kopię struktury i Wywołaj metodę na kopii, aby uniknąć wewnętrznej mutacji `this`.</span><span class="sxs-lookup"><span data-stu-id="0e92c-126">However, also as with fields, calling a method on a `readonly` local/parameter struct will actually make a copy of the struct and call the method on the copy, in order to avoid internal mutation of `this`.</span></span>

<span data-ttu-id="0e92c-127">`readonly` zmiennych lokalnych i parametrów nie można przekazywać jako argumentów `ref` lub `out`, chyba że jest również obsługiwany `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="0e92c-127">`readonly` locals and parameters can't be passed as `ref` or `out` arguments, unless/until `ref readonly` is also supported.</span></span>

## <a name="alternatives"></a><span data-ttu-id="0e92c-128">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="0e92c-128">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="0e92c-129">`val` może służyć jako alternatywna skrót do `let`.</span><span class="sxs-lookup"><span data-stu-id="0e92c-129">`val` could be used as an alternative shorthand to `let`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="0e92c-130">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="0e92c-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="0e92c-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: mam pytanie, jak obsłużyć `ref readonly` niezależnie od tej propozycji.</span><span class="sxs-lookup"><span data-stu-id="0e92c-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: I've left the question of how to handle `ref readonly` as separate from this proposal.</span></span>
- <span data-ttu-id="0e92c-132">Ta propozycja nie jest zgodna z strukturami ReadOnly/niezmiennymi typami.</span><span class="sxs-lookup"><span data-stu-id="0e92c-132">This proposal does not tackle readonly structs / immutable types.</span></span> <span data-ttu-id="0e92c-133">To pozostało na potrzeby oddzielnej propozycji.</span><span class="sxs-lookup"><span data-stu-id="0e92c-133">That is left for a separate proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="0e92c-134">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="0e92c-134">Design meetings</span></span>

- <span data-ttu-id="0e92c-135">Zwięźle omówić o 21 stycznia 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span><span class="sxs-lookup"><span data-stu-id="0e92c-135">Briefly discussed on Jan 21, 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span></span>
