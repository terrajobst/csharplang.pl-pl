---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2019
ms.locfileid: "79484973"
---
# <a name="primary-constructors"></a>Konstruktory podstawowe

* [x] proponowane
* [] Prototyp: nie uruchomiono
* [] Implementacja: nie uruchomiono
* [] Specyfikacja: nie uruchomiono

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Klasy mogą mieć listę parametrów, a kiedy tak, ich Specyfikacja klasy bazowej może mieć listę argumentów.
Parametry konstruktora podstawowego są w zakresie w całej deklaracji klasy, a jeśli są przechwytywane przez składową funkcji lub funkcję anonimową, są one przechowywane jako pola prywatne w klasie.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Często istnieje wiele standardowych kodów inicjalizacji programu. W ogólnym przypadku dana część danych `x` jest określona wiele razy:

- Jako pole prywatne `_x`
- Jako parametr `x` konstruktora
- W `_x = x;` przypisania pola z parametru w konstruktorze
- Jako `X` właściwości
- W `x = value;` setter właściwości
- W `return x;` metody pobierającej właściwości

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

W przypadku właściwości, które nie wymagają weryfikacji ani obliczeń, monotonnych czynności można zmniejszyć przy użyciu właściwości autoproperties, a tym samym wycofać potrzebę deklarowania jawnego pola zapasowego dla właściwości. Ale jeśli właściwość wymaga żadnych rodzajów logiki wykraczających poza wartość właściwości autoproperty, powyższym jest najlepszy użytkownik.

Konstruktory podstawowe zamiast tego zmniejszają obciążenie przez umieszczenie argumentów konstruktora bezpośrednio w zakresie w całej klasie, co eliminuje ponownie, aby jawnie zadeklarować pole zapasowe. W ten sposób powyższy przykład:

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

W tym przykładzie Konstruktor podstawowy zmniejsza liczbę nazwanych jednostek dla `x` z trzech do dwóch, co eliminuje pole `_x` zapasowe. Usuwa dwie z trzech deklaracji składowych (tylko deklaracje właściwości) i zmniejsza łączną liczbę wzmianki o `x`/`_x`/`X` z ośmiu do pięciu.


## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Klasy mogą mieć listę parametrów:

``` c#
public class C(int i, string s)
{
    ...
}
```

Lista parametrów powoduje, że Konstruktor ma być niejawnie zadeklarowany dla klasy, z taką samą dostępnością jak sama klasa.

``` c#
new C(5, "Hello");
```

Parametry konstruktora podstawowego są w zakresie w całej treści klasy. Jeśli są przechwytywane przez element członkowski funkcji lub funkcję anonimową, stają się one przechowywane jako pola prywatne w klasie. Jeśli są używane tylko podczas inicjowania, nie będą przechowywane w obiekcie.

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

Jeśli Klasa z konstruktorem podstawowym ma specyfikację klasy bazowej, która może mieć listę argumentów. Służy to jako lista argumentów dla inicjatora `base(...)` niejawnie zadeklarowanego konstruktora. Jeśli nie podano listy argumentów, przyjmuje się, że jest ona pusta.

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
Klasa może mieć również jawnie zdefiniowane konstruktory, ale te wszystkie muszą używać inicjatora `this(...)`. Dzięki temu Konstruktor podstawowy jest zawsze wywoływany, gdy zostanie skonstruowane nowe wystąpienie.

Wszystkie inicjatory w treści klasy staną się przypisaniami w wygenerowanym konstruktorze. Oznacza to, że w przeciwieństwie do innych klas inicjatory zostaną uruchomione *po* wywołaniu konstruktora podstawowego, a nie przed. Ponadto wygenerowana Klasa będzie zawierać przypisania umożliwiające zainicjowanie wszelkich prywatnych pól, które zostały wygenerowane w celu przechowywania parametrów konstruktora podstawowego, które zostały przechwycone przez elementy członkowskie. Te elementy członkowskie są ponownie zapisywane w celu użycia pola private zamiast parametru w sposób podobny do zamknięć dla wyrażeń lambda. Wygenerowane pola podstawowe są inicjowane jako pierwsze, a następnie przypisania generowane przez inicjatora są wykonywane w kolejności występowania w klasie.

Dla powyższego przykładu efekt deklaracji klasy jest tak, jakby został zapisany w następujący sposób:

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

Przechwytywanie ma podobne ograniczenia dotyczące przechwytywania zmiennych lokalnych w wyrażeniach lambda. Na przykład parametry `ref` i `out` są dozwolone w konstruktorach podstawowych, ale nie mogą być przechwytywane przez te jednostki członkowskie.


## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

W przybliżeniu istotny porządek.

