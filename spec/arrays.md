# <a name="arrays"></a><span data-ttu-id="8a911-101">Tablice</span><span class="sxs-lookup"><span data-stu-id="8a911-101">Arrays</span></span>

<span data-ttu-id="8a911-102">Tablica jest strukturą danych, która zawiera szereg zmiennych, które są dostępne za pośrednictwem obliczanej indeksów.</span><span class="sxs-lookup"><span data-stu-id="8a911-102">An array is a data structure that contains a number of variables which are accessed through computed indices.</span></span> <span data-ttu-id="8a911-103">Zmienne zawartych w tablicy jest określana skrótem elementy tablicy są wszystkie tego samego typu i tego typu jest nazywana typ elementu tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-103">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="8a911-104">Tablica ma rangę, która określa liczbę indeksów skojarzone z każdego elementu tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-104">An array has a rank which determines the number of indices associated with each array element.</span></span> <span data-ttu-id="8a911-105">Ranga tablicy jest również określany jako wymiary tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-105">The rank of an array is also referred to as the dimensions of the array.</span></span> <span data-ttu-id="8a911-106">Nosi nazwę tablicy o liczbie rangą równą jeden ***tablicy jednowymiarowej***.</span><span class="sxs-lookup"><span data-stu-id="8a911-106">An array with a rank of one is called a ***single-dimensional array***.</span></span> <span data-ttu-id="8a911-107">Więcej niż jedna nazywa się rangę tablicy o liczbie ***tablicy wielowymiarowej***.</span><span class="sxs-lookup"><span data-stu-id="8a911-107">An array with a rank greater than one is called a ***multi-dimensional array***.</span></span> <span data-ttu-id="8a911-108">Określone rozmiarze Wielowymiarowe tablice są często zwane tablice dwuwymiarowe, tablic trójwymiarowych i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="8a911-108">Specific sized multi-dimensional arrays are often referred to as two-dimensional arrays, three-dimensional arrays, and so on.</span></span>

<span data-ttu-id="8a911-109">Każdy wymiar tablicy ma skojarzone długości, która jest liczbą całkowitą większy lub równy zero.</span><span class="sxs-lookup"><span data-stu-id="8a911-109">Each dimension of an array has an associated length which is an integral number greater than or equal to zero.</span></span> <span data-ttu-id="8a911-110">Długości wymiarów nie są częścią typ tablicy, ale raczej są określane podczas tworzenia wystąpienia typu tablicy w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8a911-110">The dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run-time.</span></span> <span data-ttu-id="8a911-111">Długość drugiego wymiaru określa prawidłowy zakres indeksów dla tego wymiaru: Dla wymiaru o długości `N`, indeksów może wynosić od `0` do `N - 1` (włącznie).</span><span class="sxs-lookup"><span data-stu-id="8a911-111">The length of a dimension determines the valid range of indices for that dimension: For a dimension of length `N`, indices can range from `0` to `N - 1` inclusive.</span></span> <span data-ttu-id="8a911-112">Całkowita liczba elementów w tablicy jest produktem długości każdego wymiaru tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-112">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="8a911-113">Jeśli co najmniej jeden z wymiarów tablicy ma długość równą zero, tablica jest określany jako pusta.</span><span class="sxs-lookup"><span data-stu-id="8a911-113">If one or more of the dimensions of an array have a length of zero, the array is said to be empty.</span></span>

<span data-ttu-id="8a911-114">Typ elementu tablicy może być dowolnego typu, w tym typu tablicowego.</span><span class="sxs-lookup"><span data-stu-id="8a911-114">The element type of an array can be any type, including an array type.</span></span>

## <a name="array-types"></a><span data-ttu-id="8a911-115">Typy tablic</span><span class="sxs-lookup"><span data-stu-id="8a911-115">Array types</span></span>

<span data-ttu-id="8a911-116">Typ tablicy jest zapisywany jako *non_array_type* następuje co najmniej jeden *rank_specifier*s:</span><span class="sxs-lookup"><span data-stu-id="8a911-116">An array type is written as a *non_array_type* followed by one or more *rank_specifier*s:</span></span>

```antlr
array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;
```

