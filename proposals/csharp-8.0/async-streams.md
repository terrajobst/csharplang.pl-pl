---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485260"
---
# <a name="async-streams"></a><span data-ttu-id="f90df-101">Strumienie asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="f90df-101">Async Streams</span></span>

* <span data-ttu-id="f90df-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="f90df-102">[x] Proposed</span></span>
* <span data-ttu-id="f90df-103">[x] prototyp</span><span class="sxs-lookup"><span data-stu-id="f90df-103">[x] Prototype</span></span>
* <span data-ttu-id="f90df-104">[] Implementacja</span><span class="sxs-lookup"><span data-stu-id="f90df-104">[ ] Implementation</span></span>
* <span data-ttu-id="f90df-105">[] — Specyfikacja</span><span class="sxs-lookup"><span data-stu-id="f90df-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="f90df-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="f90df-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="f90df-107">C#Program obsługuje metody iteratora i metody asynchroniczne, ale nie obsługuje metody, która jest zarówno iteratorem, jak i asynchronicznym.</span><span class="sxs-lookup"><span data-stu-id="f90df-107">C# has support for iterator methods and async methods, but no support for a method that is both an iterator and an async method.</span></span>  <span data-ttu-id="f90df-108">Należy to skorygować przez umożliwienie `await` do użycia w nowej formie iteratora `async`, która zwraca `IAsyncEnumerable<T>` lub `IAsyncEnumerator<T>`, a nie `IEnumerable<T>` lub `IEnumerator<T>`, z `IAsyncEnumerable<T>` możliwego do użycia w nowym `await foreach`.</span><span class="sxs-lookup"><span data-stu-id="f90df-108">We should rectify this by allowing for `await` to be used in a new form of `async` iterator, one that returns an `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` rather than an `IEnumerable<T>` or `IEnumerator<T>`, with `IAsyncEnumerable<T>` consumable in a new `await foreach`.</span></span>  <span data-ttu-id="f90df-109">Interfejs `IAsyncDisposable` jest również używany do włączania czyszczenia asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="f90df-109">An `IAsyncDisposable` interface is also used to enable asynchronous cleanup.</span></span>

## <a name="related-discussion"></a><span data-ttu-id="f90df-110">Pokrewne dyskusje</span><span class="sxs-lookup"><span data-stu-id="f90df-110">Related discussion</span></span>
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a><span data-ttu-id="f90df-111">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="f90df-111">Detailed design</span></span>
[design]: #detailed-design

## <a name="interfaces"></a><span data-ttu-id="f90df-112">Interfejsy</span><span class="sxs-lookup"><span data-stu-id="f90df-112">Interfaces</span></span>

### <a name="iasyncdisposable"></a><span data-ttu-id="f90df-113">IAsyncDisposable</span><span class="sxs-lookup"><span data-stu-id="f90df-113">IAsyncDisposable</span></span>

