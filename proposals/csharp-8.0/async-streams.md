---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485260"
---
# <a name="async-streams"></a>Strumienie asynchroniczne

* [x] proponowane
* [x] prototyp
* [] Implementacja
* [] — Specyfikacja

## <a name="summary"></a>Podsumowanie
[summary]: #summary

C#Program obsługuje metody iteratora i metody asynchroniczne, ale nie obsługuje metody, która jest zarówno iteratorem, jak i asynchronicznym.  Należy to skorygować przez umożliwienie `await` do użycia w nowej formie iteratora `async`, która zwraca `IAsyncEnumerable<T>` lub `IAsyncEnumerator<T>`, a nie `IEnumerable<T>` lub `IEnumerator<T>`, z `IAsyncEnumerable<T>` możliwego do użycia w nowym `await foreach`.  Interfejs `IAsyncDisposable` jest również używany do włączania czyszczenia asynchronicznego.

## <a name="related-discussion"></a>Pokrewne dyskusje
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

## <a name="interfaces"></a>Interfejsy

### <a name="iasyncdisposable"></a>IAsyncDisposable

Istnieje wiele dyskusji na temat `IAsyncDisposable` (np. https://github.com/dotnet/roslyn/issues/114) i tego, czy jest to dobre pomysł.  Jednak jest to wymagana koncepcja do dodania w ramach obsługi iteratorów asynchronicznych.  Ponieważ bloki `finally` mogą zawierać `await`s, a ponieważ bloki `finally` muszą być uruchamiane w ramach usuwania iteratorów, potrzebujemy asynchronicznego usuwania.  Jest to również szczególnie przydatne w przypadku, gdy czyszczenie zasobów może trwać pewien czas, np. zamykając pliki (wymagające opróżnień), wyrejestrować wywołania zwrotne i zapewnić sposób, aby wiedzieć, kiedy Wyrejestrowanie zostało zakończone itd.

Następujący interfejs jest dodawany do podstawowych bibliotek platformy .NET (np. System. private. CoreLib/system. Runtime):
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
Podobnie jak w przypadku `Dispose`, możliwe jest wywoływanie `DisposeAsync` wiele razy, a kolejne wywołania po pierwszej należy traktować jako nops, zwracając synchronicznie zakończone pomyślne zadanie (`DisposeAsync` nie musi być bezpieczny wątkowo, chociaż nie musi obsługiwać współbieżnego wywołania).  Ponadto typy mogą implementować zarówno `IDisposable`, jak i `IAsyncDisposable`, a jeśli tak, to w podobny sposób można wywołać `Dispose`, a następnie `DisposeAsync` lub na odwrót, ale tylko pierwszy powinien być znaczący, a kolejne wywołania metody muszą być NOP.  W związku z tym, jeśli typ implementuje obydwie, zachęcamy klientów do wywołania jednokrotnie i tylko wtedy, gdy jest to bardziej odpowiednia metoda oparta na kontekście, `Dispose` w kontekstach synchronicznych i `DisposeAsync` asynchronicznie.

(Zostawiam, w jaki sposób `IAsyncDisposable` współdziała z `using` do oddzielnej dyskusji.  I pokrycie sposobu, w jaki współdziała z `foreach`, jest obsługiwane w dalszej części tej propozycji.

Rozważane alternatywy:
- _`DisposeAsync` akceptujący `CancellationToken`_ : w teorii warto mieć pewność, że wszystkie operacje asynchroniczne mogą być anulowane, a ich usuwanie polega na usunięciu, zamknięciu, free'ing zasobów itd. Czyszczenie jest nadal ważne w przypadku pracy, która została anulowana.  Te same `CancellationToken`, które spowodowały, że rzeczywista ilość pracy jest zwykle taka sama, że ten sam token przeszedł do `DisposeAsync`, dzięki czemu `DisposeAsync` bezwartościowe, ponieważ anulowanie pracy mogłoby spowodować, że `DisposeAsync` być NOP.  Jeśli ktoś chce uniknąć zablokowania oczekiwania na usunięcie, może uniknąć oczekiwania na wynikowe `ValueTask`lub oczekiwać na niego tylko przez pewien czas.
- _`DisposeAsync` zwrócić `Task`_ : teraz, że nieogólny `ValueTask` istnieje i może być skonstruowany z `IValueTaskSource`, zwrócenie `ValueTask` z `DisposeAsync` umożliwia ponowne użycie istniejącego obiektu jako obietnicy reprezentującej ostateczne zakończenie procesu `DisposeAsync`, w przypadku których `Task` zostanie ukończona asynchronicznie.`DisposeAsync`
- _Konfigurowanie `DisposeAsync` przy użyciu `bool continueOnCapturedContext` (`ConfigureAwait`)_ : podczas gdy mogą wystąpić problemy związane z tym, w jaki sposób takie koncepcje są ujawniane w `using`, `foreach`i innych konstrukcjach językowych, które go wykorzystują, z perspektywy interfejsu, nie wykonuje żadnych operacji w `await`"i nie ma niczego do skonfigurowania... odbiorcy `ValueTask` mogą korzystać z niego.
- _`IAsyncDisposable` dziedziczyć `IDisposable`_ : ponieważ należy użyć tylko jednego lub drugiego, nie ma sensu, aby wymusić implementację typów.
- _`IDisposableAsync`, a nie `IAsyncDisposable`_ : nastąpiło nadanie nazw, których elementy/typy są "asynchroniczne coś", podczas gdy operacje są "gotowe asynchroniczne", więc typy mają wartość "Async" jako prefiks, a metody mają wartość "Async" jako sufiks.

### <a name="iasyncenumerable--iasyncenumerator"></a>IAsyncEnumerable / IAsyncEnumerator

Do podstawowych bibliotek platformy .NET dodawane są dwa interfejsy:

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

Typowe użycie (bez dodatkowych funkcji języka) będzie wyglądać następująco:

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

Odrzucone opcje:
- _`Task<bool> MoveNextAsync(); T current { get; }`_ : użycie `Task<bool>` może obsługiwać używanie buforowanego obiektu zadania do reprezentowania synchronicznych, udanych wywołań `MoveNextAsync`, ale alokacja będzie nadal wymagana na potrzeby asynchronicznego uzupełniania.  Zwracając `ValueTask<bool>`, włączamy obiekt modułu wyliczającego do samego implementującego `IValueTaskSource<bool>` i służy jako kopia zapasowa `ValueTask<bool>` zwrócone z `MoveNextAsync`, co z kolei pozwala na znacznie zmniejszone przedziały.
- _`ValueTask<(bool, T)> MoveNextAsync();`_ : nie tylko trudniejsze do użycia, ale oznacza to, że `T` nie mogą być już współwariantowe.
- _`ValueTask<T?> TryMoveNextAsync();`_ : nie jest współwariantem.
- _`Task<T?> TryMoveNextAsync();`_ : nie należy do współdzielenia, alokacje dla każdego wywołania itd.
- _`ITask<T?> TryMoveNextAsync();`_ : nie należy do współdzielenia, alokacje dla każdego wywołania itd.
- _`ITask<(bool,T)> TryMoveNextAsync();`_ : nie należy do współdzielenia, alokacje dla każdego wywołania itd.
- _`Task<bool> TryMoveNextAsync(out T result);`_ : należy ustawić wynik `out`, gdy operacja zwróci synchronicznie, a nie gdy asynchronicznie ukończy zadanie potencjalnie kiedyś w przyszłości, w którym nie ma możliwości przekazania wyniku.
- _nie`IAsyncEnumerator<T>` implementowania `IAsyncDisposable`_ : możemy oddzielić te opcje.  Jednak wykonanie tej operacji komplikuje niektóre inne obszary wniosku, ponieważ kod musi być w stanie zaradzić sobie z możliwością, że moduł wyliczający nie zapewnia usuwania, co utrudnia pisanie pomocników opartych na wzorcach.  Ponadto będzie to typowy w przypadku modułów wyliczających potrzebnych do usuwania (np. wszelkich C# iteratorów asynchronicznych, które mają blok finally, większość elementów wyliczających dane z połączenia sieciowego itp.), a jeśli takie nie, można łatwo zaimplementować metodę w sposób czysty jako `public ValueTask DisposeAsync() => default(ValueTask);` z minimalnym dodatkowym obciążeniem.
- _ `IAsyncEnumerator<T> GetAsyncEnumerator()`: brak parametru tokenu anulowania.

#### <a name="viable-alternative"></a>Żywotna alternatywa:

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

`TryGetNext` jest używana w pętli wewnętrznej do korzystania z elementów z pojedynczym wywołaniem interfejsu, o ile są one dostępne synchronicznie.  Gdy następny element nie może zostać pobrany synchronicznie, zwraca wartość false, a w każdym momencie zwraca wartość false, wywołujący musi następnie wywołać `WaitForNextAsync`, aby poczekać na udostępnienie następnego elementu lub określić, że element nie będzie inny. Typowe użycie (bez dodatkowych funkcji języka) będzie wyglądać następująco:

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

Zaletą tego jest dwukrotne, jednopomocnicze i jeden główny:
- _Pomocniczy: umożliwia modułowi wyliczającemu obsługę wielu odbiorców_. Mogą istnieć scenariusze, w których jest cenny dla modułu wyliczającego, aby obsługiwał wielu klientów jednocześnie.  Nie można osiągnąć, gdy `MoveNextAsync` i `Current` są oddzielone w taki sposób, że implementacja nie może spowodować ich użycia jako niepodzielne.  Z kolei takie podejście zapewnia jedną metodę `TryGetNext`, która obsługuje wypychanie modułu wyliczającego do przodu i pobieranie następnego elementu, więc moduł wyliczający może w razie potrzeby włączyć niepodzielność.  Jednak jest to możliwe, że takie scenariusze można także włączyć, dając każdy konsumentowi swój moduł wyliczający z udostępnionego wyliczalnego.  Ponadto nie chcemy wymusić, aby każdy moduł wyliczający obsługiwał współbieżne użycie, ponieważ spowodowałoby to dodanie nieuproszczonych nadmiarów do większości przypadków, które nie wymagają tego, co oznacza, że konsument interfejsu zazwyczaj nie może polegać na tym, co się dzieje.
- _Główna: wydajność_. Podejście `MoveNextAsync`/`Current` wymaga dwóch wywołań interfejsu dla każdej operacji, podczas gdy najlepszym przypadkiem `WaitForNextAsync`/`TryGetNext` jest to, że większość iteracji kończy się synchronicznie, włączając w to ścisłą wewnętrzną pętlę z `TryGetNext`, tak aby tylko jedno wywołanie interfejsu dla operacji.  Może to mieć wymierny wpływ na sytuacje, w których wywołania interfejsu dominują obliczenia.

Jednak istnieją nieuproszczone downsides, w tym znacząco zwiększona złożoność podczas ręcznego korzystania z nich, i zwiększona szansa na wprowadzenie usterek podczas ich używania.  Mimo że korzyści z wydajności są wyświetlane w mikrotestach, nie uważamy, że wpłyną one na ogromną większość rzeczywistych zastosowań.  Jeśli wyłączy to, możemy wprowadzić drugi zestaw interfejsów w sposób jasny.

Odrzucone opcje:
- `ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: parametry `out` nie mogą być współwariantowe.  W tym miejscu znajduje się również niewielki wpływ (ogólny problem z wzorcem try), który prawdopodobnie powoduje wystąpienie bariery zapisu w środowisku uruchomieniowym dla wyników typu referencyjnego.

#### <a name="cancellation"></a>Anulowanie

Istnieje kilka możliwych podejścia do obsługi anulowania:
1. `IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` są anulowane — niezależny od: `CancellationToken` nie pojawia się w dowolnym miejscu.  Anulowanie jest realizowane przez logicznie pieczenie `CancellationToken` do wyliczalnego i/lub modułu wyliczającego w jakikolwiek sposób, na przykład w przypadku wywołania iteratora, przekazanie `CancellationToken` jako argumentu do metody iteratora i użycie go w treści iteratora, tak jak w przypadku każdego innego parametru.
2. `IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: przekazanie `CancellationToken` do `GetAsyncEnumerator`, a kolejne operacje `MoveNextAsync` będą jednakowe.
3. `IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: przekazanie `CancellationToken` do poszczególnych wywołań `MoveNextAsync`.
4. 1 & & 2: Osadź `CancellationToken`s w wyliczalnym/wyliczającym i przekaż `CancellationToken`s do `GetAsyncEnumerator`.
5. 1 & & 3: Osadź `CancellationToken`s w wyliczalnym/wyliczającym i przekaż `CancellationToken`s do `MoveNextAsync`.

Z perspektywy czysto teoretycznej (5) jest najbardziej niezawodna, w tym (a) `MoveNextAsync` akceptujący `CancellationToken` włącza największą kontrolę nad tym, co zostało anulowane, a (b) `CancellationToken` jest tylko dowolnym innym typem, który może przekazywać jako argument do iteratorów, osadzony w dowolnym typie itd.

Istnieje jednak wiele problemów z tym podejściem:
- Jak `CancellationToken` przeszedł do `GetAsyncEnumerator` umieścić ją w treści iteratora?  Możemy uwidocznić nowe słowo kluczowe `iterator`, aby uzyskać dostęp do `CancellationToken` przekazanego do `GetEnumerator`, jednak a) to wiele dodatkowych maszyn, b) czynimy to bardzo pierwszej klasy obywatelem, a c) przypadek 99% wydaje się być tym samym kodem, który wywołuje iterator i wywołując `GetAsyncEnumerator` w tym przypadku, w takim przypadku można tylko przekazać `CancellationToken` jako argument do metody.
- Jak `CancellationToken` przeszedł do `MoveNextAsync` do treści metody?  Jest to nawet gorsze, tak jakby był on narażony na `iterator` obiektu lokalnego, jego wartość może ulec zmianie między czekami, co oznacza, że każdy kod zarejestrowany w tokenie musi wyrejestrować się z niego przed oczekiwaniami, a następnie ponownie zarejestrować po; jest również możliwe, że konieczna jest taka rejestracja i Wyrejestrowanie w każdym `MoveNextAsync` wywołaniu, niezależnie od tego, czy zaimplementowane przez kompilator w iteratorze, czy przez dewelopera ręcznie.
- W jaki sposób deweloper anulował `foreach` pętlę?  Jeśli jest to możliwe poprzez nadanie `CancellationToken` wyliczalnemu/wyliczającemu, wówczas musimy obsługiwać `foreach`"w przypadku modułów wyliczających, dzięki czemu są one przeznaczone dla obywateli pierwszej klasy, a teraz trzeba zacząć myśleć o ekosystemie skompilowanej wokół modułów wyliczających (np. metody LINQ) lub b), dlatego musimy osadzić `CancellationToken` w wyliczalnym elemencie, który ma `WithCancellation` metodę rozszerzenia `IAsyncEnumerable<T>`, która będzie przechowywać podany token, a następnie przekazać ją do `GetAsyncEnumerator` opakowanego wyliczalnego, gdy `GetAsyncEnumerator` w zwróconej strukturze jest wywoływany (zignorowano ten token).  Można też użyć `CancellationToken` w treści instrukcji foreach.
- Jeśli są obsługiwane zrozumienia zapytania, w jaki sposób `CancellationToken` dostarczana do `GetEnumerator` lub `MoveNextAsync` być przekazywany do każdej klauzuli?  Najprostszym sposobem, aby można było przechwycić tę klauzulę, w tym momencie każdy token jest przekazywać do `GetAsyncEnumerator`/`MoveNextAsync` jest ignorowany.

Zalecana jest wcześniejsza wersja tego dokumentu (1), ale od przełączenia do (4).

Dwa główne problemy z programem (1):
- producenci wyliczalnych elementów nie muszą implementować niektórych wzorców i mogą wykorzystać obsługę kompilatora dla iteratorów asynchronicznych, aby zaimplementować metodę `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)`.
- prawdopodobnie wielu producentów byłoby skłonny do dodawania `CancellationToken` parametru do ich wyliczalnej asynchronicznie sygnatury, co uniemożliwi konsumentom przekazywanie tokenów anulowania, gdy mają one typ `IAsyncEnumerable`.

Istnieją dwa główne scenariusze użycia:
1. `await foreach (var i in GetData(token)) ...`, gdzie odbiorca wywołuje metodę Async-iterator,
2. `await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...`, gdzie odbiorca zajmuje się danym wystąpieniem `IAsyncEnumerable`.

Wiemy, że rozsądne kompromisy do obsługi obu scenariuszy w taki sposób, który jest wygodny dla producentów i konsumentów strumieni asynchronicznych, ma zastosowanie specjalnie do adnotacjgo parametru w metodzie iteratora Async. W tym celu jest używany atrybut `[EnumeratorCancellation]`. Umieszczenie tego atrybutu na parametrze informuje kompilator, że jeśli token jest przekazaniem do metody `GetAsyncEnumerator`, ten token powinien zostać użyty zamiast wartości pierwotnie przekazaną dla parametru.

Rozważ `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`. Realizator tej metody może po prostu użyć parametru w treści metody. Konsument może korzystać z powyższych wzorców użycia:
1. Jeśli używasz `GetData(token)`, token jest zapisywany w postaci wyliczalnej asynchronicznie i będzie używany w iteracji,
2. Jeśli używasz `givenIAsyncEnumerable.WithCancellation(token)`, token przeszedł do `GetAsyncEnumerator` zastąpi dowolny token zapisany w wyliczalnym asynchronicznie.

## <a name="foreach"></a>foreach

`foreach` zostanie rozszerzony do obsługi `IAsyncEnumerable<T>` oprócz istniejącej obsługi `IEnumerable<T>`.  I będzie obsługiwał odpowiednik `IAsyncEnumerable<T>` jako wzorca, jeśli odpowiednie elementy członkowskie są ujawnione publicznie, powracając do korzystania z interfejsu bezpośrednio, w celu włączenia rozszerzeń opartych na strukturze, które nie pozwalają na przydzielanie, a także używania alternatywnej awaitables jako typu zwracanego `MoveNextAsync` i `DisposeAsync`.

### <a name="syntax"></a>Składnia

Za pomocą składni:

```csharp
foreach (var i in enumerable)
```

C#Program będzie kontynuował traktowanie `enumerable` jako synchronicznego wyliczalnego, tak że nawet jeśli udostępni odpowiednie interfejsy API dla wyliczalnych asynchronicznie (uwidaczniając wzorzec lub implementując interfejs), będzie uwzględniać tylko synchroniczne interfejsy API.

Aby wymusić `foreach` w zamian tylko asynchronicznych interfejsów API, `await` zostanie wstawiony w następujący sposób:

```csharp
await foreach (var i in enumerable)
```

Nie zostanie dostarczona żadna składnia, która mogłaby obsługiwać korzystanie z interfejsów API Async lub Sync; Deweloper musi wybrać na podstawie używanej składni.

Odrzucone opcje:
- _`foreach (var i in await enumerable)`_ : Ta składnia jest już prawidłowa i zmiana jej znaczenia byłaby istotną zmianą.  Oznacza to, że `await` `enumerable`, należy uzyskać synchronicznie ITER z niego, a następnie synchronicznie wykonać iterację w tym celu.
- _`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)``foreach (await var i in enumerable)`_ : te wszystkie sugerują, że czekamy na następny element, ale istnieją inne oczekiwania w instrukcji foreach, w szczególności jeśli wyliczalny jest `IAsyncDisposable`, będziemy `await`"wypróbować jego asynchroniczne usuwanie.  Oczekiwany jest zakres instrukcji foreach, a nie dla każdego pojedynczego elementu, a tym samym słowo kluczowe `await` zachowuje się na poziomie `foreach`.  Ponadto, jeśli jest ona skojarzona z `foreach` umożliwia nam opisywanie `foreach` z innym terminem, np. "await foreach".  Jednak ważniejsze jest uwzględnienie `foreach` składni w tym samym czasie co składnia `using`, dzięki czemu są one spójne ze sobą, a `using (await ...)` jest już prawidłową składnią.
- _`foreach await (var i in enumerable)`_

Nadal należy wziąć pod uwagę:
- `foreach` dzisiaj nie obsługuje iteracji przez moduł wyliczający.  Oczekujemy, że firma Microsoft będzie bardziej powszechna `IAsyncEnumerator<T>`s, a tym samym zachęcamy do obsługi `await foreach` zarówno w `IAsyncEnumerable<T>`, jak i `IAsyncEnumerator<T>`.  Jednak po dodaniu takiego wsparcia przedstawimy pytanie, czy `IAsyncEnumerator<T>` jest obywatelem pierwszej klasy i czy musi mieć przeciążenia kombinatorów, które działają na modułach wyliczających oprócz wyliczalnych?    Czy chcemy zachęcić metody do zwracania modułów wyliczających zamiast wyliczalnych? Będziemy nadal omawiać ten temat.  Jeśli zdecydujesz, że nie chcesz jej obsłużyć, możemy wprowadzić metodę rozszerzenia `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);`, która umożliwia modułowi wyliczającemu dalszą `foreach`.  W przypadku podjęcia decyzji o tym, czy chcemy ją obsługiwać, musimy również zdecydować, czy `await foreach` będzie odpowiedzialny za wywoływanie `DisposeAsync` w module wyliczającym, a odpowiedź ma prawdopodobnie wartość "nie", kontrola nad usuwaniem powinna być obsługiwana przez osobę o nazwie `GetEnumerator`".

### <a name="pattern-based-compilation"></a>Kompilacja oparta na wzorcu

Kompilator powiąże się z interfejsami API opartymi na wzorcu, jeśli istnieją, a przede wszystkim używają interfejsu (wzorzec może być spełniony przy użyciu metod wystąpienia lub metod rozszerzenia).  Wymagania dotyczące wzorca są następujące:
- Wyliczalny element musi uwidaczniać metodę `GetAsyncEnumerator`, która może być wywołana bez argumentów i która zwraca moduł wyliczający, który spełnia odpowiedni wzorzec.
- Moduł wyliczający musi ujawniać `MoveNextAsync` metodę, która może być wywołana bez argumentów i która zwraca element, który może być `await`Ed i którego `GetResult()` zwraca `bool`.
- Moduł wyliczający musi także uwidaczniać Właściwość `Current`, której metoda pobierająca zwraca `T` reprezentujący rodzaj wyliczanych danych.
- Moduł wyliczający może opcjonalnie uwidocznić metodę `DisposeAsync`, która może być wywoływana bez argumentów i która zwraca element, który może być `await`Ed, i którego `GetResult()` zwraca `void`.

Ten kod:

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

jest tłumaczony na odpowiednik:

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

Jeśli typ iteracji nie ujawnia wzorca praw, zostaną użyte interfejsy.

### <a name="configureawait"></a>ConfigureAwait

Ta kompilacja oparta na wzorcu zezwala na używanie `ConfigureAwait` na wszystkich czekach, za pośrednictwem metody rozszerzenia `ConfigureAwait`:

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

Będzie on oparty na typach, które dodamy do platformy .NET, prawdopodobnie z rozszerzeniem system. Threading. Tasks. Extensions. dll:

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

Należy zauważyć, że takie podejście nie spowoduje włączenia `ConfigureAwait`, które mają być używane z wyliczalnymi opartymi na wzorcu, a następnie ponownie jest tak, że `ConfigureAwait` jest uwidoczniona tylko jako rozszerzenie na `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` i nie można go zastosować do arbitralnych oczekujących elementów, ponieważ ma to sens tylko w przypadku zastosowania do zadań (kontroluje zachowanie zaimplementowane w obsłudze kontynuacji zadania) i w ten sposób nie ma sensu w przypadku używania wzorca, w którym oczekujące operacje mogą nie być zadaniami.  Każda osoba zwracająca oczekiwane elementy mogą zapewnić własne zachowanie niestandardowe w takich zaawansowanych scenariuszach.

(Jeśli firma Microsoft może dochodzić do obsługi rozwiązania `ConfigureAwait` na poziomie zakresu lub zestawu, wówczas nie będzie to konieczne).

## <a name="async-iterators"></a>Iteratory asynchroniczne

Język/kompilator będzie obsługiwał produkcję `IAsyncEnumerable<T>`s i `IAsyncEnumerator<T>`s oprócz ich używania. Obecnie język obsługuje pisanie iteratora, takiego jak:

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

ale nie można używać `await` w treści iteratorów.  Dodamy tę pomoc techniczną.

### <a name="syntax"></a>Składnia

Istniejąca obsługa języka dla iteratorów wnioskuje charakter iteratora metody w zależności od tego, czy zawiera on dowolny `yield`s.  Ta sama wartość będzie prawdziwa dla iteratorów asynchronicznych.  Takie Iteratory asynchroniczne zostaną rozbudowane i odróżniać od iteratorów synchronicznych poprzez dodanie `async` do sygnatury, a także musi `IAsyncEnumerable<T>` lub `IAsyncEnumerator<T>` jako typ zwracany.  Na przykład powyższy przykład może być zapisany jako iterator asynchroniczny w następujący sposób:

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

Rozważane alternatywy:
- _Nie można używać `async` w podpisie_: użycie `async` jest prawdopodobnie technicznie wymagane przez kompilator, ponieważ używa go do określenia, czy `await` jest prawidłowy w tym kontekście.  Ale nawet jeśli nie jest to wymagane, firma Microsoft ustaliła, że `await` mogą być używane tylko w metodach oznaczonych jako `async`i dlatego ważne jest, aby zachować spójność.
- _Włączanie konstruktorów niestandardowych dla `IAsyncEnumerable<T>`_ : jest to coś, co może wyglądać na przyszłość, ale maszyna jest skomplikowana i nie obsługuje tego synchronicznych odpowiedników.
- _Posiadanie `iterator`go słowa kluczowego w podpisie_: Iteratory asynchroniczne użyją `async iterator` w sygnaturze i `yield` mogą być używane tylko w metodach `async`, które zawierają `iterator`; `iterator` byłyby opcjonalne w iteratorach synchronicznych.  W zależności od perspektywy ma to na celu bardzo jasne zapełnienie podpisu metody, czy `yield` jest dozwolony i czy metoda rzeczywiście zwraca wystąpienia typu `IAsyncEnumerable<T>`, a nie kompilator wytwarza jeden w zależności od tego, czy kod używa `yield` czy nie.  Ale różni się od iteratorów synchronicznych, które nie są i nie można ich wymagać.  A niektórzy deweloperzy nie przypominają dodatkowej składni.  Jeśli zaprojektowano go od podstaw, prawdopodobnie jest to wymagane, ale w tym momencie istnieje znacznie większa wartość, dzięki czemu Iteratory asynchroniczne są blisko iteratorów synchronizacji.

## <a name="linq"></a>LINQ

Istnieje ponad 200 przeciążeń metod w klasie `System.Linq.Enumerable`, wszystkie te, które działają w warunkach `IEnumerable<T>`; Niektóre z tych metod akceptują `IEnumerable<T>`, niektóre z nich generują `IEnumerable<T>`i wiele.  Dodanie obsługi LINQ dla `IAsyncEnumerable<T>` prawdopodobnie wiąże się z duplikowaniem wszystkich tych przeciążeń, dla kolejnego ~ 200.  Ponieważ `IAsyncEnumerator<T>` najprawdopodobniej jest bardziej powszechny jako autonomiczna jednostka w świecie asynchronicznym, niż `IEnumerator<T>` w świecie synchronicznym, może być konieczne wykonanie innych przeciążeń ~ 200, które współpracują z `IAsyncEnumerator<T>`.  Ponadto duża liczba przeciążeń jest uwzględniana w predykatach (np. `Where`, które mają `Func<T, bool>`), i może być pożądane, aby `IAsyncEnumerable<T>`przeciążeń, które zajmują zarówno predykaty synchroniczne, jak i asynchroniczne (np. `Func<T, ValueTask<bool>>` oprócz `Func<T, bool>`).  Chociaż nie ma to zastosowania do wszystkich obecnie ~ 400 nowych przeciążeń, przybliżone obliczenie polega na tym, że ma to zastosowanie do połowy, co oznacza kolejną liczbę przeciążeń ~ 200, w sumie ~ 600 nowe metody.

Jest to rozłożona liczba interfejsów API, z możliwością jeszcze więcej w przypadku, gdy biblioteki rozszerzeń, takie jak rozszerzenia interaktywne (IX) są brane pod uwagę.  Jednak w przypadku systemu IX istnieje już implementacja wielu z nich i nie wydaje się, że jest to świetny powód, aby przeprowadzić jego duplikowanie. Zamiast tego należy pomóc społeczności ulepszyć IX i zarekomendować, gdy deweloperzy chcą używać LINQ z `IAsyncEnumerable<T>`.

Występuje również problem z składnią opisów zapytań.  Oparty na wzorcu charakter zapytania zezwala na "po prostu" z niektórymi operatorami, np. Jeśli IX zawiera następujące metody:

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

następnie ten C# kod będzie "po prostu":

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

Nie istnieje jednak żadna składnia dotycząca zrozumienia zapytania, która obsługuje używanie `await` w klauzulach, tak więc jeśli dodaliśmy do IX, na przykład:

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

spowoduje to "po prostu":

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

nie ma jednak możliwości zapisania go przy użyciu `await` wbudowanej w klauzuli `select`.  W oddzielnym wysiłku możemy przystąpić do dodawania wyrażeń `async { ... }` do języka, w którym można zezwolić na używanie ich w przypadku zrozumienia zapytania, a powyższe można napisać jako:

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

lub, aby włączyć `await`, które mają być używane bezpośrednio w wyrażeniach, na przykład przez obsługę `async from`.  Jednak jest to mało prawdopodobne, aby projekt miał wpływ na pozostałą część zestawu funkcji w jeden sposób lub drugi. nie jest to szczególnie wysoka wartość, która jest obecnie w stanie inwestować w ten sposób, więc propozycja ma teraz nic więcej.

## <a name="integration-with-other-asynchronous-frameworks"></a>Integracja z innymi platformami asynchronicznymi

Integracja z `IObservable<T>` i innymi platformami asynchronicznymi (np. strumienie reportowe) zostałaby przeprowadzona na poziomie biblioteki, a nie na poziomie języka.  Na przykład wszystkie dane z `IAsyncEnumerator<T>` mogą być publikowane na `IObserver<T>` po prostu przez `await foreach`"przekroczenia przez moduł wyliczający" i `OnNext`"przeprowadzenie danych do obserwatora, więc możliwe jest użycie metody rozszerzenia `AsObservable<T>`.  Zużywanie `IObservable<T>` w `await foreach` wymaga buforowania danych (w przypadku wypchnięcia innego elementu, gdy nadal trwa przetwarzanie poprzedniego elementu), ale taka karta może być łatwo zaimplementowana, aby umożliwić pobieranie `IObservable<T>` z `IAsyncEnumerator<T>`.  Itd.  RX/IX udostępnia już prototypy takich implementacji, a biblioteki, takie jak https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels zapewniają różne rodzaje buforowania struktur danych.  Język nie musi być uwzględniony na tym etapie.
