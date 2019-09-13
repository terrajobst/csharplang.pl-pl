---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912435"
---
# <a name="documentation-comments"></a><span data-ttu-id="81824-101">Komentarze dokumentacji</span><span class="sxs-lookup"><span data-stu-id="81824-101">Documentation comments</span></span>

<span data-ttu-id="81824-102">C#udostępnia mechanizm programisty do dokumentowania kodu przy użyciu specjalnej składni komentarzy zawierającej tekst XML.</span><span class="sxs-lookup"><span data-stu-id="81824-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="81824-103">W plikach kodu źródłowego Komentarze z określonym formularzem mogą służyć do kierowania narzędziem do tworzenia kodu XML z tych komentarzy i elementów kodu źródłowego, które poprzedzają.</span><span class="sxs-lookup"><span data-stu-id="81824-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="81824-104">Komentarze wykorzystujące taką składnię są nazywane ***komentarzami dokumentacji***.</span><span class="sxs-lookup"><span data-stu-id="81824-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="81824-105">Muszą bezpośrednio poprzedzać typ zdefiniowany przez użytkownika (na przykład Klasa, delegat lub interfejs) lub element członkowski (na przykład pole, zdarzenie, właściwość lub metoda).</span><span class="sxs-lookup"><span data-stu-id="81824-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="81824-106">Narzędzie generowania kodu XML jest nazywane ***generatorem dokumentacji***.</span><span class="sxs-lookup"><span data-stu-id="81824-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="81824-107">(Ten generator może być, ale nie musi, sam C# kompilator). Dane wyjściowe generowane przez generator dokumentacji są nazywane ***plikiem dokumentacji***.</span><span class="sxs-lookup"><span data-stu-id="81824-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="81824-108">Plik dokumentacji jest używany jako dane wejściowe do ***przeglądarki dokumentacji***programu. Narzędzie przeznaczone do tworzenia pewnego rodzaju wizualizacji graficznej informacji o typie i powiązanej z nią dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="81824-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="81824-109">Ta specyfikacja sugeruje zestaw tagów, które mają być używane w komentarzach dokumentacji, ale użycie tych tagów nie jest wymagane, a inne Tagi mogą być używane w razie potrzeby, tak długo, jak są stosowane reguły poprawnie sformułowanego kodu XML.</span><span class="sxs-lookup"><span data-stu-id="81824-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="81824-110">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="81824-110">Introduction</span></span>

