---
ms.openlocfilehash: ff285fc202d14c2060c5f005c319c7886458a168
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66174241"
---
# <a name="variables"></a>Zmienne

Zmienne reprezentują lokalizacje przechowywania. Co zmienna ma typ, który określa, jakie wartości mogą być przechowywane w zmiennej. C# to język bezpieczny i kompilator języka C# gwarantuje, że wartości przechowywane w zmiennych są zawsze odpowiedniego typu. Wartość zmiennej można zmienić za pośrednictwem przydziału lub za pośrednictwem `++` i `--` operatorów.

Zmienna musi być ***zdecydowanie przypisywany*** ([asercję określonego przypisania](variables.md#definite-assignment)) przed jej wartość można uzyskać.

Zgodnie z opisem w poniższych sekcjach, zmienne są ***przypisane początkowo*** lub ***początkowo nieprzypisane***. Początkowo przypisanej zmiennej ma dobrze zdefiniowanych wartości początkowej i zawsze jest uznawany za zdecydowanie przypisana. Początkowo nieprzypisanej zmiennej nie ma początkowej wartości. Początkowo nieprzypisane zmiennej wziąć pod uwagę zdecydowanie przypisane w określonej lokalizacji przypisania do zmiennej musi występować w każdej ścieżce możliwe wykonanie, co prowadzi do tej lokalizacji.

## <a name="variable-categories"></a>Kategorie zmiennej

Język C# definiuje siedem kategorii zmiennych: zmiennych statycznych, zmienne wystąpienia, elementy tablicy, wartości parametrów, parametrów w formie odwołań, parametry wyjściowe i zmiennych lokalnych. W kolejnych sekcjach opisano każdy z tych kategorii.

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
`x` Zmienna statyczna jest `y` jest zmienną instance `v[0]` jest element tablicy `a` jest wartość parametru, `b` jest parametr przekazany przez odwołanie, `c` jest parametrem wyjściowym i `i` jest zmienną lokalną.

### <a name="static-variables"></a>Zmienne statyczne

Pole jest zadeklarowane za pomocą `static` nosi nazwę modyfikator ***zmienna statyczna***. Zmienna statyczna trafia do istnienia przed wykonaniem konstruktora statycznego ([konstruktorów statycznych](classes.md#static-constructors)) jego zawierający typ i przestaje istnieć, gdy domena skojarzona aplikacja przestanie istnieć.

Początkowa wartość Zmienna statyczna jest wartością domyślną ([wartości domyślne](variables.md#default-values)) typu zmiennej.

Do celów sprawdzania asercję określonego przypisania zmienna statyczna jest uznawany za początkowo przypisana.

### <a name="instance-variables"></a>Zmienne wystąpienia

Pole zadeklarowana bez `static` nosi nazwę modyfikator ***zmienną instance***.

#### <a name="instance-variables-in-classes"></a>Zmienne wystąpienia klas

Zmiennej wystąpienia klasy trafia do istnienia, nowe wystąpienie tej klasy jest tworzona, gdy przestanie istnieć, gdy nie ma żadnych odwołań do tego wystąpienia i destruktor wystąpienia (jeśli istnieje) zostanie wykonany.

Początkowa wartość zmiennej wystąpienia klasy jest wartością domyślną ([wartości domyślne](variables.md#default-values)) typu zmiennej.

Na potrzeby sprawdzania, asercja określonego przydziału, zmiennej wystąpienia klasy jest uważany za początkowo przypisana.

#### <a name="instance-variables-in-structs"></a>Zmienne wystąpienia w strukturach

Zmienną instance struktury ma dokładnie ten sam okres istnienia jako zmiennej struktury, do której należy. Innymi słowy, gdy zmienną typu struktury trafia do istnienia lub przestaje istnieć, więc zbyt czy zmienne wystąpienia struktury.

Stan początkowego przydziału zmienną instance struktury jest taka sama jak zawierającego zmiennej struktury. Innymi słowy, po zmiennej struktury jest uznawany za początkowo przypisane, więc za jego zmienne wystąpienia i po zmiennej struktury jest uznawany za początkowo nieprzypisane, jego zmienne wystąpienia są podobnie nieprzypisane.

### <a name="array-elements"></a>Elementy tablicy

Elementy tablicy rozpoczęciu istnienie, gdy tworzone jest wystąpienie tablicy i przestaną istnieć, gdy istnieją żadnych odwołań do tego wystąpienia tablicy.

Początkowa wartość każdego z elementów tablicy jest wartością domyślną ([wartości domyślne](variables.md#default-values)) typ elementów tablicy.

Na potrzeby sprawdzania, asercja określonego przydziału, do elementu tablicy jest uważany za początkowo przypisana.

### <a name="value-parameters"></a>Wartości parametrów

Parametr zadeklarowana bez `ref` lub `out` modyfikator jest ***wartość parametru***.

Wartość parametru trafia do istnienia na wywołanie funkcji elementu członkowskiego, (metody, konstruktora wystąpienia, metody dostępu lub operator) lub funkcja anonimowa do których parametr należy i jest inicjowany z wartością argumentu wywołania. Parametr wartości zwykle przestanie istnieć po powrocie funkcja składowa lub funkcja anonimowa. Jednakże jeśli parametr wartości są przechwytywane przez funkcję anonimową ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)), jego okres istnienia co najmniej rozszerza aż delegat lub drzewa wyrażeń utworzone na podstawie tego funkcja anonimowa kwalifikuje się do wyrzucanie elementów bezużytecznych.

Na potrzeby sprawdzania, asercja określonego przydziału, parametr wartości jest uważany za początkowo przypisana.

### <a name="reference-parameters"></a>Parametry odwołania

Parametr zadeklarowana za pomocą `ref` modyfikator jest ***odwołać się do parametru***.

Parametr przekazany przez odwołanie, nie powoduje utworzenia nowej lokalizacji magazynu. Zamiast tego parametru odwołania reprezentuje tę samą lokalizację magazynu zmienna, podane jako argument w funkcji składowej lub wywołania funkcji anonimowych. W związku z tym wartość parametru odwołania jest zawsze taki sam, jako zmienna bazowego.

Asercja określonego przydziału obowiązują następujące reguły do parametrów odwołania. Należy pamiętać, różne reguły dla parametrów wyjściowych, które opisano w [parametrów wyjściowych](variables.md#output-parameters).

*  Zmienna musi zostać zdecydowanie przypisany ([asercję określonego przypisania](variables.md#definite-assignment)) zanim może być przekazywany jako parametr w wywołaniu funkcji elementu członkowskiego lub delegata przekazany przez odwołanie.
*  Funkcja składowa lub funkcja anonimowa parametr przekazany przez odwołanie jest traktowany jako wstępnie przypisany.

Wewnątrz metody wystąpienia lub metoda dostępu do wystąpienia typu struktury `this` — słowo kluczowe zachowuje się dokładnie tak jak parametr typu struktury przekazany przez odwołanie ([dostęp](expressions.md#this-access)).

### <a name="output-parameters"></a>Parametry wyjściowe

Parametr zadeklarowana za pomocą `out` modyfikator jest ***parametr wyjściowy***.

Parametr wyjściowy nie powoduje utworzenia nowej lokalizacji magazynu. Zamiast tego parametru output reprezentuje tę samą lokalizację magazynu zmienna, podane jako argument w wywołaniu funkcji elementu członkowskiego lub delegata. W związku z tym wartość parametru wyjściowego jest zawsze taki sam, jako zmienna bazowego.

Asercja określonego przydziału obowiązują następujące reguły do parametrów danych wyjściowych. Należy pamiętać, różne reguły odniesienia parametrów opisanych w [odwołania do parametrów](variables.md#reference-parameters).

*  Zmienna musi nie być zdecydowanie przypisana może być przekazywany jako parametr wyjściowy w funkcji składowej, lub delegować wywołania.
*  Po zakończeniu normalne wywołanie funkcji elementu członkowskiego lub delegata każda zmienna, która została przekazana jako parametr wyjściowy jest uznawany za przypisane w tej ścieżce wykonywania.
*  Funkcja składowa lub funkcja anonimowa początkowo nieprzypisane jest traktowany jako parametr wyjściowy.
*  Każdy parametr wyjściowy, funkcja składowa lub funkcja anonimowa musi być zdecydowanie przypisana ([asercję określonego przypisania](variables.md#definite-assignment)) przed funkcją członka lub funkcja anonimowa zwraca normalnie.

W konstruktorze wystąpienia typu struktury `this` — słowo kluczowe zachowuje się dokładnie jako parametr wyjściowy typu struktury ([dostęp](expressions.md#this-access)).

### <a name="local-variables"></a>Zmienne lokalne

A ***zmienna lokalna*** zadeklarowano *local_variable_declaration*, które mogą wystąpić w *bloku*, *for_statement*, *switch_statement* lub *using_statement*; lub *foreach_statement* lub *specific_catch_clause* dla *try_statement*.

Okres istnienia zmiennej lokalnej jest części wykonywania programu, w którym magazyn jest gwarantowane do zarezerwowania dla niego. Ten okres istnienia co najmniej rozciąga się od wejścia *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, lub *specific_catch_clause* za pomocą którego jest skojarzony, aż do wykonania tego *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, lub *specific_catch_clause* kończy się w dowolny sposób. (Wprowadź ujęty *bloku* lub wywołanie metody wstrzymuje, ale nie kończy wykonywanie bieżącego *bloku*, *for_statement*, *switch_statement* , *using_statement*, *foreach_statement*, lub *specific_catch_clause*.) Jeśli zmienna lokalna jest przechwytywany przez funkcja anonimowa ([przechwyconych zmiennych zewnętrznych](expressions.md#captured-outer-variables)), jego okres istnienia co najmniej rozszerza aż delegat lub wyrażenie drzewa, utworzone na podstawie funkcja anonimowa, wraz ze wszystkimi innymi obiektami, które zaczynają odwoływać się do przechwyconej zmiennej, kwalifikują się do wyrzucania elementów bezużytecznych.

Jeśli element nadrzędny *bloku*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, lub *specific_catch_clause* jest wprowadzana cyklicznie, nowe wystąpienie zmiennej lokalnej jest tworzony za każdym razem i jego *local_variable_initializer*, jeśli istnieje, nie zostało ocenione każdorazowo.

Zmienna lokalna wynikające z *local_variable_declaration* nie została zainicjowana automatycznie i w związku z tym nie ma wartości domyślnej. Na potrzeby sprawdzania, asercja określonego przydziału, zmienna lokalna wprowadzone przez *local_variable_declaration* jest uznawany za początkowo nieprzypisane. A *local_variable_declaration* mogą obejmować *local_variable_initializer*, w którym to przypadku zmienna jest uznawany za zdecydowanie przypisany dopiero po wyrażenie inicjujące ([ Instrukcje deklaracji](variables.md#declaration-statements)).

W zakresie zmienną lokalną, wynikające z *local_variable_declaration*, jest to błąd czasu kompilacji do odwoływania się do tej zmiennej lokalnej w stanie tekstową poprzedzającym jego *local_variable_declarator*. Jeśli w deklaracji zmiennej lokalnej jest niejawne ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)), również jest błędem do odwoływania się do zmiennej w ramach jego *local_variable_declarator*.

Zmienna lokalna wynikające z *foreach_statement* lub *specific_catch_clause* jest uważana za cały zakres zdecydowanie przypisany.

Rzeczywisty okres istnienia zmiennej lokalnej jest zależna od implementacji. Na przykład kompilatora statycznie określić, że zmienna lokalna w bloku jest używana tylko dla małych części tego bloku. Za pomocą tej analizy, kompilator może wygenerować kod, który skutkuje magazynu zmiennej o krótszy okres istnienia niż bloku go zawierającego.

Magazyn, określone przez zmienną lokalnego odwołania jest odzyskiwane niezależnie od okresu istnienia tej zmiennej lokalnego odwołania ([automatyczne zarządzanie pamięcią](basic-concepts.md#automatic-memory-management)).

## <a name="default-values"></a>Wartości domyślne

Następujące kategorie zmienne są automatycznie inicjowane do wartości domyślnych:

*  Zmienne statyczne.
*  Zmienne wystąpienia wystąpienia klasy.
*  Elementy tablicy.

Wartość domyślna zmiennej zależy od typu zmiennej i jest określany w następujący sposób:

*  Dla zmiennej *value_type*, wartością domyślną jest taka sama jak wartość obliczona przez *value_type*firmy domyślnego konstruktora ([domyślne konstruktory](types.md#default-constructors)).
*  Dla zmiennej *reference_type*, wartość domyślna to `null`.

Inicjowanie do wartości domyślnych jest zazwyczaj wykonywane przez Menedżera pamięci lub modułu zbierającego elementy bezużyteczne zainicjowanie pamięci, aby wszystkie bity zero, zanim jest przydzielany do użycia. Z tego powodu jest łatwa w użyciu wszystkie bity zero do reprezentowania odwołanie o wartości null.

## <a name="definite-assignment"></a>Asercja określonego przydziału

W podanej lokalizacji w kod wykonywalny funkcji elementu członkowskiego, zmienna jest nazywany ***zdecydowanie przypisywany*** w przypadku kompilator może potwierdzić podczas analizy statycznej przepływu ([dokładne zasady ustalania określony Przypisanie](variables.md#precise-rules-for-determining-definite-assignment)), zmienna automatycznie zainicjować lub zostało celem co najmniej jedno przypisanie. Nieformalnie wspomniano, są następujące reguły asercję określonego przypisania:

*  Początkowo przypisanej zmiennej ([początkowo przypisana zmienne](variables.md#initially-assigned-variables)) zawsze jest uznawany za zdecydowanie przypisany.
*  Początkowo nieprzypisanej zmiennej ([początkowo nieprzypisane zmienne](variables.md#initially-unassigned-variables)) jest uznawana za zdecydowanie przypisany w danej lokalizacji, jeśli wszystkie możliwe wykonania ścieżki prowadzące do tej lokalizacji zawiera co najmniej jeden z następujących czynności:
    * Przypisanie proste ([przypisanie proste](expressions.md#simple-assignment)), w którym zmienna jest lewy operand.
    * Wyrażenie wywołania ([wyrażenia wywołania](expressions.md#invocation-expressions)) lub wyrażenie tworzenia obiektu ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)) który przekazuje zmiennej jako parametr wyjściowy.
    * Dla zmiennej lokalnej deklaracji zmiennej lokalnej ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)) zawierającej inicjatorze zmiennych.

Formalną specyfikację nieformalne powyższych zasad podstawowych jest opisana w [początkowo przypisana zmienne](variables.md#initially-assigned-variables), [początkowo nieprzypisane zmienne](variables.md#initially-unassigned-variables), i [dokładne zasady ustalania asercja określonego przydziału](variables.md#precise-rules-for-determining-definite-assignment).

Stany asercję określonego przypisania zmiennych wystąpienia *struct_type* zmiennej są śledzone osobno również jak zbiorczo. W dodatkowych reguł powyżej, następujące reguły dotyczą *struct_type* zmienne oraz ich zmienne wystąpienia:

*  Zmienną instance jest uznawana za zdecydowanie przypisany jeśli jego zawierający *struct_type* zmienna jest uznawany za zdecydowanie przypisany.
*  A *struct_type* zmienna jest uznawana za zdecydowanie przypisany Jeśli każdego z jego zmienne wystąpienia jest uznawany za zdecydowanie przypisany.

Asercja określonego przydziału jest to wymagane w następujących okolicznościach:

*  Zmienna musi być zdecydowanie przypisana w każdej lokalizacji, w których uzyskuje się wartość. Daje to gwarancję, niezdefiniowane wartości nigdy nie wystąpi. Wystąpienie zmiennej w wyrażeniu jest uważany za uzyskiwanie wartości zmiennej, chyba że
    * zmienna jest lewy operand przypisanie proste
    * zmienna jest przekazywana jako parametr wyjściowy lub
    * zmienna jest *struct_type* zmiennej i pojawia się jako lewy operand dostępu elementu członkowskiego.
*  Zmienna musi być zdecydowanie przypisana w każdej lokalizacji, w której jest przekazywany jako parametr przekazany przez odwołanie. Gwarantuje to, czy funkcja składowa, wywoływana można wziąć pod uwagę parametr odwołania początkowo przypisana.
*  Wszystkie parametry wyjściowe funkcji elementu członkowskiego musi być zdecydowanie przypisana w każdej lokalizacji, w którym element członkowski funkcji zwraca (za pośrednictwem `return` instrukcji lub za pośrednictwem wykonywania osiągnięcia końca treści funkcji składowej). Daje to gwarancję, że funkcji elementów członkowskich nie zwracają niezdefiniowane wartości w parametry wyjściowe kompilatora wziąć pod uwagę wywołania elementu funkcji, która przyjmuje zmienną jako parametr wyjściowy równoważne do przypisania do zmiennej umożliwiając w ten sposób.
*  `this` Zmiennej *struct_type* konstruktora wystąpienia musi być zdecydowanie przypisana w każdej lokalizacji, w których zwraca tego konstruktora wystąpienia.

### <a name="initially-assigned-variables"></a>Początkowo zmiennymi

Następujące kategorie zmienne są klasyfikowane jako początkowo przypisana:

*  Zmienne statyczne.
*  Zmienne wystąpienia wystąpienia klasy.
*  Zmienne wystąpienia struktury wstępnie przypisanych zmiennych.
*  Elementy tablicy.
*  Wartości parametrów.
*  Parametry odwołania.
*  Zmienne zadeklarowane w `catch` klauzuli lub `foreach` instrukcji.

### <a name="initially-unassigned-variables"></a>Początkowo nieprzypisane zmiennych

Następujące kategorie zmienne są klasyfikowane jako początkowo nieprzypisane:

*  Zmienne wystąpienia struktury początkowo nieprzypisane zmiennych.
*  Dane wyjściowe parametrów, w tym `this` zmiennej konstruktorów wystąpienia struktury.
*  Zmienne lokalne, oprócz tych zadeklarowanych w `catch` klauzuli lub `foreach` instrukcji.

### <a name="precise-rules-for-determining-definite-assignment"></a>Dokładne zasady określania asercję określonego przypisania

Aby określić, że zmiennych używanych jest zdecydowanie przypisany, kompilator korzystać procesu, który jest odpowiednikiem to opisane w tej sekcji.

Kompilator przetwarza treść każdy element członkowski funkcji, który ma co najmniej jednej zmiennej początkowo nieprzypisane. Dla każdej zmiennej początkowo nieprzypisane *v*, kompilator Określa ***stanu asercję określonego przypisania*** dla *v* w każdym z następujących punktów w elemencie członkowskim funkcji:

*  Na początku każdej instrukcji
*  W punkcie końcowym ([punktów końcowych i osiągalności](statements.md#end-points-and-reachability)) każdej instrukcji
*  Na każdym łuku który przekazuje sterowanie do innej instrukcji lub punkt końcowy w instrukcji
*  Na początku każdego wyrażenia
*  Na koniec każdego wyrażenia

Stan asercję określonego przypisania *v* może być albo:

*  Zdecydowanie przypisany. Oznacza to, że na wszystkich przepływów sterowania możliwe do tej pory *v* zostanie przypisana wartość.
*  Zdecydowanie Nieprzypisana. Stan zmiennej na końcu wyrażenia typu `bool`, stan zmienna, która nie jest zdecydowanie przypisywany może (ale nie zawsze) można podzielić na jeden z następujących stanów podrzędnych:
    * Zdecydowanie przypisana po wartość true, wyrażenie. Ten stan wskazuje, że *v* jest zdecydowanie przypisana, jeśli wyrażenie logiczne oceniane jako PRAWDA, ale nie jest koniecznie przypisany, jeśli wyrażenie logiczne ocenione jako fałszywe.
    * Zdecydowanie przypisana po wyrażeniu false. Ten stan wskazuje, że *v* jest zdecydowanie przypisana, jeśli wyrażenie logiczne ocenione jako fałszywe, ale nie jest koniecznie przypisany, jeśli wyrażenie logiczne jest oceniane jako PRAWDA.

Następujące reguły określają sposób, w jaki stan zmiennej *v* jest określana w każdej lokalizacji.

#### <a name="general-rules-for-statements"></a>Ogólne zasady dla instrukcji

*  *v* nie jest zdecydowanie przypisany na początku treści funkcji składowej.
*  *v* jest zdecydowanie przypisany na początku każdej instrukcji jest nieosiągalny.
*  Stan asercję określonego przypisania *v* na początku innych instrukcji jest określany przez sprawdzenie stanu asercję określonego przypisania *v* na wszystkie transfery przepływu sterowania, przeznaczonych dla początku, Instrukcja. Gdy (i tylko wtedy, gdy) *v* zdecydowanie jest przypisany na wszystkich takich transferu przepływu sterowania, na następnie *v* jest zdecydowanie przypisany na początku instrukcji. Zbiór transfery przepływu sterowania możliwe jest określana w taki sam sposób jak w przypadku sprawdzania osiągalności — instrukcja ([punktów końcowych i osiągalności](statements.md#end-points-and-reachability)).
*  Stan asercję określonego przypisania *v* w punkcie końcowym bloku `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, lub `switch` instrukcji jest określany przez sprawdzenie stanu asercję określonego przypisania *v* na wszystkie transfery przepływu sterowania, przeznaczonych dla punktu końcowego w tej instrukcji. Jeśli *v* zdecydowanie jest przypisany na wszystkich takich transferu przepływu sterowania, na następnie *v* jest zdecydowanie przypisana w punkcie końcowym instrukcji. W przeciwnym razie; *v* zdecydowanie nie jest przypisana w punkcie końcowym instrukcji. Zbiór transfery przepływu sterowania możliwe jest określana w taki sam sposób jak w przypadku sprawdzania osiągalności — instrukcja ([punktów końcowych i osiągalności](statements.md#end-points-and-reachability)).

#### <a name="block-statements-checked-and-unchecked-statements"></a>Blok instrukcji, pole jest zaznaczone oraz instrukcje niezaznaczone

Stan asercję określonego przypisania *v* w kontrolce transferu do pierwszej instrukcji listy instrukcji w bloku (lub punkt końcowy w bloku, jeśli lista instrukcji jest pusta) jest taka sama jak instrukcja asercję określonego przypisania *v* przed blokiem `checked`, lub `unchecked` instrukcji.

#### <a name="expression-statements"></a>Instrukcje wyrażeń

Dla instrukcji wyrażenia *instrukcji INSERT* składający się z wyrażenia *expr*:

*  *v* ma ten sam stan asercję określonego przypisania na początku *expr* podobnie jak na początku *instrukcji INSERT*.
*  Jeśli *v* Jeśli zdecydowanie przypisany na końcu *expr*, zdecydowanie jest przypisana w momencie zakończenia *instrukcji INSERT*; w przeciwnym razie; nie jest zdecydowanie przypisany na punkt końcowy *instrukcji INSERT*.

#### <a name="declaration-statements"></a>Instrukcje deklaracji

*  Jeśli *instrukcji INSERT* jest następnie instrukcji deklaracji bez inicjatorów, *v* ma ten sam stan asercję określonego przypisania w momencie zakończenia *instrukcji INSERT* podobnie jak na początku *instrukcji INSERT*.
*  Jeśli *instrukcji INSERT* jest następnie instrukcji deklaracji z inicjatorami, jego stan asercję określonego przypisania *v* jest określane tak, jakby *instrukcji INSERT* zostały listę instrukcji, za pomocą jednego przypisania Instrukcja dla każdej deklaracji za pomocą inicjatora (wymienione w kolejności deklaracji).

#### <a name="if-statements"></a>Jeśli instrukcji

Aby uzyskać `if` instrukcji *instrukcji INSERT* formularza:
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v* ma ten sam stan asercję określonego przypisania na początku *expr* podobnie jak na początku *instrukcji INSERT*.
*  Jeśli *v* jest zdecydowanie przypisany na końcu *expr*, a następnie jest zdecydowanie przypisany na przesyłanie przepływu sterowania do *then_stmt* i albo *else_stmt*  lub do punktu końcowego *instrukcji INSERT* przypadku nie klauzuli else.
*  Jeśli *v* ma stan "zdecydowanie przypisana, aby po wartość true, wyrażenie" na końcu *expr*, a następnie jest zdecydowanie przypisany na przesyłanie przepływu sterowania do *then_stmt*, a nie Zdecydowanie przypisany na transfer przepływu sterowania do jednej *else_stmt* lub do punktu końcowego *instrukcji INSERT* przypadku nie klauzuli else.
*  Jeśli *v* ma stan "zdecydowanie przypisana, aby po wyrażeniu false" na końcu *expr*, a następnie jest zdecydowanie przypisany na przesyłanie przepływu sterowania do *else_stmt*, a nie Zdecydowanie przypisany na przesyłanie przepływu sterowania do *then_stmt*. Zdecydowanie jest przypisana w punktu końcowego *instrukcji INSERT* tylko wtedy, gdy jest zdecydowanie przypisany na punktu końcowego *then_stmt*.
*  W przeciwnym razie *v* zostanie uznane za zdecydowanie przypisany na transfer przepływu sterowania do jednej *then_stmt* lub *else_stmt*, lub do punktu końcowego  *instrukcji INSERT* przypadku nie klauzuli else.

#### <a name="switch-statements"></a>Instrukcje Switch

W `switch` instrukcji *instrukcji INSERT* z wyrażeniem kontrolowania *expr*:

*  Stan asercję określonego przypisania *v* na początku *expr* jest taka sama jak stan *v* na początku *instrukcji INSERT*.
*  Stan asercję określonego przypisania *v* przepływowi sterowania przesyłanie danych do instrukcji zablokowanych przełącznika dostępny jest taka sama jak stan asercję określonego przypisania *v* na końcu *expr*.

#### <a name="while-statements"></a>Podczas gdy instrukcji

Aby uzyskać `while` instrukcji *instrukcji INSERT* formularza:
```csharp
while ( expr ) while_body
```

*  *v* ma ten sam stan asercję określonego przypisania na początku *expr* podobnie jak na początku *instrukcji INSERT*.
*  Jeśli *v* jest zdecydowanie przypisany na końcu *expr*, a następnie jest zdecydowanie przypisany na przesyłanie przepływu sterowania do *while_body* i punkt końcowy  *instrukcji INSERT*.
*  Jeśli *v* ma stan "zdecydowanie przypisana, aby po wartość true, wyrażenie" na końcu *expr*, a następnie jest zdecydowanie przypisany na przesyłanie przepływu sterowania do *while_body*, ale nie Zdecydowanie przypisany na punktu końcowego *instrukcji INSERT*.
*  Jeśli *v* ma stan "zdecydowanie przypisana, aby po wyrażeniu false" na końcu *expr*, a następnie jest zdecydowanie przypisany na transfer przepływu sterowania do punktu końcowego *instrukcji INSERT* , ale zdecydowanie nie przypisano na przesyłanie przepływu sterowania do *while_body*.

#### <a name="do-statements"></a>Wykonaj instrukcje

Aby uzyskać `do` instrukcji *instrukcji INSERT* formularza:
```csharp
do do_body while ( expr ) ;
```

*  *v* ma ten sam stan asercję określonego przypisania przeniesienia przepływ sterowania od początku *instrukcji INSERT* do *do_body* podobnie jak na początku *instrukcji INSERT*.
*  *v* ma ten sam stan asercję określonego przypisania na początku *expr* na punkt końcowy *do_body*.
*  Jeśli *v* jest zdecydowanie przypisany na końcu *expr*, a następnie jest zdecydowanie przypisany na transfer przepływu sterowania do punktu końcowego *instrukcji INSERT*.
*  Jeśli *v* ma stan "zdecydowanie przypisana, aby po wyrażeniu false" na końcu *expr*, a następnie jest zdecydowanie przypisany na transfer przepływu sterowania do punktu końcowego *instrukcji INSERT* .

#### <a name="for-statements"></a>Aby uzyskać instrukcje

Asercja określonego przydziału sprawdzania pod kątem `for` instrukcji formularza:
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
odbywa się tak, jakby zostały napisane instrukcji:
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

Jeśli *for_condition* pominięto w `for` instrukcji, a następnie oceny asercję określonego przypisania kontynuowane tak, jakby *for_condition* zostały zastąpione `true` w rozwinięciu powyżej .

#### <a name="break-continue-and-goto-statements"></a>Przerwij, Kontynuuj i instrukcje goto

Stan asercję określonego przypisania *v* na transfer przepływu sterowania spowodowane `break`, `continue`, lub `goto` instrukcji jest taka sama jak stan asercję określonego przypisania *v* w Początek instrukcji.

#### <a name="throw-statements"></a>Throw — instrukcje

Dla instrukcji *instrukcji INSERT* formularza
```csharp
throw expr ;
```

Stan asercję określonego przypisania *v* na początku *expr* jest taka sama jak stan asercję określonego przypisania *v* na początku *instrukcji INSERT*.

#### <a name="return-statements"></a>Instrukcje powrotu

Dla instrukcji *instrukcji INSERT* formularza
```csharp
return expr ;
```

*  Stan asercję określonego przypisania *v* na początku *expr* jest taka sama jak stan asercję określonego przypisania *v* na początku *instrukcji INSERT*.
*  Jeśli *v* to parametr wyjściowy, a następnie go musi być zdecydowanie przypisana albo:
    * Po *expr*
    * lub na końcu `finally` bloku `try` - `finally` lub `try` - `catch` - `finally` który otacza `return` instrukcji.

Dla instrukcji INSERT instrukcji formularza:
```csharp
return ;
```

*  Jeśli *v* to parametr wyjściowy, a następnie go musi być zdecydowanie przypisana albo:
    * przed *instrukcji INSERT*
    * lub na końcu `finally` bloku `try` - `finally` lub `try` - `catch` - `finally` który otacza `return` instrukcji.

#### <a name="try-catch-statements"></a>Instrukcje try-catch

Dla instrukcji *instrukcji INSERT* formularza:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  Stan asercję określonego przypisania *v* na początku *try_block* jest taka sama jak stan asercję określonego przypisania *v* na początku *instrukcji INSERT*.
*  Stan asercję określonego przypisania *v* na początku *catch_block_i* (dla dowolnej *i*) jest taka sama jak stan asercję określonego przypisania *v*na początku *instrukcji INSERT*.
*  Stan asercję określonego przypisania *v* na punktu końcowego *instrukcji INSERT* jest zdecydowanie przypisany if (i tylko wtedy, gdy) *v* zdecydowanie przydzielono punktu końcowego  *try_block* , a następnie co *catch_block_i* (dla każdego *i* z zakresu od 1 do *n*).

#### <a name="try-finally-statements"></a>Try-finally-instrukcje

Aby uzyskać `try` instrukcji *instrukcji INSERT* formularza:
```csharp
try try_block finally finally_block
```

*  Stan asercję określonego przypisania *v* na początku *try_block* jest taka sama jak stan asercję określonego przypisania *v* na początku *instrukcji INSERT*.
*  Stan asercję określonego przypisania *v* na początku *finally_block* jest taka sama jak stan asercję określonego przypisania *v* na początku *instrukcji INSERT* .
*  Stan asercję określonego przypisania *v* na punktu końcowego *instrukcji INSERT* jest zdecydowanie przypisany if (i tylko wtedy, gdy) dotyczy co najmniej jeden z następujących czynności:
    * *v* zdecydowanie przydzielono punktu końcowego *try_block*
    * *v* zdecydowanie przydzielono punktu końcowego *finally_block*

Jeśli transfer przepływu sterowania (na przykład `goto` instrukcji) wykonano, który rozpoczyna się w ramach *try_block*i kończy się poza *try_block*, następnie *v* jest również uznawany za zdecydowanie przypisany na ten transfer przepływu sterowania, jeśli *v* zdecydowanie przydzielono punktu końcowego *finally_block*. (Nie jest to tylko wtedy, gdy — Jeśli *v* jest zdecydowanie przypisana z innego powodu na to przeniesienie przepływu sterowania, a następnie go jest nadal uważana za zdecydowanie przypisany.)

#### <a name="try-catch-finally-statements"></a>Instrukcje try-catch-finally

Analiza asercję określonego przypisania `try` - `catch` - `finally` instrukcji formularza:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
odbywa się tak, jakby były instrukcji `try` - `finally` otaczającej instrukcji `try` - `catch` instrukcji:
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

Poniższy przykład pokazuje, jak różne bloki `try` — instrukcja ([instrukcjami "try"](statements.md#the-try-statement)) wpływają na asercję określonego przypisania.
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

#### <a name="foreach-statements"></a>Instrukcji foreach

Aby uzyskać `foreach` instrukcji *instrukcji INSERT* formularza:
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  Stan asercję określonego przypisania *v* na początku *expr* jest taka sama jak stan *v* na początku *instrukcji INSERT*.
*  Stan asercję określonego przypisania *v* na przesyłanie przepływu sterowania do *embedded_statement* lub punkt końcowy *instrukcji INSERT* jest taka sama jak stan *v* na końcu *expr*.

#### <a name="using-statements"></a>Za pomocą instrukcji

Aby uzyskać `using` instrukcji *instrukcji INSERT* formularza:
```csharp
using ( resource_acquisition ) embedded_statement
```

*  Stan asercję określonego przypisania *v* na początku *resource_acquisition* jest taka sama jak stan *v* na początku *instrukcji INSERT*.
*  Stan asercję określonego przypisania *v* na przesyłanie przepływu sterowania do *embedded_statement* jest taka sama jak stan *v* na końcu *resource_ pozyskiwanie*.

#### <a name="lock-statements"></a>Blokady instrukcji

Aby uzyskać `lock` instrukcji *instrukcji INSERT* formularza:
```csharp
lock ( expr ) embedded_statement
```

*  Stan asercję określonego przypisania *v* na początku *expr* jest taka sama jak stan *v* na początku *instrukcji INSERT*.
*  Stan asercję określonego przypisania *v* na przesyłanie przepływu sterowania do *embedded_statement* jest taka sama jak stan *v* na końcu *expr*.

#### <a name="yield-statements"></a>Instrukcje YIELD

Aby uzyskać `yield return` instrukcji *instrukcji INSERT* formularza:
```csharp
yield return expr ;
```

*  Stan asercję określonego przypisania *v* na początku *expr* jest taka sama jak stan *v* na początku *instrukcji INSERT*.
*  Stan asercję określonego przypisania *v* na końcu *instrukcji INSERT* jest taka sama jak stan *v* na końcu *expr*.
*  A `yield break` instrukcji nie ma wpływu na stan asercję określonego przypisania.

#### <a name="general-rules-for-simple-expressions"></a>Ogólne zasady proste wyrażenia

Następująca reguła ma zastosowanie do tego rodzaju wyrażeń: literały ([literały](expressions.md#literals)), nazwy proste ([proste nazwy](expressions.md#simple-names)), wyrażenia dostępu do składowych ([dostęp do elementu członkowskiego](expressions.md#member-access)), wyrażenia dostępu bazowego nieindeksowaną ([podstawowa dostępu](expressions.md#base-access)), `typeof` wyrażenia ([typeof — operator](expressions.md#the-typeof-operator)), domyślna wartość wyrażenia ([wyrażenia wartości domyślne ](expressions.md#default-value-expressions)) i `nameof` wyrażenia ([wyrażeń Nameof](expressions.md#nameof-expressions)).

*  Stan asercję określonego przypisania *v* na końcu wyrażenia jest taka sama jak stan asercję określonego przypisania *v* na początku wyrażenia.

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>Ogólne reguły dotyczące wyrażeń z wyrażenia osadzone

Następujące reguły mają zastosowanie do tego rodzaju wyrażeń: wyrażenia ujętego w nawiasy ([wyrażeniach z nawiasami](expressions.md#parenthesized-expressions)), wyrażeniach dostępu do elementu ([dostępu do elementu](expressions.md#element-access)), podstawowej dostęp do wyrażenia z Indeksowanie ([podstawowa dostępu](expressions.md#base-access)), zwiększyć i zmniejszyć wyrażenia ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators), [prefiksów inkrementacji i dekrementacji operatory](expressions.md#prefix-increment-and-decrement-operators)), rzutowane wyrażenia ([rzutowane wyrażenia,](expressions.md#cast-expressions)), jednoargumentowe `+`, `-`, `~`, `*` wyrażenia binarnego `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` wyrażenia ([operatorów arytmetycznych](expressions.md#arithmetic-operators), [operatory przesunięcia](expressions.md#shift-operators), [relacyjne i badania typu operatory](expressions.md#relational-and-type-testing-operators) [Operatorów logicznych](expressions.md#logical-operators)), złożone wyrażenia przypisania ([przydział złożony](expressions.md#compound-assignment)), `checked` i `unchecked` wyrażenia ([checked i unchecked operatory](expressions.md#the-checked-and-unchecked-operators)), oraz wyrażeń tworzenia tablicy i delegata ([operatora new](expressions.md#the-new-operator)).

Każda z tych wyrażeń ma jeden lub więcej wyrażeń podrzędnych, które bezwarunkowo są obliczane w ustalonej kolejności. Na przykład plik binarny `%` operator ocenia po lewej stronie operatora, a następnie po prawej stronie. Operacja indeksowania oblicza wyrażenie indeksowane, a następnie ocenia każdy wyrażenia indeksu, w kolejności od lewej do prawej. Wyrażenie *expr*, który ma podrzędną wyrażeniach *e1 i e2,..., eN*, ocenione w podanej kolejności:

*  Stan asercję określonego przypisania *v* na początku *e1* jest taka sama jak stan asercję określonego przypisania na początku *expr*.
*  Stan asercję określonego przypisania *v* na początku *ei* (*i* więcej niż jeden) jest taka sama jak stan asercję określonego przypisania na końcu poprzednie Podwyrażenie.
*  Stan asercję określonego przypisania *v* na końcu *expr* jest taka sama jak stan asercję określonego przypisania na końcu *eN*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Wyrażenia wywołania i wyrażenia tworzenia obiektów

Wyrażenie wywołania *expr* formularza:
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
lub wyrażenie tworzenia obiektu w postaci:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  Wyrażenie wywołania, stan asercję określonego przypisania *v* przed *primary_expression* jest taka sama jak stan *v* przed *expr*.
*  Wyrażenie wywołania, stan asercję określonego przypisania *v* przed *arg1* jest taka sama jak stan *v* po *primary_expression*.
*  Wyrażenie tworzenia obiektu, stan asercję określonego przypisania *v* przed *arg1* jest taka sama jak stan *v* przed *expr*.
*  Dla każdego argumentu *argi*, stan asercję określonego przypisania *v* po *argi* jest określana przez reguły wyrażenie normalne, ignorowanie dowolne `ref` lub `out`modyfikatorów.
*  Dla każdego argumentu *argi* dla każdego *i* więcej niż jeden stan asercję określonego przypisania *v* przed *argi* jest taka sama jak stanu *v* po poprzednim *arg*.
*  Jeśli zmienna *v* jest przekazywany jako `out` argumentu (czyli argument w postaci `out v`) w jednym z argumentów, a następnie stan *v* po *expr* jest zdecydowanie przypisany. W przeciwnym razie; Stan *v* po *expr* jest taka sama jak stan *v* po *argN*.
*  Dla inicjatora tablicy ([wyrażenie tworzenia tablicy](expressions.md#array-creation-expressions)), inicjatorach obiektów ([inicjatorach obiektów](expressions.md#object-initializers)), inicjatory kolekcji ([inicjatory kolekcji](expressions.md#collection-initializers)) i Inicjatory obiektów anonimowe ([wyrażenia tworzenia obiektu anonimowego](expressions.md#anonymous-object-creation-expressions)), stan asercja określonego przydziału jest określana przez rozszerzenie, które te konstrukcje są definiowane w kategoriach.

#### <a name="simple-assignment-expressions"></a>Przypisanie proste wyrażenia

Wyrażenie *expr* formularza `w = expr_rhs`:

*  Stan asercję określonego przypisania *v* przed *expr_rhs* jest taka sama jak stan asercję określonego przypisania *v* przed *expr*.
*  Stan asercję określonego przypisania *v* po *expr* jest określana przez:
   * Jeśli *w* jest tę samą zmienną jako *v*, następnie asercję określonego przypisania stan *v* po *expr* jest zdecydowanie przypisany.
   * W przeciwnym razie, jeśli przypisania występuje w ramach konstruktora wystąpienia typu struktury, jeśli *w* jest dostęp do właściwości wyznaczanie automatycznie implementowanej właściwości *P* wystąpieniu budowany i *v* jest pole ukryte zapasowy *P*, następnie asercję określonego przypisania stan *v* po *expr* to przypisane.
   * W przeciwnym razie stan asercję określonego przypisania *v* po *expr* jest taka sama jak stan asercję określonego przypisania *v* po *expr_rhs*.

#### <a name="-conditional-and-expressions"></a>& & (oraz warunkowego) wyrażeń

Wyrażenie *expr* formularza `expr_first && expr_second`:

*  Stan asercję określonego przypisania *v* przed *expr_first* jest taka sama jak stan asercję określonego przypisania *v* przed *expr*.
*  Stan asercję określonego przypisania *v* przed *expr_second* jest zdecydowanie przypisana, jeśli stan *v* po *expr_first* jest Zdecydowanie przypisany lub "zdecydowanie przypisany po wartość true, wyrażenie". W przeciwnym razie nie jest zdecydowanie przydzielone.
*  Stan asercję określonego przypisania *v* po *expr* jest określana przez:
    * Jeśli *expr_first* jest wyrażeniem stałym wartością `false`, następnie asercję określonego przypisania stan *v* po *expr* jest taka sama jak asercję określonego przypisania Stan *v* po *expr_first*.
    * W przeciwnym razie, jeśli stan *v* po *expr_first* jest zdecydowanie przypisana, następnie stan *v* po *expr* jest zdecydowanie przypisany.
    * W przeciwnym razie, jeśli stan *v* po *expr_second* jest zdecydowanie przypisana, a stan *v* po *expr_first* to " przypisane po wyrażeniu false", a następnie stan *v* po *expr* jest zdecydowanie przypisany.
    * W przeciwnym razie, jeśli stan *v* po *expr_second* jest zdecydowanie przypisany lub "zdecydowanie przypisany po wartość true, wyrażenie", a następnie stan *v* po  *wyrażenie* jest "zdecydowanie przypisana po wartość true, wyrażenie".
    * W przeciwnym razie, jeśli stan *v* po *expr_first* jest "zdecydowanie przypisany po wyrażeniu false", a stan *v* po *expr_second* jest "zdecydowanie przypisany po wyrażeniu false", a następnie stan *v* po *expr* jest "zdecydowanie przypisana po wyrażeniu false".
    * W przeciwnym razie stan *v* po *expr* nie jest zdecydowanie przypisany.

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
Zmienna `i` jest uznawany za zdecydowanie przypisane w jednym z osadzonych instrukcji dla `if` instrukcji, ale nie w innych. W `if` instrukcji w metodzie `F`, zmienna `i` zdecydowanie jest przypisana w pierwszej instrukcji osadzony, ponieważ wykonywania wyrażenia `(i = y)` zawsze poprzedza wykonywania części tekstu niniejszych osadzonych. Z drugiej strony, zmienna `i` nie jest zdecydowanie przypisana w drugiej instrukcji osadzony, ponieważ `x >= 0` może zostały przetestowane "false", co w zmiennej `i` trwa nieprzypisane.

#### <a name="-conditional-or-expressions"></a>|| (OR warunkowe) wyrażeń

Wyrażenie *expr* formularza `expr_first || expr_second`:

*  Stan asercję określonego przypisania *v* przed *expr_first* jest taka sama jak stan asercję określonego przypisania *v* przed *expr*.
*  Stan asercję określonego przypisania *v* przed *expr_second* jest zdecydowanie przypisana, jeśli stan *v* po *expr_first* jest Zdecydowanie przypisany lub "zdecydowanie przypisany po wyrażeniu false". W przeciwnym razie nie jest zdecydowanie przydzielone.
*  Instrukcja asercję określonego przypisania *v* po *expr* jest określana przez:
    * Jeśli *expr_first* jest wyrażeniem stałym wartością `true`, następnie asercję określonego przypisania stan *v* po *expr* jest taka sama jak asercję określonego przypisania Stan *v* po *expr_first*.
    * W przeciwnym razie, jeśli stan *v* po *expr_first* jest zdecydowanie przypisana, następnie stan *v* po *expr* jest zdecydowanie przypisany.
    * W przeciwnym razie, jeśli stan *v* po *expr_second* jest zdecydowanie przypisana, a stan *v* po *expr_first* to " przypisane po wartość true, wyrażenie", a następnie stan *v* po *expr* jest zdecydowanie przypisany.
    * W przeciwnym razie, jeśli stan *v* po *expr_second* jest zdecydowanie przypisany lub "zdecydowanie przypisany po wyrażeniu false", a następnie stan *v* po *expr* jest "zdecydowanie przypisana po wyrażeniu false".
    * W przeciwnym razie, jeśli stan *v* po *expr_first* "zdecydowanie przypisany po wartość true, wyrażenie" i stan *v* po *expr_second*jest "zdecydowanie przypisany po wartość true, wyrażenie", a następnie stan *v* po *expr* jest "zdecydowanie przypisana po wartość true, wyrażenie".
    * W przeciwnym razie stan *v* po *expr* nie jest zdecydowanie przypisany.

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
Zmienna `i` jest uznawany za zdecydowanie przypisane w jednym z osadzonych instrukcji dla `if` instrukcji, ale nie w innych. W `if` instrukcji w metodzie `G`, zmienna `i` zdecydowanie jest przypisana w drugiej instrukcji osadzony, ponieważ wykonywania wyrażenia `(i = y)` zawsze poprzedza wykonywania części tekstu niniejszych osadzonych. Z drugiej strony, zmienna `i` nie jest zdecydowanie przypisana w pierwszej instrukcji osadzony, ponieważ `x >= 0` może zostały przetestowane ma wartość true, co w zmiennej `i` trwa nieprzypisane.

#### <a name="-logical-negation-expressions"></a>! wyrażenia (negacja logiczna)

Wyrażenie *expr* formularza `! expr_operand`:

*  Stan asercję określonego przypisania *v* przed *expr_operand* jest taka sama jak stan asercję określonego przypisania *v* przed *expr*.
*  Stan asercję określonego przypisania *v* po *expr* jest określana przez:
    * Jeśli stan *v* po * expr_operand * jest zdecydowanie przypisana, następnie stan *v* po *expr* jest zdecydowanie przypisany.
    * Jeśli stan *v* po * expr_operand * nie jest zdecydowanie przypisana, następnie stan *v* po *expr* nie jest zdecydowanie przypisany.
    * Jeśli stan *v* po * expr_operand * jest "zdecydowanie przypisany po wyrażeniu false", a następnie stan *v* po *expr* jest "zdecydowanie przypisana po true wyrażenie".
    * Jeśli stan *v* po * expr_operand * jest "zdecydowanie przypisany po wartość true, wyrażenie", a następnie stan *v* po *expr* jest "zdecydowanie przypisana po false wyrażenie".

#### <a name="-null-coalescing-expressions"></a>?? wyrażenia (łączenie wartości null)

Wyrażenie *expr* formularza `expr_first ?? expr_second`:

*  Stan asercję określonego przypisania *v* przed *expr_first* jest taka sama jak stan asercję określonego przypisania *v* przed *expr*.
*  Stan asercję określonego przypisania *v* przed *expr_second* jest taka sama jak stan asercję określonego przypisania *v* po *expr_first*.
*  Instrukcja asercję określonego przypisania *v* po *expr* jest określana przez:
    * Jeśli *expr_first* jest wyrażeniem stałym ([wyrażeń stałych](expressions.md#constant-expressions)) o wartości null, a następnie stan *v* po *expr* jest taka sama jak Stan *v* po *expr_second*.
*  W przeciwnym razie stan *v* po *expr* jest taka sama jak stan asercję określonego przypisania *v* po *expr_first*.

#### <a name="-conditional-expressions"></a>?: wyrażenia (warunkowe)

Wyrażenie *expr* formularza `expr_cond ? expr_true : expr_false`:

*  Stan asercję określonego przypisania *v* przed *expr_cond* jest taka sama jak stan *v* przed *expr*.
*  Stan asercję określonego przypisania *v* przed *expr_true* jest zdecydowanie przypisana tylko wtedy, gdy posiada jedną z następujących czynności:
    * *expr_cond* jest wyrażeniem stałym z wartością `false`
    * Stan *v* po *expr_cond* jest zdecydowanie przypisana, lub "zdecydowanie przypisana, aby po wartość true, wyrażenie".
*  Stan asercję określonego przypisania *v* przed *expr_false* jest zdecydowanie przypisana tylko wtedy, gdy posiada jedną z następujących czynności:
    * *expr_cond* jest wyrażeniem stałym z wartością `true`
*  Stan *v* po *expr_cond* jest zdecydowanie przypisana, lub "zdecydowanie przypisana, aby po wyrażeniu false".
*  Stan asercję określonego przypisania *v* po *expr* jest określana przez:
    * Jeśli *expr_cond* jest wyrażeniem stałym ([wyrażeń stałych](expressions.md#constant-expressions)) z wartością `true` następnie stan *v* po *expr* jest taka sama jak stan *v* po *expr_true*.
    * W przeciwnym razie, jeśli *expr_cond* jest wyrażeniem stałym ([wyrażeń stałych](expressions.md#constant-expressions)) z wartością `false` następnie stan *v* po *expr* jest taka sama jak stan *v* po *expr_false*.
    * W przeciwnym razie, jeśli stan *v* po *expr_true* jest zdecydowanie przypisany i stan *v* po *expr_false* to następnie przypisano mu stan *v* po *expr* jest zdecydowanie przypisany.
    * W przeciwnym razie stan *v* po *expr* nie jest zdecydowanie przypisany.

#### <a name="anonymous-functions"></a>Funkcje anonimowe

Aby uzyskać *lambda_expression* lub *anonymous_method_expression* *wyrażenie* z treścią (albo *bloku* lub *wyrażenia* ) *treści*:

*  Stan asercję określonego przypisania zewnętrzna zmienna *v* przed *treści* jest taka sama jak stan *v* przed *expr*. Oznacza to, że stan asercję określonego przypisania zmiennych zewnętrznych jest dziedziczona z kontekstu funkcja anonimowa.
*  Stan asercję określonego przypisania zewnętrzna zmienna *v* po *expr* jest taka sama jak stan *v* przed *expr*.

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
generuje błąd w czasie kompilacji od `max` nie jest zdecydowanie przypisany której jest zadeklarowana funkcja anonimowa. Przykład
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
również generuje błąd w czasie kompilacji od przypisania do `n` w funkcja anonimowa nie ma wpływu na stan asercję określonego przypisania `n` poza funkcja anonimowa.

## <a name="variable-references"></a>Odwołań do zmiennych

A *variable_reference* jest *wyrażenie* , zostanie sklasyfikowany jako zmienną. A *variable_reference* oznacza lokalizację magazynu, który jest możliwy do pobrania bieżącą wartość i przechowywać nową wartość.

```antlr
variable_reference
    : expression
    ;
```

W języku C i C++, *variable_reference* jest znany jako *l-wartości*.

## <a name="atomicity-of-variable-references"></a>Niepodzielność odwołań do zmiennych

Odczyty i zapisy następujące typy danych są niepodzielne: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`i typy referencyjne. Ponadto odczyty i zapisy typach wyliczeniowych z typu podstawowego na poprzedniej liście również są niepodzielne. Odczytuje i zapisuje je w innych typów, w tym `long`, `ulong`, `double`, i `decimal`, a także typy zdefiniowane przez użytkownika nie ma gwarancji niepodzielnych. Oprócz funkcji biblioteki przeznaczone do tego celu nie ma żadnej gwarancji elementu atomic odczytu modify-write, takie jak w przypadku inkrementacyjna lub dekrementacyjna.

