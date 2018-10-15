# <a name="lexical-structure"></a>Struktura leksykalna

## <a name="programs"></a>Programy

C# ***program*** składa się z co najmniej jeden ***pliki źródłowe***, nazywanej formalnie ***jednostki kompilacji*** ([jednostki kompilacji](namespaces.md#compilation-units)). Plik źródłowy jest uporządkowana sekwencja znaków Unicode. Pliki źródłowe zwykle mają relację z plikami w systemie plików, ale ta zgodność nie jest wymagana. Maksymalny przenośności, zalecane jest, że pliki w systemie plików jest zakodowane za pomocą UTF-8 kodowania.

Koncepcyjnie mówiąc, program jest kompilowany, za pomocą trzech kroków:

1. Przekształcenie, które służy do konwertowania pliku Repertuar znaków określonego i schemat kodowania na sekwencję znaków Unicode.
2. Poddawać analizie leksykalnej, co przekłada strumień wejściowy znaki Unicode w strumieniu tokenów.
3. Analizy składni, która przetwarza strumień tokenów na kod wykonywalny.

## <a name="grammars"></a>Gramatyki

Ta specyfikacja przedstawia informacje o składni programowania w języku C# za pomocą dwóch gramatyki. ***Gramatyka leksykalna*** ([gramatyka Leksykalna](lexical-structure.md#lexical-grammar)) definiuje, jak znaki Unicode są łączone w celu terminatory linii w formularzu, biały znak, komentarze, tokenów i dyrektywy przetwarzania wstępnego. ***Składni gramatyki*** ([składni gramatyki](lexical-structure.md#syntactic-grammar)) definiuje, jak tokeny, wynikające z gramatyka leksykalna są łączone w celu programy formularzy C#.

### <a name="grammar-notation"></a>Notacja gramatyki

Gramatyki leksykalne i składniowych są prezentowane w formularz Backus-naur przy użyciu notacji ANTLR narzędzie gramatyki.

### <a name="lexical-grammar"></a>Gramatyka leksykalna

Gramatyka leksykalna języka C# są prezentowane w [poddawać analizie Leksykalnej](lexical-structure.md#lexical-analysis), [tokenów](lexical-structure.md#tokens), i [przetwarzania wstępnego dyrektywy](lexical-structure.md#pre-processing-directives). Terminalu symbole gramatyka leksykalna są znaki z zestawu znaków Unicode i gramatyka leksykalna Określa, jak znaki są łączone w celu tokenów formularza ([tokenów](lexical-structure.md#tokens)), biały ([biały](lexical-structure.md#white-space)), komentarze ([komentarze](lexical-structure.md#comments)) oraz dyrektywy przetwarzania wstępnego ([przetwarzania wstępnego dyrektywy](lexical-structure.md#pre-processing-directives)).

Każdy plik źródłowy w języku C# służącego musi być zgodna z *wejściowych* wersji produkcyjnej, gramatyka leksykalna ([poddawać analizie Leksykalnej](lexical-structure.md#lexical-analysis)).

### <a name="syntactic-grammar"></a>Gramatyka składni

Gramatyka składni języka C# są prezentowane w rozdziałach i dodatki, które należy wykonać w tym rozdziale. Terminalu symbole gramatyki składniowe są tokeny zdefiniowane przez gramatyka leksykalna i gramatyki składni Określa, jak tokeny są łączone w celu formularza C# programy.

Każdy plik źródłowy w języku C# służącego musi być zgodna z *compilation_unit* produkcji składni gramatyki ([jednostki kompilacji](namespaces.md#compilation-units)).

## <a name="lexical-analysis"></a>Poddawać analizie leksykalnej

*Wejściowych* produkcji definiuje strukturę leksykalne plik źródłowy C#. Każdy plik źródłowy w języku C# służącego musi być zgodna z ten produkcji gramatyka leksykalna.

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

Pięć podstawowych elementów tworzą struktura leksykalna pliku źródłowego języka C#: wiersz terminatory ([wiersz terminatory](lexical-structure.md#line-terminators)), biały ([biały](lexical-structure.md#white-space)), komentarze ([komentarze](lexical-structure.md#comments)), tokeny ([tokenów](lexical-structure.md#tokens)) oraz dyrektywy przetwarzania wstępnego ([przetwarzania wstępnego dyrektywy](lexical-structure.md#pre-processing-directives)). Z tych podstawowych elementów tylko tokeny są istotne w gramatyce składni programu w języku C# ([składni gramatyki](lexical-structure.md#syntactic-grammar)).

Leksykalne przetwarzanie plik źródłowy C# zawiera zmniejszenie pliku sekwencję tokenów, która staje się dane wejściowe do analizy składni. Terminatory wiersza, biały i komentarze, które może służyć do oddzielania tokenów, dyrektywy przetwarzania wstępnego mogą powodować części pliku źródłowego do pominięcia, a w przeciwnym razie te elementy leksykalne nie mają wpływu na strukturę składni programu w języku C#.