<span data-ttu-id="81824-111">Komentarze zawierające specjalną formę mogą służyć do kierowania narzędzia do tworzenia kodu XML z tych komentarzy i elementów kodu źródłowego, które poprzedzają.</span><span class="sxs-lookup"><span data-stu-id="81824-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="81824-112">Takie komentarze to jednowierszowe komentarze, które zaczynają się od trzech`///`ukośników () lub rozdzielane komentarze, które zaczynają się od ukośnika i dwóch gwiazdek (`/**`).</span><span class="sxs-lookup"><span data-stu-id="81824-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="81824-113">Muszą one bezpośrednio poprzedzać typ zdefiniowany przez użytkownika (na przykład Klasa, delegat lub interfejs) lub element członkowski (na przykład pole, zdarzenie, właściwość lub metoda), które mają do nich adnotacje.</span><span class="sxs-lookup"><span data-stu-id="81824-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="81824-114">Sekcje atrybutów ([Specyfikacja atrybutów](attributes.md#attribute-specification)) są uważane za część deklaracji, dlatego Komentarze do dokumentacji muszą poprzedzać atrybuty zastosowane do typu lub składowej.</span><span class="sxs-lookup"><span data-stu-id="81824-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="81824-115">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="81824-116">W *single_line_doc_comment*, jeśli `///` występuje znak *odstępu* , po znakach na każdym *single_line_doc_comment*s przylegającym do bieżącej single_line_doc_comment, to  *znak odstępu* nie jest uwzględniony w danych wyjściowych XML.</span><span class="sxs-lookup"><span data-stu-id="81824-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="81824-117">W komentarzu rozdzielanym doc, jeśli pierwszy znak, który nie jest odstępem w drugim wierszu, jest gwiazdką i ten sam wzorzec opcjonalnych znaków odstępu i znak gwiazdki jest powtarzany na początku każdego wiersza w komentarzu rozdzielanym doc. znaki Powtórzonego wzorca nie są uwzględniane w danych wyjściowych XML.</span><span class="sxs-lookup"><span data-stu-id="81824-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="81824-118">Wzorzec może zawierać znaki odstępu po, jak również przed znakiem gwiazdki.</span><span class="sxs-lookup"><span data-stu-id="81824-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="81824-119">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-119">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>
///
public class Point 
{
    /// <summary>method <c>draw</c> renders the point.</summary>
    void draw() {...}
}
```

<span data-ttu-id="81824-120">Tekst w komentarzach dokumentacji musi być poprawnie sformułowany zgodnie z regułami XML (https://www.w3.org/TR/REC-xml).</span><span class="sxs-lookup"><span data-stu-id="81824-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="81824-121">Jeśli kod XML jest źle sformułowany, generowane jest ostrzeżenie, a plik dokumentacji będzie zawierał komentarz informujący o wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="81824-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="81824-122">Chociaż deweloperzy mogą tworzyć własne zestawy tagów, zalecany zestaw jest zdefiniowany w [zalecane Tagi](documentation-comments.md#recommended-tags).</span><span class="sxs-lookup"><span data-stu-id="81824-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="81824-123">Niektóre z zalecanych tagów mają specjalne znaczenie:</span><span class="sxs-lookup"><span data-stu-id="81824-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="81824-124">`<param>` Tag jest używany do opisywania parametrów.</span><span class="sxs-lookup"><span data-stu-id="81824-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="81824-125">Jeśli jest używany ten tag, generator dokumentacji musi sprawdzić, czy określony parametr istnieje i czy wszystkie parametry zostały opisane w komentarzach dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="81824-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="81824-126">Jeśli taka weryfikacja nie powiedzie się, generator dokumentacji wygeneruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="81824-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="81824-127">Ten `cref` atrybut może być dołączany do dowolnego tagu w celu zapewnienia odwołania do elementu kodu.</span><span class="sxs-lookup"><span data-stu-id="81824-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="81824-128">Generator dokumentacji musi sprawdzić, czy ten element kodu istnieje.</span><span class="sxs-lookup"><span data-stu-id="81824-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="81824-129">Jeśli weryfikacja nie powiedzie się, generator dokumentacji wygeneruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="81824-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="81824-130">Podczas wyszukiwania nazwy opisanej w `cref` atrybucie generator dokumentacji musi uwzględniać widoczność przestrzeni nazw zgodnie z `using` instrukcjami wyświetlanymi w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="81824-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="81824-131">Dla elementów kodu, które są ogólne, nie można użyć normalnej składni ogólnej (is`List<T>`""), ponieważ generuje ona nieprawidłowy kod XML.</span><span class="sxs-lookup"><span data-stu-id="81824-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="81824-132">Nawiasy klamrowe mogą być używane zamiast nawiasów kwadratowych (czyli`List{T}`"") lub składni ucieczki XML (czyli`List&lt;T&gt;`"").</span><span class="sxs-lookup"><span data-stu-id="81824-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="81824-133">`<summary>` Tag jest przeznaczony do użycia przez Podgląd dokumentacji, aby wyświetlić dodatkowe informacje na temat typu lub elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="81824-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="81824-134">`<include>` Tag zawiera informacje z zewnętrznego pliku XML.</span><span class="sxs-lookup"><span data-stu-id="81824-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="81824-135">Należy uważnie pamiętać, że plik dokumentacji nie zawiera pełnych informacji o typie i elementach członkowskich (na przykład nie zawiera żadnych informacji o typie).</span><span class="sxs-lookup"><span data-stu-id="81824-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="81824-136">Aby uzyskać takie informacje dotyczące typu lub elementu członkowskiego, plik dokumentacji musi być używany w połączeniu z odbiciem w rzeczywistym typie lub elemencie członkowskim.</span><span class="sxs-lookup"><span data-stu-id="81824-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="81824-137">Zalecane Tagi</span><span class="sxs-lookup"><span data-stu-id="81824-137">Recommended tags</span></span>

<span data-ttu-id="81824-138">Generator dokumentacji musi akceptować i przetwarzać każdy tag, który jest prawidłowy zgodnie z regułami XML.</span><span class="sxs-lookup"><span data-stu-id="81824-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="81824-139">Poniższe Tagi zapewniają powszechnie używane funkcje w dokumentacji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="81824-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="81824-140">(Oczywiście są możliwe inne Tagi).</span><span class="sxs-lookup"><span data-stu-id="81824-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="81824-141">__Seryjn__</span><span class="sxs-lookup"><span data-stu-id="81824-141">__Tag__</span></span>          | <span data-ttu-id="81824-142">__Paragraf__</span><span class="sxs-lookup"><span data-stu-id="81824-142">__Section__</span></span>                                            | <span data-ttu-id="81824-143">__Cel__</span><span class="sxs-lookup"><span data-stu-id="81824-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="81824-144">Ustaw tekst w czcionce podobnej do kodu</span><span class="sxs-lookup"><span data-stu-id="81824-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="81824-145">Ustaw jeden lub więcej wierszy kodu źródłowego lub danych wyjściowych programu</span><span class="sxs-lookup"><span data-stu-id="81824-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="81824-146">Wskaż przykład</span><span class="sxs-lookup"><span data-stu-id="81824-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="81824-147">Identyfikuje wyjątki, które może zgłosić Metoda</span><span class="sxs-lookup"><span data-stu-id="81824-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="81824-148">Zawiera XML z pliku zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="81824-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="81824-149">Tworzenie listy lub tabeli</span><span class="sxs-lookup"><span data-stu-id="81824-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="81824-150">Zezwól na Dodawanie struktury do tekstu</span><span class="sxs-lookup"><span data-stu-id="81824-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="81824-151">Opisz parametr dla metody lub konstruktora</span><span class="sxs-lookup"><span data-stu-id="81824-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="81824-152">Określ, że słowo jest nazwą parametru</span><span class="sxs-lookup"><span data-stu-id="81824-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="81824-153">Udokumentowanie dostępności zabezpieczeń elementu członkowskiego</span><span class="sxs-lookup"><span data-stu-id="81824-153">Document the security accessibility of a member</span></span>        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | <span data-ttu-id="81824-154">Opisz dodatkowe informacje o typie</span><span class="sxs-lookup"><span data-stu-id="81824-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="81824-155">Opisywanie wartości zwracanej przez metodę</span><span class="sxs-lookup"><span data-stu-id="81824-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="81824-156">Określ link</span><span class="sxs-lookup"><span data-stu-id="81824-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="81824-157">Generowanie wpisu Zobacz również</span><span class="sxs-lookup"><span data-stu-id="81824-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="81824-158">Opisz typ lub element członkowski typu</span><span class="sxs-lookup"><span data-stu-id="81824-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="81824-159">Opisz Właściwość</span><span class="sxs-lookup"><span data-stu-id="81824-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="81824-160">Opisywanie parametru typu ogólnego</span><span class="sxs-lookup"><span data-stu-id="81824-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="81824-161">Określ, że słowo jest nazwą parametru typu</span><span class="sxs-lookup"><span data-stu-id="81824-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="81824-162">Ten tag udostępnia mechanizm wskazujący, że fragment tekstu w opisie powinien być ustawiony w specjalnej czcionce, takiej jak używany dla bloku kodu.</span><span class="sxs-lookup"><span data-stu-id="81824-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="81824-163">W przypadku wierszy rzeczywistego kodu Użyj `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="81824-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="81824-164">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="81824-165">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="81824-166">Ten tag służy do ustawiania co najmniej jednego wiersza kodu źródłowego lub danych wyjściowych programu w specjalnej czcionce.</span><span class="sxs-lookup"><span data-stu-id="81824-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="81824-167">W przypadku małych fragmentów kodu w opisach[`<c>`](documentation-comments.md#c)Użyj `<c>` ().</span><span class="sxs-lookup"><span data-stu-id="81824-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="81824-168">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="81824-169">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-169">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// <example>For example:
/// <code>
///    Point p = new Point(3,5);
///    p.Translate(-1,3);
/// </code>
/// results in <c>p</c>'s having the value (2,8).
/// </example>
/// </summary>

public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}   
```

### `<example>`

<span data-ttu-id="81824-170">Ten tag umożliwia przykładowy kod w komentarzu, aby określić sposób użycia metody lub innego elementu członkowskiego biblioteki.</span><span class="sxs-lookup"><span data-stu-id="81824-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="81824-171">Zwykle dotyczy to również użycia znacznika `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="81824-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="81824-172">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="81824-173">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-173">__Example:__</span></span>

