---
ms.openlocfilehash: 90001cf3d48f216787fc65e59166ec57c5d0ca34
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229932"
---
# <a name="unsafe-code"></a><span data-ttu-id="aa031-101">Niebezpieczny kod</span><span class="sxs-lookup"><span data-stu-id="aa031-101">Unsafe code</span></span>

<span data-ttu-id="aa031-102">Podstawowe języka C#, zgodnie z definicją w poprzednich rozdziałach różni się przede wszystkim od C i C++ w jej pominięcia wskaźników jako typ danych.</span><span class="sxs-lookup"><span data-stu-id="aa031-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="aa031-103">Zamiast tego, C# zawiera odwołania i możliwość tworzenia obiektów, które są zarządzane przez moduł odśmiecania pamięci.</span><span class="sxs-lookup"><span data-stu-id="aa031-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="aa031-104">Ten projekt, w połączeniu z innymi funkcjami sprawia, że C# znacznie bezpieczniejsze język niż C lub C++.</span><span class="sxs-lookup"><span data-stu-id="aa031-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="aa031-105">W języku podstawowym C# po prostu nie jest możliwe niezainicjowaną zmienną, wskaźnik "delegujące" lub wyrażenie, które indeksuje tablicy poza jej granicami.</span><span class="sxs-lookup"><span data-stu-id="aa031-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="aa031-106">Całej kategorii błędów oznacza rutynowo Plaga C i C++ programy związku z tym są eliminowane.</span><span class="sxs-lookup"><span data-stu-id="aa031-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="aa031-107">Chociaż praktycznie każdej konstrukcji typu wskaźnika w języku C lub C++ ma odpowiednika typu odwołania w języku C#, niemniej jednak istnieją sytuacje, gdzie dostęp do typów wskaźnika staje się koniecznością.</span><span class="sxs-lookup"><span data-stu-id="aa031-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="aa031-108">Na przykład komunikuje się z usługą system operacyjny, uzyskiwanie dostępu do zamapowanych w pamięci urządzenia lub Implementowanie czas ma istotne znaczenie algorytm nie może być możliwe lub praktyczne, bez dostępu do wskaźników.</span><span class="sxs-lookup"><span data-stu-id="aa031-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="aa031-109">Aby zaspokoić te potrzeby, C# zapewnia możliwość pisania ***niebezpieczny kod***.</span><span class="sxs-lookup"><span data-stu-id="aa031-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="aa031-110">W niebezpieczny kod jest to możliwe zadeklarować i wykonywać operacje na wskaźnikach do wykonywania konwersji między wskaźników i typów całkowitych, aby pobrać adres zmienne i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="aa031-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="aa031-111">W tym sensie pisania niebezpieczny kod przypomina znacznie pisania kodu języka C w ramach programu C#.</span><span class="sxs-lookup"><span data-stu-id="aa031-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="aa031-112">Niebezpieczny kod jest w rzeczywistości "bezpieczne" funkcji z punktu widzenia zarówno programistom, jak i użytkowników.</span><span class="sxs-lookup"><span data-stu-id="aa031-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="aa031-113">Niebezpieczny kod muszą być wyraźnie oznaczone za pomocą modyfikatora `unsafe`, dzięki czemu deweloperzy prawdopodobnie nie można używać funkcji niebezpieczne przypadkowo i działa aparat wykonywania, aby upewnić się, że niebezpieczny kod nie może działać w środowisku niezaufanym.</span><span class="sxs-lookup"><span data-stu-id="aa031-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="aa031-114">Konteksty unsafe</span><span class="sxs-lookup"><span data-stu-id="aa031-114">Unsafe contexts</span></span>

<span data-ttu-id="aa031-115">Niebezpieczne funkcje języka C# są dostępne tylko w kontekstach niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="aa031-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="aa031-116">Wprowadzono niebezpieczny kontekst, w tym `unsafe` modyfikatora w deklaracji typu lub elementu członkowskiego lub przez wprowadzenie *unsafe_statement*:</span><span class="sxs-lookup"><span data-stu-id="aa031-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="aa031-117">Deklaracja klasy, struktury, interfejsu lub delegata może obejmować `unsafe` modyfikator, w którym przypadku cały zakres tekstową tej deklaracji typu (w tym treść klasy, struktury lub interfejsu) jest uznawany za niebezpieczny kontekst.</span><span class="sxs-lookup"><span data-stu-id="aa031-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="aa031-118">Może zawierać deklarację pola, metoda, właściwość, zdarzenie, indeksator, operatora, konstruktora wystąpienia, destruktor lub Konstruktor statyczny `unsafe` modyfikator, w których przypadku cały zakres tekstową deklaracja składowej nie jest bezpieczne kontekst.</span><span class="sxs-lookup"><span data-stu-id="aa031-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="aa031-119">*Unsafe_statement* umożliwia użycie niebezpieczny kontekst, w ramach *bloku*.</span><span class="sxs-lookup"><span data-stu-id="aa031-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="aa031-120">Cały zakres tekstową skojarzonego *bloku* jest traktowane jako niebezpieczny kontekst.</span><span class="sxs-lookup"><span data-stu-id="aa031-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="aa031-121">Zaprojektuj skojarzone gramatyki zostały wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="aa031-121">The associated grammar productions are shown below.</span></span>

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

<span data-ttu-id="aa031-122">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="aa031-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="aa031-123">`unsafe` modyfikatora w deklaracji struktury powoduje, że cały zakres tekstową deklaracji struktury, stają się w niebezpiecznym kontekście.</span><span class="sxs-lookup"><span data-stu-id="aa031-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="aa031-124">W związku z tym, istnieje możliwość zadeklarować `Left` i `Right` pola typu wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="aa031-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="aa031-125">Również będą zapisywane w powyższym przykładzie</span><span class="sxs-lookup"><span data-stu-id="aa031-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="aa031-126">W tym miejscu `unsafe` modyfikatorów w deklaracjach pola spowodować, że te deklaracje uważane za niebezpieczne kontekstów.</span><span class="sxs-lookup"><span data-stu-id="aa031-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="aa031-127">Inne niż ustanawiania niebezpieczny kontekst, w związku z tym pozwalających na korzystanie z typów wskaźnika `unsafe` modyfikator nie ma wpływu na typu lub elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="aa031-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="aa031-128">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="aa031-128">In the example</span></span>

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

<span data-ttu-id="aa031-129">`unsafe` modyfikator na `F` method in Class metoda `A` powoduje po prostu tekstową stopnia `F` przestanie niebezpieczny kontekst, w którym można użyć niebezpiecznych funkcji języka.</span><span class="sxs-lookup"><span data-stu-id="aa031-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="aa031-130">W zastępowania metody `F` w `B`, trzeba ponownie określ `unsafe` modyfikator — chyba że, oczywiście, `F` method in Class metoda `B` sam musi mieć dostęp do funkcji niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="aa031-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="aa031-131">Sytuacja jest nieco inne w przypadku, gdy typem wskaźnika jest część podpisu metody</span><span class="sxs-lookup"><span data-stu-id="aa031-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

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

<span data-ttu-id="aa031-132">W tym miejscu ponieważ `F`firmy podpis zawiera typ wskaźnika, jego może być zapisany tylko w niebezpiecznym kontekście.</span><span class="sxs-lookup"><span data-stu-id="aa031-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="aa031-133">Jednak niebezpieczny kontekst mogą zostać wprowadzone w taki sposób, tworząc albo całej klasy niebezpieczne, tak jak w przypadku w `A`, albo załączając `unsafe` modyfikatora w deklaracji metody, jak w przypadku `B`.</span><span class="sxs-lookup"><span data-stu-id="aa031-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="aa031-134">Typy wskaźnika</span><span class="sxs-lookup"><span data-stu-id="aa031-134">Pointer types</span></span>

