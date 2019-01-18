---
ms.openlocfilehash: 75fcd5b00ea5cac218a9f7809c53b179df97825c
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229944"
---
# <a name="exceptions"></a>Wyjątki

Wyjątki w języku C# zapewnia jednolite, ze strukturą i bezpieczny sposób obsługi zarówno w poziomie systemu, jak i w poziomie aplikacji warunków błędów. Mechanizm wyjątków w języku C# jest bardzo podobny do C++, za pomocą kilka istotnych różnic:

*  W języku C#, wszystkie wyjątki musi być reprezentowana przez wystąpienie typu klasy pochodne `System.Exception`. W języku C++ dowolną wartość dowolnego typu może służyć do reprezentowania wyjątek.
*  W języku C# bloku finally ([instrukcjami "try"](statements.md#the-try-statement)) może służyć do pisania kodu zakończenia, który jest wykonywany w wykonywania normalnych i wyjątkowych warunków. Taki kod jest trudne do pisania w języku C++, bez duplikowania kodu.
*  W języku C# wyjątków na poziomie systemu, takie jak przepełnienia, dzielenie przez zero i wartość null wyłuskań mają dobrze zdefiniowane klasy wyjątków się z jest równoważny błędy na poziomie aplikacji.

## <a name="causes-of-exceptions"></a>Przyczyn wyjątków

Wyjątek może zostać wygenerowany na dwa różne sposoby.

