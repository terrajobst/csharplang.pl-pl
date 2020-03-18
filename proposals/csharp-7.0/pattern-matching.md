---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485169"
---
# <a name="pattern-matching-for-c-7"></a>Dopasowanie wzorca dla C# 7

Rozszerzenia zgodne z wzorcem umożliwiające C# korzystanie z wielu zalet typów danych algebraicznych i dopasowywanie wzorców z języków funkcjonalnych, ale w sposób, który bezproblemowo integruje się z działaniem języka bazowego. Podstawowe funkcje to: [typy rekordów](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), które są typami, których znaczenie semantyczne jest opisane przez kształt danych; i dopasowywanie do wzorca, które jest nowym formularzem wyrażenia, który umożliwia bardzo zwięzłe dekompozycję tych typów danych. Elementy tego podejścia są inspirowane powiązanymi funkcjami w językach [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Rozszerzalne Dopasowywanie wzorców za pośrednictwem języka lekkiego") programowania i [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Dopasowywanie obiektów ze wzorcami").

## <a name="is-expression"></a>Wyrażenie is

Operator `is` jest rozszerzony w celu przetestowania wyrażenia względem *wzorca*.

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

Ta forma *relational_expression* jest uzupełnieniem istniejących formularzy w C# specyfikacji. Jest to błąd czasu kompilacji, jeśli *relational_expression* po lewej stronie tokenu `is` nie wyznaczy wartości lub nie ma typu.

Każdy *Identyfikator* wzorca wprowadza nową zmienną lokalną, która jest *ostatecznie przypisana* po `true` operatora `is` (tj. jest ona *przypisywana w nieskończoność, gdy wartość jest równa true*).

> Uwaga: w `is-expression` i *constant_pattern*jest to technicznie niejednoznaczności między *typem* , z których każda może być prawidłową analizą kwalifikowanego identyfikatora. Staramy się powiązać ją z typem w celu zapewnienia zgodności z poprzednimi wersjami języka; tylko wtedy, gdy to się nie powiedzie, firma Microsoft rozwiązuje ten problem, tak jak w przypadku innych kontekstów, do pierwszego znalezionego (który musi być stałą lub typem). Ta niejednoznaczność jest obecna tylko po prawej stronie wyrażenia `is`.

## <a name="patterns"></a>Wzorce

Wzorce są używane w operatorze `is` i w *switch_statement* do wyrażania kształtu danych, z których dane przychodzące mają być porównywane. Wzorce mogą być cykliczne, aby części danych mogły być dopasowane do wzorców podrzędnych.

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> Uwaga: w `is-expression` i *constant_pattern*jest to technicznie niejednoznaczności między *typem* , z których każda może być prawidłową analizą kwalifikowanego identyfikatora. Staramy się powiązać ją z typem w celu zapewnienia zgodności z poprzednimi wersjami języka; tylko wtedy, gdy to się nie powiedzie, firma Microsoft rozwiązuje ten problem, tak jak w przypadku innych kontekstów, do pierwszego znalezionego (który musi być stałą lub typem). Ta niejednoznaczność jest obecna tylko po prawej stronie wyrażenia `is`.

### <a name="declaration-pattern"></a>Wzorzec deklaracji

*Declaration_pattern* obu testuje, że wyrażenie jest danego typu i rzutuje je na ten typ, jeśli test zakończy się pomyślnie. Jeśli *simple_designation* jest identyfikatorem, wprowadza zmienną lokalną danego typu o podanym identyfikatorze. Ta zmienna lokalna jest *ostatecznie przypisana* , gdy wynik operacji dopasowania do wzorca ma wartość true.

```antlr
declaration_pattern
    : type simple_designation
    ;
```

Semantyka środowiska uruchomieniowego tego wyrażenia sprawdza typ środowiska uruchomieniowego operandu *relational_expression* po lewej stronie względem *typu* we wzorcu. W przypadku tego typu środowiska uruchomieniowego (lub niektórych podtypów) wynik `is operator` jest `true`. Deklaruje nową zmienną lokalną o nazwie, która ma przypisaną wartość operandu po lewej *stronie, gdy* wynik jest `true`.

Niektóre kombinacje typu statycznego po lewej stronie i danego typu są uznawane za niezgodne i powodują błąd czasu kompilacji. Wartość typu statycznego `E` jest uznawana za *wzorzec zgodną* z typem `T`, jeśli istnieje konwersja tożsamości, niejawna konwersja odwołania, konwersja z opakowania, jawna konwersja odwołania lub konwersja rozpakowywanie z `E` do `T`. Jest to błąd czasu kompilacji, jeśli wyrażenie typu `E` nie jest zgodne ze wzorcem typu w wzorcu typów, z którym jest on dopasowany.

