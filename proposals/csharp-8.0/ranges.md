---
ms.openlocfilehash: 50f2bd2d0a84064cfe35fe65b9e5c59c052d19ac
ms.sourcegitcommit: 1dbb8e82bed5012a58a3a035bf2c3737ed570d07
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80122945"
---
# <a name="ranges"></a>Zakresy

## <a name="summary"></a>Podsumowanie

Ta funkcja zawiera dwa nowe operatory, które umożliwiają konstruowanie `System.Index` i `System.Range` obiektów oraz używanie ich do kolekcji indeksów/wycinków w czasie wykonywania.

## <a name="overview"></a>Omówienie

### <a name="well-known-types-and-members"></a>Dobrze znane typy i elementy członkowskie

Aby użyć nowych formularzy składni dla `System.Index` i `System.Range`, nowe dobrze znane typy i elementy członkowskie mogą być niezbędne, w zależności od tego, które formy składni są używane.

Aby użyć operatora "Hat" (`^`), wymagane są następujące elementy

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

Aby użyć `System.Index` typu jako argumentu w dostępie do elementu tablicy, wymagany jest następujący element członkowski:

```csharp
int System.Index.GetOffset(int length);
```

Składnia `..` dla `System.Range` będzie wymagała typu `System.Range`, a także co najmniej jednego z następujących elementów członkowskich:

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

Składnia `..` może być nieobecna dla obu argumentów lub żadnego z nich. Bez względu na liczbę argumentów, Konstruktor `Range` zawsze wystarcza do użycia składni `Range`. Jeśli jednak którykolwiek z innych elementów członkowskich jest obecny i brakuje co najmniej jednego z `..` argumentów, odpowiedni element członkowski może zostać zastąpiony.

Na koniec dla wartości typu `System.Range`, która ma być używana w wyrażeniu dostępu do elementu tablicy, musi być obecny następujący element członkowski:

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a>System. index

C#nie ma możliwości indeksowania kolekcji od końca, ale większość indeksatorów korzysta z koncepcji "from Start" lub "Length-i". Wprowadzimy nowe wyrażenie indeksu oznaczające "od końca". Funkcja wprowadzi nowy operator "Hat" prefiksu jednoargumentowego. Jeden operand musi być konwertowany do `System.Int32`. Zostanie on obniżony do odpowiedniego wywołania metody fabryki `System.Index`.

Rozszerzamy gramatykę dla *unary_expression* przy użyciu następującej formy dodatkowej składni:

```antlr
unary_expression
    : '^' unary_expression
    ;
```

Wywołujemy ten *indeks z operatora końcowego* . Wstępnie zdefiniowany *indeks z operatorów End* jest następujący:

```csharp
    System.Index operator ^(int fromEnd);
```

Zachowanie tego operatora jest zdefiniowane tylko dla wartości wejściowych większej lub równej zero.

Przykłady:

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a>System. Range

C#nie ma składniowego sposobu uzyskiwania dostępu do "zakresów" lub "wycinków" kolekcji. Zwykle użytkownicy są zmuszeni do implementowania złożonych struktur do filtrowania/obsługi wycinków pamięci lub do metod LINQ, takich jak `list.Skip(5).Take(2)`. Po dodaniu `System.Span<T>` i innych podobnych typów, ważne jest, aby ten rodzaj operacji był obsługiwany na wyższym poziomie w języku/czasie wykonywania i mieć ujednolicony interfejs.

Język wprowadzi nowy operator zakresu `x..y`. Jest to binarny operator wrostkowe, który akceptuje dwa wyrażenia. Każdy operand może zostać pominięty (przykłady poniżej) i musi być konwertowany do `System.Index`. Zostanie on obniżony do odpowiedniego wywołania metody fabryki `System.Range`.

Zamieniamy reguły C# gramatyki dla *multiplicative_expression* na następujące (w celu wprowadzenia nowego poziomu pierwszeństwa):

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

Wszystkie formularze *operatora Range* mają takie samo pierwszeństwo. Ta nowa grupa pierwszeństwa jest niższa niż *operatory jednoargumentowe* i większe niż *Operatory arytmetyczne mulitiplicative*.

Wywołujemy operator `..` operatorem *zakresu*. Wbudowany operator zakresu może być w przybliżeniu rozumiany jako odpowiadający wywołaniu wbudowanego operatora tego formularza:

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

Przykłady:

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

Ponadto `System.Index` powinna mieć niejawną konwersję z `System.Int32`, aby nie trzeba było przeciążać do mieszania liczb całkowitych i indeksów przez wielowymiarowe podpisy.

