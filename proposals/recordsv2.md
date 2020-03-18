---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2019
ms.locfileid: "79485001"
---

# <a name="records-v2"></a>Rekordy w wersji 2

W przeszłości zawarto informacje dotyczące rekordów jako funkcję umożliwiającą pracę z danymi.

"Praca z danymi" jest obszerną grupą z szeregiem aspektów, dzięki czemu może być interesująca, aby zapoznać się z każdą z nich. Zacznijmy od przejrzenia przykładu rekordów dzisiaj i niektórych jego wad.

Na przykład prosty rekord zostanie zdefiniowany dzisiaj w następujący sposób

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

a użycie zostanie odczytane

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

Ten kod ma znaczące zalety:

1. Definicja jest odporna na wersje, ale właściwości można łatwo dodać lub przenieść
2. Właściwości można ustawić w dowolnej kolejności, a nazwy w inicjacji zawsze pasują do metod dostępu
3. Właściwości z wartościami domyślnymi można po prostu pominąć

Pierwsza Luka polega na tym, że właściwości muszą być teraz modyfikowalne.

# <a name="mutability"></a>Zmienność

Co chcemy zrobić, C# aby określić element członkowski `readonly` w inicjatorach obiektów.
Ponieważ niektóre typy mogą nie zostać zaprojektowane z tą inicjalizacją, należy również pamiętać, że jest to konieczne.

Proponowane rozwiązanie jest nowym modyfikatorem, `initonly`, który można zastosować do właściwości i pól:

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

Codegen to Surprisingly proste do przodu: ustawimy pole tylko do odczytu.
W odróżnieniu od właściwości obniżone będą wyglądać:

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

Środowisko CLR uważa, że pola tylko do odczytu nie mogą być możliwe do sprawdzenia, ale nie są niebezpieczne. W celu zapewnienia bardziej zaawansowanego weryfikatora proponowana jest następująca reguła: pole tylko do odczytu może być modyfikowane tylko wewnątrz metod `initonly` lub na nowym obiekcie, który znajduje się na stosie CLR i nie został opublikowany za pośrednictwem magazynu lub wywołania metody.

Powinno to rozwiązać wiele problemów z zmienność w przykładzie `UserInfo`, bez konieczności wykonywania skomplikowanych lub kruchyych strategii emisji. Jednak niezmienności jest obecnym nowym problemem: łatwe Konstruowanie obiektu ze zmianami.

# <a name="with-ing"></a>Z-usługą

Podczas programowania przy użyciu niezmienności wprowadzanie zmian w obiekcie odbywa się przez konstruowanie kopii ze zmianami zamiast wprowadzania zmian bezpośrednio w obiekcie. Niestety, nie istnieje wygodny sposób wykonania tej czynności w programie C#, nawet w przypadku rekordów w stylu bieżącym. Wcześniej zaproponowano, aby dla rekordów, które implementują tę funkcję, był dostarczany jakiś rodzaj automatycznie generowanej metody with. Jeśli mamy taki mechanizm, ważne jest, aby współpracował z `initonly` członkami. Aby to osiągnąć, proponowane jest dodanie wyrażenia `with`, analogiczne do inicjatora obiektu. Przykładowe użycie będzie następujące:

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

Otrzymany `newUserName` obiekt będzie kopią `userInfo`, z `Username`em ustawionym na "angocke".
Codegen w wyrażeniu `with` będzie również podobna do inicjatora obiektów: tworzony jest nowy obiekt, a następnie `initonly` `Username` setter zostanie wywołana w treści metody.

Oczywiście różnica polega na tym, że tworzony nowy obiekt nie jest prostym tworzeniem nowego obiektu, jest duplikatem oryginalnego obiektu. Aby zapewnić tę funkcjonalność, wymagane jest, aby obiekt zapewniał "z konstruktorem", który zawiera zduplikowany obiekt. Przykładowy Konstruktor `With` wyglądać następująco:

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

W szczególności, wyrażenie `with` ustawi `initonly` elementy członkowskie, podobnie jak Inicjator obiektu, dlatego do obsługi weryfikacji musimy upewnić się, że nie można opublikować obiektu przed ustawieniem elementów członkowskich `initonly`. Aby to wymusić, atrybut `WithConstructor` (lub odpowiadająca składnia) wymusza nową regułę dla metody: wszystkie instrukcje Return muszą bezpośrednio zawierać wyrażenie tworzenia obiektu, prawdopodobnie z inicjatorem obiektu.

Jeśli Konstruktor `With` wymaga walidacji, użytkownik może wprowadzić konstruktora w celu wykonania tej walidacji, np.

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

Ostatnim elementem złożoności skojarzonym z `With` jest dziedziczenie. Jeśli rekord jest rozszerzalny, należy podać nowy `With` dla podklasy. Można to osiągnąć w następujący sposób:

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

Zwróć uwagę na jedną dodatkową część złożoności tutaj: Aby przesłonić Konstruktor `With` z typem pochodnym, język będzie również obsługiwał typy zwracane przez współwarianty w zastosowaniach.
Ta [Funkcja już](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)istnieje.

