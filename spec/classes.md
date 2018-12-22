# <a name="classes"></a>Klasy

Klasa jest strukturą danych, który może zawierać elementy członkowskie (stałe i pola), funkcji elementów członkowskich danych (metody, właściwości, zdarzenia, indeksatory, operatory, konstruktory wystąpień, destruktory i konstruktorów statycznych) i zagnieżdżone typy. Typy klas obsługuje dziedziczenie, mechanizm, według której rozszerzać i specialize klasę bazową klasę pochodną.

## <a name="class-declarations"></a>Deklaracje klas

A *class_declaration* jest *type_declaration* ([wpisz deklaracje](namespaces.md#type-declarations)) który deklaruje nową klasę.

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

A *class_declaration* zawiera opcjonalny zestaw *atrybuty* ([atrybuty](attributes.md)), a następnie opcjonalny zestaw *class_modifier*s ([klasy Modyfikatory](classes.md#class-modifiers)), a następnie opcjonalnie `partial` modyfikator, następuje słowa kluczowego `class` i *identyfikator* nazw, klasy, a następnie Opcjonalnie *type_parameter_list* ([parametry typu](classes.md#type-parameters)), a następnie opcjonalnie *class_base* specyfikacji ([klasy podstawowej Specyfikacja](classes.md#class-base-specification)), a następnie opcjonalny zestaw *type_parameter_constraints_clause*s ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), a następnie *class_body*  ([Klasy treści](classes.md#class-body)), opcjonalnie następuje średnik.

Deklaracja klasy nie może dostarczyć *type_parameter_constraints_clause*s, chyba że zawiera także *type_parameter_list*.

Deklaracja klasy, która dostarcza *type_parameter_list* jest ***deklaracji klasy ogólnej***. Ponadto każda klasa zagnieżdżona w deklaracji klasy ogólnej lub deklaracja struktury ogólnej jest deklaracji klasy ogólnej, ponieważ parametrów typu dla typu zawierającego należy podać w celu utworzenia skonstruowanego typu.

### <a name="class-modifiers"></a>Modyfikatorów klasy pamięci

A *class_declaration* może opcjonalnie obejmować sekwencję modyfikatorów klasy pamięci:

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

Jest to błąd czasu kompilacji dla tego samego modyfikator pojawią się wiele razy w deklaracji klasy.

`new` Modyfikator jest dozwolona dla klas zagnieżdżonych. Określa, że klasa ukrywa dziedziczonej składowej o tej samej nazwie, zgodnie z opisem w [nowy modyfikator](classes.md#the-new-modifier). Jest to błąd czasu kompilacji dla `new` modyfikatora występować w deklaracji klasy, który nie jest deklaracją klasy zagnieżdżonej.

`public`, `protected`, `internal`, I `private` Modyfikatory kontrolować ułatwień dostępu klasy. W zależności od kontekstu, w którym występuje deklaracja klasy, niektóre z tych modyfikatorów może nie być dozwolone ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)).

`abstract`, `sealed` i `static` Modyfikatory zostały omówione w poniższych sekcjach.

#### <a name="abstract-classes"></a>Klasy abstrakcyjne

`abstract` Modyfikator jest używany do wskazania, że klasa jest niekompletne i czy mają być używane tylko jako klasa bazowa. Klasa abstrakcyjna różni się od klasy nieabstrakcyjnej w następujący sposób:

*  Klasa abstrakcyjna nie można bezpośrednio utworzyć wystąpienia i jest to błąd kompilacji, aby użyć `new` operator dla klasy abstrakcyjnej. Mimo że możliwe jest zapewnienie zmiennych i wartości, których typy kompilacji są abstrakcyjne, takich zmiennych i wartości niekoniecznie będzie `null` lub zawierają odwołania do wystąpienia klas pochodzących od typy abstrakcyjne klasy nieabstrakcyjnej.
*  Klasy abstrakcyjnej jest dozwolone (ale nie wymagane) zawierać abstrakcyjne składowe.
*  Klasa abstrakcyjna nie może być zapieczętowany.

Gdy klasy nieabstrakcyjnej pochodzi z klasy abstrakcyjnej, nieabstrakcyjnej klasie musi zawierać rzeczywistej implementacji wszystkich dziedziczonych członków abstrakcyjnych, zastępując tych członków abstrakcyjnych. W przykładzie
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
Klasa abstrakcyjna `A` wprowadza metodę abstrakcyjną `F`. Klasy `B` wprowadzono kolejną metodę `G`, ale ponieważ nie zapewnia to implementacja `F`, `B` musi również być zadeklarowany jako abstrakcyjny. Klasa `C` zastępuje `F` i zawiera rzeczywiste wdrożenie. Ponieważ nie abstrakcyjne składowe w `C`, `C` jest dozwolone (ale nie muszą) być nieabstrakcyjnej.

#### <a name="sealed-classes"></a>Zapieczętowane klasy

`sealed` Modyfikator jest używany do uniemożliwiają wyprowadzanie z klasy. Błąd kompilacji występuje, jeśli określono klasy zapieczętowanej jako klasa bazowa innej klasy.

Klasa zapieczętowana nie może być klasą abstrakcyjną.

`sealed` Modyfikator jest używany głównie do uniemożliwiają wyprowadzanie niezamierzone, ale umożliwia również pewne optymalizacje w czasie wykonywania. W szczególności ponieważ wiadomo, że klasa zapieczętowana nigdy nie ma żadnych klas pochodnych, istnieje możliwość przekształcania funkcja wirtualna elementu członkowskiego wywołań w wystąpieniach klasy zapieczętowanej niewirtualną wywołania.

#### <a name="static-classes"></a>Klasy statyczne

`static` Modyfikator jest używany do oznaczania klasy został zadeklarowany jako ***klasy statycznej***. Klasa statyczna, nie można utworzyć wystąpienia nie może być używany jako typ i może zawierać tylko statyczne elementy członkowskie. Tylko statyczne klasy może zawierać deklaracje metody rozszerzenia ([metody rozszerzenia](classes.md#extension-methods)).

Deklaracja klasy statycznej podlega następującym ograniczeniom:

*  Klasa statyczna nie może zawierać `sealed` lub `abstract` modyfikator. Należy jednak pamiętać, że ponieważ klasy statycznej nie można utworzyć wystąpienia lub pochodną, działa tak, jakby był zapieczętowany i abstrakcyjny.
*  Klasa statyczna nie może zawierać *class_base* specyfikacji ([klasy podstawowej specyfikacji](classes.md#class-base-specification)) i nie można jawnie określić klasę bazową lub listy implementowanych interfejsów. Klasa statyczna niejawnie dziedziczy z typu `object`.
*  Klasa statyczna może zawierać tylko statyczne elementy członkowskie ([statyczna i wystąpienia elementów członkowskich](classes.md#static-and-instance-members)). Należy pamiętać, że stałe i zagnieżdżone typy są klasyfikowane jako statyczne elementy członkowskie.
*  Klasa statyczna nie może mieć elementów członkowskich z `protected` lub `protected internal` zadeklarowana ułatwień dostępu.

Jest to błąd czasu kompilacji narusza dowolne z tych ograniczeń.

Klasa statyczna nie ma konstruktorów wystąpienia. Nie jest możliwe do deklarowania konstruktora wystąpienia w klasie statycznej i Brak domyślnego konstruktora wystąpienia ([domyślne konstruktory](classes.md#default-constructors)) znajduje się na klasie statycznej.

Elementy członkowskie klasy statyczne nie są automatycznie statyczne i deklaracji elementu członkowskiego musi jawnie dołączyć znak `static` modyfikator (z wyjątkiem i zagnieżdżone typy stałe). Gdy klasa jest zagnieżdżony w klasie statycznej zewnętrzne, zagnieżdżona klasa nie jest Klasa statyczna, chyba że jawnie zawierającym `static` modyfikator.

__Odwoływanie się do typów klas statycznych__

A *namespace_or_type_name* ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)) jest dozwolona w celu odwołania klasy statycznej, jeśli

*  *Namespace_or_type_name* jest `T` w *namespace_or_type_name* formularza `T.I`, lub
*  *Namespace_or_type_name* jest `T` w *typeof_expression* ([listy argumentów](expressions.md#argument-lists)1) w postaci `typeof(T)`.

A *primary_expression* ([funkcji elementów członkowskich](expressions.md#function-members)) jest dozwolona w celu odwołania klasy statycznej, jeśli

*  *Primary_expression* jest `E` w *member_access* ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) w postaci `E.I`.

Błąd kompilacji, aby odwoływać się do klasy statycznej jest w innym kontekście. Na przykład, występuje błąd dla klasy statycznej do użycia jako klasę bazową składowych typu ([zagnieżdżone typy](classes.md#nested-types)) członkiem, argument typu ogólnego lub ograniczenia parametru typu. Podobnie, w typu tablicowego, typu wskaźnika, w którym nie można użyć klasy statycznej `new` wyrażenia, wyrażenia rzutowania `is` wyrażenie `as` wyrażenie `sizeof` wyrażenie lub wyrażenie wartości domyślnej.

### <a name="partial-modifier"></a>Modyfikatora "Partial"

`partial` Modyfikator jest używany w celu wskazania, że *class_declaration* jest deklaracją typu częściowego. Łączenie wielu deklaracjach częściowej typu o takiej samej nazwie w deklaracji otaczającego przestrzeń nazw lub typ do postaci jednego typu deklaracji, zgodnie z regułami określone w [typów częściowych](classes.md#partial-types).

Po deklaracji klasy rozłożone w odrębnych segmentach tekstu programu może być przydatne, jeśli te segmenty nie są tworzone lub przechowywane w różnych kontekstach. Na przykład jedną część deklaracji klasy może być maszyny generowania, natomiast druga ręcznie utworzone. Rozdzielenie tekstową dwa uniemożliwia aktualizacje za pomocą jednej z powodujące konflikt z elementem aktualizacji przez inne.

### <a name="type-parameters"></a>Parametry typu

Parametr typu jest prostym identyfikatorem, który oznacza symbol zastępczy dla argumentu typu dostarczony w celu utworzenia skonstruowanego typu. Parametr typu jest posiadanie symbol zastępczy dla typu, które zostaną udostępnione później. Przez kontrast, argument typu ([argumentów typu](types.md#type-arguments)) to rzeczywisty typ, który zostanie zastąpiony dla parametru typu, po utworzeniu skonstruowanego typu.

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

Każdy parametr typu w deklaracji klasy definiuje nazwę w obszarze deklaracji ([deklaracje](basic-concepts.md#declarations)) dla tej klasy. W związku z tym nie może on mieć taką samą nazwę jak inny parametr typu lub składowa zadeklarowana w tej klasie. Parametr typu nie może mieć taką samą nazwę jak samego typu.

### <a name="class-base-specification"></a>Klasy podstawowej specyfikacji

Deklaracja klasy może zawierać *class_base* specyfikacja definiuje bezpośrednia klasa podstawowa klasy i interfejsy ([interfejsów](interfaces.md)) bezpośrednio zaimplementowany przez klasę.

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

Klasa bazowa, określonym w deklaracji klasy może być typem klasy skonstruowany ([skonstruowany typy](types.md#constructed-types)). Klasy bazowej nie może być parametrem typu samodzielnie, chociaż może pociągać za sobą parametry typu, które znajdują się w zakresie.

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>Klas podstawowych

Gdy *class_type* znajduje się w *class_base*, określa on bezpośredniej klasie bazowej deklarowanej klasy. Jeśli nie ma w deklaracji klasy *class_base*, lub jeśli *class_base* listy tylko typy interfejsów, bezpośrednie klasy bazowej zakłada się, że `object`. Klasa dziedziczy członków jego bezpośrednia klasa bazowa, zgodnie z opisem w [dziedziczenia](classes.md#inheritance).

W przykładzie
```csharp
class A {}

class B: A {}
```
Klasa `A` bezpośrednie klasy bazowej jest nazywany `B`, i `B` jest nazywany mogą pochodzić z `A`. Ponieważ `A` jest nie zostały jawnie określić bezpośrednią klasą bazową, jego bezpośrednia klasa bazowa jest niejawnie `object`.

Dla typu klasy skonstruowany, jeśli klasa podstawowa jest określona w deklaracji klasy ogólnej klasy bazowej skonstruowanego typu uzyskuje się przez podstawianie dla każdego *type_parameter* w deklaracji klasy bazowej, odpowiedni *type_argument* skonstruowanego typu. Biorąc pod uwagę deklaracji klasy ogólnej
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
Klasa bazowa skonstruowanego typu `G<int>` będzie `B<string,int[]>`.

Bezpośrednie klasy bazowej typu klasy musi być co najmniej tak samo dostępna jak jej typ ([domeny dostępności](basic-concepts.md#accessibility-domains)). Na przykład, jest to błąd czasu kompilacji dla `public` pochodzi od klasy `private` lub `internal` klasy.

Bezpośrednie klasy bazowej typu klasy nie może być dowolny z następujących typów: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, lub `System.ValueType`. Ponadto nie można użyć deklaracji klasy ogólnej `System.Attribute` jako bezpośredniej lub pośredniej klasy bazowej.

Podczas określania znaczenie specyfikacji bezpośrednią klasą bazową `A` klasy `B`, bezpośrednie klasy bazowej `B` tymczasowo zakłada się, że `object`. Intuicyjnie daje to gwarancję, że znaczenie Specyfikacja klasy bazowej nie można rekursywnie zależeć od siebie samej. Przykład:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
Wystąpił błąd od w specyfikacji klasy bazowej `A<C.B>` bezpośrednia klasa podstawowa `C` jest uważany za `object`i dlatego (przez reguły [nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)) `C` nie jest uznawany za umieścić elementu członkowskiego `B`.

Klasy bazowe typu klasy są bezpośredniej klasy podstawowej i jej klasy bazowe. Innymi słowy zestaw klas bazowych jest przechodnia zamknięcia relacja bezpośrednia klasa bazowa. Odwołujące się do przykładu powyżej klasy bazowe `B` są `A` i `object`. W przykładzie
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
klasy bazowe `D<int>` są `C<int[]>`, `B<IComparable<int[]>>`, `A`, i `object`.

Z wyjątkiem klasy `object`, co typ klasy ma dokładnie jednego bezpośrednią klasą bazową. `object` Klasa nie ma bezpośredniej klasy bazowej i jest ultimate klasa bazowa innych klas.

Gdy klasa `B` pochodzi z klasy `A`, jest to błąd czasu kompilacji dla `A` zależą `B`. Klasa ***zależy od bezpośrednio*** jego bezpośrednia klasa bazowa (jeśli istnieje) i ***zależy od bezpośrednio*** klasy, w którym od razu zagnieżdżenia (jeśli istnieje). Biorąc pod uwagę tę definicję, kompletny zestaw klas, od których zależy od klasy jest zamknięcie zwrotnej i przechodni ***zależy od bezpośrednio*** relacji.

Przykład
```csharp
class A: A {}
```
jest błędne, ponieważ zależy od samej klasy. Podobnie w tym przykładzie
```csharp
class A: B {}
class B: C {}
class C: A {}
```
jest w wyniku błędu, ponieważ klasy rekurencyjnie zależą od siebie. Na koniec przykład
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
powoduje błąd w czasie kompilacji, ponieważ `A` zależy od `B.C` (jego bezpośrednia klasa bazowa), która jest zależna od `B` (jego natychmiastowo otaczającą klasę), która rekurencyjnie jest zależna od `A`.

Należy pamiętać, że klasa nie zależy od klas, które są w nim zagnieżdżony. W przykładzie
```csharp
class A
{
    class B: A {}
}
```
`B` zależy od `A` (ponieważ `A` jest jego bezpośrednia klasa bazowa i jego natychmiastowo otaczającą klasę), ale `A` nie zależy od `B` (ponieważ `B` nie jest klasą bazową ani otaczającej klasy `A` ). W efekcie przykładu jest prawidłowy.

Nie jest możliwe dziedziczyć `sealed` klasy. W przykładzie
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
Klasa `B` jest błąd, ponieważ próbuje on pochodzić od `sealed` klasy `A`.

#### <a name="interface-implementations"></a>Implementacje interfejsu

A *class_base* specyfikacji może zawierać listę typów interfejsów, w których przypadku klasy jest nazywany bezpośrednio zaimplementować typy danego interfejsu. Implementacje interfejsu są omówione w dalszych [implementacje interfejsu](interfaces.md#interface-implementations).

### <a name="type-parameter-constraints"></a>Ograniczenia parametru typu

Ogólny deklaracje typu i metody można opcjonalnie określić ograniczenia parametru typu, umieszczając *type_parameter_constraints_clause*s.

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

Każdy *type_parameter_constraints_clause* składa się z tokenem `where`, następuje nazwa parametru typu, a następnie dwukropek i listy ograniczeń dla tego parametru typu. Może być co najwyżej jeden `where` klauzulę dla każdego parametru typu i `where` klauzule mogą być wyświetlane w dowolnej kolejności. Podobnie jak `get` i `set` tokenów w metody dostępu właściwości `where` token nie jest słowem kluczowym.

Lista ograniczenia podany w `where` klauzuli może zawierać dowolne z następujących składników w następującej kolejności: jednego ograniczenia podstawowego, co najmniej jedno ograniczenie dodatkowej i ograniczenie konstruktora `new()`.

Ograniczenia podstawowy może być typem klasy lub ***odwoływać ograniczenie typu*** `class` lub ***wartość ograniczenia typu*** `struct`. Może być ograniczenie dodatkowej *type_parameter* lub *interface_type*.

Ograniczenie typu odwołania określa, że argument typu, używany dla parametru typu musi być typem referencyjnym. Wszystkie typy klas, typy interfejsów, typy delegatów, typy tablic i parametry typu, znane jako typu odwołania (zdefiniowany poniżej) spełnić tego ograniczenia.

Ograniczenie typu wartość określa, że argument typu, używany dla parametru typu musi być typem wartości niedopuszczającym wartości. Wszystkie typy nieprzyjmujące struktury, typach wyliczeniowych i parametry typu mające wartość ograniczenia typu spełnia tego ograniczenia. Należy pamiętać, że chociaż sklasyfikowane jako typ wartości — Typ dopuszczający wartość null ([typów dopuszczających wartości zerowe](types.md#nullable-types)) nie spełnia ograniczenia typu wartości. Parametr typu o ograniczenie typu wartości nie może mieć również *constructor_constraint*.

Typy wskaźników nigdy nie mogą mieć argumentów typu i nie są wliczane do zaspokojenia albo odwołanie do typu lub wartości typu ograniczenia.

W przypadku typu klasy, typ interfejsu lub parametrem typu ograniczenia typu określa minimalne "typ podstawowy", co argument typu, używany dla tego parametru typu muszą obsługiwać. Zawsze, gdy jest używany skonstruowanego typu lub metody rodzajowej, argument typu jest sprawdzana względem ograniczenia dotyczące parametrów typu w czasie kompilacji. Podany argument typu musi spełniać warunki opisane w [spełniających ograniczenia](types.md#satisfying-constraints).

A *class_type* ograniczenie musi spełniać następujące reguły:

*  Typ musi być typem klasy.
*  Typ nie może być `sealed`.
*  Typ nie może być jednym z następujących typów: `System.Array`, `System.Delegate`, `System.Enum`, lub `System.ValueType`.
*  Typ nie może być `object`. Ponieważ wszystkie typy wyprowadzono z klasy `object`, ograniczenia typu miałaby żadnego efektu, jeśli jego były dozwolone.
*  Co najwyżej jedno ograniczenie dla danego typu parametru może być typem klasy.

Typ określony jako *interface_type* ograniczenie musi spełniać następujące reguły:

*  Typ musi być typem interfejsu.
*  Typ nie może być określony więcej niż jeden raz w danym `where` klauzuli.

W obu przypadkach ograniczenie może obejmować dowolny z parametrów typu skojarzony typ lub metoda deklaracji jako część skonstruowanego typu i może obejmować typ został zadeklarowany.

Dowolny typ klasy lub interfejsu, określony jako ograniczenia parametru typu muszą być co najmniej tak samo dostępna ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)) jako typu ogólnego lub metody został zadeklarowany.

Typ określony jako *type_parameter* ograniczenie musi spełniać następujące reguły:

*  Typ musi być parametrem typu.
*  Typ nie może być określony więcej niż jeden raz w danym `where` klauzuli.

Ponadto musi istnieć żadne cykle w wykresie zależności parametrów typu, w którym zależność jest relacja przechodnia zdefiniowane przez:

*  Jeśli parametr typu `T` jest używany jako ograniczenie parametru typu `S` następnie `S` ***zależy od*** `T`.
*  Jeśli parametr typu `S` zależy od parametru typu `T` i `T` zależy od parametru typu `U` następnie `S` ***zależy od*** `U`.

Biorąc pod uwagę tej relacji, jest to błąd czasu kompilacji, dla parametru typu była zależna od siebie samej (bezpośrednio lub pośrednio).

Wszelkich ograniczeń musi być takie same dla wszystkich parametrów typu zależne. Jeśli parametr typu `S` zależy od typu parametru `T` następnie:

*  `T` nie może mieć wartość typu ograniczenia. W przeciwnym razie `T` skutecznie jest zapieczętowany tak `S` będą musieli się tego samego typu co `T`, eliminując konieczność łączenia się dwa parametry typu.
*  Jeśli `S` następnie posiada wartość ograniczenia typu `T` nie może mieć *class_type* ograniczenia.
*  Jeśli `S` ma *class_type* ograniczenie `A` i `T` ma *class_type* ograniczenie `B` musi być konwersję tożsamości lub niejawne Konwersja odwołania `A` do `B` lub niejawna konwersja odwołania z `B` do `A`.
*  Jeśli `S` zależy również od parametru typu `U` i `U` ma *class_type* ograniczenie `A` i `T` ma *class_type* ograniczenie `B` musi być konwersji tożsamości lub niejawna konwersja odwołania z `A` do `B` lub niejawna konwersja odwołania z `B` do `A`.

Jest on prawidłowy dla `S` ma ograniczenia typu wartości i `T` ma ograniczenia typu odwołania. Skutecznie ogranicza `T` do typów `System.Object`, `System.ValueType`, `System.Enum`i dowolny typ interfejsu.

Jeśli `where` klauzula dla parametru typu zawiera ograniczenie konstruktora (która ma postać `new()`), można użyć `new` operatora do utworzenia wystąpienia typu ([tworzenia wyrażeniaobiektów](expressions.md#object-creation-expressions)). Dowolny typ argumentu dla parametru typu o ograniczenie konstruktora musi mieć publicznego konstruktora bez parametrów (ten konstruktor niejawnie istnieje, dla każdego typu wartości) lub być parametrem typu ograniczenia typu wartości lub ograniczenie konstruktora (patrz [Ograniczenia parametru typu](classes.md#type-parameter-constraints) Aby uzyskać szczegółowe informacje).

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

Poniższy przykład jest błąd, ponieważ powoduje ona zależność cykliczną w wykresie zależności parametrów typu:
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

W poniższych przykładach pokazano też inne nieprawidłowe sytuacje:
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

***Skuteczne klasy bazowej*** parametru typu `T` jest zdefiniowana w następujący sposób:

*  Jeśli `T` nie ograniczeń podstawowego lub ograniczenia parametru typu, klasą bazową skuteczne `object`.
*  Jeśli `T` posiada wartość ograniczenia typ jest klasą bazową skuteczne `System.ValueType`.
*  Jeśli `T` ma *class_type* ograniczenie `C` , ale nie *type_parameter* jest klasą bazową obowiązywać ograniczenia, `C`.
*  Jeśli `T` nie ma przypisanego *class_type* ograniczenia, ale ma co najmniej jeden *type_parameter* ograniczenia, klasą bazową skuteczne jest najbardziej encompassed typu ([zniesione konwersji operatory](conversions.md#lifted-conversion-operators)) w zestawie skuteczne klasy bazowe jego *type_parameter* ograniczenia. Zasady spójności upewnij się, że istnieje najbardziej encompassed typu.
*  Jeśli `T` znajdują się oba *class_type* ograniczenia i co najmniej jeden *type_parameter* ograniczenia, klasą bazową skuteczne jest najbardziej encompassed typu ([zniesione konwersji operatory](conversions.md#lifted-conversion-operators)) w zestawie składający się z *class_type* ograniczenie `T` i skuteczne klasy bazowe jego *type_parameter* ograniczenia. Zasady spójności upewnij się, że istnieje najbardziej encompassed typu.
*  Jeśli `T` ma ograniczenia typu odwołania, ale nie *class_type* jest klasą bazową obowiązywać ograniczenia, `object`.

Na potrzeby tych zasad, jeśli T ma ograniczenie `V` czyli *value_type*, zamiast tego użyj specyficzny typ podstawowy elementu `V` czyli *class_type*. Nigdy nie może się zdarzyć w ograniczeniu jawnie danego, ale może wystąpić, gdy ograniczenia metody ogólnej niejawnie są dziedziczone przez nadrzędne deklaracji metody lub jawnych implementacji metody interfejsu.

Te reguły upewnij się, że od klasy bazowej zawsze *class_type*.

***Zestaw skuteczne interfejsu*** parametru typu `T` jest zdefiniowana w następujący sposób:

*  Jeśli `T` nie ma przypisanego *secondary_constraints*, jego zestaw skuteczne interfejsu jest pusty.
*  Jeśli `T` ma *interface_type* ograniczeń, ale nie *type_parameter* ograniczenia, jego zestawu skuteczne interfejsu jest jej zestaw *interface_type* ograniczenia.
*  Jeśli `T` nie ma przypisanego *interface_type* ale ma ograniczenia *type_parameter* ograniczenia, jego zestaw skuteczne interfejsu jest złożenie zestawy skuteczne interfejsu jego *type_ Parametr* ograniczenia.
*  Jeśli `T` znajdują się oba *interface_type* ograniczeń i *type_parameter* ograniczenia, jego zestaw skuteczne interfejsu jest złożenie swój zestaw *interface_type* ograniczenia i zestawy skuteczne interfejsu jego *type_parameter* ograniczenia.

Parametr typu jest ***znane jako typ referencyjny*** jeśli ma ograniczenia typu odwołania lub nie jest klasą bazową skuteczne `object` lub `System.ValueType`.

Wartości typu typu parametru ograniczonego typu może służyć do dostępu do składowych wystąpienia też dorozumianych przez ograniczenia. W przykładzie
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
metody `IPrintable` może być wywoływany bezpośrednio na `x` ponieważ `T` jest ograniczony do zaimplementowania zawsze `IPrintable`.

### <a name="class-body"></a>Treści klasy

*Class_body* klasy definiuje elementy członkowskie tej klasy.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Typy częściowe

Deklaracja typu mogą być dzielone między wieloma ***deklaracje typu częściowego***. Deklaracja typu jest tworzona z jego części postępując zgodnie z zasadami określonymi w tej sekcji — jest ona traktowana jako pojedyncza deklaracja w pozostałym czasie przetwarzania kompilacji i czasu wykonywania programu.

A *class_declaration*, *struct_declaration* lub *interface_declaration* reprezentuje deklarację typu częściowego, jeśli zawiera on `partial` modyfikator. `partial` nie jest słowem kluczowym i tylko działa jako modyfikatory, jeśli zostanie wyświetlona bezpośrednio przed słowa kluczowe `class`, `struct` lub `interface` w deklaracji typu lub przed typem `void` w deklaracji metody. W innych kontekstach może służyć jako identyfikator normalny.

Każda część deklaracji typu częściowego musi zawierać `partial` modyfikator. On musi mieć taką samą nazwę i być zadeklarowana w tej samej przestrzeni nazw lub deklaracja typu jako inne części. `partial` Modyfikator wskazuje dodatkowe elementy deklaracji typu może istnieć w innym miejscu, że istnienie takich dodatkowe elementy nie jest to wymagane, jest on prawidłowy dla typu przy użyciu jednej deklaracji, aby uwzględnić `partial` modyfikator.

Wszystkie części typu częściowego muszą być skompilowane ze sobą tak, aby części mogą być scalone w czasie kompilacji w jedną deklarację typu. Typy częściowe wyraźnie zezwala na już skompilowane typy można rozszerzyć.

Zagnieżdżone typy mogą być zadeklarowane w wiele części za pomocą `partial` modyfikator. Zazwyczaj typ zawierający jest zadeklarowany za pomocą `partial` jak dobrze, a każda część typu zagnieżdżonego jest zadeklarowana w innej części typu zawierającego.

`partial` Modyfikator nie jest dozwolone w deklaracjach typu delegata lub typu wyliczeniowego.

### <a name="attributes"></a>Atrybuty

Atrybuty typu częściowego są określane przez połączenie w nieokreślonej kolejności atrybutów każdej części. Jeśli atrybut znajduje się na wiele części, jest równoznaczne z użyciem atrybutu wielokrotnie w typie. Na przykład dwie części:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
są równoważne z deklaracji, takich jak:
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

Atrybuty w parametrach typu łączyć się w podobny sposób.

### <a name="modifiers"></a>Modyfikatory

Gdy deklarację typu częściowego obejmuje specyfikacji ułatwień dostępu ( `public`, `protected`, `internal`, i `private` Modyfikatory) musisz zaakceptować z innymi częściami obejmujących specyfikacji ułatwień dostępu. Jeśli żadna część typu częściowego, zawiera specyfikację ułatwień dostępu, typ podano dostępności odpowiednią wartość domyślną ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)).

Jeśli zawierają co najmniej jeden częściowe deklaracje typu zagnieżdżonego `new` modyfikator, ostrzeżenie nie jest zgłaszany, gdy typ zagnieżdżony ukrywa dziedziczonej składowej ([ukrywanie poprzez dziedziczenie](basic-concepts.md#hiding-through-inheritance)).

Jeśli co najmniej jeden częściowe deklaracje klasy zawierają `abstract` , modyfikator klasy jest uznawany za abstrakcyjny ([klasy abstrakcyjne](classes.md#abstract-classes)). W przeciwnym razie klasy jest traktowany jako nieabstrakcyjne.

Jeśli zawierają co najmniej jeden częściowe deklaracje klasy `sealed` , modyfikator klasy jest uznawany za zapieczętowany ([zapieczętowanych klas](classes.md#sealed-classes)). W przeciwnym razie jest uznawany za klasy niezapieczętowany.

Należy pamiętać, że klasa nie może być abstrakcyjny i zapieczętowany.

Gdy `unsafe` modyfikator jest używany w deklaracji typu częściowego, tylko tej określonej części jest uznawany za niebezpiecznym kontekście ([niebezpieczny kontekst](unsafe-code.md#unsafe-contexts)).

### <a name="type-parameters-and-constraints"></a>Parametry typu i ograniczenia

Jeśli typ ogólny jest zadeklarowany w wielu części, każda część musi podać parametry typu. Każda część musi mieć taką samą liczbę parametrów typu i taką samą nazwę dla każdego parametru typu, w kolejności.

Po deklaracji typu rodzajowego częściowego obejmuje ograniczenia (`where` klauzule), ograniczenia należy uzgodnić z innymi częściami obejmujących ograniczenia. Każda część obejmującą ograniczenia muszą mieć ograniczenia dla tego samego zestawu parametrów typu i dla każdego parametru typu zestawy główne, pomocniczej i ograniczenia konstruktora musi być równoważne. Dwa zestawy warunków ograniczających są równoważne, jeśli zawierają one te same elementy członkowskie. Jeśli żadna część typu rodzajowego częściowe określa ograniczenia parametru typu, parametry są traktowane jako typu nieograniczone.

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
jest poprawna, ponieważ te części, które obejmują skutecznie ograniczenia (dwie pierwsze) należy określić sam zestaw podstawowego, pomocniczego i ograniczenia konstruktora dla tego samego zestawu parametrów typu odpowiednio.

### <a name="base-class"></a>Klasa bazowa

Deklaracji klasy częściowej tym Specyfikacja klasy bazowej muszą wyrazić zgodę z innymi częściami obejmujących Specyfikacja klasy bazowej. Jeśli żadna z części klasy częściowej zawiera specyfikacji klasa bazowa, klasy bazowej staje się `System.Object` ([klas bazowych](classes.md#base-classes)).

### <a name="base-interfaces"></a>Interfejsy podstawowe

Zbiór interfejsy podstawowe w odniesieniu do typu zadeklarowany w wielu częściach jest złożenie interfejsy podstawowe określone w każdej części. Określonego interfejs podstawowy tylko nazwę raz w każdej części, ale jest dozwolony w przypadku wielu składa się z części nazwy tego samego bazowych. Musi istnieć tylko jedna implementacja członków dowolnej podanej interfejs podstawowy.

W przykładzie
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
Zbiór interfejsy podstawowe klasy `C` jest `IA`, `IB`, i `IC`.

Zwykle każda część zapewnia implementacja interfejsy zadeklarowanych w tej części; jednak nie jest to wymagane. Części mogą dostarczać implementację interfejsu zadeklarowanych w innej części.
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

### <a name="members"></a>Elementy członkowskie

Z wyjątkiem metody częściowe ([metod częściowych](classes.md#partial-methods)), zestaw elementów członkowskich typu zadeklarowany w wielu częściach to po prostu związek zestaw elementów członkowskich zadeklarowanych w każdej części. Treści wszystkich części deklaracji typu współużytkują ten sam obszar deklaracji ([deklaracje](basic-concepts.md#declarations)) oraz zakres każdego członka ([zakresy](basic-concepts.md#scopes)) rozszerza jednostkom wszystkie części. Domena dostępności członka zawsze zawiera wszystkie elementy typu otaczającego; `private` składowa zadeklarowana w jednej części jest ogólnie dostępne od innego składnika. Jest błąd kompilacji, aby zadeklarować ten sam element członkowski w więcej niż jedną część typu, chyba że ten element jest typu z `partial` modyfikator.

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

Kolejność elementów członkowskich w ramach danego typu jest rzadko istotny dla kodu C#, ale mogą być istotne w przypadku komunikowanie się z innych języków i środowisk. W takich przypadkach kolejność elementów członkowskich w ramach zadeklarowany w wielu częściach typu jest niezdefiniowane.

### <a name="partial-methods"></a>Metody częściowe

Metody częściowe mogą być definiowane w jednej części deklaracji typu i zaimplementowane w innym. Implementacja jest opcjonalna. Jeśli żadna część implementuje metody częściowej, deklaracji metody częściowej i wszystkie wywołania do niego są usuwane z deklaracji typu wynikające z kombinacji części.

Metod częściowych nie można zdefiniować modyfikatory dostępu, ale są niejawną kolekcją `private`. Ich zwracanym typem musi być `void`, i ich parametrów nie może mieć `out` modyfikator. Identyfikator `partial` jest rozpoznawana jako specjalne — słowo kluczowe w deklaracji metody, tylko wtedy, gdy znajdzie się on bezpośrednio przed `void` typu; w przeciwnym razie może służyć jako identyfikator normalny. Metoda częściowa nie może jawnie implementować metody interfejsu.

Istnieją dwa rodzaje częściowe deklaracje metody: Jeśli treść deklaracji metody średnikiem, deklaracja jest nazywany ***Definiowanie deklaracji metody częściowej***. Jeśli treść jest podawana jako *bloku*, deklaracja jest nazywany ***Implementowanie deklaracji metody częściowej***. W części deklaracji typu może istnieć tylko jeden Definiowanie deklaracji metody częściowej za pomocą podanego podpisu i może istnieć tylko jedna implementacja deklaracji metody częściowej za pomocą podanego podpisu. Jeśli podano deklaracji implementującej metody częściowej, odpowiedni Definiowanie deklaracji metody częściowej musi istnieć i deklaracji musi odpowiadać jako określona poniżej:

* Deklaracje muszą mieć ten sam Modyfikatory (ale nie musi to być w tej samej kolejności), nazwę metody, liczbę parametrów typu i liczby parametrów.
* Odpowiednich parametrów w deklaracjach muszą mieć ten sam Modyfikatory (ale nie musi to być w tej samej kolejności) i tymi samymi typami (modulo różnice w nazwach parametrów typu).
* Odpowiednich parametrów typu w deklaracji musi mieć tego samego ograniczenia (modulo różnice w nazwach parametrów typu).

Deklaracji implementującej metody częściowej może znajdować się w tej samej części jako odpowiednie definiujące deklaracji metody częściowej.

Tylko definiowanie metody częściowej uczestniczy w przeciążeniu rozdzielczości. W związku z tym czy podano deklaracji implementującej, wyrażenia wywołania mogą prowadzić do wywołania metody częściowej. Ponieważ metoda częściowa zawsze zwraca `void`, takie wyrażenia wywołania będą zawsze instrukcje wyrażeń. Ponadto ponieważ metoda częściowa jest niejawnie `private`, tych instrukcji będą zawsze wykonywane w ramach jednej części deklaracji typu, w którym zadeklarowano metody częściowej.

Jeśli żadna część deklaracji typu częściowego zawiera deklaracji implementującej dla danej metody częściowej, każda instrukcja wyrażenia wywołanie go po prostu zostanie usunięty z deklaracji typu połączone. Ten sposób wywołania wyrażenie, tym żadnych składowych wyrażeń, nie ma znaczenia w czasie wykonywania. Sama metoda częściowa zostaną również usunięte i nie będzie należeć do deklaracji typu połączone.

Jeśli deklaracji implementującej istnieje dla danej metody częściowej, wywołań metod częściowych zostaną zachowane. Metoda częściowa powoduje powstanie deklaracji metody, podobne do deklaracji implementującej metody częściowej z wyjątkiem następujących czynności:

* `partial` Modyfikator nie jest dołączony
* Atrybuty w deklaracji metody wynikowe są połączone atrybutów definiowania i deklaracji implementującej metody częściowej w nieokreślonej kolejności. Duplikaty nie zostaną usunięte.
* Atrybuty w parametrach wynikowy deklaracji metody są połączone atrybuty odpowiednich parametrów definiowania i deklaracji implementującej metody częściowej w nieokreślonej kolejności. Duplikaty nie zostaną usunięte.

Jeśli dla metody częściowej M jest deklaracją definiującą, ale nie deklaracji implementującej, obowiązują następujące ograniczenia:

* Jest to błąd czasu kompilacji do utworzenia delegata do metody ([delegować Tworzenie wyrażenia](expressions.md#delegate-creation-expressions)).
* Jest to błąd kompilacji, aby odwołać się do `M` wewnątrz funkcja anonimowa, która jest konwertowana na typ drzewa wyrażeń ([oceny funkcja anonimowa konwersji na typy drzewa wyrażenie](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).
* Wyrażenia, które pojawiają się jako część wywołania `M` nie wpływają na stan asercję określonego przypisania ([asercję określonego przypisania](variables.md#definite-assignment)), co potencjalnie może prowadzić do błędów kompilacji.
* `M` nie może być punktem wejścia dla aplikacji ([uruchamiania aplikacji](basic-concepts.md#application-startup)).

Metody częściowe są przydatne do umożliwiając jedną część deklaracji typu umożliwia dostosowywanie zachowania innej części, np. taki, który jest generowany przez narzędzie. Rozważmy następującą deklarację klasy częściowej:
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

Jeśli ta klasa jest skompilowana bez żadnych innych części, definiującą deklaracje metody częściowej i ich wywołania zostaną usunięte, a wynikowy deklaracji klasy połączone będzie odpowiednikiem następujących czynności:
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

Załóżmy, że innej części zostanie podany, jednak zapewniającą deklaracji implementującej metody częściowe:
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

Następnie wynikowy deklaracji klasy połączone będzie odpowiednikiem następujących czynności:
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

### <a name="name-binding"></a>Powiązania nazwy

Mimo że każda część rozszerzalnego typu musi być zadeklarowany w tej samej przestrzeni nazw, części są zwykle pisane w obrębie deklaracji innej przestrzeni nazw. W efekcie różnych `using` dyrektywy ([dyrektywy Using](namespaces.md#using-directives)) może być stosowany w przypadku każdej części. Interpretując nazwy proste ([wnioskowanie o typie](expressions.md#type-inference)) w ramach jednej strony, tylko `using` dyrektywy deklarację przestrzeni nazw, otaczający tej części są traktowane jako. Może to spowodować, że ten sam identyfikator mających różne znaczenie w różnych częściach:
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

Elementy członkowskie klasy składają się z elementów członkowskich, wynikające z jego *class_member_declaration*s i elementy członkowskie odziedziczone po bezpośredniej klasie bazowej.

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

*  Stałe, które reprezentują stałe wartości skojarzone z klasą ([stałe](classes.md#constants)).
*  Pola, które są zmiennymi klasy ([pola](classes.md#fields)).
*  Metody, które implementują obliczeń i akcji, które mogą być wykonywane przez klasę ([metody](classes.md#methods)).
*  Właściwości, które definiują o nazwie właściwości i akcje związane z odczytywaniem i zapisywaniem tych cech ([właściwości](classes.md#properties)).
*  Zdarzenia, które definiują powiadomień, które mogą być generowane przez klasę ([zdarzenia](classes.md#events)).
*  Indeksatory, umożliwiające wystąpień klasy, która ma zostać pomyślnie zindeksowane w taki sam sposób (składniowo) jako tablice ([indeksatory](classes.md#indexers)).
*  Operatory, które określają Operatory wyrażeń, które mogą być stosowane do wystąpień klasy ([operatory](classes.md#operators)).
*  Wystąpienie konstruktorów implementować akcje wymagane to inicjalizacji wystąpień klasy ([wystąpienia konstruktory](classes.md#instance-constructors))
*  Destruktory, implementujących czynności, które należy wykonać, zanim wystąpienia klasy trwale zostaną odrzucone ([destruktory](classes.md#destructors)).
*  Konstruktory statyczne, implementujących działania wymagane w celu zainicjowania samej klasy ([konstruktorów statycznych](classes.md#static-constructors)).
*  Typy, które reprezentują typy, które są lokalne w klasie ([zagnieżdżone typy](classes.md#nested-types)).

Elementy członkowskie, które mogą zawierać kod wykonywalny są nazywane zbiorczo *funkcji elementów członkowskich* o typie danej klasy. Funkcja elementów członkowskich typu klasy są metody, właściwości, zdarzenia, indeksatory, operatory, konstruktory wystąpień, destruktory i konstruktorów statycznych tego typu klasy.

A *class_declaration* tworzy nowe miejsce do deklaracji ([deklaracje](basic-concepts.md#declarations)), a *class_member_declaration*s natychmiast zawartych *klasy _declaration* wprowadzenia nowych członków do tej deklaracji przestrzeni. Następujące reguły mają zastosowanie do *class_member_declaration*s:

*  Konstruktory wystąpień, destruktory i konstruktorów statycznych musi mieć taką samą nazwę jako natychmiastowo otaczającą klasę. Wszystkie inne elementy członkowskie muszą mieć nazwy, które różnią się od nazwy natychmiastowo otaczającą klasę.
*  Nazwa — stała, pola, właściwości, zdarzenia lub typ musi się różnić od nazw innych składowych zadeklarowanych w tej samej klasy.
*  Nazwa metody muszą różnić się od nazw wszystkich innych niż metod zadeklarowanych w tej samej klasy. Ponadto, podpis ([podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading)) z metody muszą różnić się od podpisy wszystkich pozostałych metod zadeklarowanych w tej samej klasie i dwie metody zadeklarowanej w tej samej klasy nie może mieć podpisów, które różnią się jedynie przez `ref` i `out`.
*  Podpis konstruktora wystąpienia muszą różnić się od podpisy wszystkie inne konstruktory wystąpień zadeklarowane w tej samej klasy, a dwa konstruktory zadeklarowane w tej samej klasy nie może mieć podpisów, które różnią się jedynie przez `ref` i `out`.
*  Podpis indeksatora musi się różnić od podpisy innymi indeksatorami zadeklarowane w tej samej klasy.
*  Podpis operatora musi się różnić od podpisy innych operatorów zadeklarowane w tej samej klasy.

Dziedziczone składowe typu klasy ([dziedziczenia](classes.md#inheritance)) nie są częścią przestrzeni deklaracji klasy. W związku z tym Klasa pochodna może zadeklarować członka o tej samej nazwie lub podpisie, jako dziedziczonej składowej (co w efekcie ukrywa dziedziczoną składową).

### <a name="the-instance-type"></a>Typ wystąpienia

Każdy deklaracji klasy jest skojarzony typ powiązanej ([powiązane i wskazanych typów](types.md#bound-and-unbound-types)), ***typu wystąpienia***. Dla deklaracji klasy ogólnej, typu wystąpienia jest tworzona przez tworzenie skonstruowanego typu ([skonstruowany typy](types.md#constructed-types)) z deklaracji typu z każdą z podanego typu argumentów jest odpowiedni parametr typu. Ponieważ typ wystąpienia używa parametrów typu, jego można używać tylko gdy parametry typu są w zakresie; oznacza to wewnątrz deklaracji klasy. Typ wystąpienia jest typem `this` dla kodu napisanego w obrębie deklaracji klasy. Dla klas nieogólnego Typ wystąpienia jest po prostu deklarowanej klasy. Poniżej przedstawiono kilka deklaracji klasy, wraz z ich typów wystąpień: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Członkowie typy utworzone

Dziedziczone elementy członkowskie skonstruowanego typu są uzyskiwane poprzez zastąpienie dla każdego *type_parameter* w deklaracji elementu członkowskiego, odpowiedni *type_argument* skonstruowanego typu. Proces podstawienia opiera się na znaczenia semantycznego deklaracje typu i nie jest po prostu tekstową podstawienia.

Na przykład biorąc pod uwagę deklaracji klasy ogólnej
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
skonstruowany typ `Gen<int[],IComparable<string>>` ma następujące składowe:
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

Typ elementu członkowskiego `a` w deklaracji klasy ogólnej `Gen` jest "dwuwymiarową tablicę `T`", więc typ elementu członkowskiego `a` w skonstruowanego typu powyżej jest "dwuwymiarową tablicę jednowymiarową tablicę `int`", lub `int[,][]`.

W ramach wystąpienia funkcji członków, typ `this` jest typem wystąpienia ([typu wystąpienia](classes.md#the-instance-type)) zawierający deklaracji.

Wszystkie elementy członkowskie klasy generycznej można używać parametrów typu w otaczającej klasy, bezpośrednio lub jako część skonstruowanego typu. Po zamknięciu określonego skonstruowany typ ([otwarte i zamknięte typy](types.md#open-and-closed-types)) jest używany w czasie wykonywania, każde użycie parametru typu jest zastępowany rzeczywisty typ argumentu dostarczane do skonstruowanego typu. Na przykład:
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

Klasa ***dziedziczy*** członkowie jej typ bezpośredniej klasie bazowej. Dziedziczenie oznacza, że klasa niejawnie zawiera wszystkie elementy członkowskie typu bezpośrednią klasą bazową, z wyjątkiem wystąpienia konstruktory, destruktory i konstruktory statyczne klasy bazowej. Niektórych ważnych aspektów dziedziczenia są następujące:

*  Dziedziczenie jest przechodnia. Jeśli `C` jest tworzony na podstawie `B`, i `B` jest tworzony na podstawie `A`, następnie `C` dziedziczy elementów członkowskich zadeklarowanych w `B` oraz elementów członkowskich zadeklarowanych w `A`.
*  Klasa pochodna rozszerza jego bezpośrednia klasa bazowa. Klasy pochodne mogą dodawać nowych członków do tych, które dziedziczy, ale go nie można usunąć definicji dziedziczonego członka.
*  Konstruktory wystąpień, destruktory i konstruktory statyczne nie są dziedziczone, ale wszystkie inne elementy członkowskie są, niezależnie od ich deklarowana dostępność metody ([dostęp do elementu członkowskiego](basic-concepts.md#member-access)). Jednak w zależności od ich deklarowana dostępność metody dziedziczone elementy Członkowskie mogą być niedostępne w klasie pochodnej.
*  Klasa pochodna może ***Ukryj*** ([ukrywanie poprzez dziedziczenie](basic-concepts.md#hiding-through-inheritance)) odziedziczone składowe przez zadeklarowanie nowych elementów członkowskich z tej samej nazwie lub podpisie. Pamiętaj jednak, ukrywać dziedziczonego członka nie powoduje usunięcia tego elementu członkowskiego — to jedynie sprawia, że ten element członkowski dostępna bezpośrednio za pośrednictwem klasy pochodnej.
*  Wystąpienie klasy zawiera zbiór wszystkie pola wystąpienia zadeklarowana w klasie i jej klasy bazowe i niejawną konwersję ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) istnieje typ klasy pochodnej dowolny z jej typów klasy bazowej. W związku z tym odwołanie do wystąpienia klasy pochodnej niektóre mogą być traktowane jako odwołanie do wystąpienia któregokolwiek z jej klas podstawowych.
*  Klasę można zadeklarować wirtualne metody, właściwości i indeksatorów i zastępować implementację z tych funkcji składowych klas pochodnych. Dzięki temu klasy do następującej liczby etapów stwierdzono zachowanie polimorficzne, w którym akcje wykonywane przez wywołanie funkcji Członkowskich różni się zależnie od typu run-time wystąpienia za pomocą którego ten element członkowski funkcja jest wywoływana.

Dziedziczonego elementu członkowskiego typu klasy skonstruowane są członkami natychmiastowego klasy podstawowej typu ([klas bazowych](classes.md#base-classes)), który znajduje się poprzez zastąpienie argumentów typu skonstruowanego typu dla każdego wystąpienia danego typu Parametry w *class_base* specyfikacji. Te elementy członkowskie z kolei są przekształcane wstawiając dla każdego *type_parameter* w deklaracji elementu członkowskiego, odpowiedni *type_argument* z *class_base* Specyfikacja.

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

W powyższym przykładzie skonstruowanego typu `D<int>` ma składową dziedziczone `public int G(string s)` uzyskane poprzez zastąpienie argumentów typu `int` dla parametru typu `T`. `D<int>` ma również odziedziczonej składowej przed deklaracją klasy `B`. Ta dziedziczonego elementu członkowskiego jest określana przez pierwszy określająca typ klasy bazowej `B<int[]>` z `D<int>` wstawiając `int` dla `T` w specyfikacji klasy bazowej `B<T[]>`. Następnie jako argument typu `B`, `int[]` zostanie zastąpiony `U` w `public U F(long index)`, którego można dziedziczonego elementu członkowskiego `public int[] F(long index)`.

### <a name="the-new-modifier"></a>New — modyfikator

A *class_member_declaration* może zadeklarować członka o tej samej nazwie lub podpisie, jako dziedziczonego członka. W takiej sytuacji składowej klasy pochodnej jest nazywany ***Ukryj*** składowej klasy bazowej. Ukrywanie dziedziczonej składowej nie jest uważany za błąd, ale spowodować, że kompilator generuje ostrzeżenia. Aby pominąć to ostrzeżenie, może zawierać deklaracji składowej klasy pochodnej `new` modyfikator, aby wskazać, że składowa pochodnej jest przeznaczona do Ukryj składową bazową. W tym temacie omówiono w dalszej [ukrywanie poprzez dziedziczenie](basic-concepts.md#hiding-through-inheritance).

Jeśli `new` modyfikator znajduje się w deklaracji, która nie ukrywa dziedziczonego członka, w tym celu jest wyświetlane ostrzeżenie. To ostrzeżenie jest pominięty, usuwając `new` modyfikator.

### <a name="access-modifiers"></a>Modyfikatory dostępu

A *class_member_declaration* może mieć dowolną pięć rodzajów deklarowana dostępność metody ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` , lub `private`. Z wyjątkiem `protected internal` kombinacji, jest to błąd czasu kompilacji, można określić więcej niż jeden modyfikator dostępu. Gdy *class_member_declaration* nie zawiera żadnych modyfikatory dostępu `private` zakłada, że.

### <a name="constituent-types"></a>Typy składowych

Typy, które są używane w deklaracji elementu członkowskiego są nazywane składowych typów tego członka. Możliwe typy składowych to typ stałej, pola, właściwości, zdarzenia lub indeksator, zwracany typ metody lub operatora i typy parametrów metody, indeksator, operator lub konstruktora wystąpienia. Typy składowych elementu członkowskiego musi być co najmniej tak samo dostępna jak ten sam element członkowski ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).

### <a name="static-and-instance-members"></a>Statyczna i wystąpienia elementów członkowskich

Elementy członkowskie klasy są albo ***statyczne elementy członkowskie*** lub ***wystąpieniami elementów członkowskich***. Ogólnie rzecz biorąc warto traktować statyczne elementy członkowskie jako należące do typów klas i składowych wystąpienia jako należące do obiektów (wystąpienia typów klas)

Kiedy zawiera deklarację pola, metody, właściwości, zdarzenia, operator lub Konstruktor `static` modyfikator, deklaruje składową statyczną. Ponadto deklaracji stałą lub typ niejawnie deklaruje statyczny element członkowski. Statyczne elementy członkowskie mają następującą charakterystykę:

*  Gdy statyczny element członkowski `M` mowa w punkcie *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `E.M`, `E` musi oznaczają typu zawierającego `M`. Jest to błąd czasu kompilacji dla `E` do oznaczenia wystąpienia.
*  Pole statyczne identyfikuje dokładnie jednej lokalizacji magazynu, być współużytkowane przez wszystkie wystąpienia typu danej klasy zamknięte. Niezależnie od tego, ile wystąpień typu danej klasy zamknięte są tworzone istnieje tylko kiedykolwiek jedną kopię pole statyczne.
*  Funkcja statyczna elementu członkowskiego (metody, właściwości, zdarzenia, operator lub konstruktora) nie będzie działać na określonym wystąpieniu i jest to błąd kompilacji, aby odwołać się do `this` w składowej funkcji.

Jeśli deklaracja pola, metody, właściwości, zdarzenia, indeksator, Konstruktor lub destruktor nie obejmuje `static` modyfikator, deklaruje składową wystąpienia. (Elementu członkowskiego wystąpienia jest czasami nazywane niestatycznego elementu członkowskiego). Elementy członkowskie wystąpień mają następującą charakterystykę:

*  Gdy elementu członkowskiego wystąpienia `M` mowa w punkcie *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `E.M`, `E` musi oznaczają wystąpienia typu zawierającego `M`. Jest to błąd czasu powiązania dla `E` określający typ.
*  Każde wystąpienie klasy zawiera oddzielny zestaw wszystkie pola wystąpienia klasy.
*  Funkcja elementu członkowskiego wystąpienia (metoda, właściwość, indeksator, wystąpienie konstruktora lub destruktora) działa na danego wystąpienia klasy, a to wystąpienie może być dostępna jako `this` ([dostęp](expressions.md#this-access)).

W poniższym przykładzie pokazano reguły do uzyskiwania dostępu do statycznych i elementy członkowskie wystąpień:
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

`F` Metoda pokazuje, że w składową wystąpienia funkcji *simple_name* ([proste nazwy](expressions.md#simple-names)) może służyć do dostępu do składowych wystąpienia i statyczne elementy członkowskie. `G` Metoda wskazuje, że w statycznej funkcji składowej, jest to błąd czasu kompilacji do dostępu do członka wystąpienia, za pośrednictwem *simple_name*. `Main` Metoda ma pokazać, że w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)), elementy członkowskie wystąpień muszą być dostępne za pośrednictwem wystąpień i statyczne elementy członkowskie muszą być dostępne za pośrednictwem typów.

### <a name="nested-types"></a>Zagnieżdżone typy

Typem zadeklarowanym w deklaracji klasy lub struktury jest wywoływana ***zagnieżdżony typ***. Typ, który jest zadeklarowany w jednostki kompilacji lub przestrzeni nazw jest wywoływana ***typu niezagnieżdżonego***.

W przykładzie
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
Klasa `B` jest typem zagnieżdżonym, ponieważ jest on zadeklarowany w klasie `A`, a klasa `A` jest typu niezagnieżdżonego, ponieważ jest on zadeklarowany w ramach jednostki kompilacji.

#### <a name="fully-qualified-name"></a>W pełni kwalifikowana nazwa

W pełni kwalifikowana nazwa ([w pełni kwalifikowane nazwy](basic-concepts.md#fully-qualified-names)) jest typem zagnieżdżonym `S.N` gdzie `S` jest w pełni kwalifikowaną nazwę typu, który typ `N` jest zadeklarowana.

#### <a name="declared-accessibility"></a>Deklarowana dostępność metody

Typy zagnieżdżone nie mogą mieć `public` lub `internal` zadeklarowany ułatwień dostępu i mieć `internal` zadeklarowana dostępność domyślnie. Zagnieżdżone typy mogą być deklarowana dostępność metody w takiej formie, oraz jeden lub więcej dodatkowych formularzy deklarowana dostępność metody w zależności od tego, czy typ zawierający klasy lub struktury:

*  Typ zagnieżdżony, która jest zadeklarowana w klasie może być spowodowany pięć form deklarowana dostępność metody (`public`, `protected internal`, `protected`, `internal`, lub `private`) i, podobnie jak inne elementy członkowskie klasy, wartością domyślną jest `private` zadeklarowana ułatwienia dostępu.
*  Typ zagnieżdżony, która jest zadeklarowana w strukturze może mieć jedną z trzech rodzajów deklarowana dostępność metody (`public`, `internal`, lub `private`) i, podobnie jak inne elementy członkowskie struktury, wartością domyślną jest `private` zadeklarowana ułatwień dostępu.

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
deklaruje klasę zagnieżdżoną prywatnej `Node`.

#### <a name="hiding"></a>Ukrywanie

Typ zagnieżdżony może ukryć ([ukrywanie nazwy](basic-concepts.md#name-hiding)) podstawową składową. `new` Modyfikator jest dozwolone w deklaracjach typu zagnieżdżonego, dzięki czemu mogą być jawnie wyrażone ukrywanie. Przykład
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
zawiera klasę zagnieżdżoną `M` , ukrywa metodę `M` zdefiniowane w `Base`.

#### <a name="this-access"></a>Ten dostęp

Typ zagnieżdżony, a jej typ zawierający nie mają specjalne relacji w odniesieniu do *this_access* ([dostęp](expressions.md#this-access)). W szczególności `this` w ramach typu zagnieżdżonego nie można używać do odwoływania się do składowych wystąpienia typu zawierającego. W przypadkach, gdzie typ zagnieżdżony musi mieć dostęp do wystąpienia członkowie jej typ zawierający, można podać dostępu, zapewniając `this` dla wystąpienia typu zawierającego jako argument konstruktora dla typu zagnieżdżonego. Poniższy przykład
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
pokazano tej techniki. Wystąpienie `C` tworzy wystąpienie `Nested` i przekazuje swój własny `this` do `Nested`przez konstruktora w celu zapewnienia późniejszego dostępu do `C`firmy wystąpieniami elementów członkowskich.

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Dostęp do prywatnych i chronionych członków typu zawierającego

Zagnieżdżony typ ma dostęp do wszystkich elementów członkowskich, które są dostępne dla zawierającego ją typu, w tym elementy członkowskie typu zawierającego, których `private` i `protected` zadeklarowana ułatwień dostępu. Przykład
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
Pokazuje klasy `C` zawiera klasę zagnieżdżoną `Nested`. W ramach `Nested`, Metoda `G` statyczna metoda `F` zdefiniowane w `C`, i `F` prywatne zadeklarował ułatwień dostępu.

Zagnieżdżony typ również dostępu do chronionych składowych zdefiniowanych w typie podstawowym jej typ zawierający. W przykładzie
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
Klasa zagnieżdżona `Derived.Nested` uzyskuje dostęp do chronionych metoda `F` zdefiniowane w `Derived`firmy podstawowej klasy, `Base`, wywołanie za pośrednictwem wystąpienia `Derived`.

#### <a name="nested-types-in-generic-classes"></a>Zagnieżdżone typy klas ogólnych

Deklaracji klasy ogólnej może zawierać deklaracje typu zagnieżdżonego. Parametry typu otaczającej klasy może służyć w obrębie zagnieżdżonych typów. Deklaracji typu zagnieżdżonego może zawierać parametrów typu dodatkowe, które mają zastosowanie tylko do typu zagnieżdżonego.

Każdy zawartych w deklaracji klasy ogólnej deklaracji typu jest niejawnie deklaracją typu ogólnego. Podczas pisania odwołanie do typu zagnieżdżone wewnątrz typu rodzajowego, musi mieć nazwę zawierającą skonstruowanego typu, w tym argumentów typu. Jednak z w ramach zewnętrznej klasy typu zagnieżdżonego można używać bez kwalifikacji; Typ wystąpienia klasy zewnętrznej niejawnie służy przy konstruowaniu zagnieżdżonych typów. Poniższy przykład pokazuje trzy różne sposoby poprawne do odwoływania się do skonstruowanego typu utworzone na podstawie `Inner`; pierwsze dwa są równoważne:
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

Mimo że to jest źle programowania stylu, parametr typu w typie zagnieżdżonym można ukryć członka lub parametr zostało zadeklarowane w typie zewnętrznego typu:
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>Nazw zastrzeżoną składową

W celu ułatwienia podstawowych języka C# środowiska wykonawczego wykonania, dla każdej deklaracji elementu członkowskiego źródła, właściwości, zdarzenia lub indeksatora implementacji należy zarezerwować dwóch podpisów metody oparte na rodzaju deklaracji składowej, nazwy i typu. Jest to błąd czasu kompilacji dla programu zadeklarować członka, którego podpis pasuje do jednej z tych zarezerwowanych podpisów, nawet wtedy, gdy podstawowej implementacji czasu wykonywania nie oznacza, że użycie te zastrzeżenia.

Nazw zastrzeżonych nie wprowadzają deklaracji, w związku z tym nie uczestniczą w wyszukanie członka. Jednak deklarację skojarzonej zarezerwowanych metoda podpisów uczestniczą w dziedziczeniu ([dziedziczenia](classes.md#inheritance)) i może być ukryty za pomocą `new` modyfikator ([nowy modyfikator](classes.md#the-new-modifier)).

Rezerwacja te nazwy służy trzy funkcje:

*  Aby zezwolić na podstawowej implementacji użyć zwykłej identyfikatora jako nazwę metody get lub dostęp do funkcji języka C#.
*  Aby zezwolić na inne języki współpracować przy użyciu zwykłej identyfikatora jako nazwę metody get lub Ustaw dostęp do funkcji języka C#.
*  Aby mieć pewność, że źródła akceptowane przez kompilator odpowiadające jeden jest akceptowana przez inną, wprowadzając szczegóły zastrzeżoną składową nazwy spójne we wszystkich implementacji języka C#.

Deklaracja destruktora ([destruktory](classes.md#destructors)) również powoduje, że podpis mają zostać zarezerwowane ([nazwy elementów członkowskich zarezerwowane dla destruktory](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Zastrzeżone nazwy elementów członkowskich dla właściwości

Dla właściwości `P` ([właściwości](classes.md#properties)) typu `T`, następujące podpisy są zarezerwowane:

```csharp
T get_P();
void set_P(T value);
```

Zarówno podpisy są zarezerwowane, nawet jeśli właściwość jest tylko do odczytu lub tylko do zapisu.

W przykładzie
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
Klasa `A` definiuje właściwość tylko do odczytu `P`, dlatego zarezerwowanie podpisów dla `get_P` i `set_P` metody. Klasa `B` pochodzi od klasy `A` i ukrywa obu tych zarezerwowanych podpisów. Przykład generuje dane wyjściowe:
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Zastrzeżone nazwy elementów członkowskich dla zdarzeń

Zdarzenia `E` ([zdarzenia](classes.md#events)) typu delegata `T`, następujące podpisy są zarezerwowane:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Zastrzeżone nazwy elementów członkowskich dla indeksatorów

Dla indeksatora ([indeksatory](classes.md#indexers)) typu `T` z listą parametrów `L`, następujące podpisy są zarezerwowane:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Zarówno podpisy są zarezerwowane, nawet jeśli indeksatora jest tylko do odczytu lub tylko do zapisu.

Ponadto nazwa elementu członkowskiego `Item` jest zarezerwowany.

#### <a name="member-names-reserved-for-destructors"></a>Zastrzeżone nazwy elementów członkowskich dla destruktorów

Klasy zawierające destruktora ([destruktory](classes.md#destructors)), następujący podpis jest zastrzeżona:
```csharp
void Finalize();
```

## <a name="constants"></a>Stałe

A ***stałej*** jest składową klasy, która reprezentuje wartość stałą: wartość, która może zostać obliczony w czasie kompilacji. A *constant_declaration* wprowadza co najmniej jedną stałą danego typu.

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

A *constant_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)), `new` modyfikator ([nowy modyfikator](classes.md#the-new-modifier)), Nieprawidłowa kombinacja modyfikatory dostępu czterech i ([modyfikatorach dostępu](classes.md#access-modifiers)). Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych za *constant_declaration*. Mimo że stałe są traktowane jako elementy statyczne, *constant_declaration* nie wymaga ani nie zezwala na `static` modyfikator. Jest to błąd dla tego samego modyfikator pojawią się wiele razy w deklaracji stałej.

*Typu* z *constant_declaration* Określa typ elementów członkowskich wprowadzonych przez tę deklarację. Typ następuje lista *constant_declarator*s, z których każdy wprowadza nowy element członkowski. A *constant_declarator* składa się z *identyfikator* , nazwy elementu członkowskiego, a następnie "`=`" token, a następnie *constant_expression* ([ Wyrażenia stałe](expressions.md#constant-expressions)) daje wartość elementu członkowskiego.

*Typu* określone w stałą deklaracja musi być `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, *enum_type*, lub *reference_type*. Każdy *constant_expression* należy przekazać wartości typu docelowego, lub typu, który można przekonwertować na typ docelowy niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)).

*Typu* stałej musi być co najmniej tak samo dostępna jak stała się ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).

Wartość stałej jest uzyskiwana przy użyciu wyrażenia *simple_name* ([proste nazwy](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)).

Stała się uczestniczyć w *constant_expression*. W efekcie stałą mogą być używane w taki sposób, w konstrukcji, która wymaga *constant_expression*. Przykładami takich konstrukcji `case` etykiet, `goto case` instrukcji `enum` deklaracji elementu członkowskiego, atrybutów i innych deklaracji stałej.

Zgodnie z opisem w [wyrażeń stałych](expressions.md#constant-expressions), *constant_expression* jest wyrażeniem, które mogą zostać w pełni oszacowane w czasie kompilacji. Ponieważ jedynym sposobem, aby utworzyć inną niż null wartości *reference_type* innych niż `string` polega na zastosowaniu `new` operatora, a ponieważ `new` operator nie jest dozwolona w *constant_ wyrażenie*, jedyną możliwą wartością dla stałych typu *reference_type*s innych niż `string` jest `null`.

Gdy potrzeby Symboliczna nazwa wartości stałej, ale gdy typ wartości nie jest dozwolone w deklaracji stałej lub gdy nie można obliczyć wartości w czasie kompilacji przez *constant_expression*, `readonly` pola () [Pola tylko do odczytu](classes.md#readonly-fields)) mogą być używane zamiast tego.

Deklaracji stałej, która deklaruje kilka stałych jest odpowiednikiem w wielu deklaracjach stałe jednego z atrybutami, Modyfikatory i typu. Na przykład
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

Stałe są dozwolone zależą inne stałe w tym samym programie, tak długo, jak zależności nie mają charakteru cykliczne. Kompilator automatycznie rozmieszcza można obliczyć wartości stałej deklaracje w odpowiedniej kolejności. W przykładzie
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
Kompilator najpierw ocenia `A.Y`, ocenia następnie `B.Z`i na końcu ocenia `A.X`, produkcji wartości `10`, `11`, i `12`. Deklaracji stałej może zależeć od stałe z innych programów, ale takie zależności są możliwe tylko w jednym kierunku. Odwołujące się do w powyższym przykładzie, jeśli `A` i `B` zostały zgłoszone w oddzielnych programach, możliwe będzie `A.X` zależą `B.Z`, ale `B.Z` może następnie równocześnie nie zależą od `A.Y`.

## <a name="fields"></a>Pola

A ***pola*** jest elementem członkowskim, który reprezentuje zmienną związaną z obiektem lub klasą. A *field_declaration* wprowadza co najmniej jedno pole danego typu.

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

A *field_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)), `new` modyfikator ([nowy modyfikator](classes.md#the-new-modifier)), Nieprawidłowa kombinacja modyfikatory dostępu cztery ([modyfikatorach dostępu](classes.md#access-modifiers)) i `static` modyfikator ([pola statyczne i wystąpienia](classes.md#static-and-instance-fields)). Ponadto *field_declaration* mogą obejmować `readonly` modyfikator ([pola tylko do odczytu](classes.md#readonly-fields)) lub `volatile` modyfikator ([pola nietrwałego](classes.md#volatile-fields)), ale nie oba. Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych za *field_declaration*. Jest to błąd dla tego samego modyfikator pojawią się wiele razy w deklarację pola.

*Typu* z *field_declaration* Określa typ elementów członkowskich wprowadzonych przez tę deklarację. Typ następuje lista *variable_declarator*s, z których każdy wprowadza nowy element członkowski. A *variable_declarator* składa się z *identyfikator* , nazwy tego elementu członkowskiego, opcjonalnie następuje "`=`" token i *variable_initializer* ([ Inicjatory zmiennej](classes.md#variable-initializers)) daje wartość początkową tego członka.

*Typu* pola musi być co najmniej tak samo dostępna jak samo pole ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).

Wartość pola są uzyskiwane przy użyciu wyrażenia *simple_name* ([proste nazwy](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)). Wartość pola — tylko do odczytu jest modyfikowane przy użyciu *przypisania* ([operatory przypisania](expressions.md#assignment-operators)). Wartość pola — tylko do odczytu mogą być uzyskane i modyfikować za pomocą przyrostka inkrementacji i operatory dekrementacji ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators)) przedrostkowe i operatory dekrementacji ([prefiksu operatory inkrementacji i dekrementacji](expressions.md#prefix-increment-and-decrement-operators)).

Deklaracja pola, która deklaruje wiele pól jest odpowiednikiem w wielu deklaracjach pojedynczego pola z atrybutami, Modyfikatory i typu. Na przykład
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

Jeśli deklaracja pola zawiera `static` są pól wprowadzonych przez tę deklarację, modyfikator ***pola statyczne***. Gdy nie `static` modyfikator, pól wprowadzonych przez tę deklarację ***wystąpienia pól***. Pola statyczne i pola wystąpienia są dwa rodzaje kilka zmiennych ([zmienne](variables.md)) obsługiwane przez C# i w czasie ich są określane jako ***statyczne zmienne*** i ***zmienne wystąpienia*** , odpowiednio.

Statyczne pole nie jest częścią konkretnego wystąpienia; Zamiast tego należy go jest współużytkowany przez wszystkie wystąpienia typu zamknięte ([otwarte i zamknięte typy](types.md#open-and-closed-types)). Niezależnie od tego, ile wystąpień typu zamkniętej klasy są tworzone istnieje tylko kiedykolwiek jedną kopię pole statyczne w domenie skojarzonej aplikacji.

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

Pola wystąpienia należy do wystąpienia. W szczególności każde wystąpienie klasy zawiera oddzielny zestaw wszystkie pola wystąpienia tej klasy.

Gdy pole jest przywoływany w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `E.M`, jeśli `M` jest polem statyczne `E` musi oznaczają typu zawierającego `M` Jeśli `M` jest pole wystąpienia, E musi oznaczają wystąpienia typu zawierającego `M`.

Różnice statyczne elementy członkowskie wystąpień zostały one omówione w dalszych [statyczna i wystąpienia elementów członkowskich](classes.md#static-and-instance-members).

### <a name="readonly-fields"></a>Pola tylko do odczytu

Gdy *field_declaration* obejmuje `readonly` są pól wprowadzonych przez tę deklarację, modyfikator ***pola tylko do odczytu***. Bezpośrednie przypisania do pól tylko do odczytu mogą występować wyłącznie jako część tej deklaracji lub w konstruktorze wystąpienia lub statycznego konstruktora w tej samej klasie. (Pola tylko do odczytu może być przypisany do wielokrotnie w tych kontekstach.) W szczególności, bezpośrednie przypisania do `readonly` pola są dozwolone tylko w następujących kontekstów się:

*  W *variable_declarator* powoduje to wprowadzenie pola (umieszczając *variable_initializer* w deklaracji).
*  Pola wystąpienia w konstruktory wystąpień klasy, który zawiera deklarację pola; statycznego pola w konstruktorze statycznym klasy, który zawiera deklarację pola. Jest on również tylko kontekstów, w których jest on prawidłowy do przekazania `readonly` pola jako `out` lub `ref` parametru.

Podjęto próbę przypisania do `readonly` pola lub przekazać je jako `out` lub `ref` parametr w innym kontekście jest to błąd czasu kompilacji.

#### <a name="using-static-readonly-fields-for-constants"></a>Za pomocą pola statycznego tylko do odczytu dla stałych

A `static readonly` pola jest przydatne, jeśli pożądane jest Symboliczna nazwa wartości stałej, ale gdy typ wartości nie jest dozwolona w `const` deklaracji, lub gdy w czasie kompilacji, w którym nie można obliczyć wartości. W przykładzie
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
`Black`, `White`, `Red`, `Green`, i `Blue` elementów członkowskich nie można zadeklarować jako `const` elementów członkowskich, ponieważ nie można obliczyć wartości w czasie kompilacji. Jednak ich zgłaszania `static readonly` zamiast tego ma znacznie taki sam skutek.

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>Przechowywanie wersji, stałe i pola statycznego tylko do odczytu

Stałe i pola tylko do odczytu mają semantykę różnych wersji binarnego. Gdy wyrażenie odwołuje się do stałej, wartość stałej są uzyskiwane w czasie kompilacji, ale gdy wyrażenie odwołuje się do pola tylko do odczytu, wartość pola nie uzyskano czasu wykonywania. Rozważmy aplikację, która składa się z dwóch oddzielnych programach:
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

`Program1` i `Program2` przestrzenie nazw oznaczają dwa programy, które są kompilowane osobno. Ponieważ `Program1.Utils.X` jest zadeklarowany jako pola statycznego tylko do odczytu, wartość danych wyjściowych przez `Console.WriteLine` instrukcja nie jest znany w czasie kompilacji, ale raczej są uzyskiwane w czasie wykonywania. W związku z tym jeśli wartość `X` zostanie zmieniony i `Program1` jest ponownie kompilowany, `Console.WriteLine` instrukcji zwróci nowy nawet jeśli wartość `Program2` nie jest ponownie kompilowana. Jednak miał `X` zostały stałą wartość `X` będą uzyskiwane w czasie `Program2` został skompilowany i spowoduje wpływu zmian w `Program1` aż `Program2` jest ponownie kompilowana.

### <a name="volatile-fields"></a>Pola nietrwałego

Gdy *field_declaration* obejmuje `volatile` są pól wprowadzonych przez deklaracja, modyfikator ***pola nietrwałego***.

Dla pól typu nieulotnego technik optymalizacji, które zmiana kolejności instrukcji może prowadzić do nieoczekiwanych i nieprzewidywalne wyniki w programach wielowątkowych, które uzyskują dostęp do pól bez synchronizacji, takim jak udostępniany przez *lock_statement*  ([Instrukcję lock](statements.md#the-lock-statement)). Optymalizacje te można wykonać przez kompilator, działającego systemu lub sprzętu. Do pola nietrwałego takich ponownego zamawiania optymalizacje są ograniczone:

*  Odczyt pola nietrwałego jest nazywany ***volatile odczytu***. Volatile odczytu ma "nabyć semantykę"; oznacza to, że zagwarantowane jest wystąpić przed wszystkie odwołania do pamięci, które występują po nim w sekwencji instrukcji.
*  Zapis pola nietrwałego jest nazywany ***volatile zapisu***. Volatile zapisu ma "release semantykę"; oznacza to, że jest gwarantowane do wykonania po wszelkie odwołania do pamięci, przed instrukcją zapisu w kolejności instrukcji.

Te ograniczenia upewnij się, że wszystkie wątki będzie przestrzegać volatile zapisów wykonywane przez inny wątek, w kolejności, w którym zostały wykonane. Odpowiadające wdrożenia nie jest wymagany zapewnienie pojedynczego całkowita kolejność volatile zapisów wyświetlanego ze wszystkich wątków wykonania. Typ pola nietrwałego musi być jedną z następujących czynności:

*  A *reference_type*.
*  Typ `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, lub` System.UIntPtr`.
*  *Enum_type* wyliczenia podstawowego typu `byte`, `sbyte`, `short`, `ushort`, `int`, lub `uint`.

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
generuje dane wyjściowe:
```
result = 143
```

W tym przykładzie metoda `Main` uruchamia nowy wątek, który uruchamia metody `Thread2`. Ta metoda przechowuje wartość w polu trwałej o nazwie `result`, następnie przechowuje `true` w pola nietrwałego `finished`. Główny wątek czeka pole `finished` należy ustawić `true`, następnie odczytuje pola `result`. Ponieważ `finished` została zadeklarowana `volatile`, wątek główny należy odczytać wartości `143` pola `result`. Jeśli pole `finished` nie została zadeklarowana `volatile`, a następnie będzie dozwolony dla magazynu do `result` mają być widoczne dla głównego wątku po sklep `finished`i dlatego dla głównego wątku, który ma zostać odczytana wartość `0` z pole `result`. Deklarowanie `finished` jako `volatile` pola uniemożliwia takich niezgodności.

### <a name="field-initialization"></a>Inicjowanie pola

Początkowa wartość pola, czy jest to pole statyczne lub pole wystąpienia, jest wartością domyślną ([wartości domyślne](variables.md#default-values)) z typem pola. Nie jest możliwe obserwowania wartości pola, zanim wystąpił ten proces inicjowania domyślnego i tym samym polem nigdy nie jest "niezainicjowanej". Przykład
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
generuje dane wyjściowe
```
b = False, i = 0
```
Ponieważ `b` i `i` są zarówno automatycznie zainicjowany do wartości domyślnych.

### <a name="variable-initializers"></a>Inicjatory zmiennej

Może zawierać deklaracji pól *variable_initializer*s. W przypadku pola statyczne inicjatory zmiennej odpowiadają instrukcji przypisania, które są wykonywane podczas inicjowania klasy. Na przykład pola, inicjatory zmiennej odpowiadają instrukcji przypisania, które są wykonywane, gdy tworzone jest wystąpienie klasy.

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
generuje dane wyjściowe
```
x = 1.4142135623731, i = 100, s = Hello
```
ponieważ przypisania do `x` występuje podczas wykonywania inicjatorów pola statyczne i przypisania do `i` i `s` występują, gdy wykonywanie inicjatorów pola wystąpienia.

Inicjowanie wartości domyślne, które są opisane w [inicjowania pola](classes.md#field-initialization) pojawia się dla wszystkich pól, takich jak pola, które mają zmienną inicjatory. W związku z tym podczas inicjowania klasy wszystkie pola statyczne w tej klasie najpierw są inicjowane do wartości domyślnych, a następnie inicjatorów pola statyczne są wykonywane w kolejności tekstową. Podobnie po utworzeniu wystąpienia klasy, wszystkie pola wystąpienia w tym wystąpieniu najpierw są inicjowane do wartości domyślnych, a następnie inicjatorów pola wystąpienia są wykonywane w kolejności tekstową.

Jest możliwe w przypadku pól statycznych zmiennych inicjatorów należy przestrzegać w ich stanie. wartość domyślna. Jest to jednak zalecane jako stylu. Przykład
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
wykazuje to zachowanie. Pomimo cykliczne definicje i b, program jest prawidłowy. Powoduje to dane wyjściowe
```
a = 1, b = 2
```
ponieważ pola statyczne `a` i `b` są inicjowane na wartość `0` (wartość domyślna dla `int`) przed ich inicjatory są wykonywane. Podczas inicjator dla `a` uruchamia wartość `b` wynosi zero, a tym samym `a` jest inicjowany do `1`. Podczas inicjator dla `b` uruchamia wartość `a` jest już `1`, a tym samym `b` jest inicjowany do `2`.

#### <a name="static-field-initialization"></a>Inicjowanie pole statyczne

Pole statyczne inicjatory zmiennej klasy odpowiadają sekwencję przypisania, które zostały wykonane w porządku tekstowych, w jakiej występują w deklaracji klasy. Jeśli Konstruktor statyczny ([konstruktorów statycznych](classes.md#static-constructors)) istnieje w klasie inicjatorów pola statycznego jest wykonywany natychmiast, przed wykonaniem tego Konstruktor statyczny. W przeciwnym razie inicjatorów pola statyczne są wykonywane w czasie zależne od implementacji przed pierwszym użyciu pole statyczne tej klasy. Przykład
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
może dawać obu dane wyjściowe:
```
Init A
Init B
1 1
```
lub dane wyjściowe:
```
Init B
Init A
1 1
```
ponieważ wykonanie `X`firmy inicjatora i `Y`w inicjatorze mogą wystąpić w obu kolejności; są one tylko ograniczone występuje przed odwołania do tych pól. Jednak w przykładzie:
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
dane wyjściowe muszą być:
```
Init B
Init A
1 1
```
ponieważ zasady podczas wykonywania konstruktory statyczne (zgodnie z definicją w [konstruktory statyczne](classes.md#static-constructors)) stanowią, że `B`firmy Konstruktor statyczny (i w związku z tym `B`firmy pole statyczne inicjatory) musi zostać uruchomiony przed `A`w konstruktorze statycznym i inicjatorów pola.

#### <a name="instance-field-initialization"></a>Inicjowanie pola wystąpienia

Inicjatorów pola zmiennej wystąpienia klasy odpowiadają sekwencję przypisania, które są wykonywane od razu po wejściu do jednej z konstruktorów wystąpienia ([inicjatory konstruktora](classes.md#constructor-initializers)) dla tej klasy. Inicjatory zmiennej są wykonywane w kolejności tekstowych, w jakiej występują w deklaracji klasy. Proces tworzenia i inicjowania wystąpienia klasy jest dokładniejszym opisem zawartym w [wystąpienia konstruktory](classes.md#instance-constructors).

Inicjator zmiennej dla pola wystąpienia nie może odwoływać się wystąpienia tworzona. Dlatego jest to błąd czasu kompilacji, aby odwołać się do `this` w inicjatorze zmiennych w postaci, w jakiej jest błąd w czasie kompilacji dla inicjatorze zmiennej można odwoływać się do dowolnego elementu członkowskiego wystąpienia za pośrednictwem *simple_name*. W przykładzie
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
Inicjator zmiennej `y` powoduje błąd w czasie kompilacji, ponieważ widok odwołuje się członkiem wystąpienia tworzona.

## <a name="methods"></a>Metody

A ***metoda*** jest elementem członkowskim, który implementuje obliczenia lub akcji, które mogą być wykonywane przez obiekt lub klasa. Metody są deklarowane przy użyciu *method_declaration*s:

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

A *method_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)) i prawidłową kombinację modyfikatory dostępu cztery ([modyfikatorach dostępu ](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `static` ([metody statyczne i wystąpienia](classes.md#static-and-instance-methods)), `virtual` ([metody wirtualnej](classes.md#virtual-methods)), `override` ([Przesłaniaj metody](classes.md#override-methods)), `sealed` ([zapieczętowany metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)), a `extern` ([Metody zewnętrznej](classes.md#external-methods)) modyfikatorów.

Deklaracja ma prawidłową kombinację modyfikatorów, jeśli spełnione są wszystkie z następujących czynności:

*  Deklaracja zawiera prawidłową kombinację modyfikatory dostępu ([modyfikatorach dostępu](classes.md#access-modifiers)).
*  Deklaracja nie ma tego samego modyfikator wiele razy.
*  Deklaracja zawiera co najwyżej jeden z następujących modyfikatorów: `static`, `virtual`, i `override`.
*  Deklaracja zawiera co najwyżej jeden z następujących modyfikatorów: `new` i `override`.
*  Jeśli deklaracja zawiera `abstract` modyfikator, a następnie deklaracji nie zawiera żadnego z następujących modyfikatorów: `static`, `virtual`, `sealed` lub `extern`.
*  Jeśli deklaracja zawiera `private` modyfikator, a następnie deklaracji nie zawiera żadnego z następujących modyfikatorów: `virtual`, `override`, lub `abstract`.
*  Jeśli deklaracja zawiera `sealed` modyfikator, a następnie deklaracji obejmuje również `override` modyfikator.
*  Jeśli deklaracja zawiera `partial` modyfikator, a następnie go nie ma żadnego z następujących modyfikatorów: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`, lub `extern`.

Metody, która ma `async` modyfikator jest funkcji asynchronicznej i zgodna z zasadami opisanymi w [funkcje asynchroniczne](classes.md#async-functions).

*Typ_wyniku* metody deklaracja określa typ wartości, obliczana i zwracany przez metodę. *Typ_wyniku* jest `void` Jeśli metoda nie zwraca wartości. Jeśli deklaracja zawiera `partial` modyfikator, a następnie zwracany typ musi być `void`.

*Member_name* Określa nazwę metody. Jeśli metoda nie jest jawną implementacją członków ([implementacji elementu członkowskiego interfejsu jawnego](interfaces.md#explicit-interface-member-implementations)), *member_name* jest po prostu *identyfikator*. Dla implementacja interfejsu jawnego członka *member_name* składa się z *interface_type* następuje "`.`" i *identyfikator*.

Opcjonalny *type_parameter_list* określa parametry typu metody ([parametry typu](classes.md#type-parameters)). Jeśli *type_parameter_list* jest określona, ta metoda jest ***metody ogólnej***. Jeśli metoda ma `extern` , modyfikator *type_parameter_list* nie może być określony.

Opcjonalny *formal_parameter_list* określa parametry metody ([parametry metody](classes.md#method-parameters)).

Opcjonalny *type_parameter_constraints_clause*s określić ograniczenia dotyczące parametrów typu poszczególnych ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) i może być określona tylko, jeśli *type_parameter_ Lista* jest również podana, i nie ma metody `override` modyfikator.

*Typ_wyniku* , a każdy z typów, do którego odwołuje się *formal_parameter_list* metody musi być co najmniej tak samo dostępna jak metoda, sama ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).

*Method_body* jest albo średnik ***treść instrukcji*** lub ***treści wyrażenia***. Treść instrukcji składa się z *bloku*, która określa instrukcji do wykonania, gdy metoda jest wywoływana. Treść wyrażenia składa się z `=>` następuje *wyrażenie* i średnikiem oraz oznacza pojedynczego wyrażenia do wykonania, gdy metoda jest wywoływana. 

Aby uzyskać `abstract` i `extern` metod, *method_body* po prostu składa się z średnikiem. Aby uzyskać `partial` metody *method_body* może składać się ze średnikiem, treści bloku lub treści wyrażenia. Inne metody *method_body* treści bloku lub treści wyrażenia.

Jeśli *method_body* składa się średnikiem, a następnie deklaracja nie może zawierać `async` modyfikator.

Nazwa, lista parametrów typu i formalnej listy parametrów metody definiowania podpis ([podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading)) metody. W szczególności podpis metody składa się z jego nazwę, liczba parametrów typu i liczbę, Modyfikatory i typy parametrów formalnych. Do tych celów wszelkie parametr typu metody, która występuje w typie formalny parametr jest identyfikowany nie według jego nazwy, ale za pomocą jego numer porządkowy pozycji na liście argumentów typu metody. Zwracany typ nie jest częścią podpis metody nie są nazwy parametrów typu lub parametrów formalnych.

Nazwa metody muszą różnić się od nazw wszystkich innych niż metod zadeklarowanych w tej samej klasy. Ponadto podpis metody muszą różnić się od podpisy wszystkich pozostałych metod zadeklarowanych w tej samej klasie i dwie metody zadeklarowanej w tej samej klasy nie może mieć podpisów, które różnią się jedynie przez `ref` i `out`.

Metoda *type_parameter*s znajdują się w zakresie *method_declaration*i może służyć do typów formularza, w tym zakresie w *Typ_wyniku*, *method_body*, i *type_parameter_constraints_clause*s, ale nie *atrybuty*.

Wszystkie parametry formalne i typ parametrów muszą mieć różne nazwy.

### <a name="method-parameters"></a>Parametry metody

Parametry metody, jeśli są zadeklarowane za pomocą metody *formal_parameter_list*.

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

Na liście parametrów formalnych składa się z jednego lub więcej rozdzielonych przecinkami parametrów, z których mogą być tylko ostatnie *parameter_array*.

A *fixed_parameter* zawiera opcjonalny zestaw *atrybuty* ([atrybuty](attributes.md)), opcjonalny `ref`, `out` lub `this` , modyfikator *typu*, *identyfikator* oraz opcjonalny *default_argument*. Każdy *fixed_parameter* deklaruje parametr danego typu o podanej nazwie. `this` Modyfikator określa metodę jako metodę rozszerzenia i jest dozwolona tylko w pierwszym parametrze metody statycznej. Metody rozszerzenia są opisane w [metody rozszerzenia](classes.md#extension-methods).

A *fixed_parameter* z *default_argument* jest znany jako ***opcjonalny parametr***, podczas gdy *fixed_parameter* bez *default_argument* jest ***wymaganego parametru***. Wymagany parametr nie może występować po opcjonalnym parametrze w *formal_parameter_list*.

A `ref` lub `out` parametr nie może mieć *default_argument*. *Wyrażenie* w *default_argument* musi mieć jedną z następujących czynności:

*  *constant_expression*
*  wyrażenie w formie `new S()` gdzie `S` jest typem wartości
*  wyrażenie w formie `default(S)` gdzie `S` jest typem wartości

*Wyrażenie* musi umożliwiać niejawną konwersję przez tożsamość lub dopuszczające wartość null konwersji na typ parametru.

Jeśli następujące parametry opcjonalne występować w deklaracji implementującej metody częściowej ([metod częściowych](classes.md#partial-methods)), jawną implementacją członków ([implementacji elementu członkowskiego interfejsu jawnego](interfaces.md#explicit-interface-member-implementations)) lub w Deklaracja pojedynczego parametru indeksatora ([indeksatory](classes.md#indexers)) kompilator powinien zawierać ostrzeżenie, ponieważ te elementy członkowskie nigdy nie może być wywoływany w sposób, który pozwala na argumenty, które można pominąć.

A *parameter_array* zawiera opcjonalny zestaw *atrybuty* ([atrybuty](attributes.md)), `params` , modyfikator *array_type*, i *identyfikator*. Tablica parametrów deklaruje jeden parametr typu danej tablicy o podanej nazwie. *Array_type* parametru tablicy musi być typem tablicy jednowymiarowej ([tablicy typów](arrays.md#array-types)). W wywołaniu metody tablicy parametrów zezwala na pojedynczy argument typu danej tablicy, należy określić lub pozwala na zero lub więcej argumentów typu elementu tablicy, należy określić. Parameter — tablice są dokładniejszym opisem zawartym w [Parameter — tablice](classes.md#parameter-arrays).

A *parameter_array* może występować po opcjonalnym parametrze, ale nie może mieć wartości domyślnej — pominięcie argumentów *parameter_array* zamiast tego spowoduje utworzenie pustej tablicy.

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

W *formal_parameter_list* dla `M`, `i` jest parametrem ref wymagane `d` jest parametrem wymaganej wartości `b`, `s`, `o` i `t` Opcjonalna wartość parametrów i `a` jest tablicą parametrów.

Deklaracja metody tworzy miejsce na oddzielnych deklaracji parametrów, typów parametrów i zmiennych lokalnych. Nazwy są wprowadzane do tego obszaru deklaracji, lista parametrów typu i formalnej listy parametrów metody oraz lokalne deklaracje zmiennych w *bloku* metody. Jest to błąd, dla dwóch członków miejsca deklaracji metody mieć taką samą nazwę. Jest to błąd dla deklaracji metody i miejsca deklaracji zmiennej lokalnej w deklaracji zagnieżdżonej przestrzeni ma zawierać elementy o takiej samej nazwie.

Wywołanie metody ([wywołań metody opisywanego](expressions.md#method-invocations)) tworzy kopię, specyficzne dla tego wywołania, parametry formalne i zmienne lokalne, metody i listą argumentów wywołania przypisuje wartości lub zmiennej odwołania do nowo utworzona parametrów formalnych. W ramach *bloku* metody parametrów formalnych mogą być przywoływane przez ich identyfikatorów w *simple_name* wyrażenia ([proste nazwy](expressions.md#simple-names)).

Istnieją cztery rodzaje parametrów formalnych:

*  Wartości parametrów, które są zadeklarowane bez żadnych modyfikatorów.
*  Parametry, które są zadeklarowane za pomocą odwołania `ref` modyfikator.
*  Parametry wyjściowe, które są zadeklarowane za pomocą `out` modyfikator.
*  Parameter — tablice, które są zadeklarowane za pomocą `params` modyfikator.

Zgodnie z opisem w [podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading), `ref` i `out` Modyfikatory stanowią część podpisu metody, ale `params` modyfikator nie jest.

#### <a name="value-parameters"></a>Wartości parametrów

Parametr zadeklarowane za pomocą modyfikatorów nie jest parametrem wartości. Wartość parametru odnosi się do zmiennej lokalnej, która pobiera jej wartość początkową z odpowiadający argument podany w wywołaniu metody.

Formalny parametr po parametru wartości odnośnego argumentu w wywołaniu metody musi być wyrażeniem, który jest niejawnie konwertowany ([niejawne konwersje](conversions.md#implicit-conversions)) do typu formalnego parametru.

Metoda może przypisać nowe wartości do parametru wartości. Takie przypisania wpływ tylko na lokalizację magazynu lokalnego reprezentowanego przez parametr wartość — nie mają wpływu na rzeczywisty argument podany w wywołaniu metody.

#### <a name="reference-parameters"></a>Parametry odwołania

Parametr zadeklarowana za pomocą `ref` modyfikator jest parametr przekazany przez odwołanie. W odróżnieniu od wartość parametru parametr przekazany przez odwołanie, nie powoduje utworzenia nowej lokalizacji magazynu. Zamiast tego parametru odwołania reprezentuje tę samą lokalizację magazynu zmienna, podane jako argument w wywołaniu metody.

Gdy parametr formalny jest parametr przekazany przez odwołanie, odpowiadający argument w wywołaniu metody musi zawierać słowa kluczowego `ref` następuje *variable_reference* ([dokładne zasady ustalania asercja określonego przydziału](variables.md#precise-rules-for-determining-definite-assignment)) z tego samego typu co parametr formalny. Zmienna musi być zdecydowanie przypisana, zanim może być przekazywany jako parametr przekazany przez odwołanie.

W metodzie parametr przekazany przez odwołanie jest zawsze traktowany jako zdecydowanie przypisany.

Metoda zadeklarowany jako iterator ([Iteratory](classes.md#iterators)) nie może mieć parametrów odwołania.

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
generuje dane wyjściowe
```
i = 2, j = 1
```

Dla wywołania elementu `Swap` w `Main`, `x` reprezentuje `i` i `y` reprezentuje `j`. W związku z tym, wywołanie powoduje zamianę wartości `i` i `j`.

W metodzie, która pobiera parametry odwołania, istnieje możliwość, że wiele nazw do reprezentowania tej samej lokalizacji magazynu. W przykładzie
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
wywołanie `F` w `G` odwołuje się do `s` dla obu `a` i `b`. W związku z tym, dla tego wywołania, nazwy `s`, `a`, i `b` wszystkie odnoszą się do tej samej lokalizacji magazynu, a następnie wszystkie przypisania trzy Zmodyfikuj pole wystąpienia `s`.

#### <a name="output-parameters"></a>Parametry wyjściowe

Parametr zadeklarowana za pomocą `out` modyfikator jest parametrem wyjściowym. Podobnie jak parametr przekazany przez odwołanie, parametr wyjściowy nie powoduje utworzenia nowej lokalizacji magazynu. Zamiast tego parametru output reprezentuje tę samą lokalizację magazynu zmienna, podane jako argument w wywołaniu metody.

Gdy parametr formalny jest parametr wyjściowy, odpowiadający argument w wywołaniu metody musi składać się z słowa kluczowego `out` następuje *variable_reference* ([dokładne zasady ustalania asercja określonego przydziału](variables.md#precise-rules-for-determining-definite-assignment)) z tego samego typu co parametr formalny. Zmienna musi nie być zdecydowanie przypisana może być przekazywany jako parametr wyjściowy, ale następujące wywołania, gdy zmienna została przekazana jako parametr wyjściowy, zmienna jest uznawany za zdecydowanie przypisany.

W metodzie, podobnie jak w przypadku lokalnej zmiennej, parametr wyjściowy jest początkowo uznawany za nieprzypisanych i musi być zdecydowanie przypisana, zanim jego wartość jest używana.

Każdy parametr danych wyjściowych metody musi być zdecydowanie przypisana, przed powrotem z metody.

Metody zadeklarowane jako metody częściowej ([metod częściowych](classes.md#partial-methods)) lub iteratora ([Iteratory](classes.md#iterators)) nie można wyprowadzić parametrów.

Parametry wyjściowe są zwykle używane w metodach, które wywołują z wieloma wartościami zwracanymi. Na przykład:
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
```
c:\Windows\System\
hello.txt
```

Należy pamiętać, że `dir` i `name` zmienne mogą być nieprzypisane przed przekazaniem ich do `SplitPath`, i że są uwzględniane zdecydowanie przypisany następujący po wywołaniu.

#### <a name="parameter-arrays"></a>Parameter — tablice

Parametr zadeklarowana za pomocą `params` modyfikator jest tablicą parametrów. Jeśli formalnej listy parametrów zawiera tablicy parametrów, musi być ostatnim parametrem na liście, a musi być typu tablicy jednowymiarowej. Na przykład typy `string[]` i `string[][]` może być używany jako typ tablicy parametrów, ale typ `string[,]` nie jest możliwe. Nie jest możliwe połączyć `params` modyfikator z modyfikatorami `ref` i `out`.

Tablica parametrów zezwala na argumenty, które można określić w jeden z dwóch sposobów, w wywołaniu metody:

*  Argument podany dla tablicy parametrów może być wyrażeniem pojedynczym, który jest niejawnie konwertowany ([niejawne konwersje](conversions.md#implicit-conversions)) na typ tablicy parametrów. W tym przypadku tablicy parametrów funkcjonuje dokładnie parametru wartości.
*  Alternatywnie określić wywołanie zero lub więcej argumentów dla tablicy parametrów, w którym każdy argument jest wyrażeniem, które jest niejawnie konwertowany ([niejawne konwersje](conversions.md#implicit-conversions)) na typ elementu tablicy parametrów. W takim przypadku wywołanie tworzy wystąpienie typu parametru tablicy o długości odpowiadającej liczbę argumentów, inicjuje elementy wystąpienia tablicy wartościami podany argument i używa wystąpienia nowo utworzona tablica jako rzeczywisty argument.

Z wyjątkiem umożliwiając zmienną liczbę argumentów w wywołaniu, tablicy parametrów odpowiada dokładnie parametru wartości ([wartości parametrów](classes.md#value-parameters)) tego samego typu.

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
generuje dane wyjściowe
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

Pierwsze wywołanie `F` po prostu przekazuje tablicy `a` jako parametru wartości. Drugie wywołanie `F` automatycznie tworzy element czterech `int[]` z wartości danego elementu i przebiegi, które wystąpienie jako parametru wartości w tablicy. Podobnie, trzeci wywołanie `F` tworzy zero element `int[]` i przekazanie danego wystąpienia jako parametru wartości. Drugi i trzeci wywołania są dokładnie odpowiednikiem pisania:
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

Podczas rozpoznawania przeciążenia metody z tablicą parametrów mogą być stosowane, w postaci normalnym lub w postaci rozwiniętej ([dotyczy funkcja składowa](expressions.md#applicable-function-member)). Rozwinięty formularza metody jest dostępna tylko wtedy, gdy jest to normalne formularza metody nie ma zastosowania i tylko wtedy, gdy odpowiedniej metody o tej samej sygnaturze, rozwinięty formularza nie jest już zadeklarowany w tym samym typie.

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
generuje dane wyjściowe
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

W tym przykładzie dwa możliwe formy rozwiniętej metody z tablicą parametrów znajdują się już w klasie jako metody regularnego. Te formularze rozwinięty w związku z tym nie są uwzględniane podczas rozpoznawania przeciążenia i wywołań metod pierwszy i trzeci związku z tym Wybierz metody regularnego. Gdy klasa deklaruje metodę z tablicą parametrów, nie jest niczym niezwykłym również obejmować niektóre spośród rozwiniętej forms jako metody regularnego. W ten sposób można uniknąć alokacji tablicy wystąpienie, które występuje, gdy w formie metody z tablicą parametrów jest wywoływana.

Jeśli typ tablicy parametrów to `object[]`, potencjalną niejednoznacznością pojawia się między normalnym metody expended w formularzach i dla pojedynczego `object` parametru. Przyczyny niejasności jest fakt, że `object[]` sama jest niejawnie konwertowany na typ `object`. Niejednoznaczności przedstawia nie ma problemu, ponieważ może zostać rozpoznana przez wstawienie rzutowania, jeśli to konieczne.

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
generuje dane wyjściowe
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

W pierwszym i ostatnim wywołań `F`, normalne formie `F` ma zastosowanie, ponieważ istnieje niejawna konwersja z typu argumentu typu parametru (oba są typu `object[]`). W związku z tym, funkcja rozpoznawania przeciążeń wybiera normalne formie `F`, a argument jest przekazywany jako parametr wartość regularne. W drugi i trzeci wywołań, normalne formie `F` nie ma zastosowania, ponieważ nie istnieje niejawna konwersja z typu argumentu z typem parametru (typ `object` nie może być niejawnie konwertowany na typ `object[]`). Jednak rozszerzonej postaci `F` jest stosowana, dzięki czemu jest ono zaznaczone w przeciążeniu rozdzielczości. W wyniku element jeden `object[]` jest tworzony przez wywołanie, a pojedynczy element tablicy jest inicjowany z wartością podany argument (która sama jest odwołaniem do `object[]`).

### <a name="static-and-instance-methods"></a>Metody statyczne i wystąpienia

Po deklaracji metody zawiera `static` modyfikator, że metoda jest określany jako metoda statyczna. Gdy nie `static` modyfikator, metoda jest określany jako metodę wystąpienia.

Metoda statyczna nie będzie działać na określonym wystąpieniu i jest to błąd kompilacji, aby odwołać się do `this` w metodzie statycznej.

Metoda wystąpienia działa danego wystąpienia klasy, i to wystąpienie może być dostępna jako `this` ([dostęp](expressions.md#this-access)).

Gdy odwołuje się do metody *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `E.M`, jeśli `M` jest statycznej metody `E` musi oznaczają typu zawierającego `M`i jeśli `M` jest metodą wystąpienia `E` musi oznaczają wystąpienia typu zawierającego `M`.

Różnice statyczne elementy członkowskie wystąpień zostały one omówione w dalszych [statyczna i wystąpienia elementów członkowskich](classes.md#static-and-instance-members).

### <a name="virtual-methods"></a>Metody wirtualne

Po deklaracji metody wystąpienia obejmuje `virtual` modyfikator, że metoda jest określany jako metodę wirtualną. Gdy nie `virtual` modyfikator, metoda jest określany jako metody niewirtualnej.

Implementacja metody niewirtualnej jest niezmienny: Implementacja jest taki sam, czy metoda jest wywoływana w wystąpieniu klasy, w którym jest zdeklarowana lub wystąpienie klasy pochodnej. Z kolei implementację metody wirtualnej może zostać zastąpiony przez klasy pochodne. Proces zastępowanie implementacji dziedziczoną metodę wirtualną jest znany jako ***zastępowanie*** tej metody ([Przesłaniaj metody](classes.md#override-methods)).

W wywołaniu metody wirtualnej ***typu run-time*** wystąpienia, dla którego tego wywołania ma miejsce określa rzeczywista implementacja metody do wywołania. W wywołaniu metody niewirtualnej ***typów w czasie kompilacji*** wystąpienia jest czynnikiem decydującym. W dokładne warunki, kiedy metodę o nazwie `N` jest wywoływana z listą argumentów `A` w wystąpieniu typu kompilacji `C` i typu run-time `R` (gdzie `R` jest `C` lub klasa pochodnej z `C`), wywołanie jest przetwarzana w następujący sposób:

*  Po pierwsze, funkcja rozpoznawania przeciążeń jest stosowany do `C`, `N`, i `A`, aby wybrać określonej metody `M` z zestawu metod zadeklarowanych w i dziedziczone przez `C`. Jest to opisane w [wywołań metody opisywanego](expressions.md#method-invocations).
*  Następnie, jeśli `M` jest metodą niewirtualną `M` zostanie wywołana.
*  W przeciwnym razie `M` jest metodą wirtualną i wykonania najbardziej pochodnej `M` w odniesieniu do `R` zostanie wywołana.

Dla każdej wirtualnej metody zadeklarowanej w lub dziedziczone przez klasy, istnieje ***najbardziej pochodnego implementacji*** metody w odniesieniu do tej klasy. Najbardziej pochodnej metody wirtualnej `M` względem klasy `R` jest określany w następujący sposób:

*  Jeśli `R` zawiera wprowadzenie do `virtual` deklaracji `M`, ten parametr jest wykonania najbardziej pochodnej `M`.
*  W przeciwnym razie, jeśli `R` zawiera `override` z `M`, ten parametr jest wykonania najbardziej pochodnej `M`.
*  W przeciwnym razie najbardziej pochodnej implementacji `M` w odniesieniu do `R` jest taka sama jak wykonania najbardziej pochodnej `M` względem bezpośrednia klasa podstawowa `R`.

Poniższy przykład ilustruje różnice między metodami wirtualnej i niewirtualnej:
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

W tym przykładzie `A` wprowadza metody niewirtualnej `F` i metodę wirtualną `G`. Klasa `B` wprowadzono nowe metody niewirtualnej `F`, dlatego ukrywanie odziedziczonych `F`i zastępuje również dziedziczonej metody `G`. Przykład generuje dane wyjściowe:
```
A.F
B.F
B.G
B.G
```

Należy zauważyć, że instrukcja `a.G()` wywołuje `B.G`, a nie `A.G`. Jest to spowodowane środowiska wykonawczego typu wystąpienia (czyli `B`), nie kompilacji Typ wystąpienia (czyli `A`), określa rzeczywista implementacja metody do wywołania.

Ponieważ metody mogą ukryć metody dziedziczone, jest możliwe, klasa może zawierać kilka metod wirtualnych za pomocą tego samego podpisu. To nie przedstawi wystąpił problem niejednoznaczności, ponieważ wszystkie oprócz najbardziej pochodna metoda są ukryte. W przykładzie
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
`C` i `D` klasy zawierają dwie metody wirtualnej z tym samym podpisie: Jeden wprowadzone przez `A` a wynikające z `C`. Metoda wprowadzona przez `C` ukrywa metodę odziedziczone `A`. W związku z tym, deklaracji zastąpienie `D` zastępuje metodę wynikające z `C`, i nie jest możliwe dla `D` Aby przesłonić metodę wynikające z `A`. Przykład generuje dane wyjściowe:
```
B.F
B.F
D.F
D.F
```

Należy pamiętać, że jest możliwe do wywołania metody wirtualnej ukryte, uzyskując dostęp do wystąpienia `D` za pośrednictwem mniej pochodne typu, w którym metoda nie jest ukryty.

### <a name="override-methods"></a>Przesłaniaj metody

Po deklaracji metody wystąpienia obejmuje `override` modyfikator, metoda jest nazywany ***przesłonić metodę***. To metoda przesłonięcia zastępuje dziedziczoną metodę wirtualną przy użyciu tego samego podpisu. Dlatego deklaracja metody wirtualnej wprowadzono nowe metody, deklaracji metody zastąpienie specjalizuje się istniejących dziedziczoną metodę wirtualną, podając nową metodę implementacji tej metody.

Metoda zastąpione `override` deklaracji jest znany jako ***przesłonięcia metody podstawowej***. Zastąpienie metody `M` zadeklarowana w klasie `C`, zastąpiona metoda podstawowa jest określana przez badanie każdego typu klasy bazowej `C`, począwszy od rodzaju bezpośrednią klasą bazową `C` i kontynuowanie z każdym kolejnych Typ bezpośrednią klasą bazową, dopóki w typie danej klasy bazowej co najmniej jeden z dostępnych metod jest znajduje się który ma taką samą sygnaturę jak `M` po zastąpienie argumentów typu. Potrzeby lokalizowanie przesłonięte metody podstawowej metody jest traktowane jako niedostępny w `public`, jeśli jest on `protected`, jeśli jest on `protected internal`, lub jeśli jest `internal` i zadeklarowany w tym samym programie jako `C`.

Błąd kompilacji występuje, o ile spełnione dla deklaracji zastąpienie są wszystkie z następujących czynności:

*  Zastąpione metody podstawowej, można znaleźć, zgodnie z powyższym opisem.
*  Ma dokładnie jedno takie podstawowy przeciążonej. To ograniczenie obowiązuje tylko wtedy, gdy typ klasy bazowej jest skonstruowanego typu, w którym zastąpienie argumentów typu sprawia, że podpis metody dwa takie same.
*  Zastąpione metody podstawowej jest abstrakcyjny wirtualne, lub metoda musi zostać zastąpiona. Innymi słowy przesłonięte metody podstawowej, nie może być statyczne lub -virtual.
*  Zastąpione metody podstawowej nie jest metodą zapieczętowany.
*  Metoda przesłaniająca i zastąpione metody podstawowej mają taki sam typ zwracany.
*  Deklaracja zastąpienia i przesłonięte metody podstawowej mają ten sam deklarowana dostępność metody. Innymi słowy deklaracja override nie można zmienić dostępność metody wirtualnej. Jednak jeśli zastąpiona metoda podstawowa jest chronionych wewnętrznych, a zostanie ona zadeklarowana w innym zestawie, od deklarowanej zestawu zawierającego metodę przesłaniającą, a następnie metodę przesłaniającą dostępności muszą być chronione.
*  Deklaracja zastąpienie nie określono typu — parametr ograniczenia — klauzule. Zamiast tego ograniczenia są dziedziczone z przesłonięte metody podstawowej. Należy pamiętać, że ograniczenia, które są parametrów typu w metodzie zgodnym z przesłoniętą mogą być zastąpione przez argumenty typu w ograniczeniu dziedziczone. Może to prowadzić do ograniczenia, które są niedozwolone, gdy określony jawnie, takie jak typy wartości lub typach zapieczętowanych.

W poniższym przykładzie pokazano, jak działają zasady nadrzędne dla klas ogólnych:
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

Deklaracja zastąpienie mogą uzyskiwać dostęp do przesłonięte metody podstawowej, za pomocą *base_access* ([podstawowa dostępu](expressions.md#base-access)). W przykładzie
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
`base.PrintFields()` wywołania w `B` wywołuje `PrintFields` metody zadeklarowane w `A`. A *base_access* wyłącza mechanizm wywołania wirtualnego i po prostu traktuje metody podstawowej jako metody niewirtualnej. Było wywołanie `B` zostały napisane `((A)this).PrintFields()`, jak rekursywnie wywołania `PrintFields` metody zadeklarowane w `B`, nie jest zadeklarowana w `A`, ponieważ `PrintFields` jest wirtualny i typów w czasie wykonywania `((A)this)` jest `B`.

Tylko Jeśli dołączysz `override` może modyfikator metody zastępują innej metody. We wszystkich innych przypadkach metody z taką samą sygnaturę jak dziedziczonej metody po prostu ukrywa dziedziczonej metody. W przykładzie
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
`F` method in Class metoda `B` nie obejmuje `override` modyfikator i dlatego nie powoduje zastąpienia `F` method in Class metoda `A`. Zamiast `F` method in Class metoda `B` ukrywa metodę w `A`, i ostrzeżenie jest zgłaszane, ponieważ nie zawiera deklarację `new` modyfikator.

W przykładzie
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
`F` method in Class metoda `B` ukrywa wirtualnego `F` metody dziedziczone z `A`. Ponieważ nowe `F` w `B` ma dostęp do prywatnych, jej zakres obejmuje tylko treści klasy `B` i nie obejmuje `C`. Dlatego deklaracja `F` w `C` jest dozwolone, Zastąp `F` odziedziczone `A`.

### <a name="sealed-methods"></a>Metody zapieczętowane

Po deklaracji metody wystąpienia obejmuje `sealed` modyfikator, że metoda jest nazywany ***zapieczętowany metoda***. Jeśli zawiera deklarację metody wystąpienia `sealed` modyfikator, musi również zawierać `override` modyfikator. Korzystanie z `sealed` modyfikator uniemożliwia dalsze zastąpienie metody klasy pochodnej.

W przykładzie
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
Klasa `B` zawiera dwa Przesłaniaj metody: `F` metody, która ma `sealed` modyfikator i `G` metody, która nie ma. `B`używania zapieczętowanego `modifier` zapobiega `C` możliwość dalszego przesłaniania `F`.

### <a name="abstract-methods"></a>Metody abstrakcyjne

Po deklaracji metody wystąpienia obejmuje `abstract` modyfikator, że metoda jest nazywany ***metody abstrakcyjnej***. Mimo że metodę abstrakcyjną niejawnie jest również metodę wirtualną, nie może on zawierać modyfikator `virtual`.

Deklaracja metody abstrakcyjnej wprowadzono nowe metody wirtualnej, ale nie zapewnia implementacja tej metody. Zamiast tego nieabstrakcyjnej klasy pochodne są wymagane do zapewnienia ich własnych implementacji przez zastąpienie tej metody. Ponieważ metodę abstrakcyjną nie zapewnia żadnych rzeczywistą implementację *method_body* metody abstrakcyjnej po prostu składa się średnikiem.

Deklaracje metody abstrakcyjne są dozwolone tylko w klasy abstrakcyjne ([klasy abstrakcyjne](classes.md#abstract-classes)).

W przykładzie
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
`Shape` klasy definiuje abstrakcyjny pojęcie obiekt kształt geometryczny, za pomocą którego można malować sam. `Paint` Metoda jest abstrakcyjna, ponieważ nie istnieje żadne istotne Domyślna implementacja. `Ellipse` i `Box` klasy są konkretne `Shape` implementacji. Ponieważ te klasy nieabstrakcyjnej, są wymagane do zastąpienia `Paint` metody i Podaj rzeczywistą implementację.

Jest to błąd czasu kompilacji dla *base_access* ([podstawowa dostępu](expressions.md#base-access)) można odwoływać się do metody abstrakcyjnej. W przykładzie
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
Błąd kompilacji jest zgłaszany, `base.F()` wywołania ponieważ widok odwołuje się metodę abstrakcyjną.

Deklaracji metoda abstrakcyjna może zastąpić metodę wirtualną. Zapewnia to klasy abstrakcyjnej wymusić ponowną implementację metody w klasach pochodnych i powoduje, że oryginalnej implementacji metody. W przykładzie
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
Klasa `A` deklaruje metodę wirtualną klasy `B` przesłonięcia tej metody za pomocą metody abstrakcyjnej, a klasa `C` zastępuje metodę abstrakcyjną, aby podać własną implementację.

### <a name="external-methods"></a>Metody zewnętrznej

Po deklaracji metody zawiera `extern` modyfikator, że metoda jest nazywany ***metody zewnętrznej***. Metody zewnętrzne są implementowane zewnętrznie, zwykle korzystając z języka innego niż C#. Ponieważ deklaracja metody zewnętrznej zapewnia nie rzeczywistego wykonania *method_body* metody zewnętrznej po prostu składa się średnikiem. Metody zewnętrznej nie może być ogólny.

`extern` Modyfikator jest zwykle używany w połączeniu z `DllImport` atrybutu ([współdziałanie ze składnikami COM i Win32](attributes.md#interoperation-with-com-and-win32-components)), dzięki czemu zewnętrznych metody implementowane przez biblioteki dll (biblioteki dll). Środowisko wykonawcze może obsługiwać inne mechanizmy, według której można podać implementacje metod zewnętrznych.

Kiedy zawiera metody zewnętrznej `DllImport` atrybutu deklaracji metody należy również uwzględnić `static` modyfikator. W tym przykładzie pokazano użycie `extern` modyfikator i `DllImport` atrybutu:
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

Po deklaracji metody zawiera `partial` modyfikator, że metoda jest nazywany ***metody częściowej***. Metody częściowe mogą być deklarowane tylko jako członkowie typów częściowych ([typów częściowych](classes.md#partial-types)) i podlegają kilku ograniczeniom. Metody częściowe są opisane w [metod częściowych](classes.md#partial-methods).

### <a name="extension-methods"></a>Metody rozszerzenia

Gdy pierwszy parametr metody zawiera `this` modyfikator, że metoda jest nazywany ***— metoda rozszerzenia***. Metody rozszerzenia mogą być deklarowane tylko w nieogólnej, -nested klas statycznych. Pierwszy parametr metody rozszerzenia może mieć inny niż brak modyfikatorów `this`, a typ parametru nie może być typem wskaźnika.

Oto przykład klasy statycznej, która deklaruje dwie metody rozszerzające:
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

Metoda rozszerzenia jest regularne metody statycznej. Ponadto w przypadku, gdy jego otaczającej klasy statycznej znajduje się w zakresie, metoda rozszerzenia może być wywoływany przy użyciu składni wywołania metody wystąpienia ([wywołań metod rozszerzenia](expressions.md#extension-method-invocations)), za pomocą wyrażenia odbiornika jako pierwszy argument.

Następujący program używa metody rozszerzenia zadeklarowanej powyżej:
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

`Slice` Metoda jest dostępna na `string[]`i `ToInt32` metoda jest dostępna na `string`, ponieważ zostały zadeklarowane jako metody rozszerzenia. Znaczenie program jest taka sama jak poniższe, przy użyciu wywołania zwykłej statycznej metody:
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

*Method_body* metody deklaracji składa się z treści bloku, treści wyrażenia lub średnikami.

***Wyniku typu*** metody jest `void` Jeśli typ zwracany jest `void`, lub jeśli metoda jest asynchroniczne, a typem zwracanym jest `System.Threading.Tasks.Task`. W przeciwnym razie typ wyniku metody async nie jest typem zwracanym, a typ wyniku metody asynchronicznej, z typem zwracanym `System.Threading.Tasks.Task<T>` jest `T`.

Gdy metoda ma `void` wyniku typu i treści bloku `return` instrukcji ([instrukcji return](statements.md#the-return-statement)) w bloku nie można określić wyrażenie. Jeśli wykonanie bloku to metoda void zakończy się normalnie (czyli kontrolę nad przepływów poza koniec treści metody), że metoda po prostu wraca do bieżącego.
    
Gdy metoda ma `void` wynik i treści wyrażenia wyrażenie `E` musi być *statement_expression*, oraz treść jest równoznaczny z treści bloku, w postaci `{ E; }`.
    
Gdy metoda ma typ inny niż void wynik i blok treści, każdy `return` instrukcji w bloku, należy określić wyrażenie, które jest niejawnie konwertowany na typ wyniku. Punkt końcowy treści bloku metody, zwracając wartość nie może być dostępny. Innymi słowy w metodzie zwracanie wartości z treści bloku, formant nie jest dozwolona przepływ poza koniec treści metody.
    
Metoda ma typ inny niż void wynik i treści wyrażenia, wyrażenie musi być niejawnie konwertowane na typ wyniku, gdy treść jest równoznaczny z treści bloku, w postaci `{ return E; }`.
    
W przykładzie
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
Zwraca wartość `F` metoda nie powoduje błąd w czasie kompilacji, ponieważ może przepływ sterowania poza koniec treści metody. `G` i `H` metody są poprawne, dlatego wszystkie możliwe wykonania ścieżki w instrukcji return, która określa wartość zwracaną. `I` Metody jest poprawna, ponieważ jego treść jest odpowiednikiem blok instrukcji przy użyciu tylko jednej instrukcji return w nim.

### <a name="method-overloading"></a>Przeciążenie metody

Zasad rozpoznawania przeciążenia metody są opisane w [wnioskowanie o typie](expressions.md#type-inference).

## <a name="properties"></a>Właściwości

A ***właściwość*** jest elementem członkowskim, który zapewnia dostęp do właściwości obiektu lub klasy. Przykłady właściwości obejmują długość ciągu, rozmiar czcionki, podpis okna, nazwę klienta i tak dalej. Właściwości to naturalne rozszerzenie pola — nazwanych elementów członkowskich skojarzonych typów i składnia do uzyskiwania dostępu do pola i właściwości jest taka sama. Jednak w przeciwieństwie do pola, właściwości określa lokalizacji przechowywania. Właściwości mają ***Akcesory*** określające instrukcji do wykonania, gdy ich wartości są odczytu lub zapisu. Właściwości związku z tym mechanizm kojarzenia akcji z odczytu i zapisu obiektu właściwości. Ponadto pozwalają takie atrybuty, które ma zostać obliczony.

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

A *property_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)) i prawidłową kombinację modyfikatory dostępu cztery ([modyfikatorach dostępu ](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `static` ([metody statyczne i wystąpienia](classes.md#static-and-instance-methods)), `virtual` ([metody wirtualnej](classes.md#virtual-methods)), `override` ([Przesłaniaj metody](classes.md#override-methods)), `sealed` ([zapieczętowany metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)), a `extern` ([Metody zewnętrznej](classes.md#external-methods)) modyfikatorów.

Deklaracje właściwości podlegają te same reguły jako deklaracje metody ([metody](classes.md#methods)) w odniesieniu do ważnych kombinacji modyfikatorów.

*Typu* właściwości deklaracja określa typ właściwości wprowadzonych przez tę deklarację i *member_name* Określa nazwę właściwości. Chyba że właściwość jest jawną implementacją członków, *member_name* jest po prostu *identyfikator*. Aby uzyskać jawną implementacją członków ([implementacji elementu członkowskiego interfejsu jawnego](interfaces.md#explicit-interface-member-implementations)), *member_name* składa się z *interface_type* następuje " `.`"i *identyfikator*.

*Typu* właściwości musi być co najmniej tak samo dostępna jak samej właściwości ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).

A *property_body* może albo składają się z ***treści metody dostępu*** lub ***treści wyrażenia***. W treści metody dostępu *accessor_declarations*, musi być ujęta w "`{`"i"`}`" tokenów, Zadeklaruj metody dostępu ([Akcesory](classes.md#accessors)) właściwości. Metody dostępu Określ instrukcji wykonywalnych powiązane z odczytywaniem i zapisywania właściwości.

Treść wyrażenia składające się z `=>` następuje *wyrażenie* `E` , a dokładnie odpowiada na treść instrukcji średnikiem `{ get { return E; } }`i w związku z tym należy używać tylko do określenia tylko metod pobierających właściwości, których wynikiem metody pobierającej jest nadawana przez jedno wyrażenie.

A *property_initializer* mogą być przydzielane tylko dla automatycznie implementowanej właściwości ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)) i powoduje, że Inicjalizacja odpowiedniego pola takich właściwości z wartością podane przez *wyrażenie*.

Mimo że składnia służąca do uzyskiwania dostępu do właściwości jest taka sama jak dla pola, właściwości nie jest sklasyfikowany jako zmienną. W związku z tym, nie jest możliwe do przekazania właściwość jako `ref` lub `out` argumentu.

Jeśli deklaracja właściwości zawiera `extern` modyfikator, właściwość jest nazywany ***właściwość external***. Ponieważ deklaracja właściwości zewnętrznych zapewnia nie rzeczywistą implementację każdego z jego *accessor_declarations* składa się średnikiem.

### <a name="static-and-instance-properties"></a>Właściwości statyczne i wystąpienia

Jeśli deklaracja właściwości zawiera `static` modyfikator, właściwość jest nazywany ***właściwość statyczna***. Gdy nie `static` modyfikator, jest określany jako właściwość ***wystąpienia właściwości***.

Właściwość statyczna nie jest skojarzony z określonym wystąpieniem i istnieje błąd w czasie kompilacji do odwoływania się do `this` w metodach dostępu właściwości statycznej.

Właściwość wystąpienia jest skojarzona z danego wystąpienia klasy, a to wystąpienie może być dostępna jako `this` ([dostęp](expressions.md#this-access)) w metodach dostępu właściwości.

Gdy właściwość mowa w punkcie *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `E.M`, jeśli `M` jest właściwość statyczna `E` musi oznaczają typu zawierającego `M`i jeśli `M` jest właściwością wystąpienia E musi oznaczają wystąpienia typu zawierającego `M`.

Różnice statyczne elementy członkowskie wystąpień zostały one omówione w dalszych [statyczna i wystąpienia elementów członkowskich](classes.md#static-and-instance-members).

### <a name="accessors"></a>Metod dostępu

*Accessor_declarations* właściwość Podaj instrukcje wykonywalne skojarzone z odczytywaniem i zapisywaniem tej właściwości.

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

Deklaracje metody dostępu składają się z *get_accessor_declaration*, *set_accessor_declaration*, lub obu. Każda deklaracja dostępu składa się z tokenem `get` lub `set` następuje opcjonalny *accessor_modifier* i *accessor_body*.

Korzystanie z *accessor_modifier*s podlega następującym ograniczeniom:

*  *Accessor_modifier* nie można używać w interfejsie lub jawną implementacją członków.
*  Dla właściwości lub indeksatora, który nie ma `override` , modyfikator *accessor_modifier* jest dozwolona tylko wtedy, gdy właściwość lub indeksator zarówno `get` i `set` metody dostępu i następnie jest dozwolona tylko w jednym z tych metody dostępu.
*  Dla właściwości lub indeksatora, który zawiera `override` , modyfikator dostępu musi odpowiadać *accessor_modifier*(jeśli istnieje) zastąpieniu metody dostępu.
*  *Accessor_modifier* musi deklarować ułatwień dostępu, który jest ściśle bardziej restrykcyjny niż deklarowana dostępność metody, właściwości lub indeksatora, sam. Aby była precyzyjna:
   * Jeśli właściwość lub indeksator ma deklarowana dostępność metody `public`, *accessor_modifier* może być `protected internal`, `internal`, `protected`, lub `private`.
   * Jeśli właściwość lub indeksator ma deklarowana dostępność metody `protected internal`, *accessor_modifier* może być `internal`, `protected`, lub `private`.
   * Jeśli właściwość lub indeksator ma deklarowana dostępność metody `internal` lub `protected`, *accessor_modifier* musi być `private`.
   * Jeśli właściwość lub indeksator ma deklarowana dostępność metody `private`, nie *accessor_modifier* mogą być używane.

Aby uzyskać `abstract` i `extern` właściwości *accessor_body* dla każdego dostępu określony jest po prostu średnikiem. Właściwość nieabstrakcyjnej, bez extern może mieć każdy *accessor_body* się średnikiem, w którym to przypadku jest ***automatycznie implementowana właściwość*** ([automatycznie implementowane właściwości ](classes.md#automatically-implemented-properties)). Automatycznie implementowanej właściwości musi mieć co najmniej jeden metody dostępu get. Dla metod dostępu wszystkich innych nieabstrakcyjnej, bez extern właściwości *accessor_body* jest *bloku* określający instrukcji do wykonania, gdy zostanie wywołana odpowiedniej metody dostępu.

A `get` akcesor odnosi się do metody bez parametrów, z wartością zwracaną z typem właściwości. Z wyjątkiem elementem docelowym przypisania, gdy właściwość jest przywoływany w wyrażeniu, `get` metody dostępu właściwości jest wywoływana w celu obliczenia wartości właściwości ([wartości wyrażeń](expressions.md#values-of-expressions)). Treść `get` metody dostępu muszą być zgodne z regułami dotyczącymi zwracanie wartości metod opisanych w [treści metody](classes.md#method-body). W szczególności wszystkich `return` instrukcji w treści `get` dostępu należy określić wyrażenie, które jest niejawnie konwertowany na typ właściwości. Ponadto punkt końcowy `get` akcesor nie może być dostępny.

A `set` akcesor odnosi się do metody z parametrem typu właściwości pojedynczej wartości i `void` typ zwracany. Niejawny parametr `set` zawsze nosi nazwę metody dostępu `value`. Gdy właściwość jest określany jako elementem docelowym przypisania ([operatory przypisania](expressions.md#assignment-operators)), lub jako argument operacji `++` lub `--` ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators), [ Przedrostkowe i operatory dekrementacji](expressions.md#prefix-increment-and-decrement-operators)), `set` akcesor zostanie wywołana z nieprawidłowym argumentem (której wartość jest to, że po prawej stronie przypisania lub operand `++` lub `--` operator), udostępnia nową wartość ([przypisanie proste](expressions.md#simple-assignment)). Treść `set` metody dostępu muszą być zgodne z regułami dotyczącymi `void` metod opisanych w [treści metody](classes.md#method-body). W szczególności `return` instrukcji w `set` treści metody dostępu nie można określić wyrażenie. Ponieważ `set` akcesor niejawnie określono parametr o nazwie `value`, jest to błąd czasu kompilacji lokalnej deklaracji zmiennej czy stałej w `set` nazwy metody dostępu.

Na podstawie obecności lub braku `get` i `set` metod dostępu, a właściwość jest klasyfikowany w następujący sposób:

*  Właściwość, która obejmuje zarówno `get` metody dostępu i `set` metody dostępu jest określany jako ***odczytu i zapisu*** właściwości.
*  Właściwość, która ma tylko `get` metody dostępu jest określany jako ***tylko do odczytu*** właściwości. Jest to błąd czasu kompilacji, dla właściwości tylko do odczytu, aby być elementem docelowym przypisania.
*  Właściwość, która ma tylko `set` metody dostępu jest określany jako ***tylko do zapisu*** właściwości. Z wyjątkiem jako element docelowy przypisania, jest błąd kompilacji, aby odwoływać się do właściwości tylko do zapisu w wyrażeniu.

W przykładzie
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
`Button` kontroli deklaruje publiczny `Caption` właściwości. `get` Akcesor `Caption` właściwość zwraca ciąg, przechowywane w prywatnych `caption` pola. `set` Akcesor sprawdza, czy nowa wartość jest inna niż bieżąca wartość, a jeśli tak, są przechowywane nową wartość i odświeża formantu. Właściwości często oparte na wzorcu powyżej: `get` Dostępu po prostu zwraca wartość przechowywaną w pola prywatnego, a `set` akcesor modyfikuje tego pola prywatnego, a następnie wykonuje żadnych dodatkowych akcji, wymagane do pełnej aktualizacji stanu obiektu.

Biorąc pod uwagę `Button` klasy powyżej, Oto przykład użycia `Caption` właściwości:
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

W tym miejscu `set` metody dostępu jest wywoływany przez przypisywanie wartości do właściwości, a `get` metody dostępu jest wywoływany przez odwołanie do właściwości w wyrażeniu.

`get` i `set` metod dostępu właściwości nie są różne elementy członkowskie i nie jest możliwe do deklarowania metod dostępu właściwości oddzielnie. W efekcie nie jest możliwe dla dwóch metod dostępu właściwości odczytu / zapisu zapewnienie różnych ułatwień dostępu. Przykład
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
nie deklaruje pojedynczej właściwości odczytu / zapisu. Przeciwnie, deklaruje dwie właściwości o takiej samej nazwie, tylko do odczytu, a drugi jako tylko do zapisu. Ponieważ dwóch elementów członkowskich zadeklarowanych w tej samej klasy nie może mieć takiej samej nazwie, przykład powoduje, że występuje błąd w czasie kompilacji.

Gdy klasa pochodna deklaruje właściwości przez taką samą nazwę jak właściwość dziedziczona, właściwość dziedziczona ukrywa właściwość dziedziczona względem Odczyt i zapis. W przykładzie
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
`P` właściwość `B` ukrywa `P` właściwość `A` względem Odczyt i zapis. W efekcie w instrukcjach
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
Przypisanie do `b.P` powoduje błąd kompilacji, należy podać, ponieważ tylko do odczytu `P` właściwość `B` ukrywa tylko do zapisu `P` właściwość `A`. Należy jednak pamiętać, że rzutowania może służyć do dostępu do ukrytych `P` właściwości.

W przeciwieństwie do pola publiczne właściwości zapewnia oddzielenie stan wewnętrzny obiekt i jego interfejsu publicznego. Rozważmy na przykład:
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

W tym miejscu `Label` klasa korzysta z dwóch `int` pól `x` i `y`, do jego lokalizacji magazynu. Lokalizacja jest publicznie widoczne jako `X` i `Y` właściwości i `Location` właściwości typu `Point`. Jeśli w przyszłych wersjach programu `Label`, staje się bardziej wygodne do przechowywania lokalizacji jako `Point` wewnętrznie, można wprowadzić zmiany bez wywierania wpływu na interfejs publiczny klasy:
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

Gdyby `x` i `y` zamiast tego zostało `public readonly` pól, byłoby niemożliwe wprowadzić zmiany do `Label` klasy.

Udostępnianie stanu przy użyciu właściwości nie jest zawsze wszelkie mniej wydajne niż udostępnianie pól bezpośrednio. W szczególności gdy właściwość jest niewirtualną i zawiera tylko niewielką ilość kodu, środowisko wykonawcze może zastąpić wywołania do metod dostępu, jak rzeczywisty kod metody dostępu. Ten proces jest nazywany ***inlining***, i sprawia, że dostęp do właściwości wydajne niż dostęp do pola, ale zachowuje zwiększona elastyczność właściwości.

Ponieważ wywoływanie `get` metody dostępu jest równoważny odczytywania wartości pola, jest uznawane za nieprawidłowe stylu programowania dla `get` metod dostępu ma dostrzegalnych skutki uboczne. W przykładzie
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
Wartość `Next` właściwości zależy od liczby właściwości wcześniej były używane. Dlatego uzyskiwanie dostępu do właściwości tworzy zauważalny efekt i właściwości powinny być zrealizowane jako metody zamiast tego.

Konwencja "efektów ubocznych" dla `get` akcesory nie oznaczają, że `get` metod dostępu powinny być zawsze zapisywane po prostu zwrócenia wartości przechowywane w polach. W rzeczywistości `get` metod dostępu często obliczenia wartości właściwości, uzyskiwanie dostępu do wielu pól lub wywoływania metod. Jednak prawidłowo zaprojektowana `get` akcesor nie wykonuje żadnych akcji, które powodują zauważalne zmiany w stan obiektu.

Właściwości można opóźnić zainicjowanie zasobu, aż do chwili, który odwołuje się najpierw do. Na przykład:
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

`Console` Klasa zawiera trzy właściwości `In`, `Out`, i `Error`, reprezentujące odpowiednio standardowe dane wejściowe, dane wyjściowe i urządzenia z błędami. Dzięki uwidocznieniu działania tych elementów członkowskich jako właściwości, `Console` klasy można opóźnić ich inicjowania, dopóki nie są rzeczywiście używane. Na przykład, po pierwszym odwołujące się do `Out` właściwości, jak
```csharp
Console.Out.WriteLine("hello, world");
```
podstawowe `TextWriter` dla tworzenia urządzenia w danych wyjściowych. Ale jeśli aplikacja nie ma żadnego odwołania do `In` i `Error` właściwości, a następnie żadne obiekty nie są tworzone dla tych urządzeń.

### <a name="automatically-implemented-properties"></a>Automatycznie zaimplementowane właściwości

Automatycznie implementowanej właściwości (lub ***właściwości automatycznej*** w skrócie), jest właściwością nieabstrakcyjnej niż zewnętrzny z organami dostępu tylko do średnikami. Właściwości automatyczne musi mieć metodę dostępu get i opcjonalnie może mieć dostępu set.

Jeśli właściwość jest określony jako automatycznie implementowanej właściwości, polem zapasowym ukryte są automatycznie dostępne dla właściwości i metod dostępu są wdrożone w celu odczytu i zapisu do tego pola pomocniczego. Jeśli właściwość automatyczną nie metody dostępu set, uznaje się do pola pomocniczego `readonly` ([pola tylko do odczytu](classes.md#readonly-fields)). Podobnie jak w przypadku `readonly` pole, tylko metod pobierających auto właściwością również można przypisać do treści konstruktora otaczającej klasy. Takie przypisania przypisuje bezpośrednio do pola pomocniczego tylko do odczytu właściwości.

Auto właściwością opcjonalnie może mieć *property_initializer*, których są stosowane bezpośrednio do pola pomocniczego jako *variable_initializer* ([zmiennej inicjatory](classes.md#variable-initializers)) .

Poniższy przykład:
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
jest równoważny z następującą deklarację:
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
jest równoważny z następującą deklarację:
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

Zwróć uwagę, przypisania do pola tylko do odczytu czy prawne, ponieważ występują one w konstruktorze.


### <a name="accessibility"></a>Ułatwienia dostępu

Jeśli ma metody dostępu *accessor_modifier*, domena dostępności ([domeny dostępności](basic-concepts.md#accessibility-domains)) metody dostępu jest określany za pomocą deklarowana dostępność metody *accessor_modifier* . Jeśli nie ma metody dostępu *accessor_modifier*, domena dostępności metody dostępu jest określana na podstawie deklarowana dostępność metody, właściwości lub indeksatora.

Obecność *accessor_modifier* nigdy nie wpływa na wyszukanie członka ([operatory](expressions.md#operators)) lub Rozpoznanie przeciążenia ([Rozpoznanie przeciążenia](expressions.md#overload-resolution)). Modyfikatory na właściwość lub indeksator zawsze określić, które właściwości lub indeksatora jest powiązana, niezależnie od tego, w kontekście dostępu.

Po dokonaniu wyboru określoną właściwość lub indeksator, domeny dostępności określonej metody dostępu związane są używane do określenia, czy to użycie jest prawidłowy:

*  Jeśli użycie wynosi jako wartość ([wartości wyrażeń](expressions.md#values-of-expressions)), `get` dostępu musi istnieć i być dostępne.
*  W przypadku użycia jako obiekt docelowy przypisanie proste ([przypisanie proste](expressions.md#simple-assignment)), `set` dostępu musi istnieć i być dostępne.
*  Jeśli użycie wynosi jako obiekt docelowy przypisania złożonego ([przydział złożony](expressions.md#compound-assignment)), lub jako obiekt docelowy `++` lub `--` operatorów ([funkcji elementów członkowskich](expressions.md#function-members).9, [ Wyrażenia wywołania](expressions.md#invocation-expressions)), zarówno `get` metody dostępu i `set` dostępu musi istnieć i być dostępne.

W poniższym przykładzie właściwość `A.Text` jest ukryty przez właściwość `B.Text`, nawet w sytuacjach, w przypadku, gdy tylko `set` nosi nazwę metody dostępu. Z drugiej strony, właściwość `B.Count` nie jest dostępna dla klasy `M`, więc właściwości dostępnych `A.Count` zamian jest używana.

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

Metody dostępu, który jest używany do implementowania interfejsu nie może mieć *accessor_modifier*. Jeśli tylko jedną metodę dostępu jest używana do zaimplementowania interfejsu, inne metody dostępu może być zadeklarowana z *accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>Virtual, zapieczętowany, przesłonięć i metod dostępu do właściwości abstrakcyjnej

A `virtual` deklaracja właściwości określa, że metody dostępu właściwości wirtualnego. `virtual` Modyfikator ma zastosowanie do obu metod dostępu właściwości odczytu / zapisu — nie jest możliwe tylko jednej metody dostępu właściwości odczytu / zapisu być wirtualne.

`abstract` Deklaracja właściwości określa wirtualne metody dostępu właściwości, ale nie zapewnia rzeczywistego implementację metody dostępu. Zamiast tego pochodnej klasy nieabstrakcyjnej wymaganych zapewniali własną implementację dla metod dostępu przez zastąpienie właściwości. Ponieważ metody dostępu dla deklaracji właściwość abstrakcyjną zapewnia nie rzeczywistego wykonania jego *accessor_body* po prostu składa się średnikiem.

Deklaracja właściwości, która obejmuje zarówno `abstract` i `override` Modyfikatory Określa, czy właściwość jest abstrakcyjna i przesłania właściwość podstawowy. Metod dostępu do tych właściwości są również abstrakcyjne.

Deklaracje właściwości abstrakcyjne są dozwolone tylko w klasy abstrakcyjne ([klasy abstrakcyjne](classes.md#abstract-classes)). Metod dostępu właściwości wirtualnego odziedziczonej można zastąpić w klasie pochodnej przy tym deklaracja właściwości, która określa `override` dyrektywy. Jest to nazywane ***zastępowanie deklaracja właściwości***. Przesłanianie deklaracja właściwości nie deklaruje nową właściwość. Zamiast tego należy ją po prostu specjalizuje się implementacje metod dostępu do istniejącej właściwości wirtualnego.

Przesłanianie deklaracja właściwości, należy określić dokładnie tych samych modyfikatory dostępności, typ i nazwę jako właściwość dziedziczona. Jeśli jest dziedziczone właściwość tylko jednej metody dostępu (tj. Jeśli właściwość dziedziczona, jest tylko do odczytu lub tylko do zapisu), nadrzędnych właściwości musi zawierać tylko tej metody dostępu. Jeśli właściwość dziedziczona obejmuje obu metod dostępu (czyli jeśli właściwość dziedziczona odczytu / zapisu), nadrzędnych właściwości mogą obejmować dostępu jednego lub obu metod dostępu.

Przesłanianie deklaracja właściwości może obejmować `sealed` modyfikator. Użyj ten modyfikator uniemożliwia dalsze zastępowanie właściwości klasy pochodnej. Metody dostępu właściwości zapieczętowane również są zamknięte.

Z wyjątkiem różnic w deklaracji i wywołanie składni, zastąpienie wirtualnego, sealed i metody abstrakcyjne dostępu zachowują się tak samo jak wirtualny, sealed, przesłonięć i metody abstrakcyjne. W szczególności opisano zasady w [metod wirtualnych](classes.md#virtual-methods), [Przesłaniaj metody](classes.md#override-methods), [zapieczętowany metody](classes.md#sealed-methods), i [metody abstrakcyjne](classes.md#abstract-methods) zastosować tak, jakby metod dostępu zostały metody odpowiedniego formularza:

*  A `get` akcesor odnosi się do metody bez parametrów, zwracając wartość, typ właściwości i tym samym Modyfikatory jako zawierającą je właściwość.
*  A `set` akcesor odnosi się do metody z parametrem typu właściwości pojedynczej wartości `void` zwracać typ i ten sam Modyfikatory jako zawierającą je właściwość.

W przykładzie
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
`X` jest wirtualny właściwość tylko do odczytu, `Y` jest właściwością odczytu i zapisu wirtualnego i `Z` jest abstrakcyjny właściwości odczytu / zapisu. Ponieważ `Z` jest abstrakcyjny, klasa zawierająca `A` musi również być zadeklarowany jako abstrakcyjny.

Klasa, która pochodzi od klasy `A` jest wyświetlane poniżej:
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

Tutaj, deklaracje `X`, `Y`, i `Z` są zastępowanie deklaracje właściwości. Każdy deklaracja właściwości dokładnie odpowiada modyfikatory dostępności, typ i nazwę odpowiedniej właściwości dziedziczone. `get` Akcesor `X` i `set` akcesor `Y` użyj `base` — słowo kluczowe do dostępu do dziedziczonej metody dostępu. Deklaracja `Z` zastępuje obu abstrakcyjne metod dostępu — w ten sposób, Brak członków oczekujących abstrakcyjna funkcja w `B`, i `B` może być klasą nieabstrakcyjną.

Jeśli właściwość jest zadeklarowany jako `override`, wszelkich przesłoniętych metod dostępu muszą być dostępne dla kodu nadrzędnych. Ponadto deklarowana dostępność metody, właściwości lub indeksatora, sama i metod dostępu, musi być zgodna z zgodnym z przesłoniętą składową i metod dostępu. Na przykład:
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

***Zdarzeń*** jest elementem członkowskim, umożliwiająca obiektem lub klasą do udostępniania powiadomień. Klientów można dołączyć kod wykonywalny dla zdarzeń, podając ***procedury obsługi zdarzeń***.

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

*Event_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)) i prawidłową kombinację modyfikatory dostępu cztery ([modyfikatorach dostępu ](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `static` ([metody statyczne i wystąpienia](classes.md#static-and-instance-methods)), `virtual` ([metody wirtualnej](classes.md#virtual-methods)), `override` ([Przesłaniaj metody](classes.md#override-methods)), `sealed` ([zapieczętowany metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)), a `extern` ([Metody zewnętrznej](classes.md#external-methods)) modyfikatorów.

Deklaracje zdarzeń jest zależna od te same reguły jako deklaracje metody ([metody](classes.md#methods)) w odniesieniu do ważnych kombinacji modyfikatorów.

*Typu* zdarzenia deklaracja musi być *delegate_type* ([typy odwołań](types.md#reference-types)) oraz że *delegate_type* musi wynosić co najmniej jako dostępne jako samego zdarzenia ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).

Może zawierać deklaracji zdarzenia *event_accessor_declarations*. Jednakże, jeśli nie, dla innych extern nieabstrakcyjnej zdarzeń, kompilator dostarcza je automatycznie ([zdarzenia podobne do pól](classes.md#field-like-events)); w przypadku zdarzeń extern metody dostępu są udostępniane zewnętrznie.

Deklaracja zdarzenia, które pomija *event_accessor_declarations* definiuje jedno lub więcej zdarzeń — jednej dla każdego z *variable_declarator*s. Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych za takie *event_declaration*.

Jest to błąd czasu kompilacji dla *event_declaration* oba `abstract` modyfikator i rozdzielany nawias klamrowy *event_accessor_declarations*.

Gdy deklaracji zdarzenia zawiera `extern` modyfikator, zdarzenie jest nazywany ***zewnętrznego zdarzenia***. Ponieważ deklaracja zewnętrznego zdarzenia zapewnia nie rzeczywiste wdrożenie, występuje błąd dla niego oba `extern` modyfikator i *event_accessor_declarations*.

Jest to błąd czasu kompilacji dla *variable_declarator* deklaracji zdarzeń za pomocą `abstract` lub `external` modyfikator obejmujący *variable_initializer*.

Zdarzenia mogą być używane jako lewostronny operand `+=` i `-=` operatorów ([przypisanie zdarzenia](expressions.md#event-assignment)). Te operatory są używane, odpowiednio, można dołączyć programów obsługi zdarzeń do lub usuwanie programów obsługi zdarzeń zdarzeń i modyfikatory dostępu zdarzenia kontrola kontekstów, w których operacje takie są dozwolone.

Ponieważ `+=` i `-=` są tylko operacje, które są dozwolone w zdarzenie poza typ, który deklaruje zdarzenie, kod zewnętrzny można dodać i usunąć programy obsługi zdarzeń, ale nie w jakikolwiek inny sposób uzyskać lub zmodyfikować bazowego listę zdarzeń procedury obsługi.

W przypadku operacji formularza `x += y` lub `x -= y`, gdy `x` jest zdarzeniem i odwołania, ma miejsce poza typ, który zawiera deklarację `x`, wynik operacji ma typ `void` (w przeciwieństwie do konieczności Typ `x`, z wartością `x` po przypisaniu). Ta zasada uniemożliwia kod zewnętrzny pośrednio badanie podstawowego delegata zdarzenia.

W poniższym przykładzie pokazano, jak procedury obsługi zdarzeń są dołączone do wystąpień `Button` klasy:
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

W tym miejscu `LoginDialog` konstruktora wystąpienia tworzy dwa `Button` wystąpień i dołącza programów obsługi zdarzeń do `Click` zdarzenia.

### <a name="field-like-events"></a>Zdarzenia podobne do pól

Tekstu program klasy lub struktury, który zawiera deklarację zdarzenia niektóre zdarzenia mogą być używane jak pola. Ma być używana w ten sposób, nie może być zdarzenie `abstract` lub `extern`i nie może zawierać jawne *event_accessor_declarations*. Takie zdarzenie może służyć w dowolnym kontekście, który zezwala na pola. Pole zawiera delegata ([delegatów](delegates.md)) które odwołuje się do listy programów obsługi zdarzeń, które zostały dodane do zdarzenia. Jeśli zostały dodane nie procedury obsługi zdarzeń, to pole zawiera `null`.

W przykładzie
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
`Click` Służy jako pole w ramach `Button` klasy. Jak w przykładzie pokazano, pole można zbadać, modyfikacji i używać w wyrażeniach wywołanie delegata. `OnClick` Method in Class metoda `Button` klasy "wywołuje" `Click` zdarzeń. Pojęcie podnoszenie zdarzenia odpowiada dokładnie wywoływania delegata reprezentowanej przez zdarzenie — tak więc nie istnieją żadne konstrukcje specjalny język przeznaczony dla podnoszonego zdarzenia. Należy pamiętać, że wywołanie delegata jest poprzedzony wyboru, które zapewnia, że delegat jest różna od null.

Poza deklaracją elementu `Button` klasy `Click` element członkowski należy używać tylko w lewej części `+=` i `-=` operatorów, jak
```csharp
b.Click += new EventHandler(...);
```
która dołącza do listy wywołanie delegata `Click` zdarzenia i
```csharp
b.Click -= new EventHandler(...);
```
co spowoduje usunięcie delegata z listy wywołania `Click` zdarzeń.

Podczas kompilowania kodu zdarzenia podobne do pól, kompilator automatycznie tworzy magazynu do przechowywania delegata oraz metody dostępu dla zdarzenia, które dodają lub usuń programy obsługi zdarzeń do pola delegata. Operacje dodawania i usuwania są bezpieczne wątkowo i może (ale nie jest wymagane) się podczas pracy z blokadą ([instrukcję lock](statements.md#the-lock-statement)) zawierającego go obiektu zdarzenia wystąpienia lub obiekt typu ([anonimowe Tworzenie wyrażenia obiektów](expressions.md#anonymous-object-creation-expressions)) dla zdarzeń statycznych.

W efekcie deklarację wystąpienia zdarzenia w postaci:
```csharp
class X
{
    public event D Ev;
}
```
będzie kompilowana do czegoś równoważne:
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
W ramach klasy `X`, odwołuje się do `Ev` w lewej części `+=` i `-=` operatory powodują dodawanie i usuwanie metod dostępu do wywołania. Wszystkie odwołania do `Ev` są kompilowane do odwoływać się do pola ukrytego `__Ev` zamiast ([dostęp do elementu członkowskiego](expressions.md#member-access)). Nazwa "`__Ev`" dowolnej; ukryte pole może mieć dowolną nazwę lub bez nazwy w ogóle.

### <a name="event-accessors"></a>Metod dostępu zdarzeń

Deklaracje zdarzeń zwykle pominąć *event_accessor_declarations*, jak w `Button` w powyższym przykładzie. Jedna sytuacja może więc obejmuje przypadek, w której koszt magazynowania jednego pola przypadającego na zdarzenia nie są akceptowane. W takich przypadkach mogą zawierać klasę *event_accessor_declarations* i używany był mechanizm prywatnych do przechowywania listy programów obsługi zdarzeń.

*Event_accessor_declarations* zdarzenie Podaj instrukcje wykonywalne skojarzonych z Dodawanie i usuwanie programów obsługi zdarzeń.

Deklaracje metody dostępu składają się z *add_accessor_declaration* i *remove_accessor_declaration*. Każda deklaracja dostępu składa się z tokenem `add` lub `remove` następuje *bloku*. *Bloku* skojarzone z *add_accessor_declaration* Określa instrukcje Aby wykonać po dodaniu programu obsługi zdarzeń i *bloku* skojarzone z *remove_accessor_declaration* Określa instrukcje wykonywania, gdy program obsługi zdarzeń jest usuwana.

Każdy *add_accessor_declaration* i *remove_accessor_declaration* odnosi się do metody przy użyciu pojedynczej wartości parametru typu zdarzenia i `void` typ zwracany. Niejawny parametr metody dostępu zdarzeń ma nazwę `value`. Gdy zdarzenie jest używany w przypisaniu zdarzenia, metody dostępu do odpowiednich zdarzeń jest używany. W szczególności jeśli operator przypisania jest `+=` użyta metody dostępu add i operator przypisania jest `-=` , a następnie usunięcie akcesora jest używany. W obu przypadkach prawostronny operand operatora przypisania służy jako argument do metody dostępu zdarzeń. Blok *add_accessor_declaration* lub *remove_accessor_declaration* musi być zgodna z regułami dotyczącymi `void` metod opisanych w [treści metody](classes.md#method-body). W szczególności `return` instrukcje w takich bloku nie można określić wyrażenie.

Ponieważ metoda dostępu do zdarzeń niejawnie ma parametr o nazwie `value`, jest to błąd czasu kompilacji dla lokalnej zmiennej czy stałej zadeklarowanych w metody dostępu zdarzeń do nazwy.

W przykładzie
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
`Control` klasa implementuje mechanizm wewnętrzny magazyn zdarzeń. `AddEventHandler` Metoda kojarzy wartość delegata z kluczem, `GetEventHandler` metoda zwraca delegata obecnie skojarzony z kluczem, a `RemoveEventHandler` metoda usuwa obiekt delegowany jako program obsługi zdarzeń dla określonego zdarzenia. Prawdopodobnie, podstawowego mechanizmu przechowywania zaprojektowano w taki sposób, że nie ma żadnych kosztów kojarzenia `null` delegować wartość za pomocą klucza i związku z tym nieobsługiwany zdarzenia używanie Brak magazynu.

### <a name="static-and-instance-events"></a>Zdarzenia statyczne i wystąpienia

Jeśli deklaracja zdarzenia zawiera `static` modyfikator, zdarzenie jest nazywany ***zdarzeń statycznych***. Gdy nie `static` modyfikator, zdarzenie jest nazywany ***wystąpienie zdarzenia***.

Zdarzeń statycznych nie jest skojarzony z określonym wystąpieniem i istnieje błąd w czasie kompilacji do odwoływania się do `this` w metod dostępu zdarzeń statycznych.

Wystąpienie zdarzenia jest skojarzona z danego wystąpienia klasy, a to wystąpienie może być dostępna jako `this` ([dostęp](expressions.md#this-access)) w metodach dostępu tego zdarzenia.

Gdy zdarzenie jest przywoływany w *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `E.M`, jeśli `M` jest zdarzeniem statyczne `E` musi oznaczają typu zawierającego `M`i jeśli `M` to zdarzenie wystąpienia E musi oznaczają wystąpienia typu zawierającego `M`.

Różnice statyczne elementy członkowskie wystąpień zostały one omówione w dalszych [statyczna i wystąpienia elementów członkowskich](classes.md#static-and-instance-members).

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Virtual, zapieczętowany, przesłonięć i metody dostępu zdarzenia abstrakcyjnego

A `virtual` deklarację zdarzenia, która określa, że metod dostępu do tego zdarzenia jest wirtualny. `virtual` Modyfikator ma zastosowanie do obu metod dostępu zdarzeń.

`abstract` Deklaracja zdarzenia określa wirtualne metody dostępu zdarzenia, ale nie zapewnia rzeczywistego implementację metody dostępu. Zamiast tego pochodnej klasy nieabstrakcyjnej wymaganych zapewniali własną implementację dla metod dostępu przez zastąpienie zdarzenia. Ponieważ deklaracja zdarzenia abstrakcyjnego zapewnia nie rzeczywistego wykonania, nie może dostarczyć rozdzielonych nawias klamrowy *event_accessor_declarations*.

Deklaracja zdarzenia, która obejmuje zarówno `abstract` i `override` Modyfikatory Określa, że zdarzenie jest abstrakcyjna i zastępuje podstawowego zdarzenia. Metody dostępu takiego zdarzenia są także abstrakcyjne.

Deklaracje abstrakcyjne zdarzeń są dozwolone tylko w klasy abstrakcyjne ([klasy abstrakcyjne](classes.md#abstract-classes)).

Metod dostępu dziedziczone wirtualne wydarzenie może zostać przesłonięta w klasie pochodnej przez łącznie z deklaracji zdarzeń, który określa `override` modyfikator. Jest to nazywane ***zastępowanie deklaracja zdarzenia***. Przesłanianie deklaracja zdarzenia nie deklaruje nowe zdarzenie. Zamiast tego należy ją po prostu specjalizuje się implementacje metod dostępu do zdarzenia istniejącego wirtualnego.

Przesłanianie deklaracja zdarzenia należy określić dokładnie tych samych modyfikatory dostępności, typ i nazwę jako przesłoniętej zdarzenie.

Przesłanianie deklaracja zdarzenia mogą obejmować `sealed` modyfikator. Użyj ten modyfikator uniemożliwia dalsze zastępowanie zdarzenia klasę pochodną. Metod dostępu zdarzeń zapieczętowanego również są zamknięte.

Jest to błąd czasu kompilacji nadrzędnych deklaracji zdarzeń uwzględnić `new` modyfikator.

Z wyjątkiem różnic w deklaracji i wywołanie składni, zastąpienie wirtualnego, sealed i metody abstrakcyjne dostępu zachowują się tak samo jak wirtualny, sealed, przesłonięć i metody abstrakcyjne. W szczególności opisano zasady w [metod wirtualnych](classes.md#virtual-methods), [Przesłaniaj metody](classes.md#override-methods), [zapieczętowany metody](classes.md#sealed-methods), i [metody abstrakcyjne](classes.md#abstract-methods) zastosować tak, jakby metod dostępu zostały metody odpowiedniej postaci. Każdej metody dostępu odnosi się do metody przy użyciu pojedynczej wartości parametru typu zdarzenia `void` zwracać typ i ten sam Modyfikatory jako zdarzenia zawierającego.

## <a name="indexers"></a>Indeksatory

***Indeksatora*** jest element członkowski, który umożliwia to obiektowi zostać pomyślnie zindeksowane w taki sam sposób jak tablica. Indeksatory są deklarowane przy użyciu *indexer_declaration*s:

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

*Indexer_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)) i prawidłową kombinację modyfikatory dostępu cztery ([modyfikatorach dostępu ](classes.md#access-modifiers)), `new` ([nowy modyfikator](classes.md#the-new-modifier)), `virtual` ([metody wirtualnej](classes.md#virtual-methods)), `override` ([Przesłaniaj metody](classes.md#override-methods) ), `sealed` ([Zapieczętowany metody](classes.md#sealed-methods)), `abstract` ([metody abstrakcyjne](classes.md#abstract-methods)), i `extern` ([metody zewnętrznej](classes.md#external-methods)) modyfikatorów.

Deklaracje indeksatora podlegają te same reguły jako deklaracje metody ([metody](classes.md#methods)) w odniesieniu do ważnych kombinacji modyfikatorów, z jednym wyjątkiem jest, że modyfikator statyczny jest niedozwolony w deklaracji indeksatora.

Modyfikatorów `virtual`, `override`, i `abstract` wykluczają się wzajemnie z wyjątkiem w przypadku jednego. `abstract` i `override` Modyfikatory mogą być używane razem, tak aby indeksator abstrakcyjna może zastąpić jeden wirtualny.

*Typu* indeksatora deklaracja określa typ elementu indeksatora wprowadzonych przez tę deklarację. Chyba że indeksatora jest jawną implementacją członków, *typu* następuje słowa kluczowego `this`. Dla implementacja interfejsu jawnego członka *typu* następuje *interface_type*, "`.`", a słowa kluczowego `this`. W przeciwieństwie do innych członków indeksatory nie ma nazwy użytkownika.

*Formal_parameter_list* określa parametry indeksatora. Lista parametrów formalnych indeksatora odnosi się do metody ([parametry metody](classes.md#method-parameters)), z tą różnicą, że co najmniej jeden parametr musi być określony i że `ref` i `out` Modyfikatory parametrów nie są dozwolone. .

*Typu* indeksatora i każdy z typów, do którego odwołuje się *formal_parameter_list* musi być co najmniej tak samo dostępna jak indeksatora, sama ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).

*Indexer_body* może albo składają się z ***treści metody dostępu*** lub ***treści wyrażenia***. W treści metody dostępu *accessor_declarations*, musi być ujęta w "`{`"i"`}`" tokenów, Zadeklaruj metody dostępu ([Akcesory](classes.md#accessors)) właściwości. Metody dostępu Określ instrukcji wykonywalnych powiązane z odczytywaniem i zapisywania właściwości.

Treść wyrażenia składające się z "`=>`" występować wyrażenie `E` , a dokładnie odpowiada na treść instrukcji średnikiem `{ get { return E; } }`i w związku z tym należy używać tylko do określenia tylko metod pobierających indeksatory gdzie jest wynikiem metody pobierającej podane przez jedno wyrażenie.

Mimo że składnia służąca do uzyskiwania dostępu do elementu indeksatora jest taka sama jak dla elementu tablicy, element indeksator nie jest sklasyfikowany jako zmienną. W związku z tym, nie jest możliwe do przekazania element indeksatora `ref` lub `out` argumentu.

Lista parametrów formalnych indeksatora definiuje podpis ([podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading)) indeksatora. W szczególności podpis indeksatora składa się z liczbę i typy parametrów formalnych. Typ elementu oraz nazwy parametrów formalnych nie są częścią podpisu indeksatora.

Podpis indeksatora musi się różnić od podpisy innymi indeksatorami zadeklarowane w tej samej klasy.

Właściwości i indeksatorów są bardzo podobne, ale różnią się w następujący sposób:

*  Właściwość jest identyfikowany przez jego nazwę, natomiast indeksatora jest identyfikowane za pomocą jego podpisu.
*  Właściwość jest dostępny za pośrednictwem *simple_name* ([proste nazwy](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)), podczas gdy indeksatora element jest dostępny za pośrednictwem *element_access* ([dostęp indeksatora](expressions.md#indexer-access)).
*  Właściwość może być `static` elementu członkowskiego, indeksator zawsze jest składową wystąpienia.
*  A `get` metody dostępu właściwości odnosi się do metody bez parametrów, natomiast `get` akcesor indeksatora odnosi się do metody o tej samej listy parametrów formalnych jako indeksatora.
*  A `set` metody dostępu właściwości odnosi się do metody z pojedynczym parametrem o nazwie `value`, podczas gdy `set` akcesor indeksatora odnosi się do metody o tej samej listy parametrów formalnych jako indeksatora plus dodatkowy parametr o nazwie `value`.
*  Jest to błąd czasu kompilacji dla metody dostępu indeksatora zadeklarować zmienną lokalną o takiej samej nazwie jako parametru indeksatora.
*  W deklaracji właściwości nadrzędnych właściwość dziedziczona odbywa się przy użyciu składni `base.P`, gdzie `P` jest nazwą właściwości. W deklaracji nadrzędnych indeksatora dziedziczone indeksatora odbywa się przy użyciu składni `base[E]`, gdzie `E` jest rozdzielana przecinkami lista wyrażeń.
*  Nie obowiązuje koncepcja "indeksatora automatycznie implementowanej". Jest błędem nieabstrakcyjnej, inne niż zewnętrzne indeksatora przy użyciu metod dostępu średnikami.

Oprócz tych różnic wszystkie reguły zdefiniowane w [Akcesory](classes.md#accessors) i [automatycznie implementowane właściwości](classes.md#automatically-implemented-properties) dotyczy indeksatora metod dostępu również do metod dostępu do właściwości.

Jeśli deklaracja indeksator zawiera `extern` modyfikator, indeksatora jest nazywany ***zewnętrznych indeksatora***. Ponieważ deklaracja zewnętrznych indeksatora zapewnia nie rzeczywistą implementację każdego z jego *accessor_declarations* składa się średnikiem.

W poniższym przykładzie deklarujemy `BitArray` klasę, która implementuje indeksator do uzyskiwania dostępu do poszczególnych bitów w tablicy bitowych.
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

Wystąpienie `BitArray` klasy zużywa znacznie mniej pamięci niż odpowiadające `bool[]` (ponieważ każda wartość tego pierwszego zajmuje tylko jednego bitu zamiast jego 's jednobajtowego), ale pozwala na tych samych operacji co `bool[]`.

Następujące `CountPrimes` klasy używa `BitArray` i klasycznego algorytmu "wagi" Aby obliczyć liczbę blokad między 1 a maksymalną danego:
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

Należy pamiętać, że składnia służąca do uzyskiwania dostępu do elementów `BitArray` jest dokładnie taka sama, jak w przypadku `bool[]`.

Poniższy przykład przedstawia klasę siatki 26 * 10, która ma indeksatora z dwoma parametrami. Pierwszy parametr musi być wielkich lub małą literę z zakresu A-Z, a drugi musi być liczbą całkowitą z zakresu od 0 do 9.

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

Zasad rozpoznawania przeciążenia indeksatora są opisane w [wnioskowanie o typie](expressions.md#type-inference).

## <a name="operators"></a>Operatory

***Operator*** jest element członkowski, który definiuje znaczenie operator wyrażenia, które mogą być stosowane do wystąpienia klasy. Operatory są deklarowane przy użyciu *operator_declaration*s:

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

Istnieją trzy kategorie operatory z możliwością przeciążenia: Operatory jednoargumentowe ([operatorów Jednoargumentowych](classes.md#unary-operators)), operatory binarne ([operatory dwuargumentowe](classes.md#binary-operators)) i operatorów konwersji ([operatory konwersji](classes.md#conversion-operators)).

*Operator_body* jest albo średnik ***treść instrukcji*** lub ***treści wyrażenia***. Treść instrukcji składa się z *bloku*, która określa instrukcje wykonywania, gdy operator jest wywoływany. *Bloku* musi być zgodna z regułami dotyczącymi zwracanie wartości metod opisanych w [treści metody](classes.md#method-body). Treść wyrażenia składa się z `=>` następuje wyrażenia i średnika i oznacza pojedynczego wyrażenia do wykonania, gdy operator jest wywoływany.

Aby uzyskać `extern` operatorów, *operator_body* po prostu składa się z średnikiem. Dla wszystkich innych operatorów *operator_body* treści bloku lub treści wyrażenia.

Następujące reguły dotyczą wszystkie deklaracje operatora:

*  Deklaracja operatora musi zawierać zarówno `public` i `static` modyfikator.
*  Parametry operatora musi mieć wartość parametrów ([wartości parametrów](variables.md#value-parameters)). Jest to błąd czasu kompilacji dla Deklaracja operatora określić `ref` lub `out` parametrów.
*  Podpis operatora ([operatorów Jednoargumentowych](classes.md#unary-operators), [operatory dwuargumentowe](classes.md#binary-operators), [operatory konwersji](classes.md#conversion-operators)) musi się różnić od podpisy innych operatorów zadeklarowane w tej samej klasy.
*  Wszystkie typy, do którego odwołuje się Deklaracja operatora musi być co najmniej tak samo dostępna jak operator, sama ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).
*  Jest błąd dla tego samego modyfikator pojawią się wiele razy w Deklaracja operatora.

Każda kategoria operatora nakłada dodatkowe ograniczenia, zgodnie z opisem w poniższych sekcjach.

Podobnie jak inne elementy członkowskie operatory zadeklarowana w klasie bazowej są dziedziczone przez klasy pochodne. Ponieważ deklaracje operatora zawsze należy wymagać klasy lub struktury, w której operator jest zadeklarowany do wzięcia udziału w podpisie operatora, nie jest możliwe operatora zadeklarowana w klasie pochodnej spowoduje ukrycie operatora zadeklarowana w klasie bazowej. W efekcie `new` modyfikator jest nigdy nie wymagane i w związku z tym nigdy nie dozwolone, w deklaracji operatora.

Dodatkowe informacje na temat operatory jednoargumentowe i dane binarne znajdują się w [operatory](expressions.md#operators).

Dodatkowe informacje na temat operatory konwersji można znaleźć w [zdefiniowane przez użytkownika konwersje](conversions.md#user-defined-conversions).

### <a name="unary-operators"></a>Operatory jednoargumentowe

Obowiązują następujące reguły deklaracje operatora jednoargumentowego, gdy `T` wskazuje typ wystąpienia klasy lub struktury, który zawiera deklarację operatora:

*  Jednoargumentowy `+`, `-`, `!`, lub `~` operatora musi mieć jeden parametr typu `T` lub `T?` i może zwrócić dowolnego typu.
*  Jednoargumentowy `++` lub `--` operatora musi mieć jeden parametr typu `T` lub `T?` i musi zwrócić się, że tego samego typu lub pochodzić od niego.
*  Jednoargumentowy `true` lub `false` operatora musi mieć jeden parametr typu `T` lub `T?` i musi zwracać typ `bool`.

Podpis operatora jednoargumentowego składa się z token operatora (`+`, `-`, `!`, `~`, `++`, `--`, `true`, lub `false`) i typ pojedynczy parametr formalny. Zwracany typ nie jest częścią podpisu operatora jednoargumentowego ani nie jest nazwą parametru formalnego.

`true` i `false` operatory jednoargumentowe wymagają pair-wise deklaracji. Błąd kompilacji występuje, jeśli klasa deklaruje jeden z tych operatorów bez także zadeklarowanie drugiego. `true` i `false` operatory są dokładniejszym opisem zawartym w [zdefiniowanych przez użytkownika operatorów logicznych warunkowych](expressions.md#user-defined-conditional-logical-operators) i [wyrażeń logicznych](expressions.md#boolean-expressions).

W poniższym przykładzie pokazano implementację i kolejne użycie `operator ++` klasy wektor liczbą całkowitą:
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

Należy zauważyć, jak metoda operator zwraca wartość produkowane przez dodanie 1 do argumentu operacji, podobnie jak przyrostka inkrementacji i dekrementacji operatorów ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators)) oraz prefiksów inkrementacji i dekrementacji operatory ([przedrostkowe i operatory dekrementacji](expressions.md#prefix-increment-and-decrement-operators)). W przeciwieństwie do języka C++, ta metoda musi Modyfikuje wartości swojego operandu bezpośrednio. W rzeczywistości zmieniając wartość operandu naruszyłoby semantyce przyrostkowego operatora inkrementacyjnego.

### <a name="binary-operators"></a>Operatory binarne

Obowiązują następujące reguły deklaracje operatora binarnego, gdzie `T` wskazuje typ wystąpienia klasy lub struktury, który zawiera deklarację operatora:

*  Operator binarny-shift, należy wykonać dwa parametry, z których co najmniej jeden z nich musi mieć typ `T` lub `T?`i może zwrócić dowolnego typu.
*  Plik binarny `<<` lub `>>` operator przyjmuje dwa parametry, z których pierwszy musi mieć typ `T` lub `T?` i drugi z nich musi mieć typ `int` lub `int?`i może zwrócić dowolnego typu.

Podpis operatora binarnego składa się z token operatora (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, lub `<=`) i typy dwóch parametrów formalnych. Zwracany typ i nazwy parametrów formalnych nie są częścią podpisu operatora binarnego.

Niektóre operatory binarne wymagają pair-wise deklaracji. Dla każdej deklaracji albo operator parę musi być zgodna Deklaracja operatora pary. Dwie deklaracje operatora dopasowania, gdy mają taki sam zwracany typ i ten sam typ, dla każdego parametru. Następujące operatory wymagają pair-wise deklaracji:

*  `operator ==` i `operator !=`
*  `operator >` i `operator <`
*  `operator >=` i `operator <=`

### <a name="conversion-operators"></a>Operatory konwersji

Wprowadza Deklaracja operatora konwersji ***konwersja zdefiniowana przez użytkownika*** ([zdefiniowane przez użytkownika konwersje](conversions.md#user-defined-conversions)) który wspomaga wstępnie zdefiniowanych Konwersje jawne i niejawne.

Deklaracja operatora konwersji, która obejmuje `implicit` — słowo kluczowe wprowadza niejawnej konwersji zdefiniowanych przez użytkownika. Niejawne konwersje mogą występować w wielu różnych sytuacjach, takich jak funkcja elementu członkowskiego wywołania, wyrażenia cast i przypisania. Jest to dokładniejszym opisem zawartym w [niejawne konwersje](conversions.md#implicit-conversions).

Deklaracja operatora konwersji, która obejmuje `explicit` — słowo kluczowe wprowadza zdefiniowanych przez użytkownika konwersji jawnej. Konwersje jawne mogą występować w wyrażenia cast i są dokładniejszym opisem zawartym w [jawne konwersje](conversions.md#explicit-conversions).

Konwertuje operatora konwersji z typu źródłowego, wskazywanym przez typ parametru operatora konwersji na typ docelowy, wskazane przez zwracany typ operatora konwersji.

Dla typu danego źródła `S` i docelowy typ `T`, jeśli `S` lub `T` są typy dopuszczające wartości zerowe umożliwiają `S0` i `T0` odnoszą się do ich typów podstawowych, w przeciwnym razie `S0` i `T0` są równa `S` i `T` odpowiednio. Klasy lub struktury może zadeklarować konwersji z typu źródłowego `S` z typem docelowym `T` tylko wtedy, gdy spełnione są wszystkie z następujących czynności:

*  `S0` i `T0` są różnych typów.
*  Albo `S0` lub `T0` jest typem klasy lub struktury, w którym odbywa się Deklaracja operatora.
*  Ani `S0` ani `T0` jest *interface_type*.
*  Z wyjątkiem konwersje zdefiniowane przez użytkownika konwersja nie istnieje z `S` do `T` lub `T` do `S`.

Dla celów tych reguł, dowolny typ parametrów skojarzonych z `S` lub `T` są traktowane jako unikatowe typy, które mają nie relacji dziedziczenia z innych typów i wszelkie ograniczenia typu, te parametry są ignorowane.

W przykładzie
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
pierwsze deklaracje operatora dwa są dozwolone, ponieważ na potrzeby [indeksatory](classes.md#indexers).3, `T` i `int` i `string` odpowiednio są traktowane jako unikatowe typy z nie relacji. Jednak Trzeci operator jest błąd, ponieważ `C<T>` jest klasa bazowa `D<T>`.

Z drugą regułę, że wynika, że operator konwersji należy przekonwertować do lub z typu klasy lub struktury, w której operator jest zadeklarowany. Na przykład możliwe dla typu klasy lub struktury jest `C` do definiowania konwersja `C` do `int` i `int` do `C`, ale nie z `int` do `bool`.

Nie jest możliwe do przedefiniowania bezpośrednio wstępnie zdefiniowanych konwersji. W związku z tym, operatory konwersji nie są dozwolone do konwertowania z lub `object` ponieważ Konwersje jawne i niejawne już istnieją między `object` i innych typów. Podobnie źródło ani typy docelowy konwersji nie może być typ podstawowy, z drugiej strony, ponieważ konwersja następnie będzie już istnieje.

Jednak jest możliwe do deklarowania operatorów w typach ogólnych określającymi, argumenty określonego typu, konwersje, które już istnieją wstępnie zdefiniowane konwersji. W przykładzie
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
Podczas wpisywania `object` jest określony jako typ argumentu dla `T`, drugi operator deklaruje konwersji, która już istnieje (niejawny i w związku z tym również jawnie, istnieje konwersja z każdym do typu `object`).

W przypadkach, gdy istnieje wstępnie zdefiniowanych konwersji między dwoma typami zdefiniowane przez użytkownika konwersje między typami te są ignorowane. W szczególności:

*  Jeśli wstępnie zdefiniowane niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z typu `S` na typ `T`, wszystkie konwersje zdefiniowane przez użytkownika (niejawny lub jawny) z `S` do `T` są ignorowane.
*  Jeśli wstępnie zdefiniowane jawna konwersja ([jawne konwersje](conversions.md#explicit-conversions)) istnieje z typu `S` na typ `T`, zdefiniowane przez użytkownika jawnych konwersji z `S` do `T` są ignorowane. Ponadto:

Jeśli `T` jest typem interfejsu użytkownika niejawne konwersje z elementu `S` do `T` są ignorowane.

W przeciwnym razie wartość zdefiniowana przez użytkownika niejawne konwersje z elementu `S` do `T` są nadal uważane za.

Dla wszystkich typów, ale `object`, operatory zadeklarowana przez `Convertible<T>` powyżej typ nie wchodzą w konflikt z wstępnie zdefiniowanych konwersji. Na przykład:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

Jednak w przypadku typu `object`, wstępnie zdefiniowanych konwersje Ukryj konwersje zdefiniowane przez użytkownika w wszystkich przypadków, ale jedna:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

Konwersje zdefiniowane przez użytkownika nie są dozwolone do konwersji z lub do *interface_type*s. W szczególności, to ograniczenie gwarantuje, że nie transformacji zdefiniowanych przez użytkownika wystąpić podczas konwertowania na *interface_type*oraz że konwersję *interface_type* powiedzie się tylko wtedy, gdy obiekt faktycznie przekształcany implementuje określony *interface_type*.

Podpis operatora konwersji składa się z typu źródłowego i typ docelowy. (Zwróć uwagę, że jest to tylko formularz, dla którego zwracany typ uczestniczy w podpisie elementu członkowskiego). `implicit` Lub `explicit` klasyfikacji operatora konwersji nie jest częścią podpisu operatora. W związku z tym, klasie lub strukturze nie można zadeklarować zarówno `implicit` i `explicit` operatora konwersji z tymi samymi typami źródłowym i docelowym.

Ogólnie rzecz biorąc zdefiniowane przez użytkownika konwersje niejawne powinny zostać tak zaprojektowane, aby nigdy nie zgłaszają wyjątków i nigdy nie utracą informacji. Jeśli konwersja zdefiniowana przez użytkownika może powodować powstanie wyjątków (na przykład, ponieważ źródłowy jest poza zakresem) lub utraty informacji (takich jak odrzucanie bardziej znaczących bitów), a następnie tej konwersji, powinien być zdefiniowany jako jawnej konwersji.

W przykładzie
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
Konwersja z `Digit` do `byte` jest niejawny, ponieważ nigdy nie zgłasza wyjątki lub traci informacji, ale konwersja z `byte` do `Digit` jest jawne od `Digit` może być reprezentowana przez podzbiór możliwe wartości `byte`.

## <a name="instance-constructors"></a>Konstruktory wystąpień

***Konstruktora wystąpienia*** jest elementem członkowskim implementujący działania wymagane w celu zainicjowania wystąpienia klasy. Konstruktory wystąpień są deklarowane przy użyciu *constructor_declaration*s:

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

A *constructor_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)), prawidłowej kombinacji modyfikatory dostępu cztery ([modyfikatorach dostępu ](classes.md#access-modifiers)), a `extern` ([metody zewnętrznej](classes.md#external-methods)) modyfikator. Deklaracja konstruktora jest niedozwolona w zawierają ten sam modyfikator wiele razy.

*Identyfikator* z *constructor_declarator* należy nadać nazwę klasy, w którym zadeklarowany jest konstruktora wystąpienia. Jeśli jakakolwiek inna nazwa jest określona, występuje błąd kompilacji.

Opcjonalny *formal_parameter_list* wystąpienia Konstruktor podlega te same reguły jako *formal_parameter_list* metody ([metody](classes.md#methods)). Na liście parametrów formalnych definiuje podpis ([podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading)) konstruktora wystąpienia i kontroluje proces, według której rozpoznanie przeciążenia ([wnioskowanie o typie](expressions.md#type-inference)) wybiera określonego Konstruktor wystąpienia w wywołaniu.

Każdy z typów, do którego odwołuje się *formal_parameter_list* wystąpienie konstruktora musi być co najmniej tak samo dostępna jak sam Konstruktor ([ograniczenia ułatwień dostępu](basic-concepts.md#accessibility-constraints)).

Opcjonalny *constructor_initializer* określa innego konstruktora wystąpienia do wywołania przed wykonaniem instrukcji podanych w *constructor_body* tego konstruktora wystąpienia. Jest to dokładniejszym opisem zawartym w [inicjatory konstruktora](classes.md#constructor-initializers).

Jeśli deklaracja konstruktora zawiera `extern` modyfikator, Konstruktor jest określany jako ***zewnętrznych Konstruktor***. Ponieważ deklaracja konstruktora zewnętrznych zapewnia nie rzeczywistego wykonania jego *constructor_body* składa się średnikiem. Dla innych konstruktorów *constructor_body* składa się z *bloku* określający instrukcje inicjuje nowe wystąpienie klasy. Dokładnie odpowiada *bloku* z metody wystąpienia mającej `void` zwracany typ ([treści metody](classes.md#method-body)).

Konstruktory wystąpień nie są dziedziczone. W związku z tym klasa nie ma konstruktorów wystąpienia niż rzeczywiście zadeklarowana w klasie. Jeśli klasa nie zawiera żadnych deklaracji konstruktora wystąpienia, domyślnego konstruktora wystąpienia jest dostarczana automatycznie ([domyślne konstruktory](classes.md#default-constructors)).

Konstruktory wystąpień są wywoływane przez *object_creation_expression*s ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)) i *constructor_initializer*s.

### <a name="constructor-initializers"></a>Inicjatory konstruktora

Wszystkie wystąpienia konstruktory (z wyjątkiem tych, dla klasy `object`) niejawnie obejmują bezpośrednio przed wywołaniem innego konstruktora wystąpienia *constructor_body*. Konstruktor do niejawnie wywołania jest określana przez *constructor_initializer*:

*  Inicjatora konstruktora wystąpienia formularza `base(argument_list)` lub `base()` powoduje, że konstruktora wystąpień z bezpośredniej klasy bazowej do wywołania. Ten konstruktor jest zaznaczone, przy użyciu *argument_list* Jeśli obecny i zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution). Zestaw konstruktory wystąpień Release candidate składa się z wszystkie konstruktory dostępne wystąpienia zawarte w bezpośredniej klasie bazowej lub domyślnego konstruktora ([domyślne konstruktory](classes.md#default-constructors)), jeśli nie konstruktory wystąpień są deklarowane w Bezpośrednia klasa bazowa. Jeśli ten zestaw jest pusty lub nie można zidentyfikować pojedynczy najlepsze konstruktora wystąpienia, występuje błąd kompilacji.
*  Inicjatora konstruktora wystąpienia formularza `this(argument-list)` lub `this()` powoduje, że konstruktora wystąpień z klasy, do wywołania. Konstruktor jest zaznaczone, przy użyciu *argument_list* Jeśli obecny i zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution). Zestaw konstruktory wystąpień Release candidate składa się z wszystkie konstruktory dostępne wystąpienia zadeklarowanych w samej klasy. Jeśli ten zestaw jest pusty lub nie można zidentyfikować pojedynczy najlepsze konstruktora wystąpienia, występuje błąd kompilacji. Jeśli deklaracja konstruktora wystąpienia zawiera inicjatora konstruktora, który wywołuje sam Konstruktor, występuje błąd kompilacji.

Jeśli konstruktora wystąpienia nie ma konstruktora inicjatora, inicjatora konstruktora formularza `base()` niejawnie są dostarczane. W związku z tym Konstruktor deklarację wystąpienia formularza
```csharp
C(...) {...}
```
odpowiada dokładnie
```csharp
C(...): base() {...}
```

Zakres parametrów podanych przez *formal_parameter_list* deklaracja konstruktora wystąpień obejmuje inicjatora konstruktora w tej deklaracji. W związku z tym dostęp do parametrów konstruktora może inicjatora konstruktora. Na przykład:
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

Inicjatora konstruktora wystąpienia nie może uzyskać dostępu jest utworzone wystąpienie. Dlatego jest to błąd czasu kompilacji, aby odwołać się do `this` w wyrażeniu argumentu inicjatora konstruktora, co jest błąd kompilacji, wyrażenie argumentu dowolny członek wystąpienia przez odwołanie do *simple_name*.

### <a name="instance-variable-initializers"></a>Inicjatory zmiennej wystąpienia

Podczas konstruktora wystąpienia nie ma konstruktora inicjatora lub ma on postać inicjatora konstruktora `base(...)`, ten konstruktor wykonuje niejawnie inicjowania określonej przez *variable_initializer*s pola wystąpienia są zadeklarowane w swojej klasie. Odpowiada to sekwencja przypisania, które są wykonywane od razu po wejściu do konstruktora i przed niejawne wywołanie konstruktora bezpośrednią klasą bazową. Inicjatory zmiennej są wykonywane w kolejności tekstowych, w jakiej występują w deklaracji klasy.

### <a name="constructor-execution"></a>Wykonywanie konstruktora

Inicjatory zmiennych są przekształcane w instrukcji przypisania, a te instrukcje przypisania są wykonywane przed wywołaniem konstruktora wystąpienia klasy bazowej. Ta kolejność gwarantuje, że wszystkie pola wystąpienia są inicjowane przez ich zmiennej inicjatory, przed wykonaniem dowolnej instrukcji, które mają dostęp do tego wystąpienia.

Podane w przykładzie
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
Gdy `new B()` służy do tworzenia instancji `B`, są następujące wyniki:
```
x = 1, y = 0
```

Wartość `x` wynosi 1, ponieważ inicjator zmiennej jest wykonywany przed wywołaniem konstruktora wystąpienia klasy bazowej. Jednak wartość `y` ma wartość 0 (wartość domyślna `int`) ponieważ przypisanie do `y` nie jest wykonywana do czasu, po powrocie konstruktora klasy bazowej.

Warto traktować jako stwierdzenia, które są automatycznie wstawiany przed inicjatorów zmiennej wystąpienia i inicjatory konstruktora *constructor_body*. Przykład
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
zawiera kilka zmiennych inicjatory; zawiera ona także inicjatory konstruktora obu Form (`base` i `this`). Przykład odnosi się do kodu pokazanego poniżej, gdzie każdy komentarz wskazuje automatycznie wstawiono — instrukcja (składnia używana do wywołania automatycznie wstawiono Konstruktor nie jest prawidłowy, ale służy jedynie do ilustrowania mechanizmu).

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

Jeśli klasa nie zawiera żadnych deklaracji konstruktora wystąpienia, domyślnego konstruktora wystąpienia jest dostarczana automatycznie. Ten konstruktor domyślny po prostu wywołuje konstruktora bez parametrów bezpośrednie klasy bazowej. Jeśli klasa jest abstrakcyjna deklarowana dostępność metody dla domyślnego konstruktora jest chroniona. W przeciwnym razie deklarowana dostępność metody dla domyślny konstruktor jest publiczny. W związku z tym domyślny konstruktor jest zawsze formularza

```csharp
protected C(): base() {}
```
lub
```csharp
public C(): base() {}
```
gdzie `C` jest nazwą klasy. Jeśli funkcja rozpoznawania przeciążeń nie potrafi określić unikatowy najlepsze kandydatem do inicjatora konstruktora klasy bazowej, a następnie wystąpi błąd kompilacji.

W przykładzie
```csharp
class Message
{
    object sender;
    string text;
}
```
domyślny konstruktor znajduje się ponieważ klasy nie zawiera żadnych deklaracjach konstruktorów wystąpienia. W efekcie przykładzie odpowiada dokładnie
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>Konstruktory prywatne

Gdy klasa `T` deklaruje tylko konstruktory wystąpień prywatnych, nie jest możliwe dla klas spoza i tekstu programu `T` wyprowadzenia z `T` lub bezpośrednio utworzyć wystąpienia elementu `T`. W związku z tym Jeśli klasa zawiera tylko statyczne elementy członkowskie i nie jest przeznaczony do użycia, dodanie pustego wystąpienia prywatnych konstruktora uniemożliwi podczas tworzenia wystąpienia. Na przykład:
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

`Trig` Klasa grup powiązanych metod i stałe, ale nie jest przeznaczony do użycia. W związku z tym deklaruje Konstruktor pusty prywatnej SIS. Co najmniej jedno wystąpienie konstruktora musi być zadeklarowany Pomija automatyczne generowanie konstruktora domyślnego.

### <a name="optional-instance-constructor-parameters"></a>Parametry konstruktora wystąpienia opcjonalne

`this(...)` Formularz inicjatora konstruktora jest najczęściej używany w połączeniu z przeciążenia do zaimplementowania wystąpienia opcjonalne parametry konstruktora. W przykładzie
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
pierwsze konstruktory dwóch wystąpień jedynie podać wartości domyślnej brakujące argumenty. Obaj użytkownicy za pomocą `this(...)` inicjatora konstruktora, aby wywołać konstruktora wystąpienia trzeci faktycznie działa inicjowania nowego wystąpienia. Efekt jest to, że Konstruktor opcjonalne parametry:
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>Konstruktory statyczne

A ***statyczny Konstruktor*** jest elementem członkowskim implementujący działania wymagane w celu zainicjowania typu klasy zamknięte. Konstruktory statyczne są deklarowane przy użyciu *static_constructor_declaration*s:

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

A *static_constructor_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)) i `extern` modyfikator ([metody zewnętrznej](classes.md#external-methods)).

*Identyfikator* z *static_constructor_declaration* należy nadać nazwę klasy, w którym zadeklarowany jest konstruktor statyczny. Jeśli jakakolwiek inna nazwa jest określona, występuje błąd kompilacji.

Jeśli deklaracja konstruktora statycznego obejmuje `extern` modyfikator, statyczny Konstruktor jest nazywany ***zewnętrznych Konstruktor statyczny***. Ponieważ deklaracja zewnętrznych konstruktora statycznego zapewnia nie rzeczywistego wykonania jego *static_constructor_body* składa się średnikiem. Dla wszystkich innych deklaracji statyczny Konstruktor *static_constructor_body* składa się z *bloku* określający instrukcji do wykonania w celu zainicjowania klasy. Dokładnie odpowiada *method_body* metody statycznej z `void` zwracany typ ([treści metody](classes.md#method-body)).

Konstruktory statyczne nie są dziedziczone i nie można wywołać bezpośrednio.

Konstruktor statyczny dla typu klasy zamkniętego co najwyżej raz wykonuje w domenie danej aplikacji. Wykonanie statyczny Konstruktor jest wyzwalany przez pierwszy z następujących zdarzeń w domenie aplikacji:

*  Tworzone jest wystąpienie typu klasy.
*  Dowolny ze statycznych składowych typu klasy do których istnieją odwołania.

Jeśli klasa zawiera `Main` — metoda ([uruchamiania aplikacji](basic-concepts.md#application-startup)), w którym zaczyna się, statyczny Konstruktor dla tej klasy jest wykonywany przed `Main` metoda jest wywoływana.

Można zainicjować nowego typu klasy zamknięte, najpierw nowy zestaw pól statycznych ([pola statyczne i wystąpienia](classes.md#static-and-instance-fields)) dla tego konkretnego typu zamknięte jest tworzony. Każde z pól statycznych jest inicjowany do wartości domyślnej ([wartości domyślne](variables.md#default-values)). Następnie, inicjatory pola statycznego ([pole statyczne inicjowanie](classes.md#static-field-initialization)) są wykonywane dla tych pól statycznych. Na koniec Konstruktor statyczny jest wykonywany.

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
należy wygenerować dane wyjściowe:
```
Init A
A.F
Init B
B.F
```
ponieważ wykonanie `A`firmy Konstruktor statyczny jest wyzwalany przez wywołanie `A.F`i wykonywania `B`firmy Konstruktor statyczny jest wyzwalany przez wywołanie `B.F`.

Istnieje możliwość konstruowania zależności cykliczne, zezwalających na pola statyczne z inicjatorami zmiennej należy przestrzegać w ich stanie. wartość domyślna.

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
generuje dane wyjściowe
```
X = 1, Y = 2
```

Do wykonania `Main` metody pierwszego uruchomienia systemu inicjator dla `B.Y`wcześniejszej klasy `B`firmy Konstruktor statyczny. `Y`w inicjatorze powoduje, że `A`firmy statyczny Konstruktor można uruchomić, ponieważ wartość `A.X` odwołuje się do. Statyczny Konstruktor `A` z kolei przechodzi do obliczenia wartości `X`i przeprowadzeniu tak pobiera wartość domyślną `Y`, który wynosi zero. `A.X` ten sposób jest inicjowany do 1. Proces uruchamiania `A`inicjatorów pola statyczne i Konstruktor statyczny, a następnie zakończeniu i powrocie do obliczania wartości początkowej `Y`, której wynikiem staje się 2.

Ponieważ Konstruktor statyczny jest wykonywane tylko raz dla każdego zamknięty typ klasy skonstruowany, jest wygodnym miejscem do wymuszania dla parametru typu, który nie można sprawdzić w czasie kompilacji za pomocą ograniczenia sprawdzania w trakcie wykonywania ([parametr typu ograniczenia](classes.md#type-parameter-constraints)). Na przykład następujący typ używa statyczny Konstruktor do wymuszania, że argument typu jest wyliczeniem:
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

A ***destruktor*** jest elementem członkowskim implementujący działania wymagane do niszczenia wystąpienia klasy. Destruktor jest zadeklarowany za pomocą *destructor_declaration*:

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

A *destructor_declaration* może zawierać zestaw *atrybuty* ([atrybuty](attributes.md)).

*Identyfikator* z *destructor_declaration* należy nadać nazwę klasy, w którym zadeklarowany jest destruktor. Jeśli jakakolwiek inna nazwa jest określona, występuje błąd kompilacji.

Gdy obejmuje deklaracja destruktora `extern` modyfikator, destruktor jest nazywany ***zewnętrznych — destruktor***. Ponieważ deklaracja destruktora zewnętrznych zapewnia nie rzeczywistego wykonania jego *destructor_body* składa się średnikiem. Dla innych destruktory *destructor_body* składa się z *bloku* określający instrukcji do wykonania w celu niszczenia wystąpienia klasy. A *destructor_body* odpowiadają dokładnie *method_body* z metody wystąpienia mającej `void` zwracany typ ([treści metody](classes.md#method-body)).

Destruktory nie są dziedziczone. W związku z tym klasa ma żadne destruktory innym niż ten, który może być zdeklarowany przez tę klasę.

Ponieważ destruktora musi mieć żadnych parametrów, nie może być przeciążona, więc klasa może mieć co najwyżej jeden destruktor.

Destruktory są wywoływane automatycznie i nie można jawnie wywołać. Wystąpienia staje się kwalifikuje się do zniszczenia, gdy nie jest już możliwe dla jakiegokolwiek kodu użyć tego wystąpienia. Wykonanie destruktora dla tego wystąpienia może występować w dowolnym momencie, gdy wystąpienie stanie się kwalifikuje się do zniszczenia. Gdy wystąpienie jest usuwane, destruktory w łańcuch dziedziczenia tego wystąpienia są wywoływane w kolejności od najbardziej pochodnego na co najmniej pochodnych. Destruktora może być wykonywane na żadnym z wątków. Aby zapoznać się z reguł, które określają, kiedy i jak jest wykonywane destruktora, zobacz [automatyczne zarządzanie pamięcią](basic-concepts.md#automatic-memory-management).

Dane wyjściowe z przykładu
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
ponieważ destruktory w łańcuchu dziedziczenia są wywoływane w kolejności od najbardziej pochodnego na co najmniej pochodnych.

Destruktory są implementowane przez zastąpienie metody wirtualnej `Finalize` na `System.Object`. C# programy nie mogą przesłaniać tę metodę, lub zadzwoń ona (lub zastąpienia jej) bezpośrednio. Na przykład program
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
zawiera błędy, dwie.

Kompilator zachowuje się tak, jakby tę metodę i zastąpienia, nie istnieją w ogóle. W związku z tym ten program:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
jest prawidłowy, a metoda ukrywa `System.Object`firmy `Finalize` metody.

Aby uzyskać informacje dotyczące zachowania gdy wyjątek został pominięty przez destruktora, zobacz [sposób obsługi wyjątków](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Iteratory

Element członkowski funkcji ([funkcji elementów członkowskich](expressions.md#function-members)) implementowane przy użyciu blokiem iteratora ([bloki](statements.md#blocks)) nosi nazwę ***iteratora***.

Blokiem iteratora może być używana jako treść funkcji elementu członkowskiego, tak długo, jak zwracany typ odpowiedni element członkowski funkcji jest jednym z interfejsy modułu wyliczającego ([interfejsy modułu wyliczającego](classes.md#enumerator-interfaces)) lub jeden z wyliczalne interfejsy ([Wyliczalne interfejsy](classes.md#enumerable-interfaces)). Może wystąpić jako *method_body*, *operator_body* lub *accessor_body*, zdarzenia, konstruktory wystąpień, konstruktorów statycznych i destruktory nie może być zaimplementowane jako Iteratory.

Funkcja składowa jest implementowana przy użyciu blokiem iteratora, jest to błąd czasu kompilacji, na liście parametrów formalnych element członkowski funkcji, aby określić dowolne `ref` lub `out` parametrów.

### <a name="enumerator-interfaces"></a>Interfejsy modułu wyliczającego

***Interfejsy modułu wyliczającego*** są interfejsu nieogólnego `System.Collections.IEnumerator` i wszystkich wystąpień elementu ogólny interfejs `System.Collections.Generic.IEnumerator<T>`. W celu skrócenia programu, w tym rozdziale te interfejsy są określone jako `IEnumerator` i `IEnumerator<T>`, odpowiednio.

### <a name="enumerable-interfaces"></a>Wyliczalne interfejsy

***Wyliczalne interfejsy*** są interfejsu nieogólnego `System.Collections.IEnumerable` i wszystkich wystąpień elementu ogólny interfejs `System.Collections.Generic.IEnumerable<T>`. W celu skrócenia programu, w tym rozdziale te interfejsy są określone jako `IEnumerable` i `IEnumerable<T>`, odpowiednio.

### <a name="yield-type"></a>Typ YIELD

Iterator wytwarza sekwencję wartości, wszystkie tego samego typu. Tego typu jest nazywana ***uzyskanie typu*** iteratora.

*  Typ iteratora zwracającego yield `IEnumerator` lub `IEnumerable` jest `object`.
*  Typ iteratora zwracającego yield `IEnumerator<T>` lub `IEnumerable<T>` jest `T`.

### <a name="enumerator-objects"></a>Moduł wyliczający obiektów

Gdy element członkowski funkcji zwraca moduł wyliczający typu interfejsu, jest implementowana przy użyciu blokiem iteratora, wywołanie funkcji elementu członkowskiego nie natychmiast wykonuje kod w bloku iteratora. Zamiast tego ***enumerator — obiekt*** jest tworzony i zwracany. Hermetyzuje kod określony w bloku iteratora i wykonywanie kodu w bloku iteratora występuje, gdy obiekt modułu wyliczającego `MoveNext` metoda jest wywoływana. Obiekt modułu wyliczającego ma następującą charakterystykę:

*  Implementuje `IEnumerator` i `IEnumerator<T>`, gdzie `T` jest typem yield iteratora.
*  Implementuje `System.IDisposable`.
*  (Jeśli istnieje) jest inicjowany z kopią wartości argumentu, a wystąpienia wartość przekazana do funkcji elementu członkowskiego.
*  Ma cztery potencjalnych stanów, ***przed***, ***systemem***, ***zawieszone***, i ***po***i jest początkowo ***przed***  stanu.

Obiekt modułu wyliczającego zazwyczaj to wystąpienie klasy generowane przez kompilator modułu wyliczającego, która hermetyzuje kod w bloku iteratora i implementuje interfejsy modułu wyliczającego, ale możliwe są inne metody wdrażania. Jeśli klasy modułu wyliczającego jest generowany przez kompilator, będzie można zagnieżdżać tej klasy, bezpośrednio lub pośrednio, klasy zawierającej funkcję elementu członkowskiego, będzie mieć ułatwień dostępu prywatnego i będzie mieć nazwę zarezerwowany do użytku kompilatora ([identyfikatorów ](lexical-structure.md#identifiers)).

Obiekt modułu wyliczającego mogą implementować interfejsów więcej niż te wymienione powyżej.

W poniższych sekcjach opisano dokładne zachowanie `MoveNext`, `Current`, i `Dispose` członkowie `IEnumerable` i `IEnumerable<T>` dostarczane przez obiekt modułu wyliczającego implementacji interfejsu.

Pamiętaj, że moduł wyliczający obiektów nie obsługuje `IEnumerator.Reset` metody. Wywołanie tej metody powoduje, że `System.NotSupportedException` zostanie wygenerowany.

#### <a name="the-movenext-method"></a>Metoda MoveNext

`MoveNext` Metoda obiekt modułu wyliczającego hermetyzuje kod blokiem iteratora. Wywoływanie `MoveNext` metoda wykonuje kod w bloku iteratora i zestawach `Current` właściwości obiektu modułu wyliczającego, zgodnie z potrzebami. Dokładne akcję wykonywaną przez `MoveNext` zależy od stanu obiekt modułu wyliczającego podczas `MoveNext` jest wywoływana:

*  Jeśli stan obiektu modułu wyliczającego jest ***przed***, powinny być przekazywane wywołującemu `MoveNext`:
   * Zmienia stan na ***systemem***.
   * Inicjuje parametry (w tym `this`) bloku iteratora wartości argumentów i wartości wystąpienia zapisywana, gdy obiekt modułu wyliczającego został zainicjowany.
   * Wykonuje blok iterator od samego początku, dopóki nie przerywa wykonywania (opisanych poniżej).
*  Jeśli stan obiektu modułu wyliczającego jest ***systemem***, wynik wywoływania `MoveNext` jest nieokreślona.
*  Jeśli stan obiektu modułu wyliczającego jest ***zawieszone***, powinny być przekazywane wywołującemu `MoveNext`:
   * Zmienia stan na ***systemem***.
   * Przywraca wartości wszystkich zmiennych lokalnych i parametrów (w tym to) wartości zapisywana, gdy wykonanie bloku iteratora ostatnio zostało wstrzymane. Należy pamiętać, że zawartość wszystkie obiekty, które odwołują się te zmienne mogą zmieniły się od poprzedniego wywołania MoveNext.
   * Wznawia działanie natychmiast po bloku iteratora `yield return` instrukcję, która spowodowała zawieszenie wykonywania i jest kontynuowane, dopóki nie przerywa wykonywania (opisanych poniżej).
*  Jeśli stan obiektu modułu wyliczającego jest ***po***, powinny być przekazywane wywołującemu `MoveNext` zwraca `false`.


Gdy `MoveNext` wykonuje bloku iteratora, może zostać przerwane wykonywania na cztery sposoby: Przez `yield return` instrukcji, `yield break` instrukcji przez napotkania koniec bloku iteratora i wyjątek jest generowany i propagowane z bloku iteratora.

*  Gdy `yield return` napotkania instrukcji ([instrukcji yield](statements.md#the-yield-statement)):
   * Wyrażenie w instrukcji jest obliczane, niejawnie konwertowany na typ produktywności i przypisane do `Current` właściwości obiektu modułu wyliczającego.
   * Wykonywanie treści iteratora jest zawieszone. Wartości wszystkich zmiennych lokalnych i parametrów (w tym `this`) zostaną zapisane, ponieważ jest to lokalizacja `yield return` instrukcji. Jeśli `yield return` Instrukcja znajduje się w co najmniej jeden `try` blokuje skojarzonego `finally` bloki nie są wykonywane w tej chwili.
   * Stan modułu wyliczającego obiektu jest zmieniany na ***zawieszone***.
   * `MoveNext` Metoda zwraca `true` do obiektu wywołującego, wskazujący, że iteracji pomyślnie zaawansowane do następnej wartości.
*  Gdy `yield break` napotkania instrukcji ([instrukcji yield](statements.md#the-yield-statement)):
   * Jeśli `yield break` Instrukcja znajduje się w co najmniej jeden `try` blokuje skojarzonego `finally` bloki są wykonywane.
   * Stan modułu wyliczającego obiektu jest zmieniany na ***po***.
   * `MoveNext` Metoda zwraca `false` do obiektu wywołującego, wskazujący, że iteracji została zakończona.
*  Po napotkaniu końcu treści iteratora:
   * Stan modułu wyliczającego obiektu jest zmieniany na ***po***.
   * `MoveNext` Metoda zwraca `false` do obiektu wywołującego, wskazujący, że iteracji została zakończona.
*  Gdy wyjątek jest zgłaszany i propagowane z bloku iteratora:
   * Odpowiednie `finally` bloków w treści iteratora będą wykonywane przez propagacji wyjątku.
   * Stan modułu wyliczającego obiektu jest zmieniany na ***po***.
   * Propagacja wyjątków w dalszym ciągu obiektu wywołującego `MoveNext` metody.

#### <a name="the-current-property"></a>Właściwość Current

Obiekt modułu wyliczającego `Current` właściwości jest zależna od `yield return` instrukcje w bloku iteratora.

Gdy obiekt modułu wyliczającego jest w ***zawieszone*** stanu wartości `Current` jest wartość ustawioną przy użyciu poprzedniego wywołania `MoveNext`. Gdy obiekt modułu wyliczającego jest w ***przed***, ***systemem***, lub ***po*** stany wynik uzyskiwania dostępu do `Current` jest nieokreślona.

Dla iteratora instrukcję YIELD typu innego niż `object`, wynikiem uzyskiwania dostępu do `Current` przez obiekt modułu wyliczającego `IEnumerable` implementacji odnosi się do uzyskiwania dostępu do `Current` przez obiekt modułu wyliczającego `IEnumerator<T>` implementacji i wynik rzutowania `object`.

#### <a name="the-dispose-method"></a>Metoda Dispose

`Dispose` Metoda jest używana, aby wyczyścić iteracji, przenosząc obiekt modułu wyliczającego ***po*** stanu.

*  Jeśli stan obiektu modułu wyliczającego jest ***przed***, powinny być przekazywane wywołującemu `Dispose` zmienia stan na ***po***.
*  Jeśli stan obiektu modułu wyliczającego jest ***systemem***, wynik wywoływania `Dispose` jest nieokreślona.
*  Jeśli stan obiektu modułu wyliczającego jest ***zawieszone***, powinny być przekazywane wywołującemu `Dispose`:
   * Zmienia stan na ***systemem***.
   * Wykonują wszelkie na koniec bloki tak, jakby ostatnio wykonana `yield return` instrukcji zostały `yield break` instrukcji. Jeśli powoduje to, że wyjątek zgłoszony i propagowane poza treści iteratora, stan obiektu modułu wyliczającego jest ustawiony na ***po*** i wyjątek jest propagowany do obiektu wywołującego `Dispose` metody.
   * Zmienia stan na ***po***.
*  Jeśli stan obiektu modułu wyliczającego jest ***po***, powinny być przekazywane wywołującemu `Dispose` nie ma wpływu.

### <a name="enumerable-objects"></a>Wyliczalne obiektów

Gdy element członkowski funkcji zwracająca typ wyliczalny interfejs jest implementowany przy użyciu blokiem iteratora, wywołanie funkcji elementu członkowskiego nie natychmiast wykonuje kod w bloku iteratora. Zamiast tego ***wyliczanym obiektem*** jest tworzony i zwracany. Obiekt wyliczalny `GetEnumerator` metoda zwraca obiekt modułu wyliczającego, który hermetyzuje kod określony w bloku iteratora i wykonywanie kodu w bloku iteratora występuje, gdy obiekt modułu wyliczającego `MoveNext` metoda jest wywoływana. Obiekt wyliczalny ma następującą charakterystykę:

*  Implementuje `IEnumerable` i `IEnumerable<T>`, gdzie `T` jest typem yield iteratora.
*  (Jeśli istnieje) jest inicjowany z kopią wartości argumentu, a wystąpienia wartość przekazana do funkcji elementu członkowskiego.

Obiekt wyliczalny zazwyczaj to wystąpienie klasy generowane przez kompilator wyliczalny hermetyzuje kod w bloku iteratora, który implementuje wyliczalne interfejsy, ale możliwe są inne metody wdrażania. Jeśli wyliczalny klasy jest generowany przez kompilator, tej klasy będzie można zagnieżdżać, bezpośrednio lub pośrednio w klasie zawierający element członkowski funkcji, będzie mieć ułatwień dostępu prywatnego i będzie mieć nazwę zarezerwowany do użytku kompilatora ([identyfikatorów ](lexical-structure.md#identifiers)).

Obiekt wyliczalny mogą implementować interfejsów więcej niż te wymienione powyżej. W szczególności może również zastosować obiekt wyliczalny `IEnumerator` i `IEnumerator<T>`, dzięki czemu może służyć jako element wyliczalny i moduł wyliczający. W tym typie wdrożenia, po raz pierwszy obiekt wyliczalny `GetEnumerator` wywołaniu metody z zwracany jest sam obiekt wyliczalny. Kolejne wywołania obiekt wyliczalny `GetEnumerator`, jeśli istnieje, zwraca kopię obiektu wyliczalny. W związku z tym każde zwracane moduł wyliczający ma swój własny stan i zmiany w jeden moduł wyliczający nie wpływają na inny.

#### <a name="the-getenumerator-method"></a>GetEnumerator — metoda

Obiekt wyliczalny oferuje implementację `GetEnumerator` metody `IEnumerable` i `IEnumerable<T>` interfejsów. Dwa `GetEnumerator` metody udostępniania typową implementację uzyskuje i zwraca obiekt modułu wyliczającego dostępne. Obiekt modułu wyliczającego jest inicjowany za pomocą wartości argumentów i wystąpienia zapisywana, gdy obiekt wyliczalny został zainicjowany, ale w przeciwnym razie wartość funkcji obiekt modułu wyliczającego, zgodnie z opisem w [obiektów modułu wyliczającego](classes.md#enumerator-objects).

### <a name="implementation-example"></a>Przykład wdrożenia

W tej sekcji opisano możliwą implementację Iteratory pod względem konstrukcje zgodność ze starszymi wersjami języka C#. Implementacja opisane w tym miejscu opiera się na te same zasady, które są używane przez kompilator Microsoft C#, ale w żadnym wypadku nie jest obowiązkowego implementacji lub możliwe tylko jeden.

Następujące `Stack<T>` klasy implementuje jego `GetEnumerator` metody za pomocą iteratora. Iterator który wylicza elementy stosu w od góry do dołu zamówienia.

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

`GetEnumerator` Metody mogą być tłumaczone do wystąpienia klasy generowane przez kompilator modułu wyliczającego, która hermetyzuje kod w bloku iteratora, jak pokazano w następującym.

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

W poprzednim translacji, kod w bloku iteratora jest przekształcane w automacie stanów i umieszczane w `MoveNext` metody klasy modułu wyliczającego. Ponadto zmienna lokalna `i` jest przekształcane w pola w obiekcie moduł wyliczający, dzięki czemu może on nadal istnieją w wywołań `MoveNext`.

Poniższy przykład drukuje prostej tabeli mnożenie liczb całkowitych od 1 do 10. `FromTo` Metody w przykładzie zwraca obiekt wyliczeniowy i jest implementowane za pomocą iteratora.

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

`FromTo` Metoda może można przetłumaczyć egzemplarzem generowanych przez kompilator wyliczalny klasę, która hermetyzuje kod w bloku iteratora, jak pokazano w następującym.

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

Wyliczalne klasa implementuje wyliczalne interfejsy i interfejsy modułu wyliczającego, dzięki czemu może służyć jako element wyliczalny i moduł wyliczający. Po raz pierwszy `GetEnumerator` wywołaniu metody z zwracany jest sam obiekt wyliczalny. Kolejne wywołania obiekt wyliczalny `GetEnumerator`, jeśli istnieje, zwraca kopię obiektu wyliczalny. W związku z tym każde zwracane moduł wyliczający ma swój własny stan i zmiany w jeden moduł wyliczający nie wpływają na inny. `Interlocked.CompareExchange` Metoda służy do zapewnienia działania metodą o bezpiecznych wątkach.

`from` i `to` parametry są przekształcane w pola w klasie wyliczalny. Ponieważ `from` zostanie zmodyfikowany w bloku iteratora, dodatkowy `__from` pola został wprowadzony do przechowywania wartości początkowej, umożliwiającej `from` w każdy moduł wyliczający.

`MoveNext` Metoda zgłasza wyjątek `InvalidOperationException` Jeśli jest ona wywoływana, gdy `__state` jest `0`. Chroni to przed użycie wyliczanym obiektem jako obiekt modułu wyliczającego bez wywoływania pierwszy `GetEnumerator`.

Poniższy przykład przedstawia klasę proste drzewa. `Tree<T>` Klasy implementuje jego `GetEnumerator` metody za pomocą iteratora. Iterator który wylicza elementy drzewa w kolejności wrostkowe.

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

`GetEnumerator` Metody mogą być tłumaczone do wystąpienia klasy generowane przez kompilator modułu wyliczającego, która hermetyzuje kod w bloku iteratora, jak pokazano w następującym.

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

Obiekty tymczasowe wygenerowanego przez kompilator, używane w `foreach` instrukcje są podniesione do `__left` i `__right` pola obiekt modułu wyliczającego. `__state` Pola obiektu modułu wyliczającego dokładnie zostanie zaktualizowany, aby poprawny `Dispose()` metoda zostanie wywołana poprawnie, jeśli wyjątek jest zgłaszany. Należy pamiętać, że nie jest możliwość napisania przetłumaczony kod za pomocą prostego `foreach` instrukcji.

## <a name="async-functions"></a>Funkcje asynchroniczne

Metody ([metody](classes.md#methods)) lub funkcja anonimowa ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)) za pomocą `async` nosi nazwę modyfikator ***funkcji asynchronicznej***. Ogólnie rzecz biorąc określenie ***async*** służy do opisywania dowolnych funkcji, która ma `async` modyfikator.

Jest to błąd czasu kompilacji, na liście parametrów formalnych funkcji asynchronicznej, aby określić dowolne `ref` lub `out` parametrów.

*Typ_wyniku* asynchroniczna metoda musi być albo `void` lub ***typ zadania***. Typy zadań `System.Threading.Tasks.Task` i typy złożone z `System.Threading.Tasks.Task<T>`. W celu skrócenia programu, w tym rozdziale te typy są określone jako `Task` i `Task<T>`, odpowiednio. Metoda asynchroniczna zwracająca typ zadania jest nazywany zwracającą zadanie.

Dokładne definicji typów zadań jest definiowany przez implementację, ale z punktu widzenia języka typ zadania jest w jednym z stanów ukończona, zakończyło się pomyślnie lub wystąpił błąd. Zadanie uszkodzoną rejestruje odpowiednich wyjątków. Zakończono pomyślnie `Task<T>` rejestruje wynik o typie `T`. Typy zadań są oczekujący i dlatego może być argumenty operacji wyrażeń await ([wyrażeń Await](expressions.md#await-expressions)).

Asynchroniczne wywołanie funkcji ma możliwość wstrzymywania oceny przez wyrażeń await ([wyrażeń Await](expressions.md#await-expressions)) w jej treści. Ocena może zostać wznowione później w punkcie zawieszenia wyrażenie poprzez await ***delegata wznowienie***. Typ jest delegatem wznowienie `System.Action`, i gdy jest wywoływana, oceny wywołania funkcji asynchronicznej zostanie wznowiona z wyrażenia oczekującego tam, gdzie ją przerwaliśmy. ***Bieżący obiekt wywołujący*** funkcji asynchronicznej wywołanie jest oryginalny obiekt wywołujący, jeśli wywołania funkcji nigdy nie zostało zawieszone lub najnowszych wywołujący delegata wznowienie inaczej.

### <a name="evaluation-of-a-task-returning-async-function"></a>Oceny przez funkcję zwracającą zadanie asynchroniczne

Wywołanie funkcji asynchronicznej zwracania zadania powoduje wystąpienie typu zwracanego zadania do wygenerowania. Jest to nazywane ***Zwróć typ task*** funkcji asynchronicznej. Zadanie początkowo jest niekompletna.

Treść funkcji asynchronicznej jest następnie oceniany aż do jego albo została zawieszona (przez osiągnięcia wyrażenie await) lub kończy się, jaką kontroli punktu jest zwracany do obiektu wywołującego, wraz z Zwróć typ task.

Po kończy treść funkcji asynchronicznej, zwracane zadanie zostaje przeniesiony poza stanie niepełnym:

*  Jeśli treść funkcji zakończy się w wyniku dotrzeć do instrukcji return lub na końcu treści, każda wartość wyniku jest rejestrowana w zwracanego zadania, które są umieszczane w stanie sukces.
*  Jeśli w wyniku nieprzechwycony wyjątek kończy się treści funkcji ([instrukcji "throw"](statements.md#the-throw-statement)) wyjątek jest rejestrowana w zwracanego zadania, które są umieszczane w nieprawidłowym stanie.

### <a name="evaluation-of-a-void-returning-async-function"></a>Obliczanie funkcji asynchronicznej zwraca void

Jeśli jest zwracany typ funkcji asynchronicznej `void`, oceny, różni się od powyższego w następujący sposób: Ponieważ zadanie nie zostanie zwrócone, funkcja zamiast komunikuje się uzupełniania i wyjątków w bieżącym wątku ***kontekst synchronizacji***. Dokładne definicji kontekst synchronizacji jest zależna od implementacji, ale jest to reprezentacja elementu "gdzie" bieżący wątek jest uruchomiony. Kontekst synchronizacji to powiadomienie, gdy oceny funkcji asynchronicznej zwraca void rozpocznie się zakończy się pomyślnie i powoduje, że nieprzechwycony wyjątek zostanie wygenerowany.

Dzięki temu kontekst, aby śledzić liczbę zwracającą typ void async funkcje są uruchomione w nim i zdecydować, jak Propagacja wyjątków pochodzących z nich.