## <a name="adding-index-and-range-support-to-existing-library-types"></a>Dodawanie obsługi indeksu i zakresu do istniejących typów bibliotek

### <a name="implicit-index-support"></a>Niejawna obsługa indeksu

Język zapewni składową indeksatora wystąpienia z pojedynczym parametrem typu `Index` dla typów, które spełniają następujące kryteria:

- Typ jest do zliczenia.
- Typ ma dostępny indeksator wystąpienia, który przyjmuje jako argument pojedynczy `int`.
- Typ nie ma dostępnego indeksatora wystąpienia, który przyjmuje `Index` jako pierwszy parametr. `Index` musi być jedynym parametrem lub pozostałe parametry muszą być opcjonalne.

Typ jest możliwy do ***zliczenia*** , jeśli ma właściwość o nazwie `Length` lub `Count` z dostępną metodę pobierającą i typem zwracanym `int`. Język może korzystać z tej właściwości, aby przekonwertować wyrażenie typu `Index` na `int` w punkcie wyrażenia bez konieczności używania typu `Index` w ogóle. W przypadku obecności obu `Length` i `Count`, `Length` będzie preferowane. W celu uproszczenia, propozycja będzie używać nazwy `Length` do reprezentowania `Count` lub `Length`.

W przypadku takich typów język działa tak, jakby istnieje element członkowski indeksatora w formularzu `T this[Index index]`, gdzie `T` jest typem zwracanym na podstawie `int` indeksatora, w tym wszelkie adnotacje stylów `ref`. Nowy element członkowski będzie miał te same `get` i `set` elementy członkowskie o pasujących ułatwieniach dostępu jako indeksator `int`. 

Nowy indeksator zostanie zaimplementowany przez konwersję argumentu typu `Index` na `int` i emitowanie wywołania do indeksatora opartego na `int`. Na potrzeby dyskusji użyjmy przykładu `receiver[expr]`. Konwersja `expr` do `int` zostanie wykonana w następujący sposób:

- Gdy argument ma postać `^expr2` i typ `expr2` jest `int`, zostanie przetłumaczony na `receiver.Length - expr2`.
- W przeciwnym razie zostanie przetłumaczone jako `expr.GetOffset(receiver.Length)`.

Dzięki temu deweloperzy mogą korzystać z funkcji `Index` na istniejących typach bez potrzeby modyfikacji. Na przykład:

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

Wyrażenia `receiver` i `Length` zostaną rozlane zgodnie z potrzebami, aby upewnić się, że wszystkie efekty uboczne są wykonywane tylko raz. Na przykład:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

Ten kod będzie drukował "Uzyskaj długość 3".

### <a name="implicit-range-support"></a>Niejawna obsługa zakresu

Język zapewni składową indeksatora wystąpienia z pojedynczym parametrem typu `Range` dla typów, które spełniają następujące kryteria:

- Typ jest do zliczenia.
- Typ ma dostępny element członkowski o nazwie `Slice`, który ma dwa parametry typu `int`.
- Typ nie ma indeksatora wystąpienia, który przyjmuje jeden `Range` jako pierwszy parametr. `Range` musi być jedynym parametrem lub pozostałe parametry muszą być opcjonalne.

W przypadku takich typów język zostanie powiązany tak, jakby istnieje element członkowski indeksatora w formularzu `T this[Range range]`, gdzie `T` jest typem zwracanym metody `Slice`, łącznie ze wszystkimi adnotacjami stylów `ref`. Nowy element członkowski będzie miał również pasujące ułatwienia dostępu przy użyciu `Slice`. 

Gdy indeksator oparty na `Range` jest powiązany z wyrażeniem o nazwie `receiver`, zostanie on obniżony przez konwersję wyrażenia `Range` na dwie wartości, które są następnie przenoszone do metody `Slice`. Na potrzeby dyskusji użyjmy przykładu `receiver[expr]`.

Pierwszy argument `Slice` zostanie uzyskany przez konwersję wyrażenia z określonym zakresem w następujący sposób:

- Gdy `expr` ma postać `expr1..expr2` (gdzie `expr2` można pominąć), a `expr1` ma typ `int`, zostanie on wyemitowany jako `expr1`.
- Gdy `expr` ma postać `^expr1..expr2` (gdzie `expr2` można pominąć), zostanie ona wyemitowana jako `receiver.Length - expr1`.
- Gdy `expr` ma postać `..expr2` (gdzie `expr2` można pominąć), zostanie ona wyemitowana jako `0`.
- W przeciwnym razie zostanie on wyemitowany jako `expr.Start.GetOffset(receiver.Length)`.

