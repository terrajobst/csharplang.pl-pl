---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876800"
---
# <a name="variables"></a>Zmienne

Zmienne reprezentują lokalizacje magazynu. Każda zmienna ma typ, który określa, jakie wartości mogą być przechowywane w zmiennej. C#jest językiem bezpiecznym dla typu, a C# kompilator gwarantuje, że wartości przechowywane w zmiennych są zawsze odpowiednim typem. Wartość zmiennej można zmienić poprzez przypisanie lub użycie `++` operatorów i. `--`

Zmienna musi być ***ostatecznie przypisana*** ([przypisanie określone](variables.md#definite-assignment)), zanim będzie można uzyskać jej wartość.

Zgodnie z opisem w poniższych sekcjach zmienne są ***początkowo przypisywane*** lub ***początkowo***nieprzypisane. Początkowo przypisana zmienna ma dobrze zdefiniowaną wartość początkową i jest zawsze traktowana jako ostatecznie przypisana. Początkowo nieprzypisana zmienna nie ma wartości początkowej. W przypadku początkowo nieprzypisanej zmiennej, która ma zostać uznana za ostatecznie przypisaną w określonej lokalizacji, przypisanie do zmiennej musi nastąpić w każdej możliwej ścieżce wykonywania prowadzącej do tej lokalizacji.

## <a name="variable-categories"></a>Kategorie zmiennych

C#definiuje siedem kategorii zmiennych: zmiennych statycznych, zmiennych wystąpień, elementów tablicy, parametrów wartości, parametrów odwołania, parametrów wyjściowych i zmiennych lokalnych. W poniższych sekcjach opisano każdą z tych kategorii.

W przykładzie
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
`x`to zmienna statyczna, `y` czy zmienna wystąpienia, `v[0]` jest elementem tablicy, `a` jest parametrem `c` wartości, `b` jest parametrem referencyjnym, jest parametrem wyjściowym i `i` jest zmienną lokalną .

### <a name="static-variables"></a>Zmienne statyczne

Pole zadeklarowane za pomocą `static` modyfikatora nosi nazwę ***zmiennej statycznej***. Zmienna statyczna występuje przed wykonaniem konstruktora statycznego ([konstruktory statyczne](classes.md#static-constructors)) dla jego typu zawierającego i przestaje istnieć, gdy skojarzona domena aplikacji przestanie istnieć.

Początkowa wartość zmiennej statycznej jest wartością domyślną ([wartości domyślne](variables.md#default-values)) typu zmiennej.

W celach o określonym przeznaczeniu, zmienna statyczna jest traktowana jako początkowo przypisana.

### <a name="instance-variables"></a>Zmienne wystąpienia

Pole zadeklarowane bez `static` modyfikatora jest nazywane ***zmienną wystąpienia***.

#### <a name="instance-variables-in-classes"></a>Zmienne wystąpień w klasach

Zmienna wystąpienia klasy występuje, gdy jest tworzone nowe wystąpienie tej klasy i przestaje istnieć, gdy nie ma odwołań do tego wystąpienia i destruktora wystąpienia (jeśli istnieje).

Początkowa wartość zmiennej wystąpienia klasy jest wartością domyślną ([wartości domyślne](variables.md#default-values)) typu zmiennej.

Na potrzeby określonego sprawdzania przypisania zmienna wystąpienia klasy jest traktowana jako początkowo przypisana.

#### <a name="instance-variables-in-structs"></a>Zmienne wystąpienia w strukturach

Zmienna wystąpienia struktury ma dokładnie taki sam okres istnienia, jak zmienna struktury, do której należy. Innymi słowy, gdy zmienna typu struktury jest w stanie istnienia lub przestaje istnieć, więc należy wykonać te zmienne wystąpienia struktury.

Początkowy stan przypisania zmiennej wystąpienia struktury jest taki sam, jak w przypadku zawierającej ją zmiennej struktury. Innymi słowy, gdy zmienna struktury jest uznawana za wstępnie przypisaną, więc ich zmienne wystąpień i gdy zmienna struktury jest uznawana za początkowo nieprzypisane, jego zmienne wystąpienia są podobnie nieprzypisane.

### <a name="array-elements"></a>Elementy tablicy

Elementy tablicy są obecne w momencie utworzenia wystąpienia tablicy i przestaną istnieć, gdy nie ma odwołań do tego wystąpienia tablicy.

Wartość początkowa każdego z elementów tablicy jest wartością domyślną ([wartości domyślne](variables.md#default-values)) typu elementów tablicy.

Na potrzeby określonego sprawdzania przypisania element array jest traktowany jako wstępnie przypisany.

### <a name="value-parameters"></a>Parametry wartości

Parametr zadeklarowany bez `ref` modyfikatora `out` or jest ***parametrem wartości***.

Parametr value występuje w przypadku wywołania elementu członkowskiego funkcji (metody, konstruktora wystąpienia, metody dostępu lub operatora) lub funkcji anonimowej, do której należy parametr, i jest inicjowany przy użyciu wartości argumentu podanego w wywołaniu. Parametr wartości zwykle przestaje istnieć po zwrocie elementu członkowskiego funkcji lub funkcji anonimowej. Jeśli jednak parametr value jest przechwytywany przez funkcję anonimową ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)), jego czas życia rozciąga się co najmniej do momentu, aż obiekt delegowany lub drzewo wyrażenia utworzone na podstawie tej funkcji anonimowej będzie kwalifikować się do wyrzucania elementów bezużytecznych.

Na potrzeby określonego sprawdzania przypisania parametr value jest traktowany jako wstępnie przypisany.

### <a name="reference-parameters"></a>Parametry odwołania

Parametr zadeklarowany za pomocą `ref` modyfikatora jest ***parametrem referencyjnym***.

Parametr Reference nie tworzy nowej lokalizacji magazynu. Zamiast tego parametr odwołania reprezentuje tę samą lokalizację magazynu, co zmienna określona jako argument w elemencie członkowskim funkcji lub wywołaniu funkcji anonimowej. W ten sposób wartość parametru odwołania jest zawsze taka sama jak zmienna bazowa.

Następujące określone reguły przypisywania dotyczą parametrów referencyjnych. Zwróć uwagę na różne reguły parametrów wyjściowych opisanych w [parametrach wyjściowych](variables.md#output-parameters).

*  Zmienna musi być ostatecznie przypisana ([przypisanie](variables.md#definite-assignment)), zanim będzie mogła zostać przeniesiona jako parametr odwołania w składowej funkcji lub delegatze wywołania.
*  W składowej funkcji lub funkcji anonimowej jest uznawany za początkowo przypisany parametr Reference.

W ramach metody wystąpienia lub akcesora wystąpienia typu `this` struktury słowo kluczowe zachowuje się dokładnie jako parametr Reference typu struktury ([ten dostęp](expressions.md#this-access)).

### <a name="output-parameters"></a>Parametry wyjściowe

Parametr zadeklarowany za pomocą `out` modyfikatora jest ***parametrem wyjściowym***.

Parametr wyjściowy nie tworzy nowej lokalizacji magazynu. Zamiast tego parametr wyjściowy reprezentuje tę samą lokalizację magazynu, co zmienna określona jako argument elementu członkowskiego funkcji lub delegata wywołania. W ten sposób wartość parametru wyjściowego jest zawsze taka sama jak zmienna bazowa.

Następujące określone reguły przypisywania mają zastosowanie do parametrów wyjściowych. Zwróć uwagę na różne reguły parametrów referencyjnych opisanych w [parametrach referencyjnych](variables.md#reference-parameters).

*  Zmienna nie musi być ostatecznie przypisana, zanim będzie mogła zostać przeniesiona jako parametr wyjściowy w składowej funkcji lub w wywołaniu obiektu delegowanego.
*  Po normalnym zakończeniu elementu członkowskiego funkcji lub delegatze wywołania Każda zmienna, która została przeniesiona jako parametr wyjściowy, jest uznawana za przypisaną w tej ścieżce wykonywania.
*  W ramach składowej funkcji lub funkcji anonimowej, parametr wyjściowy jest uznawany za początkowo nieprzypisane.
*  Każdy parametr wyjściowy elementu członkowskiego funkcji lub funkcji anonimowej musi być ostatecznie przypisany ([przypisanie określone](variables.md#definite-assignment)) zanim element członkowski funkcji lub funkcja anonimowa normalnie zwróci wynik.

W konstruktorze wystąpienia typu `this` struktury słowo kluczowe zachowuje się dokładnie jako parametr wyjściowy typu struktury ([dostęp](expressions.md#this-access)).

### <a name="local-variables"></a>Zmienne lokalne

***Zmienna lokalna*** jest zadeklarowana przez element *local_variable_declaration*, który może wystąpić w *bloku*, *for_statement*, *switch_statement* lub *using_statement*; lub *foreach_statement* lub *specific_catch_clause* dla *try_statement*.

Okres istnienia zmiennej lokalnej jest częścią wykonywania programu, podczas której zagwarantowano, że magazyn jest zarezerwowany dla tego obiektu. Ten okres istnienia rozszerza co najmniej od wpisu do *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*lub *specific_catch_clause* , z którym jest skojarzony, do wykonywanie tego *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*lub *specific_catch_clause* kończą się w dowolny sposób. (Wprowadzenie do zamkniętego *bloku* lub wywołanie metody zawiesza się, ale nie kończy wykonywania bieżącego *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*lub *specific_ catch_clause*.) Jeśli zmienna lokalna jest przechwytywana przez funkcję anonimową ([przechwycone zmienne zewnętrzne](expressions.md#captured-outer-variables)), jego okres istnienia rozszerza co najmniej do momentu delegata lub drzewa wyrażenia utworzonego na podstawie funkcji anonimowej, wraz z innymi obiektami, które odwołują się do przechwycona zmienna kwalifikuje się do wyrzucania elementów bezużytecznych.

Jeśli *blok*nadrzędny, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*lub *specific_catch_clause* jest wprowadzany rekursywnie, tworzone jest nowe wystąpienie zmiennej lokalnej czas i jego *local_variable_initializer*, jeśli istnieją, są oceniane za każdym razem.

Zmienna lokalna wprowadzona przez *local_variable_declaration* nie jest inicjowana automatycznie i w związku z tym nie ma wartości domyślnej. Na potrzeby określonego sprawdzania przypisania, zmienna lokalna wprowadzona przez *local_variable_declaration* jest uznawana za początkowo nieprzypisane. *Local_variable_declaration* może zawierać *local_variable_initializer*, w tym przypadku zmienna jest uważana za ostatecznie przypisaną tylko po wyrażeniu inicjowania ([instrukcje deklaracji](variables.md#declaration-statements)).

W zakresie zmiennej lokalnej wprowadzonej przez *local_variable_declaration*jest to błąd czasu kompilacji, który odwołuje się do tej zmiennej lokalnej w pozycji tekstowej, która poprzedza jej *local_variable_declarator*. Jeśli deklaracja zmiennej lokalnej jest niejawna ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)), jest również błędem do odwoływania się do zmiennej w ramach jej *local_variable_declarator*.

Zmienna lokalna wprowadzona przez *foreach_statement* lub *specific_catch_clause* jest uznawana za ostatecznie przypisaną w całym zakresie.

Rzeczywisty okres istnienia zmiennej lokalnej jest zależny od implementacji. Na przykład kompilator może statycznie określić, że zmienna lokalna w bloku jest używana tylko dla małej części tego bloku. Korzystając z tej analizy, kompilator może wygenerować kod, który powoduje, że magazyn zmiennej ma krótszy okres istnienia niż jego blok zawierający.

Magazyn, do którego odwołuje się lokalna zmienna referencyjna, jest odzyskiwana niezależnie od okresu istnienia lokalnej zmiennej odwołania ([Automatyczne zarządzanie pamięcią](basic-concepts.md#automatic-memory-management)).

## <a name="default-values"></a>Wartości domyślne

Następujące kategorie zmiennych są automatycznie inicjowane na wartości domyślne:

*  Zmienne statyczne.
*  Zmienne wystąpienia klasy Instances.
*  Elementy tablicy.

Wartość domyślna zmiennej zależy od typu zmiennej i jest określana w następujący sposób:

*  Dla zmiennej *value_type*wartość domyślna jest taka sama jak wartość obliczana przez konstruktora domyślnego *value_type*([konstruktory domyślne](types.md#default-constructors)).
*  Dla zmiennej *reference_type*wartość domyślna to `null`.

Inicjowanie do wartości domyślnych jest zwykle wykonywane przez zainicjowanie pamięci przez Menedżera pamięci lub moduł wyrzucania elementów bezużytecznych do wszystkich-bitów-zero przed przydzieleniem do użycia. Z tego powodu wygodnie jest użyć wszystkich-bitów-zero do reprezentowania odwołania o wartości null.

## <a name="definite-assignment"></a>Przypisanie określone

W danej lokalizacji kodu wykonywalnego elementu członkowskiego, zmienna jest określana jako ***czasowo przypisana*** , jeśli kompilator może udowodnić, przez określoną analityczny przepływ statyczny ([precyzyjne reguły określania określonego przypisania](variables.md#precise-rules-for-determining-definite-assignment)), że zmienna Program został zainicjowany automatycznie lub miał miejsce docelowe co najmniej jedno przypisanie. Nieformalnie określone reguły przypisywania są następujące:

*  Początkowo przypisana zmienna ([wstępnie przypisane zmienne](variables.md#initially-assigned-variables)) jest zawsze uznawana za ostatecznie przypisaną.
*  Początkowo nieprzypisane zmienne (początkowo nieprzypisane[zmienne](variables.md#initially-unassigned-variables)) są uznawane za ostatecznie przypisane w danej lokalizacji, jeśli wszystkie możliwe ścieżki wykonywania prowadzące do tej lokalizacji zawierają co najmniej jedną z następujących czynności:
    * Proste przypisanie ([przypisanie proste](expressions.md#simple-assignment)), w którym zmienna jest argumentem lewego operandu.
    * Wyrażenie wywołania (wyrażenia[wywołania](expressions.md#invocation-expressions)) lub wyrażenie tworzenia obiektu ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)), które przekazuje zmienną jako parametr wyjściowy.
    * Dla zmiennej lokalnej deklaracja zmiennej lokalnej ([lokalnych deklaracji zmiennych](statements.md#local-variable-declarations)), która zawiera inicjator zmiennych.

Formalna Specyfikacja zgodna z powyższymi regułami nieformalnymi jest opisana w [wstępnie przypisanych zmiennych](variables.md#initially-assigned-variables), [początkowo nieprzypisane zmienne](variables.md#initially-unassigned-variables)i [precyzyjne reguły określania przypisania](variables.md#precise-rules-for-determining-definite-assignment).

Określone Stany przypisania zmiennych wystąpienia zmiennej *struct_type* są śledzone indywidualnie, a także w sposób zbiorczy. Oprócz powyższych reguł stosowane są następujące reguły do zmiennych *struct_type* i ich zmiennych wystąpień:

*  Zmienna wystąpienia jest uznawana za ostatecznie przypisaną, jeśli jej zawierająca zmienną *struct_type* jest uznawana za ostatecznie przypisaną.
*  Zmienna *struct_type* jest uznawana za ostatecznie przypisaną, jeśli każda z jej zmiennych wystąpienia jest uznawana za ostatecznie przypisaną.

Określone przypisanie jest wymaganiem w następujących kontekstach:

*  Zmienna musi być ostatecznie przypisana w każdej lokalizacji, w której zostanie uzyskana wartość. Gwarantuje to, że niezdefiniowane wartości nigdy nie wystąpią. Wystąpienie zmiennej w wyrażeniu jest traktowane do uzyskania wartości zmiennej, z wyjątkiem przypadków, gdy
    * Zmienna jest lewym operandem prostego przypisywania,
    * Zmienna jest przenoszona jako parametr wyjściowy lub
    * Zmienna jest zmienną *struct_type* i występuje jako lewy operand dostępu do elementu członkowskiego.
*  Zmienna musi być ostatecznie przypisana w każdej lokalizacji, w której jest przenoszona jako parametr referencyjny. Daje to pewność, że wywoływany element członkowski funkcji może uwzględniać początkowo przypisany parametr Reference.
*  Wszystkie parametry wyjściowe elementu członkowskiego funkcji muszą być ostatecznie przypisane do każdej lokalizacji, w której element członkowski funkcji zwraca ( `return` za pomocą instrukcji lub przez wykonanie do końca treści elementu członkowskiego funkcji). Gwarantuje to, że elementy członkowskie funkcji nie zwracają niezdefiniowanych wartości w parametrach wyjściowych, dzięki czemu kompilator powinien wziąć pod uwagę wywołanie elementu członkowskiego, który pobiera zmienną jako parametr wyjściowy równoważne przypisanie do zmiennej.
*  Zmienna konstruktora wystąpienia struct_type musi być ostatecznie przypisana w każdej lokalizacji, w której Konstruktor wystąpienia zwraca. `this`

### <a name="initially-assigned-variables"></a>Wstępnie przypisane zmienne

Następujące kategorie zmiennych są klasyfikowane jako początkowo przypisane:

*  Zmienne statyczne.
*  Zmienne wystąpienia klasy Instances.
*  Zmienne wystąpienia wstępnie przypisanych zmiennych struktury.
*  Elementy tablicy.
*  Parametry wartości.
*  Parametry odwołania.
*  Zmienne zadeklarowane w `catch` klauzuli `foreach` lub instrukcji.

### <a name="initially-unassigned-variables"></a>Początkowo nieprzypisane zmienne

Następujące kategorie zmiennych są klasyfikowane jako początkowo nieprzypisane:

*  Zmienne wystąpienia początkowo nieprzypisane zmienne struktury.
*  Parametry wyjściowe, w tym `this` zmienna konstruktorów wystąpień struktury.
*  Zmienne lokalne, z wyjątkiem tych zadeklarowanych `catch` w klauzuli `foreach` lub instrukcji.

### <a name="precise-rules-for-determining-definite-assignment"></a>Precyzyjne reguły określania określonego przypisania

Aby określić, że każda użyta zmienna jest ostatecznie przypisana, kompilator musi użyć procesu, który jest równoważny z opisanym w tej sekcji.

Kompilator przetwarza treść każdego elementu członkowskiego funkcji, który ma co najmniej jedną wstępnie przypisaną zmienną. Dla każdej początkowo nieprzypisanej zmiennej *v*kompilator określa ***określony stan przypisania*** dla *v* w każdym z następujących punktów w składowej funkcji:

*  Na początku każdej instrukcji
*  W punkcie końcowym ([punkty końcowe i osiągalność](statements.md#end-points-and-reachability)) każdej instrukcji
*  Na każdym łuku, który przenosi formant do innej instrukcji lub do punktu końcowego instrukcji
*  Na początku każdego wyrażenia
*  Na końcu każdego wyrażenia

Określony stan przypisania *v* może być:

*  W nieskończony sposób. Oznacza *to, że* dla wszystkich możliwych przepływów sterowania do tego punktu przypisano wartość.
*  Nie jest on ostatecznie przypisany. W przypadku stanu zmiennej na końcu wyrażenia typu `bool`, stan zmiennej, która nie jest ostatecznie przypisany, może (ale niekoniecznie) wchodzić do jednego z następujących podstanów:
    * W nieskończony sposób przypisany po prawdziwe wyrażenie. Ten stan wskazuje, że wartość *v* jest ostatecznie przypisana, jeśli wyrażenie logiczne jest oceniane jako prawdziwe, ale nie musi być przypisywane, jeśli wyrażenie logiczne zostało ocenione jako FAŁSZ.
    * Jest on przypisany w nieskończoność po wyrażeniu false. Ten stan wskazuje, że w przypadku wyrażenia logicznego ocenianego jako FAŁSZ jest przypisywana wartość " *v* ", ale nie jest to konieczne, jeśli wyrażenie logiczne jest oceniane jako true.

Poniższe reguły określają, jak stan zmiennej *v* jest określany w każdej lokalizacji.

#### <a name="general-rules-for-statements"></a>Ogólne reguły dla instrukcji

*  wartość *v* nie jest ostatecznie przypisana na początku treści elementu członkowskiego funkcji.
*  *v* jest ostatecznie przypisana na początku dowolnej nieosiągalnej instrukcji.
*  Określony stan przypisania *v* na początku każdej innej instrukcji jest określany przez sprawdzenie, czy stan przypisania jest określony *, na wszystkich* transferach przepływów sterowania przeznaczonych dla początku tej instrukcji. Jeśli (i tylko wtedy, gdy) *v* jest ostatecznie przypisany do wszystkich takich transferów przepływów sterowania, a następnie *v* jest ostatecznie przypisana na początku instrukcji. Zestaw możliwych transferów przepływów sterowania jest określany w taki sam sposób, jak sprawdzanie osiągalności instrukcji ([punkty końcowe i osiągalność](statements.md#end-points-and-reachability)).
*  Określony stan przypisania *v* w punkcie `checked`końcowym bloku, `do`, `while` `if` `unchecked` `for` ,,`foreach`,,,,, lub `lock` `using` `switch`instrukcja jest określana przez sprawdzenie, czy stan przypisania jest określony *, na wszystkich* transferach przepływów sterowania przeznaczonych dla punktu końcowego tej instrukcji. Jeśli wartość *v* jest ostatecznie przypisana do wszystkich takich transferów przepływów sterowania, to *v* jest ostatecznie przypisana w punkcie końcowym instrukcji. Przypadku wartość *v* nie jest ostatecznie przypisana w punkcie końcowym instrukcji. Zestaw możliwych transferów przepływów sterowania jest określany w taki sam sposób, jak sprawdzanie osiągalności instrukcji ([punkty końcowe i osiągalność](statements.md#end-points-and-reachability)).

#### <a name="block-statements-checked-and-unchecked-statements"></a>Blokuj instrukcje, sprawdzone i niesprawdzone instrukcje

Określony stan przypisania *v* na formancie transfer do pierwszej instrukcji z listy instrukcji w bloku (lub do punktu końcowego bloku, jeśli lista instrukcji jest pusta) jest taka sama jak w przypadku instrukcji przypisania " *v* " przed blokiem , `checked`, lub `unchecked` .

#### <a name="expression-statements"></a>Instrukcje wyrażeń

Dla instrukcji wyrażenia *stmt* , która składa się z *wyrażenia expression:*

*  *v* ma ten sam określony stan przypisania na początku *wyrażenia* , jak na początku *stmt*.
*  Jeśli wartość *v* jest ostatecznie przypisana na końcu *wyrażenia*, jest on ostatecznie przypisywany w punkcie końcowym *stmt*; przypadku nie jest on ostatecznie przypisywany w punkcie końcowym *stmt*.

#### <a name="declaration-statements"></a>Deklaracje deklaracji

*  Jeśli *stmt* jest instrukcją deklaracji bez inicjatorów, wówczas *v* ma ten sam, określony stan przypisania w punkcie końcowym *stmt* , jak na początku *stmt*.
*  Jeśli *stmt* jest instrukcją deklaracji z inicjatorami, oznacza to, że określony stan przypisania dla *v* jest określany tak, jakby *stmt* były listą instrukcji z jedną instrukcją przypisania dla każdej deklaracji z inicjatorem (w kolejności Deklaracja).

#### <a name="if-statements"></a>If — instrukcje

Dla instrukcji stmt formularza: `if`
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v* ma ten sam określony stan przypisania na początku *wyrażenia* , jak na początku *stmt*.
*  Jeśli wartość *v* jest ostatecznie przypisana na końcu *wyrażenia*, to jest on ostatecznie przypisany do transferu przepływu sterowania do *then_stmt* i do *else_stmt* lub do punktu końcowego *stmt* , jeśli nie ma klauzuli else.
*  Jeśli funkcja *v* ma stan "czas przypisania po prawdziwym wyrażeniu" na końcu wyrażenia *, to*jest on ostatecznie przypisany do transferu przepływu sterowania do *then_stmt*i nie jest on ostatecznie przypisywany w przepływie sterowania do *else_ stmt* lub do punktu końcowego *stmt* , jeśli nie ma klauzuli else.
*  Jeśli w wersji *v* znajduje się stan "czas przypisania po wartości false" na końcu *wyrażenia, to*jest on ostatecznie przypisany do transferu przepływu sterowania do *else_stmt*i nie jest on ostatecznie przypisywany w przepływie sterowania do *then_stmt* . Jest on ostatecznie przypisywany w punkcie końcowym *stmt* , jeśli i tylko wtedy, gdy jest on ostatecznie przypisywany w punkcie końcowym *then_stmt*.
*  W przeciwnym razie *v* jest uznawana za nieczasowo przypisane w przepływie sterowania do *then_stmt* lub *else_stmt*lub do punktu końcowego *stmt* , jeśli nie istnieje klauzula else.

#### <a name="switch-statements"></a>Instrukcji switch

W instrukcji stmt *z wyrażeniem kontrolnym:* `switch`

*  Określony stan przypisania *v* na początku *wyrażenia* jest taki sam jak stan *v* na początku *stmt*.
*  Określony stan przypisania *v* w przepływie sterowania przeniesieniu do osiągalnej listy instrukcji bloku przełączenia jest taki sam, jak określony stan przypisania *v* na końcu *wyrażenia*.

#### <a name="while-statements"></a>While — instrukcje

W przypadku instrukcji stmt formularza: `while`
```csharp
while ( expr ) while_body
```

*  *v* ma ten sam określony stan przypisania na początku *wyrażenia* , jak na początku *stmt*.
*  Jeśli wartość *v* jest ostatecznie przypisana na końcu *wyrażenia*, to jest on ostatecznie przypisany do transferu przepływu sterowania do *while_body* i do punktu końcowego *stmt*.
*  Jeśli funkcja *v* ma stan "czas przypisania po prawdziwym wyrażeniu" na końcu wyrażenia *, to*jest on ostatecznie przypisany do przepływu sterowania przepływem do *while_body*, ale nie jest on ostatecznie przypisywany w punkcie końcowym *stmt*.
*  Jeśli w wersji *v* znajduje się stan "czas przypisania po wartości false" na końcu *wyrażenia, to*jest on ostatecznie przypisany do przepływu sterowania, który jest przesyłany do punktu końcowego *stmt*, ale nie jest on ostatecznie przypisany w przepływie sterowania do *while _body*.

#### <a name="do-statements"></a>Instrukcje do

W przypadku instrukcji stmt formularza: `do`
```csharp
do do_body while ( expr ) ;
```

*  *v* ma ten sam nieograniczony stan przypisania w przepływie sterowania, od początku *stmt* do *do_body* , jak na początku *stmt*.
*  *v* ma ten sam określony stan przypisania na początku *wyrażenia* , jak w punkcie końcowym *do_body*.
*  Jeśli wartość *v* jest ostatecznie przypisana na końcu *wyrażenia*, to jest on ostatecznie przypisany do przepływu sterowania, który jest przenoszony do punktu końcowego *stmt*.
*  Jeśli w wersji *v* znajduje się stan "czas przypisania po wartości false" na końcu *wyrażenia, to*jest on ostatecznie przypisany do przepływu sterowania, który jest przesyłany do punktu końcowego *stmt*.

#### <a name="for-statements"></a>For — instrukcje

Określone sprawdzanie przypisania dla `for` instrukcji formularza:
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
wykonuje się tak, jakby instrukcja była zapisywana:
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

Jeśli *for_condition* zostanie pominięty z `for` instrukcji, oszacowanie ostatecznego przypisania jest realizowane tak, jakby *for_condition* zostały zastąpione `true` w powyższym rozszerzeniu.

#### <a name="break-continue-and-goto-statements"></a>Instrukcje Break, Continue i goto

Określony stan przypisania *v* w przeniesieniu przepływu `break`sterowania spowodowany przez, `continue`, lub `goto` instrukcji jest taki sam jak określony stan przypisania *v* na początku instrukcji.

#### <a name="throw-statements"></a>Throw — instrukcje

Dla instrukcji *stmt* formularza
```csharp
throw expr ;
```

Określony stan przypisania *v* na początku *wyrażenia* jest taki sam jak określony stan przypisania *v* na początku *stmt*.

#### <a name="return-statements"></a>Return — instrukcje

Dla instrukcji *stmt* formularza
```csharp
return expr ;
```

*  Określony stan przypisania *v* na początku *wyrażenia* jest taki sam jak określony stan przypisania *v* na początku *stmt*.
*  Jeśli *v* jest parametrem wyjściowym, musi być ostatecznie przypisany:
    * po wyjściu
    * lub na `finally` końcu bloku `try` - lub`try` ,który`return` należy do instrukcji. - `finally` `catch` - `finally`

W przypadku instrukcji stmt formularza:
```csharp
return ;
```

*  Jeśli *v* jest parametrem wyjściowym, musi być ostatecznie przypisany:
    * przed *stmt*
    * lub na `finally` końcu bloku `try` - lub`try` ,który`return` należy do instrukcji. - `finally` `catch` - `finally`

#### <a name="try-catch-statements"></a>Instrukcje try-catch

W przypadku instrukcji *stmt* formularza:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  Określony stan przypisania *v* na początku *try_block* jest taki sam, jak określony stan przypisania *v* na początku *stmt*.
*  Określony stan przypisania *v* na początku *catch_block_i* ( *dla każdej z*nich) jest taki sam jak określony stan przypisania *v* na początku *stmt*.
*  Określony stan przypisania *v* w punkcie końcowym *stmt* jest w przypadku, gdy (i tylko wtedy, gdy) *v* jest ostatecznie przypisywany w punkcie końcowym *try_block* i każdy *catch_block_i* (za każdym *i* z 1 do *n* ).

#### <a name="try-finally-statements"></a>Try-finally — instrukcje

W przypadku instrukcji stmt formularza: `try`
```csharp
try try_block finally finally_block
```

*  Określony stan przypisania *v* na początku *try_block* jest taki sam, jak określony stan przypisania *v* na początku *stmt*.
*  Określony stan przypisania *v* na początku *finally_block* jest taki sam, jak określony stan przypisania *v* na początku *stmt*.
*  Określony stan przypisania *v* w punkcie końcowym *stmt* jest ostatecznie przypisany, jeśli (i tylko wtedy, gdy) co najmniej jeden z następujących warunków jest spełniony:
    * wartość *v* jest ostatecznie przypisana w punkcie końcowym *try_block*
    * wartość *v* jest ostatecznie przypisana w punkcie końcowym *finally_block*

Jeśli przeprowadzono transfer przepływu sterowania `goto` (na przykład instrukcja), który rozpoczyna się w *try_block*i kończą się poza *try_block*, wówczas *v* jest również uznawany za oczekiwany w przypadku, gdy *v* jest ostatecznie przypisano w punkcie końcowym *finally_block*. (To nie jest tylko wtedy, gdy w przypadku, gdy *v* jest ostatecznie przypisany z innego powodu tego transferu przepływu sterowania, nadal jest uznawany za ostatecznie przypisany).

#### <a name="try-catch-finally-statements"></a>Try-catch-finally — instrukcje

Analiza określona przez przypisanie dla `try` - `catch` instrukcjiwpostaci-: `finally`
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
wykonuje się tak, jakby `try` instrukcja była - `finally` instrukcją otaczającą `try` - `catch` instrukcję:
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

Poniższy przykład ilustruje sposób, w jaki różne bloki `try` instrukcji ([Instrukcja try](statements.md#the-try-statement)) wpływają na określone przypisanie.
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a>Instrukcja foreach

W przypadku instrukcji stmt formularza: `foreach`
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  Określony stan przypisania *v* na początku *wyrażenia* jest taki sam jak stan *v* na początku *stmt*.
*  Określony stan przypisania *v* w przepływie sterowania transfer do *embedded_statement* lub do punktu końcowego *stmt* jest taki sam jak stan *v* na końcu *wyrażenia*.

#### <a name="using-statements"></a>Using — instrukcje

W przypadku instrukcji stmt formularza: `using`
```csharp
using ( resource_acquisition ) embedded_statement
```

*  Określony stan przypisania *v* na początku *resource_acquisition* jest taki sam jak stan *v* na początku *stmt*.
*  Określony stan przypisania *v* w przeniesieniu przepływu sterowania do *embedded_statement* jest taki sam jak stan *v* na końcu *resource_acquisition*.

#### <a name="lock-statements"></a>Instrukcje Lock

W przypadku instrukcji stmt formularza: `lock`
```csharp
lock ( expr ) embedded_statement
```

*  Określony stan przypisania *v* na początku *wyrażenia* jest taki sam jak stan *v* na początku *stmt*.
*  Określony stan przypisania *v* w przeniesieniu przepływu sterowania do *embedded_statement* jest taki sam jak stan *v* na końcu *wyrażenia*.

#### <a name="yield-statements"></a>Instrukcje Yield

W przypadku instrukcji stmt formularza: `yield return`
```csharp
yield return expr ;
```

*  Określony stan przypisania *v* na początku *wyrażenia* jest taki sam jak stan *v* na początku *stmt*.
*  Określony stan przypisania *v* na końcu *stmt* jest taki sam jak stan *v* na końcu *wyrażenia*.
*  `yield break` Instrukcja nie ma wpływu na stan o określonym przypisaniu.

#### <a name="general-rules-for-simple-expressions"></a>Ogólne reguły dla prostych wyrażeń

Następująca reguła ma zastosowanie do tych rodzajów wyrażeń: literałów ([literałów](expressions.md#literals)), proste nazwy ([proste nazwy](expressions.md#simple-names)), wyrażenia dostępu do składowych ([dostęp do elementów członkowskich](expressions.md#member-access)), nieindeksowane wyrażenia dostępu podstawowego ([dostęp podstawowy](expressions.md#base-access)), `typeof`wyrażenia ([operator typeof](expressions.md#the-typeof-operator)), wyrażenia wartości domyślnych ([wyrażenia wartości domyślnych](expressions.md#default-value-expressions)) i `nameof` wyrażenia ([wyrażenia nameof](expressions.md#nameof-expressions)).

*  Określony stan przypisania *v* na końcu takiego wyrażenia jest taki sam jak określony stan przypisania *v* na początku wyrażenia.

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>Ogólne reguły dla wyrażeń z osadzonymi wyrażeniami

Następujące reguły mają zastosowanie do tych rodzajów wyrażeń: wyrażeń ujętych w nawiasy ([wyrażenia w nawiasach](expressions.md#parenthesized-expressions)), wyrażenia dostępu do elementów ([dostęp do elementów](expressions.md#element-access)), podstawowe wyrażenia dostępu z indeksowanie ([dostęp podstawowy](expressions.md#base-access)), przyrosty i wyrażenia zmniejszania ([Operatory przyrostka i zmniejszenie](expressions.md#postfix-increment-and-decrement-operators), [Operatory przyrostu i zmniejszania prefiksu](expressions.md#prefix-increment-and-decrement-operators)), wyrażenia rzutowania ( `+`[wyrażenia rzutowania](expressions.md#cast-expressions)), jednoargumentowe `-`,, `~`, `*`wyrażenia, dane `+`binarne `-`, `*` `/`,, ,,`%`,,,,,, `<<` `>>` `<` `<=` `>` `>=` `==`, `!=`, ,`is`,, ,`^`wyrażenia ([Operatory arytmetyczne](expressions.md#arithmetic-operators), [operatory przesunięcia](expressions.md#shift-operators), relacyjne i testy typu `|` `as` `&` [ Operatory](expressions.md#relational-and-type-testing-operators), [Operatory logiczne](expressions.md#logical-operators)), złożone wyrażenia przypisania `checked` ([przypisanie złożone](expressions.md#compound-assignment)) i `unchecked` wyrażenia ([Operatory sprawdzone i niesprawdzone](expressions.md#the-checked-and-unchecked-operators)), a także tablica i delegat wyrażenia tworzenia ([operator new](expressions.md#the-new-operator)).

Każdy z tych wyrażeń zawiera co najmniej jedno Podwyrażenie, które jest niewarunkowo oceniane w ustalonej kolejności. Na przykład operator binarny `%` oblicza po lewej stronie operatora, po prawej stronie. Operacja indeksowania szacuje wyrażenie indeksowane, a następnie oblicza każde wyrażenie indeksu w kolejności od lewej do prawej. Dla *wyrażenia Expression,* które ma podwyrażenia *E1, E2,..., EN*, oceniane w tej kolejności:

*  Określony stan przypisania *v* na początku *E1* jest taki sam jak określony stan przypisania na początku *wyrażenia*.
*  Określony stan przypisania *v* na początku *EI* (*i* więcej niż jeden) jest taki sam jak określony stan przypisania na końcu poprzedniego wyrażenia podrzędnego.
*  Określony stan przypisania *v* na końcu *wyrażenia* jest taki sam, jak określony stan przypisania na końcu *pl*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Wyrażenia wywołania i wyrażenia tworzenia obiektów

*Wyrażenie wyrażenia wywołania* w postaci:
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
lub wyrażenie tworzenia obiektu formularza:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  Dla wyrażenia wywołania, określony stan przypisania *v* przed *primary_expression* jest taki sam jak stan *v* przed *wyrażeniem*.
*  Dla wyrażenia wywołania, określony stan przypisania *v* przed *arg1* jest taki sam jak stan *v* po *primary_expression*.
*  W przypadku wyrażenia tworzenia obiektu określony stan przypisania *v* przed *arg1* jest taki sam jak stan *v* przed *wyrażeniem*.
*  Dla każdego argumentu *Argi*, określony stan przypisania *v* po *Argi* jest określany przez reguły wyrażeń normalnych, ignorując wszystkie `ref` lub `out` modyfikatory.
*  Dla każdego argumentu *Argi* dla każdego *i* więcej niż jednego, określony stan przypisania *v* przed *Argi* jest taki sam jak stan *v* po poprzednim *ARG*.
*  Jeśli zmienna *v* jest `out` przenoszona jako argument (tj. argument formularza `out v`) w którymkolwiek z argumentów, stanie *v* po elemencie *Expr* jest ostatecznie przypisany. Przypadku stan *v* po wyszukiwaniu *jest taki* sam jak stan *v* po *argN*.
*  Dla inicjatorów tablicy ([wyrażenia tworzenia tablic](expressions.md#array-creation-expressions)), inicjatorów obiektów ([inicjatorów obiektów](expressions.md#object-initializers)), inicjatorów kolekcji ([Inicjatory kolekcji](expressions.md#collection-initializers)) i inicjatorów obiektów anonimowych ([Tworzenie obiektów anonimowych wyrażenia](expressions.md#anonymous-object-creation-expressions)) określony stan przypisania jest określany przez rozszerzanie, że te konstrukcje są zdefiniowane w warunkach.

#### <a name="simple-assignment-expressions"></a>Proste wyrażenia przypisania

*Wyrażenie wyrażenia w* postaci `w = expr_rhs`:

*  Określony stan przypisania wartości *v* przed *expr_rhs* jest taki sam, jak określony stan przypisania *v* przed *wyrażeniem*.
*  Określony stan przypisania *v* po wyjściu jest określany przez:
   * Jeśli *w* jest taka sama jak zmienna *v*, *oznacza to,* że określony stan przypisania *v* po wystawce jest ostatecznie przypisany.
   * W przeciwnym razie, jeśli przypisanie występuje w konstruktorze wystąpienia typu struktury, jeśli *w* jest dostęp do właściwości wskazującej automatycznie zaimplementowaną Właściwość *P* w tworzonym wystąpieniu i *v* jest polem ukrytego zapasowego *P*, a następnie określony stan przypisania *v* po elemencie *Expr* jest ostatecznie przypisany.
   * W przeciwnym razie określony stan przypisania *v* po wyszukiwaniu *jest taki* sam jak w przypadku określonego stanu przypisania *v* po *expr_rhs*.

#### <a name="-conditional-and-expressions"></a>wyrażenia & & (warunkowe i)

*Wyrażenie wyrażenia w* postaci `expr_first && expr_second`:

*  Określony stan przypisania wartości *v* przed *expr_first* jest taki sam, jak określony stan przypisania *v* przed *wyrażeniem*.
*  Określony stan przypisania wartości *v* przed *expr_second* jest ostatecznie przypisywany, jeśli stanem *v* po *expr_first* jest w nieskończony sposób przypisany lub "w znacznym przypisaniu po true Expression". W przeciwnym razie nie jest to ostatecznie przypisane.
*  Określony stan przypisania *v* po wyjściu jest określany przez:
    * Jeśli *expr_first* jest wyrażeniem stałym o wartości `false`, oznacza to, że określony stan przypisania *v* po *wyrażeniu* jest taki sam, jak w przypadku określonego stanu przypisania *v* po *expr_first*.
    * W przeciwnym razie, jeśli stan *v* po *expr_first* jest ostatecznie przypisany, stan *v* po elemencie *Expr* jest ostatecznie przypisany.
    * W przeciwnym razie, jeśli stan *v* po *expr_second* jest ostatecznie przypisany, a stanem *v* po *expr_first* jest "w sposób terminowy przypisany po wartości false", wówczas stanem *v* *po wyrażeniu jest* nieskończoność pisywany.
    * W przeciwnym razie, jeśli stan *v* po *expr_second* jest ostatecznie przypisany lub "w sposób terminowy przypisany po prawdziwe wyrażenie", wówczas stanem *v* *po wyrażeniu jest* "w sposób terminowy przypisany po prawdziwe wyrażenie".
    * W przeciwnym razie, jeśli stanem *v* po *expr_first* jest "w sposób terminowy przypisany po false Expression", a stanem *v* po *expr_second* jest "w sposób terminowy przypisany po false Expression", wówczas stanem *v* po  *wyrażenie* jest "w sposób terminowy przypisany po false Expression".
    * W przeciwnym razie stan *v* *po wyszukiwaniu nie jest* ostatecznie przypisany.

W przykładzie
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
zmienna `i` jest uznawana za ostatecznie przypisaną w jednej z osadzonych instrukcji `if` instrukcji, ale nie w drugim. W instrukcji w metodzie `F`zmienna `i` jest ostatecznie przypisana w pierwszej instrukcji osadzonej, ponieważ wykonywanie wyrażenia `(i = y)` zawsze poprzedza wykonywanie tej osadzonej instrukcji. `if` Natomiast zmienna `i` nie jest ostatecznie przypisana w drugiej instrukcji osadzonej, ponieważ `x >= 0` mogło przetestować wartość false, co spowodowało przypisanie zmiennej `i` .

#### <a name="-conditional-or-expressions"></a>|| wyrażenia (warunkowe lub)

*Wyrażenie wyrażenia w* postaci `expr_first || expr_second`:

*  Określony stan przypisania wartości *v* przed *expr_first* jest taki sam, jak określony stan przypisania *v* przed *wyrażeniem*.
*  Określony stan przypisania wartości *v* przed *expr_second* jest ostatecznie przypisany, jeśli stanem *v* po *expr_first* jest albo "w sposób terminowy przypisany po wartości false". W przeciwnym razie nie jest to ostatecznie przypisane.
*  Instrukcja przypisania o określonej wartości *v* po *wyrażeniu* jest określana przez:
    * Jeśli *expr_first* jest wyrażeniem stałym o wartości `true`, oznacza to, że określony stan przypisania *v* po *wyrażeniu* jest taki sam, jak w przypadku określonego stanu przypisania *v* po *expr_first*.
    * W przeciwnym razie, jeśli stan *v* po *expr_first* jest ostatecznie przypisany, stan *v* po elemencie *Expr* jest ostatecznie przypisany.
    * W przeciwnym razie, jeśli stan *v* po *expr_second* jest ostatecznie przypisany, a stanem *v* po *expr_first* jest "w sposób terminowy przypisany po prawdziwe wyrażenie", wówczas stanem *v* *po wyrażeniu jest* nieskończoność pisywany.
    * W przeciwnym razie, jeśli stan *v* po *expr_second* jest ostatecznie przypisany lub "w sposób terminowy przypisany po wartości false", wówczas stanem *v* *po wyrażeniu jest* "w sposób terminowy przypisany po false Expression".
    * W przeciwnym razie, jeśli stanem *v* po *expr_first* jest "w sposób terminowy przypisany po prawdziwym wyrażeniu", a stanem *v* po *expr_second* jest "w sposób terminowy przypisany po prawdziwe wyrażenie", wówczas stanem *v* *po wyrażeniu* jest "w sposób terminowy przypisany po true Expression".
    * W przeciwnym razie stan *v* *po wyszukiwaniu nie jest* ostatecznie przypisany.

W przykładzie
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
zmienna `i` jest uznawana za ostatecznie przypisaną w jednej z osadzonych instrukcji `if` instrukcji, ale nie w drugim. W instrukcji w metodzie `G`zmienna `i` jest ostatecznie przypisana w drugiej instrukcji osadzonej, ponieważ wykonanie wyrażenia `(i = y)` zawsze poprzedza wykonywanie tej osadzonej instrukcji. `if` Natomiast zmienna `i` nie jest ostatecznie przypisana w pierwszej instrukcji osadzonej, ponieważ `x >= 0` mogło przetestować wartość true, co spowodowało przypisanie zmiennej `i` .

#### <a name="-logical-negation-expressions"></a>! wyrażenia logicznej negacji

*Wyrażenie wyrażenia w* postaci `! expr_operand`:

*  Określony stan przypisania wartości *v* przed *expr_operand* jest taki sam, jak określony stan przypisania *v* przed *wyrażeniem*.
*  Określony stan przypisania *v* po wyjściu jest określany przez:
    * Jeśli stan *v* po * expr_operand * jest przypisywany w określony sposób, stan *v* po elemencie *Expr* jest ostatecznie przypisany.
    * Jeśli stan *v* *po ** expr_operand * nie jest ostatecznie przypisany, stan *v* po wyjściu nie jest przypisany ostatecznie.
    * Jeśli stanem *v* po * expr_operand * jest "w sposób terminowy przypisany po wartości false", wówczas stanem *v* *po wyrażeniu jest* "w sposób terminowy przypisany po prawdziwe wyrażenie".
    * Jeśli stanem *v* po * expr_operand * jest "w sposób terminowy przypisany po prawdziwe wyrażenie", wówczas stanem *v* *po wyrażeniu jest* "w sposób terminowy przypisany po false Expression".

#### <a name="-null-coalescing-expressions"></a>?? (zerowe łączenie) wyrażenia

*Wyrażenie wyrażenia w* postaci `expr_first ?? expr_second`:

*  Określony stan przypisania wartości *v* przed *expr_first* jest taki sam, jak określony stan przypisania *v* przed *wyrażeniem*.
*  Określony stan przypisania *v* przed *expr_second* jest taki sam, jak w przypadku określonego stanu przypisania *v* po *expr_first*.
*  Instrukcja przypisania o określonej wartości *v* po *wyrażeniu* jest określana przez:
    * Jeśli *expr_first* jest wyrażeniem stałym ([wyrażenia stałe](expressions.md#constant-expressions)) o wartości null, stanie *v* po *wyrażeniu* jest taka sama jak stan *v* po *expr_second*.
*  W przeciwnym razie stan *v* po wyszukiwaniu *jest taki* sam jak w przypadku określonego stanu przypisania *v* po *expr_first*.

#### <a name="-conditional-expressions"></a>?: (warunkowe) wyrażenia

*Wyrażenie wyrażenia w* postaci `expr_cond ? expr_true : expr_false`:

*  Określony stan przypisania *v* przed *expr_cond* jest taki sam jak stan *v* przed *wyrażeniem*.
*  Określony stan przypisania *v* przed *expr_true* jest ostatecznie przypisany, jeśli tylko jedno z następujących posiada:
    * *expr_cond* jest wyrażeniem stałym z wartością`false`
    * stan *v* po *expr_cond* jest ostatecznie przypisany lub "w sposób terminowy przypisany po true Expression".
*  Określony stan przypisania *v* przed *expr_false* jest ostatecznie przypisany, jeśli tylko jedno z następujących posiada:
    * *expr_cond* jest wyrażeniem stałym z wartością`true`
*  stan *v* po *expr_cond* jest ostatecznie przypisany lub "w sposób terminowy przypisany po false Expression".
*  Określony stan przypisania *v* po wyjściu jest określany przez:
    * Jeśli *expr_cond* jest wyrażeniem stałym[(wyrażenia stałe](expressions.md#constant-expressions)) z `true` wartością, wówczas stanem *v* po wyrażeniu *jest taka* sama jak stan *v* po *expr_true*.
    * W przeciwnym razie, jeśli *expr_cond* jest wyrażeniem stałym ([wyrażenia stałe](expressions.md#constant-expressions) `false` ) z wartością, stanem *v* po wyrażeniu jest taka sama jak stan *v* po *expr_false*.
    * W przeciwnym razie, jeśli stan *v* po *expr_true* jest ostatecznie przypisany i stanem *v* po *expr_false* jest ostatecznie przypisany, stan *v* po elemencie *Expr* jest ostatecznie przypisany.
    * W przeciwnym razie stan *v* *po wyszukiwaniu nie jest* ostatecznie przypisany.

#### <a name="anonymous-functions"></a>Funkcje anonimowe

Dla wyrażenia *lambda_expression* lub *anonymous_method_expression* *z treścią*(albo *blokiem* lub *wyrażeniem*):

*  Określony stan przypisania zewnętrznej zmiennej *v* przed *treścią* jest taki sam jak stan *v* przed *wyrażeniem*. Oznacza to, że określony stan przypisania zmiennych zewnętrznych jest Dziedziczony z kontekstu funkcji anonimowej.
*  Określony stan przypisania zmiennej zewnętrznej *v* po wyjściu jest taki sam jak stan *v* przed *wyrażeniem*.

Przykład
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
generuje błąd czasu kompilacji, ponieważ `max` nie jest on ostatecznie przypisany, gdzie jest zadeklarowana funkcja anonimowa. Przykład
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
generuje również błąd czasu kompilacji, ponieważ przypisanie do `n` funkcji anonimowej nie ma wpływu na określony `n` stan przypisania poza funkcję anonimową.

## <a name="variable-references"></a>Odwołania do zmiennych

*Variable_reference* jest *wyrażeniem* , które jest sklasyfikowane jako zmienna. *Variable_reference* oznacza lokalizację magazynu, do którego można uzyskać dostęp zarówno w celu pobrania bieżącej wartości, jak i zapisania nowej wartości.

```antlr
variable_reference
    : expression
    ;
```

W języku C C++i *variable_reference* jest znany jako *lvalue*.

## <a name="atomicity-of-variable-references"></a>Niepodzielność odwołań do zmiennych

Odczyty i zapisy następujących typów danych są niepodzielne `bool`: `char`, `byte` `uint` `ushort`, `sbyte` `short` `int`, ,,,,,itypyreferencyjne.`float` Ponadto odczyty i zapisy typów wyliczeniowych z typem podstawowym na poprzedniej liście również są niepodzielne. Odczyty i zapisy innych typów, w `long`tym `ulong` `double`,,, `decimal`i, jak również typy zdefiniowane przez użytkownika, nie są gwarantowane jako niepodzielne. Poza funkcjami biblioteki zaprojektowanymi do tego celu nie istnieje gwarancja niepodzielnego odczytu i zapisu, na przykład w przypadku zwiększenia lub zmniejszenia.

