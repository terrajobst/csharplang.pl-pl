---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703985"
---
# <a name="classes"></a>Klasy

Klasa jest strukturą danych, która może zawierać składowe danych (stałe i pola), składowe funkcji (metody, właściwości, zdarzenia, indeksatory, operatory, konstruktory wystąpień, destruktory i konstruktory statyczne) i typy zagnieżdżone. Typy klas obsługują dziedziczenie, mechanizm, za pomocą którego Klasa pochodna może zwiększyć i specjalizację klasy bazowej.

## <a name="class-declarations"></a>Deklaracje klas

*Class_declaration* jest *type_declaration* ([deklaracje typu](namespaces.md#type-declarations)), które deklaruje nową klasę.

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

*Class_declaration* składa się z opcjonalnego zestawu *atrybutów* ([atrybutów](attributes.md)), po którym następuje opcjonalny zestaw *class_modifier*s ([Modyfikatory klas](classes.md#class-modifiers)), po którym następuje opcjonalny modyfikator `partial`, po którym następuje `class` słowa kluczowego i *Identyfikator* , który nadaje nazwę klasy, a po nim opcjonalne *type_parameter_list* ([parametry typu](classes.md#type-parameters)), a po nim opcjonalne specyfikacje *class_base* ([Specyfikacja bazowa klasy](classes.md#class-base-specification)), po których następuje przez opcjonalny zestaw *type_parameter_constraints_clause*s ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), po którym następuje *class_body* ([Treść klasy](classes.md#class-body)), opcjonalnie następuje średnik.

Deklaracja klasy nie może podawać *type_parameter_constraints_clause*s, chyba że zawiera również *type_parameter_list*.

Deklaracja klasy dostarczająca *type_parameter_list* jest ***deklaracją klasy ogólnej***. Ponadto każda klasa zagnieżdżona wewnątrz deklaracji klasy generycznej lub ogólnej deklaracji struktury jest sama deklaracją klasy ogólnej, ponieważ parametry typu dla typu zawierającego muszą zostać dostarczone, aby utworzyć typ skonstruowany.

### <a name="class-modifiers"></a>Modyfikatory klas

*Class_declaration* może opcjonalnie zawierać sekwencję modyfikatorów klasy:

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

Jest to błąd czasu kompilacji dla tego samego modyfikatora do wyświetlenia wiele razy w deklaracji klasy.

Modyfikator `new` jest dozwolony dla zagnieżdżonych klas. Określa, że Klasa ukrywa dziedziczonego elementu członkowskiego o tej samej nazwie, zgodnie z opisem w [nowym modyfikatorze](classes.md#the-new-modifier). Jest to błąd czasu kompilacji dla modyfikatora `new`, który ma być wyświetlany w deklaracji klasy, która nie jest deklaracją klasy zagnieżdżonej.

Modyfikatory `public`, `protected`, `internal`i `private` kontrolują ułatwienia dostępu dla klasy. W zależności od kontekstu, w którym występuje deklaracja klasy, niektóre z tych modyfikatorów mogą nie być dozwolone ([zadeklarowane ułatwienia dostępu](basic-concepts.md#declared-accessibility)).

Modyfikatory `abstract`, `sealed` i `static` zostały omówione w poniższych sekcjach.

#### <a name="abstract-classes"></a>Klasy abstrakcyjne

Modyfikator `abstract` jest używany do wskazania, że Klasa jest niekompletna i że jest przeznaczona do użycia tylko jako klasa bazowa. Klasa abstrakcyjna różni się od klasy nieabstrakcyjnej w następujący sposób:

*  Nie można bezpośrednio utworzyć wystąpienia klasy abstrakcyjnej i jest to błąd czasu kompilacji, aby użyć operatora `new` w klasie abstrakcyjnej. Chociaż możliwe jest posiadanie zmiennych i wartości, których typy czasu kompilacji są abstrakcyjne, takie zmienne i wartości muszą być `null` lub zawierają odwołania do wystąpień klas nieabstrakcyjnych pochodzących od typów abstrakcyjnych.
*  Klasa abstrakcyjna jest dozwolona (ale nie jest wymagana), aby zawierała abstrakcyjne elementy członkowskie.
*  Klasa abstrakcyjna nie może być zapieczętowana.

Gdy Klasa nieabstrakcyjna jest pochodną klasy abstrakcyjnej, Klasa nieabstrakcyjna musi zawierać rzeczywiste implementacje wszystkich dziedziczonych abstrakcyjnych elementów członkowskich, co zastępuje te abstrakcyjne elementy członkowskie. w przykładzie
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
Klasa abstrakcyjna `A` wprowadza metodę abstrakcyjną `F`. Klasa `B` wprowadza dodatkową metodę `G`, ale ponieważ nie zapewnia implementacji `F`, `B` musi być zadeklarowana jako abstract. Klasa `C` przesłania `F` i zapewnia rzeczywistą implementację. Ponieważ w `C`nie ma abstrakcyjnych elementów członkowskich, `C` jest dozwolony (ale nie jest wymagany) jako nieabstrakcyjny.

#### <a name="sealed-classes"></a>Zapieczętowane klasy

Modyfikator `sealed` służy do zapobiegania wyprowadzaniu z klasy. Błąd czasu kompilacji występuje, jeśli Klasa zapieczętowana została określona jako klasa bazowa innej klasy.

Klasa zapieczętowana nie może być również klasą abstrakcyjną.

Modyfikator `sealed` jest używany głównie do zapobiegania niezamierzonemu wyznaczeniu, ale również zapewnia pewne optymalizacje w czasie wykonywania. W szczególności ze względu na to, że Klasa zapieczętowana nie ma żadnych klas pochodnych, istnieje możliwość przekształcenia wywołań elementów członkowskich funkcji wirtualnych w wystąpieniach klasy zapieczętowanej na wywołania niewirtualne.

#### <a name="static-classes"></a>Klasy statyczne

Modyfikator `static` służy do oznaczania klasy zadeklarowanej jako ***Klasa statyczna***. Nie można utworzyć wystąpienia klasy statycznej, nie można jej użyć jako typu i może zawierać tylko statyczne elementy członkowskie. Tylko Klasa statyczna może zawierać deklaracje metod rozszerzających ([metody rozszerzające](classes.md#extension-methods)).

Deklaracja klasy statycznej podlega następującym ograniczeniom:

*  Klasa statyczna nie może zawierać modyfikatora `sealed` ani `abstract`. Należy jednak pamiętać, że ponieważ od klasy statycznej nie można utworzyć wystąpienia ani pochodnej, zachowuje się tak, jakby było zapieczętowane i abstrakcyjne.
*  Klasa statyczna nie może zawierać specyfikacji *class_base* ([specyfikacji klasy podstawowej](classes.md#class-base-specification)) i nie może jawnie określić klasy bazowej lub listy zaimplementowanych interfejsów. Klasa statyczna niejawnie dziedziczy po typie `object`.
*  Klasa statyczna może zawierać tylko statyczne elementy członkowskie ([statyczne i składowe wystąpienia](classes.md#static-and-instance-members)). Należy zauważyć, że stałe i zagnieżdżone typy są klasyfikowane jako statyczne elementy członkowskie.
*  Klasa statyczna nie może mieć elementów członkowskich z zazadeklarowaną dostępnością `protected` lub `protected internal`.

Jest to błąd czasu kompilacji, który narusza którekolwiek z tych ograniczeń.

Klasa statyczna nie ma konstruktorów wystąpień. Nie można zadeklarować konstruktora wystąpienia w klasie statycznej i nie podano domyślnego konstruktora wystąpień ([konstruktorów domyślnych](classes.md#default-constructors)) dla klasy statycznej.

Elementy członkowskie klasy statycznej nie są automatycznie statyczne i deklaracje składowych muszą jawnie zawierać modyfikator `static` (z wyjątkiem stałych i zagnieżdżonych typów). Gdy Klasa jest zagnieżdżona w obrębie statycznej klasy zewnętrznej, Klasa zagnieżdżona nie jest klasą statyczną, chyba że jawnie zawiera modyfikator `static`.

__Odwołujące się do typów klas statycznych__

*Namespace_or_type_name* ([przestrzeń nazw i nazwy typów](basic-concepts.md#namespace-and-type-names)) mogą odwoływać się do klasy statycznej, jeśli

*  *Namespace_or_type_name* jest `T` w *namespace_or_type_name* `T.I`formularz, lub
*  *Namespace_or_type_name* jest `T` w *typeof_expression* ([Argument Lists](expressions.md#argument-lists)1) `typeof(T)`formularza.

*Primary_expression* ([Członkowie funkcji](expressions.md#function-members)) mogą odwoływać się do klasy statycznej, jeśli

*  *Primary_expression* jest `E` w *member_access* ([Sprawdzanie czasu kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) `E.I`.

W każdym innym kontekście jest to błąd czasu kompilacji, który odwołuje się do klasy statycznej. Na przykład jest to błąd dla klasy statycznej, która ma być używana jako klasa bazowa, typ składnika ([typy zagnieżdżone](classes.md#nested-types)) elementu członkowskiego, argument typu ogólnego lub ograniczenie parametru typu. Podobnie Klasa statyczna nie może być używana w typie tablicy, typ wskaźnika, wyrażenie `new`, wyrażenie rzutowania, wyrażenie `is`, wyrażenie `as`, wyrażenie `sizeof` lub wyrażenie wartości domyślnej.

### <a name="partial-modifier"></a>Modyfikator częściowy

Modyfikator `partial` jest używany do wskazania, że ta *class_declaration* jest deklaracją typu częściowego. Wiele deklaracji typu częściowego o tej samej nazwie w otaczającej przestrzeni nazw lub deklaracji typu Połącz, aby utworzyć jedną deklarację typu, zgodnie z regułami określonymi w [typach częściowych](classes.md#partial-types).

Posiadanie deklaracji klasy rozproszonej za pomocą oddzielnych segmentów tekstu programu może być przydatne, jeśli te segmenty są produkowane lub utrzymywane w różnych kontekstach. Na przykład jedna część deklaracji klasy może być wygenerowana maszyną, podczas gdy druga zostaje ręcznie utworzona. Oddzielenie tekstu między nimi uniemożliwia aktualizację przez jeden z powodu konfliktu z aktualizacjami.

### <a name="type-parameters"></a>Parametry typu

Parametr typu jest prostym identyfikatorem, który wskazuje symbol zastępczy argumentu typu dostarczonego do utworzenia konstruowanego typu. Parametr typu jest formalnym symbolem zastępczym dla typu, który zostanie dostarczony później. Z kolei argument typu ([argumenty typu](types.md#type-arguments)) jest rzeczywistym typem, który jest zastępowany dla parametru typu podczas tworzenia konstruowanego typu.

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

Każdy parametr typu w deklaracji klasy definiuje nazwę w obszarze deklaracji ([deklaracji](basic-concepts.md#declarations)) tej klasy. Dlatego nie może mieć takiej samej nazwy jak inny parametr typu lub element członkowski zadeklarowany w tej klasie. Parametr typu nie może mieć takiej samej nazwy jak sam typ.

### <a name="class-base-specification"></a>Specyfikacja bazowa klasy

Deklaracja klasy może zawierać specyfikację *class_base* , która definiuje bezpośrednią klasę bazową klasy i interfejsy ([interfejsy](interfaces.md)) bezpośrednio zaimplementowane przez klasę.

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

Klasa bazowa określona w deklaracji klasy może być skonstruowanym typem klasy ([skonstruowane typy](types.md#constructed-types)). Klasa bazowa nie może być parametrem typu, ale może zawierać parametry typu, które znajdują się w zakresie.

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>Klas podstawowych

Gdy *class_type* jest uwzględniona w *class_base*, określa bezpośrednią klasę bazową zadeklarowanej klasy. Jeśli deklaracja klasy nie ma *class_base*lub *class_base* wyświetla tylko typy interfejsów, przyjmuje się, że bezpośrednia klasa bazowa jest `object`. Klasa dziedziczy składowe z bezpośredniej klasy podstawowej, zgodnie z opisem w [dziedziczeniu](classes.md#inheritance).

w przykładzie
```csharp
class A {}

class B: A {}
```
Klasa `A` ma być bezpośrednią klasą bazową `B`, a `B` jest określana jako pochodna z `A`. Ponieważ `A` nie określa jawnie bezpośredniej klasy bazowej, jej bezpośrednia klasa bazowa jest niejawnie `object`.

W przypadku konstruowanego typu klasy, jeśli klasa bazowa jest określona w deklaracji klasy generycznej, Klasa bazowa typu konstruowanego jest uzyskiwana przez podstawianie dla każdego *type_parameter* w deklaracji klasy bazowej, odpowiadającego *type_argument* typu złożonego. Podaną deklaracje klas ogólnych
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
Klasa bazowa złożonego typu `G<int>` zostałaby `B<string,int[]>`.

Bezpośrednia klasa bazowa typu klasy musi być co najmniej równa dostępności jako samego typu klasy ([domeny dostępności](basic-concepts.md#accessibility-domains)). Na przykład jest to błąd czasu kompilowania dla klasy `public`, która dziedziczy z klasy `private` lub `internal`.

Bezpośrednia klasa bazowa typu klasy nie może być żadnym z następujących typów: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`ani `System.ValueType`. Ponadto deklaracja klasy generycznej nie może używać `System.Attribute` jako bezpośredniej lub pośredniej klasy bazowej.

Podczas określania znaczenia bezpośredniej specyfikacji klasy bazowej `A` klasy `B`, przyjmuje się, że bezpośrednia klasa bazowa `B` jest tymczasowo `object`. Intuicyjnie gwarantuje to, że znaczenie specyfikacji klasy bazowej nie może rekursywnie zależeć od siebie. Przykład:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
Wystąpił błąd, ponieważ w specyfikacji klasy bazowej `A<C.B>` bezpośrednia klasa bazowa `C` jest uznawana za `object`, a tym samym (zgodnie z regułami [przestrzeni nazw i nazw typów](basic-concepts.md#namespace-and-type-names)) `C` nie jest uważana za element członkowski `B`.

Klasy bazowe typu klasy są bezpośrednią klasą bazową i jej klasami podstawowymi. Innymi słowy, zestaw klas bazowych jest przechodnim zamknięciem bezpośredniej relacji między klasami podstawowymi. W odniesieniu do powyższego przykładu klasy bazowe `B` są `A` i `object`. w przykładzie
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
klasy bazowe `D<int>` są `C<int[]>`, `B<IComparable<int[]>>`, `A`i `object`.

Oprócz klasy `object`każdy typ klasy ma dokładnie jedną bezpośrednią klasę bazową. Klasa `object` nie ma bezpośredniej klasy podstawowej i jest ostateczną klasą bazową wszystkich innych klas.

Gdy Klasa `B` dziedziczy z klasy `A`, jest to błąd czasu kompilacji dla `A` zależnie od `B`. Klasa ***bezpośrednio zależy*** od jej bezpośredniej klasy podstawowej (jeśli istnieje) i ***bezpośrednio zależy*** od klasy, w której jest od razu zagnieżdżona (jeśli istnieje). W związku z tą definicją kompletny zestaw klas, od których zależy Klasa, jest bezpośrednie i przechodnie zamknięcie ***bezpośredniego zależy*** od relacji.

Przykład
```csharp
class A: A {}
```
jest błędem, ponieważ Klasa zależy od siebie samej. Podobnie, przykład
```csharp
class A: B {}
class B: C {}
class C: A {}
```
Wystąpił błąd, ponieważ klasy cyklicznie zależą od siebie. Na koniec przykład
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
powoduje błąd czasu kompilacji, ponieważ `A` zależy od `B.C` (jego bezpośrednia klasa bazowa), która zależy od `B` (bezpośrednio otaczającej klasy), która cyklicznie zależy od `A`.

Należy zauważyć, że Klasa nie zależy od klas, które są w niej zagnieżdżone. w przykładzie
```csharp
class A
{
    class B: A {}
}
```
`B` zależy od `A` (ponieważ `A` jest zarówno bezpośrednią klasą bazową, jak i bezpośrednio otaczającą ją klasą), ale `A` nie zależy od `B` (ponieważ `B` nie jest klasą bazową ani otaczającą klasę `A`). W tym przypadku przykład jest prawidłowy.

Nie można dziedziczyć z klasy `sealed`. w przykładzie
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
Wystąpił błąd klasy `B`, ponieważ próbuje dziedziczyć z klasy `sealed` `A`.

#### <a name="interface-implementations"></a>Implementacje interfejsu

Specyfikacja *class_base* może zawierać listę typów interfejsów, w takim przypadku Klasa jest określana do bezpośredniego zaimplementowania danego typu interfejsu. Implementacje interfejsów omówiono dokładniej w [implementacjach interfejsu](interfaces.md#interface-implementations).

### <a name="type-parameter-constraints"></a>Ograniczenia parametru typu

Deklaracje typu ogólnego i metody mogą opcjonalnie określać ograniczenia parametrów typu przez uwzględnienie *type_parameter_constraints_clause*s.

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

Każdy *type_parameter_constraints_clause* składa się z `where`tokena, po którym następuje nazwa parametru typu, po którym następuje dwukropek i lista ograniczeń dla tego parametru typu. Dla każdego parametru typu może istnieć co najwyżej jedna klauzula `where`, a klauzule `where` mogą być wyświetlane w dowolnej kolejności. Podobnie jak tokeny `get` i `set` w metodzie dostępu do właściwości, token `where` nie jest słowem kluczowym.

Lista ograniczeń podanych w klauzuli `where` może zawierać dowolny z następujących składników, w następującej kolejności: jedno ograniczenie podstawowe, jedno lub więcej ograniczeń pomocniczych i ograniczenie konstruktora, `new()`.

Ograniczenie podstawowe może być typem klasy lub ***ograniczeniem typu referencyjnego*** `class` lub `struct`***ograniczenia typu wartości*** . Ograniczenie pomocnicze może być *type_parameter* lub *INTERFACE_TYPE*.

Ograniczenie typu odwołania określa, że argument typu użyty dla parametru typu musi być typem referencyjnym. Wszystkie typy klas, typy interfejsów, typy delegatów, typy tablic i parametry typu znane jako typ referencyjny (zgodnie z definicją poniżej) spełniają to ograniczenie.

Ograniczenie typu wartości określa, że argument typu użyty dla parametru typu musi być typem wartości niedopuszczający wartości null. Wszystkie typy struktur niedopuszczające wartości null, typy wyliczeniowe i parametry typu mające ograniczenie typu wartości spełniają to ograniczenie. Należy zauważyć, że chociaż sklasyfikowane jako typ wartości, typ dopuszczający wartość null ([Typy dopuszczające wartości null](types.md#nullable-types)) nie spełnia ograniczenia typu wartości. Parametr typu z ograniczeniem typu wartości nie może mieć również *constructor_constraint*.

Typy wskaźników nigdy nie mogą być argumentami typu i nie są uważane za spełniające ograniczenia typu odwołania lub typu wartości.

Jeśli ograniczenie jest typem klasy, typem interfejsu lub parametrem typu, ten typ określa minimalny "typ podstawowy", który każdy argument typu użyty dla tego parametru typu musi obsługiwać. Za każdym razem, gdy jest używany typ skonstruowany lub metoda ogólna, argument typu jest sprawdzany względem ograniczeń w parametrze typu w czasie kompilacji. Dostarczony argument typu musi spełniać warunki opisane w temacie [spełnianie ograniczeń](types.md#satisfying-constraints).

Ograniczenie *class_type* musi spełniać następujące reguły:

*  Typ musi być typem klasy.
*  Typ nie może być `sealed`.
*  Typ nie może być jednym z następujących typów: `System.Array`, `System.Delegate`, `System.Enum`lub `System.ValueType`.
*  Typ nie może być `object`. Ponieważ wszystkie typy pochodzą od `object`, takie ograniczenie nie będzie miało wpływu, jeśli było dozwolone.
*  Co najwyżej jedno ograniczenie dla danego parametru typu może być typem klasy.

Typ określony jako ograniczenie *INTERFACE_TYPE* musi spełniać następujące reguły:

*  Typ musi być typem interfejsu.
*  Typ nie może być określony więcej niż raz w danej klauzuli `where`.

W obu przypadkach ograniczenie może dotyczyć dowolnego z parametrów typu skojarzonej deklaracji typu lub metody w ramach typu złożonego i może dotyczyć zadeklarowanego typu.

Każdy typ klasy lub interfejsu określony jako ograniczenie parametru typu musi być co najmniej jako dostępny ([ograniczenia dostępu](basic-concepts.md#accessibility-constraints)) jako zadeklarowany typ ogólny lub metoda.

Typ określony jako ograniczenie *type_parameter* musi spełniać następujące reguły:

*  Typ musi być parametrem typu.
*  Typ nie może być określony więcej niż raz w danej klauzuli `where`.

Ponadto nie może istnieć żadne cykle w grafie zależności parametrów typu, gdzie zależność jest relacją przechodnią zdefiniowaną przez:

*  Jeśli parametr typu `T` jest używany jako ograniczenie dla parametru typu `S`, `S` ***zależy od*** `T`.
*  Jeśli parametr typu `S` zależy od parametru typu `T` i `T` zależy od parametru typu `U`, `S` ***zależy od*** `U`.

Ta relacja jest błędem czasu kompilowania dla parametru typu, aby zależała od samego siebie (bezpośrednio lub pośrednio).

Wszystkie ograniczenia muszą być spójne między zależnymi parametrami typu. Jeśli parametr typu `S` zależy od parametru typu `T`, wówczas:

*  `T` nie może mieć ograniczenia typu wartości. W przeciwnym razie `T` jest skutecznie zapieczętowany, dlatego `S` zostanie wymuszone tego samego typu co `T`, eliminując konieczność stosowania dwóch parametrów typu.
*  Jeśli `S` ma ograniczenie typu wartości, `T` nie może mieć ograniczenia *class_type* .
*  Jeśli `S` ma ograniczenie *class_type* `A` i `T` ma ograniczenie *class_type* `B`, należy wykonać konwersję tożsamości lub niejawną konwersję odwołań z `A` na `B` lub niejawną konwersję odwołań z `B` do `A`.
*  Jeśli `S` również zależy od parametru typu `U`, a `U` ma ograniczenie *class_type* `A` i `T` ma ograniczenie *class_type* `B`, należy przeprowadzić konwersję tożsamości lub niejawną konwersję odwołań z `A` na `B` lub niejawną konwersję odwołania z `B` do `A`.

Jest on prawidłowy dla `S`, aby miał ograniczenie typu wartości i `T` mieć ograniczenia typu odwołania. Efektywnie te limity `T` do typów `System.Object`, `System.ValueType`, `System.Enum`i dowolnego typu interfejsu.

Jeśli klauzula `where` parametru typu zawiera ograniczenie konstruktora (które ma postać `new()`), można użyć operatora `new`, aby utworzyć wystąpienia typu ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)). Dowolny argument typu użyty dla parametru typu z ograniczeniem konstruktora musi mieć publiczny Konstruktor bez parametrów (ten Konstruktor niejawnie istnieje dla dowolnego typu wartości) lub być parametrem typu, który ma ograniczenie typu wartości lub ograniczenie konstruktora (zobacz [ograniczenia parametru typu](classes.md#type-parameter-constraints) , aby uzyskać szczegółowe informacje).

Poniżej przedstawiono przykłady ograniczeń:
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

Poniższy przykład dotyczy błędu, ponieważ powoduje cykliczność w grafie zależności parametrów typu:
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

W poniższych przykładach przedstawiono dodatkowe nieprawidłowe sytuacje:
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

***Obowiązująca Klasa bazowa*** parametru typu `T` jest zdefiniowana w następujący sposób:

*  Jeśli `T` nie ma ograniczeń podstawowych ani ograniczeń parametrów typu, jego skuteczna Klasa bazowa jest `object`.
*  Jeśli `T` ma ograniczenie typu wartości, jego obowiązująca Klasa bazowa jest `System.ValueType`.
*  Jeśli `T` ma ograniczenie *class_type* `C` ale nie ma żadnych ograniczeń *type_parameter* , jego obowiązująca Klasa bazowa jest `C`.
*  Jeśli `T` nie ma ograniczenia *class_type* , ale ma co najmniej jedno ograniczenie *type_parameter* , jego skuteczna Klasa bazowa to najbardziej z nich typ ([podniesione operatory konwersji](conversions.md#lifted-conversion-operators)) w zestawie skutecznych klas podstawowych ograniczeń *type_parameter* . Reguły spójności zapewniają, że ten typ jest najbardziej odnoszący się do tego typu.
*  Jeśli `T` ma ograniczenie *class_type* i co najmniej jeden *type_parameter* ograniczenia, jego obowiązująca Klasa bazowa to najbardziej z nich typ ([zniesione operatory konwersji](conversions.md#lifted-conversion-operators)) w zestawie składający się z ograniczenia *class_type* `T` i obowiązujących klas podstawowych jego ograniczeń *type_parameter* . Reguły spójności zapewniają, że ten typ jest najbardziej odnoszący się do tego typu.
*  Jeśli `T` ma ograniczenie typu odwołania, ale nie ma żadnych ograniczeń *class_type* , jego obowiązująca Klasa bazowa jest `object`.

Na potrzeby tych reguł, jeśli T ma ograniczenie `V`, które jest *value_type*, Użyj zamiast tego najbardziej konkretnego typu podstawowego `V`, który jest *class_type*. Może to nigdy potrwać jawnie określone ograniczenie, ale może wystąpić, gdy ograniczenia metody generycznej są niejawnie dziedziczone przez zastępowanie deklaracji metody lub jawną implementację metody interfejsu.

Te reguły zapewniają, że obowiązująca Klasa bazowa jest zawsze *class_type*.

***Efektywny zestaw interfejsów*** parametru typu `T` jest zdefiniowany w następujący sposób:

*  Jeśli `T` nie ma *secondary_constraints*, jego skuteczny zestaw interfejsów jest pusty.
*  Jeśli `T` ma ograniczenia *INTERFACE_TYPE* , ale nie ograniczenia *type_parameter* , jego obowiązujący zestaw interfejsów jest jego zestawem ograniczeń *INTERFACE_TYPE* .
*  Jeśli `T` nie ma ograniczeń *INTERFACE_TYPE* , ale ma ograniczenia *type_parameter* , jego skuteczny zestaw interfejsów jest złożeniem efektywnych zestawów interfejsów jego ograniczeń *type_parameter* .
*  Jeśli `T` ma zarówno ograniczenia *INTERFACE_TYPE* , jak i ograniczenia *type_parameter* , jego skuteczny zestaw interfejsów jest Unią zestawu ograniczeń *INTERFACE_TYPE* i obowiązującymi zestawami interfejsów jego ograniczeń *type_parameter* .

Parametr typu jest ***znany jako typ referencyjny*** , jeśli ma ograniczenie typu odwołania lub jego obowiązująca Klasa bazowa nie jest `object` ani `System.ValueType`.

Wartości typu parametru typu ograniczonego mogą służyć do uzyskiwania dostępu do elementów członkowskich wystąpienia IMPLIKOWANYCH przez ograniczenia. w przykładzie
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
Metody `IPrintable` mogą być wywoływane bezpośrednio na `x`, ponieważ `T` jest ograniczone do zawsze implementowania `IPrintable`.

### <a name="class-body"></a>Treść klasy

*Class_body* klasy definiuje elementy członkowskie tej klasy.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Typy częściowe

Deklarację typu można podzielić na wiele ***deklaracji typu częściowego***. Deklaracja typu jest zbudowana z jego części, zgodnie z regułami w tej sekcji, whereupon jest traktowany jako jedna deklaracja w trakcie pozostałej części czasu kompilacji i przetwarzania w czasie wykonywania programu.

*Class_declaration*, *struct_declaration* lub *interface_declaration* reprezentuje deklarację typu częściowego, jeśli zawiera modyfikator `partial`. `partial` nie jest słowem kluczowym i działa tylko jako modyfikator, jeśli występuje bezpośrednio przed jednym ze słów kluczowych `class`, `struct` lub `interface` w deklaracji typu lub przed typem `void` w deklaracji metody. W innych kontekstach można używać ich jako normalnego identyfikatora.

Każda część deklaracji typu częściowego musi zawierać modyfikator `partial`. Musi mieć taką samą nazwę i być zadeklarowane w tej samej przestrzeni nazw lub deklaracji typu co pozostałe części. Modyfikator `partial` wskazuje, że dodatkowe części deklaracji typu mogą istnieć w innym miejscu, ale istnienie takich dodatkowych części nie jest wymagane; jest ona prawidłowa dla typu z pojedynczą deklaracją, aby uwzględnić modyfikator `partial`.

Wszystkie części typu częściowego muszą zostać skompilowane w taki sposób, aby części mogły być scalane w czasie kompilacji w jednej deklaracji typu. Typy częściowe nie zezwalają na rozszerzone już skompilowane typy.

Zagnieżdżone typy można zadeklarować w wielu częściach przy użyciu modyfikatora `partial`. Zazwyczaj typ zawierający jest zadeklarowany przy użyciu `partial`, a każda część typu zagnieżdżonego jest zadeklarowana w innej części zawierającego go typu.

Modyfikator `partial` nie jest dozwolony w deklaracjach delegata ani wyliczeniowych.

### <a name="attributes"></a>Atrybuty

Atrybuty typu częściowego są określane przez połączenie, w nieokreślonej kolejności, atrybutów każdej części. Jeśli atrybut jest umieszczany w wielu częściach, jest równoważne do określenia atrybutu wielokrotnie w typie. Na przykład dwie części:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
są równoważne deklaracji, takiej jak:
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

Atrybuty parametrów typu łączą się w podobny sposób.

### <a name="modifiers"></a>Modyfikatory

Gdy deklaracja typu częściowego zawiera specyfikację dostępności (Modyfikatory `public`, `protected`, `internal`i `private`), musi on wyrazić zgodę na wszystkie inne części, które zawierają specyfikację dostępności. Jeśli żadna część typu częściowego nie zawiera specyfikacji dostępności, typ ma odpowiednią domyślną dostępność ([zadeklarowany dostęp](basic-concepts.md#declared-accessibility)).

Jeśli co najmniej jedna deklaracja częściowa typu zagnieżdżonego zawiera modyfikator `new`, żadne ostrzeżenie nie zostanie zgłoszone, jeśli typ zagnieżdżony ukrywa dziedziczonego elementu członkowskiego ([ukrywając przez dziedziczenie](basic-concepts.md#hiding-through-inheritance)).

Jeśli co najmniej jedna deklaracja częściowa klasy zawiera modyfikator `abstract`, Klasa jest uznawana za abstrakcyjną ([klasy abstrakcyjne](classes.md#abstract-classes)). W przeciwnym razie Klasa jest uznawana za nieabstrakcyjną.

Jeśli co najmniej jedna deklaracja częściowa klasy zawiera modyfikator `sealed`, Klasa jest uznawana za zapieczętowany ([klasy zapieczętowane](classes.md#sealed-classes)). W przeciwnym razie Klasa jest uznawana za niezapieczętowany.

Należy zauważyć, że Klasa nie może być abstrakcyjna i zapieczętowana.

Gdy modyfikator `unsafe` jest używany w deklaracji typu częściowego, tylko ta część jest traktowana jako niebezpieczny kontekst ([niebezpieczne konteksty](unsafe-code.md#unsafe-contexts)).

### <a name="type-parameters-and-constraints"></a>Parametry i ograniczenia typu

Jeśli typ ogólny jest zadeklarowany w wielu częściach, każda część musi określać parametry typu. Każda część musi mieć taką samą liczbę parametrów typu i taką samą nazwę dla każdego parametru typu, w pożądanej kolejności.

Gdy częściowa deklaracja typu ogólnego zawiera ograniczenia (klauzule`where`), ograniczenia muszą zgadzać się ze wszystkimi innymi częściami, które zawierają ograniczenia. W odniesieniu do każdej części, która zawiera ograniczenia muszą mieć ograniczenia dotyczące tego samego zestawu parametrów typu, a dla każdego parametru typu zestawy podstawowe, pomocnicze i konstruktory muszą być równoważne. Dwa zestawy ograniczeń są równoważne, jeśli zawierają te same elementy członkowskie. Jeśli żadna część częściowego typu ogólnego określa ograniczenia parametru typu, parametry typu są uznawane za nieograniczone.

Przykład
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
jest poprawna, ponieważ te części, które zawierają ograniczenia (pierwsze dwa) skutecznie określają ten sam zestaw ograniczeń podstawowych, pomocniczych i konstruktorów dla tego samego zestawu parametrów typu.

### <a name="base-class"></a>Klasa bazowa

Gdy deklaracja klasy częściowej zawiera specyfikację klasy bazowej, musi zgadzać się ze wszystkimi innymi częściami, które zawierają specyfikację klasy bazowej. Jeśli żadna część klasy częściowej nie zawiera specyfikacji klasy bazowej, Klasa bazowa stają się `System.Object` ([klasy bazowe](classes.md#base-classes)).

### <a name="base-interfaces"></a>Interfejsy podstawowe

Zestaw interfejsów podstawowych dla typu zadeklarowanego w wielu częściach jest złożeniem interfejsów podstawowych określonych w każdej części. Określony interfejs podstawowy może być nazwany tylko raz w każdej części, ale jest dozwolony dla wielu części, aby nazwać te same interfejsy podstawowe. Może istnieć tylko jedna implementacja elementów członkowskich dowolnego interfejsu podstawowego.

w przykładzie
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
zestaw interfejsów podstawowych dla klasy `C` jest `IA`, `IB`i `IC`.

Zazwyczaj każda część zawiera implementację interfejsów zadeklarowanych w tej części; nie jest to jednak wymagane. Część może zapewnić implementację interfejsu zadeklarowanego w innej części:
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a>Members

Z wyjątkiem metod częściowych ([metod częściowych](classes.md#partial-methods)) zestaw elementów członkowskich typu zadeklarowanych w wielu częściach jest po prostu Unią zestawu elementów członkowskich zadeklarowanych w każdej części. Ciała wszystkich części deklaracji typu współdzielą ten sam obszar deklaracji ([deklaracje](basic-concepts.md#declarations)), a zakres każdego elementu członkowskiego ([zakresów](basic-concepts.md#scopes)) rozciąga się na treść wszystkich części. Domena dostępności dowolnego elementu członkowskiego zawsze zawiera wszystkie części typu otaczającego; członek `private` zadeklarowany w jednej części jest swobodnie dostępny z innej części. Jest to błąd czasu kompilacji, który deklaruje ten sam element członkowski w więcej niż jednej części typu, chyba że ten element członkowski jest typem z modyfikatorem `partial`.

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

Kolejność elementów członkowskich w ramach typu jest rzadko istotna dla C# kodu, ale może być istotna w przypadku współdziałania z innymi językami i środowiskami. W takich przypadkach kolejność elementów członkowskich w typie zadeklarowanym w wielu częściach jest niezdefiniowana.

### <a name="partial-methods"></a>Metody częściowe

Metody częściowe można definiować w jednej części deklaracji typu i zaimplementowane w innej. Implementacja jest opcjonalna; Jeśli żadna część nie implementuje metody częściowej, deklaracja częściowej metody i wszystkie wywołania do niej są usuwane z deklaracji typu, która wynika z kombinacji części.

Metody częściowe nie mogą definiować modyfikatorów dostępu, ale są niejawnie `private`. Typem zwracanym musi być `void`, a ich parametry nie mogą mieć modyfikatora `out`. Identyfikator `partial` jest rozpoznawany jako specjalne słowo kluczowe w deklaracji metody tylko wtedy, gdy jest wyświetlany bezpośrednio przed typem `void`; w przeciwnym razie można go użyć jako normalnego identyfikatora. Metoda częściowa nie może jawnie implementować metod interfejsu.

Istnieją dwa rodzaje deklaracji metody częściowej: Jeśli treść deklaracji metody jest średnikiem, deklaracja jest ***określana jako definicje metody częściowej***. Jeśli treść jest przyznany jako *blok*, deklaracja jest określana jako ***implementująca Deklaracja metody częściowej***. Dla części deklaracji typu może istnieć tylko jedna definiująca deklarację metody częściowej z danym podpisem, a może istnieć tylko jedna implementująca Deklaracja metody częściowej z danym podpisem. W przypadku podanej deklaracji metody częściowej należy określić odpowiadającą jej definicje metodę częściową, a deklaracje muszą być zgodne z określonymi w następujących przypadkach:

* Deklaracje muszą mieć takie same Modyfikatory (choć niekoniecznie w tej samej kolejności), nazwę metody, liczbę parametrów typu i liczbę parametrów.
* Odpowiednie parametry w deklaracjach muszą mieć takie same Modyfikatory (choć nie muszą być w tej samej kolejności) i te same typy (różnice modulo w nazwach parametrów typu).
* Odpowiednie parametry typu w deklaracjach muszą mieć takie same ograniczenia (różnice modulo w nazwach parametrów typu).

Implementacja deklaracji metody częściowej może pojawić się w tej samej części co odpowiadająca jej definiująca Deklaracja metody częściowej.

Tylko Definiowanie metody częściowej uczestniczy w rozpoznaniu przeciążenia. W tym przypadku, niezależnie od tego, czy jest określona deklaracja implementująca, wyrażenia wywołania mogą zostać rozwiązane z wywołaniami metody częściowej. Ponieważ metoda częściowa zawsze zwraca `void`, takie wyrażenia wywołania zawsze będą instrukcją wyrażenia. Ponadto ponieważ metoda częściowa jest niejawnie `private`, takie instrukcje są zawsze wykonywane w jednej z części deklaracji typu, w ramach której zadeklarowano metodę częściową.

Jeśli żadna część deklaracji typu częściowego zawiera deklarację implementującą dla danej metody częściowej, każda instrukcja wyrażenia wywołująca ją jest po prostu usunięta z złożonej deklaracji typu. W efekcie wyrażenie wywołania, łącznie ze wszystkimi wyrażeniami składowymi, nie ma wpływu na czas wykonywania. Sama metoda częściowa jest również usuwana i nie będzie składową deklaracji typu połączonego.

Jeśli istnieje deklaracja implementująca dla danej metody częściowej, wywołania metod częściowych są zachowywane. Metoda częściowa daje podstawę deklaracji metody podobnej do implementującej deklaracji metody częściowej z wyjątkiem następujących:

* Modyfikator `partial` nie jest uwzględniony
* Atrybuty w deklaracji metody otrzymanej są połączonymi atrybutami definicji i implementowania deklaracji metody częściowej w nieokreślonej kolejności. Duplikaty nie są usuwane.
* Atrybuty w parametrach deklaracji metody będącej wynikiem są połączone atrybuty odpowiednich parametrów definiujących i implementujących deklaracji metody częściowej w nieokreślonej kolejności. Duplikaty nie są usuwane.

Jeśli zdefiniowanie deklaracji, ale nie deklaracji implementującej, jest podano dla metody częściowej M, obowiązują następujące ograniczenia:

* Jest to błąd czasu kompilacji służący do tworzenia delegata metody ([delegatów tworzenia obiektów delegowanych](expressions.md#delegate-creation-expressions)).
* Jest to błąd czasu kompilacji, który odwołuje się do `M` wewnątrz funkcji anonimowej, która jest konwertowana na typ drzewa wyrażenia ([Obliczanie konwersji funkcji anonimowych na typy drzewa wyrażeń](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).
* Wyrażenia występujące jako część wywołania `M` nie wpływają na określony stan przypisania ([przypisanie określone](variables.md#definite-assignment)), co może potencjalnie doprowadzić do błędów czasu kompilacji.
* `M` nie może być punktem wejścia dla aplikacji ([Uruchamianie aplikacji](basic-concepts.md#application-startup)).

Metody częściowe są przydatne do umożliwienia jednej części deklaracji typu, aby dostosować zachowanie innej części, np., która jest generowana przez narzędzie. Rozważmy następującą deklarację klasy częściowej:
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

Jeśli ta klasa jest skompilowana bez żadnych innych części, definicje deklaracji metody częściowej i ich wywołania zostaną usunięte, a wynikiem połączonej deklaracji klasy będzie odpowiednik następujących elementów:
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

Załóżmy, że inna część została określona, Jednakże, która zawiera deklaracje implementacji metod częściowych:
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

Następnie wynikiem połączonej deklaracji klasy będzie odpowiednik następujących:
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a>Powiązanie nazw

Chociaż każda część rozszerzalnego typu musi być zadeklarowana w obrębie tego samego obszaru nazw, części są zwykle zapisywane w ramach różnych deklaracji przestrzeni nazw. W ten sposób różne dyrektywy `using` ([dyrektywy using](namespaces.md#using-directives)) mogą być obecne dla każdej części. Podczas interpretacji prostych nazw ([wnioskowanie o typie](expressions.md#type-inference)) w jednej części należy wziąć pod uwagę tylko dyrektywy `using` deklaracji przestrzeni nazw otaczającej tę część. Może to spowodować, że ten sam identyfikator ma różne znaczenie w różnych częściach:
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a>Elementy członkowskie klasy

Elementy członkowskie klasy składają się z elementów członkowskich wprowadzonych przez *class_member_declaration*s i członków dziedziczonych z bezpośredniej klasy podstawowej.

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

Elementy członkowskie typu klasy są podzielone na następujące kategorie:

*  Stałe, które reprezentują wartości stałe skojarzone z klasą ([stałe](classes.md#constants)).
*  Pola, które są zmiennymi klasy ([pól](classes.md#fields)).
*  Metody, które implementują obliczenia i akcje, które mogą być wykonywane przez klasę ([metody](classes.md#methods)).
*  Właściwości, które definiują nazwane cechy i akcje związane z odczytem i pisaniem tych cech ([Właściwości](classes.md#properties)).
*  Zdarzenia, które definiują powiadomienia, które mogą być generowane przez klasę ([zdarzenia](classes.md#events)).
*  Indeksatory, które zezwalają na indeksowanie wystąpień klasy w taki sam sposób (syntaktycznie) jak tablice ([indeksatory](classes.md#indexers)).
*  Operatory definiujące operatory wyrażeń, które mogą być stosowane do wystąpień klasy ([Operatory](classes.md#operators)).
*  Konstruktory wystąpień, które implementują akcje wymagane do zainicjowania wystąpień klasy ([konstruktorów wystąpień](classes.md#instance-constructors))
*  Destruktory, które implementują akcje do wykonania przed wystąpieniem klasy, zostaną trwale odrzucone ([destruktory](classes.md#destructors)).
*  Konstruktory statyczne, które implementują akcje wymagane do zainicjowania samej klasy ([konstruktory statyczne](classes.md#static-constructors)).
*  Typy, które reprezentują typy, które są lokalne dla klasy ([typy zagnieżdżone](classes.md#nested-types)).

Elementy członkowskie, które mogą zawierać kod wykonywalny, są określane zbiorczo jako *elementy członkowskie* typu klasy. Składowymi funkcji typu klasy są metody, właściwości, zdarzenia, indeksatory, operatory, konstruktory wystąpień, destruktory i konstruktory statyczne tego typu klasy.

*Class_declaration* tworzy nowe miejsce deklaracji ([deklaracje](basic-concepts.md#declarations)), a *class_member_declaration*s bezpośrednio zawarte w *class_declaration* wprowadzić nowych członków do tego obszaru deklaracji. Do *class_member_declaration*s mają zastosowanie następujące reguły:

*  Konstruktory wystąpień, destruktory i konstruktory statyczne muszą mieć taką samą nazwę jak bezpośrednio otaczającą klasę. Wszystkie inne elementy członkowskie muszą mieć nazwy, które różnią się od nazwy bezpośrednio otaczającej klasy.
*  Nazwa stałej, pola, właściwości, zdarzenia lub typu musi różnić się od nazw wszystkich innych elementów członkowskich zadeklarowanych w tej samej klasie.
*  Nazwa metody musi różnić się od nazw wszystkich innych metod, które nie są zadeklarowane w tej samej klasie. Ponadto sygnatura ([sygnatury i Przeciążenie](basic-concepts.md#signatures-and-overloading)) metody musi różnić się od sygnatur wszystkich innych metod zadeklarowanych w tej samej klasie, a dwie metody zadeklarowane w tej samej klasie mogą nie mieć sygnatur, które różnią się wyłącznie `ref` i `out`.
*  Sygnatura konstruktora wystąpienia musi różnić się od sygnatur wszystkich innych konstruktorów wystąpień zadeklarowanych w tej samej klasie, a dwa konstruktory zadeklarowane w tej samej klasie mogą nie mieć sygnatur, które różnią się wyłącznie `ref` i `out`.
*  Sygnatura indeksatora musi różnić się od sygnatur wszystkich innych indeksatorów zadeklarowanych w tej samej klasie.
*  Sygnatura operatora musi różnić się od sygnatur wszystkich innych operatorów zadeklarowanych w tej samej klasie.

Dziedziczone elementy członkowskie typu klasy ([dziedziczenie](classes.md#inheritance)) nie są częścią obszaru deklaracji klasy. W efekcie Klasa pochodna może zadeklarować element członkowski o tej samej nazwie lub podpisie jako dziedziczonego elementu członkowskiego (co w efekcie powoduje ukrycie dziedziczonego elementu członkowskiego).

### <a name="the-instance-type"></a>Typ wystąpienia

Każda deklaracja klasy ma skojarzony typ powiązania ([powiązane i niepowiązane typy](types.md#bound-and-unbound-types)), ***Typ wystąpienia***. Dla deklaracji klasy generycznej typ wystąpienia jest tworzony przez utworzenie typu złożonego ([konstruowane typy](types.md#constructed-types)) z deklaracji typu, z każdym z podanych argumentów typu, które są odpowiednim parametrem typu. Ponieważ typ wystąpienia używa parametrów typu, może być używany tylko wtedy, gdy parametry typu znajdują się w zakresie; oznacza to, że wewnątrz deklaracji klasy. Typ wystąpienia jest typem `this` dla kodu zapisaną wewnątrz deklaracji klasy. Dla klas nieogólnych typ wystąpienia jest po prostu zadeklarowaną klasą. Poniżej przedstawiono kilka deklaracji klasy wraz z ich typami wystąpień: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Elementy członkowskie skonstruowanych typów

Niedziedziczone elementy członkowskie złożonego typu są uzyskiwane przez podstawianie, dla każdego *type_parameter* w deklaracji elementu członkowskiego, odpowiadającego *type_argument* typu złożonego. Proces podstawiania jest oparty na semantycznym znaczeniu deklaracji typu i nie jest po prostu podstawianie tekstu.

Na przykład, dana deklaracja klasy generycznej
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
skonstruowany typ `Gen<int[],IComparable<string>>` ma następujących członków:
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

Typ elementu członkowskiego `a` w deklaracji klasy generycznej `Gen` to "Dwuwymiarowa tablica `T`", więc typ elementu członkowskiego `a` w skonstruowanym typie powyżej to "Dwuwymiarowa tablica Jednowymiarowa tablicy `int`" lub `int[,][]`.

W ramach elementów członkowskich funkcji wystąpienia typ `this` jest typem wystąpienia ([typem wystąpienia](classes.md#the-instance-type)) zawierającej deklaracji.

Wszystkie elementy członkowskie klasy generycznej mogą używać parametrów typu z dowolnej klasy otaczającej, bezpośrednio lub jako część typu złożonego. Gdy określony zamknięty typ skonstruowany ([otwarty i zamknięty](types.md#open-and-closed-types)) jest używany w czasie wykonywania, każde użycie parametru typu jest zastępowane rzeczywistym argumentem typu dostarczonym do typu złożonego. Na przykład:
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a>Dziedziczenie

Klasa ***dziedziczy*** elementy członkowskie jego bezpośredniego typu klasy podstawowej. Dziedziczenie oznacza, że Klasa niejawnie zawiera wszystkie elementy członkowskie typu bezpośredniej klasy podstawowej, z wyjątkiem konstruktorów wystąpień, destruktorów i konstruktorów statycznych klasy bazowej. Oto kilka ważnych aspektów dziedziczenia:

*  Dziedziczenie jest przechodnie. Jeśli `C` pochodzi od `B`, a `B` pochodzi od `A`, wówczas `C` dziedziczy członków zadeklarowanych w `B`, jak również członków zadeklarowanych w `A`.
*  Klasa pochodna rozszerza swoją bezpośrednią klasę bazową. Klasa pochodna może dodawać nowych członków do tych, które dziedziczy, ale nie może usunąć definicji dziedziczonego elementu członkowskiego.
*  Konstruktory wystąpień, destruktory i konstruktory statyczne nie są dziedziczone, ale wszystkie inne elementy członkowskie są, niezależnie od ich zadeklarowanej dostępności ([dostęp do elementów członkowskich](basic-concepts.md#member-access)). Jednak w zależności od ich zadeklarowanej dostępności dziedziczone elementy członkowskie mogą nie być dostępne w klasie pochodnej.
*  Klasa pochodna może ***ukryć*** ([ukrywając przez dziedziczenie](basic-concepts.md#hiding-through-inheritance)) dziedziczone elementy członkowskie przez zadeklarowanie nowych członków o tej samej nazwie lub podpisie. Należy jednak zauważyć, że ukrycie dziedziczonego elementu członkowskiego nie powoduje usunięcia tego elementu członkowskiego — jedynie sprawia, że ten element członkowski jest niedostępny bezpośrednio za pomocą klasy pochodnej.
*  Wystąpienie klasy zawiera zestaw wszystkich pól wystąpienia zadeklarowanych w klasie i jej klasy bazowej, a niejawna konwersja ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions)) istnieje z klasy pochodnej do dowolnego typu klasy bazowej. W tym celu odwołanie do wystąpienia jakiejś klasy pochodnej może być traktowane jako odwołanie do wystąpienia którejkolwiek z jego klas podstawowych.
*  Klasa może deklarować metody wirtualne, właściwości i indeksatory, a klasy pochodne mogą przesłaniać implementację tych elementów członkowskich funkcji. Dzięki temu klasy mogą mieć zachowanie polimorficzne, w którym akcje wykonywane przez wywołanie elementu członkowskiego funkcji różnią się w zależności od typu czasu wykonywania wystąpienia, za pomocą którego wywoływany jest element członkowski funkcji.

Dziedziczone składowe typu konstruowanej klasy są elementami członkowskimi bezpośredniego typu klasy podstawowej ([klasy bazowe](classes.md#base-classes)), które można znaleźć przez podstawianie argumentów typu konstruowanego typu dla każdego wystąpienia odpowiednich parametrów typu w specyfikacji *class_base* . Te elementy członkowskie z kolei są przekształcane przez podstawianie dla każdego *type_parameter* w deklaracji elementu członkowskiego, odpowiadającego *type_argument* specyfikacji *class_base* .

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

W powyższym przykładzie, skonstruowany typ `D<int>` ma niedziedziczonej składowej `public int G(string s)` uzyskany przez podstawianie argumentu typu `int` dla parametru typu `T`. `D<int>` ma także Dziedziczony element członkowski z deklaracji klasy `B`. Ten Dziedziczony element członkowski jest określany przez najpierw określenie typu klasy bazowej `B<int[]>` `D<int>` przez podstawianie `int` dla `T` w specyfikacji klasy podstawowej `B<T[]>`. Następnie jako argument typu do `B`, `int[]` jest zastępowany dla `U` w `public U F(long index)`, co powoduje zwrócenie dziedziczonego elementu członkowskiego `public int[] F(long index)`.

### <a name="the-new-modifier"></a>Nowy modyfikator

*Class_member_declaration* może zadeklarować składową o tej samej nazwie lub podpisie jako dziedziczonego elementu członkowskiego. W takim przypadku element członkowski klasy pochodnej jest określany jako ***ukryty*** element członkowski klasy bazowej. Ukrycie dziedziczonego elementu członkowskiego nie jest uważane za błąd, ale powoduje, że kompilator wystawia ostrzeżenie. Aby pominąć ostrzeżenie, Deklaracja elementu członkowskiego klasy pochodnej może zawierać modyfikator `new`, aby wskazać, że pochodna składowa jest przeznaczona do ukrycia podstawowego elementu członkowskiego. Ten temat został omówiony w dalszej części w [ukrywaniu przez dziedziczenie](basic-concepts.md#hiding-through-inheritance).

Jeśli modyfikator `new` jest zawarty w deklaracji, która nie ukrywa dziedziczonego elementu członkowskiego, zostanie wygenerowane ostrzeżenie. To ostrzeżenie jest pomijane przez usunięcie modyfikatora `new`.

### <a name="access-modifiers"></a>Modyfikatory dostępu

*Class_member_declaration* może mieć jeden z pięciu możliwych rodzajów zadeklarowanych dostępności ([deklarowane ułatwienia dostępu](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`lub `private`. Z wyjątkiem kombinacji `protected internal`, jest to błąd czasu kompilacji, aby określić więcej niż jeden modyfikator dostępu. Gdy *class_member_declaration* nie zawiera żadnych modyfikatorów dostępu, przyjmowana jest `private`.

### <a name="constituent-types"></a>Typy składników

Typy, które są używane w deklaracji elementu członkowskiego, są nazywane typami elementów tego elementu członkowskiego. Możliwe typy składników to typ stałej, pola, właściwości, zdarzenia lub indeksatora, zwracany typ metody lub operatora oraz typy parametrów metody, indeksatora, operatora lub konstruktora wystąpienia. Typy składników składowych muszą być co najmniej tak samo dostępne, jak sam element członkowski ([ograniczenia dotyczące ułatwień dostępu](basic-concepts.md#accessibility-constraints)).

### <a name="static-and-instance-members"></a>Elementy członkowskie statyczne i wystąpienia

Elementy członkowskie klasy są ***statycznymi elementami członkowskimi*** lub ***wystąpieniami***. Ogólnie mówiąc, warto traktować statyczne elementy członkowskie jako należące do typów klas i elementów członkowskich wystąpienia jako należące do obiektów (wystąpień typów klas).

Gdy deklaracja pola, metody, właściwości, zdarzenia, operatora lub konstruktora zawiera modyfikator `static`, deklaruje statyczny element członkowski. Ponadto deklaracja stałej lub typu niejawnie deklaruje statyczny element członkowski. Statyczne składowe mają następującą charakterystykę:

*  Gdy statyczny element członkowski `M` jest przywoływany w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `E.M`, `E` musi zwrócić uwagę na typ zawierający `M`. Jest to błąd czasu kompilacji dla `E`, aby zauważyć wystąpienie.
*  Pole statyczne określa dokładnie jedną lokalizację magazynu, która ma być współużytkowana przez wszystkie wystąpienia danego typu zamkniętej klasy. Bez względu na to, ile wystąpień danego typu zamkniętej klasy są tworzone, istnieje tylko jedna kopia pola statycznego.
*  Statyczny element członkowski funkcji (metoda, właściwość, zdarzenie, operator lub Konstruktor) nie działa w konkretnym wystąpieniu i jest to błąd czasu kompilacji, aby odwołać się do `this` w takiej składowej funkcji.

Gdy deklaracja pola, metody, właściwości, zdarzenia, indeksatora, konstruktora lub destruktora nie zawiera modyfikatora `static`, deklaruje element członkowski wystąpienia. (Składowa wystąpienia jest czasami nazywana niestatyczną składową). Elementy członkowskie wystąpienia mają następującą charakterystykę:

*  Gdy element członkowski wystąpienia `M` jest przywoływany w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `E.M`, `E` musi zwrócić uwagę na wystąpienie typu zawierającego `M`. Jest to błąd czasu powiązania dla `E`, aby zauważyć typ.
*  Każde wystąpienie klasy zawiera oddzielny zestaw wszystkich pól wystąpienia klasy.
*  Element członkowski funkcji wystąpienia (metoda, właściwość, indeksator, Konstruktor wystąpienia lub destruktor) działa w danym wystąpieniu klasy i można uzyskać do niego dostęp jako `this` ([ten dostęp](expressions.md#this-access)).

Poniższy przykład ilustruje reguły uzyskiwania dostępu do statycznych i składowych wystąpień:
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

Metoda `F` pokazuje, że w elemencie członkowskim funkcji wystąpienia *simple_name* ([proste nazwy](expressions.md#simple-names)) mogą być używane do uzyskiwania dostępu do elementów członkowskich wystąpienia i statycznych elementów członkowskich. Metoda `G` pokazuje, że w składowej funkcji statycznej jest to błąd czasu kompilacji, aby uzyskać dostęp do elementu członkowskiego wystąpienia za pomocą *simple_name*. Metoda `Main` pokazuje, że w *member_access* ([dostęp do elementów członkowskich](expressions.md#member-access)) należy uzyskać dostęp do składowych wystąpień za pomocą wystąpień, a statyczne elementy członkowskie muszą być dostępne za pomocą typów.

### <a name="nested-types"></a>Typy zagnieżdżone

Typ zadeklarowany w deklaracji klasy lub struktury jest nazywany ***typem zagnieżdżonym***. Typ, który jest zadeklarowany w ramach jednostki kompilacji lub przestrzeni nazw, jest nazywany ***typem niezagnieżdżonym***.

w przykładzie
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
Klasa `B` jest typem zagnieżdżonym, ponieważ jest zadeklarowana w klasie `A`, a Klasa `A` jest typem niezagnieżdżonym, ponieważ jest zadeklarowana w ramach jednostki kompilacji.

#### <a name="fully-qualified-name"></a>W pełni kwalifikowana nazwa

W pełni kwalifikowana nazwa (w[pełni kwalifikowana](basic-concepts.md#fully-qualified-names)nazwa) dla typu zagnieżdżonego jest `S.N` gdzie `S` jest w pełni kwalifikowaną nazwą typu, w którym jest zadeklarowany `N` typu.

#### <a name="declared-accessibility"></a>Zadeklarowane ułatwienia dostępu

Typy niezagnieżdżone mogą mieć `public` lub `internal` zadeklarowane ułatwienia dostępu i mieć domyślnie `internal` zadeklarowane ułatwienia dostępu. Typy zagnieżdżone mogą mieć te formy zadeklarowanej dostępności, a także co najmniej jedną dodatkową postać zadeklarowanej dostępności, w zależności od tego, czy typ zawierający jest klasą czy strukturą:

*  Typ zagnieżdżony zadeklarowany w klasie może mieć jedną z pięciu postaci zadeklarowanej dostępności (`public`, `protected internal`, `protected`, `internal`lub `private`) i, podobnie jak inne elementy członkowskie klasy, domyślnie `private` zadeklarowane ułatwienia dostępu.
*  Zagnieżdżony typ, który jest zadeklarowany w strukturze, może mieć jedną z trzech postaci zadeklarowanej dostępności (`public`, `internal`lub `private`) i, podobnie jak w przypadku innych elementów członkowskich struktury, domyślnie `private` zadeklarowane ułatwienia dostępu.

Przykład
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
deklaruje prywatną `Node`klasy zagnieżdżonej.

#### <a name="hiding"></a>Ukryj

Zagnieżdżony typ może ukrywać ([ukrywając nazwę](basic-concepts.md#name-hiding)) podstawową składową. Modyfikator `new` jest dozwolony w deklaracjach typu zagnieżdżonego, dzięki czemu ukrywanie może być wyrażone jawnie. Przykład
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
pokazuje zagnieżdżoną klasę `M`, która ukrywa `M` metody zdefiniowanej w `Base`.

#### <a name="this-access"></a>Ten dostęp

Typ zagnieżdżony i jego typ zawierający nie mają specjalnej relacji w odniesieniu do *this_access* ([ten dostęp](expressions.md#this-access)). W celu odwoływania się do elementów członkowskich wystąpienia typu zawierającego nie można używać w tym celu `this` w ramach typu zagnieżdżonego. W przypadkach, gdy Typ zagnieżdżony wymaga dostępu do elementów członkowskich wystąpienia typu zawierającego, można uzyskać dostęp, dostarczając `this` dla wystąpienia typu zawierającego jako argument konstruktora dla typu zagnieżdżonego. Poniższy przykład
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
pokazuje tę technikę. Wystąpienie `C` tworzy wystąpienie `Nested` i przekazuje własne `this` do konstruktora `Nested`, aby zapewnić kolejny dostęp do elementów członkowskich wystąpienia `C`.

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Dostęp do prywatnych i chronionych składowych typu zawierającego

Typ zagnieżdżony ma dostęp do wszystkich elementów członkowskich, które są dostępne dla tego typu zawierającego, w tym elementów członkowskich typu zawierającego `private` i `protected` zadeklarowane ułatwienia dostępu. Przykład
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
pokazuje klasę `C`, która zawiera `Nested`klasy zagnieżdżonej. W `Nested`Metoda `G` wywołuje metodę statyczną `F` zdefiniowaną w `C`i `F` ma prywatnie zadeklarowane ułatwienia dostępu.

Typ zagnieżdżony może również uzyskiwać dostęp do chronionych elementów członkowskich zdefiniowanych w typie podstawowym typu zawierającego. w przykładzie
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
Klasa zagnieżdżona `Derived.Nested` uzyskuje dostęp do chronionej metody `F` zdefiniowanej w klasie podstawowej `Derived``Base`przez wywołanie przez wystąpienie `Derived`.

#### <a name="nested-types-in-generic-classes"></a>Zagnieżdżone typy w klasach ogólnych

Deklaracja klasy generycznej może zawierać deklaracje typu zagnieżdżonego. Parametry typu otaczającej klasy mogą być używane w zagnieżdżonych typach. Deklaracja typu zagnieżdżonego może zawierać dodatkowe parametry typu, które mają zastosowanie tylko do typu zagnieżdżonego.

Każda deklaracja typu znajdująca się w deklaracji klasy generycznej jest niejawnie deklaracją typu ogólnego. Podczas pisania odwołania do typu zagnieżdżonego w typie ogólnym, zawierający typ skonstruowany, w tym jego argumenty typu, musi mieć nazwę. Jednakże z poziomu klasy zewnętrznej można używać typu zagnieżdżonego bez kwalifikacji; Typ wystąpienia klasy zewnętrznej może być niejawnie używany podczas konstruowania typu zagnieżdżonego. Poniższy przykład pokazuje trzy różne prawidłowe sposoby odwoływania się do złożonego typu utworzonego na podstawie `Inner`; pierwsze dwa są równoważne:
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

Chociaż jest to zły styl programowania, parametr typu w typie zagnieżdżonym może ukryć element członkowski lub parametr typu zadeklarowany w typie zewnętrznym:
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>Zastrzeżone nazwy składowych

Aby ułatwić bazowa C# implementacji w czasie wykonywania, dla każdej źródłowej deklaracji składowej, która jest właściwością, zdarzeniem lub indeksatorem, implementacja musi zarezerwować dwa podpisy metod na podstawie rodzaju deklaracji elementu członkowskiego, jego nazwy i typu. Jest to błąd czasu kompilacji dla programu w celu zadeklarowania elementu członkowskiego, którego sygnatura pasuje do jednego z tych zarezerwowanych podpisów, nawet jeśli bazowa implementacja czasu wykonywania nie używa tych rezerwacji.

Nazwy zastrzeżone nie wprowadzają deklaracji, dlatego nie uczestniczą w wyszukiwaniu elementów członkowskich. Jednak skojarzone podpisy metod zarezerwowanych deklaracji, które uczestniczą w dziedziczeniu ([dziedziczenie](classes.md#inheritance)) i mogą być ukrywane za pomocą modyfikatora `new` ([nowy modyfikator](classes.md#the-new-modifier)).

Rezerwacja tych nazw służy trzy cele:

*  Aby umożliwić podstawowej implementacji użycie zwykłego identyfikatora jako nazwy metody do pobierania lub ustawiania dostępu do funkcji C# języka.
*  Aby zezwolić innym językom na współdziałanie przy użyciu zwykłego identyfikatora jako nazwy metody do pobierania lub ustawiania C# dostępu do funkcji języka.
*  Aby zapewnić, że źródło zaakceptowane przez jeden z zgodnych kompilatorów jest akceptowane przez inny, przez wprowadzenie określonych nazw zarezerwowanych elementów członkowskich spójnych we C# wszystkich implementacjach.

Deklaracja destruktora ([destruktory](classes.md#destructors)) powoduje również, że podpis ma być zarezerwowany ([nazwy składowe zarezerwowane dla destruktorów](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Nazwy elementów członkowskich zarezerwowane dla właściwości

Dla właściwości `P` ([Właściwości](classes.md#properties)) typu `T`następujące podpisy są zastrzeżone:

```csharp
T get_P();
void set_P(T value);
```

Oba podpisy są zastrzeżone, nawet jeśli właściwość jest tylko do odczytu lub tylko do zapisu.

w przykładzie
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
Klasa `A` definiuje właściwość tylko do odczytu `P`, w ten sposób zachowując sygnatury dla metod `get_P` i `set_P`. Klasa `B` pochodzi od `A` i ukrywa oba te podpisy zarezerwowane. Przykład generuje dane wyjściowe:
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Nazwy elementów członkowskich zarezerwowane dla zdarzeń

W przypadku zdarzenia `E` ([zdarzenia](classes.md#events)) typu delegata `T`następujące podpisy są zastrzeżone:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Nazwy elementów członkowskich zarezerwowane dla indeksatorów

Dla indeksatora ([indeksatorów](classes.md#indexers)) typu `T` z `L`listy parametrów następujące podpisy są zastrzeżone:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Oba podpisy są zastrzeżone, nawet jeśli indeksator jest tylko do odczytu lub tylko do zapisu.

Ponadto nazwa elementu członkowskiego `Item` jest zarezerwowana.

#### <a name="member-names-reserved-for-destructors"></a>Nazwy elementów członkowskich zarezerwowane dla destruktorów

W przypadku klasy zawierającej destruktor ([destruktory](classes.md#destructors)) następujący podpis jest zastrzeżony:
```csharp
void Finalize();
```

## <a name="constants"></a>{1&gt;Stałe&lt;1}

***Stała*** jest składową klasy, która reprezentuje wartość stałą: wartość, która może być obliczana w czasie kompilacji. *Constant_declaration* wprowadza jedną lub więcej stałych danego typu.

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

*Constant_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)), modyfikator `new` ([nowy modyfikator](classes.md#the-new-modifier)) i prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)). Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych przez *constant_declaration*. Mimo że stałe są uznawane za statyczne elementy członkowskie, *constant_declaration* nie wymagają ani nie dopuszcza modyfikatora `static`. Jest to błąd dla tego samego modyfikatora, który może występować wiele razy w deklaracji stałej.

*Typ* *constant_declaration* określa typ elementów członkowskich wprowadzonych przez deklarację. Po tym typie następuje lista *constant_declarator*s, z których każdy wprowadza nowy element członkowski. *Constant_declarator* składa się z *identyfikatora* , który ma nazwę elementu członkowskiego, po którym następuje token "`=`", po którym następuje *constant_expression* ([wyrażenia stałe](expressions.md#constant-expressions)), które dają wartość elementu członkowskiego.

*Typ* określony w deklaracji stałej musi mieć wartość `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, *enum_type*i *reference_type*. Każdy *constant_expression* musi zwracać wartość typu docelowego lub typu, który można przekonwertować na typ docelowy przez niejawną konwersję ([konwersje niejawne](conversions.md#implicit-conversions)).

*Typ* stałej musi być co najmniej tak samo jak sam sam ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).

Wartość stałej jest uzyskiwana w wyrażeniu przy użyciu *simple_name* ([nazw prostych](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)).

Stała może same uczestniczyć w *constant_expression*. W tym celu można użyć stałej w dowolnej konstrukcji, która wymaga *constant_expression*. Przykłady takich konstrukcji obejmują `case` etykiet, instrukcji `goto case`, `enum` deklaracji składowych, atrybutów i innych deklaracji stałych.

Zgodnie z opisem w [wyrażeniach stałych](expressions.md#constant-expressions) *constant_expression* jest wyrażeniem, które może być w pełni oceniane w czasie kompilacji. Ze względu na to, że jedynym sposobem utworzenia wartości innej niż null *reference_type* innej niż `string` jest zastosowanie operatora `new`, a ponieważ operator `new` nie jest dozwolony w *constant_expression*, jedyną możliwą wartością dla stałych *reference_type*s innych niż `string` jest `null`.

Gdy wymagana jest Nazwa symboliczna wartości stałej, ale jeśli typ tej wartości nie jest dozwolony w deklaracji stałej lub gdy wartość nie może zostać obliczona w czasie kompilacji przez *constant_expression*, w zamian może być używane pole `readonly` ([pola tylko do odczytu](classes.md#readonly-fields)).

Deklaracja stałej, która deklaruje wiele stałych, jest równoważna z wieloma deklaracjami pojedynczych stałych z tymi samymi atrybutami, modyfikatorami i typem. Na przykład:
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
odpowiada wyrażeniu
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

Stałe mogą zależeć od innych stałych w ramach tego samego programu, o ile zależności nie mają charakteru cyklicznego. Kompilator automatycznie organizuje ocenę stałych deklaracji w odpowiedniej kolejności. w przykładzie
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
Kompilator najpierw szacuje `A.Y`, następnie szacuje `B.Z`, a wreszcie szacuje `A.X`, generując wartości `10`, `11`i `12`. Deklaracje stałe mogą zależeć od stałych z innych programów, ale takie zależności są możliwe tylko w jednym kierunku. W odniesieniu do powyższego przykładu, jeśli `A` i `B` zostały zadeklarowane w osobnych programach, możliwe jest, aby `A.X` zależały od `B.Z`, ale `B.Z` nie zależały jednocześnie od `A.Y`.

## <a name="fields"></a>Pola

***Pole*** jest składową, która reprezentuje zmienną skojarzoną z obiektem lub klasą. *Field_declaration* wprowadza co najmniej jedno pole danego typu.

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

*Field_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)), modyfikator `new` ([nowy modyfikator](classes.md#the-new-modifier)), prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)) i modyfikator `static` ([pola statyczne i wystąpienia](classes.md#static-and-instance-fields)). Ponadto *field_declaration* może zawierać modyfikator `readonly` ([pola tylko do odczytu](classes.md#readonly-fields)) lub modyfikator `volatile` ([pola nietrwałe](classes.md#volatile-fields)), ale nie oba jednocześnie. Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych przez *field_declaration*. Jest to błąd dla tego samego modyfikatora, który może występować wiele razy w deklaracji pola.

*Typ* *field_declaration* określa typ elementów członkowskich wprowadzonych przez deklarację. Po tym typie następuje lista *variable_declarator*s, z których każdy wprowadza nowy element członkowski. *Variable_declarator* składa się z *identyfikatora* , który jest nazwą tego elementu członkowskiego, opcjonalnie następuje token "`=`" i *variable_initializer* ([inicjatory zmiennych](classes.md#variable-initializers)), który daje początkową wartość tego elementu członkowskiego.

*Typ* pola musi być co najmniej tak samo jak w przypadku samego pola ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).

Wartość pola jest uzyskiwana w wyrażeniu przy użyciu *simple_name* ([nazw prostych](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)). Wartość pola, które nie jest tylko do odczytu, jest modyfikowana przy użyciu *przypisania* ([Operatory przypisania](expressions.md#assignment-operators)). Wartość pola, które nie jest tylko do odczytu, może być zarówno uzyskana, jak i modyfikowana przy użyciu przyrostka przyrostkowego i zmniejszania (operatory[przyrostu i zmniejszania](expressions.md#postfix-increment-and-decrement-operators)) oraz operatory przyrostu i zmniejszania wartości prefiksów ([Operatory przyrostu i](expressions.md#prefix-increment-and-decrement-operators)zmniejszania).

Deklaracja pola, która deklaruje wiele pól, jest równoważna z wieloma deklaracjami pojedynczych pól z tymi samymi atrybutami, modyfikatorami i typem. Na przykład:
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
odpowiada wyrażeniu
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a>Pola statyczne i wystąpienia

Gdy deklaracja pola zawiera modyfikator `static`, pola wprowadzane przez deklarację są ***polami statycznymi***. Gdy modyfikator `static` nie jest obecny, pola wprowadzane przez deklarację są ***polami wystąpień***. Pola statyczne i wystąpienia są dwa rodzaje zmiennych ([zmienne](variables.md)) obsługiwane przez C#, a czasami są one określane odpowiednio jako ***zmienne statyczne*** i ***zmienne wystąpień***.

Pole statyczne nie jest częścią określonego wystąpienia; Zamiast tego jest współużytkowany między wszystkimi wystąpieniami typu zamkniętego ([otwarte i zamknięte](types.md#open-and-closed-types)). Niezależnie od tego, ile wystąpień typu zamkniętej klasy są tworzone, istnieje tylko jedna kopia pola statycznego dla skojarzonej domeny aplikacji.

Na przykład:
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

Pole wystąpienia należy do wystąpienia. Każde wystąpienie klasy zawiera oddzielny zestaw wszystkich pól wystąpienia tej klasy.

Gdy odwołanie do pola znajduje się w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `E.M`, jeśli `M` jest polem statycznym, `E` musi zwrócić uwagę na typ zawierający `M`i jeśli `M` jest polem wystąpienia, E musi zwrócić uwagę na wystąpienie typu zawierającego `M`.

Różnice między elementami statycznymi a składowymi wystąpienia są omówione bardziej szczegółowo w [elementach członkowskich static i instance](classes.md#static-and-instance-members).

### <a name="readonly-fields"></a>Pola tylko do odczytu

Gdy *field_declaration* zawiera modyfikator `readonly`, pola wprowadzone przez deklarację to ***pola tylko do odczytu***. Bezpośrednie przypisania do pól tylko do odczytu mogą wystąpić tylko jako część tej deklaracji lub w konstruktorze wystąpienia lub w konstruktorze statycznym w tej samej klasie. (Pole tylko do odczytu można przypisać wiele razy w tych kontekstach). W szczególnych przypadkach przypisania bezpośrednie do pola `readonly` są dozwolone tylko w następujących kontekstach:

*  W *variable_declarator* , które wprowadza pole (poprzez uwzględnienie *variable_initializer* w deklaracji).
*  Dla pola wystąpienia w konstruktorach wystąpień klasy, która zawiera deklarację pola; dla pola statycznego w konstruktorze statycznym klasy, która zawiera deklarację pola. Są to również jedyne konteksty, w których prawidłowe jest przekazanie pola `readonly` jako parametru `out` lub `ref`.

Próba przypisania do pola `readonly` lub przekazania go jako parametru `out` lub `ref` w dowolnym innym kontekście jest błędem czasu kompilacji.

#### <a name="using-static-readonly-fields-for-constants"></a>Używanie statycznych pól tylko do odczytu dla stałych

Pole `static readonly` jest przydatne, gdy wymagana jest Nazwa symboliczna wartości stałej, ale jeśli typ wartości nie jest dozwolony w deklaracji `const` lub nie można obliczyć wartości w czasie kompilacji. w przykładzie
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
elementów członkowskich `Black`, `White`, `Red`, `Green`i `Blue` nie można zadeklarować jako elementów członkowskich `const`, ponieważ ich wartości nie mogą być obliczane w czasie kompilacji. Jednakże deklarując je `static readonly` ma znacznie taki sam skutek.

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>Przechowywanie wersji stałych i statycznych pól tylko do odczytu

Stałe i tylko do odczytu pola mają różne semantyki wersji binarnej. Gdy wyrażenie odwołuje się do stałej, wartość stałej jest uzyskiwana w czasie kompilacji, ale gdy wyrażenie odwołuje się do pola tylko do odczytu, wartość pola nie zostanie uzyskana do czasu wykonania. Rozważmy aplikację, która składa się z dwóch oddzielnych programów:
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

Przestrzenie nazw `Program1` i `Program2` oznaczają dwa programy, które są kompilowane osobno. Ponieważ `Program1.Utils.X` jest zadeklarowana jako statyczne pole tylko do odczytu, wartość wyjściowa instrukcji `Console.WriteLine` nie jest znana w czasie kompilacji, ale jest uzyskiwana w czasie wykonywania. W takim przypadku, jeśli wartość `X` zostanie zmieniona i `Program1` zostanie ponownie skompilowana, instrukcja `Console.WriteLine` będzie wyprowadzać nową wartość nawet wtedy, gdy `Program2` nie zostanie ponownie skompilowana. Jednak `X` była stałą, wartość `X` zostałaby uzyskana w czasie, `Program2` została skompilowana i pozostanie nienaruszona przez zmiany w `Program1` do momentu ponownego skompilowania `Program2`.

### <a name="volatile-fields"></a>Pola nietrwałe

Gdy *field_declaration* zawiera modyfikator `volatile`, pola wprowadzone przez tę deklarację są ***polami nietrwałymi***.

W przypadku pól nietrwałych, techniki optymalizacji, które zmieniają kolejność instrukcji, mogą prowadzić do nieoczekiwanych i nieprzewidzianych wyników w programach wielowątkowych, które uzyskują dostęp do pól bez synchronizacji, takich jak zapewniane przez *lock_statement* ([instrukcja Lock](statements.md#the-lock-statement)). Optymalizacje mogą być wykonywane przez kompilator, przez system czasu wykonywania lub przez sprzęt. W przypadku pól nietrwałych takie zmiany kolejności są ograniczone:

*  Odczyt pola nietrwałego jest nazywany ***nietrwałym odczytem***. Odczyt nietrwały ma "pozyskiwanie semantyki"; oznacza to, że jest to konieczne przed wszelkimi odwołaniami do pamięci, która następuje po niej w sekwencji instrukcji.
*  Zapis pola nietrwałego jest nazywany ***zapisem nietrwałym***. Zapis nietrwały ma "semantykę wersji"; oznacza to, że ma to miejsce po dowolnych odwołaniach do pamięci przed instrukcją zapisu w sekwencji instrukcji.

Te ograniczenia zapewniają, że wszystkie wątki będą obserwować nietrwałe zapisy wykonywane przez każdy inny wątek w kolejności, w której zostały wykonane. Implementacja zgodna z implementacją nie jest wymagana do zapewnienia pojedynczej kolejności zapisów nietrwałych w postaci zaobserwowanej ze wszystkich wątków wykonania. Typ pola nietrwałego musi mieć jedną z następujących wartości:

*  *Reference_type*.
*  Typ `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`lub `System.UIntPtr`.
*  *Enum_type* ma typ podstawowy wyliczenia `byte`, `sbyte`, `short`, `ushort`, `int`lub `uint`.

Przykład
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
tworzy dane wyjściowe:
```console
result = 143
```

W tym przykładzie metoda `Main` uruchamia nowy wątek, w którym jest uruchamiana Metoda `Thread2`. Ta metoda przechowuje wartość w polu nietrwałym o nazwie `result`, a następnie przechowuje `true` w `finished`polu trwałym. Główny wątek czeka na ustawienie pola `finished` na `true`, a następnie odczytuje `result`pola. Ponieważ `finished` został zadeklarowany `volatile`, główny wątek musi odczytać wartość `143` z `result`pola. Jeśli pole `finished` nie zostało zadeklarowane `volatile`, będzie dozwolone, aby Magazyn `result` widoczny dla głównego wątku po magazynie do `finished`i dlatego dla wątku głównego odczytać wartość `0` z pola `result`. Deklarowanie `finished` jako pola `volatile` uniemożliwia takie niespójność.

### <a name="field-initialization"></a>Inicjowanie pola

Wartość początkowa pola, niezależnie od tego, czy jest to pole statyczne czy pole wystąpienia, jest wartością domyślną ([wartości domyślne](variables.md#default-values)) typu pola. Nie jest możliwe przestrzeganie wartości pola przed wystąpieniem domyślnej inicjalizacji, a w takim przypadku pole nigdy nie jest "niezainicjowane". Przykład
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
tworzy dane wyjściowe
```console
b = False, i = 0
```
ponieważ `b` i `i` są automatycznie inicjowane do wartości domyślnych.

### <a name="variable-initializers"></a>Inicjatory zmiennych

Deklaracje pól mogą zawierać *variable_initializer*s. Dla pól statycznych, inicjatory zmiennych odpowiadają instrukcjom przypisania, które są wykonywane podczas inicjowania klasy. W przypadku pól wystąpienia zmienne inicjatory odpowiadają instrukcjom przypisania, które są wykonywane podczas tworzenia wystąpienia klasy.

Przykład
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
tworzy dane wyjściowe
```console
x = 1.4142135623731, i = 100, s = Hello
```
Ponieważ przypisanie do `x` występuje, gdy inicjatory pola statycznego są wykonywane, a przypisania do `i` i `s` wystąpią, kiedy inicjator pola wystąpienia zostanie wykonany.

Inicjalizacja wartości domyślnej opisana w ramach [inicjalizacji pola](classes.md#field-initialization) występuje dla wszystkich pól, w tym pól, które mają inicjatory zmiennych. W tym przypadku po zainicjowaniu klasy wszystkie pola statyczne w tej klasie są najpierw inicjowane na wartości domyślne, a następnie inicjatory pola statycznego są wykonywane w kolejności tekstowej. Podobnie, gdy tworzone jest wystąpienie klasy, wszystkie pola wystąpienia w tym wystąpieniu są najpierw inicjowane na wartości domyślne, a następnie inicjatory pola wystąpienia są wykonywane w kolejności tekstowej.

Istnieje możliwość, że pola statyczne z inicjatorami zmiennych mają być przestrzegane w stanie wartości domyślnej. Jest to jednak zdecydowanie odradzane jako kwestia stylu. Przykład
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
wykazuje takie zachowanie. Pomimo definicji cyklicznych a i b, program jest prawidłowy. Powoduje to wyjście
```console
a = 1, b = 2
```
ponieważ pola statyczne `a` i `b` są inicjowane do `0` (wartość domyślna dla `int`) przed wykonaniem inicjatorów. Gdy inicjator `a` jest uruchomiony, wartość `b` jest równa zero, więc `a` jest inicjowana do `1`. Po uruchomieniu inicjatora `b`, wartość `a` jest już `1`, a więc `b` jest inicjowana do `2`.

#### <a name="static-field-initialization"></a>Inicjowanie pola statycznego

Inicjatory zmiennych pól statycznych klasy odpowiadają sekwencji przypisań, które są wykonywane w kolejności tekstowej, w której pojawiają się w deklaracji klasy. Jeśli w klasie istnieje Konstruktor statyczny ([konstruktory statyczne](classes.md#static-constructors)), wykonywanie inicjatorów pól statycznych odbywa się bezpośrednio przed wykonaniem tego konstruktora statycznego. W przeciwnym razie inicjatory pola statycznego są wykonywane w czasie zależnym od implementacji przed pierwszym użyciem pola statycznego tej klasy. Przykład
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
może generować dane wyjściowe:
```console
Init A
Init B
1 1
```
lub dane wyjściowe:
```console
Init B
Init A
1 1
```
ponieważ wykonywanie inicjatora `X`i inicjatora `Y`może wystąpić w dowolnej kolejności; są one ograniczone tylko przed odwołaniami do tych pól. Jednak w przykładzie:
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
dane wyjściowe muszą być następujące:
```console
Init B
Init A
1 1
```
ponieważ reguły, które są wykonywane przez konstruktory statyczne (zgodnie z definicją w [konstruktorach statycznych](classes.md#static-constructors)), zapewniają, że `B`Konstruktor statyczny (i dlatego, że `B`pola statyczne inicjatorów) muszą zostać uruchomione przed `A`statycznego konstruktora i pól.

#### <a name="instance-field-initialization"></a>Inicjowanie pola wystąpienia

Inicjatory zmiennych pola wystąpienia klasy odpowiadają sekwencji przypisań, które są wykonywane natychmiast po wprowadzeniu do dowolnego jednego z konstruktorów wystąpień ([inicjatory konstruktorów](classes.md#constructor-initializers)) tej klasy. Inicjatory zmiennych są wykonywane w kolejności tekstowej, w której pojawiają się w deklaracji klasy. Proces tworzenia i inicjowania wystąpienia klasy został szczegółowo opisany w [konstruktorach wystąpień](classes.md#instance-constructors).

Inicjator zmiennej dla pola wystąpienia nie może odwoływać się do tworzonego wystąpienia. W ten sposób jest to błąd czasu kompilacji, aby odwołać się do `this` w inicjatorze zmiennej, ponieważ jest to błąd czasu kompilacji dla inicjatora zmiennej, który odwołuje się do dowolnego elementu członkowskiego wystąpienia za pomocą *simple_name*. w przykładzie
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
Inicjator zmiennej dla `y` powoduje błąd czasu kompilacji, ponieważ odwołuje się do elementu członkowskiego tworzonego wystąpienia.

## <a name="methods"></a>Metody

***Metoda*** to element członkowski implementujący obliczenia lub akcję, które mogą być wykonywane przez obiekt lub klasę. Metody są deklarowane przy użyciu *method_declaration*s:

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

*Method_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)) i prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `static` (Metoda[statyczna i wystąpienia](classes.md#static-and-instance-methods)), `virtual` ([metody wirtualne](classes.md#virtual-methods)), `override` ([metody zastępowania](classes.md#override-methods)), `sealed` ([zapieczętowane metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)) i `extern` (metody[zewnętrzne](classes.md#external-methods)) modyfikatory.

Deklaracja ma prawidłową kombinację modyfikatorów, jeśli spełnione są wszystkie następujące warunki:

*  Deklaracja zawiera prawidłową kombinację modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)).
*  Deklaracja nie zawiera tego samego modyfikatora wiele razy.
*  Deklaracja zawiera co najwyżej jeden z następujących modyfikatorów: `static`, `virtual`i `override`.
*  Deklaracja zawiera co najwyżej jeden z następujących modyfikatorów: `new` i `override`.
*  Jeśli deklaracja zawiera modyfikator `abstract`, to Deklaracja nie zawiera żadnego z następujących modyfikatorów: `static`, `virtual`, `sealed` lub `extern`.
*  Jeśli deklaracja zawiera modyfikator `private`, to Deklaracja nie zawiera żadnego z następujących modyfikatorów: `virtual`, `override`lub `abstract`.
*  Jeśli deklaracja zawiera modyfikator `sealed`, wówczas deklaracja zawiera również modyfikator `override`.
*  Jeśli deklaracja zawiera modyfikator `partial`, wówczas nie zawiera żadnego z następujących modyfikatorów: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`lub `extern`.

Metoda, która ma modyfikator `async` jest funkcją asynchroniczną i jest zgodna z regułami opisanymi w [funkcjach asynchronicznych](classes.md#async-functions).

*Return_type* deklaracji metody Określa typ wartości obliczanej i zwracanej przez metodę. *Return_type* jest `void`, jeśli metoda nie zwraca wartości. Jeśli deklaracja zawiera modyfikator `partial`, zwracany typ musi być `void`.

*MEMBER_NAME* określa nazwę metody. O ile Metoda nie jest jawną implementacją składowej interfejsu ([jawne implementacje elementu członkowskiego interfejsu](interfaces.md#explicit-interface-member-implementations)), *MEMBER_NAME* jest po prostu *identyfikatorem*. W przypadku jawnej implementacji elementu członkowskiego interfejsu *MEMBER_NAME* składa się z *INTERFACE_TYPE* po którym następuje "`.`" i *Identyfikator*.

Opcjonalne *type_parameter_list* określa parametry typu metody ([parametry typu](classes.md#type-parameters)). Jeśli *type_parameter_list* jest określony, metoda jest ***metodą rodzajową***. Jeśli metoda ma modyfikator `extern`, nie można określić *type_parameter_list* .

Opcjonalne *formal_parameter_list* określa parametry metody ([Parametry metody](classes.md#method-parameters)).

Opcjonalne *type_parameter_constraints_clause*s określają ograniczenia dotyczące poszczególnych parametrów typu ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) i można je określić tylko w przypadku podania *type_parameter_list* również, a metoda nie ma modyfikatora `override`.

*Return_type* i każdy z typów, do których odwołuje się *formal_parameter_list* metody, musi być co najmniej tak samo samo jak Metoda ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).

*Method_body* jest średnikiem, ***treścią instrukcji*** lub ***treścią wyrażenia***. Treść instrukcji składa się z *bloku*, który określa instrukcje do wykonania, gdy metoda jest wywoływana. Treść wyrażenia składa się z `=>` po którym następuje *wyrażenie* i średnik, i oznacza pojedyncze wyrażenie do wykonania, gdy wywoływana jest metoda. 

W przypadku metod `abstract` i `extern` *method_body* składa się po prostu z średnika. Dla `partial` metod, *method_body* może składać się z średnika, treści bloku lub treści wyrażenia. Dla wszystkich innych metod *method_body* jest treścią bloku lub treścią wyrażenia.

Jeśli *method_body* składa się z średnika, wówczas deklaracja nie może zawierać modyfikatora `async`.

Nazwa, lista parametrów typu i formalna lista parametrów metody definiują sygnaturę ([sygnatury i Przeciążenie](basic-concepts.md#signatures-and-overloading)) metody. W każdym przypadku podpis metody składa się z jej nazwy, liczby parametrów typu oraz liczby, modyfikatorów i typów jego parametrów formalnych. W tym celu wszelkie parametry typu metody, która występuje w typie parametru formalnego, nie są identyfikowane przez jego nazwę, ale według pozycji porządkowej na liście argumentów typu metody. Zwracany typ nie jest częścią podpisu metody ani nie są nazwami parametrów typu lub parametrów formalnych.

Nazwa metody musi różnić się od nazw wszystkich innych metod, które nie są zadeklarowane w tej samej klasie. Ponadto podpis metody musi różnić się od sygnatur wszystkich innych metod zadeklarowanych w tej samej klasie, a dwie metody zadeklarowane w tej samej klasie mogą nie mieć podpisów, które różnią się wyłącznie `ref` i `out`.

*Type_parameter*s metody są w zakresie w *method_declaration*i mogą być używane do tworzenia typów w całym zakresie w *return_type*, *method_body*i *type_parameter_constraints_clause*s, ale nie w *atrybutach*.

Wszystkie parametry formalne i parametry typu muszą mieć różne nazwy.

### <a name="method-parameters"></a>Parametry metody

Parametry metody, jeśli istnieje, są deklarowane przez *formal_parameter_list*metody.

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

Formalna lista parametrów składa się z jednego lub więcej parametrów oddzielonych przecinkami, których tylko ostatni może być *parameter_array*.

*Fixed_parameter* składa się z opcjonalnego zestawu *atrybutów* ([atrybutów](attributes.md)), opcjonalnego modyfikatora `ref`, `out` lub `this`, *typu*, *identyfikatora* i opcjonalnego *default_argument*. Każda *fixed_parameter* deklaruje parametr danego typu o podaną nazwę. Modyfikator `this` wyznacza metodę jako metodę rozszerzającą i jest dozwolony tylko dla pierwszego parametru metody statycznej. Metody rozszerzające są dokładniej opisane w [metodach rozszerzających](classes.md#extension-methods).

*Fixed_parameter* z *default_argument* jest znany jako ***parametr opcjonalny***, podczas gdy *fixed_parameter* bez *default_argument* jest ***wymaganym parametrem***. Wymagany parametr nie może występować po opcjonalnym parametrze w *formal_parameter_list*.

Parametr `ref` lub `out` nie może mieć *default_argument*. *Wyrażenie* w *default_argument* musi mieć jedną z następujących wartości:

*  *constant_expression*
*  wyrażenie `new S()`, gdzie `S` jest typem wartości
*  wyrażenie `default(S)`, gdzie `S` jest typem wartości

*Wyrażenie* musi być niejawnie konwertowane przez tożsamość lub konwersję dopuszczającą wartości null na typ parametru.

Jeśli parametry opcjonalne występują w deklaracji metody częściowej implementującej ([metody częściowe](classes.md#partial-methods)), Jawna implementacja składowej interfejsu ([jawne implementacje elementu członkowskiego interfejsu](interfaces.md#explicit-interface-member-implementations)) lub w jednoparametrowej deklaracji indeksatora ([indeksatory](classes.md#indexers)) kompilator powinien dać ostrzeżenie, ponieważ te elementy członkowskie nigdy nie mogą być wywoływane w sposób zezwalający na pominięcie argumentów.

*Parameter_array* składa się z opcjonalnego zestawu *atrybutów* ([atrybutów](attributes.md)), modyfikator `params`, *array_type*i *Identyfikator*. Tablica parametrów deklaruje pojedynczy parametr danego typu tablicy o podaną nazwę. *Array_type* tablicy parametrów musi być typem tablicy jednowymiarowej ([typy tablicowe](arrays.md#array-types)). W wywołaniu metody, tablica parametrów zezwala na określenie pojedynczego argumentu danego typu tablicy lub dopuszcza zero lub więcej argumentów typu elementu tablicy, który ma zostać określony. Tablice parametrów są szczegółowo opisane w [tablicach parametrów](classes.md#parameter-arrays).

*Parameter_array* może występować po opcjonalnym parametrze, ale nie może mieć wartości domyślnej — pominięcie argumentów dla *parameter_array* zamiast tego spowoduje utworzenie pustej tablicy.

Poniższy przykład ilustruje różne rodzaje parametrów:
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

W *formal_parameter_list* dla `M`, `i` jest wymaganym parametrem ref, `d` jest wymaganym parametrem wartości, `b`, `s`, `o` i `t` są opcjonalnymi parametrami wartości, a `a` jest tablicą parametrów.

Deklaracja metody tworzy oddzielne miejsce deklaracji dla parametrów, parametrów typu i zmiennych lokalnych. Nazwy są wprowadzane do tej przestrzeni deklaracji przez listę parametrów typu i formalną listę parametrów metody i według lokalnych deklaracji zmiennych w *bloku* metody. Występuje błąd dla dwóch elementów członkowskich przestrzeni deklaracji metody, które mają taką samą nazwę. Jest to błąd dla przestrzeni deklaracji metody i przestrzeni lokalnej deklaracji zmiennej obszaru zagnieżdżonej deklaracji, która zawiera elementy o tej samej nazwie.

Wywołanie metody ([wywołania metody](expressions.md#method-invocations)) tworzy kopię, specyficzną dla tego wywołania, parametrów formalnych i zmiennych lokalnych metody, a lista argumentów wywołania przypisuje wartości lub odwołania do zmiennych do nowo utworzonych parametrów formalnych. W *bloku* metody do parametrów formalnych można odwoływać się ich identyfikatory w wyrażeniach *simple_name* ([proste nazwy](expressions.md#simple-names)).

Istnieją cztery rodzaje parametrów formalnych:

*  Parametry wartości, które są zadeklarowane bez żadnych modyfikatorów.
*  Parametry odwołania, które są zadeklarowane za pomocą modyfikatora `ref`.
*  Parametry wyjściowe, które są zadeklarowane za pomocą modyfikatora `out`.
*  Tablice parametrów, które są zadeklarowane za pomocą modyfikatora `params`.

Zgodnie z opisem w [podpisach i przeciążeniu](basic-concepts.md#signatures-and-overloading), modyfikatory `ref` i `out` są częścią podpisu metody, ale modyfikator `params` nie jest.

#### <a name="value-parameters"></a>Parametry wartości

Parametr zadeklarowany bez modyfikatorów jest parametrem wartości. Parametr value odnosi się do zmiennej lokalnej, która pobiera jej wartość początkową z odpowiedniego argumentu dostarczonego w wywołaniu metody.

Gdy parametr formalny jest parametrem wartości, odpowiadający mu argument w wywołaniu metody musi być wyrażeniem, które jest niejawnie konwertowane ([konwersje niejawne](conversions.md#implicit-conversions)) na typ parametru formalnego.

Metoda może przypisywać nowe wartości do parametru value. Takie przypisania mają wpływ tylko na lokalizację magazynu lokalnego reprezentowane przez parametr value — nie mają wpływu na rzeczywisty argument określony w wywołaniu metody.

#### <a name="reference-parameters"></a>Parametry odwołania

Parametr zadeklarowany za pomocą modyfikatora `ref` jest parametrem referencyjnym. W przeciwieństwie do parametru wartości, parametr odwołania nie tworzy nowej lokalizacji magazynu. Zamiast tego parametr odwołania reprezentuje tę samą lokalizację magazynu, co zmienna określona jako argument w wywołaniu metody.

Gdy parametr formalny jest parametrem referencyjnym, odpowiadający mu argument w wywołaniu metody musi zawierać słowo kluczowe `ref`, a następnie *variable_reference* ([precyzyjne reguły określania przypisania](variables.md#precise-rules-for-determining-definite-assignment), które mają być określone) tego samego typu co parametr formalny. Zmienna musi być ostatecznie przypisana, zanim będzie mogła zostać przeniesiona jako parametr referencyjny.

W ramach metody parametr odwołania jest zawsze uznawany za ostatecznie przypisany.

Metoda zadeklarowana jako iterator ([Iteratory](classes.md#iterators)) nie może mieć parametrów referencyjnych.

Przykład
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
tworzy dane wyjściowe
```console
i = 2, j = 1
```

W przypadku wywołania `Swap` w `Main``x` reprezentuje `i` i `y` reprezentuje `j`. W efekcie wywołanie ma wpływ na zamianę wartości `i` i `j`.

W metodzie, która pobiera parametry referencyjne, istnieje możliwość, że wiele nazw reprezentuje tę samą lokalizację magazynu. w przykładzie
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
Wywołanie `F` w `G` przekazuje odwołanie do `s` dla `a` i `b`. W takim przypadku nazwy `s`, `a`i `b` wszystkie odnoszą się do tej samej lokalizacji przechowywania, a wszystkie trzy przydziały modyfikują pole wystąpienia `s`.

#### <a name="output-parameters"></a>Parametry wyjściowe

Parametr zadeklarowany za pomocą modyfikatora `out` jest parametrem wyjściowym. Podobnie jak w przypadku parametru Reference, parametr wyjściowy nie tworzy nowej lokalizacji magazynu. Zamiast tego parametr wyjściowy reprezentuje taką samą lokalizację przechowywania jak zmienna określona jako argument w wywołaniu metody.

Gdy parametr formalny jest parametrem wyjściowym, odpowiadający mu argument w wywołaniu metody musi zawierać słowo kluczowe `out`, a następnie *variable_reference* ([precyzyjne reguły określania przypisania](variables.md#precise-rules-for-determining-definite-assignment), które mają być określone) tego samego typu co parametr formalny. Zmienna nie musi być ostatecznie przypisana, zanim będzie mogła zostać przeniesiona jako parametr wyjściowy, ale po wywołaniu, gdzie zmienna została przeniesiona jako parametr wyjściowy, zmienna jest uważana za ostatecznie przypisaną.

W ramach metody, podobnie jak zmienna lokalna, parametr wyjściowy jest początkowo uznawany za nieprzypisany i musi być ostatecznie przypisany przed użyciem wartości.

Każdy parametr wyjściowy metody musi być ostatecznie przypisany przed zwróceniem metody.

Metoda zadeklarowana jako metoda częściowa ([metody częściowe](classes.md#partial-methods)) lub iterator ([Iteratory](classes.md#iterators)) nie może mieć parametrów wyjściowych.

Parametry wyjściowe są zwykle używane w metodach, które generują wiele wartości zwracanych. Na przykład:
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

Przykład generuje dane wyjściowe:
```console
c:\Windows\System\
hello.txt
```

Należy pamiętać, że zmienne `dir` i `name` mogą być nieprzypisane przed przekazaniem do `SplitPath`i że są uznawane za ostatecznie przypisane po wywołaniu.

#### <a name="parameter-arrays"></a>Tablice parametrów

Parametr zadeklarowany za pomocą modyfikatora `params` jest tablicą parametrów. Jeśli lista parametrów formalnych zawiera tablicę parametrów, musi być ostatnim parametrem na liście i musi być typu tablicy jednowymiarowej. Na przykład typy `string[]` i `string[][]` mogą być używane jako typ tablicy parametrów, ale `string[,]` typu nie może. Nie można połączyć modyfikatora `params` z modyfikatorami `ref` i `out`.

Tablica parametrów zezwala na określenie argumentów na jeden z dwóch sposobów w wywołaniu metody:

*  Argument określony dla tablicy parametrów może być pojedynczym wyrażeniem, które jest niejawnie konwertowane ([konwersje niejawne](conversions.md#implicit-conversions)) na typ tablicy parametrów. W takim przypadku tablica parametrów działa dokładnie tak, jak parametr value.
*  Alternatywnie, wywołanie może określać zero lub więcej argumentów tablicy parametrów, gdzie każdy argument jest wyrażeniem, które jest niejawnie konwertowane ([konwersje niejawne](conversions.md#implicit-conversions)) na typ elementu tablicy parametrów. W takim przypadku wywołanie tworzy wystąpienie typu tablicy parametrów o długości odpowiadającej liczbie argumentów, inicjuje elementy instancji Array z podaną wartością argumentów i używa nowo utworzonego wystąpienia tablicy jako wartości rzeczywistej argument.

Oprócz dopuszczania zmiennej liczby argumentów w wywołaniu, tablica parametrów jest dokładnie równoważna z parametrem wartości ([Parametry wartości](classes.md#value-parameters)) tego samego typu.

Przykład
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
tworzy dane wyjściowe
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

Pierwsze wywołanie `F` po prostu przekazuje tablicę `a` jako parametr wartości. Drugie wywołanie `F` automatycznie tworzy cztery elementy `int[]` z podaną wartością elementu i przekazuje to wystąpienie tablicy jako parametr wartości. Podobnie trzecie wywołanie `F` powoduje utworzenie elementu zero `int[]` i przekazanie tego wystąpienia jako parametru wartości. Drugie i trzecie wywołania są dokładnie równoważne zapisowi:
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

Podczas rozpoznawania przeciążenia metoda z tablicą parametrów może być stosowana w jego normalnej postaci lub w rozwiniętej formie ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)). Rozwinięta forma metody jest dostępna tylko wtedy, gdy normalna forma metody nie ma zastosowania i tylko wtedy, gdy odpowiednia metoda o tej samej sygnaturze, co rozwinięta forma, nie jest już zadeklarowana w tym samym typie.

Przykład
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
tworzy dane wyjściowe
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

W przykładzie dwa z możliwych rozwiniętych formularzy metody z tablicą parametrów są już zawarte w klasie jako metody zwykłe. Te rozwinięte formularze nie są więc brane pod uwagę podczas rozpoznawania przeciążenia, a pierwsze i trzecie wywołania metody wybierają metody zwykłe. Gdy Klasa deklaruje metodę z tablicą parametrów, nierzadko jest również zawierać część rozwiniętych formularzy jako zwykłe metody. Dzięki temu można uniknąć alokacji wystąpienia tablicy, które występuje, gdy zostanie wywołana rozwinięta forma metody z tablicą parametrów.

Gdy typ tablicy parametrów jest `object[]`, potencjalna niejednoznaczność występuje między normalną formą metody a wykorzystaną formą dla jednego parametru `object`. Przyczyna niejednoznaczności polega na tym, że `object[]` jest sama niejawnie przekonwertowana na typ `object`. Niejednoznaczność nie ma jednak żadnego problemu, ponieważ można ją rozwiązać, wstawiając Rzutowanie w razie konieczności.

Przykład
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
tworzy dane wyjściowe
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

W pierwszym i ostatnim wywołaniu `F`jest stosowana normalna forma `F`, ponieważ istnieje niejawna konwersja z typu argumentu na typ parametru (obie są typu `object[]`). W rezultacie rozwiązanie przeciążania wybiera normalną postać `F`i argument jest przenoszona jako parametr wartości regularnej. W drugim i trzecim wywołaniu, normalna forma `F` nie ma zastosowania, ponieważ nie istnieje niejawna konwersja z typu argumentu na typ parametru (typ `object` nie może być niejawnie konwertowany na typ `object[]`). Jednakże rozwinięta forma `F` ma zastosowanie, więc jest wybierana przez metodę rozpoznawania przeciążenia. W związku z tym `object[]` jest tworzony przez wywołanie, a pojedynczy element tablicy jest inicjowany z daną wartością argumentu (która sama jest odwołaniem do `object[]`).

### <a name="static-and-instance-methods"></a>Metody static i instance

Gdy deklaracja metody zawiera modyfikator `static`, ta metoda jest określana jako metoda statyczna. Gdy nie ma modyfikatora `static`, metoda jest uznawana za metodę wystąpienia.

Metoda statyczna nie działa w określonym wystąpieniu i jest błędem czasu kompilacji, aby odwołać się do `this` w metodzie statycznej.

Metoda wystąpienia działa w danym wystąpieniu klasy i można uzyskać do niego dostęp jako `this` ([ten dostęp](expressions.md#this-access)).

Gdy metoda jest przywoływana w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `E.M`, jeśli `M` jest metodą statyczną, `E` musi zwrócić uwagę na typ zawierający `M`, a jeśli `M` jest metodą wystąpienia, `E` musi zwrócić uwagę na wystąpienie typu zawierającego `M`.

Różnice między elementami statycznymi a składowymi wystąpienia są omówione bardziej szczegółowo w [elementach członkowskich static i instance](classes.md#static-and-instance-members).

### <a name="virtual-methods"></a>Metody wirtualne

Gdy deklaracja metody wystąpienia zawiera modyfikator `virtual`, ta metoda jest określana jako metoda wirtualna. Gdy modyfikator `virtual` nie jest obecny, metoda jest uznawana za metodę niewirtualną.

Implementacja metody niewirtualnej jest niezmienna: implementacja jest taka sama, niezależnie od tego, czy metoda jest wywoływana w wystąpieniu klasy, w której jest zadeklarowana, czy też wystąpieniem klasy pochodnej. W przeciwieństwie do implementacji metody wirtualnej można zastąpić klasy pochodne. Proces zastępowania implementacji dziedziczonej metody wirtualnej jest znany jako ***zastępowanie*** tej metody ([metody zastępowania](classes.md#override-methods)).

W wywołaniu metody wirtualnej, ***Typ czasu wykonywania*** wystąpienia, dla którego odbywa się wywołanie określa rzeczywistą implementację metody do wywołania. W wywołaniu metody niewirtualnej ***Typ czasu kompilacji*** wystąpienia jest czynnikiem decydującym. W precyzyjnej terminologii, gdy metoda o nazwie `N` jest wywoływana z listą argumentów `A` w wystąpieniu z typem czasu kompilacji `C` i typem czasu wykonywania `R` (gdzie `R` jest `C` lub Klasa pochodna `C`), wywołanie jest przetwarzane w następujący sposób:

*  Najpierw rozwiązanie przeciążenia jest stosowane do `C`, `N`i `A`w celu wybrania konkretnej metody `M` z zestawu metod zadeklarowanych w i dziedziczonych przez `C`. Jest to opisane w [wywołaniu metody](expressions.md#method-invocations).
*  Następnie Jeśli `M` jest metodą niewirtualną, `M` jest wywoływana.
*  W przeciwnym razie `M` jest metodą wirtualną i zostanie wywołana najbardziej pochodna implementacja `M` w odniesieniu do `R`.

Dla każdej metody wirtualnej zadeklarowanej w lub dziedziczonej przez klasę istnieje ***najbardziej pochodna implementacja*** metody w odniesieniu do tej klasy. Najbardziej pochodna implementacja metody wirtualnej `M` w odniesieniu do klasy `R` jest określana w następujący sposób:

*  Jeśli `R` zawiera `virtual` deklaracji `M`, to najbardziej pochodna implementacja `M`.
*  W przeciwnym razie, jeśli `R` zawiera `override` `M`, to najbardziej pochodna implementacja `M`.
*  W przeciwnym razie najbardziej pochodna implementacja `M` w odniesieniu do `R` jest taka sama jak w przypadku najbardziej pochodnej implementacji `M` w odniesieniu do bezpośredniej klasy podstawowej `R`.

Poniższy przykład ilustruje różnice między metodami wirtualnymi i niewirtualnymi:
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

W przykładzie `A` wprowadza metodę niewirtualną `F` i `G`metodę wirtualną. Klasa `B` wprowadza nową metodę niewirtualną `F`, ukrywając dziedziczone `F`, a także przesłania metodę dziedziczenia `G`. Przykład generuje dane wyjściowe:
```console
A.F
B.F
B.G
B.G
```

Zwróć uwagę, że instrukcja `a.G()` wywołuje `B.G`, a nie `A.G`. Wynika to z faktu, że typ czasu wykonywania wystąpienia (czyli `B`), a nie typ czasu kompilacji wystąpienia (czyli `A`), określa rzeczywistą implementację metody do wywołania.

Ponieważ metody są dozwolone do ukrycia metod dziedziczonych, istnieje możliwość, że Klasa zawiera kilka metod wirtualnych o tej samej sygnaturze. Nie jest to problem niejednoznaczny, ponieważ wszystkie z nich oprócz metoda pochodna są ukryte. w przykładzie
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
klasy `C` i `D` zawierają dwie metody wirtualne o tej samej sygnaturze: wprowadzoną przez `A` i wprowadzoną przez `C`. Metoda wprowadzona przez `C` ukrywa metodę dziedziczoną z `A`. W związku z tym, deklaracja przesłonięcia w `D` przesłania metodę wprowadzoną przez `C`i nie jest możliwe, aby `D` przesłonić metodę wprowadzoną przez `A`. Przykład generuje dane wyjściowe:
```console
B.F
B.F
D.F
D.F
```

Należy zauważyć, że można wywołać ukrytą metodę wirtualną, uzyskując dostęp do wystąpienia `D` za pomocą mniej pochodnego typu, w którym metoda nie jest ukryta.

### <a name="override-methods"></a>Metody przesłonięcia

Gdy deklaracja metody wystąpienia zawiera modyfikator `override`, metoda jest określana jako ***Metoda przesłonięcia***. Metoda przesłonięcia zastępuje dziedziczoną metodę wirtualną z tą samą sygnaturą. Podczas gdy deklaracja metody wirtualnej wprowadza nową metodę, Deklaracja metody przesłonięcia specjalizacji istniejącej dziedziczonej metody wirtualnej, dostarczając nową implementację tej metody.

Metoda zastąpiona przez deklarację `override` jest znana jako ***zastąpiona metoda podstawowa***. Dla metody przesłonięcia `M` zadeklarowanej w klasie `C`, zastąpiona metoda bazowa jest określana przez badanie każdego typu klasy bazowej `C`, rozpoczynając od bezpośredniego typu klasy bazowej `C` i kontynuując każdy kolejny bezpośredni typ klasy bazowej, do momentu w danym typie klasy bazowej, który ma taką samą sygnaturę jak `M` po podstawieniu argumentów typu. Na potrzeby lokalizowania zastąpionej metody bazowej Metoda jest uważana za dostępną, jeśli jest `public`, jeśli jest `protected`, jeśli jest `protected internal`lub jeśli jest `internal` i zadeklarowana w tym samym programie jako `C`.

Błąd czasu kompilacji występuje, chyba że wszystkie poniższe warunki są spełnione dla deklaracji przesłonięcia:

*  Zastąpiona metoda bazowa może być zlokalizowana zgodnie z powyższym opisem.
*  Istnieje dokładnie jedna taka zastąpiona metoda bazowa. To ograniczenie ma wpływ tylko wtedy, gdy typ klasy bazowej jest typem skonstruowanym, w którym Podstawienie argumentów typu powoduje, że sygnatura dwóch metod jest taka sama.
*  Zastąpiona metoda bazowa to metoda wirtualna, abstrakcyjna lub zastępowania. Inaczej mówiąc, zastąpiona metoda bazowa nie może być statyczna ani niewirtualna.
*  Zastąpiona metoda bazowa nie jest metodą zapieczętowana.
*  Metoda override i zastąpiona metoda bazowa mają ten sam typ zwracany.
*  Deklaracja przesłonięcia i zastąpiona metoda bazowa mają takie same zadeklarowane ułatwienia dostępu. Inaczej mówiąc, deklaracja przesłonięcia nie może zmienić dostępności metody wirtualnej. Jednakże jeśli zastąpiona metoda bazowa jest chroniona wewnętrznie i jest zadeklarowana w innym zestawie niż zestaw zawierający metodę przesłonięcia, zadeklarowana dostępność metody override musi być chroniona.
*  Deklaracja przesłonięcia nie określa klauzul typu-Parameter-Constraints. Zamiast tego ograniczenia są dziedziczone z przesłoniętej metody podstawowej. Należy zauważyć, że ograniczenia, które są parametrami typu w zastąpionej metodzie, mogą zostać zastąpione przez argumenty typu w dziedziczonym ograniczeniu. Może to prowadzić do ograniczeń, które nie są dozwolone, gdy jawnie określono, takich jak typy wartości lub typy zapieczętowane.

W poniższym przykładzie pokazano, w jaki sposób zastępujące reguły działają dla klas ogólnych:
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

Deklaracja przesłonięcia może uzyskać dostęp do zastąpionej metody bazowej przy użyciu *base_access* ([dostęp podstawowy](expressions.md#base-access)). w przykładzie
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
Wywołanie `base.PrintFields()` w `B` wywołuje metodę `PrintFields` zadeklarowaną w `A`. *Base_access* wyłącza mechanizm wywoływania wirtualnego i po prostu traktuje metodę podstawową jako metodę niewirtualną. Miało to, że w `B` został zapisany `((A)this).PrintFields()`, spowoduje cykliczne wywołanie metody `PrintFields` zadeklarowanej w `B`, a nie zadeklarowanej w `A`, ponieważ `PrintFields` jest wirtualną, a typem czasu wykonywania `((A)this)` jest `B`.

Tylko poprzez dołączenie modyfikatora `override` może zastąpić inną metodę. We wszystkich innych przypadkach Metoda o tej samej sygnaturze, jako dziedziczona metoda po prostu ukrywa metodę dziedziczenia. w przykładzie
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
Metoda `F` w `B` nie zawiera modyfikatora `override` i w związku z tym nie przesłania metody `F` w `A`. Zamiast tego Metoda `F` w `B` ukrywa metodę w `A`i zostanie zgłoszone ostrzeżenie, ponieważ deklaracja nie zawiera modyfikatora `new`.

w przykładzie
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
Metoda `F` w `B` powoduje ukrycie metody `F` wirtualnej dziedziczonej z `A`. Ponieważ nowy `F` w `B` ma dostęp prywatny, jego zakres zawiera tylko treść klasy `B` i nie rozszerzy się do `C`. W związku z tym, deklaracja `F` w `C` może przesłonić `F` dziedziczone z `A`.

### <a name="sealed-methods"></a>Zapieczętowane metody

Gdy deklaracja metody wystąpienia zawiera modyfikator `sealed`, ta metoda jest określana jako ***Metoda zapieczętowana***. Jeśli deklaracja metody wystąpienia zawiera modyfikator `sealed`, musi również zawierać modyfikator `override`. Użycie modyfikatora `sealed` zapobiega dalszemu zastępowaniu metody przez klasę pochodną.

w przykładzie
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
Klasa `B` udostępnia dwie metody przesłaniania: metodę `F`, która ma modyfikator `sealed` i metodę `G`, która nie. Użycie zapieczętowanych `modifier` `B`uniemożliwia `C` dalsze przesłanianie `F`.

### <a name="abstract-methods"></a>Metody abstrakcyjne

Gdy deklaracja metody wystąpienia zawiera modyfikator `abstract`, ta metoda jest uznawana za ***metodę abstrakcyjną***. Chociaż metoda abstrakcyjna jest niejawnie również metodą wirtualną, nie może mieć modyfikatora `virtual`.

Deklaracja metody abstrakcyjnej wprowadza nową metodę wirtualną, ale nie zapewnia implementacji tej metody. Zamiast tego nieabstrakcyjne klasy pochodne są wymagane do zapewnienia własnych implementacji przez zastąpienie tej metody. Ponieważ metoda abstrakcyjna nie zapewnia rzeczywistej implementacji, *method_body* metody abstrakcyjnej składa się z średnika.

Deklaracje metody abstrakcyjnej są dozwolone tylko w klasach abstrakcyjnych ([klasach abstrakcyjnych](classes.md#abstract-classes)).

w przykładzie
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
Klasa `Shape` definiuje abstrakcyjne pojęcie obiektu kształtu geometrycznego, który może narysować sam siebie. Metoda `Paint` jest abstrakcyjna, ponieważ nie ma żadnej znaczącej implementacji domyślnej. Klasy `Ellipse` i `Box` to konkretne implementacje `Shape`. Ponieważ te klasy nie są abstrakcyjne, są one wymagane do przesłonięcia metody `Paint` i zapewnienia rzeczywistej implementacji.

Jest to błąd czasu kompilacji dla *base_access* ([dostęp podstawowy](expressions.md#base-access)), aby odwołać się do metody abstrakcyjnej. w przykładzie
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
zgłoszono błąd czasu kompilacji dla wywołania `base.F()`, ponieważ odwołuje się do metody abstrakcyjnej.

Deklaracja metody abstrakcyjnej może przesłonić metodę wirtualną. Dzięki temu Klasa abstrakcyjna może wymusić ponowną implementację metody w klasach pochodnych i powoduje, że oryginalna implementacja metody jest niedostępna. w przykładzie
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
Klasa `A` deklaruje metodę wirtualną, Klasa `B` przesłania tę metodę za pomocą metody abstrakcyjnej, a Klasa `C` przesłania metodę abstrakcyjną w celu zapewnienia własnej implementacji.

### <a name="external-methods"></a>Metody zewnętrzne

Gdy deklaracja metody zawiera modyfikator `extern`, ta metoda jest określana jako ***Metoda zewnętrzna***. Metody zewnętrzne są implementowane zewnętrznie, zazwyczaj przy użyciu języka innego C#niż. Ponieważ Deklaracja metody zewnętrznej nie zapewnia żadnej rzeczywistej implementacji, *method_body* metody zewnętrznej składa się z średnika. Metoda zewnętrzna nie może być rodzajowa.

Modyfikator `extern` jest zwykle używany w połączeniu z atrybutem `DllImport` ([współdziałaniem ze składnikami com i Win32](attributes.md#interoperation-with-com-and-win32-components)), umożliwiając Implementowanie zewnętrznych metod przy użyciu bibliotek DLL (bibliotek dołączanych dynamicznie). Środowisko wykonawcze może obsługiwać inne mechanizmy, w których można dostarczyć implementacje metod zewnętrznych.

Gdy metoda zewnętrzna zawiera atrybut `DllImport`, Deklaracja metody musi zawierać również modyfikator `static`. Ten przykład ilustruje użycie modyfikatora `extern` i atrybutu `DllImport`:
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a>Metody częściowe (podsumowanie)

Gdy deklaracja metody zawiera modyfikator `partial`, ta metoda jest uznawana za ***metodę częściową***. Metody częściowe mogą być deklarowane tylko jako elementy członkowskie typów częściowych ([typy częściowe](classes.md#partial-types)) i podlegają wielu ograniczeniom. Metody częściowe są szczegółowo opisane w [metodach częściowych](classes.md#partial-methods).

### <a name="extension-methods"></a>Metody rozszerzające

Gdy pierwszy parametr metody zawiera modyfikator `this`, ta metoda jest określana jako ***Metoda rozszerzenia***. Metody rozszerzające mogą być deklarowane tylko w nieogólnych, niezagnieżdżonych klasach statycznych. Pierwszy parametr metody rozszerzenia nie może mieć modyfikatorów innych niż `this`, a typ parametru nie może być typem wskaźnika.

Poniżej znajduje się przykład klasy statycznej, która deklaruje dwie metody rozszerzające:
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

Metoda rozszerzenia to zwykła metoda statyczna. Ponadto, gdy jej otaczająca Klasa statyczna znajduje się w zakresie, Metoda rozszerzenia może być wywoływana przy użyciu składni wywołania metody wystąpienia ([wywołania metody rozszerzenia](expressions.md#extension-method-invocations)), przy użyciu wyrażenia odbiorcy jako pierwszego argumentu.

Następujący program używa metod rozszerzających zadeklarowanych powyżej:
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

Metoda `Slice` jest dostępna na `string[]`, a metoda `ToInt32` jest dostępna na `string`, ponieważ zostały zadeklarowane jako metody rozszerzania. Znaczenie programu jest takie samo jak w przypadku następujących wywołań metod statycznych:
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a>Treść metody

*Method_body* deklaracji metody składa się z treści bloku, treści wyrażenia lub średnika.

***Typ wyniku*** metody jest `void`, jeśli typem zwracanym jest `void`, lub jeśli metoda jest Async i typem zwracanym jest `System.Threading.Tasks.Task`. W przeciwnym razie typ wyniku metody nieasynchronicznej jest jego typem zwracanym, a typ wyniku metody asynchronicznej z typem zwracanym `System.Threading.Tasks.Task<T>` jest `T`.

Gdy metoda ma `void` typ wyniku i treść bloku, instrukcje `return` ([instrukcja return](statements.md#the-return-statement)) w bloku nie mogą określać wyrażenia. Jeśli wykonywanie bloku metody void kończy się normalnie (to oznacza, że sterowanie przepływem poza końcem treści metody), ta metoda po prostu wraca do bieżącego obiektu wywołującego.
    
Gdy metoda ma `void` wynik i treść wyrażenia, wyrażenie `E` musi być *statement_expression*, a treść jest dokładnie równoważna treści bloku formularza `{ E; }`.
    
Gdy metoda ma typ wyniku inny niż void i treści bloku, każda instrukcja `return` w bloku musi określać wyrażenie, które jest niejawnie konwertowane na typ wyniku. Punkt końcowy treści bloku metody zwracającej wartość nie może być osiągalny. Innymi słowy, w metodzie zwracającej wartość z treści bloku, sterowanie nie jest dozwolone do przepływu poza końcem treści metody.
    
Gdy metoda ma typ wyniku inny niż void i treść wyrażenia, wyrażenie musi być niejawnie konwertowane na typ wyniku, a treść jest dokładnie równoważna z treścią bloku formularza `{ return E; }`.
    
w przykładzie
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
Metoda `F` zwracająca wartość powoduje błąd czasu kompilacji, ponieważ kontrolka może przepływać poza końcem treści metody. Metody `G` i `H` są poprawne, ponieważ wszystkie możliwe ścieżki wykonywania kończą się instrukcją Return, która określa wartość zwracaną. Metoda `I` jest poprawna, ponieważ jej treść jest równoważna z blokiem instrukcji z tylko jedną instrukcją Return.

### <a name="method-overloading"></a>Przeciążanie metody

Reguły rozpoznawania przeciążenia metody są opisane w [wnioskach o typie](expressions.md#type-inference).

## <a name="properties"></a>Właściwości

***Właściwość*** jest członkiem, który zapewnia dostęp do cech obiektu lub klasy. Przykłady właściwości obejmują długość ciągu znaków, rozmiar czcionki, podpis okna, nazwę klienta i tak dalej... Właściwości są naturalnym rozszerzeniem pól — obie są nazwanymi elementami członkowskimi ze skojarzonymi typami, a Składnia służąca do uzyskiwania dostępu do pól i właściwości jest taka sama. Jednak w przeciwieństwie do pól właściwości nie oznacza lokalizacji magazynu. Zamiast tego właściwości mają metody ***dostępu*** określające instrukcje, które mają być wykonywane, gdy ich wartości są odczytywane lub zapisywane. W ten sposób właściwości zapewniają mechanizm kojarzenia akcji z odczytem i pisaniem atrybutów obiektu; Ponadto umożliwiają one obliczenia takich atrybutów.

Właściwości są deklarowane przy użyciu *property_declaration*s:

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

*Property_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)) i prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `static` (Metoda[statyczna i wystąpienia](classes.md#static-and-instance-methods)), `virtual` ([metody wirtualne](classes.md#virtual-methods)), `override` ([metody zastępowania](classes.md#override-methods)), `sealed` ([zapieczętowane metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)) i `extern` (metody[zewnętrzne](classes.md#external-methods)) modyfikatory.

Deklaracje właściwości podlegają tym samym regułom, które są deklaracjami metod ([metodami](classes.md#methods)) w odniesieniu do prawidłowych kombinacji modyfikatorów.

*Typ* deklaracji właściwości określa typ właściwości wprowadzonej przez deklarację, a *MEMBER_NAME* określa nazwę właściwości. Chyba że właściwość jest jawną implementacją składowej interfejsu, *MEMBER_NAME* jest po prostu *identyfikatorem*. W przypadku jawnej implementacji elementu członkowskiego interfejsu ([jawne implementacje elementu członkowskiego interfejsu](interfaces.md#explicit-interface-member-implementations)) *MEMBER_NAME* składa się z *interface_type* po którym następuje "`.`" i *Identyfikator*.

*Typ* właściwości musi być co najmniej taki sam jak wartość właściwości ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).

*Property_body* może składać się z ***treści metody dostępu*** lub ***treści wyrażenia***. W treści metody dostępu *accessor_declarations*, która musi być ujęta w tokeny "`{`" i "`}`", zadeklaruj metody dostępu ([Akcesory](classes.md#accessors)) właściwości. Metody dostępu określają instrukcję wykonywalną skojarzoną z odczytem i zapisem właściwości.

Treść wyrażenia składająca się z `=>` po którym następuje *wyrażenie* `E`, a średnik jest dokładnie równoważny z treścią instrukcji `{ get { return E; } }`i można go użyć tylko do określenia właściwości metody pobierającej, w której wynik metody pobierającej jest określony przez pojedyncze wyrażenie.

*Property_initializer* można udzielić tylko dla automatycznie zaimplementowanej właściwości ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)) i powoduje inicjalizację pola bazowego takich właściwości z wartością podaną przez *wyrażenie*.

Mimo że składnia uzyskiwania dostępu do właściwości jest taka sama jak w przypadku pola, właściwość nie jest sklasyfikowana jako zmienna. Dlatego nie jest możliwe przekazanie właściwości jako argumentu `ref` lub `out`.

Gdy Deklaracja właściwości zawiera modyfikator `extern`, właściwość jest określana jako ***Właściwość zewnętrzna***. Ponieważ Deklaracja właściwości zewnętrznej nie zapewnia żadnej rzeczywistej implementacji, każda z jej *accessor_declarations* składa się z średnika.

### <a name="static-and-instance-properties"></a>Właściwości statyczne i wystąpienia

Gdy Deklaracja właściwości zawiera modyfikator `static`, właściwość jest określana jako ***właściwość statyczna***. Gdy modyfikator `static` nie jest obecny, właściwość jest określana jako ***Właściwość wystąpienia***.

Właściwość statyczna nie jest skojarzona z określonym wystąpieniem i jest błędem czasu kompilacji, aby odwołać się do `this` w obiektach dostępu do właściwości statycznej.

Właściwość instance jest skojarzona z danym wystąpieniem klasy i można uzyskać do niego dostęp jako `this` ([ten dostęp](expressions.md#this-access)) w obiektach dostępu tej właściwości.

Gdy właściwość jest przywoływana w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `E.M`, jeśli `M` jest właściwością statyczną, `E` musi zwrócić uwagę na typ zawierający `M`, a jeśli `M` jest właściwością wystąpienia, E musi zwrócić uwagę na wystąpienie typu zawierającego `M`.

Różnice między elementami statycznymi a składowymi wystąpienia są omówione bardziej szczegółowo w [elementach członkowskich static i instance](classes.md#static-and-instance-members).

### <a name="accessors"></a>Metod dostępu

*Accessor_declarations* właściwości określa instrukcje wykonywalne skojarzone z odczytem i zapisem tej właściwości.

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

Deklaracje metody dostępu składają się z *get_accessor_declaration*, *set_accessor_declaration*lub obu tych elementów. Każda deklaracja metody dostępu składa się z tokenu `get` lub `set` po nim opcjonalne *accessor_modifier* i *accessor_body*.

Użycie *accessor_modifier*s podlega następującym ograniczeniom:

*  Nie można użyć *accessor_modifier* w interfejsie lub w jawnej implementacji elementu członkowskiego interfejsu.
*  Dla właściwości lub indeksatora, który nie ma modyfikatora `override`, *accessor_modifier* jest dozwolony tylko wtedy, gdy właściwość lub indeksator ma metodę dostępu `get` i `set`, a następnie jest dozwolony tylko w jednym z tych metod dostępu.
*  Dla właściwości lub indeksatora, który zawiera modyfikator `override`, metoda dostępu musi być zgodna z *accessor_modifier*, jeśli istnieje, do zastąpienia metody dostępu.
*  *Accessor_modifier* musi deklarować dostępność, która jest ściśle bardziej restrykcyjna niż zadeklarowana dostępność właściwości lub indeksatora. Aby precyzyjnie:
   * Jeśli właściwość lub indeksator ma zadeklarowaną dostępność `public`, *accessor_modifier* może być `protected internal`, `internal`, `protected`lub `private`.
   * Jeśli właściwość lub indeksator ma zadeklarowaną dostępność `protected internal`, *accessor_modifier* może być `internal`, `protected`lub `private`.
   * Jeśli właściwość lub indeksator ma zadeklarowaną dostępność `internal` lub `protected`, *accessor_modifier* musi być `private`.
   * Jeśli właściwość lub indeksator ma zadeklarowaną dostępność `private`, nie można używać *accessor_modifier* .

Dla właściwości `abstract` i `extern` *accessor_body* dla każdego określonego akcesora jest po prostu średnikiem. Nieabstrakcyjna Właściwość niebędąca elementem zewnętrznym może *accessor_body* być średnikiem, w takim przypadku jest ***automatycznie implementowaną właściwością*** ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)). Automatycznie implementowana właściwość musi mieć co najmniej metodę dostępu get. W przypadku akcesorów dowolnej innej nieabstrakcyjnej właściwości nieextern *accessor_body* jest *blok* , który określa instrukcje, które mają zostać wykonane po wywołaniu odpowiedniej metody dostępu.

Metoda dostępu `get` odpowiada metodzie bez parametrów z wartością zwracaną typu właściwości. Poza elementem docelowym przypisania, gdy właściwość jest przywoływana w wyrażeniu, metoda dostępu `get` właściwości jest wywoływana, aby obliczyć wartość właściwości ([wartości wyrażeń](expressions.md#values-of-expressions)). Treść metody dostępu `get` musi być zgodna z regułami dotyczącymi metod zwracających wartość opisaną w [treści metod](classes.md#method-body). W szczególności wszystkie instrukcje `return` w treści metody dostępu `get` muszą określać wyrażenie, które jest niejawnie konwertowane na typ właściwości. Ponadto punkt końcowy metody dostępu `get` nie może być osiągalny.

Metoda dostępu `set` odnosi się do metody z parametrem pojedynczego wartości typu właściwości i `void` typem zwracanym. Niejawny parametr metody dostępu `set` ma zawsze nazwę `value`. Gdy właściwość jest przywoływana jako element docelowy przypisania ([Operatory przypisania](expressions.md#assignment-operators)), lub jako operand `++` lub `--` ([Operatory przyrostu i zmniejszania](expressions.md#postfix-increment-and-decrement-operators)wartości [prefiksu](expressions.md#prefix-increment-and-decrement-operators)), metoda dostępu `set` jest wywoływana z argumentem (którego wartość jest po prawej stronie przypisania lub operandem operatora `++` lub `--`), który zapewnia nową wartość ([przypisanie proste](expressions.md#simple-assignment)). Treść metody dostępu `set` musi być zgodna z regułami dla `void` metod opisanymi w [treści metod](classes.md#method-body). W szczególności instrukcje `return` w treści metody dostępu `set` nie mogą określać wyrażenia. Ponieważ metoda dostępu `set` niejawnie ma parametr o nazwie `value`, jest to błąd czasu kompilacji dla zmiennej lokalnej lub deklaracji stałej w metodzie dostępu `set`.

W oparciu o obecność lub brak `get` i metod dostępu `set`, właściwość jest sklasyfikowana w następujący sposób:

*  Właściwość, która zawiera zarówno metodę dostępu `get`, jak i metodę dostępu `set`, jest określana jako właściwość do ***odczytu i zapisu*** .
*  Właściwość, która ma tylko metodę dostępu `get`, jest określana jako właściwość ***tylko do odczytu*** . Jest to błąd czasu kompilacji dla właściwości tylko do odczytu, aby być elementem docelowym przypisania.
*  Właściwość, która ma tylko metodę dostępu `set`, jest określana jako właściwość ***tylko do zapisu*** . Poza elementem docelowym przypisania jest to błąd czasu kompilacji, który odwołuje się do właściwości tylko do zapisu w wyrażeniu.

w przykładzie
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
formant `Button` deklaruje publiczną właściwość `Caption`. Metoda dostępu `get` właściwości `Caption` zwraca ciąg przechowywany w prywatnym polu `caption`. Metoda dostępu `set` sprawdza, czy nowa wartość różni się od bieżącej wartości, a jeśli tak, przechowuje nową wartość i odmaluje formant. Właściwości często są zgodne ze wzorcem pokazanym powyżej: metoda dostępu `get` po prostu zwraca wartość przechowywaną w polu prywatnym, a metoda dostępu `set` modyfikuje to pole prywatne, a następnie wykonuje wszelkie dodatkowe akcje wymagane do pełnej aktualizacji stanu obiektu.

W przypadku powyższej klasy `Button` poniżej przedstawiono przykład użycia właściwości `Caption`:
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

W tym miejscu metoda dostępu `set` jest wywoływana przez przypisanie wartości do właściwości, a metoda dostępu `get` jest wywoływana przez odwołanie do właściwości w wyrażeniu.

Metody dostępu `get` i `set` właściwości nie są odrębnymi składowymi i nie można deklarować metod dostępu dla właściwości oddzielnie. W związku z tym nie jest możliwe, aby dwie metody dostępu do odczytu i zapisu miały różne ułatwienia dostępu. Przykład
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
nie deklaruje pojedynczej właściwości do odczytu i zapisu. Zamiast tego deklaruje dwie właściwości o tej samej nazwie, jeden tylko do odczytu i jeden tylko do zapisu. Ponieważ dwa elementy członkowskie zadeklarowane w tej samej klasie nie mogą mieć takiej samej nazwy, przykład powoduje błąd w czasie kompilacji.

Gdy Klasa pochodna deklaruje właściwość o takiej samej nazwie jak dziedziczona właściwość, właściwość pochodna ukrywa dziedziczonej właściwości w odniesieniu do odczytu i zapisu. w przykładzie
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
Właściwość `P` w `B` ukrywa właściwość `P` w `A` w odniesieniu do odczytu i zapisu. W tym celu w instrukcjach
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
przypisanie do `b.P` powoduje zgłoszenie błędu czasu kompilacji, ponieważ właściwość `P` tylko do odczytu w `B` ukrywa właściwość `P` tylko do zapisu w `A`. Należy jednak zauważyć, że rzutowanie może być używane w celu uzyskania dostępu do właściwości Hidden `P`.

W przeciwieństwie do pól publicznych, właściwości zapewniają rozdzielenie między wewnętrznym stanem obiektu a jego interfejsem publicznym. Rozważmy przykład:
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

W tym miejscu Klasa `Label` używa dwóch `int` pól, `x` i `y`do przechowywania jej lokalizacji. Lokalizacja jest publicznie ujawniana zarówno jako `X`, jak i Właściwość `Y` i jako właściwość `Location` typu `Point`. Jeśli w przyszłej wersji `Label`będzie bardziej wygodne przechowywanie lokalizacji jako `Point` wewnętrznie, zmiana może zostać wprowadzona bez wpływu na publiczny interfejs klasy:
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

Miało `x` i `y` zamiast tego zostały `public readonly` pola, nie byłoby możliwe dokonanie takiej zmiany w klasie `Label`.

Ujawnienie stanu za poorednictwem właściwości nie musi być mniej wydajne niż bezpośrednie udostępnianie pól. W szczególności, gdy właściwość nie jest wirtualna i zawiera tylko niewielką ilość kodu, środowisko wykonawcze może zastępować wywołania metod dostępu przy użyciu rzeczywistego kodu metod dostępu. Ten proces jest nazywany ***dekreśleniem***i sprawia, że dostęp do właściwości jest skuteczny jako dostęp do pola, a jeszcze zachowuje zwiększoną elastyczność właściwości.

Ponieważ wywoływanie metody dostępu `get` jest koncepcyjnie równoważne odczytywaniu wartości pola, jest uznawany za zły styl programowania dla metod dostępu `get`, aby mieć zauważalne efekty uboczne. w przykładzie
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
wartość właściwości `Next` zależy od tego, ile razy uzyskano dostęp do właściwości. W efekcie uzyskanie dostępu do właściwości daje zauważalny efekt uboczny, a właściwość powinna zostać zaimplementowana jako metoda.

Konwencja "brak efektów ubocznych" dla metod dostępu `get` nie oznacza, że metody dostępu `get` powinny zawsze być zapisane, aby po prostu zwracały wartości przechowywane w polach. W rzeczywistości metody dostępu `get` często obliczają wartość właściwości poprzez dostęp do wielu pól lub wywoływanie metod. Jednak prawidłowo zaprojektowany `get` metoda dostępu nie wykonuje żadnych działań, które powodują zauważalne zmiany stanu obiektu.

Właściwości można użyć, aby opóźnić inicjalizację zasobu do momentu, w którym jest ono najpierw przywoływane. Na przykład:
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

Klasa `Console` zawiera trzy właściwości, `In`, `Out`i `Error`, które reprezentują odpowiednio standardowe dane wejściowe, wyjściowe i błędy. Przez udostępnienie tych elementów członkowskich jako właściwości, Klasa `Console` może opóźnić ich inicjalizację do momentu ich faktycznego użycia. Na przykład po pierwszym odwołaniu się do właściwości `Out`, jak w
```csharp
Console.Out.WriteLine("hello, world");
```
zostanie utworzony podstawowy `TextWriter` urządzenia wyjściowego. Jeśli jednak aplikacja nie tworzy żadnych odwołań do właściwości `In` i `Error`, wówczas żadne obiekty nie są tworzone dla tych urządzeń.

### <a name="automatically-implemented-properties"></a>Automatycznie implementowane właściwości

Automatycznie implementowana Właściwość (lub ***Właściwość automatyczna*** dla krótkich) jest nieabstrakcyjną właściwością nieextern z tylko średnikami. Funkcja autowłaściwości musi mieć metodę dostępu get i opcjonalnie może mieć metodę dostępu set.

Gdy właściwość jest określona jako automatycznie implementowana właściwość, ukryte pole zapasowe jest automatycznie dostępne dla właściwości, a metody dostępu są implementowane w celu odczytu i zapisu w tym polu zapasowym. Jeśli właściwość autoproperty nie ma metody dostępu set, pole zapasowe jest uznawane za `readonly` ([pola tylko do odczytu](classes.md#readonly-fields)). Podobnie jak w przypadku pola `readonly`, metoda pobierająca, która jest jedyną właściwością, może również być przypisana do w treści konstruktora klasy otaczającej. Takie przypisanie przypisuje bezpośrednio do pola zapasowego tylko do odczytu właściwości.

Właściwość autoproperty może opcjonalnie mieć *property_initializer*, która jest stosowana bezpośrednio do pola zapasowego jako *variable_initializer* ([inicjatory zmiennych](classes.md#variable-initializers)).

Poniższy przykład:
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
jest równoważne następującej deklaracji:
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

Poniższy przykład:
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
jest równoważne następującej deklaracji:
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

Zwróć uwagę, że przypisania do pola tylko do odczytu są dozwolone, ponieważ występują w konstruktorze.


### <a name="accessibility"></a>Ułatwienia dostępu

Jeśli metoda dostępu ma *accessor_modifier*, domena dostępności ([domeny dostępu](basic-concepts.md#accessibility-domains)) metody dostępu jest określana przy użyciu deklarowanej dostępności *accessor_modifier*. Jeśli metoda dostępu nie ma *accessor_modifier*, domena dostępności metody dostępu jest określana na podstawie zadeklarowanej dostępności właściwości lub indeksatora.

Obecność *accessor_modifier* nie ma wpływu na wyszukiwanie elementów członkowskich ([operatorów](expressions.md#operators)) ani rozpoznawanie przeciążenia ([rozwiązanie przeciążenia](expressions.md#overload-resolution)). Modyfikatory właściwości lub indeksatora zawsze określają, która właściwość lub indeksator jest powiązany, niezależnie od kontekstu dostępu.

Po wybraniu określonej właściwości lub indeksatora domeny dostępności konkretnych metod dostępu są używane do określenia, czy to użycie jest prawidłowe:

*  Jeśli użycie ma charakter wartości ([wartości wyrażeń](expressions.md#values-of-expressions)), metoda dostępu `get` musi istnieć i być dostępna.
*  Jeśli użycie ma charakter docelowy przypisania prostego ([przypisanie proste](expressions.md#simple-assignment)), metoda dostępu `set` musi istnieć i być dostępna.
*  Jeśli użycie ma charakter docelowy przypisania złożonego ([przypisanie złożone](expressions.md#compound-assignment)) lub jako obiekt docelowy operatory `++` lub `--` ([elementy członkowskie funkcji](expressions.md#function-members).9, [wyrażenia wywołania](expressions.md#invocation-expressions)), zarówno metody dostępu `get`, jak i akcesor `set` muszą istnieć i być dostępne.

W poniższym przykładzie właściwość `A.Text` jest ukryta przez właściwość `B.Text`, nawet w kontekstach, w których wywoływana jest tylko metoda dostępu `set`. Z kolei Właściwość `B.Count` nie jest dostępna dla klasy `M`, więc zamiast niej zostanie użyta Właściwość dostępna `A.Count`.

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

Metoda dostępu użyta do zaimplementowania interfejsu może nie mieć *accessor_modifier*. Jeśli do zaimplementowania interfejsu jest używana tylko jedna metoda dostępu, inne metody dostępu można zadeklarować za pomocą *accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>Metody dostępu do właściwości Virtual, Sealed, override i abstract

Deklaracja właściwości `virtual` określa, że metody dostępu do właściwości są wirtualne. Modyfikator `virtual` ma zastosowanie do obu metod dostępu właściwości do odczytu i zapisu — nie jest to możliwe tylko w przypadku jednej metody dostępu do odczytu i zapisu, która ma być wirtualna.

Deklaracja właściwości `abstract` określa, że metody dostępu do właściwości są wirtualne, ale nie zapewniają rzeczywistej implementacji metod dostępu. Zamiast tego nieabstrakcyjne klasy pochodne są wymagane do zapewnienia własnych implementacji dla metod dostępu poprzez Zastępowanie właściwości. Ponieważ metoda dostępu dla deklaracji właściwości abstrakcyjnej nie oferuje rzeczywistej implementacji, jej *accessor_body* po prostu składa się z średnika.

Deklaracja właściwości, która zawiera zarówno Modyfikatory `abstract`, jak i `override` określa, że właściwość jest abstrakcyjna i przesłania Właściwość bazową. Metody dostępu takich właściwości są również abstrakcyjne.

Deklaracje właściwości abstrakcyjnych są dozwolone tylko w klasach abstrakcyjnych ([klasach abstrakcyjnych](classes.md#abstract-classes)). Metody dostępu dziedziczonej właściwości wirtualnej można zastąpić w klasie pochodnej przez dołączenie deklaracji właściwości, która określa `override` dyrektywę. Jest to tzw. ***zastępowanie deklaracji właściwości***. Zastępowanie deklaracji właściwości nie deklaruje nowej właściwości. Zamiast tego po prostu wyspecjalizowane są implementacje metod dostępu istniejącej właściwości wirtualnej.

Zastępowanie deklaracji właściwości musi określać dokładnie te same Modyfikatory dostępności, typ i nazwę, co Właściwość dziedziczona. Jeśli dziedziczona właściwość ma tylko jedną metodę dostępu (tj., jeśli dziedziczona właściwość jest tylko do odczytu lub tylko do zapisu), właściwość zastępująca musi zawierać tylko ten akcesor. Jeśli dziedziczona Właściwość zawiera obie metody dostępu (tj., jeśli dziedziczona właściwość jest do odczytu i zapisu), zastępujący właściwość może zawierać jedną metodę dostępu lub obu akcesorów.

Zastępowanie deklaracji właściwości może zawierać modyfikator `sealed`. Użycie tego modyfikatora uniemożliwia klasie pochodnej bardziej Zastępowanie właściwości. Metody dostępu właściwości zapieczętowanej są również zapieczętowane.

Z wyjątkiem różnic w składni deklaracji i wywołania, Virtual, Sealed, override i abstract metody dostępu zachowują się dokładnie tak, jak wirtualne, zapieczętowane, przesłonięcie i abstrakcyjne. Zgodnie z zasadami opisanymi w [metodach wirtualnych](classes.md#virtual-methods), [metodami przesłonięcia](classes.md#override-methods), [metodami zapieczętowanymi](classes.md#sealed-methods)i [metodami abstrakcyjnymi](classes.md#abstract-methods) stosuje się tak, jakby metody dostępu były metodami odpowiedniej formy:

*  Metoda dostępu `get` odpowiada metodzie bez parametrów z wartością zwracaną typu właściwości i tymi samymi modyfikatorami co Właściwość zawierająca.
*  Metoda dostępu `set` odnosi się do metody z parametrem pojedynczego wartości typu właściwości, `void` typem zwracanym i tymi samymi modyfikatorami co Właściwość zawierająca.

w przykładzie
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
`X` jest wirtualną właściwość tylko do odczytu, `Y` jest wirtualną właściwością odczytu i zapisu, a `Z` jest abstrakcyjną właściwością do odczytu i zapisu. Ponieważ `Z` jest abstrakcyjna, `A` zawierającej klasy również musi być zadeklarowany jako abstract.

Poniżej przedstawiono klasę, która dziedziczy z `A`:
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

W tym miejscu deklaracje `X`, `Y`i `Z` zastępują deklaracje właściwości. Każda deklaracja właściwości dokładnie dopasowuje Modyfikatory dostępności, typ i nazwę odpowiedniej dziedziczonej właściwości. Metoda dostępu `get` `X` i metoda dostępu `set` `Y` używają słowa kluczowego `base`, aby uzyskać dostęp do dziedziczonych akcesorów. Deklaracja `Z` przesłania zarówno abstrakcyjnych metod dostępu, w tym, że nie istnieją żadne oczekujące składowe funkcji abstrakcyjnych w `B`, a `B` może być klasą abstrakcyjną.

Gdy właściwość jest zadeklarowana jako `override`, wszystkie zastąpione metody dostępu muszą być dostępne dla zastępujący kod. Ponadto zadeklarowana dostępność zarówno właściwości, jak i indeksatora, jak i metod dostępu musi być zgodna z przesłoniętą składową i dostępem. Na przykład:
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a>Zdarzenia

***Zdarzenie*** jest członkiem, który umożliwia obiektowi lub klasy udostępnianie powiadomień. Klienci mogą dołączyć kod wykonywalny zdarzeń przez dostarczenie ***obsługi zdarzeń***.

Zdarzenia są deklarowane przy użyciu *event_declaration*s:

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

*Event_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)) i prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `static` (Metoda[statyczna i wystąpienia](classes.md#static-and-instance-methods)), `virtual` ([metody wirtualne](classes.md#virtual-methods)), `override` ([metody zastępowania](classes.md#override-methods)), `sealed` ([zapieczętowane metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)) i `extern` (metody[zewnętrzne](classes.md#external-methods)) modyfikatory.

Deklaracje zdarzeń podlegają tym samym regułom, które są deklaracjami metod ([metodami](classes.md#methods)) w odniesieniu do prawidłowych kombinacji modyfikatorów.

*Typ* deklaracji zdarzenia musi być *delegate_type* ([typy referencyjne](types.md#reference-types)), a *delegate_type* musi być co najmniej tak samo jak w przypadku samego zdarzenia ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).

Deklaracja zdarzenia może zawierać *event_accessor_declarations*. Jednakże jeśli nie, dla zdarzeń niezewnętrznych, kompilator dostarcza je automatycznie ([zdarzenia podobne do pól](classes.md#field-like-events)); dla zdarzeń extern, metody dostępu są udostępniane zewnętrznie.

Deklaracja zdarzenia, która pomija *event_accessor_declarations* definiuje jedno lub więcej zdarzeń — jeden dla każdej *variable_declarator*s. Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych przez takie *event_declaration*.

Jest to błąd czasu kompilacji dla *event_declaration* , aby uwzględnić zarówno modyfikator `abstract`, jak i rozdzieloną *event_accessor_declarations*nawiasów klamrowych.

Gdy deklaracja zdarzenia zawiera modyfikator `extern`, zdarzenie jest określane jako ***zdarzenie zewnętrzne***. Ponieważ deklaracja zdarzenia zewnętrznego nie oferuje rzeczywistej implementacji, jest to błąd, aby uwzględnić zarówno modyfikator `extern`, jak i *event_accessor_declarations*.

Jest to błąd czasu kompilacji dla *variable_declarator* deklaracji zdarzenia z modyfikatorem `abstract` lub `external` w celu uwzględnienia *variable_initializer*.

Zdarzenia można użyć jako lewego operandu operatorów `+=` i `-=` ([przypisanie zdarzenia](expressions.md#event-assignment)). Te operatory służą odpowiednio do dołączania obsługi zdarzeń do lub usuwania programów obsługi zdarzeń ze zdarzenia, a także Modyfikatory dostępu dla zdarzenia kontrolują konteksty, w których takie operacje są dozwolone.

Ponieważ `+=` i `-=` są jedynymi operacjami, które są dozwolone dla zdarzenia poza typem, który deklaruje zdarzenie, kod zewnętrzny może dodawać i usuwać programy obsługi dla zdarzenia, ale nie może w żaden inny sposób uzyskać ani zmodyfikować podstawowej listy programów obsługi zdarzeń.

W operacji `x += y` lub `x -= y`, gdy `x` jest zdarzeniem, a odwołanie ma miejsce poza typem, który zawiera deklarację `x`, wynik operacji ma typ `void` (w przeciwieństwie do typu `x`, z wartością `x` po przypisaniu). Ta zasada zabrania kodowi zewnętrznemu przeanalizować bazowego delegata zdarzenia.

Poniższy przykład pokazuje, jak programy obsługi zdarzeń są dołączone do wystąpień klasy `Button`:
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

W tym miejscu Konstruktor wystąpienia `LoginDialog` tworzy dwa wystąpienia `Button` i dołącza obsługę zdarzeń do zdarzeń `Click`.

### <a name="field-like-events"></a>Zdarzenia podobne do pól

W tekście programu klasy lub struktury, która zawiera deklarację zdarzenia, można użyć określonych zdarzeń, takich jak pola. Do użycia w ten sposób, zdarzenie nie może być `abstract` ani `extern`i nie może jawnie zawierać *event_accessor_declarations*. Takiego zdarzenia można użyć w dowolnym kontekście, który zezwala na pole. Pole zawiera delegata ([delegatów](delegates.md)), który odwołuje się do listy programów obsługi zdarzeń, które zostały dodane do zdarzenia. Jeśli nie dodano żadnych programów obsługi zdarzeń, pole zawiera `null`.

w przykładzie
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
`Click` jest używany jako pole w klasie `Button`. Jak pokazano w przykładzie, pole może być badane, modyfikowane i używane w wyrażeniach delegatów wywołań. Metoda `OnClick` w klasie `Button` "wywołuje" `Click` zdarzenie. Pojęcie związane z wywoływaniem zdarzenia jest dokładnie równoważne do wywołania delegata reprezentowanego przez zdarzenie, co oznacza, że nie istnieją żadne specjalne konstrukcje języka do wywoływania zdarzeń. Należy zauważyć, że wywołanie delegata jest poprzedzone przez sprawdzenie, czy delegat ma wartość różną od null.

Poza deklaracją klasy `Button`, element członkowski `Click` może być używany tylko po lewej stronie operatorów `+=` i `-=`, jak w
```csharp
b.Click += new EventHandler(...);
```
dołącza delegata do listy wywołań zdarzenia `Click` i
```csharp
b.Click -= new EventHandler(...);
```
który usuwa delegata z listy wywołań zdarzenia `Click`.

Podczas kompilowania zdarzenia przypominającego pole, kompilator automatycznie tworzy magazyn do przechowywania delegata i tworzy metody dostępu dla zdarzenia, które dodaje lub usuwa programy obsługi zdarzeń do pola delegat. Operacje dodawania i usuwania są bezpieczne dla wątków i mogą (ale nie muszą) być wykonywane podczas utrzymywania blokady ([instrukcja Lock](statements.md#the-lock-statement)) na obiekcie zawierającym zdarzenie wystąpienia lub obiekt typu ([wyrażenia tworzenia obiektów anonimowych](expressions.md#anonymous-object-creation-expressions)) dla zdarzenia statycznego.

W rezultacie deklaracja zdarzenia wystąpienia formularza:
```csharp
class X
{
    public event D Ev;
}
```
zostanie skompilowany do dowolnego elementu równoważnego:
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
W ramach klasy `X`odwołania do `Ev` po lewej stronie operatorów `+=` i `-=` powodują wywoływanie metod dodawania i usuwania. Wszystkie inne odwołania do `Ev` są kompilowane w celu odwoływania się do pola ukrytego `__Ev` zamiast tego ([dostęp do elementów członkowskich](expressions.md#member-access)). Nazwa "`__Ev`" jest arbitralna; pole ukryte może mieć dowolną nazwę lub Brak nazwy.

### <a name="event-accessors"></a>Metody dostępu zdarzeń

Deklaracje zdarzeń zwykle pomijają *event_accessor_declarations*, tak jak w powyższym przykładzie `Button`. Jedną z sytuacji, w której należy to zrobić, jest sytuacja, w której koszt magazynowania jednego pola na zdarzenie nie jest akceptowalny. W takich przypadkach Klasa może zawierać *event_accessor_declarations* i używać mechanizmu prywatnego do przechowywania listy programów obsługi zdarzeń.

*Event_accessor_declarations* zdarzenia określa instrukcje wykonywalne skojarzone z dodawaniem i usuwaniem obsługi zdarzeń.

Deklaracje metody dostępu składają się z *add_accessor_declaration* i *remove_accessor_declaration*. Każda deklaracja metody dostępu składa się z tokenu `add` lub `remove`, po którym następuje *blok*. *Blok* skojarzony z *add_accessor_declaration* określa instrukcje do wykonania po dodaniu programu obsługi zdarzeń, a *blok* skojarzony z *remove_accessor_declaration* określa instrukcje do wykonania po usunięciu programu obsługi zdarzeń.

Każda *add_accessor_declaration* i *remove_accessor_declaration* odnosi się do metody z parametrem pojedynczego wartości typu zdarzenia i `void` typem zwracanym. Niejawny parametr metody dostępu do zdarzenia nosi nazwę `value`. Gdy zdarzenie jest używane w przypisaniu zdarzenia, zostanie użyta odpowiednia metoda dostępu do zdarzeń. W odróżnieniu od tego, czy operator przypisania jest `+=`, używana jest metoda Add akcesor, a jeśli operator przypisania jest `-=`, zostanie użyta metoda dostępu Remove. W obu przypadkach jest używany operand z prawej strony operatora przypisania jako argument metody dostępu do zdarzeń. Blok *add_accessor_declaration* lub *remove_accessor_declaration* musi być zgodny z regułami `void` metodami opisanymi w [treści metody](classes.md#method-body). W szczególności instrukcje `return` w takich blokach nie mogą określać wyrażenia.

Ponieważ metoda dostępu do zdarzenia niejawnie ma parametr o nazwie `value`, jest to błąd czasu kompilacji dla zmiennej lokalnej lub stała zadeklarowana w metodzie dostępu do zdarzeń, która ma tę nazwę.

w przykładzie
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
Klasa `Control` implementuje mechanizm wewnętrznego magazynu dla zdarzeń. Metoda `AddEventHandler` kojarzy wartość delegata z kluczem, Metoda `GetEventHandler` zwraca delegata aktualnie skojarzony z kluczem, a metoda `RemoveEventHandler` usuwa delegata jako procedurę obsługi zdarzeń dla określonego zdarzenia. Zakładając, że podstawowy mechanizm magazynowania jest w taki sposób, że nie ma żadnego kosztu kojarzenia wartości delegata `null` z kluczem, a w związku z tym nieobsłużone zdarzenia nie zużywają magazynu.

### <a name="static-and-instance-events"></a>Zdarzenia statyczne i wystąpienia

Gdy deklaracja zdarzenia zawiera modyfikator `static`, zdarzenie jest określane jako ***zdarzenie statyczne***. Gdy `static` modyfikator nie jest obecny, zdarzenie jest określane jako ***zdarzenie wystąpienia***.

Zdarzenie statyczne nie jest skojarzone z określonym wystąpieniem i jest błędem czasu kompilowania, który odwołuje się do `this` w obiektach dostępu do statycznego zdarzenia.

Zdarzenie wystąpienia jest skojarzone z danym wystąpieniem klasy i można uzyskać do niego dostęp jako `this` ([ten dostęp](expressions.md#this-access)) w odniesieniu do tego zdarzenia.

Gdy do zdarzenia odwołuje się *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `E.M`, jeśli `M` jest zdarzeniem statycznym, `E` musi zwrócić uwagę na typ zawierający `M`, a jeśli `M` jest zdarzenie wystąpienia, E musi zwrócić uwagę na wystąpienie typu zawierającego `M`.

Różnice między elementami statycznymi a składowymi wystąpienia są omówione bardziej szczegółowo w [elementach członkowskich static i instance](classes.md#static-and-instance-members).

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Wirtualne, zapieczętowane, przesłonięcie i abstrakcyjne metody dostępu do zdarzeń

Deklaracja zdarzenia `virtual` określa, że metody dostępu tego zdarzenia są wirtualne. Modyfikator `virtual` ma zastosowanie do obu metod dostępu zdarzenia.

Deklaracja zdarzenia `abstract` określa, że metody dostępu do zdarzenia są wirtualne, ale nie zapewniają rzeczywistej implementacji metod dostępu. Zamiast tego nieabstrakcyjne klasy pochodne są wymagane do zapewnienia własnych implementacji dla metod dostępu przez zastąpienie zdarzenia. Ze względu na to, że deklaracja zdarzenia abstrakcyjnego nie zawiera rzeczywistej implementacji, nie może zawierać *event_accessor_declarations*rozdzielonych nawiasami klamrowymi.

Deklaracja zdarzenia, która zawiera zarówno Modyfikatory `abstract`, jak i `override` określa, że zdarzenie jest abstrakcyjne i przesłania zdarzenie podstawowe. Metody dostępu takiego zdarzenia są również abstrakcyjne.

Deklaracje zdarzeń abstrakcyjnych są dozwolone tylko w klasach abstrakcyjnych ([klasach abstrakcyjnych](classes.md#abstract-classes)).

Metody dostępu dziedziczonego zdarzenia wirtualnego można zastąpić w klasie pochodnej przez dołączenie deklaracji zdarzenia, która określa modyfikator `override`. Jest to tzw. ***zastępowanie deklaracji zdarzenia***. Zastępowanie deklaracji zdarzenia nie deklaruje nowego zdarzenia. Zamiast tego po prostu wyspecjalizowane są implementacje metod dostępu istniejącego zdarzenia wirtualnego.

Zastępowanie deklaracji zdarzenia musi określać dokładnie te same Modyfikatory dostępności, typ i nazwę, co zastąpione zdarzenie.

Zastępowanie deklaracji zdarzenia może zawierać modyfikator `sealed`. Użycie tego modyfikatora zapobiega dalszemu zastępowaniu zdarzenia przez klasę pochodną. Metody dostępu zapieczętowanego zdarzenia są również zapieczętowane.

Jest to błąd czasu kompilacji dla zastępujący deklaracji zdarzenia w celu uwzględnienia modyfikatora `new`.

Z wyjątkiem różnic w składni deklaracji i wywołania, Virtual, Sealed, override i abstract metody dostępu zachowują się dokładnie tak, jak wirtualne, zapieczętowane, przesłonięcie i abstrakcyjne. Zgodnie z zasadami opisanymi w [metodach wirtualnych](classes.md#virtual-methods), [metodami przesłonięcia](classes.md#override-methods), [metodami zapieczętowanymi](classes.md#sealed-methods)i [metodami abstrakcyjnymi](classes.md#abstract-methods) stosuje się tak, jakby metody dostępu były metodami odpowiedniego formularza. Każdy akcesor odnosi się do metody z parametrem jednowartościowym typu zdarzenia, `void` zwracanym typem i tego samego modyfikatora co zawiera zdarzenie.

## <a name="indexers"></a>Indeksatory

***Indeksator*** jest członkiem, który umożliwia indeksowanie obiektu w taki sam sposób jak tablica. Indeksatory są deklarowane przy użyciu *indexer_declaration*s:

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

*Indexer_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)) i prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `virtual` ([metody wirtualne](classes.md#virtual-methods)), `override` ([Zastąp metody](classes.md#override-methods)), `sealed` ([zapieczętowane metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)) i `extern` ([metody zewnętrzne](classes.md#external-methods)) modyfikatory.

Deklaracje indeksatora podlegają tym samym zasadom, które są deklaracjami metod ([metodami](classes.md#methods)) w odniesieniu do prawidłowych kombinacji modyfikatorów, z wyjątkiem tego, że modyfikator static nie jest dozwolony w deklaracji indeksatora.

Modyfikatory `virtual`, `override`i `abstract` wzajemnie się wykluczają, chyba że w jednym przypadku. Modyfikatory `abstract` i `override` mogą być używane razem, aby abstrakcyjny indeksator mógł zastąpić wirtualne.

*Typ* deklaracji indeksatora określa typ elementu indeksatora wprowadzonego przez deklarację. Jeśli indeksator *nie jest* jawną implementacją składowej interfejsu, następuje słowo kluczowe `this`. Dla jawnej implementacji elementu członkowskiego interfejsu *Typ* następuje *interface_type*, "`.`" i słowa kluczowego `this`. W przeciwieństwie do innych elementów członkowskich indeksatory nie mają nazw zdefiniowanych przez użytkownika.

*Formal_parameter_list* określa parametry indeksatora. Lista parametrów formalnych indeksatora odpowiada metodzie metody ([parametrów metody](classes.md#method-parameters)), z tą różnicą, że musi być określony co najmniej jeden parametr, i że Modyfikatory parametrów `ref` i `out` są niedozwolone.

*Typ* indeksatora i każdy z typów, do których odwołuje się *formal_parameter_list* musi być co najmniej tak samo jak indeksator ([ograniczenia dotyczące dostępności](basic-concepts.md#accessibility-constraints)).

*Indexer_body* może składać się z ***treści metody dostępu*** lub ***treści wyrażenia***. W treści metody dostępu *accessor_declarations*, która musi być ujęta w tokeny "`{`" i "`}`", zadeklaruj metody dostępu ([Akcesory](classes.md#accessors)) właściwości. Metody dostępu określają instrukcję wykonywalną skojarzoną z odczytem i zapisem właściwości.

Treść wyrażenia składająca się z "`=>`", po którym następuje wyrażenie `E` i średnik jest dokładnie równoważne z treścią instrukcji `{ get { return E; } }`i można go użyć tylko do określenia indeksatorów tylko do pobierającego, w których wynik metody pobierającej jest określony przez pojedyncze wyrażenie.

Mimo że składnia uzyskiwania dostępu do elementu indeksatora jest taka sama jak dla elementu tablicy, element indeksatora nie jest klasyfikowany jako zmienna. Z tego względu nie jest możliwe przekazanie elementu indeksatora jako argumentu `ref` lub `out`.

Formalna lista parametrów indeksatora definiuje sygnaturę ([sygnatury i Przeciążenie](basic-concepts.md#signatures-and-overloading)) indeksatora. W celu podpisania indeksatora składa się z liczby i typów jego parametrów formalnych. Typ elementu i nazwy parametrów formalnych nie są częścią podpisu indeksatora.

Sygnatura indeksatora musi różnić się od sygnatur wszystkich innych indeksatorów zadeklarowanych w tej samej klasie.

Indeksatory i właściwości są bardzo podobne do koncepcji, ale różnią się w następujący sposób:

*  Właściwość jest identyfikowana za pomocą jej nazwy, podczas gdy indeksator jest identyfikowany przez jego sygnaturę.
*  Dostęp do właściwości uzyskuje się za pomocą *simple_name* ([nazw prostych](expressions.md#simple-names)) *lub member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)), podczas gdy dostęp do elementu indeksatora uzyskuje się za pomocą *element_access* ([dostęp do indeksatora](expressions.md#indexer-access)).
*  Właściwość może być `static` składową, natomiast indeksator jest zawsze składową wystąpienia.
*  Metoda dostępu `get` właściwość odpowiada metodzie bez parametrów, podczas gdy metoda dostępu `get` indeksatora odpowiada metodzie z tą samą formalną listą parametrów, co indeksator.
*  Metoda dostępu `set` właściwość odpowiada metodzie z pojedynczym parametrem o nazwie `value`, podczas gdy `set` metoda dostępu indeksatora odpowiada metodzie z tą samą formalną listą parametrów, co indeksator, oraz dodatkowym parametrem o nazwie `value`.
*  Jest to błąd czasu kompilacji dla metody dostępu indeksatora, aby zadeklarować zmienną lokalną o takiej samej nazwie jak parametr indeksatora.
*  W deklaracji właściwości zastępują dostęp do właściwości dziedziczonej przy użyciu składni `base.P`, gdzie `P` jest nazwą właściwości. W przypadku przesłaniania deklaracji indeksatora Dziedziczony indeksator jest dostępny przy użyciu składni `base[E]`, gdzie `E` jest rozdzielaną przecinkami listą wyrażeń.
*  Nie ma koncepcji "automatycznie zaimplementowany indeksator". Wystąpił błąd nieabstrakcyjnego, niezewnętrznego indeksatora z dostępem średników.

Oprócz tych różnic wszystkie reguły zdefiniowane w [metodach dostępu](classes.md#accessors) i [automatycznie zaimplementowane właściwości](classes.md#automatically-implemented-properties) mają zastosowanie do metod dostępu indeksatora oraz metod dostępu do właściwości.

Gdy deklaracja indeksatora zawiera modyfikator `extern`, indeksator jest uznawany za ***zewnętrzny indeksator***. Ponieważ zewnętrzna deklaracja indeksatora nie zapewnia żadnej rzeczywistej implementacji, każda z jej *accessor_declarations* składa się z średnika.

Poniższy przykład deklaruje klasę `BitArray`, która implementuje indeksator do uzyskiwania dostępu do poszczególnych bitów w tablicy bitowej.
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

Wystąpienie klasy `BitArray` zużywa znacznie mniej pamięci niż odpowiadające im `bool[]` (ponieważ każda wartość dawna zajmuje tylko jeden bit zamiast ostatniego z nich), ale umożliwia wykonywanie tych samych operacji co `bool[]`.

W poniższej klasie `CountPrimes` używany jest `BitArray` i klasyczny algorytm "Sit", aby obliczyć liczbę wartości granicznych z przedziału od 1 do:
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

Należy zauważyć, że Składnia służąca do uzyskiwania dostępu do elementów `BitArray` jest dokładnie taka sama jak w przypadku `bool[]`.

W poniższym przykładzie przedstawiono klasę siatki 26 * 10, która ma indeksator z dwoma parametrami. Pierwszy parametr musi być wielką lub małą literą w zakresie A-Z, a drugi musi być liczbą całkowitą z zakresu 0-9.

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a>Przeciążanie indeksatora

Reguły rozpoznawania przeciążenia indeksatora są opisane w [wnioskach o typie](expressions.md#type-inference).

## <a name="operators"></a>Operatory

***Operator*** jest członkiem, który definiuje znaczenie operatora wyrażenia, który można zastosować do wystąpień klasy. Operatory są deklarowane przy użyciu *operator_declaration*s:

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

Istnieją trzy kategorie operatorów z możliwością przeciążenia: Jednoargumentowe operatory ([operatory jednoargumentowe](classes.md#unary-operators)), operatory binarne ([Operatory binarne](classes.md#binary-operators)) i operatory konwersji ([Operatory konwersji](classes.md#conversion-operators)).

*Operator_body* jest średnikiem, ***treścią instrukcji*** lub ***treścią wyrażenia***. Treść instrukcji składa się z *bloku*, który określa instrukcje do wykonania, gdy operator jest wywoływany. *Blok* musi być zgodny z regułami dotyczącymi metod zwracających wartość opisaną w [treści metody](classes.md#method-body). Treść wyrażenia składa się z `=>` po którym następuje wyrażenie i średnik, i oznacza pojedyncze wyrażenie, które ma zostać wykonane po wywołaniu operatora.

Dla operatorów `extern` *operator_body* składa się po prostu z średnika. Dla wszystkich innych operatorów *operator_body* jest treścią bloku lub treścią wyrażenia.

Następujące reguły mają zastosowanie do wszystkich deklaracji operatora:

*  Deklaracja operatora musi zawierać zarówno modyfikator `public`, jak i `static`.
*  Parametry operatora muszą być parametrami wartości ([Parametry wartości](variables.md#value-parameters)). Jest to błąd czasu kompilacji dla deklaracji operatora, aby określić parametry `ref` lub `out`.
*  Sygnatura operatora ([operatory jednoargumentowe](classes.md#unary-operators), [Operatory binarne](classes.md#binary-operators), [Operatory konwersji](classes.md#conversion-operators)) musi różnić się od sygnatur wszystkich innych operatorów zadeklarowanych w tej samej klasie.
*  Wszystkie typy, do których odwołuje się deklaracja operatora, muszą być co najmniej tak samo jak operator ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).
*  Jest to błąd dla tego samego modyfikatora, który może występować wiele razy w deklaracji operatora.

Każda kategoria operatora nakłada dodatkowe ograniczenia, zgodnie z opisem w poniższych sekcjach.

Podobnie jak w przypadku innych składowych, operatory zadeklarowane w klasie bazowej są dziedziczone przez klasy pochodne. Ponieważ deklaracje operatora zawsze wymagają klasy lub struktury, w której operator jest zadeklarowany do uczestniczenia w sygnaturze operatora, nie jest możliwe dla operatora zadeklarowanego w klasie pochodnej w celu ukrycia operatora zadeklarowanego w klasie bazowej. W związku z tym modyfikator `new` nigdy nie jest wymagany i w związku z tym nigdy nie jest dozwolony w deklaracji operatora.

Dodatkowe informacje na temat operatorów jednoargumentowych i binarnych można znaleźć w [operatorach](expressions.md#operators).

Dodatkowe informacje na temat operatorów konwersji można znaleźć w [definicjach zdefiniowanych przez użytkownika](conversions.md#user-defined-conversions).

### <a name="unary-operators"></a>Operatory jednoargumentowe

Poniższe reguły dotyczą deklaracji operatora jednoargumentowego, gdzie `T` wskazuje typ wystąpienia klasy lub struktury, która zawiera deklarację operatora:

*  Operatory jednoargumentowe `+`, `-`, `!`lub `~` muszą przyjmować jeden parametr typu `T` lub `T?` i mogą zwracać dowolny typ.
*  Operatory jednoargumentowe `++` lub `--` muszą przyjmować jeden parametr typu `T` lub `T?` i muszą zwracać ten sam typ lub typ pochodny.
*  Operatory jednoargumentowe `true` lub `false` muszą przyjmować jeden parametr typu `T` lub `T?` i muszą zwracać typ `bool`.

Sygnatura jednoargumentowego operatora składa się z tokenu operatora (`+`, `-`, `!`, `~`, `++`, `--`, `true`lub `false`) i typu pojedynczego parametru formalnego. Zwracany typ nie jest częścią podpisu operatora jednoargumentowego ani nie jest nazwą parametru formalnego.

Operatory `true` i `false` jednoargumentowe wymagają deklaracji pary. Błąd czasu kompilacji występuje, gdy Klasa deklaruje jeden z tych operatorów bez deklarowania innych. Operatory `true` i `false` są opisane bardziej szczegółowo w [warunkowych operatory logiczne](expressions.md#user-defined-conditional-logical-operators) i [wyrażeniach](expressions.md#boolean-expressions)logicznych.

W poniższym przykładzie przedstawiono implementację i kolejne użycie `operator ++` dla klasy wektorów liczb całkowitych:
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

Zwróć uwagę, jak Metoda operatora zwraca wartość wygenerowaną przez dodanie 1 do operandu, podobnie jak Operatory przyrostka i zmniejszanie ([Operatory przyrostka i zmniejszenie](expressions.md#postfix-increment-and-decrement-operators)) oraz operatory przyrostu i zmniejszania prefiksu ([Operatory przyrostka i](expressions.md#prefix-increment-and-decrement-operators)zmniejszenie). W przeciwieństwie C++do programu w przypadku tej metody nie należy modyfikować wartości operandu bezpośrednio. W rzeczywistości modyfikacja wartości operandu spowoduje naruszenie standardowej semantyki operatora przyrostu przyrostkowego.

### <a name="binary-operators"></a>Operatory binarne

Następujące reguły dotyczą deklaracji operatora binarnego, gdzie `T` określa typ wystąpienia klasy lub struktury, która zawiera deklarację operatora:

*  Binarny operator nie przesunięcia musi przyjmować dwa parametry, co najmniej jeden z nich musi mieć typ `T` lub `T?`i może zwracać dowolny typ.
*  Binarny operator `<<` lub `>>` musi przyjmować dwa parametry, z których pierwszy musi mieć typ `T` lub `T?`, a drugi, który musi mieć typ `int` lub `int?`, i może zwracać dowolny typ.

Podpis operatora Binary składa się z tokenu operatora (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`lub `<=`) i typów dwóch parametrów formalnych. Typ zwracany i nazwy parametrów formalnych nie są częścią podpisu operatora binarnego.

Niektóre operatory binarne wymagają deklaracji pary. Dla każdej deklaracji jednego z operatorów para, musi istnieć zgodna deklaracja innego operatora pary. Deklaracje dwóch operatorów są zgodne, gdy mają ten sam typ zwracany i ten sam typ dla każdego parametru. Następujące operatory wymagają deklaracji pary:

*  `operator ==` i `operator !=`
*  `operator >` i `operator <`
*  `operator >=` i `operator <=`

### <a name="conversion-operators"></a>Operatory konwersji

Deklaracja operatora konwersji wprowadza ***konwersję zdefiniowaną przez użytkownika*** ([konwersje zdefiniowane przez użytkownika](conversions.md#user-defined-conversions)), która rozszerza wstępnie zdefiniowane konwersje niejawne i jawne.

Deklaracja operatora konwersji, która zawiera `implicit` słowo kluczowe wprowadza zdefiniowaną przez użytkownika konwersję niejawną. Konwersje niejawne mogą odbywać się w różnych sytuacjach, w tym wywołaniach elementów członkowskich funkcji, wyrażeniach rzutowania i przypisaniach. Jest to bardziej szczegółowo opisane w [konwersji niejawnych](conversions.md#implicit-conversions).

Deklaracja operatora konwersji, która zawiera `explicit` słowo kluczowe wprowadza zdefiniowaną przez użytkownika konwersję jawną. Jawne konwersje mogą wystąpić w wyrażeniach rzutowania i są opisane bardziej szczegółowo w przypadku [konwersji jawnych](conversions.md#explicit-conversions).

Operator konwersji wykonuje konwersję z typu źródłowego, wskazywanego przez typ parametru operatora konwersji do typu docelowego, wskazywanego przez zwracany typ operatora konwersji.

Dla danego typu źródła `S` i typu docelowego `T`, jeśli `S` lub `T` są typami dopuszczających wartości null, pozwól `S0` i `T0` odnoszą się do ich typów podstawowych, w przeciwnym razie `S0` i `T0` są równe `S` i `T` odpowiednio. Klasa lub struktura może deklarować konwersję z typu źródłowego `S` do typu docelowego `T` tylko wtedy, gdy spełnione są wszystkie następujące warunki:

*  `S0` i `T0` są różnymi typami.
*  `S0` lub `T0` jest klasą lub typem struktury, w której odbywa się deklaracja operatora.
*  Ani `S0` ani `T0` jest *interface_typeem*.
*  Oprócz konwersji zdefiniowanych przez użytkownika konwersja nie istnieje z `S` do `T` lub `T` do `S`.

Na potrzeby tych zasad wszystkie parametry typu skojarzone z `S` lub `T` są uznawane za unikatowe typy, które nie mają relacji dziedziczenia z innymi typami, i wszelkie ograniczenia dotyczące tych parametrów typu są ignorowane.

w przykładzie
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
są dozwolone pierwsze dwie deklaracje operatora, ponieważ na [potrzeby .3,](classes.md#indexers)`T` i `int` i `string` są uznawane za unikatowe typy bez relacji. Jednak trzeci operator jest błędem, ponieważ `C<T>` jest klasą bazową `D<T>`.

Z drugiej reguły należy wykonać konwersję operatora konwersji do lub z klasy lub typu struktury, w którym jest zadeklarowany operator. Na przykład, istnieje możliwość, że Klasa lub typ struktury `C` definiować konwersję z `C` do `int` oraz z `int` do `C`, ale nie z `int` `bool`.

Nie można bezpośrednio zdefiniować wstępnie zdefiniowanej konwersji. W związku z tym operatory konwersji nie mogą konwertować z lub do `object`, ponieważ niejawne i jawne konwersje już istnieją między `object` i innymi typami. Podobnie ani źródłowe, ani docelowe typy konwersji nie mogą być typu podstawowego drugiego, ponieważ konwersja już istnieje.

Można jednak zadeklarować operatory na typach ogólnych, które dla konkretnych argumentów typu określają konwersje, które już istniały jako wstępnie zdefiniowane konwersje. w przykładzie
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
gdy typ `object` jest określony jako argument typu dla `T`, drugi operator deklaruje konwersję, która już istnieje (niejawną, a w związku z tym również jawne, konwersja istnieje z dowolnego typu do typu `object`).

W przypadkach, gdy istnieje wstępnie zdefiniowana konwersja między dwoma typami, wszystkie konwersje zdefiniowane przez użytkownika między tymi typami są ignorowane. Opracowany

*  Jeśli wstępnie zdefiniowana niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z typu `S` do typu `T`, wszystkie konwersje zdefiniowane przez użytkownika (niejawne lub jawne) z `S` do `T` są ignorowane.
*  Jeśli wstępnie zdefiniowana konwersja jawna ([jawne konwersje](conversions.md#explicit-conversions)) istnieje z typu `S` do typu `T`, wszystkie jawne konwersje zdefiniowane przez użytkownika z `S` do `T` są ignorowane. Więcej

Jeśli `T` jest typem interfejsu, zdefiniowane przez użytkownika Konwersje niejawne z `S` do `T` są ignorowane.

W przeciwnym razie niejawne konwersje zdefiniowane przez użytkownika z `S` do `T` nadal są brane pod uwagę.

Dla wszystkich typów, ale `object`operatory zadeklarowane przez typ `Convertible<T>` powyżej nie powodują konfliktu ze wstępnie zdefiniowanymi konwersjemi. Na przykład:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

Jednak w przypadku typu `object`wstępnie zdefiniowane konwersje ukrywają konwersje zdefiniowane przez użytkownika we wszystkich przypadkach, ale jeden:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

Konwersje zdefiniowane przez użytkownika nie mogą konwertować z lub na *INTERFACE_TYPE*s. W szczególności to ograniczenie gwarantuje, że podczas konwertowania na *INTERFACE_TYPE*nie występują żadne przekształcenia zdefiniowane przez użytkownika, a konwersja na *INTERFACE_TYPE* powiodła się tylko wtedy, gdy konwertowany obiekt rzeczywiście implementuje określony *INTERFACE_TYPE*.

Sygnatura operatora konwersji składa się z typu źródłowego i typu docelowego. (Należy zauważyć, że jest to jedyna forma elementu członkowskiego, dla którego typ zwracany uczestniczy w podpisie). Klasyfikacja `implicit` lub `explicit` operatora konwersji nie jest częścią podpisu operatora. W ten sposób Klasa lub struktura nie może deklarować zarówno `implicit`, jak i operatora konwersji `explicit` z tymi samymi typami źródłowymi i docelowymi.

Ogólnie rzecz biorąc, zdefiniowane przez użytkownika Konwersje niejawne powinny być zaprojektowane tak, aby nigdy nie generować wyjątków i nigdy nie tracić informacji. Jeśli zdefiniowana przez użytkownika konwersja może prowadzić do wyjątków (na przykład ponieważ argument źródłowy znajduje się poza zakresem) lub utracisz informacje (np. odrzucanie bitów o wysokim stopniu kolejności), konwersja powinna być zdefiniowana jako konwersja jawna.

w przykładzie
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
Konwersja z `Digit` na `byte` jest niejawna, ponieważ nigdy nie zgłasza wyjątków lub utraci informacje, ale konwersja z `byte` do `Digit` jest jawna, ponieważ `Digit` może reprezentować tylko podzestaw możliwych wartości `byte`.

## <a name="instance-constructors"></a>Konstruktory wystąpień

***Konstruktor wystąpienia*** jest członkiem, który implementuje akcje wymagane do zainicjowania wystąpienia klasy. Konstruktory wystąpień są deklarowane przy użyciu *constructor_declaration*s:

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

*Constructor_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)), prawidłową kombinację czterech modyfikatorów dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)) oraz modyfikator `extern` ([metody zewnętrzne](classes.md#external-methods)). Deklaracja konstruktora nie może zawierać tego samego modyfikatora wiele razy.

*Identyfikator* *constructor_declarator* musi mieć nazwę klasy, w której jest zadeklarowany Konstruktor wystąpień. Jeśli określona jest jakakolwiek inna nazwa, wystąpi błąd w czasie kompilacji.

Opcjonalne *formal_parameter_list* konstruktora wystąpienia podlegają tym samym regułom, co *Formal_parameter_list* metody ([metody](classes.md#methods)). Formalna lista parametrów definiuje sygnaturę ([sygnatury i Przeciążenie](basic-concepts.md#signatures-and-overloading)) konstruktora wystąpienia i zarządza procesem, zgodnie z którym Rozpoznanie przeciążenia ([wnioskowanie o typie](expressions.md#type-inference)) wybiera określony Konstruktor wystąpienia w wywołaniu.

Każdy typ, do którego odwołuje się *formal_parameter_list* konstruktora wystąpienia, musi być co najmniej tak samo samo jak Konstruktor ([ograniczenia dostępności](basic-concepts.md#accessibility-constraints)).

Opcjonalny *constructor_initializer* określa inny Konstruktor wystąpienia do wywołania przed wykonaniem instrukcji podanym w *constructor_body* tego konstruktora wystąpień. Jest to bardziej szczegółowo opisane w [inicjatorach konstruktora](classes.md#constructor-initializers).

Gdy deklaracja konstruktora zawiera modyfikator `extern`, Konstruktor jest nazywany ***konstruktorem zewnętrznym***. Ponieważ deklaracja konstruktora zewnętrznego nie oferuje rzeczywistej implementacji, jej *constructor_body* składa się z średnika. Dla wszystkich innych konstruktorów *constructor_body* składa się z *bloku* , który określa instrukcje inicjowania nowego wystąpienia klasy. Odpowiada to dokładnie na *blok* metody wystąpienia z typem zwracanym `void` ([treść metody](classes.md#method-body)).

Konstruktory wystąpień nie są dziedziczone. W ten sposób Klasa nie ma konstruktorów wystąpień innych niż zadeklarowane w klasie. Jeśli Klasa nie zawiera deklaracji konstruktora wystąpienia, zostanie automatycznie udostępniony Konstruktor wystąpienia domyślnego ([konstruktory domyślne](classes.md#default-constructors)).

Konstruktory wystąpień są wywoływane przez *object_creation_expression*s ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)) i *constructor_initializer*s.

### <a name="constructor-initializers"></a>Inicjatory konstruktora

Wszystkie konstruktory wystąpień (z wyjątkiem tych dla klasy `object`) niejawnie zawierają wywołanie innego konstruktora wystąpienia bezpośrednio przed *constructor_body*. Konstruktor niejawnie wywoływany jest określany przez *constructor_initializer*:

*  Inicjator konstruktora wystąpień w formularzu `base(argument_list)` lub `base()` powoduje wywołanie konstruktora wystąpienia z bezpośredniej klasy podstawowej. Ten konstruktor jest wybierany przy użyciu *argument_list* , jeśli jest obecny, i [reguł rozpoznawania przeciążenia.](expressions.md#overload-resolution) Zestaw konstruktorów wystąpień kandydatów składa się ze wszystkich dostępnych konstruktorów wystąpień zawartych w bezpośredniej klasie podstawowej lub konstruktora domyślnego ([konstruktory domyślne](classes.md#default-constructors)), jeśli nie są zadeklarowane konstruktory wystąpień w bezpośredniej klasie podstawowej. Jeśli ten zestaw jest pusty lub nie można zidentyfikować jednego najlepszego konstruktora wystąpienia, wystąpi błąd w czasie kompilacji.
*  Inicjator konstruktora wystąpień w formularzu `this(argument-list)` lub `this()` powoduje wywołanie konstruktora wystąpienia z samej klasy. Konstruktor jest wybierany przy użyciu *argument_list* , jeśli jest obecny, i reguł [rozdzielczości przeciążenia.](expressions.md#overload-resolution) Zestaw konstruktorów wystąpień kandydatów składa się ze wszystkich dostępnych konstruktorów wystąpień zadeklarowanych w samej klasie. Jeśli ten zestaw jest pusty lub nie można zidentyfikować jednego najlepszego konstruktora wystąpienia, wystąpi błąd w czasie kompilacji. Jeśli deklaracja konstruktora wystąpienia zawiera inicjator konstruktora, który wywołuje samego konstruktora, wystąpi błąd czasu kompilacji.

Jeśli Konstruktor wystąpienia nie ma inicjatora konstruktora, niejawnie podany zostanie inicjator konstruktora w formularzu `base()`. W rezultacie deklaracja konstruktora wystąpienia formularza
```csharp
C(...) {...}
```
jest dokładnie równoważne
```csharp
C(...): base() {...}
```

Zakres parametrów przyznanych przez *formal_parameter_list* deklaracji konstruktora wystąpień zawiera inicjator konstruktora tej deklaracji. W ten sposób inicjator konstruktora jest uprawniony do uzyskiwania dostępu do parametrów konstruktora. Na przykład:
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

Inicjator konstruktora wystąpień nie może uzyskać dostępu do tworzonego wystąpienia. W związku z tym jest to błąd czasu kompilacji, aby odwołać się do `this` w wyrażeniu argumentu inicjatora konstruktora, podobnie jak w przypadku błędu czasu kompilacji dla wyrażenia argumentu, aby odwołać się do dowolnego elementu członkowskiego wystąpienia za pomocą *simple_name*.

### <a name="instance-variable-initializers"></a>Inicjatory zmiennych wystąpienia

Gdy Konstruktor wystąpienia nie ma inicjatora konstruktora lub ma inicjatora konstruktora `base(...)`, ten Konstruktor niejawnie wykonuje inicjalizacje określone przez *variable_initializer*s pól wystąpienia zadeklarowanych w swojej klasie. Odnosi się to do sekwencji przypisań, które są wykonywane natychmiast po wpisie do konstruktora i przed niejawnym wywołaniem konstruktora klasy bazowej bezpośredniego. Inicjatory zmiennych są wykonywane w kolejności tekstowej, w której pojawiają się w deklaracji klasy.

### <a name="constructor-execution"></a>Wykonanie konstruktora

Inicjatory zmiennych są przekształcane w instrukcje przypisania i te instrukcje przypisania są wykonywane przed wywołaniem konstruktora wystąpienia klasy podstawowej. Takie porządkowanie gwarantuje, że wszystkie pola wystąpienia są inicjowane przez ich inicjatory zmiennych przed wykonaniem jakichkolwiek instrukcji, które mają dostęp do tego wystąpienia.

Zamieszczono przykład
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
gdy `new B()` jest używany do tworzenia wystąpienia `B`, generowane są następujące dane wyjściowe:
```console
x = 1, y = 0
```

Wartość `x` jest równa 1, ponieważ inicjator zmiennej jest wykonywany przed wywołaniem konstruktora wystąpienia klasy bazowej. Jednak wartość `y` jest równa 0 (wartość domyślna `int`), ponieważ przypisanie do `y` nie jest wykonywane, dopóki nie zostanie zwrócony Konstruktor klasy bazowej.

Warto traktować inicjatory zmiennych wystąpień i inicjatory konstruktora jako instrukcje, które są automatycznie wstawiane przed *constructor_body*. Przykład
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
zawiera kilka inicjatorów zmiennych; zawiera również inicjatory konstruktora obu formularzy (`base` i `this`). Przykład odnosi się do kodu pokazanego poniżej, gdzie każdy komentarz wskazuje automatycznie wstawioną instrukcję (składnia używana dla automatycznie wstawionego konstruktora wywołania jest nieprawidłowa, ale tylko służy do zilustrowania mechanizmu).

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a>Konstruktory domyślne

Jeśli Klasa nie zawiera deklaracji konstruktora wystąpienia, zostanie automatycznie udostępniony Konstruktor wystąpienia domyślnego. Ten konstruktor domyślny po prostu wywołuje konstruktora bez parametrów bezpośredniej klasy podstawowej. Jeśli klasa jest abstrakcyjna, zadeklarowana dostępność dla domyślnego konstruktora jest chroniona. W przeciwnym razie zadeklarowana dostępność dla domyślnego konstruktora jest publiczna. W ten sposób Konstruktor domyślny ma zawsze postać

```csharp
protected C(): base() {}
```
lub
```csharp
public C(): base() {}
```
gdzie `C` jest nazwą klasy. Jeśli rozwiązanie przeciążenia nie jest w stanie ustalić unikatowego najlepszego kandydata dla inicjatora konstruktora klasy bazowej, wystąpi błąd czasu kompilacji.

w przykładzie
```csharp
class Message
{
    object sender;
    string text;
}
```
podano Konstruktor domyślny, ponieważ Klasa nie zawiera deklaracji konstruktora wystąpień. W tym celu przykład jest dokładnie równoważny z
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>Konstruktory prywatne

Gdy Klasa `T` deklaruje tylko konstruktorów wystąpień prywatnych, nie jest możliwe w przypadku klas spoza tekstu programu `T`, które pochodzą z `T` lub bezpośrednio tworzą wystąpienia `T`. W takim przypadku, jeśli Klasa zawiera tylko statyczne elementy członkowskie i nie jest przeznaczona do wystąpienia, dodanie pustego konstruktora wystąpienia prywatnego uniemożliwi utworzenie wystąpienia. Na przykład:
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

Klasy `Trig` są powiązane z metodami i stałymi, ale nie są przeznaczone do tworzenia wystąpień. W związku z tym deklaruje on jeden pusty Konstruktor wystąpienia prywatnego. Należy zadeklarować co najmniej jeden Konstruktor wystąpienia, aby pominąć automatyczne generowanie domyślnego konstruktora.

### <a name="optional-instance-constructor-parameters"></a>Opcjonalne parametry konstruktora wystąpień

`this(...)`a forma inicjatora konstruktora jest często używana w połączeniu z przeciążeniem w celu zaimplementowania opcjonalnych parametrów konstruktora wystąpień. w przykładzie
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
pierwsze dwa konstruktory wystąpień jedynie zawierają wartości domyślne dla brakujących argumentów. Użyj inicjatora konstruktora `this(...)`, aby wywołać trzeci Konstruktor wystąpienia, który faktycznie wykonuje zadania inicjujące nowe wystąpienie. Efektem jest to, że opcjonalne parametry konstruktora:
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>Konstruktory statyczne

***Statyczny Konstruktor*** jest członkiem, który implementuje akcje wymagane do zainicjowania typu zamkniętej klasy. Konstruktory statyczne są deklarowane przy użyciu *static_constructor_declaration*s:

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

*Static_constructor_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)) i modyfikator `extern` ([metody zewnętrzne](classes.md#external-methods)).

*Identyfikator* *static_constructor_declaration* musi należeć do klasy, w której zadeklarowany jest Konstruktor statyczny. Jeśli określona jest jakakolwiek inna nazwa, wystąpi błąd w czasie kompilacji.

Gdy deklaracja konstruktora statycznego zawiera modyfikator `extern`, Konstruktor statyczny jest uznawany za ***zewnętrzny Konstruktor statyczny***. Ponieważ deklaracja zewnętrznego konstruktora statycznego nie oferuje rzeczywistej implementacji, jej *static_constructor_body* składa się z średnika. Dla wszystkich innych deklaracji statycznego konstruktora *static_constructor_body* składa się z *bloku* , który określa instrukcje do wykonania w celu zainicjowania klasy. Odpowiada to dokładnie *method_body* statycznej metody z typem zwracanym `void` ([treść metody](classes.md#method-body)).

Konstruktory statyczne nie są dziedziczone i nie mogą być wywoływane bezpośrednio.

Statyczny Konstruktor dla typu zamkniętej klasy jest wykonywany co najwyżej raz w danej domenie aplikacji. Wykonanie konstruktora statycznego jest wyzwalane przez pierwsze z następujących zdarzeń, które mają być wykonywane w domenie aplikacji:

*  Tworzone jest wystąpienie typu klasy.
*  Wszystkie statyczne elementy członkowskie typu klasy są przywoływane.

Jeśli Klasa zawiera metodę `Main` ([Uruchamianie aplikacji](basic-concepts.md#application-startup)), w której rozpoczyna się wykonywanie, Konstruktor statyczny dla tej klasy jest wykonywany przed wywołaniem metody `Main`.

Aby zainicjować nowy typ klasy zamkniętej, najpierw tworzony jest nowy zestaw pól statycznych ([pola statyczne i wystąpienia](classes.md#static-and-instance-fields)) dla danego typu zamkniętego. Każde z pól statycznych jest inicjowane z wartością domyślną ([wartości domyślne](variables.md#default-values)). Następnie elementy statyczne inicjatory pola ([Inicjalizacja pola statycznego](classes.md#static-field-initialization)) są wykonywane dla tych pól statycznych. Na koniec jest wykonywany Konstruktor statyczny.

Przykład
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
musi generować dane wyjściowe:
```console
Init A
A.F
Init B
B.F
```
ponieważ wykonywanie `A`statycznego konstruktora jest wyzwalane przez wywołanie do `A.F`, a wykonywanie `B`go konstruktora statycznego jest wyzwalane przez wywołanie do `B.F`.

Istnieje możliwość skonstruowania zależności cyklicznych, które zezwalają na przestrzeganie pól statycznych z inicjatorami zmiennych w stanie wartości domyślnych.

Przykład
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
tworzy dane wyjściowe
```console
X = 1, Y = 2
```

Aby wykonać metodę `Main`, system najpierw uruchamia inicjator dla `B.Y`, przed konstruktorem statycznym klasy `B`. Inicjator `Y`powoduje uruchomienie `A`nego konstruktora statycznego, ponieważ istnieje odwołanie do wartości `A.X`. Statyczny Konstruktor `A` z kolei będzie kontynuował Obliczanie wartości `X`i w ten sposób Pobiera wartość domyślną `Y`, która jest równa zero. `A.X` jest w ten sposób inicjowany do 1. Proces uruchamiania inicjatorów pól statycznych `A`i konstruktora statycznego, a następnie zakończenia, zwracając do obliczenia wartości początkowej `Y`, wynik, który zostanie ustawiony na 2.

Ponieważ statyczny Konstruktor jest wykonywany dokładnie jeden raz dla każdego zamkniętego typu klasy konstruowanej, jest to wygodne miejsce do wymuszania sprawdzania w czasie wykonywania na parametrze typu, który nie może być sprawdzany w czasie kompilacji za pośrednictwem ograniczeń ([ograniczenia parametru typu](classes.md#type-parameter-constraints)). Na przykład następujący typ używa konstruktora statycznego, aby wymusić, że argument typu jest wyliczeniem:
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a>Destruktory

***Destruktor*** jest członkiem, który implementuje akcje wymagane do zadestruktorowania wystąpienia klasy. Destruktor jest zadeklarowany przy użyciu *destructor_declaration*:

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

*Destructor_declaration* może zawierać zestaw *atrybutów* ([atrybutów](attributes.md)).

*Identyfikator* *destructor_declaration* musi należeć do klasy, w której jest zadeklarowany destruktor. Jeśli określona jest jakakolwiek inna nazwa, wystąpi błąd w czasie kompilacji.

Gdy deklaracja destruktora zawiera modyfikator `extern`, destruktor jest nazywany ***destruktorem zewnętrznym***. Ponieważ deklaracja zewnętrznego destruktora nie zapewnia rzeczywistej implementacji, jej *destructor_body* składa się z średnika. Dla wszystkich innych destruktorów *destructor_body* składa się z bloku, który określa instrukcje do wykonania w celu *zablokowania* wystąpienia klasy. *Destructor_body* odpowiada dokładnie *method_body* metody wystąpienia z typem zwracanym `void` ([treść metody](classes.md#method-body)).

Destruktory nie są dziedziczone. W ten sposób Klasa nie ma destruktorów innych niż ten, który może być zadeklarowany w tej klasie.

Ponieważ destruktor jest wymagany do braku parametrów, nie może być przeciążony, więc Klasa może mieć co najwyżej jeden destruktor.

Destruktory są wywoływane automatycznie i nie mogą być wywoływane jawnie. Wystąpienie kwalifikuje się do zniszczenia, gdy nie jest już możliwe do użycia w żadnym kodzie. Wykonywanie destruktora dla wystąpienia może wystąpić w dowolnym momencie, gdy wystąpienie kwalifikuje się do zniszczenia. Gdy wystąpienie jest destruktorem, destruktory w łańcuchu dziedziczenia tego wystąpienia są wywoływane, w kolejności, od najbardziej pochodnych do najmniej pochodnych. Destruktor może być wykonywany na dowolnym wątku. Aby uzyskać dalsze Omówienie zasad decydujących o sposobie i sposobach wykonywania destruktora, zobacz [Automatyczne zarządzanie pamięcią](basic-concepts.md#automatic-memory-management).

Dane wyjściowe przykładu
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
is
```
B's destructor
A's destructor
```
ponieważ destruktory w łańcuchu dziedziczenia są wywoływane w kolejności, od najbardziej pochodnych do najmniej pochodnych.

Destruktory są implementowane przez zastąpienie metody wirtualnej `Finalize` w `System.Object`. C#programy nie mogą przesłonić tej metody lub wywołać jej bezpośrednio (lub przesłonięć). Na przykład program
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
zawiera dwa błędy.

Kompilator zachowuje się tak, jakby ta metoda i przesłonięcia go nie istnieją. W tym programie:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
jest prawidłowy, a pokazana Metoda ukrywa metodę `Finalize` `System.Object`.

Aby zapoznać się z omówieniem zachowania, gdy wyjątek jest zgłaszany przez destruktor, zobacz [jak wyjątki są obsługiwane](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Iteratory

Element członkowski funkcji ([składowe funkcji](expressions.md#function-members)) zaimplementowany przy użyciu bloku iteratora ([Blocks](statements.md#blocks)) jest nazywany ***iteratorem***.

Blok iteratora może być używany jako treść składowej funkcji, tak długo, jak typ zwracany odpowiadającego elementu członkowskiego funkcji jest jednym z interfejsów modułu wyliczającego ([moduły wyliczające](classes.md#enumerator-interfaces)) lub jeden z wyliczalnych interfejsów ([wyliczalne interfejsy](classes.md#enumerable-interfaces)). Może wystąpić jako *method_body*, *operator_body* lub *accessor_body*, podczas gdy zdarzenia, konstruktory wystąpień, konstruktory statyczne i destruktory nie mogą być implementowane jako Iteratory.

Gdy element członkowski funkcji jest implementowany przy użyciu bloku iteratora, jest to błąd czasu kompilacji dla formalnej listy parametrów elementu członkowskiego funkcji, aby określić parametry `ref` lub `out`.

### <a name="enumerator-interfaces"></a>Interfejsy modułu wyliczającego

***Interfejsy modułu wyliczającego*** są interfejsami nieogólnymi `System.Collections.IEnumerator` i wszystkimi wystąpieniami `System.Collections.Generic.IEnumerator<T>`interfejsu ogólnego. Ze względu na zwięzłości w tym rozdziale te interfejsy są przywoływane odpowiednio `IEnumerator` i `IEnumerator<T>`.

### <a name="enumerable-interfaces"></a>Wyliczalne interfejsy

***Wyliczalne interfejsy*** są interfejsem nieogólnym `System.Collections.IEnumerable` i wszystkimi wystąpieniami `System.Collections.Generic.IEnumerable<T>`interfejsu ogólnego. Ze względu na zwięzłości w tym rozdziale te interfejsy są przywoływane odpowiednio `IEnumerable` i `IEnumerable<T>`.

### <a name="yield-type"></a>Typ Yield

Iterator tworzy sekwencję wartości, tego samego typu. Ten typ jest nazywany ***typem Yield*** iteratora.

*  Należy `object`typ Yield iteratora zwracającego `IEnumerator` lub `IEnumerable`.
*  Należy `T`typ Yield iteratora zwracającego `IEnumerator<T>` lub `IEnumerable<T>`.

### <a name="enumerator-objects"></a>Obiekty modułu wyliczającego

Gdy element członkowski funkcji zwracający typ interfejsu modułu wyliczającego jest zaimplementowany przy użyciu bloku iteratora, wywołanie elementu członkowskiego funkcji nie powoduje natychmiastowego wykonania kodu w bloku iteratora. Zamiast tego ***obiekt modułu wyliczającego*** jest tworzony i zwracany. Ten obiekt hermetyzuje kod określony w bloku iteratora i wykonywanie kodu w bloku iteratora występuje, gdy wywoływana jest metoda `MoveNext` obiektu modułu wyliczającego. Obiekt modułu wyliczającego ma następującą charakterystykę:

*  Implementuje `IEnumerator` i `IEnumerator<T>`, gdzie `T` jest typem Yield iteratora.
*  Implementuje `System.IDisposable`.
*  Jest on inicjowany z kopią wartości argumentów (jeśli istnieją) i wartością wystąpienia przekazaną do elementu członkowskiego funkcji.
*  Ma cztery potencjalne Stany, ***wcześniej***, ***uruchomione***, ***zawieszone***i ***po***nich, a początkowo jest w stanie ***przed*** .

Obiekt modułu wyliczającego jest zwykle wystąpieniem klasy modułu wyliczającego wygenerowanego przez kompilator, która hermetyzuje kod w bloku iteratora i implementuje interfejsy modułu wyliczającego, ale możliwe są inne metody implementacji. Jeśli klasa modułu wyliczającego jest generowana przez kompilator, ta klasa będzie zagnieżdżona, bezpośrednio lub pośrednio, w klasie zawierającej element członkowski funkcji będzie mieć prywatną dostępność i będzie miała nazwę zarezerwowaną dla użycia kompilatora ([identyfikatory](lexical-structure.md#identifiers)).

Obiekt modułu wyliczającego może zaimplementować więcej interfejsów niż określono powyżej.

W poniższych sekcjach opisano dokładne zachowanie `MoveNext`, `Current`i `Dispose` członków implementacji interfejsu `IEnumerable` i `IEnumerable<T>` dostarczonych przez obiekt modułu wyliczającego.

Należy zauważyć, że obiekty modułu wyliczającego nie obsługują metody `IEnumerator.Reset`. Wywołanie tej metody powoduje zgłoszenie `System.NotSupportedException`.

#### <a name="the-movenext-method"></a>MoveNext — Metoda

Metoda `MoveNext` obiektu modułu wyliczającego hermetyzuje kod bloku iteratora. Wywoływanie metody `MoveNext` wykonuje kod w bloku iteratora i ustawia właściwość `Current` obiektu Enumerator odpowiednio do potrzeb. Precyzyjne działanie wykonywane przez `MoveNext` zależy od stanu obiektu modułu wyliczającego, gdy `MoveNext` jest wywoływana:

*  Jeśli stan obiektu modułu wyliczającego jest ***wcześniejszy***, wywoływanie `MoveNext`:
   * Zmienia stan na ***uruchomiony***.
   * Inicjuje parametry (w tym `this`) bloku iteratora do wartości argumentów i wartości wystąpienia zapisanego podczas inicjowania obiektu modułu wyliczającego.
   * Wykonuje blok iterator od początku do momentu przerwania wykonywania (zgodnie z poniższym opisem).
*  Jeśli stan obiektu modułu wyliczającego jest ***uruchomiony***, wynik wywoływania `MoveNext` jest nieokreślony.
*  Jeśli stan obiektu modułu wyliczającego jest ***zawieszony***, wywoływanie `MoveNext`:
   * Zmienia stan na ***uruchomiony***.
   * Przywraca wartości wszystkich zmiennych lokalnych i parametrów (włącznie z tym) do wartości zapisanych podczas ostatniego wstrzymania wykonywania bloku iteratora. Należy zauważyć, że zawartość wszelkich obiektów, do których odwołuje się te zmienne, mogła ulec zmianie od czasu poprzedniego wywołania elementu MoveNext.
   * Wznawia wykonywanie bloku iteratora bezpośrednio po instrukcji `yield return`, która spowodowała zawieszenie wykonywania i jest kontynuowane do momentu przerwania wykonywania (zgodnie z poniższym opisem).
*  Jeśli stan obiektu modułu wyliczającego jest ***po***, wywołanie `MoveNext` zwraca `false`.


Gdy `MoveNext` wykonuje blok iterator, wykonywanie może zostać przerwane na cztery sposoby: przez instrukcję `yield return`, przez instrukcję `yield break`, przez napotkanie końca bloku iteratora i wyjątek zgłaszany i propagowany z bloku iteratora.

*  Po napotkaniu instrukcji `yield return` ([instrukcja Yield](statements.md#the-yield-statement)):
   * Wyrażenie określone w instrukcji jest oceniane, niejawnie konwertowane na typ Yield i przypisane do właściwości `Current` obiektu modułu wyliczającego.
   * Wykonywanie treści iteratora zostało wstrzymane. Wartości wszystkich lokalnych zmiennych i parametrów (w tym `this`) są zapisywane, podobnie jak lokalizacja tej instrukcji `yield return`. Jeśli instrukcja `yield return` znajduje się w obrębie jednego lub kilku bloków `try`, skojarzone bloki `finally` nie są wykonywane w tym momencie.
   * Stan obiektu modułu wyliczającego został zmieniony na ***wstrzymany***.
   * Metoda `MoveNext` zwraca `true` do obiektu wywołującego, co wskazuje, że iteracja została pomyślnie zaawansowana do następnej wartości.
*  Po napotkaniu instrukcji `yield break` ([instrukcja Yield](statements.md#the-yield-statement)):
   * Jeśli instrukcja `yield break` znajduje się w obrębie jednego lub kilku bloków `try`, skojarzone bloki `finally` są wykonywane.
   * Stan obiektu modułu wyliczającego jest zmieniany na ***po***.
   * Metoda `MoveNext` zwraca `false` do obiektu wywołującego, wskazując, że iteracja została zakończona.
*  Gdy zostanie osiągnięty koniec treści iteratora:
   * Stan obiektu modułu wyliczającego jest zmieniany na ***po***.
   * Metoda `MoveNext` zwraca `false` do obiektu wywołującego, wskazując, że iteracja została zakończona.
*  Gdy wyjątek jest zgłaszany i propagowany z bloku iteratora:
   * Odpowiednie bloki `finally` w treści iteratora zostaną wykonane przez propagację wyjątku.
   * Stan obiektu modułu wyliczającego jest zmieniany na ***po***.
   * Propagacja wyjątku kontynuuje proces wywołujący metody `MoveNext`.

#### <a name="the-current-property"></a>Bieżąca Właściwość

Na właściwości `Current` obiektu modułu wyliczającego wpływają instrukcje `yield return` w bloku iteratora.

Gdy obiekt modułu wyliczającego jest w stanie ***wstrzymania*** , wartość `Current` jest wartością ustawioną przez poprzednie wywołanie do `MoveNext`. Gdy obiekt modułu wyliczającego znajduje się w stanie ***sprzed***, ***uruchomiona***lub ***after*** , wynik uzyskania dostępu `Current` nie zostanie określony.

Dla iteratora z typem Yield innym niż `object`, wynik uzyskania dostępu `Current` przez implementację `IEnumerable` obiektu modułu wyliczającego odnosi się do dostępu `Current` przez implementację `IEnumerator<T>` obiektu modułu wyliczającego i rzutowanie wyniku na `object`.

#### <a name="the-dispose-method"></a>Metoda Dispose

Metoda `Dispose` służy do czyszczenia iteracji przez przełączenie obiektu Enumerator do stanu ***po*** .

*  Jeśli stan obiektu modułu wyliczającego jest ***wcześniejszy***, wywoływanie `Dispose` zmienia stan na ***po***.
*  Jeśli stan obiektu modułu wyliczającego jest ***uruchomiony***, wynik wywoływania `Dispose` jest nieokreślony.
*  Jeśli stan obiektu modułu wyliczającego jest ***zawieszony***, wywoływanie `Dispose`:
   * Zmienia stan na ***uruchomiony***.
   * Wykonuje wszystkie bloki finally tak, jakby Ostatnia wykonana instrukcja `yield return` była instrukcją `yield break`. Jeśli spowoduje to wyjątek, który ma zostać wygenerowany i rozpropagowany z treści iteratora, stan obiektu modułu wyliczającego jest ustawiony na ***po*** , a wyjątek jest propagowany do obiektu wywołującego metody `Dispose`.
   * Zmienia stan na ***po***.
*  Jeśli stan obiektu modułu wyliczającego jest ***po***, wywołanie `Dispose` nie ma wpływu.

### <a name="enumerable-objects"></a>Wyliczalne obiekty

Gdy element członkowski funkcji zwracający wyliczalny typ interfejsu jest implementowany przy użyciu bloku iteratora, wywołanie elementu członkowskiego funkcji nie powoduje natychmiastowego wykonania kodu w bloku iteratora. Zamiast tego zostanie utworzony i zwrócony ***wyliczalny obiekt*** . Metoda `GetEnumerator` obiektu wyliczeniowego zwraca obiekt modułu wyliczającego, który hermetyzuje kod określony w bloku iterator i wykonywanie kodu w bloku iteratora występuje, gdy wywoływana jest metoda `MoveNext` obiektu modułu wyliczającego. Wyliczalny obiekt ma następującą charakterystykę:

*  Implementuje `IEnumerable` i `IEnumerable<T>`, gdzie `T` jest typem Yield iteratora.
*  Jest on inicjowany z kopią wartości argumentów (jeśli istnieją) i wartością wystąpienia przekazaną do elementu członkowskiego funkcji.

Wyliczalny obiekt jest zwykle wystąpieniem klasy wyliczalnej generowanej przez kompilator, która hermetyzuje kod w bloku iteratora i implementuje wyliczalne interfejsy, ale inne metody implementacji są możliwe. Jeśli wyliczalna Klasa jest generowana przez kompilator, ta klasa będzie zagnieżdżona, bezpośrednio lub pośrednio, w klasie zawierającej element członkowski funkcji będzie mieć prywatną dostępność i będzie miała nazwę zarezerwowaną dla użycia kompilatora ([identyfikatory](lexical-structure.md#identifiers)).

Wyliczalny obiekt może zaimplementować więcej interfejsów niż określono powyżej. W szczególności, wyliczalny obiekt może również zaimplementować `IEnumerator` i `IEnumerator<T>`, co umożliwia jego pełnienie jako wyliczalny i moduł wyliczający. W tym typie implementacji przy pierwszym wywołaniu metody `GetEnumerator` obiektu wyliczalnego jest zwracany wyliczalny obiekt. Kolejne wywołania `GetEnumerator`wyliczalnego obiektu, jeśli takie istnieją, zwracają kopię wyliczalnego obiektu. W ten sposób każdy zwrócony moduł wyliczający ma swój własny stan, a zmiany w jednym wyliczającym nie wpłyną na inne.

#### <a name="the-getenumerator-method"></a>GetEnumerator — metoda

Obiekt przeliczalny zapewnia implementację metod `GetEnumerator` interfejsów `IEnumerable` i `IEnumerable<T>`. Dwie `GetEnumerator` metody współdzielą wspólną implementację, która uzyskuje i zwraca dostępny obiekt modułu wyliczającego. Obiekt modułu wyliczającego jest inicjowany przy użyciu wartości argumentów i wartości wystąpienia zapisywanych, gdy wyliczalny obiekt został zainicjowany, ale w przeciwnym razie obiekt Enumerator działa zgodnie z opisem w obiekcie [Enumerator](classes.md#enumerator-objects).

### <a name="implementation-example"></a>Przykład implementacji

W tej sekcji opisano możliwe implementacje iteratorów w postaci standardowych C# konstrukcji. Implementacja opisana tutaj jest oparta na tych samych zasadach, które są używane przez C# kompilator firmy Microsoft, ale nie oznacza to, że jest to dozwolone wdrożenie ani tylko jeden z nich.

Następująca Klasa `Stack<T>` implementuje metodę `GetEnumerator` przy użyciu iteratora. Iterator wylicza elementy stosu w kolejności od góry do dołu.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

Metodę `GetEnumerator` można przetłumaczyć na wystąpienie klasy modułu wyliczającego wygenerowanego przez kompilator, która hermetyzuje kod w bloku iteratora, jak pokazano w poniższym przykładzie.

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

W poprzednim tłumaczeniu kod w bloku iteratora jest przekształcany w komputer stanu i umieszczany w metodzie `MoveNext` klasy Enumerator. Ponadto zmienna lokalna `i` jest przełączana do pola w obiekcie modułu wyliczającego, dzięki czemu może nadal istnieć między wywołaniami `MoveNext`.

Poniższy przykład drukuje prostą tabelę mnożenia liczb całkowitych od 1 do 10. Metoda `FromTo` w przykładzie zwraca wyliczalny obiekt i jest implementowana przy użyciu iteratora.

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

Metodę `FromTo` można przetłumaczyć na wystąpienie klasy wyliczalnej kompilatora, która hermetyzuje kod w bloku iteratora, jak pokazano w poniższym przykładzie.

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Klasa wyliczalna implementuje zarówno wyliczalne interfejsy, jak i interfejsy modułu wyliczającego, umożliwiając obsługę zarówno jako wyliczalnego, jak i modułu wyliczającego. Przy pierwszym wywołaniu metody `GetEnumerator` zostanie zwrócony wyliczalny obiekt. Kolejne wywołania `GetEnumerator`wyliczalnego obiektu, jeśli takie istnieją, zwracają kopię wyliczalnego obiektu. W ten sposób każdy zwrócony moduł wyliczający ma swój własny stan, a zmiany w jednym wyliczającym nie wpłyną na inne. Metoda `Interlocked.CompareExchange` służy do zapewnienia bezpiecznego wątkowo operacji.

Parametry `from` i `to` są przełączane do pól w klasie wyliczalnej. Ponieważ `from` jest modyfikowany w bloku iteratora, jest wprowadzane dodatkowe pole `__from` do przechowywania wartości początkowej `from` w każdym wyliczeniu.

Metoda `MoveNext` zgłasza `InvalidOperationException`, jeśli jest wywoływana, gdy `__state` jest `0`. Chroni to przed użyciem wyliczalnego obiektu jako obiektu modułu wyliczającego bez uprzedniego wywołania `GetEnumerator`.

Poniższy przykład pokazuje prostą klasę drzewa. Klasa `Tree<T>` implementuje metodę `GetEnumerator` przy użyciu iteratora. Iterator wylicza elementy drzewa w kolejności wrostkowe.

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

Metodę `GetEnumerator` można przetłumaczyć na wystąpienie klasy modułu wyliczającego wygenerowanego przez kompilator, która hermetyzuje kod w bloku iteratora, jak pokazano w poniższym przykładzie.

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Kompilator wygenerował obiekty tymczasowe używany w instrukcjach `foreach`ch zostaje zniesiony do pól `__left` i `__right` obiektu modułu wyliczającego. Wartość pola `__state` obiektu wyliczającego jest dokładnie aktualizowana, tak aby poprawna `Dispose()` Metoda zostanie wywołana prawidłowo, jeśli wystąpi wyjątek. Należy zauważyć, że nie jest możliwe zapisanie przetłumaczonego kodu przy użyciu prostych instrukcji `foreach`.

## <a name="async-functions"></a>Funkcje asynchroniczne

Metoda ([metody](classes.md#methods)) lub funkcja anonimowa ([wyrażenia funkcji anonimowej](expressions.md#anonymous-function-expressions)) z modyfikatorem `async` jest nazywana ***funkcją asynchroniczną***. Ogólnie rzecz biorąc termin ***Async*** jest używany do opisywania dowolnego rodzaju funkcji, która ma modyfikator `async`.

Jest to błąd czasu kompilacji dla formalnej listy parametrów funkcji asynchronicznej, aby określić parametry `ref` lub `out`.

*Return_type* metody asynchronicznej musi być albo `void` lub ***typem zadania***. Typy zadań są `System.Threading.Tasks.Task` i typy skonstruowane z `System.Threading.Tasks.Task<T>`. Ze względu na zwięzłości w tym rozdziale te typy są przywoływane odpowiednio `Task` i `Task<T>`. Metoda asynchroniczna zwracająca typ zadania jest określana jako zadanie zwracające zadania.

Dokładna definicja typów zadań jest definiowana przez implementację, ale z punktu widzenia języka, typ zadania jest w jednym z Stanów nieukończonych, zakończonych powodzeniem lub błędem. Zadanie z błędami rejestruje odpowiedni wyjątek. Pomyślnie `Task<T>` rejestruje wynik typu `T`. Typy zadań są oczekiwane i dlatego mogą być operandami wyrażeń Await ([Expressions](expressions.md#await-expressions)).

Wywołanie funkcji asynchronicznej ma możliwość zawieszenia oceny przy użyciu wyrażeń Await ([wyrażenie await](expressions.md#await-expressions)) w swojej treści. Ocenę można później wznowić w punkcie wstrzymania wyrażenia await przy użyciu ***delegata wznawiania***. Delegat wznawiania jest typu `System.Action`i gdy jest wywoływany, oszacowanie wywołania funkcji asynchronicznej zostanie wznowione z wyrażenia await, gdzie pozostawione. Bieżącym obiektem ***wywołującym*** wywołania funkcji asynchronicznej jest oryginalny obiekt wywołujący, jeśli wywołanie funkcji nigdy nie zostało zawieszone lub ostatni obiekt wywołujący delegata wznawiania w przeciwnym razie.

### <a name="evaluation-of-a-task-returning-async-function"></a>Obliczanie funkcji asynchronicznej zwracającej zadania

Wywołanie funkcji asynchronicznej zwracającej zadanie powoduje wygenerowanie wystąpienia zwracanego typu zadania. Jest to nazywane ***zadaniem zwrotnym*** funkcji asynchronicznej. Zadanie jest początkowo w stanie niepełnym.

Treść funkcji asynchronicznej jest następnie Szacowana do momentu, aż zostanie zawieszona (przez osiągnięcie wyrażenia await) lub przerwania, w którym formant punktu jest zwracany do obiektu wywołującego, wraz z zadaniem zwrotnym.

Gdy treść funkcji asynchronicznej zostanie zakończona, zadanie powrotu zostanie przeniesione z niekompletnego stanu:

*  Jeśli treść funkcji kończy się jako wynik osiągnięcia instrukcji return lub końca treści, każda wartość wyniku zostanie zarejestrowana w zadaniu zwrotnym, która jest przesunięta w stan powodzenie.
*  Jeśli treść funkcji kończy się jako wynik nieprzechwyconego wyjątku ([Instrukcja throw](statements.md#the-throw-statement)), wyjątek jest rejestrowany w zadaniu zwrotnym, które jest umieszczane w stanie awarii.

### <a name="evaluation-of-a-void-returning-async-function"></a>Obliczanie funkcji asynchronicznej zwracającej wartość pustą

Jeśli zwracany typ funkcji asynchronicznej jest `void`, Ocena różni się od powyższych w następujący sposób: ponieważ żadne zadanie nie jest zwracane, funkcja zamiast tego komunikuje zakończenie i wyjątki z ***kontekstem synchronizacji***bieżącego wątku. Dokładna definicja kontekstu synchronizacji jest zależna od implementacji, ale jest reprezentacją "gdzie" bieżącego wątku jest uruchomiony. Kontekst synchronizacji jest powiadamiany, gdy zostanie rozpoczęta Ocena funkcji asynchronicznej zwracającej wartość void, została zakończona pomyślnie, lub spowoduje zgłoszenie nieprzechwyconego wyjątku.

Pozwala to na śledzenie liczby uruchomionych w nim funkcji asynchronicznych zwracających wartość pustą oraz decydowanie o sposobie propagowania wyjątków.