Ta wartość zostanie ponownie użyta w obliczeniu drugiego argumentu `Slice`. Po wykonaniu tej czynności będzie ona nazywana `start`. Drugi argument `Slice` zostanie uzyskany przez konwersję wyrażenia z określonym zakresem w następujący sposób:

- Gdy `expr` ma postać `expr1..expr2` (gdzie `expr1` można pominąć), a `expr2` ma typ `int`, zostanie on wyemitowany jako `expr2 - start`.
- Gdy `expr` ma postać `expr1..^expr2` (gdzie `expr1` można pominąć), zostanie ona wyemitowana jako `(receiver.Length - expr2) - start`.
- Gdy `expr` ma postać `expr1..` (gdzie `expr1` można pominąć), zostanie ona wyemitowana jako `receiver.Length - start`.
- W przeciwnym razie zostanie on wyemitowany jako `expr.End.GetOffset(receiver.Length) - start`.

Wyrażenia `receiver`, `Length`i `expr` zostaną rozlane zgodnie z potrzebami, aby upewnić się, że wszystkie efekty uboczne są wykonywane tylko raz. Na przykład:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

Ten kod będzie drukował "Uzyskaj długość 2".

Język będzie specjalny przypadku następujących znanych typów: 

- `string`: Metoda `Substring` zostanie użyta zamiast `Slice`.
- `array`: Metoda `System.Reflection.CompilerServices.GetSubArray` zostanie użyta zamiast `Slice`.

## <a name="alternatives"></a>Alternatywy

Nowe operatory (`^` i `..`) są cukrem składni. Funkcje mogą być implementowane przez jawne wywołania do `System.Index` i `System.Range` metody fabryki, ale spowoduje to powstanie dużej ilości kodu standardowego i środowisko będzie nieintuicyjne.

## <a name="il-representation"></a>Reprezentacja IL

Te dwa operatory zostaną obniżone do regularnych wywołań indeksatora/metody bez zmian w kolejnych warstwach kompilatora.

## <a name="runtime-behavior"></a>Zachowanie środowiska uruchomieniowego

- Kompilator może zoptymalizować indeksatory dla wbudowanych typów, takich jak tablice i ciągi, i obniżyć indeksowanie do odpowiednich istniejących metod.
- `System.Index` będzie zgłaszany, jeśli skonstruowane z wartością ujemną.
- `^0` nie zgłasza, ale tłumaczy na długość kolekcji/wyliczalny, do której jest dostarczany.
- `Range.All` jest semantycznie odpowiednikiem `0..^0`i można ją odbudować do tych indeksów.

## <a name="considerations"></a>Zagadnienia do rozważenia

### <a name="detect-indexable-based-on-icollection"></a>Wykrywanie indeksów na podstawie interfejsu ICollection

Inspiracją dla tego zachowania były Inicjatory kolekcji. Użycie struktury typu w celu przekazania go do funkcji. W przypadku typów inicjatorów kolekcji można wybrać funkcję przez implementację interfejsu `IEnumerable` (nieogólny).

Ta propozycja początkowo wymagała, aby typy implementują `ICollection` w celu zakwalifikowania się jako indeksowanie. To wymagało wielu specjalnych przypadków, chociaż:

- `ref struct`: te nie mogą implementować interfejsów, które są jeszcze takie jak `Span<T>` są idealnym rozwiązaniem dla obsługi indeksów/zakresów. 
- `string`: nie implementuje `ICollection` i Dodawanie `interface` ma duży koszt.

Oznacza to, że obsługa typów kluczy specjalna jest już wymagana. Specjalna wielkość liter `string` jest mniej interesująca, ponieważ język robi to w innych obszarach (`foreach` obniżanie, stałe itp.). Specjalna wielkość liter `ref struct` ma więcej informacji o tym, że jest to specjalna wielkość liter w całej klasie typów. Są one oznaczone jako indeksowane, jeśli po prostu mają właściwość o nazwie `Count` z typem zwracanym `int`. 

Po rozpatrzeniu projekt został znormalizowany, aby powiedzieć, że każdy typ, który ma właściwość `Count` / `Length` z typem zwracanym `int`, można indeksować. Spowoduje to usunięcie wszystkich specjalnych wielkości liter, nawet w przypadku `string` i tablic.

### <a name="detect-just-count"></a>Wykryj tylko liczbę

Wykrywanie w nazwach właściwości `Count` lub `Length`a komplikuje projekt a. Wybranie jednej do standaryzacji nie jest wystarczające, ponieważ skończy się z wyjątkiem dużej liczby typów:

