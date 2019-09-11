---
ms.openlocfilehash: 5fbe0267b5b33b1a24dbdca493d118c576092573
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876913"
---
# <a name="lexical-structure"></a><span data-ttu-id="ebb69-101">Struktura leksykalna</span><span class="sxs-lookup"><span data-stu-id="ebb69-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="ebb69-102">Programy</span><span class="sxs-lookup"><span data-stu-id="ebb69-102">Programs</span></span>

<span data-ttu-id="ebb69-103">C# ***Program*** składa się z co najmniej jednego ***pliku źródłowego***, znanego formalnie jako ***jednostki kompilacji*** ([jednostki kompilacji](namespaces.md#compilation-units)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="ebb69-104">Plik źródłowy jest uporządkowaną sekwencją znaków Unicode.</span><span class="sxs-lookup"><span data-stu-id="ebb69-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="ebb69-105">Pliki źródłowe zwykle mają zgodność jeden-do-jednego z plikami w systemie plików, ale ta korespondencja nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="ebb69-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="ebb69-106">W celu uzyskania maksymalnej przenośności zaleca się, aby pliki w systemie plików były kodowane przy użyciu kodowania UTF-8.</span><span class="sxs-lookup"><span data-stu-id="ebb69-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="ebb69-107">Koncepcyjnie mówiąc, program jest kompilowany przy użyciu trzech kroków:</span><span class="sxs-lookup"><span data-stu-id="ebb69-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="ebb69-108">Transformacja, która konwertuje plik z określonego znaku repertuar i schematu kodowania na sekwencję znaków Unicode.</span><span class="sxs-lookup"><span data-stu-id="ebb69-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="ebb69-109">Analiza leksykalna, która tłumaczy strumień znaków wejściowych Unicode na strumień tokenów.</span><span class="sxs-lookup"><span data-stu-id="ebb69-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="ebb69-110">Analiza składni, która tłumaczy strumień tokenów na kod wykonywalny.</span><span class="sxs-lookup"><span data-stu-id="ebb69-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="ebb69-111">Gramatykach</span><span class="sxs-lookup"><span data-stu-id="ebb69-111">Grammars</span></span>

<span data-ttu-id="ebb69-112">Ta specyfikacja przedstawia składnię języka C# programowania przy użyciu dwóch gramatyki.</span><span class="sxs-lookup"><span data-stu-id="ebb69-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="ebb69-113">***Gramatyka leksykalna*** ([Gramatyka leksykalna](lexical-structure.md#lexical-grammar)) definiuje, w jaki sposób znaki Unicode są łączone z terminatorami wierszy formularza, białym znakiem, komentarzem, tokenami i dyrektywami wstępnego przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="ebb69-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="ebb69-114">***Gramatyka składni*** ([Gramatyka składni](lexical-structure.md#syntactic-grammar)) definiuje, jak tokeny pochodzące z gramatyki leksykalnej są C# łączone z programami formularzy.</span><span class="sxs-lookup"><span data-stu-id="ebb69-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="ebb69-115">Notacja gramatyki</span><span class="sxs-lookup"><span data-stu-id="ebb69-115">Grammar notation</span></span>

<span data-ttu-id="ebb69-116">Gramatyki leksykalne i syntaktyczne są prezentowane w formularzu back-Naura za pomocą notacji narzędzia gramatyki ANTLR.</span><span class="sxs-lookup"><span data-stu-id="ebb69-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="ebb69-117">Gramatyka leksykalna</span><span class="sxs-lookup"><span data-stu-id="ebb69-117">Lexical grammar</span></span>

<span data-ttu-id="ebb69-118">Gramatyka leksykalna C# jest przedstawiana w ramach [analizy leksykalnej](lexical-structure.md#lexical-analysis), [tokenów](lexical-structure.md#tokens)i [dyrektyw wstępnego przetwarzania](lexical-structure.md#pre-processing-directives).</span><span class="sxs-lookup"><span data-stu-id="ebb69-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="ebb69-119">Symbole terminalu leksykalnej gramatycznej są znakami zestawu znaków Unicode, a Gramatyka leksykalna określa, jak znaki są łączone z tokenami formularza ([tokenami](lexical-structure.md#tokens)), białym znakiem ([białym znakiem](lexical-structure.md#white-space)), komentarzami ([komentarzami](lexical-structure.md#comments)) i dyrektywy wstępnego przetwarzania ([dyrektywy wstępnego przetwarzania](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="ebb69-120">Każdy plik źródłowy w C# programie musi być zgodny z produkcją *wejściową* gramatyki leksykalnej ([Analiza leksykalna](lexical-structure.md#lexical-analysis)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="ebb69-121">Gramatyka składni</span><span class="sxs-lookup"><span data-stu-id="ebb69-121">Syntactic grammar</span></span>

<span data-ttu-id="ebb69-122">Składnia gramatyki C# jest przedstawiona w działach i dodatkach, które obserwują ten rozdział.</span><span class="sxs-lookup"><span data-stu-id="ebb69-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="ebb69-123">Symbole terminalu składni gramatycznej są tokenami zdefiniowanymi przez gramatykę leksykalną, a Gramatyka składni określa, jak tokeny są łączone C# z programami formularzy.</span><span class="sxs-lookup"><span data-stu-id="ebb69-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="ebb69-124">Każdy plik źródłowy w C# programie musi być zgodny z produkcją *compilation_unitą* gramatyki składniowej ([jednostki kompilacji](namespaces.md#compilation-units)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="ebb69-125">Analiza leksykalna</span><span class="sxs-lookup"><span data-stu-id="ebb69-125">Lexical analysis</span></span>

<span data-ttu-id="ebb69-126">Produkcja *wejściowa* definiuje strukturę leksykalną pliku C# źródłowego.</span><span class="sxs-lookup"><span data-stu-id="ebb69-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="ebb69-127">Każdy plik źródłowy w C# programie musi być zgodny z tą leksykalną produkcją gramatyki.</span><span class="sxs-lookup"><span data-stu-id="ebb69-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

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

<span data-ttu-id="ebb69-128">Pięć podstawowych elementów tworzą strukturę leksykalną pliku C# źródłowego: Terminatory wiersza ([terminatory wierszy](lexical-structure.md#line-terminators)), biały znak ([biały znak](lexical-structure.md#white-space)), Komentarze ([Komentarze](lexical-structure.md#comments)), tokeny ([tokeny](lexical-structure.md#tokens)) i dyrektywy wstępnego przetwarzania ([dyrektywy wstępnego przetwarzania](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="ebb69-129">W przypadku tych podstawowych elementów tylko tokeny są znaczące w gramatyce składni C# programu ([gramatyki](lexical-structure.md#syntactic-grammar)składniowej).</span><span class="sxs-lookup"><span data-stu-id="ebb69-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="ebb69-130">Przetwarzanie leksykalne pliku C# źródłowego składa się z zmniejszenia pliku do sekwencji tokenów, które staną się danymi wejściowymi analizy składni.</span><span class="sxs-lookup"><span data-stu-id="ebb69-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="ebb69-131">Terminatory wierszy, biały znak i komentarze mogą służyć do oddzielania tokenów, a dyrektywy wstępnego przetwarzania mogą spowodować pominięcie sekcji w pliku źródłowym, ale w przeciwnym razie te elementy leksykalne nie mają wpływu na strukturę składni C# programu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="ebb69-132">W przypadku interpolowanych literałów ciągu ([interpolowane literały ciągu](lexical-structure.md#interpolated-string-literals)) pojedynczy token jest początkowo tworzony przez analizę leksykalną, ale jest podzielony na kilka elementów wejściowych, które są wielokrotnie poddawane analizie leksykalnej do momentu, gdy wszystkie interpolowane Literały ciągu zostały rozwiązane.</span><span class="sxs-lookup"><span data-stu-id="ebb69-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="ebb69-133">Otrzymane tokeny następnie stanowią dane wejściowe do analizy składni.</span><span class="sxs-lookup"><span data-stu-id="ebb69-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="ebb69-134">Gdy kilka leksykalnych produkcji gramatyki pasuje do sekwencji znaków w pliku źródłowym, przetwarzanie leksykalne zawsze tworzy najdłuższy możliwy element leksykalny.</span><span class="sxs-lookup"><span data-stu-id="ebb69-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="ebb69-135">Na przykład sekwencja `//` znaków jest przetwarzana jako początek jednowierszowego komentarza, ponieważ ten element leksykalny jest dłuższy niż pojedynczy `/` token.</span><span class="sxs-lookup"><span data-stu-id="ebb69-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="ebb69-136">Terminatory wiersza</span><span class="sxs-lookup"><span data-stu-id="ebb69-136">Line terminators</span></span>

