---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704003"
---
# <a name="basic-concepts"></a>Podstawowe pojęcia

## <a name="application-startup"></a>Uruchamianie aplikacji

Zestaw, który ma ***punkt wejścia*** , nosi nazwę ***aplikacji***. Po uruchomieniu aplikacji zostanie utworzona nowa ***domena aplikacji*** . Kilka różnych wystąpień aplikacji może istnieć na tym samym komputerze w tym samym czasie, a każda z nich ma własną domenę aplikacji.

Domena aplikacji umożliwia izolację aplikacji, działając jako kontener dla stanu aplikacji. Domena aplikacji pełni rolę kontenera i granicy dla typów zdefiniowanych w aplikacji i używanych przez niego bibliotek klas. Typy ładowane do jednej domeny aplikacji różnią się od tego samego typu, który został załadowany do innej domeny aplikacji, a wystąpienia obiektów nie są bezpośrednio udostępniane między domenami aplikacji. Na przykład Każda domena aplikacji ma własną kopię zmiennych statycznych dla tych typów, a Konstruktor statyczny dla typu jest uruchamiany maksymalnie raz dla każdej domeny aplikacji. Implementacje mogą być bezpłatne, aby zapewnić zasady i mechanizmy specyficzne dla implementacji dotyczące tworzenia i niszczenia domen aplikacji.

***Uruchamianie aplikacji*** występuje, gdy środowisko wykonawcze wywołuje wydaną metodę, która jest określana jako punkt wejścia aplikacji. Ta metoda punktu wejścia ma zawsze nazwę `Main` i może mieć jeden z następujących podpisów:

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

