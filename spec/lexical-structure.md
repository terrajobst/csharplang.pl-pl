---
ms.openlocfilehash: 5fbe0267b5b33b1a24dbdca493d118c576092573
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876913"
---
# <a name="lexical-structure"></a>Struktura leksykalna

## <a name="programs"></a>Programy

C# ***Program*** składa się z co najmniej jednego ***pliku źródłowego***, znanego formalnie jako ***jednostki kompilacji*** ([jednostki kompilacji](namespaces.md#compilation-units)). Plik źródłowy jest uporządkowaną sekwencją znaków Unicode. Pliki źródłowe zwykle mają zgodność jeden-do-jednego z plikami w systemie plików, ale ta korespondencja nie jest wymagana. W celu uzyskania maksymalnej przenośności zaleca się, aby pliki w systemie plików były kodowane przy użyciu kodowania UTF-8.

Koncepcyjnie mówiąc, program jest kompilowany przy użyciu trzech kroków:

1. Transformacja, która konwertuje plik z określonego znaku repertuar i schematu kodowania na sekwencję znaków Unicode.
2. Analiza leksykalna, która tłumaczy strumień znaków wejściowych Unicode na strumień tokenów.
3. Analiza składni, która tłumaczy strumień tokenów na kod wykonywalny.

## <a name="grammars"></a>Gramatykach

Ta specyfikacja przedstawia składnię języka C# programowania przy użyciu dwóch gramatyki. ***Gramatyka leksykalna*** ([Gramatyka leksykalna](lexical-structure.md#lexical-grammar)) definiuje, w jaki sposób znaki Unicode są łączone z terminatorami wierszy formularza, białym znakiem, komentarzem, tokenami i dyrektywami wstępnego przetwarzania. ***Gramatyka składni*** ([Gramatyka składni](lexical-structure.md#syntactic-grammar)) definiuje, jak tokeny pochodzące z gramatyki leksykalnej są C# łączone z programami formularzy.

### <a name="grammar-notation"></a>Notacja gramatyki

Gramatyki leksykalne i syntaktyczne są prezentowane w formularzu back-Naura za pomocą notacji narzędzia gramatyki ANTLR.

### <a name="lexical-grammar"></a>Gramatyka leksykalna

Gramatyka leksykalna C# jest przedstawiana w ramach [analizy leksykalnej](lexical-structure.md#lexical-analysis), [tokenów](lexical-structure.md#tokens)i [dyrektyw wstępnego przetwarzania](lexical-structure.md#pre-processing-directives). Symbole terminalu leksykalnej gramatycznej są znakami zestawu znaków Unicode, a Gramatyka leksykalna określa, jak znaki są łączone z tokenami formularza ([tokenami](lexical-structure.md#tokens)), białym znakiem ([białym znakiem](lexical-structure.md#white-space)), komentarzami ([komentarzami](lexical-structure.md#comments)) i dyrektywy wstępnego przetwarzania ([dyrektywy wstępnego przetwarzania](lexical-structure.md#pre-processing-directives)).

Każdy plik źródłowy w C# programie musi być zgodny z produkcją *wejściową* gramatyki leksykalnej ([Analiza leksykalna](lexical-structure.md#lexical-analysis)).

### <a name="syntactic-grammar"></a>Gramatyka składni

Składnia gramatyki C# jest przedstawiona w działach i dodatkach, które obserwują ten rozdział. Symbole terminalu składni gramatycznej są tokenami zdefiniowanymi przez gramatykę leksykalną, a Gramatyka składni określa, jak tokeny są łączone C# z programami formularzy.

Każdy plik źródłowy w C# programie musi być zgodny z produkcją *compilation_unitą* gramatyki składniowej ([jednostki kompilacji](namespaces.md#compilation-units)).

## <a name="lexical-analysis"></a>Analiza leksykalna

Produkcja *wejściowa* definiuje strukturę leksykalną pliku C# źródłowego. Każdy plik źródłowy w C# programie musi być zgodny z tą leksykalną produkcją gramatyki.

```antlr
input
    : input_section?
    ;

input_section
    : input_section_part+
    ;

input_section_part
    : input_element* new_line
    | pp_directive
    ;

input_element
    : whitespace
    | comment
    | token
    ;
```

Pięć podstawowych elementów tworzą strukturę leksykalną pliku C# źródłowego: Terminatory wiersza ([terminatory wierszy](lexical-structure.md#line-terminators)), biały znak ([biały znak](lexical-structure.md#white-space)), Komentarze ([Komentarze](lexical-structure.md#comments)), tokeny ([tokeny](lexical-structure.md#tokens)) i dyrektywy wstępnego przetwarzania ([dyrektywy wstępnego przetwarzania](lexical-structure.md#pre-processing-directives)). W przypadku tych podstawowych elementów tylko tokeny są znaczące w gramatyce składni C# programu ([gramatyki](lexical-structure.md#syntactic-grammar)składniowej).

Przetwarzanie leksykalne pliku C# źródłowego składa się z zmniejszenia pliku do sekwencji tokenów, które staną się danymi wejściowymi analizy składni. Terminatory wierszy, biały znak i komentarze mogą służyć do oddzielania tokenów, a dyrektywy wstępnego przetwarzania mogą spowodować pominięcie sekcji w pliku źródłowym, ale w przeciwnym razie te elementy leksykalne nie mają wpływu na strukturę składni C# programu.

