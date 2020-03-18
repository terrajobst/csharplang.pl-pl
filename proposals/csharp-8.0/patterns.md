---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485064"
---
# <a name="recursive-pattern-matching"></a>Cykliczne dopasowanie wzorca

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Rozszerzenia zgodne z wzorcem umożliwiające C# korzystanie z wielu zalet typów danych algebraicznych i dopasowywanie wzorców z języków funkcjonalnych, ale w sposób, który bezproblemowo integruje się z działaniem języka bazowego. Elementy tego podejścia są inspirowane powiązanymi funkcjami w językach [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Rozszerzalne Dopasowywanie wzorców za pośrednictwem języka lekkiego") programowania i [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Dopasowywanie obiektów ze wzorcami, Strona 273").

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

### <a name="is-expression"></a>Wyrażenie is

Operator `is` jest rozszerzony w celu przetestowania wyrażenia względem *wzorca*.

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

Ta forma *relational_expression* jest uzupełnieniem istniejących formularzy w C# specyfikacji. Jest to błąd czasu kompilacji, jeśli *relational_expression* po lewej stronie tokenu `is` nie wyznaczy wartości lub nie ma typu.

Każdy *Identyfikator* wzorca wprowadza nową zmienną lokalną, która jest *ostatecznie przypisana* po `true` operatora `is` (tj. jest ona *przypisywana w nieskończoność, gdy wartość jest równa true*).

> Uwaga: w `is-expression` i *constant_pattern*jest to technicznie niejednoznaczności między *typem* , z których każda może być prawidłową analizą kwalifikowanego identyfikatora. Staramy się powiązać ją z typem w celu zapewnienia zgodności z poprzednimi wersjami języka; tylko wtedy, gdy to się nie powiedzie, firma Microsoft rozwiązuje ten problem, gdy wykonamy wyrażenie w innych kontekstach, do pierwszego znalezionego (który musi być stałą lub typem). Ta niejednoznaczność jest obecna tylko po prawej stronie wyrażenia `is`.

### <a name="patterns"></a>Wzorce

Wzorce są używane w operatorze *is_pattern* , w *switch_statement*i w *switch_expression* do wyrażania kształtu danych, z których dane przychodzące (wywoływane przez nas wartość wejściową) mają być porównywane. Wzorce mogą być cykliczne, aby części danych mogły być dopasowane do wzorców podrzędnych.

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a>Wzorzec deklaracji

```antlr
declaration_pattern
    : type simple_designation
    ;
```

*Declaration_pattern* obu testuje, że wyrażenie jest danego typu i rzutuje je na ten typ, jeśli test zakończy się pomyślnie. Może to spowodować, że zmienna lokalna danego typu nazywana przez dany identyfikator, jeśli oznaczenie jest *single_variable_designation*. Ta zmienna lokalna jest *ostatecznie przypisana* , gdy wynik operacji dopasowania do wzorca jest `true`.

Semantyka środowiska uruchomieniowego tego wyrażenia sprawdza typ środowiska uruchomieniowego operandu *relational_expression* po lewej stronie względem *typu* we wzorcu.  Jeśli jest to typ środowiska uruchomieniowego (lub niektóre podtypu), a nie `null`, wynik `is operator` jest `true`.

Niektóre kombinacje typu statycznego po lewej stronie i danego typu są uznawane za niezgodne i powodują błąd czasu kompilacji. Wartość typu statycznego `E` jest określana jako *zgodna ze wzorcem* z typem `T`, jeśli istnieje konwersja tożsamości, niejawna konwersja odwołania, konwersja z opakowania, jawna konwersja odwołania lub konwersja rozpakowywania z `E` do `T`lub jeśli jeden z tych typów jest typem otwartym. Jest to błąd czasu kompilacji, jeśli dane wejściowe typu `E` nie są zgodne ze *wzorcem* z *typem* w wzorcu typów, z którym jest on dopasowany.

Wzorzec typu jest przydatny do wykonywania testów typu w czasie wykonywania dla typów referencyjnych i zastępuje idiom

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

O nieco bardziej zwięzły

```csharp
if (expr is Type v) { // code using v
```

Jeśli *Typ* jest typem wartości null, występuje błąd.

Wzorzec typu może służyć do testowania wartości typów dopuszczających wartość null: wartość typu `Nullable<T>` (lub opakowany `T`) pasuje do wzorca typu, `T2 id` Jeśli wartość jest inna niż null, a typ `T2` to `T`lub jakiś typ podstawowy lub interfejs `T`. Na przykład w fragmencie kodu

