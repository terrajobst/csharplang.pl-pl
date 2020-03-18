---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484525"
---
# <a name="callerargumentexpression"></a>CallerArgumentExpression

* [x] proponowane
* [] Prototyp: nie uruchomiono
* [] Implementacja: nie uruchomiono
* [] Specyfikacja: nie uruchomiono

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Zezwól deweloperom na przechwytywanie wyrażeń przesłanych do metody, aby umożliwić lepsze komunikaty o błędach w interfejsie API diagnostyki/testowania i zmniejsz liczbę naciśnięć klawiszy.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Gdy Walidacja potwierdzenia lub argumentu nie powiedzie się, deweloper chce wiedzieć, jak to możliwe, gdzie i dlaczego nie powiodło się. Jednak dzisiejsze interfejsy API diagnostyki nie w pełni ułatwiają to. Weź pod uwagę następującą metodę:

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

W przypadku niepowodzenia jednego z potwierdzeń ślad stosu będzie zawierać tylko nazwy pliku, numeru wiersza i metody. Deweloper nie będzie w stanie stwierdzić, które potwierdzenie nie powiodło się z tych informacji — (-y) musi otworzyć plik i przejść do podanego numeru wiersza, aby zobaczyć, co poszło źle.

Wynika to również z tego, że struktury testowania muszą dostarczać różne metody Assert. W przypadku xUnit, `Assert.True` i `Assert.False` nie są często używane, ponieważ nie zapewniają wystarczającej liczby kontekstów, co nie powiodło się.

Chociaż sytuacja jest lepsza dla weryfikacji argumentów, ponieważ nazwy nieprawidłowych argumentów są wyświetlane dla dewelopera, Deweloper musi przekazać te nazwy do wyjątków ręcznie. Jeśli powyższy przykład został ponownie zapisany w celu użycia tradycyjnego sprawdzania poprawności argumentu zamiast `Debug.Assert`, będzie wyglądać tak

```csharp
T Single<T>(this T[] array)
{
    if (array == null)
    {
        throw new ArgumentNullException(nameof(array));
    }

    if (array.Length != 1)
    {
        throw new ArgumentException("Array must contain a single element.", nameof(array));
    }

    return array[0];
}
```

Należy zauważyć, że `nameof(array)` musi być przekazywany do każdego wyjątku, chociaż jest już jasne z kontekstu, który jest nieprawidłowy.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

W powyższych przykładach, w tym ciąg `"array != null"` lub `"array.Length == 1"` w komunikacie potwierdzenia może pomóc deweloperowi określić, co się nie powiodło. Wprowadź `CallerArgumentExpression`: jest to atrybut, który może być używany przez platformę do uzyskania ciągu skojarzonego z określonym argumentem metody. Dodamy ją do `Debug.Assert` takich jak

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

Kod źródłowy w powyższym przykładzie pozostanie taki sam. Jednak kod, który faktycznie emituje kompilator, będzie odpowiadać

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

Kompilator specjalnie rozpoznaje atrybut na `Debug.Assert`. Przekazuje ciąg skojarzony z argumentem, do którego odwołuje się Konstruktor atrybutu (w tym przypadku `condition`) w witrynie wywołania. Gdy jedno z przyczyn nie powiedzie się, deweloper będzie pokazywać warunek, który był fałszywy i będzie wiedzieć, który z nich nie powiódł się.

W przypadku walidacji argumentu nie można używać atrybutu bezpośrednio, ale może być używany przez klasę pomocnika:

