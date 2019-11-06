---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616137"
---
# <a name="types"></a>Types

Typy C# języków są podzielone na dwie główne kategorie: ***typy wartości*** i ***typy odwołań***. Typy wartości i typy referencyjne mogą być ***typami ogólnymi***, które przyjmują jeden lub więcej ***parametrów typu***. Parametry typu mogą wyznaczyć zarówno typy wartości, jak i typy odwołań.

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

Końcowa Kategoria typów, wskaźników jest dostępna tylko w niebezpiecznym kodzie. Omówiono to dokładniej w [typach wskaźników](unsafe-code.md#pointer-types).

Typy wartości różnią się od typów referencyjnych w zmiennych typach wartości bezpośrednio zawierają swoje dane, natomiast zmienne typów referencyjnych przechowują ***odwołania*** do danych, które są znane jako ***obiekty***. W przypadku typów referencyjnych istnieje możliwość, że dwie zmienne odwołują się do tego samego obiektu, i w tym przypadku operacje na jednej zmiennej mają wpływ na obiekt, do którego odwołuje się inna zmienna. W przypadku typów wartości zmiennych każda z nich ma własną kopię danych i nie jest możliwe wykonywanie operacji na nich, aby wpływać na inne.

C#System typów jest jednorodny tak, że wartość dowolnego typu może być traktowana jako obiekt. Każdy typ C# bezpośrednio lub pośrednio pochodzi od typu klasy `object`, a `object` jest ostateczną klasą bazową wszystkich typów. Wartości typów referencyjnych są traktowane jako obiekty po prostu przez wyświetlanie wartości jako typu `object`. Wartości typów wartości są traktowane jako obiekty przez wykonywanie operacji pakowania i rozpakowywania ([opakowanie i rozpakowywanie](types.md#boxing-and-unboxing)).

## <a name="value-types"></a>Typy wartości

Typ wartości jest typem struktury lub typem wyliczenia. C#dostarcza zestaw wstępnie zdefiniowanych typów struktur nazywanych ***typami prostymi***. Typy proste są identyfikowane za pomocą słów zarezerwowanych.

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

W przeciwieństwie do zmiennej typu referencyjnego zmienna typu wartości może zawierać wartość `null` tylko wtedy, gdy typ wartości jest typem dopuszczającym wartość null.  Dla każdego typu wartości niedopuszczających wartości null istnieje odpowiadający mu typ wartości null oznaczający ten sam zestaw wartości i wartość `null`.

Przypisanie do zmiennej typu wartości tworzy kopię przypisanej wartości. Różni się to od przypisywania do zmiennej typu referencyjnego, która kopiuje odwołanie, ale nie obiekt identyfikowany przez odwołanie.

### <a name="the-systemvaluetype-type"></a>Typ System. ValueType

Wszystkie typy wartości niejawnie dziedziczą z klasy `System.ValueType`, która z kolei dziedziczy z klasy `object`. Nie jest możliwe, aby żaden typ dziedziczył z typu wartości, a typy wartości są w ten sposób niejawnie zapieczętowane ([klasy zapieczętowane](classes.md#sealed-classes)).

Należy pamiętać, że `System.ValueType` nie należy do *value_type*. Zamiast tego jest to *class_type* , z którego wszystkie *value_type*s są wyprowadzane automatycznie.

### <a name="default-constructors"></a>Konstruktory domyślne

Wszystkie typy wartości niejawnie deklarują publiczny Konstruktor wystąpienia bez parametrów o nazwie ***Konstruktor domyślny***. Konstruktor domyślny zwraca wystąpienie inicjowane przez zero, znane jako ***wartość domyślna*** dla typu wartości:

*  Dla wszystkich *Simple_Type*s wartość domyślna to wartość wytworzony przez wzorzec bitowy dla wszystkich zer:
    * W przypadku `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`i `ulong`wartość domyślna to `0`.
    * W przypadku `char`wartość domyślna to `'\x0000'`.
    * W przypadku `float`wartość domyślna to `0.0f`.
    * W przypadku `double`wartość domyślna to `0.0d`.
    * W przypadku `decimal`wartość domyślna to `0.0m`.
    * W przypadku `bool`wartość domyślna to `false`.
*  W przypadku `E`*enum_type* wartość domyślna to `0`, konwertowana na typ `E`.
*  Dla *struct_type*wartość domyślna to wartość utworzona przez ustawienie wszystkich pól typu wartości na wartości domyślne i wszystkie pola typu odwołania do `null`.
*  Dla *nullable_type* wartość domyślna to wystąpienie, dla którego właściwość `HasValue` ma wartość false, a właściwość `Value` jest niezdefiniowana. Wartość domyślna jest również znana jako ***wartość null*** typu Nullable.

Podobnie jak każdy inny Konstruktor instancji, Konstruktor domyślny typu wartości jest wywoływany przy użyciu operatora `new`. Ze względu na wydajność to wymaganie nie jest przeznaczone do rzeczywistej implementacji wywołania konstruktora. W poniższym przykładzie zmienne `i` i `j` są inicjowane na wartość zero.

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

Ponieważ każdy typ wartości niejawnie ma publicznego konstruktora wystąpienia bez parametrów, nie jest możliwe, aby typ struktury zawierał jawną deklarację konstruktora bez parametrów. Typ struktury jest jednak dozwolony do deklarowania sparametryzowanych konstruktorów wystąpień ([konstruktorów](structs.md#constructors)).

### <a name="struct-types"></a>Typy struktur

Typ struktury jest typem wartości, który może deklarować stałe, pola, metody, właściwości, indeksatory, operatory, konstruktory wystąpień, konstruktory statyczne i typy zagnieżdżone. Deklaracja typów struktury jest opisana w [deklaracji struktury](structs.md#struct-declarations).

### <a name="simple-types"></a>Typy proste

C#dostarcza zestaw wstępnie zdefiniowanych typów struktur nazywanych ***typami prostymi***. Typy proste są identyfikowane za pomocą słów zarezerwowanych, ale te słowa zastrzeżone są po prostu aliasami dla wstępnie zdefiniowanych typów struktur w przestrzeni nazw `System`, zgodnie z opisem w poniższej tabeli.


| __Słowo zastrzeżone__ | __Typ aliasowany__ |
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

Ponieważ typ prosty jest aliasem typu struktury, każdy typ prosty ma składowe. Na przykład `int` ma członków zadeklarowanych w `System.Int32` i członków dziedziczonych z `System.Object`i są dozwolone następujące instrukcje:

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

Proste typy różnią się od innych typów struktur w tym, że dopuszczają pewne dodatkowe operacje:

*  Większość typów prostych pozwala na tworzenie wartości, pisząc *literały* ([literały](lexical-structure.md#literals)). Na przykład `123` jest literałem typu `int`, a `'a'` jest literałem typu `char`. C#nie udostępnia żadnych elementów dla literałów typów struktury ogólnie, a wartości inne niż domyślne innych typów struktury są ostatecznie tworzone przy użyciu konstruktorów wystąpień tych typów struktury.
*  Gdy operandy wyrażenia to wszystkie typy proste, możliwe jest, aby kompilator obliczał wyrażenie w czasie kompilacji. Takie wyrażenie jest znane jako *constant_expression* ([wyrażenia stałe](expressions.md#constant-expressions)). Wyrażenia obejmujące Operatory zdefiniowane przez inne typy struktur nie są traktowane jako wyrażenia stałe.
*  Za pomocą deklaracji `const` można zadeklarować stałe typów prostych ([stałych](classes.md#constants)). Nie jest możliwe posiadanie stałych innych typów struktur, ale podobny efekt jest dostarczany przez pola `static readonly`.
*  Konwersje obejmujące typy proste mogą uczestniczyć w ocenie operatorów konwersji zdefiniowanych przez inne typy struktur, ale Operator konwersji zdefiniowany przez użytkownika nigdy nie uczestniczy w ocenie innego operatora zdefiniowanego przez użytkownika ([Obliczanie Konwersje zdefiniowane przez użytkownika](conversions.md#evaluation-of-user-defined-conversions)).

### <a name="integral-types"></a>Typy całkowite

C#obsługuje dziewięć typów całkowitych: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`i `char`. Typy całkowite mają następujące rozmiary i zakresy wartości:

*  Typ `sbyte` reprezentuje 8-bitową liczbę całkowitą ze znakiem z wartościami z przedziału od-128 do 127.
*  Typ `byte` reprezentuje 8-bitową liczbę całkowitą bez znaku z wartościami z zakresu od 0 do 255.
*  Typ `short` reprezentuje podpisane 16-bitowe liczby całkowite z wartościami z przedziału od-32768 do 32767.
*  Typ `ushort` reprezentuje 16-bitową liczbę całkowitą bez znaku z wartościami z zakresu od 0 do 65535.
*  Typ `int` reprezentuje podpisane 32-bitowe liczby całkowite z wartościami z zakresu od – 2147483648 do 2147483647.
*  Typ `uint` reprezentuje niepodpisane 32-bitowe liczby całkowite z wartościami z zakresu od 0 do 4294967295.
*  Typ `long` reprezentuje podpisane 64-bitowe liczby całkowite z wartościami z przedziału od-zakresu od do 9223372036854775807.
*  Typ `ulong` reprezentuje niepodpisane 64-bitowe liczby całkowite z wartościami z zakresu od 0 do 18446744073709551615 są.
*  Typ `char` reprezentuje 16-bitową liczbę całkowitą bez znaku z wartościami z zakresu od 0 do 65535. Zestaw możliwych wartości dla typu `char` odpowiada zestaw znaków Unicode. Chociaż `char` ma tę samą reprezentację co `ushort`, nie wszystkie operacje dozwolone na jednym typie są dozwolone na drugim.

Operatory jednoargumentowe i binarne typu całkowitego są zawsze używane z podpisaną dokładnością 32-bitową, bez znaku 64 64 32

*  Dla operatory jednoargumentowe `+` i `~` operand jest konwertowany na typ `T`, gdzie `T` jest pierwszym z `int`, `uint`, `long`i `ulong`, które mogą w pełni reprezentować wszystkie możliwe wartości operandu. Operacja jest następnie wykonywana przy użyciu dokładności typu `T`, a typ wyniku jest `T`.
*  Dla operatora jednoargumentowego `-` operand jest konwertowany na typ `T`, gdzie `T` jest pierwszym z `int` i `long`, które mogą w pełni reprezentować wszystkie możliwe wartości operandu. Operacja jest następnie wykonywana przy użyciu dokładności typu `T`, a typ wyniku jest `T`. Jednoargumentowego operatora `-` nie można zastosować do operandów typu `ulong`.
*  Dla `+`binarnych, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`i `<=` operatorów operandy są konwertowane na typ `T`, gdzie `T` jest pierwszym z `int`, `uint`, `long`i `ulong`, które mogą w pełni reprezentować wszystkie możliwe wartości obu operandów. Operacja jest następnie wykonywana przy użyciu dokładności typu `T`, a typ wyniku jest `T` (lub `bool` dla operatorów relacyjnych). Dla jednego operandu nie można używać typu `long`, a drugi jako typ `ulong` z operatorami dwuargumentowymi.
*  Dla operatorów `<<` binarnych i `>>`, lewy operand jest konwertowany na typ `T`, gdzie `T` jest pierwszym z `int`, `uint`, `long`i `ulong`, który może w pełni reprezentować wszystkie możliwe wartości operandu. Operacja jest następnie wykonywana przy użyciu dokładności typu `T`, a typ wyniku jest `T`.

Typ `char` jest klasyfikowany jako typ całkowity, ale różni się od innych typów całkowitych na dwa sposoby:

*  Brak niejawnych konwersji z innych typów na typ `char`. W szczególności, mimo że typy `sbyte`, `byte`i `ushort` zawierają zakresy wartości, które są w pełni zaprezentowane przy użyciu typu `char`, niejawne konwersje z `sbyte`, `byte`lub `ushort` do `char` nie istnieją.
*  Stałe typu `char` muszą być zapisywane jako *character_literal*s lub *integer_literal*s w połączeniu z rzutem na typ `char`. Na przykład `(char)10` jest taka sama jak `'\x000A'`.

Operatory `checked` i `unchecked` i instrukcje są używane do kontrolowania sprawdzania przepełnienia dla operacji arytmetycznych typu całkowitego i konwersji ([Operatory sprawdzone i niezaznaczone](expressions.md#the-checked-and-unchecked-operators)). W kontekście `checked` przepełnienie powoduje błąd w czasie kompilacji lub powoduje, że `System.OverflowException` zostać zgłoszone. W kontekście `unchecked` przepełnienia są ignorowane, a wszystkie bity o dużej kolejności, które nie mieszczą się w typie docelowym, są odrzucane.

### <a name="floating-point-types"></a>Typy zmiennoprzecinkowe

C#obsługuje dwa typy zmiennoprzecinkowe: `float` i `double`. Typy `float` i `double` są reprezentowane przy użyciu 32-bitowych jednoprecyzyjne i 64-bitowe formatów IEEE 754 o podwójnej precyzji, które udostępniają następujące zestawy wartości:

*  Dodatnia zero i ujemna zero. W większości sytuacji dodatnie zero i ujemna zero zachowują się identycznie jak wartość zero, ale pewne operacje odróżniają między dwoma ([operatorem dzielenia](expressions.md#division-operator)).
*  Nieskończoność dodatnia i ujemna nieskończoność. Nieskończoności są wytwarzane przez takie operacje jak dzielenie liczby niezerowej przez zero. Na przykład `1.0 / 0.0` daje nieskończoność dodatnią, a `-1.0 / 0.0` daje ujemną nieskończoność.
*  Wartość ***nie-liczbowa*** , często skracany NaN. NaNs są tworzone przez nieprawidłowe operacje zmiennoprzecinkowe, takie jak dzielenie zera przez zero.
*  Skończoną zbiór niezerowych wartości formularza `s * m * 2^e`, gdzie `s` ma wartość 1 lub-1, a `m` i `e` są określane przez określony typ zmiennoprzecinkowy: dla `float`, `0 < m < 2^24` i `-149 <= e <= 104`i dla `double`, `0 < m < 2^53` i `-1075 <= e <= 970`. Nieznormalizowane liczby zmiennoprzecinkowe są uznawane za prawidłowe wartości niezerowe.

Typ `float` może reprezentować wartości z zakresu od około `1.5 * 10^-45` do `3.4 * 10^38` z dokładnością do 7 cyfr.

Typ `double` może reprezentować wartości z zakresu od około `5.0 * 10^-324` do `1.7 × 10^308` z dokładnością do 15-16 cyfr.

Jeśli jeden z argumentów operatora binarnego jest typu zmiennoprzecinkowego, drugi operand musi być typu całkowitego lub typu zmiennoprzecinkowego, a operacja jest szacowana w następujący sposób:

*  Jeśli jeden z operandów jest typu całkowitego, ten operand jest konwertowany na typ zmiennoprzecinkowy drugiego operandu.
*  Następnie, jeśli jeden z operandów jest typu `double`, drugi operand jest konwertowany na `double`, operacja jest wykonywana przy użyciu co najmniej `double` zakresu i precyzji, a typ wyniku jest `double` (lub `bool` dla operatorów relacyjnych).
*  W przeciwnym razie operacja jest wykonywana przy użyciu co najmniej `float` zakresu i precyzji, a typ wyniku jest `float` (lub `bool` dla operatorów relacyjnych).

Operatory zmiennoprzecinkowe, w tym operatory przypisania, nigdy nie generują wyjątków. Zamiast tego w wyjątkowych sytuacjach operacje zmiennoprzecinkowe generują zero, nieskończoność lub NaN, zgodnie z poniższym opisem:

*  Jeśli wynik operacji zmiennoprzecinkowej jest za mały dla formatu docelowego, wynik operacji zmieni się na dodatnią zero lub ujemną zero.
*  Jeśli wynik operacji zmiennoprzecinkowej jest za duży dla formatu docelowego, wynik operacji będzie dodatnią nieskończoność lub ujemną nieskończoność.
*  Jeśli operacja zmiennoprzecinkowa jest nieprawidłowa, wynikiem operacji jest NaN.
*  Jeśli jeden lub oba operandy operacji zmiennoprzecinkowej to NaN, wynik operacji jest NaN.

Operacje zmiennoprzecinkowe mogą być wykonywane z większą dokładnością niż typ wyniku operacji. Na przykład niektóre architektury sprzętu obsługują typ zmiennoprzecinkowy "rozszerzony" lub "Long Double" z większym zakresem i dokładnością niż typ `double` i niejawnie wykonują wszystkie operacje zmiennoprzecinkowe przy użyciu tego typu większej precyzji. W przypadku nadmiernego kosztu wydajności można przeprowadzić takie architektury sprzętu w celu wykonywania operacji zmiennoprzecinkowych o mniejszej precyzji, a nie wymaganie, aby implementacja przepadał zarówno na C# wydajność, jak i dokładność, a także mieć wyższy poziom precyzji do użycia dla wszystkich operacji zmiennoprzecinkowych. Inne niż dostarczanie bardziej precyzyjnych wyników, rzadko ma wszelkie wymierne efekty. Jednakże w wyrażeniach `x * y / z`, gdzie mnożenie generuje wynik, który znajduje się poza zakresem `double`, ale kolejny podział wprowadza wynik z powrotem do `double`ego zakresu, fakt, że wyrażenie jest oceniane w Format wyższego poziomu może spowodować powstanie skończonego wyniku zamiast nieskończoności.

### <a name="the-decimal-type"></a>Typ dziesiętny

Typ `decimal` jest 128-bitowym typem danych odpowiednim dla obliczeń finansowych i pieniężnych. Typ `decimal` może reprezentować wartości z zakresu od `1.0 * 10^-28` do około `7.9 * 10^28` z 28-29 cyframi znaczącymi.

Skończoną zbiór wartości typu `decimal` ma postać `(-1)^s * c * 10^-e`, gdzie znak `s` ma wartość 0 lub 1, współczynnik `c` jest podawany przez `0 <= *c* < 2^96`, a `e` skali jest taka, że `0 <= e <= 28`. Typ `decimal` nie obsługuje podpisanych zer, nieskończoności lub NaN. `decimal` jest reprezentowane jako 96-bitowa liczba całkowita skalowana z potęgą dziesięciu. Dla `decimal`s z wartością bezwzględną mniejszą niż `1.0m`, wartość jest dokładna dla 28 miejsca dziesiętnego, ale nie więcej. Dla `decimal`s z wartością bezwzględną większą lub równą `1.0m`, wartość jest dokładna 28 lub 29 cyfr. W przeciwieństwie do typów danych `float` i `double`, liczba ułamków dziesiętnych, takich jak 0,1, można dokładnie przedstawić w reprezentacji `decimal`. W `float` i `double` reprezentacje takie liczby są często nieskończonymi ułamkami, co sprawia, że te reprezentacje są bardziej podatne na błędy zaokrąglania.

Jeśli jeden z argumentów operatora binarnego jest typu `decimal`, drugi operand musi być typu całkowitego lub typu `decimal`. Jeśli jest obecny operand typu całkowitego, jest konwertowany na `decimal` przed wykonaniem operacji.

Wynikiem operacji na wartościach typu `decimal` jest to, że wynikiem jest obliczenie dokładnego wyniku (zachowanie skali, zgodnie z definicją dla każdego operatora), a następnie zaokrąglenie w celu dopasowania do reprezentacji. Wyniki są zaokrąglane do najbliższej reprezentacji wartości i, gdy wynik jest równie zbliżony do dwóch wartości, które można reprezentować, do wartości, która ma parzystą liczbę w najmniejszej liczbie cyfr (jest to nazywane "zaokrągleniem przez Bank"). Wynik zero zawsze ma znak 0 i skalę równą 0.

Jeśli operacja arytmetyczna dziesiętna generuje wartość mniejszą lub równą `5 * 10^-29` w wartości bezwzględnej, wynik operacji zmieni się na zero. Jeśli `decimal` operacja arytmetyczna generuje wynik, który jest zbyt duży dla formatu `decimal`, zostanie zgłoszony `System.OverflowException`.

Typ `decimal` ma większą precyzję, ale mniejszy zakres niż typy zmiennoprzecinkowe. W ten sposób konwersje z typów zmiennoprzecinkowych na `decimal` mogą generować wyjątki przepełnienia, a konwersje z `decimal` do typów zmiennoprzecinkowych mogą spowodować utratę precyzji. Z tego względu nie istnieją niejawne konwersje między typami zmiennoprzecinkowymi a `decimal`i bez jawnych rzutowania nie można mieszać argumentów operacji zmiennoprzecinkowych i `decimal` w tym samym wyrażeniu.

### <a name="the-bool-type"></a>Typ bool

Typ `bool` reprezentuje logiczne wartości logicznych. Możliwe wartości typu `bool` to `true` i `false`.

Między `bool` i innymi typami nie istnieją żadne Konwersje standardowe. W szczególności typ `bool` jest odrębny i oddzielany od typów całkowitych i nie można użyć wartości `bool` zamiast wartości całkowitej i na odwrót.

W językach C i C++ , wartość zerowa lub zmiennoprzecinkowa albo wskaźnik o wartości null można przekonwertować na wartość logiczną `false`, a wartość typu całkowitego lub zmiennoprzecinkowego lub wskaźnik o wartości innej niż null można przekonwertować na wartość logiczną `true`. W C#programie takie konwersje są wykonywane przez jawne porównanie wartości całkowitej lub zmiennoprzecinkowej na zero lub przez jawne porównanie odwołania do obiektu `null`.

### <a name="enumeration-types"></a>Typów wyliczeniowych

Typ wyliczeniowy jest typem odrębnym o nazwanych stałych. Każdy typ wyliczeniowy ma typ podstawowy, który musi mieć `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` lub `ulong`. Zestaw wartości typu wyliczenia jest taki sam jak zestaw wartości typu podstawowego. Wartości typu wyliczenia nie są ograniczone do wartości nazwanych stałych. Typy wyliczeniowe są definiowane za poorednictwem deklaracji wyliczenia ([deklaracji wyliczenia](enums.md#enum-declarations)).

### <a name="nullable-types"></a>Typy dopuszczające wartości null

Typ dopuszczający wartość null może reprezentować wszystkie wartości jego ***typu podstawowego*** oraz dodatkową wartość null. Typ dopuszczający wartość null jest zapisywana `T?`, gdzie `T` jest typem podstawowym. Ta składnia jest skrótem `System.Nullable<T>`, a dwa formularze mogą być używane zamiennie.

***Typ wartości niedopuszczający wartości null*** to dowolny typ wartości inny niż `System.Nullable<T>` i jego skrócona `T?` (dla dowolnej `T`) oraz parametr typu, który jest ograniczony do niedopuszczający wartości null (czyli dowolny parametr typu z `struct` ograniczenie). Typ `System.Nullable<T>` określa ograniczenie typu wartości dla `T` ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), co oznacza, że typ podstawowy typu dopuszczającego wartość null może być dowolnym typem wartości niedopuszczających wartości null. Typ podstawowy typu dopuszczającego wartość null nie może być typem dopuszczającym wartość null lub typem referencyjnym. Na przykład `int??` i `string?` są nieprawidłowymi typami.

Wystąpienie typu dopuszczającego wartość null `T?` ma dwie publiczne właściwości tylko do odczytu:

*  Właściwość `HasValue` typu `bool`
*  Właściwość `Value` typu `T`

Wystąpienie, dla którego `HasValue` ma wartość true, mówi o wartości innej niż null. Wystąpienie o wartości innej niż null zawiera znaną wartość, a `Value` zwraca tę wartość.

Wystąpienie, dla którego `HasValue` ma wartość false, mówi o wartości null. Wystąpienie o wartości null ma niezdefiniowaną wartość. Próba odczytania `Value` wystąpienia o wartości null powoduje zgłoszenie `System.InvalidOperationException`. Proces uzyskiwania dostępu do właściwości `Value` wystąpienia wartości null jest określany jako ***odpakowanie***.

Oprócz konstruktora domyślnego każdy typ dopuszczający wartość null `T?` ma konstruktora publicznego, który przyjmuje jeden argument typu `T`. Nadana wartość `x` typu `T`, wywołanie konstruktora w postaci

```csharp
new T?(x)
```
tworzy wystąpienie `T?` o wartości innej niż null, dla którego `x`właściwości `Value`. Proces tworzenia wystąpienia o typie innym niż null dla danej wartości jest określany jako ***Zawijanie***.

Konwersje niejawne są dostępne z literału `null` do `T?` ([konwersje literałów wartości null](conversions.md#null-literal-conversions)) i z `T` do `T?` ([niejawne konwersje dopuszczające wartość null](conversions.md#implicit-nullable-conversions)).

## <a name="reference-types"></a>Typy odwołań

Typ referencyjny to typ klasy, typ interfejsu, typ tablicy lub typ delegata.

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

Wartość typu odwołania jest odwołaniem do ***wystąpienia*** typu, które jest znane jako ***obiekt***. Wartość specjalna `null` jest zgodna ze wszystkimi typami odwołań i wskazuje brak wystąpienia.

### <a name="class-types"></a>Typy klas

Typ klasy definiuje strukturę danych, która zawiera składowe danych (stałe i pola), składowe funkcji (metody, właściwości, zdarzenia, indeksatory, operatory, konstruktory wystąpień, destruktory i konstruktory statyczne) i typy zagnieżdżone. Typy klas obsługują dziedziczenie, mechanizm, za pomocą którego klasy pochodne mogą poszerzać i specjalizację klas bazowych. Wystąpienia typów klas są tworzone przy użyciu *object_creation_expression*s ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)).

Typy klas są opisane w [klasach](classes.md).

Niektóre wstępnie zdefiniowane typy klas mają specjalne znaczenie w C# języku, zgodnie z opisem w poniższej tabeli.


| __Typ klasy__     | __Opis__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | Ostateczną klasę bazową wszystkich innych typów. Zobacz [Typ obiektu](types.md#the-object-type). | 
| `System.String`    | Typ ciągu C# języka. Zobacz [typ ciągu](types.md#the-string-type).         |
| `System.ValueType` | Klasa bazowa wszystkich typów wartości. Zobacz [Typ System. ValueType](types.md#the-systemvaluetype-type).          |
| `System.Enum`      | Klasa bazowa wszystkich typów wyliczeniowych. Zobacz [wyliczenia](enums.md).              |
| `System.Array`     | Klasa bazowa wszystkich typów tablicowych. Zobacz [tablice](arrays.md).             |
| `System.Delegate`  | Klasa bazowa wszystkich typów delegatów. Zobacz [Delegaty](delegates.md).          |
| `System.Exception` | Klasa bazowa wszystkich typów wyjątków. Zobacz [wyjątki](exceptions.md).         |

### <a name="the-object-type"></a>Typ obiektu

Typ klasy `object` jest ostateczną klasą bazową wszystkich innych typów. Każdy typ C# bezpośrednio lub pośrednio pochodzi od typu klasy `object`.

Słowo kluczowe `object` jest po prostu aliasem wstępnie zdefiniowanej klasy `System.Object`.

### <a name="the-dynamic-type"></a>Typ dynamiczny

Typ `dynamic`, taki jak `object`, może odwoływać się do dowolnego obiektu. Gdy operatory są stosowane do wyrażeń typu `dynamic`, ich rozdzielczości są odroczone do momentu uruchomienia programu. W takim przypadku, jeśli operatora nie można legalnie zastosować do obiektu, do którego się odwoływano, podczas kompilacji nie podano żadnego błędu. Zamiast tego wyjątek zostanie wygenerowany, gdy rozwiązanie operatora zakończy się niepowodzeniem w czasie wykonywania.

Jego celem jest umożliwienie dynamicznego powiązania, które opisano szczegółowo w temacie [dynamiczne wiązanie](expressions.md#dynamic-binding).

`dynamic` jest uważana za identyczny `object` z wyjątkiem sytuacji, gdy są one następujące:

*  Operacje na wyrażeniach typu `dynamic` można powiązać dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)).
*  Wnioskowanie o typie ([wnioskowanie o typie](expressions.md#type-inference)) będzie preferowane `dynamic` przez `object`, jeśli oba są kandydatami.

Ze względu na tę równoważność są następujące:

*  Istnieje niejawna konwersja tożsamości między `object` i `dynamic`, i między tworzonymi typami, które są takie same podczas wymiany `dynamic` z `object`
*  Konwersje niejawne i jawne do i z `object` są również stosowane do i z `dynamic`.
*  Sygnatury metod, które są takie same, gdy zastępowanie `dynamic` z `object` są uważane za ten sam podpis
*  Typ `dynamic` jest odróżnienie od `object` w czasie wykonywania.
*  Wyrażenie typu `dynamic` jest określane jako ***wyrażenie dynamiczne***.

### <a name="the-string-type"></a>Typ ciągu

Typ `string` jest typem klasy zapieczętowanej, która dziedziczy bezpośrednio z `object`. Wystąpienia klasy `string` reprezentują ciągi znaków w kodzie Unicode.

Wartości typu `string` mogą być zapisywane jako literały ciągu ([literały ciągu](lexical-structure.md#string-literals)).

Słowo kluczowe `string` jest po prostu aliasem wstępnie zdefiniowanej klasy `System.String`.

### <a name="interface-types"></a>Typy interfejsów

Interfejs definiuje kontrakt. Klasa lub struktura implementująca interfejs musi być zgodna z umową. Interfejs może dziedziczyć z wielu interfejsów podstawowych, a Klasa lub struktura może zaimplementować wiele interfejsów.

Typy interfejsów są opisane w [interfejsach](interfaces.md).

### <a name="array-types"></a>Typy tablic

Tablica jest strukturą danych, która zawiera zero lub więcej zmiennych, do których można uzyskać dostęp za pomocą obliczanych indeksów. Zmienne zawarte w tablicy, nazywane również elementami tablicy, są tego samego typu, a ten typ jest nazywany typem elementu tablicy.

Typy tablic są opisane w [tablicach](arrays.md).

### <a name="delegate-types"></a>Typy delegatów

Delegat jest strukturą danych, która odwołuje się do jednej lub kilku metod. Dla metod instancji odnosi się także do odpowiednich wystąpień obiektu.

Najbliższy odpowiednik delegata w C lub C++ jest wskaźnikiem funkcji, ale natomiast wskaźnik funkcji może odwoływać się tylko do funkcji statycznych, delegat może odwoływać się zarówno do metod statycznych, jak i wystąpień. W tym drugim przypadku delegat przechowuje nie tylko odwołanie do punktu wejścia metody, ale również odwołanie do wystąpienia obiektu, dla którego ma zostać wywołana metoda.

Typy delegatów są opisane w [delegatach](delegates.md).

## <a name="boxing-and-unboxing"></a>Pakowanie i rozpakowywanie

Pojęcie opakowania i rozpakowywania jest głównym C#systemem typu. Udostępnia mostek między *value_type*s i *reference_type*s, umożliwiając przekonwertowanie dowolnej wartości *value_type* na i z typu `object`. Opakowanie i rozpakowywanie umożliwia ujednolicony widok systemu typów, w którym wartość dowolnego typu może ostatecznie być traktowana jako obiekt.

### <a name="boxing-conversions"></a>Konwersje z opakowania

Konwersja z opakowania umożliwia niejawną konwersję *value_type* na *reference_type*. Istnieją następujące konwersje napakowywania:

*  Z dowolnych *value_type* do typu `object`.
*  Z dowolnych *value_type* do typu `System.ValueType`.
*  Z dowolnych *non_nullable_value_type* do dowolnych *INTERFACE_TYPE* wdrożonych przez *value_type*.
*  Z dowolnego *nullable_type* do dowolnego *INTERFACE_TYPE* zaimplementowane przez typ podstawowy *nullable_type*.
*  Z dowolnych *enum_type* do typu `System.Enum`.
*  Z dowolnego *nullable_type* z podstawowym *enum_typeem* do typu `System.Enum`.
*  Należy zauważyć, że niejawna konwersja z parametru typu zostanie wykonana jako konwersja z opakowania, jeśli w czasie wykonywania zostanie zakończona konwersja z typu wartości na typ referencyjny ([niejawne konwersje dotyczące parametrów typu](conversions.md#implicit-conversions-involving-type-parameters)).

Opakowanie wartości *non_nullable_value_type* polega na przydzieleniu wystąpienia obiektu i skopiowaniu wartości *non_nullable_value_type* do tego wystąpienia.

Opakowanie wartości elementu *nullable_type* powoduje utworzenie odwołania o wartości null, jeśli jest to wartość `null` (`HasValue` jest `false`) lub wynik rozpakowywania i napakowanie wartości źródłowej w przeciwnym razie.

Rzeczywisty proces pakowania wartości *non_nullable_value_type* jest najlepszym wyjaśnieniem przez urojoność istnienia generycznej ***klasy***opakowywania, która zachowuje się tak, jakby została zadeklarowana w następujący sposób:

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

Opakowanie wartości `v` typu `T` obejmuje teraz wykonywanie wyrażenia `new Box<T>(v)`i zwracanie wystąpienia wyniku jako wartości typu `object`. W ten sposób instrukcje
```csharp
int i = 123;
object box = i;
```
koncepcyjnie odpowiada
```csharp
int i = 123;
object box = new Box<int>(i);
```

Klasa opakowywania, taka jak `Box<T>` powyżej, nie istnieje, a dynamiczny typ wartości opakowanej nie jest w rzeczywistości typem klasy. Zamiast tego, opakowana wartość typu `T` ma `T`typu dynamicznego i sprawdzanie typu dynamicznego przy użyciu operatora `is` może po prostu odwoływać się do typu `T`. Na przykład
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
Program będzie wyprowadzał ciąg "`Box contains an int`" w konsoli.

Konwersja z opakowania oznacza tworzenie kopii wartości w ramce. Różni się to od konwersji *reference_type* na typ `object`, w którym wartość nadal odwołuje się do tego samego wystąpienia, a po prostu jest traktowana jako mniej pochodny typ `object`. Na przykład, dana deklaracja
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
następujące instrukcje
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
spowoduje wygenerowanie wartości 10 w konsoli, ponieważ niejawna operacja opakowywania, która występuje w przypisaniu `p` do `box` powoduje skopiowanie wartości `p`. `Point` został zadeklarowany `class` zamiast tego, wartość 20 byłaby wyjściowa, ponieważ `p` i `box` odwołują się do tego samego wystąpienia.

### <a name="unboxing-conversions"></a>Odpakowywanie konwersji

Konwersja rozpakowywanie umożliwia jawne przekonwertowanie *reference_type* na *value_type*. Istnieją następujące konwersje rozpakowywania:

*  Z typu `object` do dowolnych *value_type*.
*  Z typu `System.ValueType` do dowolnych *value_type*.
*  Z dowolnego *interface_typeu* do dowolnego *non_nullable_value_type* , który implementuje *INTERFACE_TYPE*.
*  Z dowolnego *INTERFACE_TYPE* do dowolnego *nullable_type* , którego typ podstawowy implementuje *INTERFACE_TYPE*.
*  Z typu `System.Enum` do dowolnych *enum_type*.
*  Z typu `System.Enum` do dowolnego *nullable_type* z podstawowym *enum_type*.
*  Należy zauważyć, że jawna konwersja parametru typu będzie wykonywana jako konwersja rozpakowywanie, jeśli w czasie wykonywania zostanie zakończona konwersja z typu referencyjnego na typ wartości ([jawne konwersje dynamiczne](conversions.md#explicit-dynamic-conversions)).

Odpakowywanie operacji do *non_nullable_value_type* polega na pierwszym sprawdzeniu, że wystąpienie obiektu jest wartością opakowaną danego *non_nullable_value_type*, a następnie kopiując wartość z wystąpienia.

Odpakowanie do *nullable_type* generuje wartość null *nullable_type* , jeśli argument źródłowy jest `null`lub zawinięty wynik odpakowania wystąpienia obiektu do typu bazowego *nullable_type* w przeciwnym razie.

Odwołujące się do klasy pakowania urojonego opisanej w poprzedniej sekcji, niepakowanie Konwersja obiektu `box` do `T` *value_type* składa się z wykonywania wyrażenia `((Box<T>)box).value`. W ten sposób instrukcje
```csharp
object box = 123;
int i = (int)box;
```
koncepcyjnie odpowiada
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

Aby konwersja rozpakowanego do danego *non_nullable_value_type* się powiodła w czasie wykonywania, wartość operandu źródła musi być odwołaniem do opakowanej wartości tego *non_nullable_value_type*. Jeśli argumentem źródłowym jest `null`, zostanie zgłoszony `System.NullReferenceException`. Jeśli operand źródłowy jest odwołaniem do niezgodnego obiektu, zostanie zgłoszony `System.InvalidCastException`.

W przypadku niepakowanej konwersji do danego *nullable_typeu* , aby pomyślnie w czasie wykonywania, wartość operandu źródła musi być `null` lub odwołanie do wartości opakowanej bazowego *non_nullable_value_type* *nullable_type*. Jeśli operand źródłowy jest odwołaniem do niezgodnego obiektu, zostanie zgłoszony `System.InvalidCastException`.

## <a name="constructed-types"></a>Typy skonstruowane

Deklaracja typu ogólnego, sama oznacza ***niezwiązany typ ogólny*** , który jest używany jako "plan" do tworzenia wielu różnych typów w drodze zastosowania ***argumentów typu***. Argumenty typu są zapisywane w nawiasach kątowych (`<` i `>`) bezpośrednio po nazwie typu ogólnego. Typ, który zawiera co najmniej jeden argument typu, nazywa się ***skonstruowanym typem***. Skonstruowany typ może być używany w większości miejsc w języku, w którym może się pojawić nazwa typu. Niepowiązanego typu ogólnego można używać tylko w obrębie elementu *typeof_expression* ([operator typeof](expressions.md#the-typeof-operator)).

Skonstruowane typy mogą być również używane w wyrażeniach jako proste nazwy ([proste nazwy](expressions.md#simple-names)) lub podczas uzyskiwania dostępu do elementu członkowskiego ([dostęp do elementu członkowskiego](expressions.md#member-access)).

Podczas szacowania *namespace_or_type_name* są brane pod uwagę tylko typy ogólne z poprawną liczbą parametrów typu. Z tego względu można użyć tego samego identyfikatora do identyfikacji różnych typów, o ile typy mają różne liczby parametrów typu. Jest to przydatne w przypadku mieszania klas ogólnych i nieogólnych w tym samym programie:

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

*TYPE_NAME* może identyfikować skonstruowany typ, chociaż nie określa bezpośrednio parametrów typu. Taka sytuacja może wystąpić, gdy typ jest zagnieżdżony w deklaracji klasy ogólnej, a typ wystąpienia deklaracji zawierającej jest niejawnie używany do wyszukiwania nazw ([typy zagnieżdżone w klasach generycznych](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

W niebezpiecznym kodzie, skonstruowany typ nie może być używany jako *unmanaged_type* ([typy wskaźników](unsafe-code.md#pointer-types)).

### <a name="type-arguments"></a>Argumenty typu

Każdy argument na liście argumentów typu jest po prostu *typem*.

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

W niebezpiecznym kodzie ([niebezpieczny kod](unsafe-code.md)) element *type_argument* nie może być typem wskaźnika. Każdy argument typu musi spełniać wszelkie ograniczenia dotyczące odpowiedniego parametru typu ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).

### <a name="open-and-closed-types"></a>Otwarte i zamknięte typy

Wszystkie typy mogą być klasyfikowane jako ***typy otwarte*** lub ***zamknięte***. Typ otwarty to typ, który obejmuje parametry typu. Dokładniej:

*  Parametr typu definiuje typ otwarty.
*  Typ tablicy jest typem otwartym, jeśli i tylko wtedy, gdy jego typ elementu jest typem otwartym.
*  Typ skonstruowany jest typem otwartym, jeśli i tylko wtedy, gdy jeden lub więcej argumentów typu jest typem otwartym. Skonstruowany zagnieżdżony typ jest typem otwartym, jeśli i tylko wtedy, gdy co najmniej jeden z jego argumentów typu lub argumenty typu zawierającego typy zawierające są typem otwartym.

Typ zamknięty to typ, który nie jest typem otwartym.

W czasie wykonywania cały kod w deklaracji typu ogólnego jest wykonywany w kontekście zamkniętego konstruowanego typu, który został utworzony przez zastosowanie argumentów typu do deklaracji ogólnej. Każdy parametr typu w ramach typu ogólnego jest powiązany z określonym typem czasu wykonywania. Przetwarzanie wszystkich instrukcji i wyrażeń w czasie wykonywania zawsze odbywa się z typami zamkniętymi, a typy otwarte są wykonywane tylko podczas przetwarzania w czasie kompilacji.

Każdy zamknięty typ skonstruowany ma własny zestaw zmiennych statycznych, które nie są udostępniane żadnym innym zamkniętym skonstruowanym typem. Ponieważ otwarty typ nie istnieje w czasie wykonywania, nie istnieją zmienne statyczne skojarzone z typem otwartym. Dwa zamknięte typy skonstruowane są tego samego typu, jeśli są zbudowane z tego samego niepowiązanego typu ogólnego i odpowiadające im argumenty typu są tego samego typu.

### <a name="bound-and-unbound-types"></a>Powiązane i niepowiązane typy

***Typ niepowiązany*** nie odwołuje się do typu nieogólnego lub niepowiązanego typu ogólnego. ***Typ powiązany*** z terminem odwołuje się do typu nieogólnego lub typu złożonego.

Typ niepowiązany odwołuje się do jednostki zadeklarowanej przez deklarację typu. Niezwiązany typ ogólny nie jest samym typem i nie można go używać jako typu zmiennej, argumentu lub wartości zwracanej lub jako typu podstawowego. Jedyną konstrukcją, w której można odwołać się do niepowiązanego typu ogólnego, jest wyrażenie `typeof` ([operator typeof](expressions.md#the-typeof-operator)).

### <a name="satisfying-constraints"></a>Spełnianie ograniczeń

Za każdym razem, gdy jest przywoływany typ skonstruowany lub metoda ogólna, dostarczone argumenty typu są sprawdzane względem ograniczeń parametrów typu zadeklarowanych w typie ogólnym lub metodzie ([ograniczenia parametru typu](classes.md#type-parameter-constraints)). Dla każdej klauzuli `where` argument typu `A`, który odnosi się do nazwanego parametru typu, jest sprawdzany względem każdego ograniczenia w następujący sposób:

*  Jeśli ograniczenie jest typem klasy, typem interfejsu lub parametrem typu, niech `C` reprezentuje to ograniczenie z podanymi argumentami typu podstawiane dla wszystkich parametrów typu, które pojawiają się w ograniczeniu. Aby spełnić ograniczenie, musi to być przypadek, w którym typ `A` jest konwertowany na typ `C` przez jedną z następujących wartości:
    * Konwersja tożsamości ([konwersja tożsamości](conversions.md#identity-conversion))
    * Niejawna konwersja odwołania ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions))
    * Konwersja z opakowaniem ([konwersje pakujące](conversions.md#boxing-conversions)), pod warunkiem, że typ A jest typem wartości niedopuszczający wartości null.
    * Niejawne odwołanie, opakowanie lub konwersja parametrów typu z parametru typu `A`, aby `C`.
*  Jeśli ograniczenie jest ograniczeniem typu odwołania (`class`), typ `A` musi spełniać jedną z następujących wartości:
    * `A` to typ interfejsu, typ klasy, typ delegata lub typ tablicy. Należy pamiętać, że `System.ValueType` i `System.Enum` są typami odwołań, które spełniają to ograniczenie.
    * `A` jest parametrem typu, który jest znany jako typ referencyjny ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).
*  Jeśli ograniczenie jest ograniczeniem typu wartości (`struct`), typ `A` musi spełniać jedną z następujących czynności:
    * `A` jest typem struktury lub typem wyliczeniowym, ale nie typem dopuszczającym wartość null. Należy zauważyć, że `System.ValueType` i `System.Enum` są typami odwołań, które nie spełniają tego ograniczenia.
    * `A` jest parametrem typu mającym ograniczenie typu wartości ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).
*  Jeśli ograniczenie jest ograniczeniem konstruktora `new()`, `A` typu nie może być `abstract` i musi mieć publicznego konstruktora bez parametrów. Jest to spełnione, jeśli jest spełniony jeden z następujących warunków:
    * `A` jest typem wartości, ponieważ wszystkie typy wartości mają publiczny Konstruktor domyślny ([konstruktory domyślne](types.md#default-constructors)).
    * `A` jest parametrem typu mającym ograniczenie konstruktora ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).
    * `A` jest parametrem typu mającym ograniczenie typu wartości ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).
    * `A` jest klasą, która nie jest `abstract` i zawiera jawnie zadeklarowany Konstruktor `public` bez parametrów.
    * `A` nie `abstract` i ma domyślnego konstruktora ([konstruktorów domyślnych](classes.md#default-constructors)).

Błąd czasu kompilacji występuje, jeśli co najmniej jedno ograniczenie parametru typu nie jest spełnione przez podaną liczbę argumentów typu.

Ponieważ parametry typu nie są dziedziczone, ograniczenia nigdy nie są dziedziczone. W poniższym przykładzie `D` musi określić ograniczenie dla parametru typu `T`, aby `T` spełniała ograniczenie narzucone przez klasę bazową `B<T>`. W przeciwieństwie do klasy `E` nie trzeba określać ograniczenia, ponieważ `List<T>` implementuje `IEnumerable` dla wszystkich `T`.

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>Parametry typu

Parametr typu jest identyfikatorem określającym typ wartości lub typ referencyjny, z którym jest powiązany parametr w czasie wykonywania.

```antlr
type_parameter
    : identifier
    ;
```

Ponieważ parametr typu można utworzyć przy użyciu wielu różnych rzeczywistych argumentów typu, parametry typu mają nieco różne operacje i ograniczenia niż inne typy. Należą do nich następujące elementy:

*  Parametru typu nie można używać bezpośrednio do deklarowania klasy bazowej ([Klasa bazowa](classes.md#base-class)) lub interfejsu ([listy parametrów typu wariantu](interfaces.md#variant-type-parameter-lists)).
*  Reguły przeszukiwania elementów członkowskich w parametrach typu są zależne od ograniczeń (jeśli istnieją) stosowanych do parametru typu. Są one szczegółowe w [przeszukiwaniu elementów członkowskich](expressions.md#member-lookup).
*  Dostępne konwersje dla parametru typu są zależne od ograniczeń (jeśli istnieją) stosowanych do parametru typu. Są one szczegółowe w przypadku [niejawnych konwersji obejmujących parametry typu](conversions.md#implicit-conversions-involving-type-parameters) i [jawne konwersje dynamiczne](conversions.md#explicit-dynamic-conversions).
*  Literał `null` nie może zostać skonwertowany na typ określony przez parametr typu, z wyjątkiem tego, że parametr typu jest znany jako typ referencyjny ([niejawne konwersje dotyczące parametrów typu](conversions.md#implicit-conversions-involving-type-parameters)). Jednak zamiast tego można użyć wyrażenia `default` ([wyrażenie wartości domyślnej](expressions.md#default-value-expressions)). Dodatkowo można porównać wartość z typem przyznanym parametrem typu z `null` przy użyciu `==` i `!=` ([Operatory równości typu referencyjnego](expressions.md#reference-type-equality-operators)), chyba że parametr typu ma ograniczenie typu wartości.
*  Wyrażenia `new` ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)) można używać tylko z parametrem typu, jeśli parametr type jest ograniczony przez *constructor_constraint* lub ograniczenie typu wartości ([ograniczenia parametru typu](classes.md#type-parameter-constraints)).
*  Parametru typu nie można używać w dowolnym miejscu w atrybucie.
*  Parametru typu nie można użyć w dostępie do elementu członkowskiego ([dostęp do elementu członkowskiego](expressions.md#member-access)) lub nazwy typu ([przestrzeni nazw i typów](basic-concepts.md#namespace-and-type-names)) do identyfikacji statycznej składowej lub typu zagnieżdżonego.
*  W niebezpiecznym kodzie parametr typu nie może być używany jako *unmanaged_type* ([typy wskaźników](unsafe-code.md#pointer-types)).

Jako typ, parametry typu są czystym konstrukcjami czasu kompilacji. W czasie wykonywania każdy parametr typu jest powiązany z typem czasu wykonywania, który został określony przez dostarczenie argumentu typu do deklaracji typu ogólnego. W takim przypadku typ zmiennej zadeklarowanej za pomocą parametru typu w czasie wykonywania będzie zamkniętym typem skonstruowanym ([otwarte i zamknięte](types.md#open-and-closed-types)). Wykonywanie wszystkich instrukcji i wyrażeń obejmujących parametry typu w czasie wykonywania przy użyciu rzeczywistego typu, który został dostarczony jako argument typu dla tego parametru.

## <a name="expression-tree-types"></a>Typy drzewa wyrażeń

***Drzewa wyrażeń umożliwiają wyrażenie*** lambda, które mają być reprezentowane jako struktury danych zamiast kodu wykonywalnego. Drzewa wyrażeń to wartości ***typów drzewa wyrażeń*** `System.Linq.Expressions.Expression<D>`, gdzie `D` jest dowolnym typem delegata. W pozostałej części tej specyfikacji będziemy odwoływać się do tych typów przy użyciu skróconej `Expression<D>`.

Jeśli konwersja istnieje z wyrażenia lambda do typu delegata `D`, konwersja istnieje również do typu drzewa wyrażenia `Expression<D>`. Natomiast konwersja wyrażenia lambda na typ delegata generuje delegata, który odwołuje się do kodu wykonywalnego wyrażenia lambda, konwersja na typ drzewa wyrażenia tworzy reprezentację drzewa wyrażenia wyrażenia lambda.

Drzewa wyrażeń są wydajnymi reprezentacjami danych w pamięci wyrażeń lambda i tworzą strukturę wyrażenia lambda przezroczystego i jawnego.

Podobnie jak typ delegata `D`, `Expression<D>` jest określany jako parametry i zwracane typy, które są takie same jak w przypadku `D`.

Poniższy przykład przedstawia wyrażenie lambda zarówno jako kod wykonywalny, jak i drzewo wyrażenia. Ponieważ istnieje konwersja do `Func<int,int>`, konwersja istnieje również do `Expression<Func<int,int>>`:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

Następujące przypisania `del` delegata odwołuje się do metody, która zwraca `x + 1`, a drzewo wyrażenia `exp` odwołuje się do struktury danych opisującej `x => x + 1`wyrażenia.

Dokładna definicja typu ogólnego `Expression<D>` jak również precyzyjne reguły konstruowania drzewa wyrażeń, gdy wyrażenie lambda jest konwertowane na typ drzewa wyrażenia, są poza zakresem tej specyfikacji.

Ważne jest, aby zapewnić jawne:

*  Nie wszystkie wyrażenia lambda mogą być konwertowane na drzewa wyrażeń. Na przykład wyrażenia lambda z treścią instrukcji i wyrażenia lambda zawierające wyrażenia przypisania nie mogą być reprezentowane. W takich przypadkach konwersja nadal istnieje, ale zakończy się niepowodzeniem w czasie kompilacji. Te wyjątki są szczegółowo opisane w [konwersji funkcji anonimowych](conversions.md#anonymous-function-conversions).
*   `Expression<D>` oferuje metodę wystąpienia `Compile`, która tworzy delegata typu `D`:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    Wywołanie tego delegata powoduje wykonanie kodu reprezentowanego przez drzewo wyrażenia. W efekcie, zgodnie z powyższymi definicjami, del i DEL2 są równoważne, a poniższe dwie instrukcje będą miały ten sam skutek:

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    Po wykonaniu tego kodu, `i1` i `i2` będą mieć wartość `2`.

