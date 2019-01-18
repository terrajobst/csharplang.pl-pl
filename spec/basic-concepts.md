---
ms.openlocfilehash: 1c3d05674f8f7b69e70e0d9e06021537fc45f7ed
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229946"
---
# <a name="basic-concepts"></a>Podstawowe pojęcia

## <a name="application-startup"></a>Uruchamianie aplikacji

Zestaw, który ma ***punktu wejścia*** nosi nazwę ***aplikacji***. Gdy aplikacja jest uruchomić nową ***domeny aplikacji*** zostanie utworzony. Kilka różne wystąpienia aplikacji mogą istnieć na tym samym komputerze, w tym samym czasie, a każde z nich ma swoje własne domeny aplikacji.

Domeny aplikacji umożliwia izolacji aplikacji według działających jako kontener dla stanu aplikacji. Domeny aplikacji działa jako kontener i granic dla typów zdefiniowanych w aplikacji i bibliotek klas, które są używane. Typy załadowane do jednej domeny aplikacji różnią się od tego samego typu, które są ładowane do innej domeny aplikacji, a wystąpienia obiektów, nie są bezpośrednio współużytkowane między domenami aplikacji. Na przykład każda domena aplikacji ma własną kopię statyczne zmienne dla tych typów, a Konstruktor statyczny dla typu jest uruchamiany co najwyżej raz na domeny aplikacji. Implementacje są bezpłatne zapewnienie specyficzne dla implementacji zasad i mechanizmów tworzenia i niszczenia domen aplikacji.

***Uruchamianie aplikacji*** występuje, gdy środowisko wykonawcze wywołuje wyznaczonym metody, która jest określany jako punkt wejścia aplikacji. Zawsze nosi nazwę tej metody punktu wejścia `Main`i może mieć jednej z następujących:

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