W przypadku interpolowanych literałów ciągu ([interpolowane literały ciągu](lexical-structure.md#interpolated-string-literals)) pojedynczy token jest początkowo tworzony przez analizę leksykalną, ale jest podzielony na kilka elementów wejściowych, które są wielokrotnie poddawane analizie leksykalnej do momentu, gdy wszystkie interpolowane Literały ciągu zostały rozwiązane. Otrzymane tokeny następnie stanowią dane wejściowe do analizy składni.

Gdy kilka leksykalnych produkcji gramatyki pasuje do sekwencji znaków w pliku źródłowym, przetwarzanie leksykalne zawsze tworzy najdłuższy możliwy element leksykalny. Na przykład sekwencja `//` znaków jest przetwarzana jako początek jednowierszowego komentarza, ponieważ ten element leksykalny jest dłuższy niż pojedynczy `/` token.

### <a name="line-terminators"></a>Terminatory wiersza

Terminatory wierszy dzielą znaki pliku C# źródłowego na wiersze.

```antlr
new_line
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Carriage return character (U+000D) followed by line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;
```

W celu zapewnienia zgodności z narzędziami do edycji kodu źródłowego, które dodają znaczniki końca pliku i umożliwiają wyświetlanie pliku źródłowego jako sekwencji prawidłowo zakończonych wierszy, do każdego pliku źródłowego w C# programie są stosowane następujące przekształcenia:

*  Jeśli ostatni znak pliku źródłowego jest znakiem Control-Z (`U+001A`), ten znak jest usuwany.
*  Znak powrotu karetki (`U+000D`) jest dodawany na końcu pliku źródłowego, jeśli ten plik źródłowy nie jest pusty i jeśli ostatni znak w pliku źródłowym nie jest Return karetki (`U+000D`)`U+000A`, wierszem wysuwu wiersza (), separatorem wiersza (`U+2028`) lub separator akapitu (`U+2029`).

### <a name="comments"></a>Komentarze

Obsługiwane są dwie formy komentarzy: Komentarze jednowierszowe i rozdzielane Komentarze. ***Komentarze jednowierszowe*** zaczynają się od `//` znaków i zwiększają się do końca wiersza źródłowego. ***Rozdzielane Komentarze*** zaczynają się od `/*` znaków i kończą się znakami. `*/` Rozdzielane komentarze mogą obejmować wiele wierszy.

```antlr
comment
    : single_line_comment
    | delimited_comment
    ;

single_line_comment
    : '//' input_character*
    ;

input_character
    : '<Any Unicode character except a new_line_character>'
    ;

new_line_character
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;

delimited_comment
    : '/*' delimited_comment_section* asterisk+ '/'
    ;

delimited_comment_section
    : '/'
    | asterisk* not_slash_or_asterisk
    ;

asterisk
    : '*'
    ;

not_slash_or_asterisk
    : '<Any Unicode character except / or *>'
    ;
```

Komentarze nie są zagnieżdżane. Sekwencje `/*` znaków i `*/` `//` `/*` nie mają specjalnego znaczenia w komentarzuisekwencjeznakówiniemająspecjalnychznaczeniawkomentarzuograniczonym.`//`

Komentarze nie są przetwarzane w literałach znaków i ciągów.

Przykład
```csharp
/* Hello, world program
   This program writes "hello, world" to the console
*/
class Hello
{
    static void Main() {
        System.Console.WriteLine("hello, world");
    }
}
```
zawiera rozdzielany komentarz.

Przykład
```csharp
// Hello, world program
// This program writes "hello, world" to the console
//
class Hello // any name will do for this class
{
    static void Main() { // this method must be named "Main"
        System.Console.WriteLine("hello, world");
    }
}
```
pokazuje kilka komentarzy jednowierszowych.

### <a name="white-space"></a>Biały znak

Spacja jest definiowana jako dowolny znak z klasą Unicode ZS (która zawiera znak spacji), a także znak tabulacji poziomej, znak tabulacji pionowej i znak wysuwu strony.

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>Tokeny

Istnieje kilka rodzajów tokenów: identyfikatory, słowa kluczowe, literały, operatory i przerywniki. Biały znak i komentarze nie są tokenami, chociaż działają jako separatory dla tokenów.

```antlr
token
    : identifier
    | keyword
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | interpolated_string_literal
    | operator_or_punctuator
    ;
```

### <a name="unicode-character-escape-sequences"></a>Sekwencje ucieczki znaków Unicode

Sekwencja ucieczki znaków Unicode reprezentuje znak Unicode. Sekwencje ucieczki znaków Unicode są przetwarzane w identyfikatorach ([identyfikatorach](lexical-structure.md#identifiers)), literałach znaków ([literałach znaków](lexical-structure.md#character-literals)) i zwykłych literałach ciągów ([literały ciągów](lexical-structure.md#string-literals)). Znak ucieczki Unicode nie jest przetwarzany w żadnej innej lokalizacji (na przykład w celu utworzenia operatora, punctuator lub słowa kluczowego).

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Sekwencja unikowa Unicode reprezentuje pojedynczy znak Unicode utworzony przez liczbę szesnastkową po znakach "`\u`" lub "`\U`". Ponieważ C# program korzysta z 16-bitowego kodowania punktów kodowych Unicode w znakach i wartościach ciągu, znak Unicode w zakresie u + 10000 do u + 10FFFF jest niedozwolony w literale znakowym i jest reprezentowany przy użyciu pary wieloskładnikowej Unicode w literale ciągu. Znaki Unicode z punktami kodu powyżej 0x10FFFF nie są obsługiwane.

Nie wykonano wielu tłumaczeń. Na przykład literał ciągu "`\u005Cu005C`" jest odpowiednikiem "`\u005C`" zamiast "`\`". Wartość `\u005C` Unicode jest znakiem "`\`".

Przykład
```csharp
class Class1
{
    static void Test(bool \u0066) {
        char c = '\u0066';
        if (\u0066)
            System.Console.WriteLine(c.ToString());
    }        
}
```
pokazuje kilka zastosowania `\u0066`, czyli sekwencję ucieczki dla litery "`f`". Program jest równoważny
```csharp
class Class1
{
    static void Test(bool f) {
        char c = 'f';
        if (f)
            System.Console.WriteLine(c.ToString());
    }        
}
```

### <a name="identifiers"></a>Identyfikatory

Reguły dotyczące identyfikatorów podane w tej sekcji odpowiadają dokładnie wymaganiom zalecanym przez standard Unicode załącznika 31, z wyjątkiem tego, że podkreślenie jest dozwolone jako znak początkowy (jak tradycyjna w języku programowania C), sekwencje unikowe Unicode są dozwolone w identyfikatorach, a znak`@`"" jest dozwolony jako prefiks, aby można było używać słów kluczowych jako identyfikatorów.

```antlr
identifier
    : available_identifier
    | '@' identifier_or_keyword
    ;

available_identifier
    : '<An identifier_or_keyword that is not a keyword>'
    ;

identifier_or_keyword
    : identifier_start_character identifier_part_character*
    ;

identifier_start_character
    : letter_character
    | '_'
    ;

identifier_part_character
    : letter_character
    | decimal_digit_character
    | connecting_character
    | combining_character
    | formatting_character
    ;

letter_character
    : '<A Unicode character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    | '<A unicode_escape_sequence representing a character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    ;

combining_character
    : '<A Unicode character of classes Mn or Mc>'
    | '<A unicode_escape_sequence representing a character of classes Mn or Mc>'
    ;

decimal_digit_character
    : '<A Unicode character of the class Nd>'
    | '<A unicode_escape_sequence representing a character of the class Nd>'
    ;

connecting_character
    : '<A Unicode character of the class Pc>'
    | '<A unicode_escape_sequence representing a character of the class Pc>'
    ;

formatting_character
    : '<A Unicode character of the class Cf>'
    | '<A unicode_escape_sequence representing a character of the class Cf>'
    ;
```

Aby uzyskać informacje na temat klas znaków Unicode wymienionych powyżej, zobacz Standard Unicode w wersji 3,0, sekcja 4,5.

Przykłady prawidłowych identyfikatorów to "`identifier1`", "`_identifier2`" i "`@if`".

Identyfikator w programie zgodnym musi znajdować się w formacie kanonicznym zdefiniowanym przez normalizację Unicode w postaci C, zgodnie z definicją standardu Unicode w załączniku 15. Zachowanie podczas napotkania identyfikatora, którego nie ma w postaci normalizacji C, jest zdefiniowane w implementacji; jednak Diagnostyka nie jest wymagana.

Prefiks "`@`" umożliwia używanie słów kluczowych jako identyfikatorów, co jest przydatne w przypadku współdziałania z innymi językami programowania. Znak `@` nie jest w rzeczywistości częścią identyfikatora, więc identyfikator może być widziany w innych językach jako normalny identyfikator, bez prefiksu. Identyfikator z `@` prefiksem nazywa się ***identyfikatorem Verbatim***. `@` Używanie prefiksu dla identyfikatorów, które nie są słowami kluczowymi jest dozwolone, ale zdecydowanie odradza się jako kwestia stylu.

Przykład:
```csharp
class @class
{
    public static void @static(bool @bool) {
        if (@bool)
            System.Console.WriteLine("true");
        else
            System.Console.WriteLine("false");
    }    
}

class Class1
{
    static void M() {
        cl\u0061ss.st\u0061tic(true);
    }
}
```
definiuje klasę o nazwie "`class`" z metodą statyczną o nazwie`static`"", która przyjmuje parametr o`bool`nazwie "". Należy zauważyć, że ponieważ wyrażenia ucieczki Unicode nie są dozwolone w słowach`cl\u0061ss`kluczowych, token "" jest identyfikatorem i jest tym samym`@class`identyfikatorem jak "".

Dwa identyfikatory są uważane za takie same, jeśli są identyczne po zastosowaniu następujących przekształceń:

*  Prefiks "`@`", jeśli jest używany, jest usuwany.
*  Każdy *unicode_escape_sequence* jest przekształcany do odpowiedniego znaku Unicode.
*  Wszystkie *formatting_character*s zostaną usunięte.

Identyfikatory zawierające dwa kolejne znaki podkreślenia (`U+005F`) są zarezerwowane do użytku przez implementację. Na przykład implementacja może zapewnić rozszerzone słowa kluczowe, które zaczynają się od dwóch znaków podkreślenia.

### <a name="keywords"></a>słowa kluczowe

***Słowo kluczowe*** to sekwencja znaków, która jest zastrzeżona i nie może być używana jako identyfikator, z wyjątkiem sytuacji, gdy jest `@` poprzedzona znakiem.

```antlr
keyword
    : 'abstract' | 'as'       | 'base'       | 'bool'      | 'break'
    | 'byte'     | 'case'     | 'catch'      | 'char'      | 'checked'
    | 'class'    | 'const'    | 'continue'   | 'decimal'   | 'default'
    | 'delegate' | 'do'       | 'double'     | 'else'      | 'enum'
    | 'event'    | 'explicit' | 'extern'     | 'false'     | 'finally'
    | 'fixed'    | 'float'    | 'for'        | 'foreach'   | 'goto'
    | 'if'       | 'implicit' | 'in'         | 'int'       | 'interface'
    | 'internal' | 'is'       | 'lock'       | 'long'      | 'namespace'
    | 'new'      | 'null'     | 'object'     | 'operator'  | 'out'
    | 'override' | 'params'   | 'private'    | 'protected' | 'public'
    | 'readonly' | 'ref'      | 'return'     | 'sbyte'     | 'sealed'
    | 'short'    | 'sizeof'   | 'stackalloc' | 'static'    | 'string'
    | 'struct'   | 'switch'   | 'this'       | 'throw'     | 'true'
    | 'try'      | 'typeof'   | 'uint'       | 'ulong'     | 'unchecked'
    | 'unsafe'   | 'ushort'   | 'using'      | 'virtual'   | 'void'
    | 'volatile' | 'while'
    ;
```

W niektórych miejscach gramatycznych określone identyfikatory mają specjalne znaczenie, ale nie są słowami kluczowymi. Takie identyfikatory są czasami nazywane "kontekstowymi słowami kluczowymi". Na przykład w deklaracji właściwości identyfikatory "`get`" i "`set`" mają specjalne znaczenie (metody[dostępu](classes.md#accessors)). Identyfikator inny niż `get` lub `set` nigdy nie jest dozwolony w tych lokalizacjach, dlatego to użycie nie powoduje konfliktu z użyciem tych słów jako identyfikatorów. W innych przypadkach, na przykład z identyfikatorem "`var`" w deklaracjach zmiennych lokalnych niejawnie wpisanych ([lokalnych deklaracji zmiennych](statements.md#local-variable-declarations)), kontekstowe słowo kluczowe może powodować konflikt z zadeklarowanymi nazwami. W takich przypadkach zadeklarowana nazwa ma pierwszeństwo przed użyciem identyfikatora jako kontekstowego słowa kluczowego.

### <a name="literals"></a>Literały

***Literał*** jest reprezentacją kodu źródłowego wartości.

```antlr
literal
    : boolean_literal
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | null_literal
    ;
```

#### <a name="boolean-literals"></a>Literały logiczne

Istnieją dwie wartości literału logicznego `true` : `false`i.

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

Typem elementu *boolean_literal* jest `bool`.

#### <a name="integer-literals"></a>Literały całkowite

Literały całkowite są używane do zapisywania wartości `int`typów, `uint`, `long`i `ulong`. Literały całkowite mają dwie możliwe formy: dziesiętną i szesnastkową.

```antlr
integer_literal
    : decimal_integer_literal
    | hexadecimal_integer_literal
    ;

decimal_integer_literal
    : decimal_digit+ integer_type_suffix?
    ;

decimal_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

integer_type_suffix
    : 'U' | 'u' | 'L' | 'l' | 'UL' | 'Ul' | 'uL' | 'ul' | 'LU' | 'Lu' | 'lU' | 'lu'
    ;

hexadecimal_integer_literal
    : '0x' hex_digit+ integer_type_suffix?
    | '0X' hex_digit+ integer_type_suffix?
    ;

hex_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F' | 'a' | 'b' | 'c' | 'd' | 'e' | 'f';
```

Typ literału liczby całkowitej jest określany w następujący sposób:

*  Jeśli literał nie ma sufiksu, ma pierwszy z tych typów, w którym można przedstawić `int`jego wartość:, `uint`, `long`, `ulong`.
*  Jeśli literał `U` jest poprzedzony przez lub `u`, ma pierwszy z tych typów, w których można reprezentować wartość: `uint`, `ulong`.
*  Jeśli literał `L` jest poprzedzony przez lub `l`, ma pierwszy z tych typów, w których można reprezentować wartość: `long`, `ulong`.
*  Jeśli literał jest `UL`sufiksem, `LU` `ul` `uL` `Ul` ,,`Lu`,,, ,lub`lu`, jest typem `ulong`. `lU`

Jeśli wartość reprezentowana przez literał liczby całkowitej znajduje się poza zakresem `ulong` typu, wystąpi błąd w czasie kompilacji.

Ze względu na styl`L`zaleca się użycie "" zamiast "`l`" podczas pisania literałów typu `long`, ponieważ można łatwo pomylić literę`l`"" z cyfrą "`1`".

Aby zezwolić na zapisanie `int` najmniejszych możliwych i `long` wartości w postaci dziesiętnych literałów liczb całkowitych, istnieją następujące dwie reguły:

* Gdy *decimal_integer_literal* o wartości 2147483648 (2 ^ 31) i bez *integer_type_suffix* pojawia się jako token bezpośrednio po jednoargumentowym tokenie operatora minus ([jednoargumentowy operator minus](expressions.md#unary-minus-operator)), wynik jest stałą typu `int`wartość-2147483648 (-2 ^ 31). We wszystkich innych sytuacjach takie *decimal_integer_literal* jest typu `uint`.
* Gdy *decimal_integer_literal* o wartości zakresu od (2 ^ 63) i bez *integer_type_suffix* lub *integer_type_suffix* `L` lub `l` pojawia się jako token bezpośrednio po jednoargumentowym znaku minus token operatora ([jednoargumentowy operator minus](expressions.md#unary-minus-operator)), wynik jest stałą typu `long` z wartością-zakresu od (-2 ^ 63). We wszystkich innych sytuacjach takie *decimal_integer_literal* jest typu `ulong`.

#### <a name="real-literals"></a>Literały prawdziwe

Literały prawdziwe są używane do zapisywania wartości typów `float`, `double`i `decimal`.

```antlr
real_literal
    : decimal_digit+ '.' decimal_digit+ exponent_part? real_type_suffix?
    | '.' decimal_digit+ exponent_part? real_type_suffix?
    | decimal_digit+ exponent_part real_type_suffix?
    | decimal_digit+ real_type_suffix
    ;

exponent_part
    : 'e' sign? decimal_digit+
    | 'E' sign? decimal_digit+
    ;

sign
    : '+'
    | '-'
    ;

real_type_suffix
    : 'F' | 'f' | 'D' | 'd' | 'M' | 'm'
    ;
```

Jeśli *real_type_suffix* nie jest określony, typem literału rzeczywistego jest `double`. W przeciwnym razie sufiks typu rzeczywistego określa typ literału rzeczywistego w następujący sposób:

*  Literał prawdziwy sufiksu `F` lub `f` jest typu `float`. Na przykład `1f`literały, `1e10f` `1.5f`,, i `123.456F` są wszystkie typu `float`.
*  Literał prawdziwy sufiksu `D` lub `d` jest typu `double`. Na przykład `1d`literały, `1e10d` `1.5d`,, i `123.456D` są wszystkie typu `double`.
*  Literał prawdziwy sufiksu `M` lub `m` jest typu `decimal`. Na przykład `1m`literały, `1e10m` `1.5m`,, i `123.456M` są wszystkie typu `decimal`. Ten literał jest konwertowany na `decimal` wartość przez pobranie dokładnej wartości i, w razie potrzeby, zaokrąglenie do najbliższej wartości, którą można przedstawić, przy użyciu zaokrąglenia banku ([Typ dziesiętny](types.md#the-decimal-type)). Każda Skala widoczna w literale jest zachowywana, chyba że wartość jest zaokrąglana lub wartość jest równa zero (w którym ostatnim przypadku znak i Skala będzie równa 0). W związku z tym `2.900m` literał zostanie przeanalizowany w celu utworzenia wartości dziesiętnej `0`ze znakiem, współczynnikiem `2900`i skalą `3`.

Jeśli określony literał nie może być reprezentowany w wskazanym typie, wystąpi błąd w czasie kompilacji.

Wartość rzeczywistego literału typu `float` lub `double` jest określana za pomocą typu "okrągłe do najbliższe".

Należy pamiętać, że w literale rzeczywistym cyfry dziesiętne są zawsze wymagane po przecinku dziesiętnym. Na przykład jest `1.3F` słowem rzeczywistym, ale `1.F` nie jest.

#### <a name="character-literals"></a>Literały znaków

Literał znakowy reprezentuje pojedynczy znak i zwykle składa się z znaku w cudzysłowie, jak w `'a'`.

Uwaga: Notacja gramatyki ANTLR sprawia, że jest to mylące. W antlr, gdy piszesz `\'` , oznacza pojedynczy cudzysłów. `'` A gdy piszesz `\\` , oznacza pojedynczy ukośnik odwrotny `\`. W związku z tym pierwsza reguła dla literału znakowego oznacza, że rozpoczyna się od pojedynczego cudzysłowu, a następnie pojedynczego cudzysłowu. I jedenaście możliwych prostych sekwencji ucieczki to `\'` `\\`, `\"`,, `\0` `\b` ,,`\t`, `\n` ,`\r`,,, `\f` `\a` `\v`.

```antlr
character_literal
    : '\'' character '\''
    ;

character
    : single_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_character
    : '<Any character except \' (U+0027), \\ (U+005C), and new_line_character>'
    ;

simple_escape_sequence
    : '\\\'' | '\\"' | '\\\\' | '\\0' | '\\a' | '\\b' | '\\f' | '\\n' | '\\r' | '\\t' | '\\v'
    ;

hexadecimal_escape_sequence
    : '\\x' hex_digit hex_digit? hex_digit? hex_digit?;
```

Znak`\`, który następuje po znaku ukośnika odwrotnego () w *znaku* musi być jednym z następujących znaków `'` `0`:, `"` `b`, `\` `a`,,,, `f` , `n`, `r`, `t`, `u`, `U`, `x`, `v`. W przeciwnym razie wystąpi błąd w czasie kompilacji.

Szesnastkowa sekwencja ucieczki reprezentuje pojedynczy znak Unicode, z wartością utworzoną przez liczbę szesnastkową następującej po znaku`\x`"".

Jeśli wartość reprezentowana przez literał znakowy jest większa niż `U+FFFF`, wystąpi błąd w czasie kompilacji.

Sekwencja ucieczki znaków Unicode ([Sekwencje ucieczki znaków Unicode](lexical-structure.md#unicode-character-escape-sequences)) w literale znakowym musi należeć `U+FFFF`do zakresu `U+0000` od do.

Prosta sekwencja ucieczki reprezentuje kodowanie znaków Unicode, zgodnie z opisem w poniższej tabeli.


| __Sekwencja unikowa__ | __Nazwa znaku__ | __Kodowanie Unicode__ |
|---------------------|--------------------|----------------------|
| `\'`                | Pojedynczy cytat       | `0x0027`             | 
| `\"`                | Podwójny cudzysłów       | `0x0022`             | 
| `\\`| Ukośnik odwrotny |`0x005C`             | 
| `\0`                | Null               | `0x0000`             | 
| `\a`                | Alerty              | `0x0007`             | 
| `\b`                | Backspace          | `0x0008`             | 
| `\f`                | Kanał informacyjny formularza          | `0x000C`             | 
| `\n`                | Nowy wiersz           | `0x000A`             | 
| `\r`                | Znak powrotu karetki    | `0x000D`             | 
| `\t`                | Tabulator poziomy     | `0x0009`             | 
| `\v`                | Tabulator pionowy       | `0x000B`             | 

Typem elementu *character_literal* jest `char`.

#### <a name="string-literals"></a>Literały ciągu

C#obsługuje dwa formy literałów ciągu: ***zwykłe literały ciągów*** i ***literały ciągu Verbatim***.

Regularne literały ciągu składa się z zera lub większej liczby znaków ujętych w podwójne `"hello"`cudzysłowy, jak w i mogą zawierać proste sekwencje `\t` ucieczki (takie jak dla znaku tabulacji) oraz sekwencje szesnastkowe i Unicode.

Literał ciągu Verbatim składa się ze `@` znaku, po którym następuje znak podwójnego cudzysłowu, zero lub więcej znaków i zamykający znak podwójnego cudzysłowu. Prostym przykładem `@"hello"`jest. W literale ciągu Verbatim znaki między ogranicznikami są interpretowane jako Verbatim, jedyny wyjątek to *quote_escape_sequence*. W szczególności proste sekwencje unikowe i sekwencje unikowe szesnastkowe i Unicode nie są przetwarzane w literałach ciągów Verbatim. Literał ciągu Verbatim może obejmować wiele wierszy.

```antlr
string_literal
    : regular_string_literal
    | verbatim_string_literal
    ;

regular_string_literal
    : '"' regular_string_literal_character* '"'
    ;

regular_string_literal_character
    : single_regular_string_literal_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_regular_string_literal_character
    : '<Any character except " (U+0022), \\ (U+005C), and new_line_character>'
    ;

verbatim_string_literal
    : '@"' verbatim_string_literal_character* '"'
    ;

verbatim_string_literal_character
    : single_verbatim_string_literal_character
    | quote_escape_sequence
    ;

single_verbatim_string_literal_character
    : '<any character except ">'
    ;

quote_escape_sequence
    : '""'
    ;
```

Znak, który następuje po ukośniku odwrotnym`\`() w *regular_string_literal_character* , musi mieć jeden z następujących znaków `0`: `'`, `"`, `\` `a`,,, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. W przeciwnym razie wystąpi błąd w czasie kompilacji.

Przykład
```csharp
string a = "hello, world";                   // hello, world
string b = @"hello, world";                  // hello, world

string c = "hello \t world";                 // hello      world
string d = @"hello \t world";                // hello \t world

string e = "Joe said \"Hello\" to me";       // Joe said "Hello" to me
string f = @"Joe said ""Hello"" to me";      // Joe said "Hello" to me

string g = "\\\\server\\share\\file.txt";    // \\server\share\file.txt
string h = @"\\server\share\file.txt";       // \\server\share\file.txt

string i = "one\r\ntwo\r\nthree";
string j = @"one
two
three";
```
pokazuje różne literały ciągu. Ostatni literał ciągu, `j`, jest Verbatim literał ciągu, który obejmuje wiele wierszy. Znaki między znakami cudzysłowu, w tym odstępy, takie jak znaki nowego wiersza, są zachowywane Verbatim.

Ponieważ szesnastkowa sekwencja ucieczki może mieć zmienną liczbę cyfr szesnastkowych, literał `"\x123"` ciągu zawiera pojedynczy znak z wartością szesnastkową 123. Aby utworzyć ciąg zawierający znak o wartości szesnastkowej 12, po którym następuje znak 3, jeden może napisać `"\x00123"` lub `"\x12" + "3"` zamiast tego.

Typem elementu *string_literaL* jest `string`.

Każdy literał ciągu nie musi powodować wystąpienia nowego ciągu. Gdy co najmniej dwa literały ciągu, które są równoważne względem operatora równości ciągów ([Operatory równości ciągów](expressions.md#string-equality-operators)) pojawiają się w tym samym programie, te literały ciągu odwołują się do tego samego wystąpienia ciągu. Na przykład dane wyjściowe generowane przez
```csharp
class Test
{
    static void Main() {
        object a = "hello";
        object b = "hello";
        System.Console.WriteLine(a == b);
    }
}
```
jest `True` , ponieważ dwa literały odwołują się do tego samego wystąpienia ciągu.

#### <a name="interpolated-string-literals"></a>Interpolowane literały ciągów

Interpolowane literały ciągów są podobne do literałów ciągów, ale zawierają dziury, które `{` mogą `}`wystąpić w wyrażeniach. W czasie wykonywania wyrażenia są oceniane w celu zamienienia ich formularzy tekstowych na ciąg w miejscu, w którym występuje otwór. Składnia interpolacji ciągu jest opisana w sekcji ([ciągi interpolowane](expressions.md#interpolated-strings)).

Podobnie jak literały ciągu, interpolowane literały ciągów mogą być zwykłe lub Verbatim. Interpolowane zwykłe literały ciągu są rozdzielane przez `$"` i `"`, a interpolowane literały ciągu Verbatim są rozdzielane przez `$@"` i `"`.

Podobnie jak w przypadku innych literałów, analiza leksykalna interpolowanego literału ciągu początkowo skutkuje pojedynczym tokenem, zgodnie z poniższą gramatyką. Jednak przed analizą składni pojedynczy token interpolowanego literału ciągu jest podzielony na kilka tokenów części ciągu otaczających otwory, a elementy wejściowe występujące w dziurach są następnie analizowane ponownie. Może to spowodować wygenerowanie bardziej interpolowanych literałów ciągów do przetworzenia, ale w przypadku, gdy są poprawne, w rezultacie będzie możliwe przetworzenie sekwencji tokenów analizy składni.

```antlr
interpolated_string_literal
    : '$' interpolated_regular_string_literal
    | '$' interpolated_verbatim_string_literal
    ;

interpolated_regular_string_literal
    : interpolated_regular_string_whole
    | interpolated_regular_string_start  interpolated_regular_string_literal_body interpolated_regular_string_end
    ;

interpolated_regular_string_literal_body
    : regular_balanced_text
    | interpolated_regular_string_literal_body interpolated_regular_string_mid regular_balanced_text
    ;

interpolated_regular_string_whole
    : '"' interpolated_regular_string_character* '"'
    ;

interpolated_regular_string_start
    : '"' interpolated_regular_string_character* '{'
    ;

interpolated_regular_string_mid
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '{'
    ;

interpolated_regular_string_end
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '"'
    ;

interpolated_regular_string_characters_after_brace
    : interpolated_regular_string_character_no_brace
    | interpolated_regular_string_characters_after_brace interpolated_regular_string_character
    ;

interpolated_regular_string_character
    : single_interpolated_regular_string_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;

interpolated_regular_string_character_no_brace
    : '<Any interpolated_regular_string_character except close_brace_escape_sequence and any hexadecimal_escape_sequence or unicode_escape_sequence designating } (U+007D)>'
    ;

single_interpolated_regular_string_character
    : '<Any character except \" (U+0022), \\ (U+005C), { (U+007B), } (U+007D), and new_line_character>'
    ;

open_brace_escape_sequence
    : '{{'
    ;

close_brace_escape_sequence
    : '}}'
    ;
    
regular_balanced_text
    : regular_balanced_text_part+
    ;

regular_balanced_text_part
    : single_regular_balanced_text_character
    | delimited_comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' regular_balanced_text ')'
    | '[' regular_balanced_text ']'
    | '{' regular_balanced_text '}'
    ;
    
single_regular_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B), } (U+007D) and new_line_character>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
    
interpolation_format
    : interpolation_format_character+
    ;
    
interpolation_format_character
    : '<Any character except \" (U+0022), : (U+003A), { (U+007B) and } (U+007D)>'
    ;
    
interpolated_verbatim_string_literal
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_literal_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_literal_body
    : verbatim_balanced_text
    | interpolated_verbatim_string_literal_body interpolated_verbatim_string_mid verbatim_balanced_text
    ;
    
interpolated_verbatim_string_whole
    : '@"' interpolated_verbatim_string_character* '"'
    ;
    
interpolated_verbatim_string_start
    : '@"' interpolated_verbatim_string_character* '{'
    ;
    
interpolated_verbatim_string_mid
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '{'
    ;
    
interpolated_verbatim_string_end
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '"'
    ;
    
interpolated_verbatim_string_characters_after_brace
    : interpolated_verbatim_string_character_no_brace
    | interpolated_verbatim_string_characters_after_brace interpolated_verbatim_string_character
    ;
    
interpolated_verbatim_string_character
    : single_interpolated_verbatim_string_character
    | quote_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;
    
interpolated_verbatim_string_character_no_brace
    : '<Any interpolated_verbatim_string_character except close_brace_escape_sequence>'
    ;
    
single_interpolated_verbatim_string_character
    : '<Any character except \" (U+0022), { (U+007B) and } (U+007D)>'
    ;
    
verbatim_balanced_text
    : verbatim_balanced_text_part+
    ;

verbatim_balanced_text_part
    : single_verbatim_balanced_text_character
    | comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' verbatim_balanced_text ')'
    | '[' verbatim_balanced_text ']'
    | '{' verbatim_balanced_text '}'
    ;
    
single_verbatim_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B) and } (U+007D)>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
```

Token *interpolated_string_literal* jest ponownie interpretowany jako wiele tokenów i innych elementów wejściowych w następujący sposób w kolejności wystąpienia w *interpolated_string_literal*:

* Wystąpienia następujących elementów są interpretowane jako oddzielne pojedyncze tokeny: znak wiodący `$` , *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* i *interpolated_verbatim_string_end*.
* Wystąpienia *regular_balanced_text* i *verbatim_balanced_text* między tymi elementami są ponownie przetwarzane jako *input_section* ([Analiza leksykalna](lexical-structure.md#lexical-analysis)) i są ponownie interpretowane jako wyniki sekwencji elementów wejściowych. Mogą one z kolei obejmować interpolowane tokeny literałów ciągów do reinterpretacji.

Analiza składni spowoduje ponowne połączenie tokenów z *interpolated_string_expression* ([ciągami interpolowanymi](expressions.md#interpolated-strings)).

Przykłady do zrobienia


#### <a name="the-null-literal"></a>Literał o wartości null

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal* można niejawnie przekonwertować na typ referencyjny lub typ dopuszczający wartość null.

### <a name="operators-and-punctuators"></a>Operatory i przerywniki

Istnieje kilka rodzajów operatorów i przerywniki. Operatory są używane w wyrażeniach do opisywania operacji obejmujących jeden lub więcej operandów. `a + b` Na przykład, wyrażenie `+` używa operatora, aby `a` dodać dwa operandy i `b`. Przerywniki są przeznaczone do grupowania i oddzielania.

```antlr
operator_or_punctuator
    : '{'  | '}'  | '['  | ']'  | '('   | ')'  | '.'  | ','  | ':'  | ';'
    | '+'  | '-'  | '*'  | '/'  | '%'   | '&'  | '|'  | '^'  | '!'  | '~'
    | '='  | '<'  | '>'  | '?'  | '??'  | '::' | '++' | '--' | '&&' | '||'
    | '->' | '==' | '!=' | '<=' | '>='  | '+=' | '-=' | '*=' | '/=' | '%='
    | '&=' | '|=' | '^=' | '<<' | '<<=' | '=>'
    ;

right_shift
    : '>>'
    ;

right_shift_assignment
    : '>>='
    ;
```

Pionowy pasek w produkcji *right_shift* i *right_shift_assignment* jest używany do wskazania, że w przeciwieństwie do innych produkcji w gramatyce składni nie są dozwolone żadne znaki jakiegokolwiek rodzaju (nieparzyste odstępy) między tokenami. Te produkcje są traktowane specjalnie w celu zapewnienia prawidłowej obsługi *type_parameter_list*s ([parametry typu](classes.md#type-parameters)).

## <a name="pre-processing-directives"></a>Dyrektywy wstępnego przetwarzania

Dyrektywy wstępnego przetwarzania zapewniają możliwość warunkowego pomijania sekcji plików źródłowych, do zgłaszania błędów i ostrzeżeń oraz odróżnić odrębnych regionów kodu źródłowego. Termin "dyrektywy wstępnego przetwarzania" jest używany tylko w celu zapewnienia spójności z językami C++ C i programowaniem. W C#programie nie ma oddzielnego kroku przetwarzania wstępnego; dyrektywy przetwarzania wstępnego są przetwarzane jako część fazy analizy leksykalnej.

```antlr
pp_directive
    : pp_declaration
    | pp_conditional
    | pp_line
    | pp_diagnostic
    | pp_region
    | pp_pragma
    ;
```

Dostępne są następujące dyrektywy wstępnego przetwarzania:

*  `#define`i `#undef`, które są używane do definiowania i niedefiniowania odpowiednio symboli kompilacji warunkowej ([dyrektywy deklaracji](lexical-structure.md#declaration-directives)).
*  `#if`, `#elif`, `#else`, i`#endif`, które są używane do warunkowego pomijania sekcji kodu źródłowego ([dyrektywy kompilacji warunkowej](lexical-structure.md#conditional-compilation-directives)).
*  `#line`, który jest używany do sterowania numerami wierszy emitowanych pod kątem błędów i ostrzeżeń ([dyrektywy wiersza](lexical-structure.md#line-directives)).
*  `#error`i `#warning`, które są używane do wydawania błędów i ostrzeżeń odpowiednio ([dyrektywy diagnostyczne](lexical-structure.md#diagnostic-directives)).
*  `#region`i `#endregion`, które są używane do jawnego oznaczania sekcji kodu źródłowego ([dyrektywy regionu](lexical-structure.md#region-directives)).
*  `#pragma`, który jest używany do określania opcjonalnych informacji kontekstowych do kompilatora ([dyrektywy pragma](lexical-structure.md#pragma-directives)).

Dyrektywa poprzedzająca przetwarzanie zawsze zawiera oddzielny wiersz kodu źródłowego i zawsze zaczyna `#` się od znaku i nazwy dyrektywy wstępnego przetwarzania. Spacja może wystąpić przed `#` znakiem i `#` znakiem oraz między nazwą dyrektywy.

Wiersz `#define`źródłowy zawierający, `#undef` `#if` ,,`#elif` ,,`#endregion` , lub dyrektywy może kończyć się jednowierszowym komentarzem. `#else` `#endif` `#line` Rozdzielane Komentarze ( `/* */` styl komentarzy) nie są dozwolone w wierszach źródłowych zawierających dyrektywy poprzedzające przetwarzanie.

Dyrektywy poprzedzające przetwarzanie nie są tokenami i nie są częścią gramatyki składni C#. Jednakże dyrektywy wstępnego przetwarzania mogą być używane do dołączania lub wykluczania sekwencji tokenów i mogą w ten sposób mieć wpływ C# na znaczenie programu. Na przykład podczas kompilowania programu:
```csharp
#define A
#undef B

class C
{
#if A
    void F() {}
#else
    void G() {}
#endif

#if B
    void H() {}
#else
    void I() {}
#endif
}
```
wynikiem jest dokładna sekwencja tokenów co program:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

W związku z tym te dwa programy są bardzo różne i syntaktycznie są identyczne.

### <a name="conditional-compilation-symbols"></a>Symbole kompilacji warunkowej

Funkcje kompilacji `#if`warunkowej udostępniane przez `#else`, `#elif`,, i `#endif` dyrektywy są kontrolowane przez wyrażenia wstępnego przetwarzania ([wyrażenia wstępnego przetwarzania](lexical-structure.md#pre-processing-expressions)) i warunkowe symbole kompilacji.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Symbol kompilacji warunkowej ma dwa stany: ***zdefiniowane*** lub ***niezdefiniowane***. Na początku przetwarzania leksykalnego pliku źródłowego symbol kompilacji warunkowej jest niezdefiniowany, chyba że został jawnie zdefiniowany przez mechanizm zewnętrzny (na przykład opcję kompilatora wiersza polecenia). Po przetworzeniu `#define` dyrektywy, symbol kompilacji warunkowej o nazwie w tej dyrektywie zostanie zdefiniowany w tym pliku źródłowym. Symbol pozostaje zdefiniowany do czasu `#undef` przetworzenia dyrektywy dla tego samego symbolu lub do momentu osiągnięcia końca pliku źródłowego. Implikacją tego jest to, `#define` że `#undef` dyrektywy w jednym pliku źródłowym nie mają wpływu na inne pliki źródłowe w tym samym programie.

W przypadku odwołania w wyrażeniu poprzedzającym przetwarzanie, zdefiniowany symbol kompilacji warunkowej ma wartość `true`logiczną, a niezdefiniowany symbol kompilacji warunkowej ma wartość `false`logiczną. Nie istnieje wymóg, aby symbole kompilacji warunkowej były jawnie zadeklarowane przed odwołaniami do nich w wyrażeniach poprzedzających przetwarzanie. Zamiast tego symbole niezadeklarowane są po prostu niezdefiniowane i dlatego `false`mają wartość.

Przestrzeń nazw dla symboli kompilacji warunkowej jest odrębna i oddzielona od wszystkich innych nazwanych C# jednostek w programie. Do symboli kompilacji warunkowej można odwoływać `#define` się `#undef` tylko w dyrektywach i i w wyrażeniach poprzedzających przetwarzanie.

### <a name="pre-processing-expressions"></a>Wyrażenia wstępnego przetwarzania

Wyrażenia wstępnego przetwarzania mogą wystąpić w `#if` dyrektywach i `#elif` . Operatory `!`, `==`, ,`!=` isą`||` dozwolone w wyrażeniach poprzedzających przetwarzanie, a nawiasy mogą być używane do grupowania. `&&`

```antlr
pp_expression
    : whitespace? pp_or_expression whitespace?
    ;

pp_or_expression
    : pp_and_expression
    | pp_or_expression whitespace? '||' whitespace? pp_and_expression
    ;

pp_and_expression
    : pp_equality_expression
    | pp_and_expression whitespace? '&&' whitespace? pp_equality_expression
    ;

pp_equality_expression
    : pp_unary_expression
    | pp_equality_expression whitespace? '==' whitespace? pp_unary_expression
    | pp_equality_expression whitespace? '!=' whitespace? pp_unary_expression
    ;

pp_unary_expression
    : pp_primary_expression
    | '!' whitespace? pp_unary_expression
    ;

pp_primary_expression
    : 'true'
    | 'false'
    | conditional_symbol
    | '(' whitespace? pp_expression whitespace? ')'
    ;
```

W przypadku odwołania w wyrażeniu poprzedzającym przetwarzanie, zdefiniowany symbol kompilacji warunkowej ma wartość `true`logiczną, a niezdefiniowany symbol kompilacji warunkowej ma wartość `false`logiczną.

Obliczenie wyrażenia przetwarzania wstępnego zawsze zwraca wartość logiczną. Reguły obliczania dla wyrażenia przetwarzania wstępnego są takie same jak dla wyrażeń stałych ([wyrażeń stałych](expressions.md#constant-expressions)), z tą różnicą, że jedyne jednostki zdefiniowane przez użytkownika, do których można się odwoływać, są symbolami kompilacji warunkowej.

### <a name="declaration-directives"></a>Dyrektywy deklaracji

Dyrektywy deklaracji są używane do definiowania lub niedefiniowania symboli kompilacji warunkowej.

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

Przetwarzanie `#define` dyrektywy powoduje, że dany symbol kompilacji warunkowej zostanie zdefiniowany, rozpoczynając od wiersza źródłowego, który następuje po dyrektywie. Podobnie przetwarzanie `#undef` dyrektywy powoduje, że dany symbol kompilacji warunkowej staje się niezdefiniowany, rozpoczynając od wiersza źródłowego, który następuje po dyrektywie.

Wszystkie `#define` i`#undef` dyrektywy w pliku źródłowym muszą wystąpić przed pierwszym *tokenem* ([tokenami](lexical-structure.md#tokens)) w pliku źródłowym; w przeciwnym razie wystąpi błąd kompilacji. W intuicyjnych warunkach `#define` , `#undef` dyrektywy muszą poprzedzać dowolny "kod prawdziwy" w pliku źródłowym.

Przykład:
```csharp
#define Enterprise

#if Professional || Enterprise
    #define Advanced
#endif

namespace Megacorp.Data
{
    #if Advanced
    class PivotTable {...}
    #endif
}
```
jest prawidłowy, `#define` ponieważ dyrektywy poprzedzają pierwszy token `namespace` (słowo kluczowe) w pliku źródłowym.

Poniższy przykład powoduje błąd czasu kompilacji, ponieważ `#define` następujący kod prawdziwy:
```csharp
#define A
namespace N
{
    #define B
    #if B
    class Class1 {}
    #endif
}
```

A `#define` może zdefiniować znak kompilacji warunkowej, który jest już zdefiniowany, bez występowania jakichkolwiek `#undef` interwencji dla tego symbolu. W poniższym przykładzie zdefiniowano symbol `A` kompilacji warunkowej, a następnie definiuje go ponownie.
```csharp
#define A
#define A
```

A `#undef` może "usuń definicję" warunkowego symbolu kompilacji, który nie jest zdefiniowany. W poniższym przykładzie zdefiniowano symbol `A` kompilacji warunkowej, a następnie jego definicję należy wykonać dwa razy. Mimo że druga `#undef` nie ma wpływu, jest nadal ważna.
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>Dyrektywy kompilacji warunkowej

Dyrektywy kompilacji warunkowej są używane do warunkowego dołączania lub wykluczania części pliku źródłowego.

```antlr
pp_conditional
    : pp_if_section pp_elif_section* pp_else_section? pp_endif
    ;

pp_if_section
    : whitespace? '#' whitespace? 'if' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_elif_section
    : whitespace? '#' whitespace? 'elif' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_else_section:
    | whitespace? '#' whitespace? 'else' pp_new_line conditional_section?
    ;

pp_endif
    : whitespace? '#' whitespace? 'endif' pp_new_line
    ;

conditional_section
    : input_section
    | skipped_section
    ;

skipped_section
    : skipped_section_part+
    ;

skipped_section_part
    : skipped_characters? new_line
    | pp_directive
    ;

skipped_characters
    : whitespace? not_number_sign input_character*
    ;

not_number_sign
    : '<Any input_character except #>'
    ;
```

Jak wskazano w składni, dyrektywy kompilacji warunkowej muszą być zapisywane jako zestawy składające się z, w `#if` kolejności, dyrektywy, dyrektyw `#elif` , zero lub więcej dyrektywy `#else` , zero lub `#endif` jedna dyrektywa i dyrektywa. Między dyrektywami są warunkowe sekcje kodu źródłowego. Każda sekcja jest kontrolowana przez bezpośrednio poprzednią dyrektywę. Sekcja warunkowa może sama zawierać zagnieżdżone dyrektywy kompilacji warunkowej, jeśli te dyrektywy tworzą kompletne zestawy.

*Pp_conditional* wybiera co najwyżej jeden z zawartych *conditional_section*s do normalnego przetwarzania leksykalnego:

*  *Pp_expression* `#if` s dyrektyw i `#elif` są `true`oceniane w kolejności do momentu 1. Jeśli wynikiem jest wyrażenie `true`, *conditional_section* odpowiedniej dyrektywy jest zaznaczone.
*  Jeśli wszystkie *pp_expression*s `false` `#else` , a jeśli `#else` jest obecna dyrektywa, zostanie wybrana *conditional_section* dyrektywy.
*  W przeciwnym razie nie jest zaznaczona żadna *conditional_section* .

Wybrany *conditional_section*(jeśli istnieje) jest przetwarzany jako normalny *input_section*: kod źródłowy zawarty w sekcji musi być zgodny z gramatyką leksykalną; tokeny są generowane na podstawie kodu źródłowego w sekcji; i dyrektywy wstępnego przetwarzania w sekcji mają określone efekty.

Pozostałe *conditional_section*s, jeśli istnieją, są przetwarzane jako *skipped_section*s: z wyjątkiem dyrektyw poprzedzających przetwarzanie, kod źródłowy w sekcji nie musi być zgodny z gramatyką leksykalną; nie Wygenerowano żadnych tokenów z kodu źródłowego w sekcji; i dyrektywy wstępnego przetwarzania w sekcji muszą być poprawiane w sposób leksykalny, ale nie są przetwarzane w inny sposób. W *conditional_section* , który jest przetwarzany jako *skipped_section*, wszystkie zagnieżdżone *conditional_section*s (zawarte w zagnieżdżonych `#if`... `#endif` i`#region`... Konstrukcje) są również przetwarzane jako *skipped_section s.* `#endregion`

Poniższy przykład ilustruje, jak można zagnieżdżać dyrektywy kompilacji warunkowej:
```csharp
#define Debug       // Debugging on
#undef Trace        // Tracing off

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
            #if Trace
                WriteToLog(this.ToString());
            #endif
        #endif
        CommitHelper();
    }
}
```

Z wyjątkiem dyrektyw poprzedzających przetwarzanie pominięty kod źródłowy nie podlega analizie leksykalnej. Na przykład następujące elementy są prawidłowe pomimo niezakończonego komentarza w `#else` sekcji:
```csharp
#define Debug        // Debugging on

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
        #else
            /* Do something else
        #endif
    }
}
```

Należy jednak zauważyć, że dyrektywy poprzedzające przetwarzanie są wymagane do poprawnego działania, nawet w pominiętych sekcjach kodu źródłowego.

Dyrektywy poprzedzające przetwarzanie nie są przetwarzane, gdy są wyświetlane wewnątrz wielowierszowych elementów wejściowych. Na przykład program:
```csharp
class Hello
{
    static void Main() {
        System.Console.WriteLine(@"hello, 
#if Debug
        world
#else
        Nebraska
#endif
        ");
    }
}
```
wyniki w danych wyjściowych:
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

W szczególnych przypadkach zestaw dyrektyw przetwarzania wstępnego, które są przetwarzane, może zależeć od oceny *pp_expression*. Przykład:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
zawsze generuje ten sam strumień tokenu (`class` `}` `Q` `{` ), niezależnie od tego, czy `X` jest zdefiniowany. Jeśli `X` jest zdefiniowany, jedynymi przetworzonymi `#if` dyrektywami są i `#endif`, ze względu na komentarz wielowierszowy. Jeśli `X` jest niezdefiniowany, wówczas trzy dyrektywy`#if`( `#else`, `#endif`,) są częścią zestawu dyrektywy.

### <a name="diagnostic-directives"></a>Dyrektywy diagnostyczne

Dyrektywy diagnostyczne służą do jawnego generowania komunikatów błędów i ostrzeżeń, które są zgłaszane w taki sam sposób jak inne błędy i ostrzeżenia w czasie kompilacji.

```antlr
pp_diagnostic
    : whitespace? '#' whitespace? 'error' pp_message
    | whitespace? '#' whitespace? 'warning' pp_message
    ;

pp_message
    : new_line
    | whitespace input_character* new_line
    ;
```

Przykład:
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
zawsze generuje ostrzeżenie ("Przegląd kodu wymagany przed zaewidencjonowaniem") i generuje błąd czasu kompilacji ("kompilacja nie może jednocześnie debugować i detalicznych"), jeśli symbole `Debug` warunkowe i `Retail` zostały zdefiniowane. Należy pamiętać, że element *pp_message* może zawierać dowolny tekst; w przeciwnym razie nie musi zawierać poprawnie sformułowanych tokenów, jak pokazano w pojedynczym cudzysłowie w `can't`wyrazie.

### <a name="region-directives"></a>Dyrektywy regionów

Dyrektywy regionów są używane do jawnego oznaczania regionów kodu źródłowego.

```antlr
pp_region
    : pp_start_region conditional_section? pp_end_region
    ;

pp_start_region
    : whitespace? '#' whitespace? 'region' pp_message
    ;

pp_end_region
    : whitespace? '#' whitespace? 'endregion' pp_message
    ;
```

Do regionu nie dołączono znaczenia semantycznego; regiony są przeznaczone do użytku przez programistę lub przez zautomatyzowane narzędzia do oznaczania sekcji kodu źródłowego. Komunikat określony w `#region` dyrektywie lub `#endregion` również nie ma znaczenia semantycznego, a jedynie służy do identyfikowania regionu. `#region` Dopasowywanie `#endregion` i dyrektywy mogą mieć różne *pp_message*s.

Przetwarzanie leksykalne regionu:
```csharp
#region
...
#endregion
```
odpowiada dokładnie na przetwarzanie leksykalne dyrektywy warunkowej kompilacji w postaci:
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Dyrektywy linii

Dyrektywy wiersza mogą służyć do zmiany numerów wierszy i nazw plików źródłowych raportowanych przez kompilator w danych wyjściowych, takich jak ostrzeżenia i błędy, i które są używane przez atrybuty informacji o wywołującym ([atrybuty informacji o wywołaniu](attributes.md#caller-info-attributes)).

Dyrektywy wiersza są najczęściej używane w narzędziach meta-programistycznych, C# które generują kod źródłowy na podstawie innych danych wejściowych tekstu.

```antlr
pp_line
    : whitespace? '#' whitespace? 'line' whitespace line_indicator pp_new_line
    ;

line_indicator
    : decimal_digit+ whitespace file_name
    | decimal_digit+
    | 'default'
    | 'hidden'
    ;

file_name
    : '"' file_name_character+ '"'
    ;

file_name_character
    : '<Any input_character except ">'
    ;
```

Gdy żadne `#line` dyrektywy nie są obecne, kompilator zgłasza w danych wyjściowych prawdziwe numery wierszy i nazwy plików źródłowych. Podczas przetwarzania `#line` dyrektywy, która zawiera *line_indicator* , która nie `default`jest, kompilator traktuje wiersz po dyrektywie jako mający podany numer wiersza (i nazwę pliku, jeśli jest określony).

`#line default` Dyrektywa Odwraca efekt wszystkich poprzednich dyrektyw #line. Kompilator raportuje prawdziwe informacje o wierszu dla kolejnych wierszy, dokładnie tak, `#line` jakby nie zostały przetworzone dyrektywy.

`#line hidden` Dyrektywa nie ma wpływu na numery plików i wierszy raportowane w komunikatach o błędach, ale wpływa na Debugowanie na poziomie źródła. Podczas debugowania wszystkie wiersze między `#line hidden` dyrektywą a kolejną `#line` dyrektywą (nie `#line hidden`) nie zawierają informacji o numerze wiersza. Podczas przechodzenia przez kod w debugerze te wiersze zostaną całkowicie pominięte.

Należy zauważyć, że *nazwa_pliku* różni się od zwykłego literału ciągu w tym znaku ucieczki nie są przetwarzane; znak "`\`" oznacza po prostu zwykły ukośnik odwrotny w ciągu *nazwa_pliku*.

### <a name="pragma-directives"></a>Dyrektywy pragma

`#pragma` Dyrektywa przetwarzania wstępnego służy do określania opcjonalnych informacji kontekstowych do kompilatora. Informacje podane w `#pragma` dyrektywie nigdy nie zmieniają semantyki programu.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C#zawiera `#pragma` dyrektywy kontrolujące ostrzeżenia kompilatora. Przyszłe wersje języka mogą zawierać dodatkowe `#pragma` dyrektywy. Aby zapewnić współdziałanie C# z innymi kompilatorami, C# kompilator firmy Microsoft nie wydaje błędów kompilacji w `#pragma` przypadku nieznanych dyrektyw; takie dyrektywy nie generują jednak ostrzeżeń.

#### <a name="pragma-warning"></a>Ostrzeżenie pragma

`#pragma warning` Dyrektywa służy do wyłączania lub przywracania całego lub określonego zestawu komunikatów ostrzegawczych podczas kompilacji kolejnego tekstu programu.

```antlr
pragma_warning_body
    : 'warning' whitespace warning_action
    | 'warning' whitespace warning_action whitespace warning_list
    ;

warning_action
    : 'disable'
    | 'restore'
    ;

warning_list
    : decimal_digit+ (whitespace? ',' whitespace? decimal_digit+)*
    ;
```

`#pragma warning` Dyrektywa, która pomija listę ostrzeżeń, ma wpływ na wszystkie ostrzeżenia. `#pragma warning` Dyrektywa, która zawiera listę ostrzeżeń, ma wpływ tylko na te ostrzeżenia, które są określone na liście.

`#pragma warning disable` Dyrektywa powoduje wyłączenie wszystkich lub danego zestawu ostrzeżeń.

`#pragma warning restore` Dyrektywa przywraca wszystkie lub dany zestaw ostrzeżeń do stanu, który obowiązywał na początku jednostki kompilacji. Należy pamiętać, że jeśli określone ostrzeżenie zostało wyłączone zewnętrznie, `#pragma warning restore` to (czy dla wszystkich lub określonych ostrzeżeń) nie będzie ponownie włączać tego ostrzeżenia.

W poniższym przykładzie pokazano `#pragma warning` sposób tymczasowego wyłączenia ostrzeżenia raportowanego w przypadku odwołania do przestarzałych elementów członkowskich przy użyciu numeru ostrzeżenia z kompilatora firmy Microsoft. C#
```csharp
using System;

class Program
{
    [Obsolete]
    static void Foo() {}

    static void Main() {
#pragma warning disable 612
    Foo();
#pragma warning restore 612
    }
}
```
