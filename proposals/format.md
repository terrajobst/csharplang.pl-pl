---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "79485015"
---
# <a name="efficient-params-and-string-formatting"></a>Wydajne parametry i formatowanie ciągów

## <a name="summary"></a>Podsumowanie
Ta kombinacja funkcji spowoduje zwiększenie wydajności formatowania `string` wartości i przekazanie argumentów stylu `params`.

## <a name="motivation"></a>Motywacją
Narzuty przydziału dla formatowania `string` wartości mogą przeważać nad wydajnością wielu aplikacji tekstowych opartych na tekście: od nałożenia oddziału według typów `struct`, alokacji `object[]` dla `params` i pośrednich alokacji `string` podczas wywołań `string.Format`. W celu utrzymania wydajności takie aplikacje często wymagają porzucenia funkcji produktywności, takich jak `params` i `string` Interpolacja i przechodzenie do niestandardowych, ręcznie zakodowanych rozwiązań. 

Jako przykładu należy wziąć pod uwagę program MSBuild. Jest to zapisanie przy użyciu wielu nowoczesnych C# funkcji przez deweloperów, którzy mają świadomość wydajności. Jeszcze w jednym reprezentatywnym przykładzie kompilacja MSBuild wygeneruje 262MB alokacji `string` przy użyciu minimalnej szczegółowości. Z tego 1/2 alokacji są alokacje krótkoterminowe w ramach `string.Format`. Te funkcje mogą usunąć większość z programu na pulpicie .NET i uzyskać niemal zero na platformie .NET Core ze względu na dostępność `Span<T>`

Zestaw funkcji języka opisanych w tym miejscu umożliwia aplikacjom kontynuowanie korzystania z tych funkcji, z niewielką ilością lub bez zmian w bazie kodu aplikacji, przy jednoczesnym usunięciu niezamierzonych nakładów związanych z alokacją w większości przypadków.

## <a name="detailed-design"></a>Szczegółowy projekt 
Istnieje zestaw funkcji, które zostaną użyte w tym miejscu do osiągnięcia następujących wyników:

- Rozwijanie `params` w celu obsługi szerszego zestawu typów kolekcji.
- Umożliwienie deweloperom dostosowywania sposobu wykonywania interpolacji `string`. 
- Zezwalanie na interpolowane `string`, aby powiązać je z bardziej wydajnymi przeciążeniami `string.Format`.

### <a name="extending-params"></a>Rozszerzanie parametrów
Język będzie zezwalać na `params` w sygnaturze metody w celu posiadania typów `Span<T>`, `ReadOnlySpan<T>` i `IEnumerable<T>`. Te same reguły wywołania będą stosowane do tych nowych typów, które mają zastosowanie do `params T[]`:

- Nie można przeciążyć, gdy jedyną różnicą jest słowo kluczowe `params`.
- Można wywołać, przekazując serię argumentów, które są niejawnie konwertowane na `T` lub jeden `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>`.
- Musi być ostatnim parametrem w podpisie metody.
- Itp... 

`Span<T>` i `ReadOnlySpan<T>` wariantów są określane jako `Span<T>` poniżej dla uproszczenia. W przypadkach, w których zachowanie `ReadOnlySpan<T>` jest różne, zostanie jawnie wywołana. 

Zaletą `Span<T>` warianty `params` zapewniają, że kompilator zapewnia doskonałą elastyczność w sposobie przydzielania magazynu zapasowego dla wartości `Span<T>`. Przy `params T[]` kompilator musi przydzielić nowy `T[]` dla każdego wywołania metody `params`. Ponowne użycie nie jest możliwe, ponieważ musi zajmować wywoływany, przechowywany i ponownie wykorzystany parametr. Może to prowadzić do dużego nieefektywności metod z dużą ilością `params` wywołań.

Dana `Span<T>` warianty są `ref struct` element wywoływany nie może przechowywać argumentu. W związku z tym kompilator może zoptymalizować Lokacje wywołań, podejmując akcje, takie jak ponowne użycie argumentu. Może to spowodować, że powtarzające się wywołania są bardzo wydajne w porównaniu do `T[]`. Język, w którym nie ma żadnych szczególnych gwarancji dotyczących sposobu, w jaki takie callsites są zoptymalizowane. Należy zauważyć, że kompilator jest bezpłatny do używania wartości innych niż `T[]` podczas wywoływania metody `params Span<T>`. 

