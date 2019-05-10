---
ms.openlocfilehash: c9f8417dc68153f02ceb72bb1d51f3615f3c4961
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488914"
---
# <a name="documentation-comments"></a><span data-ttu-id="e0bc7-101">Komentarze dokumentacji</span><span class="sxs-lookup"><span data-stu-id="e0bc7-101">Documentation comments</span></span>

<span data-ttu-id="e0bc7-102">C# zawiera mechanizmu dla programistów udokumentować kod przy użyciu specjalnej składni komentarza zawierający tekst XML.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="e0bc7-103">W plikach kodu źródłowego komentarze o niektórych formularza może służyć do kierowania narzędzia do tworzenia XML, mimo że takie komentarze i elementy kodu źródłowego, które mogą poprzedzać.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="e0bc7-104">Komentarze przy użyciu składni takie są nazywane ***komentarzy dokumentacji***.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="e0bc7-105">Musi bezpośrednio poprzedzać typu zdefiniowanego przez użytkownika (takie jak klasy, delegata lub interfejsu) lub elementu członkowskiego (na przykład pola, zdarzenia, właściwość lub metoda).</span><span class="sxs-lookup"><span data-stu-id="e0bc7-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="e0bc7-106">Narzędzie do generowania XML nosi nazwę ***generator dokumentacji***.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="e0bc7-107">(Tego generatora może być, ale nie muszą być, kompilator języka C#, sam). Dane wyjściowe wytwarzane przez generator dokumentacji jest nazywany ***soubor dokumentace***.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="e0bc7-108">Plik dokumentacji jest używany jako dane wejściowe ***podglądu dokumentacji***; narzędzie przeznaczone do produkcji jakieś wizualizacji do wyświetlenia informacji o typie i jego skojarzone dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="e0bc7-109">Tej specyfikacji sugeruje zestaw znaczników, które ma być używany w komentarzach dokumentacji, ale korzystanie z tych tagów nie jest wymagane i inne tagi mogą być używane w razie potrzeby, jak długo reguły poprawnie sformułowany dokument XML są przestrzegane.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="e0bc7-110">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="e0bc7-110">Introduction</span></span>

<span data-ttu-id="e0bc7-111">Komentarze o specjalną postać może służyć do kierowania narzędzia do tworzenia XML, mimo że takie komentarze i elementy kodu źródłowego, które mogą poprzedzać.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="e0bc7-112">Takie komentarze są Komentarze jednowierszowe, rozpoczynające się z trzema ukośnikami (`///`), lub rozdzielanym komentarzy rozpoczynających się od ukośnika i dwóch gwiazdek (`/**`).</span><span class="sxs-lookup"><span data-stu-id="e0bc7-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="e0bc7-113">Musi bezpośrednio poprzedzać typu zdefiniowanego przez użytkownika (takie jak klasy, delegata lub interfejsu) lub element członkowski (na przykład pola, zdarzenia, właściwość lub metoda), który mogą dodawać adnotacje.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="e0bc7-114">Atrybut sekcje ([Specyfikacja atrybutu](attributes.md#attribute-specification)) są traktowane jako część deklaracji, dzięki czemu komentarzy do dokumentacji musi poprzedzać atrybuty stosowane do typu lub elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="e0bc7-115">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="e0bc7-116">W *single_line_doc_comment*, jeśli istnieje *odstępu* następujący znak `///` znaków na każdym *single_line_doc_comment*s sąsiadująco do bieżącego *single_line_doc_comment*, który następnie *odstępu* znak nie jest uwzględniony w danych wyjściowych XML.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="e0bc7-117">Rozdzielany doc — komentarza Jeśli pierwszy znak niebędący odstępem w drugim wierszu jest znak gwiazdki i tym samym wzorcem, opcjonalny odstęp i znaku gwiazdki jest powtarzany na początku każdego wiersza w rozdzielonych doc — komentarz, następnie znaki powtarzanych wzorca nie są uwzględnione w danych wyjściowych XML.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="e0bc7-118">Wzorzec może zawierać białych znaków, po, a także przed znakiem gwiazdki.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="e0bc7-119">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-119">__Example:__</span></span>

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