<span data-ttu-id="8a911-117">A *non_array_type* jest dowolnym *typu* oznacza to nie sam *array_type*.</span><span class="sxs-lookup"><span data-stu-id="8a911-117">A *non_array_type* is any *type* that is not itself an *array_type*.</span></span>

<span data-ttu-id="8a911-118">Ranga typ tablicy jest nadawana przez najdalej z lewej strony *rank_specifier* w *array_type*: A *rank_specifier* wskazuje, że tablica jest tablicy z rangą równą jeden plus liczba "`,`" tokenów w *rank_specifier*.</span><span class="sxs-lookup"><span data-stu-id="8a911-118">The rank of an array type is given by the leftmost *rank_specifier* in the *array_type*: A *rank_specifier* indicates that the array is an array with a rank of one plus the number of "`,`" tokens in the *rank_specifier*.</span></span>

<span data-ttu-id="8a911-119">Typ elementu typ tablicy to typ, który powoduje usunięcie najdalej z lewej strony *rank_specifier*:</span><span class="sxs-lookup"><span data-stu-id="8a911-119">The element type of an array type is the type that results from deleting the leftmost *rank_specifier*:</span></span>

*  <span data-ttu-id="8a911-120">Typ tablicy w formie `T[R]` jest tablicą o randze `R` i typ elementu inny niż tablica `T`.</span><span class="sxs-lookup"><span data-stu-id="8a911-120">An array type of the form `T[R]` is an array with rank `R` and a non-array element type `T`.</span></span>
*  <span data-ttu-id="8a911-121">Typ tablicy w formie `T[R][R1]...[Rn]` jest tablicą o randze `R` i typ elementu `T[R1]...[Rn]`.</span><span class="sxs-lookup"><span data-stu-id="8a911-121">An array type of the form `T[R][R1]...[Rn]` is an array with rank `R` and an element type `T[R1]...[Rn]`.</span></span>

<span data-ttu-id="8a911-122">W efekcie *rank_specifier*s są odczytywane od lewej do prawej, które przed typem ostatniego elementu inny niż tablica.</span><span class="sxs-lookup"><span data-stu-id="8a911-122">In effect, the *rank_specifier*s are read from left to right before the final non-array element type.</span></span> <span data-ttu-id="8a911-123">Typ `int[][,,][,]` to Jednowymiarowa tablica tablic trójwymiarowych tablice dwuwymiarowe `int`.</span><span class="sxs-lookup"><span data-stu-id="8a911-123">The type `int[][,,][,]` is a single-dimensional array of three-dimensional arrays of two-dimensional arrays of `int`.</span></span>

<span data-ttu-id="8a911-124">W czasie wykonywania, może być wartością typu tablicowego `null` lub odwołanie do wystąpienia tego typu tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-124">At run-time, a value of an array type can be `null` or a reference to an instance of that array type.</span></span>

### <a name="the-systemarray-type"></a><span data-ttu-id="8a911-125">Typ System.Array</span><span class="sxs-lookup"><span data-stu-id="8a911-125">The System.Array type</span></span>

<span data-ttu-id="8a911-126">Typ `System.Array` jest abstrakcyjnego typy podstawowego wszystkie typy tablic.</span><span class="sxs-lookup"><span data-stu-id="8a911-126">The type `System.Array` is the abstract base type of all array types.</span></span> <span data-ttu-id="8a911-127">Niejawna konwersja odwołania ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) istnieje z dowolnym typem tablicy do `System.Array`i konwersji jawnego odwołania ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)) istnieje z `System.Array` do dowolnego typu tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-127">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from any array type to `System.Array`, and an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `System.Array` to any array type.</span></span> <span data-ttu-id="8a911-128">Należy pamiętać, że `System.Array` sama nie jest *array_type*.</span><span class="sxs-lookup"><span data-stu-id="8a911-128">Note that `System.Array` is not itself an *array_type*.</span></span> <span data-ttu-id="8a911-129">Jest to raczej *class_type* z wszystkie *array_type*pochodzą s.</span><span class="sxs-lookup"><span data-stu-id="8a911-129">Rather, it is a *class_type* from which all *array_type*s are derived.</span></span>

<span data-ttu-id="8a911-130">W czasie wykonywania, wartości typu `System.Array` może być `null` lub odwołanie do wystąpienia dowolnego typu tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-130">At run-time, a value of type `System.Array` can be `null` or a reference to an instance of any array type.</span></span>