Jedna taka potencjalna implementacja jest następująca. Rozważ wszystkie wywołania `params` w treści metody. Kompilator może przydzielić tablicę, która ma rozmiar równy największemu wy`params`emu wywołania i użyć go dla wszystkich wywołań, tworząc odpowiednio rozmiar `Span<T>` wystąpień na tablicy. Na przykład:

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

Kompilator może wybrać emisję treści `Go` w następujący sposób:

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

Może to znacznie zmniejszyć liczbę tablic przydzieloną w aplikacji. Alokacje mogą być jeszcze bardziej zmniejszane, jeśli środowisko uruchomieniowe zapewnia narzędzia do przydzielenia przez inteligentniejszy stos tablic.

Ta optymalizacja nie może być zawsze stosowana, chociaż. Mimo że obiekt wywoływany nie może przechwycić argumentu `params`, nadal można go przechwycić w obiekcie wywołującym, gdy istnieje `ref` lub `out / ref` parametr, który jest własnym typem `ref struct`. 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

Te przypadki są statycznie wykrywalne. Może się to zdarzyć, gdy istnieje `ref` Return lub `ref struct` parametr przesłany przez `out` lub `ref`. W takim przypadku kompilator musi przydzielić świeżą `T[]` dla każdego wywołania. 

Niektóre inne potencjalne strategie optymalizacji zostały omówione na końcu tego dokumentu.

`IEnumerable<T>` wariant jest tylko wygodnym przeciążeniem. Jest to przydatne w scenariuszach, które często wykorzystują `IEnumerable<T>`, ale również mają wiele `params` użycia. Gdy jest wywoływana w `T` argumentem, magazyn zapasowy zostanie przydzielony jako `T[]`, tak jak `params T[]` jest gotowe.

### <a name="params-overload-resolution-changes"></a>zmiany rozdzielczości przeciążania parametrów
Ta propozycja oznacza, że język ma cztery warianty `params`, gdzie wcześniej. Jest to rozsądne dla metod definiowania przeciążeń metod, które różnią się tylko typem deklaracji `params`. 

Należy wziąć pod uwagę, że `StringBuilder.AppendFormat` oprócz `params object[]`należy dodać Przeciążenie `params ReadOnlySpan<object>`. Pozwoli to na znaczne zwiększenie wydajności poprzez zmniejszenie alokacji kolekcji bez konieczności wprowadzania jakichkolwiek zmian w kodzie wywołującym. 

Aby ułatwić tym językowi, należy wprowadzić następującą regułę zerwania powiązania z rozwiązaniem przeciążenia. Gdy metody kandydujące różnią się tylko parametrem `params`, kandydatów będą preferowane w następującej kolejności:

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

Ta kolejność jest najbardziej wydajna dla ogólnego przypadku.