<span data-ttu-id="81824-174">Zobacz `<code>` [(`<code>`](documentation-comments.md#code)), aby zapoznać się z przykładem.</span><span class="sxs-lookup"><span data-stu-id="81824-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="81824-175">Ten tag umożliwia udokumentowanie wyjątków, które Metoda może zgłosić.</span><span class="sxs-lookup"><span data-stu-id="81824-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="81824-176">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="81824-177">gdzie</span><span class="sxs-lookup"><span data-stu-id="81824-177">where</span></span>

* <span data-ttu-id="81824-178">`member`jest nazwą elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="81824-178">`member` is the name of a member.</span></span> <span data-ttu-id="81824-179">Generator dokumentacji sprawdza, czy dany element członkowski istnieje i tłumaczy `member` na nazwę elementu kanonicznego w pliku dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="81824-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="81824-180">`description`to opis okoliczności, w których wyjątek jest zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="81824-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="81824-181">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-181">__Example:__</span></span>

```csharp
public class DataBaseOperations
{
    /// <exception cref="MasterFileFormatCorruptException"></exception>
    /// <exception cref="MasterFileLockedOpenException"></exception>
    public static void ReadRecord(int flag) {
        if (flag == 1)
            throw new MasterFileFormatCorruptException();
        else if (flag == 2)
            throw new MasterFileLockedOpenException();
        // ...
    } 
}
```

### `<include>`

<span data-ttu-id="81824-182">Ten tag umożliwia dołączenie informacji z dokumentu XML, który jest zewnętrzny względem pliku kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="81824-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="81824-183">Plik zewnętrzny musi być poprawnie sformułowanym dokumentem XML i wyrażenie XPath jest stosowane do tego dokumentu, aby określić, jakie dane XML z tego dokumentu mają być uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="81824-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="81824-184">`<include>` Znacznik jest następnie zastępowany wybranym XML z dokumentu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="81824-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="81824-185">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="81824-186">gdzie</span><span class="sxs-lookup"><span data-stu-id="81824-186">where</span></span>

* <span data-ttu-id="81824-187">`filename`jest nazwą pliku zewnętrznego pliku XML.</span><span class="sxs-lookup"><span data-stu-id="81824-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="81824-188">Nazwa pliku jest interpretowana względem pliku, który zawiera tag include.</span><span class="sxs-lookup"><span data-stu-id="81824-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="81824-189">`xpath`jest wyrażeniem XPath, które wybiera część XML w zewnętrznym pliku XML.</span><span class="sxs-lookup"><span data-stu-id="81824-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="81824-190">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-190">__Example:__</span></span>

<span data-ttu-id="81824-191">Jeśli kod źródłowy zawiera deklarację taką jak:</span><span class="sxs-lookup"><span data-stu-id="81824-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="81824-192">a plik zewnętrzny "docs. xml" ma następującą zawartość:</span><span class="sxs-lookup"><span data-stu-id="81824-192">and the external file "docs.xml" had the following contents:</span></span>

```xml
<?xml version="1.0"?>
<extradoc>
  <class name="IntList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
  <class name="StringList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
</extradoc>
```

<span data-ttu-id="81824-193">następnie taka sama dokumentacja jest wyjściowa, jakby zawierała kod źródłowy:</span><span class="sxs-lookup"><span data-stu-id="81824-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="81824-194">Ten tag służy do tworzenia listy lub tabeli elementów.</span><span class="sxs-lookup"><span data-stu-id="81824-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="81824-195">Może zawierać `<listheader>` blok, aby zdefiniować wiersz nagłówka tabeli lub listy definicji.</span><span class="sxs-lookup"><span data-stu-id="81824-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="81824-196">(Podczas definiowania tabeli należy podać tylko wpis `term` w nagłówku).</span><span class="sxs-lookup"><span data-stu-id="81824-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="81824-197">Każdy element na liście jest określony za pomocą `<item>` bloku.</span><span class="sxs-lookup"><span data-stu-id="81824-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="81824-198">Podczas tworzenia listy definicji należy określić obie `term` i `description` .</span><span class="sxs-lookup"><span data-stu-id="81824-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="81824-199">Jednak dla tabeli, listy punktowanej lub listy numerowanej należy określić tylko `description` wartość.</span><span class="sxs-lookup"><span data-stu-id="81824-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="81824-200">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-200">__Syntax:__</span></span>

```xml
<list type="bullet" | "number" | "table">
   <listheader>
      <term>term</term>
      <description>*description*</description>
   </listheader>
   <item>
      <term>term</term>
      <description>*description*</description>
   </item>
    ...
   <item>
      <term>term</term>
      <description>description</description>
   </item>
</list>
```

<span data-ttu-id="81824-201">gdzie</span><span class="sxs-lookup"><span data-stu-id="81824-201">where</span></span>

* <span data-ttu-id="81824-202">`term`jest terminem do zdefiniowania, którego definicja znajduje `description`się w.</span><span class="sxs-lookup"><span data-stu-id="81824-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="81824-203">`description`jest elementem w postaci listy punktowanej lub numerowanej lub definicji `term`.</span><span class="sxs-lookup"><span data-stu-id="81824-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="81824-204">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-204">__Example:__</span></span>

```csharp
public class MyClass
{
    /// <summary>Here is an example of a bulleted list:
    /// <list type="bullet">
    /// <item>
    /// <description>Item 1.</description>
    /// </item>
    /// <item>
    /// <description>Item 2.</description>
    /// </item>
    /// </list>
    /// </summary>
    public static void Main () {
        // ...
    }
}
```

### `<para>`

<span data-ttu-id="81824-205">Ten tag jest używany wewnątrz innych tagów, takich `<summary>` jak ([`<remarks>`](documentation-comments.md#remarks)) lub `<returns>` ([`<returns>`](documentation-comments.md#returns)), i umożliwia dodanie struktury do tekstu.</span><span class="sxs-lookup"><span data-stu-id="81824-205">This tag is for use inside other tags, such as `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="81824-206">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="81824-207">gdzie `content` jest tekstem akapitu.</span><span class="sxs-lookup"><span data-stu-id="81824-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="81824-208">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-208">__Example:__</span></span>

```csharp
/// <summary>This is the entry point of the Point class testing program.
/// <para>This program tests each method and operator, and
/// is intended to be run after any non-trivial maintenance has
/// been performed on the Point class.</para></summary>
public static void Main() {
    // ...
}
```

### `<param>`

<span data-ttu-id="81824-209">Ten tag służy do opisywania parametru dla metody, konstruktora lub indeksatora.</span><span class="sxs-lookup"><span data-stu-id="81824-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="81824-210">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="81824-211">gdzie</span><span class="sxs-lookup"><span data-stu-id="81824-211">where</span></span>

* <span data-ttu-id="81824-212">`name`jest nazwą parametru.</span><span class="sxs-lookup"><span data-stu-id="81824-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="81824-213">`description`to opis parametru.</span><span class="sxs-lookup"><span data-stu-id="81824-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="81824-214">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-214">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <param name="xor">the new x-coordinate.</param>
/// <param name="yor">the new y-coordinate.</param>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<paramref>`

<span data-ttu-id="81824-215">Ten tag służy do wskazywania, że słowo jest parametrem.</span><span class="sxs-lookup"><span data-stu-id="81824-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="81824-216">Plik dokumentacji można przetworzyć w taki sposób, aby sformatować ten parametr w różny sposób.</span><span class="sxs-lookup"><span data-stu-id="81824-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="81824-217">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="81824-218">gdzie `name` to nazwa parametru.</span><span class="sxs-lookup"><span data-stu-id="81824-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="81824-219">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-219">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to
///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
/// <param name="xor">the new Point's x-coordinate.</param>
/// <param name="yor">the new Point's y-coordinate.</param>

public Point(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<permission>`

<span data-ttu-id="81824-220">Ten tag pozwala na udokumentowanie dostępności zabezpieczeń elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="81824-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="81824-221">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="81824-222">gdzie</span><span class="sxs-lookup"><span data-stu-id="81824-222">where</span></span>

* <span data-ttu-id="81824-223">`member`jest nazwą elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="81824-223">`member` is the name of a member.</span></span> <span data-ttu-id="81824-224">Generator dokumentacji sprawdza, czy dany element kodu istnieje i tłumaczy *składową* na nazwę elementu kanonicznego w pliku dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="81824-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="81824-225">`description`to opis dostępu do elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="81824-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="81824-226">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

<span data-ttu-id="81824-227">Ten tag służy do określania dodatkowych informacji o typie.</span><span class="sxs-lookup"><span data-stu-id="81824-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="81824-228">(USE `<summary>` ([`<summary>`](documentation-comments.md#summary)) do opisywania samego typu i elementów członkowskich typu).</span><span class="sxs-lookup"><span data-stu-id="81824-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="81824-229">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-229">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="81824-230">gdzie `description` jest tekstem uwagi.</span><span class="sxs-lookup"><span data-stu-id="81824-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="81824-231">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-231">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remarks>Uses polar coordinates</remarks>
public class Point 
{
    // ...
}
```

### `<returns>`

<span data-ttu-id="81824-232">Ten tag jest używany do opisywania wartości zwracanej przez metodę.</span><span class="sxs-lookup"><span data-stu-id="81824-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="81824-233">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="81824-234">gdzie `description` jest opisem wartości zwracanej.</span><span class="sxs-lookup"><span data-stu-id="81824-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="81824-235">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="81824-236">Ten tag umożliwia określenie linku w tekście.</span><span class="sxs-lookup"><span data-stu-id="81824-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="81824-237">Użyj `<seealso>` [(`<seealso>`](documentation-comments.md#seealso)), aby wskazać tekst, który ma być wyświetlany w sekcji Zobacz też.</span><span class="sxs-lookup"><span data-stu-id="81824-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="81824-238">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="81824-239">gdzie `member` jest nazwą elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="81824-239">where `member` is the name of a member.</span></span> <span data-ttu-id="81824-240">Generator dokumentacji sprawdza, czy dany element kodu istnieje i zmienia element *członkowski* na nazwę elementu w wygenerowanym pliku dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="81824-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="81824-241">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-241">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <see cref="Translate"/>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}

/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// </summary>
/// <see cref="Move"/>
public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}
```

### `<seealso>`

<span data-ttu-id="81824-242">Ten tag umożliwia wygenerowanie wpisu dla sekcji Zobacz też.</span><span class="sxs-lookup"><span data-stu-id="81824-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="81824-243">Użyj `<see>` [(`<see>`](documentation-comments.md#see)), aby określić łącze z tekstu.</span><span class="sxs-lookup"><span data-stu-id="81824-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="81824-244">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="81824-245">gdzie `member` jest nazwą elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="81824-245">where `member` is the name of a member.</span></span> <span data-ttu-id="81824-246">Generator dokumentacji sprawdza, czy dany element kodu istnieje i zmienia element *członkowski* na nazwę elementu w wygenerowanym pliku dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="81824-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="81824-247">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-247">__Example:__</span></span>

```csharp
/// <summary>This method determines whether two Points have the same
///    location.</summary>
/// <seealso cref="operator=="/>
/// <seealso cref="operator!="/>
public override bool Equals(object o) {
    // ...
}
```

### `<summary>`

Ten tag może służyć do opisywania typu lub elementu członkowskiego typu. <span data-ttu-id="81824-249">Użyj `<remarks>` [(`<remarks>`](documentation-comments.md#remarks)), aby opisać sam typ.</span><span class="sxs-lookup"><span data-stu-id="81824-249">Use `<remarks>` ([`<remarks>`](documentation-comments.md#remarks)) to describe the type itself.</span></span>

<span data-ttu-id="81824-250">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="81824-251">gdzie `description` to podsumowanie typu lub elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="81824-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="81824-252">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="81824-253">Ten tag pozwala na opis właściwości.</span><span class="sxs-lookup"><span data-stu-id="81824-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="81824-254">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="81824-255">gdzie `property description` jest opisem właściwości.</span><span class="sxs-lookup"><span data-stu-id="81824-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="81824-256">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="81824-257">Ten tag służy do opisywania parametru typu ogólnego dla klasy, struktury, interfejsu, delegata lub metody.</span><span class="sxs-lookup"><span data-stu-id="81824-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="81824-258">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="81824-259">gdzie `name` jest nazwą parametru typu i `description` jest jego opis.</span><span class="sxs-lookup"><span data-stu-id="81824-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="81824-260">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="81824-261">Ten tag służy do wskazywania, że słowo jest parametrem typu.</span><span class="sxs-lookup"><span data-stu-id="81824-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="81824-262">Plik dokumentacji można przetworzyć w taki sposób, aby sformatować ten parametr typu w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="81824-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="81824-263">__Obowiązuje__</span><span class="sxs-lookup"><span data-stu-id="81824-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="81824-264">gdzie `name` jest nazwą parametru typu.</span><span class="sxs-lookup"><span data-stu-id="81824-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="81824-265">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="81824-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="81824-266">Przetwarzanie pliku dokumentacji</span><span class="sxs-lookup"><span data-stu-id="81824-266">Processing the documentation file</span></span>

<span data-ttu-id="81824-267">Generator dokumentacji generuje ciąg identyfikatora dla każdego elementu w kodzie źródłowym, który jest oznaczony za pomocą komentarza do dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="81824-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="81824-268">Ten ciąg identyfikatora jednoznacznie identyfikuje element źródłowy.</span><span class="sxs-lookup"><span data-stu-id="81824-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="81824-269">Podgląd dokumentacji może używać ciągu identyfikatora do identyfikowania odpowiednich metadanych/elementu odbicia, do którego odnosi się dokumentacja.</span><span class="sxs-lookup"><span data-stu-id="81824-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="81824-270">Plik dokumentacji nie jest hierarchiczną reprezentacją kodu źródłowego; Zamiast tego jest to płaska lista z wygenerowanym ciągiem identyfikatora dla każdego elementu.</span><span class="sxs-lookup"><span data-stu-id="81824-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="81824-271">Format ciągu identyfikatora</span><span class="sxs-lookup"><span data-stu-id="81824-271">ID string format</span></span>

<span data-ttu-id="81824-272">Generator dokumentacji obserwuje następujące reguły podczas generowania ciągów identyfikatorów:</span><span class="sxs-lookup"><span data-stu-id="81824-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="81824-273">Brak białego znaku w ciągu.</span><span class="sxs-lookup"><span data-stu-id="81824-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="81824-274">Pierwsza część ciągu określa rodzaj składowej udokumentowanej za pośrednictwem pojedynczego znaku, po którym następuje dwukropek.</span><span class="sxs-lookup"><span data-stu-id="81824-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="81824-275">Zdefiniowane są następujące rodzaje elementów członkowskich:</span><span class="sxs-lookup"><span data-stu-id="81824-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="81824-276">__Opis__</span><span class="sxs-lookup"><span data-stu-id="81824-276">__Character__</span></span> | <span data-ttu-id="81824-277">__Opis__</span><span class="sxs-lookup"><span data-stu-id="81824-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="81824-278">E</span><span class="sxs-lookup"><span data-stu-id="81824-278">E</span></span>             | <span data-ttu-id="81824-279">Zdarzenie</span><span class="sxs-lookup"><span data-stu-id="81824-279">Event</span></span>                                                       |
   | <span data-ttu-id="81824-280">F</span><span class="sxs-lookup"><span data-stu-id="81824-280">F</span></span>             | <span data-ttu-id="81824-281">Pole</span><span class="sxs-lookup"><span data-stu-id="81824-281">Field</span></span>                                                       |
   | <span data-ttu-id="81824-282">M</span><span class="sxs-lookup"><span data-stu-id="81824-282">M</span></span>             | <span data-ttu-id="81824-283">Metoda (w tym konstruktory, destruktory i operatory)</span><span class="sxs-lookup"><span data-stu-id="81824-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="81824-284">N</span><span class="sxs-lookup"><span data-stu-id="81824-284">N</span></span>             | <span data-ttu-id="81824-285">Przestrzeń nazw</span><span class="sxs-lookup"><span data-stu-id="81824-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="81824-286">P</span><span class="sxs-lookup"><span data-stu-id="81824-286">P</span></span>             | <span data-ttu-id="81824-287">Właściwość (w tym indeksatory)</span><span class="sxs-lookup"><span data-stu-id="81824-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="81824-288">T</span><span class="sxs-lookup"><span data-stu-id="81824-288">T</span></span>             | <span data-ttu-id="81824-289">Typ (taki jak Klasa, delegat, enum, Interface i struct)</span><span class="sxs-lookup"><span data-stu-id="81824-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="81824-290">!</span><span class="sxs-lookup"><span data-stu-id="81824-290">!</span></span>             | <span data-ttu-id="81824-291">Ciąg błędu; pozostała część ciągu zawiera informacje o błędzie.</span><span class="sxs-lookup"><span data-stu-id="81824-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="81824-292">Na przykład generator dokumentacji generuje informacje o błędach dla łączy, których nie można rozpoznać.</span><span class="sxs-lookup"><span data-stu-id="81824-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="81824-293">Druga część ciągu jest w pełni kwalifikowana nazwa elementu, rozpoczynając od elementu głównego przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="81824-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="81824-294">Nazwa elementu, jego typy otaczające i przestrzeń nazw są oddzielone kropkami.</span><span class="sxs-lookup"><span data-stu-id="81824-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="81824-295">Jeśli nazwa samego elementu ma okresy, są one zastępowane `#(U+0023)` znakami.</span><span class="sxs-lookup"><span data-stu-id="81824-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="81824-296">(Zakłada się, że żaden element nie ma tego znaku w nazwie).</span><span class="sxs-lookup"><span data-stu-id="81824-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="81824-297">W przypadku metod i właściwości z argumentami lista argumentów następuje w nawiasach.</span><span class="sxs-lookup"><span data-stu-id="81824-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="81824-298">W przypadku tych bez argumentów nawiasy są pomijane.</span><span class="sxs-lookup"><span data-stu-id="81824-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="81824-299">Argumenty są rozdzielone przecinkami.</span><span class="sxs-lookup"><span data-stu-id="81824-299">The arguments are separated by commas.</span></span> <span data-ttu-id="81824-300">Kodowanie każdego argumentu jest takie samo jak sygnatura interfejsu wiersza polecenia w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="81824-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="81824-301">Argumenty są reprezentowane przez nazwę dokumentacji, która jest oparta na ich w pełni kwalifikowanej nazwie, modyfikowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="81824-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="81824-302">Argumenty reprezentujące typy ogólne mają dołączony `` ` `` znak (symbol wieloznaczny), po którym następuje liczba parametrów typu</span><span class="sxs-lookup"><span data-stu-id="81824-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="81824-303">Argumenty mające `out` modyfikator `ref` or mają `@` następującą nazwę typu.</span><span class="sxs-lookup"><span data-stu-id="81824-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="81824-304">Argumenty przekazane przez wartość lub za `params` pośrednictwem nie mają specjalnej notacji.</span><span class="sxs-lookup"><span data-stu-id="81824-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="81824-305">Argumenty, które są tablicami, `[lowerbound:size, ... , lowerbound:size]` są reprezentowane, gdy liczba przecinki jest rzędu mniejszym od 1, a dolne granice i rozmiar każdego wymiaru, jeśli są znane, są reprezentowane w postaci dziesiętnej.</span><span class="sxs-lookup"><span data-stu-id="81824-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="81824-306">Jeśli Dolna granica nie zostanie określona, zostanie pominięta.</span><span class="sxs-lookup"><span data-stu-id="81824-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="81824-307">Jeśli Dolna granica i rozmiar określonego wymiaru zostaną pominięte, `:` również zostanie pominięty.</span><span class="sxs-lookup"><span data-stu-id="81824-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="81824-308">Tablice nieregularne są reprezentowane przez jeden `[]` na poziom.</span><span class="sxs-lookup"><span data-stu-id="81824-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="81824-309">Argumenty, które mają typy wskaźnika inne niż void, są reprezentowane `*` przy użyciu następującej nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="81824-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="81824-310">Wskaźnik void jest reprezentowany przy użyciu nazwy `System.Void`typu.</span><span class="sxs-lookup"><span data-stu-id="81824-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="81824-311">Argumenty odwołujące się do parametrów typu ogólnego zdefiniowane w typach są kodowane `` ` `` przy użyciu znaku (znacznika kreskowego), po którym następuje indeks (liczony od zera) parametru typu.</span><span class="sxs-lookup"><span data-stu-id="81824-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="81824-312">Argumenty, które używają parametrów typu ogólnego zdefiniowane w metodach, używają podwójnego ``` `` ``` taktu zamiast `` ` `` użycia dla typów.</span><span class="sxs-lookup"><span data-stu-id="81824-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="81824-313">Argumenty odwołujące się do skonstruowanych typów ogólnych są kodowane przy użyciu typu ogólnego `{`, po którym następuje rozdzielana przecinkami lista argumentów typu, `}`po których następuje.</span><span class="sxs-lookup"><span data-stu-id="81824-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="81824-314">Przykłady identyfikatora ciągu</span><span class="sxs-lookup"><span data-stu-id="81824-314">ID string examples</span></span>

<span data-ttu-id="81824-315">Poniższe przykłady pokazują fragment C# kodu wraz z ciągiem identyfikatora utworzonym z każdego elementu źródłowego, który może mieć komentarz dokumentacji:</span><span class="sxs-lookup"><span data-stu-id="81824-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="81824-316">Typy są reprezentowane przy użyciu ich w pełni kwalifikowanej nazwy rozszerzonej o informacje ogólne:</span><span class="sxs-lookup"><span data-stu-id="81824-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

   ```csharp
   enum Color { Red, Blue, Green }

   namespace Acme
   {
       interface IProcess {...}

       struct ValueType {...}

       class Widget: IProcess
       {
           public class NestedClass {...}
           public interface IMenuItem {...}
           public delegate void Del(int i);
           public enum Direction { North, South, East, West }
       }

       class MyList<T>
       {
           class Helper<U,V> {...}
       }
   }

   "T:Color"
   "T:Acme.IProcess"
   "T:Acme.ValueType"
   "T:Acme.Widget"
   "T:Acme.Widget.NestedClass"
   "T:Acme.Widget.IMenuItem"
   "T:Acme.Widget.Del"
   "T:Acme.Widget.Direction"
   "T:Acme.MyList`1"
   "T:Acme.MyList`1.Helper`2"
   ```

*  <span data-ttu-id="81824-317">Pola są reprezentowane przez ich w pełni kwalifikowaną nazwę:</span><span class="sxs-lookup"><span data-stu-id="81824-317">Fields are represented by their fully qualified name:</span></span>

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           private int total;
       }
   
       class Widget: IProcess
       {
           public class NestedClass
           {
               private int value;
           }
   
           private string message;
           private static Color defaultColor;
           private const double PI = 3.14159;
           protected readonly double monthlyAverage;
           private long[] array1;
           private Widget[,] array2;
           private unsafe int *pCount;
           private unsafe float **ppValues;
       }
   }

   "F:Acme.ValueType.total"
   "F:Acme.Widget.NestedClass.value"
   "F:Acme.Widget.message"
   "F:Acme.Widget.defaultColor"
   "F:Acme.Widget.PI"
   "F:Acme.Widget.monthlyAverage"
   "F:Acme.Widget.array1"
   "F:Acme.Widget.array2"
   "F:Acme.Widget.pCount"
   "F:Acme.Widget.ppValues"
   ```

*  <span data-ttu-id="81824-318">Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="81824-318">Constructors.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           static Widget() {...}
           public Widget() {...}
           public Widget(string s) {...}
       }
   }

   "M:Acme.Widget.#cctor"
   "M:Acme.Widget.#ctor"
   "M:Acme.Widget.#ctor(System.String)"
   ```

*  <span data-ttu-id="81824-319">Destruktory.</span><span class="sxs-lookup"><span data-stu-id="81824-319">Destructors.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           ~Widget() {...}
       }
   }
   
   "M:Acme.Widget.Finalize"
   ```

*  <span data-ttu-id="81824-320">Form.</span><span class="sxs-lookup"><span data-stu-id="81824-320">Methods.</span></span>

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           public void M(int i) {...}
       }

       class Widget: IProcess
       {
           public class NestedClass
           {
               public void M(int i) {...}
           }

           public static void M0() {...}
           public void M1(char c, out float f, ref ValueType v) {...}
           public void M2(short[] x1, int[,] x2, long[][] x3) {...}
           public void M3(long[][] x3, Widget[][,,] x4) {...}
           public unsafe void M4(char *pc, Color **pf) {...}
           public unsafe void M5(void *pv, double *[][,] pd) {...}
           public void M6(int i, params object[] args) {...}
       }

       class MyList<T>
       {
           public void Test(T t) { }
       }

       class UseList
       {
           public void Process(MyList<int> list) { }
           public MyList<T> GetValues<T>(T inputValue) { return null; }
       }
   }

   "M:Acme.ValueType.M(System.Int32)"
   "M:Acme.Widget.NestedClass.M(System.Int32)"
   "M:Acme.Widget.M0"
   "M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
   "M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
   "M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
   "M:Acme.Widget.M4(System.Char*,Color**)"
   "M:Acme.Widget.M5(System.Void*,System.Double*[0:,0:][])"
   "M:Acme.Widget.M6(System.Int32,System.Object[])"
   "M:Acme.MyList`1.Test(`0)"
   "M:Acme.UseList.Process(Acme.MyList{System.Int32})"
   "M:Acme.UseList.GetValues``(``0)"
   ```

*  <span data-ttu-id="81824-321">Właściwości i indeksatory.</span><span class="sxs-lookup"><span data-stu-id="81824-321">Properties and indexers.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public int Width { get {...} set {...} }
           public int this[int i] { get {...} set {...} }
           public int this[string s, int i] { get {...} set {...} }
       }
   }

   "P:Acme.Widget.Width"
   "P:Acme.Widget.Item(System.Int32)"
   "P:Acme.Widget.Item(System.String,System.Int32)"
   ```

