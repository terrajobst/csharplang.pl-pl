---
ms.openlocfilehash: 50f2bd2d0a84064cfe35fe65b9e5c59c052d19ac
ms.sourcegitcommit: 1dbb8e82bed5012a58a3a035bf2c3737ed570d07
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80122945"
---
# <a name="ranges"></a><span data-ttu-id="f9cb4-101">Zakresy</span><span class="sxs-lookup"><span data-stu-id="f9cb4-101">Ranges</span></span>

## <a name="summary"></a><span data-ttu-id="f9cb4-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="f9cb4-102">Summary</span></span>

<span data-ttu-id="f9cb4-103">Ta funkcja zawiera dwa nowe operatory, które umożliwiają konstruowanie `System.Index` i `System.Range` obiektów oraz używanie ich do kolekcji indeksów/wycinków w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-103">This feature is about delivering two new operators that allow constructing `System.Index` and `System.Range` objects, and using them to index/slice collections at runtime.</span></span>

## <a name="overview"></a><span data-ttu-id="f9cb4-104">Omówienie</span><span class="sxs-lookup"><span data-stu-id="f9cb4-104">Overview</span></span>

### <a name="well-known-types-and-members"></a><span data-ttu-id="f9cb4-105">Dobrze znane typy i elementy członkowskie</span><span class="sxs-lookup"><span data-stu-id="f9cb4-105">Well-known types and members</span></span>

<span data-ttu-id="f9cb4-106">Aby użyć nowych formularzy składni dla `System.Index` i `System.Range`, nowe dobrze znane typy i elementy członkowskie mogą być niezbędne, w zależności od tego, które formy składni są używane.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-106">To use the new syntactic forms for `System.Index` and `System.Range`, new well-known types and members may be necessary, depending on which syntactic forms are used.</span></span>

<span data-ttu-id="f9cb4-107">Aby użyć operatora "Hat" (`^`), wymagane są następujące elementy</span><span class="sxs-lookup"><span data-stu-id="f9cb4-107">To use the "hat" operator (`^`), the following is required</span></span>

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

<span data-ttu-id="f9cb4-108">Aby użyć `System.Index` typu jako argumentu w dostępie do elementu tablicy, wymagany jest następujący element członkowski:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-108">To use the `System.Index` type as an argument in an array element access, the following member is required:</span></span>

```csharp
int System.Index.GetOffset(int length);
```

<span data-ttu-id="f9cb4-109">Składnia `..` dla `System.Range` będzie wymagała typu `System.Range`, a także co najmniej jednego z następujących elementów członkowskich:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-109">The `..` syntax for `System.Range` will require the `System.Range` type, as well as one or more of the following members:</span></span>

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

<span data-ttu-id="f9cb4-110">Składnia `..` może być nieobecna dla obu argumentów lub żadnego z nich.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-110">The `..` syntax allows for either, both, or none of its arguments to be absent.</span></span> <span data-ttu-id="f9cb4-111">Bez względu na liczbę argumentów, Konstruktor `Range` zawsze wystarcza do użycia składni `Range`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-111">Regardless of the number of arguments, the `Range` constructor is always sufficient for using the `Range` syntax.</span></span> <span data-ttu-id="f9cb4-112">Jeśli jednak którykolwiek z innych elementów członkowskich jest obecny i brakuje co najmniej jednego z `..` argumentów, odpowiedni element członkowski może zostać zastąpiony.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-112">However, if any of the other members are present and one or more of the `..` arguments are missing, the appropriate member may be substituted.</span></span>

<span data-ttu-id="f9cb4-113">Na koniec dla wartości typu `System.Range`, która ma być używana w wyrażeniu dostępu do elementu tablicy, musi być obecny następujący element członkowski:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-113">Finally, for a value of type `System.Range` to be used in an array element access expression, the following member must be present:</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a><span data-ttu-id="f9cb4-114">System. index</span><span class="sxs-lookup"><span data-stu-id="f9cb4-114">System.Index</span></span>

<span data-ttu-id="f9cb4-115">C#nie ma możliwości indeksowania kolekcji od końca, ale większość indeksatorów korzysta z koncepcji "from Start" lub "Length-i".</span><span class="sxs-lookup"><span data-stu-id="f9cb4-115">C# has no way of indexing a collection from the end, but rather most indexers use the "from start" notion, or do a "length - i" expression.</span></span> <span data-ttu-id="f9cb4-116">Wprowadzimy nowe wyrażenie indeksu oznaczające "od końca".</span><span class="sxs-lookup"><span data-stu-id="f9cb4-116">We introduce a new Index expression that means "from the end".</span></span> <span data-ttu-id="f9cb4-117">Funkcja wprowadzi nowy operator "Hat" prefiksu jednoargumentowego.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-117">The feature will introduce a new unary prefix "hat" operator.</span></span> <span data-ttu-id="f9cb4-118">Jeden operand musi być konwertowany do `System.Int32`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-118">Its single operand must be convertible to `System.Int32`.</span></span> <span data-ttu-id="f9cb4-119">Zostanie on obniżony do odpowiedniego wywołania metody fabryki `System.Index`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-119">It will be lowered into the appropriate `System.Index` factory method call.</span></span>