<span data-ttu-id="ebb69-137">Terminatory wierszy dzielą znaki pliku C# źródłowego na wiersze.</span><span class="sxs-lookup"><span data-stu-id="ebb69-137">Line terminators divide the characters of a C# source file into lines.</span></span>

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

<span data-ttu-id="ebb69-138">W celu zapewnienia zgodności z narzędziami do edycji kodu źródłowego, które dodają znaczniki końca pliku i umożliwiają wyświetlanie pliku źródłowego jako sekwencji prawidłowo zakończonych wierszy, do każdego pliku źródłowego w C# programie są stosowane następujące przekształcenia:</span><span class="sxs-lookup"><span data-stu-id="ebb69-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="ebb69-139">Jeśli ostatni znak pliku źródłowego jest znakiem Control-Z (`U+001A`), ten znak jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="ebb69-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="ebb69-140">Znak powrotu karetki (`U+000D`) jest dodawany na końcu pliku źródłowego, jeśli ten plik źródłowy nie jest pusty i jeśli ostatni znak w pliku źródłowym nie jest Return karetki (`U+000D`)`U+000A`, wierszem wysuwu wiersza (), separatorem wiersza (`U+2028`) lub separator akapitu (`U+2029`).</span><span class="sxs-lookup"><span data-stu-id="ebb69-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="ebb69-141">Komentarze</span><span class="sxs-lookup"><span data-stu-id="ebb69-141">Comments</span></span>

<span data-ttu-id="ebb69-142">Obsługiwane są dwie formy komentarzy: Komentarze jednowierszowe i rozdzielane Komentarze.</span><span class="sxs-lookup"><span data-stu-id="ebb69-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="ebb69-143">***Komentarze jednowierszowe*** zaczynają się od `//` znaków i zwiększają się do końca wiersza źródłowego.</span><span class="sxs-lookup"><span data-stu-id="ebb69-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="ebb69-144">***Rozdzielane Komentarze*** zaczynają się od `/*` znaków i kończą się znakami. `*/`</span><span class="sxs-lookup"><span data-stu-id="ebb69-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="ebb69-145">Rozdzielane komentarze mogą obejmować wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="ebb69-145">Delimited comments may span multiple lines.</span></span>

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

<span data-ttu-id="ebb69-146">Komentarze nie są zagnieżdżane.</span><span class="sxs-lookup"><span data-stu-id="ebb69-146">Comments do not nest.</span></span> <span data-ttu-id="ebb69-147">Sekwencje `/*` znaków i `*/` `//` `/*` nie mają specjalnego znaczenia w komentarzuisekwencjeznakówiniemająspecjalnychznaczeniawkomentarzuograniczonym.`//`</span><span class="sxs-lookup"><span data-stu-id="ebb69-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="ebb69-148">Komentarze nie są przetwarzane w literałach znaków i ciągów.</span><span class="sxs-lookup"><span data-stu-id="ebb69-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="ebb69-149">Przykład</span><span class="sxs-lookup"><span data-stu-id="ebb69-149">The example</span></span>
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
<span data-ttu-id="ebb69-150">zawiera rozdzielany komentarz.</span><span class="sxs-lookup"><span data-stu-id="ebb69-150">includes a delimited comment.</span></span>

<span data-ttu-id="ebb69-151">Przykład</span><span class="sxs-lookup"><span data-stu-id="ebb69-151">The example</span></span>
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
<span data-ttu-id="ebb69-152">pokazuje kilka komentarzy jednowierszowych.</span><span class="sxs-lookup"><span data-stu-id="ebb69-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="ebb69-153">Biały znak</span><span class="sxs-lookup"><span data-stu-id="ebb69-153">White space</span></span>

<span data-ttu-id="ebb69-154">Spacja jest definiowana jako dowolny znak z klasą Unicode ZS (która zawiera znak spacji), a także znak tabulacji poziomej, znak tabulacji pionowej i znak wysuwu strony.</span><span class="sxs-lookup"><span data-stu-id="ebb69-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="ebb69-155">Tokeny</span><span class="sxs-lookup"><span data-stu-id="ebb69-155">Tokens</span></span>

<span data-ttu-id="ebb69-156">Istnieje kilka rodzajów tokenów: identyfikatory, słowa kluczowe, literały, operatory i przerywniki.</span><span class="sxs-lookup"><span data-stu-id="ebb69-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="ebb69-157">Biały znak i komentarze nie są tokenami, chociaż działają jako separatory dla tokenów.</span><span class="sxs-lookup"><span data-stu-id="ebb69-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

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

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="ebb69-158">Sekwencje ucieczki znaków Unicode</span><span class="sxs-lookup"><span data-stu-id="ebb69-158">Unicode character escape sequences</span></span>