*  A `throw` — instrukcja ([instrukcji "throw"](statements.md#the-throw-statement)) zgłasza wyjątek, natychmiast i bezwarunkowo. Kontrolki nigdy nie osiąga instrukcję natychmiast po `throw`.
*  Niektórych wyjątkowych warunkach powstałe podczas przetwarzania instrukcji C# i wyrażeń spowodować wyjątek w pewnych okolicznościach, gdy nie można ukończyć operacji, zwykle. Na przykład operacja dzielenia liczby całkowitej ([operator dzielenia](expressions.md#division-operator)) zgłasza `System.DivideByZeroException` Jeżeli mianownik wynosi zero. Zobacz [wspólnej klasy wyjątków](exceptions.md#common-exception-classes) listę różnych wyjątków, które mogą wystąpić w ten sposób.

## <a name="the-systemexception-class"></a>Klasy System.Exception

`System.Exception` Klasa jest typem podstawowym wszystkich wyjątków. Ta klasa ma kilka istotnych właściwości, które współużytkują wszystkie wyjątki:

*  `Message` jest to właściwość tylko do odczytu typu `string` zawierający zrozumiałą dla użytkownika opis powodu, dla wyjątku.
*  `InnerException` jest to właściwość tylko do odczytu typu `Exception`. Jeśli wartość jest różna od null, odwołuje się do wyjątku, który spowodował bieżący wyjątek — oznacza to, bieżący wyjątek został zgłoszony w bloku catch obsługi `InnerException`. W przeciwnym razie jego wartość wynosi null, wskazujący, że ten wyjątek nie zostało spowodowane przez inny wyjątek. Liczba obiektów wyjątków połączonych ze sobą w ten sposób może być dowolną.

Wartości tych właściwości można określić w wywołaniach do konstruktora wystąpienia dla `System.Exception`.

## <a name="how-exceptions-are-handled"></a>Sposób obsługi wyjątków

Wyjątki są obsługiwane przez `try` — instrukcja ([instrukcjami "try"](statements.md#the-try-statement)).

Gdy wystąpi wyjątek, system wyszukuje najbliższej `catch` klauzula, która może obsłużyć wyjątek, zgodnie z ustaleniami typu run-time wyjątku. Po pierwsze bieżącej metody jest wyszukiwana w załączonym leksykalnie `try` instrukcji i klauzule powiązaną instrukcją catch instrukcji try są uwzględniane w kolejności. Jeśli ono zawiedzie, połączenie jest wyszukiwane na leksykalnie otaczającej metody, która wywołała bieżącą metodę `try` instrukcję, która zawiera punkt wywołania do bieżącej metody. To wyszukiwanie jest kontynuowane do `catch` klauzuli zostanie znaleziony, które może obsłużyć bieżący wyjątek za pomocą nazw, klasy wyjątku, który jest w tej samej klasie lub jako klasa bazowa typów w czasie wykonywania wyjątek. A `catch` klauzula, która nie nazwa klasy wyjątków może obsługiwać wszystkie wyjątki.

Po znalezieniu pasującego klauzuli catch system przygotowuje kontrola jest przekazywana do pierwszej instrukcji klauzuli "catch". Przed rozpoczęciem wykonywania klauzuli "catch", system najpierw wykonuje, w kolejności, dowolny `finally` zagnieżdżonych klauzul skojarzonych z instrukcji try więcej, niż ten, który Przechwycono wyjątek.

Jeśli nie pasującego klauzuli "catch" zostanie znaleziony, wystąpi jedno z następujących czynności:

*  Jeśli wyszukiwanie pasujące klauzuli catch osiągnie Konstruktor statyczny ([konstruktorów statycznych](classes.md#static-constructors)) lub statycznego inicjatora pola użycie, a następnie `System.TypeInitializationException` jest generowany w momencie, który wyzwolił wywołanie konstruktora statycznego. Wewnętrzny wyjątek `System.TypeInitializationException` zawiera wyjątek, który pierwotnie został zgłoszony.
*  Jeśli szukania klauzule catch osiągnie kod, który początkowo uruchomiony wątek, wykonywanie wątku jest zakończony. Wpływ stracą jest zdefiniowane w implementacji.

Wyjątki, które występują podczas wykonywania destruktor są warte szczególnej uwagi. Jeśli wystąpi wyjątek podczas wykonywania destruktor, ten wyjątek nie zostanie przechwycony, wykonanie tego destruktor zostanie zakończony i nosi nazwę destruktor klasy podstawowej (jeśli istnieje). Jeśli nie ma klasy podstawowej (podobnie jak w przypadku właściwości `object` typu) lub jeśli nie ma żadnych destruktor klasy podstawowej, wówczas wyjątek jest odrzucany.

## <a name="common-exception-classes"></a>Wspólne klasy wyjątków

Następujące wyjątki są zgłaszane przez niektóre operacje języka C#.

|                                      |                |
|--------------------------------------|----------------|
| `System.ArithmeticException`         | Klasa bazowa dla wyjątków, które występują podczas operacje arytmetyczne, takie jak `System.DivideByZeroException` i `System.OverflowException`. | 
| `System.ArrayTypeMismatchException`  | Element zgłaszany, gdy magazyn do tablicy nie powiedzie się, ponieważ rzeczywisty typ elementu przechowywane jest niezgodna z rzeczywisty typ tablicy. | 
| `System.DivideByZeroException`       | Element zgłaszany, gdy wystąpi próba dzielenia wartości całkowitej przez zero. | 
| `System.IndexOutOfRangeException`    | Element zgłaszany, gdy próba indeksu tablicy przy użyciu indeksu, który jest mniejsza od zera lub poza granice tablicy. | 
| `System.InvalidCastException`        | Element zgłaszany, gdy konwersja jawna z typem bazowym lub interfejsem do typu pochodnego nie powiedzie się w czasie wykonywania. | 
| `System.NullReferenceException`      | Generowany, gdy `null` odwołanie jest używane w sposób, który powoduje, że przywoływany obiekt jest wymagane. | 
| `System.OutOfMemoryException`        | Zgłaszany, gdy próba przydzielenia pamięci (za pośrednictwem `new`) kończy się niepowodzeniem. | 
| `System.OverflowException`           | Zgłaszany, gdy operacja arytmetyczna w `checked` przepełnienia kontekstu. | 
| `System.StackOverflowException`      | Zgłaszany, gdy stos wykonywania wyczerpaniu przez zbyt wiele wywołań metod oczekujące; Zazwyczaj niepokoju bardzo szczegółowe lub niepowiązane rekursji. | 
| `System.TypeInitializationException` | Zgłaszany, gdy statyczny Konstruktor zgłasza wyjątek, a nie `catch` klauzul istnieje, aby przechwytywać je. | 
