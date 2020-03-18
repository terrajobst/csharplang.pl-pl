---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484483"
---
# <a name="readonly-locals-and-parameters"></a>locale i parametry w trybie tylko do odczytu

* [x] proponowane
* [] Prototyp
* [] Implementacja
* [] — Specyfikacja

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Zezwalaj na dodawanie adnotacji do elementów lokalnych i parametrów jako tylko do odczytu w celu zapobieżenia przeniesieniu płytki do tych zmiennych lokalnych i parametrów.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Obecnie słowo kluczowe `readonly` można zastosować do pól; pozwala to upewnić się, że w czasie konstruowania pola można pisać tylko podczas konstrukcji (konstrukcja statyczna w przypadku pola statycznego lub konstruowania wystąpienia w przypadku pola wystąpienia), które ułatwia deweloperom uniknięcie błędów przez przypadkowe zastąpienie stanu, którego nie należy modyfikować. Ale pola nie są jedynymi miejscami, w których deweloperzy chcą mieć pewność, że wartości nie są mutacje. W szczególności często istnieje możliwość utworzenia zmiennej lokalnej do przechowywania stanu tymczasowego i przypadkowego aktualizowania stanu tymczasowego może spowodować błędne obliczenia i inne błędy, zwłaszcza wtedy, gdy takie "lokalne" są przechwytywane w wyrażeniach lambda, w których momencie zostały zniesione do pól, ale obecnie nie jest to możliwe, aby oznaczyć takie zniesione pola jako `readonly`.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Elementy lokalne będą mieć adnotację jako `readonly`, a kompilator gwarantuje, że są one ustawione tylko w czasie deklaracji (niektóre elementy lokalne w programie C# są już niejawnie do odczytu, takie jak Zmienna iteracji w pętli "foreach" lub użyta zmienna w bloku "Using", ale obecnie Deweloper nie ma możliwości oznaczania innych elementów lokalnych jako `readonly`). Takie `readonly` lokalne muszą mieć inicjatora:

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

I jako skrót dla `readonly var`, można użyć istniejącego kontekstowego słowa kluczowego `let`, np.

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

Nie istnieją żadne specjalne ograniczenia dotyczące tego, co może być inicjatorem, i mogą być obecnie ważne jako inicjator dla elementów lokalnych, np.

```csharp
readonly T data = arg1 ?? arg2;
```

`readonly` w lokalnych jest szczególnie cenny podczas pracy z wyrażeniami lambda i zamknięciami. Gdy metoda anonimowa lub lambda uzyskuje dostęp do stanu lokalnego z otaczającego zakresu, ten stan jest przechwytywany do zamknięcia przez kompilator, który jest reprezentowany przez "Klasa wyświetlania".  Każda wartość "lokalna", która jest przechwytywana, to pole w tej klasie, ale ponieważ kompilator generuje to pole w Twoim imieniu, nie ma możliwości dodawania adnotacji jako `readonly`, aby zapobiec błędnemu zapisywaniu wyrażenia lambda do "lokalnego" (w cudzysłowie, ponieważ nie jest to lokalne, co najmniej w wyniku MSIL). W przypadku `readonly` lokalnych kompilator może uniemożliwić zapis lambda z zapisu do lokalnego, który jest szczególnie cenny w scenariuszach obejmujących wielowątkowość, w przypadku których błędny zapis może spowodować niebezpieczną, ale rzadki i trudno znaleźć błąd współbieżności.

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

Jako specjalną formę lokalnego, parametry zostaną również opatrzone adnotacją jako `readonly`. Nie będzie to miało wpływu na to, co obiekt wywołujący metody jest w stanie przekazać do parametru (tak jak nie ma żadnego ograniczenia dotyczącego wartości, które mogą być przechowywane w `readonly` polu), ale podobnie jak w przypadku dowolnego `readonly` lokalnego, kompilator zabroni kodu przed zapisem do parametru po deklaracji, co oznacza, że treść metody jest zabronione przed zapisem do parametru.

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

`readonly` parametry nie wpływają na sygnaturę/metadane emitowane przez kompilator dla tej metody i po prostu wpływają na sposób, w jaki kompilator obsługuje kompilację treści metody. W tym celu na przykład podstawowa metoda wirtualna może mieć parametr `readonly` i ten parametr może być zapisywalny w przesłonięciu.

Podobnie jak w przypadku pól, `readonly` dla ustawień lokalnych i parametrów ma wpływ na lokalizację przechowywania, ale nie ma przechodniego wpływu na Graf obiektu. Jednak, podobnie jak w przypadku pól, wywoływanie metody na `readonly` struktura lokalna/parametr w rzeczywistości utworzy kopię struktury i Wywołaj metodę na kopii, aby uniknąć wewnętrznej mutacji `this`.

`readonly` zmiennych lokalnych i parametrów nie można przekazywać jako argumentów `ref` lub `out`, chyba że jest również obsługiwany `ref readonly`.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

- `val` może służyć jako alternatywna skrót do `let`.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

- `readonly ref` / `ref readonly` / `readonly ref readonly`: mam pytanie, jak obsłużyć `ref readonly` niezależnie od tej propozycji.
- Ta propozycja nie jest zgodna z strukturami ReadOnly/niezmiennymi typami. To pozostało na potrzeby oddzielnej propozycji.

## <a name="design-meetings"></a>Spotkania projektowe

- Zwięźle omówić o 21 stycznia 2015 (<https://github.com/dotnet/roslyn/issues/98>)
