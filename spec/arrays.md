# <a name="arrays"></a>Tablice

Tablica jest strukturą danych, która zawiera szereg zmiennych, które są dostępne za pośrednictwem obliczanej indeksów. Zmienne zawartych w tablicy jest określana skrótem elementy tablicy są wszystkie tego samego typu i tego typu jest nazywana typ elementu tablicy.

Tablica ma rangę, która określa liczbę indeksów skojarzone z każdego elementu tablicy. Ranga tablicy jest również określany jako wymiary tablicy. Nosi nazwę tablicy o liczbie rangą równą jeden ***tablicy jednowymiarowej***. Więcej niż jedna nazywa się rangę tablicy o liczbie ***tablicy wielowymiarowej***. Określone rozmiarze Wielowymiarowe tablice są często zwane tablice dwuwymiarowe, tablic trójwymiarowych i tak dalej.

Każdy wymiar tablicy ma skojarzone długości, która jest liczbą całkowitą większy lub równy zero. Długości wymiarów nie są częścią typ tablicy, ale raczej są określane podczas tworzenia wystąpienia typu tablicy w czasie wykonywania. Długość drugiego wymiaru określa prawidłowy zakres indeksów dla tego wymiaru: Dla wymiaru o długości `N`, indeksów może wynosić od `0` do `N - 1` (włącznie). Całkowita liczba elementów w tablicy jest produktem długości każdego wymiaru tablicy. Jeśli co najmniej jeden z wymiarów tablicy ma długość równą zero, tablica jest określany jako pusta.

Typ elementu tablicy może być dowolnego typu, w tym typu tablicowego.

## <a name="array-types"></a>Typy tablic

Typ tablicy jest zapisywany jako *non_array_type* następuje co najmniej jeden *rank_specifier*s:

```antlr
array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;
```

A *non_array_type* jest dowolnym *typu* oznacza to nie sam *array_type*.

Ranga typ tablicy jest nadawana przez najdalej z lewej strony *rank_specifier* w *array_type*: A *rank_specifier* wskazuje, że tablica jest tablicy z rangą równą jeden plus liczba "`,`" tokenów w *rank_specifier*.

Typ elementu typ tablicy to typ, który powoduje usunięcie najdalej z lewej strony *rank_specifier*:

*  Typ tablicy w formie `T[R]` jest tablicą o randze `R` i typ elementu inny niż tablica `T`.
*  Typ tablicy w formie `T[R][R1]...[Rn]` jest tablicą o randze `R` i typ elementu `T[R1]...[Rn]`.

W efekcie *rank_specifier*s są odczytywane od lewej do prawej, które przed typem ostatniego elementu inny niż tablica. Typ `int[][,,][,]` to Jednowymiarowa tablica tablic trójwymiarowych tablice dwuwymiarowe `int`.

W czasie wykonywania, może być wartością typu tablicowego `null` lub odwołanie do wystąpienia tego typu tablicy.

### <a name="the-systemarray-type"></a>Typ System.Array

Typ `System.Array` jest abstrakcyjnego typy podstawowego wszystkie typy tablic. Niejawna konwersja odwołania ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) istnieje z dowolnym typem tablicy do `System.Array`i konwersji jawnego odwołania ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)) istnieje z `System.Array` do dowolnego typu tablicy. Należy pamiętać, że `System.Array` sama nie jest *array_type*. Jest to raczej *class_type* z wszystkie *array_type*pochodzą s.

W czasie wykonywania, wartości typu `System.Array` może być `null` lub odwołanie do wystąpienia dowolnego typu tablicy.

### <a name="arrays-and-the-generic-ilist-interface"></a>Tablice i ogólny interfejs IList

Jednowymiarowa tablica `T[]` implementuje interfejs `System.Collections.Generic.IList<T>` (`IList<T>` w skrócie) i jego interfejsy podstawowe. W związku z tym istnieje niejawna konwersja z `T[]` do `IList<T>` i jego interfejsy podstawowe. Ponadto, jeśli istnieje niejawna konwersja odwołania z `S` do `T` następnie `S[]` implementuje `IList<T>` i istnieje niejawna konwersja odwołania z `S[]` do `IList<T>` i bazowej interfejsy () [Konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)). W przypadku jawnego odwołania konwersja `S` do `T` to konwersja jawna odwołania z `S[]` do `IList<T>` i jego interfejsy podstawowe ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)). Na przykład:
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