<span data-ttu-id="f90df-114">Istnieje wiele dyskusji na temat `IAsyncDisposable` (np. https://github.com/dotnet/roslyn/issues/114) i tego, czy jest to dobre pomysł.</span><span class="sxs-lookup"><span data-stu-id="f90df-114">There has been much discussion of `IAsyncDisposable` (e.g. https://github.com/dotnet/roslyn/issues/114) and whether it's a good idea.</span></span>  <span data-ttu-id="f90df-115">Jednak jest to wymagana koncepcja do dodania w ramach obsługi iteratorów asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="f90df-115">However, it's a required concept to add in support of async iterators.</span></span>  <span data-ttu-id="f90df-116">Ponieważ bloki `finally` mogą zawierać `await`s, a ponieważ bloki `finally` muszą być uruchamiane w ramach usuwania iteratorów, potrzebujemy asynchronicznego usuwania.</span><span class="sxs-lookup"><span data-stu-id="f90df-116">Since `finally` blocks may contain `await`s, and since `finally` blocks need to be run as part of disposing of iterators, we need async disposal.</span></span>  <span data-ttu-id="f90df-117">Jest to również szczególnie przydatne w przypadku, gdy czyszczenie zasobów może trwać pewien czas, np. zamykając pliki (wymagające opróżnień), wyrejestrować wywołania zwrotne i zapewnić sposób, aby wiedzieć, kiedy Wyrejestrowanie zostało zakończone itd.</span><span class="sxs-lookup"><span data-stu-id="f90df-117">It's also just generally useful any time cleaning up of resources might take any period of time, e.g. closing files (requiring flushes), deregistering callbacks and providing a way to know when deregistration has completed, etc.</span></span>

<span data-ttu-id="f90df-118">Następujący interfejs jest dodawany do podstawowych bibliotek platformy .NET (np. System. private. CoreLib/system. Runtime):</span><span class="sxs-lookup"><span data-stu-id="f90df-118">The following interface is added to the core .NET libraries (e.g. System.Private.CoreLib / System.Runtime):</span></span>
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
<span data-ttu-id="f90df-119">Podobnie jak w przypadku `Dispose`, możliwe jest wywoływanie `DisposeAsync` wiele razy, a kolejne wywołania po pierwszej należy traktować jako nops, zwracając synchronicznie zakończone pomyślne zadanie (`DisposeAsync` nie musi być bezpieczny wątkowo, chociaż nie musi obsługiwać współbieżnego wywołania).</span><span class="sxs-lookup"><span data-stu-id="f90df-119">As with `Dispose`, invoking `DisposeAsync` multiple times is acceptable, and subsequent invocations after the first should be treated as nops, returning a synchronously completed successful task (`DisposeAsync` need not be thread-safe, though, and need not support concurrent invocation).</span></span>  <span data-ttu-id="f90df-120">Ponadto typy mogą implementować zarówno `IDisposable`, jak i `IAsyncDisposable`, a jeśli tak, to w podobny sposób można wywołać `Dispose`, a następnie `DisposeAsync` lub na odwrót, ale tylko pierwszy powinien być znaczący, a kolejne wywołania metody muszą być NOP.</span><span class="sxs-lookup"><span data-stu-id="f90df-120">Further, types may implement both `IDisposable` and `IAsyncDisposable`, and if they do, it's similarly acceptable to invoke `Dispose` and then `DisposeAsync` or vice versa, but only the first should be meaningful and subsequent invocations of either should be a nop.</span></span>  <span data-ttu-id="f90df-121">W związku z tym, jeśli typ implementuje obydwie, zachęcamy klientów do wywołania jednokrotnie i tylko wtedy, gdy jest to bardziej odpowiednia metoda oparta na kontekście, `Dispose` w kontekstach synchronicznych i `DisposeAsync` asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="f90df-121">As such, if a type does implement both, consumers are encouraged to call once and only once the more relevant method based on the context, `Dispose` in synchronous contexts and `DisposeAsync` in asynchronous ones.</span></span>

<span data-ttu-id="f90df-122">(Zostawiam, w jaki sposób `IAsyncDisposable` współdziała z `using` do oddzielnej dyskusji.</span><span class="sxs-lookup"><span data-stu-id="f90df-122">(I'm leaving discussion of how `IAsyncDisposable` interacts with `using` to a separate discussion.</span></span>  <span data-ttu-id="f90df-123">I pokrycie sposobu, w jaki współdziała z `foreach`, jest obsługiwane w dalszej części tej propozycji.</span><span class="sxs-lookup"><span data-stu-id="f90df-123">And coverage of how it interacts with `foreach` is handled later in this proposal.)</span></span>

<span data-ttu-id="f90df-124">Rozważane alternatywy:</span><span class="sxs-lookup"><span data-stu-id="f90df-124">Alternatives considered:</span></span>
- <span data-ttu-id="f90df-125">_`DisposeAsync` akceptujący `CancellationToken`_ : w teorii warto mieć pewność, że wszystkie operacje asynchroniczne mogą być anulowane, a ich usuwanie polega na usunięciu, zamknięciu, free'ing zasobów itd. Czyszczenie jest nadal ważne w przypadku pracy, która została anulowana.</span><span class="sxs-lookup"><span data-stu-id="f90df-125">_`DisposeAsync` accepting a `CancellationToken`_: while in theory it makes sense that anything async can be canceled, disposal is about cleanup, closing things out, free'ing resources, etc., which is generally not something that should be canceled; cleanup is still important for work that's canceled.</span></span>  <span data-ttu-id="f90df-126">Te same `CancellationToken`, które spowodowały, że rzeczywista ilość pracy jest zwykle taka sama, że ten sam token przeszedł do `DisposeAsync`, dzięki czemu `DisposeAsync` bezwartościowe, ponieważ anulowanie pracy mogłoby spowodować, że `DisposeAsync` być NOP.</span><span class="sxs-lookup"><span data-stu-id="f90df-126">The same `CancellationToken` that caused the actual work to be canceled would typically be the same token passed to `DisposeAsync`, making `DisposeAsync` worthless because cancellation of the work would cause `DisposeAsync` to be a nop.</span></span>  <span data-ttu-id="f90df-127">Jeśli ktoś chce uniknąć zablokowania oczekiwania na usunięcie, może uniknąć oczekiwania na wynikowe `ValueTask`lub oczekiwać na niego tylko przez pewien czas.</span><span class="sxs-lookup"><span data-stu-id="f90df-127">If someone wants to avoid being blocked waiting for disposal, they can avoid waiting on the resulting `ValueTask`, or wait on it only for some period of time.</span></span>
- <span data-ttu-id="f90df-128">_`DisposeAsync` zwrócić `Task`_ : teraz, że nieogólny `ValueTask` istnieje i może być skonstruowany z `IValueTaskSource`, zwrócenie `ValueTask` z `DisposeAsync` umożliwia ponowne użycie istniejącego obiektu jako obietnicy reprezentującej ostateczne zakończenie procesu `DisposeAsync`, w przypadku których `Task` zostanie ukończona asynchronicznie.`DisposeAsync`</span><span class="sxs-lookup"><span data-stu-id="f90df-128">_`DisposeAsync` returning a `Task`_: Now that a non-generic `ValueTask` exists and can be constructed from an `IValueTaskSource`, returning `ValueTask` from `DisposeAsync` allows an existing object to be reused as the promise representing the eventual async completion of `DisposeAsync`, saving a `Task` allocation in the case where `DisposeAsync` completes asynchronously.</span></span>
- <span data-ttu-id="f90df-129">_Konfigurowanie `DisposeAsync` przy użyciu `bool continueOnCapturedContext` (`ConfigureAwait`)_ : podczas gdy mogą wystąpić problemy związane z tym, w jaki sposób takie koncepcje są ujawniane w `using`, `foreach`i innych konstrukcjach językowych, które go wykorzystują, z perspektywy interfejsu, nie wykonuje żadnych operacji w `await`"i nie ma niczego do skonfigurowania... odbiorcy `ValueTask` mogą korzystać z niego.</span><span class="sxs-lookup"><span data-stu-id="f90df-129">_Configuring `DisposeAsync` with a `bool continueOnCapturedContext` (`ConfigureAwait`)_: While there may be issues related to how such a concept is exposed to `using`, `foreach`, and other language constructs that consume this, from an interface perspective it's not actually doing any `await`'ing and there's nothing to configure... consumers of the `ValueTask` can consume it however they wish.</span></span>
- <span data-ttu-id="f90df-130">_`IAsyncDisposable` dziedziczyć `IDisposable`_ : ponieważ należy użyć tylko jednego lub drugiego, nie ma sensu, aby wymusić implementację typów.</span><span class="sxs-lookup"><span data-stu-id="f90df-130">_`IAsyncDisposable` inheriting `IDisposable`_:  Since only one or the other should be used, it doesn't make sense to force types to implement both.</span></span>
- <span data-ttu-id="f90df-131">_`IDisposableAsync`, a nie `IAsyncDisposable`_ : nastąpiło nadanie nazw, których elementy/typy są "asynchroniczne coś", podczas gdy operacje są "gotowe asynchroniczne", więc typy mają wartość "Async" jako prefiks, a metody mają wartość "Async" jako sufiks.</span><span class="sxs-lookup"><span data-stu-id="f90df-131">_`IDisposableAsync` instead of `IAsyncDisposable`_: We've been following the naming that things/types are an "async something" whereas operations are "done async", so types have "Async" as a prefix and methods have "Async" as a suffix.</span></span>

### <a name="iasyncenumerable--iasyncenumerator"></a><span data-ttu-id="f90df-132">IAsyncEnumerable / IAsyncEnumerator</span><span class="sxs-lookup"><span data-stu-id="f90df-132">IAsyncEnumerable / IAsyncEnumerator</span></span>

<span data-ttu-id="f90df-133">Do podstawowych bibliotek platformy .NET dodawane są dwa interfejsy:</span><span class="sxs-lookup"><span data-stu-id="f90df-133">Two interfaces are added to the core .NET libraries:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> MoveNextAsync();
        T Current { get; }
    }
}
```

<span data-ttu-id="f90df-134">Typowe użycie (bez dodatkowych funkcji języka) będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="f90df-134">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
        Use(enumerator.Current);
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="f90df-135">Odrzucone opcje:</span><span class="sxs-lookup"><span data-stu-id="f90df-135">Discarded options considered:</span></span>
- <span data-ttu-id="f90df-136">_`Task<bool> MoveNextAsync(); T current { get; }`_ : użycie `Task<bool>` może obsługiwać używanie buforowanego obiektu zadania do reprezentowania synchronicznych, udanych wywołań `MoveNextAsync`, ale alokacja będzie nadal wymagana na potrzeby asynchronicznego uzupełniania.</span><span class="sxs-lookup"><span data-stu-id="f90df-136">_`Task<bool> MoveNextAsync(); T current { get; }`_: Using `Task<bool>` would support using a cached task object to represent synchronous, successful `MoveNextAsync` calls, but an allocation would still be required for asynchronous completion.</span></span>  <span data-ttu-id="f90df-137">Zwracając `ValueTask<bool>`, włączamy obiekt modułu wyliczającego do samego implementującego `IValueTaskSource<bool>` i służy jako kopia zapasowa `ValueTask<bool>` zwrócone z `MoveNextAsync`, co z kolei pozwala na znacznie zmniejszone przedziały.</span><span class="sxs-lookup"><span data-stu-id="f90df-137">By returning `ValueTask<bool>`, we enable the enumerator object to itself implement `IValueTaskSource<bool>` and be used as the backing for the `ValueTask<bool>` returned from `MoveNextAsync`, which in turn allows for significantly reduced overheads.</span></span>
- <span data-ttu-id="f90df-138">_`ValueTask<(bool, T)> MoveNextAsync();`_ : nie tylko trudniejsze do użycia, ale oznacza to, że `T` nie mogą być już współwariantowe.</span><span class="sxs-lookup"><span data-stu-id="f90df-138">_`ValueTask<(bool, T)> MoveNextAsync();`_: It's not only harder to consume, but it means that `T` can no longer be covariant.</span></span>
- <span data-ttu-id="f90df-139">_`ValueTask<T?> TryMoveNextAsync();`_ : nie jest współwariantem.</span><span class="sxs-lookup"><span data-stu-id="f90df-139">_`ValueTask<T?> TryMoveNextAsync();`_: Not covariant.</span></span>
- <span data-ttu-id="f90df-140">_`Task<T?> TryMoveNextAsync();`_ : nie należy do współdzielenia, alokacje dla każdego wywołania itd.</span><span class="sxs-lookup"><span data-stu-id="f90df-140">_`Task<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="f90df-141">_`ITask<T?> TryMoveNextAsync();`_ : nie należy do współdzielenia, alokacje dla każdego wywołania itd.</span><span class="sxs-lookup"><span data-stu-id="f90df-141">_`ITask<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="f90df-142">_`ITask<(bool,T)> TryMoveNextAsync();`_ : nie należy do współdzielenia, alokacje dla każdego wywołania itd.</span><span class="sxs-lookup"><span data-stu-id="f90df-142">_`ITask<(bool,T)> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="f90df-143">_`Task<bool> TryMoveNextAsync(out T result);`_ : należy ustawić wynik `out`, gdy operacja zwróci synchronicznie, a nie gdy asynchronicznie ukończy zadanie potencjalnie kiedyś w przyszłości, w którym nie ma możliwości przekazania wyniku.</span><span class="sxs-lookup"><span data-stu-id="f90df-143">_`Task<bool> TryMoveNextAsync(out T result);`_: The `out` result would need to be set when the operation returns synchronously, not when it asynchronously completes the task potentially sometime long in the future, at which point there'd be no way to communicate the result.</span></span>
- <span data-ttu-id="f90df-144">_nie`IAsyncEnumerator<T>` implementowania `IAsyncDisposable`_ : możemy oddzielić te opcje.</span><span class="sxs-lookup"><span data-stu-id="f90df-144">_`IAsyncEnumerator<T>` not implementing `IAsyncDisposable`_: We could choose to separate these.</span></span>  <span data-ttu-id="f90df-145">Jednak wykonanie tej operacji komplikuje niektóre inne obszary wniosku, ponieważ kod musi być w stanie zaradzić sobie z możliwością, że moduł wyliczający nie zapewnia usuwania, co utrudnia pisanie pomocników opartych na wzorcach.</span><span class="sxs-lookup"><span data-stu-id="f90df-145">However, doing so complicates certain other areas of the proposal, as code must then be able to deal with the possibility that an enumerator doesn't provide disposal, which makes it difficult to write pattern-based helpers.</span></span>  <span data-ttu-id="f90df-146">Ponadto będzie to typowy w przypadku modułów wyliczających potrzebnych do usuwania (np. wszelkich C# iteratorów asynchronicznych, które mają blok finally, większość elementów wyliczających dane z połączenia sieciowego itp.), a jeśli takie nie, można łatwo zaimplementować metodę w sposób czysty jako `public ValueTask DisposeAsync() => default(ValueTask);` z minimalnym dodatkowym obciążeniem.</span><span class="sxs-lookup"><span data-stu-id="f90df-146">Further, it will be common for enumerators to have a need for disposal (e.g. any C# async iterator that has a finally block, most things enumerating data from a network connection, etc.), and if one doesn't, it is simple to implement the method purely as `public ValueTask DisposeAsync() => default(ValueTask);` with minimal additional overhead.</span></span>
- <span data-ttu-id="f90df-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: brak parametru tokenu anulowania.</span><span class="sxs-lookup"><span data-stu-id="f90df-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: No cancellation token parameter.</span></span>

#### <a name="viable-alternative"></a><span data-ttu-id="f90df-148">Żywotna alternatywa:</span><span class="sxs-lookup"><span data-stu-id="f90df-148">Viable alternative:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator();
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> WaitForNextAsync();
        T TryGetNext(out bool success);
    }
}
```

<span data-ttu-id="f90df-149">`TryGetNext` jest używana w pętli wewnętrznej do korzystania z elementów z pojedynczym wywołaniem interfejsu, o ile są one dostępne synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="f90df-149">`TryGetNext` is used in an inner loop to consume items with a single interface call as long as they're available synchronously.</span></span>  <span data-ttu-id="f90df-150">Gdy następny element nie może zostać pobrany synchronicznie, zwraca wartość false, a w każdym momencie zwraca wartość false, wywołujący musi następnie wywołać `WaitForNextAsync`, aby poczekać na udostępnienie następnego elementu lub określić, że element nie będzie inny.</span><span class="sxs-lookup"><span data-stu-id="f90df-150">When the next item can't be retrieved synchronously, it returns false, and any time it returns false, a caller must subsequently invoke `WaitForNextAsync` to either wait for the next item to be available or to determine that there will never be another item.</span></span> <span data-ttu-id="f90df-151">Typowe użycie (bez dodatkowych funkcji języka) będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="f90df-151">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.WaitForNextAsync())
    {
        while (true)
        {
            int item = enumerator.TryGetNext(out bool success);
            if (!success) break;
            Use(item);
        }
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="f90df-152">Zaletą tego jest dwukrotne, jednopomocnicze i jeden główny:</span><span class="sxs-lookup"><span data-stu-id="f90df-152">The advantage of this is two-fold, one minor and one major:</span></span>
- <span data-ttu-id="f90df-153">_Pomocniczy: umożliwia modułowi wyliczającemu obsługę wielu odbiorców_.</span><span class="sxs-lookup"><span data-stu-id="f90df-153">_Minor: Allows for an enumerator to support multiple consumers_.</span></span> <span data-ttu-id="f90df-154">Mogą istnieć scenariusze, w których jest cenny dla modułu wyliczającego, aby obsługiwał wielu klientów jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="f90df-154">There may be scenarios where it's valuable for an enumerator to support multiple concurrent consumers.</span></span>  <span data-ttu-id="f90df-155">Nie można osiągnąć, gdy `MoveNextAsync` i `Current` są oddzielone w taki sposób, że implementacja nie może spowodować ich użycia jako niepodzielne.</span><span class="sxs-lookup"><span data-stu-id="f90df-155">That can't be achieved when `MoveNextAsync` and `Current` are separate such that an implementation can't make their usage atomic.</span></span>  <span data-ttu-id="f90df-156">Z kolei takie podejście zapewnia jedną metodę `TryGetNext`, która obsługuje wypychanie modułu wyliczającego do przodu i pobieranie następnego elementu, więc moduł wyliczający może w razie potrzeby włączyć niepodzielność.</span><span class="sxs-lookup"><span data-stu-id="f90df-156">In contrast, this approach provides a single method `TryGetNext` that supports pushing the enumerator forward and getting the next item, so the enumerator can enable atomicity if desired.</span></span>  <span data-ttu-id="f90df-157">Jednak jest to możliwe, że takie scenariusze można także włączyć, dając każdy konsumentowi swój moduł wyliczający z udostępnionego wyliczalnego.</span><span class="sxs-lookup"><span data-stu-id="f90df-157">However, it's likely that such scenarios could also be enabled by giving each consumer its own enumerator from a shared enumerable.</span></span>  <span data-ttu-id="f90df-158">Ponadto nie chcemy wymusić, aby każdy moduł wyliczający obsługiwał współbieżne użycie, ponieważ spowodowałoby to dodanie nieuproszczonych nadmiarów do większości przypadków, które nie wymagają tego, co oznacza, że konsument interfejsu zazwyczaj nie może polegać na tym, co się dzieje.</span><span class="sxs-lookup"><span data-stu-id="f90df-158">Further, we don't want to enforce that every enumerator support concurrent usage, as that would add non-trivial overheads to the majority case that doesn't require it, which means a consumer of the interface generally couldn't rely on this any way.</span></span>
- <span data-ttu-id="f90df-159">_Główna: wydajność_.</span><span class="sxs-lookup"><span data-stu-id="f90df-159">_Major: Performance_.</span></span> <span data-ttu-id="f90df-160">Podejście `MoveNextAsync`/`Current` wymaga dwóch wywołań interfejsu dla każdej operacji, podczas gdy najlepszym przypadkiem `WaitForNextAsync`/`TryGetNext` jest to, że większość iteracji kończy się synchronicznie, włączając w to ścisłą wewnętrzną pętlę z `TryGetNext`, tak aby tylko jedno wywołanie interfejsu dla operacji.</span><span class="sxs-lookup"><span data-stu-id="f90df-160">The `MoveNextAsync`/`Current` approach requires two interface calls per operation, whereas the best case for `WaitForNextAsync`/`TryGetNext` is that most iterations complete synchronously, enabling a tight inner loop with `TryGetNext`, such that we only have one interface call per operation.</span></span>  <span data-ttu-id="f90df-161">Może to mieć wymierny wpływ na sytuacje, w których wywołania interfejsu dominują obliczenia.</span><span class="sxs-lookup"><span data-stu-id="f90df-161">This can have a measurable impact in situations where the interface calls dominate the computation.</span></span>

<span data-ttu-id="f90df-162">Jednak istnieją nieuproszczone downsides, w tym znacząco zwiększona złożoność podczas ręcznego korzystania z nich, i zwiększona szansa na wprowadzenie usterek podczas ich używania.</span><span class="sxs-lookup"><span data-stu-id="f90df-162">However, there are non-trivial downsides, including significantly increased complexity when consuming these manually, and an increased chance of introducing bugs when using them.</span></span>  <span data-ttu-id="f90df-163">Mimo że korzyści z wydajności są wyświetlane w mikrotestach, nie uważamy, że wpłyną one na ogromną większość rzeczywistych zastosowań.</span><span class="sxs-lookup"><span data-stu-id="f90df-163">And while the performance benefits show up in microbenchmarks, we don't believe they'll be impactful in the vast majority of real usage.</span></span>  <span data-ttu-id="f90df-164">Jeśli wyłączy to, możemy wprowadzić drugi zestaw interfejsów w sposób jasny.</span><span class="sxs-lookup"><span data-stu-id="f90df-164">If it turns out they are, we can introduce a second set of interfaces in a light-up fashion.</span></span>

<span data-ttu-id="f90df-165">Odrzucone opcje:</span><span class="sxs-lookup"><span data-stu-id="f90df-165">Discarded options considered:</span></span>
- <span data-ttu-id="f90df-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: parametry `out` nie mogą być współwariantowe.</span><span class="sxs-lookup"><span data-stu-id="f90df-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` parameters can't be covariant.</span></span>  <span data-ttu-id="f90df-167">W tym miejscu znajduje się również niewielki wpływ (ogólny problem z wzorcem try), który prawdopodobnie powoduje wystąpienie bariery zapisu w środowisku uruchomieniowym dla wyników typu referencyjnego.</span><span class="sxs-lookup"><span data-stu-id="f90df-167">There's also a small impact here (an issue with the try pattern in general) that this likely incurs a runtime write barrier for reference type results.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="f90df-168">Anulowanie</span><span class="sxs-lookup"><span data-stu-id="f90df-168">Cancellation</span></span>