*  <span data-ttu-id="81824-322">Wydarzeniach.</span><span class="sxs-lookup"><span data-stu-id="81824-322">Events.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public event Del AnEvent;
       }
   }

   "E:Acme.Widget.AnEvent"
   ```

*  <span data-ttu-id="81824-323">Operatory jednoargumentowe.</span><span class="sxs-lookup"><span data-stu-id="81824-323">Unary operators.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
   ```

   <span data-ttu-id="81824-324">Pełny zestaw nazw funkcji operatora jednoargumentowego jest następujący `op_UnaryPlus`:, `op_UnaryNegation` `op_OnesComplement` `op_Increment` `op_LogicalNot` `op_Decrement` `op_False`,,,,, ,i.`op_True`</span><span class="sxs-lookup"><span data-stu-id="81824-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="81824-325">Operatory binarne.</span><span class="sxs-lookup"><span data-stu-id="81824-325">Binary operators.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x1, Widget x2) {...}
       }
   }

   "M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
   ```

   <span data-ttu-id="81824-326">Pełny zestaw nazw funkcji operatora binarnego jest następujący: `op_Addition`, `op_BitwiseOr` `op_ExclusiveOr` `op_BitwiseAnd` `op_Subtraction`, `op_Modulus` `op_Division` `op_Multiply`,,,, `op_LeftShift` ,`op_RightShift`,,, `op_Equality`, `op_Inequality`, ,,`op_LessThan`i .`op_GreaterThanOrEqual` `op_LessThanOrEqual` `op_GreaterThan`</span><span class="sxs-lookup"><span data-stu-id="81824-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="81824-327">Operatory konwersji mają znak końcowy "`~`", po którym następuje zwracany typ.</span><span class="sxs-lookup"><span data-stu-id="81824-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static explicit operator int(Widget x) {...}
           public static implicit operator long(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
   "M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
   ```