Jak widać, punkt wejścia opcjonalnie może zwrócić `int` wartość. To zwraca wartość jest używana do zakończenia aplikacji ([zakończenia aplikacji](basic-concepts.md#application-termination)).

Punkt wejścia opcjonalnie może mieć jeden formalny parametr. Parametr może mieć dowolną nazwę, ale typ parametru musi być `string[]`. Jeśli parametr formalny jest obecny, środowisko wykonawcze tworzy i przekazuje `string[]` argument zawierająca argumenty wiersza polecenia, które zostały określone podczas uruchomienia aplikacji. `string[]` Argument nigdy nie ma wartość null, ale może mieć długości zerowej, jeśli nie określono żadnych argumentów wiersza polecenia.

Od C# obsługuje metody przeciążenie, klasy lub struktury może zawierać wiele definicji niektóre metody, pod warunkiem każdy ma inny podpis. Jednak w ramach jednego programu, nie klasy lub struktury może zawierać więcej niż jedną metodę o nazwie `Main` kwalifikuje się której definicja może być ona używana jako punkt wejścia aplikacji. Inne przeciążone wersje `Main` są dozwolone, pod warunkiem mają więcej niż jeden parametr lub ich jedynym parametrem jest inny niż typ `string[]`.

Aplikacja może składać się z wielu klas lub struktur. Możliwe jest więcej niż jeden z tych klas lub struktur zawiera metodę o nazwie `Main` kwalifikuje się której definicja może być ona używana jako punkt wejścia aplikacji. W takich przypadkach mechanizm zewnętrznych (takich jak opcja kompilatora wiersza polecenia) może służyć do wybierz jedną z następujących `Main` metod jako punkt wejścia.

W języku C# każdej metody musi być zdefiniowany jako członek klasy lub struktury. Zazwyczaj deklarowana dostępność metody ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)) metody jest określana przez modyfikatory dostępu ([modyfikatorach dostępu](classes.md#access-modifiers)) określona w jego deklaracji i podobnie deklarowana dostępność typu jest określany przez modyfikatory dostępu w jego deklaracji. Aby danej metody danego typu jako możliwy do wywołania muszą być dostępne zarówno typ, jak i element członkowski. Jednak punkt wejścia aplikacji jest przypadkiem szczególnym. Ściślej mówiąc środowisko wykonawcze mają dostęp do punktu wejścia aplikacji, niezależnie od jego deklarowana dostępność metody i niezależnie od tego, deklarowana dostępność metody jego otaczającego deklaracje typu.

Metody punktu wejścia aplikacji może nie być w deklaracji klasy ogólnej.

Pod innymi względami metodach punktu wejścia zachowują się jak te, które nie są punkty wejścia.

## <a name="application-termination"></a>Kończenie działania aplikacji

***Kończenie działania aplikacji*** przekazuje sterowanie do środowiska wykonawczego.

Jeśli zwracany typ metody aplikacji ***punktu wejścia*** metodą jest `int`, wartość zwracana służy jako aplikacji ***kod stanu zakończenia***. Celem tego kodu jest do zezwalania na komunikację powodzenia lub niepowodzenia do środowiska wykonawczego.

Jeśli zwracany typ metody punktu wejścia jest `void`, osiągając prawy nawias klamrowy (`}`), który kończy metody lub wykonywania `return` instrukcję, która ma nie wyrażenie powoduje zakończenie kod stanu `0`.

Przed zakończeniem aplikacji, wszystkie jego obiekty, które nie zostały jeszcze bezużyteczne destruktory są wywoływane, chyba że oczyszczania takie zostały pominięte (przez wywołanie metody biblioteki `GC.SuppressFinalize`, na przykład).

## <a name="declarations"></a>Deklaracje

Deklaracje w języku C# służącego Definiowanie elementów programu. C# programy są zorganizowane przy użyciu przestrzeni nazw ([przestrzenie nazw](namespaces.md)), który może zawierać typ deklaracje i deklaracji zagnieżdżonej przestrzeni nazw. Wpisz deklaracje ([wpisz deklaracje](namespaces.md#type-declarations)) są używane do definiowania klas ([klasy](classes.md)), struktur ([struktury](structs.md)), interfejsy ([interfejsów](interfaces.md) ), Typy wyliczeniowe ([wyliczenia](enums.md)) i delegatów ([delegatów](delegates.md)). Rodzaje elementy członkowskie dozwolone w deklaracji typu są zależne od postaci deklaracji typu. Na przykład deklaracje klas mogą zawierać deklaracje dla stałych ([stałe](classes.md#constants)), pola ([pola](classes.md#fields)), metody ([metody](classes.md#methods)), właściwości ([ Właściwości](classes.md#properties)), zdarzenia ([zdarzenia](classes.md#events)), indeksatorów ([indeksatory](classes.md#indexers)), operatory ([operatory](classes.md#operators)), wystąpienie konstruktory ([ Konstruktory wystąpień](classes.md#instance-constructors)), konstruktory statyczne ([konstruktorów statycznych](classes.md#static-constructors)), destruktory ([destruktory](classes.md#destructors)) i zagnieżdżone typy ([zagnieżdżone typy](classes.md#nested-types)).

Deklaracja Określa nazwę w ***deklaracji przestrzeni*** do której należy deklaracji. Z wyjątkiem przeciążone elementy członkowskie ([podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading)), jest to błąd czasu kompilacji, mieć co najmniej dwóch deklaracje, które wprowadzają składowych o tej samej nazwie w miejscu do deklaracji. Nigdy nie jest możliwe dla miejsca deklaracji w celu uwzględnienia różnych rodzajów elementów członkowskich o takiej samej nazwie. Na przykład miejsca deklaracja może nigdy nie zawierać pola i metody o tej samej nazwie.

Istnieje kilka różnych typów miejsc do magazynowania deklaracji, zgodnie z opisem w następujących.

*  W ramach wszystkich plików źródłowych programu *namespace_member_declaration*s nie otaczający *namespace_declaration* są elementami członkowskimi miejsca jednej Scalonej deklaracji, nazywanego ***globalne deklaracja przestrzeni***.
*  W ramach wszystkich plików źródłowych programu *namespace_member_declaration*s w ramach *namespace_declaration*s, który ma taką samą nazwę w pełni kwalifikowanych przestrzeni nazw są członkami jednej deklaracji połączone miejsca.
*  Każdej klasy, struktury lub interfejsu deklaracja tworzy nowe miejsce do deklaracji. Nazwy są wprowadzane do tego obszaru deklaracji za pośrednictwem *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, lub *type_parameter*s. Z wyjątkiem konstruktora wystąpienia przeciążona deklaracje i Konstruktor statyczny deklaracji klasy lub struktury nie mogą zawierać deklaracji elementu członkowskiego z taką samą nazwę jak klasy lub struktury. Klasy, struktury lub interfejsu zezwala na deklarację przeciążonych metod i indeksatorów. Ponadto klasy lub struktury pozwala na deklarację operatory i konstruktory wystąpień przeciążona. Na przykład klasy, struktury lub interfejsu może zawierać wiele deklaracje metody o tej samej nazwie, pod warunkiem te deklaracje metody różnią się w ich podpisu ([podpisów i przeciążenie](basic-concepts.md#signatures-and-overloading)). Należy pamiętać, że klasy bazowe nie przyczyniają się do obszaru deklaracji klasy i interfejsy podstawowe nie przyczyniają się do obszaru deklaracji interfejsu. W efekcie pochodne klasy lub interfejsu może zadeklarować członka o takiej samej nazwie jak dziedziczonego członka. Członek taki jest nazywany ***Ukryj*** dziedziczonego elementu członkowskiego.
*  Każdy deklaracja delegata tworzy nowe miejsce do deklaracji. Nazwy są wprowadzane do tego obszaru deklaracji parametrów formalnych (*fixed_parameter*s i *parameter_array*s) i *type_parameter*s.
*  Każda deklaracja wyliczenia tworzy nowe miejsce do deklaracji. Nazwy są wprowadzane do tego obszaru deklaracji za pośrednictwem *enum_member_declarations*.
*  Każdy deklaracji metody, deklaracja indeksatora, Deklaracja operatora, deklaracja konstruktora wystąpienia i funkcja anonimowa tworzy nowe miejsce deklaracji o nazwie ***deklaracji zmiennej lokalnej przestrzeni***. Nazwy są wprowadzane do tego obszaru deklaracji parametrów formalnych (*fixed_parameter*s i *parameter_array*s) i *type_parameter*s. Treści funkcji składowej lub funkcja anonimowa, jeśli istnieje, jest uważany za można zagnieździć w obrębie przestrzeni deklaracji zmiennej lokalnej. Jest to błąd dla deklaracji zmiennej lokalnej i miejsca zagnieżdżonych deklaracji zmiennej lokalnej, aby zawierać elementy o takiej samej nazwie. W związku z tym w obrębie deklaracji zagnieżdżonej przestrzeni nie jest możliwe zadeklarować zmienną lub stałą o takiej samej nazwie jako zmiennej lokalnej lub stałą lokalną w otaczającej przestrzeni deklaracji. Istnieje możliwość dla dwóch spacji deklaracji ma zawierać elementy o takiej samej nazwie, tak długo, jak ani miejsca deklaracja zawiera drugi.
*  Każdego *bloku* lub *switch_block* , a także *dla*, *foreach* i *przy użyciu* instrukcja, tworzy deklaracji zmiennej lokalnej miejsca dla lokalnych stałe i zmienne lokalne. Nazwy są wprowadzane do tego obszaru deklaracji za pośrednictwem *local_variable_declaration*s i *local_constant_declaration*s. Należy pamiętać, że bloki, które występują jako lub w treści funkcji składowej lub funkcja anonimowa są zagnieżdżone w obrębie przestrzeni deklaracji zmiennej lokalnej zgłoszonych przez te funkcje do swoich parametrów. Ten sposób jest błędem np. metody przy użyciu zmiennej lokalnej i parametru o takiej samej nazwie.
*  Każdy *bloku* lub *switch_block* tworzy miejsce na oddzielnych deklaracji etykiety. Nazwy są wprowadzane do tego obszaru deklaracji za pośrednictwem *labeled_statement*s i nazw, które są wywoływane za pośrednictwem *goto_statement*s. ***Etykiety deklaracji przestrzeni*** bloku zawiera wszelkich bloków zagnieżdżonych. W związku z tym w ramach zagnieżdżony blok nie jest możliwe do deklarowania etykietę o takiej samej nazwie jako etykieta w otaczającym bloku.

Ogólnie tekstową kolejność, w których nazwy są deklarowane jest bez znaczenia. W szczególności tekstową kolejność nie jest istotne dla deklaracji i korzystania z przestrzeni nazw, stałe, metody, właściwości, zdarzenia, indeksatory, operatory, konstruktory wystąpień, destruktory, konstruktory statyczne i typów. Deklaracja kolejność jest ważna w następujący sposób:

*  Kolejność deklaracji dla deklaracji pól i deklaracje zmiennych lokalnych określa kolejność wykonywania ich inicjatory (jeśli istnieje).
*  Muszą być zdefiniowane zmienne lokalne, zanim zostaną użyte ([zakresy](basic-concepts.md#scopes)).
*  Kolejności deklaracji dla deklaracji elementu członkowskiego wyliczenia ([typu wyliczeniowego](enums.md#enum-members)) ma znaczenie, gdy *constant_expression* wartości są pomijane.

Miejsce na deklarację przestrzeni nazw jest "Otwórz Zakończono", a dwie przestrzeni nazw deklaracje o takiej samej nazwie FQDN przyczyniają się do tej samej deklaracji przestrzeni. Na przykład
```csharp
namespace Megacorp.Data
{
    class Customer
    {
        ...
    }
}

namespace Megacorp.Data
{
    class Order
    {
        ...
    }
}
```

dwie deklaracje przestrzeni nazw powyżej przyczyniają się do tej samej przestrzeni deklaracji, w tym przypadku deklarowania dwóch klas z w pełni kwalifikowane nazwy `Megacorp.Data.Customer` i `Megacorp.Data.Order`. Ponieważ dwie deklaracje przyczynić się do tej samej przestrzeni deklaracji, jego spowodowałaby błąd kompilacji w przypadku każdego zawartych w deklaracji klasy o takiej samej nazwie.

Jak określono powyżej w deklaracji przestrzeni blok zawiera wszelkich bloków zagnieżdżonych. W związku z tym, w poniższym przykładzie `F` i `G` metody powoduje błąd w czasie kompilacji, ponieważ nazwa `i` jest zadeklarowany w zewnętrznym bloku i nie można ponownie zadeklarować w bloku wewnętrznego. Jednak `H` i `I` metody są prawidłowe, ponieważ dwa `i`firmy są deklarowane w osobnych blokach-nested.

```csharp
class A
{
    void F() {
        int i = 0;
        if (true) {
            int i = 1;            
        }
    }

    void G() {
        if (true) {
            int i = 0;
        }
        int i = 1;                
    }

    void H() {
        if (true) {
            int i = 0;
        }
        if (true) {
            int i = 1;
        }
    }

    void I() {
        for (int i = 0; i < 10; i++)
            H();
        for (int i = 0; i < 10; i++)
            H();
    }
}
```

## <a name="members"></a>Elementy członkowskie

Obszary nazw i typy mają ***członków***. Elementy członkowskie jednostki są ogólnie dostępne w ramach kwalifikowana nazwa, która rozpoczyna się od odwołanie do jednostki, a następnie "`.`" token, a następnie według nazwy elementu członkowskiego.

Elementy członkowskie typu albo są deklarowane w deklaracji typu lub ***dziedziczone*** z klasy bazowej tego typu. Jeśli typ dziedziczy z klasy bazowej, wszystkich elementów członkowskich klasy podstawowej, z wyjątkiem wystąpienia konstruktory, destruktory i konstruktorów statycznych stają się elementy członkowskie typu pochodnego. Deklarowaną dostępność składowej klasy bazowej nie kontroluje, czy element członkowski jest dziedziczony — dziedziczenie rozszerza się do wszystkich elementów członkowskich, którego nie ma konstruktora wystąpienia, statyczny Konstruktor lub destruktor. Jednak dziedziczonego członka nie mogą być dostępne w typie pochodnym, albo z powodu jego deklarowana dostępność metody ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)) lub ponieważ jest on ukryty przez deklarację w typie ([ukrywanie przez dziedziczenie](basic-concepts.md#hiding-through-inheritance)).

### <a name="namespace-members"></a>Elementy członkowskie Namespace

Obszary nazw i typy, które mają bez otaczającej przestrzeni nazw są elementami członkowskimi ***globalnej przestrzeni nazw***. Odpowiada to bezpośrednio nazwy zadeklarowane w deklaracji globalnej przestrzeni.

Obszary nazw i typy zadeklarowane w przestrzeni nazw są elementami członkowskimi tej przestrzeni nazw. Odpowiada to bezpośrednio nazwy zadeklarowane w obszarze deklaracji przestrzeni nazw.

Przestrzenie nazw nie ma ograniczeń dostępu. Nie jest możliwe zadeklarować prywatny, chroniony lub wewnętrzny przestrzenie nazw i nazwy przestrzeni nazw są zawsze dostępne publicznie.

### <a name="struct-members"></a>Składowe struktury

Elementy członkowskie struktury są elementów członkowskich zadeklarowanych w strukturze i elementy członkowskie dziedziczone po bezpośredniej klasie bazowej struktury `System.ValueType` i pośredniej klasy bazowej `object`.

Elementy członkowskie typu prostego odpowiadają bezpośrednio do elementów członkowskich struktury typ aliasowany przez typ prosty:

*  Elementy członkowskie `sbyte` należą do `System.SByte` struktury.
*  Elementy członkowskie `byte` należą do `System.Byte` struktury.
*  Elementy członkowskie `short` należą do `System.Int16` struktury.
*  Elementy członkowskie `ushort` należą do `System.UInt16` struktury.
*  Elementy członkowskie `int` należą do `System.Int32` struktury.
*  Elementy członkowskie `uint` należą do `System.UInt32` struktury.
*  Elementy członkowskie `long` należą do `System.Int64` struktury.
*  Elementy członkowskie `ulong` należą do `System.UInt64` struktury.
*  Elementy członkowskie `char` należą do `System.Char` struktury.
*  Elementy członkowskie `float` należą do `System.Single` struktury.
*  Elementy członkowskie `double` należą do `System.Double` struktury.
*  Elementy członkowskie `decimal` należą do `System.Decimal` struktury.
*  Elementy członkowskie `bool` należą do `System.Boolean` struktury.

### <a name="enumeration-members"></a>Elementy członkowskie wyliczenia

Elementy członkowskie wyliczenia są stałe zadeklarowane w wyliczeniu i elementy członkowskie dziedziczone po bezpośredniej klasie bazowej wyliczenia `System.Enum` i pośrednie klas bazowych `System.ValueType` i `object`.

### <a name="class-members"></a>Elementy członkowskie klasy

Elementy członkowskie klasy są elementów członkowskich zadeklarowanych w klasie i elementy członkowskie dziedziczone z klasy podstawowej (z wyjątkiem klasy `object` którego nie ma klasy bazowej). Elementy członkowskie, dziedziczone z klasy bazowej zawierają, stałe, pola, metody, właściwości, zdarzenia, indeksatory, operatorów i typów klasy bazowej, ale nie konstruktory wystąpień, destruktory i konstruktory statyczne klasy bazowej. Klasa bazowa członkowie są dziedziczeni bez względu na ich dostępność.

Deklaracja klasy może zawierać deklaracje stałe, pola, metody, właściwości, zdarzenia, indeksatory, operatorów, konstruktory wystąpień, destruktory, konstruktory statyczne i typów.

Elementy członkowskie `object` i `string` odpowiadają bezpośrednio do elementów członkowskich typu klasy one aliasu:

*  Elementy członkowskie `object` należą do `System.Object` klasy.
*  Elementy członkowskie `string` należą do `System.String` klasy.

### <a name="interface-members"></a>Elementy członkowskie interfejsu

Członkowie interfejsu są elementów członkowskich zadeklarowanych w interfejsie i wszystkie interfejsy podstawowe interfejsu. Elementy członkowskie w klasie `object` nie są ściśle rzecz biorąc, elementy członkowskie dowolnego interfejsu ([interfejsu członków](interfaces.md#interface-members)). Jednak elementy członkowskie w klasie `object` są dostępne za pośrednictwem wyszukać składowej w dowolny typ interfejsu ([wyszukanie członka](expressions.md#member-lookup)).

### <a name="array-members"></a>Elementy członkowskie tablicy

Elementy członkowskie tablicy są elementami członkowskimi, odziedziczoną z klasy `System.Array`.

### <a name="delegate-members"></a>Elementy członkowskie delegata

Elementy członkowskie obiektu delegowanego są elementami członkowskimi, odziedziczoną z klasy `System.Delegate`.

## <a name="member-access"></a>Dostęp do elementu członkowskiego

Deklaracje członków umożliwia kontrolę dostępu do elementu członkowskiego. Ułatwienia dostępu elementu członkowskiego jest ustanawiane przez deklarowana dostępność metody ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)) elementu członkowskiego w połączeniu z ułatwieniami dostępu typu zawierającego ewentualne.

Dostęp do określonego elementu członkowskiego jest dozwolony, element członkowski jest określane jako ***dostępny***. Z drugiej strony, dostęp do określonego elementu członkowskiego jest niedozwolona, element członkowski jest określane jako ***niedostępny***. Dostęp do elementu członkowskiego jest dozwolone, gdy tekstową lokalizacji, w którym odbywa się dostępu znajduje się w domenie ułatwień dostępu ([domeny dostępności](basic-concepts.md#accessibility-domains)) elementu członkowskiego.

### <a name="declared-accessibility"></a>Deklarowana dostępność metody

***Zadeklarowana dostępność*** członka może być jedną z następujących czynności:

*  Publiczny, który jest wybierany przez dołączenie `public` modyfikatora w deklaracji elementu członkowskiego. Intuicyjne znaczenie `public` jest "nie jest ograniczony dostęp".
*  Chroniony, który jest zaznaczony, w tym `protected` modyfikatora w deklaracji elementu członkowskiego. Intuicyjne znaczenie `protected` "dostęp ograniczony do zawierający klasy lub typów pochodzi od klasy zawierającej".
*  Wewnętrzny, który jest wybierany przez dołączenie `internal` modyfikatora w deklaracji elementu członkowskiego. Intuicyjne znaczenie `internal` jest "dostęp ograniczony do tego programu".
*  Chronionych wewnętrznych (to znaczy chronionych i wewnętrznych), który jest zaznaczony, tym `protected` i `internal` modyfikatora w deklaracji elementu członkowskiego. Intuicyjne znaczenie `protected internal` jest "dostęp ograniczony do tego programu lub typy pochodzące z klasy zawierającej".
*  Prywatne, który jest wybierany przez dołączenie `private` modyfikatora w deklaracji elementu członkowskiego. Intuicyjne znaczenie `private` jest "dostęp ograniczony do typu zawierającego".

W zależności od kontekstu, w którym odbywa się deklaracji składowej umieścić, dozwolone są tylko niektóre typy deklarowana dostępność metody. Ponadto po deklaracji elementu członkowskiego nie zawiera żadnych modyfikatory dostępu, kontekst, w którym odbywa się deklaracja określa domyślne zadeklarowana ułatwień dostępu.

*  Przestrzenie nazw niejawnie mają `public` zadeklarowana ułatwień dostępu. Brak modyfikatorów dostępu są dozwolone w deklaracji przestrzeni nazw.
*  Typy zadeklarowane w jednostkach kompilacji lub przestrzeni nazw mogą mieć `public` lub `internal` zadeklarowana dostępność i domyślnie `internal` zadeklarowana ułatwień dostępu.
*  Elementy członkowskie klasy można istnieje pięć rodzajów deklarowana dostępność metody i domyślnie `private` zadeklarowana ułatwień dostępu. (Należy pamiętać, że typem zadeklarowane jako składowej klasy może być spowodowany pięć rodzajów deklarowana dostępność metody, natomiast typ zadeklarowane jako składowa klasy przestrzeni nazw może mieć tylko `public` lub `internal` zadeklarowana ułatwień dostępu.)
*  Składowe struktury mogą mieć `public`, `internal`, lub `private` zadeklarowana dostępność i domyślnie `private` zadeklarowany ułatwień dostępu, ponieważ struktury niejawnie są zamknięte. Składowe struktury wprowadzone w strukturze (który nie jest dziedziczone przez tej struktury) nie może mieć `protected` lub `protected internal` zadeklarowana ułatwień dostępu. (Należy pamiętać, że typem zadeklarowane jako składowa struktury mogą mieć `public`, `internal`, lub `private` zadeklarowana dostępność, natomiast typ zadeklarowane jako składowa klasy przestrzeni nazw może mieć tylko `public` lub `internal` zadeklarowana ułatwień dostępu.)
*  Elementy członkowskie interfejsu niejawnie ma `public` zadeklarowana ułatwień dostępu. Brak modyfikatorów dostępu są dozwolone w deklaracji elementu członkowskiego interfejsu.
*  Elementy członkowskie wyliczenia niejawnie ma `public` zadeklarowana ułatwień dostępu. Brak modyfikatorów dostępu są dozwolone w deklaracji elementu członkowskiego wyliczenia.

### <a name="accessibility-domains"></a>Domen ułatwień dostępu

***Domena dostępności*** elementu członkowskiego składa się z części (prawdopodobnie rozłączne) tekst programu, w którym jest dozwolony dostęp do elementu członkowskiego. W celu definiowania domena dostępności członka, członek jest nazywany ***najwyższego poziomu*** Jeśli nie jest zadeklarowany w ramach danego typu, a członek jest nazywany ***zagnieżdżonych*** Jeśli zostanie ona zadeklarowana w ramach innego typu. Ponadto ***program tekstu*** programu w języku jest zdefiniowana jako wszystkich programów tekst zawarty we wszystkich plikach źródłowych programu i tekstu programu o typie jest zdefiniowany jako wszystkich programów tekst zawarty w *type_declaration*s tego typu (oraz, ewentualnie typy, które są zagnieżdżone w obrębie typu).

Domena dostępności wstępnie zdefiniowany typ (takie jak `object`, `int`, lub `double`) jest nieograniczona.

Domena dostępności najwyższego poziomu niepowiązany typ `T` ([powiązane i wskazanych typów](types.md#bound-and-unbound-types)) zadeklarowanego w programie `P` jest zdefiniowana w następujący sposób:

*  Jeśli deklarowana dostępność metody `T` jest `public`, domena dostępności `T` znajduje się tekst program `P` i dowolnego programu, który odwołuje się do `P`.
*  Jeśli deklarowana dostępność metody `T` jest `internal`, domena dostępności `T` znajduje się tekst program `P`.

Z tymi definicjami wynika, że domena dostępności typu niepowiązanego najwyższego poziomu jest zawsze co najmniej programu, który typ tekstu jest zadeklarowana.

Domena dostępności do skonstruowanego typu `T<A1, ..., An>` jest część wspólną domeny dostępności niepowiązanych typu ogólnego `T` i domeny dostępności argumentów typu `A1, ..., An`.

Domena dostępności członka zagnieżdżonego `M` zadeklarowane w typie `T` w programie `P` jest zdefiniowany następująco (biorąc pod uwagę, że `M` sam prawdopodobnie może być typem):

*  Jeśli deklarowana dostępność metody `M` jest `public`, domena dostępności `M` jest domena dostępności `T`.
*  Jeśli deklarowana dostępność metody `M` jest `protected internal`systemowi `D` stanowią połączenie i tekstu programu `P` i tekstu programu o dowolnym typie pochodnych `T`, która jest zadeklarowana poza `P`. Domena dostępności `M` jest część wspólną domeny dostępności `T` z `D`.
*  Jeśli deklarowana dostępność metody `M` jest `protected`systemowi `D` stanowią połączenie i tekstu programu `T` i tekstu programu o dowolnym typie pochodnych `T`. Domena dostępności `M` jest część wspólną domeny dostępności `T` z `D`.
*  Jeśli deklarowana dostępność metody `M` jest `internal`, domena dostępności `M` jest część wspólną domeny dostępności `T` tekstu programu `P`.
*  Jeśli deklarowana dostępność metody `M` jest `private`, domena dostępności `M` znajduje się tekst program `T`.

Z tymi definicjami wynika, że domena dostępności członka zagnieżdżonego jest zawsze co najmniej tekstu programu o typie, w którym zadeklarowany jest element członkowski. Ponadto wynika, że domena dostępności członka się nigdy nie więcej niż domena dostępności typu, w którym zadeklarowany jest element członkowski.

W sposób intuicyjny gdy typ lub element członkowski `M` jest dostępne, aby upewnić się, że taki dostęp jest dozwolony są oceniane następujące kroki:

*  Po pierwsze, jeśli `M` jest zadeklarowany w typie (w przeciwieństwie do jednostki kompilacji lub przestrzeni nazw), występuje błąd kompilacji, jeśli ten typ nie jest dostępny.
*  Następnie, jeśli `M` jest `public`, dostęp jest dozwolony.
*  W przeciwnym razie, jeśli `M` jest `protected internal`, dostęp jest dozwolony, jeśli ma miejsce w programie, w którym `M` zadeklarowano, lub jeśli nastąpi to w ramach klasy pochodnej klasy, w którym `M` jest zadeklarowana i odbywa się za pośrednictwem pochodnej Typ klasy ([chronionego dostępu dla wystąpienia elementów członkowskich](basic-concepts.md#protected-access-for-instance-members)).
*  W przeciwnym razie, jeśli `M` jest `protected`, dostęp jest dozwolony, jeżeli wystąpi w ramach klasy, w której `M` zadeklarowano, lub jeśli nastąpi to w ramach klasy pochodnej klasy, w którym `M` jest zadeklarowana i odbywa się za pośrednictwem pochodnej Typ klasy ([chronionego dostępu dla wystąpienia elementów członkowskich](basic-concepts.md#protected-access-for-instance-members)).
*  W przeciwnym razie, jeśli `M` jest `internal`, dostęp jest dozwolony, jeśli ma miejsce w programie, w którym `M` jest zadeklarowana.
*  W przeciwnym razie, jeśli `M` jest `private`, dostęp jest dozwolony, jeżeli wystąpi w ramach typu, w którym `M` jest zadeklarowana.
*  W przeciwnym razie typu lub elementu członkowskiego jest niedostępny i występuje błąd kompilacji.

W przykładzie
```csharp
public class A
{
    public static int X;
    internal static int Y;
    private static int Z;
}

internal class B
{
    public static int X;
    internal static int Y;
    private static int Z;

    public class C
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }

    private class D
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }
}
```
klasy i członkowie mają następujące domeny dostępności:

*  Domena dostępności `A` i `A.X` jest nieograniczona.
*  Domena dostępności `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, i `B.C.Y` to tekst programu, zawierającego programu.
*  Domena dostępności `A.Z` znajduje się tekst program `A`.
*  Domena dostępności `B.Z` i `B.D` znajduje się tekst program `B`, w tym i tekstu programu `B.C` i `B.D`.
*  Domena dostępności `B.C.Z` znajduje się tekst program `B.C`.
*  Domena dostępności `B.D.X` i `B.D.Y` znajduje się tekst program `B`, w tym i tekstu programu `B.C` i `B.D`.
*  Domena dostępności `B.D.Z` znajduje się tekst program `B.D`.

Tak jak pokazano w przykładzie, domena dostępności członka nigdy nie jest większa niż w przypadku typu zawierającego. Na przykład nawet jeśli wszystkie `X` członkowie mają publiczne deklarowana dostępność metody wszystkie elementy oprócz `A.X` domen ułatwień dostępu, które są ograniczone przez typ zawierający.

Zgodnie z opisem w [członków](basic-concepts.md#members), wszystkie elementy członkowskie klasy bazowej, chyba że dla wystąpienia konstruktorów, destruktorów i konstruktorów statycznych są dziedziczone przez typy pochodne. Dotyczy to nawet prywatnych składowych klasy bazowej. Jednakże domena dostępności członka prywatnych umożliwia stosowanie tylko tekstu programu, typu, w którym zadeklarowany jest element członkowski. W przykładzie
```csharp
class A
{
    int x;

    static void F(B b) {
        b.x = 1;        // Ok
    }
}

class B: A
{
    static void F(B b) {
        b.x = 1;        // Error, x not accessible
    }
}
```
`B` klasa dziedziczy prywatnego elementu członkowskiego `x` z `A` klasy. Ponieważ element członkowski jest prywatny, jest ona dostępna tylko w ramach *class_body* z `A`. W związku z tym, dostęp do `b.x` zakończy się pomyślnie w `A.F` metody, ale kończy się niepowodzeniem w `B.F` metody.

### <a name="protected-access-for-instance-members"></a>Chronionego dostępu do składowych wystąpienia

Gdy `protected` elementu członkowskiego wystąpienia odbywa się poza tekst programu, klasy, w której jest zadeklarowana, i kiedy `protected internal` elementu członkowskiego wystąpienia odbywa się poza i tekstu programu, w której jest zadeklarowana, dostępu musi odbywać się w ciągu deklaracji klasy, który pochodzi od klasy, w którym jest zdeklarowana. Ponadto dostęp jest wymagany do odbywać się za pośrednictwem wystąpienia tego typu klasy pochodnej lub typu klasy skonstruowany z niego. To ograniczenie zapobiega uzyskiwania dostępu do chronionych członków innych klas pochodnych, nawet wtedy, gdy elementy członkowskie są dziedziczone z klasy bazowej tego samego jednej klasy pochodnej.

Pozwól `B` jako klasa bazowa, która deklaruje element członkowski chronionego wystąpienia `M`i pozwól `D` być klasą pochodzącą z `B`. W ramach *class_body* z `D`, dostęp do `M` może mieć jedną z następujących form:

*  Niekwalifikowane *type_name* lub *primary_expression* formularza `M`.
*  A *primary_expression* formularza `E.M`, podany typ `E` jest `T` lub klasą pochodną `T`, gdzie `T` jest typem klasy `D`, ani typem klasy zbudowany z `D`
*  A *primary_expression* formularza `base.M`.

Oprócz poniższych metod dostępu do klasy pochodnej mogą uzyskiwać dostęp do konstruktora chronionego wystąpienia w klasie bazowej *constructor_initializer* ([inicjatory konstruktora](classes.md#constructor-initializers)).

W przykładzie
```csharp
public class A
{
    protected int x;

    static void F(A a, B b) {
        a.x = 1;        // Ok
        b.x = 1;        // Ok
    }
}

public class B: A
{
    static void F(A a, B b) {
        a.x = 1;        // Error, must access through instance of B
        b.x = 1;        // Ok
    }
}
```
w ramach `A`, istnieje możliwość, aby uzyskać dostęp do `x` za pośrednictwem obu wystąpień `A` i `B`, ponieważ w obu przypadkach dostępu odbywa się za pośrednictwem wystąpienia `A` lub klasą pochodną `A`. Jednak w ramach `B`, nie jest możliwe, aby uzyskać dostęp do `x` za pośrednictwem wystąpienia `A`, ponieważ `A` nie pochodzi od `B`.

W przykładzie
```csharp
class C<T>
{
    protected T x;
}

class D<T>: C<T>
{
    static void F() {
        D<T> dt = new D<T>();
        D<int> di = new D<int>();
        D<string> ds = new D<string>();
        dt.x = default(T);
        di.x = 123;
        ds.x = "test";
    }
}
```
trzy przypisania do `x` są dozwolone, ponieważ wszystkie wykonane za pomocą wystąpienia typów klas skonstruowany z typu ogólnego.

### <a name="accessibility-constraints"></a>Ograniczenia ułatwień dostępu

Kilka konstrukcji w języku C# wymaga użycia typu jako ***co najmniej tak samo dostępna jak*** członka lub innego typu. Typ `T` jest określany jako co najmniej tak samo dostępna jak elementu członkowskiego lub typu `M` Jeśli domena dostępności `T` jest podzbiorem domena dostępności `M`. Innymi słowy `T` jest co najmniej tak samo dostępna jak `M` Jeśli `T` jest dostępny we wszystkich kontekstach, w którym `M` jest dostępny.

Istnieją następujące ograniczenia ułatwień dostępu:

*  Bezpośrednie klasy bazowej typu klasy musi być co najmniej tak samo dostępna jak samego typu klasy.
*  Jawne interfejsy podstawowe typu interfejsu, musi być co najmniej tak samo dostępna jak samego typu interfejsu.
*  Typ zwracany i typy parametrów typu delegowanego musi być co najmniej tak samo dostępna jak samego typu delegata.
*  Typ stałej musi być co najmniej tak samo dostępna jak stała się.
*  Typ pola musi być co najmniej tak samo dostępna jak samo pole.
*  Typ zwracany i typy parametrów metody muszą być co najmniej tak samo dostępna jak sama metoda.
*  Typ właściwości musi być co najmniej tak samo dostępna jak samej właściwości.
*  Typ zdarzenia musi być co najmniej tak samo dostępna jak samego zdarzenia.
*  Typ i parametrów typów indeksatora musi być co najmniej tak samo dostępna jak indeksatora, sam.
*  Typ zwracany i typy parametrów operatora musi być co najmniej tak samo dostępna jak operator sam.
*  Typy parametru konstruktora wystąpień musi być co najmniej tak samo dostępna jak sam Konstruktor wystąpienia.

W przykładzie
```csharp
class A {...}

public class B: A {...}
```
`B` klasy powoduje błąd kompilacji ponieważ `A` nie jest co najmniej tak samo dostępna jak `B`.

Podobnie w przykładzie
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
`H` method in Class metoda `B` powoduje błąd w czasie kompilacji, ponieważ zwracany typ `A` nie jest co najmniej tak samo dostępna jak metoda.

## <a name="signatures-and-overloading"></a>Podpisy i przeciążenie

Metody, konstruktory wystąpień, indeksatorów i operatory charakteryzują się ich ***podpisy***:

*  Podpis metody zawiera nazwę metody, liczbę parametrów typu i typu i rodzaj (wartość, odwołanie lub danych wyjściowych) każdego z jego parametrów formalnych w kolejności od lewej do prawej. Do tych celów wszelkie parametr typu metody, która występuje w typie formalny parametr jest identyfikowany nie według jego nazwy, ale za pomocą jego numer porządkowy pozycji na liście argumentów typu metody. Podpis metody specjalnie nie zawiera typ zwracany `params` modyfikator, który może być określona dla parametru najdalej z prawej strony ani ograniczenia parametru typu opcjonalne.
*  Podpis konstruktora wystąpień składa się z typu i rodzaj (wartość, odwołanie lub danych wyjściowych) każdego z jego parametrów formalnych w kolejności od lewej do prawej. Podpis konstruktora wystąpień specjalnie nie obejmuje `params` modyfikator, który może być określona dla parametru najdalej z prawej strony.
*  Podpis indeksatora składa się z typu każdego z jego parametrów formalnych w kolejności od lewej do prawej. Podpis indeksatora specjalnie nie zawiera typu elementu, ani obejmuje `params` modyfikator, który może być określona dla parametru najdalej z prawej strony.
*  Podpis operator składa się z nazwy operatora i typu każdego z jego parametrów formalnych w kolejności od lewej do prawej. Podpis operator specjalnie nie ma typ wyniku.

Podpisy są włączenie mechanizmu ***przeciążenie*** elementów członkowskich klas, struktur i interfejsów:

*  Pozwala na przeciążenie metody, klasy, struktury lub interfejsu, aby zadeklarować, że wiele metod o tej samej nazwie, pod warunkiem ich podpisy są unikatowe w obrębie tej klasy, struktury lub interfejsu.
*  Przeciążenia konstruktorów wystąpienia pozwala na klasie lub strukturze, aby zadeklarować wiele konstruktorów wystąpienia, pod warunkiem ich podpisy są unikatowe w obrębie tej klasy lub struktury.
*  Przeciążenia indeksatorów pozwala klasy, struktury lub interfejsu, aby zadeklarować wiele indeksatorów, pod warunkiem ich podpisy są unikatowe w obrębie tej klasy, struktury lub interfejsu.
*  Przeciążanie operatorów pozwala klasie lub strukturze, aby zadeklarować, że wiele operatorów o takiej samej nazwie, pod warunkiem ich podpisy są unikatowe w obrębie tej klasy lub struktury.

Mimo że `out` i `ref` Modyfikatory parametrów są uważane za część podpisu, elementów członkowskich zadeklarowanych w jednym typie nie mogą się różnić w podpisie wyłącznie przez `ref` i `out`. Występuje błąd kompilacji, jeśli dwa elementy członkowskie są zadeklarowane w ten sam typ z podpisami, które mogą być identyczny, jeśli wszystkie parametry w obu metod za pomocą `out` Modyfikatory zostały zmienione na `ref` modyfikatorów. Do innych celów zgodnych podpisu (np. ukrycie lub zastępowanie) `ref` i `out` są uważane za część podpisu i nie pasują do siebie nawzajem. (To ograniczenie jest umożliwienie C# programy, które ma zostać poddany translacji łatwe do uruchamiania na Common Language Infrastructure (CLI), która nie zapewnia sposób definiowania metod, które różnią się jedynie w `ref` i `out`.)

Na potrzeby podpisów, typy `object` i `dynamic` są traktowane jako takie same. Elementów członkowskich zadeklarowanych w jednym typie może w związku z tym nie różnią się w podpisie wyłącznie przez `object` i `dynamic`.

Poniższy przykład przedstawia zestaw deklaracje metody przeciążonej, wraz z ich podpisy.
```csharp
interface ITest
{
    void F();                        // F()

    void F(int x);                   // F(int)

    void F(ref int x);               // F(ref int)

    void F(out int x);               // F(out int)      error

    void F(int x, int y);            // F(int, int)

    int F(string s);                 // F(string)

    int F(int x);                    // F(int)          error

    void F(string[] a);              // F(string[])

    void F(params string[] a);       // F(string[])     error
}
```

Należy pamiętać, że wszelkie `ref` i `out` Modyfikatory parametrów ([parametry metody](classes.md#method-parameters)) są dostępne w ramach sygnatury. W efekcie `F(int)` i `F(ref int)` unikatowy podpisów. Jednak `F(ref int)` i `F(out int)` nie można zadeklarować w obrębie tego samego interfejsu, ponieważ ich podpisy różnią się jedynie przez `ref` i `out`. Warto również zauważyć, że zwracany typ i `params` modyfikator nie są częścią podpis, więc nie jest możliwe przeciążenia wyłącznie na podstawie na zwracany typ lub włączenia lub wyłączenia `params` modyfikator. Działa w taki sposób, deklaracje metody `F(int)` i `F(params string[])` określonych powyżej wynik na liście błędów kompilacji.

## <a name="scopes"></a>Zakresy

***Zakres*** nazwy jest region tekst programu, w którym jest możliwe do odwoływania się do jednostki zadeklarowanej przez nazwę bez kwalifikowania nazwy. Zakresy mogą być ***zagnieżdżonych***, i w zakresie wewnętrznym może ponownie zadeklarować znaczenie nazwę z zakresu zewnętrznego (nie powoduje jednak usunięcia ograniczeń nałożonych przez [deklaracje](basic-concepts.md#declarations) , w ramach zagnieżdżony blok nie jest możliwe do deklarowania zmiennej lokalnej o tej samej nazwie jako zmiennej lokalnej w otaczającym bloku). Nazwie w zewnętrznym zakresie są określane jako ***ukryte*** w regionie program tekstu objętych w zakresie wewnętrznym, a dostęp do zewnętrznej nazwy jest możliwe tylko kwalifikując nazwę.

*  Zakres elementu członkowskiego przestrzeni nazw zadeklarowana przez *namespace_member_declaration* ([członków Namespace](namespaces.md#namespace-members)) z nie otaczającej *namespace_declaration* jest całego programu tekst.
*  Zakres elementu członkowskiego przestrzeni nazw zadeklarowana przez *namespace_member_declaration* w ramach *namespace_declaration* którego w pełni kwalifikowana nazwa to `N` jest *namespace_body*  z każdego *namespace_declaration* którego w pełni kwalifikowana nazwa to `N` lub zaczynające się od `N`, a następnie kropkę.
*  Zakres nazwa zdefiniowana przez *extern_alias_directive* rozciąga *using_directive*s, *global_attributes* i *namespace_member_ Deklaracja*s jego kompilacji natychmiast zawierający treść jednostki lub przestrzeni nazw. *Extern_alias_directive* nie wpływa na żadnych nowych elementów członkowskich do obszaru bazowego deklaracji. Innymi słowy *extern_alias_directive* nie jest przechodnia, ale raczej dotyczy tylko kompilacji zespołu lub przestrzeni nazw treści w której występuje.
*  Zakres nazwę zdefiniowany lub importowane przez *using_directive* ([dyrektywy Using](namespaces.md#using-directives)) rozciąga *namespace_member_declaration*s  *compilation_unit* lub *namespace_body* w którym *using_directive* występuje. A *using_directive* może udostępnić zero lub więcej nazw przestrzeni nazw, typu lub elementu członkowskiego w ramach określonego *compilation_unit* lub *namespace_body*, ale nie Współtworzenie żadnych nowych elementów członkowskich do obszaru bazowego deklaracji. Innymi słowy *using_directive* nie jest przechodnia, ale raczej ma wpływ tylko na *compilation_unit* lub *namespace_body* , w której występuje.
*  Zakres parametr typu zadeklarowana przez *type_parameter_list* na *class_declaration* ([klasy deklaracje](classes.md#class-declarations)) jest *class_base*, *type_parameter_constraints_clause*s, i *class_body* tego *class_declaration*.
*  Zakres parametr typu zadeklarowana przez *type_parameter_list* na *struct_declaration* ([deklaracji struktury](structs.md#struct-declarations)) jest *struct_interfaces* , *type_parameter_constraints_clause*s, i *struct_body* tego *struct_declaration*.
*  Zakres parametr typu zadeklarowana przez *type_parameter_list* na *interface_declaration* ([interfejsu deklaracje](interfaces.md#interface-declarations)) jest *interface_base* , *type_parameter_constraints_clause*s, i *interface_body* tego *interface_declaration*.
*  Zakres parametr typu zadeklarowana przez *type_parameter_list* na *delegate_declaration* ([delegować deklaracje](delegates.md#delegate-declarations)) jest *Typ_wyniku*, *formal_parameter_list*, i *type_parameter_constraints_clause*s, który *delegate_declaration*.
*  Zakres elementu członkowskiego zadeklarowana przez *class_member_declaration* ([klasy treści](classes.md#class-body)) jest *class_body* , w którym występuje deklaracja. Ponadto rozszerza zakres składowej klasy *class_body* tych pochodne klasy, które znajdują się w domenie ułatwień dostępu ([domeny dostępności](basic-concepts.md#accessibility-domains)) elementu członkowskiego.
*  Zakres elementu członkowskiego zadeklarowana przez *struct_member_declaration* ([składowe struktury](structs.md#struct-members)) jest *struct_body* , w którym występuje deklaracja.
*  Zakres elementu członkowskiego zadeklarowana przez *enum_member_declaration* ([typu wyliczeniowego](enums.md#enum-members)) jest *enum_body* , w którym występuje deklaracja.
*  Zakres parametr zadeklarowanych w *method_declaration* ([metody](classes.md#methods)) jest *method_body* tego *method_declaration*.
*  Zakres parametr zadeklarowanych w *indexer_declaration* ([indeksatory](classes.md#indexers)) jest *accessor_declarations* tego *indexer_declaration*.
*  Zakres parametr zadeklarowanych w *operator_declaration* ([operatory](classes.md#operators)) jest *bloku* tego *operator_declaration*.
*  Zakres parametr zadeklarowanych w *constructor_declaration* ([wystąpienia konstruktory](classes.md#instance-constructors)) jest *constructor_initializer* i *bloku* tego *constructor_declaration*.
*  Zakres parametr zadeklarowanych w *lambda_expression* ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)) jest *anonymous_function_body* tego *lambda_ wyrażenie*
*  Zakres parametr zadeklarowanych w *anonymous_method_expression* ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)) jest *bloku* tego *anonymous_method _expression*.
*  Zakres etykiety zadeklarowanych w *labeled_statement* ([etykietą instrukcji](statements.md#labeled-statements)) jest *bloku* , w którym występuje deklaracja.
*  Zakres zmiennej lokalnej zadeklarowanej w *local_variable_declaration* ([deklaracje zmiennych lokalnych](statements.md#local-variable-declarations)) bloku, w którym występuje deklaracja.
*  Zakres zmiennej lokalnej zadeklarowanej w *switch_block* z `switch` — instrukcja ([instrukcji switch](statements.md#the-switch-statement)) jest *switch_block*.
*  Zakres zmiennej lokalnej zadeklarowanej w *for_initializer* z `for` — instrukcja ([dla instrukcji](statements.md#the-for-statement)) jest *for_initializer*,  *for_condition*, *for_iterator*i ograniczonego *instrukcji* z `for` instrukcji.
*  Zakres stała lokalna zadeklarowana w *local_constant_declaration* ([deklaracji stałej lokalnej](statements.md#local-constant-declarations)) bloku, w którym występuje deklaracja. Jest to błąd czasu kompilacji do odwoływania się do stała lokalna w stanie tekstową poprzedzającym jego *constant_declarator*.
*  Zakres zmiennej zadeklarowany jako część *foreach_statement*, *using_statement*, *lock_statement* lub *query_expression* jest określany przez rozszerzenie danego konstrukcji.

W zakresie przestrzeni nazw, klasy, struktury lub wyliczenia elementu członkowskiego jest możliwe do odwoływania się do elementu członkowskiego w stanie tekstową poprzedzająca deklaracja elementu członkowskiego. Na przykład
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
W tym miejscu jest on prawidłowy dla `F` do odwoływania się do `i` przed jej zadeklarowaniem.

W zakresie zmiennej lokalnej jest błąd w czasie kompilacji do odwoływania się do zmiennej lokalnej w stanie tekstową poprzedzającym *local_variable_declarator* zmiennej lokalnej. Na przykład
```csharp
class A
{
    int i = 0;

    void F() {
        i = 1;                  // Error, use precedes declaration
        int i;
        i = 2;
    }

    void G() {
        int j = (j = 1);        // Valid
    }

    void H() {
        int a = 1, b = ++a;    // Valid
    }
}
```

W `F` powyższej metody, pierwsze przypisanie do `i` specjalnie odwołuje się do pola, zadeklarowany w zewnętrznym zakresie. Przeciwnie odwołuje się do zmiennej lokalnej, i powoduje błąd kompilacji, ponieważ w formie tekstu wcześniejsza deklaracja zmiennej. W `G` metody, użycie `j` w inicjatorze dla deklaracji `j` jest prawidłowy, ponieważ użycie nie poprzedza *local_variable_declarator*. W `H` metody, kolejne *local_variable_declarator* poprawnie odwołuje się do zmienna lokalna zadeklarowana we wcześniejszej *local_variable_declarator* w tej samej  *local_variable_declaration*.

Wyznaczanie zakresu reguły dla zmiennych lokalnych są przeznaczone do zagwarantowania, że znaczenie nazwa używana w kontekście wyrażenia jest zawsze taki sam sposób, w bloku. Gdyby zakres zmiennej lokalnej, aby rozszerzyć tylko z jego deklarację na końcu bloku w powyższym przykładzie pierwsze przypisanie przypisywanej zmienną instance i przypisywanej drugie przypisanie do zmiennej lokalnej, prawdopodobnie prowadzących do błędy kompilacji, gdyby nowszej, aby zmieniać wprowadza instrukcje w bloku.

Znaczenie nazwy w bloku mogą się różnić w zależności od kontekstu, w której nazwa jest używana. W przykładzie
```csharp
using System;

class A {}

class Test
{
    static void Main() {
        string A = "hello, world";
        string s = A;                            // expression context

        Type t = typeof(A);                      // type context

        Console.WriteLine(s);                    // writes "hello, world"
        Console.WriteLine(t);                    // writes "A"
    }
}
```
Nazwa `A` jest używany w kontekście wyrażenia do odwoływania się do zmiennej lokalnej `A` w kontekście typu do odwoływania się do klasy `A`.

### <a name="name-hiding"></a>Ukrywanie nazw

Zakres jednostki zwykle obejmuje więcej tekstu programu niż miejsce deklaracji jednostki. W szczególności zakres jednostki może zawierać deklaracje, które wprowadzają nowe spacje deklaracji zawierający jednostkę o takiej samej nazwie. Takie deklaracje spowodować, że oryginalna jednostka stać się ***ukryte***. Z drugiej strony, jednostki jest nazywany ***widoczne*** kiedy nie jest ukryty.

Ukrywanie nazw występuje, gdy zakresy nakładają się na siebie przez zagnieżdżanie i kiedy zakresy nakładają się poprzez dziedziczenie. Cech dwóch rodzajów ukrywanie są opisane w poniższych sekcjach.

#### <a name="hiding-through-nesting"></a>Ukrywanie poprzez zagnieżdżanie

Ukrywanie nazw poprzez zagnieżdżenia może wystąpić w wyniku zagnieżdżenia przestrzeni nazw lub typów w przestrzeni nazw, w wyniku zagnieżdżanie typów w obrębie klasy lub struktury, a parametr i deklaracje zmiennych lokalnych w wyniku.

W przykładzie
```csharp
class A
{
    int i = 0;

    void F() {
        int i = 1;
    }

    void G() {
        i = 1;
    }
}
```
w ramach `F` metody, zmienną instance `i` jest ukryta przez zmienną lokalną `i`, ale w ramach `G` metody `i` nadal odwołuje się do zmiennej wystąpienia.

Nazwa w zakresie wewnętrznym ukrywa nazwę w zakresie zewnętrznym, powoduje ukrycie wszystkich wystąpień przeciążona o takiej nazwie. W przykładzie
```csharp
class Outer
{
    static void F(int i) {}

    static void F(string s) {}

    class Inner
    {
        void G() {
            F(1);              // Invokes Outer.Inner.F
            F("Hello");        // Error
        }

        static void F(long l) {}
    }
}
```
wywołanie `F(1)` wywołuje `F` zadeklarowanych w `Inner` ponieważ wszystkie wystąpienia zewnętrzne `F` są ukryte przez wewnętrzny deklaracji. Z tego samego powodu wywołanie `F("Hello")` powoduje błąd kompilacji.

#### <a name="hiding-through-inheritance"></a>Ukrywanie poprzez dziedziczenie

Ukrywanie nazw poprzez dziedziczenie występuje, gdy klasy lub struktury ponownie zadeklarować nazwy, które były dziedziczone z klasy bazowej. Tego rodzaju ukrywaniem nazwy ma jedną z następujących form:

*  Stała, pola, właściwości, zdarzenia lub typ wprowadzony w klasie lub strukturze ukrywa wszystkie składowych klasy bazowej o takiej samej nazwie.
*  Metoda wprowadzona w klasie lub strukturze ukrywa wszystkich członków klasy podstawowej-metoda o takiej samej nazwie, a wszystkie metody klasy podstawowej, z tym samym podpisie (Nazwa metody i liczba parametrów, Modyfikatory i typy).
*  Indeksator wprowadzony w klasie lub strukturze ukrywa wszystkie indeksatory klas podstawowych, z tym samym podpisie (liczba parametrów i typy).

Zasady dotyczące deklaracje operatora ([operatory](classes.md#operators)) uniemożliwia dla klasy pochodnej do deklarowania operatora z tym samym podpisie jako operator w klasie bazowej. W związku z tym operatory nigdy nie ukrywaj siebie nawzajem.

Sprzecznie ukrywanie nazw z zakresu zewnętrznego, ukrywanie dostępna nazwa z zakresu dziedziczone powoduje, że ostrzeżenie zgłoszenia. W przykładzie
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    public void F() {}        // Warning, hiding an inherited name
}
```
Deklaracja `F` w `Derived` powoduje, że ostrzeżenie zgłoszenia. Ukrywanie odziedziczonych nazwa specjalnie nie jest błąd, ponieważ, mogłoby spowodować nieprawidłowe ewolucji osobnych klas bazowych. Na przykład powyżej sytuacji mogą pochodzić ponieważ późniejszą wersję `Base` wprowadzone `F` metodę, która nie był obecny we wcześniejszej wersji tej klasy. Powyżej sytuacji był błąd, następnie wszelkie zmiany wprowadzone do klasy bazowej w bibliotece klas oddzielnie wersjonowanych mogą potencjalnie powodować klasy pochodne staną się nieprawidłowe.

Ostrzeżenie spowodowane poprzez ukrywanie odziedziczonych nazwy mogą zostać usunięte za pośrednictwem `new` modyfikator:
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    new public void F() {}
}
```

`new` Modyfikator wskazuje, że `F` w `Derived` "nowy" i że w rzeczywistości jest przeznaczony do ukrywania odziedziczonej składowej.

Deklaracja nowej składowej ukrywa dziedziczonego członka tylko w zakresie nowego elementu członkowskiego.

```csharp
class Base
{
    public static void F() {}
}

class Derived: Base
{
    new private static void F() {}    // Hides Base.F in Derived only
}

class MoreDerived: Derived
{
    static void G() { F(); }          // Invokes Base.F
}
```

W przykładzie powyżej deklaracji `F` w `Derived` ukrywa `F` , został odziedziczony z `Base`, ale od nowej `F` w `Derived` ma dostęp do prywatnych, jego zakres nie obejmuje `MoreDerived` . W związku z tym, wywołanie `F()` w `MoreDerived.G` jest prawidłowa i wywoła `Base.F`.

## <a name="namespace-and-type-names"></a>Nazw Namespace i typ

Wiele kontekstów w C# wymagają programu *namespace_name* lub *type_name* należy określić.

```antlr
namespace_name
    : namespace_or_type_name
    ;

type_name
    : namespace_or_type_name
    ;

namespace_or_type_name
    : identifier type_argument_list?
    | namespace_or_type_name '.' identifier type_argument_list?
    | qualified_alias_member
    ;
```

A *namespace_name* jest *namespace_or_type_name* odwołujący się do przestrzeni nazw. Następujące rozwiązania, jak opisano poniżej, *namespace_or_type_name* z *namespace_name* musi odwoływać się do przestrzeni nazw, lub w przeciwnym razie wystąpi błąd kompilacji. Bez argumentów typu ([argumentów typu](types.md#type-arguments)) mogą być obecne w *namespace_name* (tylko typy mogą mieć argumentów typu).

A *type_name* jest *namespace_or_type_name* odwołujący się do typu. Następujące rozwiązania, jak opisano poniżej, *namespace_or_type_name* z *type_name* musi odwoływać się do typu, lub w przeciwnym razie wystąpi błąd kompilacji.

Jeśli *namespace_or_type_name* jest kwalifikowana alias składowej jego znaczenie jest zgodnie z opisem w [kwalifikatory alias Namespace](namespaces.md#namespace-alias-qualifiers). W przeciwnym razie *namespace_or_type_name* ma jedną z czterech form:

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

gdzie `I` jest pojedynczy identyfikator `N` jest *namespace_or_type_name* i `<A1, ..., Ak>` to opcjonalna *type_argument_list*. Gdy nie *type_argument_list* jest określony, należy wziąć pod uwagę `k` zero.

Znaczenie *namespace_or_type_name* jest określany w następujący sposób:

*   Jeśli *namespace_or_type_name* ma postać `I` lub w postaci `I<A1, ..., Ak>`:
    * Jeśli `K` jest równa zero i *namespace_or_type_name* pojawia się w obrębie deklaracji metody ogólnej ([metody](classes.md#methods)) i jeśli deklaracja zawiera parametr typu ([typu Parametry](classes.md#type-parameters)) o nazwie `I`, a następnie *namespace_or_type_name* odwołuje się do tego parametru typu.
    * W przeciwnym razie, jeśli *namespace_or_type_name* pojawia się w deklaracji typu, a następnie dla każdego typu wystąpienia `T` ([typu wystąpienia](classes.md#the-instance-type)), począwszy od typu wystąpienia tego typu Deklaracja i kontynuowanie przy użyciu typu wystąpienia każdej otaczającej deklaracji klasy lub struktury (jeśli istnieje):
        * Jeśli `K` to zero, a deklaracja `T` obejmuje parametru typu o nazwie `I`, a następnie *namespace_or_type_name* odwołuje się do tego parametru typu.
        * W przeciwnym razie, jeśli *namespace_or_type_name* pojawia się w treści deklaracji typu i `T` lub dowolny z jej typów podstawowych zawiera zagnieżdżony typ dostępny o nazwie `I` i `K`  parametry typu, a następnie *namespace_or_type_name* odwołuje się do tego typu skonstruowany przy użyciu argumentów danego typu. Jeśli istnieje więcej niż jeden taki typ, zostanie wybrany typem zadeklarowanym w ramach typu bardziej pochodnego. Należy pamiętać, że członkowie-type (stałe, pola, metody, właściwości, indeksatory, operatory, konstruktory wystąpień, destruktory i konstruktorów statycznych) i elementów członkowskich typu z różną liczbę parametrów typu są ignorowane podczas określania znaczenie *namespace_or_type_name*.
    * Jeśli poprzednie kroki powiodły się następnie dla każdej przestrzeni nazw `N`, począwszy od przestrzeni nazw, w którym *namespace_or_type_name* problem wystąpi, kontynuując każdego otaczającej przestrzeni nazw (jeśli istnieje), a kończąc Globalna przestrzeń nazw, poniższe kroki są oceniane, dopóki nie znajduje się jednostka:
        * Jeśli `K` jest równa zero i `I` jest nazwą przestrzeni nazw w `N`, następnie:
            * Jeśli lokalizacja gdzie *namespace_or_type_name* występuje jest ujęta w deklarację przestrzeni nazw dla `N` i zawiera deklarację przestrzeni nazw *extern_alias_directive* lub *using_alias_directive* który kojarzy nazwę `I` z przestrzeni nazw lub typ, a następnie *namespace_or_type_name* jest niejednoznaczny i występuje błąd kompilacji.
            * W przeciwnym razie *namespace_or_type_name* odwołuje się do przestrzeni nazw o nazwie `I` w `N`.
        * W przeciwnym razie, jeśli `N` zawiera dostępny typ o nazwie `I` i `K`  parametry typu, następnie:
            * Jeśli `K` to zero i lokalizacja gdzie *namespace_or_type_name* występuje jest ujęta w deklarację przestrzeni nazw dla `N` i zawiera deklarację przestrzeni nazw *extern_alias_directive*  lub *using_alias_directive* który kojarzy nazwę `I` z przestrzeni nazw lub typ, a następnie *namespace_or_type_name* jest niejednoznaczne i w czasie kompilacji występuje błąd.
            * W przeciwnym razie *namespace_or_type_name* odwołuje się do typu skonstruowany przy użyciu argumentów danego typu.
        * W przeciwnym razie, jeśli lokalizacja gdzie *namespace_or_type_name* występuje jest ujęta w deklarację przestrzeni nazw dla `N`:
            * Jeśli `K` jest równa zero i zawiera deklarację przestrzeni nazw *extern_alias_directive* lub *using_alias_directive* który kojarzy nazwę `I` z zaimportowaną przestrzenią nazw lub Typ, a następnie *namespace_or_type_name* odwołuje się do tej przestrzeni nazw lub typu.
            * W przeciwnym razie, jeśli deklaracji przestrzeni nazw i typ zaimportowany przez *using_namespace_directive*s i *using_alias_directive*s deklarację przestrzeni nazw zawierać dokładnie jeden typ dostępne o nazwie `I` i `K`  parametry typu, a następnie *namespace_or_type_name* odwołuje się do tego typu skonstruowany przy użyciu argumentów danego typu.
            * W przeciwnym razie, jeśli deklaracji przestrzeni nazw i typ zaimportowany przez *using_namespace_directive*s i *using_alias_directive*s deklaracji przestrzeni nazw zawiera więcej niż jeden dostępny typ o nazwie `I` i `K`  parametry typu, a następnie *namespace_or_type_name* jest niejednoznaczny i występuje błąd.
    * W przeciwnym razie *namespace_or_type_name* jest niezdefiniowana, i występuje błąd kompilacji.
*  W przeciwnym razie *namespace_or_type_name* ma postać `N.I` lub w postaci `N.I<A1, ..., Ak>`. `N` najpierw zostanie rozwiązany jak *namespace_or_type_name*. Jeśli rozdzielczość `N` zakończy się niepowodzeniem, wystąpi błąd kompilacji. W przeciwnym razie `N.I` lub `N.I<A1, ..., Ak>` zostanie rozwiązany w następujący sposób:
    * Jeśli `K` jest równa zero i `N` odwołuje się do przestrzeni nazw i `N` zawiera zagnieżdżone przestrzenie nazw o nazwie `I`, a następnie *namespace_or_type_name* odwołuje się do tego zagnieżdżone przestrzenie nazw.
    * W przeciwnym razie, jeśli `N` odwołuje się do przestrzeni nazw i `N` zawiera dostępny typ o nazwie `I` i `K`  parametry typu, a następnie *namespace_or_type_name* odwołuje się do tego typu jest zbudowany z argumentami danego typu.
    * W przeciwnym razie, jeśli `N` odwołuje się do (prawdopodobnie skonstruowany) typu klasy lub struktury i `N` lub dowolny z jej klas bazowych zawiera zagnieżdżony typ dostępny o nazwie `I` i `K`  wpisz parametry, a następnie *namespace_or_type_name* odwołuje się do tego typu skonstruowany przy użyciu argumentów danego typu. Jeśli istnieje więcej niż jeden taki typ, zostanie wybrany typem zadeklarowanym w ramach typu bardziej pochodnego. Należy pamiętać, że jeśli znaczenie `N.I` określa się jako część obsługi klasy podstawowej specyfikacji `N` następnie bezpośrednie klasy bazowej `N` jest uważany za obiektu ([klas bazowych](classes.md#base-classes)).
    * W przeciwnym razie `N.I` jest nieprawidłową *namespace_or_type_name*, i występuje błąd kompilacji.

A *namespace_or_type_name* może odwoływać się do klasy statycznej ([klas statycznych](classes.md#static-classes)) tylko wtedy, gdy

*  *Namespace_or_type_name* jest `T` w *namespace_or_type_name* formularza `T.I`, lub
*  *Namespace_or_type_name* jest `T` w *typeof_expression* ([listy argumentów](expressions.md#argument-lists)1) w postaci `typeof(T)`.

### <a name="fully-qualified-names"></a>W pełni kwalifikowane nazwy

Każdy obszar nazw i typ ma ***w pełni kwalifikowana nazwa***, który jednoznacznie identyfikuje przestrzeni nazw lub typu oraz wszystkich innych. W pełni kwalifikowaną nazwę przestrzeni nazw lub typ `N` jest określany w następujący sposób:

*  Jeśli `N` jest elementem członkowskim globalnej przestrzeni nazw, jego w pełni kwalifikowana nazwa jest `N`.
*  W przeciwnym razie jest jego w pełni kwalifikowana nazwa `S.N`, gdzie `S` jest w pełni kwalifikowaną nazwę przestrzeni nazw lub typ, w którym `N` jest zadeklarowana.

Innymi słowy, w pełni kwalifikowana nazwa `N` jest pełną ścieżką hierarchiczne identyfikatorów, które mogą prowadzić do `N`, poczynając od globalnej przestrzeni nazw. Ponieważ każdy członek przestrzeń nazw lub typ musi mieć unikatową nazwę, jest zgodna z w pełni kwalifikowaną nazwę przestrzeni nazw lub typ jest zawsze unikatowa.

W poniższym przykładzie pokazano kilka deklaracji przestrzeni nazw i typ wraz z ich skojarzone w pełni kwalifikowanych nazw.
```csharp
class A {}                // A

namespace X               // X
{
    class B               // X.B
    {
        class C {}        // X.B.C
    }

    namespace Y           // X.Y
    {
        class D {}        // X.Y.D
    }
}

namespace X.Y             // X.Y
{
    class E {}            // X.Y.E
}
```

## <a name="automatic-memory-management"></a>Automatyczne zarządzanie pamięcią

C# stosuje automatyczne zarządzanie pamięcią, która ułatwia deweloperom z ręcznie przydzielanie i zwalnianie pamięci zajmowane przez obiekty. Zasady zarządzania pamięcią automatyczną są implementowane przez ***modułu zbierającego elementy bezużyteczne***. Obiekt cyklu życia zarządzania pamięci jest następująca:

1. Po utworzeniu obiektu pamięć została przydzielona dla niego, Konstruktor jest uruchamiany i obiekt uznaje się na żywo.
2. Jeśli obiekt lub dowolnej jego części bez nie są dostępne przez wszystkie możliwe kontynuacji wykonywanie, innym niż działanie destruktory obiektu uznaje się już w użyciu i staje się kwalifikuje się do zniszczenia. Kompilator języka C# i wyrzucanie elementów bezużytecznych może wybrać do analizowania kodu w celu określenia, która odwołuje się do obiektu może być używane w przyszłości. Na przykład jeśli zmienna lokalna, która znajduje się w zakresie jest tylko istniejące odwołanie do obiektu, ale tej zmiennej lokalnej nigdy nie jest określany w wszystkie możliwe kontynuacji wykonywanie z bieżącego wykonywania punkt w procedurze, wyrzucanie elementów bezużytecznych może (ale nie jest musieli) traktować obiektu jako nie jest już używana.
3. Po kwalifikuje się do zniszczenia obiektu w pewnym później nieokreślony czas destruktor ([destruktory](classes.md#destructors)) (jeśli istnieje) dla obiektu jest uruchamiany. W normalnych warunkach destruktor obiektu jest uruchamiany tylko raz, chociaż interfejsy API specyficzne dla implementacji może zezwolić na zastąpienie tego zachowania.
4. Po jego uruchomieniu destruktora dla obiektu, jeżeli tego obiektu lub dowolnej jego części bez nie może uzyskać dostępu do żadnych możliwych kontynuacja wykonania, w tym uruchamianie destruktory, obiekt jest uznawany za niedostępne i obiekt kwalifikuje się do kolekcji.
5. Na koniec w czasie po obiekcie kwalifikuje się do kolekcji, moduł zbierający elementy bezużyteczne zwalnia pamięć skojarzonej z tym obiektem.

Moduł odśmiecania pamięci przechowuje informacje na temat użycia obiektu i używa tych informacji do decyzje dotyczące pamięci zarządzania, takich jak miejsca w pamięci, aby zlokalizować nowo utworzonego obiektu, gdy przenosić obiekt, a gdy obiekt nie jest już używana lub jest niedostępny.

Podobnie jak inne języki założono moduł wyrzucania elementów bezużytecznych C# jest zaprojektowana tak, wyrzucanie elementów bezużytecznych może wdrożyć szerokiej gamy zasad zarządzania pamięci. Na przykład C# nie wymaga uruchomić destruktorów lub zebrane obiekty tak szybko, jak jest uprawniony do lub że destruktory być uruchamiane w określonej kolejności lub na żadnym z wątków.

Zachowanie modułu odśmiecania pamięci mogą być kontrolowane w pewnym stopniu, za pomocą metod statycznych w klasie `System.GC`. Ta klasa służy do żądania kolekcji nastąpi usunięcie przez destruktory można działać (lub nie działać) i tak dalej.

Ponieważ moduł odśmiecania pamięci jest dozwolona szerokiego szerokości przy podejmowaniu decyzji podczas zbierania obiektów i uruchomić destruktory, odpowiadające implementacja może powodować danych wyjściowych, który różni się od wyświetlanych przez następujący kod. Uruchamianie programu
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }
}

class B
{
    object Ref;

    public B(object o) {
        Ref = o;
    }

    ~B() {
        Console.WriteLine("Destruct instance of B");
    }
}

class Test
{
    static void Main() {
        B b = new B(new A());
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
    }
}
```
tworzy wystąpienie klasy `A` i wystąpienia klasy `B`. Te obiekty kwalifikować się do wyrzucania elementów bezużytecznych po zmiennej `b` jest przypisywana wartość `null`, ponieważ po tym czasie nie ma możliwości każdy kod napisany przez użytkownika do nich dostęp. Dane wyjściowe mogą być albo
```
Destruct instance of A
Destruct instance of B
```
lub
```
Destruct instance of B
Destruct instance of A
```
ponieważ język nakłada nie ograniczeń w kolejności, w których obiekty są bezużyteczne.

W przypadku subtelne rozróżnienia między "kwalifikuje się do zniszczenia" i "kwalifikuje się do kolekcji" może być istotne. Na przykład
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }

    public void F() {
        Console.WriteLine("A.F");
        Test.RefA = this;
    }
}

class B
{
    public A Ref;

    ~B() {
        Console.WriteLine("Destruct instance of B");
        Ref.F();
    }
}

class Test
{
    public static A RefA;
    public static B RefB;

    static void Main() {
        RefB = new B();
        RefA = new A();
        RefB.Ref = RefA;
        RefB = null;
        RefA = null;

        // A and B now eligible for destruction
        GC.Collect();
        GC.WaitForPendingFinalizers();

        // B now eligible for collection, but A is not
        if (RefA != null)
            Console.WriteLine("RefA is not null");
    }
}
```

W programie powyżej, jeśli moduł zbierający elementy bezużyteczne zdecydował się uruchomić destruktor `A` przed destruktor `B`, a następnie może być danych wyjściowych tego programu:
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

Należy zauważyć, że wystąpienie programu `A` nie był używany i `A`firmy destruktor został uruchomiony, jest nadal możliwe dla metod `A` (w tym przypadku `F`) do wywoływania z innego destruktora. Należy również zauważyć, że uruchomiona destruktora może spowodować, że obiekt, do znów można używać z linii głównej programu. W tym przypadku uruchamianie `B`przez destruktora spowodowała wystąpienie `A` który został wcześniej nieużywany zanim staną się dostępne z odwołania na żywo `Test.RefA`. Po wywołaniu `WaitForPendingFinalizers`, wystąpienie `B` kwalifikuje się do kolekcji, ale wystąpienie `A` nie jest dostępna, ze względu na odwołanie `Test.RefA`.

Aby uniknąć nieporozumień i nieoczekiwane zachowanie, zazwyczaj jest dobrym pomysłem destruktory wykonać tylko czyszczenie danych przechowywanych w polach tego obiektu, a nie wykonać żadnych innych akcji na obiekty lub pola statyczne.

Alternatywa dla użycia destruktory jest umożliwienie klasa implementuje `System.IDisposable` interfejsu. Dzięki temu klientowi określenie, kiedy należy zwolnić zasoby obiektu, zwykle, uzyskując dostęp do obiektu jako zasób w obiekcie `using` — instrukcja ([za pomocą instrukcji](statements.md#the-using-statement)).

## <a name="execution-order"></a>Kolejność wykonywania

Wykonanie programu w języku C# działa w taki sposób, że efekty uboczne każdego wątku wykonywania są zachowywane w punktach krytycznych wykonań. A ***efekt uboczny*** jest zdefiniowany jako odczyt lub zapis pola nietrwałego, zapis do zmiennej typu nieulotnego, zapis do zewnętrznego zasobu, a następnie Zgłaszanie wyjątku. Punkty wykonywania krytycznych, w których kolejność tych efekty uboczne muszą być chronione są odwołaniami do pola nietrwałego ([pola nietrwałego](classes.md#volatile-fields)), `lock` instrukcji ([instrukcję lock](statements.md#the-lock-statement)), i Tworzenie wątku i zakończenie. Środowisko wykonawcze jest bezpłatna zmienić kolejność wykonywania programu w języku C#, z zastrzeżeniem następujące ograniczenia:

*  Zależność dane są zachowywane w obrębie wykonanie wątku. Oznacza to, że wartość każdej zmiennej jest obliczana tak, jakby wszystkie instrukcje w wątku zostały wykonane w porządku program oryginalnej.
*  Inicjowanie kolejności, zasady są zachowywane ([inicjowania pola](classes.md#field-initialization) i [zmiennej inicjatory](classes.md#variable-initializers)).
*  Kolejność efekty uboczne są zachowywane względem volatile odczytów i zapisów ([pola nietrwałego](classes.md#volatile-fields)). Ponadto środowisko wykonawcze nie należy ocenić częścią wyrażenia, czy można ustalić, czy wartość tego wyrażenia nie jest używany i nie wymagane efekty uboczne są tworzone, (wraz ze wszystkimi spowodowane przez wywołanie metody lub uzyskiwania dostępu do pola nietrwałego). Podczas wykonywania programu zostało przerwane przez zdarzenie asynchroniczne (np. wyjątek zgłoszony przez inny wątek), nie ma żadnej gwarancji czy dostrzegalnych efekty uboczne są widoczne w oryginalnym porządku program.