<span data-ttu-id="f9cb4-120">Rozszerzamy gramatykę dla *unary_expression* przy użyciu następującej formy dodatkowej składni:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-120">We augment the grammar for *unary_expression* with the following additional syntax form:</span></span>

```antlr
unary_expression
    : '^' unary_expression
    ;
```

<span data-ttu-id="f9cb4-121">Wywołujemy ten *indeks z operatora końcowego* .</span><span class="sxs-lookup"><span data-stu-id="f9cb4-121">We call this the *index from end* operator.</span></span> <span data-ttu-id="f9cb4-122">Wstępnie zdefiniowany *indeks z operatorów End* jest następujący:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-122">The predefined *index from end* operators are as follows:</span></span>

```csharp
    System.Index operator ^(int fromEnd);
```

<span data-ttu-id="f9cb4-123">Zachowanie tego operatora jest zdefiniowane tylko dla wartości wejściowych większej lub równej zero.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-123">The behavior of this operator is only defined for input values greater than or equal to zero.</span></span>

<span data-ttu-id="f9cb4-124">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-124">Examples:</span></span>

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a><span data-ttu-id="f9cb4-125">System. Range</span><span class="sxs-lookup"><span data-stu-id="f9cb4-125">System.Range</span></span>

<span data-ttu-id="f9cb4-126">C#nie ma składniowego sposobu uzyskiwania dostępu do "zakresów" lub "wycinków" kolekcji.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-126">C# has no syntactic way to access "ranges" or "slices" of collections.</span></span> <span data-ttu-id="f9cb4-127">Zwykle użytkownicy są zmuszeni do implementowania złożonych struktur do filtrowania/obsługi wycinków pamięci lub do metod LINQ, takich jak `list.Skip(5).Take(2)`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-127">Usually users are forced to implement complex structures to filter/operate on slices of memory, or resort to LINQ methods like `list.Skip(5).Take(2)`.</span></span> <span data-ttu-id="f9cb4-128">Po dodaniu `System.Span<T>` i innych podobnych typów, ważne jest, aby ten rodzaj operacji był obsługiwany na wyższym poziomie w języku/czasie wykonywania i mieć ujednolicony interfejs.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-128">With the addition of `System.Span<T>` and other similar types, it becomes more important to have this kind of operation supported on a deeper level in the language/runtime, and have the interface unified.</span></span>

<span data-ttu-id="f9cb4-129">Język wprowadzi nowy operator zakresu `x..y`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-129">The language will introduce a new range operator `x..y`.</span></span> <span data-ttu-id="f9cb4-130">Jest to binarny operator wrostkowe, który akceptuje dwa wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-130">It is a binary infix operator that accepts two expressions.</span></span> <span data-ttu-id="f9cb4-131">Każdy operand może zostać pominięty (przykłady poniżej) i musi być konwertowany do `System.Index`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-131">Either operand can be omitted (examples below), and they have to be convertible to `System.Index`.</span></span> <span data-ttu-id="f9cb4-132">Zostanie on obniżony do odpowiedniego wywołania metody fabryki `System.Range`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-132">It will be lowered to the appropriate `System.Range` factory method call.</span></span>

<span data-ttu-id="f9cb4-133">Zamieniamy reguły C# gramatyki dla *multiplicative_expression* na następujące (w celu wprowadzenia nowego poziomu pierwszeństwa):</span><span class="sxs-lookup"><span data-stu-id="f9cb4-133">We replace the C# grammar rules for *multiplicative_expression* with the following (in order to introduce a new precedence level):</span></span>

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

<span data-ttu-id="f9cb4-134">Wszystkie formularze *operatora Range* mają takie samo pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-134">All forms of the *range operator* have the same precedence.</span></span> <span data-ttu-id="f9cb4-135">Ta nowa grupa pierwszeństwa jest niższa niż *operatory jednoargumentowe* i większe niż *Operatory arytmetyczne mulitiplicative*.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-135">This new precedence group is lower than the *unary operators* and higher than the *mulitiplicative arithmetic operators*.</span></span>

<span data-ttu-id="f9cb4-136">Wywołujemy operator `..` operatorem *zakresu*.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-136">We call the `..` operator the *range operator*.</span></span> <span data-ttu-id="f9cb4-137">Wbudowany operator zakresu może być w przybliżeniu rozumiany jako odpowiadający wywołaniu wbudowanego operatora tego formularza:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-137">The built-in range operator can roughly be understood to correspond to the invocation of a built-in operator of this form:</span></span>

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

<span data-ttu-id="f9cb4-138">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-138">Examples:</span></span>

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

