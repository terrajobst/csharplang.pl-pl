# <a name="types"></a>Types

Typy w języku C# są podzielone na dwie główne kategorie: ***typy wartości*** i ***typy odwołań***. Typy wartości i typy referencyjne mogą być ***typów ogólnych***, który wykonać co najmniej jeden ***parametry typu***. Parametrów typu można wyznaczyć obu typów wartości i typy referencyjne.

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

Końcowe kategorii typów, wskaźników, jest dostępna tylko w niebezpieczny kod. Zostało to omówione w dalszych [typy wskaźników](unsafe-code.md#pointer-types).

Typy wartości różnią się z typami odwołań, zmienne typu wartości zawierają bezpośrednio swoje dane, natomiast zmienne odwołania do typów magazynu ***odwołania*** praw dotyczących danych, te ostatnie są nazywane ***obiektów***. W przypadku typów referencyjnych jest możliwe w dwóch zmiennych odwoływać się do tego samego obiektu, a zatem możliwe dla operacji na jednej zmiennej miały wpływ na obiekt odwołuje się druga zmienna. Z typami wartości zmiennych każda ma własne kopię danych, a nie jest możliwe dla operacji na jednym wpływa na drugi.

W systemie typu C# jest jednolita w taki sposób, że wartość dowolnego typu może być traktowana jako obiekt. Każdy typ w języku C#, bezpośrednio lub pośrednio pochodzi z `object` typ, klasy i `object` jest ultimate klasą bazową wszystkich typów. Wartości typu referencyjnego są traktowane jako obiekty poprzez wyświetlanie wartości jako typu `object`. Wartości typu wartości są traktowane jako obiekty, wykonując operacje pakowania, jak i rozpakowania ([pakowania i rozpakowywania](types.md#boxing-and-unboxing)).

## <a name="value-types"></a>Typy wartości

Typ wartości jest typu struktury lub typem wyliczenia. C# zawiera zestaw wstępnie zdefiniowanych struktura typów o nazwie ***typów prostych***. Proste typy są identyfikowane za pomocą słów zastrzeżonych.

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

W przeciwieństwie do zmiennej typu odwołania, zmienna typu wartości może zawierać wartość `null` tylko wtedy, gdy typ wartości jest typu dopuszczającego wartość null.  Dla każdego typu wartości niedopuszczającym wartości jest odpowiedni typ wartości null, określające elementy tego samego zestawu wartości i wartości `null`.

Przypisanie do zmiennej typu wartości tworzona jest kopia wartości jest przypisane. To różni się od przypisania do zmiennej typu odwołania, która kopiuje odwołania, ale nie identyfikowane przez odwołanie do obiektu.

### <a name="the-systemvaluetype-type"></a>Typ System.ValueType

Wszystkie typy wartości są niejawnie dziedziczą z klasy `System.ValueType`, która z kolei dziedziczy z klasy `object`. Nie jest możliwe dla dowolnego typu do pochodzić od typu wartości i typy wartości, dlatego są niejawnie zamknięte ([zapieczętowanych klas](classes.md#sealed-classes)).

Należy pamiętać, że `System.ValueType` sama nie jest *value_type*. Jest to raczej *class_type* z wszystkie *value_type*s wywodzą się automatycznie.

### <a name="default-constructors"></a>Konstruktory domyślne

Wszystkie typy wartości niejawnie zadeklarować Konstruktor wystąpienia bez parametrów publicznego o nazwie ***domyślnego konstruktora***. Domyślny konstruktor Zwraca wystąpienie inicjowany z wartością zerową znane jako ***wartość domyślna*** jako typ wartości:

*  Dla wszystkich *simple_type*, wartością domyślną jest wartość produkowane przez wzorzec bitowy w postaci samych zer:
    * Aby uzyskać `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, i `ulong`, wartość domyślna to `0`.
    * Aby uzyskać `char`, wartość domyślna to `'\x0000'`.
    * Aby uzyskać `float`, wartość domyślna to `0.0f`.
    * Aby uzyskać `double`, wartość domyślna to `0.0d`.
    * Aby uzyskać `decimal`, wartość domyślna to `0.0m`.
    * Aby uzyskać `bool`, wartość domyślna to `false`.
*  Aby uzyskać *enum_type* `E`, wartość domyślna to `0`, przekonwertowane na typ `E`.
*  Aby uzyskać *struct_type*, wartością domyślną jest wartość produkowane przez ustawienie dla wszystkich pola typu wartości przywrócić wartości domyślne i wszystkie odwołania pola typu do `null`.
*  Aby uzyskać *nullable_type* wartość domyślna to wystąpienie, dla którego `HasValue` właściwość ma wartość FAŁSZ i `Value` właściwość nie jest. Wartością domyślną jest również nazywany ***wartość null*** typu dopuszczającego wartość null.

Podobnie jak dowolnego innego konstruktora wystąpienia domyślnego konstruktora typu wartości jest wywoływana przy użyciu `new` operatora. Ze względu na wydajność to wymaganie nie jest przeznaczona do faktycznie mieć implementacji generuje wywołanie konstruktora. W przykładzie poniżej zmienne `i` i `j` są zarówno inicjowane od zera.

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

Każdy typ wartości jest niejawnie Konstruktor publiczny wystąpienia bez parametrów, dlatego nie jest możliwe dla typu struktury zawierają jawna deklaracja konstruktora bez parametrów. Typ struktury jest jednak dozwolony do deklarowania konstruktory sparametryzowane wystąpień ([konstruktory](structs.md#constructors)).

### <a name="struct-types"></a>Typy — struktura

Typ struktury jest typem wartości, które można zadeklarować stałe, pola, metody, właściwości, indeksatory, operatory, konstruktory wystąpień, konstruktorów statycznych i zagnieżdżone typy. Deklaracja typu struktury jest opisana w [deklaracji struktury](structs.md#struct-declarations).

### <a name="simple-types"></a>Typy proste

C# zawiera zestaw wstępnie zdefiniowanych struktura typów o nazwie ***typów prostych***. Proste typy są identyfikowane za pomocą słów zastrzeżonych, ale te słowa zastrzeżone są po prostu aliasami dla struktury wstępnie zdefiniowanych typów w pakietach `System` przestrzeni nazw, zgodnie z opisem w poniższej tabeli.


| __Słowo zastrzeżone__ | __Wpisz alias__ |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

Ponieważ prostego typu aliasy struktury, co typ prosty ma członków. Na przykład `int` ma elementów członkowskich zadeklarowanych w `System.Int32` i elementy członkowskie są dziedziczone z `System.Object`, oraz następujące instrukcje są dozwolone:

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

Proste typy różnią się od innych typów struktury w sposób, aby umożliwić pewne dodatkowe operacje:

*  Większość typów prostych zezwala na wartości, które mają być utworzone przez pisanie *literały* ([literały](lexical-structure.md#literals)). Na przykład `123` jest literału typu `int` i `'a'` jest literału typu `char`. C# nie przewiduje literałów typu struktury ogólnie rzecz biorąc, a wartości inne niż domyślne inne typy struktury ostatecznie są zawsze tworzone za pomocą konstruktorów wystąpienia z tych typów struktury.
*  Operandy wyrażenia w przypadku wszystkich stałych typu prostego, możliwe jest dla kompilatora można obliczyć wartości wyrażenia w czasie kompilacji. Takie wyrażenie jest znany jako *constant_expression* ([wyrażeń stałych](expressions.md#constant-expressions)). Wyrażeń obejmujących operatory zdefiniowane przez inne typy struktury nie są uważane za wyrażeń stałych.
*  Za pomocą `const` deklaracji jest możliwe do deklarowania stałych typów prostych ([stałe](classes.md#constants)). Nie jest możliwe stałe inne typy struktury, ale podobny efekt jest dostarczany przez `static readonly` pola.
*  Konwersje typów prostych obejmujące mogą uczestniczyć w wersji ewaluacyjnej usługi operatory konwersji zdefiniowane przez inne typy struktury, ale operator konwersji zdefiniowany przez użytkownika nigdy nie mogą brać udział w wersji ewaluacyjnej usługi innego operatora zdefiniowanego przez użytkownika ([oceny konwersje zdefiniowane przez użytkownika](conversions.md#evaluation-of-user-defined-conversions)).

### <a name="integral-types"></a>Typy całkowite

C# obsługuje dziewięć typów całkowitych: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, i `char`. Typy całkowite mają następujące rozmiarów i zakresy wartości:

*  `sbyte` Reprezentuje typ podpisany 8-bitowych liczb całkowitych przy użyciu wartości z zakresu od -128 i 127.
*  `byte` Typ reprezentuje liczb całkowitych bez znaku 8-bitową przy użyciu wartości z zakresu od 0 do 255.
*  `short` Reprezentuje typ podpisany 16-bitowych liczb całkowitych przy użyciu wartości z zakresu od -32768 do 32767.
*  `ushort` Typ reprezentuje liczb całkowitych bez znaku 16-bitowych przy użyciu wartości z zakresu od 0 do 65535.
*  `int` Reprezentuje typ podpisany 32-bitowych liczb całkowitych przy użyciu wartości z zakresu od -2147483648 do 2147483647.
*  `uint` Typ reprezentuje liczb całkowitych bez znaku 32-bitowych przy użyciu wartości z zakresu od 0 do 4294967295.
*  `long` Reprezentuje typ podpisany 64-bitowych liczb całkowitych przy użyciu wartości z zakresu od wartość -9223372036854775808 9223372036854775807.
*  `ulong` Typ reprezentuje liczb całkowitych bez znaku 64-bitowych przy użyciu wartości z zakresu od 0 do 18446744073709551615 są.
*  `char` Typ reprezentuje liczb całkowitych bez znaku 16-bitowych przy użyciu wartości z zakresu od 0 do 65535. Zestaw możliwych wartości dla `char` typu odnosi się do zestawu znaków Unicode. Mimo że `char` ma taką samą reprezentację jako `ushort`, nie wszystkie operacje na jednym typie są dozwolone w drugiej.

Jednoargumentowy typu całkowitego i operatory dwuargumentowe są zawsze pracują z podpisem precyzji 32-bitowych, bez znaku 32-bitową precyzję, podpisem precyzji 64-bitowych lub bez znaku 64-bitowa precyzja:

*  Dla jednoargumentowego `+` i `~` operatorów, operand jest konwertowany na typ `T`, gdzie `T` jest to pierwszy `int`, `uint`, `long`, i `ulong` można w pełni reprezentujące wszystkie Możliwe wartości argumentu operacji. Operacja jest wykonywana przy użyciu precyzji typu `T`, a typ wyniku jest `T`.
*  Dla jednoargumentowego `-` operatora, operand jest konwertowany na typ `T`, gdzie `T` jest to pierwszy `int` i `long` który pełni może reprezentować wszystkie możliwe wartości argumentu operacji. Operacja jest wykonywana przy użyciu precyzji typu `T`, a typ wyniku jest `T`. Jednoargumentowy `-` do argumentów operacji typu nie można zastosować operatora `ulong`.
*  Aby uzyskać dane binarne `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, i `<=` operatorów, operandy są konwertowane na typ `T`, gdzie `T` jest to pierwszy `int`, `uint`, `long`, i `ulong` reprezentujące można w pełni wszystkich możliwych wartości oba operandy. Operacja jest wykonywana przy użyciu precyzji typu `T`, a typ wyniku jest `T` (lub `bool` dla operatorów relacyjnych). Nie jest dozwolony dla jeden argument typu `long` i drugą typu `ulong` z operatorami dwuargumentowymi.
*  Dla pliku binarnego `<<` i `>>` operatorów, lewy operand jest konwertowany na typ `T`, gdzie `T` jest to pierwszy `int`, `uint`, `long`, i `ulong` można w pełni reprezentujące wszystkie Możliwe wartości argumentu operacji. Operacja jest wykonywana przy użyciu precyzji typu `T`, a typ wyniku jest `T`.

`char` Typu jest klasyfikowana jako integralny typ, ale różni się od innych typów całkowitych na dwa sposoby:

*  Istnieje nie niejawne konwersje z innych typów `char` typu. W szczególności mimo że `sbyte`, `byte`, i `ushort` typy mają zakresy wartości, które są w pełni stałego za pośrednictwem `char` wpisz niejawne konwersje z elementu `sbyte`, `byte`, lub `ushort` do `char` nie istnieją.
*  Stałe `char` typu musi być napisana jako *character_literal*s lub jako *integer_literal*s w połączeniu z Rzutowanie na typ `char`. Na przykład `(char)10` jest taka sama jak `'\x000A'`.

`checked` i `unchecked` operatory i instrukcje, które są używane do kontrolowania przepełnienie sprawdzania pod kątem typu całkowitego operacje arytmetyczne i konwersje ([operatory checked i unchecked](expressions.md#the-checked-and-unchecked-operators)). W `checked` kontekstu, przepełnienie powoduje błąd kompilacji lub powoduje, że `System.OverflowException` zostanie wygenerowany. W `unchecked` kontekście przepełnienia są ignorowane, a wszystkie bity wyższego rzędu, które nie mieszczą się w typie docelowym zostaną odrzucone.

### <a name="floating-point-types"></a>Zmiennoprzecinkowe typy punktów

C# obsługuje dwa punktu zmiennoprzecinkowego: `float` i `double`. `float` i `double` typy są reprezentowane przy użyciu 32-bitowe o pojedynczej dokładności i 64-bitowych podwójnej precyzji IEEE 754 formatów, które zapewniają następujące zestawy wartości:

*  Dodatnia zero i ujemna zero. W większości sytuacji, zero dodatnie i ujemne zero zachowują się identycznie, prostą wartość zero, ale niektóre operacje rozróżnienie między dwoma ([operator dzielenia](expressions.md#division-operator)).
*  Nieskończoności dodatniej i ujemna nieskończoność. Nieskończoności są produkowane przez operacje, takie jak dzielenia liczby różna od zera przez zero. Na przykład `1.0 / 0.0` daje nieskończoności dodatniej i `-1.0 / 0.0` plony minus nieskończoność.
*  ***Nie nieliczbowych*** wartości, często w postaci akronimu NaN. NaNs są produkowane przez nieprawidłowy operacji zmiennoprzecinkowych, takich jak dzielenia 0 przez zero.
*  Ograniczone zbiór wartości niezerowe wiązania w postaci `s * m * 2^e`, gdzie `s` wynosi 1 lub -1, i `m` i `e` zależą od określonego typu zmiennoprzecinkowego: Aby uzyskać `float`, `0 < m < 2^24` i `-149 <= e <= 104`oraz `double`, `0 < m < 2^53` i `1075 <= e <= 970`. Nieznormalizowany liczby zmiennoprzecinkowe są traktowane jako prawidłowe wartości różna od zera.

`float` Typ może reprezentować wartości z zakresu od około `1.5 * 10^-45` do `3.4 * 10^38` z dokładnością do 7 cyfr.

`double` Typ może reprezentować wartości z zakresu od około `5.0 * 10^-324` do `1.7 × 10^308` z dokładnością do 15-16 cyfr.

Jeśli jeden z operandów operatora binarnego jest typu zmiennoprzecinkowego, to drugi operand musi być typu całkowitego lub typu zmiennoprzecinkowego i operacji jest obliczane w następujący sposób:

*  Jeśli jeden z operandów jest typu całkowitego, że operand jest konwertowany na typ zmiennoprzecinkowych to drugi operand.
*  Następnie, jeśli jeden z operandów jest typu `double`, to drugi operand jest konwertowany na `double`, operacja jest wykonywana przy użyciu co najmniej `double` zakres i dokładność i typ wyniku jest `double` (lub `bool` dla Operatory relacyjne).
*  W przeciwnym razie operacja odbywa się przy użyciu co najmniej `float` zakres i dokładność i typ wyniku jest `float` (lub `bool` dla operatorów relacyjnych).

Zmiennoprzecinkowe operatorów, łącznie z operatorów przypisania nigdy nie generuje wyjątków. Zamiast tego w sytuacjach wyjątkowych operacji zmiennoprzecinkowych dawać zero, infinity lub NaN, zgodnie z poniższym opisem:

*  Jeśli wynik operacji zmiennoprzecinkowej jest zbyt mały dla formatu docelowego, wynik operacji staje się zerem dodatnia lub ujemna zero.
*  Jeśli wynik operacji zmiennoprzecinkowej jest zbyt duży dla formatu docelowego, wynik operacji staje się nieskończoność dodatnia lub ujemna nieskończoność.
*  Jeśli operacji zmiennoprzecinkowej jest nieprawidłowa, wynik operacji staje się NaN.
*  Jeśli jeden lub obydwa operandy operacji zmiennoprzecinkowej jest NaN, wynik operacji staje się NaN.

Zmiennoprzecinkowe operacje mogą być wykonywane przy użyciu większą precyzję niż typ wyniku operacji. Na przykład niektóre architektury sprzęt obsługuje "rozszerzony" lub "double" typ zmiennoprzecinkowy większy zakres i dokładność niż `double` wpisz i niejawnie wykonuje wszystkie operacje zmiennoprzecinkowe przy użyciu tego wyższe typu dokładności. Tylko kosztów nadmierne wydajności architektur takich sprzętu nie jest możliwe do wykonania operacji zmiennoprzecinkowych z mniejszą dokładnością i, a nie wymagają wdrażania straci wydajności i dokładności, C# umożliwia wyższe typu dokładności jako używane dla wszystkich operacji zmiennoprzecinkowych. Inne niż dostarczać dokładniejsze wyniki, rzadko ma wymierne efekty. Jednak w wyrażeniach w postaci `x * y / z`, gdzie mnożenia daje wynik, który znajduje się poza `double` zakresu, ale kolejne dzielenie oferuje tymczasowy wynik do `double` zakresu fakt, że wyrażenie obliczone w zakresie wyższe format może spowodować skończoną wynik do wyprodukowania zamiast nieskończoność.

### <a name="the-decimal-type"></a>Typu dziesiętnego

`decimal` Typ to typ danych 128-bitowych, odpowiedni do obliczeń finansowych i walutowych. `decimal` Typ może reprezentować wartości z zakresu od `1.0 * 10^-28` do około `7.9 * 10^28` z cyframi znaczącymi 28-29.

Ograniczone zestaw wartości typu `decimal` mają postać `(-1)^s * c * 10^-e`, gdzie znak `s` jest równa 0 lub 1, współczynnik `c` jest nadawana przez `0 <= *c* < 2^96`i skalowania `e` jest tak, aby `0 <= e <= 28`. `decimal` Typ nie obsługuje podpisem zer, nieskończoności lub firmy NaN. Element `decimal` jest reprezentowany jako 96-bitową liczbę całkowitą, skalowana przez potęgą liczby 10. Dla `decimal`s przy użyciu wartości bezwzględnej mniej niż `1.0m`, wartość jest dokładnie do 28 po przecinku, lecz żadnych dalszych. Aby uzyskać `decimal`s przy użyciu wartości bezwzględnej większa lub równa `1.0m`, wartość jest dokładnie do 28-29 cyfr. Sprzeczna `float` i `double` typy danych ułamkowych liczby dziesiętne, takich jak 0,1 może być reprezentowany dokładnie w `decimal` reprezentacji. W `float` i `double` reprezentacje liczby takie są często nieskończonej ułamki co te oświadczenia, które są bardziej podatne na zaokrąglania błędy.

Jeśli jeden z argumentów operatora binarnego jest typu `decimal`, to drugi operand musi być typu całkowitego lub typu `decimal`. Jeśli argument typu całkowitego jest obecny, jest konwertowany na `decimal` przed wykonaniem operacji.

Wynikiem operacji na wartościach typu `decimal` jest, który byłby wynikiem obliczania dokładny wynik (przy zachowaniu skalowania, zgodnie z definicją operatorów), a następnie Zaokrąglenie w celu dopasowania do przedstawienia. Wyniki są zaokrąglane do najbliższego wartość oraz, jeśli wynik jest jednakowo blisko dwóch reprezentowanych wartości do wartości, który ma parzystą liczbą w najmniej znaczący Pozycja cyfry (jest to określane jako "banker przez zaokrąglenie"). Zero wynik ma zawsze znak liczby 0 i o skali 0.

Jeśli dziesiętna Operacja arytmetyczna daje wartość mniejszą niż lub równa `5 * 10^-29` w wartościach bezwzględnych, wynik operacji staje się zerem. Jeśli `decimal` Operacja arytmetyczna daje wynik, który jest zbyt duży dla `decimal` formacie `System.OverflowException` zgłaszany.

`decimal` Typ ma większą precyzję ale mniejszym zakresie niż typów zmiennoprzecinkowych. W związku z tym, konwersje z typów zmiennoprzecinkowych aby `decimal` może tworzyć wyjątki przepełnienia i konwersje z `decimal` z typami zmiennoprzecinkowymi może spowodować utratę dokładności. Z tego względu nie niejawne występują konwersje między typami zmiennoprzecinkowymi a `decimal`, i bez jawnego rzutowania, nie można mieszać zmiennoprzecinkowe i `decimal` operandy w jednym wyrażeniu.

### <a name="the-bool-type"></a>Typu logicznego

`bool` Typ reprezentuje logiczną ilości logiczne. Możliwe wartości typu `bool` są `true` i `false`.

Nie standardowa występują konwersje między `bool` a innymi typami danych. W szczególności `bool` typ jest odrębna i oddzielona od typów całkowitych i `bool` wartości nie można użyć zamiast wartości całkowitej i na odwrót.

W językach C i C++ całkowitych lub zmiennoprzecinkowych wartość zero lub wskaźnikiem typu null można przekonwertować na wartość logiczną `false`, a wartość niezerową całkowitych lub zmiennoprzecinkowych lub wskaźnik zerowy można przekonwertować na wartość logiczną `true`. W języku C#, takie konwersje są wykonywane przez jawne porównanie wartości całkowitych lub zmiennoprzecinkowych do zera lub przez jawne porównanie odwołanie obiektu do `null`.

### <a name="enumeration-types"></a>Typów wyliczeniowych

Typ wyliczenia jest typem samodzielnym z nazwanych stałych. Każdy typ wyliczenia ma podstawowy typ, który musi być `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` lub `ulong`. Zbiór wartości na typ wyliczenia jest taka sama jak zestaw wartości typu podstawowego. Wartości typu wyliczenia nie są ograniczone do wartości nazwanych stałych. Typy wyliczeniowe są definiowane za pomocą deklaracje modułów wyliczających ([deklaracje wyliczeń](enums.md#enum-declarations)).

### <a name="nullable-types"></a>Typy dopuszczające wartości null

Typ dopuszczający wartość null może reprezentować wszystkie wartości jego ***typ bazowy*** oraz dodatkową wartość null. Typ dopuszczający wartość null jest zapisywany `T?`, gdzie `T` jest typem podstawowym. Ta składnia jest skrótem `System.Nullable<T>`, a dwa formularze, które mogą być używane zamiennie.

A ***typem wartościowym niebędącym*** i odwrotnie jest dowolny typ wartości inne niż `System.Nullable<T>` i jego skrót `T?` (dla każdego `T`), oraz parametr dowolnego typu, który jest ograniczony do być typem wartości niedopuszczającym wartości (czyli dowolny parametru typu z `struct` ograniczenia). `System.Nullable<T>` Typ Określa wartość ograniczenia typu dla `T` ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), co oznacza, że typ podstawowy elementu typu dopuszczającego wartość null, mogą być dowolnego typu wartości niedopuszczającym wartości. Typ dopuszczający wartość null lub typ referencyjny, nie może być typem podstawowym typu dopuszczającego wartość null. Na przykład `int??` i `string?` są nieprawidłowymi typami.

Wystąpienie typu dopuszczającego wartość null `T?` ma dwie właściwości publiczne tylko do odczytu:

*  A `HasValue` właściwości typu `bool`
*  A `Value` właściwości typu `T`

Wystąpienie, dla którego `HasValue` jest wartość PRAWDA jest określany jako inną niż null. Wystąpienia innych niż null zawiera znaną wartością i `Value` zwraca tę wartość.

Wystąpienie, dla którego `HasValue` jest FAŁSZ jest określany jako wartość null. Wystąpienie o wartości null ma wartość niezdefiniowana. Podjęto próbę odczytu `Value` wystąpienia o wartości null powoduje, że `System.InvalidOperationException` zostanie wygenerowany. Proces uzyskiwania `Value` właściwość nullable wystąpienia jest określany jako ***Odkodowywanie***.

Oprócz domyślnego konstruktora, co typ dopuszczający wartość null `T?` publiczny konstruktor, który przyjmuje pojedynczy argument typu `T`. Wartość `x` typu `T`, wywołanie konstruktora formularza

```csharp
new T?(x)
```
tworzy wystąpienie innych niż null `T?` dla którego `Value` właściwość `x`. Proces tworzenia wystąpienia typu dopuszczającego wartość null inną niż null dla danej wartości nazywa się ***zawijania***.

Niejawne konwersje są dostępne z `null` literału `T?` ([konwersje literału Null](conversions.md#null-literal-conversions)) i z `T` do `T?` ([niejawne konwersje nullable](conversions.md#implicit-nullable-conversions)).

## <a name="reference-types"></a>Typy odwołań

Typ odwołania jest typu klasy, typu interfejsu, typ tablicy lub typu delegata.

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

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

delegate_type
    : type_name
    ;
```

Wartość typu odwołania jest odwołaniem do ***wystąpienia*** typu ostatnie znane jako ***obiektu***. Specjalna wartość `null` jest zgodny ze wszystkimi typami odwołań i wskazuje brak wystąpienia.

### <a name="class-types"></a>Typy klas

Typ klasy definiuje strukturę danych, który zawiera elementy członkowskie (stałe i pola), funkcji elementów członkowskich danych (metody, właściwości, zdarzenia, indeksatory, operatory, konstruktory wystąpień, destruktory i konstruktorów statycznych) i zagnieżdżone typy. Typy klas obsługuje dziedziczenie, mechanizm, według której rozszerzać i specialize klas bazowych klas pochodnych. Wystąpienia typów klas są tworzone przy użyciu *object_creation_expression*s ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)).

Typy klas są opisane w [klasy](classes.md).

Niektóre typy wstępnie zdefiniowanych klas mają specjalne znaczenie w języku C#, zgodnie z opisem w poniższej tabeli.


| __Typ klasy__     | __Opis__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | Ultimate klasa bazowa innych typów. Zobacz [typu obiektu](types.md#the-object-type). | 
| `System.String`    | Typ ciągu języka C#. Zobacz [typu string](types.md#the-string-type).         |
| `System.ValueType` | Klasa bazowa wszystkich typów wartości. Zobacz [typu System.ValueType](types.md#the-systemvaluetype-type).          |
| `System.Enum`      | Klasa bazowa wszystkich typów wyliczenia. Zobacz [wyliczenia](enums.md).              |
| `System.Array`     | Klasa bazowa wszystkie typy tablic. Zobacz [tablic](arrays.md).             |
| `System.Delegate`  | Klasa bazowa wszystkimi typami delegatów. Zobacz [delegatów](delegates.md).          |
| `System.Exception` | Klasa bazowa wszystkich typów wyjątków. Zobacz [wyjątki](exceptions.md).         |

### <a name="the-object-type"></a>Typ obiektu

`object` Typu klasy jest ultimate klasą bazową wszystkich innych typów. Każdy typ w języku C#, bezpośrednio lub pośrednio pochodzi z `object` typ klasy.

Słowo kluczowe `object` jest po prostu aliasem dla wstępnie zdefiniowanego klasy `System.Object`.

### <a name="the-dynamic-type"></a>Typ dynamiczny

`dynamic` Typ, takie jak `object`, można odwoływać się do dowolnych obiektów. Gdy operatory są stosowane do wyrażenia typu `dynamic`, ich rozwiązania jest odroczone do czasu program jest uruchamiany. W związku z tym jeśli operator prawnie nie można zastosować do przywoływanego obiektu, błąd nie jest zwracany podczas kompilacji. Zamiast tego zostanie zgłoszony wyjątek podczas rozpoznawania operatora zakończy się niepowodzeniem w czasie wykonywania.

Jej celem jest umożliwienie dynamiczne powiązanie, które opisano szczegółowo w temacie [wiązanie dynamiczne](expressions.md#dynamic-binding).

`dynamic` uznaje się taka sama jak `object` z wyjątkiem pod następującymi względami:

*  Operacje dotyczące wyrażeń typu `dynamic` mogą być dynamicznie powiązane ([wiązanie dynamiczne](expressions.md#dynamic-binding)).
*  Wnioskowanie o typie ([wnioskowanie o typie](expressions.md#type-inference)) zostanie Preferuj `dynamic` za pośrednictwem `object` , gdy oba są kandydatami.

Ze względu na to równoważności poniżej zawiera:

*  Brak konwersji niejawnych tożsamości między `object` i `dynamic`oraz między typami skonstruowany, które są takie same w przypadku zastępowania `dynamic` z `object`
*  Jawne i niejawne konwersje do i z `object` mają zastosowanie również do i z `dynamic`.
*  Podpisy metod, które są takie same w przypadku zastępowania `dynamic` z `object` są traktowane jako ten sam podpis
*  Typ `dynamic` nie można odróżnić od `object` w czasie wykonywania.
*  Wyrażenie typu `dynamic` nazywa się ***wyrażenia dynamicznego***.

### <a name="the-string-type"></a>Typ ciągu

`string` Typ jest typem klasy zapieczętowanej, który dziedziczy bezpośrednio z `object`. Wystąpienia elementu `string` klasa reprezentuje ciągami znaków Unicode.

Wartości typu `string` typ może być zapisana jako literały ciągów ([Literały ciągu](lexical-structure.md#string-literals)).

Słowo kluczowe `string` jest po prostu aliasem dla wstępnie zdefiniowanego klasy `System.String`.

### <a name="interface-types"></a>Typy interfejsów

Interfejs definiuje kontrakt. Klasa lub struktura, która implementuje interfejs musi być zgodne z jego umową. Interfejs może dziedziczyć z wielu interfejsach podstawowych, a klasa lub struktura może zaimplementować wiele interfejsów.

Typy interfejsów są opisane w [interfejsów](interfaces.md).

### <a name="array-types"></a>Typy tablic

Tablica jest strukturą danych, która zawiera zero lub więcej zmiennych, które są dostępne za pośrednictwem obliczanej indeksów. Zmienne zawartych w tablicy jest określana skrótem elementy tablicy są wszystkie tego samego typu i tego typu jest nazywana typ elementu tablicy.

Typy tablicowe są opisane w [tablic](arrays.md).

### <a name="delegate-types"></a>Typy delegatów

Delegat jest strukturą danych, która odwołuje się do co najmniej jednej metody. Na przykład metody, również odwołuje się on do ich odpowiednich wystąpień obiektu.

Najbliższy wielokrotność delegata w C lub C++ jest wskaźnik funkcji, ale wskaźnika funkcji może odwoływać się tylko funkcje statyczne, obiekt delegowany może odwoływać się do statycznych i wystąpienia metody. W tym ostatnim przypadku delegata przechowuje nie tylko odwołanie do metody punktu wejścia, ale również odwołanie do wystąpienia obiektu do wywołania metody.

Typy delegatów są opisane w [delegatów](delegates.md).

## <a name="boxing-and-unboxing"></a>Konwersja boxing i konwersja unboxing

Pojęcie pakowania i rozpakowywania stanowi podstawę do w systemie typu C#. Stanowi Most między *value_type*s i *reference_type*s, umożliwiając dowolnej wartości *value_type* do konwersji do i z typu `object`. Pakowanie i rozpakowywanie pozwala ujednoliconego widoku system typów, w którym wartość dowolnego typu ostatecznie mogą być traktowane jako obiekt.

### <a name="boxing-conversions"></a>Konwersje boxing

Konwersja boxing pozwala *value_type* są niejawnie konwertowane na *reference_type*. Istnieją następujące konwersje boxing:

*  Za pomocą dowolnego *value_type* typowi `object`.
*  Za pomocą dowolnego *value_type* typowi `System.ValueType`.
*  Za pomocą dowolnego *non_nullable_value_type* do dowolnego *interface_type* implementowany przez *value_type*.
*  Za pomocą dowolnego *nullable_type* do dowolnego *interface_type* implementowana przez typ podstawowy elementu *nullable_type*.
*  Za pomocą dowolnego *enum_type* typowi `System.Enum`.
*  Za pomocą dowolnego *nullable_type* z odpowiednią *enum_type* typowi `System.Enum`.
*  Należy pamiętać, że niejawna konwersja z parametrem typu zostaną wykonane jako konwersja boxing Jeśli w czasie wykonywania kończy się konwersji z typu wartości typu odwołania ([niejawne konwersje dotyczące parametrów typu](conversions.md#implicit-conversions-involving-type-parameters)).

Konwersja boxing wartość *non_nullable_value_type* składa się z przydzielanie wystąpienia obiektu i kopiowanie *non_nullable_value_type* wartość do tego wystąpienia.

Konwersja boxing wartość *nullable_type* tworzy odwołanie o wartości null, jeśli jest `null` wartość (`HasValue` jest `false`), lub wynik rozpakowanie, a w przeciwnym razie konwersja boxing podstawową wartość.

Rzeczywisty proces konwersja boxing wartość *non_nullable_value_type* najlepiej tłumaczy opracowująca w wyobraźni istnienie ogólnego ***opakowywanie klasy***, który zachowuje się tak, jakby były zadeklarowane w następujący sposób:

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

OPAKOWYWANIE wartość `v` typu `T` teraz składa się z wykonującego wyrażenie `new Box<T>(v)`i zwraca wynikowy wystąpienia jako wartość typu `object`. W związku z tym instrukcje
```csharp
int i = 123;
object box = i;
```
koncepcyjnie odpowiadają
```csharp
int i = 123;
object box = new Box<int>(i);
```

Klasa pakowania, takich jak `Box<T>` powyżej faktycznie nie istnieje i dynamiczny typ spakowany wartości nie jest faktycznie typu klasy. Zamiast tego typu wartości spakowanej `T` ma typu dynamicznego `T`i wyboru typu dynamicznego przy użyciu `is` operator może po prostu odwoływać się do typu `T`. Na przykład
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
generuje ciąg "`Box contains an int`" na konsoli.

Konwersja boxing oznacza skopiowanie wartości jest ramce. To różni się od wymiany *reference_type* na typ `object`, w których wartość odwoływać się do tego samego wystąpienia w dalszym ciągu i po prostu jest traktowany jako mniej pochodnego typu `object`. Na przykład biorąc pod uwagę deklaracji
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
Poniższe instrukcje
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
zwróci wartość 10 w konsoli, ponieważ operacji niejawnej konwersji boxing, która występuje w przypisanie `p` do `box` powoduje, że wartość `p` do skopiowania. Gdyby `Point` została zadeklarowana `class` zamiast tego wartość 20 będzie wynosić danych wyjściowych, ponieważ `p` i `box` będzie odwoływać się do tego samego wystąpienia.

### <a name="unboxing-conversions"></a>Konwersji rozpakowującej

Zezwala na konwersji rozpakowującej *reference_type* być jawnie konwertowane na *value_type*. Istnieją następujące konwersji rozpakowującej:

*  Z typu `object` do dowolnego *value_type*.
*  Z typu `System.ValueType` do dowolnego *value_type*.
*  Za pomocą dowolnego *interface_type* do dowolnego *non_nullable_value_type* implementującej *interface_type*.
*  Za pomocą dowolnego *interface_type* do dowolnego *nullable_type* implementuje o typie podstawowym *interface_type*.
*  Z typu `System.Enum` do dowolnego *enum_type*.
*  Z typu `System.Enum` do dowolnego *nullable_type* z odpowiednią *enum_type*.
*  Należy pamiętać, że konwersja jawna z parametrem typu zostanie wykonany jako konwersji rozpakowującej, jeśli w czasie wykonywania kończy się konwersja z typu odwołania do typu wartości ([jawne konwersje dynamiczne](conversions.md#explicit-dynamic-conversions)).

Operacja rozpakowania do *non_nullable_value_type* składa się z wcześniejszego sprawdzenia, że wystąpienie obiektu jest zapakowaną wartością danego *non_nullable_value_type*, a następnie skopiować wartość z wystąpienie.

Rozpakowywanie do *nullable_type* generuje wartość null *nullable_type* operandów źródła jest `null`, lub opakowana wynik rozpakowywania wystąpienie obiektu, aby typ podstawowy elementu *nullable_type* inaczej.

Odwołujące się do klasy urojone pakowania, opisanego w poprzedniej sekcji, konwersji rozpakowującej obiektu `box` do *value_type* `T` składa się z wykonującego wyrażenie `((Box<T>)box).value`. W związku z tym instrukcje
```csharp
object box = 123;
int i = (int)box;
```
koncepcyjnie odpowiadają
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

Dla konwersji rozpakowującej do danego *non_nullable_value_type* zakończyło się sukcesem w czasie wykonywania, wartość operandu źródła musi być odwołaniem do wartości spakowanej tego *non_nullable_value_type*. Jeśli źródłowy argument operacji jest `null`, `System.NullReferenceException` zgłaszany. Jeśli źródłowy argument operacji jest odwołaniem do obiektu niezgodne `System.InvalidCastException` zgłaszany.

Dla konwersji rozpakowującej do danego *nullable_type* zakończyło się sukcesem w czasie wykonywania, wartość operandu źródła musi być albo `null` lub odwołanie do wartości spakowanej podstawowych *non_nullable_value_type* z *nullable_type*. Jeśli źródłowy argument operacji jest odwołaniem do obiektu niezgodne `System.InvalidCastException` zgłaszany.

## <a name="constructed-types"></a>Typy utworzone

Deklarację typu ogólnego, oznacza ***niepowiązany typ ogólny*** używany jako "planu" w celu utworzenia wielu różnych typów, za pomocą zastosowania ***argumentów typu***. Argumenty typu są zapisywane w nawiasach kątowych (`<` i `>`) bezpośrednio po nazwie typu ogólnego. Typ, który zawiera co najmniej jeden typ argumentu jest wywoływana ***skonstruowany typ***. Skonstruowany typ może służyć w większości miejsc w języku, w którym nazwa typu, który może występować. Typu rodzajowego niezwiązanego można używać tylko wewnątrz *typeof_expression* ([typeof operator](expressions.md#the-typeof-operator)).

Typy utworzone można również w wyrażeniach jako nazwy proste ([proste nazwy](expressions.md#simple-names)) lub podczas uzyskiwania dostępu do członka ([dostęp do elementu członkowskiego](expressions.md#member-access)).

Gdy *namespace_or_type_name* jest oceniana, tylko wewnętrzne typów przy użyciu poprawnej liczby parametrów są traktowane jako typu. Dlatego jest możliwe użycie tego samego identyfikatora do identyfikacji różnych typów, tak długo, jak typy mają różną liczbę parametrów typu. Jest to przydatne, gdy mieszanie ogólnych i nieogólnych klasach, w tym samym programie:

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

A *type_name* może identyfikować skonstruowanego typu, nawet jeśli go nie określono parametrów typu bezpośrednio. Taka sytuacja może wystąpić, gdy typem jest zagnieżdżony w obrębie deklaracji klasy ogólnej i typu wystąpienia zawierający deklaracji niejawnie używane podczas wyszukiwania nazwy ([zagnieżdżone typy klas ogólnych](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

W niebezpieczny kod nie może pełnić skonstruowanego typu *unmanaged_type* ([typy wskaźników](unsafe-code.md#pointer-types)).

### <a name="type-arguments"></a>Argumenty typu

Każdy argument na liście argumentów typu jest po prostu *typu*.

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

W niebezpieczny kod ([niebezpieczny kod](unsafe-code.md)), *type_argument* nie może być typem wskaźnika. Każdy argument typu musi spełniać żadnych ograniczeń na odpowiedni parametr typu ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).

### <a name="open-and-closed-types"></a>Typy otwarte i zamknięte

Wszystkie typy mogą być klasyfikowane jako ***Otwórz typy*** lub ***zamknięte typy***. Otwarty typ to typ, który obejmuje parametry typu. Więcej szczegółów:

*  Parametr typu definiuje typu otwartego.
*  Typ tablicy jest typem otwartym, tylko wtedy, gdy jego typ elementu jest typem otwartym.
*  Skonstruowany typ jest typem otwartym, tylko wtedy, gdy co najmniej jeden z argumentów typu jest typem otwartym. Skonstruowany typ zagnieżdżony jest typem otwartym, tylko wtedy, gdy jeden lub więcej argumentów typu lub argumentów typu z jej typów zawierających jest typem otwartym.

Zamknięty typ jest typem, który nie jest typem otwartym.

W czasie wykonywania cały kod w obrębie deklaracji typu ogólnego jest wykonywany w kontekście zamknięte skonstruowanego typu, który został utworzony przez stosowanie argumentów typu do deklaracji ogólnej. Każdy parametr typu w ramach typu ogólnego jest powiązany z określonego typu run-time. Przetwarzanie czasu wykonywania wszystkich instrukcji i wyrażeń zawsze odbywa się z typami zamknięte i otwarte typy występuje tylko podczas kompilacji przetwarzania.

Każdy zamknięte skonstruowanego typu ma swój własny zestaw zmiennych statycznych, które nie są współużytkowane z innymi zamknięte typy utworzone. Od typu otwartego nie istnieje w czasie wykonywania, nie istnieją żadne statyczne zmienne skojarzone z typem otwartym. Dwa typy utworzone zamknięte są tego samego typu, jeśli one są konstruowane na podstawie tego samego typu niepowiązanego ogólny i ich odpowiednie argumenty typu tego samego typu.

### <a name="bound-and-unbound-types"></a>Powiązane i niepowiązanych typów

Termin ***niepowiązany typ*** odwołuje się do typu nieogólnego lub niepowiązanych typu ogólnego. Termin ***powiązany typ*** odwołuje się do typu nieogólnego lub skonstruowanego typu.

Typu niepowiązanego odnosi się do jednostki zadeklarowanej przez deklarację typu. Typu rodzajowego niezwiązanego sam nie jest typem i nie można używać jako typ zmiennej, argumentu lub wartości zwracanej lub jako typu podstawowego. Tylko konstrukcja, w którym można się odwoływać niepowiązanych typu ogólnego jest `typeof` wyrażenia ([typeof operator](expressions.md#the-typeof-operator)).

### <a name="satisfying-constraints"></a>Spełniających ograniczenia

Zawsze, gdy odwołanie do skonstruowanego typu lub metody rodzajowej, argumenty podanego typu są porównywane z ograniczeniami parametrów typu zadeklarowanych w ogólnym typie lub metodzie ([ograniczenia parametru typu](classes.md#type-parameter-constraints)). Dla każdego `where` klauzuli, argument typu `A` , który odpowiada nazwany parametr typu jest sprawdzana względem każde ograniczenie w następujący sposób:

*  W przypadku ograniczenia typu klasy, typ interfejsu lub parametrem typu, należy umożliwić `C` reprezentuje ograniczenie z argumentami typu podane zastępują wszystkie parametry typu, które są wyświetlane w ograniczeniu. Aby spełniać ograniczenie, musi być tak, że tekst `A` jest konwertowany na typ `C` za pomocą jednej z następujących czynności:
    * Konwersja tożsamości ([konwersji tożsamości](conversions.md#identity-conversion))
    * Niejawna konwersja odwołania ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions))
    * Konwersja boxing ([konwersje Boxing](conversions.md#boxing-conversions)) pod warunkiem, że typ A jest typem wartości niedopuszczającym wartości.
    * Niejawna konwersja parametru odwołania, konwersji boxing lub typu z parametrem typu `A` do `C`.
*  Jeśli ograniczenie jest ograniczenie typu odwołania (`class`), typ `A` muszą spełniać jeden z następujących czynności:
    * `A` jest typu interfejsu, typu klasy, typu delegata lub typu tablicowego. Należy pamiętać, że `System.ValueType` i `System.Enum` są typami odwołań, które spełniają kryteria tego ograniczenia.
    * `A` jest parametrem typu, który jest znany jako typu odwołania ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).
*  Jeśli ograniczenie jest ograniczenie typów wartości (`struct`), typ `A` muszą spełniać jeden z następujących czynności:
    * `A` jest typ struktury lub typu wyliczeniowego, ale nie typu dopuszczającego wartość null. Należy pamiętać, że `System.ValueType` i `System.Enum` są typami odwołań, które nie spełniają tego ograniczenia.
    * `A` jest to parametr typu o wartości ograniczenia typu ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).
*  Jeśli to ograniczenie jest ograniczenie konstruktora `new()`, typ `A` nie może być `abstract` i musi mieć publicznego konstruktora bez parametrów. To jest spełniony, gdy jest spełniony jeden z następujących czynności:
    * `A` jest typem wartości, ponieważ wszystkie typy wartości mają publicznego konstruktora domyślnego ([domyślne konstruktory](types.md#default-constructors)).
    * `A` jest to parametr typu o ograniczenie konstruktora ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).
    * `A` jest to parametr typu o wartości ograniczenia typu ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).
    * `A` to klasa, która nie jest `abstract` i zawiera zadeklarowany w sposób jawny `public` konstruktora bez parametrów.
    * `A` nie jest `abstract` i ma domyślnego konstruktora ([domyślne konstruktory](classes.md#default-constructors)).

Błąd kompilacji występuje, jeśli co najmniej jeden z ograniczenia parametru typu nie są spełnione przez argumenty danego typu.

Ponieważ parametry typu nie są dziedziczone, ograniczenia nigdy nie są dziedziczone albo. W poniższym przykładzie `D` należy określić ograniczenie jego parametr typu `T` tak, aby `T` spełnia ograniczenia nałożone przez klasę bazową `B<T>`. Z kolei klasa `E` nie trzeba określać ograniczenie, `List<T>` implementuje `IEnumerable` dla każdego `T`.

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>Parametry typu

Parametr typu jest identyfikatorem wyznaczania typu wartości lub typ referencyjny, powiązany z parametru w czasie wykonywania.

```antlr
type_parameter
    : identifier
    ;
```

Ponieważ parametr typu mogą być utworzone przy użyciu wielu różnych rzeczywisty typ argumentów, parametry typu mają nieco inne operacje i ograniczenia od innych typów. Należą do nich następujące elementy:

*  Parametr typu nie można bezpośrednio, aby zadeklarować klasę bazową ([klasy bazowej](classes.md#base-class)) lub interfejsu ([list parametrów typu Variant](interfaces.md#variant-type-parameter-lists)).
*  Reguły dotyczące wyszukać składowej w typie, czy parametry są zależne od ograniczeń, jeśli istnieje, jest stosowane do parametru typu. Szczegółowo w [wyszukanie członka](expressions.md#member-lookup).
*  Dostępne konwersje dla parametru typu są zależne od ograniczeń, ewentualnej zastosowany do parametru typu. Szczegółowo w [niejawne konwersje dotyczące parametrów typu](conversions.md#implicit-conversions-involving-type-parameters) i [jawne konwersje dynamiczne](conversions.md#explicit-dynamic-conversions).
*  Literał `null` nie można przekonwertować na typ, określone przez parametr typu, z wyjątkiem, jeśli wiadomo, że parametr typu jako typów referencyjnych ([niejawne konwersje dotyczące parametrów typu](conversions.md#implicit-conversions-involving-type-parameters)). Jednak `default` wyrażenia ([domyślna wartość wyrażenia](expressions.md#default-value-expressions)) mogą być używane zamiast tego. Ponadto wartości o typie podanym przez parametru typu można porównać z `null` przy użyciu `==` i `!=` ([Operatory równości typu odwołania](expressions.md#reference-type-equality-operators)), chyba że parametr typu ma ograniczenia typu wartości.
*  A `new` wyrażenia ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)) należy używać tylko z parametrem typu Jeśli parametr typu jest ograniczony przez *constructor_constraint* lub typ ograniczenia (wartości[ Ograniczenia parametru typu](classes.md#type-parameter-constraints)).
*  Parametr typu nie można używać w dowolnym miejscu w atrybucie.
*  Parametr typu nie można używać w dostęp do elementu członkowskiego ([dostęp do elementu członkowskiego](expressions.md#member-access)) lub wpisz nazwę ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)) do identyfikowania statycznej składowej lub typu zagnieżdżonego.
*  W niebezpieczny kod parametr typu nie może zostać użyty jako *unmanaged_type* ([typy wskaźników](unsafe-code.md#pointer-types)).

Jako typ parametry typu są wyłącznie konstrukcję kompilacji. W czasie wykonywania każdego parametru typu jest powiązany z typu run-time, który został określony poprzez dostarczenie argument typu w deklaracji typu ogólnego. Zatem typ zmiennej jest zadeklarowana z będzie parametr typu, w czasie wykonywania, będą zamknięte skonstruowanego typu ([otwarte i zamknięte typy](types.md#open-and-closed-types)). Wykonanie czasu wykonywania wszystkich instrukcji i wyrażeń zawierających parametry typu używa rzeczywisty typ, który został dostarczony jako argument typu dla tego parametru.

## <a name="expression-tree-types"></a>Typy drzewa wyrażeń

***Drzewa wyrażeń*** zezwolić wyrażenia lambda może być reprezentowana jako struktur danych, zamiast kodu wykonywalnego. Drzewa wyrażeń są wartościami ***typy drzewa wyrażenie*** formularza `System.Linq.Expressions.Expression<D>`, gdzie `D` jest dowolny typ delegata. W pozostałej części tej specyfikacji odnoszą się do tych typów, przy użyciu skrótu `Expression<D>`.

Jeśli istnieje konwersja z wyrażenia lambda do typu delegata `D`, istnieje również konwersji na typ drzewa wyrażenia `Expression<D>`. Natomiast konwersji wyrażenia lambda do typu delegata generuje obiekt delegowany, który odwołuje się kod wykonywalny dla wyrażenia lambda, konwersja na typ drzewa wyrażeń tworzy reprezentację w postaci drzewa wyrażeń, wyrażenia lambda.

Drzewa wyrażeń są wydajne danych w pamięci reprezentacje wyrażeń lambda i upewnij struktury wyrażenia lambda, przejrzyste i jawne.

Podobnie jak typ delegata `D`, `Expression<D>` ma parametr i zwracane typy, które są takie same, jak z `D`.

Poniższy przykład przedstawia wyrażenie lambda, zarówno kodu wykonywalnego, jak i w postaci drzewa wyrażenie. Ponieważ istnieje konwersja `Func<int,int>`, również istnieje konwersja `Expression<Func<int,int>>`:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

Po tych przypisań delegata `del` odwołuje się do metody, która zwraca `x + 1`i drzewa wyrażeń `exp` odwołuje się do struktury danych, który opisuje wyrażenie `x => x + 1`.

Dokładne definicji typu ogólnego `Expression<D>` oraz dokładne zasady Konstruowanie drzewa wyrażeń, gdy wyrażenie lambda jest konwertowany na typ drzewa wyrażenia są poza zakresem tej specyfikacji.

Ważne dla zapewnienia jawnego są dwie rzeczy:

*  Nie wszystkie wyrażenia lambda można konwertować na drzewa wyrażeń. Na przykład nie można przedstawić wyrażeń lambda z treści instrukcji i wyrażeń lambda, zawierające wyrażenia przypisania. W takich przypadkach konwersji jest nadal istnieje, ale zakończy się niepowodzeniem w czasie kompilacji. Wyjątki te są szczegółowo opisane w [konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions).
*   `Expression<D>` udostępnia metodę wystąpienia `Compile` wytwarzająca delegat typu `D`:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    Ten delegat wywołania powoduje, że kod reprezentowany przez drzewo wyrażenia do wykonania. W związku z tym biorąc pod uwagę powyższe definicje, del i del2 są równoważne i dwie poniższe instrukcje mają ten sam efekt:

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    Po wykonaniu tego kodu `i1` i `i2` będziecie mieli wartość `2`.