* Propozycja używa składni, która została również zaproponowana dla rekordów pozycyjnych. Jeśli potrzebujemy obu funkcji, wymagane jest pewne zakwaterowanie. Na przykład zaproponowano modyfikator `data` dla rekordów.
* Rozmiar alokacji skonstruowanych obiektów jest mniej oczywisty, ponieważ kompilator określa, czy przydzielić pole dla podstawowego parametru konstruktora na podstawie pełnego tekstu klasy. To ryzyko jest podobne do niejawnego przechwytywania zmiennych przez wyrażenia lambda.
* Typowy Temptation (lub przypadkowe) może być przechwyceniem parametru "ten sam" na wielu poziomach dziedziczenia, ponieważ jest ona przenoszona do łańcucha konstruktorów, a nie jawnie allotting to pole chronione w klasie bazowej, co prowadzi do duplikowania alokacji dla tych samych danych w obiektach. Jest to bardzo podobne do współczesnego ryzyka przesłaniania autowłaściwości przy użyciu właściwości autoproperties. 
* Jak wspomniano powyżej, nie ma miejsca dla dodatkowej logiki, która może być zwykle wyrażona w treści konstruktora. Poniższe rozszerzenie "podstawowe konstruktory" zawiera adresy.
* Zgodnie z proponowaną semantyką kolejności wykonywania jest nieco inne niż w przypadku zwykłych konstruktorów. Może to być możliwe do zaradzić, ale kosztem niektórych propozycji rozszerzenia (szczególnie "podstawowych jednostek konstruktorów").
* Propozycja działa tylko wtedy, gdy jeden konstruktor może być wyznaczony jako podstawowy.
* Nie ma żadnego sposobu, aby mieć osobną dostępność klasy i konstruktora podstawowego. Na przykład w sytuacjach, w których konstruktory publiczne wszystkie Delegaty do jednego prywatnego konstruktora "Kompiluj-it-all", które byłyby konieczne. W razie potrzeby można zaproponować składnię w późniejszym czasie.


## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Pełne rekordy pozycyjne mogą być alternatywą lub mogą współistnieć z konstruktorami podstawowymi, w zależności od określonych. Mogą one mieć *więcej* skrótów w *mniejszej* liczbie scenariuszy. Dlatego obydwie są przydatne, ale mogą być zbyt obszernee, chyba że mogą one być ze sobą starannie zintegrowane.


## <a name="possible-extensions"></a>Możliwe rozszerzenia
[extensions]: #possible-extensions

Są to odmiany lub dodatki do podstawowej propozycji, które mogą być rozważane razem z nią lub na późniejszym etapie, jeśli uznają, że są użyteczne.

### <a name="primary-constructor-bodies"></a>Podstawowe treści konstruktora

Konstruktory często zawierają logikę sprawdzania poprawności parametrów lub inny nieuproszczony kod inicjujący, który nie może być wyrażony jako inicjatory.

Konstruktory podstawowe można rozszerzyć, aby umożliwić umieszczanie bloków instrukcji bezpośrednio w treści klasy. Te instrukcje zostaną wstawione w wygenerowanym konstruktorze w punkcie, w którym pojawiają się między inicjalizacją przypisania i dlatego zostałyby wykonane z inicjatorami. Przykład:

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a>Pola inicjatora i funkcje inicjatora

W klasie z konstruktorem podstawowym możemy rozważyć użycie deklaracji pola i metody bez modyfikatorów dostępności, aby były bardziej podobne do zmiennych lokalnych i funkcji lokalnych:

* Podobnie jak w przypadku parametrów konstruktora podstawowego "pola inicjatora" byłyby przechwytywane tylko do rzeczywistego pola prywatnego, jeśli były używane w elementach członkowskich funkcji.
* "Funkcje inicjatora" będą brane pod uwagę tylko w przypadku przechwytywania parametrów konstruktora podstawowego i pól inicjatora, jeśli były używane w innych elementach członkowskich funkcji. Jeśli nie przechwycono, mogą one być generowane w sposób bardziej optymalny, taki jak funkcje lokalne.
* Podobnie jak parametry konstruktora podstawowego nie są dostępne za pośrednictwem dostępu do elementów członkowskich, ale tylko jako nazwa prosta.

Ta funkcja może być używana w przypadku zmiennych tymczasowych i funkcji pomocnika, które mają zastosowanie tylko do inicjowania:

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

Może być zbyt delikatny, szczególnie ponieważ brak modyfikatorów dostępności w innym miejscu oznacza `private`. 

### <a name="initializer-statements"></a>Instrukcje inicjatora

Główną kombinacją powyższych elementów do rozszerzeń byłoby po prostu umożliwienie instrukcji bezpośrednio w treści klasy. Takie instrukcje są dokładnie takie jak odłożone treści konstruktorów proponowane powyżej, z tym wyjątkiem, że nie muszą być ujęte w `{ }`. Aby to było dostatecznie przydatne, zmienne "Local" i funkcje pomocnika również muszą być można wyrazić elementu na najwyższym poziomie klasy, w sposób opisany w rozszerzeniu "pola inicjatora i funkcje inicjatora" powyżej.


### <a name="member-access"></a>Dostęp do elementu członkowskiego

Podstawowa propozycja traktuje podstawowe parametry konstruktora jako parametry, które można określać tylko jako proste nazwy. Alternatywą jest umożliwienie odwoływania się do nich tak, jakby były polami prywatnymi, tj. z dostępem do elementu członkowskiego, *nawet* Jeśli nie są czasami generowane jako pola. Umożliwi to odwoływanie się do nich jako `this.x`, gdy są one cieniowane przez zmienne lokalne i dostępne z innego wystąpienia jako `other.x`.

Jeśli zastosowano do rozszerzenia "pola inicjatora i funkcje inicjatora", to również zmniejszy stopień, w jakim różnią się od zwykłych prywatnych członków. Jedyną różnicą jest to, że kompilator jest bezpłatny, aby Elide je z obiektu, jeśli jest używany tylko podczas inicjacji.