<span data-ttu-id="f9cb4-139">Ponadto `System.Index` powinna mieć niejawną konwersję z `System.Int32`, aby nie trzeba było przeciążać do mieszania liczb całkowitych i indeksów przez wielowymiarowe podpisy.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-139">Moreover, `System.Index` should have an implicit conversion from `System.Int32`, in order not to need to overload for mixing integers and indexes over multi-dimensional signatures.</span></span>

## <a name="adding-index-and-range-support-to-existing-library-types"></a><span data-ttu-id="f9cb4-140">Dodawanie obsługi indeksu i zakresu do istniejących typów bibliotek</span><span class="sxs-lookup"><span data-stu-id="f9cb4-140">Adding Index and Range support to existing library types</span></span>

### <a name="implicit-index-support"></a><span data-ttu-id="f9cb4-141">Niejawna obsługa indeksu</span><span class="sxs-lookup"><span data-stu-id="f9cb4-141">Implicit Index support</span></span>

<span data-ttu-id="f9cb4-142">Język zapewni składową indeksatora wystąpienia z pojedynczym parametrem typu `Index` dla typów, które spełniają następujące kryteria:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-142">The language will provide an instance indexer member with a single parameter of type `Index` for types which meet the following criteria:</span></span>

- <span data-ttu-id="f9cb4-143">Typ jest do zliczenia.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-143">The type is Countable.</span></span>
- <span data-ttu-id="f9cb4-144">Typ ma dostępny indeksator wystąpienia, który przyjmuje jako argument pojedynczy `int`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-144">The type has an accessible instance indexer which takes a single `int` as the argument.</span></span>
- <span data-ttu-id="f9cb4-145">Typ nie ma dostępnego indeksatora wystąpienia, który przyjmuje `Index` jako pierwszy parametr.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-145">The type does not have an accessible instance indexer which takes an `Index` as the first parameter.</span></span> <span data-ttu-id="f9cb4-146">`Index` musi być jedynym parametrem lub pozostałe parametry muszą być opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-146">The `Index` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="f9cb4-147">Typ jest możliwy do ***zliczenia*** , jeśli ma właściwość o nazwie `Length` lub `Count` z dostępną metodę pobierającą i typem zwracanym `int`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-147">A type is ***Countable*** if it  has a property named `Length` or `Count` with an accessible getter and a return type of `int`.</span></span> <span data-ttu-id="f9cb4-148">Język może korzystać z tej właściwości, aby przekonwertować wyrażenie typu `Index` na `int` w punkcie wyrażenia bez konieczności używania typu `Index` w ogóle.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-148">The language can make use of this property to convert an expression of type `Index` into an `int` at the point of the expression without the need to use the type `Index` at all.</span></span> <span data-ttu-id="f9cb4-149">W przypadku obecności obu `Length` i `Count`, `Length` będzie preferowane.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-149">In case both `Length` and `Count` are present, `Length` will be preferred.</span></span> <span data-ttu-id="f9cb4-150">W celu uproszczenia, propozycja będzie używać nazwy `Length` do reprezentowania `Count` lub `Length`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-150">For simplicity going forward, the proposal will use the name `Length` to represent `Count` or `Length`.</span></span>

<span data-ttu-id="f9cb4-151">W przypadku takich typów język działa tak, jakby istnieje element członkowski indeksatora w formularzu `T this[Index index]`, gdzie `T` jest typem zwracanym na podstawie `int` indeksatora, w tym wszelkie adnotacje stylów `ref`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-151">For such types, the language will act as if there is an indexer member of the form `T this[Index index]` where `T` is the return type of the `int` based indexer including any `ref` style annotations.</span></span> <span data-ttu-id="f9cb4-152">Nowy element członkowski będzie miał te same `get` i `set` elementy członkowskie o pasujących ułatwieniach dostępu jako indeksator `int`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-152">The new member will have the same `get` and `set` members with matching accessibility as the `int` indexer.</span></span> 

<span data-ttu-id="f9cb4-153">Nowy indeksator zostanie zaimplementowany przez konwersję argumentu typu `Index` na `int` i emitowanie wywołania do indeksatora opartego na `int`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-153">The new indexer will be implemented by converting the argument of type `Index` into an `int` and emitting a call to the `int` based indexer.</span></span> <span data-ttu-id="f9cb4-154">Na potrzeby dyskusji użyjmy przykładu `receiver[expr]`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-154">For discussion purposes, let's use the example of `receiver[expr]`.</span></span> <span data-ttu-id="f9cb4-155">Konwersja `expr` do `int` zostanie wykonana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-155">The conversion of `expr` to `int` will occur as follows:</span></span>

- <span data-ttu-id="f9cb4-156">Gdy argument ma postać `^expr2` i typ `expr2` jest `int`, zostanie przetłumaczony na `receiver.Length - expr2`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-156">When the argument is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="f9cb4-157">W przeciwnym razie zostanie przetłumaczone jako `expr.GetOffset(receiver.Length)`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-157">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="f9cb4-158">Dzięki temu deweloperzy mogą korzystać z funkcji `Index` na istniejących typach bez potrzeby modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-158">This allows for developers to use the `Index` feature on existing types without the need for modification.</span></span> <span data-ttu-id="f9cb4-159">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-159">For example:</span></span>

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

