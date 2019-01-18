---
ms.openlocfilehash: 08c14d9ef2afe30580f456995066c141653ede92
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229942"
---
# <a name="delegates"></a>Delegaty

Delegatów realizację scenariuszy, że inne języki — takich jak C++, Pascal i Modula — mają adresowane za pomocą wskaźników funkcji. W przeciwieństwie do wskaźników funkcji języka C++ należy jednak obiekty delegowane są w pełni zorientowana obiektowo i w przeciwieństwie do języka C++ wskaźników do składowych, delegatów hermetyzacji zarówno w przypadku wystąpienia obiektu, jak i metody.

Deklaracja delegata definiuje klasę, która jest pochodną klasy `System.Delegate`. Wystąpienie delegata hermetyzuje listę wywołań, który znajduje się lista jedną lub więcej metod, z których każdy jest określany jako element możliwy do wywołania. Dla wystąpienia metod, wywoływane jednostki składa się z wystąpieniem, a metoda, w tym wystąpieniu. Obiekt możliwy do wywołania metody statyczne, składa się z tylko metody. Wywoływanie wystąpienie delegata, za pomocą odpowiedniego zestawu argumentów powoduje, że każdy możliwy do wywołania jednostek pełnomocnika do wywołania z danego zestawu argumentów.

Interesujące i przydatne własności wystąpienie delegata jest, nie wiedzieć i nie interesuje klas, metod, który hermetyzuje; wszystko, co dla Ciebie ważne jest, tych metod zgodne ([delegować deklaracje](delegates.md#delegate-declarations)) z typem delegata. To sprawia, że delegaty idealnie nadaje się do wywołania "anonimowy".

## <a name="delegate-declarations"></a>Deklaracje delegata

A *delegate_declaration* jest *type_declaration* ([wpisz deklaracje](namespaces.md#type-declarations)) Określa nowy typ delegata.

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

Jest to błąd czasu kompilacji dla tego samego modyfikator pojawią się wiele razy w deklaracji delegate.

`new` Modyfikator jest dozwolone tylko w delegatów zadeklarowana wewnątrz innego typu, w którym to przypadku określa, że taki obiekt delegowany ukrywa dziedziczoną składową o tej samej nazwie, zgodnie z opisem w [nowy modyfikator](classes.md#the-new-modifier).

`public`, `protected`, `internal`, I `private` Modyfikatory kontrolować ułatwień dostępu dla typu delegata. W zależności od kontekstu, w którym występuje deklaracja delegata, niektóre z tych modyfikatorów może nie być dozwolone ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)).

Nazwa typu delegata jest *identyfikator*.

Opcjonalny *formal_parameter_list* określa parametry delegata, i *Typ_wyniku* wskazuje typ zwracany obiektu delegowanego.

Opcjonalny *variant_type_parameter_list* ([list parametrów typu Variant](interfaces.md#variant-type-parameter-lists)) określa parametry typu delegata, sam.

Typem zwracanym typem obiektu delegowanego musi być albo `void`, lub bezpieczny w danych wyjściowych ([bezpieczeństwa wariancji](interfaces.md#variance-safety)).

Wszystkie typy parametrów formalnych typ obiektu delegowanego musi być bezpieczne pod względem danych wejściowych. Ponadto dowolne `out` lub `ref` typy parametrów także musi być bezpieczne pod względem danych wyjściowych. Należy pamiętać, że nawet `out` parametry są wymagane jako dane wejściowe, bezpieczne, ze względu na ograniczenie możliwości wykonywania platformy.

Typy delegatów w języku C# są nazwa jego odpowiednika, nie jest równorzędny strukturę. W szczególności dwa typy delegatów różnych, które mają ten sam parametr wyświetla i zwraca typ są traktowane jako typy delegatów różnych. Jednak wystąpień dwóch typów różne, ale strukturalnie równoważne delegata może porównać jako równe ([delegować Operatory równości](expressions.md#delegate-equality-operators)).

Na przykład:

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

Metody `A.M1` i `B.M1 `są zgodne z typami delegatów `D1` i `D2` , ponieważ mają one takie same zwracają typ i listą parametrów; jednak te typy delegatów są dwa różne typy, dzięki czemu nie wymienne. Metody `B.M2`, `B.M3`, i `B.M4` są zgodne z typami delegatów `D1` i `D2`, ponieważ mają one różne typy zwracane lub listy parametrów.

Podobnie jak innych deklaracji typu ogólnego argumentów typu podaje się w celu utworzenia typu skonstruowanego delegata. Typy parametrów i typ zwracany typ delegata skonstruowane są tworzone, zastępując dla każdego parametru typu w deklaracji delegata, argument typu odpowiedniego typu skonstruowanego delegata. Wynikowy typ zwracany i typy parametrów są używane w określeniu, jakie metody są zgodne z typem delegowanym skonstruowany. Na przykład:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

Metoda `X.F` jest zgodny z typem delegata `Predicate<int>` , a także metoda `X.G` jest zgodny z typem delegata `Predicate<string>` .

Jedynym sposobem, aby zadeklarować typ delegata jest za pośrednictwem *delegate_declaration*. Typ obiektu delegowanego jest typu klasy, która jest pochodną `System.Delegate`. Typy delegatów są niejawną kolekcją `sealed`, więc nie jest dozwolone do wyprowadzenia dowolnego typu z typem delegowanym. Również nie jest dozwolone, aby utworzyć pochodny typ niedelegowany klasy z `System.Delegate`. Należy pamiętać, że `System.Delegate` jest sam nie typ delegata; jest typem klasy, z której pochodzą wszystkie typy delegatów.

C# zawiera specjalnej składni dla delegata, podczas tworzenia wystąpienia i wywoływania. Z wyjątkiem wystąpienia żadnych operacji, które można zastosować do klasy lub wystąpienia klasy można również będą stosowane do delegata klasy lub wystąpienia, odpowiednio. W szczególności jest możliwość dostępu do elementów członkowskich `System.Delegate` typu za pomocą składni dostępu do zwykłych elementu członkowskiego.

Zestaw metod zamknięte przez wystąpienie delegata jest wywoływana jako wywołanie listy tych praktyk. Podczas tworzenia wystąpienia delegata ([delegować zgodności](delegates.md#delegate-compatibility)) z jednej metody hermetyzującej element tej metody, a jego listy wywołań zawiera tylko jeden wpis. Jednak połączeniu dwa wystąpienia innych niż null, delegat swoimi listami wywołania są łączone — w kolejności pozostałych operand, a następnie prawy operand — w celu utworzenia nowej listy wywołania, który zawiera dwa lub więcej wpisów.

Delegaty są łączone za pomocą pliku binarnego `+` ([operator dodawania](expressions.md#addition-operator)) i `+=` operatorów ([przydział złożony](expressions.md#compound-assignment)). Delegat może zostać usunięty z kombinacji delegatów, za pomocą pliku binarnego `-` ([operator odejmowania](expressions.md#subtraction-operator)) i `-=` operatorów ([przydział złożony](expressions.md#compound-assignment)). Delegaty mogą być porównywane pod kątem równości ([delegować Operatory równości](expressions.md#delegate-equality-operators)).

W poniższym przykładzie pokazano tworzenie wystąpienia liczby obiektów delegowanych, i ich odpowiedniego wywołania listy:

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

Gdy `cd1` i `cd2` są tworzone, ich hermetyzacji jednej metody. Podczas `cd3` jest uruchomiony, ma listę wywołań z dwóch metod `M1` i `M2`, w tej kolejności. `cd4`firmy zawiera listę wywołań `M1`, `M2`, i `M1`, w tej kolejności. Na koniec `cd5`firmy zawiera listę wywołań `M1`, `M2`, `M1`, `M1`, i `M2`, w tej kolejności. Aby uzyskać więcej przykładów łączenie delegatów (a także jak usuwania), zobacz [delegować wywołania](delegates.md#delegate-invocation).

## <a name="delegate-compatibility"></a>Zgodność delegata

Metoda ani delegat `M` jest ***zgodne*** z typem delegowanym `D` jeśli spełnione są wszystkie z następujących czynności:

*  `D` i `M` mają taką samą liczbę parametrów i każdego parametru w `D` ma taką samą `ref` lub `out` Modyfikatory jako odpowiadającym mu parametrem w `M`.
*  Dla każdego parametru wartości (parametr bez `ref` lub `out` modyfikator), konwersja tożsamości ([konwersji tożsamości](conversions.md#identity-conversion)) lub niejawna konwersja odwołania ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) istnieje niż typ parametru w `D` odpowiedni typ parametru w `M`.
*  Dla każdego `ref` lub `out` parametru, wpisz parametr `D` jest taki sam jak typ parametru w `M`.
*  Istnieje tożsamość lub niejawna konwersja odwołania z typem zwracanym `M` do zwracanego typu `D`.

## <a name="delegate-instantiation"></a>Podczas tworzenia wystąpienia delegata

Wystąpienie delegata jest tworzony przez *delegate_creation_expression* ([delegować Tworzenie wyrażenia](expressions.md#delegate-creation-expressions)) lub konwersji do typu delegata. Wystąpienie delegata nowo utworzony następnie odwołuje się albo:

*  Statyczna metoda, do którego odwołuje się *delegate_creation_expression*, lub
*  Obiekt docelowy (która nie może być `null`) lub wystąpienie metody, do którego odwołuje się *delegate_creation_expression*, lub
*  Delegat innego.

Na przykład:

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

Po utworzeniu wystąpienia delegata zawsze odwołuje się do tego samego obiektu docelowego i metody. Należy pamiętać, że gdy dwa delegaty są połączone lub jeden zostanie usunięty z kolei nowe wyniki delegata z własną listą wywołania; wywołania listę obiektów delegowanych, połączone lub usunąć pozostaną bez zmian.

## <a name="delegate-invocation"></a>Wywołanie delegata

C# zapewnia specjalnej składni wywoływania delegata. Gdy wystąpienie delegata inną niż null, którego lista wywołania zawiera jeden wpis jest wywoływany, wywołuje ono jedną metodę, przy użyciu tych samych argumentów podano i zwraca taką samą wartość jak określonych do metody. (Zobacz [delegowanie wywołań](expressions.md#delegate-invocations) szczegółowe informacje na temat wywołanie delegata.) Jeśli wystąpi wyjątek podczas wywołania obiektu delegowanego, a ten wyjątek nie jest wyłapywany wewnątrz metody, która została wywołana, wyszukaj klauzuli catch wyjątku jest kontynuowane w metodzie, która wywołuje delegata, tak, jakby były wywołane bezpośrednio tej metody Metoda, do której delegować, określane.

Wywołanie wystąpienie delegata, którego lista wywołania zawiera wiele pozycji rozpoczynające się za pomocą wywołania tych metod na liście wywołania synchronicznie, w kolejności. Każda metoda tak zwane jest przekazywany tego samego zestawu argumentów, jak podano na wystąpienie delegata. Jeśli wywołanie delegata zawierają parametry odwołań ([odwołania do parametrów](classes.md#reference-parameters)), nastąpi każdego wywołania metody w odniesieniu do tej samej zmiennej; zostaną zmiany w tej zmiennej według jedną metodę liście wywołania widoczna dalsze metod w dół listy wywołań. Jeśli wywołanie delegata zawiera parametry wyjściowe lub wartości zwracanej, ich wartość końcową będą pochodzić z wywołanie delegata ostatniej, na liście.

Jeśli wystąpi wyjątek podczas przetwarzania wywołania takiego delegata, a ten wyjątek nie jest wyłapywany wewnątrz metody, która została wywołana, wyszukaj klauzuli catch wyjątku jest kontynuowane w metodzie, która wywołuje delegata, a wszystkie metody dalszej części na liście wywołania nie są wywoływane.

Próba wywołania wystąpienie delegata, w których wartość wynosi null powoduje wyjątek typu `System.NullReferenceException`.

Poniższy przykład pokazuje, jak utworzyć wystąpienie, łączenie, Usuń i wywoływać delegatów:

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

Jak pokazano w instrukcji `cd3 += cd1;`, obiekt delegowany mogą być obecne na liście wywołania wiele razy. W tym przypadku jest po prostu wywoływana raz na wystąpienie. Na liście wywołania takich po usunięciu delegata ostatniego wystąpienia na liście wywołania jest usuwany.

Bezpośrednio przed wykonaniem instrukcji końcowego `cd3 -= cd1;`, delegat `cd3` odwołuje się do listy wywołań puste. Próba usunięcia delegata z pustej listy (lub usuwanie delegata nieistniejącej z listy Niepuste) nie jest błąd.

Dane wyjściowe, generowane są:

```
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
