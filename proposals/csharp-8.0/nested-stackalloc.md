---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2019
ms.locfileid: "79485141"
---
# <a name="permit-stackalloc-in-nested-contexts"></a>Zezwalaj na `stackalloc` w zagnieżdżonych kontekstach

### <a name="stack-allocation"></a>Alokacja stosu

Modyfikujemy [*alokację stosu*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) sekcji specyfikacji C# języka, aby wyeliminować miejsca, w których może się pojawić wyrażenie `stackalloc`. Usuniemy

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

i zastąp je

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

Należy zauważyć, że dodanie *array_initializer* do *stackalloc_initializer* (i wyrażenie indeksu opcjonalne) było [rozszerzeniem w C# 7,3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) i nie zostało opisane w tym miejscu.

*Typem elementu* wyrażenia `stackalloc` jest *unmanaged_type* o nazwie w wyrażeniu stackalloc (jeśli istnieje) lub wspólny typ między elementami *array_initializer* w przeciwnym razie.

Typ *stackalloc_initializer* z *typem elementu* `K` zależy od jego kontekstu składni:
- Jeśli *stackalloc_initializer* pojawia się bezpośrednio jako *local_variable_initializer* instrukcji *local_variable_declaration* lub *for_initializer*, jego typ to `K*`.
- W przeciwnym razie jego typ jest `System.Span<K>`.

### <a name="stackalloc-conversion"></a>Konwersja stackalloc

*Konwersja stackalloc* jest nową wbudowaną niejawną konwersją z wyrażenia. Gdy typ *stackalloc_initializer* jest `K*`, istnieje niejawna *konwersja stackalloc* z *stackalloc_initializer* do `System.Span<K>`typu.