> Uwaga: [w C# 7,1 rozszerzającmy to](../csharp-7.1/generics-pattern-match.md) , aby umożliwić operacji dopasowania do wzorca, jeśli typ danych wejściowych lub typ `T` jest typem otwartym. Ten akapit jest zastępowany przez następujące elementy:
> 
> Niektóre kombinacje typu statycznego po lewej stronie i danego typu są uznawane za niezgodne i powodują błąd czasu kompilacji. Wartość typu statycznego `E` jest uznawana za *wzorzec zgodną* z typem `T`, jeśli istnieje konwersja tożsamości, niejawna konwersja odwołania, konwersja z opakowania, jawna konwersja odwołania lub konwersja rozpakowywania z `E` do `T`**lub jeśli zarówno `E`, jak i `T` jest typem otwartym**. Jest to błąd czasu kompilacji, jeśli wyrażenie typu `E` nie jest zgodne ze wzorcem typu w wzorcu typów, z którym jest on dopasowany.

Wzorzec deklaracji jest przydatny do wykonywania testów typu w czasie wykonywania typów referencyjnych i zastępuje idiom

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

O nieco bardziej zwięzły

```csharp
if (expr is Type v) { // code using v }
```

Jeśli *Typ* jest typem wartości null, występuje błąd.

Wzorzec deklaracji może służyć do testowania wartości typów dopuszczających wartość null: wartość typu `Nullable<T>` (lub opakowany `T`) pasuje do wzorca typu, `T2 id` Jeśli wartość jest inna niż null, a typ `T2` to `T`lub jakiś typ podstawowy lub interfejs `T`. Na przykład w fragmencie kodu

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

Warunek instrukcji `if` jest `true` w czasie wykonywania, a zmienna `v` przechowuje wartość `3` typu `int` wewnątrz bloku.

### <a name="constant-pattern"></a>Wzorzec stałej

```antlr
constant_pattern
    : shift_expression
    ;
```

Stały wzorzec testuje wartość wyrażenia w stosunku do wartości stałej. Stała może być dowolnym wyrażeniem stałym, takim jak literał, nazwa zadeklarowanej zmiennej `const` lub stała wyliczenia lub wyrażenie `typeof`.

Jeśli zarówno *e* , jak i *c* są typami całkowitymi, wzorzec jest uznawany za dopasowany, jeśli wynik wyrażenia `e == c` jest `true`.

W przeciwnym razie wzorzec jest uznawany za pasujący, jeśli `object.Equals(e, c)` zwraca `true`. W takim przypadku jest to błąd czasu kompilacji, jeśli typ statyczny *e* nie jest zgodny ze *wzorcem* typu stałej.

### <a name="var-pattern"></a>wzorzec wariancji

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

Wyrażenie *e* pasuje do *var_pattern* zawsze. Inaczej mówiąc, dopasowanie do *wzorca var* zawsze powiedzie się. Jeśli *simple_designation* jest identyfikatorem, wówczas w czasie wykonywania wartość *e* jest powiązana z nowo wprowadzoną zmienną lokalną. Typem zmiennej lokalnej jest typ statyczny *e*.

Jeśli nazwa `var` powiązania z typem, występuje błąd.

## <a name="switch-statement"></a>Switch, instrukcja

Instrukcja `switch` jest rozszerzona, aby wybrać do wykonania pierwszy blok ze skojarzonym wzorcem zgodnym z *wyrażeniem Switch*.

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

Kolejność dopasowywania wzorców nie jest zdefiniowana. Kompilator może dopasować wzorce poza kolejnością i ponownie wykorzystać wyniki już dopasowanych wzorców w celu obliczenia wyniku dopasowania innych wzorców.

Jeśli jest obecna wartość *Case-Guard* , jej wyrażenie jest typu `bool`. Jest on oceniany jako dodatkowy warunek, który musi być spełniony dla przypadku, aby można go było uznać za spełniony.

Jeśli *switch_label* nie ma wpływu na środowisko uruchomieniowe, występuje błąd, ponieważ jego wzorzec jest subsumed przez poprzednie przypadki. [Do zrobienia: mamy dokładniejsze informacje o technikach, których kompilator musi użyć do osiągnięcia tego orzeczenia.]

Zmienna wzorca zadeklarowana w *switch_label* jest ostatecznie przypisana w jej bloku Case, jeśli i tylko wtedy, gdy ten blok case zawiera dokładnie jeden *switch_label*.

[Do zrobienia: należy określić, kiedy *blok przełącznika* jest osiągalny.]

### <a name="scope-of-pattern-variables"></a>Zakres zmiennych wzorca

Zakres zmiennej zadeklarowanej we wzorcu jest następujący:

- Jeśli wzorzec jest etykietą przypadku, wówczas zakres zmiennej jest *blokiem przypadku*.

W przeciwnym razie zmienna jest zadeklarowana w wyrażeniu *is_pattern* , a jej zakres jest oparty na konstrukcji bezpośrednio otaczającej wyrażenie zawierające wyrażenie *is_pattern* w następujący sposób:

- Jeśli wyrażenie znajduje się w wyrażeniu lambda, jego zakres jest treścią wyrażenia lambda.
- Jeśli wyrażenie znajduje się w metodzie lub właściwości będącej częścią wyrażenia, jej zakres jest treścią metody lub właściwości.
- Jeśli wyrażenie znajduje się w klauzuli `when` klauzuli `catch`, jej zakresem jest `catch` klauzula.
- Jeśli wyrażenie znajduje się w *iteration_statement*, jego zakres jest tylko tą instrukcją.
- W przeciwnym razie, jeśli wyrażenie znajduje się w innym formularzu instrukcji, jego zakres jest zakresem zawierającym instrukcję.

Na potrzeby określenia zakresu *embedded_statement* jest uznawana za w swoim własnym zakresie. Na przykład Gramatyka dla *if_statement* jest

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Dlatego jeśli kontrolowana instrukcja *if_statement* deklaruje zmienną wzorca, jej zakres jest ograniczony do tego *embedded_statement*:

```csharp
if (x) M(y is var z);
```

W takim przypadku zakres `z` jest osadzoną instrukcją `M(y is var z);`.

Inne przypadki są błędami z innych powodów (np. w wartości domyślnej parametru lub atrybutu, co jest spowodowane błędem, ponieważ te konteksty wymagają wyrażenia stałego).

> [W C# 7,3 dodaliśmy następujące konteksty](../csharp-7.3/expression-variables-in-initializers.md) , w których można zadeklarować zmienną wzorca:
> - Jeśli wyrażenie znajduje się w *inicjatorze konstruktora*, jego zakres jest *inicjatorem konstruktora* i treścią konstruktora.
> - Jeśli wyrażenie znajduje się w inicjatorze pola, jego zakres jest *equals_value_clause* , w którym występuje.
> - Jeśli wyrażenie znajduje się w klauzuli zapytania, która jest określona do przetłumaczenia na treść wyrażenia lambda, jego zakresem jest tylko to wyrażenie.

## <a name="changes-to-syntactic-disambiguation"></a>Zmiany dotyczące uściślania składni

Istnieją sytuacje, w których C# Gramatyka jest niejednoznaczna, a Specyfikacja języka informuje, jak rozwiązać te niejasności:

> #### <a name="7652-grammar-ambiguities"></a>7.6.5.2 gramatyki niejasności
> Produkcje dla *prostej nazwy* (§ 7.6.3) i *dostęp do elementów członkowskich* (§ 7.6.5) mogą dać podstawę niejasności w gramatyce wyrażeń. Na przykład, instrukcja:
> ```csharp
> F(G<A,B>(7));
> ```
> może być interpretowany jako wywołanie `F` z dwoma argumentami, `G < A` i `B > (7)`. Alternatywnie, można go interpretować jako wywołanie do `F` z jednym argumentem, który jest wywołaniem metody generycznej `G` z dwoma argumentami typu i jednym argumentem regularnym.

> Jeśli sekwencja tokenów może być analizowana (w kontekście) jako *prosta nazwa* (§ 7.6.3), *dostęp do elementu członkowskiego* (§ 7.6.5) lub *wskaźnik — dostęp do elementów członkowskich* (§ 18.5.2) kończący się na *listą argumentów typu* (§ 4.4.1), token bezpośrednio po zamykającym tokenie `>` zostanie sprawdzony. Jeśli jest to jedna z
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> następnie *Typ argumentu-list* jest zachowywany jako część *prostej nazwy*, *dostęp do składowej* lub wskaźnika — dostęp do elementu *Członkowskiego* i wszelkie inne możliwe analizy sekwencji tokenów są odrzucane. W przeciwnym razie *Lista argumentów typu* nie jest uważana za część *prostej nazwy*, *dostęp do składowej* lub > *wskaźnik — dostęp do elementu członkowskiego*, nawet jeśli nie istnieje inna możliwa analiza sekwencji tokenów. Należy pamiętać, że te reguły nie są stosowane podczas analizowania *listy argumentów typu* w *przestrzeni nazw lub-type-name* (§ 3,8). Instrukcja
> ```csharp
> F(G<A,B>(7));
> ```
> zgodnie z tą regułą, należy interpretować jako wywołanie `F` z jednym argumentem, który jest wywołaniem metody generycznej `G` z dwoma argumentami typu i jednym argumentem regularnym. Instrukcje
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> Każdy z nich będzie interpretowany jako wywołanie `F` z dwoma argumentami. Instrukcja
> ```csharp
> x = F < A > +y;
> ```
> będzie interpretowany jako operator mniejszości, operator większości i operator jednoargumentowy, tak jakby instrukcja była zapisywana `x = (F < A) > (+y)`, a nie jako *Nazwa prosta* z *listą argumentów typu* , po której następuje operator binarny Plus. W instrukcji
> ```csharp
> x = y is C<T> + z;
> ```
> tokeny `C<T>` są interpretowane jako *przestrzeń nazw lub-type-name* z *listą argumentów typu*.

W programie C# wprowadzono wiele zmian, które nie są już wystarczające, aby obsłużyć złożoność języka.

### <a name="out-variable-declarations"></a>Deklaracje zmiennych wyjściowych

Teraz można zadeklarować zmienną w argumencie out:

```csharp
M(out Type name);
```

Jednak typ może być ogólny: 

```csharp
M(out A<B> name);
```

Ponieważ Gramatyka języka dla argumentu używa *wyrażenia*, ten kontekst podlega regule uściślania. W tym przypadku `>` zamykające następuje *Identyfikator*, który nie jest jednym z tokenów, które zezwala na traktowanie jako *listy argumentów typu*. W związku z tym proponuję **dodać *Identyfikator* do zestawu tokenów, które wyzwalają Uściślanie do *listy argumentów typu*.**

### <a name="tuples-and-deconstruction-declarations"></a>Kolekcje i deklaracje dekonstrukcji

Literał krotki działa dokładnie na ten sam problem. Rozważ wyrażenie krotki

```csharp
(A < B, C > D, E < F, G > H)
```

W ramach starych C# 6 reguł analizowania listy argumentów, będzie ona analizowana jako krotka z czterema elementami, rozpoczynając od `A < B` jako pierwszy. Jeśli jednak ta opcja pojawia się po lewej stronie dekonstrukcji, chcemy, aby takie Uściślanie zostało wyzwolone przez token *identyfikatora* , jak opisano powyżej:

```csharp
(A<B,C> D, E<F,G> H) = e;
```

Jest to Deklaracja dekonstrukcja, która deklaruje dwie zmienne, z których pierwszy jest typu `A<B,C>` i nazwanych `D`. Innymi słowy, literał krotki zawiera dwa wyrażenia, z których każdy jest wyrażeniem deklaracji.

W celu uproszczenia specyfikacji i kompilatora zaproponuję przeanalizowanie tego literału krotki jako spójnej kolekcji w dowolnym miejscu (niezależnie od tego, czy jest ona wyświetlana po lewej stronie przypisania). Jest to naturalny wynik uściślania opisany w poprzedniej sekcji.

### <a name="pattern-matching"></a>Dopasowanie do wzorca

Dopasowanie wzorca wprowadza nowy kontekst, w którym występuje niejednoznaczność typu wyrażenia. Wcześniej po prawej stronie operatora `is` było typu. Teraz może być typem lub wyrażeniem, a jeśli jest to typ, po którym może następować identyfikator. Pozwala to na techniczne zmianę znaczenia istniejącego kodu:

```csharp
var x = e is T < A > B;
```

Można to przeanalizować w ramach reguł języka C# 6 jako

```csharp
var x = ((e is T) < A) > B;
```

ale zgodnie z regułami języka C# 7 (z powyższymi uściślaniemi) zostałaby przeanalizowana jako

```csharp
var x = e is T<A> B;
```

który deklaruje zmienną `B` typu `T<A>`. Na szczęście kompilatory natywne i Roslyn mają usterkę, która daje w wyniku błąd składniowy w kodzie C# 6. W związku z tym ta konkretna zmiana nie jest problemem.

Dopasowanie do wzorca wprowadza dodatkowe tokeny, które powinny mieć na celu rozwiązanie niejednoznaczności w kierunku wybierania typu. Poniższe przykłady istniejącego prawidłowego kodu w języku C# 6 byłyby przerwane bez dodatkowych reguł uściślania:

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a>Proponowana zmiana reguły uściślania

Proponuję zmianę specyfikacji, aby zmienić listę niejednoznacznych tokenów z

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

na

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

A w niektórych kontekstach traktujemy *Identyfikator* jako niejednoznaczny token. Te konteksty są, w których kolejność niejednoznacznych tokenów jest bezpośrednio poprzedzona przez jedno ze słów kluczowych `is`, `case`lub `out`lub występuje podczas analizowania pierwszego elementu literału krotki (w którym przypadku tokeny są poprzedzone `(` lub `:`, a następnie identyfikatorem następuje `,`) lub po kolejnym elemencie literału krotki.

### <a name="modified-disambiguation-rule"></a>Zmodyfikowano regułę uściślania

Zmieniona reguła uściślania będzie wyglądać podobnie do tego

> Jeśli sekwencja tokenów może być analizowana (w kontekście) jako *prosta nazwa* (§ 7.6.3), *dostęp do elementu członkowskiego* (§ 7.6.5) lub *wskaźnik — dostęp do elementów członkowskich* (§ 18.5.2) kończący się na *listą argumentów typu* (§ 4.4.1), token bezpośrednio po zamykającym tokenie `>` jest sprawdzany, aby sprawdzić, czy jest
> - Jeden z `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; oraz
> - Jeden z operatorów relacyjnych `<  >  <=  >=  is as`; oraz
> - Słowo kluczowe zapytania kontekstowego pojawia się wewnątrz wyrażenia zapytania; oraz
> - W niektórych kontekstach traktujemy *Identyfikator* jako niejednoznaczny token. Te konteksty są, w których kolejność niejednoznacznych tokenów jest bezpośrednio poprzedzona przez jedno z słów kluczowych `is`, `case` lub `out`lub występuje podczas analizowania pierwszego elementu literału krotki (w którym przypadku tokeny są poprzedzone przez `(` lub `:`, a następnie identyfikatorem następuje `,`) lub kolejnego elementu literału krotki.
> 
> Jeśli Poniższy token znajduje się na tej liście lub identyfikator w tym kontekście, a następnie *Lista argumentów typu* jest zachowywana w ramach *prostej nazwy*, *dostęp do elementu członkowskiego* lub *wskaźnika — dostęp do elementu członkowskiego* i wszelkie inne możliwe analizy sekwencji tokenów są odrzucane.  W przeciwnym razie *Lista argumentów typu* nie jest uważana za część *prostej nazwy*, *dostępu do składowej* lub *wskaźnika do elementu członkowskiego*, nawet jeśli nie istnieje inna możliwa analiza sekwencji tokenów. Należy pamiętać, że te reguły nie są stosowane podczas analizowania *listy argumentów typu* w *przestrzeni nazw lub-type-name* (§ 3,8).

### <a name="breaking-changes-due-to-this-proposal"></a>Przerywanie zmian spowodowanych tą propozycją

Nie są znane żadne istotne zmiany spowodowane tym proponowaną regułą uściślania.

### <a name="interesting-examples"></a>Interesujące przykłady

Poniżej przedstawiono interesujące wyniki tych reguł uściślania:

Wyrażenie `(A < B, C > D)` jest krotką zawierającą dwa elementy, każde porównanie.

Wyrażenie `(A<B,C> D, E)` jest krotką z dwoma elementami, z których pierwszy jest wyrażeniem deklaracji.

Wywołanie `M(A < B, C > D, E)` ma trzy argumenty.

`M(out A<B,C> D, E)` wywołania ma dwa argumenty, z których pierwszy jest deklaracją `out`.

Wyrażenie `e is A<B> C` używa wyrażenia deklaracji.

Etykieta przypadku `case A<B> C:` używa wyrażenia deklaracji.

## <a name="some-examples-of-pattern-matching"></a>Przykłady dopasowania do wzorca

### <a name="is-as"></a>Jest jako

Możemy zastąpić idiom

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

Z nieco bardziej zwięzłe i bezpośrednie

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a>Testowanie dopuszczające wartość null

Możemy zastąpić idiom

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

Z nieco bardziej zwięzłe i bezpośrednie

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a>Uproszczenie arytmetyczne

Załóżmy, że definiujemy zestaw typów cyklicznych do reprezentowania wyrażeń (na oddzielną propozycję):

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

Teraz możemy zdefiniować funkcję, która będzie obliczać (niezredukowany) pochodną wyrażenia:

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

Uproszczenie wyrażenia demonstruje wzorce pozycyjne:

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
