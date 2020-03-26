---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281960"
---
# <a name="records-work-in-progress"></a>Rejestruje prace w toku

W przeciwieństwie do innych rekordów propozycje nie jest to propozycja, ale prace w toku zaprojektowane do rejestrowania jednomyślnych decyzji projektowych dotyczących funkcji Records. Szczegóły specyfikacji zostaną dodane w razie potrzeby w celu rozwiązania pytań.

Składnia dla rekordu jest proponowana do dodania w następujący sposób:

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

`attributes` inne niż Terminal zezwoli również na nowy atrybut kontekstowy, `data`.

Klasa (struktura) zadeklarowana z listą parametrów lub modyfikatorem `data` jest nazywana klasą rekordów (strukturą rekordów), z których każdy jest typem rekordu.

Jest to błąd do deklarowania typu rekordu bez zarówno listy parametrów, jak i modyfikatora `data`.

## <a name="members-of-a-record-type"></a>Elementy członkowskie typu rekordu

Oprócz elementów członkowskich zadeklarowanych w treści klasy, typ rekordu ma następujące dodatkowe elementy członkowskie:

### <a name="primary-constructor"></a>Konstruktor podstawowy

Typ rekordu ma Konstruktor publiczny, którego sygnatura odnosi się do parametrów wartości deklaracji typu. Jest to nazywane konstruktorem podstawowym dla typu i powoduje pominięcie niejawnie zadeklarowanego konstruktora domyślnego. Wystąpił błąd w przypadku konstruktora podstawowego i konstruktora o tym samym podpisie w klasie.
W czasie wykonywania podstawowy Konstruktor 

1. wykonuje inicjatory pola wystąpienia pojawiające się w treści klasy; a następnie wywołuje konstruktora klasy bazowej bez argumentów.

1. Inicjuje pola zapasowe generowane przez kompilator dla właściwości odpowiadających parametrom wartości (jeśli te właściwości są dostarczane przez kompilator; zobacz [właściwości z syntezą](#Synthesized Properties))


[] Do zrobienia: Dodaj składnię podstawowego wywołania i specyfikację dotyczącą wybierania konstruktora podstawowego przy użyciu rozdzielczości przeciążenia

### <a name="properties"></a>Właściwości

Dla każdego parametru rekordu deklaracji typu rekordu istnieje odpowiedni publiczny element członkowski właściwości, którego nazwa i typ są pobierane z deklaracji parametru wartości. Jeśli żadna konkretna Właściwość (tj. non-abstract) z metodą dostępu get i z tą nazwą i typem jest jawnie zadeklarowana lub dziedziczona, jest generowana przez kompilator w następujący sposób:

Dla struktury rekordu lub klasy rekordu:

* Zostanie utworzona publiczna właściwość "Get-Only". Jej wartość jest inicjowana podczas konstruowania z wartością odpowiadającego parametru podstawowego konstruktora. Każdy "dopasowany" Dziedziczony Właściwość abstract get jest zastępowany.

### <a name="equality-members"></a>Równość elementów członkowskich

Typy rekordów generują wdrożone implementacje dla następujących metod:

* `object.GetHashCode()` przesłonięcia, chyba że jest zapieczętowany lub podany przez użytkownika
* `object.Equals(object)` przesłonięcia, chyba że jest zapieczętowany lub podany przez użytkownika
* Metoda `T Equals(T)`, gdzie `T` jest bieżącym typem

`T Equals(T)` jest określony w celu przeprowadzenia równości wartości, porównując właściwość z tą samą nazwą jak każdy parametr konstruktora podstawowego z odpowiednią właściwością innego typu.
`object.Equals` wykonuje odpowiednik

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a>wyrażenie `with`

Wyrażenie `with` jest nowym wyrażeniem używającym następującej składni.

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

Wyrażenie `with` umożliwia "mutację nieniszczącą", która jest przeznaczona do tworzenia kopii wyrażenia odbiorcy z modyfikacjami właściwości wymienionymi w `anonymous_object_initializer`.

Prawidłowe wyrażenie `with` ma odbiorcę z typem innym niż void. Typ odbiornika musi zawierać dostępną metodę wystąpienia o nazwie `With` z odpowiednimi parametrami i typem zwracanym. Jeśli istnieje wiele metod `With` zastąpień, występuje błąd. Jeśli istnieje wiele `With` zastąpień, musi istnieć Metoda `With` niezastępująca, która jest metodą docelową. W przeciwnym razie należy mieć dokładnie jedną metodę `With`.

Po prawej stronie wyrażenia `with` jest `anonymous_object_initializer` z sekwencją przypisań z polem lub właściwością odbiornika po lewej stronie przypisania, a dowolnym wyrażeniem po prawej stronie, które jest niejawnie konwertowane na typ lewej strony...

Uwzględniając docelową metodę `With`, zwracany typ musi być typem wyrażenia odbiorcy lub typem podstawowym. Dla każdego parametru metody `With` musi istnieć dostępne odpowiednie pole wystąpienia lub właściwość możliwa do odczytu w typie odbiornika o tej samej nazwie i typie. Każda właściwość lub pole znajdujące się po prawej stronie wyrażenia with musi również odpowiadać parametrowi o tej samej nazwie w metodzie `With`.

Mając prawidłową metodę `With`, Obliczanie wyrażenia `with` jest równoznaczne z wywołaniem metody `With` z wyrażeniami w `anonymous_object_initializer` zastępują dla parametru o tej samej nazwie, co właściwość po lewej stronie. Jeśli nie ma żadnej pasującej właściwości dla danego parametru w `anonymous_object_initializer`, argument jest oszacowaniem pola lub właściwości o tej samej nazwie w odbiorniku.

Kolejność oceny efektów ubocznych jest następująca, a każde wyrażenie jest oceniane dokładnie raz:

1. Wyrażenie odbiorcy

2. Wyrażenia w `anonymous_object_initializer`w kolejności leksykalnej

3. Obliczanie wszelkich właściwości pasujących do parametrów metody `With` w kolejności definicji parametrów metody `With`.