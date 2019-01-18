---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229930"
---
# <a name="interfaces"></a>Interfejsy

Interfejs definiuje kontrakt. Klasa lub struktura, która implementuje interfejs musi być zgodne z jego umową. Interfejs może dziedziczyć z wielu interfejsach podstawowych, a klasa lub struktura może zaimplementować wiele interfejsów.

Interfejsy mogą zawierać metody, właściwości, zdarzeń i indeksatorów. Interfejs, sama nie zapewnia implementacje dla elementów członkowskich, które definiuje. Interfejs określa jedynie elementy członkowskie, które muszą być dostarczane przez klasy lub struktury, które implementują interfejs.

## <a name="interface-declarations"></a>Deklaracje interfejsu

*Interface_declaration* jest *type_declaration* ([wpisz deklaracje](namespaces.md#type-declarations)) oświadcza, że nowy typ interfejsu.

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

*Interface_declaration* zawiera opcjonalny zestaw *atrybuty* ([atrybuty](attributes.md)), a następnie opcjonalny zestaw *interface_modifier*s ([interfejsu Modyfikatory](interfaces.md#interface-modifiers)), a następnie opcjonalnie `partial` modyfikator, następuje słowa kluczowego `interface` i *identyfikator* , nazwy interfejsu, następuje opcjonalny *variant_type_parameter_list* specyfikacji ([list parametrów typu Variant](interfaces.md#variant-type-parameter-lists)), a następnie opcjonalnie *interface_base* Specyfikacja ([podstawowa interfejsów](interfaces.md#base-interfaces)), a następnie opcjonalnie *type_parameter_constraints_clause*specyfikacji s ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) , a następnie *interface_body* ([interfejsu treści](interfaces.md#interface-body)), opcjonalnie następuje średnik.

### <a name="interface-modifiers"></a>Modyfikatory interfejsu

*Interface_declaration* może opcjonalnie obejmować sekwencję Modyfikatory interfejsu:

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

Jest to błąd czasu kompilacji dla tego samego modyfikator pojawią się wiele razy w deklaracji interfejsu.

`new` Modyfikator jest dozwolona tylko w interfejsach zdefiniowany w klasie. Określa, że interfejs ukrywa dziedziczonej składowej o tej samej nazwie, zgodnie z opisem w [nowy modyfikator](classes.md#the-new-modifier).

`public`, `protected`, `internal`, I `private` Modyfikatory kontrolować ułatwień dostępu interfejsu. W zależności od kontekstu, w którym występuje deklaracja interfejsu, tylko niektóre z tych modyfikatorów mogą być dozwolone ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)).

### <a name="partial-modifier"></a>Modyfikatora "Partial"

`partial` Modyfikator wskazuje, że to *interface_declaration* jest deklaracją typu częściowego. Łączenie wielu deklaracjach częściowej interfejsu o takiej samej nazwie w deklaracji otaczającego przestrzeń nazw lub typ do postaci jednego interfejsu deklaracji, zgodnie z regułami określone w [typów częściowych](classes.md#partial-types).

### <a name="variant-type-parameter-lists"></a>Listy parametrów typu Variant

Listy parametrów typu Variant może nastąpić tylko na typy interfejsów i delegatów. Różnica od zwykłych *type_parameter_list*s jest opcjonalny *variance_annotation* dla każdego parametru typu.

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

Jeśli jest adnotacji wariancji `out`, parametr typu jest określany jako ***kowariantne***. Jeśli jest adnotacji wariancji `in`, parametr typu jest określany jako ***kontrawariantne***. Jeśli nie ma żadnych adnotacji wariancji, parametr typu jest określany jako ***niezmiennej***.

W przykładzie
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
`X` jest kowariantny, `Y` jest kontrawariantny i `Z` jest niezmienny.

#### <a name="variance-safety"></a>Wariancja bezpieczeństwa

Wystąpienie adnotacje dotyczące wariancji na liście parametrów typu typu ogranicza miejsca, w którym typów może mieć miejsce w deklaracji typu.

Typ `T` jest ***niebezpiecznych danych wyjściowych*** jeśli posiada jedną z następujących czynności:

*  `T` jest kontrawariantnego parametru typu
*  `T` jest typem tablicy o typie elementu niebezpiecznych danych wyjściowych
*  `T` Typ interfejsu lub delegata `S<A1,...,Ak>` skonstruowany z typu ogólnego `S<X1,...,Xk>` miejsca dla co najmniej jednej `Ai` zawiera jedną z następujących czynności:
   * `Xi` jest kowariantny lub niezmiennej i `Ai` jest niebezpieczne danych wyjściowych.
   * `Xi` jest kontrawariantny lub niezmiennej i `Ai` jest bezpieczna pod względem danych wejściowych.
   
Typ `T` jest ***niebezpiecznych danych wejściowych*** jeśli posiada jedną z następujących czynności:

*  `T` jest kowariantny parametr typu
*  `T` jest typem tablicy o typie elementu niebezpiecznych danych wejściowych
*  `T` Typ interfejsu lub delegata `S<A1,...,Ak>` skonstruowany z typu ogólnego `S<X1,...,Xk>` miejsca dla co najmniej jednej `Ai` zawiera jedną z następujących czynności:
   * `Xi` jest kowariantny lub niezmiennej i `Ai` jest niebezpieczne danych wejściowych.
   * `Xi` jest kontrawariantny lub niezmiennej i `Ai` jest niebezpieczne danych wyjściowych.

Intuicyjne typem niebezpiecznych danych wyjściowych jest zabronione w pozycji danych wyjściowych, a typem niebezpiecznych danych wejściowych jest zabronione w pozycji danych wejściowych.

Typ jest ***bezpieczne pod względem danych wyjściowych*** Jeśli nie jest dane wyjściowe — zagrożenie, i ***bezpieczne pod względem danych wejściowych*** Jeśli nie jest niebezpieczne danych wejściowych.

#### <a name="variance-conversion"></a>Konwersja wariancji

Adnotacje dotyczące wariancji ma na celu zapewnienia bardziej łagodne (ale nadal bezpiecznego typu) konwersji na typy interfejsów i delegatów. W tym celu definicje niejawne ([niejawne konwersje](conversions.md#implicit-conversions)) i jawne konwersje ([jawne konwersje](conversions.md#explicit-conversions)) upewnij użytkowania pojęcie wariancji — przetwarzania, która jest zdefiniowana w następujący sposób:

Typ `T<A1,...,An>` jest odchylenie przekonwertować na typ `T<B1,...,Bn>` Jeśli `T` interfejsem lub typem obiektu delegowanego jest zadeklarowana z parametrami typu variant `T<X1,...,Xn>`i dla każdego parametru typu variant `Xi` jedną z następujących posiada:

*  `Xi` jest kowariantny i istnieje niejawna konwersja odwołania lub tożsamości `Ai` do `Bi`
*  `Xi` jest kontrawariantny i niejawne odwołanie lub istnieje konwersja tożsamości z `Bi` do `Ai`
*  `Xi` jest niezależna i tożsamości istnieje konwersja z `Ai` do `Bi`

### <a name="base-interfaces"></a>Interfejsy podstawowe

Interfejs może dziedziczyć z zero lub więcej typów interfejsów, które są nazywane ***jawne interfejsy podstawowe*** interfejsu. Gdy interfejs ma jeden lub więcej jawne interfejsy podstawowe, następnie w deklaracji interfejsu, identyfikator interfejsu następuje dwukropek i przecinkami oddzielone listy typów interfejs podstawowy.

```antlr
interface_base
    : ':' interface_type_list
    ;
```

Dla typu skonstruowanego interfejsu jawnego interfejsy podstawowe są utworzone przez podejmowanie deklaracje jawne interfejs podstawowy deklaracją typu ogólnego oraz zastępując dla każdego *type_parameter* w interfejs podstawowy Deklaracja, odpowiedni *type_argument* skonstruowanego typu.

Jawne interfejsy podstawowe interfejsu musi być co najmniej tak samo dostępna jako interfejs, sama ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)). Na przykład, jest to błąd kompilacji, aby określić `private` lub `internal` interfejsu w *interface_base* z `public` interfejsu.

Jest to błąd czasu kompilacji, interfejsu bezpośrednio lub pośrednio dziedziczyć po sobie samym.

***Podstawowa interfejsów*** interfejsu są jawne interfejsy podstawowe i ich interfejsy podstawowe. Oznacza to zbiór interfejsy podstawowe jest pełne zamknięcie przechodnie jawne interfejsy podstawowe, jawne interfejsy podstawowe i tak dalej. Interfejs dziedziczy wszystkie elementy członkowskie jego interfejsy podstawowe. W przykładzie
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
interfejsy podstawowe z `IComboBox` są `IControl`, `ITextBox`, i `IListBox`.

Innymi słowy `IComboBox` powyżej interfejs dziedziczy członków `SetText` i `SetItems` także `Paint`.

Każdy interfejs podstawowy interfejsu muszą być bezpieczne pod względem danych wyjściowych ([bezpieczeństwa wariancji](interfaces.md#variance-safety)). Klasa lub struktura, która implementuje interfejs, dodatkowo niejawnie implementuje wszystkie interfejsy podstawowe interfejsu.

### <a name="interface-body"></a>Treść interfejsu

*Interface_body* interfejsu definiuje składowych interfejsu.

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a>Elementy członkowskie interfejsu

Członkowie interfejsu są dziedziczone z interfejsy podstawowe elementy członkowskie i elementy członkowskie zadeklarowana przez sam interfejs.

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

Deklaracja interfejsu może zadeklarować zero lub więcej elementów członkowskich. Członkowie interfejs musi być metody, właściwości, zdarzeń i indeksatorów. Interfejs nie może zawierać stałe, pola, operatory, konstruktory wystąpień, destruktorów ani typów nie może zawierać interfejs statyczne elementy członkowskie dowolnego rodzaju.

Wszyscy członkowie interfejsu niejawnie mają dostęp publiczny. Jest to błąd kompilacji, dla deklaracji elementu członkowskiego interfejsu do uwzględnienia wszelkich modyfikatorów. W szczególności członków interfejsów nie można zadeklarować za pomocą modyfikatorów `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, lub `static`.

Przykład
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
deklaruje interfejs, który zawiera jedną z możliwych rodzajów elementów członkowskich: Metody, właściwości, zdarzenia i indeksatora.

*Interface_declaration* tworzy nowe miejsce do deklaracji ([deklaracje](basic-concepts.md#declarations)), a *interface_member_declaration*s natychmiast zawartych *interface_declaration* wprowadzenia nowych członków do tej deklaracji przestrzeni. Następujące reguły mają zastosowanie do *interface_member_declaration*s:

*  Nazwa metody muszą różnić się od nazwy wszystkich właściwości i zdarzenia zadeklarowanych w tym samym interfejsie. Ponadto, podpis ([podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading)) z metody muszą różnić się od podpisy wszystkich pozostałych metod zadeklarowanych w tym samym interfejsie i dwóch metod zadeklarowanych w tym samym interfejsie nie może mieć podpisów, różnią się jedynie przez `ref` i `out`.
*  Nazwa właściwości lub zdarzenia musi się różnić od nazw innych składowych zadeklarowanych w tym samym interfejsie.
*  Podpis indeksatora musi się różnić od podpisy innymi indeksatorami zadeklarowanych w tym samym interfejsie.

Dziedziczone składowe interfejsu są specjalnie nie jest częścią przestrzeni deklaracji interfejsu. W związku z tym interfejs może zadeklarować członka o tej samej nazwie lub podpisie, jako dziedziczonego członka. W takiej sytuacji interfejs pochodny element członkowski jest nazywany Ukryj składowe interfejs podstawowy. Ukrywanie dziedziczonej składowej nie jest uważany za błąd, ale spowodować, że kompilator generuje ostrzeżenia. Aby pominąć to ostrzeżenie, musi zawierać deklaracji elementu członkowskiego interfejsu pochodnego `new` modyfikator, aby wskazać, że składowa pochodnej jest przeznaczona do Ukryj składową bazową. W tym temacie omówiono w dalszej [ukrywanie poprzez dziedziczenie](basic-concepts.md#hiding-through-inheritance).

Jeśli `new` modyfikator znajduje się w deklaracji, która nie ukrywa dziedziczonego członka, w tym celu jest wyświetlane ostrzeżenie. To ostrzeżenie jest pominięty, usuwając `new` modyfikator.

Należy pamiętać, że elementy członkowskie w klasie `object` nie są ściśle rzecz biorąc, elementy członkowskie dowolnego interfejsu ([interfejsu członków](interfaces.md#interface-members)). Jednak elementy członkowskie w klasie `object` są dostępne za pośrednictwem wyszukać składowej w dowolny typ interfejsu ([wyszukanie członka](expressions.md#member-lookup)).

### <a name="interface-methods"></a>Metody interfejsu

Metody interfejsu są deklarowane przy użyciu *interface_method_declaration*s:

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

*Atrybuty*, *Typ_wyniku*, *identyfikator*, i *formal_parameter_list* deklaracji metody interfejsu mają taki sam co oznacza, jak te deklaracji metody w klasie ([metody](classes.md#methods)). Deklaracji metody interfejsu nie jest dozwolona, aby określić treści metody, a deklaracji w związku z tym zawsze kończy się średnikiem.

Każdego typu formalnego parametru metody interfejsu muszą być bezpieczne pod względem danych wejściowych ([bezpieczeństwa wariancji](interfaces.md#variance-safety)), i zwracanym typem musi być albo `void` lub bezpieczny w danych wyjściowych. Ponadto każdy ograniczenie typu klasy, ograniczenie typu interfejsu i ograniczenia parametru typu dowolnego typu jako parametru metody muszą być bezpieczne pod względem danych wejściowych.

Te reguły upewnij się, że wszelkie kowariantny lub kontrawariantny użycia interfejsu pozostaje bezpiecznegop typu. Na przykład
```csharp
interface I<out T> { void M<U>() where U : T; }
```
jest niedozwolony ponieważ użycie `T` jako ograniczenia parametru typu na `U` nie jest bezpieczna pod danych wejściowych.

To ograniczenie nie stosowanych wcześniej sytuacji będzie można naruszyć bezpieczeństwo typu w następujący sposób:
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
To jest faktycznie wywołanie `C.M<E>`. Jednak, że wywołanie jest wymagane, aby `E` dziedziczyć `D`, więc zostałyby w tym miejscu naruszone bezpieczeństwo typów.

### <a name="interface-properties"></a>Właściwości interfejsu

Właściwości interfejsu są deklarowane przy użyciu *interface_property_declaration*s:

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

*Atrybuty*, *typu*, i *identyfikator* deklaracji właściwości interfejsu mają takie samo znaczenie jak deklaracja właściwości w klasie ([ Właściwości](classes.md#properties)).

Metod dostępu w deklaracji właściwości interfejsu odpowiadają metod dostępu w deklaracji właściwości klasy ([Akcesory](classes.md#accessors)), z tą różnicą, że treść metody dostępu musi być zawsze średnikiem. W związku z tym Akcesory wystarczy wskazać, czy właściwość jest odczytu i zapisu, tylko do odczytu lub tylko do zapisu.

Typ właściwości interfejsu muszą być bezpieczne dane wyjściowe, jeśli ma ona metody dostępu get i muszą być bezpieczne danych wejściowych w przypadku dostępu set.

### <a name="interface-events"></a>Zdarzenia interfejsu

Zdarzenia interfejsu są deklarowane przy użyciu *interface_event_declaration*s:

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

*Atrybuty*, *typu*, i *identyfikator* deklaracji zdarzenia interfejsu mają takie samo znaczenie jak w deklaracji zdarzenia w klasie ([zdarzenia ](classes.md#events)).

Typ interfejsu zdarzenia musi być bezpieczne pod względem danych wejściowych.

### <a name="interface-indexers"></a>Indeksatory interfejsów

Indeksatory interfejsu są deklarowane przy użyciu *interface_indexer_declaration*s:

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

*Atrybuty*, *typu*, i *formal_parameter_list* deklaracji interfejsu indeksator mają takie samo znaczenie jak deklaracja indeksatora w klasie ([ Indeksatory](classes.md#indexers)).

Metod dostępu w deklaracji interfejsu indeksatora odpowiadają metod dostępu w deklaracji klasy indeksatora ([indeksatory](classes.md#indexers)), z tą różnicą, że treść metody dostępu musi być zawsze średnikiem. W efekcie metod dostępu po prostu informujące o indeksatora odczytu i zapisu, tylko do odczytu lub tylko do zapisu.

Wszystkie typy parametrów formalnych indeksatora interfejsu muszą być bezpieczne pod względem danych wejściowych. Ponadto wszelkie `out` lub `ref` typy parametrów formalnych również muszą być bezpieczne pod względem danych wyjściowych. Należy pamiętać, że nawet `out` parametry są wymagane jako dane wejściowe, bezpieczne, ze względu na ograniczenie możliwości wykonywania platformy.

Typ indeksatora interfejsu muszą być bezpieczne dane wyjściowe, jeśli ma ona metody dostępu get i muszą być bezpieczne danych wejściowych w przypadku dostępu set.

### <a name="interface-member-access"></a>Dostęp do elementu członkowskiego interfejsu

Elementy członkowskie interfejsu są dostępne za pośrednictwem dostępu do elementu członkowskiego ([dostęp do elementu członkowskiego](expressions.md#member-access)) i dostęp indeksatora ([dostęp indeksatora](expressions.md#indexer-access)) wyrażeń formularza `I.M` i `I[A]`, gdzie `I` jest typem interfejsu `M` jest metoda, właściwość lub zdarzenie tego typu interfejsu i `A` to lista argumentu indeksatora.

Dla interfejsów, które są ściśle pojedyncze dziedziczenie (każdego interfejsu w łańcuch dziedziczenia ma dokładnie zero lub jeden interfejs podstawowy bezpośrednich), wpływ wyszukanie członka ([wyszukanie członka](expressions.md#member-lookup)), wywołanie metody ([ Wywołań metod](expressions.md#method-invocations)), a dostęp indeksatora ([dostęp indeksatora](expressions.md#indexer-access)) reguły są dokładnie takie same jak dla klasy i struktury: Ukryj członków więcej uzyskiwana mniej pochodnego elementów członkowskich z tej samej nazwie lub podpisie. Jednak dla interfejsów dziedziczenia wielokrotnego niejednoznaczności może wystąpić, gdy dwie lub więcej niepowiązanych interfejsy podstawowe zadeklarować składowych o tej samej nazwie lub podpisie. W tej sekcji przedstawiono kilka przykładów takich sytuacji. We wszystkich przypadkach ma jawnych rzutowań może służyć do rozwiązywania niejednoznaczności.

W przykładzie
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
pierwsze dwie instrukcje powodują błędy czasu kompilacji, ponieważ wyszukanie członka ([wyszukanie członka](expressions.md#member-lookup)) z `Count` w `IListCounter` jest niejednoznaczna. Jak pokazano w przykładzie, niejednoznaczności został rozwiązany przez rzutowanie `x` do typu odpowiedniego do interfejsu podstawowego. Takie rzutowania mieć żadnych kosztów środowiska wykonawczego — jedynie składają się one wyświetlania wystąpienia jako mniej pochodnego typu w czasie kompilacji.

W przykładzie
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
wywołanie `n.Add(1)` wybiera `IInteger.Add` , stosując zasady rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution). Podobnie wywołanie `n.Add(1.0)` wybiera `IDouble.Add`. Podczas wstawiania ma jawnych rzutowań, istnieje tylko jeden Release candidate metody i w związku z tym żadnych niejednoznaczności.

W przykładzie
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
`IBase.F` składowa ukryta przez `ILeft.F` elementu członkowskiego. Wywołanie `d.F(1)` wybiera w związku z tym `ILeft.F`, nawet jeśli `IBase.F` wydaje się nie być ukryte w ścieżce dostępu, który prowadzi przez `IRight`.

Intuicyjne reguły dla ukrywanie w interfejsach dziedziczenia wielokrotnego to po prostu: Jeśli element członkowski jest ukryty w dowolnej ścieżce dostęp, jest on ukryty wszystkich ścieżek dostępu. Ponieważ ścieżka dostępu z `IDerived` do `ILeft` do `IBase` ukrywa `IBase.F`, należy również jest ukryty w ścieżce dostępu z `IDerived` do `IRight` do `IBase`.

## <a name="fully-qualified-interface-member-names"></a>Interfejs w pełni kwalifikowanej nazwy elementów członkowskich

Elementu członkowskiego interfejsu nazywa się czasem za jego ***w pełni kwalifikowana nazwa***. W pełni kwalifikowaną nazwę elementu członkowskiego interfejsu składa się z nazwy interfejsu, w którym składowa jest zadeklarowana, następuje kropka, następuje nazwa elementu członkowskiego. W pełni kwalifikowaną nazwę elementu członkowskiego odwołuje się do interfejsu, w którym zadeklarowany jest elementu członkowskiego. Na przykład biorąc pod uwagę deklaracji
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
w pełni kwalifikowana nazwa `Paint` jest `IControl.Paint` i w pełni kwalifikowana nazwa `SetText` jest `ITextBox.SetText`.

W powyższym przykładzie nie jest możliwe do odwoływania się do `Paint` jako `ITextBox.Paint`.

Gdy interfejs jest częścią przestrzeni nazw, w pełni kwalifikowaną nazwę elementu członkowskiego interfejsu zawiera nazwę przestrzeni nazw. Na przykład
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

Tutaj, w pełni kwalifikowana nazwa `Clone` metodą jest `System.ICloneable.Clone`.

## <a name="interface-implementations"></a>Implementacje interfejsu

Interfejsy mogą być wykonywane przez klas i struktur. Aby wskazać, że klasa lub struktura bezpośrednio implementuje interfejs, identyfikator interfejsu znajduje się na liście klas bazowych, klasy lub struktury. Na przykład:
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

Klasa lub struktura, która bezpośrednio implementuje interfejs także bezpośrednio implementuje wszystkie interfejsy podstawowe interfejsu niejawnie. Ta zasada obowiązuje, nawet jeśli w klasie lub strukturze nie jawnie wszystkie interfejsy podstawowe na liście klas bazowych. Na przykład:
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

W tym miejscu klasy `TextBox` implementuje interfejsy `IControl` i `ITextBox`.

Gdy klasa `C` bezpośrednio implementuje interfejs, wszystkie klasy pochodne od C umożliwiający również implementuj interfejs niejawnie. Interfejsy podstawowe, określonym w deklaracji klasy mogą być typami skonstruowanego interfejsu ([skonstruowany typy](types.md#constructed-types)). Interfejs podstawowy nie może być parametrem typu samodzielnie, chociaż może pociągać za sobą parametry typu, które znajdują się w zakresie. Poniższy kod ilustruje, jak wdrożyć i rozszerzyć typy utworzone klasy:
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

Interfejsy podstawowe w deklaracji klasy ogólnej musi spełniać reguły unikatowości opisanego w [unikatowości implementowane interfejsy](interfaces.md#uniqueness-of-implemented-interfaces).

### <a name="explicit-interface-member-implementations"></a>W jawnej implementacji elementu członkowskiego

Do celów implementacja interfejsy, klasy lub struktury może deklarować ***implementacji elementu członkowskiego interfejsu jawnego***. Implementacja interfejsu jawnego członka jest deklaracji metody, właściwości, zdarzenia lub indeksatora, która odwołuje się do interfejsu w pełni kwalifikowaną nazwę elementu członkowskiego. Na przykład
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

W tym miejscu `IDictionary<int,T>.this` i `IDictionary<int,T>.Add` są w jawnej implementacji elementu członkowskiego.

W niektórych przypadkach nazwa elementu członkowskiego interfejsu nie może być odpowiednia dla klasy implementującej, w których przypadku składowej interfejsu informacje mogą być wprowadzone przy użyciu jawną implementacją członków. Klasy implementującej abstrakcji pliku, na przykład, prawdopodobnie będzie implementować `Close` funkcja elementu członkowskiego, która obowiązuje przy zwalnianiu zasobów plików i zaimplementować `Dispose` metody `IDisposable` interfejs, za pomocą interfejsu jawnego implementacja elementu członkowskiego:
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

Nie jest możliwe uzyskanie dostępu jawną implementacją członków przy użyciu jego w pełni kwalifikowana nazwa wywołania metody, dostęp do właściwości lub indeksatora dostępu. Implementacja interfejsu jawnego członka może zostać oceniony jedynie przez wystąpienie interfejsu i w takim przypadku odwołuje się po prostu jego nazwa elementu członkowskiego.

Jest to błąd czasu kompilacji dla jawną implementacją członków do uwzględnienia modyfikatory dostępu i jest błąd kompilacji w celu uwzględnienia modyfikatorów `abstract`, `virtual`, `override`, lub `static`.

W jawnej implementacji elementu członkowskiego ma właściwości o różnej dostępności niż pozostałe elementy członkowskie. Ponieważ implementacji elementu członkowskiego interfejsu jawnego nigdy nie są dostępne za pośrednictwem ich w pełni kwalifikowana nazwa w wywołaniu metody lub dostęp do właściwości, są one w sensie prywatnych. Jednak ponieważ są one dostępne za pośrednictwem wystąpienia interfejsu, są one w sensie również publicznych.

W jawnej implementacji elementu członkowskiego służą dwa podstawowe cele:

*  Ponieważ w jawnej implementacji elementu członkowskiego nie są dostępne za pośrednictwem wystąpienia klasy lub struktury, umożliwiają one implementacji interfejsów, które mają być wykluczone z publicznego interfejsu klasy lub struktury. Jest to szczególnie przydatne, gdy klasa lub struktura implementuje wewnętrznego interfejsu, który jest nie interesujące użytkownika tej klasy lub struktury.
*  W jawnej implementacji elementu członkowskiego zezwala na Uściślanie składowych interfejsu, z tym samym podpisie. Bez implementacji elementu członkowskiego interfejsu jawnego byłoby niemożliwe do klasy lub struktury mają różne implementacje interfejsu elementów członkowskich z tym samym podpisie i zwraca typ, jako jej byłyby niemożliwe do klasy lub struktury mają implementacji we wszystkich składowych interfejsu, w tym samym podpisie, ale różne typy zwracane.

W przypadku jawną implementacją członków był prawidłowy, klasy lub struktury należy nazwać interfejs na swojej liście klas bazowych, zawierający element, którego w pełni kwalifikowana nazwa, typ i typy parametrów dokładnie odpowiadają elementu członkowskiego interfejsu jawnego Implementacja. W związku z tym w następującej klasy
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
Deklaracja `IComparable.CompareTo` powoduje błąd w czasie kompilacji, ponieważ `IComparable` nie znajduje się na liście klas bazowych z `Shape` i nie jest interfejsem podstawowy `ICloneable`. Podobnie w deklaracjach
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
Deklaracja `ICloneable.Clone` w `Ellipse` powoduje błąd w czasie kompilacji, ponieważ `ICloneable` nie jest jawnie wymienione na liście klas bazowych z `Ellipse`.

Interfejs, w którym zadeklarowano ten element członkowski musi odwoływać się w pełni kwalifikowaną nazwę elementu członkowskiego interfejsu. W efekcie w deklaracjach
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
jawną implementacją członków z `Paint` musi być napisana jako `IControl.Paint`.

### <a name="uniqueness-of-implemented-interfaces"></a>Unikatowość implementowanych interfejsów

Interfejsy implementowane przez deklarację typu ogólnego musi pozostać unikatowe dla wszystkich możliwych typów skonstruowany. Bez tej reguły byłoby niemożliwe do należy określić poprawną metodę do wywołania dla niektórych typów skonstruowany. Na przykład załóżmy, że zezwolono deklaracji klasy ogólnej do zapisania w następujący sposób:
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

Była to dozwolone, byłoby niemożliwe do należy określić które kodu do wykonania w następujących przypadkach:
```csharp
I<int> x = new X<int,int>();
x.F();
```

Aby ustalić, czy lista interfejsu deklaracją typu ogólnego jest prawidłowy, wykonywane są następujące czynności:

*  Pozwól `L` się na liście interfejsów bezpośrednio określone w ogólnej klasy, struktury lub interfejsu deklaracji `C`.
*  Dodaj do `L` podstawowa dowolne interfejsy interfejsów, które są już w `L`.
*  Usuń duplikaty z `L`.
*  Jeśli wszystkie możliwe zbudowany typ utworzone na podstawie `C` będzie po argumentów typu są zastępowane do `L`, spowodować, że dwa interfejsy w `L` być identyczne i następnie deklaracji `C` jest nieprawidłowy. Deklaracje ograniczenia nie są uwzględniane podczas określania wszystkie możliwe typy utworzone.

W deklaracji klasy `X` powyżej listy interfejsu `L` składa się z `I<U>` i `I<V>`. Deklaracja jest nieprawidłowy, ponieważ skonstruowany dowolnego typu za pomocą `U` i `V` są tego samego typu spowodowałoby te dwa interfejsy być identyczne typy.

Możliwe jest, określonych na dziedziczenie różnych poziomach w celu ujednolicenie interfejsów:
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

Mimo że ten kod jest prawidłowy `Derived<U,V>` implementuje interfejsy `I<U>` i `I<V>`. Kod
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
wywołuje metodę w `Derived`, ponieważ `Derived<int,int>` w praktyce oznacza ponowne wdrożenie `I<int>` ([interfejs ponowną implementację](interfaces.md#interface-re-implementation)).

### <a name="implementation-of-generic-methods"></a>Implementacja metody ogólne

Gdy metody ogólnej niejawnie implementuje metodę interfejsu ograniczeń dla każdego parametr typu metody musi być równoważna w obu deklaracjach (po dowolny typ interfejsu parametry są zastępowane odpowiednie argumenty typu), gdzie — metoda Parametry typu są identyfikowane przez numer porządkowy pozycji, od lewej do prawej.

Gdy metody ogólnej jawnie implementuje metodę interfejsu, jednak bez ograniczeń są dozwolone dla metody wykonawcze. Zamiast tego ograniczenia są dziedziczone z metody interfejsu

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

Metoda `C.F<T>` niejawnie implementuje `I<object,C,string>.F<T>`. W tym przypadku `C.F<T>` jest nie wymagane (ani nie dozwolone) do określenia ograniczenie `T:object` ponieważ `object` niejawne ograniczenia dotyczące wszystkich parametrów typu. Metoda `C.G<T>` niejawnie implementuje `I<object,C,string>.G<T>` ponieważ ograniczenia odpowiadają w interfejsie, po zastąpieniu parametry typu interface odpowiednie argumenty typu. Ograniczenie dla metody `C.H<T>` występuje błąd, ponieważ zamknięte typy (`string` w tym przypadku) nie może być używane jako warunki ograniczające. Pominięcie tego ograniczenia będą również błąd, ponieważ ograniczenia implementacje metod interfejsu niejawne muszą być zgodne. W związku z tym, nie jest możliwe implementować niejawnie `I<object,C,string>.H<T>`. Ta metoda interfejsu można zaimplementować tylko przy użyciu jawną implementacją członków:
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

W tym przykładzie jawną implementacją członków wywołuje metodę publiczną o ściśle słabszy ograniczenia. Należy pamiętać, że przypisanie z `t` do `s` jest prawidłowy, ponieważ `T` dziedziczy z ograniczeniem `T:string`, mimo że to ograniczenie nie ma można wyrazić w kodzie źródłowym.

### <a name="interface-mapping"></a>Mapowanie interfejsu

Klasy lub struktury, należy podać implementacji wszystkich członków interfejsów, które są wymienione na liście klas bazowych, klasy lub struktury. Proces trwa lokalizowanie implementacji członków interfejsów w implementujące klasy lub struktury jest znany jako ***mapowania interfejsu***.

Mapowanie interfejsu dla klasy lub struktury `C` lokalizuje implementację dla każdego elementu członkowskiego każdy interfejs określony na liście klas bazowych z `C`. Implementacja interfejsu określonego elementu członkowskiego `I.M`, gdzie `I` interfejs, w którym element członkowski `M` zadeklarowano, jest określana przez badanie każda klasa lub struktura `S`, począwszy od `C` i Powtarzanie dla każdego kolejnych klasę bazową `C`, dopóki nie znajduje się dopasowania:

*  Jeśli `S` zawiera deklarację jawnej implementacji elementu członkowskiego, który odpowiada `I` i `M`, ten element członkowski jest implementacja `I.M`.
*  W przeciwnym razie, jeśli `S` zawiera deklarację niestatycznej składowej publiczny, który odpowiada `M`, ten element członkowski jest implementacja `I.M`. Jeśli więcej niż jeden element członkowski są zgodne, jest nieokreślona składowej, która jest implementacją `I.M`. Taka sytuacja może wystąpić tylko, jeśli `S` jest skonstruowanego typu, w której dwa elementy członkowskie zadeklarowane w typie ogólnym mają różnych podpisów, ale argumentów typu należy ich podpisy identyczne.

Występuje błąd kompilacji, jeśli implementacje nie można znaleźć dla wszystkich elementów członkowskich wszystkie interfejsy określone na liście klas bazowych, z `C`. Należy pamiętać, że Członkowie interfejsu obejmują te elementy członkowskie, które są dziedziczone z interfejsy podstawowe.

Na potrzeby mapowania interfejsu, element członkowski klasy `A` pasuje do elementu członkowskiego interfejsu `B` po:

*  `A` i `B` są metody i nazwę typu, a list parametrów formalnych `A` i `B` są identyczne.
*  `A` i `B` są właściwości, nazwę i typ `A` i `B` są identyczne, i `A` ma tej samej metody dostępu jako `B` (`A` może mieć dodatkowe metody dostępu, jeśli nie jest interfejsem jawne implementacja elementu członkowskiego).
*  `A` i `B` są zdarzenia oraz nazwę i typ `A` i `B` są identyczne.
*  `A` i `B` indeksatorów, typ i listy parametrów formalnych `A` i `B` są identyczne, i `A` ma tej samej metody dostępu jako `B` (`A` mają dodatkowe metody dostępu, jeśli nie jest dozwolone jawną implementacją członków).

Dostępne są następujące istotne implikacje algorytm mapowania interfejsu:

*  W jawnej implementacji elementu członkowskiego mają pierwszeństwo przed innych członków w tej samej klasie lub strukturze podczas określania składowej klasy lub struktury, która implementuje składową interfejsu.
*  Niepubliczne ani statyczne elementy członkowskie uczestniczyć w mapowania interfejsu.

W przykładzie
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
`ICloneable.Clone` członkiem `C` staje się implementacja `Clone` w `ICloneable` ponieważ implementacji elementu członkowskiego interfejsu jawnego mają pierwszeństwo przed innymi członkami.

Jeśli klasa lub struktura implementuje dwa lub więcej interfejsów zawierający element członkowski o takiej samej nazwie, typie i typy parametrów, jest możliwe mapowanie każdego z tych składowych interfejsu na jeden element członkowski klasy lub struktury. Na przykład
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

W tym miejscu `Paint` obu metod `IControl` i `IForm` są mapowane na `Paint` method in Class metoda `Page`. Oczywiście również istnieje możliwość mają oddzielne interfejsu jawnego implementacji elementu członkowskiego dla obu metod.

Jeśli klasa lub struktura implementuje interfejs, który zawiera ukryte elementy członkowskie, niektóre elementy członkowskie niekoniecznie musi być implementowany przez implementacji elementu członkowskiego interfejsu jawnego. Na przykład
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

Implementację tego interfejsu wymaga co najmniej jedną jawną implementacją członków, a następnie może wykonać jedną z następujących form
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

Gdy klasa implementuje wiele interfejsów, które mają ten sam interfejs podstawowy, może istnieć tylko jedna implementacja interfejs podstawowy. W przykładzie
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
nie jest możliwe mieć osobne implementacje dla `IControl` o nazwie na liście klas bazowych `IControl` dziedziczone przez `ITextBox`i `IControl` dziedziczone przez `IListBox`. W rzeczywistości nie są używane osobne tożsamości dla tych interfejsów. Zamiast implementacje `ITextBox` i `IListBox` udostępnianie tego samego wdrożenia `IControl`, i `ComboBox` uznaje się wystarczy do zaimplementowania trzy interfejsy `IControl`, `ITextBox`, i `IListBox`.

Elementy członkowskie klasy bazowej uczestniczyć w mapowania interfejsu. W przykładzie
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
Metoda `F` w `Class1` jest używany w `Class2`przez implementację `Interface1`.

### <a name="interface-implementation-inheritance"></a>Dziedziczenie implementacji interfejsu

Klasa dziedziczy wszystkie implementacje interfejsu udostępniane przez jej klas podstawowych.

Bez jawnie ***ponownej implementacji*** interfejsu klasy pochodnej nie może w żaden sposób zmieniać mapowania interfejs dziedziczy jej klas podstawowych. Na przykład w deklaracji
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
`Paint` method in Class metoda `TextBox` ukrywa `Paint` method in Class metoda `Control`, ale nie zmienia mapowania `Control.Paint` na `IControl.Paint`i zwraca `Paint` za pośrednictwem klasy będzie wystąpień i wystąpienia interfejsu ma następujące skutki
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

Jednak gdy metody interfejsu jest mapowana na wirtualnej metody w klasie, jest możliwe dla klasy pochodne, aby przesłonić metodę wirtualną i zmień implementację interfejsu. Na przykład poprawiania deklaracje powyżej do
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
teraz będzie można zauważyć następujące efekty
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

Ponieważ implementacji elementu członkowskiego interfejsu jawnego nie może być zadeklarowana wirtualnego, nie jest możliwe zastąpienie jawną implementacją członków. Jednak go nadaje się doskonale do jawną implementacją członków do wywoływania innej metody i czy innej metody mogą być deklarowane wirtualnego Aby zezwolić na pochodne klasy, aby go zastąpić. Na przykład
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

W tym miejscu klasy pochodne `Control` można specjalizować wykonania `IControl.Paint` przez zastąpienie `PaintControl` metody.

### <a name="interface-re-implementation"></a>Ponowna implementacja interfejsu

Klasę, która dziedziczy implementację interfejsu jest dozwolona na ***ponownego zaimplementowania*** interfejsu, umieszczając je na liście klas bazowych.

Ponowna implementacja interfejsu jest zgodna dokładnie tego samego interfejsu reguły mapowania jako początkowej implementacji interfejsu. W efekcie mapowanie dziedziczony interfejs nie ma wpływu na mapowania interfejsu ustanowione dla ponowna implementacja interfejsu. Na przykład w deklaracji
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
fakt, `Control` mapuje `IControl.Paint` na `Control.IControl.Paint` nie ma wpływu na ponowną implementację w `MyControl`, mapująca `IControl.Paint` na `MyControl.Paint`.

Dziedziczone członka publicznego deklaracje i jawne dziedziczonego elementu członkowskiego, deklaracje uczestniczyć w trakcie mapowania interfejsu ponownie implementowane interfejsy. Na przykład
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

Tutaj, implementacja `IMethods` w `Derived` mapuje metod interfejsu na `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, i `Base.I`.

Gdy klasa implementuje interfejs go niejawnie implementuje również wszystkie interfejsy podstawowe tego interfejsu. Podobnie, ponowna implementacja interfejsu jest również niejawnie ponowną implementację wszystkich interfejsów, interfejsy podstawowe. Na przykład
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

Tutaj, ponowną implementację elementu `IDerived` ponownie implementuje również `IBase`, mapowanie `IBase.F` na `D.F`.

### <a name="abstract-classes-and-interfaces"></a>Klasy abstrakcyjne i interfejsy

Takich jak klasy nieabstrakcyjnej klasy abstrakcyjnej muszą dostarczać implementacje, wszystkich elementów członkowskich interfejsów, które są wymienione na liście klas bazowych, klasy. Jednak klasy abstrakcyjnej jest dozwolone metody interfejsu na metody abstrakcyjne mapowania. Na przykład
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

Tutaj, implementacja `IMethods` mapuje `F` i `G` na metody abstrakcyjne, która musi zostać zastąpiona w nieabstrakcyjnej klasy, które wynikają z `C`.

Należy pamiętać, że implementacji elementu członkowskiego interfejsu jawnego nie może być abstrakcyjny, ale implementacji elementu członkowskiego interfejsu jawnego oczywiście są dozwolone do wywołania metody abstrakcyjne. Na przykład
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

Tutaj, klasy nieabstrakcyjnej, które wynikają z `C` byłoby potrzebnego do zastąpienia `FF` i `GG`, zapewniając w ten sposób wykonania rzeczywistego `IMethods`.