W przypadku literały ciągów znaków interpolowanych ([interpolowane literałów ciągów](lexical-structure.md#interpolated-string-literals)) pojedynczy token początkowo jest generowany przez poddawać analizie leksykalnej, ale został podzielony na kilka elementów wejściowych, które wielokrotnie poddać analizie leksykalnej dopóki wszystkie literały ciągów znaków interpolowanych zostały rozwiązane. Tokenami wynikowymi następnie służyć jako dane wejściowe do analizy składni.

Po kilku produkcji gramatyka leksykalna odpowiada sekwencji znaków w pliku źródłowym, leksykalne przetwarzania zawsze stanowi element najdłuższy leksykalne możliwe. Na przykład sekwencja znaków `//` jest przetwarzany jako początek komentarz jednowierszowy, ponieważ leksykalne elementu jest dłuższy niż jeden `/` tokenu.

### <a name="line-terminators"></a>Terminatory wiersza

Terminatory wiersza podzielić wiersze znaki plik źródłowy C#.

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

Dla zgodności przy użyciu źródła code narzędzia edycji Dodaj znaczniki końca pliku, a umożliwiające źródło pliku do wyświetlenia jako sekwencja prawidłowo zakończony wierszy, następujące przekształcenia są stosowane w kolejności, aby każdy plik źródłowy w języku C# służącego:

*  Jeśli ostatni znak pliku źródłowego jest znakiem formantu-Z (`U+001A`), ten znak jest usuwany.
*  Znak powrotu karetki (`U+000D`) zostanie dodany na końcu pliku źródłowego, jeśli ten plik źródłowy jest pusty, a ostatni znak pliku źródłowego nie jest znak powrotu karetki (`U+000D`), znak wysuwu wiersza (`U+000A`), separator wiersza (`U+2028`), lub separatorem akapitów (`U+2029`).

### <a name="comments"></a>Komentarze

Obsługiwane są dwa rodzaje komentarzy: Komentarze jednowierszowe i rozdzielany komentarzy. ***Komentarze jednowierszowe*** rozpoczynających się od znaków `//` i Rozszerz do końca wiersza źródłowego. ***Rozdzielany komentarze*** rozpoczynających się od znaków `/*` oraz kończyć się znakami `*/`. Rozdzielany komentarze może obejmować wiele wierszy.

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
    : '/*' delimited_comment_section* asterisk* '/'
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

Nie zagnieżdżaj komentarzy. Sekwencje znaków `/*` i `*/` nie mają specjalnego znaczenia w ramach `//` komentarz i sekwencje znaków `//` i `/*` nie mają specjalnego znaczenia w ramach rozdzielany komentarz.

Komentarze nie są przetwarzane w ramach znakowe i literały.

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
Zawiera rozdzielaną komentarz.

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
Pokazuje kilka Komentarze jednowierszowe.

### <a name="white-space"></a>Biały znak

Biały znak jest zdefiniowany jako dowolny znak z klasą Unicode Zs (która zawiera znak spacji), a także znaku tabulacji poziomej, znak tabulacji pionowej i formularzu znaku wysuwu.

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>Tokeny

Istnieje kilka rodzajów tokenów: identyfikatory, słowa kluczowe, literały, operatorów i przerywniki języka. Biały znak i komentarze nie są tokeny, chociaż działają jako separatory tokenów.

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

Znak sekwencji ucieczki Unicode reprezentuje znak Unicode. Sekwencje ucieczki znaków Unicode są przetwarzane w identyfikatorach ([identyfikatory](lexical-structure.md#identifiers)), znaków w literałach ([literały znakowe](lexical-structure.md#character-literals)) i regularnego literałów ([Literałyciągu](lexical-structure.md#string-literals)). Znak ucieczki Unicode nie są przetwarzane w innym miejscu (na przykład w celu utworzenia operatora, znak interpunkcyjny lub słowa kluczowego).

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Sekwencja unikowa Unicode reprezentuje pojedynczy znak Unicode sformułowany wykonując liczb szesnastkowych "`\u`"or"`\U`" znaków. Ponieważ języka C# używa kodowania 16-bitowych punkty kodowe Unicode i ciągi znaków, znak Unicode z zakresu od U + 10000 do U + 10FFFF nie jest dozwolona w literale znakowym i jest reprezentowana w literale ciągu za pomocą para zastępcza Unicode. Znaki Unicode z punktów kod powyżej 0x10FFFF nie są obsługiwane.

Wiele tłumaczeń nie są wykonywane. Na przykład, literał ciągu "`\u005Cu005C`"jest odpowiednikiem"`\u005C`"zamiast"`\`". Wartość Unicode `\u005C` jest znakiem "`\`".

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
Pokazuje kilka zastosowań `\u0066`, czyli sekwencji unikowej na literę "`f`". Program jest odpowiednikiem
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

Reguły dotyczące identyfikatorów podane w tej sekcji dokładnie odpowiadać elementowi zalecane przez Unicode Standard załącznika 31, z tą różnicą, że podkreślenie jest dozwolona jako znak początkowy (tak jak to tradycyjne w języku programowania C), specjalne Unicode, który sekwencje to dozwolone w identyfikatorach, a "`@`" znak jest dozwolony jako prefiksu potrzeba włączenia słów kluczowych jako identyfikatorów.

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

Instrukcje dotyczące klas znaków Unicode, które są wymienione powyżej Zobacz Unicode Standard, w wersji 3.0, sekcja 4.5.

Przykłady prawidłowych identyfikatorów "`identifier1`","`_identifier2`", a "`@if`".

Identyfikator programu odpowiadające musi być w formacie kanonicznym definicją formularza normalizacji Unicode C, zgodnie z definicją standardu Unicode Standard załącznika 15. Zachowanie w przypadku napotkania identyfikator nie jest w formularzu C normalizacji jest zdefiniowane w implementacji; jednak Diagnostyka nie jest wymagana.

Prefiks "`@`" umożliwia słów kluczowych jako identyfikatorów, co jest przydatne, gdy komunikuje się z innymi językami programowania. Znak `@` nie jest częścią identyfikatora, więc identyfikator może być widoczny w innych językach, jako identyfikator normalne, bez prefiksu. Identyfikator z `@` nosi nazwę prefiks ***identyfikator dosłownego wyrażenia***. Korzystanie z `@` prefiks dla identyfikatorów, które nie są słowami kluczowymi jest dozwolone, ale zdecydowanie niezalecane jako stylu.

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
definiuje klasę o nazwie "`class`"ze statyczną metodę o nazwie"`static`"pobierającej parametr o nazwie"`bool`". Należy zauważyć, że ponieważ specjalne Unicode nie są dozwolone w słów kluczowych, token "`cl\u0061ss`"jest identyfikatorem i jest taki sam identyfikator jak"`@class`".

Dwa identyfikatory są uważane za takie same, jeśli są one identyczne, po zastosowaniu następujące przekształcenia w kolejności:

*  Prefiks "`@`", jeśli używany, zostanie usunięty.
*  Każdy *unicode_escape_sequence* jest przekształcana na jego odpowiedniego znaku Unicode.
*  Wszelkie *formatting_character*s są usuwane.

Identyfikatory zawierające dwóch następujących po sobie znaki podkreślenia (`U+005F`) są zarezerwowane do użytku przez implementację. Na przykład implementacja może zawierać słów kluczowych rozszerzonych, które zaczynają się od dwóch podkreśleń.

### <a name="keywords"></a>Słowa kluczowe

A ***— słowo kluczowe*** to identyfikator jak sekwencja znaków jest zarezerwowany i nie można użyć jako identyfikatora z wyjątkiem sytuacji, gdy są poprzedzone `@` znaków.

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

W pewnych miejscach w gramatyce określone identyfikatory mają specjalne znaczenie, ale nie są słowami kluczowymi. Takie identyfikatory są czasami określane jako "kontekstowymi słowami kluczowymi". Na przykład w deklaracji właściwości "`get`"i"`set`" identyfikatory mają specjalne znaczenie ([Akcesory](classes.md#accessors)). Identyfikator innego niż `get` lub `set` nigdy nie jest dozwolona w tych lokalizacjach, więc to zastosowanie nie powoduje konfliktu z użyciem tych słów jako identyfikatorów. W innych przypadkach, takich jak o identyfikatorze "`var`" w niejawnie wpisane deklaracje zmiennych lokalnych ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)), kontekstowe słowo kluczowe, mogą powodować konflikt z nazwami zadeklarowane. W takich przypadkach zadeklarowana nazwa mają pierwszeństwo przed użyciem identyfikator kontekstowego słowa kluczowego.

### <a name="literals"></a>Literały

A ***literału*** jest reprezentacją kod źródłowy wartość.

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

#### <a name="boolean-literals"></a>Wartość logiczna literałów

Istnieją dwie wartości literałów wartości logicznych: `true` i `false`.

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

Typ *boolean_literal* jest `bool`.

#### <a name="integer-literals"></a>Literały całkowite

Literały całkowite służy do zapisywania wartości typów `int`, `uint`, `long`, i `ulong`. Literały całkowite mają dwa możliwe formy: dziesiętną, a szesnastkową.

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

Typ literału typu integer jest określany w następujący sposób:

*  Jeśli literał ma Brak przyrostka, ma to pierwsza z tych typów, w których jej wartość może być reprezentowana: `int`, `uint`, `long`, `ulong`.
*  Jeśli jako sufiks literału przez `U` lub `u`, ma to pierwsza z tych typów, w których jej wartość może być reprezentowana: `uint`, `ulong`.
*  Jeśli jako sufiks literału przez `L` lub `l`, ma to pierwsza z tych typów, w których jej wartość może być reprezentowana: `long`, `ulong`.
*  Jeśli jako sufiks literału przez `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, lub `lu`, jest on typu `ulong`.

Jeśli wartością reprezentowaną przez literał liczby całkowitej jest poza zakresem `ulong` typu, wystąpi błąd kompilacji.

Jako styl, zalecane jest, "`L`"można użyć zamiast"`l`" podczas zapisywania literały ciągów typu `long`, ponieważ łatwo pomylić literę "`l`"z cyfrą"`1`".

Aby zezwolić na najmniejszą możliwą `int` i `long` wartości do zapisania jako dziesiętna liczba całkowita literały, istnieją następujące dwie reguły:

* Gdy *decimal_integer_literal* wartością 2147483648 (2 ^ 31) i nie *integer_type_suffix* pojawia się jako token bezpośrednio po jednoargumentowego znaku minusa token operatora ([minus jednoargumentowy operator](expressions.md#unary-minus-operator)), wynik jest stałą typu `int` o wartości od -2147483648 (-2 ^ 31). W innych sytuacjach takich *decimal_integer_literal* typu `uint`.
* Gdy *decimal_integer_literal* wartością 9223372036854775808 (2 ^ 63) i nie *integer_type_suffix* lub *integer_type_suffix* `L` lub `l` pojawia się jako token bezpośrednio po jednoargumentowego znaku minusa token operatora ([jednoargumentowy minus operator](expressions.md#unary-minus-operator)), wynik jest stałą typu `long` wartością wartość -9223372036854775808 (-2 ^ 63). W innych sytuacjach takich *decimal_integer_literal* typu `ulong`.

#### <a name="real-literals"></a>Literały rzeczywistych

Literały rzeczywistych służy do zapisywania wartości typów `float`, `double`, i `decimal`.

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

Jeśli nie *real_type_suffix* określono typ rzeczywistego literał jest `double`. W przeciwnym razie sufiks rzeczywistego typu, określa typ rzeczywistego literału w następujący sposób:

*  Literał rzeczywisty sufiks przez `F` lub `f` typu `float`. Na przykład literały `1f`, `1.5f`, `1e10f`, i `123.456F` są wszystkie typu `float`.
*  Literał rzeczywisty sufiks przez `D` lub `d` typu `double`. Na przykład literały `1d`, `1.5d`, `1e10d`, i `123.456D` są wszystkie typu `double`.
*  Literał rzeczywisty sufiks przez `M` lub `m` typu `decimal`. Na przykład literały `1m`, `1.5m`, `1e10m`, i `123.456M` są wszystkie typu `decimal`. Ten literał jest konwertowany na `decimal` wartość biorąc dokładna wartość, a w razie potrzeby zaokrąglania do najbliższej przy użyciu wartości reprezentowanych przez zaokrąglenie banker ([typu dziesiętnego](types.md#the-decimal-type)). Dowolnej skali, które są widoczne w literale są zachowywane, chyba że wartość jest zaokrąglana, lub wartość wynosi zero (w takim przypadku ostatnie logowania i skalowanie będzie 0). Z tego powodu literału `2.900m` będzie analizowany w celu utworzenia dziesiętnych znakiem `0`, współczynnik `2900`i skalowanie `3`.

Jeśli określony literał nie może być przedstawiony w wskazanego typu, wystąpi błąd kompilacji.

Wartość rzeczywistego literału typu `float` lub `double` jest określana za pomocą IEEE tryb "zaokrąglony do najbliższej".

Należy pamiętać, że w rzeczywistych literał, cyfry dziesiętne zawsze są wymagane po przecinku. Na przykład `1.3F` jest literał liczby rzeczywistej ale `1.F` nie jest.

#### <a name="character-literals"></a>Literały znakowe

Literał znakowy, który reprezentuje pojedynczy znak, a składa się zwykle znaku w cudzysłowie, jak w `'a'`.

Uwaga: Notacji gramatyki ANTLR sprawia, że następujące mylące! W ANTLR, kiedy piszesz `\'` oznacza pojedynczy cudzysłów `'`. I podczas wpisywania `\\` oznacza pojedynczy ukośnik odwrotny `\`. W związku z tym pierwsze reguły dla literału znakowego oznacza, że zaczyna się od pojedynczy cudzysłów znaków, a następnie pojedynczy cudzysłów. I jedenaście możliwe proste sekwencje ucieczki są `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.

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

Znak, który następuje znak ukośnika odwrotnego (`\`) w *znak* musi mieć jedną z następujących znaków: `'`, `"`, `\`, `0`, `a`, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. W przeciwnym razie wystąpi błąd kompilacji.

Szesnastkowa sekwencja unikowa reprezentuje pojedynczy znak Unicode o wartości sformułowany wykonując liczb szesnastkowych "`\x`".

Jeśli wartością reprezentowaną przez literału znakowego jest większa niż `U+FFFF`, wystąpi błąd kompilacji.

Sekwencje znaków Unicode ([sekwencje ucieczki znaków Unicode](lexical-structure.md#unicode-character-escape-sequences)) w literale znakowym musi należeć do zakresu `U+0000` do `U+FFFF`.

Proste sekwencje reprezentuje kodowania znaków Unicode, zgodnie z opisem w poniższej tabeli.


| __Sekwencja unikowa__ | __Nazwa znaków__ | __Kodowanie Unicode__ |
|---------------------|--------------------|----------------------|
| `\'`                | pojedynczy cudzysłów       | `0x0027`             | 
| `\"`                | podwójny cudzysłów       | `0x0022`             | 
| `\\`                | Ukośnik odwrotny          | `0x005C`             | 
| `\0`                | Null               | `0x0000`             | 
| `\a`                | Zgłoś alert              | `0x0007`             | 
| `\b`                | Backspace          | `0x0008`             | 
| `\f`                | Wysuw strony          | `0x000C`             | 
| `\n`                | Nowy wiersz           | `0x000A`             | 
| `\r`                | Powrót karetki    | `0x000D`             | 
| `\t`                | tabulator poziomy     | `0x0009`             | 
| `\v`                | tabulator pionowy       | `0x000B`             | 

Typ *character_literal* jest `char`.

#### <a name="string-literals"></a>Literały ciągu

C# obsługuje dwa rodzaje literałów ciągów: ***literałów ciągów regularne*** i ***literały ciąg verbatim***.

Literał ciągu regularne składa się z zero lub więcej znaków ujęte w cudzysłów, podobnie jak w `"hello"`i mogą obejmować zarówno proste sekwencje ucieczki (takie jak `\t` na znak tabulacji) i szesnastkowym, jak i sekwencje unikowe Unicode.

Literał ciągu verbatim składa się z `@` znaków, po której następuje znak podwójnego cudzysłowu, zero lub więcej znaków i znaku podwójnego cudzysłowu zamykającego. Prosty przykład `@"hello"`. Literał ciągu verbatim, znaków między ogranicznikami interpretuje dosłownie, tylko on wyjątek *quote_escape_sequence*. W szczególności proste sekwencje ucieczki i szesnastkowym i sekwencje unikowe Unicode nie są przetwarzane w literałach ciąg verbatim. Literał ciągu verbatim może obejmować wiele wierszy.

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

Znak, który następuje znak ukośnika odwrotnego (`\`) w *regular_string_literal_character* musi mieć jedną z następujących znaków: `'`, `"`, `\`, `0`, `a` , `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. W przeciwnym razie wystąpi błąd kompilacji.

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
przedstawia różne literały ciągu. Ostatnim ciągiem literału, `j`, jest verbatim literału ciągu obejmującej wiele wierszy. Znaki między znakami cudzysłowu, w tym znak odstępu, takie jak znaki nowego wiersza są zachowywane verbatim.

Ponieważ szesnastkowa sekwencja unikowa mogą mieć różną liczbą znaków szesnastkowych, literał ciągu `"\x123"` zawiera pojedynczy znak z wartości szesnastkowej 123. Aby utworzyć ciąg zawierający znak wartości szesnastkowych 12 następuje znak 3, jeden napisać `"\x00123"` lub `"\x12" + "3"` zamiast tego.

Typ *string_literal* jest `string`.

Literał ciągu nie zawsze skutkuje nowego wystąpienia ciągu. Po co najmniej dwóch Literały ciągu, są równoważne zgodnie z operatora równości ciągu ([operatory porównania ciągów](expressions.md#string-equality-operators)) są wyświetlane w tym samym programie te literałów ciągów odwołują się do tego samego wystąpienia ciągu. Na przykład dane wyjściowe wytwarzane przez
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
jest `True` ponieważ dwa literały odwołują się do tego samego wystąpienia ciągu.

#### <a name="interpolated-string-literals"></a>Literały ciągu interpolowanego

Literały ciągów znaków interpolowanych są podobne do Literały ciągu, ale zawiera luki rozdzielone `{` i `}`, którym określeń mogą wystąpić. W czasie wykonywania są obliczane wyrażenia, mający na celu ułatwienie o swoje formularze tekstową zamieniony na ciąg w miejscu, w którym występuje dziura. Składnia i semantyka Interpolacja ciągów, które są opisane w sekcji ([ciągi interpolowane](expressions.md#interpolated-strings)).

Jak literałów ciągów literałów ciągu interpolowanego może być regularnie lub verbatim. Literały ciągu interpolowanego regularnych są rozdzielane znakami `$"` i `"`, i literały ciągu interpolowanego verbatim są rozdzielane znakami `$@"` i `"`.

Podobnie jak inne literały poddawać analizie leksykalnej literału ciągu interpolowanego początkowo powoduje pojedynczy token, zgodnie z poniższym gramatyki. Jednak przed analizą składni pojedynczy token literału ciągu interpolowanego jest dzielony na kilka tokenów dla części ciągu otaczający otwory i elementów wejściowych pojawiają się w otwory leksykalnie analizuje się ponownie. Z kolei może to zwrócić więcej interpolowane literałów ciągów do przetworzenia, ale, jeśli leksykalnie rozwiązać, ostatecznie doprowadzi do sekwencja tokeny składni analizy do przetwarzania.

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

*Interpolated_string_literal* tokenu to ponowne interpretowanie jako wiele tokenów i inne dane wejściowe elementów w następujący sposób, w kolejności ich występowania w *interpolated_string_literal*:

* Wystąpienia następujących są reinterpretowane jako osobne oddzielne tokeny: wiodące `$` logowania, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*,  *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* i *interpolated_verbatim_string_end*.
* Wystąpienia *regular_balanced_text* i *verbatim_balanced_text* między tymi są ponownie przetwarzany jako *input_section* ([poddawać analizie Leksykalnej ](lexical-structure.md#lexical-analysis)) i są reinterpretowane jako wynikowa sekwencja elementów wejściowych. Z kolei mogą one obejmować tokenów literałów ciągu interpolowanego ponowne interpretowanie.

Analiza składni zostanie następnie tokenów do *interpolated_string_expression* ([ciągi interpolowane](expressions.md#interpolated-strings)).

Przykłady zadań do wykonania


#### <a name="the-null-literal"></a>Literał null

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal* mogą być niejawnie konwertowane na typ referencyjny lub typ dopuszczający wartość null.

### <a name="operators-and-punctuators"></a>Operatory i przerywniki języka

Istnieje kilka rodzajów operatory i przerywniki języka. Operatory są używane w wyrażeniach opisujący operacje dotyczące jednego lub większej liczbie operandów. Na przykład, wyrażenie `a + b` używa `+` operatora, aby dodać dwa operandy `a` i `b`. Przerywniki języka służą do grupowania i oddzielając.

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

Pionowy pasek w *right_shift* i *right_shift_assignment* produkcji są używane do wskazania, że, w przeciwieństwie do innych produkcji w składni gramatykę, żadne znaki dowolnego rodzaju (nawet białych znaków) są dozwolone między tokenami. Zaprojektuj te są traktowane specjalnie w celu umożliwienia obsługi poprawne *type_parameter_list*s ([parametry typu](classes.md#type-parameters)).

## <a name="pre-processing-directives"></a>Dyrektywy przetwarzania wstępnego

Dyrektywy przetwarzania wstępnego zapewniają możliwość warunkowo pominąć części plików źródłowych, zgłoszenia błędu i warunków ostrzeżenia i odróżniać odrębne regiony kodu źródłowego. Termin "przetwarzania wstępnego dyrektywy" jest używana tylko w celu zachowania spójności z językami programowania C i C++. W języku C# jest nie niezależnym przetwarzania wstępnego; dyrektywy przetwarzania wstępnego są przetwarzane w ramach fazy poddawać analizie leksykalnej.

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

Dostępne są następujące dyrektywy przetwarzania wstępnego:

*  `#define` i `#undef`, które są używane do definiowania i Usuń odpowiednio symbole kompilacji warunkowej ([dyrektywy deklaracji](lexical-structure.md#declaration-directives)).
*  `#if`, `#elif`, `#else`, i `#endif`, które są używane do warunkowo pominąć części kodu źródłowego ([dyrektywy kompilacji warunkowej](lexical-structure.md#conditional-compilation-directives)).
*  `#line`, które jest używane do kontrolowania numery wierszy emitowane błędów i ostrzeżeń ([wiersz dyrektywy](lexical-structure.md#line-directives)).
*  `#error` i `#warning`, które są używane do wysyłania błędów i ostrzeżeń, odpowiednio ([diagnostycznych dyrektywy](lexical-structure.md#diagnostic-directives)).
*  `#region` i `#endregion`, które są używane do wyraźnie oznaczyć części kodu źródłowego ([dyrektywy regionu](lexical-structure.md#region-directives)).
*  `#pragma`, używany do określenia opcjonalne informacje kontekstowe w kompilatorze ([dyrektyw Pragma](lexical-structure.md#pragma-directives)).

Dyrektywy przetwarzania wstępnego zawsze zajmuje w osobnym wierszu kodu źródłowego i zawsze zaczyna się od `#` znak i nazwę dyrektywy przetwarzania wstępnego. Biały znak może występować przed `#` znaków oraz między `#` znaków i nazwa dyrektywy.

Źródło zawierające wiersza `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, lub `#endregion` dyrektywy może kończyć się znakiem jednowierszowego komentarza. Lista komentarzy ( `/* */` styl komentarzy) nie są dozwolone w wierszach źródłowych zawierających dyrektywy przetwarzania wstępnego.

Dyrektywy przetwarzania wstępnego nie są tokenów i nie są częścią składni gramatyki języka C#. Jednak dyrektywy przetwarzania wstępnego może służyć do dołączania lub wykluczania sekwencje tokenów i w ten sposób wpływają na znaczenie programu w języku C#. Na przykład, gdy kompilowany, program:
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
wyniki w dokładnie takiej samej kolejności tokenów jako programu:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

W efekcie leksykalnie, dwa programy są zupełnie inny syntaktycznie, są identyczne.

### <a name="conditional-compilation-symbols"></a>Symbole kompilacji warunkowej

Kompilacja warunkowa funkcjonalność `#if`, `#elif`, `#else`, i `#endif` dyrektyw jest kontrolowany za pośrednictwem przetwarzania wstępnego wyrażenia ([przetwarzania wstępnego wyrażeń](lexical-structure.md#pre-processing-expressions)) i symbole kompilacji warunkowej.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Kompilacja warunkowa symbolu ma dwa możliwe stany: ***zdefiniowane*** lub ***niezdefiniowane***. Na początku leksykalne przetworzenia pliku źródłowego, symbol kompilacja warunkowa jest niezdefiniowana, chyba że ma je jawnie zdefiniowany przez mechanizm zewnętrznych (takich jak opcja kompilatora wiersza polecenia). Gdy `#define` dyrektywa jest przetwarzany, symbol kompilacji warunkowej, o nazwie w tej dyrektywy staje się zdefiniowany w tym pliku źródłowym. Symbol pozostaje zdefiniowany aż `#undef` dyrektywy dla tego samego symbolu jest przetwarzana lub dopóki nie zostanie osiągnięty koniec pliku źródłowego. Domniemanie tego jest fakt, że `#define` i `#undef` dyrektywy w jednym pliku źródłowym nie mają wpływu na innych plikach źródłowych, w tym samym programie.

Gdy do którego odwołuje się wyrażenie przetwarzania wstępnego, symboli zdefiniowanych kompilacji warunkowej ma wartość logiczna `true`, a symbol Niezdefiniowany kompilacji warunkowej ma wartość logiczna `false`. Nie jest wymagane, czy symbole kompilacji warunkowej jest jawnie zadeklarowana przed występuje do nich podczas wstępnego przetwarzania wyrażenia. Zamiast tego należy niezadeklarowany symbole są po prostu niezdefiniowane i dlatego mają wartość `false`.

Przestrzeń nazw dla symbole kompilacji warunkowej jest odrębna i oddzielona od innych jednostek o nazwie w programie C#. Symbole kompilacji warunkowej mogą być przywoływane tylko w `#define` i `#undef` dyrektywy w przetwarzania wstępnego wyrażeń.

### <a name="pre-processing-expressions"></a>Przetwarzanie wstępne wyrażeń

Przetwarzanie wstępne wyrażeń może wystąpić w `#if` i `#elif` dyrektywy. Operatory `!`, `==`, `!=`, `&&` i `||` są dozwolone w przetwarzania wstępnego wyrażeń, i nawiasy mogą być używane do grupowania.

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

Gdy do którego odwołuje się wyrażenie przetwarzania wstępnego, symboli zdefiniowanych kompilacji warunkowej ma wartość logiczna `true`, a symbol Niezdefiniowany kompilacji warunkowej ma wartość logiczna `false`.

Obliczanie wyrażenia przetwarzania wstępnego zawsze daje wartość logiczną. Reguły oceny wyrażenia przetwarzania wstępnego są takie same jak w przypadku stałego wyrażenia ([wyrażeń stałych](expressions.md#constant-expressions)), z tą różnicą, że tylko obiekty zdefiniowane przez użytkownika, które mogą być przywoływane są symbole kompilacji warunkowej .

### <a name="declaration-directives"></a>Dyrektywy deklaracji

Dyrektywy deklaracji są używane do definiowania lub Usuń symbole kompilacji warunkowej.

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

Przetwarzanie `#define` dyrektywy powoduje, że symbol danej kompilacji warunkowej, aby stać się zdefiniowana, począwszy od wiersza źródłowego, który następuje po dyrektywie. Podobnie, przetwarzanie `#undef` dyrektywy powoduje, że stają się niezdefiniowane, zaczynając od wiersza źródłowego, który następuje po dyrektywie symbol danej kompilacji warunkowej.

Wszelkie `#define` i `#undef` dyrektywy w pliku źródłowym musi wystąpić przed pierwszym *tokenu* ([tokenów](lexical-structure.md#tokens)) w pliku źródłowym; w przeciwnym razie błąd kompilacji występuje. W sposób intuicyjny `#define` i `#undef` dyrektywy musi poprzedzać "rzeczywistego kodu" w pliku źródłowym.

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
jest nieprawidłowy ponieważ `#define` dyrektyw, poprzedź pierwszy token ( `namespace` — słowo kluczowe) w pliku źródłowym.

Poniższy przykład powoduje błąd kompilacji, ponieważ `#define` następuje rzeczywistego kodu:
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

A `#define` może zdefiniować symbol kompilacji warunkowej, który jest już zdefiniowany, bez żadnych aktywne `#undef` dla tego symbolu. Poniższy przykład definiuje symbol kompilacji warunkowej `A` i ponownie go definiuje.
```csharp
#define A
#define A
```

Element `#undef` może "Usuń" symbol kompilacji warunkowej, który nie został zdefiniowany. Poniższy przykład definiuje symbol kompilacji warunkowej `A` i następnie definicji do usunięcia go dwukrotnie; mimo że drugi `#undef` nie ma wpływu, jest nadal ważny.
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>Dyrektywy kompilacji warunkowej

Dyrektywy kompilacji warunkowej są używane do warunkowo dołączania lub wykluczania części pliku źródłowego.

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

Wskazane przez składnię dyrektywy kompilacji warunkowej musi być napisana jako zestawy składające się z, w kolejności, `#if` dyrektywy, zero lub więcej `#elif` dyrektyw, zero lub jeden `#else` dyrektywy i `#endif` dyrektywy. Dyrektywy mogą sekcje warunkowe kodu źródłowego. Każda sekcja jest kontrolowana przez dyrektywy bezpośrednio poprzedzającego. Sekcja warunkowa może sam zawierać dyrektywy kompilacji warunkowej zagnieżdżonych pod warunkiem dyrektywy te tworzą pełną zestawów.

A *pp_conditional* wybiera co najwyżej jeden zamkniętego *conditional_section*pod kątem normalne przetwarzanie leksykalne:

*  *Pp_expression*s `#if` i `#elif` dyrektywy są obliczane w kolejności, aż jedną daje `true`. Jeśli wyrażenie zwraca `true`, *conditional_section* odpowiedniego dyrektywy jest zaznaczone.
*  Jeśli wszystkie *pp_expression*s yield `false`i jeśli `#else` występuje dyrektywie *conditional_section* z `#else` dyrektywa jest zaznaczone.
*  W przeciwnym razie nie *conditional_section* jest zaznaczone.

Wybrane *conditional_section*, jeśli istnieje, jest przetwarzany jako normalny *input_section*: gramatyka leksykalna kodu źródłowego, znajdujących się w sekcji muszą być zgodne; tokeny są generowane na podstawie źródła kod w sekcji. i dyrektywy przetwarzania wstępnego, w sekcji realizowania efekty.

Pozostałe *conditional_section*s, jeśli istnieje, są przetwarzane jako *skipped_section*s: z wyjątkiem dyrektywy przetwarzania wstępnego, kod źródłowy w sekcji potrzebne jest zgodna leksykalne gramatyki; nie tokeny są generowane na podstawie kodu źródłowego w sekcji. i dyrektywy przetwarzania wstępnego, w sekcji musi być poprawna, leksykalnie, ale nie są przetwarzane w przeciwnym razie. W ramach *conditional_section* , jest przetwarzana jako *skipped_section*, wszelkie zagnieżdżone *conditional_section*s (zawarty w zagnieżdżone `#if`... `#endif` i `#region`... `#endregion` tworzy) również są przetwarzane jako *skipped_section*s.

Poniższy przykład ilustruje jak kompilacja warunkowa dyrektywy można zagnieżdżać:
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

Z wyjątkiem dyrektywy przetwarzania wstępnego, pominięto kod źródłowy jest poddawać analizie leksykalnej. Na przykład poniżej przedstawiono prawidłowe pomimo niezakończony komentarz w `#else` sekcji:
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

Należy jednak pamiętać, że dyrektywy przetwarzania wstępnego są wymagane było leksykalnie poprawne nawet w przypadku pominiętych części kodu źródłowego.

Dyrektywy przetwarzania wstępnego nie są przetwarzane, gdy są one wyświetlane w wielowierszowym elementów wejściowych. Na przykład program:
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

W szczególnych przypadkach zbiór dyrektywy przetwarzania wstępnego, który jest przetwarzany może zależeć od oceny *pp_expression*. Przykład:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
zawsze daje ten sam token strumienia (`class` `Q` `{` `}`), niezależnie od tego, czy `X` jest zdefiniowana. Jeśli `X` jest zdefiniowany, tylko dyrektywy przetwarzania są `#if` i `#endif`ze względu na komentarz wielowierszowy. Jeśli `X` jest niezdefiniowana, następnie trzy dyrektywy (`#if`, `#else`, `#endif`) są dostępne w ramach zestawu dyrektywy.

### <a name="diagnostic-directives"></a>Dyrektywy diagnostyczne

Dyrektywy diagnostycznych są używane do jawnie generować błędów i komunikaty ostrzegawcze, które są zgłaszane w taki sam sposób jak inne błędy kompilacji i ostrzeżenia.

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
zawsze generuje ostrzeżenie ("Przegląd kodu potrzebne przed zaewidencjonowaniem") i generuje błąd w czasie kompilacji ("kompilacji nie może być zarówno debugowania, jak i handlu detalicznego") Jeżeli symbole warunkowego `Debug` i `Retail` są zdefiniowane. Należy pamiętać, że *pp_message* może zawierać dowolny tekst; ściślej mówiąc, go nie muszą zawierać tokeny poprawnie sformułowany, jak to przedstawiono w pojedynczy cudzysłów wyraz `can't`.

### <a name="region-directives"></a>Dyrektywy regionu

Dyrektywy regionu są używane do wyraźnie oznaczyć regiony kodu źródłowego.

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

Nie znaczenia semantycznego jest dołączony do regionu; regiony są przeznaczone do użytku przez programistę lub zautomatyzowanych narzędzi do oznaczania sekcję kodu źródłowego. Komunikat określony w `#region` lub `#endregion` dyrektywy również nie ma znaczenia semantycznego; służy ona jedynie do identyfikowania regionu. Dopasowywanie `#region` i `#endregion` dyrektywy może mieć różne *pp_message*s.

Leksykalne przetwarzanie regionu:
```csharp
#region
...
#endregion
```
dokładnie odpowiada leksykalne przetwarzania dyrektywy kompilacji warunkowej w postaci:
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Dyrektywy line

Dyrektywy line można zmienić numery wierszy i nazw plików źródłowych, które są zgłaszane przez kompilator w danych wyjściowych, takie jak ostrzeżenia i błędy, a które są używane przez caller — atrybuty informacji ([Caller — atrybuty informacji](attributes.md#caller-info-attributes)).

Dyrektywy line są najczęściej używane narzędzia meta-programowania, które generuje kod źródłowy języka C# z innym tekstem danych wejściowych.

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

Gdy nie `#line` dyrektyw, kompilator raporty numery wierszy true i nazw plików źródłowych w danych wyjściowych. Podczas przetwarzania `#line` dyrektywę, który zawiera *line_indicator* , który nie jest `default`, kompilator traktuje wiersza po dyrektywie jako posiadające podany numer wiersza (i nazwę pliku, jeśli określono).

A `#line default` dyrektywy cofa efekt wszystkich poprzednich dyrektyw #line. Kompilator zgłasza true wiersza informacji dla kolejnych wierszy, dokładnie tak, jakby nie `#line` przetworzył dyrektywy.

A `#line hidden` dyrektywy nie ma wpływu na plik i numery wierszy zgłosił błąd komunikaty, ale wpływa na debugowanie na poziomie źródła. Podczas debugowania, wszystkie linie między `#line hidden` dyrektywy i kolejne `#line` — dyrektywa (nie jest to `#line hidden`) mają nie informacja o numerach wierszy. Podczas krokowego wykonywania kodu w debugerze, wiersze te zostaną pominięte całkowicie.

Należy pamiętać, że *nazwa_pliku* różni się od regularnych literału ciągu, w tym znaki ucieczki nie są przetwarzane; "`\`" znak po prostu wyznacza zwykłych ukośnika w ramach *nazwa_pliku*.

### <a name="pragma-directives"></a>Pragma — dyrektywy

`#pragma` Dyrektywy preprocesora jest używany do określenia opcjonalne informacje kontekstowe do kompilatora. Informacje dostarczone w `#pragma` dyrektywy nigdy nie zmienia semantykę program.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C# zawiera `#pragma` dyrektywy do kontrolowania ostrzeżeń kompilatora. Przyszłe wersje języka może zawierać dodatkowe `#pragma` dyrektywy. Aby zapewnić współdziałanie za pomocą innych kompilatorów języka C#, kompilator Microsoft C# nie generuje błędy kompilacji nieznany `#pragma` dyrektywy; takie nie dyrektyw, jednak generowanie ostrzeżenia.

#### <a name="pragma-warning"></a>Warning elementu pragma

`#pragma warning` Dyrektywy służy do wyłączenia lub przywrócenia wszystkich lub określonego zestawu ostrzeżenie komunikatów podczas kompilacji i kolejne tekstu.

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

A `#pragma warning` dyrektywy, które pomija listy ostrzeżenie ma wpływ na wszystkie ostrzeżenia. A `#pragma warning` dyrektywy zawierają listę ostrzeżenie dotyczy tylko tych ostrzeżeń, które są określone na liście.

A `#pragma warning disable` dyrektywy wyłącza wszystkie lub danego zestawu ostrzeżeń.

A `#pragma warning restore` dyrektywy przywraca wszystkie lub danego zestawu ostrzeżeń do stanu który obowiązywały na początku jednostki kompilacji. Należy pamiętać, że jeśli określonego ostrzeżenie zostało wyłączone z zewnątrz, `#pragma warning restore` (czy dla wszystkich lub określonych ostrzeżeń) nie będzie ponownie włączyć tego ostrzeżenia.

W poniższym przykładzie pokazano użycie `#pragma warning` tymczasowo wyłączyć ostrzeżenia zgłoszone podczas zamieniono przestarzały parametr składowych są wywoływane za pomocą numeru ostrzeżenia kompilatora Microsoft C#.
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
