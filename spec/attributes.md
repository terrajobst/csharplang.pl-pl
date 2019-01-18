---
ms.openlocfilehash: a8ad8a8b3eda1d00fa745bd92e4371eacc36b79f
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229929"
---
# <a name="attributes"></a>Atrybuty

Większość języka C# umożliwia programisty określić deklaratywne informacji na temat jednostek zdefiniowane w programie. Na przykład dostępność metody w klasie jest określona przez urządzanie ją za pomocą *method_modifier*s `public`, `protected`, `internal`, i `private`.

C# umożliwia programistom wymyślić nowych rodzajów informacji deklaratywne, o nazwie ***atrybuty***. Programiści można dołączyć atrybuty do różnych obiektów programu i pobrać informacje o atrybutach w środowisku uruchomieniowym. Na przykład zdefiniować strukturę `HelpAttribute` atrybut, który można umieścić w przypadku niektórych elementów programu (takich jak klasy i metody) umożliwia mapowanie tych elementów program do ich dokumentacji.

Atrybuty są definiowane za pomocą deklaracji klasy atrybutów ([atrybutu klasy](attributes.md#attribute-classes)), które mogą mieć parametry pozycyjne i nazwane ([Positional i nazwanych parametrów](attributes.md#positional-and-named-parameters)). Atrybuty są dołączone do jednostki w programie C# przy użyciu specyfikacji atrybutu ([Specyfikacja atrybutu](attributes.md#attribute-specification)) i mogą być pobierane w czasie wykonywania jako instancje wieloatrybutowe ([wystąpienia atrybutów](attributes.md#attribute-instances)).

## <a name="attribute-classes"></a>Klasy atrybutów

Klasa, która pochodzi z klasy abstrakcyjnej `System.Attribute`, bezpośrednio lub pośrednio, jest ***klasy atrybutu***. Deklaracja klasy atrybutu definiuje nowy rodzaj ***atrybut*** który można umieścić w deklaracji. Zgodnie z Konwencją klasy atrybutów są nazywane z sufiksem `Attribute`. Przypadki użycia atrybutu może zawierać albo pominąć ten sufiks.

### <a name="attribute-usage"></a>Użycie atrybutu

Ten atrybut `AttributeUsage` ([atrybut AttributeUsage](attributes.md#the-attributeusage-attribute)) służy do opisywania, jak można użyć klasy atrybutu.

`AttributeUsage` ma parametr pozycyjne ([Positional i nazwanych parametrów](attributes.md#positional-and-named-parameters)), który umożliwia klasę atrybutu określić rodzaje deklaracje, które mogą służyć. Przykład

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

definiuje klasę atrybutu o nazwie `SimpleAttribute` który można umieścić na *class_declaration*s i *interface_declaration*tylko s. Przykład

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

Pokazuje kilka zastosowań `Simple` atrybutu. Mimo że ten atrybut jest zdefiniowany o nazwie `SimpleAttribute`, gdy ten atrybut jest używany, `Attribute` sufiksu może zostać pominięta, wynikiem krótką nazwę `Simple`. W związku z tym w powyższym przykładzie są semantycznie równoważne do następujących:

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

`AttributeUsage` ma parametr o nazwie ([Positional i nazwanych parametrów](attributes.md#positional-and-named-parameters)) o nazwie `AllowMultiple`, która wskazuje, czy ten atrybut można określić więcej niż jeden raz dla danej jednostki. Jeśli `AllowMultiple` dla atrybutu klasy ma wartość true, a następnie jest tej klasy atrybutu ***klasy wielokrotnego użytku atrybutu***, można określić więcej niż jeden raz w jednostce. Jeśli `AllowMultiple` dla atrybutu klasy ma wartość false lub jest nieokreślony, a następnie jest tej klasy atrybutu ***jednorazowy klasy atrybutu***i może być określona co najwyżej raz na jednostkę.

Przykład

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

definiuje klasę wielokrotnego użytku atrybutu o nazwie `AuthorAttribute`. Przykład

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

przedstawia deklarację klasy z dwa przypadki użycia `Author` atrybutu.

`AttributeUsage` zawiera inny nazwany parametr o nazwie `Inherited`, która wskazuje, czy atrybut, jeśli określony w klasie bazowej, również jest dziedziczone przez klasy, które pochodzą z tej klasy bazowej. Jeśli `Inherited` dla atrybutu klasy ma wartość true, a następnie ten atrybut jest dziedziczona. Jeśli `Inherited` dla atrybutu klasy ma wartość false, a następnie ten atrybut nie jest dziedziczony. Jeśli jest nieokreślony, jego wartość domyślna to true.

Klasa atrybutu `X` niemający `AttributeUsage` atrybut jest dołączany do niego, podobnie jak w

```csharp
using System;

class X: Attribute {...}
```

Jest odpowiednikiem następujących czynności:

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a>Parametry pozycyjne i nazwane

Klasy atrybutów może mieć ***parametry pozycyjne*** i ***nazwanych parametrów***. Konstruktor publiczne wystąpienia każdej klasy atrybutu definiuje prawidłową sekwencją parametry pozycyjne dla tej klasy atrybutu. Każdego niestatycznego publiczne odczytu / zapisu pola i właściwości dla atrybutu klasy definiuje nazwany parametr do klasy atrybutu.

Przykład

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

definiuje klasę atrybutu o nazwie `HelpAttribute` posiadającą jeden parametr pozycyjne, `url`, a jeden parametr o nazwie `Topic`. Chociaż niestatyczna i publiczne, właściwość `Url` nie definiuje nazwany parametr, ponieważ nie jest odczytu i zapisu.

Ten atrybut klasy mogą być używane w następujący sposób:

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a>Typy parametrów atrybutu

Typy parametry pozycyjne i nazwane atrybutu klasy nie są ograniczone do ***typy parametrów atrybutu***, które są:

*  Jednym z następujących typów: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.
*  Typ `object`.
*  Typ `System.Type`.
*  Typ wyliczeniowy, pod warunkiem ma powszechnej dostępności i typy, w których jest zagnieżdżony, (jeśli istnieje) również mieć dostęp publiczny ([Specyfikacja atrybutu](attributes.md#attribute-specification)).
*  Tablice jednowymiarowe z powyższych typów.
*  Argument konstruktora lub pole publiczne, która nie ma jednego z następujących typów nie może służyć jako parametr pozycyjne i nazwane w specyfikacji atrybutu.

## <a name="attribute-specification"></a>Specyfikacji atrybutu.

***Specyfikacja atrybutu*** jest stosowanie wcześniej zdefiniowanego atrybutu do zgłoszenia. Atrybut jest fragmentem dodatkowych informacji deklaratywne, który jest określony w deklaracji. Atrybuty można określić w zakresie globalnym (na przykład aby określić atrybuty na zawierającego zestaw lub moduł) i *type_declaration*s ([wpisz deklaracje](namespaces.md#type-declarations)), *class_member_declaration* s ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([interfejsu członków](interfaces.md#interface-members)), *struct_member _declaration*s ([składowe struktury](structs.md#struct-members)), *enum_member_declaration*s ([typu wyliczeniowego](enums.md#enum-members)), *accessor_declarations*  ([Akcesory](classes.md#accessors)), *event_accessor_declarations* ([zdarzenia podobne do pól](classes.md#field-like-events)), a *formal_parameter_list*s ([parametry metody](classes.md#method-parameters)).

Atrybuty są określone w ***atrybutu sekcje***. Sekcja atrybut składa się z pary nawiasów kwadratowych, które otaczają przecinkami lista jednego lub więcej atrybutów. Kolejność, w którym atrybuty są określone w takiej liście i są ułożone kolejność, w którym sekcje dołączone do tej samej jednostki programu, nie ma znaczenia. Na przykład specyfikacji atrybutu `[A][B]`, `[B][A]`, `[A,B]`, i `[B,A]` są równoważne.

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

Atrybut składa się z *attribute_name* i opcjonalną listę argumenty pozycyjne i nazwane. Argumenty pozycyjne (jeśli istnieje) poprzedzać nazwane argumenty. Argument pozycyjny składa się z *attribute_argument_expression*; argument nazwany składa się z nazwy, następuje znak równości, następuje *attribute_argument_expression*, która razem , są ograniczone przez te same reguły jako przypisanie proste. Kolejność argumentów nazwanych nie ma znaczenia.

*Attribute_name* identyfikuje klasę atrybutów. Jeśli formie *attribute_name* jest *type_name* , a następnie ta nazwa musi odwoływać się do klasy atrybutu. W przeciwnym razie wystąpi błąd kompilacji. Przykład

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

powoduje błąd w czasie kompilacji, ponieważ próbuje ona użyć `Class1` jako atrybut klasy, gdy `Class1` nie jest to klasa atrybutu.

Pewnych kontekstach zezwalać na specyfikację atrybutu na więcej niż jeden element docelowy. Program jawnie określić cel umieszczając *attribute_target_specifier*. Gdy atrybut jest umieszczany na poziomie globalnym *global_attribute_target_specifier* jest wymagana. W innych lokalizacjach uzasadnione domyślna jest stosowana, ale *attribute_target_specifier* może służyć potwierdzają lub zastąpić to ustawienie domyślne, w niektórych przypadkach niejednoznaczne (lub po prostu potwierdzają wartość domyślna w przypadku jednoznaczne). Ten sposób, zazwyczaj *attribute_target_specifier*s można pominąć, z wyjątkiem na poziomie globalnym. Potencjalnie niejednoznaczne kontekstów są rozwiązywane w następujący sposób:

*  Atrybut, który został określony w zakresie globalnym można zastosować do zestaw docelowy lub docelowy modułu. Brak wartości domyślnej istnieje dla tego kontekstu, więc *attribute_target_specifier* jest zawsze wymagany w tym kontekście. Obecność `assembly` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do obiektu docelowego zestawu; obecności `module` *attribute_target_specifier* Wskazuje, że ten atrybut ma zastosowanie do modułu, docelowej.
*  Atrybut, który został określony w deklaracji delegate można zastosować delegata został zadeklarowany lub jego wartość zwracaną. W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do delegata. Obecność `type` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do delegata; obecności `return` *attribute_target_specifier* Wskazuje, że ten atrybut ma zastosowanie do wartości zwracanej.
*  Atrybut, który został określony w deklaracji metody można zastosować do metody został zadeklarowany lub jego wartość zwracaną. W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do metody. Obecność `method` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do metody; obecności `return` *attribute_target_specifier* wskazuje czy ten atrybut ma zastosowanie do wartości zwracanej.
*  Atrybut, który został określony w deklaracji operator można zastosować operatora został zadeklarowany lub jego wartość zwracaną. W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do operatora. Obecność `method` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do operatora; obecności `return` *attribute_target_specifier* Wskazuje, że ten atrybut ma zastosowanie do wartości zwracanej.
*  Atrybut, który został określony w deklaracji zdarzenia, które pomija metod dostępu zdarzeń zastosować zdarzeń został zadeklarowany, skojarzonego pola (jeśli jest to zdarzenie nie jest abstrakcyjna) lub skojarzone Dodaj i Usuń metody. W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do zdarzenia. Obecność `event` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do zdarzenia; obecności `field` *attribute_target_specifier* wskazuje Atrybut dotyczy polu. i obecności `method` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do metody.
*  Atrybut, który został określony w deklaracji metody dostępu get w deklaracji właściwości lub indeksatora można zastosować skojarzoną metodę lub jego wartość zwracaną. W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do metody. Obecność `method` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do metody; obecności `return` *attribute_target_specifier* wskazuje czy ten atrybut ma zastosowanie do wartości zwracanej.
*  Atrybut, który został określony na dostępu set w deklaracji właściwości lub indeksatora można zastosować skojarzoną metodę lub jako pojedynczy parametr niejawne. W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do metody. Obecność `method` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do metody; obecności `param` *attribute_target_specifier* wskazuje Atrybut dotyczy parameter; obecność `return` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do wartości zwracanej.
*  Atrybut jest określony w deklaracji metody dostępu add lub remove dla deklaracji zdarzenia można zastosować skojarzoną metodę lub jako pojedynczy parametr. W przypadku braku *attribute_target_specifier*, ten atrybut ma zastosowanie do metody. Obecność `method` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do metody; obecności `param` *attribute_target_specifier* wskazuje Atrybut dotyczy parameter; obecność `return` *attribute_target_specifier* wskazuje, że ten atrybut ma zastosowanie do wartości zwracanej.

W innych kontekstach włączenia *attribute_target_specifier* jest dozwolone, ale niepotrzebne. Na przykład deklarację klasy może zawierać lub pominąć specyfikator `type`:

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

Jest to błąd, aby określić nieprawidłową *attribute_target_specifier*. Na przykład specyfikator `param` nie można używać w deklaracji klasy:

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

Zgodnie z Konwencją klasy atrybutów są nazywane z sufiksem `Attribute`. *Attribute_name* formularza *type_name* może zawierać albo pominąć ten sufiks. Jeśli klasa atrybutu znajduje się zarówno z i bez tego sufiksu, niejednoznaczność jest obecny i powoduje błąd kompilacji. Jeśli *attribute_name* została wpisana tak, aby jego najdalej z prawej strony *identyfikator* jest identyfikator dosłownego wyrażenia ([identyfikatory](lexical-structure.md#identifiers)), wówczas tylko atrybut bez sufiksu jest dopasowywany, umożliwiając w ten sposób niejednoznaczność zostać rozpoznane. Przykład

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

Pokazuje dwa atrybutu klasy o nazwie `X` i `XAttribute`. Ten atrybut `[X]` jest niejednoznaczny, ponieważ mogła się odwołać do jednej `X` lub `XAttribute`. Za pomocą identyfikator dosłownego wyrażenia umożliwia dokładne zamiar można określić w tych rzadkich przypadkach. Ten atrybut `[XAttribute]` jest niejednoznaczny (mimo że będzie, jeśli było to klasa atrybutu o nazwie `XAttributeAttribute`!). Jeśli w deklaracji klasy `X` zostanie usunięty, a następnie obu atrybutów odnoszą się do klasy atrybutu o nazwie `XAttribute`, wykonując następujące czynności:

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

Jest to błąd czasu kompilacji do użycia klasy atrybutu jednorazowego użycia więcej niż jeden raz na tej samej jednostki. Przykład

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

powoduje błąd w czasie kompilacji, ponieważ próbuje ona użyć `HelpString`, klasy atrybutu jednorazowego użycia, który jest więcej niż jeden raz na deklarację `Class1`.

Wyrażenie `E` jest *attribute_argument_expression* jeśli spełnione są wszystkie z następujących instrukcji:

*  Typ `E` jest typu parametru atrybutu ([typy parametrów atrybutu](attributes.md#attribute-parameter-types)).
*  W czasie kompilacji, wartość `E` może zostać rozpoznana na jedną z następujących czynności:
   * Stała wartość.
   * Element `System.Type` obiektu.
   * Jednowymiarowa tablica *attribute_argument_expression*s.

Na przykład:

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

A *typeof_expression* ([typeof operator](expressions.md#the-typeof-operator)) używane jako wyrażenie argumentu atrybutu można odwoływać się do typu nieogólnego, zamknięte skonstruowanego typu lub niepowiązanych typu rodzajowego, ale go nie może odwoływać się Otwórz typu. To, aby upewnić się, że wyrażenie może zostać rozpoznany w czasie kompilacji.

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a>Instancje wieloatrybutowe

***Wystąpienie atrybutu*** to wystąpienie, który reprezentuje atrybut, w czasie wykonywania. Atrybut jest zdefiniowana za pomocą klasy atrybutu argumentów pozycyjnych i argumenty nazwane. Wystąpienie atrybutu jest wystąpieniem klasy atrybutu, który jest inicjowany za pomocą argumenty pozycyjne i nazwane.

Pobieranie wystąpienia atrybutu obejmuje zarówno w czasie kompilacji, jak i w czasie wykonywania przetwarzania, zgodnie z opisem w poniższych sekcjach.

### <a name="compilation-of-an-attribute"></a>Kompilacja atrybutu

Kompilacja *atrybut* z klasy atrybutów `T`, *positional_argument_list* `P` i *named_argument_list* `N`, składa się z następujących czynności:

*  Postępuj zgodnie z instrukcjami przetwarzanie w czasie kompilacji do kompilowania *object_creation_expression* formularza `new T(P)`. Te kroki spowodować błąd kompilacji, albo określić konstruktora wystąpień `C` na `T` może być wywołana w czasie wykonywania.
*  Jeśli `C` ma powszechnej dostępności, a następnie wystąpi błąd kompilacji.
*  Dla każdego *named_argument* `Arg` w `N`:
   * Pozwól `Name` można *identyfikator* z *named_argument* `Arg`.
   * `Name` należy zidentyfikować niestatycznych odczytu i zapisu publiczne pole lub właściwość na `T`. Jeśli `T` ma nie ma takiego pola lub właściwości, a następnie wystąpi błąd kompilacji.
*  Zachowaj następujące informacje dotyczące czasu wykonywania podczas tworzenia wystąpienia atrybutu: atrybut klasy `T`, Konstruktor wystąpienia `C` na `T`, *positional_argument_list* `P` i *named_argument_list* `N`.

### <a name="run-time-retrieval-of-an-attribute-instance"></a>Pobieranie czasu wykonywania wystąpienia atrybutu

Kompilacja *atrybut* daje to klasa atrybutu `T`, Konstruktor wystąpienia `C` na `T`, *positional_argument_list* `P`i *named_argument_list* `N`. Biorąc pod uwagę te informacje, wystąpienie atrybutu mogą być pobierane w czasie wykonywania, wykonując następujące czynności:

*  Postępuj zgodnie z instrukcjami przetwarzanie w czasie wykonywania do wykonywania *object_creation_expression* formularza `new T(P)`, za pomocą konstruktora wystąpienia `C` ustalony w czasie kompilacji. Następujące kroki, powoduje wyjątek lub utworzyć wystąpienie `O` z `T`.
*  Dla każdego *named_argument* `Arg` w `N`, w kolejności:
   * Pozwól `Name` można *identyfikator* z *named_argument* `Arg`. Jeśli `Name` nie identyfikuje na niestatycznej publiczne odczytu / zapisu pole lub właściwość `O`, wówczas wyjątek jest zgłaszany.
   * Pozwól `Value` być wynikiem oceny *attribute_argument_expression* z `Arg`.
   * Jeśli `Name` identyfikuje pole na `O`, następnie ustaw to pole `Value`.
   * W przeciwnym razie `Name` identyfikuje właściwość na `O`. Ustaw tę właściwość na `Value`.
   * Wynik jest `O`, wystąpienie klasy atrybutu `T` , została zainicjowana przy użyciu *positional_argument_list* `P` i *named_argument_list* `N`.

## <a name="reserved-attributes"></a>Atrybuty zarezerwowane

Niewielka liczba atrybutów wpływa na język, w jakiś sposób. Te atrybuty obejmują:

*  `System.AttributeUsageAttribute` ([Atrybut AttributeUsage](attributes.md#the-attributeusage-attribute)), który jest używany do opisu sposobów, w którym tworzy się klasę atrybutów może służyć.
*  `System.Diagnostics.ConditionalAttribute` ([Atrybut Conditional](attributes.md#the-conditional-attribute)), która jest używana do definiowania metod warunkowych.
*  `System.ObsoleteAttribute` ([Przestarzałe atrybutu](attributes.md#the-obsolete-attribute)), który jest używany do oznaczania element członkowski jako przestarzałe.
*  `System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` i `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Caller — atrybuty informacji](attributes.md#caller-info-attributes)), które są używane do dostarczania informacji na temat Kontekst wywołania do opcjonalnych parametrów.

### <a name="the-attributeusage-attribute"></a>Atrybut AttributeUsage

Ten atrybut `AttributeUsage` służy do opisywania sposobu, w której można użyć klasy atrybutu.

Klasa, która zostanie nadany `AttributeUsage` atrybut musi pochodzić od klasy `System.Attribute`, bezpośrednio lub pośrednio. W przeciwnym razie wystąpi błąd kompilacji.

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a>Atrybut Conditional

Ten atrybut `Conditional` umożliwia określenie ***metoda warunkowa*** i ***klasy atrybut conditional***.

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a>Metoda warunkowa

Metoda ozdobione `Conditional` atrybutu jest metodą warunkowe. `Conditional` Atrybut wskazuje warunek, testując symbol kompilacji warunkowej. Wywołania metody warunkowego, są uwzględnione lub pominięta, w zależności od tego, czy ten symbol jest zdefiniowany w punkcie połączenia. Jeśli zdefiniowano symbol wywołanie jest uwzględniany; w przeciwnym razie wywołanie (w tym ocena odbiornika i parametry połączenia) zostanie pominięty.

Metody warunkowego podlega następującym ograniczeniom:

*  Metoda warunkowa musi być metodą w *class_declaration* lub *struct_declaration*. Występuje błąd kompilacji, jeśli `Conditional` atrybut jest określony dla metody w deklaracji interfejsu.
*  Metoda warunkowa musi mieć typ zwracany `void`.
*  Metoda warunkowa nie może być oznaczona przy użyciu `override` modyfikator. Metoda warunkowego może być oznaczony za pomocą `virtual` modyfikator, jednak. Zastąpienia taka metoda są niejawnie warunkowe i nie może być jawnie oznaczone za pomocą `Conditional` atrybutu.
*  Metoda warunkowa nie może być implementację metody interfejsu. W przeciwnym razie wystąpi błąd kompilacji.

Ponadto błąd kompilacji występuje, jeśli metoda warunkowego jest stosowana w *delegate_creation_expression*. Przykład

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

deklaruje `Class1.M` jako metody warunkowego. `Class2`firmy `Test` metoda wywołuje tę metodę. Ponieważ symbol kompilacji warunkowej `DEBUG` jest zdefiniowana, jeśli `Class2.Test` jest wywołana, będzie wywoływać `M`. Jeśli symbol `DEBUG` miał nie został zdefiniowany, następnie `Class2.Test` nie może wywołać `Class1.M`.

Należy zauważyć, że dołączania lub wykluczania wywołanie metody warunkowego jest kontrolowana przez symbole kompilacji warunkowej w momencie wywołania. W przykładzie

Plik `class1.cs`:

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

Plik `class2.cs`:

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

Plik `class3.cs`:

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

klasy `Class2` i `Class3` każdy zawiera wywołania metody warunkowego `Class1.F`, który warunkowego opiera się na niezależnie od tego czy `DEBUG` jest zdefiniowana. Ponieważ ten symbol jest zdefiniowany w kontekście `Class2` , ale nie `Class3`, wywołanie `F` w `Class2` jest uwzględniony, podczas wywołania `F` w `Class3` zostanie pominięty.

Używanie warunkowego metod w łańcuchu dziedziczenia może być mylące. Wywołania metody warunkowego za pomocą `base`, w postaci `base.M`, podlegają zasadom wywołanie normalne metoda warunkowa. W przykładzie

Plik `class1.cs`:

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

Plik `class2.cs`:

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

Plik `class3.cs`:

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

`Class2` zawiera wywołanie `M` zdefiniowany w klasie bazowej. To wywołanie jest pominięty, ponieważ metoda podstawowa jest warunkowe na podstawie obecności symbol `DEBUG`, który jest niezdefiniowany. Dlatego metoda zapisuje do konsoli "`Class2.M executed`" tylko. Korzystać z *pp_declaration*s może wyeliminować takich problemów.

#### <a name="conditional-attribute-classes"></a>Atrybut Conditional klas

Tworzy się klasę atrybutów ([atrybutu klasy](attributes.md#attribute-classes)) dekorowane za pomocą co najmniej jeden `Conditional` atrybutów jest ***klasy atrybut conditional***. Klasa atrybut conditional związku z tym jest skojarzona z symbole kompilacji warunkowej, zadeklarowany w jego `Conditional` atrybutów. W tym przykładzie:

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

deklaruje `TestAttribute` jako klasa atrybut conditional skojarzone z symbole kompilacji warunkowej `ALPHA` i `BETA`.

Atrybut specyfikacji ([Specyfikacja atrybutu](attributes.md#attribute-specification)) atrybutu warunkowego uwzględniono Jeśli co najmniej jeden z jego symbole kompilacji warunkowej skojarzone zdefiniowano punkcie specyfikacji, w przeciwnym razie atrybutu Specyfikacja zostanie pominięty.

Należy zauważyć, że dołączania lub wykluczania specyfikacji atrybutu klasy atrybut conditional jest kontrolowane przez symbole kompilacji warunkowej w punkcie specyfikacji. W przykładzie

Plik `test.cs`:

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

Plik `class1.cs`:

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

Plik `class2.cs`:

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

klasy `Class1` i `Class2` czy każdy dekorowane za pomocą atrybutu `Test`, który warunkowego opiera się na niezależnie od tego czy `DEBUG` jest zdefiniowana. Ponieważ ten symbol jest zdefiniowany w kontekście `Class1` , ale nie `Class2`, specyfikację `Test` atrybutu na `Class1` jest uwzględniony, podczas specyfikację `Test` atrybutu na `Class2` zostanie pominięty.

### <a name="the-obsolete-attribute"></a>Atrybut przestarzałe

Ten atrybut `Obsolete` służy do oznaczania typów i składowych typów, które nie powinny być używane.

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

Jeśli program używa typu lub elementu członkowskiego, który zostanie nadany `Obsolete` atrybutu, kompilator generuje ostrzeżenia lub błędu. Ściślej mówiąc, kompilator generuje ostrzeżenie, jeśli podano parametr nie błędu lub jeśli parametr błędu zostanie podana z wartością `false`. Kompilator generuje błąd, jeśli określono parametr błędu i ma wartość `true`.

W przykładzie

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

Klasa `A` zostanie nadany `Obsolete` atrybutu. Każde użycie `A` w `Main` wyników ostrzeżenia, który zawiera określony komunikat "Ta klasa jest przestarzała; Użyj klasy B."

### <a name="caller-info-attributes"></a>Caller — atrybuty informacji

Do celów takich jak rejestrowanie i raportowanie czasami jest przydatne w przypadku element członkowski funkcji uzyskać pewne informacje kompilacji dotyczące kodu wywołującego. Caller — atrybuty informacji umożliwiają do przekazania takich informacji w sposób niewidoczny dla użytkownika.

Gdy parametr opcjonalny jest oznaczony za pomocą jednego z caller — atrybuty informacji, pomijając odnośnego argumentu w wywołaniu nie zawsze powoduje wartość domyślna parametru do zastąpienia. Zamiast tego Jeśli określone informacje o kontekście wywołania jest dostępna, te informacje zostaną przekazane jako wartość argumentu.

Na przykład:

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

Wywołanie `Log()` bez argumentów zostanie wydrukowana w wierszu numer i ścieżkę pliku wywołanie, a także nazwę elementu członkowskiego, w którym wystąpił wywołania.

Caller — atrybuty informacji może wystąpić w dowolnym miejscu, następujące parametry opcjonalne, w tym w deklaracjach delegata. Jednak określonych caller — atrybuty informacji ma żadnych ograniczeń dotyczących rodzajów parametry, których można atrybutu tak, aby zawsze będzie niejawna konwersja z podstawioną wartość do typu parametru.

Jest to błąd, musi mieć tego samego atrybutu obiektu wywołującego w informacje dotyczące parametru definiowania i wdrażania część deklaracji metody częściowej. Tylko caller — atrybuty informacji w części definiujące są stosowane, natomiast caller — atrybuty informacji występuje tylko w części implementującej są ignorowane.

Informacje o wywołującym nie ma wpływu na rozpoznanie przeciążenia. Zgodnie z atrybutami następujące parametry opcjonalne są nadal pominięty z kodu źródłowego obiektu wywołującego, funkcja rozpoznawania przeciążeń ignoruje tych parametrów w taki sam sposób ignoruje inne pominięty parametry opcjonalne ([Rozpoznanie przeciążenia](expressions.md#overload-resolution)).

Informacje o wywołującym tylko zostanie zastąpiony, gdy funkcja jest jawnie wywoływana w kodzie źródłowym. Niejawne wywołania, takie jak nadrzędnego niejawne Konstruktor wywołuje nie mają lokalizacji źródłowej i nie zostanie zastąpiony przez informacje o wywołującym. Ponadto wywołania, które są dynamicznie powiązane nie zostanie zastąpiony przez informacje o wywołującym. Gdy informacji o obiekcie wywołującym opartego na atrybutach parametr zostanie pominięty w takich przypadkach w zamian jest używana wartość określoną wartość domyślną parametru.

Jedynym wyjątkiem jest wyrażenia zapytania. Są one traktowane jako rozszerzenia składni, a jeśli wywołań, mogą zwiększyć Pomiń parametry opcjonalne z caller — atrybuty informacji, informacje o wywołującym zostaną zastąpione. Lokalizacja używana jest lokalizacja klauzul zapytania, które wywołanie został wygenerowany z.

Jeśli więcej niż jeden atrybut informacje obiekt wywołujący jest określony dla danego parametru, są preferowane w następującej kolejności: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.

#### <a name="the-callerlinenumber-attribute"></a>Atrybut CallerLineNumber

`System.Runtime.CompilerServices.CallerLineNumberAttribute` Jest dozwolone na temat parametrów opcjonalnych, gdy istnieje niejawna konwersja standardowa ([standardowe i niejawne konwersje](conversions.md#standard-implicit-conversions)) z wartości stałej `int.MaxValue` typowi parametru. Daje to gwarancję, że można przekazać dowolną liczbę nieujemną wiersza do tej wartości bez błędu.

Jeśli wywołanie funkcji z lokalizacji w kodzie źródłowym pomija parametr opcjonalny z `CallerLineNumberAttribute`, a następnie literał liczbowy reprezentujący numer wiersza w tej lokalizacji jest używana jako argument wywołania zamiast domyślnej wartości parametru.

Jeśli wywołanie obejmuje wiele wierszy, wiersz wybrany zależy od implementacji.

Należy zauważyć, że numer wiersza może mieć wpływ `#line` dyrektywy ([wiersz dyrektywy](lexical-structure.md#line-directives)).

#### <a name="the-callerfilepath-attribute"></a>Atrybut CallerFilePath

`System.Runtime.CompilerServices.CallerFilePathAttribute` Jest dozwolone na temat parametrów opcjonalnych, gdy istnieje niejawna konwersja standardowa ([standardowe i niejawne konwersje](conversions.md#standard-implicit-conversions)) z `string` typowi parametru.

Jeśli wywołanie funkcji z lokalizacji w kodzie źródłowym pomija opcjonalny parametr za pomocą `CallerFilePathAttribute`, następnie literał ciągu reprezentujący tej lokalizacji ścieżki pliku jest używana jako argument wywołania zamiast domyślnej wartości parametru.

Format ścieżki pliku zależy od implementacji.

Należy zauważyć, że ścieżka pliku mogą mieć wpływ na `#line` dyrektywy ([wiersz dyrektywy](lexical-structure.md#line-directives)).

#### <a name="the-callermembername-attribute"></a>Atrybut CallerMemberName

`System.Runtime.CompilerServices.CallerMemberNameAttribute` Jest dozwolone na temat parametrów opcjonalnych, gdy istnieje niejawna konwersja standardowa ([standardowe i niejawne konwersje](conversions.md#standard-implicit-conversions)) z `string` typowi parametru.

Wywołania funkcji z lokalizacji, w ramach treści funkcji składowej lub atrybut zastosowania do elementu członkowskiego funkcja sam lub jego typem zwracanym, parametry lub parametrów typu w kodzie źródłowym pomija parametr opcjonalny z `CallerMemberNameAttribute`, a następnie literał ciągu reprezentujący nazwę tego elementu członkowskiego jest używana jako argument wywołania zamiast domyślnej wartości parametru.

Wywołania występujących w ramach metod ogólnych, aby uzyskać nazwę metody, sama jest używany bez lista parametrów typu.

Do wywołania, które występują w implementacji elementu członkowskiego interfejsu jawnego nazwę metody, sama jest używany bez poprzedzającego kwalifikacji interfejsu.

Do wywołania, występujących w ramach metod dostępu właściwości lub zdarzenia nazwę elementu członkowskiego, używana jest właściwość lub zdarzenie, sam.

Wywołania występujących w ramach metod dostępu indeksatora, nazwę elementu członkowskiego, używany jest dostarczona przez `IndexerNameAttribute` ([atrybutu IndexerName](attributes.md#the-indexername-attribute)) elementu członkowskiego indeksatora, jeśli jest obecny lub domyślna nazwa `Item` inaczej.

Do wywołania, znajdujących się w deklaracji konstruktory wystąpień, konstruktorów statycznych, destruktory i operatory elementu członkowskiego używana nazwa jest zależna od implementacji.

## <a name="attributes-for-interoperation"></a>Atrybuty do celów międzyoperacyjności

Uwaga: Ta sekcja ma zastosowanie tylko do wdrożenia programu Microsoft .NET C#.

### <a name="interoperation-with-com-and-win32-components"></a>Współdziałanie z składniki COM i Win32

Środowiska wykonawczego .NET zawiera dużą liczbę atrybutów, które umożliwiają C# programy pod kątem współdziałania ze składnikami napisanymi przy użyciu modelu COM i bibliotek Win32 dll. Na przykład `DllImport` atrybut może być używany na `static extern` metodę w celu wskazania, że implementacji metody ma zostać znaleziona w bibliotece DLL systemu Win32. Te atrybuty znajdują się w `System.Runtime.InteropServices` przestrzeni nazw i szczegółowa dokumentacja tych atrybutów znajduje się w dokumentacji środowiska uruchomieniowego .NET.

### <a name="interoperation-with-other-net-languages"></a>Współdziałanie z innymi językami .NET

#### <a name="the-indexername-attribute"></a>Atrybutu IndexerName

Indeksatory są implementowane w środowisku .NET za pomocą właściwości indeksowanych i mieć nazwę w metadanych platformy .NET. Jeśli nie `IndexerName` atrybut był obecny indeksatora, a następnie nazwę `Item` jest używane domyślnie. `IndexerName` Atrybut umożliwia dla deweloperów zastąpić to ustawienie domyślne i określ inną nazwę.

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