<span data-ttu-id="ebb69-159">Sekwencja ucieczki znaków Unicode reprezentuje znak Unicode.</span><span class="sxs-lookup"><span data-stu-id="ebb69-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="ebb69-160">Sekwencje ucieczki znaków Unicode są przetwarzane w identyfikatorach ([identyfikatorach](lexical-structure.md#identifiers)), literałach znaków ([literałach znaków](lexical-structure.md#character-literals)) i zwykłych literałach ciągów ([literały ciągów](lexical-structure.md#string-literals)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="ebb69-161">Znak ucieczki Unicode nie jest przetwarzany w żadnej innej lokalizacji (na przykład w celu utworzenia operatora, punctuator lub słowa kluczowego).</span><span class="sxs-lookup"><span data-stu-id="ebb69-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="ebb69-162">Sekwencja unikowa Unicode reprezentuje pojedynczy znak Unicode utworzony przez liczbę szesnastkową po znakach "`\u`" lub "`\U`".</span><span class="sxs-lookup"><span data-stu-id="ebb69-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="ebb69-163">Ponieważ C# program korzysta z 16-bitowego kodowania punktów kodowych Unicode w znakach i wartościach ciągu, znak Unicode w zakresie u + 10000 do u + 10FFFF jest niedozwolony w literale znakowym i jest reprezentowany przy użyciu pary wieloskładnikowej Unicode w literale ciągu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="ebb69-164">Znaki Unicode z punktami kodu powyżej 0x10FFFF nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="ebb69-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="ebb69-165">Nie wykonano wielu tłumaczeń.</span><span class="sxs-lookup"><span data-stu-id="ebb69-165">Multiple translations are not performed.</span></span> <span data-ttu-id="ebb69-166">Na przykład literał ciągu "`\u005Cu005C`" jest odpowiednikiem "`\u005C`" zamiast "`\`".</span><span class="sxs-lookup"><span data-stu-id="ebb69-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="ebb69-167">Wartość `\u005C` Unicode jest znakiem "`\`".</span><span class="sxs-lookup"><span data-stu-id="ebb69-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="ebb69-168">Przykład</span><span class="sxs-lookup"><span data-stu-id="ebb69-168">The example</span></span>
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
<span data-ttu-id="ebb69-169">pokazuje kilka zastosowania `\u0066`, czyli sekwencję ucieczki dla litery "`f`".</span><span class="sxs-lookup"><span data-stu-id="ebb69-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="ebb69-170">Program jest równoważny</span><span class="sxs-lookup"><span data-stu-id="ebb69-170">The program is equivalent to</span></span>
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

### <a name="identifiers"></a><span data-ttu-id="ebb69-171">Identyfikatory</span><span class="sxs-lookup"><span data-stu-id="ebb69-171">Identifiers</span></span>

<span data-ttu-id="ebb69-172">Reguły dotyczące identyfikatorów podane w tej sekcji odpowiadają dokładnie wymaganiom zalecanym przez standard Unicode załącznika 31, z wyjątkiem tego, że podkreślenie jest dozwolone jako znak początkowy (jak tradycyjna w języku programowania C), sekwencje unikowe Unicode są dozwolone w identyfikatorach, a znak`@`"" jest dozwolony jako prefiks, aby można było używać słów kluczowych jako identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="ebb69-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

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

<span data-ttu-id="ebb69-173">Aby uzyskać informacje na temat klas znaków Unicode wymienionych powyżej, zobacz Standard Unicode w wersji 3,0, sekcja 4,5.</span><span class="sxs-lookup"><span data-stu-id="ebb69-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="ebb69-174">Przykłady prawidłowych identyfikatorów to "`identifier1`", "`_identifier2`" i "`@if`".</span><span class="sxs-lookup"><span data-stu-id="ebb69-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="ebb69-175">Identyfikator w programie zgodnym musi znajdować się w formacie kanonicznym zdefiniowanym przez normalizację Unicode w postaci C, zgodnie z definicją standardu Unicode w załączniku 15.</span><span class="sxs-lookup"><span data-stu-id="ebb69-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="ebb69-176">Zachowanie podczas napotkania identyfikatora, którego nie ma w postaci normalizacji C, jest zdefiniowane w implementacji; jednak Diagnostyka nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="ebb69-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="ebb69-177">Prefiks "`@`" umożliwia używanie słów kluczowych jako identyfikatorów, co jest przydatne w przypadku współdziałania z innymi językami programowania.</span><span class="sxs-lookup"><span data-stu-id="ebb69-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="ebb69-178">Znak `@` nie jest w rzeczywistości częścią identyfikatora, więc identyfikator może być widziany w innych językach jako normalny identyfikator, bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="ebb69-179">Identyfikator z `@` prefiksem nazywa się ***identyfikatorem Verbatim***.</span><span class="sxs-lookup"><span data-stu-id="ebb69-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="ebb69-180">`@` Używanie prefiksu dla identyfikatorów, które nie są słowami kluczowymi jest dozwolone, ale zdecydowanie odradza się jako kwestia stylu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="ebb69-181">Przykład:</span><span class="sxs-lookup"><span data-stu-id="ebb69-181">The example:</span></span>
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
<span data-ttu-id="ebb69-182">definiuje klasę o nazwie "`class`" z metodą statyczną o nazwie`static`"", która przyjmuje parametr o`bool`nazwie "".</span><span class="sxs-lookup"><span data-stu-id="ebb69-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="ebb69-183">Należy zauważyć, że ponieważ wyrażenia ucieczki Unicode nie są dozwolone w słowach`cl\u0061ss`kluczowych, token "" jest identyfikatorem i jest tym samym`@class`identyfikatorem jak "".</span><span class="sxs-lookup"><span data-stu-id="ebb69-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="ebb69-184">Dwa identyfikatory są uważane za takie same, jeśli są identyczne po zastosowaniu następujących przekształceń:</span><span class="sxs-lookup"><span data-stu-id="ebb69-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="ebb69-185">Prefiks "`@`", jeśli jest używany, jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="ebb69-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="ebb69-186">Każdy *unicode_escape_sequence* jest przekształcany do odpowiedniego znaku Unicode.</span><span class="sxs-lookup"><span data-stu-id="ebb69-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="ebb69-187">Wszystkie *formatting_character*s zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="ebb69-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="ebb69-188">Identyfikatory zawierające dwa kolejne znaki podkreślenia (`U+005F`) są zarezerwowane do użytku przez implementację.</span><span class="sxs-lookup"><span data-stu-id="ebb69-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="ebb69-189">Na przykład implementacja może zapewnić rozszerzone słowa kluczowe, które zaczynają się od dwóch znaków podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="ebb69-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="ebb69-190">słowa kluczowe</span><span class="sxs-lookup"><span data-stu-id="ebb69-190">Keywords</span></span>

<span data-ttu-id="ebb69-191">***Słowo kluczowe*** to sekwencja znaków, która jest zastrzeżona i nie może być używana jako identyfikator, z wyjątkiem sytuacji, gdy jest `@` poprzedzona znakiem.</span><span class="sxs-lookup"><span data-stu-id="ebb69-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

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

<span data-ttu-id="ebb69-192">W niektórych miejscach gramatycznych określone identyfikatory mają specjalne znaczenie, ale nie są słowami kluczowymi.</span><span class="sxs-lookup"><span data-stu-id="ebb69-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="ebb69-193">Takie identyfikatory są czasami nazywane "kontekstowymi słowami kluczowymi".</span><span class="sxs-lookup"><span data-stu-id="ebb69-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="ebb69-194">Na przykład w deklaracji właściwości identyfikatory "`get`" i "`set`" mają specjalne znaczenie (metody[dostępu](classes.md#accessors)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="ebb69-195">Identyfikator inny niż `get` lub `set` nigdy nie jest dozwolony w tych lokalizacjach, dlatego to użycie nie powoduje konfliktu z użyciem tych słów jako identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="ebb69-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="ebb69-196">W innych przypadkach, na przykład z identyfikatorem "`var`" w deklaracjach zmiennych lokalnych niejawnie wpisanych ([lokalnych deklaracji zmiennych](statements.md#local-variable-declarations)), kontekstowe słowo kluczowe może powodować konflikt z zadeklarowanymi nazwami.</span><span class="sxs-lookup"><span data-stu-id="ebb69-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="ebb69-197">W takich przypadkach zadeklarowana nazwa ma pierwszeństwo przed użyciem identyfikatora jako kontekstowego słowa kluczowego.</span><span class="sxs-lookup"><span data-stu-id="ebb69-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="ebb69-198">Literały</span><span class="sxs-lookup"><span data-stu-id="ebb69-198">Literals</span></span>

<span data-ttu-id="ebb69-199">***Literał*** jest reprezentacją kodu źródłowego wartości.</span><span class="sxs-lookup"><span data-stu-id="ebb69-199">A ***literal*** is a source code representation of a value.</span></span>

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

#### <a name="boolean-literals"></a><span data-ttu-id="ebb69-200">Literały logiczne</span><span class="sxs-lookup"><span data-stu-id="ebb69-200">Boolean literals</span></span>

<span data-ttu-id="ebb69-201">Istnieją dwie wartości literału logicznego `true` : `false`i.</span><span class="sxs-lookup"><span data-stu-id="ebb69-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="ebb69-202">Typem elementu *boolean_literal* jest `bool`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="ebb69-203">Literały całkowite</span><span class="sxs-lookup"><span data-stu-id="ebb69-203">Integer literals</span></span>

<span data-ttu-id="ebb69-204">Literały całkowite są używane do zapisywania wartości `int`typów, `uint`, `long`i `ulong`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="ebb69-205">Literały całkowite mają dwie możliwe formy: dziesiętną i szesnastkową.</span><span class="sxs-lookup"><span data-stu-id="ebb69-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

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

<span data-ttu-id="ebb69-206">Typ literału liczby całkowitej jest określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ebb69-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="ebb69-207">Jeśli literał nie ma sufiksu, ma pierwszy z tych typów, w którym można przedstawić `int`jego wartość:, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="ebb69-208">Jeśli literał `U` jest poprzedzony przez lub `u`, ma pierwszy z tych typów, w których można reprezentować wartość: `uint`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="ebb69-209">Jeśli literał `L` jest poprzedzony przez lub `l`, ma pierwszy z tych typów, w których można reprezentować wartość: `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="ebb69-210">Jeśli literał jest `UL`sufiksem, `LU` `ul` `uL` `Ul` ,,`Lu`,,, ,lub`lu`, jest typem `ulong`. `lU`</span><span class="sxs-lookup"><span data-stu-id="ebb69-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="ebb69-211">Jeśli wartość reprezentowana przez literał liczby całkowitej znajduje się poza zakresem `ulong` typu, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ebb69-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="ebb69-212">Ze względu na styl`L`zaleca się użycie "" zamiast "`l`" podczas pisania literałów typu `long`, ponieważ można łatwo pomylić literę`l`"" z cyfrą "`1`".</span><span class="sxs-lookup"><span data-stu-id="ebb69-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="ebb69-213">Aby zezwolić na zapisanie `int` najmniejszych możliwych i `long` wartości w postaci dziesiętnych literałów liczb całkowitych, istnieją następujące dwie reguły:</span><span class="sxs-lookup"><span data-stu-id="ebb69-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="ebb69-214">Gdy *decimal_integer_literal* o wartości 2147483648 (2 ^ 31) i bez *integer_type_suffix* pojawia się jako token bezpośrednio po jednoargumentowym tokenie operatora minus ([jednoargumentowy operator minus](expressions.md#unary-minus-operator)), wynik jest stałą typu `int`wartość-2147483648 (-2 ^ 31).</span><span class="sxs-lookup"><span data-stu-id="ebb69-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="ebb69-215">We wszystkich innych sytuacjach takie *decimal_integer_literal* jest typu `uint`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="ebb69-216">Gdy *decimal_integer_literal* o wartości zakresu od (2 ^ 63) i bez *integer_type_suffix* lub *integer_type_suffix* `L` lub `l` pojawia się jako token bezpośrednio po jednoargumentowym znaku minus token operatora ([jednoargumentowy operator minus](expressions.md#unary-minus-operator)), wynik jest stałą typu `long` z wartością-zakresu od (-2 ^ 63).</span><span class="sxs-lookup"><span data-stu-id="ebb69-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="ebb69-217">We wszystkich innych sytuacjach takie *decimal_integer_literal* jest typu `ulong`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="ebb69-218">Literały prawdziwe</span><span class="sxs-lookup"><span data-stu-id="ebb69-218">Real literals</span></span>

<span data-ttu-id="ebb69-219">Literały prawdziwe są używane do zapisywania wartości typów `float`, `double`i `decimal`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

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

<span data-ttu-id="ebb69-220">Jeśli *real_type_suffix* nie jest określony, typem literału rzeczywistego jest `double`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="ebb69-221">W przeciwnym razie sufiks typu rzeczywistego określa typ literału rzeczywistego w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ebb69-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="ebb69-222">Literał prawdziwy sufiksu `F` lub `f` jest typu `float`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="ebb69-223">Na przykład `1f`literały, `1e10f` `1.5f`,, i `123.456F` są wszystkie typu `float`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="ebb69-224">Literał prawdziwy sufiksu `D` lub `d` jest typu `double`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="ebb69-225">Na przykład `1d`literały, `1e10d` `1.5d`,, i `123.456D` są wszystkie typu `double`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="ebb69-226">Literał prawdziwy sufiksu `M` lub `m` jest typu `decimal`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="ebb69-227">Na przykład `1m`literały, `1e10m` `1.5m`,, i `123.456M` są wszystkie typu `decimal`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="ebb69-228">Ten literał jest konwertowany na `decimal` wartość przez pobranie dokładnej wartości i, w razie potrzeby, zaokrąglenie do najbliższej wartości, którą można przedstawić, przy użyciu zaokrąglenia banku ([Typ dziesiętny](types.md#the-decimal-type)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="ebb69-229">Każda Skala widoczna w literale jest zachowywana, chyba że wartość jest zaokrąglana lub wartość jest równa zero (w którym ostatnim przypadku znak i Skala będzie równa 0).</span><span class="sxs-lookup"><span data-stu-id="ebb69-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="ebb69-230">W związku z tym `2.900m` literał zostanie przeanalizowany w celu utworzenia wartości dziesiętnej `0`ze znakiem, współczynnikiem `2900`i skalą `3`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="ebb69-231">Jeśli określony literał nie może być reprezentowany w wskazanym typie, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ebb69-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="ebb69-232">Wartość rzeczywistego literału typu `float` lub `double` jest określana za pomocą typu "okrągłe do najbliższe".</span><span class="sxs-lookup"><span data-stu-id="ebb69-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="ebb69-233">Należy pamiętać, że w literale rzeczywistym cyfry dziesiętne są zawsze wymagane po przecinku dziesiętnym.</span><span class="sxs-lookup"><span data-stu-id="ebb69-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="ebb69-234">Na przykład jest `1.3F` słowem rzeczywistym, ale `1.F` nie jest.</span><span class="sxs-lookup"><span data-stu-id="ebb69-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="ebb69-235">Literały znaków</span><span class="sxs-lookup"><span data-stu-id="ebb69-235">Character literals</span></span>

<span data-ttu-id="ebb69-236">Literał znakowy reprezentuje pojedynczy znak i zwykle składa się z znaku w cudzysłowie, jak w `'a'`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="ebb69-237">Uwaga: Notacja gramatyki ANTLR sprawia, że jest to mylące.</span><span class="sxs-lookup"><span data-stu-id="ebb69-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="ebb69-238">W antlr, gdy piszesz `\'` , oznacza pojedynczy cudzysłów. `'`</span><span class="sxs-lookup"><span data-stu-id="ebb69-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="ebb69-239">A gdy piszesz `\\` , oznacza pojedynczy ukośnik odwrotny `\`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="ebb69-240">W związku z tym pierwsza reguła dla literału znakowego oznacza, że rozpoczyna się od pojedynczego cudzysłowu, a następnie pojedynczego cudzysłowu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="ebb69-241">I jedenaście możliwych prostych sekwencji ucieczki to `\'` `\\`, `\"`,, `\0` `\b` ,,`\t`, `\n` ,`\r`,,, `\f` `\a` `\v`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

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

<span data-ttu-id="ebb69-242">Znak`\`, który następuje po znaku ukośnika odwrotnego () w *znaku* musi być jednym z następujących znaków `'` `0`:, `"` `b`, `\` `a`,,,, `f` , `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="ebb69-243">W przeciwnym razie wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ebb69-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="ebb69-244">Szesnastkowa sekwencja ucieczki reprezentuje pojedynczy znak Unicode, z wartością utworzoną przez liczbę szesnastkową następującej po znaku`\x`"".</span><span class="sxs-lookup"><span data-stu-id="ebb69-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="ebb69-245">Jeśli wartość reprezentowana przez literał znakowy jest większa niż `U+FFFF`, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ebb69-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="ebb69-246">Sekwencja ucieczki znaków Unicode ([Sekwencje ucieczki znaków Unicode](lexical-structure.md#unicode-character-escape-sequences)) w literale znakowym musi należeć `U+FFFF`do zakresu `U+0000` od do.</span><span class="sxs-lookup"><span data-stu-id="ebb69-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="ebb69-247">Prosta sekwencja ucieczki reprezentuje kodowanie znaków Unicode, zgodnie z opisem w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ebb69-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="ebb69-248">__Sekwencja unikowa__</span><span class="sxs-lookup"><span data-stu-id="ebb69-248">__Escape sequence__</span></span> | <span data-ttu-id="ebb69-249">__Nazwa znaku__</span><span class="sxs-lookup"><span data-stu-id="ebb69-249">__Character name__</span></span> | <span data-ttu-id="ebb69-250">__Kodowanie Unicode__</span><span class="sxs-lookup"><span data-stu-id="ebb69-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="ebb69-251">Pojedynczy cytat</span><span class="sxs-lookup"><span data-stu-id="ebb69-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="ebb69-252">Podwójny cudzysłów</span><span class="sxs-lookup"><span data-stu-id="ebb69-252">Double quote</span></span>       | `0x0022`             | 
| <span data-ttu-id="ebb69-253">`\\`| Ukośnik odwrotny |`0x005C`</span><span class="sxs-lookup"><span data-stu-id="ebb69-253">`\\`                | Backslash          | `0x005C`</span></span>             | 
| `\0`                | <span data-ttu-id="ebb69-254">Null</span><span class="sxs-lookup"><span data-stu-id="ebb69-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="ebb69-255">Alerty</span><span class="sxs-lookup"><span data-stu-id="ebb69-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="ebb69-256">Backspace</span><span class="sxs-lookup"><span data-stu-id="ebb69-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="ebb69-257">Kanał informacyjny formularza</span><span class="sxs-lookup"><span data-stu-id="ebb69-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="ebb69-258">Nowy wiersz</span><span class="sxs-lookup"><span data-stu-id="ebb69-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="ebb69-259">Znak powrotu karetki</span><span class="sxs-lookup"><span data-stu-id="ebb69-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="ebb69-260">Tabulator poziomy</span><span class="sxs-lookup"><span data-stu-id="ebb69-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="ebb69-261">Tabulator pionowy</span><span class="sxs-lookup"><span data-stu-id="ebb69-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="ebb69-262">Typem elementu *character_literal* jest `char`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="ebb69-263">Literały ciągu</span><span class="sxs-lookup"><span data-stu-id="ebb69-263">String literals</span></span>

<span data-ttu-id="ebb69-264">C#obsługuje dwa formy literałów ciągu: ***zwykłe literały ciągów*** i ***literały ciągu Verbatim***.</span><span class="sxs-lookup"><span data-stu-id="ebb69-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="ebb69-265">Regularne literały ciągu składa się z zera lub większej liczby znaków ujętych w podwójne `"hello"`cudzysłowy, jak w i mogą zawierać proste sekwencje `\t` ucieczki (takie jak dla znaku tabulacji) oraz sekwencje szesnastkowe i Unicode.</span><span class="sxs-lookup"><span data-stu-id="ebb69-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="ebb69-266">Literał ciągu Verbatim składa się ze `@` znaku, po którym następuje znak podwójnego cudzysłowu, zero lub więcej znaków i zamykający znak podwójnego cudzysłowu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="ebb69-267">Prostym przykładem `@"hello"`jest.</span><span class="sxs-lookup"><span data-stu-id="ebb69-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="ebb69-268">W literale ciągu Verbatim znaki między ogranicznikami są interpretowane jako Verbatim, jedyny wyjątek to *quote_escape_sequence*.</span><span class="sxs-lookup"><span data-stu-id="ebb69-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="ebb69-269">W szczególności proste sekwencje unikowe i sekwencje unikowe szesnastkowe i Unicode nie są przetwarzane w literałach ciągów Verbatim.</span><span class="sxs-lookup"><span data-stu-id="ebb69-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="ebb69-270">Literał ciągu Verbatim może obejmować wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="ebb69-270">A verbatim string literal may span multiple lines.</span></span>

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

<span data-ttu-id="ebb69-271">Znak, który następuje po ukośniku odwrotnym`\`() w *regular_string_literal_character* , musi mieć jeden z następujących znaków `0`: `'`, `"`, `\` `a`,,, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="ebb69-272">W przeciwnym razie wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ebb69-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="ebb69-273">Przykład</span><span class="sxs-lookup"><span data-stu-id="ebb69-273">The example</span></span>
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
<span data-ttu-id="ebb69-274">pokazuje różne literały ciągu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-274">shows a variety of string literals.</span></span> <span data-ttu-id="ebb69-275">Ostatni literał ciągu, `j`, jest Verbatim literał ciągu, który obejmuje wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="ebb69-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="ebb69-276">Znaki między znakami cudzysłowu, w tym odstępy, takie jak znaki nowego wiersza, są zachowywane Verbatim.</span><span class="sxs-lookup"><span data-stu-id="ebb69-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="ebb69-277">Ponieważ szesnastkowa sekwencja ucieczki może mieć zmienną liczbę cyfr szesnastkowych, literał `"\x123"` ciągu zawiera pojedynczy znak z wartością szesnastkową 123.</span><span class="sxs-lookup"><span data-stu-id="ebb69-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="ebb69-278">Aby utworzyć ciąg zawierający znak o wartości szesnastkowej 12, po którym następuje znak 3, jeden może napisać `"\x00123"` lub `"\x12" + "3"` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="ebb69-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="ebb69-279">Typem elementu *string_literaL* jest `string`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="ebb69-280">Każdy literał ciągu nie musi powodować wystąpienia nowego ciągu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="ebb69-281">Gdy co najmniej dwa literały ciągu, które są równoważne względem operatora równości ciągów ([Operatory równości ciągów](expressions.md#string-equality-operators)) pojawiają się w tym samym programie, te literały ciągu odwołują się do tego samego wystąpienia ciągu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="ebb69-282">Na przykład dane wyjściowe generowane przez</span><span class="sxs-lookup"><span data-stu-id="ebb69-282">For instance, the output produced by</span></span>
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
<span data-ttu-id="ebb69-283">jest `True` , ponieważ dwa literały odwołują się do tego samego wystąpienia ciągu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="ebb69-284">Interpolowane literały ciągów</span><span class="sxs-lookup"><span data-stu-id="ebb69-284">Interpolated string literals</span></span>

<span data-ttu-id="ebb69-285">Interpolowane literały ciągów są podobne do literałów ciągów, ale zawierają dziury, które `{` mogą `}`wystąpić w wyrażeniach.</span><span class="sxs-lookup"><span data-stu-id="ebb69-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="ebb69-286">W czasie wykonywania wyrażenia są oceniane w celu zamienienia ich formularzy tekstowych na ciąg w miejscu, w którym występuje otwór.</span><span class="sxs-lookup"><span data-stu-id="ebb69-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="ebb69-287">Składnia interpolacji ciągu jest opisana w sekcji ([ciągi interpolowane](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="ebb69-288">Podobnie jak literały ciągu, interpolowane literały ciągów mogą być zwykłe lub Verbatim.</span><span class="sxs-lookup"><span data-stu-id="ebb69-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="ebb69-289">Interpolowane zwykłe literały ciągu są rozdzielane przez `$"` i `"`, a interpolowane literały ciągu Verbatim są rozdzielane przez `$@"` i `"`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="ebb69-290">Podobnie jak w przypadku innych literałów, analiza leksykalna interpolowanego literału ciągu początkowo skutkuje pojedynczym tokenem, zgodnie z poniższą gramatyką.</span><span class="sxs-lookup"><span data-stu-id="ebb69-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="ebb69-291">Jednak przed analizą składni pojedynczy token interpolowanego literału ciągu jest podzielony na kilka tokenów części ciągu otaczających otwory, a elementy wejściowe występujące w dziurach są następnie analizowane ponownie.</span><span class="sxs-lookup"><span data-stu-id="ebb69-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="ebb69-292">Może to spowodować wygenerowanie bardziej interpolowanych literałów ciągów do przetworzenia, ale w przypadku, gdy są poprawne, w rezultacie będzie możliwe przetworzenie sekwencji tokenów analizy składni.</span><span class="sxs-lookup"><span data-stu-id="ebb69-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

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

<span data-ttu-id="ebb69-293">Token *interpolated_string_literal* jest ponownie interpretowany jako wiele tokenów i innych elementów wejściowych w następujący sposób w kolejności wystąpienia w *interpolated_string_literal*:</span><span class="sxs-lookup"><span data-stu-id="ebb69-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="ebb69-294">Wystąpienia następujących elementów są interpretowane jako oddzielne pojedyncze tokeny: znak wiodący `$` , *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* i *interpolated_verbatim_string_end*.</span><span class="sxs-lookup"><span data-stu-id="ebb69-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="ebb69-295">Wystąpienia *regular_balanced_text* i *verbatim_balanced_text* między tymi elementami są ponownie przetwarzane jako *input_section* ([Analiza leksykalna](lexical-structure.md#lexical-analysis)) i są ponownie interpretowane jako wyniki sekwencji elementów wejściowych.</span><span class="sxs-lookup"><span data-stu-id="ebb69-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="ebb69-296">Mogą one z kolei obejmować interpolowane tokeny literałów ciągów do reinterpretacji.</span><span class="sxs-lookup"><span data-stu-id="ebb69-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="ebb69-297">Analiza składni spowoduje ponowne połączenie tokenów z *interpolated_string_expression* ([ciągami interpolowanymi](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="ebb69-298">Przykłady do zrobienia</span><span class="sxs-lookup"><span data-stu-id="ebb69-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="ebb69-299">Literał o wartości null</span><span class="sxs-lookup"><span data-stu-id="ebb69-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="ebb69-300">*Null_literal* można niejawnie przekonwertować na typ referencyjny lub typ dopuszczający wartość null.</span><span class="sxs-lookup"><span data-stu-id="ebb69-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="ebb69-301">Operatory i przerywniki</span><span class="sxs-lookup"><span data-stu-id="ebb69-301">Operators and punctuators</span></span>

<span data-ttu-id="ebb69-302">Istnieje kilka rodzajów operatorów i przerywniki.</span><span class="sxs-lookup"><span data-stu-id="ebb69-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="ebb69-303">Operatory są używane w wyrażeniach do opisywania operacji obejmujących jeden lub więcej operandów.</span><span class="sxs-lookup"><span data-stu-id="ebb69-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="ebb69-304">`a + b` Na przykład, wyrażenie `+` używa operatora, aby `a` dodać dwa operandy i `b`.</span><span class="sxs-lookup"><span data-stu-id="ebb69-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="ebb69-305">Przerywniki są przeznaczone do grupowania i oddzielania.</span><span class="sxs-lookup"><span data-stu-id="ebb69-305">Punctuators are for grouping and separating.</span></span>

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

<span data-ttu-id="ebb69-306">Pionowy pasek w produkcji *right_shift* i *right_shift_assignment* jest używany do wskazania, że w przeciwieństwie do innych produkcji w gramatyce składni nie są dozwolone żadne znaki jakiegokolwiek rodzaju (nieparzyste odstępy) między tokenami.</span><span class="sxs-lookup"><span data-stu-id="ebb69-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="ebb69-307">Te produkcje są traktowane specjalnie w celu zapewnienia prawidłowej obsługi *type_parameter_list*s ([parametry typu](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="ebb69-308">Dyrektywy wstępnego przetwarzania</span><span class="sxs-lookup"><span data-stu-id="ebb69-308">Pre-processing directives</span></span>

<span data-ttu-id="ebb69-309">Dyrektywy wstępnego przetwarzania zapewniają możliwość warunkowego pomijania sekcji plików źródłowych, do zgłaszania błędów i ostrzeżeń oraz odróżnić odrębnych regionów kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="ebb69-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="ebb69-310">Termin "dyrektywy wstępnego przetwarzania" jest używany tylko w celu zapewnienia spójności z językami C++ C i programowaniem.</span><span class="sxs-lookup"><span data-stu-id="ebb69-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="ebb69-311">W C#programie nie ma oddzielnego kroku przetwarzania wstępnego; dyrektywy przetwarzania wstępnego są przetwarzane jako część fazy analizy leksykalnej.</span><span class="sxs-lookup"><span data-stu-id="ebb69-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

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

<span data-ttu-id="ebb69-312">Dostępne są następujące dyrektywy wstępnego przetwarzania:</span><span class="sxs-lookup"><span data-stu-id="ebb69-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="ebb69-313">`#define`i `#undef`, które są używane do definiowania i niedefiniowania odpowiednio symboli kompilacji warunkowej ([dyrektywy deklaracji](lexical-structure.md#declaration-directives)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="ebb69-314">`#if`, `#elif`, `#else`, i`#endif`, które są używane do warunkowego pomijania sekcji kodu źródłowego ([dyrektywy kompilacji warunkowej](lexical-structure.md#conditional-compilation-directives)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="ebb69-315">`#line`, który jest używany do sterowania numerami wierszy emitowanych pod kątem błędów i ostrzeżeń ([dyrektywy wiersza](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="ebb69-316">`#error`i `#warning`, które są używane do wydawania błędów i ostrzeżeń odpowiednio ([dyrektywy diagnostyczne](lexical-structure.md#diagnostic-directives)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="ebb69-317">`#region`i `#endregion`, które są używane do jawnego oznaczania sekcji kodu źródłowego ([dyrektywy regionu](lexical-structure.md#region-directives)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="ebb69-318">`#pragma`, który jest używany do określania opcjonalnych informacji kontekstowych do kompilatora ([dyrektywy pragma](lexical-structure.md#pragma-directives)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="ebb69-319">Dyrektywa poprzedzająca przetwarzanie zawsze zawiera oddzielny wiersz kodu źródłowego i zawsze zaczyna `#` się od znaku i nazwy dyrektywy wstępnego przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="ebb69-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="ebb69-320">Spacja może wystąpić przed `#` znakiem i `#` znakiem oraz między nazwą dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="ebb69-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="ebb69-321">Wiersz `#define`źródłowy zawierający, `#undef` `#if` ,,`#elif` ,,`#endregion` , lub dyrektywy może kończyć się jednowierszowym komentarzem. `#else` `#endif` `#line`</span><span class="sxs-lookup"><span data-stu-id="ebb69-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="ebb69-322">Rozdzielane Komentarze ( `/* */` styl komentarzy) nie są dozwolone w wierszach źródłowych zawierających dyrektywy poprzedzające przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="ebb69-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="ebb69-323">Dyrektywy poprzedzające przetwarzanie nie są tokenami i nie są częścią gramatyki składni C#.</span><span class="sxs-lookup"><span data-stu-id="ebb69-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="ebb69-324">Jednakże dyrektywy wstępnego przetwarzania mogą być używane do dołączania lub wykluczania sekwencji tokenów i mogą w ten sposób mieć wpływ C# na znaczenie programu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="ebb69-325">Na przykład podczas kompilowania programu:</span><span class="sxs-lookup"><span data-stu-id="ebb69-325">For example, when compiled, the program:</span></span>
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
<span data-ttu-id="ebb69-326">wynikiem jest dokładna sekwencja tokenów co program:</span><span class="sxs-lookup"><span data-stu-id="ebb69-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="ebb69-327">W związku z tym te dwa programy są bardzo różne i syntaktycznie są identyczne.</span><span class="sxs-lookup"><span data-stu-id="ebb69-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="ebb69-328">Symbole kompilacji warunkowej</span><span class="sxs-lookup"><span data-stu-id="ebb69-328">Conditional compilation symbols</span></span>

<span data-ttu-id="ebb69-329">Funkcje kompilacji `#if`warunkowej udostępniane przez `#else`, `#elif`,, i `#endif` dyrektywy są kontrolowane przez wyrażenia wstępnego przetwarzania ([wyrażenia wstępnego przetwarzania](lexical-structure.md#pre-processing-expressions)) i warunkowe symbole kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ebb69-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="ebb69-330">Symbol kompilacji warunkowej ma dwa stany: ***zdefiniowane*** lub ***niezdefiniowane***.</span><span class="sxs-lookup"><span data-stu-id="ebb69-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="ebb69-331">Na początku przetwarzania leksykalnego pliku źródłowego symbol kompilacji warunkowej jest niezdefiniowany, chyba że został jawnie zdefiniowany przez mechanizm zewnętrzny (na przykład opcję kompilatora wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="ebb69-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="ebb69-332">Po przetworzeniu `#define` dyrektywy, symbol kompilacji warunkowej o nazwie w tej dyrektywie zostanie zdefiniowany w tym pliku źródłowym.</span><span class="sxs-lookup"><span data-stu-id="ebb69-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="ebb69-333">Symbol pozostaje zdefiniowany do czasu `#undef` przetworzenia dyrektywy dla tego samego symbolu lub do momentu osiągnięcia końca pliku źródłowego.</span><span class="sxs-lookup"><span data-stu-id="ebb69-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="ebb69-334">Implikacją tego jest to, `#define` że `#undef` dyrektywy w jednym pliku źródłowym nie mają wpływu na inne pliki źródłowe w tym samym programie.</span><span class="sxs-lookup"><span data-stu-id="ebb69-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="ebb69-335">W przypadku odwołania w wyrażeniu poprzedzającym przetwarzanie, zdefiniowany symbol kompilacji warunkowej ma wartość `true`logiczną, a niezdefiniowany symbol kompilacji warunkowej ma wartość `false`logiczną.</span><span class="sxs-lookup"><span data-stu-id="ebb69-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="ebb69-336">Nie istnieje wymóg, aby symbole kompilacji warunkowej były jawnie zadeklarowane przed odwołaniami do nich w wyrażeniach poprzedzających przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="ebb69-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="ebb69-337">Zamiast tego symbole niezadeklarowane są po prostu niezdefiniowane i dlatego `false`mają wartość.</span><span class="sxs-lookup"><span data-stu-id="ebb69-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="ebb69-338">Przestrzeń nazw dla symboli kompilacji warunkowej jest odrębna i oddzielona od wszystkich innych nazwanych C# jednostek w programie.</span><span class="sxs-lookup"><span data-stu-id="ebb69-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="ebb69-339">Do symboli kompilacji warunkowej można odwoływać `#define` się `#undef` tylko w dyrektywach i i w wyrażeniach poprzedzających przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="ebb69-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="ebb69-340">Wyrażenia wstępnego przetwarzania</span><span class="sxs-lookup"><span data-stu-id="ebb69-340">Pre-processing expressions</span></span>

<span data-ttu-id="ebb69-341">Wyrażenia wstępnego przetwarzania mogą wystąpić w `#if` dyrektywach i `#elif` .</span><span class="sxs-lookup"><span data-stu-id="ebb69-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="ebb69-342">Operatory `!`, `==`, ,`!=` isą`||` dozwolone w wyrażeniach poprzedzających przetwarzanie, a nawiasy mogą być używane do grupowania. `&&`</span><span class="sxs-lookup"><span data-stu-id="ebb69-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

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

<span data-ttu-id="ebb69-343">W przypadku odwołania w wyrażeniu poprzedzającym przetwarzanie, zdefiniowany symbol kompilacji warunkowej ma wartość `true`logiczną, a niezdefiniowany symbol kompilacji warunkowej ma wartość `false`logiczną.</span><span class="sxs-lookup"><span data-stu-id="ebb69-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="ebb69-344">Obliczenie wyrażenia przetwarzania wstępnego zawsze zwraca wartość logiczną.</span><span class="sxs-lookup"><span data-stu-id="ebb69-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="ebb69-345">Reguły obliczania dla wyrażenia przetwarzania wstępnego są takie same jak dla wyrażeń stałych ([wyrażeń stałych](expressions.md#constant-expressions)), z tą różnicą, że jedyne jednostki zdefiniowane przez użytkownika, do których można się odwoływać, są symbolami kompilacji warunkowej.</span><span class="sxs-lookup"><span data-stu-id="ebb69-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="ebb69-346">Dyrektywy deklaracji</span><span class="sxs-lookup"><span data-stu-id="ebb69-346">Declaration directives</span></span>

<span data-ttu-id="ebb69-347">Dyrektywy deklaracji są używane do definiowania lub niedefiniowania symboli kompilacji warunkowej.</span><span class="sxs-lookup"><span data-stu-id="ebb69-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="ebb69-348">Przetwarzanie `#define` dyrektywy powoduje, że dany symbol kompilacji warunkowej zostanie zdefiniowany, rozpoczynając od wiersza źródłowego, który następuje po dyrektywie.</span><span class="sxs-lookup"><span data-stu-id="ebb69-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="ebb69-349">Podobnie przetwarzanie `#undef` dyrektywy powoduje, że dany symbol kompilacji warunkowej staje się niezdefiniowany, rozpoczynając od wiersza źródłowego, który następuje po dyrektywie.</span><span class="sxs-lookup"><span data-stu-id="ebb69-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="ebb69-350">Wszystkie `#define` i`#undef` dyrektywy w pliku źródłowym muszą wystąpić przed pierwszym *tokenem* ([tokenami](lexical-structure.md#tokens)) w pliku źródłowym; w przeciwnym razie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ebb69-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="ebb69-351">W intuicyjnych warunkach `#define` , `#undef` dyrektywy muszą poprzedzać dowolny "kod prawdziwy" w pliku źródłowym.</span><span class="sxs-lookup"><span data-stu-id="ebb69-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="ebb69-352">Przykład:</span><span class="sxs-lookup"><span data-stu-id="ebb69-352">The example:</span></span>
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
<span data-ttu-id="ebb69-353">jest prawidłowy, `#define` ponieważ dyrektywy poprzedzają pierwszy token `namespace` (słowo kluczowe) w pliku źródłowym.</span><span class="sxs-lookup"><span data-stu-id="ebb69-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="ebb69-354">Poniższy przykład powoduje błąd czasu kompilacji, ponieważ `#define` następujący kod prawdziwy:</span><span class="sxs-lookup"><span data-stu-id="ebb69-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
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

<span data-ttu-id="ebb69-355">A `#define` może zdefiniować znak kompilacji warunkowej, który jest już zdefiniowany, bez występowania jakichkolwiek `#undef` interwencji dla tego symbolu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="ebb69-356">W poniższym przykładzie zdefiniowano symbol `A` kompilacji warunkowej, a następnie definiuje go ponownie.</span><span class="sxs-lookup"><span data-stu-id="ebb69-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="ebb69-357">A `#undef` może "usuń definicję" warunkowego symbolu kompilacji, który nie jest zdefiniowany.</span><span class="sxs-lookup"><span data-stu-id="ebb69-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="ebb69-358">W poniższym przykładzie zdefiniowano symbol `A` kompilacji warunkowej, a następnie jego definicję należy wykonać dwa razy. Mimo że druga `#undef` nie ma wpływu, jest nadal ważna.</span><span class="sxs-lookup"><span data-stu-id="ebb69-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="ebb69-359">Dyrektywy kompilacji warunkowej</span><span class="sxs-lookup"><span data-stu-id="ebb69-359">Conditional compilation directives</span></span>

<span data-ttu-id="ebb69-360">Dyrektywy kompilacji warunkowej są używane do warunkowego dołączania lub wykluczania części pliku źródłowego.</span><span class="sxs-lookup"><span data-stu-id="ebb69-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

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

<span data-ttu-id="ebb69-361">Jak wskazano w składni, dyrektywy kompilacji warunkowej muszą być zapisywane jako zestawy składające się z, w `#if` kolejności, dyrektywy, dyrektyw `#elif` , zero lub więcej dyrektywy `#else` , zero lub `#endif` jedna dyrektywa i dyrektywa.</span><span class="sxs-lookup"><span data-stu-id="ebb69-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="ebb69-362">Między dyrektywami są warunkowe sekcje kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="ebb69-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="ebb69-363">Każda sekcja jest kontrolowana przez bezpośrednio poprzednią dyrektywę.</span><span class="sxs-lookup"><span data-stu-id="ebb69-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="ebb69-364">Sekcja warunkowa może sama zawierać zagnieżdżone dyrektywy kompilacji warunkowej, jeśli te dyrektywy tworzą kompletne zestawy.</span><span class="sxs-lookup"><span data-stu-id="ebb69-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="ebb69-365">*Pp_conditional* wybiera co najwyżej jeden z zawartych *conditional_section*s do normalnego przetwarzania leksykalnego:</span><span class="sxs-lookup"><span data-stu-id="ebb69-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="ebb69-366">*Pp_expression* `#if` s dyrektyw i `#elif` są `true`oceniane w kolejności do momentu 1.</span><span class="sxs-lookup"><span data-stu-id="ebb69-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="ebb69-367">Jeśli wynikiem jest wyrażenie `true`, *conditional_section* odpowiedniej dyrektywy jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="ebb69-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="ebb69-368">Jeśli wszystkie *pp_expression*s `false` `#else` , a jeśli `#else` jest obecna dyrektywa, zostanie wybrana *conditional_section* dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="ebb69-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="ebb69-369">W przeciwnym razie nie jest zaznaczona żadna *conditional_section* .</span><span class="sxs-lookup"><span data-stu-id="ebb69-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="ebb69-370">Wybrany *conditional_section*(jeśli istnieje) jest przetwarzany jako normalny *input_section*: kod źródłowy zawarty w sekcji musi być zgodny z gramatyką leksykalną; tokeny są generowane na podstawie kodu źródłowego w sekcji; i dyrektywy wstępnego przetwarzania w sekcji mają określone efekty.</span><span class="sxs-lookup"><span data-stu-id="ebb69-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="ebb69-371">Pozostałe *conditional_section*s, jeśli istnieją, są przetwarzane jako *skipped_section*s: z wyjątkiem dyrektyw poprzedzających przetwarzanie, kod źródłowy w sekcji nie musi być zgodny z gramatyką leksykalną; nie Wygenerowano żadnych tokenów z kodu źródłowego w sekcji; i dyrektywy wstępnego przetwarzania w sekcji muszą być poprawiane w sposób leksykalny, ale nie są przetwarzane w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="ebb69-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="ebb69-372">W *conditional_section* , który jest przetwarzany jako *skipped_section*, wszystkie zagnieżdżone *conditional_section*s (zawarte w zagnieżdżonych `#if`... `#endif` i`#region`... Konstrukcje) są również przetwarzane jako *skipped_section s.* `#endregion`</span><span class="sxs-lookup"><span data-stu-id="ebb69-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="ebb69-373">Poniższy przykład ilustruje, jak można zagnieżdżać dyrektywy kompilacji warunkowej:</span><span class="sxs-lookup"><span data-stu-id="ebb69-373">The following example illustrates how conditional compilation directives can nest:</span></span>
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

<span data-ttu-id="ebb69-374">Z wyjątkiem dyrektyw poprzedzających przetwarzanie pominięty kod źródłowy nie podlega analizie leksykalnej.</span><span class="sxs-lookup"><span data-stu-id="ebb69-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="ebb69-375">Na przykład następujące elementy są prawidłowe pomimo niezakończonego komentarza w `#else` sekcji:</span><span class="sxs-lookup"><span data-stu-id="ebb69-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
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

<span data-ttu-id="ebb69-376">Należy jednak zauważyć, że dyrektywy poprzedzające przetwarzanie są wymagane do poprawnego działania, nawet w pominiętych sekcjach kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="ebb69-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="ebb69-377">Dyrektywy poprzedzające przetwarzanie nie są przetwarzane, gdy są wyświetlane wewnątrz wielowierszowych elementów wejściowych.</span><span class="sxs-lookup"><span data-stu-id="ebb69-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="ebb69-378">Na przykład program:</span><span class="sxs-lookup"><span data-stu-id="ebb69-378">For example, the program:</span></span>
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
<span data-ttu-id="ebb69-379">wyniki w danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="ebb69-379">results in the output:</span></span>
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="ebb69-380">W szczególnych przypadkach zestaw dyrektyw przetwarzania wstępnego, które są przetwarzane, może zależeć od oceny *pp_expression*.</span><span class="sxs-lookup"><span data-stu-id="ebb69-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="ebb69-381">Przykład:</span><span class="sxs-lookup"><span data-stu-id="ebb69-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="ebb69-382">zawsze generuje ten sam strumień tokenu (`class` `}` `Q` `{` ), niezależnie od tego, czy `X` jest zdefiniowany.</span><span class="sxs-lookup"><span data-stu-id="ebb69-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="ebb69-383">Jeśli `X` jest zdefiniowany, jedynymi przetworzonymi `#if` dyrektywami są i `#endif`, ze względu na komentarz wielowierszowy.</span><span class="sxs-lookup"><span data-stu-id="ebb69-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="ebb69-384">Jeśli `X` jest niezdefiniowany, wówczas trzy dyrektywy`#if`( `#else`, `#endif`,) są częścią zestawu dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="ebb69-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="ebb69-385">Dyrektywy diagnostyczne</span><span class="sxs-lookup"><span data-stu-id="ebb69-385">Diagnostic directives</span></span>

<span data-ttu-id="ebb69-386">Dyrektywy diagnostyczne służą do jawnego generowania komunikatów błędów i ostrzeżeń, które są zgłaszane w taki sam sposób jak inne błędy i ostrzeżenia w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ebb69-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

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

<span data-ttu-id="ebb69-387">Przykład:</span><span class="sxs-lookup"><span data-stu-id="ebb69-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="ebb69-388">zawsze generuje ostrzeżenie ("Przegląd kodu wymagany przed zaewidencjonowaniem") i generuje błąd czasu kompilacji ("kompilacja nie może jednocześnie debugować i detalicznych"), jeśli symbole `Debug` warunkowe i `Retail` zostały zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="ebb69-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="ebb69-389">Należy pamiętać, że element *pp_message* może zawierać dowolny tekst; w przeciwnym razie nie musi zawierać poprawnie sformułowanych tokenów, jak pokazano w pojedynczym cudzysłowie w `can't`wyrazie.</span><span class="sxs-lookup"><span data-stu-id="ebb69-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="ebb69-390">Dyrektywy regionów</span><span class="sxs-lookup"><span data-stu-id="ebb69-390">Region directives</span></span>

<span data-ttu-id="ebb69-391">Dyrektywy regionów są używane do jawnego oznaczania regionów kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="ebb69-391">The region directives are used to explicitly mark regions of source code.</span></span>

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

<span data-ttu-id="ebb69-392">Do regionu nie dołączono znaczenia semantycznego; regiony są przeznaczone do użytku przez programistę lub przez zautomatyzowane narzędzia do oznaczania sekcji kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="ebb69-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="ebb69-393">Komunikat określony w `#region` dyrektywie lub `#endregion` również nie ma znaczenia semantycznego, a jedynie służy do identyfikowania regionu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="ebb69-394">`#region` Dopasowywanie `#endregion` i dyrektywy mogą mieć różne *pp_message*s.</span><span class="sxs-lookup"><span data-stu-id="ebb69-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="ebb69-395">Przetwarzanie leksykalne regionu:</span><span class="sxs-lookup"><span data-stu-id="ebb69-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="ebb69-396">odpowiada dokładnie na przetwarzanie leksykalne dyrektywy warunkowej kompilacji w postaci:</span><span class="sxs-lookup"><span data-stu-id="ebb69-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="ebb69-397">Dyrektywy linii</span><span class="sxs-lookup"><span data-stu-id="ebb69-397">Line directives</span></span>

<span data-ttu-id="ebb69-398">Dyrektywy wiersza mogą służyć do zmiany numerów wierszy i nazw plików źródłowych raportowanych przez kompilator w danych wyjściowych, takich jak ostrzeżenia i błędy, i które są używane przez atrybuty informacji o wywołującym ([atrybuty informacji o wywołaniu](attributes.md#caller-info-attributes)).</span><span class="sxs-lookup"><span data-stu-id="ebb69-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="ebb69-399">Dyrektywy wiersza są najczęściej używane w narzędziach meta-programistycznych, C# które generują kod źródłowy na podstawie innych danych wejściowych tekstu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

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

<span data-ttu-id="ebb69-400">Gdy żadne `#line` dyrektywy nie są obecne, kompilator zgłasza w danych wyjściowych prawdziwe numery wierszy i nazwy plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="ebb69-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="ebb69-401">Podczas przetwarzania `#line` dyrektywy, która zawiera *line_indicator* , która nie `default`jest, kompilator traktuje wiersz po dyrektywie jako mający podany numer wiersza (i nazwę pliku, jeśli jest określony).</span><span class="sxs-lookup"><span data-stu-id="ebb69-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="ebb69-402">`#line default` Dyrektywa Odwraca efekt wszystkich poprzednich dyrektyw #line.</span><span class="sxs-lookup"><span data-stu-id="ebb69-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="ebb69-403">Kompilator raportuje prawdziwe informacje o wierszu dla kolejnych wierszy, dokładnie tak, `#line` jakby nie zostały przetworzone dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="ebb69-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="ebb69-404">`#line hidden` Dyrektywa nie ma wpływu na numery plików i wierszy raportowane w komunikatach o błędach, ale wpływa na Debugowanie na poziomie źródła.</span><span class="sxs-lookup"><span data-stu-id="ebb69-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="ebb69-405">Podczas debugowania wszystkie wiersze między `#line hidden` dyrektywą a kolejną `#line` dyrektywą (nie `#line hidden`) nie zawierają informacji o numerze wiersza.</span><span class="sxs-lookup"><span data-stu-id="ebb69-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="ebb69-406">Podczas przechodzenia przez kod w debugerze te wiersze zostaną całkowicie pominięte.</span><span class="sxs-lookup"><span data-stu-id="ebb69-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="ebb69-407">Należy zauważyć, że *nazwa_pliku* różni się od zwykłego literału ciągu w tym znaku ucieczki nie są przetwarzane; znak "`\`" oznacza po prostu zwykły ukośnik odwrotny w ciągu *nazwa_pliku*.</span><span class="sxs-lookup"><span data-stu-id="ebb69-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="ebb69-408">Dyrektywy pragma</span><span class="sxs-lookup"><span data-stu-id="ebb69-408">Pragma directives</span></span>

<span data-ttu-id="ebb69-409">`#pragma` Dyrektywa przetwarzania wstępnego służy do określania opcjonalnych informacji kontekstowych do kompilatora.</span><span class="sxs-lookup"><span data-stu-id="ebb69-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="ebb69-410">Informacje podane w `#pragma` dyrektywie nigdy nie zmieniają semantyki programu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="ebb69-411">C#zawiera `#pragma` dyrektywy kontrolujące ostrzeżenia kompilatora.</span><span class="sxs-lookup"><span data-stu-id="ebb69-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="ebb69-412">Przyszłe wersje języka mogą zawierać dodatkowe `#pragma` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="ebb69-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="ebb69-413">Aby zapewnić współdziałanie C# z innymi kompilatorami, C# kompilator firmy Microsoft nie wydaje błędów kompilacji w `#pragma` przypadku nieznanych dyrektyw; takie dyrektywy nie generują jednak ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="ebb69-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="ebb69-414">Ostrzeżenie pragma</span><span class="sxs-lookup"><span data-stu-id="ebb69-414">Pragma warning</span></span>

<span data-ttu-id="ebb69-415">`#pragma warning` Dyrektywa służy do wyłączania lub przywracania całego lub określonego zestawu komunikatów ostrzegawczych podczas kompilacji kolejnego tekstu programu.</span><span class="sxs-lookup"><span data-stu-id="ebb69-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

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

<span data-ttu-id="ebb69-416">`#pragma warning` Dyrektywa, która pomija listę ostrzeżeń, ma wpływ na wszystkie ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="ebb69-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="ebb69-417">`#pragma warning` Dyrektywa, która zawiera listę ostrzeżeń, ma wpływ tylko na te ostrzeżenia, które są określone na liście.</span><span class="sxs-lookup"><span data-stu-id="ebb69-417">A `#pragma warning` directive that includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="ebb69-418">`#pragma warning disable` Dyrektywa powoduje wyłączenie wszystkich lub danego zestawu ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="ebb69-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="ebb69-419">`#pragma warning restore` Dyrektywa przywraca wszystkie lub dany zestaw ostrzeżeń do stanu, który obowiązywał na początku jednostki kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ebb69-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="ebb69-420">Należy pamiętać, że jeśli określone ostrzeżenie zostało wyłączone zewnętrznie, `#pragma warning restore` to (czy dla wszystkich lub określonych ostrzeżeń) nie będzie ponownie włączać tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="ebb69-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="ebb69-421">W poniższym przykładzie pokazano `#pragma warning` sposób tymczasowego wyłączenia ostrzeżenia raportowanego w przypadku odwołania do przestarzałych elementów członkowskich przy użyciu numeru ostrzeżenia z kompilatora firmy Microsoft. C#</span><span class="sxs-lookup"><span data-stu-id="ebb69-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
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
