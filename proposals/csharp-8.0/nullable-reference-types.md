---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484616"
---
# <a name="nullable-reference-types-in-c"></a>Typy odwołań dopuszczających wartość null wC# #

Celem tej funkcji jest:

* Zezwól deweloperom na wyrażenie, czy zmienna, parametr lub wynik typu referencyjnego mają mieć wartość null, czy nie.
* Podaj ostrzeżenia, gdy takie zmienne, parametry i wyniki nie są używane zgodnie z tym zamiarem.

## <a name="expression-of-intent"></a>Wyrażenie intencji

Język zawiera już składnię `T?` dla typów wartości. Poszerzenie tej składni do typów referencyjnych jest proste.

Przyjęto założenie, że intencja niebędącego typem odwołania `T` ma wartość różną od null.

## <a name="checking-of-nullable-references"></a>Sprawdzanie odwołań dopuszczających wartość null

Analiza przepływu śledzi zmienne odwołania do wartości null. Jeśli analiza uzna, że nie będzie ona mieć wartości null (np. po sprawdzeniu lub przypisaniu), ich wartość zostanie uznana za odwołanie o wartości innej niż null.

Odwołanie dopuszczające wartość null może być również jawnie traktowane jako inne niż null z przyrostkowym operatorem `x!` (operator "Dammit"), w przypadku gdy analiza przepływu nie może ustanowić niezerowej sytuacji, którą zna Deweloper.

W przeciwnym razie zostanie wyświetlone ostrzeżenie, jeśli odwołanie do wartości null jest wywoływane lub jest konwertowane na typ inny niż null.

Podczas konwertowania z `S[]` na `T?[]` i z `S?[]` na `T[]`należy uzyskać ostrzeżenie.

Ostrzeżenie jest wyrażane podczas konwersji z `C<S>` na `C<T?>`, z wyjątkiem sytuacji, gdy parametr typu jest współvariant (`out`) i podczas konwersji z `C<S?>` na `C<T>`, z wyjątkiem sytuacji, gdy parametr type ma wartość kontrawariantne (`in`).

Ostrzeżenie jest udzielane na `C<T?>`, jeśli parametr typu ma ograniczenia inne niż null. 

## <a name="checking-of-non-null-references"></a>Sprawdzanie odwołań o wartości innej niż null

Ostrzeżenie jest wyrażane, Jeśli literał o wartości null jest przypisany do zmiennej innej niż null lub przekazany jako parametr inny niż null.

Ostrzeżenie jest również udzielane, jeśli Konstruktor nie inicjuje jawnie pól odwołania o wartości innej niż null.

Nie można prawidłowo śledzić, że wszystkie elementy tablicy odwołań o wartości innej niż null są inicjowane. Można jednak wydać ostrzeżenie, jeśli nie zostanie przypisany żaden element nowo utworzonej tablicy, zanim tablica zostanie odczytana lub przeniesiona. To może obsłużyć typowy przypadek bez zbyt dużej szumu.

Musimy zdecydować, czy `default(T)` generuje ostrzeżenie, czy jest po prostu traktowany jak typ `T?`.

## <a name="metadata-representation"></a>Reprezentacja metadanych

Parametry definiowania wartości null powinny być reprezentowane w metadanych jako atrybuty. Oznacza to, że kompilatory niskiego poziomu zignorują je.

Musimy zdecydować, czy dołączane są tylko adnotacje dopuszczające wartość null, lub też wskazuje, czy w zestawie nie ma wartości "on".

## <a name="generics"></a>Typy ogólne

Jeśli parametr typu `T` ma ograniczenia, które nie dopuszcza wartości null, jest traktowany jako niedopuszczający wartości null w swoim zakresie.

Jeśli parametr typu jest nieograniczony lub ma tylko ograniczenia dopuszczające wartość null, sytuacja jest nieco bardziej złożona: oznacza to, że odpowiedni argument typu może być dopuszczany do wartości null lub nie dopuszczający *wartości null.* Bezpiecznym zadaniem do wykonania w tej sytuacji jest traktowanie parametru typu jako wartości null i niedopuszczający *wartości null,* co daje ostrzeżenia w przypadku naruszenia obu tych warunków. 

Warto rozważać, czy jawne ograniczenia odwołań do wartości null powinny być dozwolone. Należy jednak pamiętać, że nie można uniknąć, że typy odwołań dopuszczających wartość null nie są *jawnie* określone w pewnych przypadkach (dziedziczone ograniczenia).

Ograniczenie `class` jest inne niż null. Możemy rozważyć, czy `class?` powinien być prawidłowym ograniczeniem null, co oznacza, że typ odwołania dopuszcza wartość null.

## <a name="type-inference"></a>Wnioskowanie o typie