## <a name="an-example"></a><span data-ttu-id="81824-328">Przykład</span><span class="sxs-lookup"><span data-stu-id="81824-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="81824-329">C#kod źródłowy</span><span class="sxs-lookup"><span data-stu-id="81824-329">C# source code</span></span>

<span data-ttu-id="81824-330">Poniższy przykład pokazuje kod `Point` źródłowy klasy:</span><span class="sxs-lookup"><span data-stu-id="81824-330">The following example shows the source code of a `Point` class:</span></span>

```csharp
namespace Graphics
{

/// <summary>Class <c>Point</c> models a point in a two-dimensional plane.
/// </summary>
public class Point 
{

    /// <summary>Instance variable <c>x</c> represents the point's
    ///    x-coordinate.</summary>
    private int x;

    /// <summary>Instance variable <c>y</c> represents the point's
    ///    y-coordinate.</summary>
    private int y;

    /// <value>Property <c>X</c> represents the point's x-coordinate.</value>
    public int X
    {
        get { return x; }
        set { x = value; }
    }

    /// <value>Property <c>Y</c> represents the point's y-coordinate.</value>
    public int Y
    {
        get { return y; }
        set { y = value; }
    }

    /// <summary>This constructor initializes the new Point to
    ///    (0,0).</summary>
    public Point() : this(0,0) {}

    /// <summary>This constructor initializes the new Point to
    ///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
    /// <param><c>xor</c> is the new Point's x-coordinate.</param>
    /// <param><c>yor</c> is the new Point's y-coordinate.</param>
    public Point(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location to
    ///    the given coordinates.</summary>
    /// <param><c>xor</c> is the new x-coordinate.</param>
    /// <param><c>yor</c> is the new y-coordinate.</param>
    /// <see cref="Translate"/>
    public void Move(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location by
    ///    the given x- and y-offsets.
    /// <example>For example:
    /// <code>
    ///    Point p = new Point(3,5);
    ///    p.Translate(-1,3);
    /// </code>
    /// results in <c>p</c>'s having the value (2,8).
    /// </example>
    /// </summary>
    /// <param><c>xor</c> is the relative x-offset.</param>
    /// <param><c>yor</c> is the relative y-offset.</param>
    /// <see cref="Move"/>
    public void Translate(int xor, int yor) {
        X += xor;
        Y += yor;
    }

    /// <summary>This method determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>o</c> is the object to be compared to the current object.
    /// </param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="operator=="/>
    /// <seealso cref="operator!="/>
    public override bool Equals(object o) {
        if (o == null) {
            return false;
        }

        if (this == o) {
            return true;
        }

        if (GetType() == o.GetType()) {
            Point p = (Point)o;
            return (X == p.X) && (Y == p.Y);
        }
        return false;
    }

    /// <summary>Report a point's location as a string.</summary>
    /// <returns>A string representing a point's location, in the form (x,y),
    ///    without any leading, training, or embedded whitespace.</returns>
    public override string ToString() {
        return "(" + X + "," + Y + ")";
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator!="/>
    public static bool operator==(Point p1, Point p2) {
        if ((object)p1 == null || (object)p2 == null) {
            return false;
        }

        if (p1.GetType() == p2.GetType()) {
            return (p1.X == p2.X) && (p1.Y == p2.Y);
        }

        return false;
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points do not have the same location and the
    ///    exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator=="/>
    public static bool operator!=(Point p1, Point p2) {
        return !(p1 == p2);
    }

    /// <summary>This is the entry point of the Point class testing
    /// program.
    /// <para>This program tests each method and operator, and
    /// is intended to be run after any non-trivial maintenance has
    /// been performed on the Point class.</para></summary>
    public static void Main() {
        // class test code goes here
    }
}
}
```