<span data-ttu-id="e0bc7-120">Tekst w ramach komentarzy do dokumentacji musi być poprawnie sformułowany zgodnie z regułami XML (https://www.w3.org/TR/REC-xml).</span><span class="sxs-lookup"><span data-stu-id="e0bc7-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="e0bc7-121">Jeśli kod XML jest ill sformułowany, generowane jest ostrzeżenie, a plik dokumentacja będzie zawierać komentarz informujący o tym, że wystąpił błąd podczas.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="e0bc7-122">Mimo że Deweloperzy są bezpłatne tworzenie własnych zestawów tagów, zalecany zestaw jest zdefiniowany w [zalecane tagi](documentation-comments.md#recommended-tags).</span><span class="sxs-lookup"><span data-stu-id="e0bc7-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="e0bc7-123">Niektóre zalecane tagi mają specjalne znaczenie:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="e0bc7-124">`<param>` Tag jest używany do opisania parametrów.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="e0bc7-125">Jeśli takie tag jest używany, generator dokumentacji musi sprawdzić, czy określony parametr istnieje i czy wszystkie parametry są opisane w komentarzach dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="e0bc7-126">Jeśli taka weryfikacja zakończy się niepowodzeniem, generator dokumentacji generuje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="e0bc7-127">`cref` Atrybutu mogą być dołączane do każdego znacznika, aby zapewnić odwołanie do elementu kodu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="e0bc7-128">Generator dokumentacji należy sprawdzić, czy ten element kodu istnieje.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="e0bc7-129">Jeśli weryfikacja zakończy się niepowodzeniem, generator dokumentacji generuje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="e0bc7-130">Podczas wyszukiwania dla nazwy opisanego w `cref` atrybutu, generator dokumentacji muszą przestrzegać widoczność przestrzeni nazw zgodnie z opisem w `using` instrukcji w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="e0bc7-131">Dla elementów kodu znajdujące ogólnego normalne ogólna składnia (czyli "`List<T>`") nie można użyć, ponieważ generuje nieprawidłowy kod XML.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="e0bc7-132">Nawiasy klamrowe można używać zamiast nawiasy kwadratowe (czyli "`List{T}`"), lub składni XML ucieczki mogą być używane (oznacza to, "`List&lt;T&gt;`").</span><span class="sxs-lookup"><span data-stu-id="e0bc7-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="e0bc7-133">`<summary>` Tag jest przeznaczona do użycia przez Podgląd dokumentacji, aby wyświetlić dodatkowe informacje na temat typu lub elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="e0bc7-134">`<include>` Tag zawierały informacje z zewnętrznego pliku XML.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="e0bc7-135">Należy dokładnie, czy plik dokumentacji nie zapewnia pełne informacje na temat typów i elementów członkowskich (na przykład, go nie zawiera żadnych informacji o typie).</span><span class="sxs-lookup"><span data-stu-id="e0bc7-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="e0bc7-136">Aby uzyskać informacje dotyczące typu lub elementu członkowskiego, konieczne jest użycie pliku dokumentacji, w połączeniu z odbiciem na rzeczywisty typ lub element członkowski.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="e0bc7-137">Zalecane tagi</span><span class="sxs-lookup"><span data-stu-id="e0bc7-137">Recommended tags</span></span>

<span data-ttu-id="e0bc7-138">Generator dokumentacji musi przyjmował i przetwarzał dowolny tag, który jest prawidłowy, zgodnie z regułami XML.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="e0bc7-139">Następujące znaczniki oferuje powszechnie używanych w dokumentacji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="e0bc7-140">(Oczywiście innych tagów są możliwe).</span><span class="sxs-lookup"><span data-stu-id="e0bc7-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="e0bc7-141">__Tag__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-141">__Tag__</span></span>          | <span data-ttu-id="e0bc7-142">__Sekcja__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-142">__Section__</span></span>                                            | <span data-ttu-id="e0bc7-143">__Cel__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="e0bc7-144">Ustaw tekst czcionką podobny kod</span><span class="sxs-lookup"><span data-stu-id="e0bc7-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="e0bc7-145">Ustaw jeden lub więcej wierszy źródła kodu lub dane wyjściowe programu</span><span class="sxs-lookup"><span data-stu-id="e0bc7-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="e0bc7-146">Przykład wskazać</span><span class="sxs-lookup"><span data-stu-id="e0bc7-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="e0bc7-147">Określa metodę może zgłosić wyjątek</span><span class="sxs-lookup"><span data-stu-id="e0bc7-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="e0bc7-148">Obejmuje XML z pliku zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="e0bc7-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="e0bc7-149">Tworzenie listy lub tabeli</span><span class="sxs-lookup"><span data-stu-id="e0bc7-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="e0bc7-150">Zezwala na strukturę, które mają zostać dodane do tekstu</span><span class="sxs-lookup"><span data-stu-id="e0bc7-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="e0bc7-151">Opis parametru dla metody lub konstruktora</span><span class="sxs-lookup"><span data-stu-id="e0bc7-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="e0bc7-152">Zidentyfikować wyrazem nazwę parametru</span><span class="sxs-lookup"><span data-stu-id="e0bc7-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="e0bc7-153">Dokument ułatwień dostępu zabezpieczeń elementu członkowskiego</span><span class="sxs-lookup"><span data-stu-id="e0bc7-153">Document the security accessibility of a member</span></span>        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | <span data-ttu-id="e0bc7-154">Przedstawiono dodatkowe informacje o typie</span><span class="sxs-lookup"><span data-stu-id="e0bc7-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="e0bc7-155">Opisz wartość zwracaną przez metodę</span><span class="sxs-lookup"><span data-stu-id="e0bc7-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="e0bc7-156">Podaj łącze</span><span class="sxs-lookup"><span data-stu-id="e0bc7-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="e0bc7-157">Generuj wpis Zobacz też</span><span class="sxs-lookup"><span data-stu-id="e0bc7-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="e0bc7-158">Opis typu lub składowej typu</span><span class="sxs-lookup"><span data-stu-id="e0bc7-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="e0bc7-159">Opis właściwości</span><span class="sxs-lookup"><span data-stu-id="e0bc7-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="e0bc7-160">Opis parametru typu ogólnego</span><span class="sxs-lookup"><span data-stu-id="e0bc7-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="e0bc7-161">Zidentyfikować wyrazem nazwie parametru typu</span><span class="sxs-lookup"><span data-stu-id="e0bc7-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="e0bc7-162">Ten tag udostępnia mechanizm do wskazania, że fragment tekstu w obrębie opis powinna zostać ustawiona w specjalne takim dla bloku kodu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="e0bc7-163">Linie rzeczywisty kod, można użyć `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="e0bc7-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="e0bc7-164">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="e0bc7-165">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="e0bc7-166">Ten tag jest używany, aby ustawić jeden lub więcej wierszy źródła kodu lub dane wyjściowe programu w niektórych specjalne.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="e0bc7-167">Mały kod fragmentów w narracja, można użyć `<c>` ([`<c>`](documentation-comments.md#c)).</span><span class="sxs-lookup"><span data-stu-id="e0bc7-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="e0bc7-168">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="e0bc7-169">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-169">__Example:__</span></span>

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

<span data-ttu-id="e0bc7-170">Ten tag umożliwia przykładowy kod w komentarzu, aby określić, jak można użyć metody lub innego członka biblioteki.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="e0bc7-171">Zazwyczaj także wymagałoby to użycia tagu `<code>` ([`<code>`](documentation-comments.md#code)) również.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="e0bc7-172">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="e0bc7-173">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-173">__Example:__</span></span>

<span data-ttu-id="e0bc7-174">Zobacz `<code>` ([`<code>`](documentation-comments.md#code)) przykład.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="e0bc7-175">Ten tag umożliwia dokumentowanie metody może zgłosić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="e0bc7-176">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="e0bc7-177">gdzie</span><span class="sxs-lookup"><span data-stu-id="e0bc7-177">where</span></span>

* <span data-ttu-id="e0bc7-178">`member` jest nazwą elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-178">`member` is the name of a member.</span></span> <span data-ttu-id="e0bc7-179">Generator dokumentacji sprawdza, czy dany element istnieje i czy tłumaczy `member` do nazwy kanonicznej elementu w pliku dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="e0bc7-180">`description` znajduje się opis sytuacji, w których jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="e0bc7-181">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-181">__Example:__</span></span>

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

<span data-ttu-id="e0bc7-182">Ten tag umożliwia, łącznie z informacjami z dokumentu XML, który jest zewnętrzne w stosunku do pliku kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="e0bc7-183">Zewnętrzny plik musi być poprawnie sformułowany dokument XML, a wyrażenie XPath jest stosowany do tego dokumentu, aby określić, jakie XML z tego dokumentu do uwzględnienia.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="e0bc7-184">`<include>` Tag następnie zastępowane wybrane XML z dokumentu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="e0bc7-185">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="e0bc7-186">gdzie</span><span class="sxs-lookup"><span data-stu-id="e0bc7-186">where</span></span>

* <span data-ttu-id="e0bc7-187">`filename` jest nazwą pliku z zewnętrznego pliku XML.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="e0bc7-188">Nazwa pliku jest interpretowane względem pliku który zawiera tagu include.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="e0bc7-189">`xpath` to wyrażenie XPath wybierające niektóre z pliku XML z zewnętrznego pliku XML.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="e0bc7-190">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-190">__Example:__</span></span>

<span data-ttu-id="e0bc7-191">Jeśli kod źródłowy zawiera deklarację, takich jak:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="e0bc7-192">i zewnętrznego pliku "docs.xml" miało następującą zawartość:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="e0bc7-193">następnie ten sam dokumentacji przedstawiono dane wyjściowe, tak, jakby zawarte w kodzie źródłowym:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="e0bc7-194">Ten tag jest używany do tworzenia listy lub tabela elementów.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="e0bc7-195">Może on zawierać `<listheader>` bloku, aby zdefiniować wiersz nagłówka tabeli lub definicji listy.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="e0bc7-196">(Podczas definiowania tabeli, tylko wpis dla `term` w nagłówku muszą być dostarczone.)</span><span class="sxs-lookup"><span data-stu-id="e0bc7-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="e0bc7-197">Każdy element na liście jest określony za pomocą `<item>` bloku.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="e0bc7-198">Podczas tworzenia listy definicji zarówno `term` i `description` musi być określona.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="e0bc7-199">Jednak dla tabeli, listy punktowanej lub listę numerowaną, tylko `description` muszą być określone.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="e0bc7-200">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-200">__Syntax:__</span></span>

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

<span data-ttu-id="e0bc7-201">gdzie</span><span class="sxs-lookup"><span data-stu-id="e0bc7-201">where</span></span>

* <span data-ttu-id="e0bc7-202">`term` to wyrażenie, aby zdefiniować, którego definicja jest `description`.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="e0bc7-203">`description` Element punktora lub Lista numerowana lub definicji `term`.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="e0bc7-204">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-204">__Example:__</span></span>

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

<span data-ttu-id="e0bc7-205">Ten tag jest przeznaczona do użytku wewnątrz innych tagów, takich jak `<summary>` ([`<remark>`](documentation-comments.md#remark)) lub `<returns>` ([`<returns>`](documentation-comments.md#returns)) i pozwala na strukturę, które mają zostać dodane do tekstu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-205">This tag is for use inside other tags, such as `<summary>` ([`<remark>`](documentation-comments.md#remark)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="e0bc7-206">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="e0bc7-207">gdzie `content` jest tekst akapitu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="e0bc7-208">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-208">__Example:__</span></span>

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

<span data-ttu-id="e0bc7-209">Ten tag jest używany do opisania parametrów dla metody, Konstruktor lub indeksatora.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="e0bc7-210">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="e0bc7-211">gdzie</span><span class="sxs-lookup"><span data-stu-id="e0bc7-211">where</span></span>

* <span data-ttu-id="e0bc7-212">`name` jest nazwą parametru.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="e0bc7-213">`description` znajduje się opis parametru.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="e0bc7-214">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-214">__Example:__</span></span>

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

<span data-ttu-id="e0bc7-215">Ten tag jest używany do wskazania, że wyraz jest parametrem.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="e0bc7-216">Plik dokumentacji mogą być przetwarzane, aby sformatować ten parametr w jakiś sposób distinct.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="e0bc7-217">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="e0bc7-218">gdzie `name` jest nazwą parametru.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="e0bc7-219">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-219">__Example:__</span></span>

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

<span data-ttu-id="e0bc7-220">Ten tag umożliwia ułatwień dostępu zabezpieczeń elementu członkowskiego do udokumentowania.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="e0bc7-221">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="e0bc7-222">gdzie</span><span class="sxs-lookup"><span data-stu-id="e0bc7-222">where</span></span>

* <span data-ttu-id="e0bc7-223">`member` jest nazwą elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-223">`member` is the name of a member.</span></span> <span data-ttu-id="e0bc7-224">Generator dokumentacji sprawdza, czy dany element kodu istnieje i wykonuje translację *elementu członkowskiego* do nazwy kanonicznej elementu w pliku dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="e0bc7-225">`description` znajduje się opis dostępu do elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="e0bc7-226">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

<span data-ttu-id="e0bc7-227">Ten tag jest używany do określenia dodatkowych informacji o typie.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="e0bc7-228">(Użyj `<summary>` ([`<summary>`](documentation-comments.md#summary)) do opisywania samego typu i elementy członkowskie typu.)</span><span class="sxs-lookup"><span data-stu-id="e0bc7-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="e0bc7-229">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-229">__Syntax:__</span></span>

```xml
<remark>description</remark>
```

<span data-ttu-id="e0bc7-230">gdzie `description` jest tekst uwagi.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="e0bc7-231">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-231">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remark>Uses polar coordinates</remark>
public class Point 
{
    // ...
}
```

### `<returns>`

<span data-ttu-id="e0bc7-232">Ten tag jest używany do opisania wartość zwracaną przez metodę.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="e0bc7-233">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="e0bc7-234">gdzie `description` znajduje się opis wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="e0bc7-235">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="e0bc7-236">Ten tag umożliwia łącza, należy określić w tekście.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="e0bc7-237">Użyj `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) do wskazania tekst, który jest wyświetlany w sekcji Zobacz też.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="e0bc7-238">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="e0bc7-239">gdzie `member` jest nazwa elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-239">where `member` is the name of a member.</span></span> <span data-ttu-id="e0bc7-240">Generator dokumentacji sprawdza, czy dany element kodu istnieje i zmienia *elementu członkowskiego* do nazwy elementu w pliku wygenerowaną dokumentację.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="e0bc7-241">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-241">__Example:__</span></span>

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

<span data-ttu-id="e0bc7-242">Ten tag umożliwia wpis do wygenerowania dla sekcji Zobacz też.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="e0bc7-243">Użyj `<see>` ([`<see>`](documentation-comments.md#see)) do określenia łącze między w tekście.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="e0bc7-244">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="e0bc7-245">gdzie `member` jest nazwa elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-245">where `member` is the name of a member.</span></span> <span data-ttu-id="e0bc7-246">Generator dokumentacji sprawdza, czy dany element kodu istnieje i zmienia *elementu członkowskiego* do nazwy elementu w pliku wygenerowaną dokumentację.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="e0bc7-247">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-247">__Example:__</span></span>

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

Ten tag może służyć do opisu typu lub składowej typu. <span data-ttu-id="e0bc7-249">Użyj `<remark>` ([`<remark>`](documentation-comments.md#remark)) do opisywania samego typu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-249">Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) to describe the type itself.</span></span>

<span data-ttu-id="e0bc7-250">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="e0bc7-251">gdzie `description` znajduje się podsumowanie typu lub elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="e0bc7-252">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="e0bc7-253">Ten tag umożliwia właściwość, która ma być opisane.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="e0bc7-254">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="e0bc7-255">gdzie `property description` znajduje się opis właściwości.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="e0bc7-256">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="e0bc7-257">Ten tag jest używany do opisania parametr typu ogólnego dla klasy, struktury, interfejsu, delegata lub metody.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="e0bc7-258">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="e0bc7-259">gdzie `name` to nazwa parametru typu i `description` jest jego opis.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="e0bc7-260">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="e0bc7-261">Ten tag jest używany do wskazania, że wyraz jest parametrem typu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="e0bc7-262">Plik dokumentacji mogą być przetwarzane, aby sformatować ten parametr typu w jakiś sposób distinct.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="e0bc7-263">__Składnia:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="e0bc7-264">gdzie `name` jest nazwa parametru typu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="e0bc7-265">__Przykład:__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="e0bc7-266">Przetwarzanie pliku dokumentacji</span><span class="sxs-lookup"><span data-stu-id="e0bc7-266">Processing the documentation file</span></span>

<span data-ttu-id="e0bc7-267">Generator dokumentacji generuje ciąg Identyfikatora dla każdego elementu w kodzie źródłowym, który jest oznaczony za pomocą komentarzy dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="e0bc7-268">Ciąg ten identyfikator unikatowo identyfikuje element źródła.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="e0bc7-269">Podgląd dokumentacja umożliwia zidentyfikować odpowiedni element metadanych/odbicia, do której stosują się dokumentacji ciąg Identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="e0bc7-270">Plik dokumentacji nie jest hierarchiczną reprezentację kodu źródłowego; jest to raczej płaskiej listy z ciągiem Identyfikatora wygenerowany dla każdego elementu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="e0bc7-271">Format ciągu Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="e0bc7-271">ID string format</span></span>

<span data-ttu-id="e0bc7-272">Podczas generowania ciągi identyfikatorów, generator dokumentacji przestrzega następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="e0bc7-273">Biały znak nie zostanie umieszczony w ciągu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="e0bc7-274">Pierwsza część ciągu określa rodzaj elementu członkowskiego udokumentowane, za pośrednictwem pojedynczy znak z dwukropkiem.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="e0bc7-275">Zdefiniowane są następujące rodzaje składowych:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="e0bc7-276">__Znak__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-276">__Character__</span></span> | <span data-ttu-id="e0bc7-277">__Opis__</span><span class="sxs-lookup"><span data-stu-id="e0bc7-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="e0bc7-278">E</span><span class="sxs-lookup"><span data-stu-id="e0bc7-278">E</span></span>             | <span data-ttu-id="e0bc7-279">Zdarzenie</span><span class="sxs-lookup"><span data-stu-id="e0bc7-279">Event</span></span>                                                       |
   | <span data-ttu-id="e0bc7-280">F</span><span class="sxs-lookup"><span data-stu-id="e0bc7-280">F</span></span>             | <span data-ttu-id="e0bc7-281">Pole</span><span class="sxs-lookup"><span data-stu-id="e0bc7-281">Field</span></span>                                                       |
   | <span data-ttu-id="e0bc7-282">M</span><span class="sxs-lookup"><span data-stu-id="e0bc7-282">M</span></span>             | <span data-ttu-id="e0bc7-283">Metody (w tym konstruktory, destruktory i operatorów)</span><span class="sxs-lookup"><span data-stu-id="e0bc7-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="e0bc7-284">N</span><span class="sxs-lookup"><span data-stu-id="e0bc7-284">N</span></span>             | <span data-ttu-id="e0bc7-285">Przestrzeń nazw</span><span class="sxs-lookup"><span data-stu-id="e0bc7-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="e0bc7-286">P</span><span class="sxs-lookup"><span data-stu-id="e0bc7-286">P</span></span>             | <span data-ttu-id="e0bc7-287">Właściwości (łącznie z indeksatorów)</span><span class="sxs-lookup"><span data-stu-id="e0bc7-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="e0bc7-288">T</span><span class="sxs-lookup"><span data-stu-id="e0bc7-288">T</span></span>             | <span data-ttu-id="e0bc7-289">Wpisz (na przykład klasa, delegowanego, wyliczenia, interfejsu i struktury)</span><span class="sxs-lookup"><span data-stu-id="e0bc7-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="e0bc7-290">!</span><span class="sxs-lookup"><span data-stu-id="e0bc7-290">!</span></span>             | <span data-ttu-id="e0bc7-291">Ciąg błędu; Pozostała część ciągu zawiera informacje o tym błędzie.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="e0bc7-292">Na przykład generator dokumentacji generuje informacje o błędzie dla łączy, których nie można rozpoznać.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="e0bc7-293">Druga część ciągu jest w pełni kwalifikowana nazwa elementu, począwszy od głównego obszaru nazw.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="e0bc7-294">Nazwa elementu, jego otaczającego typów i przestrzeni nazw są oddzielone kropkami.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="e0bc7-295">Jeśli nazwa elementu zawiera kropek, są zastępowane przez `#(U+0023)` znaków.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="e0bc7-296">(Zakłada się, że element nie ma tego znaku w jego nazwę.)</span><span class="sxs-lookup"><span data-stu-id="e0bc7-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="e0bc7-297">Dla metod i właściwości z argumentami poniżej listy argumentów, ujęte w nawiasy.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="e0bc7-298">Dla osób, bez argumentów nawiasy są pomijane.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="e0bc7-299">Argumenty są oddzielone przecinkami.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-299">The arguments are separated by commas.</span></span> <span data-ttu-id="e0bc7-300">Kodowanie każdy argument jest taka sama jak sygnatury interfejsu wiersza polecenia w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="e0bc7-301">Argumenty są reprezentowane przez ich nazwy dokumentacji, która opiera się na ich w pełni kwalifikowaną nazwę, zmodyfikowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="e0bc7-302">Argumenty, które reprezentują typy rodzajowe mają dołączonych `` ` `` znak (początkowych), a następnie liczbę parametrów typu</span><span class="sxs-lookup"><span data-stu-id="e0bc7-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="e0bc7-303">Argumentów mających `out` lub `ref` modyfikator musi `@` zgodnie z ich nazwy typu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="e0bc7-304">Argumenty przekazywane przez wartość lub za pośrednictwem `params` mają nie specjalne notacji.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="e0bc7-305">Argumenty, które są tablice są reprezentowane jako `[lowerbound:size, ... , lowerbound:size]` gdzie liczba przecinków jest rangi minus jeden, a dolne granice i rozmiaru każdego wymiaru, jeśli jest znany, są reprezentowane w zapisie dziesiętnym.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="e0bc7-306">Jeśli dolna granica lub rozmiar nie zostanie określony, zostanie pominięty.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="e0bc7-307">W przypadku pominięcia dolną granicę i rozmiar w konkretnym wymiarze `:` pominięto w także.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="e0bc7-308">Tablice nieregularne są reprezentowane przez jedną `[]` na poziomie.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="e0bc7-309">Argumenty, które mają typ wskaźnika innego niż void są reprezentowane przy użyciu `*` po nazwie typu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="e0bc7-310">Pusty wskaźnik jest reprezentowane za pomocą nazwę typu `System.Void`.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="e0bc7-311">Argumenty, które odwołują się do parametrów typu genetycznego zdefiniowany dla typów są zakodowane przy użyciu `` ` `` znak (początkowych) następuje liczony od zera indeks parametru typu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="e0bc7-312">Argumenty, które używać parametrów typu ogólnego, zdefiniowane w metodach używać początkowych double ``` `` ``` zamiast `` ` `` używane dla typów.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="e0bc7-313">Argumenty, które odnoszą się do typów ogólnych stworzonego elementu są zakodowane przy użyciu typu ogólnego, a następnie `{`, następuje rozdzielana przecinkami lista argumentów typu, a następnie `}`.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="e0bc7-314">Identyfikator ciągu przykłady</span><span class="sxs-lookup"><span data-stu-id="e0bc7-314">ID string examples</span></span>

<span data-ttu-id="e0bc7-315">W poniższych przykładach każdego pokazano fragment kodu języka C# wraz z ciągiem Identyfikatora wyprodukowanych z każdego elementu źródłowego w stanie konieczności komentarza do dokumentacji:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="e0bc7-316">Typy są reprezentowane w pełni kwalifikowaną nazwę, rozszerzone informacje ogólne:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="e0bc7-317">Pola są reprezentowane przez ich w pełni kwalifikowana nazwa:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="e0bc7-318">Konstruktory.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-318">Constructors.</span></span>

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

*  <span data-ttu-id="e0bc7-319">Destruktory.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-319">Destructors.</span></span>

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

*  <span data-ttu-id="e0bc7-320">Metody.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-320">Methods.</span></span>

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

*  <span data-ttu-id="e0bc7-321">Właściwości i indeksatorów.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="e0bc7-322">zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-322">Events.</span></span>

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

*  <span data-ttu-id="e0bc7-323">Operatory jednoargumentowe.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-323">Unary operators.</span></span>

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

   <span data-ttu-id="e0bc7-324">Kompletny zestaw nazw funkcji operator jednoargumentowy używana jest następująca: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, i `op_False`.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="e0bc7-325">Operatory binarne.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-325">Binary operators.</span></span>

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

   <span data-ttu-id="e0bc7-326">Kompletny zestaw nazw funkcji operatora binarnego jest następująca: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, i `op_GreaterThanOrEqual`.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="e0bc7-327">Operatory konwersji ma końcowe "`~`" następuje zwracanego typu.</span><span class="sxs-lookup"><span data-stu-id="e0bc7-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="e0bc7-328">Przykład</span><span class="sxs-lookup"><span data-stu-id="e0bc7-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="e0bc7-329">Kod źródłowy języka C#</span><span class="sxs-lookup"><span data-stu-id="e0bc7-329">C# source code</span></span>

<span data-ttu-id="e0bc7-330">Poniższy przykład pokazuje kod źródłowy `Point` klasy:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="e0bc7-331">Wynikowy kod XML</span><span class="sxs-lookup"><span data-stu-id="e0bc7-331">Resulting XML</span></span>

<span data-ttu-id="e0bc7-332">Oto dane wyjściowe generowane przez jeden generator dokumentacji, gdy kod źródłowy dla klasy `Point`, jak pokazano powyżej:</span><span class="sxs-lookup"><span data-stu-id="e0bc7-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