```csharp
public static class Verify
{
    public static void Argument(bool condition, string message, [CallerArgumentExpression("condition")] string conditionExpression = null)
    {
        if (!condition) throw new ArgumentException(message: message, paramName: conditionExpression);
    }

    public static void InRange(int argument, int low, int high,
        [CallerArgumentExpression("argument")] string argumentExpression = null,
        [CallerArgumentExpression("low")] string lowExpression = null,
        [CallerArgumentExpression("high")] string highExpression = null)
    {
        if (argument < low)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be less than {lowExpression} ({low}).");
        }

        if (argument > high)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be greater than {highExpression} ({high}).");
        }
    }

    public static void NotNull<T>(T argument, [CallerArgumentExpression("argument")] string argumentExpression = null)
        where T : class
    {
        if (argument == null) throw new ArgumentNullException(paramName: argumentExpression);
    }
}

T Single<T>(this T[] array)
{
    Verify.NotNull(array); // paramName: "array"
    Verify.Argument(array.Length == 1, "Array must contain a single element."); // paramName: "array.Length == 1"

    return array[0];
}

T ElementAt(this T[] array, int index)
{
    Verify.NotNull(array); // paramName: "array"
    // paramName: "index"
    // message: "index (-1) cannot be less than 0 (0).", or
    //          "index (6) cannot be greater than array.Length - 1 (5)."
    Verify.InRange(index, 0, array.Length - 1);

    return array[index];
}
```

Propozycja dodania takiej klasy pomocnika do struktury jest w toku https://github.com/dotnet/corefx/issues/17068. Jeśli ta funkcja języka została zaimplementowana, można zaktualizować propozycję, aby skorzystać z tej funkcji.

### <a name="extension-methods"></a>Metody rozszerzające

`CallerArgumentExpression`można odwoływać się do parametru `this` w metodzie rozszerzenia. Na przykład:

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

`thisExpression` otrzyma wyrażenie odpowiadające obiektowi przed kropką. Jeśli jest wywoływana za pomocą składni metody statycznej, np. `Ext.ShouldBe(contestant.Points, 1337)`, będzie ona zachowywać się tak, jakby pierwszy parametr nie był oznaczony `this`.

Zawsze powinno być wyrażenie odpowiadające parametrowi `this`. Nawet jeśli wystąpienie klasy wywołuje metodę rozszerzającą dla samej siebie, np. `this.Single()` z wewnątrz typu kolekcji, `this` jest przypuszczany przez kompilator, więc `"this"` zostanie przeniesiona. Jeśli ta reguła zostanie zmieniona w przyszłości, możemy rozważyć przekazanie `null` lub pustego ciągu.

### <a name="extra-details"></a>Dodatkowe szczegóły

- Podobnie jak w przypadku innych atrybutów `Caller*`, takich jak `CallerMemberName`, ten atrybut może być używany tylko w przypadku parametrów z wartościami domyślnymi.
- Dozwolone są liczne parametry oznaczone `CallerArgumentExpression`, jak pokazano powyżej.
- Przestrzeń nazw atrybutu zostanie `System.Runtime.CompilerServices`.
- Jeśli podano `null` lub ciąg, który nie jest nazwą parametru (np. `"notAParameterName"`), kompilator przekaże pusty ciąg.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

- Osoby, które wiedzą, jak używać dekompilatorów, będą mogli zobaczyć część kodu źródłowego w witrynach wywołań dla metod oznaczonych za pomocą tego atrybutu. Może to być niepożądane/nieoczekiwane w przypadku oprogramowania zamkniętego źródła.

- Chociaż nie jest to luka w samej funkcji, źródłem problemu może być to, że obecnie istnieje `Debug.Assert` interfejs API, który ma tylko `bool`. Nawet jeśli Przeciążenie pobierające komunikat miał drugi parametr oznaczony przy użyciu tego atrybutu i został wykonany jako opcjonalny, kompilator nadal będzie musiał wybrać komunikat No-Message w rozdzielczości przeciążenia. W związku z tym, przeciążenia bez komunikatów należy usunąć, aby skorzystać z tej funkcji, która byłaby zmianą binarną (choć nie źródłową).

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

