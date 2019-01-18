---
ms.openlocfilehash: 72d17175dfb8ef284dce6cf7e5837420fa06f16a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229928"
---
# <a name="structs"></a>Struktury

Struktury są podobne do klasy, w tym, że reprezentują one struktur danych, które mogą zawierać elementy członkowskie danych i funkcji elementów członkowskich. W przeciwieństwie do klasy, struktury są typami wartości i nie wymagają alokacji sterty. Zmienną typu struktury bezpośrednio zawiera dane struktury, zmienna typu klasy zawiera odwołanie do danych, a ostatni znany jako obiekt.

Struktury są szczególnie przydatne w przypadku małych strukturach danych, które mają semantyki wartości. Liczby zespolone, punkty w układzie współrzędnych lub par klucz wartość ze słownika są dobrym przykładem struktury. Klucz do tych struktur danych jest kilka elementów członkowskich danych, że nie wymagają użycia dziedziczenie lub tożsamości referencyjnej i że można je łatwo implementować przy użyciu semantyki wartości lokalizacji przypisania kopiowania wartości zamiast odwołania.

Zgodnie z opisem w [typów prostych](types.md#simple-types), typy proste dostarczana przez C#, takie jak `int`, `double`, i `bool`, są w rzeczywistości wszystkie typy struktury. Tak, jak te wstępnie zdefiniowanych typów są strukturami, użytkownik może również używać struktury i przeładowania operatora do wdrożenia nowych typów "pierwotne" w języku C#. Dwa przykłady takich typów znajdują się na końcu tego rozdziału ([przykłady struktury](structs.md#struct-examples)).

## <a name="struct-declarations"></a>Deklaracje struktury

A *struct_declaration* jest *type_declaration* ([wpisz deklaracje](namespaces.md#type-declarations)) określa nowa struktura:

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

A *struct_declaration* zawiera opcjonalny zestaw *atrybuty* ([atrybuty](attributes.md)), a następnie opcjonalny zestaw *struct_modifier*s ([Modyfikatory struktury](structs.md#struct-modifiers)), a następnie opcjonalnie `partial` modyfikator, następuje słowa kluczowego `struct` i *identyfikator* nazwy, struktury, a następnie Opcjonalnie *type_parameter_list* specyfikacji ([parametry typu](classes.md#type-parameters)), a następnie opcjonalnie *struct_interfaces* specyfikacji ([Modyfikatora "partial"](structs.md#partial-modifier))), a następnie opcjonalnie *type_parameter_constraints_clause*specyfikacji s ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), a następnie *struct_body* ([treści struktury](structs.md#struct-body)), opcjonalnie następuje średnik.

### <a name="struct-modifiers"></a>Modyfikatory — struktura

A *struct_declaration* może opcjonalnie obejmować sekwencję Modyfikatory struktury:

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

Jest to błąd czasu kompilacji dla tego samego modyfikator pojawią się wiele razy w deklaracji struktury.

Modyfikatory deklaracji struktury mają takie samo znaczenie jak deklaracji klasy ([klasy deklaracje](classes.md#class-declarations)).

### <a name="partial-modifier"></a>Modyfikatora "Partial"

`partial` Modyfikator wskazuje, że to *struct_declaration* jest deklaracją typu częściowego. Wielu deklaracjach częściowej struktury o takiej samej nazwie w deklaracji otaczającego przestrzeń nazw lub typ są łączone w celu utworzenia jednej deklaracji struktury, zgodnie z regułami określone w [typów częściowych](classes.md#partial-types).

### <a name="struct-interfaces"></a>Interfejsy — struktura

Deklaracja struktury mogą obejmować *struct_interfaces* specyfikacji, w których przypadku struktury jest nazywany bezpośrednio zaimplementować typy danego interfejsu.

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

Implementacje interfejsu są omówione w dalszych [implementacje interfejsu](interfaces.md#interface-implementations).

### <a name="struct-body"></a>Treść — struktura

*Struct_body* struktury definiuje elementy członkowskie struktury.

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>Składowe struktury

Elementy członkowskie struktury składają się z elementów członkowskich, wynikające z jego *struct_member_declaration*s i elementy członkowskie są dziedziczone z typu `System.ValueType`.

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

Z wyjątkiem różnic, o których wspomniano w [klasy i struktury różnice](structs.md#class-and-struct-differences), opisy elementów członkowskich klasy w [elementy członkowskie klasy](classes.md#class-members) za pośrednictwem [Iteratory](classes.md#iterators) dotyczą — struktura członków, a także.

## <a name="class-and-struct-differences"></a>Klasy i struktury różnice

Struktury różnią się od klas w kilka istotnych różnic:

*  Struktury są typami wartości ([wartość semantyki](structs.md#value-semantics)).
*  Wszystkie typy struktury niejawnie dziedziczą z klasy `System.ValueType` ([dziedziczenia](structs.md#inheritance)).
*  Przypisanie do zmiennej typu struktury tworzona jest kopia wartości przypisywane ([przypisania](structs.md#assignment)).
*  Wartość domyślna struktury to wartości generowane przez ustawienie dla wszystkich pola typu wartości przywrócić wartości domyślne i wszystkie odwołania pola typu do `null` ([wartości domyślne](structs.md#default-values)).
*  Operacje pakowania, jak i rozpakowywanie są używane do konwersji między typem struktury i `object` ([pakowania i rozpakowywania](structs.md#boxing-and-unboxing)).
*  Znaczenie `this` różni się dla struktur ([dostęp](expressions.md#this-access)).
*  Deklaracje pola wystąpienia struktury nie mogą zawierać inicjatory zmiennej ([pola inicjatory](structs.md#field-initializers)).
*  Struktura nie ma zezwolenia na Zadeklaruj Konstruktor wystąpienia bez parametrów ([konstruktory](structs.md#constructors)).
*  Struktura nie jest dozwolone było zadeklarowanie destruktora ([destruktory](structs.md#destructors)).

### <a name="value-semantics"></a>Semantyka wartości

Struktury są typami wartości ([typy wartości](types.md#value-types)) i są określane jako ma wartość semantyki. Klasy, z drugiej strony, są typami odwołań ([typy odwołań](types.md#reference-types)) i jest nazywany ma semantyką odwołań.

Zmienną typu struktury bezpośrednio zawiera dane struktury, zmienna typu klasy zawiera odwołanie do danych, a ostatni znany jako obiekt. Gdy struktura `B` zawiera pole wystąpienia typu `A` i `A` jest typem struktury jest to błąd czasu kompilacji dla `A` zależą `B` lub typem skonstruowany na podstawie `B`. Struktura `X` ***zależy od bezpośrednio*** struktury `Y` Jeśli `X` zawiera pole wystąpienia typu `Y`. Biorąc pod uwagę tę definicję, kompletny zestaw struktury, od których zależy struktury jest przechodnia zamknięcia ***zależy od bezpośrednio*** relacji.  Na przykład
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
jest to błąd, ponieważ `Node` zawiera swój własny typ pola wystąpienia.  Inny przykład
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
jest to błąd, ponieważ każdy z typów `A`, `B`, i `C` są zależne od siebie nawzajem.

W przypadku klas jest możliwe w dwóch zmiennych odwoływać się do tego samego obiektu, a zatem możliwe dla operacji na jednej zmiennej miały wpływ na obiekt odwołuje się druga zmienna. Przy użyciu struktury, zmienne każdego mają własne kopii danych (z wyjątkiem w przypadku właściwości `ref` i `out` zmiennych parametrów), i nie jest możliwe dla operacji na jednym wpłynie na inne. Ponadto, ponieważ struktury nie są typami odwołań, nie jest możliwe w przypadku wartości typu struktury jako `null`.

Biorąc pod uwagę deklaracji
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
fragment kodu
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
Wyświetla wartość `10`. Przypisywanie `a` do `b` tworzona jest kopia wartości, a `b` związku z tym jest niezależny od przypisania do `a.x`. Gdyby `Point` zamiast tego zostało zadeklarowane jako klasa, dane wyjściowe będą `100` ponieważ `a` i `b` będzie odwoływać się do tego samego obiektu.

### <a name="inheritance"></a>Dziedziczenie

Wszystkie typy struktury niejawnie dziedziczą z klasy `System.ValueType`, która z kolei dziedziczy z klasy `object`. Deklaracja struktura może określić listy implementowanych interfejsów, ale nie jest możliwe dla deklaracji struktury określić klasę bazową.

Typy struktury nigdy nie są abstrakcyjne i zawsze niejawnie są zamknięte. `abstract` i `sealed` Modyfikatory w związku z tym są niedozwolone w deklaracji struktury.

Ponieważ dziedziczenie nie jest obsługiwane w przypadku struktur, nie może być deklarowana dostępność metody składowej struktury `protected` lub `protected internal`.

Funkcji składowych w strukturze nie może być `abstract` lub `virtual`i `override` modyfikator jest dozwolony tylko po to, aby zastąpić metod odziedziczone `System.ValueType`.

### <a name="assignment"></a>Przypisanie

Przypisanie do zmiennej typu struktury tworzona jest kopia wartości jest przypisane. To różni się od przypisania do zmiennej typu klasy, która kopiuje odwołania, ale nie identyfikowane przez odwołanie do obiektu.

Podobnie jak przypisanie, gdy struktury jest przekazywany jako parametr wartość lub zwrócone w wyniku funkcji elementu członkowskiego, tworzona jest kopia struktury. Struktury mogą być przekazywane przez odwołanie do funkcji elementu członkowskiego, używając `ref` lub `out` parametru.

Gdy właściwość lub indeksator struktury jest elementem docelowym przypisania, związane z dostępem do właściwości lub indeksatora wyrażenia do wystąpienia muszą być klasyfikowane jako zmienną. Jeśli wyrażenie wystąpienia jest klasyfikowana jako wartość, wystąpi błąd kompilacji. Jest to opisane w dalszej części [przypisanie proste](expressions.md#simple-assignment).

### <a name="default-values"></a>Wartości domyślne

Zgodnie z opisem w [wartości domyślne](variables.md#default-values), kilka rodzajów zmienne są automatycznie inicjowane z wartością przywrócić wartości domyślne po ich utworzeniu. Dla zmiennych typu klasy i inne typy odwołań, ta wartość domyślna to `null`. Jednak ponieważ struktury są typami wartości, które nie może być `null`, wartością domyślną struktury jest wartością, generowane przez ustawienie dla wszystkich pola typu wartości przywrócić wartości domyślne i wszystkie odwołania pola typu do `null`.

Odwołujące się do `Point` struktury zadeklarowane powyżej przykładzie
```csharp
Point[] a = new Point[100];
```
Inicjuje każdy `Point` w tablicy wartości generowane przez ustawienie `x` i `y` pola na wartość zero.

Wartość domyślna struktury odnosi się do wartości zwracanej przez domyślny konstruktor obiektu struktury ([domyślne konstruktory](types.md#default-constructors)). W odróżnieniu od klasy struktury nie jest dozwolone Zadeklaruj Konstruktor wystąpienia bez parametrów. Zamiast tego, co struktura niejawnie ma Konstruktor wystąpienia bez parametrów, która zawsze zwraca wartość, która wynika z ustawienia wszystkie pola typu wartości, aby przywrócić wartości domyślne i wszystkie odwołania pola typu do `null`.

Struktury, powinny być zaprojektowane należy wziąć pod uwagę domyślny stan inicjowania nieprawidłowym stanie. W przykładzie
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
Konstruktor zdefiniowany przez użytkownika wystąpień chroni przed wartości null, tylko gdy jest jawnie wywoływana. W przypadkach, gdzie `KeyValuePair` zmiennej podlega inicjowania wartości domyślne, `key` i `value` pola będą miały wartość null, a struktura musi być przygotowana do obsługi tego stanu.

### <a name="boxing-and-unboxing"></a>Konwersja boxing i konwersja unboxing

Wartość typu klasy może być konwertowany na typ `object` lub do typu interfejsu, który jest implementowany przez klasę, po prostu, traktując odwołanie jako inny typ w czasie kompilacji. Podobnie, wartość typu `object` lub wartość typu interfejsu można przekonwertować do typu klasy bez zmiany odwołania (ale oczywiście wyboru typu run-time jest wymagana w tym przypadku).

Ponieważ struktury nie są typami odwołań, te operacje są implementowane w inny sposób dla typów struktury. Gdy wartość typu struktury jest konwertowany na typ `object` lub do typu interfejsu, który jest implementowany przez strukturę, odbywa się operacją pakowania. Podobnie gdy wartość typu `object` lub wartości typu interfejsu, jest konwertowany do typu struktury, odbywa się operacja rozpakowania. Najważniejszą różnicą z tej samej operacji na typy klas jest, czy konwersja boxing i konwersja unboxing kopiuje wartość struktury z wystąpienia programu prostokątnych ani do. W związku z tym po operacji pakowania i rozpakowywanie zmiany wprowadzone do rozpakowanych struktury nie są odzwierciedlane w strukturze opakowanej.

Kiedy typ struktury zastępuje metody wirtualnej odziedziczone `System.Object` (takie jak `Equals`, `GetHashCode`, lub `ToString`), wywołanie metody wirtualnej za pomocą wystąpienia typu struktury nie powoduje pakowania wystąpić. Ta zasada obowiązuje, nawet wtedy, gdy struktury jest używany jako parametr typu i wywołania odbywa się przez wystąpienie typu typu parametru. Na przykład:
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

Dane wyjściowe programu są:
```
1
2
3
```

Mimo że jest nieprawidłowy dla `ToString` ma skutki uboczne, w przykładzie pokazano, opakowywanie nie wystąpił trzech wywołań `x.ToString()`.

Podobnie pakowanie nigdy niejawnie występuje, gdy dostęp do składowej w parametrze typu ograniczone. Na przykład, załóżmy, że interfejs `ICounter` zawiera metodę `Increment` który może służyć do modyfikowania wartości. Jeśli `ICounter` jest używany jako ograniczenie, implementacja `Increment` metoda jest wywoływana z odwołaniem do zmiennej, `Increment` została wywołana na nigdy nie ramce kopii.

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

Pierwsze wywołanie `Increment` Modyfikuje wartość w zmiennej `x`. Nie jest to równoważne drugie wywołanie `Increment`, który modyfikuje wartości w ramce kopię `x`. W związku z tym dane wyjściowe programu jest:
```
0
1
1
```

Aby uzyskać dodatkowe szczegóły dotyczące pakowania i rozpakowywania, zobacz [pakowania i rozpakowywania](types.md#boxing-and-unboxing).

### <a name="meaning-of-this"></a>Znaczenie tego

W ramach konstruktora wystąpienia lub funkcja składowa klasy `this` zostanie sklasyfikowany jako wartość. W związku z tym, podczas gdy `this` może służyć do odwoływania się do wystąpienia dla którego funkcja elementu członkowskiego wywołano, nie jest możliwe do przypisania do `this` w funkcji składowej klasy.

W konstruktorze wystąpienia struktury `this` odpowiada `out` parametr typu struktury, a także wewnątrz funkcji składowej wystąpienia struktury, `this` odpowiada `ref` parametr typu struktury. W obu przypadkach `this` zostanie sklasyfikowany jako zmienną, i można modyfikować na całej strukturze, dla której wywołano element członkowski funkcji przez przypisywanie `this` lub przez przekazanie jako `ref` lub `out` parametru.

### <a name="field-initializers"></a>Inicjatory pola

Zgodnie z opisem w [wartości domyślne](structs.md#default-values), wartością domyślną struktury składa się z wartość, która wynika z ustawienia wszystkie pola typu wartości, aby przywrócić wartości domyślne i wszystkie odwołania pola typu do `null`. Z tego powodu struktury nie zezwala na wystąpienia deklaracji pól, aby uwzględnić zmienną inicjatory. To ograniczenie dotyczy tylko pola wystąpień. Pola statyczne struktury mogą zawierać inicjatory zmiennej.

Przykład
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
jest w wyniku błędu, ponieważ deklaracji pól wystąpienia zawierać inicjatory zmiennej.

### <a name="constructors"></a>Konstruktorów

W odróżnieniu od klasy struktury nie jest dozwolone Zadeklaruj Konstruktor wystąpienia bez parametrów. Zamiast tego, co struktura niejawnie ma Konstruktor wystąpienia bez parametrów, która zawsze zwraca wartość, która wynika z ustawienia wszystkie pola typu wartości, aby przywrócić wartości domyślne i wszystkie odwołania pola typu do wartości null ([domyślne konstruktory](types.md#default-constructors)). Struktury można zadeklarować konstruktorów wystąpienia o parametry. Na przykład
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

Podane powyżej deklaracji, instrukcje
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
Obie umożliwiają tworzenie `Point` z `x` i `y` inicjowane od zera.

Konstruktora wystąpienia struktury nie może zawierać inicjatora konstruktora formularza `base(...)`.

Jeśli konstruktora wystąpienia struktury nie określono inicjatora konstruktora `this` odnosi się zmienna `out` parametr typu struktury i podobnie jak `out` parametru `this` musi zostać zdecydowanie przypisany ( [Asercję określonego przypisania](variables.md#definite-assignment)) w każdej lokalizacji, gdzie Konstruktor zwraca. Jeśli Konstruktor wystąpienia struktury określa inicjatora konstruktora `this` odpowiada zmiennej `ref` parametr typu struktury i podobnie jak `ref` parametru `this` jest uznawany za zdecydowanie przypisany na wpis w treści konstruktora. Należy wziąć pod uwagę implementacji konstruktora wystąpienia poniżej:
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

Nie funkcję składową wystąpienia (łącznie z metody dostępu set dla właściwości `X` i `Y`) może być wywoływany, dopóki wszystkie pola struktury budowany został zdecydowanie przypisany. Jedynym wyjątkiem obejmuje automatycznie implementowanych właściwości ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)). Reguły asercję określonego przypisania ([przypisanie proste wyrażenia](variables.md#simple-assignment-expressions)) specjalnie wykluczone przypisania do właściwości automatycznej typu struktury w konstruktorze wystąpienia tego typu struktury: takie przypisanie jest uznawany za określony Przypisanie do pola pomocniczego ukryte właściwości auto. W związku z tym że jest dozwolone:

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a>Destruktory

Struktura nie jest dozwolona było zadeklarowanie destruktora.

### <a name="static-constructors"></a>Konstruktory statyczne

Większość tych samych zasad, jak w przypadku klasy konstruktorów statycznych dla struktury, postępuj zgodnie z. Wykonanie Konstruktor statyczny dla elementu typ struktury jest wyzwalany przez pierwszy z następujących zdarzeń w domenie aplikacji:

*  Odwołuje się do statycznego elementu członkowskiego typu struktury.
*  Zadeklarowany w sposób jawny Konstruktor typu struktury jest wywoływany.

Tworzenie wartości domyślne ([wartości domyślne](structs.md#default-values)) struktury typów nie będzie wyzwalać Konstruktor statyczny. (Na przykład jest początkowa wartość elementów w tablicy).

## <a name="struct-examples"></a>Przykłady — struktura

Poniżej pokazano dwa przykłady istotne przy użyciu `struct` typy, aby utworzyć typy, które może służyć podobnie do wstępnie zdefiniowanych typów języka, ale z semantyką zmodyfikowane.

### <a name="database-integer-type"></a>Typ liczby całkowitej bazy danych

`DBInt` Poniżej struktura implementuje typ całkowitoliczbowy, który może reprezentować pełnego zestawu wartości `int` typu oraz dodatkowy stan, który wskazuje nieznanej wartości. Typ z następującą charakterystyką jest często używany w bazach danych.

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a>Typ wartości logicznej bazy danych

`DBBool` Poniżej struktura implementuje przechowywanymi w trzech typu logicznego. Możliwe wartości tego typu są `DBBool.True`, `DBBool.False`, i `DBBool.Null`, gdzie `Null` elementu członkowskiego określa nieznaną wartość. Takie przechowywanymi na trzy typy logiczne są często używane w bazach danych.

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
