---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703967"
---
# <a name="enums"></a>Wyliczenia

***Typ wyliczeniowy*** jest odrębnym typem wartości ([typy wartości](types.md#value-types)), który deklaruje zestaw nazwanych stałych.

Przykład

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

deklaruje typ wyliczeniowy o nazwie `Color` z członkami `Red`, `Green`i `Blue`.

## <a name="enum-declarations"></a>Deklaracje wyliczenia

Deklaracja wyliczenia deklaruje nowy typ wyliczeniowy. Deklaracja wyliczenia rozpoczyna się od słowa kluczowego `enum`i definiuje nazwę, ułatwienia dostępu, typ podstawowy i elementy członkowskie wyliczenia.

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

Każdy typ wyliczeniowy ma odpowiedni typ całkowity nazywany ***typem podstawowym*** typu wyliczeniowego. Ten typ podstawowy musi być w stanie reprezentować wszystkie wartości modułu wyliczającego zdefiniowane w wyliczeniu. Deklaracja wyliczenia może jawnie zadeklarować typ podstawowy `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` lub `ulong`. Należy zauważyć, że `char` nie może być używany jako typ podstawowy. Deklaracja wyliczenia, która nie deklaruje jawnie typu podstawowego, ma typ podstawowy `int`.

Przykład

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

deklaruje Wyliczenie z typem podstawowym `long`. Deweloper może zdecydować się na użycie podstawowego typu `long`, tak jak w przykładzie, aby umożliwić użycie wartości, które znajdują się w zakresie `long`, ale nie w zakresie `int`lub aby zachować tę opcję w przyszłości.

## <a name="enum-modifiers"></a>Modyfikatory wyliczenia

*Enum_declaration* może opcjonalnie zawierać sekwencję modyfikatorów wyliczenia:

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

Jest to błąd czasu kompilacji dla tego samego modyfikatora do wyświetlenia wiele razy w deklaracji wyliczenia.

Modyfikatory deklaracji wyliczenia mają takie samo znaczenie jak dla deklaracji klasy ([Modyfikatory klas](classes.md#class-modifiers)). Należy jednak zauważyć, że Modyfikatory `abstract` i `sealed` są niedozwolone w deklaracji wyliczenia. Wyliczenia nie mogą być abstrakcyjne i nie zezwalają na wyprowadzanie.

## <a name="enum-members"></a>Elementy członkowskie wyliczenia

Treść deklaracji typu wyliczeniowego definiuje zero lub więcej elementów członkowskich wyliczenia, które są nazwanymi stałymi typu wyliczeniowego. Żadne dwa elementy członkowskie wyliczenia nie mogą mieć takiej samej nazwy.

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

Każdy element członkowski wyliczenia ma skojarzoną wartość stałą. Typ tej wartości jest typem podstawowym dla zawierającego Wyliczenie. Wartość stała dla każdego elementu członkowskiego wyliczenia musi znajdować się w zakresie typu podstawowego dla wyliczenia. Przykład

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

powoduje błąd czasu kompilacji, ponieważ wartości stałe `-1`, `-2`i `-3` nie należą do zakresu podstawowej `uint`typu całkowitego.

Wiele elementów członkowskich wyliczenia może korzystać z tej samej skojarzonej wartości. Przykład

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

pokazuje Wyliczenie, w którym znajdują się dwa elementy członkowskie wyliczenia--`Blue` i `Max`--mają tę samą skojarzoną wartość.

Skojarzona wartość elementu członkowskiego wyliczenia jest przypisywana niejawnie lub jawnie. Jeśli deklaracja elementu członkowskiego wyliczenia ma inicjator *constant_expression* , wartość tego wyrażenia stałej, niejawnie przekonwertowana na typ podstawowy wyliczenia, jest skojarzoną wartością elementu członkowskiego wyliczenia. Jeśli deklaracja elementu członkowskiego wyliczenia nie ma inicjatora, jego skojarzona wartość jest ustawiana niejawnie w następujący sposób:

*  Jeśli składowa wyliczenia jest pierwszym elementem członkowskim wyliczenia zadeklarowanym w typie wyliczeniowym, jego skojarzona wartość jest równa zero.
*  W przeciwnym razie skojarzona wartość elementu członkowskiego wyliczenia jest uzyskiwana przez zwiększenie skojarzonej wartości składowej po raz ostatni przez jeden. Ta zwiększona wartość musi znajdować się w zakresie wartości, które mogą być reprezentowane przez typ podstawowy, w przeciwnym razie wystąpi błąd w czasie kompilacji.

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

drukuje nazwy elementów członkowskich wyliczenia i skojarzonych z nimi wartości. Dane wyjściowe:

```console
Red = 0
Green = 10
Blue = 11
```

z następujących powodów:

*  `Red` elementu członkowskiego wyliczenia jest automatycznie przypisana wartość zero (ponieważ nie ma inicjatora i jest pierwszym elementem członkowskim wyliczenia);
*  `Green` elementu członkowskiego wyliczenia jawnie podano wartość `10`;
*  a `Blue` elementu członkowskiego wyliczenia jest automatycznie przypisywana wartość, która jest większa od elementu członkowskiego, który jest poprzedzony znakiem.

Skojarzona wartość elementu członkowskiego wyliczenia może nie, bezpośrednio lub pośrednio, używać wartości własnego skojarzonego elementu członkowskiego wyliczenia. Oprócz tego ograniczenia cykliczności inicjatory składowych enum mogą swobodnie odwoływać się do innych inicjatorów składowych wyliczenia, niezależnie od ich pozycji tekstowych. W obrębie inicjatora elementu członkowskiego wyliczenia wartości innych elementów członkowskich wyliczenia są zawsze traktowane jako mają typ ich typ podstawowy, więc rzutowania nie są konieczne w przypadku odwoływania się do innych elementów członkowskich wyliczenia.

Przykład

```csharp
enum Circular
{
    A = B,
    B
}
```

powoduje błąd czasu kompilacji, ponieważ deklaracje `A` i `B` są cykliczne. `A` zależy od `B` jawnie, a `B` zależy od niejawnego `A`.

Elementy członkowskie wyliczenia są nazwane i w zakresie dokładnie analogiczne do pól w klasach. Zakres elementu członkowskiego wyliczenia jest treścią zawierającego typ wyliczeniowy. W tym zakresie elementy członkowskie wyliczenia mogą być określane przez ich prostą nazwę. Ze wszystkich innych kodów nazwa elementu członkowskiego wyliczenia musi być kwalifikowana nazwą jego typu wyliczeniowego. Elementy członkowskie wyliczenia nie mają zadeklarowanych dostępności — element członkowski wyliczenia jest dostępny, jeśli jego typ wyliczeniowy jest dostępny.

## <a name="the-systemenum-type"></a>Typ System. Enum

Typ `System.Enum` jest abstrakcyjną klasą bazową wszystkich typów wyliczeniowych (jest to różne i inne niż typ podstawowy typu wyliczeniowego), a elementy członkowskie dziedziczone z `System.Enum` są dostępne w dowolnym typie wyliczeniowym. Konwersja z opakowania ([konwersje pakujące](types.md#boxing-conversions)) istnieje z dowolnego typu wyliczeniowego do `System.Enum`, a konwersja rozpakowywania ([konwersje rozpakowywanie](types.md#unboxing-conversions)) istnieje z `System.Enum` do dowolnego typu wyliczeniowego.

Należy pamiętać, że `System.Enum` nie należy do *enum_type*. Nie jest to *class_type* , z której pochodzą wszystkie *enum_type*s. Typ `System.Enum` dziedziczy z typu `System.ValueType` ([Typ System. ValueType](types.md#the-systemvaluetype-type)), który z kolei dziedziczy po typie `object`. W czasie wykonywania, wartość typu `System.Enum` może być `null` lub odwołanie do wartości opakowanej dowolnego typu wyliczeniowego.

## <a name="enum-values-and-operations"></a>Wartości wyliczeniowe i operacje

Każdy typ wyliczeniowy definiuje odrębny typ; jawna konwersja wyliczenia ([jawne konwersje wyliczenia](conversions.md#explicit-enumeration-conversions)) jest wymagana do konwersji między typem wyliczenia a typem całkowitym lub między dwoma typami wyliczeniowymi. Zbiór wartości, dla których można zastosować typ wyliczeniowy, nie jest ograniczony przez elementy członkowskie wyliczenia. W szczególności każda wartość typu podstawowego wyliczenia może być rzutowana na typ wyliczeniowy i jest odrębną prawidłową wartością tego typu wyliczeniowego.

Elementy członkowskie wyliczenia mają typ zawierający typ wyliczeniowy (z wyjątkiem innych inicjatorów elementów członkowskich wyliczenia: zobacz [elementy członkowskie wyliczenia](enums.md#enum-members)). Wartość elementu członkowskiego wyliczenia zadeklarowana w typie wyliczeniowy `E` ze skojarzoną wartością `v` jest `(E)v`.

Następujące operatory mogą być używane na wartości typów wyliczeniowych: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Operatory porównania wyliczenia](expressions.md#enumeration-comparison-operators)), binarne `+` ([operator dodawania](expressions.md#addition-operator)), Binary `-` ([operator odejmowania](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Wyliczenie operatory logiczne](expressions.md#enumeration-logical-operators)), `~` ([operator](expressions.md#bitwise-complement-operator)dopełnienia bitowego), `++` i `--` ( [Operatory przyrostka zwiększania i zmniejszania](expressions.md#postfix-increment-and-decrement-operators) oraz [Operatory przyrostu i zmniejszania prefiksu](expressions.md#prefix-increment-and-decrement-operators).

Każdy typ wyliczeniowy jest automatycznie pochodzący z klasy `System.Enum` (co z kolei pochodzi od `System.ValueType` i `object`). W ten sposób dziedziczone metody i właściwości tej klasy mogą być używane dla wartości typu wyliczeniowego.
