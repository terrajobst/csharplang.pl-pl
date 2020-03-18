---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/23/2019
ms.locfileid: "79484945"
---
# <a name="records"></a>rekordy

* [x] proponowane
* [] Prototyp: [ukończono](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)
* [] Implementacja: [w toku](https://github.com/dotnet/roslyn/BRANCH_NAME)
* [] Specyfikacja: zawarta Specyfikacja robocza

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Rekordy są nową, uproszczoną formą deklaracji C# dla typów klas i struktur, które łączą zalety wielu prostszych funkcji. Opisujemy nowe funkcje (parametry obiektu wywołującego i *z*parametrami s), nadaj składnię i semantykę dla deklaracji rekordu, a następnie podaj kilka przykładów.


## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Znacząca liczba deklaracji typu w programie C# jest nieco większa niż zagregowane kolekcje danych o określonym typie. Niestety, zadeklarowanie takich typów wymaga dużej ilości kodu standardowego. *Rekordy* zapewniają mechanizm deklarujący typ danych przez Opisywanie elementów członkowskich agregacji wraz z dodatkowym kodem lub odchyleniami od zwykłych standardowych (jeśli istnieją).

Zobacz [przykłady](#examples)poniżej.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a>Caller — parametry odbiornika

Obecnie *argumentem domyślnym* parametru metody musi być 
- *wyrażenie stałe*; oraz
- wyrażenie `new S()`, gdzie `S` jest typem wartości; oraz
- wyrażenie `default(S)`, gdzie `S` jest typem wartości

Rozszerzającmy ten sposób, aby dodać następujące elementy
- wyrażenie `this.Identifier`

Ten nowy formularz nosi nazwę *domyślnego argumentu Caller-Receiver*i jest dozwolony tylko wtedy, gdy spełnione są wszystkie poniższe warunki
- Metoda, w której występuje, jest metodą wystąpienia; lub
- Wyrażenie `this.Identifier` jest powiązane z elementem członkowskim wystąpienia typu otaczającego, który musi być polem lub właściwością. lub
- Element członkowski, do którego jest powiązany (i metoda dostępu `get`, jeśli jest to właściwość), jest co najmniej tak samo jak metoda; lub
- Typ `this.Identifier` jest niejawnie konwertowany przez tożsamość lub konwersję dopuszczającą wartości null na typ parametru (jest to istniejące ograniczenie dla *argumentu default*).

Gdy argument zostanie pominięty z wywołania elementu członkowskiego funkcji dla odpowiadającego opcjonalnego parametru z *argumentem default-Receiver Caller*, wartość elementu członkowskiego odbiorcy zostanie niejawnie przeniesiona. 

> **Uwagi dotyczące projektowania**: główna przyczyna parametru Caller-Receiver ma obsługiwać *wyrażenie with*. Pomysłem jest to, że można zadeklarować metodę podobną do tej
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> a następnie użyj go jak tego
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> Aby utworzyć nowy `Point` tak jak istniejący `Point`, ale z wartością `X` zmieniony.
> 
> Jest to pytanie otwarte, niezależnie od tego, czy forma składni *wyrażenia with* jest dodawana, gdy mamy obsługę parametrów odbiornika wywołującego, więc jest to możliwe, *zamiast* tego jako *uzupełnienie* *wyrażenia with*.

- [] **Problem otwarty**: co to jest kolejność, w której jest oceniany *argument "odbiornik wywołujący"* , w odniesieniu do innych argumentów? Czy na pewno nie określono go?

### <a name="with-expressions"></a>wyrażenia with

Proponowany jest nowy formularz wyrażenia:

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

Token `with` jest nowym słowem kluczowym zależnym od kontekstu.

Każdy *Identyfikator* po lewej stronie *with_initializer* musi być powiązany z dostępnym polem wystąpienia lub właściwością typu *primary_expression* *with_expression*. Nie mogą istnieć zduplikowane nazwy między tymi identyfikatorami danego *with_expression*.

*With_expression* formularza

> `with` *e1* `{` *Identyfikator* = *e2*,... `}`

jest traktowany jako wywołanie formularza

> *e1*`.With(`*identifier2*`:` *e2*,...`)`

Gdzie, dla każdej metody o nazwie `With`, która jest członkiem dostępnego wystąpienia *E1*, wybieramy *identifier2* jako nazwę pierwszego parametru w tej metodzie, która ma parametr Caller-Receiver, który jest tym samym członkiem, co pole wystąpienia lub właściwość powiązane z *identyfikatorem*. Jeśli ten parametr nie może być zidentyfikowany, ta metoda jest eliminowana z uwagi. Metoda, która ma zostać wywołana, została wybrana spośród pozostałych kandydatów przez rozpoznanie przeciążenia.

> **Uwagi dotyczące projektowania**: określone parametry obiektu wywołującego-Receiver, wiele korzyści z *wyrażenia with —* są dostępne bez tego formularza specjalnej składni. W związku z tym rozważamy, czy jest to konieczne. Jej główną korzyścią jest umożliwienie jednemu programowi w odniesieniu do nazw pól i właściwości, a nie pod względem nazw parametrów. W ten sposób poprawimy zarówno czytelność, jak i jakość narzędzi (np. przejście do definicji w identyfikatorze *with_expression* przejdzie do właściwości, a nie do parametru metody).

- [] **Otwórz problem**: ten opis należy zmodyfikować, aby obsługiwać metody rozszerzenia.

### <a name="pattern-matching"></a>dopasowanie wzorca

Patrz [Specyfikacja dopasowania wzorca](csharp-8.0/patterns.md#positional-pattern) dla specyfikacji `Deconstruct` i jej relacji do dopasowania do wzorca.

> **Uwagi dotyczące projektowania**: na podstawie `Deconstruct` wygenerowanego przez kompilator, zgodnie z opisem w niniejszej umowie, oraz specyfikacji dla deklaracji typu wzorzec-rekord
> ```cs
> public class Point(int X, int Y);
> ```
> Program obsługuje dopasowanie do wzorca pozycyjnego w następujący sposób
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a>deklaracje typu rekordu

Składnia `class` lub `struct` deklaracji jest rozszerzona o obsługę parametrów wartości; parametry są właściwościami typu:

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> **Uwagi dotyczące projektowania**: ponieważ typy rekordów są często przydatne bez potrzeby dla elementów członkowskich zadeklarowanych jawnie w treści klasy, firma Microsoft modyfikuje składnię deklaracji, aby umożliwić jej po prostu średnik.

Klasa (struktura) zadeklarowana z *parametrami Record* jest nazywana *klasą rekordów* (*strukturą rekordów*), z której jest *typem rekordu*.

- [] **Otwórz problem**: musimy uwzględnić *primary_constructor_body* w gramatyce, aby mogła ona występować w deklaracji typu rekordu.
- [] **Otwórz problem**: Jakie są reguły konfliktów nazw dla nazw parametrów? Najprawdopodobniej jeden z nich nie może powodować konfliktu z parametrem typu lub innym *parametrem rekordu*.
- [] **Otwórz problem**: musimy określić zakres parametrów rekordu. Gdzie można ich używać? Najprawdopodobniej w inicjatorach pola wystąpienia i *primary_constructor_body* co najmniej.
- [] **Problem otwarty**: Czy można utworzyć deklarację typu rekordu jako częściowej? Jeśli tak, należy powtórzyć parametry dla każdej części?

#### <a name="members-of-a-record-type"></a>Elementy członkowskie typu rekordu

Oprócz elementów członkowskich zadeklarowanych w *treści klasy*, typ rekordu ma następujące dodatkowe elementy członkowskie:

##### <a name="primary-constructor"></a>Konstruktor podstawowy

Typ rekordu ma Konstruktor `public`, którego sygnatura odnosi się do parametrów wartości deklaracji typu. Jest to nazywane *konstruktorem podstawowym* dla typu i powoduje pominięcie niejawnie zadeklarowanego *konstruktora domyślnego* .

W czasie wykonywania podstawowy Konstruktor

* Inicjuje pola zapasowe generowane przez kompilator dla właściwości odpowiadających parametrom wartości (jeśli te właściwości są udostępniane przez kompilator; [Zobacz 1.1.2](#1.1.2)); następnie
* wykonuje inicjatory pola wystąpienia pojawiające się w *treści klasy*; a następnie
* wywołuje Konstruktor klasy bazowej:
    * Jeśli w *record_base_arguments*znajdują się argumenty, zostanie wywołany Konstruktor podstawowy wybrany przez metodę rozpoznawania przeciążenia z tymi argumentami.
    * W przeciwnym razie Konstruktor podstawowy jest wywoływany bez argumentów.
* wykonuje treść każdej *primary_constructor_body*, jeśli istnieje, w kolejności źródłowej.

- [] **Otwórz problem**: musimy określić tę kolejność, szczególnie w przypadku jednostek kompilacji dla częściowych.
- [] **Otwórz problem**: musimy określić, że każdy jawnie zadeklarowany Konstruktor musi być łańcuchem do głównego konstruktora.
- [] **Otwórz problem**: czy zezwolić na zmianę modyfikatora dostępu dla głównego konstruktora?
- [] **Problem otwarty**: w strukturze rekordów występuje błąd w przypadku braku parametrów rekordu?

##### <a name="primary-constructor-body"></a>Podstawowa treść konstruktora

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

*Primary_constructor_body* może być używana tylko w deklaracji typu rekordu. *Identyfikator* *primary_constructor_body* powinien określać typ rekordu, w którym jest zadeklarowany.

*Primary_constructor_body* nie deklaruje elementu członkowskiego samodzielnie, ale jest sposobem, w jaki programista może dostarczyć atrybuty dla i określić dostęp do obiektu podstawowego konstruktora typu rekordu. Umożliwia także programistom dostarczanie dodatkowego kodu, który zostanie wykonany w przypadku konstruowania wystąpienia typu rekordu.

- [] **Otwórz problem**: należy pamiętać, że Konstruktor domyślny struktury pomija ten element.
- [] **Otwórz problem**: należy określić kolejność wykonywania inicjalizacji.
- [] **Problem otwarty**: czy w deklaracji typu bez rekordu nie ma zezwolenia na *primary_constructor_body* (najprawdopodobniej bez atrybutów i modyfikatorów) i traktuje ją jak kod inicjatora pola wystąpienia?

##### <a name="properties"></a>Właściwości

Dla każdego parametru rekordu deklaracji typu rekordu istnieje odpowiedni element członkowski właściwości `public`, którego nazwa i typ są pobierane z deklaracji parametru wartości. Jego nazwa jest *identyfikatorem* *record_property_name*, jeśli istnieje, lub *identyfikatorem* *record_parameter* w przeciwnym razie. Jeśli żadna konkretna (czyli nieabstrakcyjna) Właściwość publiczna z metodą dostępu `get` i z tą nazwą i typem jest jawnie zadeklarowana lub dziedziczona, jest generowana przez kompilator w następujący sposób:

* Dla *struktury rekordu* lub *klasy rekordu*`sealed`:
 * `private` pole `readonly` jest tworzone jako pole zapasowe dla właściwości `readonly`. Jej wartość jest inicjowana podczas konstruowania z wartością odpowiadającego parametru podstawowego konstruktora.
 * Metoda dostępu `get` właściwości jest zaimplementowana w celu zwrócenia wartości pola zapasowego.
 * Każdy "zgodny" `get` akcesora dziedziczonej właściwości wirtualnej jest zastępowany.

> **Uwagi dotyczące projektowania**: innymi słowy, jeśli rozszerzono klasę bazową lub implementuje interfejs, który deklaruje publiczną właściwość abstrakcyjną o tej samej nazwie i typie jako parametr rekordu, ta właściwość jest zastępowana lub zaimplementowana.

- [] **Problem otwarty**: Czy można zmienić modyfikator dostępu dla właściwości, gdy jest on jawnie zadeklarowany?
- [] **Problem otwarty**: czy istnieje możliwość zastąpienia pola dla właściwości?

##### <a name="object-methods"></a>Metody obiektów

Dla *struktury rekordu* lub *klasy rekordu*`sealed`, implementacje metod `object.GetHashCode()` i `object.Equals(object)` są wytwarzane przez kompilator, chyba że zostaną dostarczone przez użytkownika.

- [] **Otwórz problem**: należy precyzyjnie określić ich implementację.
- [] **Otwórz problem**: należy również dodać `IEquatable<T>` interfejsu dla typu rekordu i określić, że są udostępniane implementacje.
- [] **Otwórz problem**: należy również określić, że implementujemy każdy `IEquatable<T>.Equals`.
- [] **Problem otwarty**: należy określić dokładnie, w jaki sposób rozwiązujemy problem z równą się w celu dziedziczenia rekordów: w tym celu generują metody równości, takie jak symetryczne, przechodnie, zwrotne itd.
- [] **Otwórz problem**: zaproponowano implementację `operator ==` i `operator !=` dla typów rekordów.
- [] **Otwórz problem**: Czy chcesz automatycznie wygenerować implementację `object.ToString`?

##### `Deconstruct`

Typ rekordu ma metodę `public` wygenerowaną przez kompilator `void Deconstruct`, chyba że jest ona dostarczana przez użytkownika. Każdy parametr jest `out` parametr o tej samej nazwie i typie co odpowiedni parametr typu rekordu. Wdrożenie tej metody przez kompilator przypisuje każdy parametr `out` z wartością odpowiedniej właściwości.

Zapoznaj [się ze specyfikacją dopasowania wzorca](csharp-8.0/patterns.md#positional-pattern) dla semantyki `Deconstruct`.

##### <a name="with-method"></a>`With`, Metoda

O ile nie istnieje zadeklarowany przez użytkownika element członkowski o nazwie `With` zadeklarowany, typ rekordu ma metodę dostarczoną przez kompilator o nazwie `With`, której typ zwracany jest samym typem rekordu i zawierający jeden parametr wartości odpowiadający każdemu *parametrowi rekordu* w tej samej kolejności, w której te parametry występują w deklaracji typu rekordu. Każdy parametr musi mieć *domyślny argument Caller* dla odpowiadającej właściwości.

W klasie `abstract` rekordów, dostarczona przez kompilator `With` Metoda jest abstrakcyjna. W strukturze rekordów lub zapieczętowanej klasie rekordów, `With` Metoda udostępniona przez kompilator jest `sealed`. W przeciwnym razie metoda `With` podana przez kompilator to "wirtualna, a jej implementacja zwróci nowe wystąpienie utworzone przez wywołanie konstruktora podstawowego z parametrami jako argumentami, aby utworzyć nowe wystąpienie z parametrów i zwrócić to nowe wystąpienie.

- [] **Otwórz problem**: należy również określić warunki, które zastępują lub implementują dziedziczone metody `With` wirtualnej lub metody `With` z zaimplementowanych interfejsów.
- [] **Otwórz problem**: należy powiedzieć, co się dzieje, gdy odziedziczymy niewirtualną metodę `With`.

> **Uwagi dotyczące projektowania**: ponieważ typy rekordów są domyślnie niezmienne, Metoda `With` zapewnia sposób tworzenia nowego wystąpienia, które jest takie samo jak istniejące wystąpienie, ale z wybranymi właściwościami, które mają nowe wartości. Na przykład podanym
> ```cs
> public class Point(int X, int Y);
> ```
> istnieje element członkowski dostarczony przez kompilator
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> Co umożliwia zmienną typu rekordu
> ```cs
> var p = new Point(3, 4);
> ```
> do zamienienia na wystąpienie, które ma co najmniej jedną właściwość inną
> ```cs
>     p = p.With(X: 5);
> ```
> Można to również wyrazić przy użyciu *with_expression*:
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a>Przykłady

#### <a name="compatibility-of-record-types"></a>Zgodność typów rekordów

Ponieważ programista może dodawać członków do deklaracji typu rekordu, często istnieje możliwość zmiany zestawu elementów rekordu bez wpływu na istniejących klientów. Na przykład, przy użyciu początkowej wersji typu rekordu

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

Nowy element typu rekordu można compatibly dodać w następnej wersji typu bez wpływu na zgodność binarną lub źródłową:

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a>przykład struktury rekordu

Ta struktura rekordu

```cs
public struct Pair(object First, object Second);
```

jest tłumaczony na ten kod

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- [] **Otwórz problem**: czy należy wykonać implementację Equals (para druga) jako publiczną składową pary?
- [] **Otwórz problem**: ta implementacja `Equals` nie jest symetryczna w celu dziedziczenia.
- [] **Problem otwarty**: czy powinien zostać zadeklarować rekord `operator ==` i `operator !=`?

> **Uwagi dotyczące projektowania**: ponieważ jeden typ rekordu może dziedziczyć z innego, a ta implementacja `Equals` nie może być symetryczna w tym przypadku, nie jest poprawna. Proponujemy zaimplementowanie równości w ten sposób:
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> Rekordy pochodne byłyby `override EqualityContract`. Bardziej atrakcyjną alternatywą jest ograniczenie dziedziczenia.

#### <a name="sealed-record-example"></a>przykład zapieczętowanego rekordu

Ta klasa zapieczętowanego rekordu

```cs
public sealed class Student(string Name, decimal Gpa);
```

jest przetłumaczony na ten kod

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a>przykład klasy rekordu abstrakcyjnego

Ta klasa typu abstrakcyjnego

```cs
public abstract class Person(string Name);
```

jest przetłumaczony na ten kod

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a>Łączenie abstrakcyjnych i zapieczętowanych rekordów

Podanej powyżej klasy `Person` rekordów abstrakcyjnych

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

jest przetłumaczony na ten kod

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Podobnie jak w przypadku dowolnej funkcji języka należy zwrócić uwagę na to, czy dodatkowa złożoność tego języka jest zapłacona w dodatkowym przejrzystości oferowanym C# przez funkcję.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Rozważamy Dodawanie *konstruktorów podstawowych* w C# 6. Chociaż zajmują one taką samą powierzchnię syntaktyczną jak ta propozycja, stwierdzamy, że są one niewielkimi korzyściami oferowanymi przez rekordy.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

W treści wniosku są wyświetlane otwarte pytania.

## <a name="design-meetings"></a>Spotkania projektowe

TBD
