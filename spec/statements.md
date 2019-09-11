---
ms.openlocfilehash: 94346034a667ad4af26796c0c4bbc96d6ed79aba
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876837"
---
# <a name="statements"></a>Instrukcje

C#zawiera różne instrukcje. Większość z tych instrukcji będzie znana deweloperom, którzy zaprogramowany w języku C i C++.

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

*Embedded_statement* nie jest używany do instrukcji, które są wyświetlane w innych instrukcjach. Użycie *embedded_statement* zamiast *instrukcji* wyklucza użycie instrukcji deklaracji oraz instrukcji oznaczonych etykietami w tych kontekstach. Przykład
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
wynikiem jest błąd czasu kompilacji, ponieważ `if` instrukcja wymaga elementu *embedded_statement* , a nie *instrukcji* dla jego gałęzi IF. Jeśli ten kod był dozwolony, zmienna `i` zostałaby zadeklarowana, ale nigdy nie można jej użyć. Należy jednak pamiętać, że przez umieszczenie `i`deklaracji w bloku, przykład jest prawidłowy.

## <a name="end-points-and-reachability"></a>Punkty końcowe i osiągalność

Każda instrukcja ma ***punkt końcowy***. W intuicyjnych warunkach punkt końcowy instrukcji jest lokalizacją, która jest bezpośrednio zgodna z instrukcją. Reguły wykonywania dla złożonych instrukcji (Instrukcje zawierające osadzone instrukcje) określają akcję, która jest wykonywana, gdy kontrolka osiągnie punkt końcowy osadzonej instrukcji. Na przykład, gdy formant osiągnie punkt końcowy instrukcji w bloku, formant jest przenoszony do następnej instrukcji w bloku.

Jeśli instrukcja może zostać osiągnięta przez wykonanie, instrukcja jest określana jako ***osiągalna***. Z drugiej strony, jeśli nie jest możliwe, że instrukcja zostanie wykonana, instrukcja jest określana jako ***nieosiągalna***.

W przykładzie
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
drugie wywołanie `Console.WriteLine` jest nieosiągalne, ponieważ nie ma możliwości wykonania instrukcji.

Ostrzeżenie jest zgłaszane, gdy kompilator ustali, że instrukcja jest nieosiągalna. Nie jest to błąd, aby instrukcja była nieosiągalna.

