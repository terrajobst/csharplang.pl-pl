---
ms.openlocfilehash: dbea611280a644adc25247b9887986e129c59b68
ms.sourcegitcommit: a5e393b018b04dfa55aae0000290ca087b508495
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310361"
---
# <a name="unsafe-code"></a>Niebezpieczny kod

Język podstawowy C# , zgodnie z definicją w poprzednich działach, różni się w szczególności od C++ C i w jego pominięciu jako typu danych. Zamiast tego C# zawiera odwołania i możliwość tworzenia obiektów zarządzanych przez moduł wyrzucania elementów bezużytecznych. Ten projekt, w połączeniu z innymi funkcjami, sprawia C# , że jest to bardzo bezpieczniejszy język C++niż C lub. W języku podstawowym C# po prostu nie jest możliwe posiadanie niezainicjowanej zmiennej, wskaźnika "zawieszonego" lub wyrażenia, które indeksuje tablicę poza jej granicami. Całe kategorie błędów, które rutynowo Plague C i C++ programy są w ten sposób eliminowane.

Praktycznie każda konstrukcja typu wskaźnika w języku C lub C++ ma odpowiednik typu referencyjnego w C#, jednak istnieją sytuacje, w których dostęp do typów wskaźnika jest konieczny. Na przykład współdziałanie z podstawowym systemem operacyjnym, uzyskiwanie dostępu do urządzenia mapowanego na pamięć lub implementowanie algorytmu krytycznego czasowo może nie być możliwe ani praktyczne bez dostępu do wskaźników. Aby rozwiązać ten konieczność C# , program umożliwia zapisanie ***niebezpiecznego kodu***.

W niebezpiecznym kodzie można zadeklarować i obsłużyć wskaźniki, aby wykonać konwersje między wskaźnikami i typami całkowitymi, aby przyjąć adres zmiennych i tak dalej. W sensie pisanie niebezpiecznego kodu jest podobne do pisania kodu C w C# ramach programu.

Niebezpieczny kod jest w rzeczywistości funkcją "bezpieczna" z perspektywy deweloperów i użytkowników. Niebezpieczny kod musi być wyraźnie oznaczony modyfikatorem `unsafe`, dlatego deweloperzy nie mogą przypadkowo korzystać z niebezpiecznych funkcji, a aparat wykonywania działa w celu zapewnienia, że nie można wykonać niebezpiecznego kodu w niezaufanym środowisku.

## <a name="unsafe-contexts"></a>Niebezpieczne konteksty

Niebezpieczne funkcje programu C# są dostępne tylko w niebezpiecznych kontekstach. Niebezpieczny kontekst jest wprowadzany przez włączenie modyfikatora `unsafe` w deklaracji typu lub składowej lub przez zastosowanie *unsafe_statement*:

*  Deklaracja klasy, struktury, interfejsu lub delegata może zawierać modyfikator `unsafe`, w którym to przypadku cały tekstowy zakres tej deklaracji typu (w tym treść klasy, struktury lub interfejsu) jest traktowany jako niebezpieczny kontekst.
*  Deklaracja pola, metody, właściwości, zdarzenia, indeksatora, operatora, konstruktora wystąpienia, destruktora lub konstruktora statycznego może zawierać modyfikator `unsafe`, w takim przypadku cały tekstowy zakres tej deklaracji składowej jest traktowany jako niebezpieczny kontekst.
*  *Unsafe_statement* umożliwia użycie niebezpiecznego kontekstu w *bloku*. Cały tekstowy zakres skojarzonego *bloku* jest traktowany jako niebezpieczny kontekst.

Skojarzone produkcyjne gramatyki są pokazane poniżej.

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

w przykładzie

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

modyfikator `unsafe` określony w deklaracji struktury powoduje, że cały tekst zakresu deklaracji struktury staje się niebezpiecznym kontekstem. W ten sposób można zadeklarować pola `Left` i `Right` jako typu wskaźnika. Powyższy przykład może być również zapisany

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

W tym miejscu Modyfikatory `unsafe` w deklaracjach pola powodują, że te deklaracje są uznawane za niebezpieczne konteksty.

Poza ustanowieniem niebezpiecznego kontekstu, co pozwala na użycie typów wskaźników, modyfikator `unsafe` nie ma wpływu na typ lub element członkowski. w przykładzie

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

modyfikator `unsafe` w metodzie `F` w `A` po prostu powoduje, że tekst zakresu `F` stanie się niebezpiecznym kontekstem, w którym można używać niebezpiecznych funkcji języka. W przypadku przesłonięcia `F` w `B`nie trzeba ponownie określać modyfikatora `unsafe`--chyba że metoda `F` w programie `B` musi mieć dostęp do funkcji niebezpiecznych.

Sytuacja jest nieco inna, gdy typ wskaźnika jest częścią podpisu metody

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

Tutaj, ponieważ podpis `F`zawiera typ wskaźnika, można go napisać tylko w niebezpiecznym kontekście. Niebezpieczny kontekst można jednak wprowadzić, tworząc całą klasę jako niebezpieczną, tak jak w przypadku `A`lub przez dołączenie modyfikatora `unsafe` w deklaracji metody, tak jak w przypadku `B`.

## <a name="pointer-types"></a>Typy wskaźnika

