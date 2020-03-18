---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484553"
---
# <a name="simplified-null-argument-checking"></a>Uproszczone sprawdzanie argumentów o wartości null

## <a name="summary"></a>Podsumowanie
Ta propozycja zapewnia uproszczoną składnię do sprawdzania poprawności argumentów metody nie są `null` i wyrzucają `ArgumentNullException` odpowiednie.

## <a name="motivation"></a>Motywacją
Prace nad projektowaniem typów referencyjnych dopuszczających wartość null spowodowały badanie kodu niezbędnego do weryfikacji argumentu `null`. Uwzględniając, że NRT nie ma wpływu na deweloperów wykonujących kod, nadal musi dodawać `if (arg is null) throw` kod kliszy kotłowej nawet w projektach, które są w pełni `null` czyste. Dzięki temu możemy poznać minimalną składnię argumentu `null` walidacji w języku. 

Ta składnia weryfikacji parametrów `null` powinna być często sparowana z NRT, dlatego propozycja jest w pełni niezależna od niej. Składnia może być używana niezależnie od dyrektyw `#nullable`.

## <a name="detailed-design"></a>Szczegółowy projekt 

### <a name="null-validation-parameter-syntax"></a>Składnia parametru weryfikacji o wartości null
Operator wykrzyknika, `!`, może być umieszczony po nazwie parametru na liście parametrów, co spowoduje, C# że kompilator emituje standardowy `null` Sprawdzanie kodu dla tego parametru. Jest to określane jako `null` składni parametrów walidacji. Na przykład:

``` csharp
void M(string name!) {
    ...
}
```

Zostanie przetłumaczony na:

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

Wygenerowane `null` Sprawdzanie zostanie przeprowadzone przed dowolnym kodem utworzonym przez dewelopera w metodzie. Gdy wiele parametrów zawiera operator `!`, sprawdzenia będą wykonywane w takiej samej kolejności, w jakiej są zadeklarowane parametry.

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

Sprawdzenie będzie przeznaczone dla równości referencyjnych `null`, nie wywołuje `==` ani żadnych operatorów zdefiniowanych przez użytkownika. Oznacza to również, że operator `!` może zostać dodany tylko do parametrów, których typ może być testowany pod kątem równości względem `null`. Oznacza to, że nie można jej użyć na parametrze, którego typ jest znany jako typ wartości.

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

W przypadku konstruktora `null` Walidacja zostanie wykonana przed jakimkolwiek innym kodem w konstruktorze. Obejmuje to: 

- Tworzenie łańcuchów do innych konstruktorów z `this` lub `base` 
- Inicjatory pola, które są niejawnie wykonywane w konstruktorze

Na przykład:

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

Zostanie przetłumaczy w następujący sposób:

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

Uwaga: nie jest to kod C# prawny, ale zamiast tego wystarczy przybliżyć to, co robi implementacja. 

Składnia parametru walidacji `null` będzie również prawidłowa na listach parametrów lambda. Jest to prawidłowe, nawet w składni jednego parametru, który nie ma parens.

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

Składnia jest również prawidłowa dla parametrów metod iteratora. W przeciwieństwie do innego kodu w iteratoru `null` Walidacja będzie wykonywana, gdy wywoływana jest metoda iteratora, a nie gdy zostanie przeprowadzony bazowy moduł wyliczający. Jest to prawdziwe w przypadku iteratorów tradycyjnych lub `async`.

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

Operatora `!` można używać tylko dla list parametrów, które mają skojarzoną treść metody. Oznacza to, że nie można go użyć w metodzie `abstract`, `interface`, `delegate` lub `partial` metodzie.

### <a name="extending-is-null"></a>Rozszerzanie ma wartość null
Typy, dla których wyrażenie `is null` jest prawidłowe, zostaną rozszerzone w taki sposób, aby obejmowało parametry typu nieograniczone. Pozwoli to na wypełnienie zamiaru sprawdzenia dla `null` wszystkich typów, które sprawdzają `null`. W przypadku typów, które nie są czasowo znane jako typy wartości. Na przykład parametry typu, które są ograniczone do `struct` nie mogą być używane z tą składnią.

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

Zachowanie `is null` na parametrze typu będzie takie samo, jak `== null` dzisiaj. W przypadku wystąpienia parametru typu jako typu wartości kod zostanie oceniony jako `false`. W przypadku, gdy jest to typ referencyjny, kod będzie sprawdzał poprawność `is null`.