<span data-ttu-id="f90df-169">Istnieje kilka możliwych podejścia do obsługi anulowania:</span><span class="sxs-lookup"><span data-stu-id="f90df-169">There are several possible approaches to supporting cancellation:</span></span>
1. <span data-ttu-id="f90df-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` są anulowane — niezależny od: `CancellationToken` nie pojawia się w dowolnym miejscu.</span><span class="sxs-lookup"><span data-stu-id="f90df-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` are cancellation-agnostic: `CancellationToken` doesn't appear anywhere.</span></span>  <span data-ttu-id="f90df-171">Anulowanie jest realizowane przez logicznie pieczenie `CancellationToken` do wyliczalnego i/lub modułu wyliczającego w jakikolwiek sposób, na przykład w przypadku wywołania iteratora, przekazanie `CancellationToken` jako argumentu do metody iteratora i użycie go w treści iteratora, tak jak w przypadku każdego innego parametru.</span><span class="sxs-lookup"><span data-stu-id="f90df-171">Cancellation is achieved by logically baking the `CancellationToken` into the enumerable and/or enumerator in whatever manner is appropriate, e.g. when calling an iterator, passing the `CancellationToken` as an argument to the iterator method and using it in the body of the iterator, as is done with any other parameter.</span></span>
2. <span data-ttu-id="f90df-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: przekazanie `CancellationToken` do `GetAsyncEnumerator`, a kolejne operacje `MoveNextAsync` będą jednakowe.</span><span class="sxs-lookup"><span data-stu-id="f90df-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: You pass a `CancellationToken` to `GetAsyncEnumerator`, and subsequent `MoveNextAsync` operations respect it however it can.</span></span>
3. <span data-ttu-id="f90df-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: przekazanie `CancellationToken` do poszczególnych wywołań `MoveNextAsync`.</span><span class="sxs-lookup"><span data-stu-id="f90df-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: You pass a `CancellationToken` to each individual `MoveNextAsync` call.</span></span>
4. <span data-ttu-id="f90df-174">1 & & 2: Osadź `CancellationToken`s w wyliczalnym/wyliczającym i przekaż `CancellationToken`s do `GetAsyncEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="f90df-174">1 && 2: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `GetAsyncEnumerator`.</span></span>
5. <span data-ttu-id="f90df-175">1 & & 3: Osadź `CancellationToken`s w wyliczalnym/wyliczającym i przekaż `CancellationToken`s do `MoveNextAsync`.</span><span class="sxs-lookup"><span data-stu-id="f90df-175">1 && 3: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `MoveNextAsync`.</span></span>

