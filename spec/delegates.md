---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704098"
---
# <a name="delegates"></a>Delegaty

Delegaty umożliwiają scenariusze, które inne języki — C++takie jak Pascal i moduł, są rozkierowane przy użyciu wskaźników funkcji. W przeciwieństwie C++ do wskaźników funkcji Delegaty są w pełni zorientowane obiektowo i w C++ przeciwieństwie do wskaźników do funkcji składowych, Delegaty hermetyzują zarówno wystąpienie obiektu, jak i metodę.

Deklaracja delegata definiuje klasę, która pochodzi od klasy `System.Delegate`. Wystąpienie delegata hermetyzuje listę wywołań, która jest listą jednej lub więcej metod, z których każdy jest określany jako możliwy do wywołania. Dla metod instancji obiekt, który można wywołać, składa się z wystąpienia i metody dla tego wystąpienia. Dla metod statycznych obiekt, który można wywołać, składa się tylko z metody. Wywoływanie wystąpienia delegata z odpowiednim zestawem argumentów powoduje, że każdy możliwy do wywołania obiekt delegowany jest wywoływany z danym zestawem argumentów.

Interesująca i przydatna właściwość wystąpienia delegata polega na tym, że nie wie ani nie chroni klasy metod, które są hermetyzowane; wszystkie te kwestie polegają na tym, że te metody są zgodne ([deklaracje delegatów](delegates.md#delegate-declarations)) z typem delegata. Dzięki temu Delegaty doskonale nadają się do wywołania "anonimowe".

## <a name="delegate-declarations"></a>Deklaracje delegatów

*Delegate_declaration* jest *type_declaration* ([deklaracje typu](namespaces.md#type-declarations)), które deklaruje nowy typ delegata.

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

Jest to błąd czasu kompilacji dla tego samego modyfikatora do wyświetlenia wiele razy w deklaracji delegata.

Modyfikator `new` jest dozwolony tylko w delegatach zadeklarowanych w innym typie. w takim przypadku określa, że taki delegat ukrywa Dziedziczony element członkowski o tej samej nazwie, zgodnie z opisem w [nowym modyfikatorze](classes.md#the-new-modifier).

Modyfikatory `public`, `protected`, `internal`i `private` kontrolują dostępność typu delegata. W zależności od kontekstu, w którym występuje deklaracja delegata, niektóre z tych modyfikatorów mogą nie być dozwolone ([zadeklarowane ułatwienia dostępu](basic-concepts.md#declared-accessibility)).

Nazwa typu delegata to *Identyfikator*.

Opcjonalne *formal_parameter_list* określa parametry delegata, a *return_type* wskazuje zwracany typ delegata.

Opcjonalne *variant_type_parameter_list* ([listy parametrów typu Variant](interfaces.md#variant-type-parameter-lists)) określają parametry typu dla samego delegata.

Typem zwracanym typu delegata musi być `void`lub danych wyjściowych ([odchylenie zabezpieczeń](interfaces.md#variance-safety)).

Wszystkie typy parametrów formalnych typu delegata muszą być bezpieczne dla danych wejściowych. Ponadto wszystkie typy parametrów `out` i `ref` muszą być również bezpieczne w danych wyjściowych. Należy zauważyć, że nawet `out` parametry muszą być bezpieczne dla danych wejściowych z powodu ograniczenia podstawowej platformy wykonywania.

Typy delegatów C# w programie są odpowiednikami nazw, a nie są odpowiednikami strukturalnymi. W każdym przypadku dwa różne typy delegatów, które mają te same listy parametrów i typ zwracany, są traktowane jako różne typy delegatów. Jednak wystąpienia dwóch różnych, ale strukturalnie równoważnych typów delegatów mogą być porównywane jako równe ([delegatów równości](expressions.md#delegate-equality-operators)).

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

Metody `A.M1` i `B.M1` są zgodne z typami delegatów `D1` i `D2`, ponieważ mają ten sam typ zwracany i listę parametrów; Jednak te typy delegatów są dwa różne typy, więc nie są zamienne. Metody `B.M2`, `B.M3`i `B.M4` są niezgodne z typami delegatów `D1` i `D2`, ponieważ mają różne typy zwracane lub listy parametrów.

Podobnie jak w przypadku innych deklaracji typu ogólnego, należy pomieścić argumenty typu, aby utworzyć skonstruowany typ delegata. Typy parametrów i typ zwracany skonstruowanego typu delegata są tworzone przez podstawianie, dla każdego parametru typu w deklaracji delegata, odpowiadającego mu argumentu typu złożonego typu delegata. Wynikowy typ zwracany i typy parametrów są używane w celu określenia, jakie metody są zgodne ze skonstruowanym typem delegata. Na przykład:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

Metoda `X.F` jest zgodna z typem delegata `Predicate<int>`, a metoda `X.G` jest zgodna z typem delegata `Predicate<string>`.

Jedynym sposobem deklarowania typu delegata jest za pośrednictwem *delegate_declaration*. Typ delegata jest typem klasy, który jest pochodną `System.Delegate`. Typy delegatów są niejawnie `sealed`, więc nie jest dozwolone, aby dziedziczyć dowolny typ z typu delegata. Nie jest również dozwolone uzyskanie typu klasy niedelegowanej z `System.Delegate`. Należy zauważyć, że `System.Delegate` nie należy do typu delegata; jest to typ klasy, z której są wyprowadzane wszystkie typy delegatów.

C#zawiera specjalną składnię dla tworzenia wystąpień delegata i wywołania. Oprócz tworzenia wystąpienia, każda operacja, którą można zastosować do klasy lub wystąpienia klasy, można również zastosować odpowiednio do klasy lub wystąpienia obiektu delegowanego. W szczególności możliwe jest uzyskanie dostępu do elementów członkowskich typu `System.Delegate` za pośrednictwem standardowej składni dostępu do elementu członkowskiego.

Zestaw metod, które są hermetyzowane przez wystąpienie delegata, nazywa się listą wywołania. Po utworzeniu wystąpienia delegata ([delegowanie zgodności](delegates.md#delegate-compatibility)) z pojedynczej metody hermetyzuje tę metodę, a jej Lista wywołań zawiera tylko jeden wpis. Jednak gdy dwa wystąpienia delegatów o wartości innej niż null są łączone, ich listy wywołań są łączone — w pozostałym operandzie Order Right a prawy operand — aby utworzyć nową listę wywołań, która zawiera co najmniej dwa wpisy.

Delegaty są łączone za pomocą `+` binarnych ([operator dodawania](expressions.md#addition-operator)) i operatory `+=` ([przypisanie złożone](expressions.md#compound-assignment)). Delegat może zostać usunięty z kombinacji delegatów, przy użyciu `-` binarny ([operator odejmowania](expressions.md#subtraction-operator)) i operatory `-=` ([przypisanie złożone](expressions.md#compound-assignment)). Delegatów można porównywać pod kątem równości ([delegatów kryterium równości](expressions.md#delegate-equality-operators)).

Poniższy przykład pokazuje wystąpienie liczby delegatów i odpowiadające im listy wywołań:

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

Po utworzeniu wystąpienia `cd1` i `cd2` są one hermetyzowane jedną metodą. Gdy `cd3` jest tworzone wystąpienie, zawiera listę wywołań dwóch metod, `M1` i `M2`w tej kolejności. Lista wywołań `cd4`zawiera `M1`, `M2`i `M1`w tej kolejności. Na końcu Lista wywołań `cd5`zawiera `M1`, `M2`, `M1`, `M1`i `M2`w tej kolejności. Aby uzyskać więcej przykładów łączenia (jak również usuwania) delegatów, zobacz [delegating wywołania](delegates.md#delegate-invocation).

## <a name="delegate-compatibility"></a>Delegowanie zgodności

Metoda lub delegat `M` jest ***zgodny*** z typem delegata `D` jeśli spełnione są wszystkie następujące warunki:

*  `D` i `M` mają tę samą liczbę parametrów, a każdy parametr w `D` ma takie same Modyfikatory `ref` lub `out`, jak odpowiedni parametr w `M`.
*  Dla każdego parametru wartości (parametru bez modyfikatora `ref` lub `out`), konwersja tożsamości ([konwersja tożsamości](conversions.md#identity-conversion)) lub niejawna konwersja odwołania ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions)) istnieje z typu parametru w `D` do odpowiedniego typu parametru w `M`.
*  Dla każdego `ref` lub `out` parametru typ parametru w `D` jest taki sam jak typ parametru w `M`.
*  Dla zwracanego typu `D``M` istnieje konwersja niejawnego odwołania.

## <a name="delegate-instantiation"></a>Tworzenie wystąpienia delegata

Wystąpienie delegata jest tworzone przez *delegate_creation_expression* (autorzy[tworzenia delegatów](expressions.md#delegate-creation-expressions)) lub konwersję do typu delegata. Nowo utworzone wystąpienie delegata odnosi się do:

*  Metoda statyczna, do której odwołuje się *delegate_creation_expression*, lub
*  Obiekt docelowy (którego nie można `null`) i metoda wystąpienia, do której odwołuje się *delegate_creation_expression*lub
*  Inny delegat.

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

Po utworzeniu wystąpienia obiekty delegatów zawsze odwołują się do tego samego obiektu docelowego i metody. Pamiętaj, że gdy dwa Delegaty są łączone lub jeden z nich jest usuwany z innego, nowy delegat ma własną listę wywołań; listy wywołań delegatów połączonych lub usuniętych pozostaną niezmienione.

## <a name="delegate-invocation"></a>Delegowanie wywołania

C#zawiera specjalną składnię do wywoływania delegata. Gdy wystąpienie delegata o wartości innej niż null, którego lista wywołań zawiera jeden wpis, wywołuje jedną metodę z tymi samymi argumentami i zwraca tę samą wartość, co odwołanie do metody. (Zobacz [delegowanie wywołań](expressions.md#delegate-invocations) , aby uzyskać szczegółowe informacje na temat delegowania wywołania.) Jeśli wystąpi wyjątek w trakcie wywołania tego delegata, a ten wyjątek nie jest przechwytywany w metodzie, która została wywołana, wyszukiwanie w klauzuli catch wyjątku kontynuuje się w metodzie, która wywołała delegata, tak jakby ta metoda miała bezpośrednio nazwę metody, do której odwołuje się ten delegat.

Wywołanie wystąpienia delegata, którego lista wywołania zawiera wiele wpisów, jest realizowane przez wywołanie każdej metody z listy wywołań synchronicznie, w kolejności. Każda metoda wywołana jest przenoszona do tego samego zestawu argumentów, co zostało przekazane do wystąpienia delegata. Jeśli takie wywołanie delegata zawiera parametry odwołania ([parametry odwołania](classes.md#reference-parameters)), każde wywołanie metody zostanie wykonane z odwołaniem do tej samej zmiennej; zmiany tej zmiennej według jednej metody na liście wywołań będą widoczne dla metod w dalszej postaci listy wywołań. Jeśli delegat delegowany zawiera parametry wyjściowe lub wartość zwracaną, ich końcowa wartość będzie pochodzić z wywołania ostatniego delegata na liście.

Jeśli wystąpi wyjątek podczas przetwarzania wywołania tego delegata, a ten wyjątek nie jest przechwytywany w metodzie, która została wywołana, wyszukiwanie w klauzuli catch wyjątku kontynuuje się w metodzie, która wywołała delegata, i inne inne metody Lista wywołań nie została wywołana.

Próba wywołania wystąpienia delegata, którego wartość jest równa null, spowoduje wyjątek typu `System.NullReferenceException`.

Poniższy przykład pokazuje, jak tworzyć wystąpienia, łączyć, usuwać i wywoływać delegatów:

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

Jak pokazano w instrukcji `cd3 += cd1;`, delegat może być obecny na liście wywołań wiele razy. W tym przypadku jest to po prostu wywoływana raz na wystąpienie. Na liście wywołań, takiej jak w przypadku usunięcia tego delegata, ostatnie wystąpienie na liście wywołań jest aktualnie usunięte.

Bezpośrednio przed wykonaniem ostatecznej instrukcji `cd3 -= cd1;`delegat `cd3` odnosi się do pustej listy wywołań. Próba usunięcia delegata z pustej listy (lub usunięcia nieistniejącej delegata z listy niepustej) nie jest błędem.

Tworzone dane wyjściowe to:

```console
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