**Wady**

- Wprowadzanie wszystkich instrukcji return w `WithConstructor`s zawiera nowe wyrażenia obiektu jest restrykcyjne.
  Może to być możliwe do skorygowania przez analizę przepływu, która zapewnia, że nowy obiekt nie będzie miał metody
- Obsługa wariancji w zastąpieniach za pomocą wskazówek kompilatora będzie wymagała metod zastępczych, co zwiększy się z głębokością dziedziczenia. Konieczność metody zastępczej jest spowodowana wymaganiem w czasie wykonywania, które zastępuje sygnatury dokładnie zgodne. W przypadku poluzu wymagań środowiska uruchomieniowego metody zastępcze nie będą w ogóle wymagane.
- Używanie łańcuchowych konstruktorów formularza `Type(Type original)` efektywnie rezerwuje ten konstruktor do użycia wzorca. Ponieważ konstruktory mają unikatową semantykę i nie można jej zmienić nazwy, może to być ograniczone.


## <a name="wrapping-it-all-up-records"></a>Zawijaj wszystko: rekordy

Powyższe funkcje umożliwiają zastosowanie stylu programowania, który był bardzo trudny. Jednak nawet w przypadku nowych funkcji może być całkiem pełny i podatne na dodawanie adnotacji do wszystkiego. Istnieje również kilka elementów, takich jak Equals i GetHashCode, które można już dzisiaj napisać, to właśnie pracochłonne.
Ponadto znaczący wpływ na implementację tych nowych elementów pierwotnych polega na tym, że strukturalna równość jest taka, która powinna być zmieniana wraz z typem danych w miarę dodawania nowych danych, ale podczas ich obsługi ręcznie jest możliwe, że te rzeczy będą mogły zostać zsynchronizowane.

W związku z tym jest proponowana C# obsługa nowej składni dla rekordów, a nie do udostępniania nowych funkcji, ale do ustawiania ustawień domyślnych i generowania kodu przeznaczonego do użycia w rekordach. Przykładowa składnia będzie wyglądać następująco

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

Wygenerowany kod dla tej klasy będzie uwzględniał wszystkie pola publiczne i właściwości automatycznie jako strukturalne elementy członkowskie rekordu. Elementy członkowskie rekordu można dostosować przy użyciu nowego atrybutu `RecordMember(bool)`, który może być używany do dołączania lub wykluczania elementów członkowskich. Elementy członkowskie rekordu będą `initonly` domyślnie i równość zostałyby wygenerowane automatycznie dla klasy na podstawie rekordów elementów członkowskich. W każdym momencie zachowanie tych elementów członkowskich może być dostosowane po prostu przez zadeklarowanie ich w źródle. Implementacja oparta na użytkowniku zastąpi domyślną implementację we wszystkich użyciach wzorców.

Należy pamiętać, że równość w przypadku dziedziczenia jest złożona, ale wydaje się, że zostały one odpowiednio rozwiązane w [innych propozycjach rekordów](records.md).

## <a name="primary-constructors"></a>Konstruktory podstawowe

Poprzednia pozycja rekordu zawierała również nową składnię dla listy parametrów w samym typie, np.

```C#
class Point(int X, int Y);
```

W nowym projekcie lista parametrów będzie funkcją ortogonalną C# , którą można czysto zintegrować z rekordami. Jeśli w rekordzie znajduje się Konstruktor podstawowy, będzie on miał nowe wartości domyślne, podobnie jak pola publiczne i właściwości automatyczne: parametry w konstruktorze podstawowym będą używane do generowania właściwości publicznego rekordu-elementu członkowskiego o tej samej nazwie. Ponadto Konstruktor podstawowy może być teraz używany do generowania automatycznie dekonstruktora.

Na przykład następujący rekord z konstruktorem podstawowym

```C#
data class Point(int X, int Y);
```

będzie równoważne

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

a końcowe generowanie powyższego poziomu byłyby

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

Należy zwrócić uwagę na to, że w przypadku klasy danych z konstruktorem podstawowym została pobrana jedna inna informacja na temat: zamiast ustawiania pól podstawowych wewnątrz wygenerowanego konstruktora chronionego, delegat do konstruktora podstawowego. Jeśli Klasa Point ma inny niepodstawowy element członkowski rekordu, np.

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

następnie spowoduje to zmianę wygenerowanego chronionego konstruktora w następujący sposób:

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

W szczególności nie odpowiada to na temat dziedziczenia rekordów za pomocą konstruktorów podstawowych. Na przykład,

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

Zamiast rozwiązywania w dowolny sposób, bardziej jawne podejście może wymagać, aby lista parametrów była dostarczana z listą podstawową, np.

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

Lista parametrów na liście podstawowej zostałaby następnie zastosowana do wywołania `base` w wygenerowanym konstruktorze głównym:

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

Tak jak w przypadku konstruktora podstawowego może oznaczać poza rekordem, który jest nadal otwarty do dalszej propozycji.