<span data-ttu-id="f90df-176">Z perspektywy czysto teoretycznej (5) jest najbardziej niezawodna, w tym (a) `MoveNextAsync` akceptujący `CancellationToken` włącza największą kontrolę nad tym, co zostało anulowane, a (b) `CancellationToken` jest tylko dowolnym innym typem, który może przekazywać jako argument do iteratorów, osadzony w dowolnym typie itd.</span><span class="sxs-lookup"><span data-stu-id="f90df-176">From a purely theoretical perspective, (5) is the most robust, in that (a) `MoveNextAsync` accepting a `CancellationToken` enables the most fine-grained control over what's canceled, and (b) `CancellationToken` is just any other type that can passed as an argument into iterators, embedded in arbitrary types, etc.</span></span>

<span data-ttu-id="f90df-177">Istnieje jednak wiele problemów z tym podejściem:</span><span class="sxs-lookup"><span data-stu-id="f90df-177">However, there are multiple problems with that approach:</span></span>
- <span data-ttu-id="f90df-178">Jak `CancellationToken` przeszedł do `GetAsyncEnumerator` umieścić ją w treści iteratora?</span><span class="sxs-lookup"><span data-stu-id="f90df-178">How does a `CancellationToken` passed to `GetAsyncEnumerator` make it into the body of the iterator?</span></span>  <span data-ttu-id="f90df-179">Możemy uwidocznić nowe słowo kluczowe `iterator`, aby uzyskać dostęp do `CancellationToken` przekazanego do `GetEnumerator`, jednak a) to wiele dodatkowych maszyn, b) czynimy to bardzo pierwszej klasy obywatelem, a c) przypadek 99% wydaje się być tym samym kodem, który wywołuje iterator i wywołując `GetAsyncEnumerator` w tym przypadku, w takim przypadku można tylko przekazać `CancellationToken` jako argument do metody.</span><span class="sxs-lookup"><span data-stu-id="f90df-179">We could expose a new `iterator` keyword that you could dot off of to get access to the `CancellationToken` passed to `GetEnumerator`, but a) that's a lot of additional machinery, b) we're making it a very first-class citizen, and c) the 99% case would seem to be the same code both calling an iterator and calling `GetAsyncEnumerator` on it, in which case it can just pass the `CancellationToken` as an argument into the method.</span></span>
- <span data-ttu-id="f90df-180">Jak `CancellationToken` przeszedł do `MoveNextAsync` do treści metody?</span><span class="sxs-lookup"><span data-stu-id="f90df-180">How does a `CancellationToken` passed to `MoveNextAsync` get into the body of the method?</span></span>  <span data-ttu-id="f90df-181">Jest to nawet gorsze, tak jakby był on narażony na `iterator` obiektu lokalnego, jego wartość może ulec zmianie między czekami, co oznacza, że każdy kod zarejestrowany w tokenie musi wyrejestrować się z niego przed oczekiwaniami, a następnie ponownie zarejestrować po; jest również możliwe, że konieczna jest taka rejestracja i Wyrejestrowanie w każdym `MoveNextAsync` wywołaniu, niezależnie od tego, czy zaimplementowane przez kompilator w iteratorze, czy przez dewelopera ręcznie.</span><span class="sxs-lookup"><span data-stu-id="f90df-181">This is even worse, as if it's exposed off of an `iterator` local object, its value could change across awaits, which means any code that registered with the token would need to unregister from it prior to awaits and then re-register after; it's also potentially quite expensive to need to do such registering and unregistering in every `MoveNextAsync` call, regardless of whether implemented by the compiler in an iterator or by a developer manually.</span></span>
- <span data-ttu-id="f90df-182">W jaki sposób deweloper anulował `foreach` pętlę?</span><span class="sxs-lookup"><span data-stu-id="f90df-182">How does a developer cancel a `foreach` loop?</span></span>  <span data-ttu-id="f90df-183">Jeśli jest to możliwe poprzez nadanie `CancellationToken` wyliczalnemu/wyliczającemu, wówczas musimy obsługiwać `foreach`"w przypadku modułów wyliczających, dzięki czemu są one przeznaczone dla obywateli pierwszej klasy, a teraz trzeba zacząć myśleć o ekosystemie skompilowanej wokół modułów wyliczających (np. metody LINQ) lub b), dlatego musimy osadzić `CancellationToken` w wyliczalnym elemencie, który ma `WithCancellation` metodę rozszerzenia `IAsyncEnumerable<T>`, która będzie przechowywać podany token, a następnie przekazać ją do `GetAsyncEnumerator` opakowanego wyliczalnego, gdy `GetAsyncEnumerator` w zwróconej strukturze jest wywoływany (zignorowano ten token).</span><span class="sxs-lookup"><span data-stu-id="f90df-183">If it's done by giving a `CancellationToken` to an enumerable/enumerator, then either a) we need to support `foreach`'ing over enumerators, which raises them to being first-class citizens, and now you need to start thinking about an ecosystem built up around enumerators (e.g. LINQ methods) or b) we need to embed the `CancellationToken` in the enumerable anyway by having some `WithCancellation` extension method off of `IAsyncEnumerable<T>` that would store the provided token and then pass it into  the wrapped enumerable's `GetAsyncEnumerator` when the `GetAsyncEnumerator` on the returned struct is invoked (ignoring that token).</span></span>  <span data-ttu-id="f90df-184">Można też użyć `CancellationToken` w treści instrukcji foreach.</span><span class="sxs-lookup"><span data-stu-id="f90df-184">Or, you can just use the `CancellationToken` you have in the body of the foreach.</span></span>
- <span data-ttu-id="f90df-185">Jeśli są obsługiwane zrozumienia zapytania, w jaki sposób `CancellationToken` dostarczana do `GetEnumerator` lub `MoveNextAsync` być przekazywany do każdej klauzuli?</span><span class="sxs-lookup"><span data-stu-id="f90df-185">If/when query comprehensions are supported, how would the `CancellationToken` supplied to `GetEnumerator` or `MoveNextAsync` be passed into each clause?</span></span>  <span data-ttu-id="f90df-186">Najprostszym sposobem, aby można było przechwycić tę klauzulę, w tym momencie każdy token jest przekazywać do `GetAsyncEnumerator`/`MoveNextAsync` jest ignorowany.</span><span class="sxs-lookup"><span data-stu-id="f90df-186">The easiest way would simply be for the clause to capture it, at which point whatever token is passed to `GetAsyncEnumerator`/`MoveNextAsync` is ignored.</span></span>

<span data-ttu-id="f90df-187">Zalecana jest wcześniejsza wersja tego dokumentu (1), ale od przełączenia do (4).</span><span class="sxs-lookup"><span data-stu-id="f90df-187">An earlier version of this document recommended (1), but we since switched to (4).</span></span>

<span data-ttu-id="f90df-188">Dwa główne problemy z programem (1):</span><span class="sxs-lookup"><span data-stu-id="f90df-188">The two main problems with (1):</span></span>
- <span data-ttu-id="f90df-189">producenci wyliczalnych elementów nie muszą implementować niektórych wzorców i mogą wykorzystać obsługę kompilatora dla iteratorów asynchronicznych, aby zaimplementować metodę `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)`.</span><span class="sxs-lookup"><span data-stu-id="f90df-189">producers of cancellable enumerables have to implement some boilerplate, and can only leverage the compiler's support for async-iterators to implement a `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` method.</span></span>
- <span data-ttu-id="f90df-190">prawdopodobnie wielu producentów byłoby skłonny do dodawania `CancellationToken` parametru do ich wyliczalnej asynchronicznie sygnatury, co uniemożliwi konsumentom przekazywanie tokenów anulowania, gdy mają one typ `IAsyncEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="f90df-190">it is likely that many producers would be tempted to just add a `CancellationToken` parameter to their async-enumerable signature instead, which will prevent consumers from passing the cancellation token they want when they are given an `IAsyncEnumerable` type.</span></span>

<span data-ttu-id="f90df-191">Istnieją dwa główne scenariusze użycia:</span><span class="sxs-lookup"><span data-stu-id="f90df-191">There are two main consumption scenarios:</span></span>
1. <span data-ttu-id="f90df-192">`await foreach (var i in GetData(token)) ...`, gdzie odbiorca wywołuje metodę Async-iterator,</span><span class="sxs-lookup"><span data-stu-id="f90df-192">`await foreach (var i in GetData(token)) ...` where the consumer calls the async-iterator method,</span></span>
2. <span data-ttu-id="f90df-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...`, gdzie odbiorca zajmuje się danym wystąpieniem `IAsyncEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="f90df-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` where the consumer deals with a given `IAsyncEnumerable` instance.</span></span>

