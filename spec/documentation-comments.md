---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912435"
---
# <a name="documentation-comments"></a>Komentarze dokumentacji

C#udostępnia mechanizm programisty do dokumentowania kodu przy użyciu specjalnej składni komentarzy zawierającej tekst XML. W plikach kodu źródłowego Komentarze z określonym formularzem mogą służyć do kierowania narzędziem do tworzenia kodu XML z tych komentarzy i elementów kodu źródłowego, które poprzedzają. Komentarze wykorzystujące taką składnię są nazywane ***komentarzami dokumentacji***. Muszą bezpośrednio poprzedzać typ zdefiniowany przez użytkownika (na przykład Klasa, delegat lub interfejs) lub element członkowski (na przykład pole, zdarzenie, właściwość lub metoda). Narzędzie generowania kodu XML jest nazywane ***generatorem dokumentacji***. (Ten generator może być, ale nie musi, sam C# kompilator). Dane wyjściowe generowane przez generator dokumentacji są nazywane ***plikiem dokumentacji***. Plik dokumentacji jest używany jako dane wejściowe do ***przeglądarki dokumentacji***programu. Narzędzie przeznaczone do tworzenia pewnego rodzaju wizualizacji graficznej informacji o typie i powiązanej z nią dokumentacji.

Ta specyfikacja sugeruje zestaw tagów, które mają być używane w komentarzach dokumentacji, ale użycie tych tagów nie jest wymagane, a inne Tagi mogą być używane w razie potrzeby, tak długo, jak są stosowane reguły poprawnie sformułowanego kodu XML.

## <a name="introduction"></a>Wprowadzenie