<span data-ttu-id="f9cb4-160">Wyrażenia `receiver` i `Length` zostaną rozlane zgodnie z potrzebami, aby upewnić się, że wszystkie efekty uboczne są wykonywane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-160">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="f9cb4-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-161">For example:</span></span>

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

<span data-ttu-id="f9cb4-162">Ten kod będzie drukował "Uzyskaj długość 3".</span><span class="sxs-lookup"><span data-stu-id="f9cb4-162">This code will print "Get Length 3".</span></span>

### <a name="implicit-range-support"></a><span data-ttu-id="f9cb4-163">Niejawna obsługa zakresu</span><span class="sxs-lookup"><span data-stu-id="f9cb4-163">Implicit Range support</span></span>

<span data-ttu-id="f9cb4-164">Język zapewni składową indeksatora wystąpienia z pojedynczym parametrem typu `Range` dla typów, które spełniają następujące kryteria:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-164">The language will provide an instance indexer member with a single parameter of type `Range` for types which meet the following criteria:</span></span>

- <span data-ttu-id="f9cb4-165">Typ jest do zliczenia.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-165">The type is Countable.</span></span>
- <span data-ttu-id="f9cb4-166">Typ ma dostępny element członkowski o nazwie `Slice`, który ma dwa parametry typu `int`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-166">The type has an accessible member named `Slice` which has two parameters of type `int`.</span></span>
- <span data-ttu-id="f9cb4-167">Typ nie ma indeksatora wystąpienia, który przyjmuje jeden `Range` jako pierwszy parametr.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-167">The type does not have an instance indexer which takes a single `Range` as the first parameter.</span></span> <span data-ttu-id="f9cb4-168">`Range` musi być jedynym parametrem lub pozostałe parametry muszą być opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-168">The `Range` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="f9cb4-169">W przypadku takich typów język zostanie powiązany tak, jakby istnieje element członkowski indeksatora w formularzu `T this[Range range]`, gdzie `T` jest typem zwracanym metody `Slice`, łącznie ze wszystkimi adnotacjami stylów `ref`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-169">For such types, the language will bind as if there is an indexer member of the form `T this[Range range]` where `T` is the return type of the `Slice` method including any `ref` style annotations.</span></span> <span data-ttu-id="f9cb4-170">Nowy element członkowski będzie miał również pasujące ułatwienia dostępu przy użyciu `Slice`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-170">The new member will also have matching accessibility with `Slice`.</span></span> 

<span data-ttu-id="f9cb4-171">Gdy indeksator oparty na `Range` jest powiązany z wyrażeniem o nazwie `receiver`, zostanie on obniżony przez konwersję wyrażenia `Range` na dwie wartości, które są następnie przenoszone do metody `Slice`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-171">When the `Range` based indexer is bound on an expression named `receiver`, it will be lowered by converting the `Range` expression into two values that are then passed to the `Slice` method.</span></span> <span data-ttu-id="f9cb4-172">Na potrzeby dyskusji użyjmy przykładu `receiver[expr]`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-172">For discussion purposes, let's use the example of `receiver[expr]`.</span></span>

