---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484581"
---
# <a name="declaration-expressions"></a>Wyrażenia deklaracji

Obsługa przypisań deklaracji jako wyrażeń.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Zezwalaj na inicjowanie w punkcie deklaracji w większej liczbie przypadków, upraszczając kod i zezwalając na używanie `var`.

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

Zezwalaj na deklaracje `ref` argumentów, podobnie jak `out var`.

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Wyrażenia są rozszerzane, aby uwzględnić przypisanie deklaracji. Pierwszeństwo jest takie samo jak przypisanie.

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

Przypisanie deklaracji jest pojedynczym elementem lokalnym.

Typ wyrażenia przypisania deklaracji jest typem deklaracji.
Jeśli typ jest `var`, typ wnioskowany jest typem wyrażenia inicjującego. 

Wyrażenie przypisania deklaracji może być l-wartością dla `ref` wartości argumentu w szczególności.

Jeśli wyrażenie przypisania deklaracji deklaruje typ wartości, a wyrażenie jest wartością r-Value, wartością wyrażenia jest kopia.

Wyrażenie przypisania deklaracji może deklarować `ref` lokalnie.
Występuje niejednoznaczność, gdy `ref` jest używany w wyrażeniu deklaracji w argumencie `ref`.
Inicjator zmiennej lokalnej określa, czy deklaracja jest `ref` lokalnego.

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

Zakres elementów lokalnych zadeklarowanych w wyrażeniach przypisania deklaracji jest taki sam jak zakres odpowiednich wyrażeń deklaracji z języka C # 7.0.

Jest to błąd czasu kompilacji, aby odwołać się do lokalnego tekstu w tekście poprzedzającym wyrażenie deklaracji.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives
Bez zmian. Ta funkcja jest tylko skrótową składnią.

Więcej ogólnych wyrażeń sekwencji: zobacz [#377](https://github.com/dotnet/csharplang/issues/377).

Aby zezwolić na używanie `var` w większej liczbie przypadków, Zezwól na oddzielną deklarację i przypisanie `var` lokalnych i wywnioskowanie typu z przypisań ze wszystkich ścieżek kodu.

## <a name="see-also"></a>Zobacz też
[see-also]: #see-also
Zobacz podstawowe wyrażenie deklaracji w [#595](https://github.com/dotnet/csharplang/issues/595).

Zobacz deklarację dekonstrukcji w funkcji [dekonstrukcja](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) .
