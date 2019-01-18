---
ms.openlocfilehash: f797692cbd6aeb6035aa7c8ed3139740466c6e42
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229996"
---
# <a name="lexical-structure"></a><span data-ttu-id="2b658-101">Struktura leksykalna</span><span class="sxs-lookup"><span data-stu-id="2b658-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="2b658-102">Programy</span><span class="sxs-lookup"><span data-stu-id="2b658-102">Programs</span></span>

<span data-ttu-id="2b658-103">C# ***program*** składa się z co najmniej jeden ***pliki źródłowe***, nazywanej formalnie ***jednostki kompilacji*** ([jednostki kompilacji](namespaces.md#compilation-units)).</span><span class="sxs-lookup"><span data-stu-id="2b658-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="2b658-104">Plik źródłowy jest uporządkowana sekwencja znaków Unicode.</span><span class="sxs-lookup"><span data-stu-id="2b658-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="2b658-105">Pliki źródłowe zwykle mają relację z plikami w systemie plików, ale ta zgodność nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="2b658-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="2b658-106">Maksymalny przenośności, zalecane jest, że pliki w systemie plików jest zakodowane za pomocą UTF-8 kodowania.</span><span class="sxs-lookup"><span data-stu-id="2b658-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="2b658-107">Koncepcyjnie mówiąc, program jest kompilowany, za pomocą trzech kroków:</span><span class="sxs-lookup"><span data-stu-id="2b658-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="2b658-108">Przekształcenie, które służy do konwertowania pliku Repertuar znaków określonego i schemat kodowania na sekwencję znaków Unicode.</span><span class="sxs-lookup"><span data-stu-id="2b658-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="2b658-109">Poddawać analizie leksykalnej, co przekłada strumień wejściowy znaki Unicode w strumieniu tokenów.</span><span class="sxs-lookup"><span data-stu-id="2b658-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="2b658-110">Analizy składni, która przetwarza strumień tokenów na kod wykonywalny.</span><span class="sxs-lookup"><span data-stu-id="2b658-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="2b658-111">Gramatyki</span><span class="sxs-lookup"><span data-stu-id="2b658-111">Grammars</span></span>

<span data-ttu-id="2b658-112">Ta specyfikacja przedstawia informacje o składni programowania w języku C# za pomocą dwóch gramatyki.</span><span class="sxs-lookup"><span data-stu-id="2b658-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="2b658-113">***Gramatyka leksykalna*** ([gramatyka Leksykalna](lexical-structure.md#lexical-grammar)) definiuje, jak znaki Unicode są łączone w celu terminatory linii w formularzu, biały znak, komentarze, tokenów i dyrektywy przetwarzania wstępnego.</span><span class="sxs-lookup"><span data-stu-id="2b658-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="2b658-114">***Składni gramatyki*** ([składni gramatyki](lexical-structure.md#syntactic-grammar)) definiuje, jak tokeny, wynikające z gramatyka leksykalna są łączone w celu programy formularzy C#.</span><span class="sxs-lookup"><span data-stu-id="2b658-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="2b658-115">Notacja gramatyki</span><span class="sxs-lookup"><span data-stu-id="2b658-115">Grammar notation</span></span>

<span data-ttu-id="2b658-116">Gramatyki leksykalne i składniowych są prezentowane w formularz Backus-naur przy użyciu notacji ANTLR narzędzie gramatyki.</span><span class="sxs-lookup"><span data-stu-id="2b658-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="2b658-117">Gramatyka leksykalna</span><span class="sxs-lookup"><span data-stu-id="2b658-117">Lexical grammar</span></span>

<span data-ttu-id="2b658-118">Gramatyka leksykalna języka C# są prezentowane w [poddawać analizie Leksykalnej](lexical-structure.md#lexical-analysis), [tokenów](lexical-structure.md#tokens), i [przetwarzania wstępnego dyrektywy](lexical-structure.md#pre-processing-directives).</span><span class="sxs-lookup"><span data-stu-id="2b658-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="2b658-119">Terminalu symbole gramatyka leksykalna są znaki z zestawu znaków Unicode i gramatyka leksykalna Określa, jak znaki są łączone w celu tokenów formularza ([tokenów](lexical-structure.md#tokens)), biały ([biały](lexical-structure.md#white-space)), komentarze ([komentarze](lexical-structure.md#comments)) oraz dyrektywy przetwarzania wstępnego ([przetwarzania wstępnego dyrektywy](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="2b658-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="2b658-120">Każdy plik źródłowy w języku C# służącego musi być zgodna z *wejściowych* wersji produkcyjnej, gramatyka leksykalna ([poddawać analizie Leksykalnej](lexical-structure.md#lexical-analysis)).</span><span class="sxs-lookup"><span data-stu-id="2b658-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="2b658-121">Gramatyka składni</span><span class="sxs-lookup"><span data-stu-id="2b658-121">Syntactic grammar</span></span>

<span data-ttu-id="2b658-122">Gramatyka składni języka C# są prezentowane w rozdziałach i dodatki, które należy wykonać w tym rozdziale.</span><span class="sxs-lookup"><span data-stu-id="2b658-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="2b658-123">Terminalu symbole gramatyki składniowe są tokeny zdefiniowane przez gramatyka leksykalna i gramatyki składni Określa, jak tokeny są łączone w celu formularza C# programy.</span><span class="sxs-lookup"><span data-stu-id="2b658-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="2b658-124">Każdy plik źródłowy w C# program musi być zgodna z *compilation_unit* produkcji składni gramatyki ([jednostki kompilacji](namespaces.md#compilation-units)).</span><span class="sxs-lookup"><span data-stu-id="2b658-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="2b658-125">Poddawać analizie leksykalnej</span><span class="sxs-lookup"><span data-stu-id="2b658-125">Lexical analysis</span></span>

<span data-ttu-id="2b658-126">*Wejściowych* produkcji definiuje strukturę leksykalne plik źródłowy C#.</span><span class="sxs-lookup"><span data-stu-id="2b658-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="2b658-127">Każdy plik źródłowy w języku C# służącego musi być zgodna z ten produkcji gramatyka leksykalna.</span><span class="sxs-lookup"><span data-stu-id="2b658-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

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

<span data-ttu-id="2b658-128">Pięć podstawowych elementów tworzą struktura leksykalna C# pliku źródłowego: Wiersz terminatory ([wiersz terminatory](lexical-structure.md#line-terminators)), biały ([biały](lexical-structure.md#white-space)), komentarze ([komentarze](lexical-structure.md#comments)), tokeny ([tokenów](lexical-structure.md#tokens)), i Przetwarzanie wstępne dyrektywy ([przetwarzania wstępnego dyrektywy](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="2b658-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="2b658-129">Z tych podstawowych elementów tylko tokeny są istotne w gramatyce składni programu w języku C# ([składni gramatyki](lexical-structure.md#syntactic-grammar)).</span><span class="sxs-lookup"><span data-stu-id="2b658-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="2b658-130">Leksykalne przetwarzanie plik źródłowy C# zawiera zmniejszenie pliku sekwencję tokenów, która staje się dane wejściowe do analizy składni.</span><span class="sxs-lookup"><span data-stu-id="2b658-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="2b658-131">Terminatory wiersza, biały i komentarze, które może służyć do oddzielania tokenów, dyrektywy przetwarzania wstępnego mogą powodować części pliku źródłowego do pominięcia, a w przeciwnym razie te elementy leksykalne nie mają wpływu na strukturę składni programu w języku C#.</span><span class="sxs-lookup"><span data-stu-id="2b658-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="2b658-132">W przypadku literały ciągów znaków interpolowanych ([interpolowane literałów ciągów](lexical-structure.md#interpolated-string-literals)) pojedynczy token początkowo jest generowany przez poddawać analizie leksykalnej, ale został podzielony na kilka elementów wejściowych, które wielokrotnie poddać analizie leksykalnej dopóki wszystkie literały ciągów znaków interpolowanych zostały rozwiązane.</span><span class="sxs-lookup"><span data-stu-id="2b658-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="2b658-133">Tokenami wynikowymi następnie służyć jako dane wejściowe do analizy składni.</span><span class="sxs-lookup"><span data-stu-id="2b658-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="2b658-134">Po kilku produkcji gramatyka leksykalna odpowiada sekwencji znaków w pliku źródłowym, leksykalne przetwarzania zawsze stanowi element najdłuższy leksykalne możliwe.</span><span class="sxs-lookup"><span data-stu-id="2b658-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="2b658-135">Na przykład sekwencja znaków `//` jest przetwarzany jako początek komentarz jednowierszowy, ponieważ leksykalne elementu jest dłuższy niż jeden `/` tokenu.</span><span class="sxs-lookup"><span data-stu-id="2b658-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="2b658-136">Terminatory wiersza</span><span class="sxs-lookup"><span data-stu-id="2b658-136">Line terminators</span></span>