<span data-ttu-id="f9cb4-173">Pierwszy argument `Slice` zostanie uzyskany przez konwersję wyrażenia z określonym zakresem w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-173">The first argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="f9cb4-174">Gdy `expr` ma postać `expr1..expr2` (gdzie `expr2` można pominąć), a `expr1` ma typ `int`, zostanie on wyemitowany jako `expr1`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-174">When `expr` is of the form `expr1..expr2` (where `expr2` can be omitted) and `expr1` has type `int`, then it will be emitted as `expr1`.</span></span>
- <span data-ttu-id="f9cb4-175">Gdy `expr` ma postać `^expr1..expr2` (gdzie `expr2` można pominąć), zostanie ona wyemitowana jako `receiver.Length - expr1`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-175">When `expr` is of the form `^expr1..expr2` (where `expr2` can be omitted), then it will be emitted as `receiver.Length - expr1`.</span></span>
- <span data-ttu-id="f9cb4-176">Gdy `expr` ma postać `..expr2` (gdzie `expr2` można pominąć), zostanie ona wyemitowana jako `0`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-176">When `expr` is of the form `..expr2` (where `expr2` can be omitted), then it will be emitted as `0`.</span></span>
- <span data-ttu-id="f9cb4-177">W przeciwnym razie zostanie on wyemitowany jako `expr.Start.GetOffset(receiver.Length)`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-177">Otherwise, it will be emitted as `expr.Start.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="f9cb4-178">Ta wartość zostanie ponownie użyta w obliczeniu drugiego argumentu `Slice`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-178">This value will be re-used in the calculation of the second `Slice` argument.</span></span> <span data-ttu-id="f9cb4-179">Po wykonaniu tej czynności będzie ona nazywana `start`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-179">When doing so it will be referred to as `start`.</span></span> <span data-ttu-id="f9cb4-180">Drugi argument `Slice` zostanie uzyskany przez konwersję wyrażenia z określonym zakresem w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-180">The second argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="f9cb4-181">Gdy `expr` ma postać `expr1..expr2` (gdzie `expr1` można pominąć), a `expr2` ma typ `int`, zostanie on wyemitowany jako `expr2 - start`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-181">When `expr` is of the form `expr1..expr2` (where `expr1` can be omitted) and `expr2` has type `int`, then it will be emitted as `expr2 - start`.</span></span>
- <span data-ttu-id="f9cb4-182">Gdy `expr` ma postać `expr1..^expr2` (gdzie `expr1` można pominąć), zostanie ona wyemitowana jako `(receiver.Length - expr2) - start`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-182">When `expr` is of the form `expr1..^expr2` (where `expr1` can be omitted), then it will be emitted as `(receiver.Length - expr2) - start`.</span></span>
- <span data-ttu-id="f9cb4-183">Gdy `expr` ma postać `expr1..` (gdzie `expr1` można pominąć), zostanie ona wyemitowana jako `receiver.Length - start`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-183">When `expr` is of the form `expr1..` (where `expr1` can be omitted), then it will be emitted as `receiver.Length - start`.</span></span>
- <span data-ttu-id="f9cb4-184">W przeciwnym razie zostanie on wyemitowany jako `expr.End.GetOffset(receiver.Length) - start`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-184">Otherwise, it will be emitted as `expr.End.GetOffset(receiver.Length) - start`.</span></span>

<span data-ttu-id="f9cb4-185">Wyrażenia `receiver`, `Length`i `expr` zostaną rozlane zgodnie z potrzebami, aby upewnić się, że wszystkie efekty uboczne są wykonywane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-185">The `receiver`, `Length`, and `expr` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="f9cb4-186">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-186">For example:</span></span>

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

<span data-ttu-id="f9cb4-187">Ten kod będzie drukował "Uzyskaj długość 2".</span><span class="sxs-lookup"><span data-stu-id="f9cb4-187">This code will print "Get Length 2".</span></span>

<span data-ttu-id="f9cb4-188">Język będzie specjalny przypadku następujących znanych typów:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-188">The language will special case the following known types:</span></span> 

- <span data-ttu-id="f9cb4-189">`string`: Metoda `Substring` zostanie użyta zamiast `Slice`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-189">`string`: the method `Substring` will be used instead of `Slice`.</span></span>
- <span data-ttu-id="f9cb4-190">`array`: Metoda `System.Reflection.CompilerServices.GetSubArray` zostanie użyta zamiast `Slice`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-190">`array`: the method `System.Reflection.CompilerServices.GetSubArray` will be used instead of `Slice`.</span></span>

## <a name="alternatives"></a><span data-ttu-id="f9cb4-191">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="f9cb4-191">Alternatives</span></span>

<span data-ttu-id="f9cb4-192">Nowe operatory (`^` i `..`) są cukrem składni.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-192">The new operators (`^` and `..`) are syntactic sugar.</span></span> <span data-ttu-id="f9cb4-193">Funkcje mogą być implementowane przez jawne wywołania do `System.Index` i `System.Range` metody fabryki, ale spowoduje to powstanie dużej ilości kodu standardowego i środowisko będzie nieintuicyjne.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-193">The functionality can be implemented by explicit calls to `System.Index` and `System.Range` factory methods, but it will result in a lot more boilerplate code, and the experience will be unintuitive.</span></span>

## <a name="il-representation"></a><span data-ttu-id="f9cb4-194">Reprezentacja IL</span><span class="sxs-lookup"><span data-stu-id="f9cb4-194">IL Representation</span></span>

<span data-ttu-id="f9cb4-195">Te dwa operatory zostaną obniżone do regularnych wywołań indeksatora/metody bez zmian w kolejnych warstwach kompilatora.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-195">These two operators will be lowered to regular indexer/method calls, with no change in subsequent compiler layers.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="f9cb4-196">Zachowanie środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="f9cb4-196">Runtime behavior</span></span>

- <span data-ttu-id="f9cb4-197">Kompilator może zoptymalizować indeksatory dla wbudowanych typów, takich jak tablice i ciągi, i obniżyć indeksowanie do odpowiednich istniejących metod.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-197">Compiler can optimize indexers for built-in types like arrays and strings, and lower the indexing to the appropriate existing methods.</span></span>
- <span data-ttu-id="f9cb4-198">`System.Index` będzie zgłaszany, jeśli skonstruowane z wartością ujemną.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-198">`System.Index` will throw if constructed with a negative value.</span></span>
- <span data-ttu-id="f9cb4-199">`^0` nie zgłasza, ale tłumaczy na długość kolekcji/wyliczalny, do której jest dostarczany.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-199">`^0` does not throw, but it translates to the length of the collection/enumerable it is supplied to.</span></span>
- <span data-ttu-id="f9cb4-200">`Range.All` jest semantycznie odpowiednikiem `0..^0`i można ją odbudować do tych indeksów.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-200">`Range.All` is semantically equivalent to `0..^0`, and can be deconstructed to these indices.</span></span>

