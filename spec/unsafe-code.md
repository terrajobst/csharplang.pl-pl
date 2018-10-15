# <a name="unsafe-code"></a>Niebezpieczny kod

Podstawowe języka C#, zgodnie z definicją w poprzednich rozdziałach różni się przede wszystkim od C i C++ w jej pominięcia wskaźników jako typ danych. Zamiast tego, C# zawiera odwołania i możliwość tworzenia obiektów, które są zarządzane przez moduł odśmiecania pamięci. Ten projekt, w połączeniu z innymi funkcjami sprawia, że C# znacznie bezpieczniejsze język niż C lub C++. W języku podstawowym C# po prostu nie jest możliwe niezainicjowaną zmienną, wskaźnik "delegujące" lub wyrażenie, które indeksuje tablicy poza jej granicami. Całej kategorii błędów oznacza rutynowo Plaga C i C++ programy związku z tym są eliminowane.

Chociaż praktycznie każdej konstrukcji typu wskaźnika w języku C lub C++ ma odpowiednika typu odwołania w języku C#, niemniej jednak istnieją sytuacje, gdzie dostęp do typów wskaźnika staje się koniecznością. Na przykład komunikuje się z usługą system operacyjny, uzyskiwanie dostępu do zamapowanych w pamięci urządzenia lub Implementowanie czas ma istotne znaczenie algorytm nie może być możliwe lub praktyczne, bez dostępu do wskaźników. Aby zaspokoić te potrzeby, C# zapewnia możliwość pisania ***niebezpieczny kod***.

W niebezpieczny kod jest to możliwe zadeklarować i wykonywać operacje na wskaźnikach do wykonywania konwersji między wskaźników i typów całkowitych, aby pobrać adres zmienne i tak dalej. W tym sensie pisania niebezpieczny kod przypomina znacznie pisania kodu języka C w ramach programu C#.

Niebezpieczny kod jest w rzeczywistości "bezpieczne" funkcji z punktu widzenia zarówno programistom, jak i użytkowników. Niebezpieczny kod muszą być wyraźnie oznaczone za pomocą modyfikatora `unsafe`, dzięki czemu deweloperzy prawdopodobnie nie można używać funkcji niebezpieczne przypadkowo i działa aparat wykonywania, aby upewnić się, że niebezpieczny kod nie może działać w środowisku niezaufanym.

## <a name="unsafe-contexts"></a>Konteksty unsafe

Niebezpieczne funkcje języka C# są dostępne tylko w kontekstach niebezpieczne. Wprowadzono niebezpieczny kontekst, w tym `unsafe` modyfikatora w deklaracji typu lub elementu członkowskiego lub przez wprowadzenie *unsafe_statement*:

*  Deklaracja klasy, struktury, interfejsu lub delegata może obejmować `unsafe` modyfikator, w którym przypadku cały zakres tekstową tej deklaracji typu (w tym treść klasy, struktury lub interfejsu) jest uznawany za niebezpieczny kontekst.
*  Może zawierać deklarację pola, metoda, właściwość, zdarzenie, indeksator, operatora, konstruktora wystąpienia, destruktor lub Konstruktor statyczny `unsafe` modyfikator, w których przypadku cały zakres tekstową deklaracja składowej nie jest bezpieczne kontekst.
*  *Unsafe_statement* umożliwia użycie niebezpieczny kontekst, w ramach *bloku*. Cały zakres tekstową skojarzonego *bloku* jest traktowane jako niebezpieczny kontekst.

Zaprojektuj skojarzone gramatyki zostały wymienione poniżej.

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

W przykładzie

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

`unsafe` modyfikatora w deklaracji struktury powoduje, że cały zakres tekstową deklaracji struktury, stają się w niebezpiecznym kontekście. W związku z tym, istnieje możliwość zadeklarować `Left` i `Right` pola typu wskaźnika. Również będą zapisywane w powyższym przykładzie

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

W tym miejscu `unsafe` modyfikatorów w deklaracjach pola spowodować, że te deklaracje uważane za niebezpieczne kontekstów.

Inne niż ustanawiania niebezpieczny kontekst, w związku z tym pozwalających na korzystanie z typów wskaźnika `unsafe` modyfikator nie ma wpływu na typu lub elementu członkowskiego. W przykładzie

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

`unsafe` modyfikator na `F` method in Class metoda `A` powoduje po prostu tekstową stopnia `F` przestanie niebezpieczny kontekst, w którym można użyć niebezpiecznych funkcji języka. W zastępowania metody `F` w `B`, trzeba ponownie określ `unsafe` modyfikator — chyba że, oczywiście, `F` method in Class metoda `B` sam musi mieć dostęp do funkcji niebezpieczne.

Sytuacja jest nieco inne w przypadku, gdy typem wskaźnika jest część podpisu metody

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

W tym miejscu ponieważ `F`firmy podpis zawiera typ wskaźnika, jego może być zapisany tylko w niebezpiecznym kontekście. Jednak niebezpieczny kontekst mogą zostać wprowadzone w taki sposób, tworząc albo całej klasy niebezpieczne, tak jak w przypadku w `A`, albo załączając `unsafe` modyfikatora w deklaracji metody, jak w przypadku `B`.

## <a name="pointer-types"></a>Typy wskaźnika

