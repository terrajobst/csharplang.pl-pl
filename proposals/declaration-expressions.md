---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484581"
---
# <a name="declaration-expressions"></a><span data-ttu-id="7576e-101">Wyrażenia deklaracji</span><span class="sxs-lookup"><span data-stu-id="7576e-101">Declaration expressions</span></span>

<span data-ttu-id="7576e-102">Obsługa przypisań deklaracji jako wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="7576e-102">Support declaration assignments as expressions.</span></span>

## <a name="motivation"></a><span data-ttu-id="7576e-103">Motywacją</span><span class="sxs-lookup"><span data-stu-id="7576e-103">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="7576e-104">Zezwalaj na inicjowanie w punkcie deklaracji w większej liczbie przypadków, upraszczając kod i zezwalając na używanie `var`.</span><span class="sxs-lookup"><span data-stu-id="7576e-104">Allow initialization at the point of declaration in more cases, simplifying code, and allowing `var` to be used.</span></span>

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

<span data-ttu-id="7576e-105">Zezwalaj na deklaracje `ref` argumentów, podobnie jak `out var`.</span><span class="sxs-lookup"><span data-stu-id="7576e-105">Allow declarations for `ref` arguments, similar to `out var`.</span></span>

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a><span data-ttu-id="7576e-106">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="7576e-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="7576e-107">Wyrażenia są rozszerzane, aby uwzględnić przypisanie deklaracji.</span><span class="sxs-lookup"><span data-stu-id="7576e-107">Expressions are extended to include declaration assignment.</span></span> <span data-ttu-id="7576e-108">Pierwszeństwo jest takie samo jak przypisanie.</span><span class="sxs-lookup"><span data-stu-id="7576e-108">Precedence is the same as assignment.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

<span data-ttu-id="7576e-109">Przypisanie deklaracji jest pojedynczym elementem lokalnym.</span><span class="sxs-lookup"><span data-stu-id="7576e-109">The declaration assignment is of a single local.</span></span>

<span data-ttu-id="7576e-110">Typ wyrażenia przypisania deklaracji jest typem deklaracji.</span><span class="sxs-lookup"><span data-stu-id="7576e-110">The type of a declaration assignment expression is the type of the declaration.</span></span>
<span data-ttu-id="7576e-111">Jeśli typ jest `var`, typ wnioskowany jest typem wyrażenia inicjującego.</span><span class="sxs-lookup"><span data-stu-id="7576e-111">If the type is `var`, the inferred type is the type of the initializing expression.</span></span> 

<span data-ttu-id="7576e-112">Wyrażenie przypisania deklaracji może być l-wartością dla `ref` wartości argumentu w szczególności.</span><span class="sxs-lookup"><span data-stu-id="7576e-112">The declaration assignment expression may be an l-value, for `ref` argument values in particular.</span></span>

<span data-ttu-id="7576e-113">Jeśli wyrażenie przypisania deklaracji deklaruje typ wartości, a wyrażenie jest wartością r-Value, wartością wyrażenia jest kopia.</span><span class="sxs-lookup"><span data-stu-id="7576e-113">If the declaration assignment expression declares a value type, and the expression is an r-value, the value of the expression is a copy.</span></span>

<span data-ttu-id="7576e-114">Wyrażenie przypisania deklaracji może deklarować `ref` lokalnie.</span><span class="sxs-lookup"><span data-stu-id="7576e-114">The declaration assignment expression may declare a `ref` local.</span></span>
<span data-ttu-id="7576e-115">Występuje niejednoznaczność, gdy `ref` jest używany w wyrażeniu deklaracji w argumencie `ref`.</span><span class="sxs-lookup"><span data-stu-id="7576e-115">There is an ambiguity when `ref` is used for a declaration expression in a `ref` argument.</span></span>
<span data-ttu-id="7576e-116">Inicjator zmiennej lokalnej określa, czy deklaracja jest `ref` lokalnego.</span><span class="sxs-lookup"><span data-stu-id="7576e-116">The local variable initializer determines whether the declaration is a `ref` local.</span></span>

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

<span data-ttu-id="7576e-117">Zakres elementów lokalnych zadeklarowanych w wyrażeniach przypisania deklaracji jest taki sam jak zakres odpowiednich wyrażeń deklaracji z języka C # 7.0.</span><span class="sxs-lookup"><span data-stu-id="7576e-117">The scope of locals declared in declaration assignment expressions is the same the scope of corresponding declaration expressions from C#7.0.</span></span>

<span data-ttu-id="7576e-118">Jest to błąd czasu kompilacji, aby odwołać się do lokalnego tekstu w tekście poprzedzającym wyrażenie deklaracji.</span><span class="sxs-lookup"><span data-stu-id="7576e-118">It is a compile time error to refer to a local in text preceding the declaration expression.</span></span>

## <a name="alternatives"></a><span data-ttu-id="7576e-119">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="7576e-119">Alternatives</span></span>
[alternatives]: #alternatives
<span data-ttu-id="7576e-120">Bez zmian.</span><span class="sxs-lookup"><span data-stu-id="7576e-120">No change.</span></span> <span data-ttu-id="7576e-121">Ta funkcja jest tylko skrótową składnią.</span><span class="sxs-lookup"><span data-stu-id="7576e-121">This feature is just syntactic shorthand after all.</span></span>

<span data-ttu-id="7576e-122">Więcej ogólnych wyrażeń sekwencji: zobacz [#377](https://github.com/dotnet/csharplang/issues/377).</span><span class="sxs-lookup"><span data-stu-id="7576e-122">More general sequence expressions: see [#377](https://github.com/dotnet/csharplang/issues/377).</span></span>

<span data-ttu-id="7576e-123">Aby zezwolić na używanie `var` w większej liczbie przypadków, Zezwól na oddzielną deklarację i przypisanie `var` lokalnych i wywnioskowanie typu z przypisań ze wszystkich ścieżek kodu.</span><span class="sxs-lookup"><span data-stu-id="7576e-123">To allow use of `var` in more cases, allow separate declaration and assignment of `var` locals, and infer the type from assignments from all code paths.</span></span>

## <a name="see-also"></a><span data-ttu-id="7576e-124">Zobacz też</span><span class="sxs-lookup"><span data-stu-id="7576e-124">See also</span></span>
[see-also]: #see-also
<span data-ttu-id="7576e-125">Zobacz podstawowe wyrażenie deklaracji w [#595](https://github.com/dotnet/csharplang/issues/595).</span><span class="sxs-lookup"><span data-stu-id="7576e-125">See Basic Declaration Expression in [#595](https://github.com/dotnet/csharplang/issues/595).</span></span>

<span data-ttu-id="7576e-126">Zobacz deklarację dekonstrukcji w funkcji [dekonstrukcja](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) .</span><span class="sxs-lookup"><span data-stu-id="7576e-126">See Deconstruction Declaration in the [deconstruction](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) feature.</span></span>