### <a name="arrays-and-the-generic-ilist-interface"></a><span data-ttu-id="8a911-131">Tablice i ogólny interfejs IList</span><span class="sxs-lookup"><span data-stu-id="8a911-131">Arrays and the generic IList interface</span></span>

<span data-ttu-id="8a911-132">Jednowymiarowa tablica `T[]` implementuje interfejs `System.Collections.Generic.IList<T>` (`IList<T>` w skrócie) i jego interfejsy podstawowe.</span><span class="sxs-lookup"><span data-stu-id="8a911-132">A one-dimensional array `T[]` implements the interface `System.Collections.Generic.IList<T>` (`IList<T>` for short) and its base interfaces.</span></span> <span data-ttu-id="8a911-133">W związku z tym istnieje niejawna konwersja z `T[]` do `IList<T>` i jego interfejsy podstawowe.</span><span class="sxs-lookup"><span data-stu-id="8a911-133">Accordingly, there is an implicit conversion from `T[]` to `IList<T>` and its base interfaces.</span></span> <span data-ttu-id="8a911-134">Ponadto, jeśli istnieje niejawna konwersja odwołania z `S` do `T` następnie `S[]` implementuje `IList<T>` i istnieje niejawna konwersja odwołania z `S[]` do `IList<T>` i bazowej interfejsy () [Konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="8a911-134">In addition, if there is an implicit reference conversion from `S` to `T` then `S[]` implements `IList<T>` and there is an implicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="8a911-135">W przypadku jawnego odwołania konwersja `S` do `T` to konwersja jawna odwołania z `S[]` do `IList<T>` i jego interfejsy podstawowe ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="8a911-135">If there is an explicit reference conversion from `S` to `T` then there is an explicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="8a911-136">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8a911-136">For example:</span></span>
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

<span data-ttu-id="8a911-137">Przypisanie `lst2 = oa1` generuje błąd w czasie kompilacji, ponieważ konwersja z `object[]` do `IList<string>` to konwersja jawna, nie niejawne.</span><span class="sxs-lookup"><span data-stu-id="8a911-137">The assignment `lst2 = oa1` generates a compile-time error since the conversion from `object[]` to `IList<string>` is an explicit conversion, not implicit.</span></span> <span data-ttu-id="8a911-138">Rzutowanie `(IList<string>)oa1` spowoduje zgłoszenie wyjątku w czasie wykonywania, ponieważ `oa1` odwołania `object[]` i nie `string[]`.</span><span class="sxs-lookup"><span data-stu-id="8a911-138">The cast `(IList<string>)oa1` will cause an exception to be thrown at run-time since `oa1` references an `object[]` and not a `string[]`.</span></span> <span data-ttu-id="8a911-139">Jednak rzutowania `(IList<string>)oa2` nie spowoduje zgłoszenie wyjątku od `oa2` odwołania `string[]`.</span><span class="sxs-lookup"><span data-stu-id="8a911-139">However the cast `(IList<string>)oa2` will not cause an exception to be thrown since `oa2` references a `string[]`.</span></span>

<span data-ttu-id="8a911-140">Zawsze, gdy istnieje odwołanie do jawnych lub niejawnych konwersja `S[]` do `IList<T>`, jest również jawnego odwołania konwersja `IList<T>` i bazowej interfejsy do `S[]` ([jawnego odwołania Konwersje](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="8a911-140">Whenever there is an implicit or explicit reference conversion from `S[]` to `IList<T>`, there is also an explicit reference conversion from `IList<T>` and its base interfaces to `S[]` ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span>

<span data-ttu-id="8a911-141">Gdy typem tablicowym `S[]` implementuje `IList<T>`, niektórzy członkowie zaimplementowany interfejs może zgłaszać wyjątki.</span><span class="sxs-lookup"><span data-stu-id="8a911-141">When an array type `S[]` implements `IList<T>`, some of the members of the implemented interface may throw exceptions.</span></span> <span data-ttu-id="8a911-142">Dokładne zachowanie implementację interfejsu wykracza poza zakres tej specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="8a911-142">The precise behavior of the implementation of the interface is beyond the scope of this specification.</span></span>

## <a name="array-creation"></a><span data-ttu-id="8a911-143">Do utworzenia tablicy</span><span class="sxs-lookup"><span data-stu-id="8a911-143">Array creation</span></span>

<span data-ttu-id="8a911-144">Wystąpienia tablicy są tworzone przez *array_creation_expression*s ([wyrażenie tworzenia tablicy](expressions.md#array-creation-expressions)) lub według pola lub lokalne deklaracje zmiennych, które obejmują *array_initializer*([Array inicjatory](arrays.md#array-initializers)).</span><span class="sxs-lookup"><span data-stu-id="8a911-144">Array instances are created by *array_creation_expression*s ([Array creation expressions](expressions.md#array-creation-expressions)) or by field or local variable declarations that include an *array_initializer* ([Array initializers](arrays.md#array-initializers)).</span></span>

<span data-ttu-id="8a911-145">Po utworzeniu instancji tablicy Ranga i długość każdego wymiaru są ustanowione, a następnie pozostaje niezmienna przez cały okres istnienia wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="8a911-145">When an array instance is created, the rank and length of each dimension are established and then remain constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="8a911-146">Innymi słowy nie jest możliwe zmienić pozycję istniejącego wystąpienia tablicy nie jest możliwe zmienić rozmiar jej wymiarów.</span><span class="sxs-lookup"><span data-stu-id="8a911-146">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

<span data-ttu-id="8a911-147">Wystąpienie tablicy jest zawsze typu tablicowego.</span><span class="sxs-lookup"><span data-stu-id="8a911-147">An array instance is always of an array type.</span></span> <span data-ttu-id="8a911-148">`System.Array` Typ jest typem abstrakcyjnym, nie można utworzyć wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="8a911-148">The `System.Array` type is an abstract type that cannot be instantiated.</span></span>

<span data-ttu-id="8a911-149">Elementów tablic utworzonych przez *array_creation_expression*s zawsze są inicjowane na wartość domyślna ([wartości domyślne](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="8a911-149">Elements of arrays created by *array_creation_expression*s are always initialized to their default value ([Default values](variables.md#default-values)).</span></span>

## <a name="array-element-access"></a><span data-ttu-id="8a911-150">Dostęp do elementu tablicy</span><span class="sxs-lookup"><span data-stu-id="8a911-150">Array element access</span></span>

<span data-ttu-id="8a911-151">Elementy tablicy są dostępne przy użyciu *element_access* wyrażenia ([tablicy dostępu](expressions.md#array-access)) w postaci `A[I1, I2, ..., In]`, gdzie `A` to wyrażenie typu tablicowego, a każdy `Ix` jest Wyrażenie typu `int`, `uint`, `long`, `ulong`, lub można niejawnie przekonwertować do jednej lub kilku z tych typów.</span><span class="sxs-lookup"><span data-stu-id="8a911-151">Array elements are accessed using *element_access* expressions ([Array access](expressions.md#array-access)) of the form `A[I1, I2, ..., In]`, where `A` is an expression of an array type and each `Ix` is an expression of type `int`, `uint`, `long`, `ulong`, or can be implicitly converted to one or more of these types.</span></span> <span data-ttu-id="8a911-152">Wynik dostęp do elementu tablicy jest zmienną, a mianowicie elementu tablicy wybranych przez wskaźniki.</span><span class="sxs-lookup"><span data-stu-id="8a911-152">The result of an array element access is a variable, namely the array element selected by the indices.</span></span>

<span data-ttu-id="8a911-153">Elementy tablicy mogą być wyliczane przy użyciu `foreach` — instrukcja ([Instrukcja foreach](statements.md#the-foreach-statement)).</span><span class="sxs-lookup"><span data-stu-id="8a911-153">The elements of an array can be enumerated using a `foreach` statement ([The foreach statement](statements.md#the-foreach-statement)).</span></span>

## <a name="array-members"></a><span data-ttu-id="8a911-154">Elementy członkowskie tablicy</span><span class="sxs-lookup"><span data-stu-id="8a911-154">Array members</span></span>

<span data-ttu-id="8a911-155">Każdy typ tablicy dziedziczy elementów członkowskich zadeklarowanych za `System.Array` typu.</span><span class="sxs-lookup"><span data-stu-id="8a911-155">Every array type inherits the members declared by the `System.Array` type.</span></span>

## <a name="array-covariance"></a><span data-ttu-id="8a911-156">Kowariancja tablicy</span><span class="sxs-lookup"><span data-stu-id="8a911-156">Array covariance</span></span>

<span data-ttu-id="8a911-157">Dla dowolnych dwóch *reference_type*s `A` i `B`, jeśli niejawna konwersja odwołania ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) lub konwersji jawnej odwołania ([ Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)) istnieje z `A` do `B`, istnieje również tej samej konwersji odwołanie z typu tablicy, a następnie `A[R]` na typ array `B[R]`, gdzie `R` jest dowolnym dany *rank_specifier* (ale tablicy takie same dla obu typów).</span><span class="sxs-lookup"><span data-stu-id="8a911-157">For any two *reference_type*s `A` and `B`, if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `A` to `B`, then the same reference conversion also exists from the array type `A[R]` to the array type `B[R]`, where `R` is any given *rank_specifier* (but the same for both array types).</span></span> <span data-ttu-id="8a911-158">Ta relacja jest znany jako ***Kowariancja tablicy***.</span><span class="sxs-lookup"><span data-stu-id="8a911-158">This relationship is known as ***array covariance***.</span></span> <span data-ttu-id="8a911-159">Kowariancja tablicy w szczególności oznacza, że wartość typu tablicowego `A[R]` faktycznie może być odwołaniem do wystąpienia typu tablicowego `B[R]`, o ile istnieje niejawna konwersja odwołania `B` do `A`.</span><span class="sxs-lookup"><span data-stu-id="8a911-159">Array covariance in particular means that a value of an array type `A[R]` may actually be a reference to an instance of an array type `B[R]`, provided an implicit reference conversion exists from `B` to `A`.</span></span>

<span data-ttu-id="8a911-160">Ze względu na Kowariancja tablicy przypisania do elementów tablicami typu odwołania zawierają sprawdzanie w czasie wykonania, która gwarantuje, że wartość jest przypisana do elementu tablicy jest faktycznie typu dozwolonych ([przypisanie proste](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="8a911-160">Because of array covariance, assignments to elements of reference type arrays include a run-time check which ensures that the value being assigned to the array element is actually of a permitted type ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="8a911-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8a911-161">For example:</span></span>
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

<span data-ttu-id="8a911-162">Przypisanie do `array[i]` w `Fill` metoda obejmuje sprawdzanie w czasie wykonywania, który zapewnia, że obiekt odwołuje się `value` jest `null` lub wystąpienie, który jest zgodny z typem elementu rzeczywiste `array`.</span><span class="sxs-lookup"><span data-stu-id="8a911-162">The assignment to `array[i]` in the `Fill` method implicitly includes a run-time check which ensures that the object referenced by `value` is either `null` or an instance that is compatible with the actual element type of `array`.</span></span> <span data-ttu-id="8a911-163">W `Main`, dwa pierwsze wywołania `Fill` powiedzie się, ale trzecie powoduje wywołanie `System.ArrayTypeMismatchException` zostanie wygenerowany po pierwsze przypisanie do wykonywania `array[i]`.</span><span class="sxs-lookup"><span data-stu-id="8a911-163">In `Main`, the first two invocations of `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` to be thrown upon executing the first assignment to `array[i]`.</span></span> <span data-ttu-id="8a911-164">Wyjątek występuje, ponieważ spakowany `int` nie mogą być przechowywane `string` tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-164">The exception occurs because a boxed `int` cannot be stored in a `string` array.</span></span>

<span data-ttu-id="8a911-165">Kowariancja tablicy specjalnie nie obejmuje tablic *value_type*s.</span><span class="sxs-lookup"><span data-stu-id="8a911-165">Array covariance specifically does not extend to arrays of *value_type*s.</span></span> <span data-ttu-id="8a911-166">Na przykład, nie istnieje Konwersja tego zezwala `int[]` powinien być traktowany jako `object[]`.</span><span class="sxs-lookup"><span data-stu-id="8a911-166">For example, no conversion exists that permits an `int[]` to be treated as an `object[]`.</span></span>

## <a name="array-initializers"></a><span data-ttu-id="8a911-167">Inicjatory tablic</span><span class="sxs-lookup"><span data-stu-id="8a911-167">Array initializers</span></span>

<span data-ttu-id="8a911-168">Inicjatora tablicy mogą być określone w deklaracji pól ([pola](classes.md#fields)), deklaracje zmiennych lokalnych ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)) i wyrażenie tworzenia tablicy ([tworzenia tablicy wyrażenia](expressions.md#array-creation-expressions)):</span><span class="sxs-lookup"><span data-stu-id="8a911-168">Array initializers may be specified in field declarations ([Fields](classes.md#fields)), local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), and array creation expressions ([Array creation expressions](expressions.md#array-creation-expressions)):</span></span>

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="8a911-169">Inicjatora tablicy składa się z sekwencji zmiennej inicjatory, ujęta w "`{`"i"`}`"tokenów i oddzielone"`,`" tokenów.</span><span class="sxs-lookup"><span data-stu-id="8a911-169">An array initializer consists of a sequence of variable initializers, enclosed by "`{`" and "`}`" tokens and separated by "`,`" tokens.</span></span> <span data-ttu-id="8a911-170">Każdy inicjatorze zmiennych jest wyrażeniem lub, w przypadku tablicy wielowymiarowej zagnieżdżonego inicjatora tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-170">Each variable initializer is an expression or, in the case of a multi-dimensional array, a nested array initializer.</span></span>

<span data-ttu-id="8a911-171">Kontekst, w którym jest używany Inicjator tablicy Określa typ tablicy, inicjowany.</span><span class="sxs-lookup"><span data-stu-id="8a911-171">The context in which an array initializer is used determines the type of the array being initialized.</span></span> <span data-ttu-id="8a911-172">W wyrażenie tworzenia tablicy typu tablicy bezpośrednio poprzedza inicjatora lub wynika z wyrażenia w inicjatorze tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-172">In an array creation expression, the array type immediately precedes the initializer, or is inferred from the expressions in the array initializer.</span></span> <span data-ttu-id="8a911-173">W polu lub deklaracja zmiennej typ tablicy jest typu pola lub zmiennej deklarowanej.</span><span class="sxs-lookup"><span data-stu-id="8a911-173">In a field or variable declaration, the array type is the type of the field or variable being declared.</span></span> <span data-ttu-id="8a911-174">Gdy inicjatora tablicy jest używany w pole lub deklaracji zmiennych, takich jak:</span><span class="sxs-lookup"><span data-stu-id="8a911-174">When an array initializer is used in a field or variable declaration, such as:</span></span>
```csharp
int[] a = {0, 2, 4, 6, 8};
```
<span data-ttu-id="8a911-175">jest po prostu skrótem wyrażenie tworzenia tablicy równoważne:</span><span class="sxs-lookup"><span data-stu-id="8a911-175">it is simply shorthand for an equivalent array creation expression:</span></span>
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

<span data-ttu-id="8a911-176">Dla tablicy jednowymiarowej inicjatora tablicy musi składać się z sekwencji wyrażeń, które są przypisania zgodny z typem elementu tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-176">For a single-dimensional array, the array initializer must consist of a sequence of expressions that are assignment compatible with the element type of the array.</span></span> <span data-ttu-id="8a911-177">Wyrażenia zainicjować elementy tablicy w rosnącej kolejności, poczynając od elementu o indeksie zero.</span><span class="sxs-lookup"><span data-stu-id="8a911-177">The expressions initialize array elements in increasing order, starting with the element at index zero.</span></span> <span data-ttu-id="8a911-178">Liczba wyrażeń w inicjatorze tablicy określa długość tworzenia instancji tabeli.</span><span class="sxs-lookup"><span data-stu-id="8a911-178">The number of expressions in the array initializer determines the length of the array instance being created.</span></span> <span data-ttu-id="8a911-179">Na przykład tworzy powyżej inicjatora tablicy `int[]` wystąpienia o długości 5 i następnie inicjuje wystąpienie z następującymi wartościami:</span><span class="sxs-lookup"><span data-stu-id="8a911-179">For example, the array initializer above creates an `int[]` instance of length 5 and then initializes the instance with the following values:</span></span>
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

<span data-ttu-id="8a911-180">Wielowymiarowe tablice inicjatora tablicy musi mieć dowolną liczbę poziomów zagnieżdżania, ponieważ wymiary tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-180">For a multi-dimensional array, the array initializer must have as many levels of nesting as there are dimensions in the array.</span></span> <span data-ttu-id="8a911-181">Najbardziej zewnętrznej poziom zagnieżdżenia odnosi się do wymiaru skrajnie po lewej stronie i najbardziej poziom zagnieżdżenia odnosi się do wymiaru po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="8a911-181">The outermost nesting level corresponds to the leftmost dimension and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="8a911-182">Długość każdego wymiaru tablicy jest określana przez liczbę elementów znajdujących się na odpowiedni poziom zagnieżdżenia w inicjatorze tablicy.</span><span class="sxs-lookup"><span data-stu-id="8a911-182">The length of each dimension of the array is determined by the number of elements at the corresponding nesting level in the array initializer.</span></span> <span data-ttu-id="8a911-183">Dla każdego zagnieżdżonego inicjatora tablicy liczba elementów musi być taka sama jak innych inicjatora tablicy na tym samym poziomie.</span><span class="sxs-lookup"><span data-stu-id="8a911-183">For each nested array initializer, the number of elements must be the same as the other array initializers at the same level.</span></span> <span data-ttu-id="8a911-184">Przykład:</span><span class="sxs-lookup"><span data-stu-id="8a911-184">The example:</span></span>
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
<span data-ttu-id="8a911-185">tworzy tablicę dwuwymiarową o długości do 5 dla wymiaru skrajnie po lewej stronie i o długości dwóch dla wymiaru po prawej stronie:</span><span class="sxs-lookup"><span data-stu-id="8a911-185">creates a two-dimensional array with a length of five for the leftmost dimension and a length of two for the rightmost dimension:</span></span>
```csharp
int[,] b = new int[5, 2];
```
<span data-ttu-id="8a911-186">a następnie Inicjuje tablicę wystąpień z następującymi wartościami:</span><span class="sxs-lookup"><span data-stu-id="8a911-186">and then initializes the array instance with the following values:</span></span>
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

<span data-ttu-id="8a911-187">Jeśli wymiar innego niż po prawej stronie podano o zerowej długości, kolejne wymiary są zakłada się, że również mieć długości zerowej.</span><span class="sxs-lookup"><span data-stu-id="8a911-187">If a dimension other than the rightmost is given with length zero, the subsequent dimensions are assumed to also have length zero.</span></span> <span data-ttu-id="8a911-188">Przykład:</span><span class="sxs-lookup"><span data-stu-id="8a911-188">The example:</span></span>
```csharp
int[,] c = {};
```
<span data-ttu-id="8a911-189">Tworzy dwuwymiarową tablicę o długości zerowej najdalej z lewej strony i po prawej stronie wymiaru:</span><span class="sxs-lookup"><span data-stu-id="8a911-189">creates a two-dimensional array with a length of zero for both the leftmost and the rightmost dimension:</span></span>
```csharp
int[,] c = new int[0, 0];
```

<span data-ttu-id="8a911-190">Kiedy wyrażenie tworzenia tablicy obejmuje zarówno długości wymiarów jawne, jak i inicjatora tablicy, długości muszą być wyrażeniami stałymi, a liczba elementów na każdym poziomie zagnieżdżania, które musi odpowiadać odpowiedniego długość wymiaru.</span><span class="sxs-lookup"><span data-stu-id="8a911-190">When an array creation expression includes both explicit dimension lengths and an array initializer, the lengths must be constant expressions and the number of elements at each nesting level must match the corresponding dimension length.</span></span> <span data-ttu-id="8a911-191">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="8a911-191">Here are some examples:</span></span>
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

<span data-ttu-id="8a911-192">Tutaj, inicjator dla `y` powoduje błąd w czasie kompilacji, ponieważ wyrażenie długości wymiarów nie jest stałą, a inicjator dla `z` powoduje błąd w czasie kompilacji, ponieważ długość i liczba elementów w Inicjator nie są zgodne.</span><span class="sxs-lookup"><span data-stu-id="8a911-192">Here, the initializer for `y` results in a compile-time error because the dimension length expression is not a constant, and the initializer for `z` results in a compile-time error because the length and the number of elements in the initializer do not agree.</span></span>