Komentarze zawierające specjalną formę mogą służyć do kierowania narzędzia do tworzenia kodu XML z tych komentarzy i elementów kodu źródłowego, które poprzedzają. Takie komentarze to jednowierszowe komentarze, które zaczynają się od trzech`///`ukośników () lub rozdzielane komentarze, które zaczynają się od ukośnika i dwóch gwiazdek (`/**`). Muszą one bezpośrednio poprzedzać typ zdefiniowany przez użytkownika (na przykład Klasa, delegat lub interfejs) lub element członkowski (na przykład pole, zdarzenie, właściwość lub metoda), które mają do nich adnotacje. Sekcje atrybutów ([Specyfikacja atrybutów](attributes.md#attribute-specification)) są uważane za część deklaracji, dlatego Komentarze do dokumentacji muszą poprzedzać atrybuty zastosowane do typu lub składowej.

__Obowiązuje__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

W *single_line_doc_comment*, jeśli `///` występuje znak *odstępu* , po znakach na każdym *single_line_doc_comment*s przylegającym do bieżącej single_line_doc_comment, to  *znak odstępu* nie jest uwzględniony w danych wyjściowych XML.

W komentarzu rozdzielanym doc, jeśli pierwszy znak, który nie jest odstępem w drugim wierszu, jest gwiazdką i ten sam wzorzec opcjonalnych znaków odstępu i znak gwiazdki jest powtarzany na początku każdego wiersza w komentarzu rozdzielanym doc. znaki Powtórzonego wzorca nie są uwzględniane w danych wyjściowych XML. Wzorzec może zawierać znaki odstępu po, jak również przed znakiem gwiazdki.

__Przykład:__

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

Tekst w komentarzach dokumentacji musi być poprawnie sformułowany zgodnie z regułami XML (https://www.w3.org/TR/REC-xml). Jeśli kod XML jest źle sformułowany, generowane jest ostrzeżenie, a plik dokumentacji będzie zawierał komentarz informujący o wystąpieniu błędu.

Chociaż deweloperzy mogą tworzyć własne zestawy tagów, zalecany zestaw jest zdefiniowany w [zalecane Tagi](documentation-comments.md#recommended-tags). Niektóre z zalecanych tagów mają specjalne znaczenie:

*  `<param>` Tag jest używany do opisywania parametrów. Jeśli jest używany ten tag, generator dokumentacji musi sprawdzić, czy określony parametr istnieje i czy wszystkie parametry zostały opisane w komentarzach dokumentacji. Jeśli taka weryfikacja nie powiedzie się, generator dokumentacji wygeneruje ostrzeżenie.
*  Ten `cref` atrybut może być dołączany do dowolnego tagu w celu zapewnienia odwołania do elementu kodu. Generator dokumentacji musi sprawdzić, czy ten element kodu istnieje. Jeśli weryfikacja nie powiedzie się, generator dokumentacji wygeneruje ostrzeżenie. Podczas wyszukiwania nazwy opisanej w `cref` atrybucie generator dokumentacji musi uwzględniać widoczność przestrzeni nazw zgodnie z `using` instrukcjami wyświetlanymi w kodzie źródłowym. Dla elementów kodu, które są ogólne, nie można użyć normalnej składni ogólnej (is`List<T>`""), ponieważ generuje ona nieprawidłowy kod XML. Nawiasy klamrowe mogą być używane zamiast nawiasów kwadratowych (czyli`List{T}`"") lub składni ucieczki XML (czyli`List&lt;T&gt;`"").
*  `<summary>` Tag jest przeznaczony do użycia przez Podgląd dokumentacji, aby wyświetlić dodatkowe informacje na temat typu lub elementu członkowskiego.
*  `<include>` Tag zawiera informacje z zewnętrznego pliku XML.

Należy uważnie pamiętać, że plik dokumentacji nie zawiera pełnych informacji o typie i elementach członkowskich (na przykład nie zawiera żadnych informacji o typie). Aby uzyskać takie informacje dotyczące typu lub elementu członkowskiego, plik dokumentacji musi być używany w połączeniu z odbiciem w rzeczywistym typie lub elemencie członkowskim.

## <a name="recommended-tags"></a>Zalecane Tagi

Generator dokumentacji musi akceptować i przetwarzać każdy tag, który jest prawidłowy zgodnie z regułami XML. Poniższe Tagi zapewniają powszechnie używane funkcje w dokumentacji użytkownika. (Oczywiście są możliwe inne Tagi).


| __Seryjn__          | __Paragraf__                                            | __Cel__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | Ustaw tekst w czcionce podobnej do kodu                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | Ustaw jeden lub więcej wierszy kodu źródłowego lub danych wyjściowych programu |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | Wskaż przykład                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | Identyfikuje wyjątki, które może zgłosić Metoda           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | Zawiera XML z pliku zewnętrznego                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | Tworzenie listy lub tabeli                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | Zezwól na Dodawanie struktury do tekstu                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | Opisz parametr dla metody lub konstruktora       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | Określ, że słowo jest nazwą parametru               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | Udokumentowanie dostępności zabezpieczeń elementu członkowskiego        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | Opisz dodatkowe informacje o typie           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | Opisywanie wartości zwracanej przez metodę                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | Określ link                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | Generowanie wpisu Zobacz również                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | Opisz typ lub element członkowski typu                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | Opisz Właściwość                                    |
| `<typeparam>`    |                                                        | Opisywanie parametru typu ogólnego                      |
| `<typeparamref>` |                                                        | Określ, że słowo jest nazwą parametru typu          |

### `<c>`

Ten tag udostępnia mechanizm wskazujący, że fragment tekstu w opisie powinien być ustawiony w specjalnej czcionce, takiej jak używany dla bloku kodu. W przypadku wierszy rzeczywistego kodu Użyj `<code>` ([`<code>`](documentation-comments.md#code)).

__Obowiązuje__

```xml
<c>text</c>
```

__Przykład:__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

Ten tag służy do ustawiania co najmniej jednego wiersza kodu źródłowego lub danych wyjściowych programu w specjalnej czcionce. W przypadku małych fragmentów kodu w opisach[`<c>`](documentation-comments.md#c)Użyj `<c>` ().

__Obowiązuje__

```xml
<code>source code or program output</code>
```

__Przykład:__

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

Ten tag umożliwia przykładowy kod w komentarzu, aby określić sposób użycia metody lub innego elementu członkowskiego biblioteki. Zwykle dotyczy to również użycia znacznika `<code>` ([`<code>`](documentation-comments.md#code)).

__Obowiązuje__

```xml
<example>description</example>
```

__Przykład:__

Zobacz `<code>` [(`<code>`](documentation-comments.md#code)), aby zapoznać się z przykładem.

### `<exception>`

Ten tag umożliwia udokumentowanie wyjątków, które Metoda może zgłosić.

__Obowiązuje__

```xml
<exception cref="member">description</exception>
```

gdzie

* `member`jest nazwą elementu członkowskiego. Generator dokumentacji sprawdza, czy dany element członkowski istnieje i tłumaczy `member` na nazwę elementu kanonicznego w pliku dokumentacji.
* `description`to opis okoliczności, w których wyjątek jest zgłaszany.

__Przykład:__

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

Ten tag umożliwia dołączenie informacji z dokumentu XML, który jest zewnętrzny względem pliku kodu źródłowego. Plik zewnętrzny musi być poprawnie sformułowanym dokumentem XML i wyrażenie XPath jest stosowane do tego dokumentu, aby określić, jakie dane XML z tego dokumentu mają być uwzględniane. `<include>` Znacznik jest następnie zastępowany wybranym XML z dokumentu zewnętrznego.

__Obowiązuje__

```
<include file="filename" path="xpath" />
```

gdzie

* `filename`jest nazwą pliku zewnętrznego pliku XML. Nazwa pliku jest interpretowana względem pliku, który zawiera tag include.
* `xpath`jest wyrażeniem XPath, które wybiera część XML w zewnętrznym pliku XML.

__Przykład:__

Jeśli kod źródłowy zawiera deklarację taką jak:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

a plik zewnętrzny "docs. xml" ma następującą zawartość:

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

następnie taka sama dokumentacja jest wyjściowa, jakby zawierała kod źródłowy:

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

Ten tag służy do tworzenia listy lub tabeli elementów. Może zawierać `<listheader>` blok, aby zdefiniować wiersz nagłówka tabeli lub listy definicji. (Podczas definiowania tabeli należy podać tylko wpis `term` w nagłówku).

Każdy element na liście jest określony za pomocą `<item>` bloku. Podczas tworzenia listy definicji należy określić obie `term` i `description` . Jednak dla tabeli, listy punktowanej lub listy numerowanej należy określić tylko `description` wartość.

__Obowiązuje__

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

gdzie

* `term`jest terminem do zdefiniowania, którego definicja znajduje `description`się w.
* `description`jest elementem w postaci listy punktowanej lub numerowanej lub definicji `term`.

__Przykład:__

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

Ten tag jest używany wewnątrz innych tagów, takich `<summary>` jak ([`<remarks>`](documentation-comments.md#remarks)) lub `<returns>` ([`<returns>`](documentation-comments.md#returns)), i umożliwia dodanie struktury do tekstu.

__Obowiązuje__

```xml
<para>content</para>
```

gdzie `content` jest tekstem akapitu.

__Przykład:__

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

Ten tag służy do opisywania parametru dla metody, konstruktora lub indeksatora.

__Obowiązuje__

```xml
<param name="name">description</param>
```

gdzie

* `name`jest nazwą parametru.
* `description`to opis parametru.

__Przykład:__

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

Ten tag służy do wskazywania, że słowo jest parametrem. Plik dokumentacji można przetworzyć w taki sposób, aby sformatować ten parametr w różny sposób.

__Obowiązuje__

```xml
<paramref name="name"/>
```

gdzie `name` to nazwa parametru.

__Przykład:__

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

Ten tag pozwala na udokumentowanie dostępności zabezpieczeń elementu członkowskiego.

__Obowiązuje__

```xml
<permission cref="member">description</permission>
```

gdzie

* `member`jest nazwą elementu członkowskiego. Generator dokumentacji sprawdza, czy dany element kodu istnieje i tłumaczy *składową* na nazwę elementu kanonicznego w pliku dokumentacji.
* `description`to opis dostępu do elementu członkowskiego.

__Przykład:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

Ten tag służy do określania dodatkowych informacji o typie. (USE `<summary>` ([`<summary>`](documentation-comments.md#summary)) do opisywania samego typu i elementów członkowskich typu).

__Obowiązuje__

```xml
<remarks>description</remarks>
```

gdzie `description` jest tekstem uwagi.

__Przykład:__

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

Ten tag jest używany do opisywania wartości zwracanej przez metodę.

__Obowiązuje__

```xml
<returns>description</returns>
```

gdzie `description` jest opisem wartości zwracanej.

__Przykład:__

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

Ten tag umożliwia określenie linku w tekście. Użyj `<seealso>` [(`<seealso>`](documentation-comments.md#seealso)), aby wskazać tekst, który ma być wyświetlany w sekcji Zobacz też.

__Obowiązuje__

```xml
<see cref="member"/>
```

gdzie `member` jest nazwą elementu członkowskiego. Generator dokumentacji sprawdza, czy dany element kodu istnieje i zmienia element *członkowski* na nazwę elementu w wygenerowanym pliku dokumentacji.

__Przykład:__

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

Ten tag umożliwia wygenerowanie wpisu dla sekcji Zobacz też. Użyj `<see>` [(`<see>`](documentation-comments.md#see)), aby określić łącze z tekstu.

__Obowiązuje__

```xml
<seealso cref="member"/>
```

gdzie `member` jest nazwą elementu członkowskiego. Generator dokumentacji sprawdza, czy dany element kodu istnieje i zmienia element *członkowski* na nazwę elementu w wygenerowanym pliku dokumentacji.

__Przykład:__

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

Ten tag może służyć do opisywania typu lub elementu członkowskiego typu. Użyj `<remarks>` [(`<remarks>`](documentation-comments.md#remarks)), aby opisać sam typ.

__Obowiązuje__

```xml
<summary>description</summary>
```

gdzie `description` to podsumowanie typu lub elementu członkowskiego.

__Przykład:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

Ten tag pozwala na opis właściwości.

__Obowiązuje__

```xml
<value>property description</value>
```

gdzie `property description` jest opisem właściwości.

__Przykład:__

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

Ten tag służy do opisywania parametru typu ogólnego dla klasy, struktury, interfejsu, delegata lub metody.

__Obowiązuje__

```xml
<typeparam name="name">description</typeparam>
```

gdzie `name` jest nazwą parametru typu i `description` jest jego opis.

__Przykład:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

Ten tag służy do wskazywania, że słowo jest parametrem typu. Plik dokumentacji można przetworzyć w taki sposób, aby sformatować ten parametr typu w dowolny sposób.

__Obowiązuje__

```xml
<typeparamref name="name"/>
```

gdzie `name` jest nazwą parametru typu.

__Przykład:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>Przetwarzanie pliku dokumentacji

Generator dokumentacji generuje ciąg identyfikatora dla każdego elementu w kodzie źródłowym, który jest oznaczony za pomocą komentarza do dokumentacji. Ten ciąg identyfikatora jednoznacznie identyfikuje element źródłowy. Podgląd dokumentacji może używać ciągu identyfikatora do identyfikowania odpowiednich metadanych/elementu odbicia, do którego odnosi się dokumentacja.

Plik dokumentacji nie jest hierarchiczną reprezentacją kodu źródłowego; Zamiast tego jest to płaska lista z wygenerowanym ciągiem identyfikatora dla każdego elementu.

### <a name="id-string-format"></a>Format ciągu identyfikatora

Generator dokumentacji obserwuje następujące reguły podczas generowania ciągów identyfikatorów:

*  Brak białego znaku w ciągu.

*  Pierwsza część ciągu określa rodzaj składowej udokumentowanej za pośrednictwem pojedynczego znaku, po którym następuje dwukropek. Zdefiniowane są następujące rodzaje elementów członkowskich:

   | __Opis__ | __Opis__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | Zdarzenie                                                       |
   | F             | Pole                                                       |
   | M             | Metoda (w tym konstruktory, destruktory i operatory) |
   | N             | Przestrzeń nazw                                                   |
   | P             | Właściwość (w tym indeksatory)                               |
   | T             | Typ (taki jak Klasa, delegat, enum, Interface i struct) |
   | !             | Ciąg błędu; pozostała część ciągu zawiera informacje o błędzie. Na przykład generator dokumentacji generuje informacje o błędach dla łączy, których nie można rozpoznać. |

*  Druga część ciągu jest w pełni kwalifikowana nazwa elementu, rozpoczynając od elementu głównego przestrzeni nazw. Nazwa elementu, jego typy otaczające i przestrzeń nazw są oddzielone kropkami. Jeśli nazwa samego elementu ma okresy, są one zastępowane `#(U+0023)` znakami. (Zakłada się, że żaden element nie ma tego znaku w nazwie).
*  W przypadku metod i właściwości z argumentami lista argumentów następuje w nawiasach. W przypadku tych bez argumentów nawiasy są pomijane. Argumenty są rozdzielone przecinkami. Kodowanie każdego argumentu jest takie samo jak sygnatura interfejsu wiersza polecenia w następujący sposób:
   *  Argumenty są reprezentowane przez nazwę dokumentacji, która jest oparta na ich w pełni kwalifikowanej nazwie, modyfikowane w następujący sposób:
      * Argumenty reprezentujące typy ogólne mają dołączony `` ` `` znak (symbol wieloznaczny), po którym następuje liczba parametrów typu
      * Argumenty mające `out` modyfikator `ref` or mają `@` następującą nazwę typu. Argumenty przekazane przez wartość lub za `params` pośrednictwem nie mają specjalnej notacji.
      * Argumenty, które są tablicami, `[lowerbound:size, ... , lowerbound:size]` są reprezentowane, gdy liczba przecinki jest rzędu mniejszym od 1, a dolne granice i rozmiar każdego wymiaru, jeśli są znane, są reprezentowane w postaci dziesiętnej. Jeśli Dolna granica nie zostanie określona, zostanie pominięta. Jeśli Dolna granica i rozmiar określonego wymiaru zostaną pominięte, `:` również zostanie pominięty. Tablice nieregularne są reprezentowane przez jeden `[]` na poziom.
      * Argumenty, które mają typy wskaźnika inne niż void, są reprezentowane `*` przy użyciu następującej nazwy typu. Wskaźnik void jest reprezentowany przy użyciu nazwy `System.Void`typu.
      * Argumenty odwołujące się do parametrów typu ogólnego zdefiniowane w typach są kodowane `` ` `` przy użyciu znaku (znacznika kreskowego), po którym następuje indeks (liczony od zera) parametru typu.
      * Argumenty, które używają parametrów typu ogólnego zdefiniowane w metodach, używają podwójnego ``` `` ``` taktu zamiast `` ` `` użycia dla typów.
      * Argumenty odwołujące się do skonstruowanych typów ogólnych są kodowane przy użyciu typu ogólnego `{`, po którym następuje rozdzielana przecinkami lista argumentów typu, `}`po których następuje.

### <a name="id-string-examples"></a>Przykłady identyfikatora ciągu

Poniższe przykłady pokazują fragment C# kodu wraz z ciągiem identyfikatora utworzonym z każdego elementu źródłowego, który może mieć komentarz dokumentacji:

*  Typy są reprezentowane przy użyciu ich w pełni kwalifikowanej nazwy rozszerzonej o informacje ogólne:

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

*  Pola są reprezentowane przez ich w pełni kwalifikowaną nazwę:

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

*  Konstruktor.

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

*  Destruktory.

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

*  Form.

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

*  Właściwości i indeksatory.

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

*  Wydarzeniach.

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

*  Operatory jednoargumentowe.

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

   Pełny zestaw nazw funkcji operatora jednoargumentowego jest następujący `op_UnaryPlus`:, `op_UnaryNegation` `op_OnesComplement` `op_Increment` `op_LogicalNot` `op_Decrement` `op_False`,,,,, ,i.`op_True`

*  Operatory binarne.

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

   Pełny zestaw nazw funkcji operatora binarnego jest następujący: `op_Addition`, `op_BitwiseOr` `op_ExclusiveOr` `op_BitwiseAnd` `op_Subtraction`, `op_Modulus` `op_Division` `op_Multiply`,,,, `op_LeftShift` ,`op_RightShift`,,, `op_Equality`, `op_Inequality`, ,,`op_LessThan`i .`op_GreaterThanOrEqual` `op_LessThanOrEqual` `op_GreaterThan`

*  Operatory konwersji mają znak końcowy "`~`", po którym następuje zwracany typ.

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

## <a name="an-example"></a>Przykład

### <a name="c-source-code"></a>C#kod źródłowy

Poniższy przykład pokazuje kod `Point` źródłowy klasy:

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

### <a name="resulting-xml"></a>Wyniki XML

Poniżej przedstawiono dane wyjściowe generowane przez jeden generator dokumentacji, gdy podano kod źródłowy klasy `Point`, pokazany powyżej:

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
