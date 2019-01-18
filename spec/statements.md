---
ms.openlocfilehash: 8f9551b9e7f70379836c23a60f0d37dc02f8e18e
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "47230004"
---
# <a name="statements"></a>Instrukcje

C# zawiera szereg instrukcji. Większość z tych instrukcji, nie będą niczym nowym dla deweloperów, którzy mają programowane w językach C i C++.

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

*Embedded_statement* nieterminalnych służy do instrukcji, które są wyświetlane w inne instrukcje. Korzystanie z *embedded_statement* zamiast *instrukcji* nie obejmuje korzystanie z instrukcje deklaracji i labeled — instrukcje w tych kontekstach. Przykład
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
powoduje błąd w czasie kompilacji, ponieważ `if` instrukcja wymaga *embedded_statement* zamiast *instrukcji* jego IF gałęzi. Jeśli ten kod, były dozwolone, następnie zmienna `i` może być zadeklarowany, ale nigdy nie może być używana. Jednak, umieszczając `i`w deklaracji w bloku, przykład jest nieprawidłowa.

## <a name="end-points-and-reachability"></a>Punkty końcowe i osiągalności

Ma każdej instrukcji ***punktu końcowego***. W sposób intuicyjny punkt końcowy w instrukcji jest to lokalizacja, który następuje bezpośrednio po instrukcji. Zasady wykonywania instrukcje złożone (instrukcji, które zawierają osadzonych instrukcji) określ akcję, która jest wykonywana, gdy kontrola osiąga punkt końcowy osadzona instrukcja. Na przykład gdy kontrola osiąga punkt końcowy w instrukcji w bloku, formant jest przekazywany do następnej instrukcji w bloku.

Jeśli oświadczenie prawdopodobnie można osiągnąć przez wykonanie, instrukcja jest nazywany ***osiągalny***. Z drugiej strony, w przypadku braku możliwości instrukcja zostanie wykonana instrukcja jest nazywany ***nieosiągalny***.

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
drugie wywołanie `Console.WriteLine` jest nieosiągalny, ponieważ nie ma możliwości zostanie wykonana instrukcja.

Ostrzeżenie jest zgłaszane w przypadku kompilator Określa, że instrukcja jest nieosiągalna. Jest specjalnie nie błąd dla instrukcji jest nieosiągalne.