### <a name="intersection-with-nullable-reference-types"></a>Część wspólna z typami odwołań dopuszczających wartość null
Wszystkie parametry, które mają zastosowany operator `!`, zaczynają się od nie`null`go stanu dopuszczającego wartość null. Jest to prawdziwe, nawet jeśli typ samego parametru jest potencjalnie `null`. Może wystąpić w przypadku jawnego typu dopuszczającego wartość null, takiego jak powiedzieć `string?`lub z nieograniczeniem typu parametru. 

Gdy składnia `!` w parametrach jest łączona z jawnym typem dopuszczającym wartość null w parametrze, zostanie wygenerowane ostrzeżenie przez kompilator:

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a>Otwarte problemy
None

## <a name="considerations"></a>Zagadnienia do rozważenia

### <a name="constructors"></a>Konstruktorów
Generowanie kodu dla konstruktorów oznacza, że istnieje małe, ale zauważalne, zmiana zachowania podczas przechodzenia z standardowej weryfikacji `null`ej dzisiaj i składni parametru weryfikacji `null` (`!`). Sprawdzanie `null` w standardowej weryfikacji odbywa się po obu inicjatorach pola i wywołaniach `base` lub `this`. Oznacza to, że deweloper nie może koniecznie migrować 100% `null` weryfikacji do nowej składni. Konstruktory z co najmniej wymagają pewnej inspekcji.

Po dokonaniu dyskusji chociaż została podjęta decyzja, że jest to bardzo mało prawdopodobne, aby spowodować poważne problemy z wdrażaniem. Jest to bardziej logiczne, że sprawdzanie `null` zostanie uruchomione przed jakąkolwiek logiką w konstruktorze. Może ponownie odwiedzać w przypadku wykrycia znaczących problemów ze zgodnością.

### <a name="warning-when-mixing--and-"></a>Ostrzegaj przy miksowaniu? lub!
Istniała długotrwała dyskusja dotycząca tego, czy należy wydać ostrzeżenie, gdy składnia `!` jest stosowana do parametru, który jawnie wpisano do typu dopuszczającego wartość null. Na powierzchni, która wygląda jak Deklaracja bezsensowniea przez dewelopera, ale istnieją przypadki, w których hierarchie typów mogą zmusić deweloperów do takiej sytuacji. 

Rozważmy następujące hierarchie klas w szeregu zestawów (przy założeniu, że wszystkie są kompilowane z włączonym sprawdzaniem `null`):

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

W tym miejscu autor `C3` zdecydował się dodać `null` walidacji do `o`parametru. Jest to całkowicie zgodne z tym, w jaki sposób funkcja jest przeznaczona do użycia.

Teraz wyobraź sobie, że autor Assembly2 zdecyduje się na dodanie następującego przesłonięcia:

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

Jest to dozwolone w przypadku typów referencyjnych dopuszczających wartość null, ponieważ jest to prawne, aby zamówienie było bardziej elastyczne dla pozycji wejściowych. Funkcja NRT ogólnie zezwala na rozsądną współkontrawariancjaę parametru/zwracaną wartość null. Jednak język wykonuje sprawdzanie współ/kontrawariancja na podstawie najbardziej określonego przesłonięcia, a nie oryginalnej deklaracji. Oznacza to, że autor elementu Assembly3 wyświetli ostrzeżenie o typie `o` niezgodnym i będzie musiał zmienić podpis na następujący, aby wyeliminować: 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

W tym momencie autor Assembly3 ma kilka opcji:

- Mogą zaakceptować/pominąć ostrzeżenie dotyczące `object?` i `object` niezgodności.
- Mogą zaakceptować/pominąć ostrzeżenie dotyczące `object?` i `!` niezgodności.
- Można po prostu usunąć `null` sprawdzanie poprawności (Usuń `!` i przeprowadzić jawne sprawdzanie)

Jest to prawdziwy scenariusz, ale teraz pomysłem jest przechodzenie do przodu z ostrzeżeniem. W przypadku wypróbowania ostrzeżenia zdarza się częściej niż przewidywane, możemy je później usunąć (odwrotnie nie jest prawdziwe).

### <a name="implicit-property-setter-arguments"></a>Niejawne argumenty metody ustawiającej właściwości
`value` argument parametru jest niejawny i nie jest wyświetlany na liście parametrów. Oznacza to, że nie może być elementem docelowym tej funkcji. Składnia metody ustawiającej właściwość może zostać rozszerzona w celu uwzględnienia listy parametrów, aby umożliwić stosowanie operatora `!`. Ale jest to poprzed pomysłem, że ta funkcja sprawia, że `null` sprawdzanie poprawności jest prostsze. Ponieważ niejawny argument `value` po prostu nie będzie działać z tą funkcją.

## <a name="future-considerations"></a>Zagadnienia w przyszłości
None