<span data-ttu-id="aa031-135">W niebezpiecznym kontekście *typu* ([typy](types.md)) może być *pointer_type* , a także *value_type* lub *reference_type* .</span><span class="sxs-lookup"><span data-stu-id="aa031-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="aa031-136">Jednak *pointer_type* mogą być również używane w `typeof` wyrażenia ([wyrażenia tworzenia obiektu anonimowego](expressions.md#anonymous-object-creation-expressions)) poza niebezpieczny kontekst jako takie użycie nie jest niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="aa031-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="aa031-137">A *pointer_type* jest zapisywany jako *unmanaged_type* lub słowa kluczowego `void`, a następnie `*` tokenu:</span><span class="sxs-lookup"><span data-stu-id="aa031-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="aa031-138">Typ określony przed `*` w wskaźnik typu jest nazywana ***typu Obiekt obsługujący*** typu wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="aa031-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="aa031-139">Reprezentuje typ zmiennej, na który wskazuje wartości typu wskaźnik.</span><span class="sxs-lookup"><span data-stu-id="aa031-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="aa031-140">W przeciwieństwie do odwołania (wartości typów referencyjnych) wskaźniki nie są śledzone przez moduł zbierający elementy bezużyteczne — moduł odśmiecania pamięci nie zna wskaźników i danych, które wskazują.</span><span class="sxs-lookup"><span data-stu-id="aa031-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="aa031-141">Z tego powodu wskaźnik nie jest dozwolona wskaż odwołanie lub do struktury zawierającej odwołania, a musi być typ hermetyzowanym wskaźnikiem *unmanaged_type*.</span><span class="sxs-lookup"><span data-stu-id="aa031-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="aa031-142">*Unmanaged_type* jest dowolny typ, który nie jest *reference_type* lub skonstruowany z typu, a nie zawiera *reference_type* lub skonstruowany typ pola na dowolnym poziomie zagnieżdżania.</span><span class="sxs-lookup"><span data-stu-id="aa031-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="aa031-143">Innymi słowy *unmanaged_type* jest jednym z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="aa031-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="aa031-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, lub `bool`.</span><span class="sxs-lookup"><span data-stu-id="aa031-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="aa031-145">Wszelkie *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="aa031-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="aa031-146">Wszelkie *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="aa031-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="aa031-147">Dowolny zdefiniowany przez użytkownika *struct_type* , nie jest zbudowany typ i zawiera pola *unmanaged_type*tylko s.</span><span class="sxs-lookup"><span data-stu-id="aa031-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="aa031-148">Intuicyjne reguły do mieszania wskaźników i odwołań jest referents odwołania (obiekty) mogą zawierać wskaźniki, że referents wskaźników nie mogą zawierać odwołań.</span><span class="sxs-lookup"><span data-stu-id="aa031-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="aa031-149">Niektóre przykłady typów wskaźnika są podane w poniższej tabeli:</span><span class="sxs-lookup"><span data-stu-id="aa031-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="aa031-150">__Przykład__</span><span class="sxs-lookup"><span data-stu-id="aa031-150">__Example__</span></span> | <span data-ttu-id="aa031-151">__Opis__</span><span class="sxs-lookup"><span data-stu-id="aa031-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="aa031-152">wskaźnik do `byte`</span><span class="sxs-lookup"><span data-stu-id="aa031-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="aa031-153">wskaźnik do `char`</span><span class="sxs-lookup"><span data-stu-id="aa031-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="aa031-154">Wskaźnik do wskaźnika do `int`</span><span class="sxs-lookup"><span data-stu-id="aa031-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="aa031-155">Jednowymiarowa tablica wskaźników do `int`</span><span class="sxs-lookup"><span data-stu-id="aa031-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="aa031-156">Wskaźnik do nieznanego typu</span><span class="sxs-lookup"><span data-stu-id="aa031-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="aa031-157">Dla danego wdrożenia wszystkich typów wskaźnika musi mieć ten sam rozmiar i reprezentacji.</span><span class="sxs-lookup"><span data-stu-id="aa031-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="aa031-158">W przeciwieństwie do C i C++, gdy wiele wskaźniki są deklarowane w tej samej deklaracji w języku C# `*` są zapisywane wraz z typem podstawowym, nie jako znak interpunkcyjny prefiks każdej nazwy wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="aa031-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="aa031-159">Na przykład</span><span class="sxs-lookup"><span data-stu-id="aa031-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="aa031-160">Wartość wskaźnika typu `T*` reprezentuje adres zmiennej typu `T`.</span><span class="sxs-lookup"><span data-stu-id="aa031-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="aa031-161">Operatora pośredniego wskaźnika `*` ([operację wskaźnika pośredniego](unsafe-code.md#pointer-indirection)) może służyć do dostępu do tej zmiennej.</span><span class="sxs-lookup"><span data-stu-id="aa031-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="aa031-162">Na przykład, biorąc pod uwagę zmienną `P` typu `int*`, wyrażenie `*P` oznacza `int` znaleziono pod adresem zawartym w zmiennej `P`.</span><span class="sxs-lookup"><span data-stu-id="aa031-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="aa031-163">Podobnie jak odwołanie do obiektu może być wskaźnik `null`.</span><span class="sxs-lookup"><span data-stu-id="aa031-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="aa031-164">Zastosowanie operatora pośredniego do `null` wskaźnika powoduje zachowanie zdefiniowane w implementacji.</span><span class="sxs-lookup"><span data-stu-id="aa031-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="aa031-165">Wskaźnik o wartości `null` jest reprezentowany przez wszystkie bity zero.</span><span class="sxs-lookup"><span data-stu-id="aa031-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="aa031-166">`void*` Typu reprezentuje wskaźnik do nieznanego typu.</span><span class="sxs-lookup"><span data-stu-id="aa031-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="aa031-167">Ponieważ obiekt obsługujący typ jest nieznany, nie można zastosować operatora pośredniego do wskaźnika typu `void*`, ani nie można wykonać wszystkie operacje arytmetyczne na taki wskaźnik.</span><span class="sxs-lookup"><span data-stu-id="aa031-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="aa031-168">Jednak wskaźnika typu `void*` mogą być rzutowane do jakichkolwiek innych typów wskaźnika (i na odwrót).</span><span class="sxs-lookup"><span data-stu-id="aa031-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="aa031-169">Typy wskaźników są oddzielne kategorii typów.</span><span class="sxs-lookup"><span data-stu-id="aa031-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="aa031-170">W odróżnieniu od typy odwołań i typy wartości typów wskaźnika nie dziedziczą `object` i nie występują konwersje między typami wskaźnika a `object`.</span><span class="sxs-lookup"><span data-stu-id="aa031-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="aa031-171">W szczególności, konwersja boxing i konwersja unboxing ([pakowania i rozpakowywania](types.md#boxing-and-unboxing)) nie są obsługiwane dla wskaźników.</span><span class="sxs-lookup"><span data-stu-id="aa031-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="aa031-172">Jednak konwersje są dozwolone, między różnymi typami wskaźnika oraz między typami wskaźnika a typami całkowitymi.</span><span class="sxs-lookup"><span data-stu-id="aa031-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="aa031-173">Jest to opisane w [konwersje wskaźników](unsafe-code.md#pointer-conversions).</span><span class="sxs-lookup"><span data-stu-id="aa031-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="aa031-174">A *pointer_type* nie można użyć jako argument typu ([skonstruowany typy](types.md#constructed-types)) i wnioskowanie o typie ([wnioskowanie o typie](expressions.md#type-inference)) kończy się niepowodzeniem na wywołania metody rodzajowej, które będzie mieć wywnioskowane Wpisz argument był typem wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="aa031-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="aa031-175">A *pointer_type* może być używany jako typ pola nietrwałego ([pola nietrwałego](classes.md#volatile-fields)).</span><span class="sxs-lookup"><span data-stu-id="aa031-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="aa031-176">Chociaż wskaźniki mogą być przekazywane jako `ref` lub `out` parametrów w ten sposób może spowodować niezdefiniowane zachowanie, ponieważ wskaźnik może również być równa wskaż zmiennej lokalnej, który już nie istnieje, po powrocie z metody o nazwie lub stały obiekt, do którego użyć w celu wskazania, już nie zostanie rozwiązany.</span><span class="sxs-lookup"><span data-stu-id="aa031-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="aa031-177">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="aa031-177">For example:</span></span>

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

<span data-ttu-id="aa031-178">Metoda może zwracać wartość pewnego typu, a ten typ może być wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="aa031-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="aa031-179">Na przykład, gdy wskaźnik do ciągłej sekwencji `int`s, liczba elementów w sekwencji i niektóre inne `int` wartość następującej metody zwraca adres tę wartość w tej sekwencji, jeśli wystąpi dopasowanie; w przeciwnym razie zwraca `null`:</span><span class="sxs-lookup"><span data-stu-id="aa031-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

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

<span data-ttu-id="aa031-180">W niebezpiecznym kontekście kilka konstrukcji są dostępne dla działających na wskaźnikach:</span><span class="sxs-lookup"><span data-stu-id="aa031-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="aa031-181">`*` Operator może służyć do wykonywania operację wskaźnika pośredniego ([operację wskaźnika pośredniego](unsafe-code.md#pointer-indirection)).</span><span class="sxs-lookup"><span data-stu-id="aa031-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="aa031-182">`->` Operator może służyć do uzyskania dostępu do członka struktury za pomocą wskaźnika ([dostęp do elementu członkowskiego wskaźnik](unsafe-code.md#pointer-member-access)).</span><span class="sxs-lookup"><span data-stu-id="aa031-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="aa031-183">`[]` Operator może służyć do indeksu wskaźnik ([wskaźnika elementu dostępu](unsafe-code.md#pointer-element-access)).</span><span class="sxs-lookup"><span data-stu-id="aa031-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="aa031-184">`&` Operator może być używany do uzyskiwania adresu zmiennej ([operatora address-of](unsafe-code.md#the-address-of-operator)).</span><span class="sxs-lookup"><span data-stu-id="aa031-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="aa031-185">`++` i `--` operatorzy mogą służyć do inkrementacja i dekrementacja wskaźników ([wskaźnik inkrementacja i dekrementacja](unsafe-code.md#pointer-increment-and-decrement)).</span><span class="sxs-lookup"><span data-stu-id="aa031-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="aa031-186">`+` i `-` operatorów można wykonać arytmetyki wskaźnika ([arytmetyki wskaźnika](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="aa031-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="aa031-187">`==`, `!=`, `<`, `>`, `<=`, I `=>` operatorzy mogą służyć do porównywania wskaźników ([porównanie wskaźników](unsafe-code.md#pointer-comparison)).</span><span class="sxs-lookup"><span data-stu-id="aa031-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="aa031-188">`stackalloc` Operator może służyć do przydzielania pamięci ze stosu wywołań ([stałych buforów o rozmiarze](unsafe-code.md#fixed-size-buffers)).</span><span class="sxs-lookup"><span data-stu-id="aa031-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="aa031-189">`fixed` Instrukcji może służyć do tymczasowo naprawić zmienną, dzięki czemu można uzyskać adresu ([instrukcji fixed](unsafe-code.md#the-fixed-statement)).</span><span class="sxs-lookup"><span data-stu-id="aa031-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="aa031-190">Stałe i ruchome zmiennych</span><span class="sxs-lookup"><span data-stu-id="aa031-190">Fixed and moveable variables</span></span>

<span data-ttu-id="aa031-191">Operator address-of ([operatora address-of](unsafe-code.md#the-address-of-operator)) i `fixed` — instrukcja ([instrukcji fixed](unsafe-code.md#the-fixed-statement)) dzielą zmiennych na dwie kategorie: ***Naprawiono zmienne*** i ***ruchome zmienne***.</span><span class="sxs-lookup"><span data-stu-id="aa031-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="aa031-192">Stałe zmienne znajdują się w lokalizacji przechowywania, które nie ma wpływu na działanie modułu odśmiecania pamięci.</span><span class="sxs-lookup"><span data-stu-id="aa031-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="aa031-193">(Stałe zmienne przykładami zmienne lokalne, wartości parametrów i zmiennych utworzonych przez wyłuskanie wskaźników.) Z drugiej strony zmienne ruchome znajdują się w lokalizacji przechowywania, które podlegają relokacji lub usunięcia przez moduł odśmiecania pamięci.</span><span class="sxs-lookup"><span data-stu-id="aa031-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="aa031-194">(Zmienne ruchome należą pola obiektów oraz elementów tablic.)</span><span class="sxs-lookup"><span data-stu-id="aa031-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="aa031-195">`&` — Operator ([operatora address-of](unsafe-code.md#the-address-of-operator)) pozwala na adres zmienna ustalona można uzyskać bez ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="aa031-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="aa031-196">Jednak ponieważ ruchome zmienna jest przeniesienie lub usunięcia przez moduł odśmiecania pamięci, adres zmiennej ruchome można uzyskać jedynie przy użyciu `fixed` — instrukcja ([instrukcji fixed](unsafe-code.md#the-fixed-statement)) i ten adres pozostaje ważny tylko na czas trwania tego `fixed` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="aa031-197">W dokładne warunki zmienna ustalona jest jednym z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="aa031-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="aa031-198">Zmienna wynikające z *simple_name* ([proste nazwy](expressions.md#simple-names)) odwołujący się do zmiennej lokalnej lub wartość parametru, chyba, że zmienna jest przechwytywany przez funkcję anonimową.</span><span class="sxs-lookup"><span data-stu-id="aa031-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="aa031-199">Zmienna wynikające z *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `V.I`, gdzie `V` jest stałą zmienną *struct_type*.</span><span class="sxs-lookup"><span data-stu-id="aa031-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="aa031-200">Zmienna wynikające z *pointer_indirection_expression* ([operację wskaźnika pośredniego](unsafe-code.md#pointer-indirection)) w postaci `*P`, *pointer_member_access* ([Dostęp do elementu członkowskiego wskaźnika](unsafe-code.md#pointer-member-access)) w postaci `P->I`, lub *pointer_element_access* ([wskaźnika elementu dostępu](unsafe-code.md#pointer-element-access)) w postaci `P[E]`.</span><span class="sxs-lookup"><span data-stu-id="aa031-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="aa031-201">Wszystkie inne zmienne są klasyfikowane jako ruchome zmiennych.</span><span class="sxs-lookup"><span data-stu-id="aa031-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="aa031-202">Należy zauważyć, że pole statyczne zostanie sklasyfikowany jako zmienną ruchome.</span><span class="sxs-lookup"><span data-stu-id="aa031-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="aa031-203">Należy również zauważyć, że `ref` lub `out` parametru jest klasyfikowana jako zmienną ruchome, nawet jeśli argumentu dla parametru jest zmienna ustalona.</span><span class="sxs-lookup"><span data-stu-id="aa031-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="aa031-204">Na koniec należy pamiętać, że zmienna produkowane przez wyłuskaniem wskaźnika zawsze zostanie sklasyfikowany jako zmienna ustalona.</span><span class="sxs-lookup"><span data-stu-id="aa031-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="aa031-205">Konwersje wskaźników</span><span class="sxs-lookup"><span data-stu-id="aa031-205">Pointer conversions</span></span>

<span data-ttu-id="aa031-206">W niebezpiecznym kontekście zestaw dostępnych niejawne konwersje ([niejawne konwersje](conversions.md#implicit-conversions)) jest rozszerzona, aby uwzględnić poniższe konwersje niejawne wskaźnika:</span><span class="sxs-lookup"><span data-stu-id="aa031-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="aa031-207">Za pomocą dowolnego *pointer_type* typowi `void*`.</span><span class="sxs-lookup"><span data-stu-id="aa031-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="aa031-208">Z `null` literału do dowolnego *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="aa031-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="aa031-209">Ponadto w niebezpiecznym kontekście, zestaw dostępności Konwersje jawne ([jawne konwersje](conversions.md#explicit-conversions)) jest rozszerzona, aby uwzględnić poniższe Konwersje jawne wskaźnika:</span><span class="sxs-lookup"><span data-stu-id="aa031-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="aa031-210">Za pomocą dowolnego *pointer_type* do żadnej innej *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="aa031-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="aa031-211">Z `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, lub `ulong` do dowolnego *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="aa031-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="aa031-212">Za pomocą dowolnego *pointer_type* do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, lub `ulong`.</span><span class="sxs-lookup"><span data-stu-id="aa031-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="aa031-213">Na koniec w niebezpiecznym kontekście zestaw standardowych niejawne konwersje ([standardowe i niejawne konwersje](conversions.md#standard-implicit-conversions)) obejmuje następujące konwersja wskaźnika:</span><span class="sxs-lookup"><span data-stu-id="aa031-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="aa031-214">Za pomocą dowolnego *pointer_type* typowi `void*`.</span><span class="sxs-lookup"><span data-stu-id="aa031-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="aa031-215">Konwersje między typami wskaźnika, dwa nigdy nie zmienia wartości rzeczywistej wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="aa031-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="aa031-216">Innymi słowy konwersja z typu jeden wskaźnik do innego nie ma wpływu na podstawowy adres podany przez wskaźnik.</span><span class="sxs-lookup"><span data-stu-id="aa031-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="aa031-217">Gdy jeden typ wskaźnika jest konwertowany na inny, jeśli wskaźnik wynikowy nie jest prawidłowo wyrównany dla typu wskazywanego, zachowanie jest niezdefiniowane, jeżeli wynik jest wyłuskiwany.</span><span class="sxs-lookup"><span data-stu-id="aa031-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="aa031-218">Ogólnie rzecz biorąc, pojęcie "prawidłowo wyrównane" jest przechodnia: Jeśli wskaźnik do typu `A` jest prawidłowo wyrównany dla wskaźnika do typu `B`, która z kolei jest prawidłowo wyrównany dla wskaźnika do typu `C`, następnie wskaźnik do typu `A`jest prawidłowo wyrównany dla wskaźnika do typu `C`.</span><span class="sxs-lookup"><span data-stu-id="aa031-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="aa031-219">Należy wziąć pod uwagę następujący przypadek, w którym masz jeden typ zmiennej jest dostępna za pośrednictwem wskaźnik do innego typu:</span><span class="sxs-lookup"><span data-stu-id="aa031-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="aa031-220">Po typ wskaźnika jest konwertowana na wskaźnik do bajtów, wskazuje wynik najniższą zaadresowane bajt zmiennej.</span><span class="sxs-lookup"><span data-stu-id="aa031-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="aa031-221">Kolejne przyrosty wyniku rozmiarze zmiennej, uzyskanie wskaźniki do pozostałe bajty tej zmiennej.</span><span class="sxs-lookup"><span data-stu-id="aa031-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="aa031-222">Na przykład następującą metodę wyświetla każdy z ośmiu bajtów w wartość o podwójnej precyzji jako wartość szesnastkowa:</span><span class="sxs-lookup"><span data-stu-id="aa031-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

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

<span data-ttu-id="aa031-223">Oczywiście dane wyjściowe wytwarzane zależy od kolejność bajtów.</span><span class="sxs-lookup"><span data-stu-id="aa031-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="aa031-224">Mapowania między wskaźników i liczby całkowite są zdefiniowane w implementacji.</span><span class="sxs-lookup"><span data-stu-id="aa031-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="aa031-225">Jednakże w przypadku 32 \* i architektury procesorów 64-bitowym z liniowej przestrzeni adresowej, konwersje wskaźników do i z typów całkowitych zazwyczaj zachowują się tak samo jak konwersji `uint` lub `ulong` wartości, odpowiednio do lub z tych typów całkowitych.</span><span class="sxs-lookup"><span data-stu-id="aa031-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="aa031-226">Tablice wskaźników</span><span class="sxs-lookup"><span data-stu-id="aa031-226">Pointer arrays</span></span>

<span data-ttu-id="aa031-227">W niebezpiecznym kontekście można konstruować tablice wskaźników.</span><span class="sxs-lookup"><span data-stu-id="aa031-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="aa031-228">Dozwolone są tylko niektóre z konwersji, które mają zastosowanie do innych typów tablicy na tablicach wskaźnika:</span><span class="sxs-lookup"><span data-stu-id="aa031-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="aa031-229">Niejawna konwersja odwołania ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) za pomocą dowolnego *array_type* do `System.Array` i interfejsy, implementuje również odnosi się do tablic wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="aa031-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="aa031-230">Jednak dowolne próba uzyskania dostępu do elementów tablicy przy użyciu `System.Array` lub interfejsy implementuje spowodują wyjątek w czasie wykonywania, zgodnie z typów wskaźnika nie są konwertowane na `object`.</span><span class="sxs-lookup"><span data-stu-id="aa031-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="aa031-231">Niejawne i jawne odwoływać się do konwersji ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions), [Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)) z typu tablicy jednowymiarowej `S[]` do `System.Collections.Generic.IList<T>` i jego ogólne interfejsy podstawowe nigdy nie dotyczą tablice wskaźników, ponieważ typy wskaźnika nie można użyć jako argumentów typu, a brak konwersji z typów wskaźnika do typów nie będącego wskaźnikiem.</span><span class="sxs-lookup"><span data-stu-id="aa031-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="aa031-232">Konwersja jawnego odwołania ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)) z `System.Array` i interfejsy implementuje do dowolnego *array_type* dotyczy tablice wskaźników.</span><span class="sxs-lookup"><span data-stu-id="aa031-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="aa031-233">Konwersje jawne odwołań ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)) z `System.Collections.Generic.IList<S>` i bazowej interfejsy na typ tablicy jednowymiarowej `T[]` nigdy nie dotyczy tablice wskaźników, ponieważ nie może być typów wskaźnika używane jako argumenty typu i brak konwersji z typów wskaźnika do typów nie będącego wskaźnikiem.</span><span class="sxs-lookup"><span data-stu-id="aa031-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="aa031-234">Te ograniczenia oznaczają, że rozszerzenie dla `foreach` instrukcji przez tablice opisanego w [Instrukcja foreach](statements.md#the-foreach-statement) nie można zastosować do wskaźnika tablic.</span><span class="sxs-lookup"><span data-stu-id="aa031-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="aa031-235">Zamiast tego Instrukcja foreach formularza</span><span class="sxs-lookup"><span data-stu-id="aa031-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="aa031-236">gdzie typ `x` jest typem tablicy w formie `T[,,...,]`, `N` jest liczba wymiarów, minus 1 i `T` lub `V` jest typem wskaźnika, jest rozwinięta, przy użyciu zagnieżdżonych dla pętli w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="aa031-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

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

<span data-ttu-id="aa031-237">Zmienne `a`, `i0`, `i1`,..., `iN` nie są widoczne i dostępne dla `x` lub *embedded_statement* lub inny kod źródłowy programu.</span><span class="sxs-lookup"><span data-stu-id="aa031-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="aa031-238">Zmienna `v` jest tylko do odczytu w osadzonych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="aa031-239">Jeśli nie jest jawną konwersję ([konwersje wskaźników](unsafe-code.md#pointer-conversions)) z `T` (typ elementu) do `V`, jest generowany błąd i nie trzeba wykonywać dalszych czynności są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="aa031-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="aa031-240">Jeśli `x` ma wartość `null`, `System.NullReferenceException` jest generowany w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="aa031-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="aa031-241">Wskaźniki w wyrażeniach</span><span class="sxs-lookup"><span data-stu-id="aa031-241">Pointers in expressions</span></span>

<span data-ttu-id="aa031-242">W niebezpiecznym kontekście wyrażenie może przynieść wynik typem wskaźnika, ale poza niebezpiecznym kontekście jest to błąd czasu kompilacji, wyrażenie typu wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="aa031-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="aa031-243">W dokładne warunki, poza niebezpiecznym kontekście błąd kompilacji występuje ewentualne *simple_name* ([proste nazwy](expressions.md#simple-names)), *member_access* ([dostęp do elementu członkowskiego ](expressions.md#member-access)), *invocation_expression* ([wyrażenia wywołania](expressions.md#invocation-expressions)), lub *element_access* ([dostępu elementu](expressions.md#element-access)) jest typem wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="aa031-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="aa031-244">W niebezpiecznym kontekście *primary_no_array_creation_expression* ([wyrażenia podstawowe](expressions.md#primary-expressions)) i *unary_expression* ([operatory jednoargumentowe](expressions.md#unary-operators)) Zaprojektuj zezwala na następujące konstrukcje dodatkowe:</span><span class="sxs-lookup"><span data-stu-id="aa031-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

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

<span data-ttu-id="aa031-245">W poniższych sekcjach opisano te konstrukcje.</span><span class="sxs-lookup"><span data-stu-id="aa031-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="aa031-246">Pierwszeństwo i kojarzenie operatorów unsafe jest implikowany przez gramatyki.</span><span class="sxs-lookup"><span data-stu-id="aa031-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="aa031-247">Operatora pośredniego wskaźnika</span><span class="sxs-lookup"><span data-stu-id="aa031-247">Pointer indirection</span></span>

<span data-ttu-id="aa031-248">A *pointer_indirection_expression* składa się z gwiazdką (`*`) następuje *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="aa031-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="aa031-249">Jednoargumentowy `*` operator oznacza operację wskaźnika pośredniego i służy do uzyskiwania zmiennej, na który wskazuje wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="aa031-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="aa031-250">Wynik obliczania wartości `*P`, gdzie `P` to wyrażenie typu wskaźnika `T*`, jest zmienną typu `T`.</span><span class="sxs-lookup"><span data-stu-id="aa031-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="aa031-251">Chodzi o błąd kompilacji, aby zastosować jednoargumentowe `*` operatora wyrażenia typu `void*` lub wyrażenie, które nie są typu wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="aa031-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="aa031-252">Efektem zastosowania jednoargumentowe `*` operatora `null` wskaźnik jest zdefiniowane w implementacji.</span><span class="sxs-lookup"><span data-stu-id="aa031-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="aa031-253">W szczególności, nie ma żadnej gwarancji, które zgłasza tę operację `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="aa031-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="aa031-254">Jeśli nieprawidłowa wartość przypisany do wskaźnika, zachowanie jednoargumentowe `*` operator jest niezdefiniowany.</span><span class="sxs-lookup"><span data-stu-id="aa031-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="aa031-255">Wśród nieprawidłowe wartości wyłuskaniem wskaźnika przez jednoargumentowy `*` operator jest nieodpowiednio wyrównany dla typu wskazywanego adresu (Zobacz przykład w [konwersje wskaźników](unsafe-code.md#pointer-conversions)), a adres zmiennej po zakończenie okresu jego istnienia.</span><span class="sxs-lookup"><span data-stu-id="aa031-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="aa031-256">Do celów analizy asercję określonego przypisania, zmienna utworzone przez obliczenie wyrażenia w postaci `*P` jest uznawany za wstępnie przypisanych ([początkowo przypisana zmienne](variables.md#initially-assigned-variables)).</span><span class="sxs-lookup"><span data-stu-id="aa031-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="aa031-257">Dostęp do elementu członkowskiego wskaźnika</span><span class="sxs-lookup"><span data-stu-id="aa031-257">Pointer member access</span></span>

<span data-ttu-id="aa031-258">A *pointer_member_access* składa się z *primary_expression*, a następnie "`->`" token, a następnie *identyfikator* i opcjonalnie *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="aa031-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="aa031-259">Dostępu do elementu członkowskiego wskaźnika w postaci `P->I`, `P` musi być wyrażeniem typu wskaźnika innego niż `void*`, i `I` musi oznaczają dostępny członek typu, do którego `P` punktów.</span><span class="sxs-lookup"><span data-stu-id="aa031-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="aa031-260">Dostęp do elementu członkowskiego wskaźnika w postaci `P->I` jest oceniane dokładnie jako `(*P).I`.</span><span class="sxs-lookup"><span data-stu-id="aa031-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="aa031-261">Aby uzyskać opis operatora pośredniego wskaźnika (`*`), zobacz [operację wskaźnika pośredniego](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="aa031-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="aa031-262">Aby uzyskać opis operatora dostępu do elementu członkowskiego (`.`), zobacz [dostęp do elementu członkowskiego](expressions.md#member-access).</span><span class="sxs-lookup"><span data-stu-id="aa031-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="aa031-263">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="aa031-263">In the example</span></span>

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

<span data-ttu-id="aa031-264">`->` operator jest używany, aby uzyskać dostęp do pól i wywołać metodę struktury za pomocą wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="aa031-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="aa031-265">Ponieważ operacja `P->I` odpowiada dokładnie `(*P).I`, `Main` metoda można tak samo dobrze zarówno zapisanych:</span><span class="sxs-lookup"><span data-stu-id="aa031-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

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

### <a name="pointer-element-access"></a><span data-ttu-id="aa031-266">Dostęp do elementu wskaźnika</span><span class="sxs-lookup"><span data-stu-id="aa031-266">Pointer element access</span></span>

<span data-ttu-id="aa031-267">A *pointer_element_access* składa się z *primary_no_array_creation_expression* występować wyrażenie w "`[`"i"`]`".</span><span class="sxs-lookup"><span data-stu-id="aa031-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="aa031-268">Dostępu do elementu wskaźnika w postaci `P[E]`, `P` musi być wyrażeniem typu wskaźnika innego niż `void*`, i `E` musi być wyrażeniem, które mogą być niejawnie konwertowane na `int`, `uint`, `long`, lub `ulong`.</span><span class="sxs-lookup"><span data-stu-id="aa031-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="aa031-269">Dostęp do elementu wskaźnika w postaci `P[E]` jest oceniane dokładnie jako `*(P + E)`.</span><span class="sxs-lookup"><span data-stu-id="aa031-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="aa031-270">Aby uzyskać opis operatora pośredniego wskaźnika (`*`), zobacz [operację wskaźnika pośredniego](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="aa031-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="aa031-271">Opis wskaźnika operator dodawania (`+`), zobacz [arytmetyki wskaźnika](unsafe-code.md#pointer-arithmetic).</span><span class="sxs-lookup"><span data-stu-id="aa031-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="aa031-272">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="aa031-272">In the example</span></span>

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

<span data-ttu-id="aa031-273">dostęp element wskaźnik jest wykorzystywany do inicjacji buforu znaku w `for` pętli.</span><span class="sxs-lookup"><span data-stu-id="aa031-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="aa031-274">Ponieważ operacja `P[E]` odpowiada dokładnie `*(P + E)`, przykładu można tak samo dobrze zarówno zapisanych:</span><span class="sxs-lookup"><span data-stu-id="aa031-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

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

<span data-ttu-id="aa031-275">Operator dostępu do elementu wskaźnik nie sprawdza obecności liczbach błędów i zachowanie podczas uzyskiwania dostępu do liczbach element jest niezdefiniowany.</span><span class="sxs-lookup"><span data-stu-id="aa031-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="aa031-276">To jest taka sama jak C i C++.</span><span class="sxs-lookup"><span data-stu-id="aa031-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="aa031-277">Operator address-of</span><span class="sxs-lookup"><span data-stu-id="aa031-277">The address-of operator</span></span>

<span data-ttu-id="aa031-278">*Addressof_expression* składa się z handlowe "i" (`&`) następuje *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="aa031-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="aa031-279">Podane wyrażenie `E` jest typu `T` i zostanie sklasyfikowany jako zmienna ustalona ([stałe i zmienne ruchome](unsafe-code.md#fixed-and-moveable-variables)), konstrukcja `&E` oblicza adres zmiennej przez `E`.</span><span class="sxs-lookup"><span data-stu-id="aa031-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="aa031-280">Typ wyniku jest `T*` i zostanie sklasyfikowany jako wartość.</span><span class="sxs-lookup"><span data-stu-id="aa031-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="aa031-281">Występuje błąd kompilacji, jeśli `E` nie jest sklasyfikowany jako zmiennej, jeśli `E` zostanie sklasyfikowany jako tylko do odczytu zmiennej lokalnej, lub jeśli `E` oznacza ruchome zmiennej.</span><span class="sxs-lookup"><span data-stu-id="aa031-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="aa031-282">W przypadku ostatniej instrukcji fixed ([instrukcji fixed](unsafe-code.md#the-fixed-statement)) może służyć do tymczasowego "naprawić" zmiennej przed uzyskaniem adresu.</span><span class="sxs-lookup"><span data-stu-id="aa031-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="aa031-283">Jak wspomniano w [dostęp do elementu członkowskiego](expressions.md#member-access), poza konstruktora wystąpienia lub Konstruktor statyczny dla elementu struktury lub klasy, która definiuje `readonly` pola, to pole jest uważany za wartość, a nie zmienną.</span><span class="sxs-lookup"><span data-stu-id="aa031-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="aa031-284">W efekcie adresu nie może być przyjęty.</span><span class="sxs-lookup"><span data-stu-id="aa031-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="aa031-285">Podobnie nie może być przyjęty adres stałą.</span><span class="sxs-lookup"><span data-stu-id="aa031-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="aa031-286">`&` Operator nie wymaga jej argument zostanie ostatecznie przypisane, ale następujące `&` operacji zmiennej, do którego jest stosowany operator jest uznawany za zdecydowanie przypisane w ścieżce wykonywania, w której występuje operacji.</span><span class="sxs-lookup"><span data-stu-id="aa031-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="aa031-287">Jest to faktycznie odpowiedzialność programisty, aby upewnić się, że poprawne inicjowanie zmiennej miały miejsce w tej sytuacji.</span><span class="sxs-lookup"><span data-stu-id="aa031-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="aa031-288">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="aa031-288">In the example</span></span>

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

<span data-ttu-id="aa031-289">`i` jest uznawany za zdecydowanie przypisany następujące `&i` operacji używane do zainicjowania `p`.</span><span class="sxs-lookup"><span data-stu-id="aa031-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="aa031-290">Przypisanie do `*p` obowiązuje inicjuje `i`, ale włączenie ten proces inicjowania jest odpowiedzialny za programisty i błąd kompilacji nie może wystąpić, jeśli usunięto przypisanie.</span><span class="sxs-lookup"><span data-stu-id="aa031-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="aa031-291">Reguły asercję określonego przypisania dla `&` operator istnieje w taki sposób, że można uniknąć nadmiarowe inicjowanie zmiennych lokalnych.</span><span class="sxs-lookup"><span data-stu-id="aa031-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="aa031-292">Na przykład wiele zewnętrzne interfejsy API zająć wskaźnik do struktury, która jest wypełniane przez interfejs API.</span><span class="sxs-lookup"><span data-stu-id="aa031-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="aa031-293">Wywołania tych interfejsów API, zazwyczaj pass adresu zmiennej lokalnej struktury i bez reguły, nadmiarowe inicjowanie zmiennej struktury jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="aa031-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="aa031-294">Wskaźnik inkrementacja i dekrementacja</span><span class="sxs-lookup"><span data-stu-id="aa031-294">Pointer increment and decrement</span></span>

<span data-ttu-id="aa031-295">W niebezpiecznym kontekście `++` i `--` operatorów ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators) i [przedrostkowe i operatory dekrementacji](expressions.md#prefix-increment-and-decrement-operators)) mogą być stosowane do wskaźnika zmienne wszystkich typów z wyjątkiem `void*`.</span><span class="sxs-lookup"><span data-stu-id="aa031-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="aa031-296">W związku z tym, dla każdego typu wskaźnika `T*`, niejawnie zdefiniowano następujące operatory:</span><span class="sxs-lookup"><span data-stu-id="aa031-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="aa031-297">Operatory działają tak samo jak `x + 1` i `x - 1`odpowiednio ([arytmetyki wskaźnika](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="aa031-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="aa031-298">Innymi słowy, do zmiennej wskaźnika typu `T*`, `++` operator dodaje `sizeof(T)` do adresem zawartym w zmiennej, a `--` odejmuje operator `sizeof(T)` z adresem zawartym w zmiennej.</span><span class="sxs-lookup"><span data-stu-id="aa031-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="aa031-299">Jeśli wskaźnik inkrementacyjna lub dekrementacyjna przepełnienie operacji domeny typu wskaźnika, wynik zależy od implementacji, ale są tworzone bez wyjątków.</span><span class="sxs-lookup"><span data-stu-id="aa031-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="aa031-300">Arytmetyczny wskaźnik</span><span class="sxs-lookup"><span data-stu-id="aa031-300">Pointer arithmetic</span></span>

<span data-ttu-id="aa031-301">W niebezpiecznym kontekście `+` i `-` operatorów ([operator dodawania](expressions.md#addition-operator) i [operator odejmowania](expressions.md#subtraction-operator)) mogą być stosowane do wartości wszystkich typów wskaźnika, z wyjątkiem `void*`.</span><span class="sxs-lookup"><span data-stu-id="aa031-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="aa031-302">W związku z tym, dla każdego typu wskaźnika `T*`, niejawnie zdefiniowano następujące operatory:</span><span class="sxs-lookup"><span data-stu-id="aa031-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

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

<span data-ttu-id="aa031-303">Podane wyrażenie `P` typu wskaźnika `T*` i wyrażenie `N` typu `int`, `uint`, `long`, lub `ulong`, wyrażenia `P + N` i `N + P` obliczeń wartość wskaźnika typu `T*` , wynikiem dodawania `N * sizeof(T)` adres podany przez `P`.</span><span class="sxs-lookup"><span data-stu-id="aa031-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="aa031-304">Podobnie, wyrażenie `P - N` oblicza wartość wskaźnika typu `T*` , wynikiem odejmowania `N * sizeof(T)` z adresem podanym przez `P`.</span><span class="sxs-lookup"><span data-stu-id="aa031-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="aa031-305">Biorąc pod uwagę dwa wyrażenia `P` i `Q`, typu wskaźnika `T*`, wyrażenie `P - Q` oblicza różnicę między adresy podane przez `P` i `Q` a następnie podzielenie tej różnicy przez `sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="aa031-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="aa031-306">Typ wyniku jest zawsze `long`.</span><span class="sxs-lookup"><span data-stu-id="aa031-306">The type of the result is always `long`.</span></span> <span data-ttu-id="aa031-307">W efekcie `P - Q` jest obliczana jako `((long)(P) - (long)(Q)) / sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="aa031-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="aa031-308">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="aa031-308">For example:</span></span>

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

<span data-ttu-id="aa031-309">która generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="aa031-309">which produces the output:</span></span>

```
p - q = -14
q - p = 14
```

<span data-ttu-id="aa031-310">Jeśli operacji arytmetycznych wskaźnika przepełnienia domeny typu wskaźnika, wynik został obcięty w sposób zdefiniowanych w implementacji, ale są tworzone bez wyjątków.</span><span class="sxs-lookup"><span data-stu-id="aa031-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="aa031-311">Porównanie wskaźników</span><span class="sxs-lookup"><span data-stu-id="aa031-311">Pointer comparison</span></span>

<span data-ttu-id="aa031-312">W niebezpiecznym kontekście `==`, `!=`, `<`, `>`, `<=`, i `=>` operatorów ([relacyjne i badania typu operatory](expressions.md#relational-and-type-testing-operators)) mogą być stosowane do wszystkich wartości typy wskaźników.</span><span class="sxs-lookup"><span data-stu-id="aa031-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="aa031-313">Dostępne są następujące operatory porównania wskaźnika:</span><span class="sxs-lookup"><span data-stu-id="aa031-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="aa031-314">Ponieważ istnieje niejawna konwersja z dowolnego typu wskaźnika do `void*` operandów dowolny typ wskaźnika typu można porównać przy użyciu tych operatorów.</span><span class="sxs-lookup"><span data-stu-id="aa031-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="aa031-315">Operatory porównania porównują adresy podane przez dwa operandy tak, jakby były one liczb całkowitych bez znaku.</span><span class="sxs-lookup"><span data-stu-id="aa031-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="aa031-316">Sizeof — operator</span><span class="sxs-lookup"><span data-stu-id="aa031-316">The sizeof operator</span></span>

<span data-ttu-id="aa031-317">`sizeof` Operator zwraca liczbę bajtów zajmowane przez zmienną danego typu.</span><span class="sxs-lookup"><span data-stu-id="aa031-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="aa031-318">Typ określony jako argument `sizeof` musi być *unmanaged_type* ([typy wskaźników](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="aa031-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="aa031-319">Wynik `sizeof` operator jest wartość typu `int`.</span><span class="sxs-lookup"><span data-stu-id="aa031-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="aa031-320">W przypadku niektórych wstępnie zdefiniowanych typów, `sizeof` operator daje stałą wartość, jak pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="aa031-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="aa031-321">__Expression__</span><span class="sxs-lookup"><span data-stu-id="aa031-321">__Expression__</span></span>   | <span data-ttu-id="aa031-322">__wynik__</span><span class="sxs-lookup"><span data-stu-id="aa031-322">__Result__</span></span> |
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

<span data-ttu-id="aa031-323">W przypadku wszystkich innych typów wynik `sizeof` operator jest zdefiniowane w implementacji i zostanie sklasyfikowany jako wartość, a nie jako stała.</span><span class="sxs-lookup"><span data-stu-id="aa031-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="aa031-324">Kolejność, w której elementy członkowskie są pakowane w struktury jest nieokreślona.</span><span class="sxs-lookup"><span data-stu-id="aa031-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="aa031-325">Do celów wyrównanie może istnieć bez nazwy, wypełnienie na początku struktury, w ramach struktury, a na koniec struktury.</span><span class="sxs-lookup"><span data-stu-id="aa031-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="aa031-326">Zawartość usługi bits, używane jako uzupełnienia jest nieokreślony.</span><span class="sxs-lookup"><span data-stu-id="aa031-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="aa031-327">Po zastosowaniu do argumentu operacji, która ma typ struktury, wynik jest całkowita liczba bajtów w zmiennej tego typu, w tym wszelkie dopełnienie.</span><span class="sxs-lookup"><span data-stu-id="aa031-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="aa031-328">Fixed — instrukcja</span><span class="sxs-lookup"><span data-stu-id="aa031-328">The fixed statement</span></span>

<span data-ttu-id="aa031-329">W niebezpiecznym kontekście *embedded_statement* ([instrukcji](statements.md)) produkcji zezwala na dodatkowe konstrukcji `fixed` instrukcję, która służy do "naprawić" ruchomych zmiennej taki sposób, że jego adres pozostaje stały czas trwania instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

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

<span data-ttu-id="aa031-330">Każdy *fixed_pointer_declarator* deklaruje zmienną lokalną o danym *pointer_type* i inicjuje tej zmiennej lokalnej o adresie obliczane przez odpowiednie *fixed_ pointer_initializer*.</span><span class="sxs-lookup"><span data-stu-id="aa031-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="aa031-331">Zmienna lokalna zadeklarowana w `fixed` instrukcji jest dostępny we wszystkich *fixed_pointer_initializer*s pojawiają się po prawej stronie deklaracji tej zmiennej, a w polu *embedded_statement* programu `fixed` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="aa031-332">Zmienna lokalna zadeklarowana przez `fixed` instrukcji jest traktowane jako tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="aa031-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="aa031-333">Występuje błąd kompilacji, jeśli osadzona instrukcja próbuje zmodyfikować tej zmiennej lokalnej (za pośrednictwem przydziału lub `++` i `--` operatory) lub przekazać je jako `ref` lub `out` parametru.</span><span class="sxs-lookup"><span data-stu-id="aa031-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="aa031-334">A *fixed_pointer_initializer* może być jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="aa031-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="aa031-335">Token "`&`" następuje *variable_reference* ([dokładne zasady określania asercję określonego przypisania](variables.md#precise-rules-for-determining-definite-assignment)) ze zmienną ruchome ([stałe i zmienne ruchome](unsafe-code.md#fixed-and-moveable-variables)) niezarządzany typ `T`, podany typ `T*` jest niejawnie konwertowany na typ wskaźnika, podany w `fixed` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="aa031-336">W tym przypadku inicjatora oblicza adres zmiennej danego, a zmienna jest gwarantowane, pozostają do ustalonego adresu na czas trwania `fixed` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="aa031-337">Wyrażenie *array_type* elementami typu niezarządzanego `T`, podany typ `T*` jest niejawnie konwertowany na typ wskaźnika, podany w `fixed` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="aa031-338">W tym przypadku inicjatora oblicza adres do pierwszego elementu w tablicy, a całej tablicy jest gwarantowane, pozostają do ustalonego adresu na czas trwania `fixed` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="aa031-339">Jeśli wyrażenie tablicy ma wartość null lub jeśli tablica nie zawiera żadnego elementu, inicjator oblicza adresów równy zero.</span><span class="sxs-lookup"><span data-stu-id="aa031-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="aa031-340">Wyrażenie typu `string`, podany typ `char*` jest niejawnie konwertowany na typ wskaźnika, podany w `fixed` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="aa031-341">W tym przypadku inicjatora oblicza adres pierwszego wystąpienia znaku w ciągu, a cały ciąg jest gwarantowane, pozostają do ustalonego adresu na czas trwania `fixed` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="aa031-342">Zachowanie `fixed` instrukcji jest zdefiniowane w implementacji Jeśli wyrażenie ciągu ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="aa031-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="aa031-343">A *simple_name* lub *member_access* , odwołuje się do elementu członkowskiego buforu o ustalonym rozmiarze zmiennej ruchome podany typ elementu członkowskiego buforu o ustalonym rozmiarze jest niejawnie konwertowany na typ wskaźnika, biorąc pod uwagę w `fixed` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="aa031-344">W tym przypadku inicjatora oblicza wskaźnik do pierwszego elementu bufor o ustalonym rozmiarze ([stałych buforów rozmiar w wyrażeniach](unsafe-code.md#fixed-size-buffers-in-expressions)), i bufor o ustalonym rozmiarze musi pozostać do ustalonego adresu na czas trwania `fixed`instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="aa031-345">Dla każdego adresu, obliczona przez *fixed_pointer_initializer* `fixed` instrukcji zapewnia, że zmienna odwołuje się adres nie jest przeniesienie lub usunięcia przez moduł odśmiecania pamięci w czasie trwania `fixed` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="aa031-346">Na przykład, jeśli adres jest obliczana przez *fixed_pointer_initializer* odwołuje się do pola obiektu lub element instancji `fixed` instrukcji gwarantuje, że zawierające wystąpienie obiektu nie został przeniesiony lub usuwane w okresie istnienia instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="aa031-347">Jest odpowiedzialny za programisty, aby upewnić się, że wskaźniki utworzonych przez `fixed` instrukcje nie przeżywa poza wykonanie tych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="aa031-348">Na przykład, gdy wskaźniki utworzony przez `fixed` instrukcje są przekazywane do zewnętrzne interfejsy API, jest programisty ponosić odpowiedzialność za zapewnienie zachowanie za pomocą interfejsów API Brak pamięci te wskaźniki.</span><span class="sxs-lookup"><span data-stu-id="aa031-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="aa031-349">Naprawiono obiektów może spowodować fragmentację sterty (ponieważ nie można ich przenieść).</span><span class="sxs-lookup"><span data-stu-id="aa031-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="aa031-350">Z tego powodu należy ustalić obiektów tylko wtedy, gdy jest to absolutnie konieczne i następnie dla najkrótszej ilość czasu, to możliwe.</span><span class="sxs-lookup"><span data-stu-id="aa031-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="aa031-351">Przykład</span><span class="sxs-lookup"><span data-stu-id="aa031-351">The example</span></span>

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

<span data-ttu-id="aa031-352">Pokazuje kilka zastosowań `fixed` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="aa031-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="aa031-353">Pierwsza instrukcja poprawki i uzyskuje adres pole statyczne, druga instrukcja poprawki i uzyskuje adres pola wystąpienia i trzecią instrukcję poprawki i uzyskuje adres elementu tablicy.</span><span class="sxs-lookup"><span data-stu-id="aa031-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="aa031-354">W każdym przypadku byłoby błędem jest użycie zwykłych `&` operatora, ponieważ zmienne są klasyfikowane jako ruchome zmienne.</span><span class="sxs-lookup"><span data-stu-id="aa031-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="aa031-355">Czwarty `fixed` instrukcja w powyższym przykładzie generuje podobny efekt, w trzeciej.</span><span class="sxs-lookup"><span data-stu-id="aa031-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="aa031-356">Ten przykład `fixed` używa instrukcji `string`:</span><span class="sxs-lookup"><span data-stu-id="aa031-356">This example of the `fixed` statement uses `string`:</span></span>

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

<span data-ttu-id="aa031-357">W niebezpiecznym kontekście elementów tablicy jednowymiarowej tablic są przechowywane w kolejności rosnącej indeksu, począwszy od indeksu `0` i kończąc indeksu `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="aa031-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="aa031-358">Dla wielowymiarowych tablic, tablicy, które elementy są przechowywane w taki sposób, że indeksy po prawej stronie wymiaru zwiększa się najpierw, a następnie następnego left wymiaru i tak dalej po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="aa031-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="aa031-359">W ramach `fixed` instrukcję, która uzyskuje wskaźnik `p` na wystąpienie tablicy `a`, wartości wskaźnika, począwszy od `p` do `p + a.Length - 1` reprezentują adresy elementów w tablicy.</span><span class="sxs-lookup"><span data-stu-id="aa031-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="aa031-360">Podobnie, zmienne w zakresie od `p[0]` do `p[a.Length - 1]` reprezentują elementy bieżącej tablicy.</span><span class="sxs-lookup"><span data-stu-id="aa031-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="aa031-361">Biorąc pod uwagę sposób, w którym przechowywane są tablicami, mogą traktujemy tablicę żadnego wymiaru tak, jakby była liniowego.</span><span class="sxs-lookup"><span data-stu-id="aa031-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="aa031-362">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="aa031-362">For example:</span></span>

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

<span data-ttu-id="aa031-363">która generuje dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="aa031-363">which produces the output:</span></span>

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="aa031-364">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="aa031-364">In the example</span></span>

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

<span data-ttu-id="aa031-365">`fixed` instrukcja zostaje użyty do naprawy tablicy, dzięki czemu jego adresu może być przekazywany do metody, która przyjmuje wskaźnik.</span><span class="sxs-lookup"><span data-stu-id="aa031-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="aa031-366">W przykładzie:</span><span class="sxs-lookup"><span data-stu-id="aa031-366">In the example:</span></span>

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

<span data-ttu-id="aa031-367">fixed — instrukcja zostaje użyty do naprawy bufor o ustalonym rozmiarze, struktury, dzięki czemu jego adresu może służyć jako wskaźnik.</span><span class="sxs-lookup"><span data-stu-id="aa031-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="aa031-368">A `char*` wartości generowane przez rozwiązywanie wystąpieniu ciągu zawsze wskazuje na ciąg zakończony znakiem null.</span><span class="sxs-lookup"><span data-stu-id="aa031-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="aa031-369">W instrukcji fixed, który uzyskuje wskaźnik `p` do wystąpienia ciągu `s`, wartości wskaźnika, począwszy od `p` do `p + s.Length - 1` reprezentują adresy znaków w ciągu i wartość wskaźnika `p + s.Length` zawsze wskazuje znak null (znak wartości `'\0'`).</span><span class="sxs-lookup"><span data-stu-id="aa031-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="aa031-370">Modyfikowanie obiektów typu zarządzanego przez stały wskaźniki mogą powoduje niezdefiniowane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="aa031-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="aa031-371">Na przykład ponieważ ciągów są niezmienne, to programisty ponosić odpowiedzialność za zapewnienie znaków odwołuje się wskaźnik do ciągu stałych nie są modyfikowane.</span><span class="sxs-lookup"><span data-stu-id="aa031-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="aa031-372">Automatyczne zakończenia null ciągów jest szczególnie przydatna podczas wywoływania zewnętrzne interfejsy API, który oczekiwać ciągi "Stylu C".</span><span class="sxs-lookup"><span data-stu-id="aa031-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="aa031-373">Należy jednak zauważyć, że wystąpieniu ciągu może zawierać znaków o wartości null.</span><span class="sxs-lookup"><span data-stu-id="aa031-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="aa031-374">Jeśli takie znaki null są zainstalowane, ciąg pojawi obcięte traktowane jako zakończony znakiem null `char*`.</span><span class="sxs-lookup"><span data-stu-id="aa031-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="aa031-375">Bufory o ustalonym rozmiarze</span><span class="sxs-lookup"><span data-stu-id="aa031-375">Fixed size buffers</span></span>

<span data-ttu-id="aa031-376">Bufory o ustalonym rozmiarze są używane do deklarowania "Stylu C" w tekście tablice jako elementy członkowskie struktury i są szczególnie przydatne w przypadku powiązania z niezarządzanych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="aa031-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="aa031-377">Deklaracje buforu o ustalonym rozmiarze</span><span class="sxs-lookup"><span data-stu-id="aa031-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="aa031-378">A ***ustalony rozmiar buforu*** jest element członkowski, który reprezentuje magazyn dla bufora o stałej długości zmiennych danego typu.</span><span class="sxs-lookup"><span data-stu-id="aa031-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="aa031-379">Deklaracja buforu o ustalonym rozmiarze wprowadza bufory o ustalonym rozmiarze co najmniej jeden element danego typu.</span><span class="sxs-lookup"><span data-stu-id="aa031-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="aa031-380">Bufory o ustalonym rozmiarze są dozwolone tylko w deklaracjach struktury i mogą występować tylko w kontekstach unsafe ([niebezpieczny kontekst](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="aa031-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

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

<span data-ttu-id="aa031-381">Deklaracja buforu o ustalonym rozmiarze mogą obejmować zestaw atrybutów ([atrybuty](attributes.md)), `new` modyfikator ([Modyfikatory](classes.md#modifiers)), prawidłowej kombinacji modyfikatory dostępu cztery ([typu Parametry i ograniczenia](classes.md#type-parameters-and-constraints)) i `unsafe` modyfikator ([niebezpieczny kontekst](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="aa031-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="aa031-382">Atrybuty i Modyfikatory mają zastosowanie do wszystkich elementów członkowskich zadeklarowanych przez deklarację buforu o ustalonym rozmiarze.</span><span class="sxs-lookup"><span data-stu-id="aa031-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="aa031-383">Jest to błąd dla tego samego modyfikator pojawią się wiele razy w deklaracji buforu o ustalonym rozmiarze.</span><span class="sxs-lookup"><span data-stu-id="aa031-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="aa031-384">Deklaracja buforu o ustalonym rozmiarze jest niedozwolona w obejmują `static` modyfikator.</span><span class="sxs-lookup"><span data-stu-id="aa031-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="aa031-385">Typ elementu buforu deklaracji buforu o ustalonym rozmiarze Określa typ elementu bufory wprowadzonych przez tę deklarację.</span><span class="sxs-lookup"><span data-stu-id="aa031-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="aa031-386">Typ elementu buforu musi być jednym z wstępnie zdefiniowanych typów `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, lub `bool`.</span><span class="sxs-lookup"><span data-stu-id="aa031-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="aa031-387">Typ elementu buforu następuje lista deklaratorów buforu o ustalonym rozmiarze, każdy z nich wprowadzono nowy element członkowski.</span><span class="sxs-lookup"><span data-stu-id="aa031-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="aa031-388">Deklarator buforu o ustalonym rozmiarze składa się z identyfikatora i nazwy elementu członkowskiego, następuje ujęte w wyrażeniu stałym `[` i `]` tokenów.</span><span class="sxs-lookup"><span data-stu-id="aa031-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="aa031-389">Wyrażenie stałe wskazuje, że liczba elementów w elemencie członkowskim, wynikające z tego deklaratora buforu o ustalonym rozmiarze.</span><span class="sxs-lookup"><span data-stu-id="aa031-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="aa031-390">Typ stałego wyrażenia musi być jawnie przekonwertować na typ `int`, a wartość musi być dodatnią liczbą całkowitą od zera.</span><span class="sxs-lookup"><span data-stu-id="aa031-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="aa031-391">Elementy bufor o ustalonym rozmiarze są musi być określone sekwencyjnie w pamięci.</span><span class="sxs-lookup"><span data-stu-id="aa031-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="aa031-392">Deklaracja buforu o ustalonym rozmiarze, która deklaruje wiele bufory o ustalonym rozmiarze jest odpowiednikiem w wielu deklaracjach deklarację buforu jednego o stałym rozmiarze z tych atrybutów i typy elementów.</span><span class="sxs-lookup"><span data-stu-id="aa031-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="aa031-393">Na przykład</span><span class="sxs-lookup"><span data-stu-id="aa031-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="aa031-394">odpowiada wyrażeniu</span><span class="sxs-lookup"><span data-stu-id="aa031-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="aa031-395">Bufory o ustalonym rozmiarze w wyrażeniach</span><span class="sxs-lookup"><span data-stu-id="aa031-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="aa031-396">Wyszukiwanie elementu członkowskiego ([operatory](expressions.md#operators)) o stałym rozmiarze buforu element członkowski będzie kontynuowane tak samo jak wyszukać składowej pola.</span><span class="sxs-lookup"><span data-stu-id="aa031-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="aa031-397">Bufor o ustalonym rozmiarze można odwoływać się do usługi za pomocą wyrażenia *simple_name* ([wnioskowanie o typie](expressions.md#type-inference)) lub *member_access* ([Sprawdzanie czasu kompilacji Rozpoznanie przeciążenia dynamiczne](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="aa031-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="aa031-398">Gdy członek buforu o ustalonym rozmiarze jest określany jako prostą nazwą, efekt jest taki sam jak dostęp do elementu członkowskiego w postaci `this.I`, gdzie `I` należy buforu o ustalonym rozmiarze.</span><span class="sxs-lookup"><span data-stu-id="aa031-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="aa031-399">Dostępu do elementu członkowskiego w postaci `E.I`, jeśli `E` jest typ struktury i wyszukiwania elementu członkowskiego `I` , następnie identyfikuje typ struktury elementu członkowskiego o stałym rozmiarze, `E.I` jest obliczane sklasyfikowanych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="aa031-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="aa031-400">Jeśli wyrażenie `E.I` nie występuje w niebezpiecznym kontekście, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="aa031-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="aa031-401">Jeśli `E` jest klasyfikowana jako wartość, wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="aa031-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="aa031-402">W przeciwnym razie, jeśli `E` jest zmienną ruchome ([stałe i zmienne ruchome](unsafe-code.md#fixed-and-moveable-variables)) i wyrażenie `E.I` nie *fixed_pointer_initializer* ([stałe Instrukcja](unsafe-code.md#the-fixed-statement)), wystąpi błąd kompilacji.</span><span class="sxs-lookup"><span data-stu-id="aa031-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="aa031-403">W przeciwnym razie `E` odwołuje się zmienna ustalona i wynik wyrażenia to wskaźnik do pierwszego elementu członkowskiego buforu o ustalonym rozmiarze `I` w `E`.</span><span class="sxs-lookup"><span data-stu-id="aa031-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="aa031-404">Wynik jest typu `S*`, gdzie `S` jest typ elementu `I`i jest klasyfikowana jako wartość.</span><span class="sxs-lookup"><span data-stu-id="aa031-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="aa031-405">Kolejne elementy buforu o ustalonym rozmiarze można uzyskać dostęp przy użyciu operacji wskaźnika od pierwszego elementu.</span><span class="sxs-lookup"><span data-stu-id="aa031-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="aa031-406">W odróżnieniu od dostęp do tablic dostęp do elementów bufor o ustalonym rozmiarze niebezpieczna operacja i nie jest zaznaczone zakres.</span><span class="sxs-lookup"><span data-stu-id="aa031-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="aa031-407">Poniższy przykład deklaruje i wykorzystuje struktury ze składową buforu o ustalonym rozmiarze.</span><span class="sxs-lookup"><span data-stu-id="aa031-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

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

### <a name="definite-assignment-checking"></a><span data-ttu-id="aa031-408">Sprawdzanie asercję określonego przypisania</span><span class="sxs-lookup"><span data-stu-id="aa031-408">Definite assignment checking</span></span>

<span data-ttu-id="aa031-409">Bufory o ustalonym rozmiarze nie są obciążane sprawdzanie asercję określonego przypisania ([asercję określonego przypisania](variables.md#definite-assignment)), a członkowie buforu o ustalonym rozmiarze są ignorowane na potrzeby asercję określonego przypisania, sprawdzanie zmiennych typu struktury.</span><span class="sxs-lookup"><span data-stu-id="aa031-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="aa031-410">Gdy peryferyjnych zawierającego zmiennej struktury elementu członkowskiego buforu o ustalonym rozmiarze jest zmienną statyczną, zmienną instance wystąpienia klasy lub elementu tablicy, elementy bufor o ustalonym rozmiarze są automatycznie inicjowane do wartości domyślnych ([Wartości domyślne](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="aa031-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="aa031-411">We wszystkich innych przypadkach oryginalnej zawartości buforu o stałym rozmiarze jest niezdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="aa031-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="aa031-412">Alokacja stosu</span><span class="sxs-lookup"><span data-stu-id="aa031-412">Stack allocation</span></span>

<span data-ttu-id="aa031-413">W niebezpiecznym kontekście deklaracji zmiennej lokalnej ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)) może zawierać inicjatora alokacji stosu, który przydziela pamięć ze stosu wywołań.</span><span class="sxs-lookup"><span data-stu-id="aa031-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="aa031-414">*Unmanaged_type* wskazuje typ elementów, które będą przechowywane w lokalizacji nowo przydzielonego i *wyrażenie* wskazuje liczbę tych elementów.</span><span class="sxs-lookup"><span data-stu-id="aa031-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="aa031-415">Razem te Określ rozmiar alokacji wymagane.</span><span class="sxs-lookup"><span data-stu-id="aa031-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="aa031-416">Ponieważ rozmiar alokacji stosu nie może być ujemna, jest to błąd kompilacji, aby określić liczbę elementów jako *constant_expression* ma wartość ujemną.</span><span class="sxs-lookup"><span data-stu-id="aa031-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="aa031-417">Inicjator alokacji stosu w postaci `stackalloc T[E]` wymaga `T` na typ niezarządzany ([typy wskaźników](unsafe-code.md#pointer-types)) i `E` jako wyrażenie typu `int`.</span><span class="sxs-lookup"><span data-stu-id="aa031-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="aa031-418">Konstrukcja przydziela `E * sizeof(T)` bajtów z wywołanie stosu i zwraca wskaźnik typu `T*`, do nowo przydzielonego bloku.</span><span class="sxs-lookup"><span data-stu-id="aa031-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="aa031-419">Jeśli `E` jest liczbą ujemną, a następnie zachowanie jest niezdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="aa031-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="aa031-420">Jeśli `E` wynosi zero, a następnie odbywa się nie przydział i zwrócony wskaźnik jest zdefiniowane w implementacji.</span><span class="sxs-lookup"><span data-stu-id="aa031-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="aa031-421">Jeśli nie ma wystarczającej ilości pamięci do przydzielenia bloku o danym rozmiarze `System.StackOverflowException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="aa031-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="aa031-422">Zawartość nowo przydzielonego pamięci jest niezdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="aa031-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="aa031-423">Inicjatory alokacji stosu nie są dozwolone w `catch` lub `finally` bloków ([instrukcjami "try"](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="aa031-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="aa031-424">Nie istnieje sposób jawnego zwolnienie pamięci przydzielonej za pomocą `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="aa031-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="aa031-425">Wszystkie bloki pamięci na stosie, utworzony podczas wykonywania funkcji elementu członkowskiego są automatycznie odrzucane po powrocie z tej funkcji elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="aa031-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="aa031-426">Odpowiada to `alloca` funkcji, rozszerzenie w implementacji języka C i C++.</span><span class="sxs-lookup"><span data-stu-id="aa031-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="aa031-427">W przykładzie</span><span class="sxs-lookup"><span data-stu-id="aa031-427">In the example</span></span>

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

<span data-ttu-id="aa031-428">`stackalloc` inicjator jest używany w `IntToString` metodę, aby przydzielić bufor o długości 16 znaków na stosie.</span><span class="sxs-lookup"><span data-stu-id="aa031-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="aa031-429">Rozmiar buforu jest automatycznie odrzucane po powrocie z metody.</span><span class="sxs-lookup"><span data-stu-id="aa031-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="aa031-430">Dynamiczna alokacja pamięci</span><span class="sxs-lookup"><span data-stu-id="aa031-430">Dynamic memory allocation</span></span>

<span data-ttu-id="aa031-431">Z wyjątkiem `stackalloc` operatora, C# zawiera nie wstępnie zdefiniowanych konstrukcje zarządzania zebranych pamięci niż pamięci.</span><span class="sxs-lookup"><span data-stu-id="aa031-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="aa031-432">Takich usług są zazwyczaj dostarczane dzięki obsłudze bibliotek klas lub zaimportować bezpośrednio od zasadniczego systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="aa031-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="aa031-433">Na przykład `Memory` klasy poniżej pokazano, jak funkcje sterty zasadniczego systemu operacyjnego może być uzyskiwany za pomocą języka C#:</span><span class="sxs-lookup"><span data-stu-id="aa031-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

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

<span data-ttu-id="aa031-434">Przykład pokazujący `Memory` klasy są podane poniżej:</span><span class="sxs-lookup"><span data-stu-id="aa031-434">An example that uses the `Memory` class is given below:</span></span>

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

<span data-ttu-id="aa031-435">Przykład przydziela 256 bajtów pamięci za pomocą `Memory.Alloc` i inicjuje blok pamięci z wartościami, zwiększenie od 0 do 255.</span><span class="sxs-lookup"><span data-stu-id="aa031-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="aa031-436">Następnie przydziela tablicy typu byte 256 elementu i używa `Memory.Copy` skopiować zawartość bloku pamięci do tablicy typu byte.</span><span class="sxs-lookup"><span data-stu-id="aa031-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="aa031-437">Na koniec bloku pamięci jest zwalniana, za pomocą `Memory.Free` i zawartości tablicy bajtów są wysyłane na konsolę.</span><span class="sxs-lookup"><span data-stu-id="aa031-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