### <a name="variant"></a>Typu
CoreFX jest prototypem nowego typu zarządzanego o nazwie [Variant](https://github.com/dotnet/corefxlab/pull/2595). Ten typ jest przeznaczony do użycia w interfejsach API, które oczekują wartości heterogenicznych, ale nie chcesz, aby obciążenie było włączone przy użyciu `object` jako parametru. Typ `Variant` zapewnia Uniwersalny magazyn, ale unika alokacji opakowania dla najczęściej używanych typów. Użycie tego typu w interfejsach API, takich jak `string.Format`, może wyeliminować obciążenie opakowania w większości przypadków.

Ten typ nie musi być specjalny dla języka. Jest ona wprowadzana w tym dokumencie osobno, gdy staną się szczegółami implementacji innych części wniosku. 

### <a name="efficient-interpolated-strings"></a>Wydajne parametry interpolowane
Ciągi interpolowane to popularne, jeszcze niewydajne funkcje w C#programie. Najbardziej typowa składnia, używając interpolowanej `string` jako `string`, tłumaczy na wywołanie `string.Format(string, params object[])`. Spowoduje to naliczenie przydziałów dla wszystkich typów wartości, pośrednich alokacji `string`, ponieważ implementacja w dużym stopniu używa `object.ToString` do formatowania, a także alokacji tablic, gdy liczba argumentów przekroczy wartość parametrów w przypadku przeciążenia "szybkie" `string.Format`. 

Język zmieni swoją interpolację, aby wziąć pod uwagę alternatywne przeciążenia `string.Format`. Rozważ wszystkie formy `string.Format(string, params)` i wybierz "najlepsze" Przeciążenie, które spełnia typy argumentów.
"Najlepsze" Przeciążenie `params` zostanie określone przez zasady omówione powyżej. Oznacza to, że interpolowane `string` mogą teraz wiązać się z bardzo wydajnymi przeciążeniami, takimi jak `string.Format(string format, params ReadOnlySpan<Variant> args)`. W wielu przypadkach spowoduje to usunięcie wszystkich przydziałów pośrednich.

### <a name="customizable-interpolated-strings"></a>Dostosowywalne ciągi interpolowane
Deweloperzy mogą dostosować zachowanie interpolowanych ciągów z `FormattableString`. Zawiera dane, które przechodzą do ciągu interpolowanego: format `string` i argumentów jako tablicę. Mimo to nadal ma alokację tablicy z opakowaniem i argumentem, a także alokację dla `FormattableString` (jest to `abstract class`). W związku z tym nie jest to małe użycie w przypadku aplikacji, które są przydzielane w `string` formatowania.

Aby format ciągu interpolowanego był wydajny, język rozpoznaje nowy typ: `System.ValueFormattableString`. Wszystkie interpolowane ciągi będą miały konwersję typu docelowego do tego typu. Zostanie to zaimplementowane przez przetłumaczenie ciągu interpolowanego na wywołanie `ValueFormattableString.Create` dokładnie tak, jak zostało wykonane dla `FormattableString.Create` dzisiaj. Język będzie obsługiwał wszystkie opcje `params` opisane w tym dokumencie podczas wyszukiwania najbardziej odpowiedniej metody `ValueFormattableString.Create`. 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

Reguły rozpoznawania przeciążenia zostaną zmienione na preferowane `ValueFormattableString` przez `string`, gdy argument jest ciągiem interpolowanym. Oznacza to, że będzie to przydatne w przypadku przeciążeń, które różnią się tylko `string` i `ValueFormattableString`. Takie Przeciążenie już dziś z `FormattableString` nie jest cenne, ponieważ kompilator zawsze preferuje `string` wersji (chyba że deweloper użyje jawnego rzutowania). 

## <a name="open-issues"></a>Otwarte problemy

### <a name="valueformattablestring-breaking-change"></a>ValueFormattableStringa zmiana
Zmiana, która ma być preferowana `ValueFormattableString` podczas rozpoznawania przeciążenia przez `string` jest istotną zmianą. Deweloper może zdefiniować typ o nazwie `ValueFormattableString` dzisiaj i używać go w przeciążeniach metody z `string`. Ta proponowana zmiana spowodowałaby, że kompilator wybierze inne Przeciążenie po zaimplementowaniu tego zestawu funkcji. 

Jest to prawdopodobnie niska. Typ musi mieć pełną nazwę `System.ValueFormattableString` i musi mieć `static` metody o nazwie `Create`. Biorąc pod względu, że deweloperzy są zdecydowanie odradzani od definiowania dowolnego typu w przestrzeni nazw `System` ten podział wydaje się taki sam, jak w przypadku rozsądnego naruszenia.

### <a name="expanding-to-more-types"></a>Rozszerzanie do większej liczby typów
Z tego względu będziemy mogli rozważyć dodanie `IList<T>`, `ICollection<T>` i `IReadOnlyList<T>` do zestawu kolekcji, dla których `params` jest obsługiwana. W ramach wdrożenia będzie kosztować niewielką ilość w tym miejscu.

Program LDM musi zdecydować, czy pozostała do tego język. Dodanie `IEnumerable<T>` usuwa bardzo konkretny punkt tarcia. Brak tego rozwiązania `params` wielu klientów zmuszony do przydzielenia `T[]` z `IEnumerable<T>` podczas wywoływania metody `params`. Dodanie `IEnumerable<T>` rozwiązuje ten problem. Nie ma konkretnego punktu tarcia, który w tym miejscu rozwiązuje inne interfejsy. 

## <a name="considerations"></a>Zagadnienia do rozważenia

### <a name="variant2-and-variant3"></a>Variant2 i Variant3
Zespół CoreFX ma także nieprzydzielany zestaw typów magazynu dla maksymalnie trzech argumentów `Variant`. Są to pojedyncze `Variant`, `Variant2` i `Variant3`. Wszystko, co ma parę metod do uzyskania bezpłatnej alokacji `Span<Variant>` z nich: `CreateSpan` i `KeepAlive`. Oznacza to, że dla `params Span<Variant>` do trzech argumentów można całkowicie bezpłatnie przydzielić witrynę wywołania.

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

Metodę `Go` można obniżyć do następujących:

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

Wymaga to bardzo małego nakładu pracy nad propozycją w celu ponownego użycia `T[]` między wywołaniami `params Span<T>`. Kompilator musi już zarządzać tymczasowym dla każdego wywołania i przeprowadzić oczyszczanie pracy po (nawet jeśli w jednym przypadku oznacza to, że jest to tylko oznaczenie wewnętrzne temp as Free). 

Uwaga: funkcja `KeepAlive` jest wymagana tylko na pulpicie. W przypadku platformy .NET Core Metoda nie będzie dostępna i w związku z tym kompilator nie emituje wywołania do niego.

### <a name="clr-stack-allocation-helpers"></a>Pomocnicy alokacji stosu CLR
Środowisko CLR zapewnia tylko [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) na potrzeby alokacji stosu ciągłej pamięci. Ta instrukcja jest ograniczona, ponieważ działa tylko dla typów `unmanaged`. Oznacza to, że nie można go użyć jako uniwersalnego rozwiązania do wydajnego alokowania magazynu zapasowego `params 
 Span<T>`. 

To ograniczenie nie dotyczy jednak niektórych zasadniczych ograniczeń, ale zamiast tego bardziej artefaktu historii. Środowisko CLR może dodać nowe kody operacji/wewnętrzne, które zapewniają uniwersalną alokację stosu. Można je następnie użyć do przydzielenia magazynu zapasowego dla większości wywołań `params Span<T>`.

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

Metodę `Go` można obniżyć do następujących:

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

To podejście jest bardzo wydajne, ponieważ powoduje to dodatkowe użycie stosu. W algorytmie, który ma głębokiego stosu i wiele `params` użycia, może to spowodować, że `StackOverflowException` zostanie wygenerowany, gdy prosta alokacja `T[]` się powiedzie. 

Niestety C# nie jest skonfigurowana dla typu analizy międzymetodowej, w której może zostać wyznaczony, czy wywołanie powinno korzystać z przydziału stosu lub sterty `params`. Można tylko naprawdę rozważyć każde wywołanie.

Środowisko CLR jest najlepszą konfiguracją dla tego typu wyznaczania w czasie wykonywania. Dlatego, że środowisko uruchomieniowe może zapewnić dwie metody alokacji uniwersalnego stosu:

1. `Span<T> StackAlloc<T>(int length)`: ma takie same zachowania i ograniczenia dotyczące `localloc`, z wyjątkiem tego, że może on korzystać z dowolnego typu `T`. 
1. `Span<T> MaybeStackAlloc<T>(int length)`: to środowisko uruchomieniowe może zdecydować się na wdrożenie przez wykonanie alokacji stosu lub sterty. Środowisko uruchomieniowe może następnie użyć kontekstu wykonywania, w którym jest wywoływana, aby określić sposób przydzielenia `Span<T>`. Obiekt wywołujący zawsze będzie traktować go tak, jakby był przydzielony przez stos.

W przypadku bardzo prostych przypadków, takich jak jeden do dwóch argumentów C# , kompilator może zawsze używać `StackAlloc<T>` Variant. Jest to mało prawdopodobne, aby znacząco przyczynić się do wyczerpania stosu w większości przypadków. W przypadku innych przypadków kompilator może użyć `MaybeStackAlloc<T>` zamiast tego, aby umożliwić wykonanie wywołania przez środowisko uruchomieniowe.

Wybór będzie prawdopodobnie wymagał dokładniejszego zbadania i zbadania rzeczywistych aplikacji świata. Jednak jeśli te nowe funkcje wewnętrzne są dostępne, zapewnia to elastyczność tego typu.

### <a name="why-not-varargs"></a>Dlaczego VarArgs? 
Istniejąca funkcja [VarArgs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) została uznana za możliwe rozwiązanie. Ta funkcja jest przeznaczona głównie dla C++scenariuszy/CLI i ma znane otwory dla innych scenariuszy. Ponadto w systemie UNIX występuje znaczny koszt. Dlatego nie było ono widoczne jako rozwiązanie do rozwiązania.

## <a name="related-issues"></a>Pokrewne problemy
Ta specyfikacja jest związana z następującymi problemami: 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