<span data-ttu-id="2b658-137">Terminatory wiersza podzielić wiersze znaki plik źródłowy C#.</span><span class="sxs-lookup"><span data-stu-id="2b658-137">Line terminators divide the characters of a C# source file into lines.</span></span>

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

<span data-ttu-id="2b658-138">Dla zgodności przy użyciu źródła code narzędzia edycji Dodaj znaczniki końca pliku, a umożliwiające źródło pliku do wyświetlenia jako sekwencja prawidłowo zakończony wierszy, następujące przekształcenia są stosowane w kolejności, aby każdy plik źródłowy w języku C# służącego:</span><span class="sxs-lookup"><span data-stu-id="2b658-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="2b658-139">Jeśli ostatni znak pliku źródłowego jest znakiem formantu-Z (`U+001A`), ten znak jest usuwany.</span><span class="sxs-lookup"><span data-stu-id="2b658-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="2b658-140">Znak powrotu karetki (`U+000D`) zostanie dodany na końcu pliku źródłowego, jeśli ten plik źródłowy jest pusty, a ostatni znak pliku źródłowego nie jest znak powrotu karetki (`U+000D`), znak wysuwu wiersza (`U+000A`), separator wiersza (`U+2028`), lub separatorem akapitów (`U+2029`).</span><span class="sxs-lookup"><span data-stu-id="2b658-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="2b658-141">Komentarze</span><span class="sxs-lookup"><span data-stu-id="2b658-141">Comments</span></span>

<span data-ttu-id="2b658-142">Obsługiwane są dwa rodzaje komentarzy: Komentarze jednowierszowe i rozdzielany komentarzy.</span><span class="sxs-lookup"><span data-stu-id="2b658-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="2b658-143">***Komentarze jednowierszowe*** rozpoczynających się od znaków `//` i Rozszerz do końca wiersza źródłowego.</span><span class="sxs-lookup"><span data-stu-id="2b658-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="2b658-144">***Rozdzielany komentarze*** rozpoczynających się od znaków `/*` oraz kończyć się znakami `*/`.</span><span class="sxs-lookup"><span data-stu-id="2b658-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="2b658-145">Rozdzielany komentarze może obejmować wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="2b658-145">Delimited comments may span multiple lines.</span></span>

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

<span data-ttu-id="2b658-146">Nie zagnieżdżaj komentarzy.</span><span class="sxs-lookup"><span data-stu-id="2b658-146">Comments do not nest.</span></span> <span data-ttu-id="2b658-147">Sekwencje znaków `/*` i `*/` nie mają specjalnego znaczenia w ramach `//` komentarz i sekwencje znaków `//` i `/*` nie mają specjalnego znaczenia w ramach rozdzielany komentarz.</span><span class="sxs-lookup"><span data-stu-id="2b658-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="2b658-148">Komentarze nie są przetwarzane w ramach znakowe i literały.</span><span class="sxs-lookup"><span data-stu-id="2b658-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="2b658-149">Przykład</span><span class="sxs-lookup"><span data-stu-id="2b658-149">The example</span></span>
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
<span data-ttu-id="2b658-150">Zawiera rozdzielaną komentarz.</span><span class="sxs-lookup"><span data-stu-id="2b658-150">includes a delimited comment.</span></span>

<span data-ttu-id="2b658-151">Przykład</span><span class="sxs-lookup"><span data-stu-id="2b658-151">The example</span></span>
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
<span data-ttu-id="2b658-152">Pokazuje kilka Komentarze jednowierszowe.</span><span class="sxs-lookup"><span data-stu-id="2b658-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="2b658-153">Biały znak</span><span class="sxs-lookup"><span data-stu-id="2b658-153">White space</span></span>

<span data-ttu-id="2b658-154">Biały znak jest zdefiniowany jako dowolny znak z klasą Unicode Zs (która zawiera znak spacji), a także znaku tabulacji poziomej, znak tabulacji pionowej i formularzu znaku wysuwu.</span><span class="sxs-lookup"><span data-stu-id="2b658-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="2b658-155">Tokeny</span><span class="sxs-lookup"><span data-stu-id="2b658-155">Tokens</span></span>

<span data-ttu-id="2b658-156">Istnieje kilka rodzajów tokenów: identyfikatory, słowa kluczowe, literały, operatorów i przerywniki języka.</span><span class="sxs-lookup"><span data-stu-id="2b658-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="2b658-157">Biały znak i komentarze nie są tokeny, chociaż działają jako separatory tokenów.</span><span class="sxs-lookup"><span data-stu-id="2b658-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

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

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="2b658-158">Sekwencje ucieczki znaków Unicode</span><span class="sxs-lookup"><span data-stu-id="2b658-158">Unicode character escape sequences</span></span>

<span data-ttu-id="2b658-159">Znak sekwencji ucieczki Unicode reprezentuje znak Unicode.</span><span class="sxs-lookup"><span data-stu-id="2b658-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="2b658-160">Sekwencje ucieczki znaków Unicode są przetwarzane w identyfikatorach ([identyfikatory](lexical-structure.md#identifiers)), znaków w literałach ([literały znakowe](lexical-structure.md#character-literals)) i regularnego literałów ([Literałyciągu](lexical-structure.md#string-literals)).</span><span class="sxs-lookup"><span data-stu-id="2b658-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="2b658-161">Znak ucieczki Unicode nie są przetwarzane w innym miejscu (na przykład w celu utworzenia operatora, znak interpunkcyjny lub słowa kluczowego).</span><span class="sxs-lookup"><span data-stu-id="2b658-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="2b658-162">Sekwencja unikowa Unicode reprezentuje pojedynczy znak Unicode sformułowany wykonując liczb szesnastkowych "`\u`"or"`\U`" znaków.</span><span class="sxs-lookup"><span data-stu-id="2b658-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="2b658-163">Ponieważ języka C# używa kodowania 16-bitowych punkty kodowe Unicode i ciągi znaków, znak Unicode z zakresu od U + 10000 do U + 10FFFF nie jest dozwolona w literale znakowym i jest reprezentowana w literale ciągu za pomocą para zastępcza Unicode.</span><span class="sxs-lookup"><span data-stu-id="2b658-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="2b658-164">Znaki Unicode z punktów kod powyżej 0x10FFFF nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="2b658-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="2b658-165">Wiele tłumaczeń nie są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="2b658-165">Multiple translations are not performed.</span></span> <span data-ttu-id="2b658-166">Na przykład, literał ciągu "`\u005Cu005C`"jest odpowiednikiem"`\u005C`"zamiast"`\`".</span><span class="sxs-lookup"><span data-stu-id="2b658-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="2b658-167">Wartość Unicode `\u005C` jest znakiem "`\`".</span><span class="sxs-lookup"><span data-stu-id="2b658-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="2b658-168">Przykład</span><span class="sxs-lookup"><span data-stu-id="2b658-168">The example</span></span>
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
<span data-ttu-id="2b658-169">Pokazuje kilka zastosowań `\u0066`, czyli sekwencji unikowej na literę "`f`".</span><span class="sxs-lookup"><span data-stu-id="2b658-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="2b658-170">Program jest odpowiednikiem</span><span class="sxs-lookup"><span data-stu-id="2b658-170">The program is equivalent to</span></span>
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

### <a name="identifiers"></a><span data-ttu-id="2b658-171">Identyfikatory</span><span class="sxs-lookup"><span data-stu-id="2b658-171">Identifiers</span></span>

<span data-ttu-id="2b658-172">Reguły dotyczące identyfikatorów podane w tej sekcji dokładnie odpowiadać elementowi zalecane przez Unicode Standard załącznika 31, z tą różnicą, że podkreślenie jest dozwolona jako znak początkowy (tak jak to tradycyjne w języku programowania C), specjalne Unicode, który sekwencje to dozwolone w identyfikatorach, a "`@`" znak jest dozwolony jako prefiksu potrzeba włączenia słów kluczowych jako identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="2b658-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

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