<span data-ttu-id="f90df-194">Wiemy, że rozsądne kompromisy do obsługi obu scenariuszy w taki sposób, który jest wygodny dla producentów i konsumentów strumieni asynchronicznych, ma zastosowanie specjalnie do adnotacjgo parametru w metodzie iteratora Async.</span><span class="sxs-lookup"><span data-stu-id="f90df-194">We find that a reasonable compromise to support both scenarios in a way that is convenient for both producers and consumers of async-streams is to use a specially annotated parameter in the async-iterator method.</span></span> <span data-ttu-id="f90df-195">W tym celu jest używany atrybut `[EnumeratorCancellation]`.</span><span class="sxs-lookup"><span data-stu-id="f90df-195">The `[EnumeratorCancellation]` attribute is used for this purpose.</span></span> <span data-ttu-id="f90df-196">Umieszczenie tego atrybutu na parametrze informuje kompilator, że jeśli token jest przekazaniem do metody `GetAsyncEnumerator`, ten token powinien zostać użyty zamiast wartości pierwotnie przekazaną dla parametru.</span><span class="sxs-lookup"><span data-stu-id="f90df-196">Placing this attribute on a parameter tells the compiler that if a token is passed to the `GetAsyncEnumerator` method, that token should be used instead of the value originally passed for the parameter.</span></span>

<span data-ttu-id="f90df-197">Rozważ `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span><span class="sxs-lookup"><span data-stu-id="f90df-197">Consider `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span></span> <span data-ttu-id="f90df-198">Realizator tej metody może po prostu użyć parametru w treści metody.</span><span class="sxs-lookup"><span data-stu-id="f90df-198">The implementer of this method can simply use the parameter in the method body.</span></span> <span data-ttu-id="f90df-199">Konsument może korzystać z powyższych wzorców użycia:</span><span class="sxs-lookup"><span data-stu-id="f90df-199">The consumer can use either consumption patterns above:</span></span>
1. <span data-ttu-id="f90df-200">Jeśli używasz `GetData(token)`, token jest zapisywany w postaci wyliczalnej asynchronicznie i będzie używany w iteracji,</span><span class="sxs-lookup"><span data-stu-id="f90df-200">if you use `GetData(token)`, then the token is saved into the async-enumerable and will be used in iteration,</span></span>
2. <span data-ttu-id="f90df-201">Jeśli używasz `givenIAsyncEnumerable.WithCancellation(token)`, token przeszedł do `GetAsyncEnumerator` zastąpi dowolny token zapisany w wyliczalnym asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="f90df-201">if you use `givenIAsyncEnumerable.WithCancellation(token)`, then the token passed to `GetAsyncEnumerator` will supersede any token saved in the async-enumerable.</span></span>

## <a name="foreach"></a><span data-ttu-id="f90df-202">foreach</span><span class="sxs-lookup"><span data-stu-id="f90df-202">foreach</span></span>

<span data-ttu-id="f90df-203">`foreach` zostanie rozszerzony do obsługi `IAsyncEnumerable<T>` oprócz istniejącej obsługi `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="f90df-203">`foreach` will be augmented to support `IAsyncEnumerable<T>` in addition to its existing support for `IEnumerable<T>`.</span></span>  <span data-ttu-id="f90df-204">I będzie obsługiwał odpowiednik `IAsyncEnumerable<T>` jako wzorca, jeśli odpowiednie elementy członkowskie są ujawnione publicznie, powracając do korzystania z interfejsu bezpośrednio, w celu włączenia rozszerzeń opartych na strukturze, które nie pozwalają na przydzielanie, a także używania alternatywnej awaitables jako typu zwracanego `MoveNextAsync` i `DisposeAsync`.</span><span class="sxs-lookup"><span data-stu-id="f90df-204">And it will support the equivalent of `IAsyncEnumerable<T>` as a pattern if the relevant members are exposed publicly, falling back to using the interface directly if not, in order to enable struct-based extensions that avoid allocating as well as using alternative awaitables as the return type of `MoveNextAsync` and `DisposeAsync`.</span></span>

### <a name="syntax"></a><span data-ttu-id="f90df-205">Składnia</span><span class="sxs-lookup"><span data-stu-id="f90df-205">Syntax</span></span>

<span data-ttu-id="f90df-206">Za pomocą składni:</span><span class="sxs-lookup"><span data-stu-id="f90df-206">Using the syntax:</span></span>

```csharp
foreach (var i in enumerable)
```

<span data-ttu-id="f90df-207">C#Program będzie kontynuował traktowanie `enumerable` jako synchronicznego wyliczalnego, tak że nawet jeśli udostępni odpowiednie interfejsy API dla wyliczalnych asynchronicznie (uwidaczniając wzorzec lub implementując interfejs), będzie uwzględniać tylko synchroniczne interfejsy API.</span><span class="sxs-lookup"><span data-stu-id="f90df-207">C# will continue to treat `enumerable` as a synchronous enumerable, such that even if it exposes the relevant APIs for async enumerables (exposing the pattern or implementing the interface), it will only consider the synchronous APIs.</span></span>

<span data-ttu-id="f90df-208">Aby wymusić `foreach` w zamian tylko asynchronicznych interfejsów API, `await` zostanie wstawiony w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f90df-208">To force `foreach` to instead only consider the asynchronous APIs, `await` is inserted as follows:</span></span>

```csharp
await foreach (var i in enumerable)
```

<span data-ttu-id="f90df-209">Nie zostanie dostarczona żadna składnia, która mogłaby obsługiwać korzystanie z interfejsów API Async lub Sync; Deweloper musi wybrać na podstawie używanej składni.</span><span class="sxs-lookup"><span data-stu-id="f90df-209">No syntax would be provided that would support using either the async or the sync APIs; the developer must choose based on the syntax used.</span></span>

<span data-ttu-id="f90df-210">Odrzucone opcje:</span><span class="sxs-lookup"><span data-stu-id="f90df-210">Discarded options considered:</span></span>
- <span data-ttu-id="f90df-211">_`foreach (var i in await enumerable)`_ : Ta składnia jest już prawidłowa i zmiana jej znaczenia byłaby istotną zmianą.</span><span class="sxs-lookup"><span data-stu-id="f90df-211">_`foreach (var i in await enumerable)`_: This is already valid syntax, and changing its meaning would be a breaking change.</span></span>  <span data-ttu-id="f90df-212">Oznacza to, że `await` `enumerable`, należy uzyskać synchronicznie ITER z niego, a następnie synchronicznie wykonać iterację w tym celu.</span><span class="sxs-lookup"><span data-stu-id="f90df-212">This means to `await` the `enumerable`, get back something synchronously iterable from it, and then synchronously iterate through that.</span></span>
- <span data-ttu-id="f90df-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)``foreach (await var i in enumerable)`_ : te wszystkie sugerują, że czekamy na następny element, ale istnieją inne oczekiwania w instrukcji foreach, w szczególności jeśli wyliczalny jest `IAsyncDisposable`, będziemy `await`"wypróbować jego asynchroniczne usuwanie.</span><span class="sxs-lookup"><span data-stu-id="f90df-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_: These all suggest that we're awaiting the next item, but there are other awaits involved in foreach, in particular if the enumerable is an `IAsyncDisposable`, we will be `await`'ing its async disposal.</span></span>  <span data-ttu-id="f90df-214">Oczekiwany jest zakres instrukcji foreach, a nie dla każdego pojedynczego elementu, a tym samym słowo kluczowe `await` zachowuje się na poziomie `foreach`.</span><span class="sxs-lookup"><span data-stu-id="f90df-214">That await is as the scope of the foreach rather than for each individual element, and thus the `await` keyword deserves to be at the `foreach` level.</span></span>  <span data-ttu-id="f90df-215">Ponadto, jeśli jest ona skojarzona z `foreach` umożliwia nam opisywanie `foreach` z innym terminem, np. "await foreach".</span><span class="sxs-lookup"><span data-stu-id="f90df-215">Further, having it associated with the `foreach` gives us a way to describe the `foreach` with a different term, e.g. a "await foreach".</span></span>  <span data-ttu-id="f90df-216">Jednak ważniejsze jest uwzględnienie `foreach` składni w tym samym czasie co składnia `using`, dzięki czemu są one spójne ze sobą, a `using (await ...)` jest już prawidłową składnią.</span><span class="sxs-lookup"><span data-stu-id="f90df-216">But more importantly, there's value in considering `foreach` syntax at the same time as `using` syntax, so that they remain consistent with each other, and `using (await ...)` is already valid syntax.</span></span>
- _`foreach await (var i in enumerable)`_