## <a name="considerations"></a><span data-ttu-id="f9cb4-201">Zagadnienia do rozważenia</span><span class="sxs-lookup"><span data-stu-id="f9cb4-201">Considerations</span></span>

### <a name="detect-indexable-based-on-icollection"></a><span data-ttu-id="f9cb4-202">Wykrywanie indeksów na podstawie interfejsu ICollection</span><span class="sxs-lookup"><span data-stu-id="f9cb4-202">Detect Indexable based on ICollection</span></span>

<span data-ttu-id="f9cb4-203">Inspiracją dla tego zachowania były Inicjatory kolekcji.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-203">The inspiration for this behavior was collection initializers.</span></span> <span data-ttu-id="f9cb4-204">Użycie struktury typu w celu przekazania go do funkcji.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-204">Using the structure of a type to convey that it had opted into a feature.</span></span> <span data-ttu-id="f9cb4-205">W przypadku typów inicjatorów kolekcji można wybrać funkcję przez implementację interfejsu `IEnumerable` (nieogólny).</span><span class="sxs-lookup"><span data-stu-id="f9cb4-205">In the case of collection initializers types can opt into the feature by implementing the interface `IEnumerable` (non generic).</span></span>

<span data-ttu-id="f9cb4-206">Ta propozycja początkowo wymagała, aby typy implementują `ICollection` w celu zakwalifikowania się jako indeksowanie.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-206">This proposal initially required that types implement `ICollection` in order to qualify as Indexable.</span></span> <span data-ttu-id="f9cb4-207">To wymagało wielu specjalnych przypadków, chociaż:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-207">That required a number of special cases though:</span></span>

- <span data-ttu-id="f9cb4-208">`ref struct`: te nie mogą implementować interfejsów, które są jeszcze takie jak `Span<T>` są idealnym rozwiązaniem dla obsługi indeksów/zakresów.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-208">`ref struct`: these cannot implement interfaces yet types like `Span<T>` are ideal for index / range support.</span></span> 
- <span data-ttu-id="f9cb4-209">`string`: nie implementuje `ICollection` i Dodawanie `interface` ma duży koszt.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-209">`string`: does not implement `ICollection` and adding that `interface` has a large cost.</span></span>

<span data-ttu-id="f9cb4-210">Oznacza to, że obsługa typów kluczy specjalna jest już wymagana.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-210">This means to support key types special casing is already needed.</span></span> <span data-ttu-id="f9cb4-211">Specjalna wielkość liter `string` jest mniej interesująca, ponieważ język robi to w innych obszarach (`foreach` obniżanie, stałe itp.). Specjalna wielkość liter `ref struct` ma więcej informacji o tym, że jest to specjalna wielkość liter w całej klasie typów.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-211">The special casing of `string` is less interesting as the language does this in other areas (`foreach` lowering, constants, etc ...). The special casing of `ref struct` is more concerning as it's special casing an entire class of types.</span></span> <span data-ttu-id="f9cb4-212">Są one oznaczone jako indeksowane, jeśli po prostu mają właściwość o nazwie `Count` z typem zwracanym `int`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-212">They get labeled as Indexable if they simply have a property named `Count` with a return type of `int`.</span></span> 

<span data-ttu-id="f9cb4-213">Po rozpatrzeniu projekt został znormalizowany, aby powiedzieć, że każdy typ, który ma właściwość `Count` / `Length` z typem zwracanym `int`, można indeksować.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-213">After consideration the design was normalized to say that any type which has a property `Count` / `Length` with a return type of `int` is Indexable.</span></span> <span data-ttu-id="f9cb4-214">Spowoduje to usunięcie wszystkich specjalnych wielkości liter, nawet w przypadku `string` i tablic.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-214">That removes all special casing, even for `string` and arrays.</span></span>

### <a name="detect-just-count"></a><span data-ttu-id="f9cb4-215">Wykryj tylko liczbę</span><span class="sxs-lookup"><span data-stu-id="f9cb4-215">Detect just Count</span></span>

<span data-ttu-id="f9cb4-216">Wykrywanie w nazwach właściwości `Count` lub `Length`a komplikuje projekt a.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-216">Detecting on the property names `Count` or `Length` does complicate the design a bit.</span></span> <span data-ttu-id="f9cb4-217">Wybranie jednej do standaryzacji nie jest wystarczające, ponieważ skończy się z wyjątkiem dużej liczby typów:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-217">Picking just one to standardize though is not sufficient as it ends up excluding a large number of types:</span></span>