Jak pokazano, punkt wejścia może opcjonalnie zwrócić wartość `int`. Ta wartość zwracana jest używana podczas kończenia aplikacji ([zakończenie aplikacji](basic-concepts.md#application-termination)).

Punkt wejścia może opcjonalnie mieć jeden parametr formalny. Parametr może mieć dowolną nazwę, ale typem parametru musi być `string[]`. Jeśli parametr formalny jest obecny, środowisko wykonawcze tworzy i przekazuje argument `string[]` zawierający argumenty wiersza polecenia, które zostały określone podczas uruchamiania aplikacji. Argument `string[]` nigdy nie ma wartości null, ale może mieć długość zero, jeśli nie określono żadnych argumentów wiersza polecenia.

Ponieważ C# obsługuje Przeciążenie metod, Klasa lub struktura może zawierać wiele definicji niektórych metod, pod warunkiem, że każda z nich ma inną sygnaturę. Jednak w ramach jednego programu żadna Klasa lub struktura nie może zawierać więcej niż jednej metody o nazwie `Main`, której definicja kwalifikuje się do użycia jako punkt wejścia aplikacji. Inne przeciążone wersje `Main` są dozwolone, jednak pod warunkiem, że mają więcej niż jeden parametr lub ich parametr jest inny niż typ `string[]`.

Aplikacja może składać się z wielu klas lub struktur. Istnieje możliwość, aby więcej niż jedna z tych klas lub struktur zawierała metodę o nazwie `Main`, której definicja kwalifikuje się do użycia jako punkt wejścia aplikacji. W takich przypadkach mechanizm zewnętrzny (na przykład opcja kompilatora wiersza polecenia) musi zostać użyty do wybrania jednej z tych metod `Main` jako punktu wejścia.

W C#programie każda metoda musi być zdefiniowana jako element członkowski klasy lub struktury. Zwykle zadeklarowana dostępność ([zadeklarowana dostępność](basic-concepts.md#declared-accessibility)) metody jest określana przez Modyfikatory dostępu ([Modyfikatory dostępu](classes.md#access-modifiers)) określone w swojej deklaracji, a podobnie zadeklarowana dostępność typu jest określana przez Modyfikatory dostępu określone w jego deklaracji. Aby można było wywołać daną metodę danego typu, zarówno typ, jak i element członkowski muszą być dostępne. Jednak punkt wejścia aplikacji jest szczególnym przypadkiem. W szczególności środowisko wykonawcze może uzyskać dostęp do punktu wejścia aplikacji niezależnie od jego zadeklarowanej dostępności i bez względu na zadeklarowaną dostępność jego deklaracji typu.

Metoda punktu wejścia aplikacji nie może znajdować się w deklaracji klasy ogólnej.

We wszystkich innych aspektach metody punktu wejścia zachowują się jak te, które nie są punktami wejścia.

## <a name="application-termination"></a>Zakończenie aplikacji

***Zakończenie aplikacji*** zwraca kontrolę do środowiska wykonawczego.

Jeśli zwracanym typem metody ***punktu wejścia*** aplikacji jest `int`, zwracana wartość służy jako ***kod stanu zakończenia***aplikacji. Celem tego kodu jest umożliwienie komunikacji sukcesu lub niepowodzenia środowiska wykonawczego.

Jeśli zwracanym typem metody punktu wejścia jest `void`, osiąganie prawego nawiasu klamrowego (`}`), który kończy tę metodę, lub wykonywanie instrukcji `return`, która nie zawiera wyrażenia, powoduje kod stanu zakończenia `0`.

Przed zakończeniem działania aplikacji są wywoływane destruktory dla wszystkich obiektów, które nie zostały jeszcze odebrane w pamięci podręcznej, chyba że takie oczyszczanie zostało pominięte (przez wywołanie metody biblioteki `GC.SuppressFinalize`, na przykład).

## <a name="declarations"></a>Deklaracje

Deklaracje w C# programie definiują elementy składowe programu. C#programy są zorganizowane przy użyciu przestrzeni nazw ([przestrzenie nazw](namespaces.md)), które mogą zawierać deklaracje typów i zagnieżdżone deklaracje przestrzeni nazw. Deklaracje typów ([deklaracje typów](namespaces.md#type-declarations)) służą do definiowania klas ([klas](classes.md)), struktur ([struktur](structs.md)), interfejsów ([interfejsów](interfaces.md)), wyliczeniowych ([wyliczeniowych](enums.md)) i delegatów ([delegatów](delegates.md)). Rodzaje elementów członkowskich dozwolone w deklaracji typu zależą od formy deklaracji typu. Na przykład deklaracje klasy mogą zawierać deklaracje dla stałych ([stałych](classes.md#constants)), pól ([pól](classes.md#fields)), metod ([metod](classes.md#methods)), właściwości ([Właściwości](classes.md#properties)), zdarzeń ([zdarzeń](classes.md#events)), indeksatorów ([indeksatorów](classes.md#indexers)), Operatory ([Operatory](classes.md#operators)), konstruktory wystąpień ([konstruktory wystąpień](classes.md#instance-constructors)), konstruktory statyczne ([konstruktory statyczne](classes.md#static-constructors)), destruktory ([destruktory](classes.md#destructors)) i zagnieżdżone typy ([typy zagnieżdżone](classes.md#nested-types)).

Deklaracja definiuje nazwę w ***obszarze deklaracji*** , do którego należy deklaracja. Z wyjątkiem przeciążonych elementów członkowskich ([sygnatur i Przeciążenie](basic-concepts.md#signatures-and-overloading)), jest to błąd czasu kompilacji, który ma dwie lub więcej deklaracji, które wprowadzają elementy członkowskie o tej samej nazwie w obszarze deklaracji. Nie jest możliwe, aby miejsce deklaracji zawierało różne rodzaje składowych o tej samej nazwie. Na przykład obszar deklaracji nigdy nie może zawierać pola i metody o tej samej nazwie.

Istnieje kilka różnych typów obszarów deklaracji, zgodnie z opisem w poniższej tabeli.

*  We wszystkich plikach źródłowych programu *namespace_member_declaration*s bez otaczających *namespace_declaration* są członkami pojedynczego, połączonego obszaru deklaracji nazywanego ***przestrzenią globalną deklaracji***.
*  We wszystkich plikach źródłowych programu *namespace_member_declaration*s w *namespace_declaration*s, które mają taką samą w pełni kwalifikowaną nazwę przestrzeni nazw, są członkami pojedynczej połączonej deklaracji.
*  Każda klasa, struktura lub Deklaracja interfejsu tworzy nową przestrzeń deklaracji. Nazwy są wprowadzane do tego obszaru deklaracji za poorednictwem *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s lub *type_parameter*s. Z wyjątkiem deklaracji konstruktora wystąpień przeciążonych i deklaracji konstruktora statycznego Klasa lub struktura nie może zawierać deklaracji składowej o takiej samej nazwie jak Klasa lub struktura. Klasa, struktura lub interfejs umożliwia deklarację przeciążonych metod i indeksatorów. Ponadto Klasa lub struktura zezwala na deklarację konstruktorów przeciążonych wystąpień i operatorów. Na przykład Klasa, struktura lub interfejs może zawierać wiele deklaracji metody o tej samej nazwie, pod warunkiem, że te deklaracje metod różnią się w sygnaturze ([sygnatur i Przeciążenie](basic-concepts.md#signatures-and-overloading)). Należy zauważyć, że klasy bazowe nie przyczyniają się do przestrzeni deklaracji klasy, a interfejsy podstawowe nie przyczyniają się do obszaru deklaracji interfejsu. W ten sposób Klasa pochodna lub interfejs może zadeklarować element członkowski o takiej samej nazwie jak Dziedziczony element członkowski. Członek ten jest wymieniony do ***ukrycia*** dziedziczonego elementu członkowskiego.
*  Każda deklaracja delegata tworzy nową przestrzeń deklaracji. Nazwy są wprowadzane do tego obszaru deklaracji za poorednictwem parametrów formalnych (*fixed_parameter*s i *parameter_array*s) i *type_parameter*s.
*  Każda deklaracja wyliczenia tworzy nową przestrzeń deklaracji. Nazwy są wprowadzane do tego obszaru deklaracji za poorednictwem *enum_member_declarations*.
*  Każda deklaracja metody, deklaracja indeksatora, deklaracja operatora, deklaracja konstruktora wystąpień i funkcja anonimowa tworzą nowe miejsce deklaracji zwane ***przestrzenią lokalnej deklaracji zmiennej***. Nazwy są wprowadzane do tego obszaru deklaracji za poorednictwem parametrów formalnych (*fixed_parameter*s i *parameter_array*s) i *type_parameter*s. Treść składowej funkcji lub funkcji anonimowej, jeśli istnieje, jest uznawana za zagnieżdżoną w obrębie przestrzeni deklaracji zmiennej lokalnej. Jest to błąd dla przestrzeni lokalnej deklaracji zmiennej i zagnieżdżona przestrzeń deklaracji zmiennej lokalnej, która zawiera elementy o tej samej nazwie. W tym celu w ramach zagnieżdżonej deklaracji nie można zadeklarować zmiennej lokalnej ani stałej o takiej samej nazwie jak zmienna lokalna lub stała w otaczającym obszarze deklaracji. Możliwe jest, aby dwie spacje deklaracji zawierały elementy o tej samej nazwie, o ile nie zawiera ona żadnej z nich.
*  Każdy *blok* lub *switch_block* , jak również instrukcja *for*, *foreach* i *using* , tworzy przestrzeń lokalnej deklaracji zmiennej dla zmiennych lokalnych i stałych lokalnych. Nazwy są wprowadzane do tego obszaru deklaracji za poorednictwem *local_variable_declaration*s i *local_constant_declaration*s. Należy zauważyć, że Bloki występujące jako lub w treści elementu członkowskiego funkcji lub funkcji anonimowej są zagnieżdżone w obszarze deklaracji zmiennej lokalnej zadeklarowanej przez te funkcje dla ich parametrów. W rezultacie jest to błąd, na przykład, Metoda ze zmienną lokalną i parametr o tej samej nazwie.
*  Każdy *blok* lub *switch_block* tworzy oddzielne miejsce deklaracji dla etykiet. Nazwy są wprowadzane do tego obszaru deklaracji za pomocą *labeled_statement*s, a nazwy są przywoływane przez *goto_statement*s. ***Miejsce deklaracji etykiety*** bloku zawiera wszystkie zagnieżdżone bloki. W tym celu w bloku zagnieżdżonym nie można zadeklarować etykiety o takiej samej nazwie jak etykieta w otaczającym bloku.

Kolejność tekstu, w której zadeklarowane są nazwy, zazwyczaj nie ma znaczenia. W szczególności kolejność tekstu nie jest istotna dla deklaracji i użycia przestrzeni nazw, stałych, metod, właściwości, zdarzeń, indeksatorów, konstruktorów wystąpień, destruktorów, konstruktorów statycznych i typów. Kolejność deklaracji jest istotna w następujący sposób:

*  Kolejność deklaracji dla deklaracji pola i deklaracji zmiennych lokalnych określa kolejność, w jakiej są wykonywane ich inicjatory (jeśli istnieją).
*  Zmienne lokalne muszą być zdefiniowane przed użyciem ([zakresy](basic-concepts.md#scopes)).
*  Kolejność deklaracji deklaracji elementu członkowskiego wyliczenia ([składowe wyliczenia](enums.md#enum-members)) jest istotna, gdy wartości *constant_expression* są pomijane.

Obszar deklaracji przestrzeni nazw to "otwarta zakończona", a dwie deklaracje przestrzeni nazw o tej samej w pełni kwalifikowanej nazwie współtworzą ten sam obszar deklaracji. Na przykład:
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

Powyższe dwie deklaracje przestrzeni nazw przyczyniają się do tego samego obszaru deklaracji, w tym przypadku deklarując dwie klasy z w pełni kwalifikowanymi nazwami `Megacorp.Data.Customer` i `Megacorp.Data.Order`. Ponieważ dwie deklaracje przyczyniają się do tego samego obszaru deklaracji, powodowały to błąd czasu kompilacji, jeśli każda z nich zawiera deklarację klasy o tej samej nazwie.

Jak określono powyżej, obszar deklaracji bloku zawiera wszystkie zagnieżdżone bloki. W związku z tym w poniższym przykładzie metody `F` i `G` powodują błąd czasu kompilacji, ponieważ nazwa `i` jest zadeklarowana w bloku zewnętrznym i nie można jej ponownie zadeklarować w bloku wewnętrznym. Jednakże metody `H` i `I` są prawidłowe, ponieważ dwa `i` są zadeklarowane w oddzielnych blokach niezagnieżdżonych.

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

## <a name="members"></a>Members

Przestrzenie nazw i typy mają ***składowe***. Elementy członkowskie jednostki są ogólnie dostępne przy użyciu kwalifikowanej nazwy, która rozpoczyna się od odwołania do jednostki, po której następuje token "`.`", po którym następuje nazwa elementu członkowskiego.

Elementy członkowskie typu są zadeklarowane w deklaracji typu lub ***dziedziczone*** z klasy bazowej typu. Gdy typ dziedziczy z klasy bazowej, wszystkie elementy członkowskie klasy podstawowej, z wyjątkiem konstruktorów wystąpień, destruktorów i konstruktorów statycznych, stają się elementami członkowskimi typu pochodnego. Zadeklarowana dostępność składowej klasy bazowej nie kontroluje, czy element członkowski jest dziedziczony — dziedziczenie rozciąga się do dowolnego elementu członkowskiego, który nie jest konstruktorem wystąpienia, konstruktorem statycznym ani destruktorem. Jednak Dziedziczony element członkowski może nie być dostępny w typie pochodnym, z powodu jego zadeklarowanej dostępności ([zadeklarowanej dostępności](basic-concepts.md#declared-accessibility)) lub jest ukryty przez deklarację w samym typie ([ukrywając poprzez dziedziczenie](basic-concepts.md#hiding-through-inheritance)).

### <a name="namespace-members"></a>Elementy członkowskie przestrzeni nazw

Przestrzenie nazw i typy, które nie mają otaczającej przestrzeni nazw, są elementami członkowskimi ***globalnej przestrzeni nazw***. Odnosi się to bezpośrednio do nazw zadeklarowanych w globalnej przestrzeni deklaracji.

Przestrzenie nazw i typy zadeklarowane w przestrzeni nazw są elementami członkowskimi tej przestrzeni nazw. Odnosi się to bezpośrednio do nazw zadeklarowanych w obszarze deklaracji przestrzeni nazw.

Przestrzenie nazw nie mają ograniczeń dostępu. Nie można zadeklarować prywatnych, chronionych lub wewnętrznych przestrzeni nazw, a nazwy przestrzeni nazw są zawsze publicznie dostępne.

### <a name="struct-members"></a>Elementy członkowskie struktury

Elementy członkowskie struktury są składowymi zadeklarowanymi w strukturze i składowymi dziedziczonymi z bezpośredniej klasy podstawowej struktury `System.ValueType` i pośrednią klasę bazową `object`.

Elementy członkowskie typu prostego odpowiadają bezpośrednio elementom członkowskim typu struct, które są aliase według typu prostego:

*  Elementy członkowskie `sbyte` są członkami struktury `System.SByte`.
*  Elementy członkowskie `byte` są członkami struktury `System.Byte`.
*  Elementy członkowskie `short` są członkami struktury `System.Int16`.
*  Elementy członkowskie `ushort` są członkami struktury `System.UInt16`.
*  Elementy członkowskie `int` są członkami struktury `System.Int32`.
*  Elementy członkowskie `uint` są członkami struktury `System.UInt32`.
*  Elementy członkowskie `long` są członkami struktury `System.Int64`.
*  Elementy członkowskie `ulong` są członkami struktury `System.UInt64`.
*  Elementy członkowskie `char` są członkami struktury `System.Char`.
*  Elementy członkowskie `float` są członkami struktury `System.Single`.
*  Elementy członkowskie `double` są członkami struktury `System.Double`.
*  Elementy członkowskie `decimal` są członkami struktury `System.Decimal`.
*  Elementy członkowskie `bool` są członkami struktury `System.Boolean`.

### <a name="enumeration-members"></a>Elementy członkowskie wyliczenia

Elementy członkowskie wyliczenia to stałe zadeklarowane w wyliczeniu i składowe dziedziczone z bezpośredniej klasy podstawowej wyliczenia `System.Enum` i pośrednich klas podstawowych `System.ValueType` i `object`.

### <a name="class-members"></a>Elementy członkowskie klasy

Elementy członkowskie klasy są składowymi zadeklarowanymi w klasie i składowymi dziedziczonymi z klasy podstawowej (z wyjątkiem klasy `object`, która nie ma klasy bazowej). Elementy członkowskie dziedziczone z klasy bazowej obejmują stałe, pola, metody, właściwości, zdarzenia, indeksatory, operatory i typy klasy bazowej, ale nie konstruktorów wystąpień, destruktorów i konstruktorów statycznych klasy bazowej. Składowe klasy bazowej są dziedziczone bez względu na ich dostępność.

Deklaracja klasy może zawierać deklaracje stałych, pól, metod, właściwości, zdarzeń, indeksatorów, operatorów, konstruktorów wystąpień, destruktorów, konstruktorów statycznych i typów.

Elementy członkowskie `object` i `string` odpowiadają bezpośrednio członkom typów klas, których aliasy:

*  Elementy członkowskie `object` są członkami klasy `System.Object`.
*  Elementy członkowskie `string` są członkami klasy `System.String`.

### <a name="interface-members"></a>Elementy członkowskie interfejsu

Elementy członkowskie interfejsu są elementami zadeklarowanymi w interfejsie i we wszystkich interfejsach podstawowych interfejsu. Elementy członkowskie klasy `object` nie są, ściśle mówiące, członkami dowolnego interfejsu ([elementy członkowskie interfejsu](interfaces.md#interface-members)). Jednakże elementy członkowskie klasy `object` są dostępne za pośrednictwem wyszukiwania elementów członkowskich w dowolnym typie interfejsu ([wyszukiwanie elementu członkowskiego](expressions.md#member-lookup)).

### <a name="array-members"></a>Elementy członkowskie tablicy

Elementy członkowskie tablicy są elementami dziedziczonymi z klasy `System.Array`.

### <a name="delegate-members"></a>Deleguj członków

Elementy członkowskie delegata są elementami dziedziczonymi z klasy `System.Delegate`.

## <a name="member-access"></a>Dostęp do elementu członkowskiego

Deklaracje elementów członkowskich umożliwiają kontrolę dostępu do elementów członkowskich. Dostępność elementu członkowskiego jest określana przez zadeklarowane dostępność ([zadeklarowane dostępność](basic-concepts.md#declared-accessibility)) elementu członkowskiego połączonego z ułatwieniami dostępu do typu, jeśli istnieje.

Gdy dostęp do określonego elementu członkowskiego jest dozwolony, członek jest uznawany za ***dostępny***. Z drugiej strony, gdy dostęp do określonego elementu członkowskiego jest niedozwolony, członek jest uznawany za ***niedostępny***. Dostęp do elementu członkowskiego jest dozwolony, gdy lokalizacja tekstowa, w której odbywa się dostęp, jest uwzględniona w domenie dostępności ([domeny ułatwień](basic-concepts.md#accessibility-domains)dostępu) elementu członkowskiego.

### <a name="declared-accessibility"></a>Zadeklarowane ułatwienia dostępu

***Zadeklarowana dostępność*** elementu członkowskiego może być jedną z następujących czynności:

*  Public, który jest wybierany przez dołączenie modyfikatora `public` w deklaracji elementu członkowskiego. Intuicyjne znaczenie `public` to "dostęp bez ograniczeń".
*  Chroniony, który jest wybierany przez dołączenie modyfikatora `protected` w deklaracji elementu członkowskiego. Intuicyjne znaczenie `protected` to "dostęp ograniczony do klasy zawierającej lub typów pochodzących od klasy zawierającej".
*  Wewnętrzna, która jest wybierana przez dołączenie modyfikatora `internal` w deklaracji elementu członkowskiego. Intuicyjne znaczenie `internal` to "dostęp ograniczony do tego programu".
*  Chroniona wewnętrznie (czyli chroniona lub wewnętrzna), która jest wybierana przez dołączenie do `protected` i modyfikatora `internal` w deklaracji elementu członkowskiego. Intuicyjne znaczenie `protected internal` to "dostęp ograniczony do tego programu lub typów pochodzących od klasy zawierającej".
*  Prywatny, który jest wybierany przez dołączenie modyfikatora `private` w deklaracji elementu członkowskiego. Intuicyjne znaczenie `private` to "dostęp ograniczony do typu zawierającego".

W zależności od kontekstu, w którym jest realizowana Deklaracja elementu członkowskiego, dozwolone są tylko niektóre typy zadeklarowanych ułatwień dostępu. Ponadto, gdy Deklaracja elementu członkowskiego nie zawiera żadnych modyfikatorów dostępu, kontekst, w którym odbywa się deklaracja, określa domyślny dostępną dostępność.

*  Przestrzenie nazw niejawnie mają zadeklarowaną dostępność `public`. W deklaracjach przestrzeni nazw nie są dozwolone Modyfikatory dostępu.
*  Typy zadeklarowane w jednostkach kompilacji lub przestrzeniach nazw mogą mieć `public` lub `internal` zadeklarowane ułatwienia dostępu i domyślne dla `internal` zadeklarowane ułatwienia dostępu.
*  Elementy członkowskie klasy mogą mieć jeden z pięciu rodzajów zadeklarowanych ułatwień dostępu i domyślne do `private` zadeklarowane ułatwienia dostępu. (Należy zauważyć, że typ zadeklarowany jako składowa klasy może mieć którykolwiek z pięciu rodzajów zadeklarowanej dostępności, podczas gdy typ zadeklarowany jako element członkowski przestrzeni nazw może mieć tylko `public` lub `internal` zadeklarowany dostęp.)
*  Elementy członkowskie struktury mogą mieć `public`, `internal` lub `private` zadeklarowane ułatwienia dostępu i domyślne dla `private` zadeklarowane ułatwienia dostępu, ponieważ struktury są niejawnie zapieczętowane. Elementy członkowskie struktury wprowadzone w strukturze (która nie jest dziedziczona przez tę strukturę) nie mogą mieć zadeklarowanej dostępności `protected` ani `protected internal`. (Należy zauważyć, że typ zadeklarowany jako element członkowski struktury może mieć `public`, `internal` lub `private` zadeklarowane ułatwienia dostępu, podczas gdy typ zadeklarowany jako składowa przestrzeni nazw może mieć tylko `public` lub `internal` zadeklarowane ułatwienia dostępu.)
*  Elementy członkowskie interfejsu niejawnie mają zadeklarowaną dostępność `public`. W deklaracjach składowych interfejsu nie można używać modyfikatorów dostępu.
*  Elementy członkowskie wyliczenia niejawnie mają zadeklarowaną dostępność `public`. Modyfikatory dostępu są niedozwolone w deklaracjach składowych wyliczenia.

### <a name="accessibility-domains"></a>Domeny ułatwień dostępu

***Domena dostępności*** elementu członkowskiego składa się z sekcji (ewentualnie rozłączonych) tekstu programu, w którym dozwolony jest dostęp do elementu członkowskiego. Aby można było zdefiniować domenę dostępności elementu członkowskiego, członek jest nazywany ***najwyższego poziomu*** , jeśli nie jest zadeklarowany w obrębie typu, a element członkowski jest określany jako ***zagnieżdżony*** , jeśli jest zadeklarowany w innym typie. Ponadto ***tekst programu*** w programie jest zdefiniowany jako cały tekst programu zawarty we wszystkich plikach źródłowych programu, a tekst programu typu jest zdefiniowany jako cały tekst programu zawarty w *type_declaration*s tego typu (w tym, możliwe typy, które są zagnieżdżone w typie).

Domena dostępności wstępnie zdefiniowanego typu (na przykład `object`, `int` lub `double`) jest nieograniczona.

Domena dostępności niepowiązanego typu najwyższego poziomu `T` ([powiązane i niepowiązane typy](types.md#bound-and-unbound-types)) zadeklarowane w programie `P` zdefiniowane w następujący sposób:

*  Jeśli deklarowana dostępność `T` jest `public`, domena dostępności `T` jest tekstem programu `P` i dowolnym programem, który odwołuje się do `P`.
*  Jeśli deklarowana dostępność `T` jest `internal`, domena dostępności `T` jest tekstem programu `P`.

Z tych definicji wynika, że domena dostępności niepowiązanego typu najwyższego poziomu jest zawsze co najmniej programem tekstowym programu, w którym ten typ jest zadeklarowany.

Domena dostępności dla konstruowanego typu `T<A1, ..., An>` jest częścią wspólną domeny dostępności niepowiązanego typu ogólnego `T` i domen dostępności argumentów typu `A1, ..., An`.

Domena dostępności zagnieżdżonej składowej `M` zadeklarowana w typie `T` w programie `P` jest zdefiniowana w następujący sposób (zwracając uwagę, że `M` może być typem):

*  Jeśli deklarowana dostępność `M` jest `public`, domena dostępności `M` jest domeną dostępności `T`.
*  Jeśli deklarowana dostępność `M` jest `protected internal`, niech `D` to Unia programu tekstowego `P` i tekst programu dowolnego typu pochodzącego od `T`, który jest zadeklarowany poza `P`. Domena dostępności `M` to część wspólna domeny dostępności `T` z `D`.
*  Jeśli deklarowana dostępność `M` jest `protected`, niech `D` będzie złożeniem tekstu programu `T` i tekstu programu dowolnego typu pochodzącego z `T`. Domena dostępności `M` to część wspólna domeny dostępności `T` z `D`.
*  Jeśli deklarowana dostępność `M` jest `internal`, domena dostępności `M` jest częścią wspólną domeny dostępności `T` z tekstem programu `P`.
*  Jeśli deklarowana dostępność `M` jest `private`, domena dostępności `M` jest tekstem programu `T`.

Z tych definicji następuje, że domena dostępności zagnieżdżonego elementu członkowskiego jest zawsze co najmniej tekstem programu typu, w którym jest zadeklarowany element członkowski. Ponadto, gdy domena dostępności elementu członkowskiego nigdy nie jest większa niż domena dostępności typu, w którym jest zadeklarowany element członkowski.

W intuicyjnych warunkach podczas uzyskiwania dostępu do typu lub elementu członkowskiego `M` są oceniane następujące kroki, aby upewnić się, że dostęp jest dozwolony:

*  Po pierwsze, jeśli `M` jest zadeklarowany w obrębie typu (w przeciwieństwie do jednostki kompilacji lub przestrzeni nazw), błąd czasu kompilacji występuje, jeśli ten typ nie jest dostępny.
*  Następnie Jeśli `M` jest `public`, dostęp jest dozwolony.
*  W przeciwnym razie, jeśli wartość `M` jest `protected internal`, dostęp jest dozwolony, jeśli wystąpi w programie, w którym jest zadeklarowany `M` lub jeśli występuje w klasie pochodnej klasy, w której jest zadeklarowana `M` i odbywa się za pośrednictwem typu klasy pochodnej ([chronione dostęp do elementów członkowskich wystąpienia](basic-concepts.md#protected-access-for-instance-members)).
*  W przeciwnym razie, jeśli `M` jest `protected`, dostęp jest dozwolony, jeśli występuje w klasie, w której jest zadeklarowany `M` lub jeśli występuje w klasie pochodzącej od klasy, w której jest zadeklarowany `M` i odbywa się za pośrednictwem typu klasy pochodnej ([chronione dostęp do elementów członkowskich wystąpienia](basic-concepts.md#protected-access-for-instance-members)).
*  W przeciwnym razie, jeśli `M` jest `internal`, dostęp jest dozwolony, jeśli występuje w programie, w którym zadeklarowano `M`.
*  W przeciwnym razie, jeśli `M` jest `private`, dostęp jest dozwolony, jeśli występuje w ramach typu, w którym zadeklarowano `M`.
*  W przeciwnym razie typ lub element członkowski jest niedostępny i wystąpi błąd kompilacji.

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
klasy i składowe mają następujące domeny ułatwień dostępu:

*  Domena dostępności `A` i `A.X` jest nieograniczona.
*  Domena dostępności `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X` i `B.C.Y` jest tekstem programu zawierającego program.
*  Domena dostępności `A.Z` jest tekstem programu `A`.
*  Domena dostępności `B.Z` i `B.D` to tekst programu `B`, w tym tekst programu `B.C` i `B.D`.
*  Domena dostępności `B.C.Z` jest tekstem programu `B.C`.
*  Domena dostępności `B.D.X` i `B.D.Y` to tekst programu `B`, w tym tekst programu `B.C` i `B.D`.
*  Domena dostępności `B.D.Z` jest tekstem programu `B.D`.

Jak pokazano na przykładzie, domena dostępności elementu członkowskiego nigdy nie jest większa niż typ zawierającego. Na przykład mimo że wszyscy członkowie `X` mają publicznie zadeklarowane ułatwienia dostępu, a `A.X` mają domeny dostępności, które są ograniczone przez typ zawierający.

Zgodnie z opisem w części [Członkowie](basic-concepts.md#members), wszyscy członkowie klasy podstawowej, z wyjątkiem konstruktorów wystąpień, destruktory i konstruktory statyczne są dziedziczone przez typy pochodne. Obejmuje to nawet prywatne elementy członkowskie klasy bazowej. Jednak domena dostępności prywatnego elementu członkowskiego zawiera tylko tekst programu typu, w którym jest zadeklarowany element członkowski. W przykładzie
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
Klasa `B` dziedziczy prywatną składową `x` z klasy `A`. Ponieważ element członkowski jest prywatny, jest dostępny tylko w *class_body* `A`. W ten sposób dostęp do `b.x` powiedzie się w metodzie `A.F`, ale w metodzie `B.F` kończy się niepowodzeniem.

### <a name="protected-access-for-instance-members"></a>Chroniony dostęp dla członków wystąpienia

Gdy dostęp do elementu członkowskiego wystąpienia `protected` jest uzyskiwany poza tekstem programu klasy, w którym jest zadeklarowany, a w przypadku uzyskania dostępu do składowej wystąpienia `protected internal` poza tekstem programu, w którym jest zadeklarowany, dostęp musi odbywać się w obrębie klasy Deklaracja, która pochodzi od klasy, w której jest zadeklarowana. Ponadto dostęp musi odbywać się za pośrednictwem wystąpienia tego typu klasy pochodnej lub klasy z tego typu. To ograniczenie uniemożliwia jednej klasie pochodnej dostęp do chronionych elementów członkowskich innych klas pochodnych, nawet gdy elementy członkowskie są dziedziczone z tej samej klasy bazowej.

Zezwól `B` będzie klasą bazową, która deklaruje element członkowski chronionego wystąpienia `M` i pozwól `D` będzie klasą pochodzącą z `B`. W *class_body* `D` dostęp do `M` może mieć jedną z następujących form:

*  Niekwalifikowana *TYPE_NAME* lub *primary_expression* formularza `M`.
*  *Primary_expression* formularza `E.M`, pod warunkiem, że typ `E` jest `T` lub klasy pochodnej od `T`, gdzie `T` jest typem klasy `D` lub typem klasy skonstruowanym z `D`
*  *Primary_expression* formularza `base.M`.

Oprócz tych formularzy dostępu Klasa pochodna może uzyskać dostęp do konstruktora chronionych wystąpień klasy bazowej w *constructor_initializer* ([inicjatory konstruktora](classes.md#constructor-initializers)).

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
w `A` możliwe jest uzyskanie dostępu do `x` przez wystąpienia obu `A` i `B`, ponieważ w obu przypadkach dostęp odbywa się za pośrednictwem wystąpienia `A` lub klasy pochodzącej z `A`. Jednak w `B` nie jest możliwe uzyskanie dostępu do `x` za pośrednictwem wystąpienia `A`, ponieważ `A` nie pochodzi od `B`.

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
trzy przypisania do `x` są dozwolone, ponieważ są one wykonywane za pośrednictwem wystąpień typów klas zbudowanych z typu ogólnego.

### <a name="accessibility-constraints"></a>Ograniczenia dotyczące ułatwień dostępu

Kilka konstrukcji w C# języku wymaga, aby typ był ***co najmniej*** taki sam jak element członkowski lub inny typ. Typ `T` jest uznawany za co najmniej jako element członkowski lub typ `M`, jeśli domena dostępności `T` jest nadzbiorem domeny dostępności `M`. Innymi słowy, `T` jest co najmniej tak samo dostępne, jak `M`, jeśli `T` jest dostępny we wszystkich kontekstach, w których jest dostępny `M`.

Istnieją następujące ograniczenia dotyczące ułatwień dostępu:

*  Bezpośrednia klasa bazowa typu klasy musi być co najmniej równa dostępności jako samego typu klasy.
*  Jawne interfejsy podstawowe typu interfejsu muszą być co najmniej takie same jak typ interfejsu.
*  Typ zwracany i typy parametrów typu delegata muszą być co najmniej tak samo jak typ delegata.
*  Typ stałej musi być co najmniej tak samo jak jako stała.
*  Typ pola musi być co najmniej tak samo jak w przypadku samego pola.
*  Typ zwracany i typy parametrów metody muszą być co najmniej tak samo samo jak metoda.
*  Typ właściwości musi być co najmniej taki sam jak wartość właściwości.
*  Typ zdarzenia musi być co najmniej tak samo jak w przypadku samego zdarzenia.
*  Typ i typy parametrów indeksatora muszą być co najmniej tak samo dostępne jak indeksator.
*  Typ zwracany i typy parametrów operatora muszą być co najmniej takie same jak dla samego operatora.
*  Typy parametrów konstruktora wystąpienia muszą być co najmniej tak samo samo jak Konstruktor wystąpień.

W przykładzie
```csharp
class A {...}

public class B: A {...}
```
Klasa `B` powoduje błąd czasu kompilacji, ponieważ `A` nie jest co najmniej równa dostępności jako `B`.

Podobnie, w przykładzie
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
Metoda `H` w `B` powoduje błąd kompilacji, ponieważ typ zwracany `A` nie jest co najmniej taki sam jak metoda.

## <a name="signatures-and-overloading"></a>Sygnatury i Przeciążenie

Metody, konstruktory wystąpień, indeksatory i operatory są scharakteryzowane przez ich ***sygnatury***:

*  Podpis metody składa się z nazwy metody, liczby parametrów typu i typu i rodzaju (wartości, odwołania lub danych wyjściowych) każdego z parametrów formalnych, które są uwzględniane w kolejności od lewej do prawej. W tym celu wszelkie parametry typu metody, która występuje w typie parametru formalnego, nie są identyfikowane przez jego nazwę, ale według pozycji porządkowej na liście argumentów typu metody. Sygnatura metody w szczególności nie zawiera typu zwracanego, modyfikatora `params`, który może być określony dla tego samego parametru lub dla ograniczeń parametru typu opcjonalnego.
*  Sygnatura konstruktora wystąpienia składa się z typu i rodzaju (wartości, odwołania lub danych wyjściowych) każdego z jego parametrów formalnych, w kolejności od lewej do prawej. Podpis konstruktora wystąpienia w szczególności nie zawiera modyfikatora `params`, który może być określony dla każdego parametru z prawej strony.
*  Podpis indeksatora składa się z typu każdego z parametrów formalnych, który jest traktowany w kolejności od lewej do prawej. Podpis indeksatora w szczególności nie zawiera typu elementu ani nie zawiera modyfikatora `params`, który może być określony dla najwyższego parametru.
*  Podpis operatora składa się z nazwy operatora i typu każdego z jego parametrów formalnych, w kolejności od lewej do prawej. Podpis operatora jawnie nie zawiera typu wyniku.

Sygnatury są mechanizmem włączania do ***przeciążenia*** elementów członkowskich w klasach, strukturach i interfejsach:

*  Przeciążanie metod pozwala klasy, struktury lub interfejsu zadeklarować wiele metod o tej samej nazwie, pod warunkiem, że ich sygnatury są unikatowe w ramach tej klasy, struktury lub interfejsu.
*  Przeciążanie konstruktorów wystąpień pozwala klasie lub strukturze zadeklarować wiele konstruktorów wystąpień, pod warunkiem, że ich sygnatury są unikatowe w obrębie tej klasy lub struktury.
*  Przeciążanie indeksatorów umożliwia klasy, struktury lub interfejsu zadeklarować wiele indeksatorów, pod warunkiem, że ich sygnatury są unikatowe w ramach tej klasy, struktury lub interfejsu.
*  Przeciążanie operatorów pozwala klasie lub strukturze zadeklarować wiele operatorów o tej samej nazwie, pod warunkiem, że ich sygnatury są unikatowe w tej klasie lub strukturze.

Chociaż Modyfikatory parametrów `out` i `ref` są uważane za część podpisu, składowe zadeklarowane w pojedynczym typie nie mogą różnić się w podpisie wyłącznie przez `ref` i `out`. Błąd czasu kompilacji występuje, jeśli dwa składowe są zadeklarowane w tym samym typie z podpisami, które byłyby takie same, jeśli wszystkie parametry w obu metodach z modyfikatorami `out` zostały zmienione na Modyfikatory `ref`. Do innych celów dopasowywania podpisów (np. ukrycia lub przesłaniania) `ref` i `out` są uważane za część podpisu i nie pasują do siebie nawzajem. (To ograniczenie umożliwia łatwe tłumaczenie C# programów na Common Language Infrastructure (CLI), które nie udostępniają sposobu definiowania metod, które różnią się wyłącznie w `ref` i `out`).

Na potrzeby podpisów typy `object` i `dynamic` są uważane za takie same. Elementy członkowskie zadeklarowane w pojedynczym typie mogą nie różnić się w podpisie wyłącznie przez `object` i `dynamic`.

Poniższy przykład pokazuje zestaw przeciążonych deklaracji metod wraz z ich podpisami.
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

Należy zauważyć, że Modyfikatory parametrów `ref` i `out` ([Parametry metody](classes.md#method-parameters)) są częścią sygnatury. W ten sposób `F(int)` i `F(ref int)` są unikatowymi sygnaturami. Nie można jednak zadeklarować `F(ref int)` i `F(out int)` w tym samym interfejsie, ponieważ ich sygnatury różnią się wyłącznie `ref` i `out`. Należy również zauważyć, że typ zwracany i modyfikator `params` nie są częścią podpisu, więc nie jest możliwe przeciążanie wyłącznie na podstawie zwracanego typu lub przy włączaniu lub wykluczeniu modyfikatora `params`. W związku z tym deklaracje metod `F(int)` i `F(params string[])` zidentyfikowane powyżej powodują błąd w czasie kompilacji.

## <a name="scopes"></a>Zakresy

***Zakres*** nazwy jest regionem tekstu programu, w którym można odwołać się do jednostki zadeklarowanej przez nazwę bez kwalifikacji nazwy. Zakresy mogą być ***zagnieżdżane***, a zakres wewnętrzny może redeklarować znaczenie nazwy z zewnętrznego zakresu (nie jest to jednak konieczne usunięcie ograniczenia wynikającego z [deklaracji](basic-concepts.md#declarations) , które w bloku zagnieżdżonym nie można zadeklarować zmiennej lokalnej przy użyciu taka sama nazwa jak zmienna lokalna w otaczającym bloku. Nazwa z zewnętrznego zakresu jest następnie określana jako ***Ukryta*** w regionie tekstu programu objętego zakresem wewnętrznym, a dostęp do nazwy zewnętrznej jest możliwy tylko przez zakwalifikowanie nazwy.

*  Zakresem elementu członkowskiego przestrzeni nazw zadeklarowanego przez *namespace_member_declaration* ([elementy członkowskie obszaru nazw](namespaces.md#namespace-members)) bez otaczającego *namespace_declaration* jest cały tekst programu.
*  Zakres elementu członkowskiego przestrzeni nazw zadeklarowany przez element *namespace_member_declaration* w *namespace_declaration* , którego w pełni kwalifikowana nazwa jest `N` to *namespace_body* wszystkich *namespace_declaration* , których pełna kwalifikowana nazwa jest `N` lub rozpoczyna się od `N`, po którym następuje kropka.
*  Zakres nazwy zdefiniowany przez *extern_alias_directive* jest rozszerzany na *using_directive*s, *global_attributes* i *namespace_member_declaration*s z jego jednostki kompilacji lub treści przestrzeni nazw. *Extern_alias_directive* nie współtworzy żadnych nowych członków do źródłowej przestrzeni deklaracji. Innymi słowy, *extern_alias_directive* nie jest przechodni, ale raczej wpływa tylko na jednostkę kompilacji lub treść przestrzeni nazw, w której występuje.
*  Zakres nazwy zdefiniowany lub zaimportowany przez *using_directive* ([dyrektywy using](namespaces.md#using-directives)) rozciąga się na *namespace_member_declaration*s *compilation_unit* lub *namespace_body* , w którym *using_directive* występuje. *Using_directive* może wprowadzać zero lub większą liczbę nazw, typów lub elementów członkowskich, które są dostępne w ramach określonego *compilation_unit* lub *namespace_body*, ale nie współtworzy żadnych nowych członków do źródłowej przestrzeni deklaracji. Innymi słowy, *using_directive* nie jest przechodni, ale ma wpływ tylko na *compilation_unit* lub *namespace_body* , w których występuje.
*  Zakres parametru typu zadeklarowanego przez *type_parameter_list* w *class_declaration* ([deklaracji klasy](classes.md#class-declarations)) to *class_base*, *type_parameter_constraints_clause*i *class_body* tego *elementu class_declaration*.
*  Zakres parametru typu zadeklarowany przez *type_parameter_list* w *struct_declaration* ([deklaracji struktury](structs.md#struct-declarations)) to *struct_interfaces*, *type_parameter_constraints_clause*i *struct_body* *struct_declaration*.
*  Zakres parametru typu zadeklarowany przez *type_parameter_list* w *interface_declaration* ([deklaracji interfejsu](interfaces.md#interface-declarations)) to *interface_base*, *type_parameter_constraints_clause*s i *interface_body* tego *interface_declaration*.
*  Zakres parametru typu zadeklarowany przez *type_parameter_list* w *delegate_declaration* ([deklaracje delegatów](delegates.md#delegate-declarations)) to *return_type*, *formal_parameter_list*i *type_parameter_constraints_clause* s tej *delegate_declaration*.
*  Zakres składowej zadeklarowanej przez *class_member_declaration* ([Treść klasy](classes.md#class-body)) to *class_body* , w której występuje deklaracja. Ponadto zakres elementu członkowskiego klasy rozciąga się na *class_body* tych klas pochodnych, które znajdują się w domenie dostępności ([domeny ułatwień dostępu](basic-concepts.md#accessibility-domains)) elementu członkowskiego.
*  Zakres elementu członkowskiego zadeklarowany przez *struct_member_declaration* ([elementy członkowskie struktury](structs.md#struct-members)) jest *struct_body* , w którym występuje deklaracja.
*  Zakres elementu członkowskiego zadeklarowany przez *enum_member_declaration* ([elementy członkowskie wyliczenia](enums.md#enum-members)) to *enum_body* , w której występuje deklaracja.
*  Zakres parametru zadeklarowany w *method_declaration* ([metody](classes.md#methods)) to *method_body* *method_declaration*.
*  Zakres parametru zadeklarowany w *indexer_declaration* ([indeksatory](classes.md#indexers)) to *accessor_declarations* tego *indexer_declaration*.
*  Zakres parametru zadeklarowany w *operator_declaration* ([Operatory](classes.md#operators)) jest *blokiem* tego *operator_declaration*.
*  Zakres parametru zadeklarowany w *constructor_declaration* ([Konstruktor wystąpień](classes.md#instance-constructors)) to *constructor_initializer* i *blok* *constructor_declaration*.
*  Zakres parametru zadeklarowany w elemencie *lambda_expression* ([anonimowe wyrażenia funkcji](expressions.md#anonymous-function-expressions)) to *anonymous_function_body* *lambda_expression*
*  Zakres parametru zadeklarowany w *anonymous_method_expression* ([wyrażenia funkcji anonimowej](expressions.md#anonymous-function-expressions)) jest *blokiem* tego *anonymous_method_expression*.
*  Zakres etykiety zadeklarowanej w *labeled_statement* ([instrukcje z etykietą](statements.md#labeled-statements)) to *blok* , w którym występuje deklaracja.
*  Zakres zmiennej lokalnej zadeklarowanej w *local_variable_declaration* ([lokalna deklaracja zmiennej](statements.md#local-variable-declarations)) to blok, w którym występuje deklaracja.
*  Zakres zmiennej lokalnej zadeklarowanej w *switch_block* instrukcji `switch` ([instrukcja SWITCH](statements.md#the-switch-statement)) to *switch_block*.
*  Zakres zmiennej lokalnej zadeklarowanej w *for_initializer* instrukcji `for` ([instrukcja for](statements.md#the-for-statement)) to *for_initializer*, *for_condition*, *for_iterator*i zawartej *instrukcji* Instrukcja `for`.
*  Zakres stałej lokalnej zadeklarowanej w *local_constant_declaration* ([lokalna deklaracja stała](statements.md#local-constant-declarations)) to blok, w którym występuje deklaracja. Jest to błąd czasu kompilacji, który odwołuje się do lokalnej stałej w pozycji tekstowej, która poprzedza jej *constant_declarator*.
*  Zakres zmiennej zadeklarowanej jako część elementu *foreach_statement*, *using_statement*, *lock_statement* lub *query_expression* jest określany przez rozszerzenie danej konstrukcji.

W zakresie przestrzeni nazw, klasy, struktury lub składowej wyliczenia można odwołać się do elementu członkowskiego w pozycji tekstowej, która poprzedza deklarację elementu członkowskiego. Na przykład:
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
W tym miejscu jest prawidłowy dla `F`, aby odwołać się do `i` przed zadeklarowaniem.

W zakresie zmiennej lokalnej jest to błąd czasu kompilacji, który odwołuje się do zmiennej lokalnej w pozycji tekstowej, która poprzedza *local_variable_declarator* zmiennej lokalnej. Na przykład:
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

W powyższej metodzie `F` pierwsze przypisanie do `i` nie odwołuje się do pola zadeklarowanego w zewnętrznym zakresie. Nie odnosi się do zmiennej lokalnej i powoduje błąd czasu kompilacji, ponieważ jest ona poprzedzona znakiem deklaracji zmiennej. W metodzie `G` użycie `j` w inicjatorze dla deklaracji `j` jest prawidłowe, ponieważ użycie nie poprzedza *local_variable_declarator*. W metodzie `H` kolejne *local_variable_declarator* prawidłowo odwołują się do zmiennej lokalnej zadeklarowanej we wcześniejszej *local_variable_declarator* w tym samym *local_variable_declaration*.

Reguły określania zakresu dla zmiennych lokalnych są zaprojektowane w celu zagwarantowania, że znaczenie nazwy używanej w kontekście wyrażenia jest zawsze takie samo w bloku. Jeśli zakres zmiennej lokalnej miał zostać rozbudowany tylko od jej deklaracji do końca bloku, wówczas w powyższym przykładzie pierwsze przypisanie zostanie przypisane do zmiennej wystąpienia, a drugie przypisanie zostanie przypisane do zmiennej lokalnej, co może prowadzić do Błędy czasu kompilacji, jeśli instrukcje bloku były później zmieniane.

Znaczenie nazwy w bloku może się różnić w zależności od kontekstu, w którym jest używana nazwa. W przykładzie
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
Nazwa `A` jest używana w kontekście wyrażenia w celu odwoływania się do zmiennej lokalnej `A` i w kontekście typu, aby odwołać się do klasy `A`.

### <a name="name-hiding"></a>Ukrywanie nazwy

Zakres jednostki zwykle obejmuje więcej tekstu programu niż miejsce zadeklarowane w jednostce. W szczególności zakres jednostki może zawierać deklaracje wprowadzające nowe obszary deklaracji zawierające jednostki o tej samej nazwie. Takie deklaracje powodują, że oryginalna jednostka zostanie ***Ukryta***. Z drugiej strony jednostka jest ***widoczna*** , gdy nie jest ukryta.

Ukrywanie nazw występuje, gdy zakresy nakładają się na zagnieżdżenie i gdy zakresy nakładają się na dziedziczenie. Właściwości dwóch typów ukrywania są opisane w poniższych sekcjach.

#### <a name="hiding-through-nesting"></a>Ukrywanie przez zagnieżdżanie

Nazwa ukrywająca przy użyciu zagnieżdżenia może wystąpić w wyniku zagnieżdżania przestrzeni nazw lub typów w przestrzeni nazw, w wyniku zagnieżdżania typów w obrębie klas lub struktur, a także jako wynik deklaracji parametrów i zmiennych lokalnych.

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
w metodzie `F` zmienna wystąpienia `i` jest ukryta przez zmienną lokalną `i`, ale w ramach metody `G`, `i` nadal odwołuje się do zmiennej wystąpienia.

Gdy nazwa w zakresie wewnętrznym ukrywa nazwę w zewnętrznym zakresie, ukrywa wszystkie załadowane wystąpienia tej nazwy. W przykładzie
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
Wywołanie `F(1)` wywołuje `F` zadeklarowane w `Inner`, ponieważ wszystkie zewnętrzne wystąpienia `F` są ukrywane przez wewnętrzną deklarację. Z tego samego powodu wywołanie `F("Hello")` powoduje błąd w czasie kompilacji.

#### <a name="hiding-through-inheritance"></a>Ukrywanie poprzez dziedziczenie

Nazwa ukrywając przy użyciu dziedziczenia występuje, gdy klasy lub struktury ponownie deklarują nazwy dziedziczone z klas bazowych. Ten typ ukrywania nazwy ma jedną z następujących form:

*  Stałe, pole, właściwość, zdarzenie lub typ wprowadzone w klasie lub strukturze ukrywa wszystkie składowe klasy bazowej o tej samej nazwie.
*  Metoda wprowadzona w klasie lub strukturze ukrywa wszystkie składowe klas podstawowych niebędących metodami o tej samej nazwie i wszystkie metody klasy bazowej o tej samej sygnaturze (nazwa metody i liczba parametrów, modyfikatory i typy).
*  Indeksator wprowadzony w klasie lub strukturze ukrywa wszystkie indeksatory klasy bazowej o tej samej sygnaturze (liczba parametrów i typy).

Reguły dotyczące deklaracji operatora ([Operatory](classes.md#operators)) sprawiają, że Klasa pochodna nie może deklarować operatora z tym samym podpisem jako operatora w klasie bazowej. W tym celu operatory nigdy nie są ukrywane.

W przeciwieństwie do ukrywania nazwy z zewnętrznego zakresu ukrycie dostępnej nazwy z dziedziczonego zakresu powoduje ostrzeżenie o zgłoszeniu. W przykładzie
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
Deklaracja `F` w `Derived` powoduje, że ostrzeżenie zostanie zgłoszone. Ukrycie dziedziczonej nazwy nie jest jawnie błędem, ponieważ spowodowałoby to wykluczenie oddzielnego ewolucji klas bazowych. Na przykład Powyższa sytuacja może być spowodowana tym, że w nowszej wersji `Base` wprowadzono metodę `F`, która nie jest obecna we wcześniejszej wersji klasy. W przypadku powyższej sytuacji Wystąpił błąd, a następnie wszelkie zmiany wprowadzone do klasy podstawowej w bibliotece klas z odrębną wersją mogą potencjalnie spowodować, że klasy pochodne staną się nieprawidłowe.

Ostrzeżenie spowodowane ukrywaniem dziedziczonej nazwy można wyeliminować za pomocą modyfikatora `new`:
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

Modyfikator `new` wskazuje, że `F` w `Derived` ma wartość "New" i że jest rzeczywiście przeznaczony do ukrycia dziedziczonego elementu członkowskiego.

Deklaracja nowego elementu członkowskiego powoduje ukrycie dziedziczonego elementu członkowskiego tylko w zakresie nowego elementu członkowskiego.

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

W powyższym przykładzie deklaracja `F` w `Derived` powoduje ukrycie `F`, który został odziedziczony z `Base`, ale ponieważ nowy `F` w `Derived` ma dostęp prywatny, jego zakres nie obejmuje `MoreDerived`. W ten sposób wywołanie `F()` w `MoreDerived.G` jest prawidłowe i wywoła `Base.F`.

## <a name="namespace-and-type-names"></a>Nazwa przestrzeni nazw i typów

Kilka kontekstów w C# programie wymaga określenia *namespace_name* lub *TYPE_NAME* .

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

*Namespace_name* jest *namespace_or_type_name* , który odwołuje się do przestrzeni nazw. Poniższe rozwiązanie, jak opisano poniżej, *namespace_or_type_name* *namespace_name* musi odwoływać się do przestrzeni nazw lub w przeciwnym razie wystąpi błąd w czasie kompilacji. Żadne argumenty typu ([argumenty typu](types.md#type-arguments)) nie mogą być obecne w *namespace_name* (tylko typy mogą mieć argumenty typu).

*TYPE_NAME* jest *namespace_or_type_name* , który odwołuje się do typu. Poniższe rozwiązanie, jak opisano poniżej, *namespace_or_type_name* *TYPE_NAME* musi odwoływać się do typu lub w przeciwnym razie wystąpi błąd w czasie kompilacji.

Jeśli *namespace_or_type_name* jest aliasem kwalifikowanym, jego znaczenie jest zgodnie z opisem w [kwalifikatorach aliasów przestrzeni nazw](namespaces.md#namespace-alias-qualifiers). W przeciwnym razie *namespace_or_type_name* ma jedną z czterech postaci:

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

gdzie `I` jest pojedynczym identyfikatorem, `N` jest *namespace_or_type_name* i `<A1, ..., Ak>` jest opcjonalnym *type_argument_listem*. Gdy nie określono *type_argument_list* , należy wziąć pod uwagę, że `k` ma wartość zero.

Znaczenie *namespace_or_type_name* jest określone w następujący sposób:

*   Jeśli *namespace_or_type_name* ma postać `I` lub formularza `I<A1, ..., Ak>`:
    * Jeśli `K` wynosi zero, a *namespace_or_type_name* pojawia się w deklaracji metody ogólnej ([metod](classes.md#methods)) i jeśli ta deklaracja zawiera parametr typu ([parametry typu](classes.md#type-parameters)) o nazwie @ no__t-4, a następnie *namespace_or_type_ Nazwa* odwołuje się do tego parametru typu.
    * W przeciwnym razie, jeśli *namespace_or_type_name* pojawia się w deklaracji typu, a następnie dla każdego typu wystąpienia @ no__t-1 ([Typ wystąpienia](classes.md#the-instance-type)), rozpoczynając od typu wystąpienia tego typu deklaracji i kontynuując typ wystąpienia każdego zawrzeć deklarację klasy lub struktury (jeśli istnieje):
        * Jeśli `K` ma wartość zero, a deklaracja `T` zawiera parametr typu o nazwie @ no__t-2, wówczas *namespace_or_type_name* odwołuje się do tego parametru typu.
        * W przeciwnym razie, jeśli *namespace_or_type_name* pojawia się w treści deklaracji typu, a `T` lub dowolny z jej typów podstawowych zawiera zagnieżdżony dostępny typ o nazwie @ no__t-2 i `K` @ no__t-4type, a następnie *namespace_or_type _name* odwołuje się do tego typu skonstruowanego za pomocą podanych argumentów typu. Jeśli jest więcej niż jeden taki typ, zostanie wybrany typ zadeklarowany w ramach bardziej pochodnego typu. Należy zauważyć, że elementy członkowskie inne niż typy (stałe, pola, metody, właściwości, indeksatory, operatory, konstruktory wystąpień, destruktory i konstruktory statyczne) i składowe typu z inną liczbą parametrów typu są ignorowane podczas określania znaczenia *namespace_or_type_name*.
    * Jeśli poprzednie kroki nie powiodły się, dla każdej przestrzeni nazw @ no__t-0, rozpoczynając od przestrzeni nazw, w której występuje *namespace_or_type_name* , kontynuując z każdą otaczającą przestrzeń nazw (jeśli istnieje) i kończącą się globalną przestrzenią nazw, wykonaj następujące czynności: kroki są oceniane do momentu zlokalizowania jednostki:
        * Jeśli `K` to zero i `I` to nazwa przestrzeni nazw w @ no__t-2, a następnie:
            * Jeśli lokalizacja, w której występuje *namespace_or_type_name* , jest ujęta w deklarację przestrzeni nazw dla `N`, a Deklaracja przestrzeni nazw zawiera *extern_alias_directive* lub *using_alias_directive* , które kojarzą nazwę @ No __t-4 z przestrzenią nazw lub typem, a następnie *namespace_or_type_name* jest niejednoznaczny i występuje błąd kompilacji.
            * W przeciwnym razie *namespace_or_type_name* odwołuje się do przestrzeni nazw o nazwie `I` w `N`.
        * W przeciwnym razie, jeśli `N` zawiera dostępny typ o nazwie @ no__t-1 i `K` @ no__t-3type parametry, wówczas:
            * Jeśli wartość `K` wynosi zero, a lokalizacja, w której występuje *namespace_or_type_name* , jest ujęta w deklarację przestrzeni nazw dla `N`, a Deklaracja przestrzeni nazw zawiera *extern_alias_directive* lub *using_alias_directive* , które kojarzy nazwę @ no__t-5 z przestrzenią nazw lub typem, a następnie *namespace_or_type_name* jest niejednoznaczna i wystąpi błąd w czasie kompilacji.
            * W przeciwnym razie *namespace_or_type_name* odwołuje się do typu złożonego za pomocą podanych argumentów typu.
        * W przeciwnym razie, jeśli lokalizacja, w której występuje *namespace_or_type_name* , jest ujęta w deklarację przestrzeni nazw dla `N`:
            * Jeśli `K` to zero, a Deklaracja przestrzeni nazw zawiera *extern_alias_directive* lub *using_alias_directive* , która kojarzy nazwę @ no__t-3 z zaimportowaną przestrzenią nazw lub typem, *namespace_or_type_name* odwołuje się do tego Przestrzeń nazw lub typ.
            * W przeciwnym razie, jeśli przestrzenie nazw i deklaracje typów zaimportowane przez *using_namespace_directive*s i *using_alias_directive*z deklaracji przestrzeni nazw zawierają dokładnie jeden dostępny typ o nazwie @ no__t-2 i `K` @ no__t-4type parametry, a następnie *namespace_or_type_name* odwołuje się do tego typu skonstruowanego za pomocą podanych argumentów typu.
            * W przeciwnym razie, jeśli przestrzenie nazw i deklaracje typów zaimportowane przez *using_namespace_directive*s i *using_alias_directive*z deklaracji przestrzeni nazw zawierają więcej niż jeden dostępny typ o nazwie @ no__t-2 i `K` @ no__t-4type parametry, a następnie *namespace_or_type_name* jest niejednoznaczny i wystąpi błąd.
    * W przeciwnym razie *namespace_or_type_name* jest niezdefiniowany i wystąpi błąd w czasie kompilacji.
*  W przeciwnym razie *namespace_or_type_name* ma postać `N.I` lub formularza `N.I<A1, ..., Ak>`. `N` jest najpierw rozpoznawana jako *namespace_or_type_name*. Jeśli rozwiązanie `N` nie powiedzie się, wystąpi błąd w czasie kompilacji. W przeciwnym razie `N.I` lub `N.I<A1, ..., Ak>` są rozwiązywane w następujący sposób:
    * Jeśli `K` wynosi zero, a `N` odwołuje się do przestrzeni nazw, a `N` zawiera zagnieżdżoną przestrzeń nazw o nazwie `I`, a następnie *namespace_or_type_name* odwołuje się do tej zagnieżdżonej przestrzeni nazw.
    * W przeciwnym razie, jeśli `N` odwołuje się do przestrzeni nazw, a `N` zawiera dostępny typ o nazwie @ no__t-2 i `K` @ no__t-4type parametry, a następnie *namespace_or_type_name* odwołuje się do tego typu skonstruowanego za pomocą podanych argumentów typu.
    * W przeciwnym razie, jeśli `N` odwołuje się do klasy lub typu struktury, a `N` lub dowolnej z jej klas podstawowych zawiera zagnieżdżony dostępny typ o nazwie @ no__t-2 i `K` @ no__t-4type parametry, a następnie *namespace_or_type_name* odwołuje się do Ten typ skonstruowany przy użyciu podanych argumentów typu. Jeśli jest więcej niż jeden taki typ, zostanie wybrany typ zadeklarowany w ramach bardziej pochodnego typu. Należy pamiętać, że jeśli znaczenie `N.I` jest określane jako część rozpoznawania specyfikacji klasy bazowej `N`, to bezpośrednia klasa bazowa `N` jest uznawana za obiekt ([klasy bazowe](classes.md#base-classes)).
    * W przeciwnym razie `N.I` jest nieprawidłowym *namespace_or_type_name*i wystąpi błąd w czasie kompilacji.

*Namespace_or_type_name* może odwoływać się do klasy statycznej ([klasy static](classes.md#static-classes)) tylko wtedy, gdy

*  *Namespace_or_type_name* jest `T` w *namespace_or_type_name* formularza `T.I`, lub
*  *Namespace_or_type_name* jest `T` w *typeof_expression* ([Argument List](expressions.md#argument-lists)1) formularza `typeof(T)`.

### <a name="fully-qualified-names"></a>W pełni kwalifikowane nazwy

Każda przestrzeń nazw i typ mają w ***pełni kwalifikowaną nazwę***, która jednoznacznie identyfikuje przestrzeń nazw lub typ między wszystkimi innymi. W pełni kwalifikowana nazwa przestrzeni nazw lub typu `N` jest określana w następujący sposób:

*  Jeśli `N` należy do globalnej przestrzeni nazw, jego w pełni kwalifikowana nazwa jest `N`.
*  W przeciwnym razie jego w pełni kwalifikowana nazwa jest `S.N`, gdzie `S` to w pełni kwalifikowana nazwa przestrzeni nazw lub typu, w którym zadeklarowano `N`.

Innymi słowy, w pełni kwalifikowana nazwa `N` jest pełną ścieżką hierarchiczną identyfikatorów, które prowadzą do `N`, rozpoczynając od globalnej przestrzeni nazw. Ponieważ każdy element członkowski przestrzeni nazw lub typu musi mieć unikatową nazwę, następuje, że w pełni kwalifikowana nazwa przestrzeni nazw lub typu jest zawsze unikatowa.

W poniższym przykładzie przedstawiono kilka nazw i deklaracji typów wraz z ich skojarzonymi w pełni kwalifikowanymi nazwami.
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

C#Program wykorzystuje automatyczne zarządzanie pamięcią, dzięki czemu deweloperzy mogą bezpłatnie przydzielać i zwalniać pamięć zajmowaną przez obiekty. Automatyczne zasady zarządzania pamięcią są implementowane przez ***Moduł wyrzucania elementów bezużytecznych***. Cykl życia zarządzania pamięcią obiektu jest następujący:

1. Po utworzeniu obiektu zostanie do niego przydzielono pamięć, Konstruktor zostanie uruchomiony, a obiekt jest uznawany za aktywny.
2. Jeśli obiektu lub dowolnej jego części nie można uzyskać dostępu do żadnego możliwej kontynuacji wykonywania, z wyjątkiem uruchamiania destruktorów, obiekt jest uważany za nieużywany i będzie uprawniony do zniszczenia. C# Kompilator i moduł wyrzucania elementów bezużytecznych mogą analizować kod, aby określić, które odwołania do obiektu mogą być używane w przyszłości. Na przykład jeśli zmienna lokalna, która znajduje się w zakresie, jest jedynym istniejącym odwołaniem do obiektu, ale ta zmienna lokalna nigdy nie jest określana w żadnej możliwej kontynuacji wykonywania z bieżącego punktu wykonywania w procedurze, Moduł wyrzucania elementów bezużytecznych może (ale nie) wymagane do) traktuje obiekt jako nieużywany.
3. Gdy obiekt kwalifikuje się do zniszczenia, w przypadku, gdy nie zostanie określony później destruktor ([destruktory](classes.md#destructors)) (jeśli istnieje) dla obiektu jest uruchamiany. W normalnych warunkach destruktor dla obiektu jest uruchamiany tylko raz, ale interfejsy API specyficzne dla implementacji mogą zezwalać na przesłanianie tego zachowania.
4. Po uruchomieniu destruktora obiektu, jeśli ten obiekt lub jakakolwiek jego część nie są dostępne w żadnej możliwej kontynuacji wykonywania, w tym uruchamiania destruktorów, obiekt jest uznawany za niedostępny i obiekt kwalifikuje się do kolekcji.
5. Na koniec po pewnym czasie po przypisaniu obiektu do kolekcji moduł wyrzucania elementów bezużytecznych zwolni pamięć skojarzoną z tym obiektem.

Moduł wyrzucania elementów bezużytecznych przechowuje informacje o użyciu obiektów i używa tych informacji do podejmowania decyzji dotyczących zarządzania pamięcią, takich jak miejsce w pamięci do lokalizowania nowo utworzonego obiektu, kiedy przemieszczenie obiektu, gdy obiekt nie jest już używany lub niedostępny.

Podobnie jak w przypadku innych języków, które zakładają istnienie C# wyrzucania elementów bezużytecznych, zaprojektowano tak, aby moduł zbierający elementy bezużyteczne mógł zaimplementować szeroką gamę zasad zarządzania pamięcią Na przykład program C# nie wymaga uruchomienia destruktorów lub obiektów, które są zbierane zaraz po ich zakwalifikowaniu lub że destruktory są uruchamiane w określonej kolejności lub w dowolnym wątku.

Zachowanie modułu wyrzucania elementów bezużytecznych może być kontrolowane w pewnym stopniu za pomocą metod statycznych klasy `System.GC`. Ta klasa może służyć do żądania kolekcji, destruktorów, które mają być uruchamiane (lub nie są uruchamiane) i tak dalej.

Ponieważ moduł wyrzucania elementów bezużytecznych jest dozwolony szerokiej szerokości geograficznej podczas wybierania obiektów i destruktorów, implementacja zgodna może generować dane wyjściowe, które różnią się od pokazanego w poniższym kodzie. Program
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
tworzy wystąpienie klasy `A` i wystąpienie klasy `B`. Obiekty te stają się kwalifikować do wyrzucania elementów bezużytecznych, gdy zmienna `b` ma przypisaną wartość `null`, ponieważ po tym czasie nie jest możliwe uzyskanie dostępu do nich przez każdy kod pisany przez użytkownika. Dane wyjściowe mogą być albo

```console
Destruct instance of A
Destruct instance of B
```
lub
```console
Destruct instance of B
Destruct instance of A
```
Ponieważ język nie nakłada żadnych ograniczeń w kolejności, w której obiekty są odbierane jako elementy bezużyteczne.

W delikatnych przypadkach rozróżnienie między elementami "kwalifikujące się do zniszczenia" i "kwalifikujące się do kolekcji" może być ważne. Na przykład
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

Jeśli moduł wyrzucania elementów bezużytecznych wybierze uruchomienie destruktora `A` przed destruktorem `B`, dane wyjściowe tego programu mogą być następujące:
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

Należy zauważyć, że chociaż wystąpienie `A` nie jest używane i destruktor `A` było uruchomione, nadal jest możliwe dla metod `A` (w tym przypadku `F`), które mają być wywoływane z innego destruktora. Należy również pamiętać, że uruchomienie destruktora może spowodować, że obiekt będzie można użyć ponownie z programu linii głównej. W takim przypadku uruchomienie destruktora `B` spowodowało wystąpienie `A`, które wcześniej nie było używane do uzyskania dostępu z odwołania na żywo `Test.RefA`. Po wywołaniu `WaitForPendingFinalizers` wystąpienie `B` kwalifikuje się do kolekcji, ale wystąpienie `A` nie jest z powodu odwołania `Test.RefA`.

Aby uniknąć nieporozumień i nieoczekiwanego zachowania, zwykle dobrym pomysłem jest, aby destruktory wykonywały tylko oczyszczanie danych przechowywanych w własnych polach obiektu, a nie do wykonywania żadnych akcji na obiektach, do których istnieją odwołania lub pola statyczne.

Alternatywą dla korzystania z destruktorów jest umożliwienie klasy implementacji interfejsu `System.IDisposable`. Umożliwia to klientowi obiektu określenie czasu zwolnienia zasobów obiektu, zazwyczaj przez uzyskanie dostępu do obiektu jako zasobu w instrukcji `using` ([instrukcja using](statements.md#the-using-statement)).

## <a name="execution-order"></a>Kolejność wykonywania

Wykonanie C# programu jest wykonywane w taki sposób, że efekty uboczne każdego wątku wykonywanego są zachowywane w kluczowych punktach wykonania. ***Efekt uboczny*** jest definiowany jako Odczyt lub zapis pola nietrwałego, zapis do zmiennej nietrwałej, zapis do zasobu zewnętrznego i wyrzucanie wyjątku. Krytyczne punkty wykonywania, w których kolejność tych efektów ubocznych należy zachować, są odwołaniami do pól nietrwałych ([nietrwałe pola](classes.md#volatile-fields)), `lock` instrukcji ([instrukcja Lock](statements.md#the-lock-statement)) oraz tworzenia i kończenia wątku. Środowisko wykonawcze jest bezpłatne, aby zmienić kolejność wykonywania C# programu, zgodnie z następującymi ograniczeniami:

*  Zależność danych jest zachowywana w wątku wykonywania. Oznacza to, że wartość każdej zmiennej jest obliczana tak, jakby wszystkie instrukcje w wątku były wykonywane w oryginalnej kolejności programu.
*  Reguły porządkowania inicjalizacji są zachowywane ([Inicjalizacja pola](classes.md#field-initialization) i [inicjatory zmiennych](classes.md#variable-initializers)).
*  Porządkowanie efektów ubocznych jest zachowywane w odniesieniu do nietrwałych odczytów i zapisów ([pola nietrwałe](classes.md#volatile-fields)). Ponadto środowisko wykonawcze nie musi obliczać części wyrażenia, jeśli może wywnioskować, że wartość tego wyrażenia nie jest używana i że nie są generowane żadne niepotrzebne efekty uboczne (w tym wszelkie powodowane przez wywołanie metody lub uzyskanie dostępu do pola nietrwałego). Po przerwaniu wykonywania programu przez zdarzenie asynchroniczne (takie jak wyjątek zgłoszony przez inny wątek) nie jest gwarantowane, że zauważalne efekty uboczne są widoczne w oryginalnym porządku programu.