W niebezpiecznym kontekście *Typ* ([typy](types.md)) może być *pointer_type* , a także *value_type* lub *reference_type*. Jednak *pointer_type* mogą być również używane w wyrażeniu `typeof` ([wyrażenia anonimowego tworzenia obiektów](expressions.md#anonymous-object-creation-expressions)) poza niebezpiecznym kontekstem, ponieważ takie użycie nie jest niebezpieczne.

```antlr
type_unsafe
    : pointer_type
    ;
```

*Pointer_type* jest zapisywana jako *unmanaged_type* lub `void`słowa kluczowego, po którym następuje token `*`:

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

Typ określony przed `*` w typie wskaźnika jest nazywany ***typem referent*** typu wskaźnika. Reprezentuje typ zmiennej, do której należy wartość typu wskaźnika.

W przeciwieństwie do odwołań (wartości typów odwołań), wskaźniki nie są śledzone przez moduł wyrzucania elementów bezużytecznych — Moduł wyrzucania elementów bezużytecznych nie ma wiedzy na temat wskaźników i danych, do których one wskazują. Z tego powodu wskaźnik nie może wskazywać na odwołanie lub do struktury, która zawiera odwołania, a typem referent wskaźnika musi być *unmanaged_type*.

*Unmanaged_type* to dowolny typ, który nie jest typu *reference_type* lub skonstruowany, i nie zawiera pól *reference_type* ani skonstruowanych typów na dowolnym poziomie zagnieżdżania. Innymi słowy, *unmanaged_type* jest jedną z następujących czynności:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`lub `bool`.
*  Dowolny *enum_type*.
*  Dowolny *pointer_type*.
*  Dowolny *struct_type* zdefiniowany przez użytkownika, który nie jest typem skonstruowanym i zawiera pola tylko *unmanaged_type*s.

Intuicyjna reguła do mieszania wskaźników i odwołań polega na tym, że referents odwołań (obiekty) mogą zawierać wskaźniki, ale referents wskaźników nie może zawierać odwołań.

Niektóre przykłady typów wskaźników są podane w poniższej tabeli:

| __Przykład__ | __Opis__                               |
|-------------|-----------------------------------------------|
| `byte*`     | Wskaźnik do `byte`                             |
| `char*`     | Wskaźnik do `char`                             |
| `int**`     | Wskaźnik do wskaźnika do `int`                   |
| `int*[]`    | Jednowymiarowa tablica wskaźników do `int` |
| `void*`     | Wskaźnik do nieznanego typu                       |

Dla danej implementacji wszystkie typy wskaźnika muszą mieć taki sam rozmiar i reprezentację.

W przeciwieństwie do C++języka C i, gdy wiele wskaźników jest zadeklarowanych w tej C# samej deklaracji, w `*` jest zapisywana tylko z typem podstawowym, a nie jako prefiks punctuator na każdej nazwie wskaźnika. Na przykład:

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

Wartość wskaźnika typu `T*` reprezentuje adres zmiennej typu `T`. Operator pośredni wskaźnika `*` ([pośredni wskaźnik](unsafe-code.md#pointer-indirection)) może być używany w celu uzyskania dostępu do tej zmiennej. Na przykład, uwzględniając zmienną `P` typu `int*`, wyrażenie `*P` oznacza zmienną `int` znalezioną pod adresem zawartym w `P`.

Podobnie jak odwołanie do obiektu, wskaźnik może być `null`. Zastosowanie operatora pośredniego do wskaźnika `null` skutkuje zachowaniem zdefiniowanym przez implementację. Wskaźnik o wartości `null` jest reprezentowany przez wszystkie bity-zero.

Typ `void*` reprezentuje wskaźnik do nieznanego typu. Ponieważ typ referent jest nieznany, operator pośredni nie może zostać zastosowany do wskaźnika typu `void*`, ani nie można wykonać żadnych operacji arytmetycznych na tym wskaźniku. Jednak wskaźnik typu `void*` może być rzutowany do dowolnego innego typu wskaźnika (i odwrotnie).

Typy wskaźnika są oddzielną kategorią typów. W przeciwieństwie do typów referencyjnych i typów wartości, typy wskaźnika nie dziedziczą po `object` i nie istnieją konwersje między typami wskaźnika i `object`. W szczególności opakowanie i rozpakowywanie ([opakowanie i](types.md#boxing-and-unboxing)rozpakowywanie) nie jest obsługiwane w przypadku wskaźników. Jednak konwersje są dozwolone między różnymi typami wskaźników i między typami wskaźnika a typami całkowitymi. Jest to opisane w temacie [konwersje wskaźnika](unsafe-code.md#pointer-conversions).

Nie można użyć *pointer_type* jako argumentu typu ([konstruowane typy](types.md#constructed-types)) i wnioskowania typu ([wnioskowanie o typie](expressions.md#type-inference)) kończy się niepowodzeniem dla wywołań metod ogólnych, które zostałyby wywnioskowane argumentu typu jako typ wskaźnika.

*Pointer_type* może być używany jako typ pola nietrwałego ([pola nietrwałe](classes.md#volatile-fields)).

Chociaż wskaźniki mogą być przesyłane jako parametry `ref` lub `out`, może to spowodować niezdefiniowane zachowanie, ponieważ wskaźnik może być również ustawiony tak, aby wskazywał zmienną lokalną, która już nie istnieje, gdy wywołana metoda zwróci wartość lub stała obiektu, do którego jest on używany, nie jest już naprawiona. Na przykład:

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

Metoda może zwracać wartość pewnego typu, a ten typ może być wskaźnikiem. Na przykład, gdy nastąpiło wskaźnik do ciągłej sekwencji `int`s, tej sekwencji elementów i innej wartości `int`, następująca metoda zwraca adres tej wartości w tej sekwencji, jeśli występuje dopasowanie; w przeciwnym razie zwraca `null`:

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

W niebezpiecznym kontekście kilka konstrukcji jest dostępnych do obsługi wskaźników:

*  Operator `*` może służyć do wykonywania pośredniego wskaźnika ([pośredni wskaźnik](unsafe-code.md#pointer-indirection)).
*  Operatora `->` można używać w celu uzyskania dostępu do elementu członkowskiego struktury za pomocą wskaźnika ([dostęp do elementu członkowskiego wskaźnika](unsafe-code.md#pointer-member-access)).
*  Operator `[]` może służyć do indeksowania wskaźnika ([dostęp do elementu wskaźnika](unsafe-code.md#pointer-element-access)).
*  Operator `&` może służyć do uzyskania adresu zmiennej ([operator adresu](unsafe-code.md#the-address-of-operator)).
*  Operatory `++` i `--` mogą służyć do zwiększania i zmniejszania wskaźników ([przyrostu wskaźnika i zmniejszania](unsafe-code.md#pointer-increment-and-decrement)).
*  Operatory `+` i `-` mogą służyć do wykonywania operacji arytmetycznych wskaźnika ([arytmetyki wskaźnika](unsafe-code.md#pointer-arithmetic)).
*  Operatory `==`, `!=`, `<`, `>`, `<=`i `=>` mogą służyć do porównywania wskaźników ([porównanie wskaźników](unsafe-code.md#pointer-comparison)).
*  Operatora `stackalloc` można użyć do przydzielenia pamięci ze stosu wywołań ([bufory o stałym rozmiarze](unsafe-code.md#fixed-size-buffers)).
*  Instrukcji `fixed` można użyć do tymczasowej naprawy zmiennej, aby można było uzyskać jej adres ([ustalona instrukcja](unsafe-code.md#the-fixed-statement)).

## <a name="fixed-and-moveable-variables"></a>Zmienne stałe i ruchome

Operator address-of ([operator address-of](unsafe-code.md#the-address-of-operator)) i instrukcja `fixed` ([stała instrukcja](unsafe-code.md#the-fixed-statement)) dzielą zmienne na dwie kategorie: ***stałe zmienne*** i ***ruchome zmienne***.

Stałe zmienne znajdują się w lokalizacjach magazynu, które nie mają wpływ na działanie modułu wyrzucania elementów bezużytecznych. (Przykładowe zmienne stałe obejmują zmienne lokalne, parametry wartości i zmienne utworzone przez wyłuskanie wskaźników). Z drugiej strony, ruchome zmienne znajdują się w lokalizacjach przechowywania, które podlegają relokacji lub usuwaniu przez moduł wyrzucania elementów bezużytecznych. (Przykłady ruchomych zmiennych zawierają pola w obiektach i elementy tablic).

Operator `&` ([operator address-of](unsafe-code.md#the-address-of-operator)) zezwala na uzyskanie adresu stałej zmiennej bez ograniczeń. Jednak ze względu na to, że ruchoma zmienna podlega relokacji lub utylizacji przez moduł wyrzucania elementów bezużytecznych, adres ruchomej zmiennej można uzyskać tylko przy użyciu instrukcji `fixed` ([stałej instrukcji](unsafe-code.md#the-fixed-statement)) i ten adres pozostaje prawidłowy tylko dla czasu trwania instrukcji `fixed`.

W dokładnych warunkach Zmienna stała jest jedną z następujących:

*  Zmienna będąca wynikiem *simple_name* ([proste nazwy](expressions.md#simple-names)), która odwołuje się do zmiennej lokalnej lub parametru wartości, chyba że zmienna jest przechwytywana przez funkcję anonimową.
*  Zmienna będąca wynikiem *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) `V.I`formularza, gdzie `V` jest stałą zmienną *struct_type*.
*  Zmienna będąca wynikiem *pointer_indirection_expression* ([pośredni wskaźnik](unsafe-code.md#pointer-indirection)) formularza `*P`, *pointer_member_access* ([dostęp do elementu członkowskiego wskaźnika](unsafe-code.md#pointer-member-access)) w `P->I`lub *pointer_element_access* ([wskaźnik dostępu do elementu](unsafe-code.md#pointer-element-access)) formularza `P[E]`.

Wszystkie inne zmienne są klasyfikowane jako ruchome zmienne.

Należy zauważyć, że pole statyczne jest sklasyfikowane jako ruchoma zmienna. Należy również zauważyć, że parametr `ref` lub `out` jest klasyfikowany jako ruchoma zmienna, nawet jeśli argument określony dla parametru jest zmienną stałą. Na koniec należy zauważyć, że zmienna utworzona przez odwołujący się do wskaźnika jest zawsze sklasyfikowana jako stała zmienna.

## <a name="pointer-conversions"></a>Konwersje wskaźników

W niebezpiecznym kontekście zestaw dostępnych konwersji niejawnych ([konwersje niejawne](conversions.md#implicit-conversions)) jest rozszerzony w celu uwzględnienia następujących niejawnych konwersji wskaźnika:

*  Z dowolnego *pointer_type* typu `void*`.
*  Z literału `null` do dowolnego *pointer_type*.

Ponadto w przypadku niebezpiecznego kontekstu zestaw dostępnych konwersji jawnych ([Konwersje jawne](conversions.md#explicit-conversions)) jest rozszerzany w celu uwzględnienia następujących jawnych konwersji wskaźnika:

*  Z dowolnego *pointer_type* do dowolnego innego *pointer_type*.
*  Z `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`lub `ulong` do dowolnej *pointer_type*.
*  Z dowolnego *pointer_type* do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`lub `ulong`.

Na koniec w niebezpiecznym kontekście zestaw standardowych konwersji niejawnych ([konwersje niejawne standardowe](conversions.md#standard-implicit-conversions)) zawiera następującą konwersję wskaźnika:

*  Z dowolnego *pointer_type* typu `void*`.

Konwersje między dwoma typami wskaźnika nigdy nie zmieniają rzeczywistej wartości wskaźnika. Inaczej mówiąc, konwersja z jednego typu wskaźnika do innego nie ma wpływu na adres źródłowy określony przez wskaźnik.

Gdy jeden typ wskaźnika jest konwertowany na inny, jeśli wynikowy wskaźnik nie jest prawidłowo wyrównany dla typu wskazanego, zachowanie jest niezdefiniowane, jeśli wynik jest wywołujący. Ogólnie pojęcie "prawidłowo wyrównane" jest przechodnie: Jeśli wskaźnik do typu `A` jest prawidłowo wyrównany dla wskaźnika do typu `B`, który z kolei jest prawidłowo wyrównany dla wskaźnika do typu `C`, a następnie wskaźnik do typu `A` jest prawidłowo wyrównany dla wskaźnika do typu `C`.

Rozważmy następujący przypadek, w którym zmienna z jednym typem jest dostępna za pośrednictwem wskaźnika do innego typu:

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

Gdy typ wskaźnika jest konwertowany na wskaźnik do typu Byte, wynik wskazuje najniższy przyjmowany bajt zmiennej. Kolejne przyrosty wyniku, do rozmiaru zmiennej, dają wskaźniki do pozostałych bajtów tej zmiennej. Na przykład Poniższa metoda wyświetla każdy osiem bajtów jako wartość szesnastkową:

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

Oczywiście wygenerowane dane wyjściowe są zależne od bajtów.

Mapowania między wskaźnikami i liczbami całkowitymi są zdefiniowane w implementacji. Jednakże w przypadku 32 * i 64-bitowych architektur procesora z liniową przestrzenią adresową Konwersje wskaźników do lub z typów całkowitych zwykle zachowują się dokładnie tak, jak konwersje wartości `uint` lub `ulong`, do lub z tych typów całkowitych.

### <a name="pointer-arrays"></a>Tablice wskaźników

W niebezpiecznym kontekście tablice wskaźników mogą być skonstruowane. W tablicach wskaźników są dozwolone tylko niektóre konwersje, które mają zastosowanie do innych typów tablicowych:

*  Niejawna konwersja odwołań ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions)) z dowolnych *array_type* do `System.Array` i interfejsów, które implementuje również ma zastosowanie do tablic wskaźników. Jednak każda próba uzyskania dostępu do elementów tablicy za pomocą `System.Array` lub interfejsów, które implementuje, spowoduje wyjątek w czasie wykonywania, ponieważ typy wskaźnika nie są konwertowane na `object`.
*  Niejawne i jawne konwersje referencyjne ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions), [jawne konwersje odwołań](conversions.md#explicit-reference-conversions)) z jednowymiarowego typu tablicy `S[]` do `System.Collections.Generic.IList<T>` i jego generyczne interfejsy podstawowe nigdy nie mają zastosowania do tablic wskaźników, ponieważ typy wskaźnika nie mogą być używane jako argumenty typu i nie istnieją konwersje z typów wskaźnika do typów niewskaźnikowych.
*  Jawna konwersja odwołań ([jawne konwersje odwołań](conversions.md#explicit-reference-conversions)) z `System.Array` i interfejsy, które implementuje do dowolnego *array_type* mają zastosowanie do tablic wskaźników.
*  Jawne konwersje referencyjne ([jawne konwersje odwołań](conversions.md#explicit-reference-conversions)) z `System.Collections.Generic.IList<S>` i ich interfejsy podstawowe do jednowymiarowego typu tablicy `T[]` nigdy nie mają zastosowania do tablic wskaźników, ponieważ typy wskaźnika nie mogą być używane jako argumenty typu i nie istnieją konwersje z typów wskaźnika do typów niewskaźnikowych.

Ograniczenia te oznaczają, że nie można zastosować rozwinięcia instrukcji `foreach` w odniesieniu do tablic opisanych w [instrukcji foreach](statements.md#the-foreach-statement) w tablicach wskaźników. Zamiast tego instrukcja foreach formularza

```csharp
foreach (V v in x) embedded_statement
```

gdy typ `x` jest typem tablicy `T[,,...,]`, `N` jest liczbą wymiarów minus 1, `T` lub `V` jest typem wskaźnika, jest rozwinięty przy użyciu zagnieżdżonych pętli for w następujący sposób:

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

Zmienne `a`, `i0`, `i1`,..., `iN` nie są widoczne dla `x` ani do *embedded_statement* ani do innych kodów źródłowych programu. Zmienna `v` jest tylko do odczytu w instrukcji osadzonej. Jeśli nie istnieje jawna konwersja ([Konwersje wskaźników](unsafe-code.md#pointer-conversions)) z `T` (typ elementu) do `V`, zostanie wygenerowany błąd i nie są podejmowane żadne dalsze kroki. Jeśli `x` ma wartość `null`, zostanie zgłoszony `System.NullReferenceException` w czasie wykonywania.

## <a name="pointers-in-expressions"></a>Wskaźniki w wyrażeniach

W niebezpiecznym kontekście wyrażenie może dawać wynik typu wskaźnika, ale poza niebezpiecznym kontekstem, jest to błąd czasu kompilacji dla wyrażenia, które ma być typu wskaźnika. W konkretnych warunkach, poza niebezpiecznym kontekstem, występuje błąd kompilacji, jeśli *jakiekolwiek simple_name* ([proste nazwy](expressions.md#simple-names)), *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)), *invocation_expression* ([wyrażenia wywołania](expressions.md#invocation-expressions)) lub *element_access* ([dostęp do elementu](expressions.md#element-access)) jest typu wskaźnika.

W niebezpiecznym kontekście produkcje *primary_no_array_creation_expression* ([wyrażenia podstawowe](expressions.md#primary-expressions)) i *unary_expression* ([operatory jednoargumentowe](expressions.md#unary-operators)) zezwalają na następujące dodatkowe konstrukcje:

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

Te konstrukcje są opisane w poniższych sekcjach. Pierwszeństwo i łączność operatorów niebezpiecznych są implikowane przez gramatykę.

### <a name="pointer-indirection"></a>Pośredni wskaźnik

*Pointer_indirection_expression* składa się z gwiazdki (`*`), po którym następuje *unary_expression*.

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

Operator jednoargumentowy `*` oznacza pośrednią wskaźnik i służy do uzyskania zmiennej, do której wskazuje wskaźnik. Wynik oceny `*P`, gdzie `P` jest wyrażeniem typu wskaźnika `T*`, jest zmienną typu `T`. Jest to błąd czasu kompilacji, aby zastosować jednoargumentowy operator `*` do wyrażenia typu `void*` lub do wyrażenia, które nie jest typu wskaźnika.

Efekt zastosowania jednoargumentowego operatora `*` do wskaźnika `null` jest zdefiniowany przez implementację. W szczególności nie ma gwarancji, że ta operacja zgłasza `System.NullReferenceException`.

Jeśli do wskaźnika przypisano nieprawidłową wartość, zachowanie jednoargumentowego operatora `*` jest niezdefiniowane. Wśród nieprawidłowych wartości w celu wyłuskania wskaźnika przez jednoargumentowy operator `*` są niewłaściwie wyrównane dla typu wskazywanego (Zobacz przykład w przypadku [konwersji wskaźników](unsafe-code.md#pointer-conversions)) i adres zmiennej po zakończeniu okresu istnienia.

W celach analizy z określonym przypisaniem zmienna utworzona przez obliczenie wyrażenia `*P` jest uważana za wstępnie przypisaną ([wstępnie przypisane zmienne](variables.md#initially-assigned-variables)).

### <a name="pointer-member-access"></a>Dostęp do elementu członkowskiego wskaźnika

*Pointer_member_access* składa się z *primary_expression*, po którym następuje token "`->`", po którym następuje *Identyfikator* i opcjonalny *type_argument_list*.

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

W celu uzyskania dostępu do elementu członkowskiego wskaźnika `P->I`, `P` musi być wyrażeniem typu wskaźnika innego niż `void*`, a `I` musi zwrócić uwagę na dostępną składową typu, do której `P` punktów.

Dostęp do elementu członkowskiego wskaźnika `P->I` jest oceniany dokładnie jako `(*P).I`. Aby uzyskać opis operatora pośredniego wskaźnika (`*`), zobacz [wskaźnik pośredni wskaźnika](unsafe-code.md#pointer-indirection). Aby uzyskać opis operatora dostępu do elementów członkowskich (`.`), zobacz [dostęp do elementu członkowskiego](expressions.md#member-access).

w przykładzie

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

operator `->` służy do uzyskiwania dostępu do pól i wywoływania metody struktury za pomocą wskaźnika. Ponieważ `P->I` operacji jest dokładnie równoważne `(*P).I`, Metoda `Main` mogła być również zapisywana:

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

*Pointer_element_access* składa się z *primary_no_array_creation_expression* po którym następuje wyrażenie ujęte w "`[`" i "`]`".

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

W celu uzyskania dostępu do elementu wskaźnika `P[E]`, `P` musi być wyrażeniem typu wskaźnika innego niż `void*`, a `E` musi być wyrażeniem, które może być niejawnie konwertowane na `int`, `uint`, `long`lub `ulong`.

Dostęp do elementu wskaźnika w formularzu `P[E]` jest oceniane dokładnie jako `*(P + E)`. Aby uzyskać opis operatora pośredniego wskaźnika (`*`), zobacz [wskaźnik pośredni wskaźnika](unsafe-code.md#pointer-indirection). Aby uzyskać opis operatora dodawania wskaźnika (`+`), zobacz [arytmetyka wskaźnika](unsafe-code.md#pointer-arithmetic).

w przykładzie

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

dostęp do elementu wskaźnika jest używany do inicjowania buforu znaków w pętli `for`. Ponieważ `P[E]` operacji jest dokładnie równoważne `*(P + E)`, to przykład może być równie dobrze zapisany:

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

Operator dostępu do elementów wskaźnika nie sprawdza występowania błędów poza granicami, a zachowanie podczas uzyskiwania dostępu do elementu poza granicami jest niezdefiniowane. Jest to taka sama jak C i C++.

### <a name="the-address-of-operator"></a>Operator address-of

*Addressof_expression* składa się ze znaku handlowego "i" (`&`), po którym następuje *unary_expression*.

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

Podano `E` wyrażenia, które jest typu `T` i są klasyfikowane jako zmienna stała ([zmienne stałe i ruchome](unsafe-code.md#fixed-and-moveable-variables)), konstrukcja `&E` oblicza adres zmiennej określonej przez `E`. Typ wyniku jest `T*` i jest sklasyfikowany jako wartość. Błąd czasu kompilacji występuje, jeśli `E` nie jest sklasyfikowany jako zmienna, jeśli `E` jest sklasyfikowany jako zmienna lokalna tylko do odczytu lub w przypadku, gdy `E` wskazuje ruchomą zmienną. W ostatnim przypadku, stała instrukcja ([stała instrukcja](unsafe-code.md#the-fixed-statement)) może służyć do tymczasowego "naprawy" zmiennej przed uzyskaniem jej adresu. Jak określono w [dostępie do elementu członkowskiego](expressions.md#member-access), poza konstruktorem wystąpienia lub konstruktorem statycznym dla struktury lub klasy, która definiuje pole `readonly`, to pole jest uznawane za wartość, a nie zmienną. W związku z tym adres nie może zostać pobrany. Analogicznie, nie można pobrać adresu stałej.

Operator `&` nie wymaga, aby jego argument był ostatecznie przypisany, ale po operacji `&` zmienna, do której zastosowano operator, jest uważana za ostatecznie przypisaną w ścieżce wykonywania, w której występuje operacja. Programista jest odpowiedzialny za zagwarantowanie, że w tej sytuacji w rzeczywistości odbywa się poprawna inicjalizacja zmiennej.

w przykładzie

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

`i` jest uznawany za ostatecznie przypisany po operacji `&i` użytej do zainicjowania `p`. Przypisanie do `*p` w efekcie jest inicjowane `i`, ale dołączenie tej inicjalizacji jest odpowiedzialne za programistę i nie wystąpi błąd czasu kompilacji, jeśli przypisanie zostało usunięte.

Reguły o określonym wykorzystaniu dla operatora `&` istnieją w taki sposób, że można uniknąć nadmiarowego inicjowania zmiennych lokalnych. Na przykład wiele zewnętrznych interfejsów API przyjmuje wskaźnik do struktury, która jest wypełniana przez interfejs API. Wywołania takich interfejsów API zwykle przekazują adres lokalnej zmiennej struktury i bez reguły, nadmiarowe inicjowanie zmiennej struktury będzie wymagane.

### <a name="pointer-increment-and-decrement"></a>Zwiększenie i zmniejszenie wskaźnika

W niebezpiecznym kontekście operatory `++` i `--` (operatory[przyrostu i zmniejszania](expressions.md#postfix-increment-and-decrement-operators) wartości [prefiksu i zmniejszania](expressions.md#prefix-increment-and-decrement-operators)przyrostu) mogą być stosowane do zmiennych wskaźnikowych wszystkich typów z wyjątkiem `void*`. W tym celu dla każdego typu wskaźnika `T*`następujące operatory są niejawnie zdefiniowane:

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

Operatory generują te same wyniki co `x + 1` i `x - 1`, odpowiednio ([arytmetyka wskaźnika](unsafe-code.md#pointer-arithmetic)). Innymi słowy dla zmiennej wskaźnika typu `T*`, operator `++` dodaje `sizeof(T)` do adresu zawartego w zmiennej, a operator `--` odejmuje `sizeof(T)` od adresu zawartego w zmiennej.

Jeśli operacja zwiększania lub zmniejszania wskaźnika przepełnił domenę typu wskaźnika, wynik jest zdefiniowany przez implementację, ale nie są generowane żadne wyjątki.

### <a name="pointer-arithmetic"></a>Arytmetyczny wskaźnik

W niebezpiecznym kontekście operatory `+` i `-` (operator[dodawania](expressions.md#addition-operator) i [operator odejmowania](expressions.md#subtraction-operator)) mogą być stosowane do wartości wszystkich typów wskaźnika, z wyjątkiem `void*`. W tym celu dla każdego typu wskaźnika `T*`następujące operatory są niejawnie zdefiniowane:

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

Podaje wyrażenie `P` typu wskaźnika `T*` i wyrażenia `N` typu `int`, `uint`, `long`lub `ulong`, wyrażenia `P + N` i `N + P` obliczają wartość wskaźnika typu `T*`, która powoduje dodanie `N * sizeof(T)` do adresu podaną przez `P`. Analogicznie, wyrażenie `P - N` oblicza wartość wskaźnika typu `T*`, które wynikają z odejmowania `N * sizeof(T)` od adresu podanych przez `P`.

Uwzględniając dwa wyrażenia, `P` i `Q`, typu wskaźnika `T*`, wyrażenie `P - Q` oblicza różnicę między adresami podaną przez `P` i `Q`, a następnie dzieli tę różnicę na `sizeof(T)`. Typ wyniku jest zawsze `long`. W efekcie `P - Q` jest obliczana jako `((long)(P) - (long)(Q)) / sizeof(T)`.

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

co daje wynik:

```console
p - q = -14
q - p = 14
```

Jeśli operacja arytmetyczna wskaźnika przepełni domenę typu wskaźnika, wynik jest obcinany w sposób zdefiniowany dla implementacji, ale nie są generowane żadne wyjątki.

### <a name="pointer-comparison"></a>Porównanie wskaźników

W niebezpiecznym kontekście operatory `==`, `!=`, `<`, `>`, `<=`i `=>` ([Operatory relacyjne i typu-test](expressions.md#relational-and-type-testing-operators)) mogą być stosowane do wartości wszystkich typów wskaźnika. Operatory porównania wskaźników są następujące:

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

Ponieważ niejawna konwersja istnieje z dowolnego typu wskaźnika do typu `void*`, operandy dowolnego typu wskaźnika można porównać przy użyciu tych operatorów. Operatory porównania porównują adresy nadawane przez dwa operandy, tak jakby były to liczby całkowite bez znaku.

### <a name="the-sizeof-operator"></a>Operator sizeof

Operator `sizeof` zwraca liczbę bajtów zajmowanych przez zmienną danego typu. Typ określony jako operand do `sizeof` musi być *unmanaged_type* ([typami wskaźników](unsafe-code.md#pointer-types)).

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

Wynik operatora `sizeof` jest wartością typu `int`. W przypadku niektórych wstępnie zdefiniowanych typów operator `sizeof` zwraca wartość stałą, jak pokazano w poniższej tabeli.


| __Expression__   | __wynik__ |
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

Dla wszystkich innych typów wynik operatora `sizeof` jest zdefiniowany przez implementację i jest sklasyfikowany jako wartość, a nie jako stała.

Kolejność, w której elementy członkowskie są pakowane w strukturę, nie została określona.

Na potrzeby wyrównania może istnieć nienazwane uzupełnienie na początku struktury, w obrębie struktury i na końcu struktury. Zawartość bitów użytych jako uzupełnienie jest nieokreślona.

W przypadku zastosowania do operandu, który ma typ struktury, wynik jest łączną liczbą bajtów w zmiennej tego typu, włącznie z dopełnieniem.

## <a name="the-fixed-statement"></a>Fixed — Instrukcja

W niebezpiecznym kontekście produkcja *embedded_statement* ([instrukcje](statements.md)) dopuszczają dodatkową konstrukcję, instrukcję `fixed`, która jest używana do "naprawy" ruchomej zmiennej w taki sposób, że jej adres pozostaje stałą na czas trwania instrukcji.

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

Każda *fixed_pointer_declarator* deklaruje zmienną lokalną danego *pointer_type* i inicjuje zmienną lokalną o adresie obliczonym przez odpowiedni *fixed_pointer_initializer*. Zmienna lokalna zadeklarowana w instrukcji `fixed` jest dostępna w dowolnych *fixed_pointer_initializer*s występujących po prawej stronie deklaracji tej zmiennej, a także w *embedded_statement* instrukcji `fixed`. Zmienna lokalna zadeklarowana przez instrukcję `fixed` jest traktowana jako tylko do odczytu. Błąd czasu kompilacji występuje, jeśli osadzona instrukcja próbuje zmodyfikować tę zmienną lokalną (poprzez przypisanie lub `++` i operatory `--`) lub przekazać ją jako parametr `ref` lub `out`.

*Fixed_pointer_initializer* może być jedną z następujących:

*  Token "`&`", po którym następuje *variable_reference* ([precyzyjne reguły określania typu przypisania](variables.md#precise-rules-for-determining-definite-assignment)) do zmiennej ruchomej ([zmienne stałe i ruchome](unsafe-code.md#fixed-and-moveable-variables)) niezarządzanej `T`, pod warunkiem, że typ `T*` jest niejawnie konwertowany na typ wskaźnika podany w instrukcji `fixed`. W takim przypadku inicjator oblicza adres danej zmiennej, a zmienna ma gwarancję pozostawania na stałym adresie na czas trwania instrukcji `fixed`.
*  Wyrażenie *array_type* z elementami typu niezarządzanego `T`, pod warunkiem, że typ `T*` jest niejawnie konwertowany na typ wskaźnika podany w instrukcji `fixed`. W takim przypadku inicjator oblicza adres pierwszego elementu w tablicy, a cała tablica ma być pozostawać w stałym adresie na czas trwania instrukcji `fixed`. Jeśli wyrażenie tablicy ma wartość null lub jeśli tablica ma elementy zerowe, inicjator oblicza adres równy zero.
*  Wyrażenie typu `string`, pod warunkiem, że typ `char*` jest niejawnie konwertowany na typ wskaźnika podany w instrukcji `fixed`. W takim przypadku inicjator oblicza adres pierwszego znaku w ciągu, a cały ciąg ma pozostawać w stałym adresie na czas trwania instrukcji `fixed`. Zachowanie instrukcji `fixed` jest zdefiniowane w implementacji, jeśli wyrażenie ciągu ma wartość null.
*  *Simple_name* lub *member_access* , który odwołuje się do elementu członkowskiego buforu o ustalonym rozmiarze zmiennej ruchomej, pod warunkiem, że typ składowej buforu o ustalonym rozmiarze jest niejawnie konwertowany na typ wskaźnika podany w instrukcji `fixed`. W takim przypadku inicjator oblicza wskaźnik do pierwszego elementu buforu o ustalonym rozmiarze ([bufory o stałym rozmiarze w wyrażeniach](unsafe-code.md#fixed-size-buffers-in-expressions)), a bufor o ustalonym rozmiarze jest gwarantowany na stały adres na czas trwania instrukcji `fixed`.

Dla każdego adresu obliczanego za pomocą *fixed_pointer_initializer* instrukcji `fixed` zapewnia, że zmienna, do której odwołuje się adres, nie podlega relokalizacji ani niepodyspozycji przez moduł wyrzucania elementów bezużytecznych na czas trwania instrukcji `fixed`. Na przykład, jeśli adres obliczony przez *fixed_pointer_initializer* odwołuje się do pola obiektu lub elementu wystąpienia tablicy, instrukcja `fixed` gwarantuje, że wystąpienie obiektu zawierającego nie jest przeszukiwane lub usunięte w okresie istnienia instrukcji.

Programista jest odpowiedzialny za zapewnienie, że wskaźniki utworzone przez instrukcje `fixed` nie przeżyły poza wykonywanie tych instrukcji. Na przykład, gdy wskaźniki utworzone przez instrukcje `fixed` są przesyłane do zewnętrznych interfejsów API, zależy to programista, aby upewnić się, że interfejsy API nie przechowują pamięci tych wskaźników.

Stałe obiekty mogą spowodować fragmentację sterty (ponieważ nie można ich przenieść). Z tego powodu obiekty powinny być naprawione tylko wtedy, gdy jest to absolutnie konieczne, a następnie tylko dla najkrótszej ilości czasu.

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

ilustruje kilka zastosowania instrukcji `fixed`. Pierwsza instrukcja naprawia i uzyskuje adres pola statycznego, druga instrukcja naprawia i uzyskuje adres pola wystąpienia, a trzecia instrukcja naprawia i uzyskuje adres elementu tablicy. W każdym przypadku Wystąpił błąd podczas używania operatora regularnego `&`, ponieważ zmienne są wszystkie sklasyfikowane jako ruchome zmienne.

Czwarta instrukcja `fixed` w powyższym przykładzie daje podobny wynik do trzeciej.

Ten przykład instrukcji `fixed` używa `string`:

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

W niebezpieczne elementy tablicy jednowymiarowej są przechowywane w kolejności rosnącego indeksu, zaczynając od indeksu `0` i kończące się na `Length - 1`indeksu. W przypadku tablic wielowymiarowych elementy tablic są przechowywane w taki sposób, aby indeksy w wymiarze z prawej strony były zwiększane jako pierwsze, następnie w następnym lewym wymiarze itd. W instrukcji `fixed`, która uzyskuje wskaźnik `p` do `a`wystąpienia tablicy, wartości wskaźnika od `p` do `p + a.Length - 1` reprezentują adresy elementów w tablicy. Podobnie zmienne od `p[0]` do `p[a.Length - 1]` reprezentują rzeczywiste elementy tablicy. Ze względu na sposób przechowywania tablic można traktować tablicę dowolnego wymiaru, tak jakby były liniowe.

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

co daje wynik:

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

w przykładzie

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

Instrukcja `fixed` jest używana do naprawienia tablicy, aby można było przekazywać jej adresy do metody, która przyjmuje wskaźnik.

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

stała Instrukcja służy do naprawienia buforu o ustalonym rozmiarze struktury, tak aby można było użyć adresu jako wskaźnika.

Wartość `char*` generowana przez naprawianie wystąpienia ciągu zawsze wskazuje na ciąg zakończony znakiem null. W ramach stałej instrukcji, która uzyskuje wskaźnik `p` do wystąpienia ciągu `s`, wartości wskaźnika od `p` do `p + s.Length - 1` reprezentują adresy znaków w ciągu, a wartość wskaźnika `p + s.Length` zawsze wskazuje znak null (znak z wartością `'\0'`).

Modyfikowanie obiektów typu zarządzanego za poorednictwem stałych wskaźników może spowodować niezdefiniowane zachowanie. Na przykład ze względu na to, że ciągi są niezmienne, zależy to programisty, aby upewnić się, że znaki, do których odwołuje się wskaźnik do ustalonego ciągu nie są modyfikowane.

Automatyczne Kończenie pustej wartości null ciągów jest szczególnie wygodne podczas wywoływania zewnętrznych interfejsów API, które oczekują ciągów "C-style". Należy jednak zauważyć, że wystąpienie ciągu może zawierać znaki o wartości null. Jeśli takie znaki mają wartość null, ciąg zostanie wyświetlony jako obcięty, gdy jest traktowany jako `char*`zakończona wartością null.

## <a name="fixed-size-buffers"></a>Bufory o ustalonym rozmiarze

Bufory o ustalonym rozmiarze są używane do deklarowania "stylu języka C" tablic wbudowanych jako elementów członkowskich struktur i są szczególnie przydatne w połączeniu z niezarządzanymi interfejsami API.

### <a name="fixed-size-buffer-declarations"></a>Deklaracje buforu o ustalonym rozmiarze

***Bufor o ustalonym rozmiarze*** jest składową reprezentującą magazyn dla bufora o stałej długości zmiennych danego typu. Deklaracja buforu o ustalonym rozmiarze wprowadza jeden lub więcej buforów o ustalonym rozmiarze danego typu elementu. Bufory o ustalonym rozmiarze są dozwolone tylko w deklaracjach struktury i mogą występować tylko w niebezpiecznym kontekście ([niebezpieczne konteksty](unsafe-code.md#unsafe-contexts)).

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

Deklaracja buforu o ustalonym rozmiarze może zawierać zestaw atrybutów ([atrybutów](attributes.md)), modyfikator `new` ([Modyfikatory](classes.md#modifiers)), prawidłową kombinację czterech modyfikatorów dostępu ([Parametry i ograniczenia typu](classes.md#type-parameters-and-constraints)) oraz modyfikator `unsafe` ([niebezpieczne konteksty](unsafe-code.md#unsafe-contexts)). Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych przez deklarację buforu o ustalonym rozmiarze. Występuje błąd dla tego samego modyfikatora w deklaracji buforu o ustalonym rozmiarze.

Deklaracja buforu o ustalonym rozmiarze nie może zawierać modyfikatora `static`.

Typ elementu buforu deklaracji buforu o ustalonym rozmiarze określa typ elementu buforów wprowadzonych przez deklarację. Typem elementu buforu musi być jeden ze wstępnie zdefiniowanych typów `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`lub `bool`.

Po typie elementu buforu następuje lista buforów o ustalonym rozmiarze deklaratory, z których każdy wprowadza nowy element członkowski. Bufor o ustalonym rozmiarze deklarator składa się z identyfikatora, który zawiera nazwę elementu członkowskiego, a następnie wyrażenie stałe ujęte w tokenach `[` i `]`. Stałe wyrażenie oznacza liczbę elementów w elemencie członkowskim wprowadzonym przez ten bufor o ustalonym rozmiarze deklarator. Typ wyrażenia stałego musi być niejawnie konwertowany na typ `int`, a wartość musi być dodatnią liczbą całkowitą różną od zera.

Elementy buforu o ustalonym rozmiarze są gwarantowane w kolejności sekwencyjnej w pamięci.

Deklaracja buforu o ustalonym rozmiarze, która deklaruje wiele buforów o ustalonym rozmiarze, jest równoważna z wieloma deklaracjami o pojedynczej deklaracji buforu o ustalonym rozmiarze z tymi samymi atrybutami i typami elementów. Na przykład:

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

Wyszukiwanie elementów członkowskich[](expressions.md#operators)buforu o ustalonym rozmiarze jest realizowane dokładnie tak samo jak odnośnik elementu członkowskiego pola.

Bufor o ustalonym rozmiarze może być przywoływany w wyrażeniu przy użyciu *simple_name* ([wnioskowanie o typie](expressions.md#type-inference)) lub *member_access* ([Sprawdzanie czasu kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Gdy element członkowski buforu o ustalonym rozmiarze jest przywoływany jako nazwa prosta, efekt jest taki sam jak dostęp do elementu członkowskiego w formularzu `this.I`, gdzie `I` jest członkiem buforu o ustalonym rozmiarze.

W celu uzyskania dostępu do elementu członkowskiego w formularzu `E.I`, jeśli `E` jest typu struktury i wyszukiwanie elementu członkowskiego `I` w tym typie struktury identyfikuje element członkowski o stałym rozmiarze, `E.I` jest oceniane sklasyfikowane w następujący sposób:

*  Jeśli wyrażenie `E.I` nie występuje w niebezpiecznym kontekście, wystąpi błąd w czasie kompilacji.
*  Jeśli `E` jest sklasyfikowana jako wartość, wystąpi błąd w czasie kompilacji.
*  W przeciwnym razie, jeśli `E` jest zmienną ruchomą ([zmienne stałe i ruchome](unsafe-code.md#fixed-and-moveable-variables)), a `E.I` wyrażenia nie jest *fixed_pointer_initializer* ([stała instrukcja](unsafe-code.md#the-fixed-statement)), wystąpi błąd w czasie kompilacji.
*  W przeciwnym razie `E` odwołuje się do stałej zmiennej, a wynik wyrażenia jest wskaźnikiem do pierwszego elementu składowej buforu o ustalonym rozmiarze `I` w `E`. Wynik jest typu `S*`, gdzie `S` jest typem elementu `I`i jest sklasyfikowany jako wartość.

Kolejne elementy buforu o ustalonym rozmiarze są dostępne przy użyciu operacji wskaźnika z pierwszego elementu. W przeciwieństwie do dostępu do tablic, dostęp do elementów buforu o ustalonym rozmiarze jest niebezpieczną operacją i nie jest sprawdzany zakres.

Poniższy przykład deklaruje i używa struktury z elementem członkowskim buforu o ustalonym rozmiarze.

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

### <a name="definite-assignment-checking"></a>Określone sprawdzanie przypisania

Bufory o ustalonym rozmiarze nie podlegają nieograniczonemu sprawdzaniu przypisania ([przypisanie](variables.md#definite-assignment)), a elementy członkowskie buforu o ustalonym rozmiarze są ignorowane na potrzeby nieograniczonego sprawdzania przypisania zmiennych typu struktury.

Gdy najbardziej nieruchoma zmienna struktury składowej buforu o ustalonym rozmiarze to zmienna statyczna, zmienna wystąpienia klasy lub element tablicy, elementy buforu o ustalonym rozmiarze są automatycznie inicjowane na wartości domyślne ([wartości domyślne](variables.md#default-values)). We wszystkich innych przypadkach początkowa zawartość buforu o ustalonym rozmiarze jest niezdefiniowana.

## <a name="stack-allocation"></a>Alokacja stosu

W niebezpiecznym kontekście deklaracja zmiennej lokalnej ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)) może zawierać inicjator alokacji stosu, który przydziela pamięć ze stosu wywołań.

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

*Unmanaged_type* wskazuje typ elementów, które będą przechowywane w nowo przydzielonym miejscu, a *wyrażenie* wskazuje liczbę tych elementów. Razem, określają one wymagany rozmiar alokacji. Ponieważ rozmiar alokacji stosu nie może być ujemny, jest to błąd czasu kompilacji, aby określić liczbę elementów jako *constant_expression* , których wynikiem jest wartość ujemna.

Inicjator alokacji stosu w formularzu `stackalloc T[E]` wymaga, aby `T` być typem niezarządzanym ([typami wskaźników](unsafe-code.md#pointer-types)) i `E` być wyrażeniem typu `int`. Konstrukcja alokuje `E * sizeof(T)` bajty ze stosu wywołań i zwraca wskaźnik o typie `T*`do nowo przydzielonego bloku. Jeśli `E` jest wartością ujemną, zachowanie jest niezdefiniowane. Jeśli `E` ma wartość zero, alokacja nie zostanie wykonana i zwracany wskaźnik jest zdefiniowany przez implementację. Jeśli nie jest dostępna wystarczająca ilość pamięci do przydzielenia bloku danego rozmiaru, zostanie zgłoszony `System.StackOverflowException`.

Zawartość nowo przydzieloną pamięci jest niezdefiniowana.

Inicjatory alokacji stosu są niedozwolone w blokach `catch` lub `finally` ([Instrukcja try](statements.md#the-try-statement)).

Nie istnieje sposób, aby jawnie zwolnić pamięć przydzieloną przy użyciu `stackalloc`. Wszystkie bloki pamięci przydzieloną na stosie utworzone podczas wykonywania elementu członkowskiego funkcji są automatycznie odrzucane, gdy ten element członkowski funkcji zwraca. Odnosi się to do funkcji `alloca`, która jest powszechnie dostępna w języku C C++ i implementacji.

w przykładzie

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

Inicjator `stackalloc` jest używany w metodzie `IntToString` do przydzielania bufora 16 znaków na stosie. Bufor jest automatycznie odrzucany, gdy metoda zwraca.

## <a name="dynamic-memory-allocation"></a>Alokacja pamięci dynamicznej

Oprócz operatora `stackalloc`, nie zawiera C# wstępnie zdefiniowanych konstrukcji do zarządzania pamięcią, która nie jest odzyskiwana. Takie usługi są zazwyczaj udostępniane przez obsługę bibliotek klas lub zaimportowane bezpośrednio z bazowego systemu operacyjnego. Na przykład Klasa `Memory` poniżej ilustruje, w jaki sposób można uzyskać dostęp do funkcji sterty bazowego systemu operacyjnego C#:

```csharp
using System;
using System.Runtime.InteropServices;

public static unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    private static readonly IntPtr s_heap = GetProcessHeap();

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size)
    {
        void* result = HeapAlloc(s_heap, HEAP_ZERO_MEMORY, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count)
    {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd)
        {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd)
        {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block)
    {
        if (!HeapFree(s_heap, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size)
    {
        void* result = HeapReAlloc(s_heap, HEAP_ZERO_MEMORY, block, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block)
    {
        int result = (int)HeapSize(s_heap, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    private const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    private static extern IntPtr GetProcessHeap();

    [DllImport("kernel32")]
    private static extern void* HeapAlloc(IntPtr hHeap, int flags, UIntPtr size);

    [DllImport("kernel32")]
    private static extern bool HeapFree(IntPtr hHeap, int flags, void* block);

    [DllImport("kernel32")]
    private static extern void* HeapReAlloc(IntPtr hHeap, int flags, void* block, UIntPtr size);

    [DllImport("kernel32")]
    private static extern UIntPtr HeapSize(IntPtr hHeap, int flags, void* block);
}
```

Przykład, który używa klasy `Memory`, podano poniżej:

```csharp
class Test
{
    static unsafe void Main()
    {
        byte* buffer = null;
        try
        {
            const int Size = 256;
            buffer = (byte*)Memory.Alloc(Size);
            for (int i = 0; i < Size; i++) buffer[i] = (byte)i;
            byte[] array = new byte[Size];
            fixed (byte* p = array) Memory.Copy(buffer, p, Size);
            for (int i = 0; i < Size; i++) Console.WriteLine(array[i]);
        }
        finally
        {
            if (buffer != null) Memory.Free(buffer);
        }
    }
}
```

W przykładzie przydzielane są 256 bajtów pamięci za pośrednictwem `Memory.Alloc` i inicjuje blok pamięci z wartościami rosnącymi z przedziału od 0 do 255. Następnie alokuje tablicę bajtów elementu 256 i używa `Memory.Copy`, aby skopiować zawartość bloku pamięci do tablicy bajtów. Na koniec blok pamięci jest zwalniany przy użyciu `Memory.Free`, a zawartość tablicy bajtów jest wyprowadzana w konsoli programu.
