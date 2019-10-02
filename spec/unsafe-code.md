---
ms.openlocfilehash: 4faef9a12bdff54fa59a55a0206fa72bda4ea585
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704063"
---
# <a name="unsafe-code"></a><span data-ttu-id="55f40-101">Niebezpieczny kod</span><span class="sxs-lookup"><span data-stu-id="55f40-101">Unsafe code</span></span>

<span data-ttu-id="55f40-102">Język podstawowy C# , zgodnie z definicją w poprzednich działach, różni się w szczególności od C++ C i w jego pominięciu jako typu danych.</span><span class="sxs-lookup"><span data-stu-id="55f40-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="55f40-103">Zamiast tego C# zawiera odwołania i możliwość tworzenia obiektów zarządzanych przez moduł wyrzucania elementów bezużytecznych.</span><span class="sxs-lookup"><span data-stu-id="55f40-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="55f40-104">Ten projekt, w połączeniu z innymi funkcjami, sprawia C# , że jest to bardzo bezpieczniejszy język C++niż C lub.</span><span class="sxs-lookup"><span data-stu-id="55f40-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="55f40-105">W języku podstawowym C# po prostu nie jest możliwe posiadanie niezainicjowanej zmiennej, wskaźnika "zawieszonego" lub wyrażenia, które indeksuje tablicę poza jej granicami.</span><span class="sxs-lookup"><span data-stu-id="55f40-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="55f40-106">Całe kategorie błędów, które rutynowo Plague C i C++ programy są w ten sposób eliminowane.</span><span class="sxs-lookup"><span data-stu-id="55f40-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="55f40-107">Praktycznie każda konstrukcja typu wskaźnika w języku C lub C++ ma odpowiednik typu referencyjnego w C#, jednak istnieją sytuacje, w których dostęp do typów wskaźnika jest konieczny.</span><span class="sxs-lookup"><span data-stu-id="55f40-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="55f40-108">Na przykład współdziałanie z podstawowym systemem operacyjnym, uzyskiwanie dostępu do urządzenia mapowanego na pamięć lub implementowanie algorytmu krytycznego czasowo może nie być możliwe ani praktyczne bez dostępu do wskaźników.</span><span class="sxs-lookup"><span data-stu-id="55f40-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="55f40-109">Aby rozwiązać ten konieczność C# , program umożliwia zapisanie ***niebezpiecznego kodu***.</span><span class="sxs-lookup"><span data-stu-id="55f40-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="55f40-110">W niebezpiecznym kodzie można zadeklarować i obsłużyć wskaźniki, aby wykonać konwersje między wskaźnikami i typami całkowitymi, aby przyjąć adres zmiennych i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="55f40-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="55f40-111">W sensie pisanie niebezpiecznego kodu jest podobne do pisania kodu C w C# ramach programu.</span><span class="sxs-lookup"><span data-stu-id="55f40-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="55f40-112">Niebezpieczny kod jest w rzeczywistości funkcją "bezpieczna" z perspektywy deweloperów i użytkowników.</span><span class="sxs-lookup"><span data-stu-id="55f40-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="55f40-113">Niebezpieczny kod musi być wyraźnie oznaczony modyfikatorem `unsafe`, dlatego deweloperzy nie mogą przypadkowo korzystać z niebezpiecznych funkcji, a aparat wykonywania działa w celu zapewnienia, że nie można wykonać niebezpiecznego kodu w niezaufanym środowisku.</span><span class="sxs-lookup"><span data-stu-id="55f40-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="55f40-114">Niebezpieczne konteksty</span><span class="sxs-lookup"><span data-stu-id="55f40-114">Unsafe contexts</span></span>

<span data-ttu-id="55f40-115">Niebezpieczne funkcje programu C# są dostępne tylko w niebezpiecznych kontekstach.</span><span class="sxs-lookup"><span data-stu-id="55f40-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="55f40-116">Niebezpieczny kontekst jest wprowadzany przez dołączenie modyfikatora `unsafe` w deklaracji typu lub składowej lub przez zastosowanie *unsafe_statement*:</span><span class="sxs-lookup"><span data-stu-id="55f40-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="55f40-117">Deklaracja klasy, struktury, interfejsu lub delegata może zawierać modyfikator `unsafe`. w takim przypadku cały tekstowy zakres tej deklaracji typu (w tym treść klasy, struktury lub interfejsu) jest traktowany jako niebezpieczny kontekst.</span><span class="sxs-lookup"><span data-stu-id="55f40-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="55f40-118">Deklaracja pola, metody, właściwości, zdarzenia, indeksatora, operatora, konstruktora wystąpienia, destruktora lub konstruktora statycznego może zawierać modyfikator `unsafe`. w takim przypadku cały tekstowy zakres tej deklaracji składowej jest traktowany jako niebezpieczny kontekst.</span><span class="sxs-lookup"><span data-stu-id="55f40-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="55f40-119">*Unsafe_statement* umożliwia użycie niebezpiecznego kontekstu w *bloku*.</span><span class="sxs-lookup"><span data-stu-id="55f40-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="55f40-120">Cały tekstowy zakres skojarzonego *bloku* jest traktowany jako niebezpieczny kontekst.</span><span class="sxs-lookup"><span data-stu-id="55f40-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="55f40-121">Skojarzone produkcyjne gramatyki są pokazane poniżej.</span><span class="sxs-lookup"><span data-stu-id="55f40-121">The associated grammar productions are shown below.</span></span>

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

<span data-ttu-id="55f40-122">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="55f40-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="55f40-123">modyfikator `unsafe` określony w deklaracji struktury powoduje, że cały tekst zakresu deklaracji struktury staje się niebezpiecznym kontekstem.</span><span class="sxs-lookup"><span data-stu-id="55f40-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="55f40-124">W ten sposób można zadeklarować pola `Left` i `Right` jako typu wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="55f40-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="55f40-125">Powyższy przykład może być również zapisany</span><span class="sxs-lookup"><span data-stu-id="55f40-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="55f40-126">W tym miejscu Modyfikatory `unsafe` w deklaracjach pola powodują, że te deklaracje są uznawane za niebezpieczne konteksty.</span><span class="sxs-lookup"><span data-stu-id="55f40-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="55f40-127">Oprócz ustanowienia niebezpiecznego kontekstu, co pozwala na korzystanie z typów wskaźników, modyfikator `unsafe` nie ma wpływu na typ lub element członkowski.</span><span class="sxs-lookup"><span data-stu-id="55f40-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="55f40-128">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="55f40-128">In the example</span></span>

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

<span data-ttu-id="55f40-129">modyfikator `unsafe` w metodzie `F` w `A` po prostu powoduje, że zakres tekstu `F` stanie się niebezpiecznym kontekstem, w którym można używać niebezpiecznych funkcji języka.</span><span class="sxs-lookup"><span data-stu-id="55f40-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="55f40-130">W przypadku przesłonięcia `F` w `B` nie trzeba ponownie określać modyfikatora `unsafe` — chyba że metoda `F` w `B` nie potrzebuje dostępu do funkcji niebezpiecznych.</span><span class="sxs-lookup"><span data-stu-id="55f40-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="55f40-131">Sytuacja jest nieco inna, gdy typ wskaźnika jest częścią podpisu metody</span><span class="sxs-lookup"><span data-stu-id="55f40-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

<span data-ttu-id="55f40-132">Tutaj, ponieważ podpis `F` zawiera typ wskaźnika, może być zapisany tylko w niebezpiecznym kontekście.</span><span class="sxs-lookup"><span data-stu-id="55f40-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="55f40-133">Niebezpieczny kontekst można jednak wprowadzić poprzez uczynienie całej klasy jako niebezpiecznej, tak jak w przypadku `A` lub poprzez dołączenie modyfikatora `unsafe` w deklaracji metody, tak jak w przypadku `B`.</span><span class="sxs-lookup"><span data-stu-id="55f40-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="55f40-134">Typy wskaźnika</span><span class="sxs-lookup"><span data-stu-id="55f40-134">Pointer types</span></span>