```csharp
int? x = 3;
if (x is int v) { // code using v
```

Warunek instrukcji `if` jest `true` w czasie wykonywania, a zmienna `v` przechowuje wartość `3` typu `int` wewnątrz bloku. Po bloku Zmienna `v` jest w zakresie, ale nie jest ostatecznie przypisana.

#### <a name="constant-pattern"></a>Wzorzec stałej

```antlr
constant_pattern
    : constant_expression
    ;
```

Stały wzorzec testuje wartość wyrażenia w stosunku do wartości stałej. Stała może być dowolnym wyrażeniem stałym, takim jak literał, nazwa zadeklarowanej zmiennej `const` lub stała wyliczenia. Gdy wartość wejściowa nie jest typu otwartego, wyrażenie stałe jest niejawnie konwertowane na typ dopasowanego wyrażenia; Jeśli typ wartości wejściowej nie jest zgodny ze *wzorcem* z typem wyrażenia stałego, Operacja dopasowania do wzorca jest błędem.

Wzorzec *c* jest uznawany za zgodny ze przekonwertowaną wartością wejściową *e* , jeśli `object.Equals(c, e)` zwróci `true`.

Oczekujemy, że `e is null` jako najbardziej typowym sposobem na przetestowanie `null` w nowo zapisanym kodzie, ponieważ nie można wywoływać `operator==`zdefiniowanej przez użytkownika.

#### <a name="var-pattern"></a>Wzorzec wariancji

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

Jeśli *oznaczenie* jest *simple_designation*, wyrażenie *e* pasuje do wzorca. Inaczej mówiąc, dopasowanie do *wzorca var* zawsze powiedzie się z *simple_designation*. Jeśli *simple_designation* jest *single_variable_designation*, wartość *e* jest związana z nowo wprowadzoną zmienną lokalną. Typem zmiennej lokalnej jest typ statyczny *e*.

Jeśli *oznaczenie* jest *tuple_designation*, wzorzec jest odpowiednikiem *positional_pattern* formularza `(var` *oznaczenie*,... `)`, gdzie *oznaczenie*s znajduje się w *tuple_designation*.  Na przykład wzorzec `var (x, (y, z))` jest równoznaczny z `(var x, (var y, var z))`.

Jeśli nazwa `var` powiązania z typem, występuje błąd.

#### <a name="discard-pattern"></a>Odrzuć wzorzec

```antlr
discard_pattern
    : '_'
    ;
```

Wyrażenie *e* pasuje do wzorca `_` zawsze. Innymi słowy każde wyrażenie pasuje do wzorca odrzucania.

Nie można użyć wzorca odrzucania jako wzorca *is_pattern_expression*.

#### <a name="positional-pattern"></a>Wzorzec pozycyjny

Wzorzec pozycyjny sprawdza, czy wartość wejściowa nie jest `null`, wywołuje odpowiednią metodę `Deconstruct` i wykonuje dalsze Dopasowywanie wzorców do wartości wyników.  Obsługuje również składnię wzorca przypominającą krotkę (bez podanego typu), gdy typ wartości wejściowej jest taki sam jak typ zawierający `Deconstruct`, lub jeśli typ wartości wejściowej jest typem krotki, lub jeśli typ wartości wejściowej to `object` lub `ITuple` i typ środowiska uruchomieniowego w wyrażeniu implementuje `ITuple`.

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

Jeśli *Typ* zostanie pominięty, przyjmujemy, że jest to typ statyczny wartości wejściowej.

Uwzględniając dopasowanie wartości wejściowej do *typu* wzorca `(` *subpattern_list* `)`, metoda jest wybierana przez wyszukiwanie w *typie* dla dostępnych deklaracji `Deconstruct` i wybranie jednej z nich przy użyciu tych samych reguł dla deklaracji dekonstrukcji.

Jeśli *positional_pattern* pomija typ, ma jeden *podwzorzec* bez *identyfikatora*, nie ma *property_subpattern* i nie *simple_designation*. Powoduje to odróżnienie między *constant_pattern* w nawiasach i *positional_pattern*.