- <span data-ttu-id="f9cb4-218">Użyj `Length`: wyklucza całkiem wszystkie kolekcje w przestrzeni nazw System. Collections i sub-Namespaces.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-218">Use `Length`: excludes pretty much every collection in System.Collections and sub-namespaces.</span></span> <span data-ttu-id="f9cb4-219">Te oferty mają na celu wypróbowanie od `ICollection`, w związku z czym Preferuj `Count` o długości.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-219">Those tend to derive from `ICollection` and hence prefer `Count` over length.</span></span>
- <span data-ttu-id="f9cb4-220">Użyj `Count`: wyklucza `string`, tablice, `Span<T>` i większość typów opartych na `ref struct`</span><span class="sxs-lookup"><span data-stu-id="f9cb4-220">Use `Count`: excludes `string`, arrays, `Span<T>` and most `ref struct` based types</span></span>

<span data-ttu-id="f9cb4-221">Dodatkowe komplikacje związane z początkowym wykrywaniem typów z indeksem są przeważone przez jego uproszczenie w innych aspektach.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-221">The extra complication on the initial detection of Indexable types is outweighed by its simplification in other aspects.</span></span>

### <a name="choice-of-slice-as-a-name"></a><span data-ttu-id="f9cb4-222">Wybór wycinka jako nazwy</span><span class="sxs-lookup"><span data-stu-id="f9cb4-222">Choice of Slice as a name</span></span>

<span data-ttu-id="f9cb4-223">Nazwa `Slice` została wybrana jako niefaktyczna nazwa standardowa dla operacji w stylu wycinka w programie .NET.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-223">The name `Slice` was chosen as it's the de-facto standard name for slice style operations in .NET.</span></span> <span data-ttu-id="f9cb4-224">Począwszy od netcoreapp 2.1 wszystkie typy stylu zakresu używają nazwy `Slice` na potrzeby operacji dzielenia.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-224">Starting with netcoreapp2.1 all span style types use the name `Slice` for slicing operations.</span></span> <span data-ttu-id="f9cb4-225">Przed netcoreapp 2.1 w rzeczywistości nie ma żadnych przykładów tworzenia wycinków, na przykład.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-225">Prior to netcoreapp2.1 there really aren't any examples of slicing to look to for an example.</span></span> <span data-ttu-id="f9cb4-226">Typy takie jak `List<T>`, `ArraySegment<T>``SortedList<T>` byłyby idealne dla wycinków, ale koncepcja nie istniała podczas dodawania typów.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-226">Types like `List<T>`, `ArraySegment<T>`, `SortedList<T>` would've been ideal for slicing but the concept didn't exist when types were added.</span></span> 

<span data-ttu-id="f9cb4-227">W takim przypadku `Slice` być jedynym przykładem, został wybrany jako nazwa.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-227">Thus, `Slice` being the sole example, it was chosen as the name.</span></span>

### <a name="index-target-type-conversion"></a><span data-ttu-id="f9cb4-228">Konwersja typu elementu docelowego indeksu</span><span class="sxs-lookup"><span data-stu-id="f9cb4-228">Index target type conversion</span></span>

<span data-ttu-id="f9cb4-229">Inny sposób wyświetlania przekształcenia `Index` w wyrażeniu indeksatora jest jako konwersja typu docelowego.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-229">Another way to view the `Index` transformation in an indexer expression is as a target type conversion.</span></span> <span data-ttu-id="f9cb4-230">Zamiast powiązań tak, jakby istnieje element członkowski formularza `return_type this[Index]`, język zamiast przypisuje do `int`konwersję typu docelowego.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-230">Instead of binding as if there is a member of the form `return_type this[Index]`, the language instead assigns a target typed conversion to `int`.</span></span> 

<span data-ttu-id="f9cb4-231">Koncepcja ta może być uogólniona wszystkim członkom dostępu do typów z liczbą.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-231">This concept could be generalized to all member access on Countable types.</span></span> <span data-ttu-id="f9cb4-232">Za każdym razem, gdy wyrażenie z typem `Index` jest używany jako argument wywołania elementu członkowskiego wystąpienia, a odbiornik jest liczony, wówczas wyrażenie będzie miało konwersję typu docelowego do `int`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-232">Whenever an expression with type `Index` is used as an argument to an instance member invocation and the receiver is Countable then the expression will have a target type conversion to `int`.</span></span> <span data-ttu-id="f9cb4-233">Wywołania elementu członkowskiego odpowiednie dla tej konwersji obejmują metody, indeksatory, właściwości, metody rozszerzające itp... Tylko konstruktory są wykluczone, ponieważ nie mają odbiornika.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-233">The member invocations applicable for this conversion include methods, indexers, properties, extension methods, etc ... Only constructors are excluded as they have no receiver.</span></span> 