<span data-ttu-id="55f40-135">W niebezpiecznym kontekście *Typ* ([typy](types.md)) może być *pointer_type* , a także *value_type* lub *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="55f40-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="55f40-136">Jednak *pointer_type* może być również używany w wyrażeniu `typeof` ([wyrażenia anonimowego tworzenia obiektów](expressions.md#anonymous-object-creation-expressions)) poza niebezpiecznym kontekstem, ponieważ takie użycie nie jest niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="55f40-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="55f40-137">*Pointer_type* jest zapisywana jako *unmanaged_type* lub słowo kluczowe `void`, po którym następuje token `*`:</span><span class="sxs-lookup"><span data-stu-id="55f40-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="55f40-138">Typ określony przed `*` w typie wskaźnika jest nazywany ***typem referent*** typu wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="55f40-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="55f40-139">Reprezentuje typ zmiennej, do której należy wartość typu wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="55f40-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="55f40-140">W przeciwieństwie do odwołań (wartości typów odwołań), wskaźniki nie są śledzone przez moduł wyrzucania elementów bezużytecznych — Moduł wyrzucania elementów bezużytecznych nie ma wiedzy na temat wskaźników i danych, do których one wskazują.</span><span class="sxs-lookup"><span data-stu-id="55f40-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="55f40-141">Z tego powodu wskaźnik nie może wskazywać na odwołanie lub do struktury, która zawiera odwołania, a typem referent wskaźnika musi być *unmanaged_type*.</span><span class="sxs-lookup"><span data-stu-id="55f40-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="55f40-142">*Unmanaged_type* jest dowolnego typu, który nie jest typu *reference_type* ani skonstruowany, i nie zawiera pól *reference_type* lub skonstruowany typ na dowolnym poziomie zagnieżdżenia.</span><span class="sxs-lookup"><span data-stu-id="55f40-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="55f40-143">Innymi słowy, *unmanaged_type* jest jednym z następujących:</span><span class="sxs-lookup"><span data-stu-id="55f40-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="55f40-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0, 1 lub 2.</span><span class="sxs-lookup"><span data-stu-id="55f40-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="55f40-145">Dowolny *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="55f40-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="55f40-146">Dowolny *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="55f40-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="55f40-147">Każdy zdefiniowany przez użytkownika *struct_type* , który nie jest typem skonstruowanym i zawiera pola tylko *unmanaged_type*.</span><span class="sxs-lookup"><span data-stu-id="55f40-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="55f40-148">Intuicyjna reguła do mieszania wskaźników i odwołań polega na tym, że referents odwołań (obiekty) mogą zawierać wskaźniki, ale referents wskaźników nie może zawierać odwołań.</span><span class="sxs-lookup"><span data-stu-id="55f40-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="55f40-149">Niektóre przykłady typów wskaźników są podane w poniższej tabeli:</span><span class="sxs-lookup"><span data-stu-id="55f40-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="55f40-150">__Przykład__</span><span class="sxs-lookup"><span data-stu-id="55f40-150">__Example__</span></span> | <span data-ttu-id="55f40-151">__Opis__</span><span class="sxs-lookup"><span data-stu-id="55f40-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="55f40-152">Wskaźnik do `byte`</span><span class="sxs-lookup"><span data-stu-id="55f40-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="55f40-153">Wskaźnik do `char`</span><span class="sxs-lookup"><span data-stu-id="55f40-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="55f40-154">Wskaźnik do wskaźnika do `int`</span><span class="sxs-lookup"><span data-stu-id="55f40-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="55f40-155">Jednowymiarowa tablica wskaźników do `int`</span><span class="sxs-lookup"><span data-stu-id="55f40-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="55f40-156">Wskaźnik do nieznanego typu</span><span class="sxs-lookup"><span data-stu-id="55f40-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="55f40-157">Dla danej implementacji wszystkie typy wskaźnika muszą mieć taki sam rozmiar i reprezentację.</span><span class="sxs-lookup"><span data-stu-id="55f40-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="55f40-158">W przeciwieństwie do C++języka C i, gdy wiele wskaźników jest zadeklarowanych w tej C# samej deklaracji, w `*` jest zapisywana tylko z typem podstawowym, a nie jako prefiks punctuator na każdej nazwie wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="55f40-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="55f40-159">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="55f40-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="55f40-160">Wartość wskaźnika typu `T*` reprezentuje adres zmiennej typu `T`.</span><span class="sxs-lookup"><span data-stu-id="55f40-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="55f40-161">Operator pośredni wskaźnika `*` ([pośredni wskaźnik](unsafe-code.md#pointer-indirection)) może być używany w celu uzyskania dostępu do tej zmiennej.</span><span class="sxs-lookup"><span data-stu-id="55f40-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="55f40-162">Na przykład w przypadku zmiennej `P` typu `int*` wyrażenie `*P` oznacza zmienną `int` znalezioną pod adresem zawartym w `P`.</span><span class="sxs-lookup"><span data-stu-id="55f40-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="55f40-163">Podobnie jak odwołanie do obiektu, wskaźnik może być `null`.</span><span class="sxs-lookup"><span data-stu-id="55f40-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="55f40-164">Zastosowanie operatora pośredniego do wskaźnika `null` skutkuje zachowaniem zdefiniowanym przez implementację.</span><span class="sxs-lookup"><span data-stu-id="55f40-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="55f40-165">Wskaźnik o wartości `null` jest reprezentowany przez wszystkie bity-zero.</span><span class="sxs-lookup"><span data-stu-id="55f40-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="55f40-166">Typ `void*` reprezentuje wskaźnik do nieznanego typu.</span><span class="sxs-lookup"><span data-stu-id="55f40-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="55f40-167">Ponieważ typ referent jest nieznany, operator pośredni nie może zostać zastosowany do wskaźnika typu `void*`, ani nie można wykonać operacji arytmetycznych na tym wskaźniku.</span><span class="sxs-lookup"><span data-stu-id="55f40-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="55f40-168">Jednak wskaźnik typu `void*` może być rzutowany do dowolnego innego typu wskaźnika (i odwrotnie).</span><span class="sxs-lookup"><span data-stu-id="55f40-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="55f40-169">Typy wskaźnika są oddzielną kategorią typów.</span><span class="sxs-lookup"><span data-stu-id="55f40-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="55f40-170">W przeciwieństwie do typów referencyjnych i typów wartości, typy wskaźnika nie dziedziczą po `object` i nie istnieją konwersje między typami wskaźnika i `object`.</span><span class="sxs-lookup"><span data-stu-id="55f40-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="55f40-171">W szczególności opakowanie i rozpakowywanie ([opakowanie i](types.md#boxing-and-unboxing)rozpakowywanie) nie jest obsługiwane w przypadku wskaźników.</span><span class="sxs-lookup"><span data-stu-id="55f40-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="55f40-172">Jednak konwersje są dozwolone między różnymi typami wskaźników i między typami wskaźnika a typami całkowitymi.</span><span class="sxs-lookup"><span data-stu-id="55f40-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="55f40-173">Jest to opisane w temacie [konwersje wskaźnika](unsafe-code.md#pointer-conversions).</span><span class="sxs-lookup"><span data-stu-id="55f40-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="55f40-174">Nie można użyć elementu *pointer_type* jako argumentu typu ([konstruowane typy](types.md#constructed-types)) i wnioskowania typu ([wnioskowanie o typie](expressions.md#type-inference)) nie powiodło się dla wywołań metod ogólnych, które zostałyby wywnioskowane argumentu typu jako typu wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="55f40-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="55f40-175">*Pointer_type* może być używany jako typ pola nietrwałego ([pola nietrwałe](classes.md#volatile-fields)).</span><span class="sxs-lookup"><span data-stu-id="55f40-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="55f40-176">Chociaż wskaźniki mogą być przesyłane jako parametry `ref` lub `out`, wykonanie tej operacji może spowodować niezdefiniowane zachowanie, ponieważ wskaźnik może być również ustawiony jako zmienna lokalna, która już nie istnieje, gdy wywołana metoda zwróci wartość lub obiektem stałym, do którego użył , nie jest już naprawiony.</span><span class="sxs-lookup"><span data-stu-id="55f40-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="55f40-177">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="55f40-177">For example:</span></span>

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

<span data-ttu-id="55f40-178">Metoda może zwracać wartość pewnego typu, a ten typ może być wskaźnikiem.</span><span class="sxs-lookup"><span data-stu-id="55f40-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="55f40-179">Na przykład, gdy nastąpiło wskaźnik do ciągłej sekwencji @no__t 0s, liczba elementów tej sekwencji i inna wartość `int`, następująca metoda zwraca adres tej wartości w tej sekwencji, jeśli występuje dopasowanie; w przeciwnym razie zwraca `null`:</span><span class="sxs-lookup"><span data-stu-id="55f40-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

<span data-ttu-id="55f40-180">W niebezpiecznym kontekście kilka konstrukcji jest dostępnych do obsługi wskaźników:</span><span class="sxs-lookup"><span data-stu-id="55f40-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="55f40-181">Operator `*` może służyć do wykonywania pośredniego wskaźnika ([pośredni wskaźnik](unsafe-code.md#pointer-indirection)).</span><span class="sxs-lookup"><span data-stu-id="55f40-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="55f40-182">Operator `->` może być używany w celu uzyskania dostępu do elementu członkowskiego struktury za pomocą wskaźnika ([dostęp do elementu członkowskiego wskaźnika](unsafe-code.md#pointer-member-access)).</span><span class="sxs-lookup"><span data-stu-id="55f40-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="55f40-183">Operator `[]` może służyć do indeksowania wskaźnika ([dostęp do elementu wskaźnika](unsafe-code.md#pointer-element-access)).</span><span class="sxs-lookup"><span data-stu-id="55f40-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="55f40-184">Operator `&` może służyć do uzyskania adresu zmiennej ([operator adresu](unsafe-code.md#the-address-of-operator)).</span><span class="sxs-lookup"><span data-stu-id="55f40-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="55f40-185">Operatory `++` i `--` mogą służyć do zwiększania i zmniejszania wskaźników ([przyrostu i zmniejszania wskaźnika](unsafe-code.md#pointer-increment-and-decrement)).</span><span class="sxs-lookup"><span data-stu-id="55f40-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="55f40-186">Operatory `+` i `-` mogą służyć do wykonywania operacji arytmetycznych wskaźnika ([arytmetyki wskaźnika](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="55f40-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="55f40-187">Operatory `==`, `!=`, `<`, `>`, `<=` i `=>` umożliwiają porównanie wskaźników ([porównanie wskaźników](unsafe-code.md#pointer-comparison)).</span><span class="sxs-lookup"><span data-stu-id="55f40-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="55f40-188">Operatora `stackalloc` można użyć do przydzielenia pamięci ze stosu wywołań ([bufory o ustalonym rozmiarze](unsafe-code.md#fixed-size-buffers)).</span><span class="sxs-lookup"><span data-stu-id="55f40-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="55f40-189">Instrukcji `fixed` można użyć do tymczasowej naprawy zmiennej, aby można było uzyskać jej adres ([ustalona instrukcja](unsafe-code.md#the-fixed-statement)).</span><span class="sxs-lookup"><span data-stu-id="55f40-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="55f40-190">Zmienne stałe i ruchome</span><span class="sxs-lookup"><span data-stu-id="55f40-190">Fixed and moveable variables</span></span>

<span data-ttu-id="55f40-191">Operator address-of ([operator adresu](unsafe-code.md#the-address-of-operator)) i instrukcja `fixed` ([stała instrukcja](unsafe-code.md#the-fixed-statement)) dzielą zmienne na dwie kategorie: ***Stałe zmienne*** i ***ruchome zmienne***.</span><span class="sxs-lookup"><span data-stu-id="55f40-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="55f40-192">Stałe zmienne znajdują się w lokalizacjach magazynu, które nie mają wpływ na działanie modułu wyrzucania elementów bezużytecznych.</span><span class="sxs-lookup"><span data-stu-id="55f40-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="55f40-193">(Przykładowe zmienne stałe obejmują zmienne lokalne, parametry wartości i zmienne utworzone przez wyłuskanie wskaźników). Z drugiej strony, ruchome zmienne znajdują się w lokalizacjach przechowywania, które podlegają relokacji lub usuwaniu przez moduł wyrzucania elementów bezużytecznych.</span><span class="sxs-lookup"><span data-stu-id="55f40-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="55f40-194">(Przykłady ruchomych zmiennych zawierają pola w obiektach i elementy tablic).</span><span class="sxs-lookup"><span data-stu-id="55f40-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="55f40-195">Operator `&` ([operator address-of](unsafe-code.md#the-address-of-operator)) zezwala na uzyskanie adresu stałej zmiennej bez ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="55f40-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="55f40-196">Jednak ze względu na to, że przyruchoma zmienna podlega relokacji lub utylizacji przez moduł wyrzucania elementów bezużytecznych, adres ruchomej zmiennej można uzyskać tylko przy użyciu instrukcji `fixed` ([stała instrukcja](unsafe-code.md#the-fixed-statement)) i ten adres pozostaje prawidłowy tylko dla czas trwania tej instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="55f40-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="55f40-197">W dokładnych warunkach Zmienna stała jest jedną z następujących:</span><span class="sxs-lookup"><span data-stu-id="55f40-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="55f40-198">Zmienna będąca wynikiem *simple_name* ([proste nazwy](expressions.md#simple-names)), która odwołuje się do zmiennej lokalnej lub parametru wartości, chyba że zmienna jest przechwytywana przez funkcję anonimową.</span><span class="sxs-lookup"><span data-stu-id="55f40-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="55f40-199">Zmienna będąca wynikiem *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `V.I`, gdzie `V` to stała zmienna elementu *struct_type*.</span><span class="sxs-lookup"><span data-stu-id="55f40-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="55f40-200">Zmienna będąca wynikiem *pointer_indirection_expression* ([pośredni wskaźnik](unsafe-code.md#pointer-indirection)) formularza `*P`, *pointer_member_access* ([dostęp do elementu członkowskiego wskaźnika](unsafe-code.md#pointer-member-access)) formularza `P->I` lub *pointer_element_access* ( [Dostęp do elementu wskaźnika](unsafe-code.md#pointer-element-access)) formularza `P[E]`.</span><span class="sxs-lookup"><span data-stu-id="55f40-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="55f40-201">Wszystkie inne zmienne są klasyfikowane jako ruchome zmienne.</span><span class="sxs-lookup"><span data-stu-id="55f40-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="55f40-202">Należy zauważyć, że pole statyczne jest sklasyfikowane jako ruchoma zmienna.</span><span class="sxs-lookup"><span data-stu-id="55f40-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="55f40-203">Należy również zauważyć, że parametr `ref` lub `out` jest klasyfikowany jako ruchoma zmienna, nawet jeśli argument określony dla parametru jest zmienną stałą.</span><span class="sxs-lookup"><span data-stu-id="55f40-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="55f40-204">Na koniec należy zauważyć, że zmienna utworzona przez odwołujący się do wskaźnika jest zawsze sklasyfikowana jako stała zmienna.</span><span class="sxs-lookup"><span data-stu-id="55f40-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="55f40-205">Konwersje wskaźników</span><span class="sxs-lookup"><span data-stu-id="55f40-205">Pointer conversions</span></span>

<span data-ttu-id="55f40-206">W niebezpiecznym kontekście zestaw dostępnych konwersji niejawnych ([konwersje niejawne](conversions.md#implicit-conversions)) jest rozszerzony w celu uwzględnienia następujących niejawnych konwersji wskaźnika:</span><span class="sxs-lookup"><span data-stu-id="55f40-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="55f40-207">Z dowolnych *pointer_type* do typu `void*`.</span><span class="sxs-lookup"><span data-stu-id="55f40-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="55f40-208">Z literału `null` do dowolnych *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="55f40-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="55f40-209">Ponadto w przypadku niebezpiecznego kontekstu zestaw dostępnych konwersji jawnych ([Konwersje jawne](conversions.md#explicit-conversions)) jest rozszerzany w celu uwzględnienia następujących jawnych konwersji wskaźnika:</span><span class="sxs-lookup"><span data-stu-id="55f40-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="55f40-210">Z dowolnych *pointer_type* do innych *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="55f40-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="55f40-211">Z `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` lub `ulong` do dowolnych *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="55f40-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="55f40-212">Z dowolnego *pointer_type* do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` lub `ulong`.</span><span class="sxs-lookup"><span data-stu-id="55f40-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="55f40-213">Na koniec w niebezpiecznym kontekście zestaw standardowych konwersji niejawnych ([konwersje niejawne standardowe](conversions.md#standard-implicit-conversions)) zawiera następującą konwersję wskaźnika:</span><span class="sxs-lookup"><span data-stu-id="55f40-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="55f40-214">Z dowolnych *pointer_type* do typu `void*`.</span><span class="sxs-lookup"><span data-stu-id="55f40-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="55f40-215">Konwersje między dwoma typami wskaźnika nigdy nie zmieniają rzeczywistej wartości wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="55f40-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="55f40-216">Inaczej mówiąc, konwersja z jednego typu wskaźnika do innego nie ma wpływu na adres źródłowy określony przez wskaźnik.</span><span class="sxs-lookup"><span data-stu-id="55f40-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="55f40-217">Gdy jeden typ wskaźnika jest konwertowany na inny, jeśli wynikowy wskaźnik nie jest prawidłowo wyrównany dla typu wskazanego, zachowanie jest niezdefiniowane, jeśli wynik jest wywołujący.</span><span class="sxs-lookup"><span data-stu-id="55f40-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="55f40-218">Ogólnie pojęcie "prawidłowo wyrównane" jest przechodnie: Jeśli wskaźnik do typu `A` jest prawidłowo wyrównany dla wskaźnika do typu `B`, które z kolei jest prawidłowo wyrównane dla wskaźnika do typu `C`, a następnie wskaźnik do typu `A` jest prawidłowo wyrównany dla wskaźnik do typu `C`.</span><span class="sxs-lookup"><span data-stu-id="55f40-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="55f40-219">Rozważmy następujący przypadek, w którym zmienna z jednym typem jest dostępna za pośrednictwem wskaźnika do innego typu:</span><span class="sxs-lookup"><span data-stu-id="55f40-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="55f40-220">Gdy typ wskaźnika jest konwertowany na wskaźnik do typu Byte, wynik wskazuje najniższy przyjmowany bajt zmiennej.</span><span class="sxs-lookup"><span data-stu-id="55f40-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="55f40-221">Kolejne przyrosty wyniku, do rozmiaru zmiennej, dają wskaźniki do pozostałych bajtów tej zmiennej.</span><span class="sxs-lookup"><span data-stu-id="55f40-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="55f40-222">Na przykład Poniższa metoda wyświetla każdy osiem bajtów jako wartość szesnastkową:</span><span class="sxs-lookup"><span data-stu-id="55f40-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="55f40-223">Oczywiście wygenerowane dane wyjściowe są zależne od bajtów.</span><span class="sxs-lookup"><span data-stu-id="55f40-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="55f40-224">Mapowania między wskaźnikami i liczbami całkowitymi są zdefiniowane w implementacji.</span><span class="sxs-lookup"><span data-stu-id="55f40-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="55f40-225">Jednakże w przypadku 32 \* i 64-bitowych architektur procesora z liniową przestrzenią adresową Konwersje wskaźników do lub z typów całkowitych zwykle zachowują się dokładnie tak, jak konwersje wartości `uint` lub `ulong` (odpowiednio do lub z tych typów całkowitych).</span><span class="sxs-lookup"><span data-stu-id="55f40-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="55f40-226">Tablice wskaźników</span><span class="sxs-lookup"><span data-stu-id="55f40-226">Pointer arrays</span></span>

<span data-ttu-id="55f40-227">W niebezpiecznym kontekście tablice wskaźników mogą być skonstruowane.</span><span class="sxs-lookup"><span data-stu-id="55f40-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="55f40-228">W tablicach wskaźników są dozwolone tylko niektóre konwersje, które mają zastosowanie do innych typów tablicowych:</span><span class="sxs-lookup"><span data-stu-id="55f40-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="55f40-229">Niejawna konwersja odwołań ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions)) z dowolnych *array_type* do `System.Array` i interfejsów, które implementuje również ma zastosowanie do tablic wskaźników.</span><span class="sxs-lookup"><span data-stu-id="55f40-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="55f40-230">Jednak każda próba uzyskania dostępu do elementów tablicy przy użyciu `System.Array` lub interfejsów, które implementuje, spowoduje wyjątek w czasie wykonywania, ponieważ typy wskaźnika nie są konwertowane na `object`.</span><span class="sxs-lookup"><span data-stu-id="55f40-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="55f40-231">Niejawne i jawne konwersje referencyjne ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions), [jawne konwersje odwołań](conversions.md#explicit-reference-conversions)) z jednowymiarowego typu tablicy `S[]` do `System.Collections.Generic.IList<T>` i jego ogólnych interfejsów podstawowych nigdy nie mają zastosowania do tablic wskaźników, Ponieważ typy wskaźnika nie mogą być używane jako argumenty typu, a nie istnieją konwersje z typów wskaźnika do typów niewskaźnikowych.</span><span class="sxs-lookup"><span data-stu-id="55f40-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="55f40-232">Jawna konwersja odwołań ([jawne konwersje odwołań](conversions.md#explicit-reference-conversions)) z `System.Array` i interfejsów, które implementuje do dowolnego *array_type* ma zastosowanie do tablic wskaźników.</span><span class="sxs-lookup"><span data-stu-id="55f40-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="55f40-233">Jawne konwersje referencyjne ([jawne konwersje odwołań](conversions.md#explicit-reference-conversions)) z `System.Collections.Generic.IList<S>` i jego interfejsy bazowe do jednowymiarowego typu tablicy `T[]` nigdy nie mają zastosowania do tablic wskaźników, ponieważ typy wskaźnika nie mogą być używane jako argumenty typu, a istnieją brak konwersji z typów wskaźnika do typów niewskaźnikowych.</span><span class="sxs-lookup"><span data-stu-id="55f40-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="55f40-234">Ograniczenia te oznaczają, że nie można zastosować rozwinięcia instrukcji `foreach` względem tablic opisanych w [instrukcji foreach](statements.md#the-foreach-statement) do tablic wskaźników.</span><span class="sxs-lookup"><span data-stu-id="55f40-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="55f40-235">Zamiast tego instrukcja foreach formularza</span><span class="sxs-lookup"><span data-stu-id="55f40-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="55f40-236">Jeśli typ `x` jest typem tablicy formularza `T[,,...,]`, `N` to liczba wymiarów minus 1 i `T` lub `V` jest typem wskaźnika, jest rozwinięty przy użyciu zagnieżdżonych pętli for — w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="55f40-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

<span data-ttu-id="55f40-237">Zmienne `a`, `i0`, `i1`,..., `iN` nie są widoczne dla `x` lub *embedded_statement* ani żadnego innego kodu źródłowego programu.</span><span class="sxs-lookup"><span data-stu-id="55f40-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="55f40-238">Zmienna `v` jest tylko do odczytu w osadzonej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="55f40-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="55f40-239">Jeśli nie istnieje jawna konwersja ([Konwersje wskaźników](unsafe-code.md#pointer-conversions)) z `T` (typ elementu) do `V`, zostanie wygenerowany błąd i nie są podejmowane żadne dalsze kroki.</span><span class="sxs-lookup"><span data-stu-id="55f40-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="55f40-240">Jeśli `x` ma wartość `null`, `System.NullReferenceException` jest zgłaszany w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="55f40-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="55f40-241">Wskaźniki w wyrażeniach</span><span class="sxs-lookup"><span data-stu-id="55f40-241">Pointers in expressions</span></span>

<span data-ttu-id="55f40-242">W niebezpiecznym kontekście wyrażenie może dawać wynik typu wskaźnika, ale poza niebezpiecznym kontekstem, jest to błąd czasu kompilacji dla wyrażenia, które ma być typu wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="55f40-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="55f40-243">W konkretnych warunkach, poza niebezpiecznym kontekstem, występuje błąd czasu kompilacji, jeśli dowolna *simple_name* ([proste nazwy](expressions.md#simple-names)), *member_access* ([dostęp do elementów członkowskich](expressions.md#member-access)), *invocation_expression* ([wyrażenia wywołania](expressions.md#invocation-expressions)) lub  *element_access* ([dostęp do elementu](expressions.md#element-access)) jest typu wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="55f40-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="55f40-244">W niebezpiecznym kontekście produkcja *primary_no_array_creation_expression* ([wyrażenia podstawowe](expressions.md#primary-expressions)) i *unary_expression* ([operatory jednoargumentowe](expressions.md#unary-operators)) dopuszczają następujące dodatkowe konstrukcje:</span><span class="sxs-lookup"><span data-stu-id="55f40-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

<span data-ttu-id="55f40-245">Te konstrukcje są opisane w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="55f40-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="55f40-246">Pierwszeństwo i łączność operatorów niebezpiecznych są implikowane przez gramatykę.</span><span class="sxs-lookup"><span data-stu-id="55f40-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="55f40-247">Pośredni wskaźnik</span><span class="sxs-lookup"><span data-stu-id="55f40-247">Pointer indirection</span></span>

<span data-ttu-id="55f40-248">*Pointer_indirection_expression* składa się z gwiazdki (`*`), a następnie *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="55f40-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="55f40-249">Operator jednoargumentowy `*` oznacza pośrednią wskaźnik i służy do uzyskania zmiennej, do której wskazuje wskaźnik.</span><span class="sxs-lookup"><span data-stu-id="55f40-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="55f40-250">Wynik oceny `*P`, gdzie `P` jest wyrażeniem typu wskaźnika `T*`, jest zmienną typu `T`.</span><span class="sxs-lookup"><span data-stu-id="55f40-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="55f40-251">Jest to błąd czasu kompilacji, aby zastosować jednoargumentowy operator `*` do wyrażenia typu `void*` lub do wyrażenia, które nie jest typu wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="55f40-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="55f40-252">Efekt zastosowania jednoargumentowego operatora `*` do wskaźnika `null` jest zdefiniowany przez implementację.</span><span class="sxs-lookup"><span data-stu-id="55f40-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="55f40-253">W szczególności nie ma żadnej gwarancji, że ta operacja zgłasza `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="55f40-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="55f40-254">Jeśli do wskaźnika przypisano nieprawidłową wartość, zachowanie jednoargumentowego operatora `*` jest niezdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="55f40-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="55f40-255">Wśród nieprawidłowych wartości w celu wyłuskania wskaźnika przez jednoargumentowy operator `*` to adres niewłaściwie wyrównany dla typu wskazywanego (Zobacz przykład w przypadku [konwersji wskaźników](unsafe-code.md#pointer-conversions)) i adres zmiennej po zakończeniu okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="55f40-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="55f40-256">W celach analizy z określonym przypisaniem zmienna utworzona w wyniku oceny wyrażenia `*P` jest uznawana za wstępnie przypisane ([wstępnie przypisane zmienne](variables.md#initially-assigned-variables)).</span><span class="sxs-lookup"><span data-stu-id="55f40-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="55f40-257">Dostęp do elementu członkowskiego wskaźnika</span><span class="sxs-lookup"><span data-stu-id="55f40-257">Pointer member access</span></span>

<span data-ttu-id="55f40-258">*Pointer_member_access* składa się z *primary_expression*, po którym następuje token "`->`", po którym następuje *Identyfikator* i opcjonalny *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="55f40-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="55f40-259">W celu uzyskania dostępu do elementu członkowskiego wskaźnika `P->I`, `P` musi być wyrażeniem typu wskaźnika innego niż `void*`, a `I` musi zwrócić uwagę na dostępną składową typu, do którego `P` punkty.</span><span class="sxs-lookup"><span data-stu-id="55f40-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="55f40-260">Dostęp do elementu członkowskiego wskaźnika `P->I` jest oceniany dokładnie jako `(*P).I`.</span><span class="sxs-lookup"><span data-stu-id="55f40-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="55f40-261">Aby uzyskać opis operatora pośredniego wskaźnika (`*`), zobacz [wskaźnik pośredni wskaźnika](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="55f40-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="55f40-262">Aby uzyskać opis operatora dostępu do elementów członkowskich (`.`), zobacz [dostęp do elementu członkowskiego](expressions.md#member-access).</span><span class="sxs-lookup"><span data-stu-id="55f40-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="55f40-263">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="55f40-263">In the example</span></span>

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

<span data-ttu-id="55f40-264">operator `->` służy do uzyskiwania dostępu do pól i wywoływania metody struktury za pomocą wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="55f40-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="55f40-265">Ponieważ operacja `P->I` jest precyzyjnie równoważna z `(*P).I`, Metoda `Main` mogła być również zapisywana:</span><span class="sxs-lookup"><span data-stu-id="55f40-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a><span data-ttu-id="55f40-266">Dostęp do elementu wskaźnika</span><span class="sxs-lookup"><span data-stu-id="55f40-266">Pointer element access</span></span>

<span data-ttu-id="55f40-267">*Pointer_element_access* składa się z *primary_no_array_creation_expression* , a następnie wyrażenia ujętego w "`[`" i "`]`".</span><span class="sxs-lookup"><span data-stu-id="55f40-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="55f40-268">W przypadku dostępu do elementu wskaźnika w postaci `P[E]`, `P` musi być wyrażeniem typu wskaźnika innego niż `void*`, a `E` musi być wyrażeniem, które można niejawnie przekonwertować na `int`, `uint`, `long` lub `ulong`.</span><span class="sxs-lookup"><span data-stu-id="55f40-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="55f40-269">Dostęp do elementu wskaźnika w postaci `P[E]` jest obliczany dokładnie jako `*(P + E)`.</span><span class="sxs-lookup"><span data-stu-id="55f40-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="55f40-270">Aby uzyskać opis operatora pośredniego wskaźnika (`*`), zobacz [wskaźnik pośredni wskaźnika](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="55f40-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="55f40-271">Aby uzyskać opis operatora dodawania wskaźnika (`+`), zobacz [arytmetyka wskaźnika](unsafe-code.md#pointer-arithmetic).</span><span class="sxs-lookup"><span data-stu-id="55f40-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="55f40-272">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="55f40-272">In the example</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

<span data-ttu-id="55f40-273">dostęp do elementu wskaźnika jest używany do inicjowania buforu znaków w pętli `for`.</span><span class="sxs-lookup"><span data-stu-id="55f40-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="55f40-274">Ponieważ operacja `P[E]` jest precyzyjnie równoważna z `*(P + E)`, przykład może być równie dobrze zapisany:</span><span class="sxs-lookup"><span data-stu-id="55f40-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

<span data-ttu-id="55f40-275">Operator dostępu do elementów wskaźnika nie sprawdza występowania błędów poza granicami, a zachowanie podczas uzyskiwania dostępu do elementu poza granicami jest niezdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="55f40-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="55f40-276">Jest to taka sama jak C i C++.</span><span class="sxs-lookup"><span data-stu-id="55f40-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="55f40-277">Operator address-of</span><span class="sxs-lookup"><span data-stu-id="55f40-277">The address-of operator</span></span>

<span data-ttu-id="55f40-278">*Addressof_expression* składa się ze znaku handlowego "i" (`&`), a następnie *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="55f40-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="55f40-279">Podano wyrażenie `E`, które jest typu `T` i jest klasyfikowane jako zmienna stała ([zmienne stałe i ruchome](unsafe-code.md#fixed-and-moveable-variables)), konstrukcja `&E` oblicza adres zmiennej określonej przez `E`.</span><span class="sxs-lookup"><span data-stu-id="55f40-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="55f40-280">Typ wyniku jest `T*` i jest sklasyfikowany jako wartość.</span><span class="sxs-lookup"><span data-stu-id="55f40-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="55f40-281">Błąd czasu kompilacji występuje, jeśli `E` nie jest sklasyfikowany jako zmienna, jeśli `E` jest sklasyfikowany jako zmienna lokalna tylko do odczytu lub jeśli `E` oznacza ruchomą zmienną.</span><span class="sxs-lookup"><span data-stu-id="55f40-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="55f40-282">W ostatnim przypadku, stała instrukcja ([stała instrukcja](unsafe-code.md#the-fixed-statement)) może służyć do tymczasowego "naprawy" zmiennej przed uzyskaniem jej adresu.</span><span class="sxs-lookup"><span data-stu-id="55f40-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="55f40-283">Jak określono w [dostępie do elementu członkowskiego](expressions.md#member-access), poza konstruktorem wystąpienia lub konstruktorem statycznym dla struktury lub klasy, która definiuje pole `readonly`, to pole jest uznawane za wartość, a nie zmienną.</span><span class="sxs-lookup"><span data-stu-id="55f40-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="55f40-284">W związku z tym adres nie może zostać pobrany.</span><span class="sxs-lookup"><span data-stu-id="55f40-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="55f40-285">Analogicznie, nie można pobrać adresu stałej.</span><span class="sxs-lookup"><span data-stu-id="55f40-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="55f40-286">Operator `&` nie wymaga, aby jego argument był ostatecznie przypisany, ale po operacji `&` zmienna, do której zastosowano operator, jest uważana za ostatecznie przypisaną w ścieżce wykonywania, w której występuje operacja.</span><span class="sxs-lookup"><span data-stu-id="55f40-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="55f40-287">Programista jest odpowiedzialny za zagwarantowanie, że w tej sytuacji w rzeczywistości odbywa się poprawna inicjalizacja zmiennej.</span><span class="sxs-lookup"><span data-stu-id="55f40-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="55f40-288">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="55f40-288">In the example</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="55f40-289">`i` jest uznawany za ostatecznie przypisany po operacji `&i` użytej do zainicjowania `p`.</span><span class="sxs-lookup"><span data-stu-id="55f40-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="55f40-290">Przypisanie do `*p` jest inicjowane `i`, ale dołączenie tej inicjalizacji jest odpowiedzialne za programistę i nie wystąpi błąd czasu kompilacji, jeśli przypisanie zostało usunięte.</span><span class="sxs-lookup"><span data-stu-id="55f40-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="55f40-291">Reguły o określonym przypisaniu dla operatora `&` istnieją w taki sposób, że można uniknąć nadmiarowego inicjowania zmiennych lokalnych.</span><span class="sxs-lookup"><span data-stu-id="55f40-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="55f40-292">Na przykład wiele zewnętrznych interfejsów API przyjmuje wskaźnik do struktury, która jest wypełniana przez interfejs API.</span><span class="sxs-lookup"><span data-stu-id="55f40-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="55f40-293">Wywołania takich interfejsów API zwykle przekazują adres lokalnej zmiennej struktury i bez reguły, nadmiarowe inicjowanie zmiennej struktury będzie wymagane.</span><span class="sxs-lookup"><span data-stu-id="55f40-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="55f40-294">Zwiększenie i zmniejszenie wskaźnika</span><span class="sxs-lookup"><span data-stu-id="55f40-294">Pointer increment and decrement</span></span>

<span data-ttu-id="55f40-295">W niebezpiecznym kontekście operatory `++` i `--` (operatory[przyrostu i zmniejszania](expressions.md#postfix-increment-and-decrement-operators) wartości [prefiksu i zmniejszania](expressions.md#prefix-increment-and-decrement-operators)) mogą być stosowane do zmiennych wskaźnikowych wszystkich typów, z wyjątkiem `void*`.</span><span class="sxs-lookup"><span data-stu-id="55f40-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="55f40-296">W tym celu dla każdego typu wskaźnika `T*` następujące operatory są niejawnie zdefiniowane:</span><span class="sxs-lookup"><span data-stu-id="55f40-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="55f40-297">Operatory te same wyniki, co `x + 1` i `x - 1` ([arytmetyka wskaźnika](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="55f40-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="55f40-298">Innymi słowy dla zmiennej wskaźnikowej typu `T*` operator `++` dodaje `sizeof(T)` do adresu zawartego w zmiennej, a operator `--` odejmuje `sizeof(T)` od adresu zawartego w zmiennej.</span><span class="sxs-lookup"><span data-stu-id="55f40-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="55f40-299">Jeśli operacja zwiększania lub zmniejszania wskaźnika przepełnił domenę typu wskaźnika, wynik jest zdefiniowany przez implementację, ale nie są generowane żadne wyjątki.</span><span class="sxs-lookup"><span data-stu-id="55f40-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="55f40-300">Arytmetyczny wskaźnik</span><span class="sxs-lookup"><span data-stu-id="55f40-300">Pointer arithmetic</span></span>

<span data-ttu-id="55f40-301">W niebezpiecznym kontekście operatory "`+` i `-`" (operator[dodawania](expressions.md#addition-operator) i [operator odejmowania](expressions.md#subtraction-operator)) mogą być stosowane do wartości wszystkich typów wskaźnika, z wyjątkiem `void*`.</span><span class="sxs-lookup"><span data-stu-id="55f40-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="55f40-302">W tym celu dla każdego typu wskaźnika `T*` następujące operatory są niejawnie zdefiniowane:</span><span class="sxs-lookup"><span data-stu-id="55f40-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

<span data-ttu-id="55f40-303">Jeśli wyrażenie `P` typu wskaźnika `T*` i wyrażenie `N` typu `int`, `uint`, `long` lub `ulong`, wyrażenia `P + N` i `N + P` obliczą wartość wskaźnika typu `T*`, która powoduje dodanie 0 do adresu podanym przez 1.</span><span class="sxs-lookup"><span data-stu-id="55f40-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="55f40-304">Analogicznie, wyrażenie `P - N` oblicza wartość wskaźnika typu `T*` będącą wynikiem odejmowania `N * sizeof(T)` od adresu podanych przez `P`.</span><span class="sxs-lookup"><span data-stu-id="55f40-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="55f40-305">Uwzględniając dwa wyrażenia, `P` i `Q`, typu wskaźnika `T*`, wyrażenie `P - Q` oblicza różnicę między adresami podaną przez `P` i `Q`, a następnie dzieli tę różnicę przez `sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="55f40-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="55f40-306">Typ wyniku jest zawsze `long`.</span><span class="sxs-lookup"><span data-stu-id="55f40-306">The type of the result is always `long`.</span></span> <span data-ttu-id="55f40-307">W efekcie `P - Q` jest obliczana jako `((long)(P) - (long)(Q)) / sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="55f40-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="55f40-308">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="55f40-308">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

<span data-ttu-id="55f40-309">co daje wynik:</span><span class="sxs-lookup"><span data-stu-id="55f40-309">which produces the output:</span></span>

```console
p - q = -14
q - p = 14
```

<span data-ttu-id="55f40-310">Jeśli operacja arytmetyczna wskaźnika przepełni domenę typu wskaźnika, wynik jest obcinany w sposób zdefiniowany dla implementacji, ale nie są generowane żadne wyjątki.</span><span class="sxs-lookup"><span data-stu-id="55f40-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="55f40-311">Porównanie wskaźników</span><span class="sxs-lookup"><span data-stu-id="55f40-311">Pointer comparison</span></span>

<span data-ttu-id="55f40-312">W niebezpiecznym kontekście operatory `==`, `!=`, `<`, `>`, `<=` i `=>` ([Operatory relacyjne i typu-test](expressions.md#relational-and-type-testing-operators)) mogą być stosowane do wartości wszystkich typów wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="55f40-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="55f40-313">Operatory porównania wskaźników są następujące:</span><span class="sxs-lookup"><span data-stu-id="55f40-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="55f40-314">Ponieważ niejawna konwersja istnieje z dowolnego typu wskaźnika do typu `void*`, operandy dowolnego typu wskaźnika można porównać przy użyciu tych operatorów.</span><span class="sxs-lookup"><span data-stu-id="55f40-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="55f40-315">Operatory porównania porównują adresy nadawane przez dwa operandy, tak jakby były to liczby całkowite bez znaku.</span><span class="sxs-lookup"><span data-stu-id="55f40-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="55f40-316">Operator sizeof</span><span class="sxs-lookup"><span data-stu-id="55f40-316">The sizeof operator</span></span>

<span data-ttu-id="55f40-317">Operator `sizeof` zwraca liczbę bajtów zajmowanych przez zmienną danego typu.</span><span class="sxs-lookup"><span data-stu-id="55f40-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="55f40-318">Typ określony jako argument operacji `sizeof` musi być *unmanaged_type* ([typy wskaźników](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="55f40-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="55f40-319">Wynik operatora `sizeof` jest wartością typu `int`.</span><span class="sxs-lookup"><span data-stu-id="55f40-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="55f40-320">W przypadku niektórych wstępnie zdefiniowanych typów operator `sizeof` zwraca wartość stałą, jak pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="55f40-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="55f40-321">__Expression__</span><span class="sxs-lookup"><span data-stu-id="55f40-321">__Expression__</span></span>   | <span data-ttu-id="55f40-322">__wynik__</span><span class="sxs-lookup"><span data-stu-id="55f40-322">__Result__</span></span> |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

<span data-ttu-id="55f40-323">Dla wszystkich innych typów wynik operatora `sizeof` jest zdefiniowany przez implementację i jest sklasyfikowany jako wartość, a nie jako stała.</span><span class="sxs-lookup"><span data-stu-id="55f40-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="55f40-324">Kolejność, w której elementy członkowskie są pakowane w strukturę, nie została określona.</span><span class="sxs-lookup"><span data-stu-id="55f40-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="55f40-325">Na potrzeby wyrównania może istnieć nienazwane uzupełnienie na początku struktury, w obrębie struktury i na końcu struktury.</span><span class="sxs-lookup"><span data-stu-id="55f40-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="55f40-326">Zawartość bitów użytych jako uzupełnienie jest nieokreślona.</span><span class="sxs-lookup"><span data-stu-id="55f40-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="55f40-327">W przypadku zastosowania do operandu, który ma typ struktury, wynik jest łączną liczbą bajtów w zmiennej tego typu, włącznie z dopełnieniem.</span><span class="sxs-lookup"><span data-stu-id="55f40-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="55f40-328">Fixed — Instrukcja</span><span class="sxs-lookup"><span data-stu-id="55f40-328">The fixed statement</span></span>

<span data-ttu-id="55f40-329">W niebezpiecznym kontekście produkcja *embedded_statement* ([instrukcje](statements.md)) dopuszczają dodatkową konstrukcję, instrukcję `fixed`, która jest używana do "naprawy" ruchomej zmiennej w taki sposób, że jej adres pozostaje stałą na czas trwania instrukcji .</span><span class="sxs-lookup"><span data-stu-id="55f40-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

<span data-ttu-id="55f40-330">Każdy *fixed_pointer_declarator* deklaruje zmienną lokalną danego *pointer_type* i inicjuje zmienną lokalną o adresie obliczonym przez odpowiedni *fixed_pointer_initializer*.</span><span class="sxs-lookup"><span data-stu-id="55f40-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="55f40-331">Zmienna lokalna zadeklarowana w instrukcji `fixed` jest dostępna w dowolnych *fixed_pointer_initializer*s występujących po prawej stronie deklaracji tej zmiennej, a także w *embedded_statement* instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="55f40-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="55f40-332">Zmienna lokalna zadeklarowana przez instrukcję `fixed` jest traktowana jako tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="55f40-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="55f40-333">Błąd czasu kompilacji występuje, jeśli osadzona instrukcja próbuje zmodyfikować tę zmienną lokalną (przez przypisanie lub `++` i `--` operatory) lub przekazać ją jako parametr `ref` lub `out`.</span><span class="sxs-lookup"><span data-stu-id="55f40-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="55f40-334">*Fixed_pointer_initializer* może być jedną z następujących:</span><span class="sxs-lookup"><span data-stu-id="55f40-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="55f40-335">Token "`&`", po którym następują *variable_reference* ([precyzyjne reguły określania przypisania](variables.md#precise-rules-for-determining-definite-assignment)) do ruchomej zmiennej ([stałe i ruchome zmienne](unsafe-code.md#fixed-and-moveable-variables)) niezarządzanego typu `T`, pod warunkiem, że typ `T*` jest niejawne konwersję na typ wskaźnika podanym w instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="55f40-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="55f40-336">W takim przypadku inicjator oblicza adres danej zmiennej, a zmienna ma gwarancję pozostawania na stałym adresie dla czasu trwania instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="55f40-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="55f40-337">Wyrażenie *array_type* z elementami typu niezarządzanego `T`, pod warunkiem, że typ `T*` jest niejawnie konwertowany na typ wskaźnika podany w instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="55f40-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="55f40-338">W takim przypadku inicjator oblicza adres pierwszego elementu w tablicy, a cała tablica ma być pozostawać w stałym adresie na czas trwania instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="55f40-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="55f40-339">Jeśli wyrażenie tablicy ma wartość null lub jeśli tablica ma elementy zerowe, inicjator oblicza adres równy zero.</span><span class="sxs-lookup"><span data-stu-id="55f40-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="55f40-340">Wyrażenie typu `string`, pod warunkiem, że typ `char*` jest niejawnie konwertowany na typ wskaźnika podany w instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="55f40-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="55f40-341">W takim przypadku inicjator oblicza adres pierwszego znaku w ciągu, a cały ciąg ma pozostawać w stałym adresie dla czasu trwania instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="55f40-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="55f40-342">Zachowanie instrukcji `fixed` jest zdefiniowane w implementacji, jeśli wyrażenie String ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="55f40-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="55f40-343">Element *simple_name* lub *member_access* , który odwołuje się do elementu członkowskiego buforu o ustalonym rozmiarze zmiennej ruchomej, pod warunkiem, że typ składowej buforu o ustalonym rozmiarze jest niejawnie konwertowany na typ wskaźnika podany w instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="55f40-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="55f40-344">W takim przypadku inicjator oblicza wskaźnik do pierwszego elementu buforu o ustalonym rozmiarze ([bufory o stałym rozmiarze w wyrażeniach](unsafe-code.md#fixed-size-buffers-in-expressions)), a bufor o ustalonym rozmiarze jest gwarantowany na stały adres na czas trwania instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="55f40-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="55f40-345">Dla każdego adresu obliczanego przez *fixed_pointer_initializer* instrukcja `fixed` zapewnia, że zmienna, do której odwołuje się adres, nie podlega relokacji ani niepodyspozycji przez moduł wyrzucania elementów bezużytecznych na czas trwania instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="55f40-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="55f40-346">Na przykład, jeśli adres obliczony przez *fixed_pointer_initializer* odwołuje się do pola obiektu lub elementu w instancji Array, instrukcja `fixed` gwarantuje, że wystąpienie obiektu zawierającego nie jest ponownie zlokalizowane lub usunięte podczas okres istnienia instrukcji.</span><span class="sxs-lookup"><span data-stu-id="55f40-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="55f40-347">Programista jest odpowiedzialny za zapewnienie, że wskaźniki utworzone przez instrukcje `fixed` nie przeżyły poza wykonywanie tych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="55f40-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="55f40-348">Na przykład, gdy wskaźniki utworzone przez instrukcje `fixed` są przesyłane do zewnętrznych interfejsów API, zależy to od programisty, aby upewnić się, że interfejsy API nie przechowują pamięci tych wskaźników.</span><span class="sxs-lookup"><span data-stu-id="55f40-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="55f40-349">Stałe obiekty mogą spowodować fragmentację sterty (ponieważ nie można ich przenieść).</span><span class="sxs-lookup"><span data-stu-id="55f40-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="55f40-350">Z tego powodu obiekty powinny być naprawione tylko wtedy, gdy jest to absolutnie konieczne, a następnie tylko dla najkrótszej ilości czasu.</span><span class="sxs-lookup"><span data-stu-id="55f40-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="55f40-351">Przykład</span><span class="sxs-lookup"><span data-stu-id="55f40-351">The example</span></span>

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

<span data-ttu-id="55f40-352">ilustruje kilka zastosowania instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="55f40-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="55f40-353">Pierwsza instrukcja naprawia i uzyskuje adres pola statycznego, druga instrukcja naprawia i uzyskuje adres pola wystąpienia, a trzecia instrukcja naprawia i uzyskuje adres elementu tablicy.</span><span class="sxs-lookup"><span data-stu-id="55f40-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="55f40-354">W każdym przypadku Wystąpił błąd, aby użyć zwykłego operatora `&`, ponieważ zmienne są wszystkie sklasyfikowane jako ruchome zmienne.</span><span class="sxs-lookup"><span data-stu-id="55f40-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="55f40-355">Czwarta instrukcja `fixed` w powyższym przykładzie daje podobny wynik do trzeciej.</span><span class="sxs-lookup"><span data-stu-id="55f40-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="55f40-356">Ten przykład instrukcji `fixed` używa `string`:</span><span class="sxs-lookup"><span data-stu-id="55f40-356">This example of the `fixed` statement uses `string`:</span></span>

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

<span data-ttu-id="55f40-357">W niebezpieczne elementy tablicy jednowymiarowej są przechowywane w kolejności rosnącego indeksu, rozpoczynając od indeksu `0` i kończąc z indeksem `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="55f40-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="55f40-358">W przypadku tablic wielowymiarowych elementy tablic są przechowywane w taki sposób, aby indeksy w wymiarze z prawej strony były zwiększane jako pierwsze, następnie w następnym lewym wymiarze itd.</span><span class="sxs-lookup"><span data-stu-id="55f40-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="55f40-359">W instrukcji `fixed`, która uzyskuje wskaźnik `p` do wystąpienia tablicy `a`, wartości wskaźnika od `p` do `p + a.Length - 1` reprezentują adresy elementów w tablicy.</span><span class="sxs-lookup"><span data-stu-id="55f40-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="55f40-360">Podobnie zmienne od `p[0]` do `p[a.Length - 1]` reprezentują rzeczywiste elementy tablicy.</span><span class="sxs-lookup"><span data-stu-id="55f40-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="55f40-361">Ze względu na sposób przechowywania tablic można traktować tablicę dowolnego wymiaru, tak jakby były liniowe.</span><span class="sxs-lookup"><span data-stu-id="55f40-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="55f40-362">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="55f40-362">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

<span data-ttu-id="55f40-363">co daje wynik:</span><span class="sxs-lookup"><span data-stu-id="55f40-363">which produces the output:</span></span>

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="55f40-364">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="55f40-364">In the example</span></span>

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

<span data-ttu-id="55f40-365">Instrukcja `fixed` służy do naprawienia tablicy, aby można było przekazywać jej adresy do metody, która przyjmuje wskaźnik.</span><span class="sxs-lookup"><span data-stu-id="55f40-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="55f40-366">W przykładzie:</span><span class="sxs-lookup"><span data-stu-id="55f40-366">In the example:</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

<span data-ttu-id="55f40-367">stała Instrukcja służy do naprawienia buforu o ustalonym rozmiarze struktury, tak aby można było użyć adresu jako wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="55f40-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="55f40-368">Wartość `char*` wygenerowane przez naprawianie wystąpienia ciągu zawsze wskazuje na ciąg zakończony znakiem null.</span><span class="sxs-lookup"><span data-stu-id="55f40-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="55f40-369">W ramach stałej instrukcji, która uzyskuje wskaźnik `p` do wystąpienia ciągu `s`, wartości wskaźnika od `p` do `p + s.Length - 1` reprezentują adresy znaków w ciągu, a wartość wskaźnika `p + s.Length` zawsze wskazuje znak null ( znak z wartością `'\0'`).</span><span class="sxs-lookup"><span data-stu-id="55f40-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="55f40-370">Modyfikowanie obiektów typu zarządzanego za poorednictwem stałych wskaźników może spowodować niezdefiniowane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="55f40-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="55f40-371">Na przykład ze względu na to, że ciągi są niezmienne, zależy to programisty, aby upewnić się, że znaki, do których odwołuje się wskaźnik do ustalonego ciągu nie są modyfikowane.</span><span class="sxs-lookup"><span data-stu-id="55f40-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="55f40-372">Automatyczne Kończenie pustej wartości null ciągów jest szczególnie wygodne podczas wywoływania zewnętrznych interfejsów API, które oczekują ciągów "C-style".</span><span class="sxs-lookup"><span data-stu-id="55f40-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="55f40-373">Należy jednak zauważyć, że wystąpienie ciągu może zawierać znaki o wartości null.</span><span class="sxs-lookup"><span data-stu-id="55f40-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="55f40-374">Jeśli takie znaki mają wartość null, ciąg zostanie wyświetlony jako obcięty, gdy jest traktowany jako `char*` o wartości null.</span><span class="sxs-lookup"><span data-stu-id="55f40-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="55f40-375">Bufory o ustalonym rozmiarze</span><span class="sxs-lookup"><span data-stu-id="55f40-375">Fixed size buffers</span></span>

<span data-ttu-id="55f40-376">Bufory o ustalonym rozmiarze są używane do deklarowania "stylu języka C" tablic wbudowanych jako elementów członkowskich struktur i są szczególnie przydatne w połączeniu z niezarządzanymi interfejsami API.</span><span class="sxs-lookup"><span data-stu-id="55f40-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="55f40-377">Deklaracje buforu o ustalonym rozmiarze</span><span class="sxs-lookup"><span data-stu-id="55f40-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="55f40-378">***Bufor o ustalonym rozmiarze*** jest składową reprezentującą magazyn dla bufora o stałej długości zmiennych danego typu.</span><span class="sxs-lookup"><span data-stu-id="55f40-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="55f40-379">Deklaracja buforu o ustalonym rozmiarze wprowadza jeden lub więcej buforów o ustalonym rozmiarze danego typu elementu.</span><span class="sxs-lookup"><span data-stu-id="55f40-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="55f40-380">Bufory o ustalonym rozmiarze są dozwolone tylko w deklaracjach struktury i mogą występować tylko w niebezpiecznym kontekście ([niebezpieczne konteksty](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="55f40-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

<span data-ttu-id="55f40-381">Deklaracja buforu o ustalonym rozmiarze może zawierać zestaw atrybutów ([atrybutów](attributes.md)), modyfikator `new` ([Modyfikatory](classes.md#modifiers)), prawidłową kombinację czterech modyfikatorów dostępu ([parametrów i ograniczeń typu](classes.md#type-parameters-and-constraints)) oraz modyfikator `unsafe` ([niebezpieczny Konteksty](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="55f40-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="55f40-382">Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych przez deklarację buforu o ustalonym rozmiarze.</span><span class="sxs-lookup"><span data-stu-id="55f40-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="55f40-383">Występuje błąd dla tego samego modyfikatora w deklaracji buforu o ustalonym rozmiarze.</span><span class="sxs-lookup"><span data-stu-id="55f40-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="55f40-384">Deklaracja buforu o ustalonym rozmiarze nie może zawierać modyfikatora `static`.</span><span class="sxs-lookup"><span data-stu-id="55f40-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="55f40-385">Typ elementu buforu deklaracji buforu o ustalonym rozmiarze określa typ elementu buforów wprowadzonych przez deklarację.</span><span class="sxs-lookup"><span data-stu-id="55f40-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="55f40-386">Typem elementu buforu musi być jeden ze wstępnie zdefiniowanych typów `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0 lub 1.</span><span class="sxs-lookup"><span data-stu-id="55f40-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="55f40-387">Po typie elementu buforu następuje lista buforów o ustalonym rozmiarze deklaratory, z których każdy wprowadza nowy element członkowski.</span><span class="sxs-lookup"><span data-stu-id="55f40-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="55f40-388">Bufor o ustalonym rozmiarze deklarator składa się z identyfikatora, który zawiera nazwę elementu członkowskiego, a następnie wyrażenie stałe ujęte w tokenach `[` i `]`.</span><span class="sxs-lookup"><span data-stu-id="55f40-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="55f40-389">Stałe wyrażenie oznacza liczbę elementów w elemencie członkowskim wprowadzonym przez ten bufor o ustalonym rozmiarze deklarator.</span><span class="sxs-lookup"><span data-stu-id="55f40-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="55f40-390">Typ wyrażenia stałego musi być niejawnie konwertowany na typ `int`, a wartość musi być dodatnią liczbą całkowitą różną od zera.</span><span class="sxs-lookup"><span data-stu-id="55f40-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="55f40-391">Elementy buforu o ustalonym rozmiarze są gwarantowane w kolejności sekwencyjnej w pamięci.</span><span class="sxs-lookup"><span data-stu-id="55f40-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="55f40-392">Deklaracja buforu o ustalonym rozmiarze, która deklaruje wiele buforów o ustalonym rozmiarze, jest równoważna z wieloma deklaracjami o pojedynczej deklaracji buforu o ustalonym rozmiarze z tymi samymi atrybutami i typami elementów.</span><span class="sxs-lookup"><span data-stu-id="55f40-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="55f40-393">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="55f40-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="55f40-394">odpowiada wyrażeniu</span><span class="sxs-lookup"><span data-stu-id="55f40-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="55f40-395">Bufory o ustalonym rozmiarze w wyrażeniach</span><span class="sxs-lookup"><span data-stu-id="55f40-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="55f40-396">Wyszukiwanie elementów członkowskich[](expressions.md#operators)buforu o ustalonym rozmiarze jest realizowane dokładnie tak samo jak odnośnik elementu członkowskiego pola.</span><span class="sxs-lookup"><span data-stu-id="55f40-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="55f40-397">Bufor o ustalonym rozmiarze może być przywoływany w wyrażeniu przy użyciu *simple_name* ([wnioskowanie o typie](expressions.md#type-inference)) lub *member_access* ([Sprawdzanie w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="55f40-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="55f40-398">Gdy element członkowski buforu o ustalonym rozmiarze jest przywoływany jako nazwa prosta, efekt jest taki sam jak w przypadku dostępu do elementu członkowskiego w postaci `this.I`, gdzie `I` jest składową buforu o ustalonym rozmiarze.</span><span class="sxs-lookup"><span data-stu-id="55f40-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="55f40-399">W celu uzyskania dostępu do elementu członkowskiego w formularzu `E.I`, jeśli `E` jest typu struktury i Wyszukiwanie elementów członkowskich `I` w tym typie struktury identyfikuje element członkowski o stałym rozmiarze, a następnie `E.I` jest oceniane sklasyfikowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="55f40-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="55f40-400">Jeśli wyrażenie `E.I` nie występuje w niebezpiecznym kontekście, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="55f40-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="55f40-401">Jeśli `E` jest sklasyfikowany jako wartość, wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="55f40-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="55f40-402">W przeciwnym razie, jeśli `E` jest zmienną ruchomą ([zmienne stałe i ruchome](unsafe-code.md#fixed-and-moveable-variables)) i wyrażenie `E.I` nie jest *fixed_pointer_initializer* ([stała instrukcja](unsafe-code.md#the-fixed-statement)), wystąpi błąd w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="55f40-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="55f40-403">W przeciwnym razie `E` odwołuje się do stałej zmiennej, a wynik wyrażenia jest wskaźnikiem do pierwszego elementu składowej buforu o ustalonym rozmiarze `I` w `E`.</span><span class="sxs-lookup"><span data-stu-id="55f40-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="55f40-404">Wynik jest typu `S*`, gdzie `S` jest typem elementu `I` i jest sklasyfikowany jako wartość.</span><span class="sxs-lookup"><span data-stu-id="55f40-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="55f40-405">Kolejne elementy buforu o ustalonym rozmiarze są dostępne przy użyciu operacji wskaźnika z pierwszego elementu.</span><span class="sxs-lookup"><span data-stu-id="55f40-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="55f40-406">W przeciwieństwie do dostępu do tablic, dostęp do elementów buforu o ustalonym rozmiarze jest niebezpieczną operacją i nie jest sprawdzany zakres.</span><span class="sxs-lookup"><span data-stu-id="55f40-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="55f40-407">Poniższy przykład deklaruje i używa struktury z elementem członkowskim buforu o ustalonym rozmiarze.</span><span class="sxs-lookup"><span data-stu-id="55f40-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a><span data-ttu-id="55f40-408">Określone sprawdzanie przypisania</span><span class="sxs-lookup"><span data-stu-id="55f40-408">Definite assignment checking</span></span>

<span data-ttu-id="55f40-409">Bufory o ustalonym rozmiarze nie podlegają nieograniczonemu sprawdzaniu przypisania ([przypisanie](variables.md#definite-assignment)), a elementy członkowskie buforu o ustalonym rozmiarze są ignorowane na potrzeby nieograniczonego sprawdzania przypisania zmiennych typu struktury.</span><span class="sxs-lookup"><span data-stu-id="55f40-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="55f40-410">Gdy najbardziej nieruchoma zmienna struktury składowej buforu o ustalonym rozmiarze to zmienna statyczna, zmienna wystąpienia klasy lub element tablicy, elementy buforu o ustalonym rozmiarze są automatycznie inicjowane na wartości domyślne ([domyślne wartości](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="55f40-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="55f40-411">We wszystkich innych przypadkach początkowa zawartość buforu o ustalonym rozmiarze jest niezdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="55f40-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="55f40-412">Alokacja stosu</span><span class="sxs-lookup"><span data-stu-id="55f40-412">Stack allocation</span></span>

<span data-ttu-id="55f40-413">W niebezpiecznym kontekście deklaracja zmiennej lokalnej ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)) może zawierać inicjator alokacji stosu, który przydziela pamięć ze stosu wywołań.</span><span class="sxs-lookup"><span data-stu-id="55f40-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="55f40-414">*Unmanaged_type* wskazuje typ elementów, które będą przechowywane w nowo przydzielonym miejscu, a *wyrażenie* wskazuje liczbę tych elementów.</span><span class="sxs-lookup"><span data-stu-id="55f40-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="55f40-415">Razem, określają one wymagany rozmiar alokacji.</span><span class="sxs-lookup"><span data-stu-id="55f40-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="55f40-416">Ponieważ rozmiar alokacji stosu nie może być ujemny, jest to błąd czasu kompilacji, aby określić liczbę elementów jako *constant_expression* , których wynikiem jest wartość ujemna.</span><span class="sxs-lookup"><span data-stu-id="55f40-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="55f40-417">Inicjator alokacji stosu w formularzu `stackalloc T[E]` wymaga, aby `T` był typem niezarządzanym ([typy wskaźników](unsafe-code.md#pointer-types)) i `E` jako wyrażenie typu `int`.</span><span class="sxs-lookup"><span data-stu-id="55f40-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="55f40-418">Konstrukcja przydziela `E * sizeof(T)` bajtów z stosu wywołań i zwraca wskaźnik o typie `T*` do nowo przydzielonego bloku.</span><span class="sxs-lookup"><span data-stu-id="55f40-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="55f40-419">Jeśli `E` jest wartością ujemną, zachowanie jest niezdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="55f40-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="55f40-420">Jeśli `E` ma wartość zero, alokacja nie zostanie wykonana, a zwrócony wskaźnik jest zdefiniowany przez implementację.</span><span class="sxs-lookup"><span data-stu-id="55f40-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="55f40-421">Jeśli nie jest dostępna wystarczająca ilość pamięci do przydzielenia bloku danego rozmiaru, zostanie zgłoszony `System.StackOverflowException`.</span><span class="sxs-lookup"><span data-stu-id="55f40-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="55f40-422">Zawartość nowo przydzieloną pamięci jest niezdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="55f40-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="55f40-423">Inicjatory alokacji stosu są niedozwolone w blokach `catch` lub `finally` ([Instrukcja try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="55f40-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="55f40-424">Nie istnieje sposób, aby jawnie zwolnić ilość pamięci przydzieloną przy użyciu `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="55f40-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="55f40-425">Wszystkie bloki pamięci przydzieloną na stosie utworzone podczas wykonywania elementu członkowskiego funkcji są automatycznie odrzucane, gdy ten element członkowski funkcji zwraca.</span><span class="sxs-lookup"><span data-stu-id="55f40-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="55f40-426">Odnosi się to do funkcji `alloca`, czyli rozszerzenia często dostępnego w języku C++ C i implementacji.</span><span class="sxs-lookup"><span data-stu-id="55f40-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="55f40-427">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="55f40-427">In the example</span></span>

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

<span data-ttu-id="55f40-428">Inicjator `stackalloc` jest używany w metodzie `IntToString` do przydzielania bufora 16 znaków na stosie.</span><span class="sxs-lookup"><span data-stu-id="55f40-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="55f40-429">Bufor jest automatycznie odrzucany, gdy metoda zwraca.</span><span class="sxs-lookup"><span data-stu-id="55f40-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="55f40-430">Alokacja pamięci dynamicznej</span><span class="sxs-lookup"><span data-stu-id="55f40-430">Dynamic memory allocation</span></span>

<span data-ttu-id="55f40-431">Z wyjątkiem `stackalloc`, C# nie zawiera wstępnie zdefiniowanych konstrukcji do zarządzania niepamięciową ilością pamięci.</span><span class="sxs-lookup"><span data-stu-id="55f40-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="55f40-432">Takie usługi są zazwyczaj udostępniane przez obsługę bibliotek klas lub zaimportowane bezpośrednio z bazowego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="55f40-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="55f40-433">Na przykład Klasa `Memory` ilustruje, w jaki sposób funkcje sterty bazowego systemu operacyjnego są dostępne z C#:</span><span class="sxs-lookup"><span data-stu-id="55f40-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

<span data-ttu-id="55f40-434">Przykład korzystający z klasy `Memory` jest przedstawiony poniżej:</span><span class="sxs-lookup"><span data-stu-id="55f40-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

<span data-ttu-id="55f40-435">W przykładzie przydzielane są 256 bajtów pamięci za pośrednictwem `Memory.Alloc` i inicjuje blok pamięci z wartościami rosnącymi z przedziału od 0 do 255.</span><span class="sxs-lookup"><span data-stu-id="55f40-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="55f40-436">Następnie alokuje tablicę bajtów elementu 256 i używa `Memory.Copy` do skopiowania zawartości bloku pamięci do tablicy bajtów.</span><span class="sxs-lookup"><span data-stu-id="55f40-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="55f40-437">Na koniec blok pamięci jest zwalniany przy użyciu `Memory.Free`, a zawartość tablicy bajtowej jest wyprowadzana w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="55f40-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
