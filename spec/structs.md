---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704029"
---
# <a name="structs"></a>Struktury

Struktury są podobne do klas, które reprezentują struktury danych, które mogą zawierać składowe danych i składowe funkcji. Jednak w przeciwieństwie do klas, struktury są typami wartości i nie wymagają przydziału sterty. Zmienna typu struktury bezpośrednio zawiera dane struktury, podczas gdy zmienna typu klasy zawiera odwołanie do danych, które są znane jako obiekt.

Struktury są szczególnie przydatne w przypadku małych struktur danych, które mają semantykę wartości. Liczby zespolone, punkty w układzie współrzędnych lub pary klucz-wartość w słowniku są wszystkimi dobrymi przykładami struktur. Klucz do tych struktur danych polega na tym, że mają niewiele składowych danych, że nie wymagają użycia dziedziczenia lub referencyjnego tożsamości oraz że można je wygodnie zaimplementować przy użyciu semantyki wartości, w której przypisanie kopiuje wartość zamiast odwołania.

Zgodnie z opisem w [typach prostych](types.md#simple-types), typy proste dostarczone przez C#, takie jak `int`, `double` i `bool`, są w rzeczywistości wszystkimi typami struktury. Tak jak te wstępnie zdefiniowane typy są strukturami, można również użyć struktur i przeciążania operatora w celu zaimplementowania nowych typów "pierwotnych" w C# języku. Dwa przykłady takich typów są podane na końcu tego rozdziału ([przykłady struktury](structs.md#struct-examples)).

## <a name="struct-declarations"></a>Deklaracje struktury

*Struct_declaration* to *type_declaration* ([deklaracje typu](namespaces.md#type-declarations)), które deklaruje nową strukturę:

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

*Struct_declaration* składa się z opcjonalnego zestawu *atrybutów* ([atrybutów](attributes.md)), po których następuje opcjonalny zestaw *struct_modifier*s ([Modyfikatory struktury](structs.md#struct-modifiers)), po którym następuje opcjonalny modyfikator `partial`, po którym następuje słowo kluczowe `struct` i *Identyfikator* , który nazywa strukturę, po którym następuje opcjonalna Specyfikacja *Type_parameter_list* ([parametry typu](classes.md#type-parameters)), a następnie opcjonalna Specyfikacja *struct_interfaces* ([częściowa modyfikator](structs.md#partial-modifier)), po którym następuje opcjonalna Specyfikacja *type_parameter_constraints_clause*s ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), po której następuje *struct_body* ([treść struktury](structs.md#struct-body)), opcjonalnie po której następuje średnik.

### <a name="struct-modifiers"></a>Modyfikatory struktury

*Struct_declaration* może opcjonalnie zawierać sekwencję modyfikatorów struktury:

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

Jest to błąd czasu kompilacji dla tego samego modyfikatora do wyświetlenia wiele razy w deklaracji struktury.

Modyfikatory deklaracji struktury mają takie samo znaczenie jak dla deklaracji klasy ([deklaracji klas](classes.md#class-declarations)).

### <a name="partial-modifier"></a>Modyfikator częściowy

Modyfikator `partial` wskazuje, że ta *struct_declaration* jest deklaracją typu częściowego. Wiele deklaracji częściowej struktury o tej samej nazwie w otaczającej przestrzeni nazw lub deklaracji typu Połącz, aby utworzyć jedną deklarację struktury, zgodnie z regułami określonymi w [typach częściowych](classes.md#partial-types).

### <a name="struct-interfaces"></a>Interfejsy struktury

Deklaracja struktury może zawierać specyfikację *struct_interfaces* , w takim przypadku struktura jest określana do bezpośredniego zaimplementowania danego typu interfejsu.

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

Implementacje interfejsów omówiono dokładniej w [implementacjach interfejsu](interfaces.md#interface-implementations).

### <a name="struct-body"></a>Treść struktury

*Struct_body* struktury definiuje elementy członkowskie struktury.

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>Elementy członkowskie struktury

Elementy członkowskie struktury składają się z elementów członkowskich wprowadzonych przez *struct_member_declaration*i członków dziedziczonych z typu `System.ValueType`.

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

Z wyjątkiem różnic zanotowanych w [różnicach klas i struktur](structs.md#class-and-struct-differences), opisy elementów członkowskich klasy, które znajdują się w [składowych klasy](classes.md#class-members) za pomocą [iteratorów](classes.md#iterators) stosują się również do elementów członkowskich struktury.

## <a name="class-and-struct-differences"></a>Różnice klas i struktur

Struktury różnią się od klas na kilka ważnych sposobów:

*  Struktury są typami wartości ([semantyką wartości](structs.md#value-semantics)).
*  Wszystkie typy struktur niejawnie dziedziczą z klasy `System.ValueType` ([dziedziczenie](structs.md#inheritance)).
*  Przypisanie do zmiennej typu struktury tworzy kopię przypisanej wartości ([przypisanie](structs.md#assignment)).
*  Wartość domyślna struktury jest wartością wygenerowaną przez ustawienie wszystkich pól typu wartości na wartości domyślne i wszystkie pola typu odwołania do `null` ([wartości domyślne](structs.md#default-values)).
*  Operacje pakowania i rozpakowywania są używane do konwersji między typem struktury a `object` ([opakowaniem i rozpakowywaniem](structs.md#boxing-and-unboxing)).
*  Znaczenie `this` jest różne dla struktur ([ten dostęp](expressions.md#this-access)).
*  Deklaracje pola wystąpienia dla struktury nie mogą zawierać inicjatorów zmiennych ([inicjatorów pól](structs.md#field-initializers)).
*  Struktura nie może deklarować konstruktora wystąpień bez parametrów ([konstruktorów](structs.md#constructors)).
*  Struktura nie może deklarować destruktora ([destruktory](structs.md#destructors)).

### <a name="value-semantics"></a>Semantyka wartości

Struktury są typami wartości ([typami wartości](types.md#value-types)) i są określane jako semantyka wartości. Klasy, z drugiej strony, to typy odwołań ([typy odwołań](types.md#reference-types)) i są określane jako semantyka odwołania.

Zmienna typu struktury bezpośrednio zawiera dane struktury, podczas gdy zmienna typu klasy zawiera odwołanie do danych, które są znane jako obiekt. Gdy struktura `B` zawiera pole wystąpienia typu `A`, a `A` jest typem struktury, jest to błąd czasu kompilacji dla `A`, który jest zależny od `B` lub typu złożonego z `B`. Struktura `X` ***bezpośrednio zależy*** od struktury `Y`, jeśli `X` zawiera pole wystąpienia typu `Y`. W związku z tą definicją kompletny zestaw struktur, od których zależy struktura, jest przechodnim zamykaniem ***bezpośrednio zależnym od*** relacji.  Na przykład:
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
jest błędem, ponieważ `Node` zawiera pole wystąpienia własnego typu.  Inny przykład
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
jest błędem, ponieważ każdy z typów `A`, `B` i `C` zależy od siebie.

Klasy, istnieje możliwość, że dwie zmienne odwołują się do tego samego obiektu, i w tym przypadku operacje na jednej zmiennej mają wpływ na obiekt, do którego odwołuje się inna zmienna. W przypadku struktur każda z tych zmiennych ma własną kopię danych (z wyjątkiem `ref` i `out` zmiennych parametrów) i nie jest możliwe, aby operacje na nich miały wpływ na inne. Ponadto, ponieważ struktury nie są typami odwołań, nie jest możliwe, aby wartości typu struktury były `null`.

Dana deklaracja
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
Wyświetla wartość `10`. Przypisanie `a` do `b` tworzy kopię wartości, a `b` nie ma wpływ na przypisanie do `a.x`. @No__t-0 zamiast zadeklarować jako Klasa, dane wyjściowe byłyby `100`, ponieważ `a` i `b` odwołują się do tego samego obiektu.

### <a name="inheritance"></a>Dziedziczenie

Wszystkie typy struktur niejawnie dziedziczą z klasy `System.ValueType`, co z kolei dziedziczy z klasy `object`. Deklaracja struktury może określać listę zaimplementowanych interfejsów, ale nie jest możliwe, aby deklaracja struktury mogła określić klasę bazową.

Typy struktur nigdy nie są abstrakcyjne i zawsze są zapieczętowane niejawnie. Modyfikatory `abstract` i `sealed` nie są w związku z tym niedozwolone w deklaracji struktury.

Ponieważ dziedziczenie nie jest obsługiwane dla struktur, zadeklarowana dostępność elementu członkowskiego struktury nie może być `protected` lub `protected internal`.

Składowe funkcji w strukturze nie mogą być `abstract` lub `virtual`, a modyfikator `override` jest dozwolony tylko w celu przesłonięcia metod dziedziczonych z `System.ValueType`.

### <a name="assignment"></a>Przypisanie

Przypisanie do zmiennej typu struktury tworzy kopię przypisanej wartości. Różni się to od przypisywania do zmiennej typu klasy, która kopiuje odwołanie, ale nie obiekt identyfikowany przez odwołanie.

Podobnie jak w przypadku przypisania, gdy struktura jest przekazana jako parametr wartości lub zwracana jako wynik elementu członkowskiego funkcji, tworzona jest kopia struktury. Struktura może być przenoszona przez odwołanie do składowej funkcji przy użyciu parametru `ref` lub `out`.

Gdy właściwość lub indeksator struktury jest elementem docelowym przypisania, wyrażenie wystąpienia skojarzone z właściwością lub dostępem indeksatora musi być sklasyfikowane jako zmienna. Jeśli wyrażenie wystąpienia jest sklasyfikowane jako wartość, wystąpi błąd w czasie kompilacji. Opisano to szczegółowo w temacie [proste przypisanie](expressions.md#simple-assignment).

### <a name="default-values"></a>Wartości domyślne

Zgodnie z opisem w [wartości domyślne](variables.md#default-values)kilka rodzajów zmiennych jest automatycznie inicjowanych do wartości domyślnej podczas ich tworzenia. Dla zmiennych typów klas i innych typów odwołań ta wartość domyślna to `null`. Jednak ponieważ struktury są typami wartości, które nie mogą być `null`, wartość domyślna struktury jest wartością wygenerowaną przez ustawienie wszystkich pól typu wartości na wartość domyślną i wszystkie pola typu odwołania do `null`.

Odwoływanie się do struktury `Point` zadeklarowanej powyżej, przykład
```csharp
Point[] a = new Point[100];
```
Inicjuje każdy `Point` w tablicy do wartości wygenerowanej przez ustawienie pól `x` i `y` na zero.

Wartość domyślna struktury odpowiada wartości zwracanej przez konstruktora domyślnego struktury ([konstruktorów domyślnych](types.md#default-constructors)). W przeciwieństwie do klasy, struktura nie może deklarować konstruktora wystąpienia bez parametrów. Zamiast tego każda struktura niejawnie ma Konstruktor wystąpienia bez parametrów, który zawsze zwraca wartość, która wynika z ustawienia wszystkich pól typu wartości na wartości domyślne i wszystkie pola typu odwołania do `null`.

Struktury powinny zostać zaprojektowane w celu uwzględnienia domyślnego stanu inicjalizacji. W przykładzie
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
zdefiniowany przez użytkownika Konstruktor wystąpień chroni przed wartościami null tylko wtedy, gdy jest on jawnie wywoływany. W przypadkach, gdy zmienna `KeyValuePair` podlega inicjacji wartości domyślnej, pola `key` i `value` będą mieć wartość null, a struktura musi być przygotowana do obsługi tego stanu.

### <a name="boxing-and-unboxing"></a>Pakowanie i rozpakowywanie

Wartość typu klasy można przekonwertować na typ `object` lub do typu interfejsu, który jest implementowany przez klasę po prostu przez traktowanie odwołania jako innego typu w czasie kompilacji. Analogicznie, wartość typu `object` lub wartość typu interfejsu można przekonwertować z powrotem na typ klasy bez zmiany odwołania (ale oczywiście w tym przypadku jest wymagane sprawdzanie typu w czasie wykonywania).

Ponieważ struktury nie są typami odwołań, operacje te są implementowane inaczej dla typów struktury. Gdy wartość typu struktury jest konwertowana na typ `object` lub do typu interfejsu, który jest implementowany przez strukturę, odbywa się operacja opakowywania. Podobnie, gdy wartość typu `object` lub wartość typu interfejsu jest konwertowana z powrotem na typ struktury, wykonywana jest operacja rozpakowywania. Kluczową różnicą między tymi samymi operacjami w typach klas jest to, że opakowanie i rozpakowywanie kopiuje wartość struktury do lub z wystąpienia zapakowanego. W tym celu po rozpakowywania lub rozpakowywania zmiany wprowadzone do struktury nieopakowanej nie są odzwierciedlone w strukturze opakowanej.

Gdy typ struktury zastępuje metodę wirtualną dziedziczoną z `System.Object` (na przykład `Equals`, `GetHashCode` lub `ToString`), wywołanie metody wirtualnej za pośrednictwem wystąpienia typu struktury nie powoduje, że opakowanie nie występuje. Jest to prawdziwe nawet wtedy, gdy struktura jest używana jako parametr typu, a wywołanie odbywa się za pomocą wystąpienia typu parametru typu. Na przykład:
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

Dane wyjściowe programu są następujące:
```console
1
2
3
```

Mimo że jest to zły styl dla `ToString`, aby mieć efekt uboczny, w przykładzie pokazano, że nie nastąpiły żadne opakowanie dla trzech wywołań `x.ToString()`.

Podobnie opakowanie nigdy nie występuje niejawnie podczas uzyskiwania dostępu do elementu członkowskiego w parametrze typu ograniczonego. Załóżmy na przykład, że interfejs `ICounter` zawiera metodę `Increment`, której można użyć do zmodyfikowania wartości. Jeśli `ICounter` jest używany jako ograniczenie, implementacja metody `Increment` jest wywoływana z odwołaniem do zmiennej, która `Increment` została wywołana w, nigdy nie jako kopia w ramce.

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

Pierwsze wywołanie `Increment` modyfikuje wartość w zmiennej `x`. Nie jest to równoznaczne z drugim wywołaniem `Increment`, które modyfikuje wartość w opakowanej kopii `x`. W rezultacie dane wyjściowe programu są następujące:
```console
0
1
1
```

Aby uzyskać więcej informacji na temat pakowania i rozpakowywania, zobacz [opakowanie i rozpakowywanie](types.md#boxing-and-unboxing).

### <a name="meaning-of-this"></a>Znaczenie tego

W ramach konstruktora wystąpienia lub składowej funkcji wystąpienia klasy `this` jest sklasyfikowana jako wartość. W tym przypadku `this` może służyć do odwoływania się do wystąpienia, dla którego wywołano element członkowski funkcji, nie można przypisać do `this` w składowej funkcji klasy.

W konstruktorze wystąpienia struktury, `this` odpowiada parametrowi `out` typu struct i w elemencie członkowskim funkcji instancji struktury `this` odpowiada parametrowi `ref` typu struktury. W obu przypadkach `this` jest klasyfikowane jako zmienna i istnieje możliwość zmodyfikowania całej struktury, dla której element członkowski funkcji został wywołany przez przypisanie do `this` lub przez przekazanie go jako parametru `ref` lub `out`.

### <a name="field-initializers"></a>Inicjatory pola

Zgodnie z opisem w [wartości domyślne](structs.md#default-values), wartość domyślna struktury składa się z wartości, która wynika z ustawiania wszystkich pól typu wartości na wartości domyślne i wszystkie pola typu odwołania do `null`. Z tego powodu struktura nie zezwala na deklaracje pól wystąpienia, aby uwzględnić inicjatory zmiennych. To ograniczenie dotyczy tylko pól wystąpień. Pola statyczne struktury mogą zawierać inicjatory zmiennych.

Przykład
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
Wystąpił błąd, ponieważ deklaracje pola wystąpienia obejmują inicjatory zmiennych.

### <a name="constructors"></a>Konstruktorów

W przeciwieństwie do klasy, struktura nie może deklarować konstruktora wystąpienia bez parametrów. Zamiast tego każda struktura niejawnie ma Konstruktor wystąpienia bez parametrów, który zawsze zwraca wartość, która wynika z ustawienia wszystkich pól typu wartości na wartości domyślne i wszystkie pola typu odwołania do wartości null ([konstruktory domyślne](types.md#default-constructors)). Struktura może deklarować konstruktory wystąpień z parametrami. Na przykład:
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

W przypadku powyższej deklaracji instrukcje
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
Oba te elementy tworzą `Point` z `x` i `y` inicjowane na zero.

Konstruktor wystąpienia struktury nie może zawierać inicjatora konstruktora w postaci `base(...)`.

Jeśli Konstruktor wystąpienia struktury nie określa inicjatora konstruktora, zmienna `this` odpowiada parametrowi `out` typu struktury i podobnej do parametru `out`, a `this` musi być ostatecznie przypisana ([przypisanie określone ](variables.md#definite-assignment)) w każdej lokalizacji, w której Konstruktor zwraca. Jeśli Konstruktor wystąpienia struktury określa inicjator konstruktora, zmienna `this` odpowiada parametrowi `ref` typu struktury i podobnej do parametru `ref`, `this` jest uważana za ostatecznie przypisaną do treści konstruktora. . Rozważmy implementację konstruktora wystąpień poniżej:
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

Żadna funkcja członkowska wystąpienia (w tym zestaw metod dostępu dla właściwości `X` i `Y`) może zostać wywołana, dopóki wszystkie pola w strukturze nie zostały ostatecznie przypisane. Jedyny wyjątek obejmuje automatycznie zaimplementowane właściwości ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)). Określone reguły przypisywania ([proste wyrażenia przypisania](variables.md#simple-assignment-expressions)), które wykluczają przypisanie do właściwości automatycznego typu struktury w konstruktorze wystąpienia tego typu struktury: takie przypisanie jest uznawane za określone przypisanie ukrytego pole zapasowe właściwości autoproperty. W ten sposób można wykonać następujące czynności:

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

Struktura nie może deklarować destruktora.

### <a name="static-constructors"></a>Konstruktory statyczne

Konstruktory statyczne dla struktur są zgodne z większością reguł dla klas. Wykonanie statycznego konstruktora dla typu struktury jest wyzwalane przez pierwsze z następujących zdarzeń, które mają wystąpić w domenie aplikacji:

*  Istnieje odwołanie do statycznego elementu członkowskiego typu struktury.
*  Wywoływany jest jawnie zadeklarowany Konstruktor typu struktury.

Tworzenie wartości domyślnych ([wartości domyślne](structs.md#default-values)) typów struktur nie wyzwala statycznego konstruktora. (Przykładem jest początkowa wartość elementów w tablicy).

## <a name="struct-examples"></a>Przykłady struktury

Poniżej przedstawiono dwa znaczące przykłady użycia typów `struct` do tworzenia typów, które mogą być używane podobnie do wstępnie zdefiniowanych typów języka, ale ze zmodyfikowaną semantyką.

### <a name="database-integer-type"></a>Typ liczby całkowitej bazy danych

W poniższej strukturze `DBInt` jest implementowany typ Integer, który może reprezentować pełny zestaw wartości typu `int` oraz dodatkowy stan, który wskazuje nieznaną wartość. Typ z tymi cechami jest często używany w bazach danych.

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

Struktura `DBBool` jest implementowana przy użyciu trzech wartości typu logicznego. Możliwe wartości tego typu to `DBBool.True`, `DBBool.False` i `DBBool.Null`, gdzie członek `Null` wskazuje nieznaną wartość. Takie trzy typy logiczne są często używane w bazach danych.

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