Aby ustalić, czy określonej instrukcji lub punkt końcowy jest osiągalny, kompilator wykonuje analizę przepływu zgodnie z regułami osiągalności zdefiniowane dla każdej instrukcji. Analizę przepływu bierze pod uwagę wartości wyrażeń stałych ([wyrażeń stałych](expressions.md#constant-expressions)) które kontrolują zachowanie instrukcji, ale możliwe wartości inne niż stałe wyrażenia nie są uwzględniane. Innymi słowy do celów analizy przepływu sterowania, niestałe wyrażenie danego typu została uznana za wszystkie możliwe wartości tego typu.

W przykładzie
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
wyrażenie logiczne z `if` instrukcja jest wyrażeniem stałym, ponieważ oba operandy z `==` operator są stałymi. Jak stałe wyrażenie jest obliczane w czasie kompilacji, tworzenie wartość `false`, `Console.WriteLine` wywołanie jest traktowane jako niedostępny. Jednak jeśli `i` zostanie zmieniony jako zmienna lokalna
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
`Console.WriteLine` wywołanie jest uważany za dostępny, mimo że w rzeczywistości zostanie nigdy nie uruchomiony.

*Bloku* funkcji składowej jest zawsze uważane za dostępne. Kolejno oceny zasad osiągalności każdej instrukcji w bloku, można określić wysyłające dowolnej podanej instrukcji.

W przykładzie
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
osiągalności drugiego `Console.WriteLine` jest określany w następujący sposób:

*  Pierwszy `Console.WriteLine` Instrukcja wyrażenia jest osiągalny ponieważ blok `F` metody jest osiągalny.
*  Punkt końcowy pierwszego `Console.WriteLine` Instrukcja wyrażenia jest dostępny, ponieważ w tej instrukcji jest nieosiągalny.
*  `if` Instrukcji jest dostępny, ponieważ koniec wskazuje pierwszego `Console.WriteLine` Instrukcja wyrażenia jest osiągalny.
*  Drugi `Console.WriteLine` Instrukcja wyrażenia jest osiągalny ponieważ wyrażenie logiczne z `if` instrukcji nie ma wartości stałej `false`.

Istnieją dwie sytuacje, w których jest to błąd czasu kompilacji dla punktu końcowego instrukcji być dostępny:

*  Ponieważ `switch` instrukcji nie zezwala na sekcji przełącznika, aby "przechodzić" do następnej sekcji przełącznika, jest to błąd czasu kompilacji dla punktu końcowego listy instrukcji w sekcji przełącznika, być dostępny. Jeśli ten błąd występuje, jest zazwyczaj wskazuje który `break` brakuje instrukcji.
*  Jest to błąd czasu kompilacji dla punktu końcowego w bloku funkcja elementu członkowskiego, który oblicza wartość być dostępny. Jeśli ten błąd występuje, zazwyczaj jest wskazanie, `return` brakuje instrukcji.

## <a name="blocks"></a>Bloki

A *bloku* zezwala na wiele instrukcji, które ma zostać zapisany w kontekstach, których jest dozwolone pojedynczej instrukcji.

```antlr
block
    : '{' statement_list? '}'
    ;
```

A *bloku* składa się z opcjonalną *statement_list* ([Wyświetla listę instrukcji](statements.md#statement-lists)), ujęty w nawiasy klamrowe. W przypadku pominięcia listy instrukcji bloku jest określany jako pusta.

Blok może zawierać instrukcje deklaracji ([instrukcje deklaracji](statements.md#declaration-statements)). Zakres zmiennej lokalnej zmiennej czy stałej zadeklarowane w bloku jest bloku.

Blok jest wykonywany w następujący sposób:

*  Jeśli blok jest pusta, kontrola jest przekazywana do punktu końcowego bloku.
*  Jeśli blok nie jest pusty, kontrola jest przekazywana do listy instrukcji. Gdy i kontrola osiąga punkt końcowy listy instrukcji, kontrola jest przekazywana do punktu końcowego bloku.

Listy instrukcji w bloku jest osiągalna, jeśli blok, sama jest osiągalny.

Punkt końcowy w bloku jest osiągalny, jeśli blok jest puste lub jeśli punkt końcowy listy instrukcji jest nieosiągalny.

A *bloku* zawierający co najmniej jeden `yield` instrukcji ([instrukcji yield](statements.md#the-yield-statement)) nazywa się blokiem iteratora. Iterator bloki są używane do implementowania funkcji członków jako Iteratory ([Iteratory](classes.md#iterators)). Niektóre dodatkowe ograniczenia mają zastosowanie do iteratora bloków:

*  Jest to błąd czasu kompilacji dla `return` instrukcję, aby są wyświetlane w bloku iteratora (ale `yield return` są dozwolone).
*  Jest to błąd czasu kompilacji dla blokiem iteratora, tak zawierała niebezpiecznym kontekście ([niebezpieczny kontekst](unsafe-code.md#unsafe-contexts)). Blokiem iteratora zawsze definiuje bezpieczne kontekstu, nawet wtedy, gdy jego deklaracji jest zagnieżdżony w niebezpiecznym kontekście.

### <a name="statement-lists"></a>List — instrukcja

A ***listy instrukcji*** składa się z jedną lub więcej instrukcji, zapisane w sekwencji. Instrukcja listy występują w *bloku*s ([bloki](statements.md#blocks)) i w *switch_block*s ([instrukcji switch](statements.md#the-switch-statement)).

```antlr
statement_list
    : statement+
    ;
```

Listy instrukcji jest wykonywana przez transferowanie formantu do pierwszej instrukcji. Gdy i kontrola osiąga punkt końcowy w instrukcji, kontrola jest przekazywana do następnej instrukcji. Gdy i kontrola osiąga punkt końcowy ostatnią instrukcję, kontrola jest przekazywana do punktu końcowego listy instrukcji.

Instrukcja z listy instrukcji jest osiągalna, jeśli co najmniej jedną z następujących ma wartość true:

*  Instrukcja jest pierwszej instrukcji i samej listy instrukcji jest nieosiągalny.
*  Poprzednia instrukcja punkt końcowy jest osiągalny.
*  Instrukcja to instrukcja labeled i etykieta odwołuje się osiągalny `goto` instrukcji.

Punkt końcowy listy instrukcji jest osiągalny, jeśli punkt końcowy ostatnią instrukcję, na liście jest dostępny.

## <a name="the-empty-statement"></a>Pusta instrukcja

*Empty_statement* nic nie robi.

```antlr
empty_statement
    : ';'
    ;
```

Pusta instrukcja jest używane, gdy istnieją żadne operacje do wykonania w kontekście, w których wymagane jest użycie instrukcji.

Wykonanie pustą instrukcję po prostu przekazuje sterowanie do instrukcji punktu końcowego. W związku z tym punkt końcowy pusta instrukcja jest osiągalna, jeśli pusta instrukcja jest osiągalny.

Pusta instrukcja może służyć podczas zapisywania `while` instrukcja o wartości null treści:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Ponadto pusta instrukcja może służyć do deklarowania etykietę tuż przed zamykającym "`}`" bloku:
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>Labeled — instrukcje

A *labeled_statement* zezwala na instrukcję, aby być poprzedzona przez etykietę. Labeled — instrukcje są dozwolone w blokach, ale nie są dozwolone jako osadzonych instrukcji.

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Instrukcja labeled deklaruje etykietę o nazwie podanej przez *identyfikator*. Zakres etykiety jest cały blok, w którym etykiety zadeklarowano, wraz ze wszystkimi zagnieżdżonych bloków. Jest to błąd czasu kompilacji dla dwóch etykiet o takiej samej nazwie, aby mieć nakładających się zakresów.

Etykiety mogą być przywoływane z `goto` instrukcji ([instrukcji goto](statements.md#the-goto-statement)) w zakresie etykiety. Oznacza to, że `goto` instrukcji można przekazać sterowanie wewnątrz bloków i poza bloków, ale nigdy nie na bloki.

Etykiety mają własnej przestrzeni deklaracji i nie kolidują z innymi identyfikatorami. Przykład
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
jest prawidłowy i używa nazwy `x` jako parametr i etykietę.

Wykonanie instrukcji oznaczonej etykietą dokładnie odpowiada wykonywania instrukcji następującej etykiety.

Oprócz osiągalności dostarczone przez Normalny przepływ sterowania, jest osiągalna, jeśli etykieta odwołuje się osiągalny instrukcji oznaczonej etykietą `goto` instrukcji. (Wyjątek: Jeśli `goto` Instrukcja znajduje się wewnątrz `try` zawierającej `finally` bloku i instrukcja labeled znajduje się poza `try`i punkt końcowy `finally` bloku jest niedostępny, a następnie nie jest dostępny z etykietą instrukcji które `goto` instrukcja.)

## <a name="declaration-statements"></a>Instrukcje deklaracji

A *declaration_statement* deklaruje lokalną zmienną lub stałą. Instrukcje deklaracji są dozwolone w blokach, ale nie są dozwolone jako osadzonych instrukcji.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Deklaracje zmiennych lokalnych

A *local_variable_declaration* deklaruje jeden lub więcej zmiennych lokalnych.

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

*Local_variable_type* z *local_variable_declaration* bezpośrednio określa rodzaj zmiennych, wprowadzone przez tę deklarację, albo wskazuje z identyfikatorem `var` , można wywnioskować typu, oparte na inicjatora. Typ następuje lista *local_variable_declarator*s, z których każdy wprowadza nową zmienną. A *local_variable_declarator* składa się z *identyfikator* , nazwy zmiennej, opcjonalnie następuje "`=`" token i *local_variable_initializer* daje to początkowa wartość zmiennej.

W kontekście deklaracji zmiennej lokalnej, var identyfikator działa jako kontekstowe słowo kluczowe ([słowa kluczowe](lexical-structure.md#keywords)). Gdy *local_variable_type* jest określony jako `var` i bez typu o nazwie `var` jest w zakresie deklaracja znajduje się ***niejawnie wpisanych deklaracji zmiennej lokalnej***, którego typem jest wnioskowany z typu wyrażenia inicjatora skojarzone. Niejawnie wpisane deklaracje zmiennych lokalnych podlegają następującym ograniczeniom:

*  *Local_variable_declaration* nie może zawierać wiele *local_variable_declarator*s.
*  *Local_variable_declarator* musi zawierać *local_variable_initializer*.
*  *Local_variable_initializer* musi być *wyrażenie*.
*  Inicjator *wyrażenie* musi mieć typ kompilacji.
*  Inicjator *wyrażenie* nie można odwołać się sama Zmienna zadeklarowana

Poniżej przedstawiono przykłady niepoprawne niejawnie wpisane deklaracji zmiennych lokalnych:

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

Wartość zmiennej lokalnej jest uzyskiwana przy użyciu wyrażenia *simple_name* ([proste nazwy](expressions.md#simple-names)), i wartość zmiennej lokalnej jest modyfikowane przy użyciu *przypisania* () [Operatory przypisania](expressions.md#assignment-operators)). Zmienna lokalna musi zostać zdecydowanie przypisany ([asercję określonego przypisania](variables.md#definite-assignment)) w każdej lokalizacji, w których uzyskuje się wartość.

Zakres zmiennej lokalnej zadeklarowanej w *local_variable_declaration* bloku, w którym występuje deklaracja. Jest błędem do odwoływania się do zmiennej lokalnej w stanie tekstową poprzedzającym *local_variable_declarator* zmiennej lokalnej. W zakresie zmiennej lokalnej jest błąd kompilacji, aby zadeklarować lokalnego innej zmiennej czy stałej o takiej samej nazwie.

Deklaracji zmiennej lokalnej, która deklaruje wiele zmiennych jest odpowiednikiem w wielu deklaracjach zmiennych pojedynczego tego samego typu. Ponadto inicjatorze zmiennej w deklaracji zmiennej lokalnej dokładnie odpowiada instrukcji przypisania, który jest wstawiany bezpośrednio po deklaracji.

Przykład
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
dokładnie odpowiada
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

W niejawnie wpisane deklaracji zmiennej lokalnej typ zmiennej lokalnej deklarowanej przyjmuje się taka sama jak typ wyrażenia używane do inicjowania zmiennej. Na przykład:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

Niejawnie wpisane lokalnej deklaracji zmiennej powyżej dokładnie odpowiadają następujące deklaracje jawnie wpisanych:
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>Deklaracji stałej lokalnej

A *local_constant_declaration* deklaruje co najmniej jedną stałą lokalnego.

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

*Typu* z *local_constant_declaration* Określa typ stałe wprowadzonych przez tę deklarację. Typ następuje lista *constant_declarator*s, z których każdy wprowadza nowe — stała. A *constant_declarator* składa się z *identyfikator* nazw stałą, a następnie "`=`" token, a następnie *constant_expression* ([ Wyrażenia stałe](expressions.md#constant-expressions)) daje wartość stałej.

*Typu* i *constant_expression* deklaracji stałej lokalnej należy wykonać te same zasady, jak te w deklaracji stałej składowej ([stałe](classes.md#constants)).

Wartość stała lokalna są uzyskiwane przy użyciu wyrażenia *simple_name* ([proste nazwy](expressions.md#simple-names)).

Zakres stała lokalna jest bloku, w którym występuje deklaracja. Jest błędem do odwoływania się do stała lokalna w stanie tekstową poprzedzającym jego *constant_declarator*. W zakresie stała lokalna jest błąd kompilacji, aby zadeklarować lokalnego innej zmiennej czy stałej o takiej samej nazwie.

Deklaracji stałej lokalnej, która deklaruje kilka stałych jest odpowiednikiem w wielu deklaracjach pojedynczego stałe tego samego typu.

## <a name="expression-statements"></a>Instrukcje wyrażeń

*Expression_statement* daje w wyniku podanego wyrażenia. Wartość obliczona przez wyrażenie, jeśli istnieje, zostanie odrzucony.

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

Nie wszystkie wyrażenia są dozwolone jako instrukcji. W szczególności wyrażenia takie jak `x + y` i `x == 1` który jedynie obliczenia wartości, (które zostaną odrzucone), nie są dozwolone jako instrukcji.

Wykonywanie *expression_statement* oblicza wyrażenie zawarte, a następnie przekazuje sterowanie do punktu końcowego *expression_statement*. Punkt końcowy *expression_statement* jest dostępny, jeśli to *expression_statement* jest osiągalny.

## <a name="selection-statements"></a>Instrukcje wyboru

Instrukcje wyboru Wybierz jeden z wielu możliwych instrukcji do wykonania na podstawie wartości w wyrażeniu.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>If — instrukcja

`if` Instrukcja wybiera instrukcję do wykonania w oparciu o wartość wyrażenia logicznego.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

`else` Część jest skojarzony z leksykalnie najbliższej poprzedzającej `if` , jest dozwolony przez składnię. W efekcie `if` instrukcji formularza
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

*  *Boolean_expression* ([wyrażeń logicznych](expressions.md#boolean-expressions)) jest oceniany.
*  Jeśli wyrażenie logiczne daje `true`, kontrola jest przekazywana do pierwszej instrukcji osadzonych. Gdy i kontrola osiąga punkt końcowy w tej instrukcji, kontrola jest przekazywana do punktu końcowego `if` instrukcji.
*  Jeśli wyrażenie logiczne daje `false` i, jeśli `else` części, kontrola jest przekazywana do drugiego osadzona instrukcja. Gdy i kontrola osiąga punkt końcowy w tej instrukcji, kontrola jest przekazywana do punktu końcowego `if` instrukcji.
*  Jeśli wyrażenie logiczne daje `false` i, jeśli `else` część nie jest obecny, kontrola jest przekazywana do punktu końcowego `if` instrukcji.

Pierwszy osadzona instrukcja `if` instrukcja jest osiągalny Jeśli `if` instrukcja jest osiągalny i wyrażenie warunkowe nie ma wartości stałej `false`.

Drugi osadzona instrukcja `if` instrukcji, jeśli jest obecny, jest dostępny jeśli `if` instrukcja jest osiągalny i wyrażenie warunkowe nie ma wartości stałej `true`.

Punkt końcowy `if` instrukcja jest osiągalna, jeśli co najmniej jeden z jego osadzonych instrukcji punktu końcowego jest osiągalny. Ponadto punkt końcowy `if` instrukcji bez `else` części jest dostępny jeśli `if` instrukcja jest osiągalny i wyrażenie warunkowe nie ma wartości stałej `true`.

### <a name="the-switch-statement"></a>Instrukcja switch

Instrukcja switch wybiera do wykonania listę instrukcji, mających element label skojarzonego przełącznika, który odpowiada wartości w wyrażeniu przełącznika.

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

A *switch_statement* składa się z słowa kluczowego `switch`, następuje wyrażenia ujętego w nawiasy (nazywane w wyrażeniu przełącznika), a następnie *switch_block*. *Switch_block* składa się z zero lub więcej *switch_section*s, ujęte w nawiasy klamrowe. Każdy *switch_section* składa się z co najmniej jeden *switch_label*następuje s *statement_list* ([Wyświetla listę instrukcji](statements.md#statement-lists)).

***Regulujące typu*** z `switch` instrukcji jest ustanawiane przez wyrażenie switch.

*  Jeśli typ wyrażenia switch `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, lub *enum_type*, lub jeśli jest typu dopuszczającego wartość null, jednego z tych typów, a następnie jest regulujące typu `switch` instrukcji.
*  W przeciwnym razie dokładnie jeden zdefiniowany przez użytkownika niejawnej konwersji ([zdefiniowane przez użytkownika konwersje](conversions.md#user-defined-conversions)) musi istnieć z typu wyrażenia switch do jednej z następujących możliwych typów regulujące: `sbyte`, `byte`, `short` , `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, lub typ dopuszczający wartość null, jednego z tych typów.
*  W przeciwnym razie nie takie istnieje niejawna konwersja lub jeśli więcej niż jeden taki niejawnej konwersji istnieje, występuje błąd kompilacji.

Wyrażenie stałe każdego `case` etykiety musi określa wartość, która jest niejawnie konwertowany ([niejawne konwersje](conversions.md#implicit-conversions)) do zarządzania typu `switch` instrukcji. Występuje błąd kompilacji, jeśli co najmniej dwóch `case` etykiety w tej samej `switch` instrukcji Określ taką samą wartość stałą.

Może być co najwyżej jeden `default` etykiet w instrukcji switch.

A `switch` instrukcja jest wykonywana w następujący sposób:

*  Wyrażenie switch jest obliczane i konwertowane na typ zarządzania.
*  Jeśli określono jednej ze stałych w `case` etykiety w tej samej `switch` instrukcja jest równa wartości w wyrażeniu przełącznika, kontrola jest przekazywana do listy instrukcji po dopasowanej `case` etykiety.
*  Jeśli żadna ze stałych określone w `case` etykiety w tej samej `switch` instrukcja jest równa wartości w wyrażeniu przełącznika i, jeśli `default` etykiety, kontrola jest przekazywana do instrukcji poniżej listy `default` Etykieta.
*  Jeśli żadna ze stałych określone w `case` etykiety w tej samej `switch` instrukcja jest równa wartości w wyrażeniu przełącznika i jeśli nie `default` etykiety, kontrola jest przekazywana do punktu końcowego `switch` instrukcji.

Jeśli punkt końcowy listy instrukcji w sekcji przełącznika, jest dostępny, występuje błąd kompilacji. Jest to nazywane reguły "nie należą do". Przykład
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
jest prawidłowy, ponieważ żadna sekcja przełącznika ma osiągalnego punktu końcowego. W przeciwieństwie do C i C++ wykonanie sekcji przełącznika nie jest dozwolone "przechodzić" do następnej sekcji przełącznika, a przykład
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
spowoduje błąd kompilacji. Podczas wykonywania sekcji przełącznika ma być stosowana przez wykonanie innej sekcji przełącznika, jawnie `goto case` lub `goto default` instrukcji należy użyć:
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

Wiele etykiet są dozwolone w *switch_section*. Przykład
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
jest prawidłowy. Przykład naruszają reguły "nie należą do", ponieważ etykiet `case 2:` i `default:` są dostępne w ramach tego samego *switch_section*.

Reguła "nie należą do" uniemożliwia klasę typowych błędów, które występują w językach C i C++ gdy `break` instrukcji przypadkowo są pomijane. Ponadto ze względu na tę regułę sekcji przełącznika `switch` instrukcji można dowolnie zmieniać wprowadza bez wywierania wpływu na zachowanie poufności informacji. Na przykład sekcji `switch` powyższych instrukcji można wycofać bez wywierania wpływu na zachowanie poufności informacji:
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

Listy instrukcji w sekcji przełącznika, zazwyczaj kończy się `break`, `goto case`, lub `goto default` instrukcji, ale wszystkie konstrukcji, która renderuje punkt końcowy listy instrukcji jest nieosiągalny jest dozwolone. Na przykład `while` instrukcji wyrażenia logicznego w wartości clientauthtrustmode `true` wiadomo, że nigdy nie zasięg jej punkt końcowy. Podobnie `throw` lub `return` instrukcji zawsze zostanie przetransferowany kontrolki w innym miejscu i nigdy nie osiągnie jej punkt końcowy. Tak więc poniższy przykład jest prawidłowy:
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

Typ zarządzania `switch` instrukcja może być typem `string`. Na przykład:
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

Operatory porównania ciągów, takich jak ([operatory porównania ciągów](expressions.md#string-equality-operators)), `switch` instrukcji jest uwzględniana wielkość liter i wykona sekcji danym przełącznikiem tylko wtedy, gdy ciąg wyrażenia switch dokładnie odpowiada `case` etykiety stała.

Gdy regulujące typu `switch` instrukcja jest `string`, wartość `null` jest dozwolona jako stała etykietę "case".

*Statement_list*s *switch_block* może zawierać instrukcje deklaracji ([instrukcje deklaracji](statements.md#declaration-statements)). Zakres zmiennej lokalnej zmiennej czy stałej zadeklarowanych w blok "switch" jest blok "switch".

Listy instrukcji w sekcji danym przełącznikiem jest osiągalny Jeśli `switch` instrukcji jest osiągalny i co najmniej jedną z następujących ma wartość true:

*  Wyrażenie switch jest wartością stałą.
*  Wyrażenie switch jest wartością stałą, który odpowiada `case` etykiety w sekcji przełącznika.
*  Wyrażenie switch jest wartością stałą, która nie pasuje do żadnego `case` etykietę, a sekcja przełącznika zawiera `default` etykiety.
*  Etykiety switch sekcji przełącznika odwołuje się osiągalny `goto case` lub `goto default` instrukcji.

Punkt końcowy `switch` instrukcja jest osiągalna, jeśli co najmniej jedną z następujących ma wartość true:

*  `switch` Instrukcja zawiera osiągalny `break` instrukcję, która kończy działanie `switch` instrukcji.
*  `switch` Instrukcja jest osiągalny, wyrażenie switch jest wartością stałą, a nie `default` etykiety jest obecny.
*  `switch` Instrukcja jest osiągalny, wyrażenie switch jest wartością stałą, która nie pasuje do żadnego `case` etykiety, a nie `default` etykiety jest obecny.

## <a name="iteration-statements"></a>Instrukcje iteracji

Instrukcje iteracji wykonać wielokrotnie osadzona instrukcja.

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>While — instrukcja

`while` Instrukcji warunkowo wykonuje osadzona instrukcja zero lub więcej razy.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

A `while` instrukcja jest wykonywana w następujący sposób:

*  *Boolean_expression* ([wyrażeń logicznych](expressions.md#boolean-expressions)) jest oceniany.
*  Jeśli wyrażenie logiczne daje `true`, kontrola jest przekazywana do instrukcji osadzonych. Gdy, a kontrola osiąga punkt końcowy osadzonych instrukcji (prawdopodobnie w wykonywania `continue` instrukcji), kontrola jest przekazywana do stanu sprzed `while` instrukcji.
*  Jeśli wyrażenie logiczne daje `false`, kontrola jest przekazywana do punktu końcowego `while` instrukcji.

W ramach osadzona instrukcja z `while` instrukcji, `break` — instrukcja ([instrukcji break](statements.md#the-break-statement)) może służyć do kontrola jest przekazywana do punktu końcowego `while` — instrukcja (tym samym Kończenie iteracji osadzonego Instrukcja) i `continue` — instrukcja ([instrukcji continue](statements.md#the-continue-statement)) może służyć do kontrola jest przekazywana do punktu końcowego osadzona instrukcja (ten sposób wykonywania iteracji innego `while` instrukcji).

Osadzona instrukcja z `while` instrukcja jest osiągalny Jeśli `while` instrukcja jest osiągalny i wyrażenie warunkowe nie ma wartości stałej `false`.

Punkt końcowy `while` instrukcja jest osiągalna, jeśli co najmniej jedną z następujących ma wartość true:

*  `while` Instrukcja zawiera osiągalny `break` instrukcję, która kończy działanie `while` instrukcji.
*  `while` Instrukcja jest osiągalny i wyrażenie warunkowe nie ma wartości stałej `true`.

### <a name="the-do-statement"></a>Instrukcji

`do` Instrukcji warunkowo wykonuje osadzona instrukcja jeden lub więcej razy.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

A `do` instrukcja jest wykonywana w następujący sposób:

*  Kontrola jest przekazywana do instrukcji osadzonych.
*  Gdy, a kontrola osiąga punkt końcowy osadzonych instrukcji (prawdopodobnie w wykonywania `continue` instrukcji), *boolean_expression* ([wyrażeń logicznych](expressions.md#boolean-expressions)) jest oceniany. Jeśli wyrażenie logiczne daje `true`, kontrola jest przekazywana do stanu sprzed `do` instrukcji. W przeciwnym razie kontrola jest przekazywana do punktu końcowego `do` instrukcji.

W ramach osadzona instrukcja z `do` instrukcji, `break` — instrukcja ([instrukcji break](statements.md#the-break-statement)) może służyć do kontrola jest przekazywana do punktu końcowego `do` — instrukcja (tym samym Kończenie iteracji osadzonego Instrukcja) i `continue` — instrukcja ([instrukcji continue](statements.md#the-continue-statement)) może służyć do kontrola jest przekazywana do punktu końcowego osadzona instrukcja.

Osadzona instrukcja z `do` instrukcja jest osiągalny Jeśli `do` instrukcji jest nieosiągalny.

Punkt końcowy `do` instrukcja jest osiągalna, jeśli co najmniej jedną z następujących ma wartość true:

*  `do` Instrukcja zawiera osiągalny `break` instrukcję, która kończy działanie `do` instrukcji.
*  Osadzona instrukcja punkt końcowy jest osiągalny i wyrażenie warunkowe nie ma wartości stałej `true`.

### <a name="the-for-statement"></a>For — instrukcja

`for` Instrukcja oblicza sekwencję wyrażenia inicjowania, a następnie, gdy warunek ma wartość true, wielokrotnie wykonuje osadzona instrukcja i ocenia sekwencji wyrażeń iteracji.

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

*For_initializer*, jeśli jest obecny, składa się z jednej *local_variable_declaration* ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)) lub Podaj listę *statement_ wyrażenie*s ([instrukcje wyrażeń](statements.md#expression-statements)) rozdzielonych przecinkami. Zakres zmienna lokalna zadeklarowana przez *for_initializer* rozpoczyna się od *local_variable_declarator* zmiennej i rozszerza się na końcu osadzona instrukcja. Zakres obejmuje *for_condition* i *for_iterator*.

*For_condition*, jeśli jest obecna, musi być *boolean_expression* ([wyrażeń logicznych](expressions.md#boolean-expressions)).

*For_iterator*, jeśli jest obecny, składa się z listy *statement_expression*s ([instrukcje wyrażeń](statements.md#expression-statements)) rozdzielonych przecinkami.

Dla instrukcji jest wykonywany w następujący sposób:

*  Jeśli *for_initializer* jest obecne, inicjatory zmiennej lub wyrażenia instrukcji są wykonywane w kolejności, są one zapisywane. Wykonanie tego kroku tylko raz.
*  Jeśli *for_condition* jest obecny, zostanie on oceniony.
*  Jeśli *for_condition* nie jest obecny lub jeśli wyznaczające `true`, kontrola jest przekazywana do instrukcji osadzonych. Gdy, a kontrola osiąga punkt końcowy osadzonych instrukcji (prawdopodobnie w wykonywania `continue` instrukcji), wyrażeń *for_iterator*, jeśli istnieją, są obliczane w kolejności, a następnie innej iteracji wykonywana, począwszy od wersji ewaluacyjnej usługi *for_condition* w poprzednim kroku.
*  Jeśli *for_condition* jest obecny i ocena daje `false`, kontrola jest przekazywana do punktu końcowego `for` instrukcji.

W ramach osadzona instrukcja z `for` instrukcji, `break` — instrukcja ([instrukcji break](statements.md#the-break-statement)) może służyć do kontrola jest przekazywana do punktu końcowego `for` — instrukcja (tym samym Kończenie iteracji osadzonego Instrukcja) i `continue` — instrukcja ([instrukcji continue](statements.md#the-continue-statement)) może służyć do kontrola jest przekazywana do punktu końcowego osadzona instrukcja (wykonywanie w związku z tym *for_iterator* i wykonywanie iteracji innego `for` instrukcji, począwszy od *for_condition*).

Osadzona instrukcja z `for` instrukcja jest osiągalna, jeśli jest spełniony jeden z następujących czynności:

*  `for` Instrukcja jest dostępny i nie *for_condition* jest obecny.
*  `for` Instrukcja jest osiągalny i *for_condition* jest obecny i nie ma wartości stałej `false`.

Punkt końcowy `for` instrukcja jest osiągalna, jeśli co najmniej jedną z następujących ma wartość true:

*  `for` Instrukcja zawiera osiągalny `break` instrukcję, która kończy działanie `for` instrukcji.
*  `for` Instrukcja jest osiągalny i *for_condition* jest obecny i nie ma wartości stałej `true`.

### <a name="the-foreach-statement"></a>Instrukcja foreach

`foreach` Instrukcji wylicza elementów kolekcji, wykonywania osadzonych instrukcji dla każdego elementu kolekcji.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

*Typu* i *identyfikator* z `foreach` instrukcja deklaruje ***zmiennej iteracyjnej*** instrukcji. Jeśli `var` identyfikator jest podawana jako *local_variable_type*, a nie typu o nazwie `var` jest w zakresie, zmienną iteracji jest nazywany ***zmiennej iteracyjnej niejawnie wpisane***, i jego typ przyjmuje się typ elementu `foreach` instrukcji, jak określono poniżej. Zmienna iteracji odnosi się do zmiennej lokalnej tylko do odczytu z zakresem, który rozciąga osadzona instrukcja. Podczas wykonywania `foreach` instrukcji, Zmienna iteracji reprezentuje element kolekcji, dla którego iteracji obecnie wykonywane. Występuje błąd kompilacji, jeśli osadzona instrukcja próbuje zmodyfikować zmienną iteracji (za pośrednictwem przydziału lub `++` i `--` operatory) lub Przekaż Zmienna iteracji jako `ref` lub `out` parametru.

W następującym w celu skrócenia programu, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` i `IEnumerator<T>` można znaleźć odpowiadające typy w przestrzeniach nazw `System.Collections` i `System.Collections.Generic`.

Foreach — instrukcja przetwarzania kompilacji najpierw określi ***— typ kolekcji***, ***typu modułu wyliczającego*** i ***typ elementu*** wyrażenia. Oznaczanie przebiega w następujący sposób:

*  Jeśli typ `X` z *wyrażenie* jest typem tablicy, a następnie istnieje niejawna konwersja odwołania z `X` do `IEnumerable` interfejsu (ponieważ `System.Array` implementuje ten interfejs). ***— Typ kolekcji*** jest `IEnumerable` interfejsu ***typu modułu wyliczającego*** jest `IEnumerator` interfejsu i ***typ elementu*** jest typ elementu Tablica typu `X`.
*  Jeśli typ `X` z *wyrażenie* jest `dynamic` , a następnie istnieje niejawna konwersja z *wyrażenie* do `IEnumerable` interfejsu ([niejawne dynamiczny Konwersje](conversions.md#implicit-dynamic-conversions)). ***— Typ kolekcji*** jest `IEnumerable` interfejsu i ***typu modułu wyliczającego*** jest `IEnumerator` interfejsu. Jeśli `var` identyfikator jest podawana jako *local_variable_type* , a następnie ***typ elementu*** jest `dynamic`, w przeciwnym razie jest `object`.
*  W przeciwnym razie określenia czy typ `X` ma odpowiednią `GetEnumerator` metody:
   * Wykonaj wyszukać składowej w typie `X` o identyfikatorze `GetEnumerator` i bez argumentów typu. Jeśli wyszukanie członka nie generuje dopasowania lub powoduje niejednoznaczność lub tworzy dopasowanie, która nie jest grupą metod, sprawdź, czy interfejs wyliczalny zgodnie z poniższym opisem. Zaleca się, że ostrzeżenie wydawane Jeśli wyszukiwanie elementu członkowskiego generuje wszystko oprócz grupy metod lub Brak dopasowania.
   * Wykonanie rozwiązania przeciążenia, przy użyciu wynikowy grupy metod i pustą listą argumentów. Jeśli funkcja rozpoznawania przeciążeń w wyniku żadnych odpowiednich metod, powoduje niejednoznaczność lub skutkuje pojedynczego najlepszą metodę, ale ta metoda jest statyczny lub niepubliczny, wyszukaj interfejs wyliczalny zgodnie z poniższym opisem. Zaleca się, że ostrzeżenie wydawane Jeśli funkcja rozpoznawania przeciążeń generuje niczego poza metodą jednoznaczną publiczne wystąpienia lub nie ma zastosowania metody.
   * Jeśli typ zwracany `E` z `GetEnumerator` metoda nie jest klasą, typu struktury lub interfejsu, błąd jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.
   * Wyszukiwanie elementu członkowskiego jest wykonywana na `E` z identyfikatorem `Current` i bez argumentów typu. W przypadku wyszukiwania elementu członkowskiego generuje niezgodności, wynikiem jest błąd lub wynik jest jakikolwiek inny niż właściwości publiczne wystąpienia, która umożliwia odczytywanie, jest generowany błąd i nie trzeba wykonywać dalszych czynności są wykonywane.
   * Wyszukiwanie elementu członkowskiego jest wykonywana na `E` z identyfikatorem `MoveNext` i bez argumentów typu. W przypadku wyszukiwania elementu członkowskiego generuje niezgodności, wynikiem jest błąd lub wynik jest końcówką grupy metod, jest generowany błąd i nie trzeba wykonywać dalszych czynności są wykonywane.
   * Rozpoznanie przeciążenia odbywa się w grupie metody z pustą listą argumentów. Jeśli wyniki rozpoznawania przeciążenia w nie odpowiednich metod, powoduje niejednoznaczność lub wyniki w pojedynczej najlepszej metody, ale ta metoda jest statyczna lub niepubliczna lub nie jest typem zwracanym `bool`, jest generowany błąd i nie trzeba wykonywać dalszych czynności są wykonywane.
   * ***— Typ kolekcji*** jest `X`, ***typu modułu wyliczającego*** jest `E`i ***typ elementu*** jest typem `Current` właściwości.

*  W przeciwnym razie sprawdź, czy wyliczalny interfejs:
   * Jeśli wśród wszystkich typów `Ti` dla której istnieje niejawna konwersja z `X` do `IEnumerable<Ti>`, ma typ unikatowego `T` tak, aby `T` nie `dynamic` i dla wszystkich innych `Ti` istnieje niejawna konwersja z `IEnumerable<T>` do `IEnumerable<Ti>`, a następnie ***— typ kolekcji*** jest interfejsem `IEnumerable<T>`, ***typu modułu wyliczającego*** interfejs `IEnumerator<T>`i ***typ elementu*** jest `T`.
   * W przeciwnym razie, jeśli istnieje więcej niż jeden taki typ `T`, a następnie jest generowany błąd i nie trzeba wykonywać dalszych czynności są wykonywane.
   * W przeciwnym razie, jeśli istnieje niejawna konwersja z `X` do `System.Collections.IEnumerable` interfejsu, a następnie ***— typ kolekcji*** ten interfejs jest ***typu modułu wyliczającego*** interfejs `System.Collections.IEnumerator`i ***typ elementu*** jest `object`.
   * W przeciwnym razie jest generowany błąd i nie trzeba wykonywać dalszych czynności są wykonywane.

Powyższe kroki, jeśli to się powiedzie, jednoznacznie wygenerować typu kolekcji `C`, moduł wyliczający typu `E` i typ elementu `T`. Instrukcja foreach formularza
```csharp
foreach (V v in x) embedded_statement
```
jest rozszerzany do:
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

Zmienna `e` nie jest widoczne lub dostępne do wyrażenia `x` lub osadzona instrukcja lub inny kod źródłowy programu. Zmienna `v` jest tylko do odczytu w osadzonych instrukcji. Jeśli nie jest jawną konwersję ([jawne konwersje](conversions.md#explicit-conversions)) z `T` (typ elementu) do `V` ( *local_variable_type* w instrukcji foreach), błąd jest generowany. i nie trzeba wykonywać dalszych czynności są wykonywane. Jeśli `x` ma wartość `null`, `System.NullReferenceException` jest generowany w czasie wykonywania.

Implementacja jest dozwolony do zaimplementowania danego Instrukcja foreach inaczej, np. ze względu na wydajność, tak długo, jak zachowanie jest zgodne z rozszerzeniem powyżej.

Umieszczanie `v` wewnątrz while jest ważne w przypadku jak są przechwytywane przez funkcję anonimowe występujących w pętli *embedded_statement*.

Na przykład:
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
Jeśli `v` została zadeklarowana poza while pętli, mogłyby być udostępniane między wszystkie iteracje i jego wartość po dla pętli będzie końcowa wartość `13`, czyli jakich wywołania z `f` będzie drukować. Zamiast tego ponieważ każda iteracja ma swoje własne zmiennej `v`, jeden przechwycone przez `f` w pierwszej iteracji będą w dalszym zawierającą wartość parametru `7`, czyli, co zostanie wydrukowany. (Uwaga: zadeklarowana wcześniejszych wersjach języka C# `v` pętli while poza.)

Treść na koniec bloku jest tworzona zgodnie z następujących czynności:

*  Jeśli istnieje niejawna konwersja z `E` do `System.IDisposable` interfejsu, następnie
   *  Jeśli `E` jest typem wartości niedopuszczającym wartości, a następnie na koniec klauzula jest rozwinięty, odpowiednik semantyczne:

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  W przeciwnym razie na koniec klauzula jest rozwinięty, odpowiednik semantyczne:

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   z wyjątkiem, że jeśli `E` jest typem wartości lub parametrem typu wystąpienia typu wartości, a następnie rzutowanie typu `e` do `System.IDisposable` nie spowoduje pakowania wystąpić.

*  W przeciwnym razie, jeśli `E` typie zapieczętowanym jest ostatecznie klauzula jest rozwinięty, pusty blok:

   ```csharp
   finally {
   }
   ```

*  W przeciwnym razie na koniec klauzuli zostanie rozszerzona, aby:

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   Zmienna lokalna `d` nie jest widoczne lub dostępne dla kodu użytkownika. W szczególności, nie powoduje konfliktu z innych zmiennych, których zakres obejmuje bloku finally.

Kolejność, w której `foreach` przechodzi przez elementy tablicy, jest następujący: Dla elementów tablice jednowymiarowe jest przesunięta w indeksie kolejności rosnącej, zaczynając od indeksu `0` i kończąc indeksu `Length - 1`. Dla wielowymiarowych tablic elementów jest przesunięta w taki sposób, że indeksy po prawej stronie wymiaru są pierwszy zwiększona, a następnie dalej wymiaru po lewej stronie, i tak dalej, aby po lewej stronie.

Poniższy przykład wyświetla każdej wartości w dwuwymiarowej tablicy, w kolejności elementów:
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
Dane wyjściowe, generowane jest następująca:
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

W przykładzie
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
Typ `n` jest `int`, typ elementu `numbers`.

## <a name="jump-statements"></a>Instrukcje skoku

Instrukcje skoku bezwarunkowo przekazać sterowanie.

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

Nosi nazwę lokalizacji, do którego instrukcja skoku przekazuje sterowanie ***docelowej*** instrukcji skoku.

Gdy występuje instrukcja skoku, w bloku, a celem tej instrukcji skoku znajduje się poza ten blok, instrukcja skoku jest nazywany ***wyjść*** bloku. Gdy instrukcja skoku, mogą przesyłać sterowanie na zewnątrz bloku, je nigdy nie przekazać sterowanie do bloku.

Wykonanie instrukcji skoku jest złożona z obecnością aktywne `try` instrukcji. W przypadku braku takich `try` instrukcji, instrukcja skoku bezwarunkowo przekazuje sterowanie z instrukcji skoku do jego element docelowy. Obecności takich aktywne `try` instrukcji wykonywania jest bardziej złożona. Czy instrukcja skoku istnieje co najmniej jeden `try` skojarzone bloki `finally` bloków, kontrola początkowo jest przekazywany do `finally` bloku najbardziej wewnętrzną funkcją `try` instrukcji. Gdy, a kontrola osiąga punkt końcowy `finally` bloku, formant jest przekazywany do `finally` dalej otaczający blok `try` instrukcji. Ten proces jest powtarzany do momentu `finally` bloki wszystkich pośredniczących `try` instrukcje zostały wykonane.

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
`finally` skojarzone z dwóch bloków `try` przed kontrola jest przekazywana do obiekt docelowy instrukcji skoku, wykonywane są instrukcje.

Dane wyjściowe, generowane jest następująca:
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Instrukcja break

`break` Instrukcja kończy działanie najbliższej otaczającej `switch`, `while`, `do`, `for`, lub `foreach` instrukcji.

```antlr
break_statement
    : 'break' ';'
    ;
```

Celem `break` instrukcji jest punktem końcowym najbliższej otaczającej `switch`, `while`, `do`, `for`, lub `foreach` instrukcji. Jeśli `break` instrukcja nie jest ujęta w `switch`, `while`, `do`, `for`, lub `foreach` występuje błąd kompilacji instrukcji.

Gdy wiele `switch`, `while`, `do`, `for`, lub `foreach` instrukcje są zagnieżdżone w obrębie siebie nawzajem `break` poufności informacji odnoszą się tylko do deklaracji najbardziej. Aby przekazać sterowanie na wielu poziomach zagnieżdżenia, `goto` — instrukcja ([instrukcji goto](statements.md#the-goto-statement)) musi być używana.

A `break` instrukcja nie może zamknąć `finally` bloku ([instrukcjami "try"](statements.md#the-try-statement)). Gdy `break` występuje instrukcja w obrębie `finally` zablokować, obiektu docelowego `break` instrukcja musi być w tej samej `finally` zablokować; w przeciwnym razie wystąpi błąd kompilacji.

A `break` instrukcja jest wykonywana w następujący sposób:

*  Jeśli `break` instrukcja kończy działanie co najmniej jeden `try` skojarzone bloki `finally` bloków, kontrola początkowo jest przekazywany do `finally` bloku najbardziej wewnętrzną funkcją `try` instrukcji. Gdy, a kontrola osiąga punkt końcowy `finally` bloku, formant jest przekazywany do `finally` dalej otaczający blok `try` instrukcji. Ten proces jest powtarzany do momentu `finally` bloki wszystkich pośredniczących `try` instrukcje zostały wykonane.
*  Kontrola jest przekazywana do lokalizacji docelowej `break` instrukcji.

Ponieważ `break` instrukcji bezwarunkowo transfery kontrolować w innym miejscu, punkt końcowy `break` instrukcji nigdy nie jest dostępny.

### <a name="the-continue-statement"></a>Instrukcja kontynuowania

`continue` Instrukcji uruchamia nową iterację najbliższej otaczającej `while`, `do`, `for`, lub `foreach` instrukcji.

```antlr
continue_statement
    : 'continue' ';'
    ;
```

Celem `continue` instrukcja jest punkt końcowy osadzona instrukcja najbliższej otaczającej `while`, `do`, `for`, lub `foreach` instrukcji. Jeśli `continue` instrukcja nie jest ujęta w `while`, `do`, `for`, lub `foreach` występuje błąd kompilacji instrukcji.

Gdy wiele `while`, `do`, `for`, lub `foreach` instrukcje są zagnieżdżone w obrębie siebie nawzajem `continue` poufności informacji odnoszą się tylko do deklaracji najbardziej. Aby przekazać sterowanie na wielu poziomach zagnieżdżenia, `goto` — instrukcja ([instrukcji goto](statements.md#the-goto-statement)) musi być używana.

A `continue` instrukcja nie może zamknąć `finally` bloku ([instrukcjami "try"](statements.md#the-try-statement)). Gdy `continue` występuje instrukcja w obrębie `finally` zablokować, obiektu docelowego `continue` instrukcja musi być w tej samej `finally` zablokować; w przeciwnym razie wystąpi błąd kompilacji.

A `continue` instrukcja jest wykonywana w następujący sposób:

*  Jeśli `continue` instrukcja kończy działanie co najmniej jeden `try` skojarzone bloki `finally` bloków, kontrola początkowo jest przekazywany do `finally` bloku najbardziej wewnętrzną funkcją `try` instrukcji. Gdy, a kontrola osiąga punkt końcowy `finally` bloku, formant jest przekazywany do `finally` dalej otaczający blok `try` instrukcji. Ten proces jest powtarzany do momentu `finally` bloki wszystkich pośredniczących `try` instrukcje zostały wykonane.
*  Kontrola jest przekazywana do lokalizacji docelowej `continue` instrukcji.

Ponieważ `continue` instrukcji bezwarunkowo transfery kontrolować w innym miejscu, punkt końcowy `continue` instrukcji nigdy nie jest dostępny.

### <a name="the-goto-statement"></a>Goto — instrukcja

`goto` Instrukcji przekazuje sterowanie do instrukcji, która jest oznaczona przez etykietę.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

Celem `goto` *identyfikator* instrukcja to instrukcja labeled od podanej etykiety. Jeśli etykiety o podanej nazwie nie istnieje w bieżącym funkcja składowa lub `goto` instrukcja nie jest w zakresie etykiety, wystąpi błąd kompilacji. Ta reguła zezwala na używanie `goto` instrukcję, aby przekazać sterowanie poza zakres zagnieżdżonych, ale nie do zagnieżdżony zakres. W przykładzie
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
`goto` instrukcja jest używane do przekazywania kontroli zagnieżdżony zakres.

Celem `goto case` instrukcja jest listy instrukcji w bezpośrednio otaczającej `switch` instrukcji ([instrukcji switch](statements.md#the-switch-statement)), który zawiera `case` etykietę z danej wartości stałej. Jeśli `goto case` instrukcja nie jest ujęta w `switch` instrukcji, jeśli *constant_expression* jest niejawnie konwertowany ([niejawne konwersje](conversions.md#implicit-conversions)) do zarządzania typu najbliższej otaczającej `switch` instrukcji, lub jeśli najbliższej otaczającej `switch` nie zawiera instrukcji `case` etykiety z danej wartości stałych, wystąpi błąd kompilacji.

Celem `goto default` instrukcja jest listy instrukcji w bezpośrednio otaczającej `switch` — instrukcja ([instrukcji switch](statements.md#the-switch-statement)), który zawiera `default` etykiety. Jeśli `goto default` instrukcja nie jest ujęta w `switch` instrukcji, lub jeśli najbliższej otaczającej `switch` nie zawiera instrukcji `default` etykiety, wystąpi błąd kompilacji.

A `goto` instrukcja nie może zamknąć `finally` bloku ([instrukcjami "try"](statements.md#the-try-statement)). Gdy `goto` występuje instrukcja w obrębie `finally` zablokować, obiekt docelowy `goto` instrukcja musi być w tej samej `finally` bloku, lub w przeciwnym razie wystąpi błąd kompilacji.

A `goto` instrukcja jest wykonywana w następujący sposób:

*  Jeśli `goto` instrukcja kończy działanie co najmniej jeden `try` skojarzone bloki `finally` bloków, kontrola początkowo jest przekazywany do `finally` bloku najbardziej wewnętrzną funkcją `try` instrukcji. Gdy, a kontrola osiąga punkt końcowy `finally` bloku, formant jest przekazywany do `finally` dalej otaczający blok `try` instrukcji. Ten proces jest powtarzany do momentu `finally` bloki wszystkich pośredniczących `try` instrukcje zostały wykonane.
*  Kontrola jest przekazywana do lokalizacji docelowej `goto` instrukcji.

Ponieważ `goto` instrukcji bezwarunkowo transfery kontrolować w innym miejscu, punkt końcowy `goto` instrukcji nigdy nie jest dostępny.

### <a name="the-return-statement"></a>Instrukcja return

`return` Instrukcja zwraca kontrolę do bieżącego obiektu wywołującego funkcji, w którym `return` występuje instrukcja.

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

A `return` instrukcji za pomocą wyrażenia nie można użyć tylko w funkcji elementu członkowskiego, który nie może obliczyć wartość, czyli metody typu wyniku ([treści metody](classes.md#method-body)) `void`, `set` metody dostępu właściwości lub indeksator, `add` i `remove` Akcesory zdarzenie, Konstruktor wystąpienia, statyczny Konstruktor lub destruktor.

A `return` instrukcji za pomocą wyrażenia należy używać tylko w funkcji elementu członkowskiego, który oblicza wartość, czyli metody z typem innym niż void wynik `get` metody dostępu właściwości lub indeksatora lub operatora zdefiniowanego przez użytkownika. Niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) musi istnieć z typu wyrażenia do zwracanego typu zawierającego element członkowski funkcji.

Zwróć instrukcji można używać w treści wyrażenia funkcja anonimowa ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)) i uczestniczyć w określeniu, jakie konwersje istnieje dla tych funkcji.

Jest to błąd czasu kompilacji dla `return` instrukcji, które będą wyświetlane na `finally` bloku ([instrukcjami "try"](statements.md#the-try-statement)).

A `return` instrukcja jest wykonywana w następujący sposób:

*  Jeśli `return` Instrukcja określa wyrażenie, wyrażenie jest obliczane i wynikową wartość jest konwertowana na typ zwracany funkcji zawierającego przez niejawną konwersję. Wynik konwersji staje się wartości wyniku utworzonej przez funkcję.
*  Jeśli `return` instrukcji jest ujęta w co najmniej jeden `try` lub `catch` skojarzone bloki `finally` bloków, kontrola początkowo jest przekazywany do `finally` bloku najbardziej wewnętrzną funkcją `try` instrukcji. Gdy, a kontrola osiąga punkt końcowy `finally` bloku, formant jest przekazywany do `finally` dalej otaczający blok `try` instrukcji. Ten proces jest powtarzany do momentu `finally` blokuje wszystkie otaczającej `try` instrukcje zostały wykonane.
*  Jeśli zawierającego funkcja nie jest funkcji asynchronicznej, zwróceniem sterowania obiektowi wywołującemu zawierającego funkcji wraz z wartość wyniku ewentualne.
*  Jeśli funkcja zawierającego funkcji asynchronicznej, zwróceniem sterowania do bieżącego obiektu wywołującego, a wartość wyniku, jest rejestrowana w zadaniu zwracanym zgodnie z opisem w ([interfejsy modułu wyliczającego](classes.md#enumerator-interfaces)).

Ponieważ `return` instrukcji bezwarunkowo transfery kontrolować w innym miejscu, punkt końcowy `return` instrukcji nigdy nie jest dostępny.

### <a name="the-throw-statement"></a>Instrukcji "throw"

`throw` Instrukcji zgłasza wyjątek.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

A `throw` instrukcji za pomocą wyrażenia zgłasza wartość utworzone przez obliczenie wyrażenia. Wyrażenie musi oznaczają wartości o typie danej klasy `System.Exception`, typu klasy, która pochodzi od klasy `System.Exception` lub typu parametru typu, który ma `System.Exception` (lub jego podklasę) jako swojej klasy bazowej skuteczne. Jeśli generuje obliczania wyrażenia `null`, `System.NullReferenceException` jest generowany, zamiast tego.

A `throw` instrukcji za pomocą wyrażenia nie można użyć tylko w `catch` zablokować, w którym to przypadku tej instrukcji ponownie zgłasza wyjątek, który jest aktualnie obsługiwany przez który `catch` bloku.

Ponieważ `throw` instrukcji bezwarunkowo transfery kontrolować w innym miejscu, punkt końcowy `throw` instrukcji nigdy nie jest dostępny.

Gdy wyjątek jest zgłaszany, kontrola jest przekazywana do pierwszego `catch` w otaczającej klauzuli `try` instrukcję, która może obsłużyć wyjątek. Proces, który ma miejsce, od punktu rzuceniem wyjątku do punktu transferowanie formantu do obsługi odpowiednich wyjątków jest znany jako ***propagacji wyjątku***. Propagacji wyjątku, który składa się z wielokrotnie oceny następujące kroki, dopóki `catch` klauzula, która odpowiada wyjątek zostanie znaleziony. W tym opisie ***throw punktu*** początkowo jest lokalizacja, w którym wyjątek jest zgłaszany.

*  W bieżącym elemencie członkowskim funkcji każdego `try` instrukcję, która zawiera punkt throw jest sprawdzany pod. Dla każdej instrukcji `S`, począwszy od najbardziej wewnętrzną funkcją `try` instrukcji i kończącą najbardziej zewnętrzna `try` instrukcji, są sprawdzane następujące czynności:

   * Jeśli `try` bloku `S` otacza punktu throw i jeśli S ma co najmniej jeden `catch` klauzule `catch` klauzule są badane w kolejności występowania, aby zlokalizować odpowiedni program obsługi wyjątku, zgodnie z regułami określonymi w Sekcja [instrukcjami "try"](statements.md#the-try-statement). Jeśli pasujący obiekt typu `catch` znajduje się klauzuli, propagacji wyjątku zostało zakończone przez przeniesienie kontroli do bloku, który `catch` klauzuli.

   * W przeciwnym razie, jeśli `try` bloku lub `catch` bloku `S` otacza punktu throw i, jeśli `S` ma `finally` bloku, formant jest przekazywany do `finally` bloku. Jeśli `finally` inny wyjątek bloku, zakończeniem przetwarzania bieżącego wyjątku. W przeciwnym razie, gdy kontrola osiąga punkt końcowy `finally` bloku, przetwarzanie bieżącego wyjątku jest kontynuowane.

*  Jeśli nie znajdują się w na bieżącego wywołania funkcji obsługi wyjątków, wywołania funkcji zostanie zakończony i wystąpi jedno z następujących czynności:

   * Jeśli bieżącą funkcję jest inne niż async, powyższe kroki powtarzają się do obiektu wywołującego funkcji z punktem throw odpowiadający instrukcji, z którego została wywołana funkcja składowa.

   * W przypadku bieżącej funkcji asynchronicznych i zwracającą zadanie, wyjątek jest rejestrowana w zadaniu zwracanym przechodzi w stan uszkodzoną lub anulowane zgodnie z opisem w [interfejsy modułu wyliczającego](classes.md#enumerator-interfaces).

   * W przypadku bieżącej funkcji asynchronicznych i zwracającą typ void, kontekst synchronizacji bieżącego wątku jest powiadamiany, zgodnie z opisem w [Wyliczalne interfejsy](classes.md#enumerable-interfaces).

*  Przetwarzanie wyjątków zakończy wszystkie wywołania funkcji elementu członkowskiego, w bieżącym wątku, wskazującą, czy wątek żadna procedura obsługi wyjątku, następnie wątek jest zakończony. Wpływ stracą jest zdefiniowane w implementacji.

## <a name="the-try-statement"></a>Instrukcjami "try"

`try` Instrukcji udostępnia mechanizm przechwytywanie wyjątków, które występują podczas wykonywania bloku. Ponadto `try` instrukcja oferuje możliwość określenia bloku kodu, który jest zawsze wykonywane, kiedy formant opuszcza `try` instrukcji.

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

*  A `try` bloku, po której następuje co najmniej jeden `catch` bloków.
*  A `try` bloku następuje `finally` bloku.
*  A `try` bloku, po której następuje co najmniej jeden `catch` następuje bloki `finally` bloku.

Podczas `catch` klauzula Określa *exception_specifier*, musi być typu `System.Exception`, typ, który pochodzi od klasy `System.Exception` lub parametr typu, który ma `System.Exception` (lub jego podklasę) jako jego obowiązujące od Klasa bazowa.

Gdy `catch` klauzula określa zarówno *exception_specifier* z *identyfikator*, ***zmiennej wyjątku*** o podanej nazwie i typie jest zadeklarowana. Zmienna wyjątek odnosi się do zmiennej lokalnej z zakresem, który rozciąga `catch` klauzuli. Podczas wykonywania *exception_filter* i *bloku*, zmienna wyjątek reprezentuje aktualnie obsługiwany wyjątek. Do celów sprawdzania asercję określonego przypisania zmiennej wyjątek jest uznawany za zdecydowanie przypisana, aby w swoim zakresie całego.

Chyba że `catch` klauzula zawiera nazwa zmiennej wyjątku, nie można uzyskać dostępu do obiektu wyjątku w filtrze i `catch` bloku.

A `catch` klauzula, która nie określa *exception_specifier* nosi nazwę ogólnego `catch` klauzuli.

Niektóre języki programowania może obsługiwać wyjątki, które nie są stałego, ponieważ obiekt jest pochodną `System.Exception`, mimo że takie wyjątki nigdy nie można wygenerować przez kod języka C#. Ogólny `catch` klauzuli mogą służyć do przechwytywania wyjątków. W efekcie ogólnego `catch` klauzuli semantycznie różni się od jednego, który określa typ `System.Exception`, w tym, że pierwsza również może przechwytywać wyjątki od innych języków.

Aby zlokalizować program obsługi wyjątku, `catch` klauzule są badane w kolejności leksykalne. Jeśli `catch` klauzula Określa typ, ale żaden filtr wyjątku, jest to błąd czasu kompilacji do późniejszej `catch` klauzuli w tym samym `try` instrukcję, aby określić typ, który jest taki sam jak lub jest pochodną, wpisz. Jeśli `catch` klauzula określa Brak typu i żaden filtr, musi być ostatni `catch` klauzuli w tym `try` instrukcji.

W ramach `catch` bloku, `throw` — instrukcja ([instrukcji "throw"](statements.md#the-throw-statement)) za pomocą wyrażenia nie może służyć do ponownego zgłoszenia wyjątku, który został zgłoszony przez `catch` bloku. Przypisania do zmiennej wyjątku nie należy zmieniać wyjątek, który jest ponownie zgłoszony.

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
Metoda `F` wyłapuje wyjątek, zapisuje niektóre informacje diagnostyczne w konsoli, zmienia zmiennej wyjątku i ponownie zgłosi wyjątek. Wyjątek, który jest ponownie zgłoszony jest oryginalnego wyjątku, dzięki czemu dane wyjściowe, generowane są:
```
Exception in F: G
Exception in Main: G
```

Jeśli pierwszy blok catch miał zgłoszony `e` zamiast ponowne generowanie bieżący wyjątek, dane wyjściowe wytwarzane byłoby w następujący sposób:
```csharp
Exception in F: G
Exception in Main: F
```

Jest to błąd czasu kompilacji dla `break`, `continue`, lub `goto` instrukcję, aby przekazać sterowanie z `finally` bloku. Gdy `break`, `continue`, lub `goto` występuje instrukcja w `finally` bloku, obiekt docelowy instrukcji musi być w tej samej `finally` bloku, lub w przeciwnym razie wystąpi błąd kompilacji.

Jest to błąd czasu kompilacji dla `return` instrukcji w `finally` bloku.

A `try` instrukcja jest wykonywana w następujący sposób:

*  Kontrola jest przekazywana do `try` bloku.
*  Gdy, a kontrola osiąga punkt końcowy `try` bloku:
   *  Jeśli `try` instrukcja zawiera `finally` bloku `finally` blok jest wykonywany.
   *  Kontrola jest przekazywana do punktu końcowego `try` instrukcji.

*  Jeśli wyjątek jest propagowany do `try` instrukcji podczas wykonywania `try` bloku:
   *  `catch` Zdań, jeśli są sprawdzane w kolejności występowania, aby zlokalizować odpowiedni program obsługi wyjątku. Jeśli `catch` klauzuli nie określa typu lub określa typ wyjątku lub typ podstawowy typu wyjątku:
      *  Jeśli `catch` klauzuli deklaruje zmienną wyjątek, obiekt wyjątku jest przypisywana do zmiennej wyjątku.
      *  Jeśli `catch` klauzuli deklaruje filtra wyjątku, filtr jest oceniany. Jeśli daje w wyniku `false`klauzuli "catch" nie ma dopasowania i wyszukiwanie jest kontynuowane za pośrednictwem dowolnych kolejnych `catch` klauzule obsługi odpowiednie.
      *  W przeciwnym razie `catch` klauzula jest uznawane za zgodne i kontrola jest przekazywana do dopasowywania `catch` bloku.
      *  Gdy, a kontrola osiąga punkt końcowy `catch` bloku:
         * Jeśli `try` instrukcja zawiera `finally` bloku `finally` blok jest wykonywany.
         * Kontrola jest przekazywana do punktu końcowego `try` instrukcji.
      *  Jeśli wyjątek jest propagowany do `try` instrukcji podczas wykonywania `catch` bloku:
         *  Jeśli `try` instrukcja zawiera `finally` bloku `finally` blok jest wykonywany.
         *  Wyjątek jest propagowany do następnego otaczający `try` instrukcji.
   *  Jeśli `try` nie ma instrukcji `catch` klauzule lub jeśli nie `catch` klauzuli odpowiada wyjątek:
      *  Jeśli `try` instrukcja zawiera `finally` bloku `finally` blok jest wykonywany.
      *  Wyjątek jest propagowany do następnego otaczający `try` instrukcji.

Instrukcje `finally` bloku są wykonywane zawsze, kiedy formant opuszcza `try` instrukcji. Jest to istotne, czy transfer kontroli występuje w wyniku normalnego wykonania, w wyniku wykonania `break`, `continue`, `goto`, lub `return` instrukcji, lub w wyniku propagowanie wyjątku z `try` Instrukcja.

Jeśli wyjątek zostanie zgłoszony podczas wykonywania `finally` zablokować, a nie przechwycono w tym samym bloku finally, wyjątek jest propagowany do następnego otaczający `try` instrukcji. Jeśli inny wyjątek w trakcie propagowanie, ten wyjątek zostanie utracony. Proces propagowanie wyjątku jest omówiona w opisie `throw` — instrukcja ([instrukcji "throw"](statements.md#the-throw-statement)).

`try` Bloku `try` instrukcja jest osiągalny Jeśli `try` instrukcji jest nieosiągalny.

A `catch` bloku `try` instrukcja jest osiągalny Jeśli `try` instrukcji jest nieosiągalny.

`finally` Bloku `try` instrukcja jest osiągalny Jeśli `try` instrukcji jest nieosiągalny.

Punkt końcowy `try` instrukcja jest osiągalna, jeśli są spełnione oba z następujących czynności:

*  Punkt końcowy `try` bloku jest dostępny, lub koniec punktu z co najmniej jeden `catch` bloku jest osiągalny.
*  Jeśli `finally` bloku jest obecny, punkt końcowy `finally` bloku jest osiągalny.

## <a name="the-checked-and-unchecked-statements"></a>Checked i unchecked instrukcji

`checked` i `unchecked` instrukcje służą do sterowania ***przepełnienia, sprawdzając kontekst*** typu całkowitego operacje arytmetyczne i konwersje.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

`checked` Instrukcji powoduje, że wszystkie wyrażenia w *bloku* mogło zostać ocenione w kontekście sprawdzanym i `unchecked` instrukcji powoduje, że wszystkie wyrażenia w *bloku* mogło zostać ocenione kontekst niezaznaczone.

`checked` i `unchecked` instrukcje są dokładnie odpowiednikiem `checked` i `unchecked` operatorów ([operatory checked i unchecked](expressions.md#the-checked-and-unchecked-operators)), z tą różnicą, że działają one na blokach zamiast wyrażenia .

## <a name="the-lock-statement"></a>Instrukcji "lock"

`lock` Instrukcji uzyskuje blokadę wykluczania wzajemnego dla danego obiektu, wykonuje instrukcję tak długo, a następnie zwalnia blokadę.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

Wyrażenie `lock` instrukcja musi oznaczają wartości typu, znane jako *reference_type*. Brak konwersji niejawnej konwersji boxing ([konwersje Boxing](conversions.md#boxing-conversions)) nigdy nie jest wykonywane dla wyrażenie `lock` instrukcji, a więc jest to błąd kompilacji, wyrażenia wskazać wartość *value_type*.

A `lock` instrukcji formularza
```csharp
lock (x) ...
```
gdzie `x` to wyrażenie *reference_type*, odpowiada dokładnie
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

Natomiast przechowywane blokadę wykluczania wzajemnego kodu wykonywanego w tym samym wątku wykonywania można również uzyskać i zwalnia blokadę. Jednak kodu wykonywanego w innych wątków jest zablokowany rezygnację z włączenia blokady dopóki blokada jest zwalniana.

Blokowanie `System.Type` obiektów w celu synchronizowania dostępu do danych statycznych nie jest zalecane. Inny kod może zostać zablokowany na tym samym typie, co może skutkować zakleszczenia. Lepszym rozwiązaniem jest do synchronizowania dostępu do danych statycznych, blokując prywatnego obiektu statycznego. Na przykład:
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

`using` Instrukcji uzyskuje co najmniej jeden zasób, wykonuje instrukcję, a następnie usuwa zasób.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

A ***zasobów*** jest klasa lub struktura, która implementuje `System.IDisposable`, która obejmuje pojedynczej metody bez parametrów o nazwie `Dispose`. Kod, który używa zasobu może wywołać `Dispose` do wskazania, że zasób nie jest już potrzebny. Jeśli `Dispose` nie jest wywoływana, a następnie automatyczne usuwanie ostatecznie występuje w wyniku wyrzucania elementów bezużytecznych.

Jeśli formie *resource_acquisition* jest *local_variable_declaration* następnie typ *local_variable_declaration* musi być albo `dynamic` lub typu który może być niejawnie konwertowane na `System.IDisposable`. Jeśli formie *resource_acquisition* jest *wyrażenie* , a następnie to wyrażenie musi umożliwiać niejawną konwersję `System.IDisposable`.

Zmienne lokalne są deklarowane w *resource_acquisition* są przeznaczone tylko do odczytu i może zawierać inicjatora. Występuje błąd kompilacji, jeśli osadzona instrukcja próbuje zmodyfikować te zmienne lokalne (za pośrednictwem przydziału lub `++` i `--` operatory), należy przyjąć adresu je lub przekazać je jako `ref` lub `out` parametrów.

A `using` instrukcji jest tłumaczony na trzy części: pozyskiwanie, użycia i usuwania. Użycie zasobów jest niejawnie ujęty w `try` instrukcję, która obejmuje `finally` klauzuli. To `finally` klauzuli Usuwa zasób. Jeśli `null` nabywa zasobów, następnie Brak wywołania `Dispose` jest wykonywane, i jest zgłaszany żaden wyjątek. Jeśli zasób jest typu `dynamic` konwersją dynamicznie za pośrednictwem niejawną konwersję dynamicznej ([niejawne konwersje dynamiczne](conversions.md#implicit-dynamic-conversions)) do `IDisposable` podczas pozyskiwania w celu zapewnienia, że konwersja została się powodzeniem przed rozpoczęciem użytkowania i usuwania.

A `using` instrukcji formularza
```csharp
using (ResourceType resource = expression) statement
```
odnosi się do jednej z trzech możliwych rozszerzeń. Gdy `ResourceType` jest typem wartości niedopuszczającym wartości jest rozszerzenie
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

W przeciwnym razie, gdy `ResourceType` jest typem wartościowym lub typem referencyjnym, inne niż `dynamic`, jest rozszerzenie
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

W przeciwnym razie, gdy `ResourceType` jest `dynamic`, jest rozszerzenie
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

W obu rozwinięciu `resource` zmienna jest tylko do odczytu w osadzonych instrukcji i `d` zmienna jest niedostępna z powodu i niewidoczności, aż do osadzona instrukcja.

Implementacja jest dozwolony do zaimplementowania danego przy użyciu instrukcji w różny sposób, np. ze względu na wydajność, tak długo, jak zachowanie jest zgodne z rozszerzeniem powyżej.

A `using` instrukcji formularza
```csharp
using (expression) statement
```
ma ten sam trzy możliwe rozszerzenia. W tym przypadku `ResourceType` jest niejawnie typ kompilacji `expression`, jeśli taki istnieje. W przeciwnym razie interfejsu `IDisposable` sam jest używany jako `ResourceType`. `resource` Zmienna jest niedostępna z powodu i niewidoczności, aż do osadzona instrukcja.

Gdy *resource_acquisition* mają postać *local_variable_declaration*, istnieje możliwość nabywanie wielu zasobów danego typu. A `using` instrukcji formularza
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
jest zagnieżdżona dokładnie równoważna sekwencji `using` instrukcji:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

Poniższy przykład tworzy plik o nazwie `log.txt` i zapisuje dwa wiersze tekstu do pliku. Przykład następnie spowoduje otwarcie tego samego pliku do odczytu i kopiuje zawartej wierszy tekstu do konsoli.
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

Ponieważ `TextWriter` i `TextReader` implementacji klasy `IDisposable` interfejsu, można użyć w przykładzie `using` instrukcje, aby upewnić się, że pliku podstawowego jest poprawnie zamknięte po zapisu lub operacje odczytu.

## <a name="the-yield-statement"></a>Instrukcja yield

`yield` Instrukcja jest używane w bloku iteratora ([bloki](statements.md#blocks)) umożliwiające uzyskanie wartości do obiektu modułu wyliczającego ([obiektów modułu wyliczającego](classes.md#enumerator-objects)) lub obiekt wyliczalny ([Wyliczalny obiektów](classes.md#enumerable-objects)) iterator lub w celu sygnalizowania, że końcem iteracji.

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield` nie jest zarezerwowanym słowem ma specjalne znaczenie tylko wtedy, gdy jest używana bezpośrednio przed `return` lub `break` — słowo kluczowe. W innych kontekstach `yield` można użyć jako identyfikatora.

Istnieje kilka ograniczeń o tym, gdzie `yield` instrukcji może znajdować się, jak opisano w następujących.

*  Jest to błąd czasu kompilacji dla `yield` instrukcji (Dowolna forma) można znajdować się poza *method_body*, *operator_body* lub *accessor_body*
*  Jest to błąd czasu kompilacji dla `yield` instrukcji (Dowolna forma) się wewnątrz funkcji anonimowych.
*  Jest to błąd czasu kompilacji dla `yield` instrukcji (Dowolna forma) pojawią się w `finally` klauzuli `try` instrukcji.
*  Jest to błąd czasu kompilacji dla `yield return` instrukcję, aby występować w dowolnym miejscu `try` instrukcji, która zawiera dowolne `catch` klauzul.

Poniższy przykład pokazuje niektóre prawidłowe i nieprawidłowego użycia `yield` instrukcji.

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

Niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) musi istnieć z typu wyrażenia w `yield return` instrukcję, aby typ yield ([uzyskanie typu](classes.md#yield-type)) iteratora.

A `yield return` instrukcja jest wykonywana w następujący sposób:

*  Wyrażenie w instrukcji jest obliczane, niejawnie konwertowany na typ produktywności i przypisane do `Current` właściwości obiektu modułu wyliczającego.
*  Wykonanie bloku iteratora jest zawieszone. Jeśli `yield return` Instrukcja znajduje się w co najmniej jeden `try` blokuje skojarzonego `finally` bloki nie są wykonywane w tej chwili.
*  `MoveNext` Metoda obiekt modułu wyliczającego zwraca `true` do obiektu wywołującego, wskazujący, że obiekt modułu wyliczającego pomyślnie zaawansowane do następnego elementu.

Następne wywołanie obiekt modułu wyliczającego `MoveNext` metoda wznawia wykonanie bloku iteratora, z którym ostatnio zostało wstrzymane.

A `yield break` instrukcja jest wykonywana w następujący sposób:

*  Jeśli `yield break` instrukcji jest ujęta w co najmniej jeden `try` skojarzone bloki `finally` bloków, kontrola początkowo jest przekazywany do `finally` bloku najbardziej wewnętrzną funkcją `try` instrukcji. Gdy, a kontrola osiąga punkt końcowy `finally` bloku, formant jest przekazywany do `finally` dalej otaczający blok `try` instrukcji. Ten proces jest powtarzany do momentu `finally` blokuje wszystkie otaczającej `try` instrukcje zostały wykonane.
*  Formant zostaje zwrócony do obiektu wywołującego, bloku iteratora. Jest to `MoveNext` metody lub `Dispose` metody obiektu modułu wyliczającego.

Ponieważ `yield break` instrukcji bezwarunkowo transfery kontrolować w innym miejscu, punkt końcowy `yield break` instrukcji nigdy nie jest dostępny.