W niebezpiecznym kontekście *typu* ([typy](types.md)) może być *pointer_type* , a także *value_type* lub *reference_type* . Jednak *pointer_type* mogą być również używane w `typeof` wyrażenia ([wyrażenia tworzenia obiektu anonimowego](expressions.md#anonymous-object-creation-expressions)) poza niebezpieczny kontekst jako takie użycie nie jest niebezpieczne.

```antlr
type_unsafe
    : pointer_type
    ;
```

A *pointer_type* jest zapisywany jako *unmanaged_type* lub słowa kluczowego `void`, a następnie `*` tokenu:

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

Typ określony przed `*` w wskaźnik typu jest nazywana ***typu Obiekt obsługujący*** typu wskaźnika. Reprezentuje typ zmiennej, na który wskazuje wartości typu wskaźnik.

W przeciwieństwie do odwołania (wartości typów referencyjnych) wskaźniki nie są śledzone przez moduł zbierający elementy bezużyteczne — moduł odśmiecania pamięci nie zna wskaźników i danych, które wskazują. Z tego powodu wskaźnik nie jest dozwolona wskaż odwołanie lub do struktury zawierającej odwołania, a musi być typ hermetyzowanym wskaźnikiem *unmanaged_type*.

*Unmanaged_type* jest dowolny typ, który nie jest *reference_type* lub skonstruowany z typu, a nie zawiera *reference_type* lub skonstruowany typ pola na dowolnym poziomie zagnieżdżania. Innymi słowy *unmanaged_type* jest jednym z następujących czynności:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, lub `bool`.
*  Wszelkie *enum_type*.
*  Wszelkie *pointer_type*.
*  Dowolny zdefiniowany przez użytkownika *struct_type* , nie jest zbudowany typ i zawiera pola *unmanaged_type*tylko s.

Intuicyjne reguły do mieszania wskaźników i odwołań jest referents odwołania (obiekty) mogą zawierać wskaźniki, że referents wskaźników nie mogą zawierać odwołań.

Niektóre przykłady typów wskaźnika są podane w poniższej tabeli:

| __Przykład__ | __Opis__                               |
|-------------|-----------------------------------------------|
| `byte*`     | wskaźnik do `byte`                             |
| `char*`     | wskaźnik do `char`                             |
| `int**`     | Wskaźnik do wskaźnika do `int`                   |
| `int*[]`    | Jednowymiarowa tablica wskaźników do `int` |
| `void*`     | Wskaźnik do nieznanego typu                       |

Dla danego wdrożenia wszystkich typów wskaźnika musi mieć ten sam rozmiar i reprezentacji.

W przeciwieństwie do C i C++, gdy wiele wskaźniki są deklarowane w tej samej deklaracji w języku C# `*` są zapisywane wraz z typem podstawowym, nie jako znak interpunkcyjny prefiks każdej nazwy wskaźnika. Na przykład

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

Wartość wskaźnika typu `T*` reprezentuje adres zmiennej typu `T`. Operatora pośredniego wskaźnika `*` ([operację wskaźnika pośredniego](unsafe-code.md#pointer-indirection)) może służyć do dostępu do tej zmiennej. Na przykład, biorąc pod uwagę zmienną `P` typu `int*`, wyrażenie `*P` oznacza `int` znaleziono pod adresem zawartym w zmiennej `P`.

Podobnie jak odwołanie do obiektu może być wskaźnik `null`. Zastosowanie operatora pośredniego do `null` wskaźnika powoduje zachowanie zdefiniowane w implementacji. Wskaźnik o wartości `null` jest reprezentowany przez wszystkie bity zero.

`void*` Typu reprezentuje wskaźnik do nieznanego typu. Ponieważ obiekt obsługujący typ jest nieznany, nie można zastosować operatora pośredniego do wskaźnika typu `void*`, ani nie można wykonać wszystkie operacje arytmetyczne na taki wskaźnik. Jednak wskaźnika typu `void*` mogą być rzutowane do jakichkolwiek innych typów wskaźnika (i na odwrót).

Typy wskaźników są oddzielne kategorii typów. W odróżnieniu od typy odwołań i typy wartości typów wskaźnika nie dziedziczą `object` i nie występują konwersje między typami wskaźnika a `object`. W szczególności, konwersja boxing i konwersja unboxing ([pakowania i rozpakowywania](types.md#boxing-and-unboxing)) nie są obsługiwane dla wskaźników. Jednak konwersje są dozwolone, między różnymi typami wskaźnika oraz między typami wskaźnika a typami całkowitymi. Jest to opisane w [konwersje wskaźników](unsafe-code.md#pointer-conversions).

A *pointer_type* nie można użyć jako argument typu ([skonstruowany typy](types.md#constructed-types)) i wnioskowanie o typie ([wnioskowanie o typie](expressions.md#type-inference)) kończy się niepowodzeniem na wywołania metody rodzajowej, które będzie mieć wywnioskowane Wpisz argument był typem wskaźnika.

A *pointer_type* może być używany jako typ pola nietrwałego ([pola nietrwałego](classes.md#volatile-fields)).

Chociaż wskaźniki mogą być przekazywane jako `ref` lub `out` parametrów w ten sposób może spowodować niezdefiniowane zachowanie, ponieważ wskaźnik może również być równa wskaż zmiennej lokalnej, który już nie istnieje, po powrocie z metody o nazwie lub stały obiekt, do którego użyć w celu wskazania, już nie zostanie rozwiązany. Na przykład:

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

Metoda może zwracać wartość pewnego typu, a ten typ może być wskaźnika. Na przykład, gdy wskaźnik do ciągłej sekwencji `int`s, liczba elementów w sekwencji i niektóre inne `int` wartość następującej metody zwraca adres tę wartość w tej sekwencji, jeśli wystąpi dopasowanie; w przeciwnym razie zwraca `null`:

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

W niebezpiecznym kontekście kilka konstrukcji są dostępne dla działających na wskaźnikach:

*  `*` Operator może służyć do wykonywania operację wskaźnika pośredniego ([operację wskaźnika pośredniego](unsafe-code.md#pointer-indirection)).
*  `->` Operator może służyć do uzyskania dostępu do członka struktury za pomocą wskaźnika ([dostęp do elementu członkowskiego wskaźnik](unsafe-code.md#pointer-member-access)).
*  `[]` Operator może służyć do indeksu wskaźnik ([wskaźnika elementu dostępu](unsafe-code.md#pointer-element-access)).
*  `&` Operator może być używany do uzyskiwania adresu zmiennej ([operatora address-of](unsafe-code.md#the-address-of-operator)).
*  `++` i `--` operatorzy mogą służyć do inkrementacja i dekrementacja wskaźników ([wskaźnik inkrementacja i dekrementacja](unsafe-code.md#pointer-increment-and-decrement)).
*  `+` i `-` operatorów można wykonać arytmetyki wskaźnika ([arytmetyki wskaźnika](unsafe-code.md#pointer-arithmetic)).
*  `==`, `!=`, `<`, `>`, `<=`, I `=>` operatorzy mogą służyć do porównywania wskaźników ([porównanie wskaźników](unsafe-code.md#pointer-comparison)).
*  `stackalloc` Operator może służyć do przydzielania pamięci ze stosu wywołań ([stałych buforów o rozmiarze](unsafe-code.md#fixed-size-buffers)).
*  `fixed` Instrukcji może służyć do tymczasowo naprawić zmienną, dzięki czemu można uzyskać adresu ([instrukcji fixed](unsafe-code.md#the-fixed-statement)).

## <a name="fixed-and-moveable-variables"></a>Stałe i ruchome zmiennych

Operator address-of ([operatora address-of](unsafe-code.md#the-address-of-operator)) i `fixed` instrukcji ([instrukcji fixed](unsafe-code.md#the-fixed-statement)) podzielić zmiennych na dwie kategorie: ***stałej zmienne***i ***ruchome zmienne***.

Stałe zmienne znajdują się w lokalizacji przechowywania, które nie ma wpływu na działanie modułu odśmiecania pamięci. (Stałe zmienne przykładami zmienne lokalne, wartości parametrów i zmiennych utworzonych przez wyłuskanie wskaźników.) Z drugiej strony zmienne ruchome znajdują się w lokalizacji przechowywania, które podlegają relokacji lub usunięcia przez moduł odśmiecania pamięci. (Zmienne ruchome należą pola obiektów oraz elementów tablic.)

`&` — Operator ([operatora address-of](unsafe-code.md#the-address-of-operator)) pozwala na adres zmienna ustalona można uzyskać bez ograniczeń. Jednak ponieważ ruchome zmienna jest przeniesienie lub usunięcia przez moduł odśmiecania pamięci, adres zmiennej ruchome można uzyskać jedynie przy użyciu `fixed` — instrukcja ([instrukcji fixed](unsafe-code.md#the-fixed-statement)) i ten adres pozostaje ważny tylko na czas trwania tego `fixed` instrukcji.

W dokładne warunki zmienna ustalona jest jednym z następujących czynności:

*  Zmienna wynikające z *simple_name* ([proste nazwy](expressions.md#simple-names)) odwołujący się do zmiennej lokalnej lub wartość parametru, chyba, że zmienna jest przechwytywany przez funkcję anonimową.
*  Zmienna wynikające z *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `V.I`, gdzie `V` jest stałą zmienną *struct_type*.
*  Zmienna wynikające z *pointer_indirection_expression* ([operację wskaźnika pośredniego](unsafe-code.md#pointer-indirection)) w postaci `*P`, *pointer_member_access* ([Dostęp do elementu członkowskiego wskaźnika](unsafe-code.md#pointer-member-access)) w postaci `P->I`, lub *pointer_element_access* ([wskaźnika elementu dostępu](unsafe-code.md#pointer-element-access)) w postaci `P[E]`.

Wszystkie inne zmienne są klasyfikowane jako ruchome zmiennych.

Należy zauważyć, że pole statyczne zostanie sklasyfikowany jako zmienną ruchome. Należy również zauważyć, że `ref` lub `out` parametru jest klasyfikowana jako zmienną ruchome, nawet jeśli argumentu dla parametru jest zmienna ustalona. Na koniec należy pamiętać, że zmienna produkowane przez wyłuskaniem wskaźnika zawsze zostanie sklasyfikowany jako zmienna ustalona.

## <a name="pointer-conversions"></a>Konwersje wskaźników

W niebezpiecznym kontekście zestaw dostępnych niejawne konwersje ([niejawne konwersje](conversions.md#implicit-conversions)) jest rozszerzona, aby uwzględnić poniższe konwersje niejawne wskaźnika:

*  Za pomocą dowolnego *pointer_type* typowi `void*`.
*  Z `null` literału do dowolnego *pointer_type*.

Ponadto w niebezpiecznym kontekście, zestaw dostępności Konwersje jawne ([jawne konwersje](conversions.md#explicit-conversions)) jest rozszerzona, aby uwzględnić poniższe Konwersje jawne wskaźnika:

*  Za pomocą dowolnego *pointer_type* do żadnej innej *pointer_type*.
*  Z `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, lub `ulong` do dowolnego *pointer_type*.
*  Za pomocą dowolnego *pointer_type* do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, lub `ulong`.

Na koniec w niebezpiecznym kontekście zestaw standardowych niejawne konwersje ([standardowe i niejawne konwersje](conversions.md#standard-implicit-conversions)) obejmuje następujące konwersja wskaźnika:

*  Za pomocą dowolnego *pointer_type* typowi `void*`.

Konwersje między typami wskaźnika, dwa nigdy nie zmienia wartości rzeczywistej wskaźnika. Innymi słowy konwersja z typu jeden wskaźnik do innego nie ma wpływu na podstawowy adres podany przez wskaźnik.

Gdy jeden typ wskaźnika jest konwertowany na inny, jeśli wskaźnik wynikowy nie jest prawidłowo wyrównany dla typu wskazywanego, zachowanie jest niezdefiniowane, jeżeli wynik jest wyłuskiwany. Ogólnie rzecz biorąc, pojęcie "prawidłowo wyrównane" jest przechodnia: Jeśli wskaźnik do typu `A` jest prawidłowo wyrównany dla wskaźnika do typu `B`, która z kolei jest prawidłowo wyrównany dla wskaźnika do typu `C`, następnie wskaźnik do typu `A`jest prawidłowo wyrównany dla wskaźnika do typu `C`.

Należy wziąć pod uwagę następujący przypadek, w którym masz jeden typ zmiennej jest dostępna za pośrednictwem wskaźnik do innego typu:

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

Po typ wskaźnika jest konwertowana na wskaźnik do bajtów, wskazuje wynik najniższą zaadresowane bajt zmiennej. Kolejne przyrosty wyniku rozmiarze zmiennej, uzyskanie wskaźniki do pozostałe bajty tej zmiennej. Na przykład następującą metodę wyświetla każdy z ośmiu bajtów w wartość o podwójnej precyzji jako wartość szesnastkowa:

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

Oczywiście dane wyjściowe wytwarzane zależy od kolejność bajtów.

Mapowania między wskaźników i liczby całkowite są zdefiniowane w implementacji. Jednakże w przypadku 32 * i architektury procesorów 64-bitowym z liniowej przestrzeni adresowej, konwersje wskaźników do i z typów całkowitych zazwyczaj zachowują się tak samo jak konwersji `uint` lub `ulong` wartości, odpowiednio do lub z tych typów całkowitych.

### <a name="pointer-arrays"></a>Tablice wskaźników

W niebezpiecznym kontekście można konstruować tablice wskaźników. Dozwolone są tylko niektóre z konwersji, które mają zastosowanie do innych typów tablicy na tablicach wskaźnika:

*  Niejawna konwersja odwołania ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) za pomocą dowolnego *array_type* do `System.Array` i interfejsy, implementuje również odnosi się do tablic wskaźnika. Jednak dowolne próba uzyskania dostępu do elementów tablicy przy użyciu `System.Array` lub interfejsy implementuje spowodują wyjątek w czasie wykonywania, zgodnie z typów wskaźnika nie są konwertowane na `object`.
*  Niejawne i jawne odwoływać się do konwersji ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions), [Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)) z typu tablicy jednowymiarowej `S[]` do `System.Collections.Generic.IList<T>` i jego ogólne interfejsy podstawowe nigdy nie dotyczą tablice wskaźników, ponieważ typy wskaźnika nie można użyć jako argumentów typu, a brak konwersji z typów wskaźnika do typów nie będącego wskaźnikiem.
*  Konwersja jawnego odwołania ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)) z `System.Array` i interfejsy implementuje do dowolnego *array_type* dotyczy tablice wskaźników.
*  Konwersje jawne odwołań ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)) z `System.Collections.Generic.IList<S>` i bazowej interfejsy na typ tablicy jednowymiarowej `T[]` nigdy nie dotyczy tablice wskaźników, ponieważ nie może być typów wskaźnika używane jako argumenty typu i brak konwersji z typów wskaźnika do typów nie będącego wskaźnikiem.

Te ograniczenia oznaczają, że rozszerzenie dla `foreach` instrukcji przez tablice opisanego w [Instrukcja foreach](statements.md#the-foreach-statement) nie można zastosować do wskaźnika tablic. Zamiast tego Instrukcja foreach formularza

```csharp
foreach (V v in x) embedded_statement
```

gdzie typ `x` jest typem tablicy w formie `T[,,...,]`, `N` jest liczba wymiarów, minus 1 i `T` lub `V` jest typem wskaźnika, jest rozwinięta, przy użyciu zagnieżdżonych dla pętli w następujący sposób:

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

Zmienne `a`, `i0`, `i1`,..., `iN` nie są widoczne i dostępne dla `x` lub *embedded_statement* lub inny kod źródłowy programu. Zmienna `v` jest tylko do odczytu w osadzonych instrukcji. Jeśli nie jest jawną konwersję ([konwersje wskaźników](unsafe-code.md#pointer-conversions)) z `T` (typ elementu) do `V`, jest generowany błąd i nie trzeba wykonywać dalszych czynności są wykonywane. Jeśli `x` ma wartość `null`, `System.NullReferenceException` jest generowany w czasie wykonywania.

## <a name="pointers-in-expressions"></a>Wskaźniki w wyrażeniach

W niebezpiecznym kontekście wyrażenie może przynieść wynik typem wskaźnika, ale poza niebezpiecznym kontekście jest to błąd czasu kompilacji, wyrażenie typu wskaźnika. W dokładne warunki, poza niebezpiecznym kontekście błąd kompilacji występuje ewentualne *simple_name* ([proste nazwy](expressions.md#simple-names)), *member_access* ([dostęp do elementu członkowskiego ](expressions.md#member-access)), *invocation_expression* ([wyrażenia wywołania](expressions.md#invocation-expressions)), lub *element_access* ([dostępu elementu](expressions.md#element-access)) jest typem wskaźnika.

W niebezpiecznym kontekście *primary_no_array_creation_expression* ([wyrażenia podstawowe](expressions.md#primary-expressions)) i *unary_expression* ([operatory jednoargumentowe](expressions.md#unary-operators)) Zaprojektuj zezwala na następujące konstrukcje dodatkowe:

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

W poniższych sekcjach opisano te konstrukcje. Pierwszeństwo i kojarzenie operatorów unsafe jest implikowany przez gramatyki.

### <a name="pointer-indirection"></a>Operatora pośredniego wskaźnika

A *pointer_indirection_expression* składa się z gwiazdką (`*`) następuje *unary_expression*.

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

Jednoargumentowy `*` operator oznacza operację wskaźnika pośredniego i służy do uzyskiwania zmiennej, na który wskazuje wskaźnika. Wynik obliczania wartości `*P`, gdzie `P` to wyrażenie typu wskaźnika `T*`, jest zmienną typu `T`. Chodzi o błąd kompilacji, aby zastosować jednoargumentowe `*` operatora wyrażenia typu `void*` lub wyrażenie, które nie są typu wskaźnika.

Efektem zastosowania jednoargumentowe `*` operatora `null` wskaźnik jest zdefiniowane w implementacji. W szczególności, nie ma żadnej gwarancji, które zgłasza tę operację `System.NullReferenceException`.

Jeśli nieprawidłowa wartość przypisany do wskaźnika, zachowanie jednoargumentowe `*` operator jest niezdefiniowany. Wśród nieprawidłowe wartości wyłuskaniem wskaźnika przez jednoargumentowy `*` operator jest nieodpowiednio wyrównany dla typu wskazywanego adresu (Zobacz przykład w [konwersje wskaźników](unsafe-code.md#pointer-conversions)), a adres zmiennej po zakończenie okresu jego istnienia.

Do celów analizy asercję określonego przypisania, zmienna utworzone przez obliczenie wyrażenia w postaci `*P` jest uznawany za wstępnie przypisanych ([początkowo przypisana zmienne](variables.md#initially-assigned-variables)).

### <a name="pointer-member-access"></a>Dostęp do elementu członkowskiego wskaźnika

A *pointer_member_access* składa się z *primary_expression*, a następnie "`->`" token, a następnie *identyfikator* i opcjonalnie *type_argument_list*.

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

Dostępu do elementu członkowskiego wskaźnika w postaci `P->I`, `P` musi być wyrażeniem typu wskaźnika innego niż `void*`, i `I` musi oznaczają dostępny członek typu, do którego `P` punktów.

Dostęp do elementu członkowskiego wskaźnika w postaci `P->I` jest oceniane dokładnie jako `(*P).I`. Aby uzyskać opis operatora pośredniego wskaźnika (`*`), zobacz [operację wskaźnika pośredniego](unsafe-code.md#pointer-indirection). Aby uzyskać opis operatora dostępu do elementu członkowskiego (`.`), zobacz [dostęp do elementu członkowskiego](expressions.md#member-access).

W przykładzie

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

`->` operator jest używany, aby uzyskać dostęp do pól i wywołać metodę struktury za pomocą wskaźnika. Ponieważ operacja `P->I` odpowiada dokładnie `(*P).I`, `Main` metoda można tak samo dobrze zarówno zapisanych:

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a>Dostęp do elementu wskaźnika

A *pointer_element_access* składa się z *primary_no_array_creation_expression* występować wyrażenie w "`[`"i"`]`".

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

Dostępu do elementu wskaźnika w postaci `P[E]`, `P` musi być wyrażeniem typu wskaźnika innego niż `void*`, i `E` musi być wyrażeniem, które mogą być niejawnie konwertowane na `int`, `uint`, `long`, lub `ulong`.

Dostęp do elementu wskaźnika w postaci `P[E]` jest oceniane dokładnie jako `*(P + E)`. Aby uzyskać opis operatora pośredniego wskaźnika (`*`), zobacz [operację wskaźnika pośredniego](unsafe-code.md#pointer-indirection). Opis wskaźnika operator dodawania (`+`), zobacz [arytmetyki wskaźnika](unsafe-code.md#pointer-arithmetic).

W przykładzie

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

dostęp element wskaźnik jest wykorzystywany do inicjacji buforu znaku w `for` pętli. Ponieważ operacja `P[E]` odpowiada dokładnie `*(P + E)`, przykładu można tak samo dobrze zarówno zapisanych:

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

Operator dostępu do elementu wskaźnik nie sprawdza obecności liczbach błędów i zachowanie podczas uzyskiwania dostępu do liczbach element jest niezdefiniowany. To jest taka sama jak C i C++.

### <a name="the-address-of-operator"></a>Operator address-of

*Addressof_expression* składa się z handlowe "i" (`&`) następuje *unary_expression*.

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

Podane wyrażenie `E` jest typu `T` i zostanie sklasyfikowany jako zmienna ustalona ([stałe i zmienne ruchome](unsafe-code.md#fixed-and-moveable-variables)), konstrukcja `&E` oblicza adres zmiennej przez `E`. Typ wyniku jest `T*` i zostanie sklasyfikowany jako wartość. Występuje błąd kompilacji, jeśli `E` nie jest sklasyfikowany jako zmiennej, jeśli `E` zostanie sklasyfikowany jako tylko do odczytu zmiennej lokalnej, lub jeśli `E` oznacza ruchome zmiennej. W przypadku ostatniej instrukcji fixed ([instrukcji fixed](unsafe-code.md#the-fixed-statement)) może służyć do tymczasowego "naprawić" zmiennej przed uzyskaniem adresu. Jak wspomniano w [dostęp do elementu członkowskiego](expressions.md#member-access), poza konstruktora wystąpienia lub Konstruktor statyczny dla elementu struktury lub klasy, która definiuje `readonly` pola, to pole jest uważany za wartość, a nie zmienną. W efekcie adresu nie może być przyjęty. Podobnie nie może być przyjęty adres stałą.

`&` Operator nie wymaga jej argument zostanie ostatecznie przypisane, ale następujące `&` operacji zmiennej, do którego jest stosowany operator jest uznawany za zdecydowanie przypisane w ścieżce wykonywania, w której występuje operacji. Jest to faktycznie odpowiedzialność programisty, aby upewnić się, że poprawne inicjowanie zmiennej miały miejsce w tej sytuacji.

W przykładzie

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

`i` jest uznawany za zdecydowanie przypisany następujące `&i` operacji używane do zainicjowania `p`. Przypisanie do `*p` obowiązuje inicjuje `i`, ale włączenie ten proces inicjowania jest odpowiedzialny za programisty i błąd kompilacji nie może wystąpić, jeśli usunięto przypisanie.

Reguły asercję określonego przypisania dla `&` operator istnieje w taki sposób, że można uniknąć nadmiarowe inicjowanie zmiennych lokalnych. Na przykład wiele zewnętrzne interfejsy API zająć wskaźnik do struktury, która jest wypełniane przez interfejs API. Wywołania tych interfejsów API, zazwyczaj pass adresu zmiennej lokalnej struktury i bez reguły, nadmiarowe inicjowanie zmiennej struktury jest wymagana.

### <a name="pointer-increment-and-decrement"></a>Wskaźnik inkrementacja i dekrementacja

W niebezpiecznym kontekście `++` i `--` operatorów ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators) i [przedrostkowe i operatory dekrementacji](expressions.md#prefix-increment-and-decrement-operators)) mogą być stosowane do wskaźnika zmienne wszystkich typów z wyjątkiem `void*`. W związku z tym, dla każdego typu wskaźnika `T*`, niejawnie zdefiniowano następujące operatory:

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

Operatory działają tak samo jak `x + 1` i `x - 1`odpowiednio ([arytmetyki wskaźnika](unsafe-code.md#pointer-arithmetic)). Innymi słowy, do zmiennej wskaźnika typu `T*`, `++` operator dodaje `sizeof(T)` do adresem zawartym w zmiennej, a `--` odejmuje operator `sizeof(T)` z adresem zawartym w zmiennej.

Jeśli wskaźnik inkrementacyjna lub dekrementacyjna przepełnienie operacji domeny typu wskaźnika, wynik zależy od implementacji, ale są tworzone bez wyjątków.

### <a name="pointer-arithmetic"></a>Arytmetyczny wskaźnik

W niebezpiecznym kontekście `+` i `-` operatorów ([operator dodawania](expressions.md#addition-operator) i [operator odejmowania](expressions.md#subtraction-operator)) mogą być stosowane do wartości wszystkich typów wskaźnika, z wyjątkiem `void*`. W związku z tym, dla każdego typu wskaźnika `T*`, niejawnie zdefiniowano następujące operatory:

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

Podane wyrażenie `P` typu wskaźnika `T*` i wyrażenie `N` typu `int`, `uint`, `long`, lub `ulong`, wyrażenia `P + N` i `N + P` obliczeń wartość wskaźnika typu `T*` , wynikiem dodawania `N * sizeof(T)` adres podany przez `P`. Podobnie, wyrażenie `P - N` oblicza wartość wskaźnika typu `T*` , wynikiem odejmowania `N * sizeof(T)` z adresem podanym przez `P`.

Biorąc pod uwagę dwa wyrażenia `P` i `Q`, typu wskaźnika `T*`, wyrażenie `P - Q` oblicza różnicę między adresy podane przez `P` i `Q` a następnie podzielenie tej różnicy przez `sizeof(T)`. Typ wyniku jest zawsze `long`. W efekcie `P - Q` jest obliczana jako `((long)(P) - (long)(Q)) / sizeof(T)`.

Na przykład:

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

która generuje dane wyjściowe:

```
p - q = -14
q - p = 14
```

Jeśli operacji arytmetycznych wskaźnika przepełnienia domeny typu wskaźnika, wynik został obcięty w sposób zdefiniowanych w implementacji, ale są tworzone bez wyjątków.

### <a name="pointer-comparison"></a>Porównanie wskaźników

W niebezpiecznym kontekście `==`, `!=`, `<`, `>`, `<=`, i `=>` operatorów ([relacyjne i badania typu operatory](expressions.md#relational-and-type-testing-operators)) mogą być stosowane do wszystkich wartości typy wskaźników. Dostępne są następujące operatory porównania wskaźnika:

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

Ponieważ istnieje niejawna konwersja z dowolnego typu wskaźnika do `void*` operandów dowolny typ wskaźnika typu można porównać przy użyciu tych operatorów. Operatory porównania porównują adresy podane przez dwa operandy tak, jakby były one liczb całkowitych bez znaku.

### <a name="the-sizeof-operator"></a>Sizeof — operator

`sizeof` Operator zwraca liczbę bajtów zajmowane przez zmienną danego typu. Typ określony jako argument `sizeof` musi być *unmanaged_type* ([typy wskaźników](unsafe-code.md#pointer-types)).

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

Wynik `sizeof` operator jest wartość typu `int`. W przypadku niektórych wstępnie zdefiniowanych typów, `sizeof` operator daje stałą wartość, jak pokazano w poniższej tabeli.


| __Wyrażenie__   | __wynik__ |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

W przypadku wszystkich innych typów wynik `sizeof` operator jest zdefiniowane w implementacji i zostanie sklasyfikowany jako wartość, a nie jako stała.

Kolejność, w której elementy członkowskie są pakowane w struktury jest nieokreślona.

Do celów wyrównanie może istnieć bez nazwy, wypełnienie na początku struktury, w ramach struktury, a na koniec struktury. Zawartość usługi bits, używane jako uzupełnienia jest nieokreślony.

Po zastosowaniu do argumentu operacji, która ma typ struktury, wynik jest całkowita liczba bajtów w zmiennej tego typu, w tym wszelkie dopełnienie.

## <a name="the-fixed-statement"></a>Fixed — instrukcja

W niebezpiecznym kontekście *embedded_statement* ([instrukcji](statements.md)) produkcji zezwala na dodatkowe konstrukcji `fixed` instrukcję, która służy do "naprawić" ruchomych zmiennej taki sposób, że jego adres pozostaje stały czas trwania instrukcji.

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

Każdy *fixed_pointer_declarator* deklaruje zmienną lokalną o danym *pointer_type* i inicjuje tej zmiennej lokalnej o adresie obliczane przez odpowiednie *fixed_ pointer_initializer*. Zmienna lokalna zadeklarowana w `fixed` instrukcji jest dostępny we wszystkich *fixed_pointer_initializer*s pojawiają się po prawej stronie deklaracji tej zmiennej, a w polu *embedded_statement* programu `fixed` instrukcji. Zmienna lokalna zadeklarowana przez `fixed` instrukcji jest traktowane jako tylko do odczytu. Występuje błąd kompilacji, jeśli osadzona instrukcja próbuje zmodyfikować tej zmiennej lokalnej (za pośrednictwem przydziału lub `++` i `--` operatory) lub przekazać je jako `ref` lub `out` parametru.

A *fixed_pointer_initializer* może być jedną z następujących czynności:

*  Token "`&`" następuje *variable_reference* ([dokładne zasady określania asercję określonego przypisania](variables.md#precise-rules-for-determining-definite-assignment)) ze zmienną ruchome ([stałe i zmienne ruchome](unsafe-code.md#fixed-and-moveable-variables)) niezarządzany typ `T`, podany typ `T*` jest niejawnie konwertowany na typ wskaźnika, podany w `fixed` instrukcji. W tym przypadku inicjatora oblicza adres zmiennej danego, a zmienna jest gwarantowane, pozostają do ustalonego adresu na czas trwania `fixed` instrukcji.
*  Wyrażenie *array_type* elementami typu niezarządzanego `T`, podany typ `T*` jest niejawnie konwertowany na typ wskaźnika, podany w `fixed` instrukcji. W tym przypadku inicjatora oblicza adres do pierwszego elementu w tablicy, a całej tablicy jest gwarantowane, pozostają do ustalonego adresu na czas trwania `fixed` instrukcji. Jeśli wyrażenie tablicy ma wartość null lub jeśli tablica nie zawiera żadnego elementu, inicjator oblicza adresów równy zero.
*  Wyrażenie typu `string`, podany typ `char*` jest niejawnie konwertowany na typ wskaźnika, podany w `fixed` instrukcji. W tym przypadku inicjatora oblicza adres pierwszego wystąpienia znaku w ciągu, a cały ciąg jest gwarantowane, pozostają do ustalonego adresu na czas trwania `fixed` instrukcji. Zachowanie `fixed` instrukcji jest zdefiniowane w implementacji Jeśli wyrażenie ciągu ma wartość null.
*  A *simple_name* lub *member_access* , odwołuje się do elementu członkowskiego buforu o ustalonym rozmiarze zmiennej ruchome podany typ elementu członkowskiego buforu o ustalonym rozmiarze jest niejawnie konwertowany na typ wskaźnika, biorąc pod uwagę w `fixed` instrukcji. W tym przypadku inicjatora oblicza wskaźnik do pierwszego elementu bufor o ustalonym rozmiarze ([stałych buforów rozmiar w wyrażeniach](unsafe-code.md#fixed-size-buffers-in-expressions)), i bufor o ustalonym rozmiarze musi pozostać do ustalonego adresu na czas trwania `fixed`instrukcji.

Dla każdego adresu, obliczona przez *fixed_pointer_initializer* `fixed` instrukcji zapewnia, że zmienna odwołuje się adres nie jest przeniesienie lub usunięcia przez moduł odśmiecania pamięci w czasie trwania `fixed` instrukcji. Na przykład, jeśli adres jest obliczana przez *fixed_pointer_initializer* odwołuje się do pola obiektu lub element instancji `fixed` instrukcji gwarantuje, że zawierające wystąpienie obiektu nie został przeniesiony lub usuwane w okresie istnienia instrukcji.

Jest odpowiedzialny za programisty, aby upewnić się, że wskaźniki utworzonych przez `fixed` instrukcje nie przeżywa poza wykonanie tych instrukcji. Na przykład, gdy wskaźniki utworzony przez `fixed` instrukcje są przekazywane do zewnętrzne interfejsy API, jest programisty ponosić odpowiedzialność za zapewnienie zachowanie za pomocą interfejsów API Brak pamięci te wskaźniki.

Naprawiono obiektów może spowodować fragmentację sterty (ponieważ nie można ich przenieść). Z tego powodu należy ustalić obiektów tylko wtedy, gdy jest to absolutnie konieczne i następnie dla najkrótszej ilość czasu, to możliwe.

Przykład

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

Pokazuje kilka zastosowań `fixed` instrukcji. Pierwsza instrukcja poprawki i uzyskuje adres pole statyczne, druga instrukcja poprawki i uzyskuje adres pola wystąpienia i trzecią instrukcję poprawki i uzyskuje adres elementu tablicy. W każdym przypadku byłoby błędem jest użycie zwykłych `&` operatora, ponieważ zmienne są klasyfikowane jako ruchome zmienne.

Czwarty `fixed` instrukcja w powyższym przykładzie generuje podobny efekt, w trzeciej.

Ten przykład `fixed` używa instrukcji `string`:

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

W niebezpiecznym kontekście elementów tablicy jednowymiarowej tablic są przechowywane w kolejności rosnącej indeksu, począwszy od indeksu `0` i kończąc indeksu `Length - 1`. Dla wielowymiarowych tablic, tablicy, które elementy są przechowywane w taki sposób, że indeksy po prawej stronie wymiaru zwiększa się najpierw, a następnie następnego left wymiaru i tak dalej po lewej stronie. W ramach `fixed` instrukcję, która uzyskuje wskaźnik `p` na wystąpienie tablicy `a`, wartości wskaźnika, począwszy od `p` do `p + a.Length - 1` reprezentują adresy elementów w tablicy. Podobnie, zmienne w zakresie od `p[0]` do `p[a.Length - 1]` reprezentują elementy bieżącej tablicy. Biorąc pod uwagę sposób, w którym przechowywane są tablicami, mogą traktujemy tablicę żadnego wymiaru tak, jakby była liniowego.

Na przykład:

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

która generuje dane wyjściowe:

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

W przykładzie

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

`fixed` instrukcja zostaje użyty do naprawy tablicy, dzięki czemu jego adresu może być przekazywany do metody, która przyjmuje wskaźnik.

W przykładzie:

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

fixed — instrukcja zostaje użyty do naprawy bufor o ustalonym rozmiarze, struktury, dzięki czemu jego adresu może służyć jako wskaźnik.

A `char*` wartości generowane przez rozwiązywanie wystąpieniu ciągu zawsze wskazuje na ciąg zakończony znakiem null. W instrukcji fixed, który uzyskuje wskaźnik `p` do wystąpienia ciągu `s`, wartości wskaźnika, począwszy od `p` do `p + s.Length - 1` reprezentują adresy znaków w ciągu i wartość wskaźnika `p + s.Length` zawsze wskazuje znak null (znak wartości `'\0'`).

Modyfikowanie obiektów typu zarządzanego przez stały wskaźniki mogą powoduje niezdefiniowane zachowanie. Na przykład ponieważ ciągów są niezmienne, to programisty ponosić odpowiedzialność za zapewnienie znaków odwołuje się wskaźnik do ciągu stałych nie są modyfikowane.

Automatyczne zakończenia null ciągów jest szczególnie przydatna podczas wywoływania zewnętrzne interfejsy API, który oczekiwać ciągi "Stylu C". Należy jednak zauważyć, że wystąpieniu ciągu może zawierać znaków o wartości null. Jeśli takie znaki null są zainstalowane, ciąg pojawi obcięte traktowane jako zakończony znakiem null `char*`.

## <a name="fixed-size-buffers"></a>Bufory o ustalonym rozmiarze

Bufory o ustalonym rozmiarze są używane do deklarowania "Stylu C" w tekście tablice jako elementy członkowskie struktury i są szczególnie przydatne w przypadku powiązania z niezarządzanych interfejsów API.

### <a name="fixed-size-buffer-declarations"></a>Deklaracje buforu o ustalonym rozmiarze

A ***ustalony rozmiar buforu*** jest element członkowski, który reprezentuje magazyn dla bufora o stałej długości zmiennych danego typu. Deklaracja buforu o ustalonym rozmiarze wprowadza bufory o ustalonym rozmiarze co najmniej jeden element danego typu. Bufory o ustalonym rozmiarze są dozwolone tylko w deklaracjach struktury i mogą występować tylko w kontekstach unsafe ([niebezpieczny kontekst](unsafe-code.md#unsafe-contexts)).

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

Deklaracja buforu o ustalonym rozmiarze mogą obejmować zestaw atrybutów ([atrybuty](attributes.md)), `new` modyfikator ([Modyfikatory](classes.md#modifiers)), prawidłowej kombinacji modyfikatory dostępu cztery ([typu Parametry i ograniczenia](classes.md#type-parameters-and-constraints)) i `unsafe` modyfikator ([niebezpieczny kontekst](unsafe-code.md#unsafe-contexts)). Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych przez deklarację buforu o ustalonym rozmiarze. Jest to błąd dla tego samego modyfikator pojawią się wiele razy w deklaracji buforu o ustalonym rozmiarze.

Deklaracja buforu o ustalonym rozmiarze jest niedozwolona w obejmują `static` modyfikator.

Typ elementu buforu deklaracji buforu o ustalonym rozmiarze Określa typ elementu bufory wprowadzonych przez tę deklarację. Typ elementu buforu musi być jednym z wstępnie zdefiniowanych typów `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, lub `bool`.

Typ elementu buforu następuje lista deklaratorów buforu o ustalonym rozmiarze, każdy z nich wprowadzono nowy element członkowski. Deklarator buforu o ustalonym rozmiarze składa się z identyfikatora i nazwy elementu członkowskiego, następuje ujęte w wyrażeniu stałym `[` i `]` tokenów. Wyrażenie stałe wskazuje, że liczba elementów w elemencie członkowskim, wynikające z tego deklaratora buforu o ustalonym rozmiarze. Typ stałego wyrażenia musi być jawnie przekonwertować na typ `int`, a wartość musi być dodatnią liczbą całkowitą od zera.

Elementy bufor o ustalonym rozmiarze są musi być określone sekwencyjnie w pamięci.

Deklaracja buforu o ustalonym rozmiarze, która deklaruje wiele bufory o ustalonym rozmiarze jest odpowiednikiem w wielu deklaracjach deklarację buforu jednego o stałym rozmiarze z tych atrybutów i typy elementów. Na przykład

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

odpowiada wyrażeniu

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a>Bufory o ustalonym rozmiarze w wyrażeniach

Wyszukiwanie elementu członkowskiego ([operatory](expressions.md#operators)) o stałym rozmiarze buforu element członkowski będzie kontynuowane tak samo jak wyszukać składowej pola.

Bufor o ustalonym rozmiarze można odwoływać się do usługi za pomocą wyrażenia *simple_name* ([wnioskowanie o typie](expressions.md#type-inference)) lub *member_access* ([Sprawdzanie czasu kompilacji Rozpoznanie przeciążenia dynamiczne](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Gdy członek buforu o ustalonym rozmiarze jest określany jako prostą nazwą, efekt jest taki sam jak dostęp do elementu członkowskiego w postaci `this.I`, gdzie `I` należy buforu o ustalonym rozmiarze.

Dostępu do elementu członkowskiego w postaci `E.I`, jeśli `E` jest typ struktury i wyszukiwania elementu członkowskiego `I` , następnie identyfikuje typ struktury elementu członkowskiego o stałym rozmiarze, `E.I` jest obliczane sklasyfikowanych w następujący sposób:

*  Jeśli wyrażenie `E.I` nie występuje w niebezpiecznym kontekście, wystąpi błąd kompilacji.
*  Jeśli `E` jest klasyfikowana jako wartość, wystąpi błąd kompilacji.
*  W przeciwnym razie, jeśli `E` jest zmienną ruchome ([stałe i zmienne ruchome](unsafe-code.md#fixed-and-moveable-variables)) i wyrażenie `E.I` nie *fixed_pointer_initializer* ([stałe Instrukcja](unsafe-code.md#the-fixed-statement)), wystąpi błąd kompilacji.
*  W przeciwnym razie `E` odwołuje się zmienna ustalona i wynik wyrażenia to wskaźnik do pierwszego elementu członkowskiego buforu o ustalonym rozmiarze `I` w `E`. Wynik jest typu `S*`, gdzie `S` jest typ elementu `I`i jest klasyfikowana jako wartość.

Kolejne elementy buforu o ustalonym rozmiarze można uzyskać dostęp przy użyciu operacji wskaźnika od pierwszego elementu. W odróżnieniu od dostęp do tablic dostęp do elementów bufor o ustalonym rozmiarze niebezpieczna operacja i nie jest zaznaczone zakres.

Poniższy przykład deklaruje i wykorzystuje struktury ze składową buforu o ustalonym rozmiarze.

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a>Sprawdzanie asercję określonego przypisania

Bufory o ustalonym rozmiarze nie są obciążane sprawdzanie asercję określonego przypisania ([asercję określonego przypisania](variables.md#definite-assignment)), a członkowie buforu o ustalonym rozmiarze są ignorowane na potrzeby asercję określonego przypisania, sprawdzanie zmiennych typu struktury.

Gdy peryferyjnych zawierającego zmiennej struktury elementu członkowskiego buforu o ustalonym rozmiarze jest zmienną statyczną, zmienną instance wystąpienia klasy lub elementu tablicy, elementy bufor o ustalonym rozmiarze są automatycznie inicjowane do wartości domyślnych ([Wartości domyślne](variables.md#default-values)). We wszystkich innych przypadkach oryginalnej zawartości buforu o stałym rozmiarze jest niezdefiniowane.

## <a name="stack-allocation"></a>Alokacja stosu

W niebezpiecznym kontekście deklaracji zmiennej lokalnej ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)) może zawierać inicjatora alokacji stosu, który przydziela pamięć ze stosu wywołań.

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

*Unmanaged_type* wskazuje typ elementów, które będą przechowywane w lokalizacji nowo przydzielonego i *wyrażenie* wskazuje liczbę tych elementów. Razem te Określ rozmiar alokacji wymagane. Ponieważ rozmiar alokacji stosu nie może być ujemna, jest to błąd kompilacji, aby określić liczbę elementów jako *constant_expression* ma wartość ujemną.

Inicjator alokacji stosu w postaci `stackalloc T[E]` wymaga `T` na typ niezarządzany ([typy wskaźników](unsafe-code.md#pointer-types)) i `E` jako wyrażenie typu `int`. Konstrukcja przydziela `E * sizeof(T)` bajtów z wywołanie stosu i zwraca wskaźnik typu `T*`, do nowo przydzielonego bloku. Jeśli `E` jest liczbą ujemną, a następnie zachowanie jest niezdefiniowane. Jeśli `E` wynosi zero, a następnie odbywa się nie przydział i zwrócony wskaźnik jest zdefiniowane w implementacji. Jeśli nie ma wystarczającej ilości pamięci do przydzielenia bloku o danym rozmiarze `System.StackOverflowException` zgłaszany.

Zawartość nowo przydzielonego pamięci jest niezdefiniowane.

Inicjatory alokacji stosu nie są dozwolone w `catch` lub `finally` bloków ([instrukcjami "try"](statements.md#the-try-statement)).

Nie istnieje sposób jawnego zwolnienie pamięci przydzielonej za pomocą `stackalloc`. Wszystkie bloki pamięci na stosie, utworzony podczas wykonywania funkcji elementu członkowskiego są automatycznie odrzucane po powrocie z tej funkcji elementu członkowskiego. Odpowiada to `alloca` funkcji, rozszerzenie w implementacji języka C i C++.

W przykładzie

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

`stackalloc` inicjator jest używany w `IntToString` metodę, aby przydzielić bufor o długości 16 znaków na stosie. Rozmiar buforu jest automatycznie odrzucane po powrocie z metody.

## <a name="dynamic-memory-allocation"></a>Dynamiczna alokacja pamięci

Z wyjątkiem `stackalloc` operatora, C# zawiera nie wstępnie zdefiniowanych konstrukcje zarządzania zebranych pamięci niż pamięci. Takich usług są zazwyczaj dostarczane dzięki obsłudze bibliotek klas lub zaimportować bezpośrednio od zasadniczego systemu operacyjnego. Na przykład `Memory` klasy poniżej pokazano, jak funkcje sterty zasadniczego systemu operacyjnego może być uzyskiwany za pomocą języka C#:

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

Przykład pokazujący `Memory` klasy są podane poniżej:

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

Przykład przydziela 256 bajtów pamięci za pomocą `Memory.Alloc` i inicjuje blok pamięci z wartościami, zwiększenie od 0 do 255. Następnie przydziela tablicy typu byte 256 elementu i używa `Memory.Copy` skopiować zawartość bloku pamięci do tablicy typu byte. Na koniec bloku pamięci jest zwalniana, za pomocą `Memory.Free` i zawartości tablicy bajtów są wysyłane na konsolę.