<span data-ttu-id="2b658-173">Instrukcje dotyczące klas znaków Unicode, które są wymienione powyżej Zobacz Unicode Standard, w wersji 3.0, sekcja 4.5.</span><span class="sxs-lookup"><span data-stu-id="2b658-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="2b658-174">Przykłady prawidłowych identyfikatorów "`identifier1`","`_identifier2`", a "`@if`".</span><span class="sxs-lookup"><span data-stu-id="2b658-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="2b658-175">Identyfikator programu odpowiadające musi być w formacie kanonicznym definicją formularza normalizacji Unicode C, zgodnie z definicją standardu Unicode Standard załącznika 15.</span><span class="sxs-lookup"><span data-stu-id="2b658-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="2b658-176">Zachowanie w przypadku napotkania identyfikator nie jest w formularzu C normalizacji jest zdefiniowane w implementacji; jednak Diagnostyka nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="2b658-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="2b658-177">Prefiks "`@`" umożliwia słów kluczowych jako identyfikatorów, co jest przydatne, gdy komunikuje się z innymi językami programowania.</span><span class="sxs-lookup"><span data-stu-id="2b658-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="2b658-178">Znak `@` nie jest częścią identyfikatora, więc identyfikator może być widoczny w innych językach, jako identyfikator normalne, bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="2b658-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="2b658-179">Identyfikator z `@` nosi nazwę prefiks ***identyfikator dosłownego wyrażenia***.</span><span class="sxs-lookup"><span data-stu-id="2b658-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="2b658-180">Korzystanie z `@` prefiks dla identyfikatorów, które nie są słowami kluczowymi jest dozwolone, ale zdecydowanie niezalecane jako stylu.</span><span class="sxs-lookup"><span data-stu-id="2b658-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="2b658-181">Przykład:</span><span class="sxs-lookup"><span data-stu-id="2b658-181">The example:</span></span>
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
<span data-ttu-id="2b658-182">definiuje klasę o nazwie "`class`"ze statyczną metodę o nazwie"`static`"pobierającej parametr o nazwie"`bool`".</span><span class="sxs-lookup"><span data-stu-id="2b658-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="2b658-183">Należy zauważyć, że ponieważ specjalne Unicode nie są dozwolone w słów kluczowych, token "`cl\u0061ss`"jest identyfikatorem i jest taki sam identyfikator jak"`@class`".</span><span class="sxs-lookup"><span data-stu-id="2b658-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="2b658-184">Dwa identyfikatory są uważane za takie same, jeśli są one identyczne, po zastosowaniu następujące przekształcenia w kolejności:</span><span class="sxs-lookup"><span data-stu-id="2b658-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="2b658-185">Prefiks "`@`", jeśli używany, zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="2b658-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="2b658-186">Każdy *unicode_escape_sequence* jest przekształcana na jego odpowiedniego znaku Unicode.</span><span class="sxs-lookup"><span data-stu-id="2b658-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="2b658-187">Wszelkie *formatting_character*s są usuwane.</span><span class="sxs-lookup"><span data-stu-id="2b658-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="2b658-188">Identyfikatory zawierające dwóch następujących po sobie znaki podkreślenia (`U+005F`) są zarezerwowane do użytku przez implementację.</span><span class="sxs-lookup"><span data-stu-id="2b658-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="2b658-189">Na przykład implementacja może zawierać słów kluczowych rozszerzonych, które zaczynają się od dwóch podkreśleń.</span><span class="sxs-lookup"><span data-stu-id="2b658-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="2b658-190">Słowa kluczowe</span><span class="sxs-lookup"><span data-stu-id="2b658-190">Keywords</span></span>

<span data-ttu-id="2b658-191">A ***— słowo kluczowe*** to identyfikator jak sekwencja znaków jest zarezerwowany i nie można użyć jako identyfikatora z wyjątkiem sytuacji, gdy są poprzedzone `@` znaków.</span><span class="sxs-lookup"><span data-stu-id="2b658-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

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