Aby określić, czy dana instrukcja lub punkt końcowy są osiągalne, kompilator wykonuje analizę przepływu zgodnie z regułami osiągalności określonymi dla każdej instrukcji. Analiza przepływu uwzględnia wartości wyrażeń stałych ([wyrażeń stałych](expressions.md#constant-expressions)) kontrolujących zachowanie instrukcji, ale nie są brane pod uwagę możliwe wartości wyrażeń niestałych. Innymi słowy, w celach analizy przepływu sterowania, niestałe wyrażenie danego typu jest uznawane za ma dowolną możliwą wartość tego typu.

W przykładzie
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
wyrażenie `if` logiczne instrukcji jest wyrażeniem stałym, ponieważ oba operandy `==` operatora są stałymi. Ponieważ wyrażenie stałe jest oceniane w czasie kompilacji, przez wygenerowanie wartości `false` `Console.WriteLine` , wywołanie jest uznawane za nieosiągalne. Jeśli `i` jednak zostanie zmieniony jako zmienna lokalna
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
`Console.WriteLine` wywołanie jest uznawane za osiągalne, mimo że w rzeczywistości nigdy nie zostanie wykonane.

*Blok* składowej funkcji jest zawsze uznawany za osiągalny. Poprzez dokonanie oceny reguł osiągalności każdej instrukcji w bloku, można określić osiągalność każdej z tych instrukcji.

W przykładzie
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
osiągalność sekundy `Console.WriteLine` jest określana w następujący sposób:

*  Pierwsza `Console.WriteLine` Instrukcja wyrażenia jest osiągalna, ponieważ blok `F` metody jest osiągalny.
*  Punkt końcowy pierwszej `Console.WriteLine` instrukcji wyrażenia jest osiągalny, ponieważ ta instrukcja jest osiągalna.
*  Instrukcja jest osiągalna, ponieważ punkt końcowy pierwszej `Console.WriteLine` instrukcji wyrażenia jest osiągalny. `if`
*  Druga `Console.WriteLine` Instrukcja wyrażenia jest osiągalna, ponieważ wyrażenie `if` logiczne instrukcji nie ma stałej wartości `false`.

Istnieją dwie sytuacje, w których jest to błąd czasu kompilacji dla punktu końcowego instrukcji, aby uzyskać dostęp:

*  `switch` Ponieważ instrukcja nie zezwala sekcji Switch na "przechodzenie" do następnej sekcji Switch, jest to błąd czasu kompilacji dla punktu końcowego listy instrukcji przełącznika, aby uzyskać dostęp. W przypadku wystąpienia tego błędu jest to zazwyczaj wskazanie `break` braku instrukcji.
*  Jest to błąd czasu kompilacji dla punktu końcowego bloku elementu członkowskiego funkcji, który oblicza wartość jako osiągalną. W przypadku wystąpienia tego błędu zwykle jest to wskazanie, że `return` brakuje instrukcji.

## <a name="blocks"></a>Bloki

*Blok* umożliwia zapisanie wielu instrukcji w kontekstach, w których Pojedyncza instrukcja jest dozwolona.

```antlr
block
    : '{' statement_list? '}'
    ;
```

*Blok* składa się z opcjonalnych *statement_list* ([list instrukcji](statements.md#statement-lists)) ujętych w nawiasy klamrowe. Jeśli lista instrukcji zostanie pominięta, blok jest określany jako pusty.

Blok może zawierać instrukcje deklaracji ([instrukcje deklaracji](statements.md#declaration-statements)). Zakres zmiennej lokalnej lub stałej zadeklarowanej w bloku jest blokiem.

Blok jest wykonywany w następujący sposób:

*  Jeśli blok jest pusty, sterowanie jest przekazywane do punktu końcowego bloku.
*  Jeśli blok nie jest pusty, sterowanie jest przekazywane do listy instrukcji. Gdy i Jeśli kontrolka osiągnie punkt końcowy listy instrukcji, sterowanie jest przekazywane do punktu końcowego bloku.

Lista instrukcji bloku jest osiągalna, jeśli blok jest osiągalny.

Punkt końcowy bloku jest osiągalny, jeśli blok jest pusty lub jeśli punkt końcowy listy instrukcji jest osiągalny.

*Blok* zawierający jedną lub więcej `yield` instrukcji ([instrukcja Yield](statements.md#the-yield-statement)) nosi nazwę bloku iteratora. Bloki iteratorów są używane do implementowania składowych funkcji jako Iteratory ([Iteratory](classes.md#iterators)). Dodatkowe ograniczenia dotyczą bloków iteratora:

*  Jest to błąd czasu kompilacji dla `return` instrukcji, która ma być wyświetlana w bloku iteratora (ale `yield return` instrukcje są dozwolone).
*  Jest to błąd czasu kompilacji dla bloku iteratora, który zawiera niebezpieczny kontekst ([niebezpieczne konteksty](unsafe-code.md#unsafe-contexts)). Blok iteratora zawsze definiuje bezpieczny kontekst, nawet jeśli jego deklaracja jest zagnieżdżona w niebezpiecznym kontekście.

### <a name="statement-lists"></a>Listy instrukcji

***Lista instrukcji*** składa się z jednej lub kilku instrukcji utworzonych w sekwencji. Listy instrukcji występują w *blokach bloku*s ([bloki](statements.md#blocks)) i w *switch_block*s ([instrukcja SWITCH](statements.md#the-switch-statement)).

```antlr
statement_list
    : statement+
    ;
```

Lista instrukcji jest wykonywana przez przeniesienie kontroli do pierwszej instrukcji. Gdy i Jeśli kontrolka osiągnie punkt końcowy instrukcji, sterowanie jest przekazywane do następnej instrukcji. Gdy i Jeśli kontrolka osiągnie punkt końcowy ostatniej instrukcji, sterowanie jest przekazywane do punktu końcowego listy instrukcji.

Instrukcja na liście instrukcji jest dostępna, jeśli co najmniej jeden z następujących warunków jest spełniony:

*  Instrukcja jest pierwszą instrukcją i sama lista instrukcji jest osiągalna.
*  Punkt końcowy poprzedniej instrukcji jest osiągalny.
*  Instrukcja jest instrukcją z etykietą, a etykieta jest przywoływana przez instrukcję `goto` osiągalną.

Punkt końcowy listy instrukcji jest dostępny, jeśli punkt końcowy ostatniej instrukcji na liście jest osiągalny.

## <a name="the-empty-statement"></a>Pusta instrukcja

*Empty_statement* nic nie robi.

```antlr
empty_statement
    : ';'
    ;
```

Pusta instrukcja jest używana, gdy nie ma żadnych operacji do wykonania w kontekście, w którym instrukcja jest wymagana.

Wykonanie pustej instrukcji po prostu przenosi kontrolę do punktu końcowego instrukcji. W związku z tym punkt końcowy pustej instrukcji jest osiągalny, jeśli pusta instrukcja jest osiągalna.

Pustą instrukcję można użyć podczas pisania `while` instrukcji z pustą treścią:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Ponadto można użyć pustej instrukcji, aby zadeklarować etykietę tuż przed zamykaniem "`}`" bloku:
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>Instrukcje oznaczone

*Labeled_statement* umożliwia prefiks instrukcji poprzedzonej etykietą. Instrukcje z etykietami są dozwolone w blokach, ale nie są dozwolone jako osadzone instrukcje.

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Instrukcja labeled deklaruje etykietę o nazwie podaną przez *Identyfikator*. Zakres etykiety to cały blok, w którym jest zadeklarowana etykieta, w tym wszelkie zagnieżdżone bloki. Jest to błąd czasu kompilacji dla dwóch etykiet o tej samej nazwie, aby mieć nakładające się zakresy.

Do etykiet można odwoływać się `goto` z instrukcji ([instrukcji goto](statements.md#the-goto-statement)) w zakresie etykiety. Oznacza to, `goto` że instrukcje mogą przekazywać kontrolę w blokach i poza bloki, ale nigdy nie w blokach.

Etykiety mają własne miejsce deklaracji i nie zakłócają innych identyfikatorów. Przykład
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
jest prawidłowy i używa nazwy `x` zarówno jako parametru, jak i etykiety.

Wykonanie instrukcji oznaczonej etykietą odpowiada dokładnie na wykonanie instrukcji następującej po etykiecie.

Oprócz osiągalności zapewnianej przez normalny przepływ sterowania, instrukcja z etykietą jest osiągalna, jeśli do etykiety odwołuje się osiągalna `goto` instrukcja. Oprócz `finally` `try` `finally` Jeśli instrukcja znajduje się wewnątrz, `try` która zawiera blok, a instrukcja z etykietą jest poza, a punkt końcowy bloku jest nieosiągalny, a następnie instrukcja z etykietą nie jest dostępna od `goto` tej `goto` instrukcji).

## <a name="declaration-statements"></a>Deklaracje deklaracji

*Declaration_statement* deklaruje lokalną zmienną lub stałą. Instrukcje deklaracji są dozwolone w blokach, ale nie są dozwolone jako osadzone instrukcje.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Deklaracje zmiennej lokalnej

*Local_variable_declaration* deklaruje co najmniej jedną zmienną lokalną.

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

*Local_variable_type* *local_variable_declaration* bezpośrednio określa typ zmiennych wprowadzonych przez deklarację lub wskazuje identyfikator `var` , który powinien zostać wywnioskowany na podstawie skład. Po tym typie następuje lista *local_variable_declarator*s, z których każdy wprowadza nową zmienną. *Local_variable_declarator* składa się z *identyfikatora* , który nazywa zmienną, opcjonalnie po której następuje token`=`"" i *local_variable_initializer* , który daje początkową wartość zmiennej.

W kontekście deklaracji zmiennej lokalnej, identyfikator var działa jako kontekstowe słowo kluczowe ([słowa kluczowe](lexical-structure.md#keywords)). Gdy *local_variable_type* jest określony jako `var` , `var` a typ nie jest w zakresie, deklaracja jest ***niejawnie wpisaną deklaracją zmiennej lokalnej***, której typ jest wywnioskowany na podstawie typu skojarzonego inicjatora wyrażenia. Niejawnie wpisane deklaracje zmiennych lokalnych podlegają następującym ograniczeniom:

*  *Local_variable_declaration* nie może zawierać wielu *local_variable_declarator*s.
*  *Local_variable_declarator* musi zawierać element *local_variable_initializer*.
*  *Local_variable_initializer* musi być *wyrażeniem*.
*  *Wyrażenie* inicjatora musi mieć typ czasu kompilacji.
*  *Wyrażenie* inicjatora nie może odwoływać się do zadeklarowanej zmiennej

Poniżej przedstawiono przykłady nieprawidłowych niejawnie wpisanych deklaracji zmiennych lokalnych:

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

Wartość zmiennej lokalnej jest uzyskiwana w wyrażeniu przy użyciu *simple_name* ([nazw prostych](expressions.md#simple-names)), a wartość zmiennej lokalnej jest modyfikowana przy użyciu *przypisania* ([Operatory przypisania](expressions.md#assignment-operators)). Zmienna lokalna musi być ostatecznie przypisana ([przypisanie określone](variables.md#definite-assignment)) w każdej lokalizacji, w której jest uzyskiwana wartość.

Zakres zmiennej lokalnej zadeklarowanej w *local_variable_declaration* to blok, w którym występuje deklaracja. Jest to błąd, aby odwołać się do zmiennej lokalnej w pozycji tekstowej, która poprzedza *local_variable_declarator* zmiennej lokalnej. W zakresie zmiennej lokalnej jest to błąd czasu kompilacji, aby zadeklarować inną zmienną lokalną lub stałą o tej samej nazwie.

Deklaracja zmiennej lokalnej, która deklaruje wiele zmiennych, jest równoważna z wieloma deklaracjami pojedynczych zmiennych tego samego typu. Ponadto inicjator zmiennej w deklaracji zmiennej lokalnej odpowiada dokładnie instrukcji przypisania, która jest wstawiana bezpośrednio po deklaracji.

Przykład
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
odpowiada dokładnie na
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

W deklaracji niejawnie wpisanej zmiennej lokalnej typ zadeklarowanej zmiennej lokalnej jest taki sam jak typ wyrażenia użytego do zainicjowania zmiennej. Na przykład:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

Niejawnie wpisane deklaracje zmiennej lokalnej są dokładnie równoważne z następującymi jawnie określonymi deklaracjami:
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>Lokalne deklaracje stałych

*Local_constant_declaration* deklaruje co najmniej jedną stałą lokalną.

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

*Typ* *local_constant_declaration* określa typ stałych wprowadzonych przez deklarację. Po tym typie następuje lista *constant_declarator*s, z których każdy wprowadza nową stałą. *Constant_declarator* składa się z *identyfikatora* , który nazywa stałą, po którym następuje token`=`"", po którym następuje *constant_expression* ([wyrażenia stałe](expressions.md#constant-expressions)), które dają wartość stałej.

*Typ* i *constant_expression* deklaracji stałej lokalnej muszą być zgodne z tymi samymi regułami, co w przypadku deklaracji stałego elementu członkowskiego ([stałe](classes.md#constants)).

Wartość stałej lokalnej jest uzyskiwana w wyrażeniu przy użyciu elementu *simple_name* ([Simple Names](expressions.md#simple-names)).

Zakres stałej lokalnej to blok, w którym występuje deklaracja. Jest to błąd, aby odwołać się do lokalnej stałej w pozycji tekstowej, która poprzedza jej *constant_declarator*. W zakresie stałej lokalnej jest to błąd czasu kompilacji, aby zadeklarować inną zmienną lokalną lub stałą o tej samej nazwie.

Lokalna deklaracja stałej, która deklaruje wiele stałych jest równoznaczna z wieloma deklaracjami pojedynczych stałych tego samego typu.

## <a name="expression-statements"></a>Instrukcje wyrażeń

*Expression_statement* oblicza określone wyrażenie. Wartość obliczona przez wyrażenie (jeśli istnieje) jest odrzucana.

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

Nie wszystkie wyrażenia są dozwolone jako instrukcje. W szczególności wyrażenia takie jak `x + y` i `x == 1` , które tylko obliczają wartość (która zostanie odrzucona), nie są dozwolone jako instrukcje.

Wykonanie elementu *expression_statement* powoduje oszacowanie zawartego wyrażenia, a następnie przekazanie kontroli do punktu końcowego *expression_statement*. Punkt końcowy *expression_statement* jest osiągalny, jeśli *expression_statement* jest osiągalny.

## <a name="selection-statements"></a>Instrukcje wyboru

Instrukcje wyboru wybierz jedną z wielu możliwych instrukcji do wykonania na podstawie wartości niektórych wyrażeń.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>Instrukcja if

`if` Instrukcja wybiera instrukcję do wykonania na podstawie wartości wyrażenia logicznego.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Część jest skojarzona z leksykalną najbliższą poprzednią `if` , która jest dozwolona przez składnię. `else` W tym celu `if` , instrukcja formularza
```csharp
if (x) if (y) F(); else G();
```
odpowiada wyrażeniu
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

`if` Instrukcja jest wykonywana w następujący sposób:

*  *Boolean_expression* ([wyrażenia logiczne](expressions.md#boolean-expressions)) są oceniane.
*  Jeśli wyrażenie logiczne daje `true`, sterowanie jest przekazywane do pierwszej osadzonej instrukcji. Gdy i jeśli kontrola osiągnie punkt końcowy tej instrukcji, sterowanie jest przekazywane do punktu `if` końcowego instrukcji.
*  Jeśli wyrażenie logiczne zwraca `false` wartość i `else` jeśli część jest obecna, kontrola jest przekazywana do drugiej osadzonej instrukcji. Gdy i jeśli kontrola osiągnie punkt końcowy tej instrukcji, sterowanie jest przekazywane do punktu `if` końcowego instrukcji.
*  Jeśli wyrażenie logiczne zwraca `false` wartość i `else` jeśli część nie istnieje, kontrola jest przekazywana do punktu `if` końcowego instrukcji.

Pierwsza osadzona instrukcja `if` instrukcji jest osiągalna, `if` Jeśli instrukcja jest osiągalna, a wyrażenie logiczne nie ma stałej wartości `false`.

Druga osadzona instrukcja `if` instrukcji, jeśli jest obecna, jest osiągalna, `if` Jeśli instrukcja jest osiągalna, a wyrażenie logiczne nie ma stałej wartości `true`.

Punkt `if` końcowy instrukcji jest osiągalny, jeśli punkt końcowy co najmniej jednej z jej osadzonych instrukcji jest osiągalny. Ponadto punkt `if` końcowy instrukcji bez `else` `if` części jest osiągalny, jeśli instrukcja jest osiągalna, a wyrażenie logiczne nie ma stałej wartości `true`.

### <a name="the-switch-statement"></a>Instrukcja Switch

Instrukcja Switch wybiera do wykonania list instrukcji z skojarzoną etykietą Switch, która odpowiada wartości wyrażenia Switch.

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

*Switch_statement* składa się ze słowa `switch`kluczowego, po którym następuje wyrażenie ujęte w nawiasy (zwane wyrażeniem Switch), a następnie *switch_block*. *Switch_block* składa się z zera lub więcej *switch_section*s ujętych w nawiasy klamrowe. Każdy *switch_section* składa się z jednego lub więcej *switch_label*s, po których następuje *statement_list* ([Lista instrukcji](statements.md#statement-lists)).

Typ`switch` ***rządzący*** instrukcji jest ustalany przez wyrażenie Switch.

*  `sbyte`Jeśli typ wyrażenia Switch to, ,`uint` `byte` `ushort` ,,`string`,,,, `short` ,,`int`lub `long` `ulong` `bool` `char`  *enum_type*, lub jeśli jest typem dopuszczającym wartość null odpowiadającym jednemu z tych typów, to jest to typ, `switch` który jest typem instrukcji.
*  W przeciwnym razie dokładnie jedna zdefiniowana przez użytkownika konwersja niejawna ([konwersje zdefiniowane przez użytkownika](conversions.md#user-defined-conversions)) musi istnieć z typu wyrażenia przełącznika do jednego z następujących możliwych rodzajów typów: `sbyte`, `byte`, `short`, `ushort` , `int` `uint`, ,,`string`,, lub, typu dopuszczającego wartość null odpowiadającego jednemu z tych typów. `long` `ulong` `char`
*  W przeciwnym razie, jeśli taka niejawna konwersja nie istnieje lub jeśli istnieje więcej niż jedna taka niejawna konwersja, wystąpi błąd w czasie kompilacji.

Wyrażenie stałe każdej `case` etykiety musi oznaczać wartość, która jest niejawnie przekonwertowana ([konwersje niejawne](conversions.md#implicit-conversions)) na typ `switch` decydujący instrukcji. Błąd czasu kompilacji występuje, jeśli co `case` najmniej dwie etykiety w tej samej `switch` instrukcji określają tę samą wartość stałą.

W instrukcji switch może istnieć co `default` najwyżej jedna etykieta.

`switch` Instrukcja jest wykonywana w następujący sposób:

*  Wyrażenie Switch jest oceniane i konwertowane na typ zarządzający.
*  Jeśli jedna ze stałych określonych w `case` etykiecie w tej samej `switch` instrukcji jest równa wartości wyrażenia Switch, formant zostanie przeniesiony do listy instrukcji po dopasowanym `case` oznaczeniu.
*  Jeśli żadna ze stałych określonych `case` w etykietach w tej samej `switch` instrukcji nie jest równa wartości `default` wyrażenia Switch i jeśli etykieta jest obecna, sterowanie `default` jest przekazywane do listy instrukcji po oznakowan.
*  Jeśli żadna ze stałych określonych `case` w etykietach w tej samej `switch` instrukcji nie jest równa wartości wyrażenia Switch i jeśli żadna etykieta nie `default` jest obecna, kontrola `switch` jest przekazywana do punktu końcowego instrukcji.

Jeśli punkt końcowy listy instrukcji przełącznika jest osiągalny, wystąpi błąd w czasie kompilacji. Ta wartość jest znana jako reguła "bez powracania". Przykład
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
jest prawidłowy, ponieważ żadna sekcja Switch nie ma osiągalnego punktu końcowego. W przeciwieństwie do C++języka C i wykonywanie sekcji Switch nie jest dozwolone w sekcji "przechodzenie" do następnego przełącznika i przykład
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
powoduje błąd czasu kompilacji. Po wykonaniu sekcji Switch po wykonaniu kolejnej sekcji Switch musi być użyta jawna `goto case` instrukcja or: `goto default`
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

W *switch_section*jest dozwolonych wiele etykiet. Przykład
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
jest prawidłowy. Przykład nie narusza reguły "No-through", ponieważ etykiety `case 2:` i `default:` są częścią tego samego *switch_sectionu*.

Reguła "bez powracania" uniemożliwia wspólną klasę błędów występujących w języku C oraz przypadki C++ , `break` w których instrukcje są przypadkowo pomijane. Ponadto ze względu na tę regułę sekcje `switch` przełącznika instrukcji można arbitralnie zmienić bez wpływu na zachowanie instrukcji. Na przykład sekcje `switch` powyższej instrukcji można odwrócić bez wpływu na zachowanie instrukcji:
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

Lista instrukcji switch zwykle kończy się w `break`instrukcji, `goto case`, lub `goto default` , ale dowolna konstrukcja, która renderuje punkt końcowy listy instrukcji jest nieosiągalna. Na przykład, `while` instrukcja kontrolowana przez wyrażenie `true` logiczne jest znana, aby nie dotrzeć do punktu końcowego. Podobnie, instrukcja `throw` lub `return` zawsze przenosi formant w innym miejscu i nigdy nie dociera do punktu końcowego. W tym przypadku następujący przykład jest prawidłowy:
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

Typ `switch` decydujący instrukcji może być typem `string`. Przykład:
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

Podobnie jak w przypadku operatorów równości ciągów ([Operatory równości ciągów](expressions.md#string-equality-operators)) `switch` , instrukcja uwzględnia wielkość liter i wykona daną sekcję Switch tylko wtedy, gdy ciąg wyrażenia przełącznika dokładnie pasuje do `case` stałej etykiet.

W przypadku, gdy typem `switch` instrukcji jest `string`, wartość `null` jest dozwolona jako stała etykiet wielkości liter.

*Statement_list*s *switch_block* może zawierać instrukcje deklaracji ([instrukcje deklaracji](statements.md#declaration-statements)). Zakres zmiennej lokalnej lub stałej zadeklarowanej w bloku przełącznika to blok przełącznika.

Lista instrukcji danej sekcji Switch jest osiągalna, jeśli `switch` instrukcja jest osiągalna i co najmniej jeden z następujących warunków jest spełniony:

*  Wyrażenie Switch jest wartością niestałą.
*  Wyrażenie Switch jest stałą wartością zgodną `case` z etykietą w sekcji Switch.
*  Wyrażenie Switch jest wartością stałą, która nie jest zgodna z `case` żadną etykietą, a sekcja Switch `default` zawiera etykietę.
*  Do etykiety przełącznika w sekcji przełącznika jest przywoływana osiągalna `goto case` instrukcja `goto default` or.

Punkt `switch` końcowy instrukcji jest dostępny, jeśli co najmniej jeden z następujących warunków jest spełniony:

*  Instrukcja zawiera `break` osiągalną`switch` instrukcję, która kończy wykonywanie instrukcji. `switch`
*  Instrukcja jest osiągalna, wyrażenie Switch jest wartością niestałą i żadna etykieta nie `default` jest obecna. `switch`
*  Instrukcja jest osiągalna, wyrażenie Switch jest wartością stałą, która nie jest zgodna z `case` żadną etykietą i `default` żadna etykieta nie jest obecna. `switch`

## <a name="iteration-statements"></a>Instrukcje iteracji

Instrukcja iteracji wielokrotnie wykonuje osadzoną instrukcję.

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>Instrukcja while

`while` Instrukcja warunkowo wykonuje osadzoną instrukcję zero lub więcej razy.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

`while` Instrukcja jest wykonywana w następujący sposób:

*  *Boolean_expression* ([wyrażenia logiczne](expressions.md#boolean-expressions)) są oceniane.
*  Jeśli wyrażenie logiczne daje `true`, sterowanie jest przekazywane do osadzonej instrukcji. Gdy i Jeśli kontrolka osiągnie punkt końcowy osadzonej instrukcji (prawdopodobnie z wykonania `continue` instrukcji), sterowanie jest przekazywane na początek `while` instrukcji.
*  Jeśli wyrażenie logiczne daje `false`, sterowanie jest przekazywane do punktu `while` końcowego instrukcji.

W osadzonej instrukcji `while` instrukcji `break` instrukcja ([instrukcja break](statements.md#the-break-statement)) może być używana do transferowania `while` kontroli do punktu końcowego instrukcji (w związku z tym kończącej iterację osadzonej instrukcji) i `continue` instrukcja ([Instrukcja continue](statements.md#the-continue-statement)) może być używana do transferowania kontroli do punktu końcowego osadzonej instrukcji (w związku z tym wykonywanie innej `while` iteracji instrukcji).

Osadzona instrukcja `while` instrukcji jest osiągalna, `while` Jeśli instrukcja jest osiągalna, a wyrażenie logiczne nie ma stałej wartości `false`.

Punkt `while` końcowy instrukcji jest dostępny, jeśli co najmniej jeden z następujących warunków jest spełniony:

*  Instrukcja zawiera `break` osiągalną`while` instrukcję, która kończy wykonywanie instrukcji. `while`
*  Instrukcja jest osiągalna, a wyrażenie logiczne nie ma stałej wartości `true`. `while`

### <a name="the-do-statement"></a>Instrukcja do

`do` Instrukcja warunkowo wykonuje osadzoną instrukcję jeden lub więcej razy.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

`do` Instrukcja jest wykonywana w następujący sposób:

*  Kontrolka jest przenoszona do osadzonej instrukcji.
*  Gdy i jeśli formant osiągnie punkt końcowy osadzonej instrukcji (prawdopodobnie z wykonania `continue` instrukcji), jest oceniane *Boolean_expression* ([wyrażenia logiczne](expressions.md#boolean-expressions)). Jeśli wyrażenie logiczne daje `true`, sterowanie jest przekazywane na początek `do` instrukcji. W przeciwnym razie kontrola jest przekazywana do punktu `do` końcowego instrukcji.

W osadzonej instrukcji `do` instrukcji `break` instrukcja ([instrukcja break](statements.md#the-break-statement)) może być używana do transferowania `do` kontroli do punktu końcowego instrukcji (w związku z tym kończącej iterację osadzonej instrukcji) i `continue` instrukcja ([Instrukcja continue](statements.md#the-continue-statement)) może służyć do transferowania kontroli do punktu końcowego osadzonej instrukcji.

Osadzona instrukcja `do` instrukcji jest osiągalna, `do` Jeśli instrukcja jest osiągalna.

Punkt `do` końcowy instrukcji jest dostępny, jeśli co najmniej jeden z następujących warunków jest spełniony:

*  Instrukcja zawiera `break` osiągalną`do` instrukcję, która kończy wykonywanie instrukcji. `do`
*  Punkt końcowy osadzonej instrukcji jest osiągalny, a wyrażenie logiczne nie ma stałej wartości `true`.

### <a name="the-for-statement"></a>Instrukcja for

`for` Instrukcja oblicza sekwencję wyrażeń inicjalizacji, a następnie, gdy warunek ma wartość true, wielokrotnie wykonuje osadzoną instrukcję i szacuje sekwencję wyrażeń iteracji.

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

*For_initializer*, jeśli jest obecny, składa się z *local_variable_declaration* ([lokalnych deklaracji zmiennych](statements.md#local-variable-declarations)) lub listy *statement_expression*s ([instrukcje wyrażeń](statements.md#expression-statements)) oddzielone przecinkami. Zakres zmiennej lokalnej zadeklarowanej przez *for_initializer* zaczyna się od *local_variable_declarator* dla zmiennej i rozciąga się na koniec osadzonej instrukcji. Zakres zawiera *for_condition* i *for_iterator*.

*For_condition*, jeśli jest obecny, musi być *Boolean_expression* ([wyrażenie logiczne](expressions.md#boolean-expressions)).

*For_iterator*, jeśli jest obecny, składa się z listy *statement_expression*s ([instrukcje wyrażeń](statements.md#expression-statements)) oddzielone przecinkami.

Instrukcja for jest wykonywana w następujący sposób:

*  Jeśli *for_initializer* jest obecny, inicjatory zmiennych lub wyrażenia instrukcji są wykonywane w kolejności, w jakiej zostały wpisane. Ten krok jest wykonywany tylko raz.
*  Jeśli *for_condition* jest obecny, zostanie ona oceniona.
*  Jeśli *for_condition* nie istnieje lub w przypadku oceny `true`, kontrola jest przekazywana do osadzonej instrukcji. Gdy i Jeśli kontrolka osiągnie punkt końcowy osadzonej instrukcji (prawdopodobnie z wykonania `continue` instrukcji), wyrażenia *for_iterator*(jeśli istnieją) są oceniane w sekwencji, a następnie wykonywana jest inna iteracja, rozpoczynając od Obliczanie *for_condition* w powyższym kroku.
*  Jeśli *for_condition* jest obecny i oceny `false`, kontrola jest przekazywana do punktu `for` końcowego instrukcji.

W osadzonej instrukcji `for` instrukcji `break` instrukcja ([instrukcja break](statements.md#the-break-statement)) może być używana do transferowania `for` kontroli do punktu końcowego instrukcji (w związku z tym kończącej iterację osadzonej instrukcji) i `continue` instrukcja ([Instrukcja continue](statements.md#the-continue-statement)) może służyć do transferowania kontroli do punktu końcowego osadzonej instrukcji (w związku z tym wykonywania *for_iterator* i `for` wykonywania innej iteracji instrukcji, zaczynając od *for_condition*).

Osadzona instrukcja `for` instrukcji jest osiągalna, jeśli jedno z następujących warunków jest prawdziwe:

*  Instrukcja jest osiągalna i nie ma *for_condition.* `for`
*  Instrukcja jest osiągalna, a *for_condition* jest obecny i nie ma stałej wartości `false`. `for`

Punkt `for` końcowy instrukcji jest dostępny, jeśli co najmniej jeden z następujących warunków jest spełniony:

*  Instrukcja zawiera `break` osiągalną`for` instrukcję, która kończy wykonywanie instrukcji. `for`
*  Instrukcja jest osiągalna, a *for_condition* jest obecny i nie ma stałej wartości `true`. `for`

### <a name="the-foreach-statement"></a>Instrukcja foreach

`foreach` Instrukcja wylicza elementy kolekcji, wykonując osadzoną instrukcję dla każdego elementu kolekcji.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

*Typ* i *Identyfikator* `foreach` instrukcji deklaruje ***zmienną iteracji*** instrukcji. Jeśli identyfikator jest określony jako *local_variable_type*, a typ o nazwie `var` nie należy do zakresu, Zmienna iteracji jest określana jako ***Zmienna iteracji niejawnie wpisanej***, a jej typ jest traktowany jako typ elementu `var` `foreach` instrukcja, jak określono poniżej. Zmienna iteracji odnosi się do zmiennej lokalnej tylko do odczytu z zakresem, który wykracza poza osadzoną instrukcję. Podczas wykonywania `foreach` instrukcji Zmienna iteracji reprezentuje element kolekcji, dla którego obecnie trwa wykonywanie iteracji. Błąd czasu kompilacji występuje, `++` Jeśli osadzona instrukcja próbuje zmodyfikować zmienną iteracji (poprzez przypisanie lub operatory i `--` ) albo przekazać zmienną iteracji jako `ref` parametr lub `out` .

W następujących przypadkach dla zwięzłości, `IEnumerable` `IEnumerable<T>` , `IEnumerator`i `IEnumerator<T>` zapoznaj się z odpowiednimi typami w przestrzeniach nazw `System.Collections` i `System.Collections.Generic`.

Przetwarzanie instrukcji foreach w czasie kompilacji najpierw określa ***Typ kolekcji***, ***Typ modułu wyliczającego*** i ***Typ elementu*** wyrażenia. To obliczanie jest przeprowadzane w następujący sposób:

*  Jeśli `X` typ *wyrażenia* jest typem tablicy, istnieje niejawna `X` `IEnumerable` konwersja odwołania z do interfejsu (ponieważ `System.Array` implementuje ten interfejs). ***Typem kolekcji*** jest `IEnumerable` interfejs, ***typem modułu wyliczającego*** jest `IEnumerator` interfejs, a ***Typ*** `X`elementu jest typem elementu typu tablicy.
*  `X` Jeśli typem *wyrażenia* jest `dynamic` ,istniejeniejawnakonwersjazwyrażeniadointerfejsu(niejawnekonwersjedynamiczne).`IEnumerable` [](conversions.md#implicit-dynamic-conversions) ***Typ kolekcji*** jest `IEnumerable` interfejsem, a ***typem modułu wyliczającego*** jest `IEnumerator` interfejs. `dynamic` `object` Jeśli identyfikator jest określony jako local_variable_type, typ elementu to, w przeciwnym razie. `var`
*  W przeciwnym razie Ustal, czy `X` typ ma odpowiednią `GetEnumerator` metodę:
   * Wykonaj wyszukiwanie elementów członkowskich w typie `X` z identyfikatorem `GetEnumerator` i bez argumentów typu. Jeśli wyszukiwanie elementu członkowskiego nie produkuje dopasowania lub tworzy niejednoznaczność lub tworzy dopasowanie, które nie jest grupą metod, należy sprawdzić, czy wyliczalny interfejs został opisany poniżej. Zaleca się, aby ostrzeżenie było wydawane, gdy wyszukiwanie elementów członkowskich produkuje wszystkie elementy poza grupą metod lub nie odpowiada.
   * Wykonaj rozwiązanie przeciążenia przy użyciu grupy metod i pustej listy argumentów. Jeśli rozwiązanie przeciążenia skutkuje brakiem odpowiednich metod, wyniki są niejednoznaczne lub mają jedną najlepszą metodę, ale ta metoda jest statyczna lub nie jest publiczna, Wyszukaj wyliczalny interfejs zgodnie z poniższym opisem. Zaleca się, aby ostrzeżenie było wydawane, jeśli rozwiązanie przeciążania produkuje wszystko, z wyjątkiem jednoznacznej metody wystąpienia publicznego lub nie ma zastosowania do odpowiednich metod.
   * Jeśli zwracany typ `E` `GetEnumerator` metody nie jest klasą, strukturą lub typem interfejsu, jest generowany błąd i nie są podejmowane żadne dalsze kroki.
   * Wyszukiwanie elementu członkowskiego jest `E` wykonywane na z `Current` identyfikatorem i bez argumentów typu. Jeśli wyszukiwanie elementu członkowskiego nie powoduje dopasowania, wynikiem jest błąd lub wynikiem jest wszystko z wyjątkiem właściwości wystąpienia publicznego, która umożliwia odczytywanie, zostanie wygenerowany błąd i nie są podejmowane żadne dalsze kroki.
   * Wyszukiwanie elementu członkowskiego jest `E` wykonywane na z `MoveNext` identyfikatorem i bez argumentów typu. Jeśli wyszukiwanie elementu członkowskiego nie powoduje dopasowania, wynikiem jest błąd lub wynikiem jest wszystko poza grupą metod, zostanie wygenerowany błąd i nie są podejmowane żadne dalsze kroki.
   * Rozpoznanie przeciążenia jest wykonywane w grupie metod z pustą listą argumentów. Jeśli rozwiązanie przeciążenia skutkuje brakiem odpowiednich metod, wyniki są niejednoznaczne lub mają jedną najlepszą metodę, ale ta metoda jest statyczna lub nie jest publiczna lub jej typem zwracanym jest `bool`błąd i nie są podejmowane żadne dalsze kroki.
   * ***Typ kolekcji*** to `X`, ***Typ*** `E`modułu wyliczającego, `Current` a ***Typ elementu*** to typ właściwości.

*  W przeciwnym razie Sprawdź, czy wyliczalny interfejs:
   * Jeśli wśród wszystkich `Ti` typów, dla których istnieje niejawna konwersja z `X` do na `IEnumerable<Ti>`, istnieje unikatowy typ `T` , który `T` nie `dynamic` jest i dla wszystkich innych `Ti` niejawna `IEnumerable<T>` konwersja `IEnumerable<Ti>`z na, a następnie ***Typ kolekcji*** to `IEnumerable<T>`interfejs, ***typem modułu wyliczającego*** jest interfejs `IEnumerator<T>`, a ***typem elementu*** jest `T`.
   * W przeciwnym razie, jeśli istnieje więcej niż jeden taki `T`typ, zostanie wygenerowany błąd i nie zostaną wykonane żadne dalsze kroki.
   * W przeciwnym razie, jeśli istnieje niejawna `X` konwersja z `System.Collections.IEnumerable` do interfejsu, ***typem kolekcji*** jest ten interfejs, ***typem modułu wyliczającego*** jest interfejs `System.Collections.IEnumerator`, a ***typem elementu*** jest `object`.
   * W przeciwnym razie zostanie wygenerowany błąd i nie są podejmowane żadne dalsze kroki.

Powyższe kroki, w przypadku powodzenia, jednoznacznie tworzą typ `C`kolekcji, typ `E` modułu wyliczającego i typ `T`elementu. Instrukcja foreach formularza
```csharp
foreach (V v in x) embedded_statement
```
następnie jest rozwinięty do:
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

Zmienna `e` nie jest widoczna lub dostępna dla wyrażenia `x` lub osadzonej instrukcji ani żadnego innego kodu źródłowego programu. Zmienna `v` jest tylko do odczytu w osadzonej instrukcji. Jeśli nie istnieje jawna konwersja ([jawne konwersje](conversions.md#explicit-conversions)) z `T` (typ elementu) do `V` ( *local_variable_type* w instrukcji foreach), zostanie wygenerowany błąd i nie są podejmowane żadne dalsze kroki. Jeśli `x` ma wartość `null`, `System.NullReferenceException` jest zgłaszany w czasie wykonywania.

Implementacja może zaimplementować daną instrukcję foreach inaczej, np. ze względu na wydajność, o ile zachowanie jest spójne z powyższym rozszerzeniem.

Umieszczanie `v` wewnątrz pętli while jest ważne dla sposobu, w jaki są przechwytywane przez jakąkolwiek anonimową funkcję występującą w *embedded_statement*.

Przykład:
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
Jeśli `v` został zadeklarowany poza pętlą while, będzie współużytkowana przez wszystkie iteracje, a jej wartość po pętli for będzie wartością końcową, `13`czyli to, `f` co to jest wywołanie. Zamiast tego, ponieważ każda iteracja ma własną `v`zmienną, przechwycona `f` przez w pierwszej iteracji będzie nadal przechowywać wartość `7`, co oznacza, że zostanie wydrukowany. (Uwaga: wcześniejsze wersje C# zadeklarowane `v` poza pętlą while).

Treść bloku finally jest tworzona zgodnie z poniższymi krokami:

*  Jeśli istnieje niejawna konwersja z `E` `System.IDisposable` do interfejsu,
   *  Jeśli `E` jest typem wartości niedopuszczających wartości null, klauzula finally jest rozwinięta do semantycznego odpowiednika:

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  W przeciwnym razie klauzula finally jest rozwinięta do semantycznego odpowiednika:

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   z tą różnicą, że jeśli `E` jest typem wartości, lub parametrem typu skonkretyzowanym do typu wartości, `e` rzutowanie na `System.IDisposable` nie spowoduje wystąpienia opakowania.

*  W przeciwnym razie `E` , jeśli jest typem zapieczętowanym, klauzula finally jest rozwinięta do pustego bloku:

   ```csharp
   finally {
   }
   ```

*  W przeciwnym razie klauzula finally jest rozwinięta do:

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   Zmienna `d` lokalna nie jest widoczna dla żadnego kodu użytkownika ani nie jest dostępna dla żadnego z nich. W szczególności nie powoduje konfliktu z żadną inną zmienną, której zakres zawiera blok finally.

Kolejność, w której `foreach` przechodzą elementy tablicy, jest następująca: W przypadku elementów tablic jednowymiarowych odbywa się rosnącą kolejnością indeksu, rozpoczynając od `0` indeksu i kończąc na `Length - 1`indeksie. W przypadku tablic wielowymiarowych elementy są przenoszone w taki sposób, że indeksy w wymiarze z prawej strony są najpierw zwiększane, następnie następny lewy wymiar itd.

Poniższy przykład drukuje każdą wartość w dwuwymiarowej tablicy, w kolejności elementów:
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
Utworzone dane wyjściowe są następujące:
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

W przykładzie
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
Typ `n` `int`elementu jest`numbers`wywnioskowany na wartość

## <a name="jump-statements"></a>Instrukcje skoku

Instrukcje skoku bezwarunkowo przetransferować kontrolę.

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

Lokalizacja, do której instrukcja skoku przeniesie formant jest nazywana ***elementem docelowym*** instrukcji skoku.

Gdy instrukcja skoku występuje w bloku, a obiekt docelowy tej instrukcji skoku znajduje się poza tym blokiem, instrukcja skoku jest określana w celu ***opuszczenia*** bloku. Chociaż instrukcja skoku może przetransferować kontrolę z bloku, nigdy nie można przesłać kontroli do bloku.

Wykonywanie instrukcji skoku jest skomplikowane przez obecność `try` instrukcji wykonywanych przez interwencję. W przypadku braku takich `try` instrukcji instrukcja skoku bezwarunkowo przesyła kontrolę z instrukcji skoku do jej obiektu docelowego. W obecności takich `try` instrukcji, wykonywanie jest bardziej skomplikowane. Jeśli instrukcja skoku `try` kończy jeden lub więcej bloków ze skojarzonymi `finally` blokami, kontrola `finally` jest początkowo przekazywana do bloku wewnętrznej `try` instrukcji. Gdy i jeśli kontrola osiągnie punkt `finally` końcowy bloku, sterowanie jest przekazywane `finally` do bloku następnej otaczającej `try` instrukcji. Ten proces jest powtarzany do `finally` momentu wykonania bloków wszystkich `try` instrukcji interwencji.

W przykładzie
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
bloki skojarzone z dwiema `try` instrukcjami są wykonywane przed przekazaniem kontroli do elementu docelowego instrukcji skoku. `finally`

Utworzone dane wyjściowe są następujące:
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Instrukcja break

`while` `switch` `do`Instrukcja kończy najbliższych otaczających, ,`for`,, lub`foreach`instrukcji. `break`

```antlr
break_statement
    : 'break' ';'
    ;
```

Obiekt docelowy `break` instrukcji jest punktem końcowym najbliższej `switch`otaczającej, `while`, `do` `for`,, lub `foreach` instrukcji. `for` `while` `switch` `foreach` Jeśli instrukcja nie jest ujęta w instrukcji,, `do`, lub, wystąpi błąd w czasie kompilacji. `break`

Gdy wiele `switch`instrukcji `while`, `do`, ,`for` lub`foreach` są zagnieżdżone w obrębie siebie, `break` instrukcja ma zastosowanie tylko do najbardziej wewnętrznej instrukcji. Aby przenieść kontrolę na wiele poziomów zagnieżdżenia, `goto` należy użyć instrukcji ([instrukcji goto](statements.md#the-goto-statement)).

Instrukcja nie może `finally` opuścić bloku ([Instrukcja try](statements.md#the-try-statement)). `break` Gdy instrukcja występuje `finally` w bloku `break` , obiekt docelowy instrukcji musi znajdować się w tym samym `finally` bloku; w przeciwnym razie wystąpi błąd w czasie kompilacji. `break`

`break` Instrukcja jest wykonywana w następujący sposób:

*  `finally` `try` `finally` Jeśli instrukcja kończy jeden lub więcej `try` bloków ze skojarzonymi blokami, kontrola jest początkowo przekazywana do bloku wewnętrznej instrukcji. `break` Gdy i jeśli kontrola osiągnie punkt `finally` końcowy bloku, sterowanie jest przekazywane `finally` do bloku następnej otaczającej `try` instrukcji. Ten proces jest powtarzany do `finally` momentu wykonania bloków wszystkich `try` instrukcji interwencji.
*  Kontrolka jest przenoszona do elementu docelowego `break` instrukcji.

Ponieważ instrukcja bezwarunkowo transferuje kontrolę w innym miejscu, punkt `break` końcowy instrukcji nie jest nigdy osiągalny. `break`

### <a name="the-continue-statement"></a>Instrukcja continue

`do` `while` `foreach` `for`Instrukcja uruchamia nową iterację najbliższej otaczającej,,, lub instrukcji. `continue`

```antlr
continue_statement
    : 'continue' ';'
    ;
```

Obiekt `continue` docelowy instrukcji jest punktem końcowym osadzonej instrukcji najbliższej `do` `while`otaczającej,, `for`lub `foreach` instrukcji. `foreach` `for` `do` `while`Jeśli instrukcja nie jest ujęta w instrukcji,,, lub, wystąpi błąd w czasie kompilacji. `continue`

Gdy wiele `while`instrukcji `do`, `for`,, `foreach` lub są zagnieżdżone w obrębie siebie, `continue` instrukcja ma zastosowanie tylko do najbardziej wewnętrznej instrukcji. Aby przenieść kontrolę na wiele poziomów zagnieżdżenia, `goto` należy użyć instrukcji ([instrukcji goto](statements.md#the-goto-statement)).

Instrukcja nie może `finally` opuścić bloku ([Instrukcja try](statements.md#the-try-statement)). `continue` Gdy instrukcja występuje `finally` w bloku `continue` , obiekt docelowy instrukcji musi znajdować się w tym samym `finally` bloku; w przeciwnym razie wystąpi błąd w czasie kompilacji. `continue`

`continue` Instrukcja jest wykonywana w następujący sposób:

*  `finally` `try` `finally` Jeśli instrukcja kończy jeden lub więcej `try` bloków ze skojarzonymi blokami, kontrola jest początkowo przekazywana do bloku wewnętrznej instrukcji. `continue` Gdy i jeśli kontrola osiągnie punkt `finally` końcowy bloku, sterowanie jest przekazywane `finally` do bloku następnej otaczającej `try` instrukcji. Ten proces jest powtarzany do `finally` momentu wykonania bloków wszystkich `try` instrukcji interwencji.
*  Kontrolka jest przenoszona do elementu docelowego `continue` instrukcji.

Ponieważ instrukcja bezwarunkowo transferuje kontrolę w innym miejscu, punkt `continue` końcowy instrukcji nie jest nigdy osiągalny. `continue`

### <a name="the-goto-statement"></a>Instrukcja goto

`goto` Instrukcja przekazuje formant do instrukcji, która jest oznaczona etykietą.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

Obiekt docelowy `goto` instrukcji *identyfikatora* jest instrukcją z etykietą z daną etykietą. Jeśli etykieta o danej nazwie nie istnieje w elemencie członkowskim bieżącej funkcji lub jeśli `goto` instrukcja nie znajduje się w zakresie etykiety, wystąpi błąd w czasie kompilacji. Ta reguła umożliwia użycie `goto` instrukcji w celu przeniesienia kontroli poza zagnieżdżony zakres, ale nie do zakresu zagnieżdżonego. W przykładzie
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
`goto` instrukcja jest używana do transferowania kontroli poza zagnieżdżony zakres.

Obiektem docelowym `goto case` instrukcji jest lista instrukcji w bezpośrednio `switch` otaczającej instrukcji ( `case` [instrukcji switch](statements.md#the-switch-statement)), która zawiera etykietę o danej wartości stałej. `switch` `switch` [](conversions.md#implicit-conversions)Jeśli instrukcja nie jest ujęta w instrukcję, jeśli constant_expression nie jest zamiennie niejawnie (konwersje niejawne) do typu, najbliższej instrukcji otaczającej lub jeśli `goto case` Najbliższa otaczająca `switch` instrukcja nie `case` zawiera etykiety zawierającej daną wartość stałą, wystąpi błąd w czasie kompilacji.

Obiektem docelowym `goto default` instrukcji jest lista instrukcji w bezpośrednio `switch` otaczającej instrukcji ( `default` [instrukcji switch](statements.md#the-switch-statement)), która zawiera etykietę. Jeśli instrukcja nie jest ujęta `switch` w instrukcję lub `switch` Jeśli Najbliższa otaczająca instrukcja nie zawiera `default` etykiety, wystąpi błąd w czasie kompilacji. `goto default`

Instrukcja nie może `finally` opuścić bloku ([Instrukcja try](statements.md#the-try-statement)). `goto` Gdy instrukcja występuje `finally` w bloku `goto` , obiekt docelowy instrukcji musi znajdować się w tym samym `finally` bloku lub w przeciwnym razie wystąpi błąd w czasie kompilacji. `goto`

`goto` Instrukcja jest wykonywana w następujący sposób:

*  `finally` `try` `finally` Jeśli instrukcja kończy jeden lub więcej `try` bloków ze skojarzonymi blokami, kontrola jest początkowo przekazywana do bloku wewnętrznej instrukcji. `goto` Gdy i jeśli kontrola osiągnie punkt `finally` końcowy bloku, sterowanie jest przekazywane `finally` do bloku następnej otaczającej `try` instrukcji. Ten proces jest powtarzany do `finally` momentu wykonania bloków wszystkich `try` instrukcji interwencji.
*  Kontrolka jest przenoszona do elementu docelowego `goto` instrukcji.

Ponieważ instrukcja bezwarunkowo transferuje kontrolę w innym miejscu, punkt `goto` końcowy instrukcji nie jest nigdy osiągalny. `goto`

### <a name="the-return-statement"></a>Instrukcja return

Instrukcja zwraca kontrolę do bieżącego obiektu wywołującego funkcji, w `return` której występuje instrukcja. `return`

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

`add` `void` `set` [](classes.md#method-body)Instrukcja bez wyrażenia może być używana tylko w składowej funkcji, która nie oblicza wartości, czyli metody z typem wyniku (treść metody), akcesorem właściwości lub indeksatora, `return` metody `remove` dostępu do zdarzenia, Konstruktor wystąpienia, Konstruktor statyczny lub destruktor.

Instrukcji z wyrażeniem można użyć tylko w składowej funkcji, która oblicza wartość, czyli metodę z typem wyniku innym niż void `get` , akcesorem właściwości lub indeksatorem lub operatorem zdefiniowanym przez użytkownika. `return` Niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) musi istnieć z typu wyrażenia do zwracanego typu elementu członkowskiego funkcji zawierającego.

Instrukcji return można także używać w treści wyrażeń funkcji anonimowych ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)) i uczestniczyć w ustaleniu, które konwersje istnieją dla tych funkcji.

Jest to błąd czasu kompilacji dla `return` instrukcji, która ma być wyświetlana `finally` w bloku ([Instrukcja try](statements.md#the-try-statement)).

`return` Instrukcja jest wykonywana w następujący sposób:

*  `return` Jeśli instrukcja określa wyrażenie, wyrażenie jest oceniane i wynikowa wartość jest konwertowana na typ zwracany funkcji zawierającej przez niejawną konwersję. Wynik konwersji jest wartością wynikową wygenerowaną przez funkcję.
*  `finally` `try` `try` `finally` `catch` Jeśli instrukcja jest ujęta w jeden lub więcej lub bloków ze skojarzonymi blokami, kontrola jest początkowo przekazywana do bloku wewnętrznej instrukcji. `return` Gdy i jeśli kontrola osiągnie punkt `finally` końcowy bloku, sterowanie jest przekazywane `finally` do bloku następnej otaczającej `try` instrukcji. Ten proces jest powtarzany do `finally` momentu wykonania bloków wszystkich `try` instrukcji otaczających.
*  Jeśli funkcja zawierająca nie jest funkcją asynchroniczną, formant jest zwracany do obiektu wywołującego funkcji zawierającej wraz z wartością wyniku (jeśli istnieje).
*  Jeśli funkcja zawierająca jest funkcją asynchroniczną, formant jest zwracany do bieżącego obiektu wywołującego, a wartość wynikowa (jeśli istnieje) jest rejestrowana w zadaniu zwrotnym, zgodnie z opisem w temacie ([moduły wyliczające](classes.md#enumerator-interfaces)).

Ponieważ instrukcja bezwarunkowo transferuje kontrolę w innym miejscu, punkt `return` końcowy instrukcji nie jest nigdy osiągalny. `return`

### <a name="the-throw-statement"></a>Instrukcja throw

`throw` Instrukcja zgłasza wyjątek.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

`throw` Instrukcja z wyrażeniem zwraca wartość wygenerowaną przez obliczenie wyrażenia. Wyrażenie musi wskazywać wartość typu `System.Exception`klasy w typie klasy, który pochodzi od `System.Exception` lub typu parametru typu, który ma `System.Exception` (lub podklasę) jako obowiązującą klasę bazową. Jeśli zostanie wygenerowane obliczenie wyrażenia `null`, w `System.NullReferenceException` zamian zostanie zgłoszony.

Instrukcja bez wyrażenia może być używana tylko `catch` w bloku, w takim przypadku instrukcja ponownie zgłasza wyjątek, który jest obecnie obsługiwany przez ten `catch` blok. `throw`

Ponieważ instrukcja bezwarunkowo transferuje kontrolę w innym miejscu, punkt `throw` końcowy instrukcji nie jest nigdy osiągalny. `throw`

Gdy wyjątek jest zgłaszany, kontrola jest przekazywana do pierwszej `catch` klauzuli w `try` otaczającej instrukcji, która może obsłużyć wyjątek. Proces, który ma miejsce od momentu, gdy wyjątek zgłaszany do punktu transferu kontroli do odpowiedniej procedury obsługi wyjątków jest znany jako ***Propagacja wyjątku***. Propagacja wyjątku polega na wielokrotnym ocenie następujących kroków do momentu `catch` znalezienia klauzuli zgodnej z wyjątkiem. W tym opisie ***punkt throw*** jest początkowo lokalizacją, w której został zgłoszony wyjątek.

*  W bieżącym elemencie członkowskim funkcji są badane `try` wszystkie instrukcje, które obejmują punkt throw. Dla każdej instrukcji `S`, zaczynając od najbardziej wewnętrznej `try` instrukcji i kończąc na zewnętrznej `try` instrukcji, oceniane są następujące kroki:

   * `try` Jeśli `catch` `catch` blok otaczający punkt throw i jeśli S ma jedną klauzulę, klauzule są badane w kolejności występowania, aby znaleźć odpowiednią procedurę obsługi dla wyjątku, zgodnie z regułami określonymi w `S` Sekcja [instrukcji try](statements.md#the-try-statement). Jeśli znajduje się `catch` klauzula dopasowywania, Propagacja wyjątku jest wykonywana przez przeniesienie kontroli do bloku tej `catch` klauzuli.

   * W przeciwnym razie, `try` Jeśli blok `catch` lub blok `S` otaczający punkt rzutowania i jeśli `S` ma `finally` blok, sterowanie jest przekazywane do `finally` bloku. `finally` Jeśli blok zgłasza inny wyjątek, przetwarzanie bieżącego wyjątku zostanie zakończone. W przeciwnym razie, gdy formant osiągnie punkt `finally` końcowy bloku, przetwarzanie bieżącego wyjątku jest kontynuowane.

*  Jeśli program obsługi wyjątków nie znajduje się w bieżącym wywołaniu funkcji, wywołanie funkcji jest przerywane i jedna z następujących sytuacji:

   * Jeśli bieżąca funkcja nie jest asynchroniczna, powyższe kroki są powtórzone dla obiektu wywołującego funkcji z punktem rzutowania odpowiadającym instrukcji, z której wywołano element członkowski funkcji.

   * Jeśli bieżąca funkcja jest asynchroniczna i zwracająca zadania, wyjątek jest rejestrowany w zadaniu zwrotnym, które jest umieszczane w stanie awarii lub anulowania zgodnie z opisem w temacie [interfejsy modułu wyliczającego](classes.md#enumerator-interfaces).

   * Jeśli bieżąca funkcja ma wartość Async i zwraca wartość void, kontekst synchronizacji bieżącego wątku zostanie powiadomiony zgodnie z opisem w [wyliczalnych interfejsach](classes.md#enumerable-interfaces).

*  Jeśli przetwarzanie wyjątku kończy wszystkie wywołania elementu członkowskiego funkcji w bieżącym wątku, wskazując, że wątek nie ma obsługi dla wyjątku, wątek jest zakończony. Wpływ tego zakończenia jest zdefiniowany przez implementację.

## <a name="the-try-statement"></a>Instrukcja try

`try` Instrukcja zawiera mechanizm przechwytywania wyjątków, które występują podczas wykonywania bloku. Ponadto instrukcja umożliwia określenie bloku kodu, który jest zawsze wykonywany, gdy kontrolka `try` opuszcza instrukcję. `try`

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

Istnieją trzy możliwe formy `try` instrukcji:

*  Blok, po którym następuje co najmniej `catch` jeden blok. `try`
*  Blok, po którym następuje `finally` blok. `try`
*  Blok, po którym następuje jeden lub `catch` więcej bloków, po `finally` którym następuje blok. `try`

`System.Exception` `System.Exception` `System.Exception`Gdy klauzula określa exception_specifier, typ musi być typem, który pochodzi z lub typu parametru typu, który ma (lub podklasę tej klasy) jako obowiązującą klasę bazową. `catch`

Gdy klauzula określa zarówno *exception_specifier* z *identyfikatorem*, ***zmienna wyjątku*** o podanej nazwie i typie jest zadeklarowana. `catch` Zmienna wyjątku odpowiada zmiennej lokalnej z zakresem, który wykracza `catch` poza klauzulę. Podczas wykonywania *exception_filter* i *bloku*zmienna wyjątku reprezentuje aktualnie obsługiwany wyjątek. W celach o określonym zapisywaniu zmienna wyjątku jest uznawana za ostatecznie przypisaną w całym zakresie.

O ile `catch` klauzula nie zawiera nazwy zmiennej wyjątku, nie można uzyskać dostępu do obiektu Exception w filtrze i bloku. `catch`

Klauzula, która nie określa elementu *exception_specifier* , jest nazywana klauzulą generalną `catch`. `catch`

Niektóre języki programowania mogą obsługiwać wyjątki, które nie są reprezentowane jako obiekt pochodny `System.Exception`, chociaż takie wyjątki nigdy nie mogą być generowane C# przez kod. Klauzula General `catch` może służyć do przechwytywania takich wyjątków. W rezultacie Klauzula ogólna `catch` jest semantycznie różna od jednej, która określa typ `System.Exception`, w tym, że dawny może również przechwytywać wyjątki z innych języków.

Aby zlokalizować procedurę obsługi dla wyjątku, `catch` klauzule są badane w kolejności leksykalnej. Jeśli klauzula określa typ, ale nie filtr wyjątku, jest to błąd czasu kompilacji dla późniejszej `catch` klauzuli w tej samej `try` instrukcji, aby określić typ, który jest taki sam jak lub pochodzi od typu. `catch` Jeśli klauzula nie określa typu ani filtru, musi być ostatnią `catch` klauzulą tej `try` instrukcji. `catch`

`catch` W bloku instrukcja`throw` ([Instrukcja throw](statements.md#the-throw-statement)) bez wyrażenia może służyć do ponownego zgłoszenia wyjątku, który został przechwycony przez blok. `catch` Przypisania do zmiennej wyjątku nie powodują zmiany zgłoszonego wyjątku.

W przykładzie
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
Metoda `F` przechwytuje wyjątek, zapisuje niektóre informacje diagnostyczne do konsoli, modyfikuje zmienną wyjątku i ponowne zgłasza wyjątek. Wyjątek, który jest ponownie zgłaszany, to oryginalny wyjątek, dlatego utworzony wynik to:
```
Exception in F: G
Exception in Main: G
```

Jeśli pierwszy blok catch został zgłoszony `e` zamiast ponownego zgłoszenia bieżącego wyjątku, utworzone dane wyjściowe byłyby następujące:
```
Exception in F: G
Exception in Main: F
```

Jest to błąd `break`czasu kompilacji dla instrukcji, `continue`lub `goto` , aby przenieść kontrolę z `finally` bloku. `continue` `goto` Gdy instrukcja `finally` ,, lub występuje w bloku,obiektdocelowyinstrukcjimusiznajdowaćsięwtymsamymblokulubwprzeciwnymraziewystąpibłądwczasiekompilacji.`finally` `break`

Jest to błąd czasu kompilacji dla `return` instrukcji, która ma być wykonywana `finally` w bloku.

`try` Instrukcja jest wykonywana w następujący sposób:

*  Kontrolka jest przenoszona `try` do bloku.
*  Gdy i jeśli kontrola osiągnie punkt `try` końcowy bloku:
   *  Jeśli instrukcja zawiera blok, zostaje wykonany `finally` blok. `finally` `try`
   *  Kontrolka jest przenoszona do punktu `try` końcowego instrukcji.

*  Jeśli wyjątek jest propagowany do `try` instrukcji podczas wykonywania `try` bloku:
   *  `catch` Klauzule, jeśli istnieją, są badane w kolejności występowania, aby znaleźć odpowiednią procedurę obsługi dla wyjątku. `catch` Jeśli klauzula nie określa typu lub określa typ wyjątku lub typ podstawowy typu wyjątku:
      *  `catch` Jeśli klauzula deklaruje zmienną wyjątku, obiekt wyjątku jest przypisywany do zmiennej wyjątku.
      *  `catch` Jeśli klauzula deklaruje filtr wyjątku, filtr jest obliczany. Jeśli wartość jest równa `false`, klauzula catch nie jest zgodna i wyszukiwanie kontynuuje się przez wszelkie kolejne `catch` klauzule dla odpowiedniej procedury obsługi.
      *  W przeciwnym `catch` razie klauzulajestuważanazadopasowanie,akontrolajestprzekazywanado`catch` zgodnego bloku.
      *  Gdy i jeśli kontrola osiągnie punkt `catch` końcowy bloku:
         * Jeśli instrukcja zawiera blok, zostaje wykonany `finally` blok. `finally` `try`
         * Kontrolka jest przenoszona do punktu `try` końcowego instrukcji.
      *  Jeśli wyjątek jest propagowany do `try` instrukcji podczas wykonywania `catch` bloku:
         *  Jeśli instrukcja zawiera blok, zostaje wykonany `finally` blok. `finally` `try`
         *  Wyjątek jest propagowany do następnej otaczającej `try` instrukcji.
   *  Jeśli instrukcja nie `catch` ma klauzul lub jeśli żadna klauzula `catch` nie pasuje do wyjątku: `try`
      *  Jeśli instrukcja zawiera blok, zostaje wykonany `finally` blok. `finally` `try`
      *  Wyjątek jest propagowany do następnej otaczającej `try` instrukcji.

Instrukcje `finally` bloku są zawsze wykonywane, gdy kontrolka `try` opuszcza instrukcję. Jest to prawdą, czy transfer kontroli odbywa się w wyniku normalnego wykonywania, w `break`wyniku wykonywania `continue`instrukcji,, `goto`, lub `return` `try` w wyniku propagowania wyjątku z Merge.

Jeśli wyjątek jest zgłaszany podczas wykonywania `finally` bloku i nie jest przechwytywany w obrębie tego samego bloku finally, wyjątek jest propagowany do następnej `try` otaczającej instrukcji. Jeśli inny wyjątek był w trakcie propagowania, ten wyjątek zostanie utracony. Proces propagowania wyjątku jest dokładniej opisany w opisie `throw` instrukcji ([Instrukcja throw](statements.md#the-throw-statement)).

Blok instrukcji jest osiągalny, `try` Jeśli instrukcja jest osiągalna. `try` `try`

Blok instrukcji jest osiągalny, `try` Jeśli instrukcja jest osiągalna. `try` `catch`

Blok instrukcji jest osiągalny, `try` Jeśli instrukcja jest osiągalna. `try` `finally`

Punkt `try` końcowy instrukcji jest osiągalny, jeśli są spełnione oba poniższe warunki:

*  Punkt `try` końcowy bloku jest osiągalny lub punkt końcowy co najmniej jednego `catch` bloku jest osiągalny.
*  Jeśli znajduje się `finally` blok, punkt końcowy bloku jest osiągalny. `finally`

## <a name="the-checked-and-unchecked-statements"></a>Sprawdzone i niesprawdzone instrukcje

Instrukcje `checked` and`unchecked` są używane do kontrolowania ***kontekstu sprawdzania przepełnienia*** dla operacji arytmetycznych typu całkowitego i konwersji.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

Instrukcja powoduje, że wszystkie wyrażenia w *bloku* są oceniane w kontekście sprawdzonym, a instrukcja powoduje `unchecked` , że wszystkie wyrażenia w bloku są oceniane w niesprawdzonym kontekście. `checked`

`unchecked` Instrukcje `checked` and`unchecked` [są precyzyjnie](expressions.md#the-checked-and-unchecked-operators)równoważne operatorom and (operatory sprawdzone i niezaznaczone), z tą różnicą, że działają na blokach zamiast wyrażeń. `checked`

## <a name="the-lock-statement"></a>Instrukcja lock

`lock` Instrukcja uzyskuje blokadę wykluczania wzajemnego dla danego obiektu, wykonuje instrukcję, a następnie zwalnia blokadę.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

Wyrażenie `lock` instrukcji musi wskazywać wartość typu znanego jako *reference_type*. Żadna niejawna konwersja opakowania ([konwersje](conversions.md#boxing-conversions)napakowywania) nie jest kiedykolwiek wykonywana dla `lock` wyrażenia instrukcji, w rezultacie jest to błąd czasu kompilacji dla wyrażenia, aby zauważyć wartość *value_type*.

`lock` Instrukcja formularza
```csharp
lock (x) ...
```
gdzie `x` jest wyrażeniem *reference_type*, jest dokładnie równoważne
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
z tą różnicą, że `x` jest obliczany tylko raz.

Gdy blokada wzajemnego wykluczania jest utrzymywana, kod wykonywany w tym samym wątku wykonywania może również uzyskać i zwolnić blokadę. Jednak kod wykonywany w innych wątkach ma zablokowany dostęp do blokady do momentu zwolnienia blokady.

Blokowanie `System.Type` obiektów w celu synchronizacji dostępu do danych statycznych nie jest zalecane. Inny kod może być zablokowany dla tego samego typu, co może spowodować zakleszczenie. Lepszym rozwiązaniem jest synchronizowanie dostępu do danych statycznych przez zablokowanie prywatnego obiektu statycznego. Na przykład:
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a>Instrukcja using

`using` Instrukcja uzyskuje jeden lub więcej zasobów, wykonuje instrukcję, a następnie usuwa zasób.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

***Zasób*** jest klasą lub strukturą, która `System.IDisposable`implementuje, która zawiera pojedynczą metodę bez parametrów `Dispose`o nazwie. Kod używający zasobu może wywoływać `Dispose` , aby wskazać, że zasób nie jest już wymagany. Jeśli `Dispose` nie jest wywoływana, automatyczne usuwanie następuje ostatecznie w wyniku wyrzucania elementów bezużytecznych.

Jeśli forma *resource_acquisition* jest *local_variable_declaration* , typem *local_variable_declaration* musi być albo `dynamic` lub typu, który może być niejawnie konwertowany na. `System.IDisposable` Jeśli forma *resource_acquisition* jest *wyrażeniem* , to wyrażenie musi być niejawnie konwertowane na `System.IDisposable`.

Zmienne lokalne zadeklarowane w *resource_acquisition* są tylko do odczytu i muszą zawierać inicjator. Błąd czasu kompilacji występuje, `++` Jeśli osadzona instrukcja próbuje zmodyfikować te zmienne lokalne (poprzez przypisanie lub operatory i `--` ), pobrać ich adresy lub przekazać je jako `ref` lub `out` parametry.

`using` Instrukcja jest tłumaczona na trzy części: pozyskiwanie, użycie i usuwanie. Użycie zasobu jest niejawnie ujęte w `try` instrukcji, która `finally` zawiera klauzulę. Ta `finally` klauzula usuwa zasób. Jeśli zasób zostanie uzyskany, żadne `Dispose` wywołanie nie zostanie wykonane i żaden wyjątek nie jest zgłaszany. `null` Jeśli zasób jest typu `dynamic` , jest dynamicznie konwertowany przy użyciu niejawnej konwersji dynamicznej ([niejawne konwersje dynamiczne](conversions.md#implicit-dynamic-conversions)) do `IDisposable` podczas pozyskiwania w celu zapewnienia, że konwersja zakończy się pomyślnie przed użyciem i myśl.

`using` Instrukcja formularza
```csharp
using (ResourceType resource = expression) statement
```
odpowiada jednemu z trzech możliwych rozszerzeń. Gdy `ResourceType` jest typem wartości niedopuszczających wartości null, rozszerzenie jest
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

W przeciwnym razie `ResourceType` , gdy jest typem wartości null lub typem odwołania innym niż `dynamic`, rozszerzenie jest
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

`ResourceType` W`dynamic`przeciwnym razie rozszerzenie jest
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

W obu rozszerzeniach `resource` zmienna jest tylko do odczytu w osadzonej instrukcji, `d` a zmienna jest niedostępna w, i niewidoczna dla, osadzonej instrukcji.

Implementacja może implementować daną instrukcję using inaczej, np. ze względu na wydajność, o ile zachowanie jest spójne z powyższym rozszerzeniem.

`using` Instrukcja formularza
```csharp
using (expression) statement
```
ma te same trzy możliwe rozszerzenia. W tym przypadku `ResourceType` jest niejawnie typem `expression`czasu kompilacji, jeśli ma. W przeciwnym razie `IDisposable` sam interfejs jest używany `ResourceType`jako. `resource` Zmienna jest niedostępna w, i niewidoczna dla osadzonej instrukcji.

Gdy *resource_acquisition* przyjmuje postać *local_variable_declaration*, możliwe jest uzyskanie wielu zasobów danego typu. `using` Instrukcja formularza
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
jest precyzyjnym odpowiednikiem sekwencji zagnieżdżonych `using` instrukcji:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

Poniższy przykład tworzy plik o nazwie `log.txt` i zapisuje dwa wiersze tekstu do pliku. W tym przykładzie zostanie otwarty ten sam plik do odczytu i kopiowania zawartych wierszy tekstu do konsoli programu.
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

Ponieważ klasy `TextReader` `IDisposable` i implementują interfejs, przykład może używać `using` instrukcji, aby upewnić się, że plik źródłowy jest prawidłowo zamknięty po operacji zapisu lub odczytu. `TextWriter`

## <a name="the-yield-statement"></a>Instrukcja Yield

Instrukcja jest używana w bloku iteratora ([bloki](statements.md#blocks)) do przesyłania wartości do obiektu modułu wyliczającego ([obiektów modułu wyliczającego](classes.md#enumerator-objects)) lub wyliczalnego obiektu ([obiektów wyliczalnych](classes.md#enumerable-objects)) iteratora lub do sygnalizowania końcem iteracji. `yield`

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield`nie jest słowem zastrzeżonym; ma specjalne znaczenie tylko wtedy, gdy jest używana bezpośrednio `return` przed `break` słowem kluczowym or. W innych kontekstach `yield` można użyć jako identyfikatora.

Istnieje kilka ograniczeń w miejscu, w `yield` którym instrukcja może zostać wyświetlona, zgodnie z opisem w poniższej tabeli.

*  Jest to `yield` błąd czasu kompilacji dla instrukcji (jednej z formularzy), która ma być wyświetlana poza *method_body*, *operator_body* lub *accessor_body*
*  Jest to błąd czasu kompilacji dla `yield` instrukcji (każdej z formularzy), która ma być wyświetlana wewnątrz funkcji anonimowej.
*  Jest to błąd czasu kompilacji dla `yield` instrukcji (każdej z formularzy) do wyświetlenia `finally` w klauzuli `try` instrukcji.
*  Jest to błąd czasu kompilacji dla `yield return` instrukcji pojawia się gdziekolwiek `try` w instrukcji, która zawiera `catch` klauzule.

W poniższym przykładzie przedstawiono nieprawidłowe i nieprawidłowe zastosowania `yield` instrukcji.

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

Niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) musi istnieć z typu wyrażenia w `yield return` instrukcji do typu yield ([Typ Yield](classes.md#yield-type)) iteratora.

`yield return` Instrukcja jest wykonywana w następujący sposób:

*  Wyrażenie określone w instrukcji jest oceniane, niejawnie konwertowane na typ Yield i przypisywane do `Current` właściwości obiektu modułu wyliczającego.
*  Wykonywanie bloku iteratora zostało wstrzymane. Jeśli instrukcja znajduje się w jednym lub większej `try` liczbie bloków, `finally` skojarzone bloki nie są wykonywane w tym momencie. `yield return`
*  Metoda obiektu Enumerator powraca `true` do jego obiektu wywołującego, wskazując, że obiekt Enumerator został pomyślnie zaawansowany do następnego elementu. `MoveNext`

Następne wywołanie `MoveNext` metody obiektu modułu wyliczającego wznawia wykonywanie bloku iteratora od momentu ostatniego wstrzymania.

`yield break` Instrukcja jest wykonywana w następujący sposób:

*  `try` `finally` `finally` `try` Jeśli instrukcja jest ujęta w jeden lub więcej bloków ze skojarzonymi blokami, kontrola jest początkowo przekazywana do bloku wewnętrznej instrukcji. `yield break` Gdy i jeśli kontrola osiągnie punkt `finally` końcowy bloku, sterowanie jest przekazywane `finally` do bloku następnej otaczającej `try` instrukcji. Ten proces jest powtarzany do `finally` momentu wykonania bloków wszystkich `try` instrukcji otaczających.
*  Formant jest zwracany do obiektu wywołującego bloku iteratora. Jest `MoveNext` to metoda lub `Dispose` metoda obiektu modułu wyliczającego.

Ponieważ instrukcja bezwarunkowo transferuje kontrolę w innym miejscu, punkt `yield break` końcowy instrukcji nie jest nigdy osiągalny. `yield break`