<span data-ttu-id="f90df-217">Nadal należy wziąć pod uwagę:</span><span class="sxs-lookup"><span data-stu-id="f90df-217">Still to consider:</span></span>
- <span data-ttu-id="f90df-218">`foreach` dzisiaj nie obsługuje iteracji przez moduł wyliczający.</span><span class="sxs-lookup"><span data-stu-id="f90df-218">`foreach` today does not support iterating through an enumerator.</span></span>  <span data-ttu-id="f90df-219">Oczekujemy, że firma Microsoft będzie bardziej powszechna `IAsyncEnumerator<T>`s, a tym samym zachęcamy do obsługi `await foreach` zarówno w `IAsyncEnumerable<T>`, jak i `IAsyncEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="f90df-219">We expect it will be more common to have `IAsyncEnumerator<T>`s handed around, and thus it's tempting to support `await foreach` with both `IAsyncEnumerable<T>` and `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="f90df-220">Jednak po dodaniu takiego wsparcia przedstawimy pytanie, czy `IAsyncEnumerator<T>` jest obywatelem pierwszej klasy i czy musi mieć przeciążenia kombinatorów, które działają na modułach wyliczających oprócz wyliczalnych?</span><span class="sxs-lookup"><span data-stu-id="f90df-220">But once we add such support, it introduces the question of whether `IAsyncEnumerator<T>` is a first-class citizen, and whether we need to have overloads of combinators that operate on enumerators in addition to enumerables?</span></span>    <span data-ttu-id="f90df-221">Czy chcemy zachęcić metody do zwracania modułów wyliczających zamiast wyliczalnych?</span><span class="sxs-lookup"><span data-stu-id="f90df-221">Do we want to encourage methods to return enumerators rather than enumerables?</span></span> <span data-ttu-id="f90df-222">Będziemy nadal omawiać ten temat.</span><span class="sxs-lookup"><span data-stu-id="f90df-222">We should continue to discuss this.</span></span>  <span data-ttu-id="f90df-223">Jeśli zdecydujesz, że nie chcesz jej obsłużyć, możemy wprowadzić metodę rozszerzenia `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);`, która umożliwia modułowi wyliczającemu dalszą `foreach`.</span><span class="sxs-lookup"><span data-stu-id="f90df-223">If we decide we don't want to support it, we might want to introduce an extension method `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` that would allow an enumerator to still be `foreach`'d.</span></span>  <span data-ttu-id="f90df-224">W przypadku podjęcia decyzji o tym, czy chcemy ją obsługiwać, musimy również zdecydować, czy `await foreach` będzie odpowiedzialny za wywoływanie `DisposeAsync` w module wyliczającym, a odpowiedź ma prawdopodobnie wartość "nie", kontrola nad usuwaniem powinna być obsługiwana przez osobę o nazwie `GetEnumerator`".</span><span class="sxs-lookup"><span data-stu-id="f90df-224">If we decide we do want to support it, we'll need to also decide on whether the `await foreach` would be responsible for calling `DisposeAsync` on the enumerator, and the answer is likely "no, control over disposal should be handled by whoever called `GetEnumerator`."</span></span>

### <a name="pattern-based-compilation"></a><span data-ttu-id="f90df-225">Kompilacja oparta na wzorcu</span><span class="sxs-lookup"><span data-stu-id="f90df-225">Pattern-based Compilation</span></span>

<span data-ttu-id="f90df-226">Kompilator powiąże się z interfejsami API opartymi na wzorcu, jeśli istnieją, a przede wszystkim używają interfejsu (wzorzec może być spełniony przy użyciu metod wystąpienia lub metod rozszerzenia).</span><span class="sxs-lookup"><span data-stu-id="f90df-226">The compiler will bind to the pattern-based APIs if they exist, preferring those over using the interface (the pattern may be satisfied with instance methods or extension methods).</span></span>  <span data-ttu-id="f90df-227">Wymagania dotyczące wzorca są następujące:</span><span class="sxs-lookup"><span data-stu-id="f90df-227">The requirements for the pattern are:</span></span>
- <span data-ttu-id="f90df-228">Wyliczalny element musi uwidaczniać metodę `GetAsyncEnumerator`, która może być wywołana bez argumentów i która zwraca moduł wyliczający, który spełnia odpowiedni wzorzec.</span><span class="sxs-lookup"><span data-stu-id="f90df-228">The enumerable must expose a `GetAsyncEnumerator` method that may be called with no arguments and that returns an enumerator that meets the relevant pattern.</span></span>
- <span data-ttu-id="f90df-229">Moduł wyliczający musi ujawniać `MoveNextAsync` metodę, która może być wywołana bez argumentów i która zwraca element, który może być `await`Ed i którego `GetResult()` zwraca `bool`.</span><span class="sxs-lookup"><span data-stu-id="f90df-229">The enumerator must expose a `MoveNextAsync` method that may be called with no arguments and that returns something which may be `await`ed and whose `GetResult()` returns a `bool`.</span></span>
- <span data-ttu-id="f90df-230">Moduł wyliczający musi także uwidaczniać Właściwość `Current`, której metoda pobierająca zwraca `T` reprezentujący rodzaj wyliczanych danych.</span><span class="sxs-lookup"><span data-stu-id="f90df-230">The enumerator must also expose `Current` property whose getter returns a `T` representing the kind of data being enumerated.</span></span>
- <span data-ttu-id="f90df-231">Moduł wyliczający może opcjonalnie uwidocznić metodę `DisposeAsync`, która może być wywoływana bez argumentów i która zwraca element, który może być `await`Ed, i którego `GetResult()` zwraca `void`.</span><span class="sxs-lookup"><span data-stu-id="f90df-231">The enumerator may optionally expose a `DisposeAsync` method that may be invoked with no arguments and that returns something that can be `await`ed and whose `GetResult()` returns `void`.</span></span>

<span data-ttu-id="f90df-232">Ten kod:</span><span class="sxs-lookup"><span data-stu-id="f90df-232">This code:</span></span>

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

<span data-ttu-id="f90df-233">jest tłumaczony na odpowiednik:</span><span class="sxs-lookup"><span data-stu-id="f90df-233">is translated to the equivalent of:</span></span>

```csharp
var enumerable = ...;
var enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
       T item = enumerator.Current;
       ...
    }
}
finally
{
    await enumerator.DisposeAsync(); // omitted, along with the try/finally, if the enumerator doesn't expose DisposeAsync
}
```

<span data-ttu-id="f90df-234">Jeśli typ iteracji nie ujawnia wzorca praw, zostaną użyte interfejsy.</span><span class="sxs-lookup"><span data-stu-id="f90df-234">If the iterated type doesn't expose the right pattern, the interfaces will be used.</span></span>

### <a name="configureawait"></a><span data-ttu-id="f90df-235">ConfigureAwait</span><span class="sxs-lookup"><span data-stu-id="f90df-235">ConfigureAwait</span></span>

<span data-ttu-id="f90df-236">Ta kompilacja oparta na wzorcu zezwala na używanie `ConfigureAwait` na wszystkich czekach, za pośrednictwem metody rozszerzenia `ConfigureAwait`:</span><span class="sxs-lookup"><span data-stu-id="f90df-236">This pattern-based compilation will allow `ConfigureAwait` to be used on all of the awaits, via a `ConfigureAwait` extension method:</span></span>

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

<span data-ttu-id="f90df-237">Będzie on oparty na typach, które dodamy do platformy .NET, prawdopodobnie z rozszerzeniem system. Threading. Tasks. Extensions. dll:</span><span class="sxs-lookup"><span data-stu-id="f90df-237">This will be based on types we'll add to .NET as well, likely to System.Threading.Tasks.Extensions.dll:</span></span>

```csharp
// Approximate implementation, omitting arg validation and the like
namespace System.Threading.Tasks
{
    public static class AsyncEnumerableExtensions
    {
        public static ConfiguredAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext) =>
            new ConfiguredAsyncEnumerable<T>(enumerable, continueOnCapturedContext);

        public struct ConfiguredAsyncEnumerable<T>
        {
            private readonly IAsyncEnumerable<T> _enumerable;
            private readonly bool _continueOnCapturedContext;

            internal ConfiguredAsyncEnumerable(IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext)
            {
                _enumerable = enumerable;
                _continueOnCapturedContext = continueOnCapturedContext;
            }

            public ConfiguredAsyncEnumerator<T> GetAsyncEnumerator() =>
                new ConfiguredAsyncEnumerator<T>(_enumerable.GetAsyncEnumerator(), _continueOnCapturedContext);

            public struct Enumerator
            {
                private readonly IAsyncEnumerator<T> _enumerator;
                private readonly bool _continueOnCapturedContext;

                internal Enumerator(IAsyncEnumerator<T> enumerator, bool continueOnCapturedContext)
                {
                    _enumerator = enumerator;
                    _continueOnCapturedContext = continueOnCapturedContext;
                }

                public ConfiguredValueTaskAwaitable<bool> MoveNextAsync() =>
                    _enumerator.MoveNextAsync().ConfigureAwait(_continueOnCapturedContext);

                public T Current => _enumerator.Current;

                public ConfiguredValueTaskAwaitable DisposeAsync() =>
                    _enumerator.DisposeAsync().ConfigureAwait(_continueOnCapturedContext);
            }
        }
    }
}
```