<span data-ttu-id="2b658-192">W pewnych miejscach w gramatyce określone identyfikatory mają specjalne znaczenie, ale nie są słowami kluczowymi.</span><span class="sxs-lookup"><span data-stu-id="2b658-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="2b658-193">Takie identyfikatory są czasami określane jako "kontekstowymi słowami kluczowymi".</span><span class="sxs-lookup"><span data-stu-id="2b658-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="2b658-194">Na przykład w deklaracji właściwości "`get`"i"`set`" identyfikatory mają specjalne znaczenie ([Akcesory](classes.md#accessors)).</span><span class="sxs-lookup"><span data-stu-id="2b658-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="2b658-195">Identyfikator innego niż `get` lub `set` nigdy nie jest dozwolona w tych lokalizacjach, więc to zastosowanie nie powoduje konfliktu z użyciem tych słów jako identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="2b658-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="2b658-196">W innych przypadkach, takich jak o identyfikatorze "`var`" w niejawnie wpisane deklaracje zmiennych lokalnych ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)), kontekstowe słowo kluczowe, mogą powodować konflikt z nazwami zadeklarowane.</span><span class="sxs-lookup"><span data-stu-id="2b658-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="2b658-197">W takich przypadkach zadeklarowana nazwa mają pierwszeństwo przed użyciem identyfikator kontekstowego słowa kluczowego.</span><span class="sxs-lookup"><span data-stu-id="2b658-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="2b658-198">Literały</span><span class="sxs-lookup"><span data-stu-id="2b658-198">Literals</span></span>

<span data-ttu-id="2b658-199">A ***literału*** jest reprezentacją kod źródłowy wartość.</span><span class="sxs-lookup"><span data-stu-id="2b658-199">A ***literal*** is a source code representation of a value.</span></span>

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

#### <a name="boolean-literals"></a><span data-ttu-id="2b658-200">Wartość logiczna literałów</span><span class="sxs-lookup"><span data-stu-id="2b658-200">Boolean literals</span></span>

<span data-ttu-id="2b658-201">Istnieją dwie wartości literałów wartości logicznych: `true` i `false`.</span><span class="sxs-lookup"><span data-stu-id="2b658-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="2b658-202">Typ *boolean_literal* jest `bool`.</span><span class="sxs-lookup"><span data-stu-id="2b658-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="2b658-203">Literały całkowite</span><span class="sxs-lookup"><span data-stu-id="2b658-203">Integer literals</span></span>

<span data-ttu-id="2b658-204">Literały całkowite służy do zapisywania wartości typów `int`, `uint`, `long`, i `ulong`.</span><span class="sxs-lookup"><span data-stu-id="2b658-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="2b658-205">Literały całkowite mają dwa możliwe formy: dziesiętną, a szesnastkową.</span><span class="sxs-lookup"><span data-stu-id="2b658-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

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

<span data-ttu-id="2b658-206">Typ literału typu integer jest określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2b658-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="2b658-207">Jeśli literał ma Brak przyrostka, ma to pierwsza z tych typów, w których jej wartość może być reprezentowana: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="2b658-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="2b658-208">Jeśli jako sufiks literału przez `U` lub `u`, ma to pierwsza z tych typów, w których jej wartość może być reprezentowana: `uint`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="2b658-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="2b658-209">Jeśli jako sufiks literału przez `L` lub `l`, ma to pierwsza z tych typów, w których jej wartość może być reprezentowana: `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="2b658-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="2b658-210">Jeśli jako sufiks literału przez `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, lub `lu`, jest on typu `ulong`.</span><span class="sxs-lookup"><span data-stu-id="2b658-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="2b658-211">Jeśli wartością reprezentowaną przez literał liczby całkowitej jest poza zakresem `ulong` typu, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2b658-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="2b658-212">Jako styl, zalecane jest, "`L`"można użyć zamiast"`l`" podczas zapisywania literały ciągów typu `long`, ponieważ łatwo pomylić literę "`l`"z cyfrą"`1`".</span><span class="sxs-lookup"><span data-stu-id="2b658-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="2b658-213">Aby zezwolić na najmniejszą możliwą `int` i `long` wartości do zapisania jako dziesiętna liczba całkowita literały, istnieją następujące dwie reguły:</span><span class="sxs-lookup"><span data-stu-id="2b658-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="2b658-214">Gdy *decimal_integer_literal* wartością 2147483648 (2 ^ 31) i nie *integer_type_suffix* pojawia się jako token bezpośrednio po jednoargumentowego znaku minusa token operatora ([minus jednoargumentowy operator](expressions.md#unary-minus-operator)), wynik jest stałą typu `int` o wartości od -2147483648 (-2 ^ 31).</span><span class="sxs-lookup"><span data-stu-id="2b658-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="2b658-215">W innych sytuacjach takich *decimal_integer_literal* typu `uint`.</span><span class="sxs-lookup"><span data-stu-id="2b658-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="2b658-216">Gdy *decimal_integer_literal* wartością 9223372036854775808 (2 ^ 63) i nie *integer_type_suffix* lub *integer_type_suffix* `L` lub `l` pojawia się jako token bezpośrednio po jednoargumentowego znaku minusa token operatora ([jednoargumentowy minus operator](expressions.md#unary-minus-operator)), wynik jest stałą typu `long` wartością wartość -9223372036854775808 (-2 ^ 63).</span><span class="sxs-lookup"><span data-stu-id="2b658-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="2b658-217">W innych sytuacjach takich *decimal_integer_literal* typu `ulong`.</span><span class="sxs-lookup"><span data-stu-id="2b658-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="2b658-218">Literały rzeczywistych</span><span class="sxs-lookup"><span data-stu-id="2b658-218">Real literals</span></span>

<span data-ttu-id="2b658-219">Literały rzeczywistych służy do zapisywania wartości typów `float`, `double`, i `decimal`.</span><span class="sxs-lookup"><span data-stu-id="2b658-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

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

<span data-ttu-id="2b658-220">Jeśli nie *real_type_suffix* określono typ rzeczywistego literał jest `double`.</span><span class="sxs-lookup"><span data-stu-id="2b658-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="2b658-221">W przeciwnym razie sufiks rzeczywistego typu, określa typ rzeczywistego literału w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2b658-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="2b658-222">Literał rzeczywisty sufiks przez `F` lub `f` typu `float`.</span><span class="sxs-lookup"><span data-stu-id="2b658-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="2b658-223">Na przykład literały `1f`, `1.5f`, `1e10f`, i `123.456F` są wszystkie typu `float`.</span><span class="sxs-lookup"><span data-stu-id="2b658-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="2b658-224">Literał rzeczywisty sufiks przez `D` lub `d` typu `double`.</span><span class="sxs-lookup"><span data-stu-id="2b658-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="2b658-225">Na przykład literały `1d`, `1.5d`, `1e10d`, i `123.456D` są wszystkie typu `double`.</span><span class="sxs-lookup"><span data-stu-id="2b658-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="2b658-226">Literał rzeczywisty sufiks przez `M` lub `m` typu `decimal`.</span><span class="sxs-lookup"><span data-stu-id="2b658-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="2b658-227">Na przykład literały `1m`, `1.5m`, `1e10m`, i `123.456M` są wszystkie typu `decimal`.</span><span class="sxs-lookup"><span data-stu-id="2b658-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="2b658-228">Ten literał jest konwertowany na `decimal` wartość biorąc dokładna wartość, a w razie potrzeby zaokrąglania do najbliższej przy użyciu wartości reprezentowanych przez zaokrąglenie banker ([typu dziesiętnego](types.md#the-decimal-type)).</span><span class="sxs-lookup"><span data-stu-id="2b658-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="2b658-229">Dowolnej skali, które są widoczne w literale są zachowywane, chyba że wartość jest zaokrąglana, lub wartość wynosi zero (w takim przypadku ostatnie logowania i skalowanie będzie 0).</span><span class="sxs-lookup"><span data-stu-id="2b658-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="2b658-230">Z tego powodu literału `2.900m` będzie analizowany w celu utworzenia dziesiętnych znakiem `0`, współczynnik `2900`i skalowanie `3`.</span><span class="sxs-lookup"><span data-stu-id="2b658-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="2b658-231">Jeśli określony literał nie może być przedstawiony w wskazanego typu, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2b658-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="2b658-232">Wartość rzeczywistego literału typu `float` lub `double` jest określana za pomocą IEEE tryb "zaokrąglony do najbliższej".</span><span class="sxs-lookup"><span data-stu-id="2b658-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="2b658-233">Należy pamiętać, że w rzeczywistych literał, cyfry dziesiętne zawsze są wymagane po przecinku.</span><span class="sxs-lookup"><span data-stu-id="2b658-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="2b658-234">Na przykład `1.3F` jest literał liczby rzeczywistej ale `1.F` nie jest.</span><span class="sxs-lookup"><span data-stu-id="2b658-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="2b658-235">Literały znakowe</span><span class="sxs-lookup"><span data-stu-id="2b658-235">Character literals</span></span>

<span data-ttu-id="2b658-236">Literał znakowy, który reprezentuje pojedynczy znak, a składa się zwykle znaku w cudzysłowie, jak w `'a'`.</span><span class="sxs-lookup"><span data-stu-id="2b658-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="2b658-237">Uwaga: Notacja gramatyki ANTLR sprawia, że następujące mylące!</span><span class="sxs-lookup"><span data-stu-id="2b658-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="2b658-238">W ANTLR, kiedy piszesz `\'` oznacza pojedynczy cudzysłów `'`.</span><span class="sxs-lookup"><span data-stu-id="2b658-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="2b658-239">I podczas wpisywania `\\` oznacza pojedynczy ukośnik odwrotny `\`.</span><span class="sxs-lookup"><span data-stu-id="2b658-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="2b658-240">W związku z tym pierwsze reguły dla literału znakowego oznacza, że zaczyna się od pojedynczy cudzysłów znaków, a następnie pojedynczy cudzysłów.</span><span class="sxs-lookup"><span data-stu-id="2b658-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="2b658-241">I jedenaście możliwe proste sekwencje ucieczki są `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span><span class="sxs-lookup"><span data-stu-id="2b658-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

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

<span data-ttu-id="2b658-242">Znak, który następuje znak ukośnika odwrotnego (`\`) w *znak* musi mieć jedną z następujących znaków: `'`, `"`, `\`, `0`, `a`, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="2b658-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="2b658-243">W przeciwnym razie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2b658-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="2b658-244">Szesnastkowa sekwencja unikowa reprezentuje pojedynczy znak Unicode o wartości sformułowany wykonując liczb szesnastkowych "`\x`".</span><span class="sxs-lookup"><span data-stu-id="2b658-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="2b658-245">Jeśli wartością reprezentowaną przez literału znakowego jest większa niż `U+FFFF`, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2b658-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="2b658-246">Sekwencje znaków Unicode ([sekwencje ucieczki znaków Unicode](lexical-structure.md#unicode-character-escape-sequences)) w literale znakowym musi należeć do zakresu `U+0000` do `U+FFFF`.</span><span class="sxs-lookup"><span data-stu-id="2b658-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="2b658-247">Proste sekwencje reprezentuje kodowania znaków Unicode, zgodnie z opisem w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="2b658-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="2b658-248">__Sekwencja unikowa__</span><span class="sxs-lookup"><span data-stu-id="2b658-248">__Escape sequence__</span></span> | <span data-ttu-id="2b658-249">__Nazwa znaków__</span><span class="sxs-lookup"><span data-stu-id="2b658-249">__Character name__</span></span> | <span data-ttu-id="2b658-250">__Kodowanie Unicode__</span><span class="sxs-lookup"><span data-stu-id="2b658-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="2b658-251">pojedynczy cudzysłów</span><span class="sxs-lookup"><span data-stu-id="2b658-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="2b658-252">podwójny cudzysłów</span><span class="sxs-lookup"><span data-stu-id="2b658-252">Double quote</span></span>       | `0x0022`             | 
| `\\`                | <span data-ttu-id="2b658-253">Ukośnik odwrotny</span><span class="sxs-lookup"><span data-stu-id="2b658-253">Backslash</span></span>          | `0x005C`             | 
| `\0`                | <span data-ttu-id="2b658-254">Null</span><span class="sxs-lookup"><span data-stu-id="2b658-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="2b658-255">Alerty</span><span class="sxs-lookup"><span data-stu-id="2b658-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="2b658-256">Backspace</span><span class="sxs-lookup"><span data-stu-id="2b658-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="2b658-257">Wysuw strony</span><span class="sxs-lookup"><span data-stu-id="2b658-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="2b658-258">Nowy wiersz</span><span class="sxs-lookup"><span data-stu-id="2b658-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="2b658-259">Powrót karetki</span><span class="sxs-lookup"><span data-stu-id="2b658-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="2b658-260">tabulator poziomy</span><span class="sxs-lookup"><span data-stu-id="2b658-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="2b658-261">tabulator pionowy</span><span class="sxs-lookup"><span data-stu-id="2b658-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="2b658-262">Typ *character_literal* jest `char`.</span><span class="sxs-lookup"><span data-stu-id="2b658-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="2b658-263">Literały ciągu</span><span class="sxs-lookup"><span data-stu-id="2b658-263">String literals</span></span>

<span data-ttu-id="2b658-264">C# obsługuje dwa rodzaje literałów ciągów: ***literałów ciągów regularne*** i ***literały ciąg verbatim***.</span><span class="sxs-lookup"><span data-stu-id="2b658-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="2b658-265">Literał ciągu regularne składa się z zero lub więcej znaków ujęte w cudzysłów, podobnie jak w `"hello"`i mogą obejmować zarówno proste sekwencje ucieczki (takie jak `\t` na znak tabulacji) i szesnastkowym, jak i sekwencje unikowe Unicode.</span><span class="sxs-lookup"><span data-stu-id="2b658-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="2b658-266">Literał ciągu verbatim składa się z `@` znaków, po której następuje znak podwójnego cudzysłowu, zero lub więcej znaków i znaku podwójnego cudzysłowu zamykającego.</span><span class="sxs-lookup"><span data-stu-id="2b658-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="2b658-267">Prosty przykład `@"hello"`.</span><span class="sxs-lookup"><span data-stu-id="2b658-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="2b658-268">Literał ciągu verbatim, znaków między ogranicznikami interpretuje dosłownie, tylko on wyjątek *quote_escape_sequence*.</span><span class="sxs-lookup"><span data-stu-id="2b658-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="2b658-269">W szczególności proste sekwencje ucieczki i szesnastkowym i sekwencje unikowe Unicode nie są przetwarzane w literałach ciąg verbatim.</span><span class="sxs-lookup"><span data-stu-id="2b658-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="2b658-270">Literał ciągu verbatim może obejmować wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="2b658-270">A verbatim string literal may span multiple lines.</span></span>

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

<span data-ttu-id="2b658-271">Znak, który następuje znak ukośnika odwrotnego (`\`) w *regular_string_literal_character* musi mieć jedną z następujących znaków: `'`, `"`, `\`, `0`, `a` , `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="2b658-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="2b658-272">W przeciwnym razie wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2b658-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="2b658-273">Przykład</span><span class="sxs-lookup"><span data-stu-id="2b658-273">The example</span></span>
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
<span data-ttu-id="2b658-274">przedstawia różne literały ciągu.</span><span class="sxs-lookup"><span data-stu-id="2b658-274">shows a variety of string literals.</span></span> <span data-ttu-id="2b658-275">Ostatnim ciągiem literału, `j`, jest verbatim literału ciągu obejmującej wiele wierszy.</span><span class="sxs-lookup"><span data-stu-id="2b658-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="2b658-276">Znaki między znakami cudzysłowu, w tym znak odstępu, takie jak znaki nowego wiersza są zachowywane verbatim.</span><span class="sxs-lookup"><span data-stu-id="2b658-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="2b658-277">Ponieważ szesnastkowa sekwencja unikowa mogą mieć różną liczbą znaków szesnastkowych, literał ciągu `"\x123"` zawiera pojedynczy znak z wartości szesnastkowej 123.</span><span class="sxs-lookup"><span data-stu-id="2b658-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="2b658-278">Aby utworzyć ciąg zawierający znak wartości szesnastkowych 12 następuje znak 3, jeden napisać `"\x00123"` lub `"\x12" + "3"` zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="2b658-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="2b658-279">Typ *string_literal* jest `string`.</span><span class="sxs-lookup"><span data-stu-id="2b658-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="2b658-280">Literał ciągu nie zawsze skutkuje nowego wystąpienia ciągu.</span><span class="sxs-lookup"><span data-stu-id="2b658-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="2b658-281">Po co najmniej dwóch Literały ciągu, są równoważne zgodnie z operatora równości ciągu ([operatory porównania ciągów](expressions.md#string-equality-operators)) są wyświetlane w tym samym programie te literałów ciągów odwołują się do tego samego wystąpienia ciągu.</span><span class="sxs-lookup"><span data-stu-id="2b658-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="2b658-282">Na przykład dane wyjściowe wytwarzane przez</span><span class="sxs-lookup"><span data-stu-id="2b658-282">For instance, the output produced by</span></span>
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
<span data-ttu-id="2b658-283">jest `True` ponieważ dwa literały odwołują się do tego samego wystąpienia ciągu.</span><span class="sxs-lookup"><span data-stu-id="2b658-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="2b658-284">Literały ciągu interpolowanego</span><span class="sxs-lookup"><span data-stu-id="2b658-284">Interpolated string literals</span></span>

<span data-ttu-id="2b658-285">Literały ciągów znaków interpolowanych są podobne do Literały ciągu, ale zawiera luki rozdzielone `{` i `}`, którym określeń mogą wystąpić.</span><span class="sxs-lookup"><span data-stu-id="2b658-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="2b658-286">W czasie wykonywania są obliczane wyrażenia, mający na celu ułatwienie o swoje formularze tekstową zamieniony na ciąg w miejscu, w którym występuje dziura.</span><span class="sxs-lookup"><span data-stu-id="2b658-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="2b658-287">Składnia i semantyka Interpolacja ciągów, które są opisane w sekcji ([ciągi interpolowane](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="2b658-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="2b658-288">Jak literałów ciągów literałów ciągu interpolowanego może być regularnie lub verbatim.</span><span class="sxs-lookup"><span data-stu-id="2b658-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="2b658-289">Literały ciągu interpolowanego regularnych są rozdzielane znakami `$"` i `"`, i literały ciągu interpolowanego verbatim są rozdzielane znakami `$@"` i `"`.</span><span class="sxs-lookup"><span data-stu-id="2b658-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="2b658-290">Podobnie jak inne literały poddawać analizie leksykalnej literału ciągu interpolowanego początkowo powoduje pojedynczy token, zgodnie z poniższym gramatyki.</span><span class="sxs-lookup"><span data-stu-id="2b658-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="2b658-291">Jednak przed analizą składni pojedynczy token literału ciągu interpolowanego jest dzielony na kilka tokenów dla części ciągu otaczający otwory i elementów wejściowych pojawiają się w otwory leksykalnie analizuje się ponownie.</span><span class="sxs-lookup"><span data-stu-id="2b658-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="2b658-292">Z kolei może to zwrócić więcej interpolowane literałów ciągów do przetworzenia, ale, jeśli leksykalnie rozwiązać, ostatecznie doprowadzi do sekwencja tokeny składni analizy do przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="2b658-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

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

<span data-ttu-id="2b658-293">*Interpolated_string_literal* tokenu to ponowne interpretowanie jako wiele tokenów i inne dane wejściowe elementów w następujący sposób, w kolejności ich występowania w *interpolated_string_literal*:</span><span class="sxs-lookup"><span data-stu-id="2b658-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="2b658-294">Wystąpienia następujących są reinterpretowane jako osobne oddzielne tokeny: wiodące `$` logowania, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*,  *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* i *interpolated_verbatim_string_end*.</span><span class="sxs-lookup"><span data-stu-id="2b658-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="2b658-295">Wystąpienia *regular_balanced_text* i *verbatim_balanced_text* między tymi są ponownie przetwarzany jako *input_section* ([poddawać analizie Leksykalnej ](lexical-structure.md#lexical-analysis)) i są reinterpretowane jako wynikowa sekwencja elementów wejściowych.</span><span class="sxs-lookup"><span data-stu-id="2b658-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="2b658-296">Z kolei mogą one obejmować tokenów literałów ciągu interpolowanego ponowne interpretowanie.</span><span class="sxs-lookup"><span data-stu-id="2b658-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="2b658-297">Analiza składni zostanie następnie tokenów do *interpolated_string_expression* ([ciągi interpolowane](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="2b658-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="2b658-298">Przykłady zadań do wykonania</span><span class="sxs-lookup"><span data-stu-id="2b658-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="2b658-299">Literał null</span><span class="sxs-lookup"><span data-stu-id="2b658-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="2b658-300">*Null_literal* mogą być niejawnie konwertowane na typ referencyjny lub typ dopuszczający wartość null.</span><span class="sxs-lookup"><span data-stu-id="2b658-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="2b658-301">Operatory i przerywniki języka</span><span class="sxs-lookup"><span data-stu-id="2b658-301">Operators and punctuators</span></span>

<span data-ttu-id="2b658-302">Istnieje kilka rodzajów operatory i przerywniki języka.</span><span class="sxs-lookup"><span data-stu-id="2b658-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="2b658-303">Operatory są używane w wyrażeniach opisujący operacje dotyczące jednego lub większej liczbie operandów.</span><span class="sxs-lookup"><span data-stu-id="2b658-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="2b658-304">Na przykład, wyrażenie `a + b` używa `+` operatora, aby dodać dwa operandy `a` i `b`.</span><span class="sxs-lookup"><span data-stu-id="2b658-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="2b658-305">Przerywniki języka służą do grupowania i oddzielając.</span><span class="sxs-lookup"><span data-stu-id="2b658-305">Punctuators are for grouping and separating.</span></span>

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

<span data-ttu-id="2b658-306">Pionowy pasek w *right_shift* i *right_shift_assignment* produkcji są używane do wskazania, że, w przeciwieństwie do innych produkcji w składni gramatykę, żadne znaki dowolnego rodzaju (nawet białych znaków) są dozwolone między tokenami.</span><span class="sxs-lookup"><span data-stu-id="2b658-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="2b658-307">Zaprojektuj te są traktowane specjalnie w celu umożliwienia obsługi poprawne *type_parameter_list*s ([parametry typu](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="2b658-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="2b658-308">Dyrektywy przetwarzania wstępnego</span><span class="sxs-lookup"><span data-stu-id="2b658-308">Pre-processing directives</span></span>

<span data-ttu-id="2b658-309">Dyrektywy przetwarzania wstępnego zapewniają możliwość warunkowo pominąć części plików źródłowych, zgłoszenia błędu i warunków ostrzeżenia i odróżniać odrębne regiony kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="2b658-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="2b658-310">Termin "przetwarzania wstępnego dyrektywy" jest używana tylko w celu zachowania spójności z językami programowania C i C++.</span><span class="sxs-lookup"><span data-stu-id="2b658-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="2b658-311">W języku C# jest nie niezależnym przetwarzania wstępnego; dyrektywy przetwarzania wstępnego są przetwarzane w ramach fazy poddawać analizie leksykalnej.</span><span class="sxs-lookup"><span data-stu-id="2b658-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

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

<span data-ttu-id="2b658-312">Dostępne są następujące dyrektywy przetwarzania wstępnego:</span><span class="sxs-lookup"><span data-stu-id="2b658-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="2b658-313">`#define` i `#undef`, które są używane do definiowania i Usuń odpowiednio symbole kompilacji warunkowej ([dyrektywy deklaracji](lexical-structure.md#declaration-directives)).</span><span class="sxs-lookup"><span data-stu-id="2b658-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="2b658-314">`#if`, `#elif`, `#else`, i `#endif`, które są używane do warunkowo pominąć części kodu źródłowego ([dyrektywy kompilacji warunkowej](lexical-structure.md#conditional-compilation-directives)).</span><span class="sxs-lookup"><span data-stu-id="2b658-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="2b658-315">`#line`, które jest używane do kontrolowania numery wierszy emitowane błędów i ostrzeżeń ([wiersz dyrektywy](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="2b658-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="2b658-316">`#error` i `#warning`, które są używane do wysyłania błędów i ostrzeżeń, odpowiednio ([diagnostycznych dyrektywy](lexical-structure.md#diagnostic-directives)).</span><span class="sxs-lookup"><span data-stu-id="2b658-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="2b658-317">`#region` i `#endregion`, które są używane do wyraźnie oznaczyć części kodu źródłowego ([dyrektywy regionu](lexical-structure.md#region-directives)).</span><span class="sxs-lookup"><span data-stu-id="2b658-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="2b658-318">`#pragma`, używany do określenia opcjonalne informacje kontekstowe w kompilatorze ([dyrektyw Pragma](lexical-structure.md#pragma-directives)).</span><span class="sxs-lookup"><span data-stu-id="2b658-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="2b658-319">Dyrektywy przetwarzania wstępnego zawsze zajmuje w osobnym wierszu kodu źródłowego i zawsze zaczyna się od `#` znak i nazwę dyrektywy przetwarzania wstępnego.</span><span class="sxs-lookup"><span data-stu-id="2b658-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="2b658-320">Biały znak może występować przed `#` znaków oraz między `#` znaków i nazwa dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="2b658-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="2b658-321">Źródło zawierające wiersza `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, lub `#endregion` dyrektywy może kończyć się znakiem jednowierszowego komentarza.</span><span class="sxs-lookup"><span data-stu-id="2b658-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="2b658-322">Lista komentarzy ( `/* */` styl komentarzy) nie są dozwolone w wierszach źródłowych zawierających dyrektywy przetwarzania wstępnego.</span><span class="sxs-lookup"><span data-stu-id="2b658-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="2b658-323">Dyrektywy przetwarzania wstępnego nie są tokenów i nie są częścią składni gramatyki języka C#.</span><span class="sxs-lookup"><span data-stu-id="2b658-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="2b658-324">Jednak dyrektywy przetwarzania wstępnego może służyć do dołączania lub wykluczania sekwencje tokenów i w ten sposób wpływają na znaczenie programu w języku C#.</span><span class="sxs-lookup"><span data-stu-id="2b658-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="2b658-325">Na przykład, gdy kompilowany, program:</span><span class="sxs-lookup"><span data-stu-id="2b658-325">For example, when compiled, the program:</span></span>
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
<span data-ttu-id="2b658-326">wyniki w dokładnie takiej samej kolejności tokenów jako programu:</span><span class="sxs-lookup"><span data-stu-id="2b658-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="2b658-327">W efekcie leksykalnie, dwa programy są zupełnie inny syntaktycznie, są identyczne.</span><span class="sxs-lookup"><span data-stu-id="2b658-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="2b658-328">Symbole kompilacji warunkowej</span><span class="sxs-lookup"><span data-stu-id="2b658-328">Conditional compilation symbols</span></span>

<span data-ttu-id="2b658-329">Kompilacja warunkowa funkcjonalność `#if`, `#elif`, `#else`, i `#endif` dyrektyw jest kontrolowany za pośrednictwem przetwarzania wstępnego wyrażenia ([przetwarzania wstępnego wyrażeń](lexical-structure.md#pre-processing-expressions)) i symbole kompilacji warunkowej.</span><span class="sxs-lookup"><span data-stu-id="2b658-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="2b658-330">Kompilacja warunkowa symbolu ma dwa możliwe stany: ***zdefiniowane*** lub ***niezdefiniowane***.</span><span class="sxs-lookup"><span data-stu-id="2b658-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="2b658-331">Na początku leksykalne przetworzenia pliku źródłowego, symbol kompilacja warunkowa jest niezdefiniowana, chyba że ma je jawnie zdefiniowany przez mechanizm zewnętrznych (takich jak opcja kompilatora wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="2b658-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="2b658-332">Gdy `#define` dyrektywa jest przetwarzany, symbol kompilacji warunkowej, o nazwie w tej dyrektywy staje się zdefiniowany w tym pliku źródłowym.</span><span class="sxs-lookup"><span data-stu-id="2b658-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="2b658-333">Symbol pozostaje zdefiniowany aż `#undef` dyrektywy dla tego samego symbolu jest przetwarzana lub dopóki nie zostanie osiągnięty koniec pliku źródłowego.</span><span class="sxs-lookup"><span data-stu-id="2b658-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="2b658-334">Domniemanie tego jest fakt, że `#define` i `#undef` dyrektywy w jednym pliku źródłowym nie mają wpływu na innych plikach źródłowych, w tym samym programie.</span><span class="sxs-lookup"><span data-stu-id="2b658-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="2b658-335">Gdy do którego odwołuje się wyrażenie przetwarzania wstępnego, symboli zdefiniowanych kompilacji warunkowej ma wartość logiczna `true`, a symbol Niezdefiniowany kompilacji warunkowej ma wartość logiczna `false`.</span><span class="sxs-lookup"><span data-stu-id="2b658-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="2b658-336">Nie jest wymagane, czy symbole kompilacji warunkowej jest jawnie zadeklarowana przed występuje do nich podczas wstępnego przetwarzania wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="2b658-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="2b658-337">Zamiast tego należy niezadeklarowany symbole są po prostu niezdefiniowane i dlatego mają wartość `false`.</span><span class="sxs-lookup"><span data-stu-id="2b658-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="2b658-338">Przestrzeń nazw dla symbole kompilacji warunkowej jest odrębna i oddzielona od innych jednostek o nazwie w programie C#.</span><span class="sxs-lookup"><span data-stu-id="2b658-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="2b658-339">Symbole kompilacji warunkowej mogą być przywoływane tylko w `#define` i `#undef` dyrektywy w przetwarzania wstępnego wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="2b658-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="2b658-340">Przetwarzanie wstępne wyrażeń</span><span class="sxs-lookup"><span data-stu-id="2b658-340">Pre-processing expressions</span></span>

<span data-ttu-id="2b658-341">Przetwarzanie wstępne wyrażeń może wystąpić w `#if` i `#elif` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="2b658-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="2b658-342">Operatory `!`, `==`, `!=`, `&&` i `||` są dozwolone w przetwarzania wstępnego wyrażeń, i nawiasy mogą być używane do grupowania.</span><span class="sxs-lookup"><span data-stu-id="2b658-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

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

<span data-ttu-id="2b658-343">Gdy do którego odwołuje się wyrażenie przetwarzania wstępnego, symboli zdefiniowanych kompilacji warunkowej ma wartość logiczna `true`, a symbol Niezdefiniowany kompilacji warunkowej ma wartość logiczna `false`.</span><span class="sxs-lookup"><span data-stu-id="2b658-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="2b658-344">Obliczanie wyrażenia przetwarzania wstępnego zawsze daje wartość logiczną.</span><span class="sxs-lookup"><span data-stu-id="2b658-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="2b658-345">Reguły oceny wyrażenia przetwarzania wstępnego są takie same jak w przypadku stałego wyrażenia ([wyrażeń stałych](expressions.md#constant-expressions)), z tą różnicą, że tylko obiekty zdefiniowane przez użytkownika, które mogą być przywoływane są symbole kompilacji warunkowej .</span><span class="sxs-lookup"><span data-stu-id="2b658-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="2b658-346">Dyrektywy deklaracji</span><span class="sxs-lookup"><span data-stu-id="2b658-346">Declaration directives</span></span>

<span data-ttu-id="2b658-347">Dyrektywy deklaracji są używane do definiowania lub Usuń symbole kompilacji warunkowej.</span><span class="sxs-lookup"><span data-stu-id="2b658-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="2b658-348">Przetwarzanie `#define` dyrektywy powoduje, że symbol danej kompilacji warunkowej, aby stać się zdefiniowana, począwszy od wiersza źródłowego, który następuje po dyrektywie.</span><span class="sxs-lookup"><span data-stu-id="2b658-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="2b658-349">Podobnie, przetwarzanie `#undef` dyrektywy powoduje, że stają się niezdefiniowane, zaczynając od wiersza źródłowego, który następuje po dyrektywie symbol danej kompilacji warunkowej.</span><span class="sxs-lookup"><span data-stu-id="2b658-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="2b658-350">Wszelkie `#define` i `#undef` dyrektywy w pliku źródłowym musi wystąpić przed pierwszym *tokenu* ([tokenów](lexical-structure.md#tokens)) w pliku źródłowym; w przeciwnym razie błąd kompilacji występuje.</span><span class="sxs-lookup"><span data-stu-id="2b658-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="2b658-351">W sposób intuicyjny `#define` i `#undef` dyrektywy musi poprzedzać "rzeczywistego kodu" w pliku źródłowym.</span><span class="sxs-lookup"><span data-stu-id="2b658-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="2b658-352">Przykład:</span><span class="sxs-lookup"><span data-stu-id="2b658-352">The example:</span></span>
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
<span data-ttu-id="2b658-353">jest nieprawidłowy ponieważ `#define` dyrektyw, poprzedź pierwszy token ( `namespace` — słowo kluczowe) w pliku źródłowym.</span><span class="sxs-lookup"><span data-stu-id="2b658-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="2b658-354">Poniższy przykład powoduje błąd kompilacji, ponieważ `#define` następuje rzeczywistego kodu:</span><span class="sxs-lookup"><span data-stu-id="2b658-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
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

<span data-ttu-id="2b658-355">A `#define` może zdefiniować symbol kompilacji warunkowej, który jest już zdefiniowany, bez żadnych aktywne `#undef` dla tego symbolu.</span><span class="sxs-lookup"><span data-stu-id="2b658-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="2b658-356">Poniższy przykład definiuje symbol kompilacji warunkowej `A` i ponownie go definiuje.</span><span class="sxs-lookup"><span data-stu-id="2b658-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="2b658-357">Element `#undef` może "Usuń" symbol kompilacji warunkowej, który nie został zdefiniowany.</span><span class="sxs-lookup"><span data-stu-id="2b658-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="2b658-358">Poniższy przykład definiuje symbol kompilacji warunkowej `A` i następnie definicji do usunięcia go dwukrotnie; mimo że drugi `#undef` nie ma wpływu, jest nadal ważny.</span><span class="sxs-lookup"><span data-stu-id="2b658-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="2b658-359">Dyrektywy kompilacji warunkowej</span><span class="sxs-lookup"><span data-stu-id="2b658-359">Conditional compilation directives</span></span>

<span data-ttu-id="2b658-360">Dyrektywy kompilacji warunkowej są używane do warunkowo dołączania lub wykluczania części pliku źródłowego.</span><span class="sxs-lookup"><span data-stu-id="2b658-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

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

<span data-ttu-id="2b658-361">Wskazane przez składnię dyrektywy kompilacji warunkowej musi być napisana jako zestawy składające się z, w kolejności, `#if` dyrektywy, zero lub więcej `#elif` dyrektyw, zero lub jeden `#else` dyrektywy i `#endif` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="2b658-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="2b658-362">Dyrektywy mogą sekcje warunkowe kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="2b658-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="2b658-363">Każda sekcja jest kontrolowana przez dyrektywy bezpośrednio poprzedzającego.</span><span class="sxs-lookup"><span data-stu-id="2b658-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="2b658-364">Sekcja warunkowa może sam zawierać dyrektywy kompilacji warunkowej zagnieżdżonych pod warunkiem dyrektywy te tworzą pełną zestawów.</span><span class="sxs-lookup"><span data-stu-id="2b658-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="2b658-365">A *pp_conditional* wybiera co najwyżej jeden zamkniętego *conditional_section*pod kątem normalne przetwarzanie leksykalne:</span><span class="sxs-lookup"><span data-stu-id="2b658-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="2b658-366">*Pp_expression*s `#if` i `#elif` dyrektywy są obliczane w kolejności, aż jedną daje `true`.</span><span class="sxs-lookup"><span data-stu-id="2b658-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="2b658-367">Jeśli wyrażenie zwraca `true`, *conditional_section* odpowiedniego dyrektywy jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="2b658-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="2b658-368">Jeśli wszystkie *pp_expression*s yield `false`i jeśli `#else` występuje dyrektywie *conditional_section* z `#else` dyrektywa jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="2b658-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="2b658-369">W przeciwnym razie nie *conditional_section* jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="2b658-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="2b658-370">Wybrane *conditional_section*, jeśli istnieje, jest przetwarzany jako normalny *input_section*: gramatyka leksykalna kodu źródłowego, znajdujących się w sekcji muszą być zgodne; tokeny są generowane na podstawie źródła kod w sekcji. i dyrektywy przetwarzania wstępnego, w sekcji realizowania efekty.</span><span class="sxs-lookup"><span data-stu-id="2b658-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="2b658-371">Pozostałe *conditional_section*s, jeśli istnieje, są przetwarzane jako *skipped_section*s: z wyjątkiem dyrektywy przetwarzania wstępnego, kod źródłowy w sekcji potrzebne jest zgodna leksykalne gramatyki; nie tokeny są generowane na podstawie kodu źródłowego w sekcji. i dyrektywy przetwarzania wstępnego, w sekcji musi być poprawna, leksykalnie, ale nie są przetwarzane w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="2b658-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="2b658-372">W ramach *conditional_section* , jest przetwarzana jako *skipped_section*, wszelkie zagnieżdżone *conditional_section*s (zawarty w zagnieżdżone `#if`... `#endif` i `#region`... `#endregion` tworzy) również są przetwarzane jako *skipped_section*s.</span><span class="sxs-lookup"><span data-stu-id="2b658-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="2b658-373">Poniższy przykład ilustruje jak kompilacja warunkowa dyrektywy można zagnieżdżać:</span><span class="sxs-lookup"><span data-stu-id="2b658-373">The following example illustrates how conditional compilation directives can nest:</span></span>
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

<span data-ttu-id="2b658-374">Z wyjątkiem dyrektywy przetwarzania wstępnego, pominięto kod źródłowy jest poddawać analizie leksykalnej.</span><span class="sxs-lookup"><span data-stu-id="2b658-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="2b658-375">Na przykład poniżej przedstawiono prawidłowe pomimo niezakończony komentarz w `#else` sekcji:</span><span class="sxs-lookup"><span data-stu-id="2b658-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
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

<span data-ttu-id="2b658-376">Należy jednak pamiętać, że dyrektywy przetwarzania wstępnego są wymagane było leksykalnie poprawne nawet w przypadku pominiętych części kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="2b658-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="2b658-377">Dyrektywy przetwarzania wstępnego nie są przetwarzane, gdy są one wyświetlane w wielowierszowym elementów wejściowych.</span><span class="sxs-lookup"><span data-stu-id="2b658-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="2b658-378">Na przykład program:</span><span class="sxs-lookup"><span data-stu-id="2b658-378">For example, the program:</span></span>
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
<span data-ttu-id="2b658-379">wyniki w danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="2b658-379">results in the output:</span></span>
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="2b658-380">W szczególnych przypadkach zbiór dyrektywy przetwarzania wstępnego, który jest przetwarzany może zależeć od oceny *pp_expression*.</span><span class="sxs-lookup"><span data-stu-id="2b658-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="2b658-381">Przykład:</span><span class="sxs-lookup"><span data-stu-id="2b658-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="2b658-382">zawsze daje ten sam token strumienia (`class` `Q` `{` `}`), niezależnie od tego, czy `X` jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="2b658-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="2b658-383">Jeśli `X` jest zdefiniowany, tylko dyrektywy przetwarzania są `#if` i `#endif`ze względu na komentarz wielowierszowy.</span><span class="sxs-lookup"><span data-stu-id="2b658-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="2b658-384">Jeśli `X` jest niezdefiniowana, następnie trzy dyrektywy (`#if`, `#else`, `#endif`) są dostępne w ramach zestawu dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="2b658-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="2b658-385">Dyrektywy diagnostyczne</span><span class="sxs-lookup"><span data-stu-id="2b658-385">Diagnostic directives</span></span>

<span data-ttu-id="2b658-386">Dyrektywy diagnostycznych są używane do jawnie generować błędów i komunikaty ostrzegawcze, które są zgłaszane w taki sam sposób jak inne błędy kompilacji i ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="2b658-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

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

<span data-ttu-id="2b658-387">Przykład:</span><span class="sxs-lookup"><span data-stu-id="2b658-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="2b658-388">zawsze generuje ostrzeżenie ("Przegląd kodu potrzebne przed zaewidencjonowaniem") i generuje błąd w czasie kompilacji ("kompilacji nie może być zarówno debugowania, jak i handlu detalicznego") Jeżeli symbole warunkowego `Debug` i `Retail` są zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="2b658-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="2b658-389">Należy pamiętać, że *pp_message* może zawierać dowolny tekst; ściślej mówiąc, go nie muszą zawierać tokeny poprawnie sformułowany, jak to przedstawiono w pojedynczy cudzysłów wyraz `can't`.</span><span class="sxs-lookup"><span data-stu-id="2b658-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="2b658-390">Dyrektywy regionu</span><span class="sxs-lookup"><span data-stu-id="2b658-390">Region directives</span></span>

<span data-ttu-id="2b658-391">Dyrektywy regionu są używane do wyraźnie oznaczyć regiony kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="2b658-391">The region directives are used to explicitly mark regions of source code.</span></span>

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

<span data-ttu-id="2b658-392">Nie znaczenia semantycznego jest dołączony do regionu; regiony są przeznaczone do użytku przez programistę lub zautomatyzowanych narzędzi do oznaczania sekcję kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="2b658-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="2b658-393">Komunikat określony w `#region` lub `#endregion` dyrektywy również nie ma znaczenia semantycznego; służy ona jedynie do identyfikowania regionu.</span><span class="sxs-lookup"><span data-stu-id="2b658-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="2b658-394">Dopasowywanie `#region` i `#endregion` dyrektywy może mieć różne *pp_message*s.</span><span class="sxs-lookup"><span data-stu-id="2b658-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="2b658-395">Leksykalne przetwarzanie regionu:</span><span class="sxs-lookup"><span data-stu-id="2b658-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="2b658-396">dokładnie odpowiada leksykalne przetwarzania dyrektywy kompilacji warunkowej w postaci:</span><span class="sxs-lookup"><span data-stu-id="2b658-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="2b658-397">Dyrektywy line</span><span class="sxs-lookup"><span data-stu-id="2b658-397">Line directives</span></span>

<span data-ttu-id="2b658-398">Dyrektywy line można zmienić numery wierszy i nazw plików źródłowych, które są zgłaszane przez kompilator w danych wyjściowych, takie jak ostrzeżenia i błędy, a które są używane przez caller — atrybuty informacji ([Caller — atrybuty informacji](attributes.md#caller-info-attributes)).</span><span class="sxs-lookup"><span data-stu-id="2b658-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="2b658-399">Dyrektywy line są najczęściej używane narzędzia meta-programowania, które generuje kod źródłowy języka C# z innym tekstem danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="2b658-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

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

<span data-ttu-id="2b658-400">Gdy nie `#line` dyrektyw, kompilator raporty numery wierszy true i nazw plików źródłowych w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="2b658-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="2b658-401">Podczas przetwarzania `#line` dyrektywę, który zawiera *line_indicator* , który nie jest `default`, kompilator traktuje wiersza po dyrektywie jako posiadające podany numer wiersza (i nazwę pliku, jeśli określono).</span><span class="sxs-lookup"><span data-stu-id="2b658-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="2b658-402">A `#line default` dyrektywy cofa efekt wszystkich poprzednich dyrektyw #line.</span><span class="sxs-lookup"><span data-stu-id="2b658-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="2b658-403">Kompilator zgłasza true wiersza informacji dla kolejnych wierszy, dokładnie tak, jakby nie `#line` przetworzył dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="2b658-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="2b658-404">A `#line hidden` dyrektywy nie ma wpływu na plik i numery wierszy zgłosił błąd komunikaty, ale wpływa na debugowanie na poziomie źródła.</span><span class="sxs-lookup"><span data-stu-id="2b658-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="2b658-405">Podczas debugowania, wszystkie linie między `#line hidden` dyrektywy i kolejne `#line` — dyrektywa (nie jest to `#line hidden`) mają nie informacja o numerach wierszy.</span><span class="sxs-lookup"><span data-stu-id="2b658-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="2b658-406">Podczas krokowego wykonywania kodu w debugerze, wiersze te zostaną pominięte całkowicie.</span><span class="sxs-lookup"><span data-stu-id="2b658-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="2b658-407">Należy pamiętać, że *nazwa_pliku* różni się od regularnych literału ciągu, w tym znaki ucieczki nie są przetwarzane; "`\`" znak po prostu wyznacza zwykłych ukośnika w ramach *nazwa_pliku*.</span><span class="sxs-lookup"><span data-stu-id="2b658-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="2b658-408">Pragma — dyrektywy</span><span class="sxs-lookup"><span data-stu-id="2b658-408">Pragma directives</span></span>

<span data-ttu-id="2b658-409">`#pragma` Dyrektywy preprocesora jest używany do określenia opcjonalne informacje kontekstowe do kompilatora.</span><span class="sxs-lookup"><span data-stu-id="2b658-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="2b658-410">Informacje dostarczone w `#pragma` dyrektywy nigdy nie zmienia semantykę program.</span><span class="sxs-lookup"><span data-stu-id="2b658-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="2b658-411">C# zawiera `#pragma` dyrektywy do kontrolowania ostrzeżeń kompilatora.</span><span class="sxs-lookup"><span data-stu-id="2b658-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="2b658-412">Przyszłe wersje języka może zawierać dodatkowe `#pragma` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="2b658-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="2b658-413">Aby zapewnić współdziałanie za pomocą innych kompilatorów języka C#, kompilator Microsoft C# nie generuje błędy kompilacji nieznany `#pragma` dyrektywy; takie nie dyrektyw, jednak generowanie ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="2b658-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="2b658-414">Warning elementu pragma</span><span class="sxs-lookup"><span data-stu-id="2b658-414">Pragma warning</span></span>

<span data-ttu-id="2b658-415">`#pragma warning` Dyrektywy służy do wyłączenia lub przywrócenia wszystkich lub określonego zestawu ostrzeżenie komunikatów podczas kompilacji i kolejne tekstu.</span><span class="sxs-lookup"><span data-stu-id="2b658-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

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

<span data-ttu-id="2b658-416">A `#pragma warning` dyrektywy, które pomija listy ostrzeżenie ma wpływ na wszystkie ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="2b658-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="2b658-417">A `#pragma warning` dyrektywy zawierają listę ostrzeżenie dotyczy tylko tych ostrzeżeń, które są określone na liście.</span><span class="sxs-lookup"><span data-stu-id="2b658-417">A `#pragma warning` directive the includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="2b658-418">A `#pragma warning disable` dyrektywy wyłącza wszystkie lub danego zestawu ostrzeżeń.</span><span class="sxs-lookup"><span data-stu-id="2b658-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="2b658-419">A `#pragma warning restore` dyrektywy przywraca wszystkie lub danego zestawu ostrzeżeń do stanu który obowiązywały na początku jednostki kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2b658-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="2b658-420">Należy pamiętać, że jeśli określonego ostrzeżenie zostało wyłączone z zewnątrz, `#pragma warning restore` (czy dla wszystkich lub określonych ostrzeżeń) nie będzie ponownie włączyć tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="2b658-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="2b658-421">W poniższym przykładzie pokazano użycie `#pragma warning` tymczasowo wyłączyć ostrzeżenia zgłoszone podczas zamieniono przestarzały parametr składowych są wywoływane za pomocą numeru ostrzeżenia kompilatora Microsoft C#.</span><span class="sxs-lookup"><span data-stu-id="2b658-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
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
