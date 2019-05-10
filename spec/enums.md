---
ms.openlocfilehash: c3b716e6eb331be2ee33fffeb42c1e2406cd3a5c
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488755"
---
# <a name="enums"></a>Wyliczenia

***Typu wyliczeniowego*** jest typem wartości odrębne ([typy wartości](types.md#value-types)) oświadcza, że zestaw nazwanych stałych.

Przykład

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

deklaruje typ wyliczeniowy o nazwie `Color` z elementami członkowskimi `Red`, `Green`, i `Blue`.

## <a name="enum-declarations"></a>Deklaracje wyliczeń

Deklaracja wyliczenia deklaruje nowy typ wyliczenia. Deklaracja wyliczenia zaczyna się od słowa kluczowego `enum`i określa nazwę, ułatwień dostępu, typ podstawowy i elementów członkowskich wyliczenia.

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

Każdy typ wyliczenia ma odpowiedni typ całkowity, o nazwie ***typ bazowy*** typu wyliczeniowego. Ten typ podstawowy musi umożliwiać do reprezentowania wartości modułu wyliczającego zdefiniowane w wyliczeniu. Deklaracja wyliczenia mogą jawnie deklarować podstawowym typem `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` lub `ulong`. Należy pamiętać, że `char` nie można używać jako typu podstawowego. Deklaracja wyliczenia, które jawnie deklaruj typu podstawowego jest podstawowym typem `int`.

Przykład

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

deklaruje wyliczenie z podstawowym typem `long`. Deweloper wybrać korzystanie z podstawowym typem `long`, jak w przykładzie, aby umożliwić korzystanie z wartości, które znajdują się w zakresie `long` , ale nie w zakresie `int`, lub zachować tę opcję, aby w przyszłości.

## <a name="enum-modifiers"></a>Modyfikatory wyliczenia

*Enum_declaration* może opcjonalnie obejmować sekwencję Modyfikatory wyliczenia:

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

Jest to błąd czasu kompilacji dla tego samego modyfikator pojawią się wiele razy w deklaracji wyliczenia.

Modyfikatorów deklaracji wyliczenia mają takie samo znaczenie jak deklaracji klasy ([klasy Modyfikatory](classes.md#class-modifiers)). Zauważ, że `abstract` i `sealed` modyfikatorów nie są dozwolone w deklaracji wyliczenia. Wyliczenia nie może być abstrakcyjny i nie pozwalają na tworzenie wartości pochodnych.

## <a name="enum-members"></a>Elementy członkowskie wyliczenia

Treść deklaracji typu wyliczenia definiuje zero lub więcej członków typu wyliczeniowego, które są nazwane stałe typu wyliczeniowego. Nie dwa elementy członkowskie wyliczenia może mieć takiej samej nazwie.

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

Każdy element członkowski wyliczenia ma skojarzoną wartość stałą. Typ tej wartości jest podstawowym typem wyliczenia zawierającego. Stała wartość dla każdego elementu członkowskiego wyliczenia musi być z zakresu od typu podstawowego dla wyliczenia. Przykład

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

powoduje błąd w czasie kompilacji, ponieważ wartości stałych `-1`, `-2`, i `-3` nie znajdują się w zakresie bazowego typu całkowitego `uint`.

Wiele elementów członkowskich wyliczenia mogą mieć taką samą wartość skojarzona. Przykład

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

zawiera wyliczenie, w której dwa elementy członkowskie wyliczenia — `Blue` i `Max` — są takie same skojarzone wartości.

Skojarzona wartość elementu członkowskiego wyliczenia przypisano jawnie lub niejawnie. Jeśli deklaracja składowej wyliczenia ma *constant_expression* inicjator, wartość tego wyrażenie stałe niejawnie konwertowane na podstawowym typem wyliczenia, jest skojarzona wartość składowej wyliczenia. Jeśli deklaracja składowej wyliczenia nie ma inicjatora, jej powiązaną wartość ustawiono niejawnie, wykonując następujące czynności:

*  Jeśli element członkowski wyliczenia jest zadeklarowana w typie wyliczeniowym pierwszego elementu członkowskiego wyliczenia, jej powiązaną wartość wynosi zero.
*  W przeciwnym razie skojarzona wartość składowej wyliczenia są uzyskiwane przez zwiększenie skojarzona wartość w formie tekstu poprzedniego elementu członkowskiego wyliczenia o jeden. Ta zwiększona wartość musi należeć do zakresu wartości, które mogą być reprezentowane przez typ podstawowy, w przeciwnym razie wystąpi błąd kompilacji.

Przykład

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

Wyświetla nazwy elementów członkowskich wyliczenia i ich skojarzone wartości. Dane wyjściowe to:

```
Red = 0
Green = 10
Blue = 11
```

z następujących powodów:

*  element członkowski wyliczenia `Red` jest automatycznie przypisywana wartość zero (ponieważ nie ma inicjatora i jest pierwszy element członkowski wyliczenia).
*  element członkowski wyliczenia `Green` jest jawnie podana wartość `10`;
*  i elementów członkowskich wyliczenia `Blue` jest automatycznie przypisywana wartość większa o jeden od elementu członkowskiego, przechwyceniem formie tekstu.

Skojarzona wartość elementu członkowskiego wyliczenia nie mogą bezpośrednio lub pośrednio, należy użyć wartości członków skojarzonych wyliczenia. Inne niż to ograniczenie zależność cykliczną inicjatorów składowych typu wyliczeniowego swobodnie mogą odwoływać się do innych inicjatorów składowych typu wyliczeniowego, niezależnie od ich pozycji tekstową. W obrębie inicjatora składowej wyliczenia wartości innych elementów członkowskich wyliczenia są zawsze traktowane jako mające typ ich typu podstawowego, aby rzutowania są niezbędne, przy odwoływaniu się do innych elementów członkowskich wyliczenia.

Przykład

```csharp
enum Circular
{
    A = B,
    B
}
```

powoduje błąd w czasie kompilacji, ponieważ deklaracje `A` i `B` są cykliczne. `A` zależy od `B` jawnie, i `B` zależy od `A` niejawnie.

Elementy członkowskie wyliczenia są o nazwie i o określonym zakresie w sposób dokładnie analogiczny do pól w klasach. Zakres elementu członkowskiego wyliczenia jest zbiorem jej typ zawierający wyliczenia. W tym zakresie typu wyliczeniowego może odnosić się według nazwy proste. Od innego kodu nazwa elementu członkowskiego wyliczenia musi być kwalifikowana nazwą typu wyliczenia. Elementy członkowskie wyliczenia nie mają żadnych deklarowana dostępność metody — element członkowski wyliczenia jest dostępny, jeśli jego zawierający typ wyliczenia jest dostępny.

## <a name="the-systemenum-type"></a>Typ System.Enum

Typ `System.Enum` jest abstrakcyjna klasa bazowa wszystkich typów wyliczenia (jest to odrębne i różni się od typ podstawowy elementu typu wyliczeniowego) i elementy członkowskie są dziedziczone z `System.Enum` są dostępne w żadnych typu wyliczeniowego. Konwersja boxing ([konwersje Boxing](types.md#boxing-conversions)) istnieje z dowolnego typu enum do `System.Enum`i konwersji rozpakowującej ([konwersje rozpakowywania](types.md#unboxing-conversions)) istnieje z `System.Enum` do dowolnego typu wyliczenia.

Należy pamiętać, że `System.Enum` sama nie jest *enum_type*. Jest to raczej *class_type* z wszystkie *enum_type*pochodzą s. Typ `System.Enum` dziedziczy z typu `System.ValueType` ([typu System.ValueType](types.md#the-systemvaluetype-type)), która z kolei dziedziczy z typu `object`. W czasie wykonywania, wartości typu `System.Enum` może być `null` lub odwołanie do wartości spakowanej dowolnego typu wyliczeniowego.

## <a name="enum-values-and-operations"></a>Wartości wyliczenia i operacje

Każdy typ wyliczenia definiuje typ samodzielny; Konwersja jawna wyliczenia ([Konwersje jawne wyliczenie](conversions.md#explicit-enumeration-conversions)) jest wymagana do konwertowania z zakresu od typu wyliczeniowego do typu całkowitego lub między dwoma typami wyliczenia. Zbiór wartości, które mogą przejąć typ wyliczeniowy nie jest ograniczona przez jej elementów członkowskich wyliczenia. W szczególności dowolną wartość w podstawowym typem wyliczenia mogą być rzutowane na typ wyliczeniowy i jest różne prawidłową wartością tego typu wyliczeniowego.

Elementy członkowskie wyliczenia mają typ ich typem zawierającym wyliczenia (z wyjątkiem w ramach innych inicjatorów składowych typu wyliczeniowego: zobacz [typu wyliczeniowego](enums.md#enum-members)). Wartość elementu członkowskiego wyliczenia zostało zadeklarowane w typie wyliczenia `E` o wartość skojarzonego `v` jest `(E)v`.

Można używać następujących operatorów na wartości typu wyliczenia: `==`, `!=`, `<`, `>`, `<=`, `>=`  ([operatory porównania wyliczenie](expressions.md#enumeration-comparison-operators)) , binarne `+`  ([operator dodawania](expressions.md#addition-operator)), binarny `-`  ([operator odejmowania](expressions.md#subtraction-operator)), `^`, `&` , `|`  ([Operatorów logicznych wyliczenie](expressions.md#enumeration-logical-operators)), `~`  ([operator dopełnienia bitowego](expressions.md#bitwise-complement-operator)), `++` i `--`  ([Przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators) i [przedrostkowe i operatory dekrementacji](expressions.md#prefix-increment-and-decrement-operators)).

Każdy typ wyliczenia automatycznie dziedziczy z klasy `System.Enum` (który z kolei pochodzi od klasy `System.ValueType` i `object`). W efekcie dziedziczonej metody i właściwości tej klasy można na wartościach typu wyliczeniowego.
