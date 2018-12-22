# <a name="documentation-comments"></a>Komentarze dokumentacji

C# zawiera mechanizmu dla programistów udokumentować kod przy użyciu specjalnej składni komentarza zawierający tekst XML. W plikach kodu źródłowego komentarze o niektórych formularza może służyć do kierowania narzędzia do tworzenia XML, mimo że takie komentarze i elementy kodu źródłowego, które mogą poprzedzać. Komentarze przy użyciu składni takie są nazywane ***komentarzy dokumentacji***. Musi bezpośrednio poprzedzać typu zdefiniowanego przez użytkownika (takie jak klasy, delegata lub interfejsu) lub elementu członkowskiego (na przykład pola, zdarzenia, właściwość lub metoda). Narzędzie do generowania XML nosi nazwę ***generator dokumentacji***. (Tego generatora może być, ale nie muszą być, kompilator języka C#, sam). Dane wyjściowe wytwarzane przez generator dokumentacji jest nazywany ***soubor dokumentace***. Plik dokumentacji jest używany jako dane wejściowe ***podglądu dokumentacji***; narzędzie przeznaczone do produkcji jakieś wizualizacji do wyświetlenia informacji o typie i jego skojarzone dokumentacji.

Tej specyfikacji sugeruje zestaw znaczników, które ma być używany w komentarzach dokumentacji, ale korzystanie z tych tagów nie jest wymagane i inne tagi mogą być używane w razie potrzeby, jak długo reguły poprawnie sformułowany dokument XML są przestrzegane.

## <a name="introduction"></a>Wprowadzenie