W przypadku wnioskowania o typie, jeśli typ współtworzenia ma typ referencyjny dopuszczający wartość null, Typ otrzymany powinien dopuszczać wartość null. Innymi słowy, wartość zerowa jest propagowana.

Należy rozważyć, czy literał `null` jako wyrażenie uczestniczące ma wchodzić w skład wartości null. Nie jest to dzisiaj: dla typów wartości, które prowadzi do błędu, natomiast w przypadku typów referencyjnych wartość null została pomyślnie konwertowana na typ zwykły. 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a>Zmiany powodujące niezgodność

Ostrzeżenia o wartościach innych niż null są oczywistymi istotnymi zmianami w istniejącym kodzie i powinny towarzyszyć mechanizmem akceptacji.

Mniej oczywiście ostrzeżenia dotyczące typów dopuszczających wartości null (jak opisano powyżej) są istotną zmianą istniejącego kodu w niektórych scenariuszach, w których wartość null jest niejawna:

* Parametry typu nieograniczonego będą traktowane jako niejawnie dopuszczające wartość null, więc przypisanie ich do `object` lub dostępu np. `ToString` zwróci ostrzeżenia.
* Jeśli wnioskowanie o typie wnioskuje o wartości null z wyrażeń `null`, wówczas istniejący kod czasami będzie zwracać wartości null zamiast typów niedopuszczających wartości null, co może prowadzić do nowych ostrzeżeń.

Dlatego ostrzeżenia null muszą być również opcjonalne

Na koniec Dodawanie adnotacji do istniejącego interfejsu API będzie stanowić istotną zmianę dla użytkowników, którzy wybrały ostrzeżenia, podczas uaktualniania biblioteki. Jest to również konieczne, aby można było wybrać lub wycofać. "Chcę rozwiązać usterkę, ale nie jestem gotowy do pracy z nowymi adnotacjami"

Podsumowując, musisz mieć możliwość wyboru z:
* Ostrzeżenia dopuszczające wartość null
* Ostrzeżenia o wartościach innych niż null
* Ostrzeżenia z adnotacji w innych plikach

Stopień szczegółowości wyboru zaproponuje model podobny do analizatora, gdzie swaths kodu może być dołączany do dyrektywy pragmy, a poziomy ważności mogą być wybierane przez użytkownika. Ponadto opcje poszczególnych bibliotek ("Ignoruj adnotacje z JSON.NET, dopóki nie zajdzie do rozpatrzenia") mogą być można wyrazić elementu w kodzie jako atrybuty.

Projekt doświadczenia z uczestnictwa/przejścia jest decydujący dla sukcesu i użyteczności tej funkcji. Musimy upewnić się, że:

* Użytkownicy mogą stopniowo przyjmować sprawdzanie wartości null, gdy chcą
* Autorzy biblioteki mogą dodawać adnotacje dotyczące wartości null bez obaw klientów.
* Pomimo tego nie ma sensu "Configuration okropnej"

## <a name="tweaks"></a>Ulepszenia

Możemy rozważyć nieużywanie adnotacji `?` lokalnych, ale po prostu zapoznaj się z tym, czy są one używane zgodnie z przypisanymi do nich informacjami. Nie podoba mi się to; Myślę, że powinna być jednolicie poinformowania użytkowników.

Możemy uwzględnić skróconą `T! x` dla parametrów, która automatycznie generuje sprawdzenie wartości null środowiska uruchomieniowego.

Niektóre wzorce dla typów ogólnych, takie jak `FirstOrDefault` lub `TryGet`, mają nieco brzmienia zachowanie przy użyciu argumentów typu niedopuszczających wartości null, ponieważ jawnie zwracają wartości domyślne w niektórych sytuacjach. Możemy spróbować Nuance system typów w celu lepszego dopasowania. Na przykład możemy zezwolić na `?` w przypadku parametrów typu nieograniczonego, mimo że argument typu może już mieć wartość null. Mam wątpliwości, że jest to możliwe, i prowadzi do Weirdness związanych z interakcją z typami *wartości* null. 

## <a name="nullable-value-types"></a>Typy wartości dopuszczające wartość null

Możemy rozważyć przyjęcie niektórych powyższych semantyki dla typów wartości null.

Wspomniano już wnioskowanie o typie, w którym możemy wnioskować `int?` od `(7, null)`, zamiast tylko wydawania błędu.

Kolejną możliwością jest zastosowanie analizy przepływu do wartości null. Jeśli są uznawane za niemające wartości null, możemy faktycznie zezwolić na użycie jako typu niedopuszczający wartości null w określonych sposobach (np. dostęp do elementów członkowskich). Należy zachować ostrożność, aby można było sprawdzić, czy elementy, które są *już* wykonywane na wartości typu null, będą preferowane ze względu na zgodność z poprzednimi przyczynami.