W celu wyodrębnienia wartości, aby były zgodne ze wzorcami na liście,
- Jeśli *Typ* został pominięty, a typ wartości wejściowej jest typem krotki, liczba wzorców jest wymagana tak samo jak Kardynalność krotki. Każdy element krotki jest dopasowywany do odpowiadającego mu *podwzorca*, a dopasowanie powiedzie się, jeśli wszystkie z nich zakończyły się powodzeniem. Jeśli którykolwiek z *wzorców* ma *Identyfikator*, to musi należeć do nazwy elementu krotki w odpowiedniej pozycji w typie krotki.
- W przeciwnym razie, Jeśli odpowiednia `Deconstruct` istnieje jako element członkowski *typu*, jest to błąd czasu kompilacji, jeśli typ wartości wejściowej nie jest *zgodny ze wzorcem* *typu*. W czasie wykonywania wartość wejściowa jest testowana względem *typu*. Jeśli to się nie powiedzie, dopasowanie do wzorca pozycyjnego zakończy się niepowodzeniem. Jeśli to się powiedzie, wartość wejściowa jest konwertowana na ten typ, a `Deconstruct` jest wywoływana z użyciem świeżych zmiennych generowanych przez kompilator w celu uzyskania parametrów `out`. Każda odebrana wartość jest dopasowywana do odpowiadającego jej *wzorca*, a dopasowanie powiedzie się, jeśli wszystkie z nich zakończyły się powodzeniem. Jeśli którykolwiek z *wzorców* ma *Identyfikator*, należy nazwać parametr w odpowiedniej pozycji `Deconstruct`.
- W przeciwnym razie, jeśli *Typ* został pominięty, a wartość wejściowa jest typu `object` lub `ITuple` lub jakiś typ, który można przekonwertować na `ITuple` przez niejawną konwersję odwołania, a żaden *Identyfikator* nie pojawia się w podwzorcach, zgadzamy się przy użyciu `ITuple`.
- W przeciwnym razie wzorzec jest błędem czasu kompilacji.

Kolejność, w której podwzorce są dopasowane w czasie wykonywania, nie jest określona, a dopasowanie nie może próbować dopasować wszystkich wzorców.

##### <a name="example"></a>Przykład

Ten przykład używa wielu funkcji opisanych w tej specyfikacji

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a>Wzorzec właściwości

Wzorzec właściwości sprawdza, czy wartość wejściowa nie jest `null` i rekursywnie dopasowuje wartości wyodrębnione przy użyciu dostępnych właściwości lub pól.

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

Jeśli którykolwiek z _wzorców_ _property_pattern_ nie zawiera _identyfikatora_ (musi to być drugi formularz, który ma _Identyfikator_), występuje błąd.  Końcowy przecinek po ostatnim przedwzorcu jest opcjonalny.

Należy zauważyć, że wzorzec sprawdzania wartości null jest spoza prostego wzorca właściwości. Aby sprawdzić, czy ciąg `s` ma wartość różną od null, można napisać dowolną z następujących form

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

Nadano dopasowanie wyrażenia *e* do *typu* wzorca `{` *property_pattern_list* `}`, jest to błąd czasu kompilacji, jeśli wyrażenie *e* nie jest *zgodne ze wzorcem* typu *t* oznaczonego przez *Typ*. Jeśli typ nie jest obecny, przyjmujemy, że jest to typ statyczny *e*. Jeśli *Identyfikator* jest obecny, deklaruje zmienną *wzorca typu Type.* Każdy z identyfikatorów pojawiających się po lewej stronie *property_pattern_list* musi wyznaczyć dostępną właściwość lub pole z *T*. Jeśli *simple_designation* *property_pattern* jest obecny, definiuje zmienną wzorca typu *T*.

W czasie wykonywania wyrażenie jest testowane względem *T*. Jeśli to się nie powiedzie, dopasowanie wzorca właściwości zakończy się niepowodzeniem, a wynik jest `false`. Jeśli to się powiedzie, wszystkie *property_subpattern* pola lub właściwości są odczytywane i ich wartość pasuje do odpowiadającego jej wzorca. Wynik całego dopasowania jest `false` tylko wtedy, gdy wynik któregokolwiek z tych elementów jest `false`. Kolejność, w której są dopasowywane wzorce, nie jest określona, a dopasowanie nie może być zgodne ze wszystkimi podwzorcami w czasie wykonywania. Jeśli dopasowanie się powiedzie, a *simple_designation* *property_pattern* jest *Single_variable_designation*, definiuje zmienną typu *T* , która ma przypisaną dopasowaną wartość.

> Uwaga: wzorzec właściwości może służyć do dopasowania do wzorca przy użyciu typów anonimowych.

##### <a name="example"></a>Przykład

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a>Wyrażenie Switch