- Użyj `Length`: wyklucza całkiem wszystkie kolekcje w przestrzeni nazw System. Collections i sub-Namespaces. Te oferty mają na celu wypróbowanie od `ICollection`, w związku z czym Preferuj `Count` o długości.
- Użyj `Count`: wyklucza `string`, tablice, `Span<T>` i większość typów opartych na `ref struct`

Dodatkowe komplikacje związane z początkowym wykrywaniem typów z indeksem są przeważone przez jego uproszczenie w innych aspektach.

### <a name="choice-of-slice-as-a-name"></a>Wybór wycinka jako nazwy

Nazwa `Slice` została wybrana jako niefaktyczna nazwa standardowa dla operacji w stylu wycinka w programie .NET. Począwszy od netcoreapp 2.1 wszystkie typy stylu zakresu używają nazwy `Slice` na potrzeby operacji dzielenia. Przed netcoreapp 2.1 w rzeczywistości nie ma żadnych przykładów tworzenia wycinków, na przykład. Typy takie jak `List<T>`, `ArraySegment<T>``SortedList<T>` byłyby idealne dla wycinków, ale koncepcja nie istniała podczas dodawania typów. 

W takim przypadku `Slice` być jedynym przykładem, został wybrany jako nazwa.

### <a name="index-target-type-conversion"></a>Konwersja typu elementu docelowego indeksu

Inny sposób wyświetlania przekształcenia `Index` w wyrażeniu indeksatora jest jako konwersja typu docelowego. Zamiast powiązań tak, jakby istnieje element członkowski formularza `return_type this[Index]`, język zamiast przypisuje do `int`konwersję typu docelowego. 

Koncepcja ta może być uogólniona wszystkim członkom dostępu do typów z liczbą. Za każdym razem, gdy wyrażenie z typem `Index` jest używany jako argument wywołania elementu członkowskiego wystąpienia, a odbiornik jest liczony, wówczas wyrażenie będzie miało konwersję typu docelowego do `int`. Wywołania elementu członkowskiego odpowiednie dla tej konwersji obejmują metody, indeksatory, właściwości, metody rozszerzające itp... Tylko konstruktory są wykluczone, ponieważ nie mają odbiornika. 

Konwersja typu docelowego zostanie zaimplementowana w następujący sposób dla dowolnego wyrażenia, które ma typ `Index`. W celach dyskusji można używać przykładu `receiver[expr]`:

- Gdy `expr` ma postać `^expr2` i typ `expr2` jest `int`, zostanie przetłumaczony na `receiver.Length - expr2`.
- W przeciwnym razie zostanie przetłumaczone jako `expr.GetOffset(receiver.Length)`.

Wyrażenia `receiver` i `Length` zostaną rozlane zgodnie z potrzebami, aby upewnić się, że wszystkie efekty uboczne są wykonywane tylko raz. Na przykład:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

Ten kod będzie drukował "Uzyskaj długość 3". 

Ta funkcja będzie korzystna dla każdego elementu członkowskiego, który miał parametr, który reprezentuje indeks. Na przykład: `List<T>.InsertAt`. Ma to również potencjalne znaczenie dla pomyłki, ponieważ język nie może podawać wskazówek dotyczących tego, czy wyrażenie jest przeznaczone do indeksowania. Wszystkie te możliwości można przekonwertować dowolne wyrażenie `Index`, aby `int` podczas wywoływania elementu członkowskiego w przypadku typu możliwego do zliczenia. 

Dotyczących

- Ta konwersja ma zastosowanie tylko wtedy, gdy wyrażenie z typem `Index` jest bezpośrednio argumentem elementu członkowskiego. Nie ma zastosowania do żadnych wyrażeń zagnieżdżonych.

## <a name="decisions-made-during-implementation"></a>Decyzje podejmowane w trakcie implementacji

- Wszystkie elementy członkowskie we wzorcu muszą być elementami członkowskimi wystąpienia
- Jeśli znaleziono metodę Length, ale ma zły typ zwracany, Kontynuuj wyszukiwanie liczby
- Indeksator używany dla wzorca indeksu musi mieć dokładnie jeden parametr int
- Metoda wycinka użyta dla wzorca zakresu musi mieć dokładnie dwa parametry int
- Podczas wyszukiwania elementów członkowskich wzorca szukamy pierwotnych definicji, a nie skonstruowanych elementów członkowskich

## <a name="design-meetings"></a>Spotkania projektowe

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a>Masz