Przypisanie `lst2 = oa1` generuje błąd w czasie kompilacji, ponieważ konwersja z `object[]` do `IList<string>` to konwersja jawna, nie niejawne. Rzutowanie `(IList<string>)oa1` spowoduje zgłoszenie wyjątku w czasie wykonywania, ponieważ `oa1` odwołania `object[]` i nie `string[]`. Jednak rzutowania `(IList<string>)oa2` nie spowoduje zgłoszenie wyjątku od `oa2` odwołania `string[]`.

Zawsze, gdy istnieje odwołanie do jawnych lub niejawnych konwersja `S[]` do `IList<T>`, jest również jawnego odwołania konwersja `IList<T>` i bazowej interfejsy do `S[]` ([jawnego odwołania Konwersje](conversions.md#explicit-reference-conversions)).

Gdy typem tablicowym `S[]` implementuje `IList<T>`, niektórzy członkowie zaimplementowany interfejs może zgłaszać wyjątki. Dokładne zachowanie implementację interfejsu wykracza poza zakres tej specyfikacji.

## <a name="array-creation"></a>Do utworzenia tablicy

Wystąpienia tablicy są tworzone przez *array_creation_expression*s ([wyrażenie tworzenia tablicy](expressions.md#array-creation-expressions)) lub według pola lub lokalne deklaracje zmiennych, które obejmują *array_initializer*([Array inicjatory](arrays.md#array-initializers)).

Po utworzeniu instancji tablicy Ranga i długość każdego wymiaru są ustanowione, a następnie pozostaje niezmienna przez cały okres istnienia wystąpienia. Innymi słowy nie jest możliwe zmienić pozycję istniejącego wystąpienia tablicy nie jest możliwe zmienić rozmiar jej wymiarów.

Wystąpienie tablicy jest zawsze typu tablicowego. `System.Array` Typ jest typem abstrakcyjnym, nie można utworzyć wystąpienia.

Elementów tablic utworzonych przez *array_creation_expression*s zawsze są inicjowane na wartość domyślna ([wartości domyślne](variables.md#default-values)).

## <a name="array-element-access"></a>Dostęp do elementu tablicy

Elementy tablicy są dostępne przy użyciu *element_access* wyrażenia ([tablicy dostępu](expressions.md#array-access)) w postaci `A[I1, I2, ..., In]`, gdzie `A` to wyrażenie typu tablicowego, a każdy `Ix` jest Wyrażenie typu `int`, `uint`, `long`, `ulong`, lub można niejawnie przekonwertować do jednej lub kilku z tych typów. Wynik dostęp do elementu tablicy jest zmienną, a mianowicie elementu tablicy wybranych przez wskaźniki.

Elementy tablicy mogą być wyliczane przy użyciu `foreach` — instrukcja ([Instrukcja foreach](statements.md#the-foreach-statement)).

## <a name="array-members"></a>Elementy członkowskie tablicy

Każdy typ tablicy dziedziczy elementów członkowskich zadeklarowanych za `System.Array` typu.

## <a name="array-covariance"></a>Kowariancja tablicy

Dla dowolnych dwóch *reference_type*s `A` i `B`, jeśli niejawna konwersja odwołania ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) lub konwersji jawnej odwołania ([ Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)) istnieje z `A` do `B`, istnieje również tej samej konwersji odwołanie z typu tablicy, a następnie `A[R]` na typ array `B[R]`, gdzie `R` jest dowolnym dany *rank_specifier* (ale tablicy takie same dla obu typów). Ta relacja jest znany jako ***Kowariancja tablicy***. Kowariancja tablicy w szczególności oznacza, że wartość typu tablicowego `A[R]` faktycznie może być odwołaniem do wystąpienia typu tablicowego `B[R]`, o ile istnieje niejawna konwersja odwołania `B` do `A`.

Ze względu na Kowariancja tablicy przypisania do elementów tablicami typu odwołania zawierają sprawdzanie w czasie wykonania, która gwarantuje, że wartość jest przypisana do elementu tablicy jest faktycznie typu dozwolonych ([przypisanie proste](expressions.md#simple-assignment)). Na przykład:
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

Przypisanie do `array[i]` w `Fill` metoda obejmuje sprawdzanie w czasie wykonywania, który zapewnia, że obiekt odwołuje się `value` jest `null` lub wystąpienie, który jest zgodny z typem elementu rzeczywiste `array`. W `Main`, dwa pierwsze wywołania `Fill` powiedzie się, ale trzecie powoduje wywołanie `System.ArrayTypeMismatchException` zostanie wygenerowany po pierwsze przypisanie do wykonywania `array[i]`. Wyjątek występuje, ponieważ spakowany `int` nie mogą być przechowywane `string` tablicy.

Kowariancja tablicy specjalnie nie obejmuje tablic *value_type*s. Na przykład, nie istnieje Konwersja tego zezwala `int[]` powinien być traktowany jako `object[]`.

## <a name="array-initializers"></a>Inicjatory tablic

Inicjatora tablicy mogą być określone w deklaracji pól ([pola](classes.md#fields)), deklaracje zmiennych lokalnych ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)) i wyrażenie tworzenia tablicy ([tworzenia tablicy wyrażenia](expressions.md#array-creation-expressions)):

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

Inicjatora tablicy składa się z sekwencji zmiennej inicjatory, ujęta w "`{`"i"`}`"tokenów i oddzielone"`,`" tokenów. Każdy inicjatorze zmiennych jest wyrażeniem lub, w przypadku tablicy wielowymiarowej zagnieżdżonego inicjatora tablicy.

Kontekst, w którym jest używany Inicjator tablicy Określa typ tablicy, inicjowany. W wyrażenie tworzenia tablicy typu tablicy bezpośrednio poprzedza inicjatora lub wynika z wyrażenia w inicjatorze tablicy. W polu lub deklaracja zmiennej typ tablicy jest typu pola lub zmiennej deklarowanej. Gdy inicjatora tablicy jest używany w pole lub deklaracji zmiennych, takich jak:
```csharp
int[] a = {0, 2, 4, 6, 8};
```
jest po prostu skrótem wyrażenie tworzenia tablicy równoważne:
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

Dla tablicy jednowymiarowej inicjatora tablicy musi składać się z sekwencji wyrażeń, które są przypisania zgodny z typem elementu tablicy. Wyrażenia zainicjować elementy tablicy w rosnącej kolejności, poczynając od elementu o indeksie zero. Liczba wyrażeń w inicjatorze tablicy określa długość tworzenia instancji tabeli. Na przykład tworzy powyżej inicjatora tablicy `int[]` wystąpienia o długości 5 i następnie inicjuje wystąpienie z następującymi wartościami:
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

Wielowymiarowe tablice inicjatora tablicy musi mieć dowolną liczbę poziomów zagnieżdżania, ponieważ wymiary tablicy. Najbardziej zewnętrznej poziom zagnieżdżenia odnosi się do wymiaru skrajnie po lewej stronie i najbardziej poziom zagnieżdżenia odnosi się do wymiaru po prawej stronie. Długość każdego wymiaru tablicy jest określana przez liczbę elementów znajdujących się na odpowiedni poziom zagnieżdżenia w inicjatorze tablicy. Dla każdego zagnieżdżonego inicjatora tablicy liczba elementów musi być taka sama jak innych inicjatora tablicy na tym samym poziomie. Przykład:
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
tworzy tablicę dwuwymiarową o długości do 5 dla wymiaru skrajnie po lewej stronie i o długości dwóch dla wymiaru po prawej stronie:
```csharp
int[,] b = new int[5, 2];
```
a następnie Inicjuje tablicę wystąpień z następującymi wartościami:
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

Jeśli wymiar innego niż po prawej stronie podano o zerowej długości, kolejne wymiary są zakłada się, że również mieć długości zerowej. Przykład:
```csharp
int[,] c = {};
```
Tworzy dwuwymiarową tablicę o długości zerowej najdalej z lewej strony i po prawej stronie wymiaru:
```csharp
int[,] c = new int[0, 0];
```

Kiedy wyrażenie tworzenia tablicy obejmuje zarówno długości wymiarów jawne, jak i inicjatora tablicy, długości muszą być wyrażeniami stałymi, a liczba elementów na każdym poziomie zagnieżdżania, które musi odpowiadać odpowiedniego długość wymiaru. Oto kilka przykładów:
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

Tutaj, inicjator dla `y` powoduje błąd w czasie kompilacji, ponieważ wyrażenie długości wymiarów nie jest stałą, a inicjator dla `z` powoduje błąd w czasie kompilacji, ponieważ długość i liczba elementów w Inicjator nie są zgodne.
