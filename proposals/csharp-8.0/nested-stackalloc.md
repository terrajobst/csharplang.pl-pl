---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2019
ms.locfileid: "79485141"
---
# <a name="permit-stackalloc-in-nested-contexts"></a><span data-ttu-id="8e6ff-101">Zezwalaj na `stackalloc` w zagnieżdżonych kontekstach</span><span class="sxs-lookup"><span data-stu-id="8e6ff-101">Permit `stackalloc` in nested contexts</span></span>

### <a name="stack-allocation"></a><span data-ttu-id="8e6ff-102">Alokacja stosu</span><span class="sxs-lookup"><span data-stu-id="8e6ff-102">Stack allocation</span></span>

<span data-ttu-id="8e6ff-103">Modyfikujemy [*alokację stosu*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) sekcji specyfikacji C# języka, aby wyeliminować miejsca, w których może się pojawić wyrażenie `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="8e6ff-103">We modify the section [*Stack allocation*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) of the C# language specification to relax the places when a `stackalloc` expression may appear.</span></span> <span data-ttu-id="8e6ff-104">Usuniemy</span><span class="sxs-lookup"><span data-stu-id="8e6ff-104">We delete</span></span>

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="8e6ff-105">i zastąp je</span><span class="sxs-lookup"><span data-stu-id="8e6ff-105">and replace them with</span></span>

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

<span data-ttu-id="8e6ff-106">Należy zauważyć, że dodanie *array_initializer* do *stackalloc_initializer* (i wyrażenie indeksu opcjonalne) było [rozszerzeniem w C# 7,3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) i nie zostało opisane w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="8e6ff-106">Note that the addition of an *array_initializer* to *stackalloc_initializer* (and making the index expression optional) was an [extension in C# 7.3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) and is not described here.</span></span>

<span data-ttu-id="8e6ff-107">*Typem elementu* wyrażenia `stackalloc` jest *unmanaged_type* o nazwie w wyrażeniu stackalloc (jeśli istnieje) lub wspólny typ między elementami *array_initializer* w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="8e6ff-107">The *element type* of the `stackalloc` expression is the *unmanaged_type* named in the stackalloc expression, if any, or the common type among the elements of the *array_initializer* otherwise.</span></span>

<span data-ttu-id="8e6ff-108">Typ *stackalloc_initializer* z *typem elementu* `K` zależy od jego kontekstu składni:</span><span class="sxs-lookup"><span data-stu-id="8e6ff-108">The type of the *stackalloc_initializer* with *element type* `K` depends on its syntactic context:</span></span>
- <span data-ttu-id="8e6ff-109">Jeśli *stackalloc_initializer* pojawia się bezpośrednio jako *local_variable_initializer* instrukcji *local_variable_declaration* lub *for_initializer*, jego typ to `K*`.</span><span class="sxs-lookup"><span data-stu-id="8e6ff-109">If the *stackalloc_initializer* appears directly as the *local_variable_initializer* of a *local_variable_declaration* statement or a *for_initializer*, then its type is `K*`.</span></span>
- <span data-ttu-id="8e6ff-110">W przeciwnym razie jego typ jest `System.Span<K>`.</span><span class="sxs-lookup"><span data-stu-id="8e6ff-110">Otherwise its type is `System.Span<K>`.</span></span>

### <a name="stackalloc-conversion"></a><span data-ttu-id="8e6ff-111">Konwersja stackalloc</span><span class="sxs-lookup"><span data-stu-id="8e6ff-111">Stackalloc Conversion</span></span>

<span data-ttu-id="8e6ff-112">*Konwersja stackalloc* jest nową wbudowaną niejawną konwersją z wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="8e6ff-112">The *stackalloc conversion* is a new built-in implicit conversion from expression.</span></span> <span data-ttu-id="8e6ff-113">Gdy typ *stackalloc_initializer* jest `K*`, istnieje niejawna *konwersja stackalloc* z *stackalloc_initializer* do `System.Span<K>`typu.</span><span class="sxs-lookup"><span data-stu-id="8e6ff-113">When the type of a *stackalloc_initializer* is `K*`, there is an implicit *stackalloc conversion* from the *stackalloc_initializer* to the type `System.Span<K>`.</span></span>