<span data-ttu-id="f90df-238">Należy zauważyć, że takie podejście nie spowoduje włączenia `ConfigureAwait`, które mają być używane z wyliczalnymi opartymi na wzorcu, a następnie ponownie jest tak, że `ConfigureAwait` jest uwidoczniona tylko jako rozszerzenie na `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` i nie można go zastosować do arbitralnych oczekujących elementów, ponieważ ma to sens tylko w przypadku zastosowania do zadań (kontroluje zachowanie zaimplementowane w obsłudze kontynuacji zadania) i w ten sposób nie ma sensu w przypadku używania wzorca, w którym oczekujące operacje mogą nie być zadaniami.</span><span class="sxs-lookup"><span data-stu-id="f90df-238">Note that this approach will not enable `ConfigureAwait` to be used with pattern-based enumerables, but then again it's already the case that the `ConfigureAwait` is only exposed as an extension on `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` and can't be applied to arbitrary awaitable things, as it only makes sense when applied to Tasks (it controls a behavior implemented in Task's continuation support), and thus doesn't make sense when using a pattern where the awaitable things may not be tasks.</span></span>  <span data-ttu-id="f90df-239">Każda osoba zwracająca oczekiwane elementy mogą zapewnić własne zachowanie niestandardowe w takich zaawansowanych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="f90df-239">Anyone returning awaitable things can provide their own custom behavior in such advanced scenarios.</span></span>

<span data-ttu-id="f90df-240">(Jeśli firma Microsoft może dochodzić do obsługi rozwiązania `ConfigureAwait` na poziomie zakresu lub zestawu, wówczas nie będzie to konieczne).</span><span class="sxs-lookup"><span data-stu-id="f90df-240">(If we can come up with some way to support a scope- or assembly-level `ConfigureAwait` solution, then this won't be necessary.)</span></span>

## <a name="async-iterators"></a><span data-ttu-id="f90df-241">Iteratory asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="f90df-241">Async Iterators</span></span>

<span data-ttu-id="f90df-242">Język/kompilator będzie obsługiwał produkcję `IAsyncEnumerable<T>`s i `IAsyncEnumerator<T>`s oprócz ich używania.</span><span class="sxs-lookup"><span data-stu-id="f90df-242">The language / compiler will support producing `IAsyncEnumerable<T>`s and `IAsyncEnumerator<T>`s in addition to consuming them.</span></span> <span data-ttu-id="f90df-243">Obecnie język obsługuje pisanie iteratora, takiego jak:</span><span class="sxs-lookup"><span data-stu-id="f90df-243">Today the language supports writing an iterator like:</span></span>

```csharp
static IEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            Thread.Sleep(1000);
            yield return i;
        }
    }
    finally
    {
        Thread.Sleep(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="f90df-244">ale nie można używać `await` w treści iteratorów.</span><span class="sxs-lookup"><span data-stu-id="f90df-244">but `await` can't be used in the body of these iterators.</span></span>  <span data-ttu-id="f90df-245">Dodamy tę pomoc techniczną.</span><span class="sxs-lookup"><span data-stu-id="f90df-245">We will add that support.</span></span>

### <a name="syntax"></a><span data-ttu-id="f90df-246">Składnia</span><span class="sxs-lookup"><span data-stu-id="f90df-246">Syntax</span></span>

<span data-ttu-id="f90df-247">Istniejąca obsługa języka dla iteratorów wnioskuje charakter iteratora metody w zależności od tego, czy zawiera on dowolny `yield`s.</span><span class="sxs-lookup"><span data-stu-id="f90df-247">The existing language support for iterators infers the iterator nature of the method based on whether it contains any `yield`s.</span></span>  <span data-ttu-id="f90df-248">Ta sama wartość będzie prawdziwa dla iteratorów asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="f90df-248">The same will be true for async iterators.</span></span>  <span data-ttu-id="f90df-249">Takie Iteratory asynchroniczne zostaną rozbudowane i odróżniać od iteratorów synchronicznych poprzez dodanie `async` do sygnatury, a także musi `IAsyncEnumerable<T>` lub `IAsyncEnumerator<T>` jako typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="f90df-249">Such async iterators will be demarcated and differentiated from synchronous iterators via adding `async` to the signature, and must then also have either `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` as its return type.</span></span>  <span data-ttu-id="f90df-250">Na przykład powyższy przykład może być zapisany jako iterator asynchroniczny w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f90df-250">For example, the above example could be written as an async iterator as follows:</span></span>

```csharp
static async IAsyncEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            await Task.Delay(1000);
            yield return i;
        }
    }
    finally
    {
        await Task.Delay(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="f90df-251">Rozważane alternatywy:</span><span class="sxs-lookup"><span data-stu-id="f90df-251">Alternatives considered:</span></span>
- <span data-ttu-id="f90df-252">_Nie można używać `async` w podpisie_: użycie `async` jest prawdopodobnie technicznie wymagane przez kompilator, ponieważ używa go do określenia, czy `await` jest prawidłowy w tym kontekście.</span><span class="sxs-lookup"><span data-stu-id="f90df-252">_Not using `async` in the signature_: Using `async` is likely technically required by the compiler, as it uses it to determine whether `await` is valid in that context.</span></span>  <span data-ttu-id="f90df-253">Ale nawet jeśli nie jest to wymagane, firma Microsoft ustaliła, że `await` mogą być używane tylko w metodach oznaczonych jako `async`i dlatego ważne jest, aby zachować spójność.</span><span class="sxs-lookup"><span data-stu-id="f90df-253">But even if it's not required, we've established that `await` may only be used in methods marked as `async`, and it seems important to keep the consistency.</span></span>
- <span data-ttu-id="f90df-254">_Włączanie konstruktorów niestandardowych dla `IAsyncEnumerable<T>`_ : jest to coś, co może wyglądać na przyszłość, ale maszyna jest skomplikowana i nie obsługuje tego synchronicznych odpowiedników.</span><span class="sxs-lookup"><span data-stu-id="f90df-254">_Enabling custom builders for `IAsyncEnumerable<T>`_:  That's something we could look at for the future, but the machinery is complicated and we don't support that for the synchronous counterparts.</span></span>
- <span data-ttu-id="f90df-255">_Posiadanie `iterator`go słowa kluczowego w podpisie_: Iteratory asynchroniczne użyją `async iterator` w sygnaturze i `yield` mogą być używane tylko w metodach `async`, które zawierają `iterator`; `iterator` byłyby opcjonalne w iteratorach synchronicznych.</span><span class="sxs-lookup"><span data-stu-id="f90df-255">_Having an `iterator` keyword in the signature_: Async iterators would use `async iterator` in the signature, and `yield` could only be used in `async` methods that included `iterator`; `iterator` would then be made optional on synchronous iterators.</span></span>  <span data-ttu-id="f90df-256">W zależności od perspektywy ma to na celu bardzo jasne zapełnienie podpisu metody, czy `yield` jest dozwolony i czy metoda rzeczywiście zwraca wystąpienia typu `IAsyncEnumerable<T>`, a nie kompilator wytwarza jeden w zależności od tego, czy kod używa `yield` czy nie.</span><span class="sxs-lookup"><span data-stu-id="f90df-256">Depending on your perspective, this has the benefit of making it very clear by the signature of the method whether `yield` is allowed and whether the method is actually meant to return instances of type `IAsyncEnumerable<T>` rather than the compiler manufacturing one based on whether the code uses `yield` or not.</span></span>  <span data-ttu-id="f90df-257">Ale różni się od iteratorów synchronicznych, które nie są i nie można ich wymagać.</span><span class="sxs-lookup"><span data-stu-id="f90df-257">But it is different from synchronous iterators, which don't and can't be made to require one.</span></span>  <span data-ttu-id="f90df-258">A niektórzy deweloperzy nie przypominają dodatkowej składni.</span><span class="sxs-lookup"><span data-stu-id="f90df-258">Plus some developers don't like the extra syntax.</span></span>  <span data-ttu-id="f90df-259">Jeśli zaprojektowano go od podstaw, prawdopodobnie jest to wymagane, ale w tym momencie istnieje znacznie większa wartość, dzięki czemu Iteratory asynchroniczne są blisko iteratorów synchronizacji.</span><span class="sxs-lookup"><span data-stu-id="f90df-259">If we were designing it from scratch, we'd probably make this required, but at this point there's much more value in keeping async iterators close to sync iterators.</span></span>

## <a name="linq"></a><span data-ttu-id="f90df-260">LINQ</span><span class="sxs-lookup"><span data-stu-id="f90df-260">LINQ</span></span>

<span data-ttu-id="f90df-261">Istnieje ponad 200 przeciążeń metod w klasie `System.Linq.Enumerable`, wszystkie te, które działają w warunkach `IEnumerable<T>`; Niektóre z tych metod akceptują `IEnumerable<T>`, niektóre z nich generują `IEnumerable<T>`i wiele.</span><span class="sxs-lookup"><span data-stu-id="f90df-261">There are over ~200 overloads of methods on the `System.Linq.Enumerable` class, all of which work in terms of `IEnumerable<T>`; some of these accept `IEnumerable<T>`, some of them produce `IEnumerable<T>`, and many do both.</span></span>  <span data-ttu-id="f90df-262">Dodanie obsługi LINQ dla `IAsyncEnumerable<T>` prawdopodobnie wiąże się z duplikowaniem wszystkich tych przeciążeń, dla kolejnego ~ 200.</span><span class="sxs-lookup"><span data-stu-id="f90df-262">Adding LINQ support for `IAsyncEnumerable<T>` would likely entail duplicating all of these overloads for it, for another ~200.</span></span>  <span data-ttu-id="f90df-263">Ponieważ `IAsyncEnumerator<T>` najprawdopodobniej jest bardziej powszechny jako autonomiczna jednostka w świecie asynchronicznym, niż `IEnumerator<T>` w świecie synchronicznym, może być konieczne wykonanie innych przeciążeń ~ 200, które współpracują z `IAsyncEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="f90df-263">And since `IAsyncEnumerator<T>` is likely to be more common as a standalone entity in the asynchronous world than `IEnumerator<T>` is in the synchronous world, we could potentially need another ~200 overloads that work with `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="f90df-264">Ponadto duża liczba przeciążeń jest uwzględniana w predykatach (np. `Where`, które mają `Func<T, bool>`), i może być pożądane, aby `IAsyncEnumerable<T>`przeciążeń, które zajmują zarówno predykaty synchroniczne, jak i asynchroniczne (np. `Func<T, ValueTask<bool>>` oprócz `Func<T, bool>`).</span><span class="sxs-lookup"><span data-stu-id="f90df-264">Plus, a large number of the overloads deal with predicates (e.g. `Where` that takes a `Func<T, bool>`), and it may be desirable to have `IAsyncEnumerable<T>`-based overloads that deal with both synchronous and asynchronous predicates (e.g. `Func<T, ValueTask<bool>>` in addition to `Func<T, bool>`).</span></span>  <span data-ttu-id="f90df-265">Chociaż nie ma to zastosowania do wszystkich obecnie ~ 400 nowych przeciążeń, przybliżone obliczenie polega na tym, że ma to zastosowanie do połowy, co oznacza kolejną liczbę przeciążeń ~ 200, w sumie ~ 600 nowe metody.</span><span class="sxs-lookup"><span data-stu-id="f90df-265">While this isn't applicable to all of the now ~400 new overloads, a rough calculation is that it'd be applicable to half, which means another ~200 overloads, for a total of ~600 new methods.</span></span>

<span data-ttu-id="f90df-266">Jest to rozłożona liczba interfejsów API, z możliwością jeszcze więcej w przypadku, gdy biblioteki rozszerzeń, takie jak rozszerzenia interaktywne (IX) są brane pod uwagę.</span><span class="sxs-lookup"><span data-stu-id="f90df-266">That is a staggering number of APIs, with the potential for even more when extension libraries like Interactive Extensions (Ix) are considered.</span></span>  <span data-ttu-id="f90df-267">Jednak w przypadku systemu IX istnieje już implementacja wielu z nich i nie wydaje się, że jest to świetny powód, aby przeprowadzić jego duplikowanie. Zamiast tego należy pomóc społeczności ulepszyć IX i zarekomendować, gdy deweloperzy chcą używać LINQ z `IAsyncEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="f90df-267">But Ix already has an implementation of many of these, and there doesn't seem to be a great reason to duplicate that work; we should instead help the community improve Ix and recommend it for when developers want to use LINQ with `IAsyncEnumerable<T>`.</span></span>

<span data-ttu-id="f90df-268">Występuje również problem z składnią opisów zapytań.</span><span class="sxs-lookup"><span data-stu-id="f90df-268">There is also the issue of query comprehension syntax.</span></span>  <span data-ttu-id="f90df-269">Oparty na wzorcu charakter zapytania zezwala na "po prostu" z niektórymi operatorami, np. Jeśli IX zawiera następujące metody:</span><span class="sxs-lookup"><span data-stu-id="f90df-269">The pattern-based nature of query comprehensions would allow them to "just work" with some operators, e.g. if Ix provides the following methods:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

<span data-ttu-id="f90df-270">następnie ten C# kod będzie "po prostu":</span><span class="sxs-lookup"><span data-stu-id="f90df-270">then this C# code will "just work":</span></span>

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

<span data-ttu-id="f90df-271">Nie istnieje jednak żadna składnia dotycząca zrozumienia zapytania, która obsługuje używanie `await` w klauzulach, tak więc jeśli dodaliśmy do IX, na przykład:</span><span class="sxs-lookup"><span data-stu-id="f90df-271">However, there is no query comprehension syntax that supports using `await` in the clauses, so if Ix added, for example:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

<span data-ttu-id="f90df-272">spowoduje to "po prostu":</span><span class="sxs-lookup"><span data-stu-id="f90df-272">then this would "just work":</span></span>

```csharp
IAsyncEnumerable<string> result = from url in urls
                                  where item % 2 == 0
                                  select SomeAsyncMethod(item);

async ValueTask<int> SomeAsyncMethod(int item)
{
    await Task.Yield();
    return item * 2;
}
```

<span data-ttu-id="f90df-273">nie ma jednak możliwości zapisania go przy użyciu `await` wbudowanej w klauzuli `select`.</span><span class="sxs-lookup"><span data-stu-id="f90df-273">but there'd be no way to write it with the `await` inline in the `select` clause.</span></span>  <span data-ttu-id="f90df-274">W oddzielnym wysiłku możemy przystąpić do dodawania wyrażeń `async { ... }` do języka, w którym można zezwolić na używanie ich w przypadku zrozumienia zapytania, a powyższe można napisać jako:</span><span class="sxs-lookup"><span data-stu-id="f90df-274">As a separate effort, we could look into adding `async { ... }` expressions to the language, at which point we could allow them to be used in query comprehensions and the above could instead be written as:</span></span>

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

<span data-ttu-id="f90df-275">lub, aby włączyć `await`, które mają być używane bezpośrednio w wyrażeniach, na przykład przez obsługę `async from`.</span><span class="sxs-lookup"><span data-stu-id="f90df-275">or to enabling `await` to be used directly in expressions, such as by supporting `async from`.</span></span>  <span data-ttu-id="f90df-276">Jednak jest to mało prawdopodobne, aby projekt miał wpływ na pozostałą część zestawu funkcji w jeden sposób lub drugi. nie jest to szczególnie wysoka wartość, która jest obecnie w stanie inwestować w ten sposób, więc propozycja ma teraz nic więcej.</span><span class="sxs-lookup"><span data-stu-id="f90df-276">However, it's unlikely a design here would impact the rest of the feature set one way or the other, and this isn't a particularly high-value thing to invest in right now, so the proposal is to do nothing additional here right now.</span></span>

## <a name="integration-with-other-asynchronous-frameworks"></a><span data-ttu-id="f90df-277">Integracja z innymi platformami asynchronicznymi</span><span class="sxs-lookup"><span data-stu-id="f90df-277">Integration with other asynchronous frameworks</span></span>

<span data-ttu-id="f90df-278">Integracja z `IObservable<T>` i innymi platformami asynchronicznymi (np. strumienie reportowe) zostałaby przeprowadzona na poziomie biblioteki, a nie na poziomie języka.</span><span class="sxs-lookup"><span data-stu-id="f90df-278">Integration with `IObservable<T>` and other asynchronous frameworks (e.g. reactive streams) would be done at the library level rather than at the language level.</span></span>  <span data-ttu-id="f90df-279">Na przykład wszystkie dane z `IAsyncEnumerator<T>` mogą być publikowane na `IObserver<T>` po prostu przez `await foreach`"przekroczenia przez moduł wyliczający" i `OnNext`"przeprowadzenie danych do obserwatora, więc możliwe jest użycie metody rozszerzenia `AsObservable<T>`.</span><span class="sxs-lookup"><span data-stu-id="f90df-279">For example, all of the data from an `IAsyncEnumerator<T>` can be published to an `IObserver<T>` simply by `await foreach`'ing over the enumerator and `OnNext`'ing the data to the observer, so an `AsObservable<T>` extension method is possible.</span></span>  <span data-ttu-id="f90df-280">Zużywanie `IObservable<T>` w `await foreach` wymaga buforowania danych (w przypadku wypchnięcia innego elementu, gdy nadal trwa przetwarzanie poprzedniego elementu), ale taka karta może być łatwo zaimplementowana, aby umożliwić pobieranie `IObservable<T>` z `IAsyncEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="f90df-280">Consuming an `IObservable<T>` in a `await foreach` requires buffering the data (in case another item is pushed while the previous item is still being processing), but such a push-pull adapter can easily be implemented to enable an `IObservable<T>` to be pulled from with an `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="f90df-281">Itd.  RX/IX udostępnia już prototypy takich implementacji, a biblioteki, takie jak https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels zapewniają różne rodzaje buforowania struktur danych.</span><span class="sxs-lookup"><span data-stu-id="f90df-281">Etc.  Rx/Ix already provide prototypes of such implementations, and libraries like https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels provide various kinds of buffering data structures.</span></span>  <span data-ttu-id="f90df-282">Język nie musi być uwzględniony na tym etapie.</span><span class="sxs-lookup"><span data-stu-id="f90df-282">The language need not be involved at this stage.</span></span>
