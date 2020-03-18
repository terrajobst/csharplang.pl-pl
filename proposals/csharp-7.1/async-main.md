---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484798"
---
# <a name="async-main"></a>Asynchroniczny, główny

* [x] proponowane
* [] Prototyp
* [] Implementacja
* [] — Specyfikacja

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Zezwalaj na używanie `await` w metodzie głównej/EntryPoint aplikacji przez umożliwienie, aby punkt wejścia zwracał `Task` / `Task<int>` i być oznaczony `async`.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Jest bardzo powszechny podczas uczenia C#się, podczas pisania narzędzi opartych na konsoli i podczas pisania małych aplikacji testowych w celu wywołania i `await` metod `async` z poziomu głównego.  Obecnie dodaliśmy poziom złożoności w tym miejscu, wymuszając takie `await`, aby można było wykonać w oddzielnym metodzie asynchronicznej, co sprawia, że deweloperzy muszą pisać zwyczajowo, takie jak następujące:

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

Firma Microsoft może usunąć konieczność stosowania tego standardowego i ułatwić rozpoczęcie pracy po prostu przez umożliwienie głównej `async`, w taki sposób, aby można było używać `await`s.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Następujące podpisy są obecnie dozwolone dla punktu wejścia:

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

Rozszerzono listę dozwolonych elementów wejścia do uwzględnienia:

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

Aby uniknąć zagrożeń związanych ze zgodnością, nowe sygnatury będą traktowane jako prawidłowy punkt wejścia, jeśli nie są obecne przeciążenia poprzedniego zestawu.
Język/kompilator nie wymaga, aby punkt wejścia był oznaczony jako `async`, ale oczekuje się, że większość z nich zostanie oznaczona jako taka.

Gdy jeden z tych elementów jest zidentyfikowany jako punkt wejścia, kompilator wykryje rzeczywistą metodę punktu wejścia, która wywołuje jedną z następujących zakodowanych metod:
- ```static Task Main()``` spowoduje, że kompilator emituje odpowiednik ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task Main(string[])``` spowoduje, że kompilator emituje odpowiednik ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```
- ```static Task<int> Main()``` spowoduje, że kompilator emituje odpowiednik ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task<int> Main(string[])``` spowoduje, że kompilator emituje odpowiednik ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```

Przykładowe użycie:

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Główna wadą jest po prostu dodatkową złożonością obsługi dodatkowych sygnatur wejścia/wyjścia.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Inne rozważane warianty:

Zezwalanie na `async void`.  Musimy zachować semantykę identyczną dla kodu wywołującego go bezpośrednio, co może utrudnić wygenerowanie go przez wygenerowany punkt wejścia (bez zwrócenia zadania).  Możemy rozwiązać ten problem, generując dwie inne metody, np.

```csharp
public static async void Main()
{
   ... // await code
}
```

stanowi

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

Istnieją również kwestie dotyczące promowania użycia `async void`.

Użycie "MainAsync" zamiast "Main" jako nazwy.  Chociaż sufiks Async jest zalecany w przypadku metod zwracających zadania, to przede wszystkim funkcje biblioteki, które główne nie są, i obsługujące dodatkowe nazwy punktu wejścia poza "Main" nie są takie same.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

Nie dotyczy

## <a name="design-meetings"></a>Spotkania projektowe

Nie dotyczy
