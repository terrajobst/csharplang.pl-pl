---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2019
ms.locfileid: "79485127"
---

# <a name="discriminated-unions--enum-class"></a>Związki rozłączne/`enum class`

`enum class`es są nowym rodzajem deklaracji typu, czasami nazywanymi związkami rozłącznych, gdzie każde możliwe wystąpienie typu jest wymienione, a każde wystąpienie nie nakłada się.

`enum class` jest definiowana przy użyciu następującej składni:

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

Przykładowa składnia:

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a>Semantyki

Definicja `enum class` definiuje typ główny, który jest klasą abstrakcyjną o tej samej nazwie co deklaracja `enum class` i zestaw składowych, z których każdy ma typ, który jest podtypem typu głównego. Jeśli istnieje wiele definicji `partial enum class`, wszystkie elementy członkowskie będą traktowane jako elementy członkowskie definicji klasy wyliczeniowej. W przeciwieństwie do definicji klasy abstrakcyjnej zdefiniowanej przez użytkownika, `enum class` głównym typem jest domyślnie częściowe i zdefiniowane do posiadania domyślnego *prywatnego* konstruktora bez parametrów.

Należy zwrócić uwagę, że ponieważ typ główny jest zdefiniowany jako klasa abstrakcyjna częściowa, można również dodać częściowe definicje *typu głównego* , gdzie są dozwolone formularze składni standardowej dla treści klasy.
Jednak żadne typy nie mogą dziedziczyć bezpośrednio od typu głównego w jakiejkolwiek deklaracji, poza tymi określonymi jako elementy członkowskie `enum class`. Ponadto dla typu głównego nie można używać konstruktorów zdefiniowanych przez użytkownika.

Istnieją cztery rodzaje deklaracji składowej `enum class`:

* Proste składowe klasy

* złożone składowe klas

* `enum class` członkowie

* elementy członkowskie wartości.

### <a name="simple-class-members"></a>Proste składowe klasy

Prosta deklaracja składowej klasy definiuje nową, zagnieżdżoną klasę "Records" (celowo niezdefiniowana w tym dokumencie) o tej samej nazwie. Klasa zagnieżdżona dziedziczy po typie głównym.

Powyższy przykładowy kod,

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

Deklaracja `enum class` ma semantykę równoważną następującej deklaracji

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a>Złożone składowe klas

Można również zagnieżdżać całą deklarację klasy poniżej deklaracji `enum class`. Będzie ona traktowana jako Klasa zagnieżdżona typu głównego. Składnia zezwala dowolnej deklaracji klasy, ale jest wymagana dla składowej klasy złożonej, aby dziedziczyć z bezpośredniej deklaracji `enum class`. 

### <a name="enum-class-members"></a>`enum class` członkowie

`enum classes` mogą być zagnieżdżone w innych, np.

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

Jest to prawie identyczne z semantyką `enum class`najwyższego poziomu, z tą różnicą, że zagnieżdżona Klasa enum definiuje zagnieżdżony typ główny, a wszystko poniżej zagnieżdżonej klasy enum jest podtypem typu zagnieżdżonego, a nie typu głównego najwyższego poziomu.

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a>Elementy członkowskie wartości

`enum classes` może również zawierać elementy członkowskie wartości. Elementy członkowskie wartości definiują publiczne właściwości statyczne tylko do pobrania dla typu głównego, które również zwracają typ główny, np.

```C#
enum class Color
{
    Red,
    Green
}
```

ma właściwości równoważne

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

Kompletna semantyka jest traktowana jako szczegóły implementacji, ale jest gwarantowane, że jedno unikatowe wystąpienie zostanie zwrócone dla każdej właściwości, a to samo wystąpienie zostanie zwrócone w powtórzonych wywołaniach.


### <a name="switch-expression-and-patterns"></a>Przełącz wyrażenie i wzorce

Istnieją pewne proponowane modyfikacje dopasowania do wzorca oraz wyrażenie przełącznika do obsługi `enum classes`. Wyrażenia Switch mogą już być zgodne z typami za pomocą wzorca zmiennej, ale obecnie dla typów referencyjnych żaden Zestaw ramion przełączania w wyrażeniu Switch nie jest uważany za kompletny, z wyjątkiem dopasowania do typu statycznego argumentu lub podtypu.

Wyrażenia Switch zostałyby zmienione tak, aby, jeśli typ główny `enum class` jest typem statycznym argumentu dla wyrażenia Switch, a istnieje zestaw wzorców pasujących do wszystkich elementów członkowskich wyliczenia, przełącznik zostanie uznany za wyczerpujący.

Ponieważ składowe wartości nie są stałymi i nie definiują nowych typów statycznych, nie można ich dopasować do wzorca. Aby to umożliwić, nowy wzorzec przy użyciu składni wzorca stałego zostanie dodany, aby umożliwić dopasowanie względem elementów członkowskich wartości `enum class`. Dopasowanie jest zdefiniowane do sukcesu, jeśli i tylko wtedy, gdy argument dopasowania do wzorca i wartość zwrócona przez składową wartości `enum class` będzie równa równości, chociaż implementacja nie jest wymagana do wykonania tego sprawdzenia.


## <a name="open-questions"></a>Otwarte pytania

- [] Co robi algorytm Common Type o `enum class` członkowskich? Czy ten prawidłowy kod?
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- [] Dodanie nowego wzorca tylko dla elementów członkowskich wartości wygląda intensywnie. Czy istnieje bardziej ogólna konstrukcja wersji, która ma sens?
    - [] Elementy członkowskie wartości również nie mapują się również do równoległej konstrukcji klasy zagnieżdżonej ze względu na to

- [] Jest przełączany do argumentu z `enum class` typem statycznym gwarantowanym jako czas stały?

- [] Czy istnieje sposób, aby `enum class`es nie były uznawane za kompletne w wyrażeniu Switch? Prefiks z `virtual`?

- [] Jakie Modyfikatory powinny być dozwolone w `enum class`?