Komentarze o specjalną postać może służyć do kierowania narzędzia do tworzenia XML, mimo że takie komentarze i elementy kodu źródłowego, które mogą poprzedzać. Takie komentarze są Komentarze jednowierszowe, rozpoczynające się z trzema ukośnikami (`///`), lub rozdzielanym komentarzy rozpoczynających się od ukośnika i dwóch gwiazdek (`/**`). Musi bezpośrednio poprzedzać typu zdefiniowanego przez użytkownika (takie jak klasy, delegata lub interfejsu) lub element członkowski (na przykład pola, zdarzenia, właściwość lub metoda), który mogą dodawać adnotacje. Atrybut sekcje ([Specyfikacja atrybutu](attributes.md#attribute-specification)) są traktowane jako część deklaracji, dzięki czemu komentarzy do dokumentacji musi poprzedzać atrybuty stosowane do typu lub elementu członkowskiego.

__Składnia:__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

W *single_line_doc_comment*, jeśli istnieje *odstępu* następujący znak `///` znaków na każdym *single_line_doc_comment*s sąsiadująco do bieżącego *single_line_doc_comment*, który następnie *odstępu* znak nie jest uwzględniony w danych wyjściowych XML.

Rozdzielany doc — komentarza Jeśli pierwszy znak niebędący odstępem w drugim wierszu jest znak gwiazdki i tym samym wzorcem, opcjonalny odstęp i znaku gwiazdki jest powtarzany na początku każdego wiersza w rozdzielonych doc — komentarz, następnie znaki powtarzanych wzorca nie są uwzględnione w danych wyjściowych XML. Wzorzec może zawierać białych znaków, po, a także przed znakiem gwiazdki.

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

Tekst w ramach komentarzy do dokumentacji musi być poprawnie sformułowany zgodnie z regułami XML (http://www.w3.org/TR/REC-xml). Jeśli kod XML jest ill sformułowany, generowane jest ostrzeżenie, a plik dokumentacja będzie zawierać komentarz informujący o tym, że wystąpił błąd podczas.

Mimo że Deweloperzy są bezpłatne tworzenie własnych zestawów tagów, zalecany zestaw jest zdefiniowany w [zalecane tagi](documentation-comments.md#recommended-tags). Niektóre zalecane tagi mają specjalne znaczenie:

*  `<param>` Tag jest używany do opisania parametrów. Jeśli takie tag jest używany, generator dokumentacji musi sprawdzić, czy określony parametr istnieje i czy wszystkie parametry są opisane w komentarzach dokumentacji. Jeśli taka weryfikacja zakończy się niepowodzeniem, generator dokumentacji generuje ostrzeżenie.
*  `cref` Atrybutu mogą być dołączane do każdego znacznika, aby zapewnić odwołanie do elementu kodu. Generator dokumentacji należy sprawdzić, czy ten element kodu istnieje. Jeśli weryfikacja zakończy się niepowodzeniem, generator dokumentacji generuje ostrzeżenie. Podczas wyszukiwania dla nazwy opisanego w `cref` atrybutu, generator dokumentacji muszą przestrzegać widoczność przestrzeni nazw zgodnie z opisem w `using` instrukcji w kodzie źródłowym. Dla elementów kodu znajdujące ogólnego normalne ogólna składnia (ie "`List<T>`") nie można użyć, ponieważ generuje nieprawidłowy kod XML. Nawiasy klamrowe można używać zamiast nawiasy kwadratowe (ie "`List{T}`"), lub można używać składni XML ucieczki (ie "`List&lt;T&gt;`").
*  `<summary>` Tag jest przeznaczona do użycia przez Podgląd dokumentacji, aby wyświetlić dodatkowe informacje na temat typu lub elementu członkowskiego.
*  `<include>` Tag zawierały informacje z zewnętrznego pliku XML.

Należy dokładnie, czy plik dokumentacji nie zapewnia pełne informacje na temat typów i elementów członkowskich (na przykład, go nie zawiera żadnych informacji o typie). Aby uzyskać informacje dotyczące typu lub elementu członkowskiego, konieczne jest użycie pliku dokumentacji, w połączeniu z odbiciem na rzeczywisty typ lub element członkowski.

## <a name="recommended-tags"></a>Zalecane tagi

Generator dokumentacji musi przyjmował i przetwarzał dowolny tag, który jest prawidłowy, zgodnie z regułami XML. Następujące znaczniki oferuje powszechnie używanych w dokumentacji użytkownika. (Oczywiście innych tagów są możliwe).


| __Tag__          | __Sekcja__                                            | __Cel__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | Ustaw tekst czcionką podobny kod                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | Ustaw jeden lub więcej wierszy źródła kodu lub dane wyjściowe programu |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | Przykład wskazać                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | Określa metodę może zgłosić wyjątek           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | Obejmuje XML z pliku zewnętrznego                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | Tworzenie listy lub tabeli                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | Zezwala na strukturę, które mają zostać dodane do tekstu                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | Opis parametru dla metody lub konstruktora       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | Zidentyfikować wyrazem nazwę parametru               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | Dokument ułatwień dostępu zabezpieczeń elementu członkowskiego        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | Przedstawiono dodatkowe informacje o typie           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | Opisz wartość zwracaną przez metodę                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | Podaj łącze                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | Generuj wpis Zobacz też                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | Opis typu lub składowej typu                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | Opis właściwości                                    |
| `<typeparam>`    |                                                        | Opis parametru typu ogólnego                      |
| `<typeparamref>` |                                                        | Zidentyfikować wyrazem nazwie parametru typu          |

### `<c>`

Ten tag udostępnia mechanizm do wskazania, że fragment tekstu w obrębie opis powinna zostać ustawiona w specjalne takim dla bloku kodu. Linie rzeczywisty kod, można użyć `<code>` ([`<code>`](documentation-comments.md#code)).

__Składnia:__

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

Ten tag jest używany, aby ustawić jeden lub więcej wierszy źródła kodu lub dane wyjściowe programu w niektórych specjalne. Mały kod fragmentów w narracja, można użyć `<c>` ([`<c>`](documentation-comments.md#c)).

__Składnia:__

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

Ten tag umożliwia przykładowy kod w komentarzu, aby określić, jak można użyć metody lub innego członka biblioteki. Zazwyczaj także wymagałoby to użycia tagu `<code>` ([`<code>`](documentation-comments.md#code)) również.

__Składnia:__

```xml
<example>description</example>
```

__Przykład:__

Zobacz `<code>` ([`<code>`](documentation-comments.md#code)) przykład.

### `<exception>`

Ten tag umożliwia dokumentowanie metody może zgłosić wyjątek.

__Składnia:__

```xml
<exception cref="member">description</exception>
```

gdzie

* `member` jest nazwą elementu członkowskiego. Generator dokumentacji sprawdza, czy dany element istnieje i czy tłumaczy `member` do nazwy kanonicznej elementu w pliku dokumentacji.
* `description` znajduje się opis sytuacji, w których jest wyjątek.

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

Ten tag umożliwia, łącznie z informacjami z dokumentu XML, który jest zewnętrzne w stosunku do pliku kodu źródłowego. Zewnętrzny plik musi być poprawnie sformułowany dokument XML, a wyrażenie XPath jest stosowany do tego dokumentu, aby określić, jakie XML z tego dokumentu do uwzględnienia. `<include>` Tag następnie zastępowane wybrane XML z dokumentu zewnętrznego.

__Składnia:__

```
<include file="filename" path="xpath" />
```

gdzie

* `filename` jest nazwą pliku z zewnętrznego pliku XML. Nazwa pliku jest interpretowane względem pliku który zawiera tagu include.
* `xpath` to wyrażenie XPath wybierające niektóre z pliku XML z zewnętrznego pliku XML.

__Przykład:__

Jeśli kod źródłowy zawiera deklarację, takich jak:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

i zewnętrznego pliku "docs.xml" miało następującą zawartość:

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

następnie ten sam dokumentacji przedstawiono dane wyjściowe, tak, jakby zawarte w kodzie źródłowym:

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

Ten tag jest używany do tworzenia listy lub tabela elementów. Może on zawierać `<listheader>` bloku, aby zdefiniować wiersz nagłówka tabeli lub definicji listy. (Podczas definiowania tabeli, tylko wpis dla `term` w nagłówku muszą być dostarczone.)

Każdy element na liście jest określony za pomocą `<item>` bloku. Podczas tworzenia listy definicji zarówno `term` i `description` musi być określona. Jednak dla tabeli, listy punktowanej lub listę numerowaną, tylko `description` muszą być określone.

__Składnia:__

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

* `term` to wyrażenie, aby zdefiniować, którego definicja jest `description`.
* `description` Element punktora lub Lista numerowana lub definicji `term`.

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

Ten tag jest przeznaczona do użytku wewnątrz innych tagów, takich jak `<summary>` ([`<remark>`](documentation-comments.md#remark)) lub `<returns>` ([`<returns>`](documentation-comments.md#returns)) i pozwala na strukturę, które mają zostać dodane do tekstu.

__Składnia:__

```xml
<para>content</para>
```

gdzie `content` jest tekst akapitu.

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

Ten tag jest używany do opisania parametrów dla metody, Konstruktor lub indeksatora.

__Składnia:__

```xml
<param name="name">description</param>
```

gdzie

* `name` jest nazwą parametru.
* `description` znajduje się opis parametru.

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

Ten tag jest używany do wskazania, że wyraz jest parametrem. Plik dokumentacji mogą być przetwarzane, aby sformatować ten parametr w jakiś sposób distinct.

__Składnia:__

```xml
<paramref name="name"/>
```

gdzie `name` jest nazwą parametru.

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

Ten tag umożliwia ułatwień dostępu zabezpieczeń elementu członkowskiego do udokumentowania.

__Składnia:__

```xml
<permission cref="member">description</permission>
```

gdzie

* `member` jest nazwą elementu członkowskiego. Generator dokumentacji sprawdza, czy dany element kodu istnieje i wykonuje translację *elementu członkowskiego* do nazwy kanonicznej elementu w pliku dokumentacji.
* `description` znajduje się opis dostępu do elementu członkowskiego.

__Przykład:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

Ten tag jest używany do określenia dodatkowych informacji o typie. (Użyj `<summary>` ([`<summary>`](documentation-comments.md#summary)) do opisywania samego typu i elementy członkowskie typu.)

__Składnia:__

```xml
<remark>description</remark>
```

gdzie `description` jest tekst uwagi.

__Przykład:__

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

Ten tag jest używany do opisania wartość zwracaną przez metodę.

__Składnia:__

```xml
<returns>description</returns>
```

gdzie `description` znajduje się opis wartość zwracaną.

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

Ten tag umożliwia łącza, należy określić w tekście. Użyj `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) do wskazania tekst, który jest wyświetlany w sekcji Zobacz też.

__Składnia:__

```xml
<see cref="member"/>
```

gdzie `member` jest nazwa elementu członkowskiego. Generator dokumentacji sprawdza, czy dany element kodu istnieje i zmienia *elementu członkowskiego* do nazwy elementu w pliku wygenerowaną dokumentację.

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

Ten tag umożliwia wpis do wygenerowania dla sekcji Zobacz też. Użyj `<see>` ([`<see>`](documentation-comments.md#see)) do określenia łącze między w tekście.

__Składnia:__

```xml
<seealso cref="member"/>
```

gdzie `member` jest nazwa elementu członkowskiego. Generator dokumentacji sprawdza, czy dany element kodu istnieje i zmienia *elementu członkowskiego* do nazwy elementu w pliku wygenerowaną dokumentację.

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

Ten tag może służyć do opisu typu lub składowej typu. Użyj `<remark>` ([`<remark>`](documentation-comments.md#remark)) do opisywania samego typu.

__Składnia:__

```xml
<summary>description</summary>
```

gdzie `description` znajduje się podsumowanie typu lub elementu członkowskiego.

__Przykład:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

Ten tag umożliwia właściwość, która ma być opisane.

__Składnia:__

```xml
<value>property description</value>
```

gdzie `property description` znajduje się opis właściwości.

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

Ten tag jest używany do opisania parametr typu ogólnego dla klasy, struktury, interfejsu, delegata lub metody.

__Składnia:__

```xml
<typeparam name="name">description</typeparam>
```

gdzie `name` to nazwa parametru typu i `description` jest jego opis.

__Przykład:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

Ten tag jest używany do wskazania, że wyraz jest parametrem typu. Plik dokumentacji mogą być przetwarzane, aby sformatować ten parametr typu w jakiś sposób distinct.

__Składnia:__

```xml
<typeparamref name="name"/>
```

gdzie `name` jest nazwa parametru typu.

__Przykład:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>Przetwarzanie pliku dokumentacji

Generator dokumentacji generuje ciąg Identyfikatora dla każdego elementu w kodzie źródłowym, który jest oznaczony za pomocą komentarzy dokumentacji. Ciąg ten identyfikator unikatowo identyfikuje element źródła. Podgląd dokumentacja umożliwia zidentyfikować odpowiedni element metadanych/odbicia, do której stosują się dokumentacji ciąg Identyfikatora.

Plik dokumentacji nie jest hierarchiczną reprezentację kodu źródłowego; jest to raczej płaskiej listy z ciągiem Identyfikatora wygenerowany dla każdego elementu.

### <a name="id-string-format"></a>Format ciągu Identyfikatora

Podczas generowania ciągi identyfikatorów, generator dokumentacji przestrzega następujące reguły:

*  Biały znak nie zostanie umieszczony w ciągu.

*  Pierwsza część ciągu określa rodzaj elementu członkowskiego udokumentowane, za pośrednictwem pojedynczy znak z dwukropkiem. Zdefiniowane są następujące rodzaje składowych:

   | __Znak__ | __Opis__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | Zdarzenie                                                       |
   | F             | Pole                                                       |
   | M             | Metody (w tym konstruktory, destruktory i operatorów) |
   | N             | Przestrzeń nazw                                                   |
   | P             | Właściwości (łącznie z indeksatorów)                               |
   | T             | Wpisz (na przykład klasa, delegowanego, wyliczenia, interfejsu i struktury) |
   | !             | Ciąg błędu; Pozostała część ciągu zawiera informacje o tym błędzie. Na przykład generator dokumentacji generuje informacje o błędzie dla łączy, których nie można rozpoznać. |

*  Druga część ciągu jest w pełni kwalifikowana nazwa elementu, począwszy od głównego obszaru nazw. Nazwa elementu, jego otaczającego typów i przestrzeni nazw są oddzielone kropkami. Jeśli nazwa elementu zawiera kropek, są zastępowane przez `#(U+0023)` znaków. (Zakłada się, że element nie ma tego znaku w jego nazwę.)
*  Dla metod i właściwości z argumentami poniżej listy argumentów, ujęte w nawiasy. Dla osób, bez argumentów nawiasy są pomijane. Argumenty są oddzielone przecinkami. Kodowanie każdy argument jest taka sama jak sygnatury interfejsu wiersza polecenia w następujący sposób:
   *  Argumenty są reprezentowane przez ich nazwy dokumentacji, która opiera się na ich w pełni kwalifikowaną nazwę, zmodyfikowana w następujący sposób:
      * Argumenty, które reprezentują typy rodzajowe mają dołączonych `` ` `` znak (początkowych), a następnie liczbę parametrów typu
      * Argumentów mających `out` lub `ref` modyfikator musi `@` zgodnie z ich nazwy typu. Argumenty przekazywane przez wartość lub za pośrednictwem `params` mają nie specjalne notacji.
      * Argumenty, które są tablice są reprezentowane jako `[lowerbound:size, ... , lowerbound:size]` gdzie liczba przecinków jest rangi minus jeden, a dolne granice i rozmiaru każdego wymiaru, jeśli jest znany, są reprezentowane w zapisie dziesiętnym. Jeśli dolna granica lub rozmiar nie zostanie określony, zostanie pominięty. W przypadku pominięcia dolną granicę i rozmiar w konkretnym wymiarze `:` pominięto w także. Tablice nieregularne są reprezentowane przez jedną `[]` na poziomie.
      * Argumenty, które mają typ wskaźnika innego niż void są reprezentowane przy użyciu `*` po nazwie typu. Pusty wskaźnik jest reprezentowane za pomocą nazwę typu `System.Void`.
      * Argumenty, które odwołują się do parametrów typu genetycznego zdefiniowany dla typów są zakodowane przy użyciu `` ` `` znak (początkowych) następuje liczony od zera indeks parametru typu.
      * Argumenty, które używać parametrów typu ogólnego, zdefiniowane w metodach używać początkowych double ``` `` ``` zamiast `` ` `` używane dla typów.
      * Argumenty, które odnoszą się do typów ogólnych stworzonego elementu są zakodowane przy użyciu typu ogólnego, a następnie `{`, następuje rozdzielana przecinkami lista argumentów typu, a następnie `}`.

### <a name="id-string-examples"></a>Identyfikator ciągu przykłady

W poniższych przykładach każdego pokazano fragment kodu języka C# wraz z ciągiem Identyfikatora wyprodukowanych z każdego elementu źródłowego w stanie konieczności komentarza do dokumentacji:

*  Typy są reprezentowane w pełni kwalifikowaną nazwę, rozszerzone informacje ogólne:

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

*  Pola są reprezentowane przez ich w pełni kwalifikowana nazwa:

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

*  Konstruktory.

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

*  Metody.

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

*  Właściwości i indeksatorów.

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

*  zdarzenia.

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

   Kompletny zestaw nazw funkcji operator jednoargumentowy używana jest następująca: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, i `op_False`.

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

   Kompletny zestaw nazw funkcji operatora binarnego jest następująca: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, i `op_GreaterThanOrEqual`.

*  Operatory konwersji ma końcowe "`~`" następuje zwracanego typu.

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

### <a name="c-source-code"></a>Kod źródłowy języka C#

Poniższy przykład pokazuje kod źródłowy `Point` klasy:

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

### <a name="resulting-xml"></a>Wynikowy kod XML

Oto dane wyjściowe generowane przez jeden generator dokumentacji, gdy kod źródłowy dla klasy `Point`, jak pokazano powyżej:

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