<span data-ttu-id="f9cb4-234">Konwersja typu docelowego zostanie zaimplementowana w następujący sposób dla dowolnego wyrażenia, które ma typ `Index`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-234">The target type conversion will be implemented as follows for any expression which has a type of `Index`.</span></span> <span data-ttu-id="f9cb4-235">W celach dyskusji można używać przykładu `receiver[expr]`:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-235">For discussion purposes lets use the example of `receiver[expr]`:</span></span>

- <span data-ttu-id="f9cb4-236">Gdy `expr` ma postać `^expr2` i typ `expr2` jest `int`, zostanie przetłumaczony na `receiver.Length - expr2`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-236">When `expr` is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="f9cb4-237">W przeciwnym razie zostanie przetłumaczone jako `expr.GetOffset(receiver.Length)`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-237">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="f9cb4-238">Wyrażenia `receiver` i `Length` zostaną rozlane zgodnie z potrzebami, aby upewnić się, że wszystkie efekty uboczne są wykonywane tylko raz.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-238">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="f9cb4-239">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f9cb4-239">For example:</span></span>

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

<span data-ttu-id="f9cb4-240">Ten kod będzie drukował "Uzyskaj długość 3".</span><span class="sxs-lookup"><span data-stu-id="f9cb4-240">This code will print "Get Length 3".</span></span> 

<span data-ttu-id="f9cb4-241">Ta funkcja będzie korzystna dla każdego elementu członkowskiego, który miał parametr, który reprezentuje indeks.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-241">This feature would be beneficial to any member which had a parameter that represented an index.</span></span> <span data-ttu-id="f9cb4-242">Na przykład: `List<T>.InsertAt`.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-242">For example `List<T>.InsertAt`.</span></span> <span data-ttu-id="f9cb4-243">Ma to również potencjalne znaczenie dla pomyłki, ponieważ język nie może podawać wskazówek dotyczących tego, czy wyrażenie jest przeznaczone do indeksowania.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-243">This also has the potential for confusion as the language can't give any guidance as to whether or not an expression is meant for indexing.</span></span> <span data-ttu-id="f9cb4-244">Wszystkie te możliwości można przekonwertować dowolne wyrażenie `Index`, aby `int` podczas wywoływania elementu członkowskiego w przypadku typu możliwego do zliczenia.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-244">All it can do is convert any `Index` expression to `int` when invoking a member on a Countable type.</span></span> 

<span data-ttu-id="f9cb4-245">Dotyczących</span><span class="sxs-lookup"><span data-stu-id="f9cb4-245">Restrictions:</span></span>

- <span data-ttu-id="f9cb4-246">Ta konwersja ma zastosowanie tylko wtedy, gdy wyrażenie z typem `Index` jest bezpośrednio argumentem elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-246">This conversion is only applicable when the expression with type `Index` is directly an argument to the member.</span></span> <span data-ttu-id="f9cb4-247">Nie ma zastosowania do żadnych wyrażeń zagnieżdżonych.</span><span class="sxs-lookup"><span data-stu-id="f9cb4-247">It would not apply to any nested expressions.</span></span>

## <a name="decisions-made-during-implementation"></a><span data-ttu-id="f9cb4-248">Decyzje podejmowane w trakcie implementacji</span><span class="sxs-lookup"><span data-stu-id="f9cb4-248">Decisions made during implementation</span></span>

- <span data-ttu-id="f9cb4-249">Wszystkie elementy członkowskie we wzorcu muszą być elementami członkowskimi wystąpienia</span><span class="sxs-lookup"><span data-stu-id="f9cb4-249">All members in the pattern must be instance members</span></span>
- <span data-ttu-id="f9cb4-250">Jeśli znaleziono metodę Length, ale ma zły typ zwracany, Kontynuuj wyszukiwanie liczby</span><span class="sxs-lookup"><span data-stu-id="f9cb4-250">If a Length method is found but it has the wrong return type, continue looking for Count</span></span>
- <span data-ttu-id="f9cb4-251">Indeksator używany dla wzorca indeksu musi mieć dokładnie jeden parametr int</span><span class="sxs-lookup"><span data-stu-id="f9cb4-251">The indexer used for the Index pattern must have exactly one int parameter</span></span>
- <span data-ttu-id="f9cb4-252">Metoda wycinka użyta dla wzorca zakresu musi mieć dokładnie dwa parametry int</span><span class="sxs-lookup"><span data-stu-id="f9cb4-252">The Slice method used for the Range pattern must have exactly two int parameters</span></span>
- <span data-ttu-id="f9cb4-253">Podczas wyszukiwania elementów członkowskich wzorca szukamy pierwotnych definicji, a nie skonstruowanych elementów członkowskich</span><span class="sxs-lookup"><span data-stu-id="f9cb4-253">When looking for the pattern members, we look for original definitions, not constructed members</span></span>

## <a name="design-meetings"></a><span data-ttu-id="f9cb4-254">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="f9cb4-254">Design Meetings</span></span>

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a><span data-ttu-id="f9cb4-255">Masz</span><span class="sxs-lookup"><span data-stu-id="f9cb4-255">Questions</span></span>