- Jeśli w celu wyświetlenia kodu źródłowego w witrynach wywołań dla metod, które używają tego atrybutu, okazuje się, że będzie to problem, możemy wyrazić zgodę na efekty atrybutów. Deweloperzy umożliwią korzystanie z tego atrybutu `[assembly: EnableCallerArgumentExpression]` całego zestawu, które znajdują się w `AssemblyInfo.cs`.
  - W przypadku, gdy efekty atrybutów nie są włączone, metody wywołujące oznaczone atrybutem nie mogą być błędem, aby zezwolić istniejącym metodom na korzystanie z atrybutu i zachować zgodność ze źródłem. Jednak atrybut zostanie zignorowany, a metoda zostanie wywołana z dodaną wartością domyślną.

```csharp
// Assembly1

void Foo(string bar); // V1
void Foo(string bar, string barExpression = "not provided"); // V2
void Foo(string bar, [CallerArgumentExpression("bar")] string barExpression = "not provided"); // V3

// Assembly2

Foo(a); // V1: Compiles to Foo(a), V2, V3: Compiles to Foo(a, "not provided")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")

// Assembly3

[assembly: EnableCallerArgumentExpression]

Foo(a); // V1: Compiles to Foo(a), V2: Compiles to Foo(a, "not provided"), V3: Compiles to Foo(a, "a")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")
```

- Aby zapobiec występowaniu[ drawbacks] [problemu ze zgodnością binarną] za każdym razem, gdy chcemy dodać nowe informacje o wywołującym do `Debug.Assert`, rozwiązaniem alternatywnym będzie dodanie struktury `CallerInfo` do struktury zawierającej wszystkie niezbędne informacje o wywołującym.

```csharp
struct CallerInfo
{
    public string MemberName { get; set; }
    public string TypeName { get; set; }
    public string Namespace { get; set; }
    public string FullTypeName { get; set; }
    public string FilePath { get; set; }
    public int LineNumber { get; set; }
    public int ColumnNumber { get; set; }
    public Type Type { get; set; }
    public MethodBase Method { get; set; }
    public string[] ArgumentExpressions { get; set; }
}

[Flags]
enum CallerInfoOptions
{
    MemberName = 1, TypeName = 2, ...
}

public static class Debug
{
    public static void Assert(bool condition,
        // If a flag is not set here, the corresponding CallerInfo member is not populated by the caller, so it's
        // pay-for-play friendly.
        [CallerInfo(CallerInfoOptions.FilePath | CallerInfoOptions.Method | CallerInfoOptions.ArgumentExpressions)] CallerInfo callerInfo = default(CallerInfo))
    {
        string filePath = callerInfo.FilePath;
        MethodBase method = callerInfo.Method;
        string conditionExpression = callerInfo.ArgumentExpressions[0];
        ...
    }
}

class Bar
{
    void Foo()
    {
        Debug.Assert(false);

        // Translates to:

        var callerInfo = new CallerInfo();
        callerInfo.FilePath = @"C:\Bar.cs";
        callerInfo.Method = MethodBase.GetCurrentMethod();
        callerInfo.ArgumentExpressions = new string[] { "false" };
        Debug.Assert(false, callerInfo);
    }
}
```

Zostało to pierwotnie zaproponowane w https://github.com/dotnet/csharplang/issues/87.

Istnieje kilka wad tego podejścia:

- Mimo że jest to zrozumiałe dla odtworzenia, dzięki czemu można określić, które właściwości są potrzebne, może to znacząco obniżyć wydajność przez przydzielenie tablicy dla wyrażeń/wywołań `MethodBase.GetCurrentMethod` nawet wtedy, gdy potwierdzenie zakończy się powodzeniem.

- Ponadto podczas przekazywania nowej flagi do atrybutu `CallerInfo` nie będzie to zmiana istotna, `Debug.Assert` nie będzie zagwarantować, że w rzeczywistości otrzymasz nowy parametr z witryn wywołań, które zostały skompilowane względem starej wersji metody.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

TBD

## <a name="design-meetings"></a>Spotkania projektowe

Nie dotyczy