### <a name="resulting-xml"></a><span data-ttu-id="81824-331">Wyniki XML</span><span class="sxs-lookup"><span data-stu-id="81824-331">Resulting XML</span></span>

<span data-ttu-id="81824-332">Poniżej przedstawiono dane wyjściowe generowane przez jeden generator dokumentacji, gdy podano kod źródłowy klasy `Point`, pokazany powyżej:</span><span class="sxs-lookup"><span data-stu-id="81824-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <summary>Class <c>Point</c> models a point in a two-dimensional
            plane.
            </summary>
        </member>

        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>

        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>

        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
        (0,0).</summary>
        </member>

        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
            <param><c>xor</c> is the new Point's x-coordinate.</param>
            <param><c>yor</c> is the new Point's y-coordinate.</param>
        </member>

        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>xor</c> is the new x-coordinate.</param>
            <param><c>yor</c> is the new y-coordinate.</param>
            <see cref="M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>

        <member
            name="M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by
            the given x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>xor</c> is the relative x-offset.</param>
            <param><c>yor</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>

        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the same
            location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.
            </param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
            <seealso
      cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y),
            without any leading, training, or embedded whitespace.</returns>
        </member>

        <member
       name="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
     cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member
      name="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the
            exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trivial maintenance has
            been performed on the Point class.</para></summary>
        </member>

        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>

        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