*Switch_expression* jest dodawany do obsługi semantyki podobnej do `switch`dla kontekstu wyrażenia.

Składnia C# języka jest rozszerzana o następujące produkcje składniowe:

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

*Switch_expression* nie jest dozwolony jako *expression_statement*.

> Przeglądamy ten złagodzeniu wymagań dotyczących w przyszłej wersji.

Typ *switch_expression* jest [*najlepszym typowym typem*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) wyrażeń występujących po prawej stronie tokenów `=>` *switch_expression_arm*s, jeśli taki typ istnieje, a wyrażenie w każdym ramieniu wyrażenia Switch może być niejawnie konwertowane na ten typ.  Ponadto dodamy nową *konwersję wyrażenia Switch*, która jest wstępnie zdefiniowaną niejawną konwersją z wyrażenia Switch do każdego typu `T`, dla którego istnieje niejawna konwersja z wyrażenia ramienia do `T`.

Jeśli jakiś wzorzec *switch_expression_arm*nie może wpływać na wynik, ponieważ niektóre poprzednie wzorce i zabezpieczenia będą zawsze zgodne, występuje błąd.

Wyrażenie Switch jest uznawane za *wyczerpujące* , jeśli niektóre ARM wyrażenia Switch obsługuje każdą wartość danych wejściowych.  Kompilator tworzy ostrzeżenie, jeśli wyrażenie Switch nie jest *wyczerpujące*.

W czasie wykonywania, wynik *switch_expression* jest wartością *wyrażenia* pierwszego *switch_expression_arm* , dla którego wyrażenie po lewej stronie *switch_expression* dopasowuje wzorzec *switch_expression_arm*i dla którego występuje *case_guard* *switch_expression_arm, a*jeśli jest obecny, oblicza się w `true`. Jeśli nie ma takich *switch_expression_arm*, *switch_expression* zgłasza wystąpienie `System.Runtime.CompilerServices.SwitchExpressionException`wyjątków.

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a>Opcjonalna parens podczas przełączania do literału krotki

Aby można było przełączać się do literału krotki przy użyciu *switch_statement*, należy napisać, co wydaje się być nadmiarowe parens

```csharp
switch ((a, b))
{
```

Aby zezwolić

```csharp
switch (a, b)
{
```

nawiasy instrukcji switch są opcjonalne, gdy wyrażenie, które jest włączane, jest literałem krotki.

### <a name="order-of-evaluation-in-pattern-matching"></a>Kolejność obliczeń w dopasowaniu do wzorca

Zapewnianie elastyczności kompilatora w przypadku zmiany kolejności operacji wykonywanych podczas dopasowywania do wzorca może pozwolić na elastyczność, która może być używana do poprawy efektywności dopasowania do wzorca. Wymaganie (niewymuszone) byłoby, że właściwości, do których uzyskuje się dostęp we wzorcu, a metody dekonstrukcji muszą mieć wartość "czysty" (wolny, idempotentne, itp.). Nie oznacza to, że możemy dodać czystość jako koncepcję języka, tylko dzięki czemu możemy zapewnić elastyczność kompilatora w przypadku operacji zmiany kolejności.

**Rozwiązanie 2018-04-04 LDM**: Confirmed: kompilator może zmieniać kolejność wywołań `Deconstruct`, dostępu do właściwości i wywołań metod w `ITuple`i może założyć, że zwracane wartości są takie same w przypadku wielu wywołań. Kompilator nie powinien wywoływać funkcji, które nie mają wpływu na wynik. przed wprowadzeniem jakichkolwiek zmian w kolejności oceny wygenerowanej przez kompilator w przyszłości będzie bardzo uważnie.

### <a name="some-possible-optimizations"></a>Niektóre możliwe optymalizacje

Kompilacja dopasowania do wzorca może korzystać ze wspólnych części wzorców. Na przykład, jeśli test typu najwyższego poziomu dwóch kolejnych wzorców w *switch_statement* jest tego samego typu, wygenerowany kod może pominąć test typu dla drugiego wzorca.

Jeśli niektóre wzorce są liczbami całkowitymi lub ciągami, kompilator może wygenerować ten sam rodzaj kodu, który generuje dla instrukcji switch we wcześniejszych wersjach języka.

Aby uzyskać więcej informacji na temat tego rodzaju optymalizacji, zobacz [[Scott i Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "Kiedy jest uwzględniana zgodność z algorytmami heurystycznego kompilowania?").
