---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: e134bb7058e9848120b93b345f96d6ac0cb8c815
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2020
ms.locfileid: "71704085"
---
# <a name="expressions"></a>Wyrażenia

Wyrażenie jest sekwencją operatorów i argumentów operacji. Ten rozdział definiuje składnię, kolejność oceny argumentów operacji i operatorów oraz znaczenie wyrażeń.

## <a name="expression-classifications"></a>Klasyfikacje wyrażeń

Wyrażenie jest klasyfikowane jako jedno z następujących:

*  Wartość. Każda wartość ma skojarzony typ.
*  Zmienna. Każda zmienna ma skojarzony typ, a mianowicie zadeklarowany typ zmiennej.
*  Przestrzeń nazw. Wyrażenie z tą klasyfikacją może występować tylko jako lewa strona *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)). W każdym innym kontekście wyrażenie sklasyfikowane jako przestrzeń nazw powoduje błąd w czasie kompilacji.
*  Typ. Wyrażenie z tą klasyfikacją może występować tylko jako lewa strona *member_access* ([dostęp do składowej](expressions.md#member-access)) lub jako operand dla operatora `as` ([operator as](expressions.md#the-as-operator)), operator `is` ([operator is](expressions.md#the-is-operator)) lub operator `typeof` ([operator typeof](expressions.md#the-typeof-operator)). W każdym innym kontekście wyrażenie sklasyfikowane jako typ powoduje błąd w czasie kompilacji.
*  Grupa metod, która jest zestawem przeciążonych metod wynikających z odnośnika elementu członkowskiego ([wyszukiwanie elementu członkowskiego](expressions.md#member-lookup)). Grupa metod może mieć skojarzone wyrażenie wystąpienia i listę powiązanych argumentów typu. Gdy wywoływana jest metoda wystąpienia, wynik oceny wyrażenia wystąpienia zmieni się na wystąpienie reprezentowane przez `this` ([ten dostęp](expressions.md#this-access)). Grupa metod jest dozwolona w *invocation_expression* ([wyrażenia wywołania](expressions.md#invocation-expressions)), *delegate_creation_expression* ([wyrażenia tworzenia delegatów](expressions.md#delegate-creation-expressions)) i jako lewa strona operatora is i może być niejawnie konwertowana na zgodny typ delegata ([konwersje grup metod](conversions.md#method-group-conversions)). W każdym innym kontekście wyrażenie sklasyfikowane jako grupa metod powoduje błąd w czasie kompilacji.
*  Literał o wartości null. Wyrażenie z tą klasyfikacją może być niejawnie konwertowane na typ referencyjny lub typ dopuszczający wartość null.
*  Funkcja anonimowa. Wyrażenie z tą klasyfikacją może być niejawnie konwertowane na zgodny typ delegata lub typ drzewa wyrażenia.
*  Dostęp do właściwości. Każdy dostęp do właściwości ma skojarzony typ, a nie typ właściwości. Ponadto dostęp do właściwości może mieć skojarzone wyrażenie wystąpienia. Gdy wywoływana jest metoda dostępu (`get` lub `set` bloku) właściwości wystąpienia, wynik oceny wyrażenia wystąpienia zostaje przywoływany przez `this` ([dostęp](expressions.md#this-access)).
*  Dostęp do zdarzenia. Każdy dostęp do zdarzenia ma skojarzony typ, a nie typ zdarzenia. Ponadto dostęp do zdarzeń może mieć skojarzone wyrażenie wystąpienia. Dostęp do zdarzenia może być wyświetlany jako lewy argument operacji dla operatorów `+=` i `-=` ([przypisanie zdarzenia](expressions.md#event-assignment)). W każdym innym kontekście wyrażenie sklasyfikowane jako dostęp do zdarzenia powoduje błąd w czasie kompilacji.
*  Dostęp indeksatora. Każdy dostęp indeksatora ma skojarzony typ, a mianowicie typ elementu indeksatora. Ponadto dostęp indeksatora ma skojarzone wyrażenie wystąpienia i skojarzoną listę argumentów. Gdy wywoływana jest metoda dostępu (`get` lub `set` bloku), wynik oceny wyrażenia wystąpienia zmieni się na wystąpienie reprezentowane przez `this` ([ten dostęp](expressions.md#this-access)), a wynik oceny listy argumentów jest listą parametrów wywołania ().
*  Nic nie robić Dzieje się tak, gdy wyrażenie jest wywołaniem metody z typem zwracanym `void`. Wyrażenie sklasyfikowane jako Nothing jest prawidłowe tylko w kontekście *statement_expression* ([instrukcji wyrażeń](statements.md#expression-statements)).

Końcowy wynik wyrażenia nigdy nie jest obszarem nazw, typem, grupą metod lub dostępem do zdarzeń. Zamiast tego, jak wspomniano powyżej, te kategorie wyrażeń są konstrukcjami pośrednimi, które są dozwolone tylko w określonych kontekstach.

Dostęp do właściwości lub dostęp do indeksatora jest zawsze ponownie klasyfikowany jako wartość przez wykonanie wywołania *metody dostępu get* lub *set metody*dostępu. Określony akcesor jest określany przez kontekst dostępu właściwości lub indeksatora: Jeśli dostęp jest elementem docelowym przypisania, *metoda dostępu set* jest wywoływana, aby przypisać nową wartość ([przypisanie proste](expressions.md#simple-assignment)). W przeciwnym razie *metoda dostępu get* jest wywoływana w celu uzyskania bieżącej wartości ([wartości wyrażeń](expressions.md#values-of-expressions)).

### <a name="values-of-expressions"></a>Wartości wyrażeń

Większość konstrukcji obejmujących wyrażenie ostatecznie wymaga wyrażenia do określenia ***wartości***. W takich przypadkach, jeśli wyrażenie rzeczywiste wskazuje przestrzeń nazw, typ, grupę metod lub wartość Nothing, wystąpi błąd w czasie kompilacji. Jeśli jednak wyrażenie oznacza dostęp do właściwości, dostęp indeksatora lub zmienną, wartość właściwości, indeksatora lub zmiennej jest niejawnie zastępowana:

*  Wartość zmiennej jest po prostu wartością przechowywaną obecnie w lokalizacji magazynu identyfikowanej przez zmienną. Zmienna musi być traktowana jako czasowo przypisana ([przypisanie](variables.md#definite-assignment)), zanim będzie można uzyskać jej wartość lub w przeciwnym razie wystąpi błąd w czasie kompilacji.
*  Wartość wyrażenia dostępu do właściwości jest uzyskiwana przez wywołanie *metody dostępu get* właściwości. Jeśli właściwość nie ma *metody dostępu get*, wystąpi błąd w czasie kompilacji. W przeciwnym razie jest wykonywane wywołanie elementu członkowskiego ([Sprawdzanie w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a wynik wywołania będzie wartością wyrażenia dostępu do właściwości.
*  Wartość wyrażenia dostępu indeksatora jest uzyskiwana przez wywołanie *metody dostępu get* indeksatora. Jeśli indeksator nie ma *metody dostępu get*, wystąpi błąd w czasie kompilacji. W przeciwnym razie wywołanie elementu członkowskiego funkcji ([Sprawdzanie w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) jest wykonywane z listą argumentów skojarzoną z wyrażeniem dostępu indeksatora, a wynik wywołania będzie wartością wyrażenia dostępu indeksatora.

## <a name="static-and-dynamic-binding"></a>Statyczne i dynamiczne powiązanie

Proces określania znaczenia operacji na podstawie typu lub wartości wyrażeń elementów (argumentów, operandów, odbiorników) jest często określany jako ***powiązanie***. Na przykład znaczenie wywołania metody jest określane na podstawie typu odbiorcy i argumentów. Znaczenie operatora jest określane na podstawie typu jego operandów.

W C# znaczeniu operacji jest zwykle określane w czasie kompilacji na podstawie typu czasu kompilowania jego wyrażeń składowych. Podobnie, jeśli wyrażenie zawiera błąd, błąd jest wykrywany i raportowany przez kompilator. To podejście jest znane jako ***powiązanie statyczne***.

Jeśli jednak wyrażenie jest wyrażeniem dynamicznym (tj. ma typ `dynamic`), oznacza to, że wszelkie powiązania, które uczestniczą w systemie, powinny być oparte na typie czasu wykonywania (tj. rzeczywisty typ obiektu, który jest w czasie wykonywania), a nie typ, który jest w czasie kompilacji. W związku z tym powiązanie takiej operacji jest odroczone do czasu, w którym operacja ma zostać wykonana podczas uruchamiania programu. Nazywa się to ***dynamicznym wiązaniem***.

Gdy operacja jest powiązana dynamicznie, w kompilatorze nie jest wykonywane żadne sprawdzenie. Zamiast tego, Jeśli powiązanie czasu wykonywania nie powiedzie się, błędy są raportowane jako wyjątki w czasie wykonywania.

Następujące operacje w programie C# podlegają powiązaniu:

*  Dostęp do elementu członkowskiego: `e.M`
*  Wywołanie metody: `e.M(e1, ..., eN)`
*  Delegowanie wywołania:`e(e1, ..., eN)`
*  Dostęp do elementów: `e[e1, ..., eN]`
*  Tworzenie obiektu: `new C(e1, ..., eN)`
*  Przeciążone operatory jednoargumentowe: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`
*  Przeciążone operatory binarne: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`
*  Operatory przypisania: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`
*  Konwersje niejawne i jawne

Gdy nie są używane żadne wyrażenia dynamiczne C# , domyślnie jest to powiązanie statyczne, co oznacza, że typy wyrażeń elementów w czasie kompilacji są stosowane w procesie wyboru. Jeśli jednak jeden z wyrażeń składowych w wymienionych powyżej operacjach jest wyrażeniem dynamicznym, operacja jest zamiast tego dynamicznie powiązana.

### <a name="binding-time"></a>Czas powiązania

Statyczne powiązanie odbywa się w czasie kompilacji, natomiast dynamiczne wiązanie odbywa się w czasie wykonywania. W poniższych sekcjach termin ***czas wiązania*** odnosi się do czasu kompilacji lub czasu wykonywania w zależności od tego, kiedy powiązanie ma miejsce.

Poniższy przykład ilustruje koncepcje statycznego i dynamicznego powiązania oraz czas powiązania:
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

Pierwsze dwa wywołania są statycznie powiązane: Przeciążenie `Console.WriteLine` jest wybierane w oparciu o typ czasu kompilacji ich argumentu. W rezultacie czas powiązania to czas kompilacji.

Trzecie wywołanie jest powiązane dynamicznie: Przeciążenie `Console.WriteLine` jest wybierane na podstawie typu czasu wykonywania tego argumentu. Dzieje się tak, ponieważ argument jest wyrażeniem dynamicznym — typem czasu kompilacji jest `dynamic`. W rezultacie czas powiązania dla trzeciego wywołania to czas wykonywania.

### <a name="dynamic-binding"></a>Wiązanie dynamiczne

Celem powiązania dynamicznego jest umożliwienie C# programom współdziałania z ***obiektami dynamicznymi***, np. obiektów, które nie są zgodne z normalnymi C# regułami typu System. Obiekty dynamiczne mogą być obiektami z innych języków programowania z różnymi typami systemów lub mogą być obiektami, które są programowo instalatorem do implementowania własnej semantyki powiązań dla różnych operacji.

Mechanizm, za pomocą którego obiekt dynamiczny implementuje swoją własną semantykę, ma zdefiniowaną implementację. Określony interfejs — ponownie wdrożony — jest implementowany przez obiekty dynamiczne, aby sygnalizować czas C# wykonywania, który ma specjalną semantykę. Z tego względu, gdy operacje na obiekcie dynamicznym są dynamicznie powiązane, ich własnej semantyki powiązania, a C# nie z określonymi w tym dokumencie, przejmowanie.

Chociaż celem powiązania dynamicznego jest umożliwienie współdziałania z obiektami dynamicznymi, C# zezwala na dynamiczne wiązanie dla wszystkich obiektów, niezależnie od tego, czy są one dynamiczne, czy nie. Pozwala to na bezproblemowe integrację obiektów dynamicznych, ponieważ wyniki operacji na nich mogą nie być obiektami dynamicznymi, ale nadal są typu nieznanego dla programisty w czasie kompilacji. Ponadto dynamiczne powiązanie może pomóc wyeliminować podatny na błędy kod oparty na odbiciu nawet wtedy, gdy żadne obiekty nie są obiektami dynamicznymi.

W poniższych sekcjach opisano dla każdej konstrukcji w języku dokładnie, gdy jest stosowane dynamiczne wiązanie, jakie jest sprawdzanie czasu kompilacji — jeśli jest stosowana, a także wynik klasyfikacji wyników i wyrażeń w czasie kompilacji.

### <a name="types-of-constituent-expressions"></a>Typy wyrażeń składowych

Gdy operacja jest statyczna, typ wyrażenia elementu (np. odbiorca, argument, indeks lub operand) jest zawsze uznawany za typ czasu kompilacji tego wyrażenia.

Gdy operacja jest powiązana dynamicznie, typ wyrażenia elementu składowego jest określany na różne sposoby w zależności od typu czasu kompilacji w wyrażeniu składnika:

*  Wyrażenie elementu `dynamic` typu czasu kompilacji jest uznawane za posiadające typ wartości rzeczywistej, do której wyrażenie oblicza w czasie wykonywania
*  Wyrażenie elementu składowego, którego typem czasu kompilacji jest parametr typu, jest uznawany za posiadający typ, z którym jest powiązany parametr typu w czasie wykonywania
*  W przeciwnym razie wyrażenie składowe jest uznawane za mające typ czasu kompilacji.

## <a name="operators"></a>Operatory

Wyrażenia są zbudowane z ***argumentów operacji*** i ***operatorów***. Operatory wyrażenia wskazują operacje do zastosowania dla operandów. Przykłady operatorów to `+`, `-`, `*`, `/`i `new`. Przykładami operandów są literały, pola, zmienne lokalne i wyrażenia.

Istnieją trzy rodzaje operatorów:

*  Operatory jednoargumentowe. Operatory jednoargumentowe przyjmują jeden operand i używają obu prefiksów (takich jak `--x`) lub notacji przyrostkowej (na przykład `x++`).
*  Operatory binarne. Operatory binarne przyjmują dwa operandy i wszystkie używają notacji wrostkowe (na przykład `x + y`).
*  Operator Trzyelementowy. Tylko jeden operator Trzyelementowy, `?:`, istnieje; przyjmuje trzy operandy i używa notacji wrostkowe (`c ? x : y`).

Kolejność obliczania operatorów w wyrażeniu jest określana przez ***pierwszeństwo*** i ***łączność*** operatorów ([pierwszeństwo operatorów i łączność](expressions.md#operator-precedence-and-associativity)).

Argumenty operacji w wyrażeniu są oceniane od lewej do prawej. Na przykład w `F(i) + G(i++) * H(i)`Metoda `F` jest wywoływana przy użyciu starej wartości `i`, a następnie Metoda `G` jest wywoływana z starą wartością `i`, a wreszcie Metoda `H` jest wywoływana z nową wartością `i`. Jest to niezależne od i niepowiązane z pierwszeństwem operatorów.

Niektóre operatory mogą być ***przeciążone***. Przeciążanie operatora zezwala na określenie implementacji operatora zdefiniowanego przez użytkownika dla operacji, w których jeden lub oba operandy są typu klasy lub struktury zdefiniowanej przez użytkownika ([przeciążanie operatora](expressions.md#operator-overloading)).

### <a name="operator-precedence-and-associativity"></a>Pierwszeństwo operatorów i łączność

Gdy wyrażenie zawiera wiele operatorów ***pierwszeństwo*** operatorów określa kolejność, w jakiej są oceniane poszczególne operatory. Na przykład wyrażenie `x + y * z` jest oceniane jako `x + (y * z)`, ponieważ operator `*` ma wyższy priorytet niż binarny operator `+`. Pierwszeństwo operatora jest ustanawiane przez definicję skojarzonej produkcji gramatyki. Na przykład *additive_expression* składa się z sekwencji *multiplicative_expression*s oddzielonej przez `+` lub `-` operatory, dzięki czemu operatory `+` i `-` mają niższy priorytet niż operatory `*`, `/`i `%`.

Poniższa tabela podsumowuje wszystkie operatory w kolejności od najwyższego do najniższego:

| __Sekcja__                                                                                   | __Kategoria__                | __Operatory__ | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [Wyrażenia podstawowe](expressions.md#primary-expressions)                                     | Podstawowa                     | `x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate` | 
| [Operatory jednoargumentowe](expressions.md#unary-operators)                                             | Jednoargumentowy                       | `+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x` | 
| [Operatory arytmetyczne](expressions.md#arithmetic-operators)                                   | Mnożeniowy              | `*`  `/`  `%` | 
| [Operatory arytmetyczne](expressions.md#arithmetic-operators)                                   | Dana                    | `+`  `-`      | 
| [Operatory przesunięcia](expressions.md#shift-operators)                                             | Shift                       | `<<`  `>>`    | 
| [Operatory relacyjne i testowe typu](expressions.md#relational-and-type-testing-operators) | Testowanie relacyjne i typu | `<`  `>`  `<=`  `>=`  `is`  `as` | 
| [Operatory relacyjne i testowe typu](expressions.md#relational-and-type-testing-operators) | Równość                    | `==`  `!=`    | 
| [Operatory logiczne](expressions.md#logical-operators)                                         | AND logiczne                 | `&`           | 
| [Operatory logiczne](expressions.md#logical-operators)                                         | XOR logiczne                 | `^`           | 
| [Operatory logiczne](expressions.md#logical-operators)                                         | OR logiczne                  | <code>&#124;</code>           |
| [Warunkowe operatory logiczne](expressions.md#conditional-logical-operators)                 | AND warunkowe             | `&&`          | 
| [Warunkowe operatory logiczne](expressions.md#conditional-logical-operators)                 | OR warunkowe              | <code>&#124;&#124;</code>          | 
| [Operator łączenia wartości null](expressions.md#the-null-coalescing-operator)                   | Łączenie wartości null             | `??`          | 
| [Operator warunkowy](expressions.md#conditional-operator)                                   | Warunkowy                 | `?:`          | 
| [Operatory przypisania](expressions.md#assignment-operators), [wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)  | Przypisanie i wyrażenie lambda | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

Gdy operand występuje między dwoma operatorami o takim samym priorytecie, łączność operatorów kontroluje kolejność wykonywania operacji:

*  Z wyjątkiem operatorów przypisania i operatora łączenia wartości null, wszystkie operatory binarne są z ***lewej strony skojarzenia***, co oznacza, że operacje są wykonywane od lewej do prawej. Na przykład `x + y + z` jest oceniane jako `(x + y) + z`.
*  Operatory przypisania, operator łączenia wartości null i operator warunkowy (`?:`) są ***z prawej strony skojarzenia***, co oznacza, że operacje są wykonywane od prawej do lewej. Na przykład `x = y = z` jest oceniane jako `x = (y = z)`.

Pierwszeństwo i asocjacyjność mogą być kontrolowane za pomocą nawiasów. Na przykład `x + y * z` najpierw mnoży `y` przez `z` , a następnie dodaje wynik do `x`, ale `(x + y) * z` najpierw dodaje `x` i `y` i następnie wynik mnoży przez `z`.

### <a name="operator-overloading"></a>Przeładowanie operatora

Wszystkie operatory jednoargumentowe i binarne mają wstępnie zdefiniowane implementacje, które są automatycznie dostępne w dowolnym wyrażeniu. Oprócz wstępnie zdefiniowanych implementacji implementacje zdefiniowane przez użytkownika mogą być wprowadzane przez uwzględnienie deklaracji `operator` w klasach i strukturach ([Operatory](classes.md#operators)). Implementacje operatorów zdefiniowane przez użytkownika zawsze mają pierwszeństwo przed wstępnie zdefiniowanymi implementacjami operatorów: tylko wtedy, gdy nie istnieją żadne odpowiednie implementacje operatorów zdefiniowane przez użytkownika, zostaną uznane wstępnie zdefiniowane implementacje operatorów, zgodnie z opisem w temacie [rozpoznawanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution) i [rozpoznawanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution).

***Operatory jednoargumentowe*** z możliwością przeciążenia:
```csharp
+   -   !   ~   ++   --   true   false
```

Mimo że `true` i `false` nie są używane jawnie w wyrażeniach (i dlatego nie są uwzględnione w tabeli pierwszeństwa w [priorytecie operatora i łączność](expressions.md#operator-precedence-and-associativity)), są uznawane za operatory, ponieważ są wywoływane w kilku kontekstach wyrażeń: wyrażenia logiczne ([wyrażenia logiczne](expressions.md#boolean-expressions)) i wyrażenia obejmujące warunkowe ([operator warunkowy](expressions.md#conditional-operator)) i warunkowe operatory logiczne ([warunkowe operatory logiczne](expressions.md#conditional-logical-operators)).

***Operatory binarne*** z możliwością przeciążenia są następujące:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Tylko operatory wymienione powyżej mogą być przeciążone. W szczególności nie jest możliwe przeciążanie dostępu do składowych, wywoływanie metody lub `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`i `is`.

Gdy operator binarny jest przeciążony, odpowiedni operator przypisania, jeśli istnieje, również jest niejawnie przeciążony. Na przykład Przeciążenie operatora `*` jest również przeciążeniem `*=`operatora. Opisano to dokładniej w [przypisaniu złożonym](expressions.md#compound-assignment). Należy zauważyć, że sam operator przypisania (`=`) nie może być przeciążony. Przypisanie zawsze wykonuje prostą kopię wartości w zmiennej.

Operacje Cast, takie jak `(T)x`, są przeciążone przez dostarczanie zdefiniowanych przez użytkownika konwersji ([konwersje zdefiniowane przez użytkownika](conversions.md#user-defined-conversions)).

Dostęp do elementu, taki jak `a[x]`, nie jest uważany za operatora z możliwością przeciążenia. Zamiast tego, zdefiniowane przez użytkownika indeksowanie jest obsługiwane za poorednictwem indeksatorów ([indeksatorów](classes.md#indexers)).

W wyrażeniach operatory są wywoływane przy użyciu notacji operatora, a w deklaracjach operatory są odwoływane przy użyciu notacji funkcjonalnej. W poniższej tabeli przedstawiono relacje między operatorami i notacjami funkcjonalnymi dla operatorów jednoargumentowych i binarnych. W pierwszym wpisie *op* oznacza każdy jednoargumentowy operator prefiksu z możliwością przeciążenia. W drugim wpisie *op* oznacza jednoargumentowy przyrost `++` i operatory `--`. W trzecim wpisie *op* oznacza dowolny operator binarny z możliwością przeciążenia.


| __Notacja operatora__ | __Notacja funkcjonalna__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

Deklaracje operatora zdefiniowane przez użytkownika zawsze wymagają, aby co najmniej jeden z parametrów był klasą lub typem struktury, który zawiera deklarację operatora. Dlatego nie jest możliwe, aby operator zdefiniowany przez użytkownika miał taki sam podpis jak wstępnie zdefiniowany operator.

Deklaracje operatora zdefiniowane przez użytkownika nie mogą modyfikować składni, pierwszeństwa ani łącznośći operatora. Na przykład operator `/` jest zawsze operatorem binarnym, zawsze ma poziom pierwszeństwa określony w [priorytecie operatora i łączność](expressions.md#operator-precedence-and-associativity), a zawsze jest w pełni skojarzony z lewej strony.

Chociaż jest możliwe, aby operator zdefiniowany przez użytkownika wykonywał jakiekolwiek obliczenia, należy zdecydowanie odradzać implementacje, które generują wyniki inne niż te, które są w sposób intuicyjny. Na przykład implementacja `operator ==` powinna porównać dwa operandy dla równości i zwrócić odpowiedni `bool`y wynik.

Opisy poszczególnych operatorów w [wyrażeniach podstawowych](expressions.md#primary-expressions) za pomocą [warunkowych operatorów logicznych](expressions.md#conditional-logical-operators) określają wstępnie zdefiniowane implementacje operatorów i wszelkie dodatkowe reguły, które mają zastosowanie do każdego operatora. Opisy korzystają z warunków ***przeciążania przeciążenia operatora jednoargumentowego***, ***rozpoznawania przeciążania operatora binarnego***i ***promocji liczbowych***, definicji znalezionych w poniższych sekcjach.

### <a name="unary-operator-overload-resolution"></a>Rozpoznanie przeciążenia operatora jednoargumentowego

Operacja `op x` lub `x op`, gdzie `op` jest operatorem jednoargumentowym z możliwością przeciążenia, a `x` jest wyrażeniem typu `X`, jest przetwarzana w następujący sposób:

*  Zestaw operatorów zdefiniowanych przez użytkownika w `X` dla operacji `operator op(x)` jest określany przy użyciu reguł [kandydujących operatorów zdefiniowanych przez użytkownika](expressions.md#candidate-user-defined-operators).
*  Jeśli zestaw operatorów zdefiniowanych przez użytkownika nie jest pusty, zostanie to zestaw operatorów kandydujących dla operacji. W przeciwnym razie wstępnie zdefiniowane jednoargumentowe implementacje `operator op`, w tym ich zniesione formularze, staną się zestawem operatorów kandydujących dla operacji. Wstępnie zdefiniowane implementacje danego operatora są określone w opisie operatora ([wyrażenia podstawowe](expressions.md#primary-expressions) i [operatory jednoargumentowe](expressions.md#unary-operators)).
*  Reguły [rozpoznawania przeciążenia są stosowane](expressions.md#overload-resolution) do zestawu operatorów kandydujących w celu wybrania najlepszego operatora w odniesieniu do listy argumentów `(x)`, a ten operator będzie wynikiem procesu rozpoznawania przeciążenia. Jeśli nie można wybrać pojedynczego najlepszego operatora rozpoznawania przeciążenia, wystąpi błąd w czasie powiązania.

### <a name="binary-operator-overload-resolution"></a>Rozpoznawanie przeciążenia operatora binarnego

Operacja `x op y`, gdzie `op` jest operatorem binarnym przeciążania, `x` jest wyrażeniem typu `X`, a `y` jest wyrażeniem typu `Y`, jest przetwarzana w następujący sposób:

*  Zestaw operatorów zdefiniowanych przez użytkownika kandydujących zapewnianych przez `X` i `Y` dla operacji `operator op(x,y)` został określony. Zestaw składa się z Unii operatorów kandydujących dostarczanych przez `X` i operatorów kandydujących dostarczonych przez `Y`, z których każdy określa użycie reguł [kandydujących operatorów zdefiniowanych przez użytkownika](expressions.md#candidate-user-defined-operators). Jeśli `X` i `Y` są tego samego typu lub jeśli `X` i `Y` pochodzą od wspólnego typu podstawowego, współdzielone operatory kandydujące występują tylko w połączonym zestawie.
*  Jeśli zestaw operatorów zdefiniowanych przez użytkownika nie jest pusty, zostanie to zestaw operatorów kandydujących dla operacji. W przeciwnym razie wstępnie zdefiniowane implementacje `operator op` binarnych, w tym ich zniesione formularze, staną się zestawem operatorów kandydujących dla operacji. Wstępnie zdefiniowane implementacje danego operatora są określone w opisie operatora ([Operatory arytmetyczne](expressions.md#arithmetic-operators) za pomocą [warunkowych operatorów logicznych](expressions.md#conditional-logical-operators)). Dla wstępnie zdefiniowanych typów wyliczeniowych i delegatów jedynymi uznanymi operatorami są te zdefiniowane przez typ wyliczeniowy lub delegata, który jest typem czasu powiązania jednego z operandów.
*  Reguły [rozpoznawania przeciążenia są stosowane](expressions.md#overload-resolution) do zestawu operatorów kandydujących w celu wybrania najlepszego operatora w odniesieniu do listy argumentów `(x,y)`, a ten operator będzie wynikiem procesu rozpoznawania przeciążenia. Jeśli nie można wybrać pojedynczego najlepszego operatora rozpoznawania przeciążenia, wystąpi błąd w czasie powiązania.

### <a name="candidate-user-defined-operators"></a>Kandydujące Operatory zdefiniowane przez użytkownika

Uwzględniając typ `T` i `operator op(A)`operacji, gdzie `op` jest operatorem przeciążania, a `A` jest listą argumentów, zestaw operatorów zdefiniowanych przez użytkownika w `T` dla `operator op(A)` jest określony w następujący sposób:

*  Określ typ `T0`. Jeśli `T` jest typem dopuszczającym wartość null, `T0` jest jego typem podstawowym, w przeciwnym razie `T0` jest równa `T`.
*  W przypadku wszystkich `operator op` deklaracji w `T0` i wszystkich podnoszenia formularzy takich operatorów, jeśli ma zastosowanie co najmniej jednego operatora ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)) w odniesieniu do listy argumentów `A`, a następnie zestaw operatorów kandydujących składa się ze wszystkich odpowiednich operatorów w `T0`.
*  W przeciwnym razie, jeśli `T0` jest `object`, zestaw operatorów kandydujących jest pusty.
*  W przeciwnym razie zestaw operatorów kandydujących udostępniany przez `T0` jest zestawem operatorów kandydujących dostarczanych przez bezpośrednią klasę bazową `T0`lub obowiązującą klasą bazową `T0` Jeśli `T0` jest parametrem typu.

### <a name="numeric-promotions"></a>Promocje numeryczne

Promocja liczbowa składa się z automatycznego wykonywania niektórych niejawnych konwersji argumentów operacji dla wstępnie zdefiniowanych jednoargumentowych i binarnych operatorów liczbowych. Promocja numeryczna nie jest odrębnym mechanizmem, ale raczej efektem zastosowania rozpoznawania przeciążenia do wstępnie zdefiniowanych operatorów. Promocja liczbowa w specjalny sposób nie wpływa na ocenę operatorów zdefiniowanych przez użytkownika, chociaż Operatory zdefiniowane przez użytkownika mogą być implementowane w taki sposób, aby miały podobne skutki.

Przykładem promocji numerycznych jest rozważenie wstępnie zdefiniowanych implementacji operatora `*` binarnych:

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

Gdy reguły rozpoznawania przeciążenia ([rozwiązanie przeciążenia](expressions.md#overload-resolution)) są stosowane do tego zestawu operatorów, efektem jest wybranie pierwszego z operatorów, dla których istnieją niejawne konwersje z typów operandów. Na przykład dla operacji `b * s`, gdzie `b` jest `byte` i `s` jest `short`, rozpoznawanie przeciążenia wybiera `operator *(int,int)` jako najlepszy operator. W związku z tym efektem jest to, że `b` i `s` są konwertowane na `int`, a typ wyniku jest `int`. Podobnie w przypadku operacji `i * d`, gdzie `i` jest `int` i `d` jest `double`, rozpoznawanie przeciążenia wybiera `operator *(double,double)` jako najlepszy operator.

#### <a name="unary-numeric-promotions"></a>Jednoargumentowe promocje liczbowe

Jednoargumentowa promocja numeryczna występuje dla argumentów operacji wstępnie zdefiniowanych `+`, `-`i `~` operatory jednoargumentowe. Jednoargumentowa promocja liczbowa polega na przekonwertowaniu operandów typu `sbyte`, `byte`, `short`, `ushort`lub `char` do typu `int`. Ponadto dla jednoargumentowego operatora `-`, Jednoargumentowa promocja liczbowa konwertuje operandy typu `uint` do typu `long`.

#### <a name="binary-numeric-promotions"></a>Binarne promocje liczbowe

Binarne promocję liczbową występuje w przypadku operandów wstępnie zdefiniowanych `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, `<=` i operatorów binarnych. Binarne promocję liczbową niejawnie konwertuje oba operandy na wspólny typ, który, w przypadku operatorów nierelacyjnych, również staje się typem wyniku operacji. Binarne promocje liczbowe polega na zastosowaniu następujących reguł w kolejności, w jakiej są wyświetlane w tym miejscu:

*  Jeśli oba operandy są typu `decimal`, drugi operand jest konwertowany na typ `decimal`lub wystąpi błąd w czasie powiązania, jeśli inny operand jest typu `float` lub `double`.
*  W przeciwnym razie, jeśli jeden z operandów jest typu `double`, drugi operand jest konwertowany na typ `double`.
*  W przeciwnym razie, jeśli jeden z operandów jest typu `float`, drugi operand jest konwertowany na typ `float`.
*  W przeciwnym razie, jeśli jeden z operandów jest typu `ulong`, drugi operand jest konwertowany na typ `ulong`lub wystąpi błąd w czasie powiązania, jeśli inny operand jest typu `sbyte`, `short`, `int`lub `long`.
*  W przeciwnym razie, jeśli jeden z operandów jest typu `long`, drugi operand jest konwertowany na typ `long`.
*  W przeciwnym razie, jeśli jeden z operandów jest typu `uint`, a drugi operand jest typu `sbyte`, `short`lub `int`, oba operandy są konwertowane na typ `long`.
*  W przeciwnym razie, jeśli jeden z operandów jest typu `uint`, drugi operand jest konwertowany na typ `uint`.
*  W przeciwnym razie oba operandy są konwertowane na typ `int`.

Należy zauważyć, że pierwsza reguła nie zezwala na wszystkie operacje, które mieszają typ `decimal` z typami `double` i `float`. Reguła wynika z faktu, że nie istnieją niejawne konwersje między typem `decimal` i `double` i `float`.

Należy również zauważyć, że nie jest możliwe, aby operand był typu `ulong`, gdy inny operand jest podpisanym typem całkowitym. Przyczyną jest brak typu całkowitego, który może reprezentować pełny zakres `ulong`, a także z podpisanymi typami całkowitymi.

W obu powyższych przypadkach wyrażenie cast może służyć do jawnej konwersji jednego operandu na typ, który jest zgodny z drugim operandem.

W przykładzie
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
Wystąpił błąd w czasie powiązania, ponieważ nie można pomnożyć `decimal` przez `double`. Błąd został rozwiązany przez jawne przekonwertowanie drugiego operandu na `decimal`w następujący sposób:

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>Podniesione operatory

***Podnoszone operatory*** zezwalają wstępnie zdefiniowanym i zdefiniowanym przez użytkownika operatorom, którzy działają na typach wartości, które nie dopuszczają wartości null, również są używane z dopuszczaniem wartości null dla tych typów. Podnoszone operatory są zbudowane ze wstępnie zdefiniowanych i zdefiniowanych przez użytkownika operatorów spełniających określone wymagania, zgodnie z opisem w następujących tematach:

*   Dla operatorów jednoargumentowych

    ```csharp
    +  ++  -  --  !  ~
    ```

    podnieśna postać operatora istnieje, jeśli argument operacji i typ wyniku nie dopuszcza wartości null. Unieś formularz jest konstruowany przez dodanie pojedynczego modyfikatora `?` do operandu i typów wyników. Podnieśny operator generuje wartość null, jeśli operand ma wartość null. W przeciwnym razie, podnieśny operator odwinie operand, stosuje operator podstawowy i otacza wynik.

*   Dla operatorów binarnych

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    podnieśna postać operatora istnieje, jeśli operand i typy wyników to wszystkie wartości niedopuszczające wartości null. Unieś formularz jest konstruowany przez dodanie pojedynczego modyfikatora `?` do każdego operandu i typu wyniku. Podnieśny operator generuje wartość null, jeśli jeden lub oba operandy mają wartość null (wyjątek `&` i `|` operatory typu `bool?`, zgodnie z opisem w [operatorach logicznych logicznej](expressions.md#boolean-logical-operators)). W przeciwnym razie podnieśny operator odpakuje operandy, zastosuje operator podstawowy i otacza wynik.

*   Dla operatorów równości

    ```csharp
    ==  !=
    ```

    podnieśna postać operatora istnieje, jeśli typy operandów to zarówno typ wartości niedopuszczający wartości null, jak i typ wyniku `bool`. Zniesiony formularz jest konstruowany przez dodanie pojedynczego modyfikatora `?` do każdego typu operandu. Podnieśny operator traktuje dwie wartości null, a wartość null nie jest równa żadnej wartości innej niż null. Jeśli oba operandy mają wartość różną od null, przenoszonego operatora odwinie operandy i zastosuje operatora bazowego, aby wygenerować wynik `bool`.

*   Dla operatorów relacyjnych

    ```csharp
    <  >  <=  >=
    ```

    podnieśna postać operatora istnieje, jeśli typy operandów to zarówno typ wartości niedopuszczający wartości null, jak i typ wyniku `bool`. Zniesiony formularz jest konstruowany przez dodanie pojedynczego modyfikatora `?` do każdego typu operandu. Podnieśny operator tworzy wartość `false`, jeśli jeden lub oba operandy mają wartość null. W przeciwnym razie podnieśny operator odwinie operandy i zastosuje operatora bazowego, aby wygenerować wynik `bool`.

## <a name="member-lookup"></a>Wyszukiwanie elementów członkowskich

Przeszukiwanie elementu członkowskiego jest procesem, w którym określa się znaczenie nazwy w kontekście typu. Wyszukiwanie elementu członkowskiego może odbywać się w ramach oceny *simple_name* ([prostych nazw](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w wyrażeniu. Jeśli *simple_name* lub *member_access* występuje jako *primary_expression* *invocation_expression* ([wywołania metody](expressions.md#method-invocations)), element członkowski jest określany jako wywoływany.

Jeśli element członkowski jest metodą lub zdarzeniem lub jeśli jest to stała, pole lub właściwość typu delegata ([delegatów](delegates.md)) lub typu `dynamic` ([Typ dynamiczny](types.md#the-dynamic-type)), wówczas element członkowski jest określany jako *wywoływać*.

Funkcja wyszukiwania elementów członkowskich traktuje nie tylko nazwę elementu członkowskiego, ale również liczbę parametrów typu, które ma element członkowski i czy element członkowski jest dostępny. Na potrzeby wyszukiwania elementów członkowskich, metod ogólnych i zagnieżdżonych typów ogólnych ma liczbę parametrów typu wskazanych w odpowiednich deklaracjach, a wszystkie inne elementy członkowskie mają parametry typu zero.

Przeszukiwanie elementu członkowskiego o nazwie `N` z parametrami typu `K` w `T` jest przetwarzane w następujący sposób:

*  Najpierw zostanie wyznaczona zbiór dostępnych składowych o nazwie `N`:
    * Jeśli `T` jest parametrem typu, zestaw jest Unią zestawów dostępnych składowych o nazwie `N` w każdym typie określonym jako ograniczenie podstawowe lub ograniczenie pomocnicze ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) dla `T`oraz zestaw dostępnych elementów członkowskich o nazwie `N` w `object`.
    * W przeciwnym razie zestaw składa się ze wszystkich dostępnych członków ([dostęp członków](basic-concepts.md#member-access)) o nazwie `N` w `T`, łącznie z dziedziczonymi członkami i dostępnymi elementami członkowskimi o nazwie `N` w `object`. Jeśli `T` jest typem skonstruowanym, zestaw elementów członkowskich jest uzyskiwany przez podstawianie argumentów typu, zgodnie z opisem w [składowych typów skonstruowanych](classes.md#members-of-constructed-types). Elementy członkowskie, które zawierają modyfikator `override`, są wykluczone z zestawu.
*  Następnie Jeśli `K` ma wartość zero, wszystkie typy zagnieżdżone, których deklaracje obejmują parametry typu, są usuwane. Jeśli `K` nie jest zerem, wszystkie elementy członkowskie z inną liczbą parametrów typu są usuwane. Należy pamiętać, że jeśli `K` ma wartość zero, metody mające parametry typu nie zostaną usunięte, ponieważ proces wnioskowania typów ([wnioskowanie](expressions.md#type-inference)o typie) może być w stanie wywnioskować argumenty typu.
*  Następnie, jeśli element członkowski jest *wywoływany*, wszystkie elementy członkowskie inne niż*wywoływać* są usuwane z zestawu.
*  Następnie elementy członkowskie, które są ukryte przez inne elementy członkowskie, są usuwane z zestawu. Dla każdego elementu członkowskiego `S.M` w zestawie, gdzie `S` jest typem, w którym zadeklarowany jest `M` elementu członkowskiego, są stosowane następujące reguły:
    * Jeśli `M` to stała, pole, właściwość, zdarzenie lub element członkowski wyliczenia, wszystkie elementy członkowskie zadeklarowane w typie podstawowym `S` są usuwane z zestawu.
    * Jeśli `M` jest deklaracją typu, wszystkie typy, które nie są zadeklarowane w typie podstawowym `S`, są usuwane z zestawu, a wszystkie deklaracje typu z tą samą liczbą parametrów typu jako `M` zadeklarowane w typie podstawowym `S` są usuwane z zestawu.
    * Jeśli `M` jest metodą, wszystkie elementy członkowskie inne niż metody zadeklarowane w typie podstawowym `S` są usuwane z zestawu.
*  Następnie elementy członkowskie interfejsu, które są ukryte przez elementy członkowskie klasy, są usuwane z zestawu. Ten krok ma wpływ tylko wtedy, gdy `T` jest parametrem typu, a `T` ma zarówno obowiązującą klasę bazową inną niż `object`, jak i niepusty obowiązujący zestaw interfejsów ([ograniczenia parametru typu](classes.md#type-parameter-constraints)). Dla każdego elementu członkowskiego `S.M` w zestawie, gdzie `S` jest typem, w którym zadeklarowany jest `M` elementu członkowskiego, są stosowane następujące reguły, jeśli `S` jest deklaracją klasy inną niż `object`:
    * Jeśli `M` jest stałą, pole, właściwość, zdarzenie, element członkowski wyliczenia lub Deklaracja typu, wszystkie elementy członkowskie zadeklarowane w deklaracji interfejsu są usuwane z zestawu.
    * Jeśli `M` jest metodą, wszystkie elementy członkowskie inne niż metody zadeklarowane w deklaracji interfejsu są usuwane z zestawu, a wszystkie metody z tą samą sygnaturą co `M` zadeklarowane w deklaracji interfejsu są usuwane z zestawu.
*  Na koniec po usunięciu ukrytych elementów członkowskich zostanie określony wynik wyszukiwania:
    * Jeśli zestaw składa się z pojedynczego elementu członkowskiego, który nie jest metodą, wówczas ten element członkowski jest wynikiem wyszukiwania.
    * W przeciwnym razie, jeśli zestaw zawiera tylko metody, ta grupa metod jest wynikiem wyszukiwania.
    * W przeciwnym razie odnośnik jest niejednoznaczny i wystąpi błąd w czasie powiązania.

W przypadku przeszukiwania elementów członkowskich w typach innych niż parametry typu i interfejsy oraz wyszukiwania elementów członkowskich w interfejsach, które są ściśle dziedziczenia (każdy interfejs w łańcuchu dziedziczenia ma dokładnie zero lub jeden bezpośredni interfejs podstawowy), efekt reguł wyszukiwania to Wystarczy, że pochodni członkowie ukrywają elementy bazowe o tej samej nazwie lub podpisie. Takie wyszukiwania o pojedynczej dziedziczeniu nigdy nie są niejednoznaczne. Niejasności, które mogą wystąpić w odnośnikach elementów członkowskich w interfejsach z wieloma dziedziczeniem, są opisane w [dostęp do elementu członkowskiego interfejsu](interfaces.md#interface-member-access).

### <a name="base-types"></a>Typy podstawowe

W celach wyszukiwania elementów członkowskich typ `T` jest traktowany jako mający następujące typy podstawowe:

*  Jeśli `T` jest `object`, `T` nie ma typu podstawowego.
*  Jeśli `T` jest *enum_type*, typy podstawowe `T` są typami klas `System.Enum`, `System.ValueType`i `object`.
*  Jeśli `T` jest *struct_type*, typy podstawowe `T` są typami klas `System.ValueType` i `object`.
*  Jeśli `T` jest *class_type*, podstawowe typy `T` są klasami podstawowymi `T`, łącznie z typem klasy `object`.
*  Jeśli `T` jest *INTERFACE_TYPE*, podstawowe typy `T` są podstawowymi interfejsami `T` i typem klasy `object`.
*  Jeśli `T` jest *array_type*, typy podstawowe `T` są typami klas `System.Array` i `object`.
*  Jeśli `T` jest *delegate_type*, typy podstawowe `T` są typami klas `System.Delegate` i `object`.

## <a name="function-members"></a>Elementy członkowskie funkcji

Składowe funkcji są elementami członkowskimi, które zawierają instrukcje wykonywalne. Składowe funkcji są zawsze członkami typów i nie mogą być elementami członkowskimi przestrzeni nazw. C#definiuje następujące kategorie elementów członkowskich funkcji:

*  Metody
*  Właściwości
*  Zdarzenia
*  Indeksatory
*  Operatory zdefiniowane przez użytkownika
*  Konstruktory wystąpień
*  Konstruktory statyczne
*  Destruktory

Oprócz destruktorów i konstruktorów statycznych (które nie mogą być wywoływane jawnie), instrukcje zawarte w składowych funkcji są wykonywane za pomocą wywołań elementu członkowskiego funkcji. Rzeczywista składnia do pisania wywołania składowej funkcji zależy od określonej kategorii elementu członkowskiego funkcji.

Lista argumentów ([listy argumentów](expressions.md#argument-lists)) wywołania elementu członkowskiego funkcji udostępnia wartości rzeczywiste lub odwołania do zmiennych dla parametrów elementu członkowskiego funkcji.

Wywołania metod ogólnych mogą wykorzystywać wnioskowanie o typie w celu określenia zestawu argumentów typu, które mają zostać przekazane do metody. Ten proces jest opisany w [wnioskach o typie](expressions.md#type-inference).

Wywołania metod, indeksatorów, operatorów i wystąpień stosują rozwiązanie przeciążenia, aby określić, który zestaw elementów członkowskich funkcji ma być wywoływany. Ten proces jest opisany w temacie [rozpoznawanie przeciążenia](expressions.md#overload-resolution).

Gdy określony element członkowski funkcji zostanie zidentyfikowany w czasie wiązania, prawdopodobnie przez rozpoznanie przeciążenia, rzeczywisty proces wykonywania wywoływany przez element członkowski funkcji jest opisany w [kontroli w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Poniższa tabela zawiera podsumowanie przetwarzania, które odbywa się w konstrukcjach obejmujących sześć kategorii elementów członkowskich funkcji, które można jawnie wywołać. W tabeli, `e`, `x`, `y`i `value` wskazują wyrażenia sklasyfikowane jako zmienne lub wartości, `T` wskazuje wyrażenie sklasyfikowane jako typ, `F` jest prostą nazwą metody, a `P` jest prostą nazwą właściwości.


| __Construct__     | __Przykład__    | __Opis__ |
|-------------------|----------------|-----------------|
| Wywołanie metody | `F(x,y)`       | Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszej metody `F` w zawierającej ją klasie lub strukturze. Metoda jest wywoływana z listą argumentów `(x,y)`. Jeśli metoda nie jest `static`, wyrażenie wystąpienia jest `this`. | 
|                   | `T.F(x,y)`     | Rozpoznanie przeciążenia jest stosowane do wybierania najlepszej metody `F` w klasie lub strukturze `T`. Błąd czasu powiązania występuje, jeśli metoda nie jest `static`. Metoda jest wywoływana z listą argumentów `(x,y)`. | 
|                   | `e.F(x,y)`     | Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszej metody F w klasie, strukturze lub interfejsie określonej przez typ `e`. Jeśli metoda jest `static`, występuje błąd czasu powiązania. Metoda jest wywoływana z wyrażeniem wystąpienia `e` i listą argumentów `(x,y)`. | 
| Dostęp do właściwości   | `P`            | Zostanie wywołana metoda dostępu `get` `P` właściwości w zawierającej ją klasie lub strukturze. Błąd czasu kompilacji występuje, jeśli `P` jest tylko do zapisu. Jeśli `P` nie jest `static`, wyrażenie wystąpienia jest `this`. | 
|                   | `P = value`    | Metoda dostępu `set` `P` właściwości w zawartej klasie lub strukturze jest wywoływana z listą argumentów `(value)`. Błąd czasu kompilacji występuje, jeśli `P` jest tylko do odczytu. Jeśli `P` nie jest `static`, wyrażenie wystąpienia jest `this`. | 
|                   | `T.P`          | Metoda dostępu `get` `P` właściwości w klasie lub strukturze `T` jest wywoływana. Błąd czasu kompilacji występuje, jeśli `P` nie `static` lub jeśli `P` jest tylko do zapisu. | 
|                   | `T.P = value`  | Metoda dostępu `set` `P` właściwości w klasie lub strukturze `T` jest wywoływana z listą argumentów `(value)`. Błąd czasu kompilacji występuje, jeśli `P` nie `static` lub jeśli `P` jest tylko do odczytu. | 
|                   | `e.P`          | Metoda dostępu `get` `P` właściwości w klasie, strukturze lub interfejsie określona przez typ `e` jest wywoływana z wyrażeniem wystąpienia `e`. Błąd czasu powiązania występuje, jeśli `P` jest `static` lub jeśli `P` jest tylko do zapisu. | 
|                   | `e.P = value`  | Metoda dostępu `set` `P` właściwości w klasie, strukturze lub interfejsie określona przez typ `e` jest wywoływana przy użyciu wyrażenia wystąpienia `e` i listy argumentów `(value)`. Błąd czasu powiązania występuje, jeśli `P` jest `static` lub jeśli `P` jest tylko do odczytu. | 
| Dostęp do zdarzeń      | `E += value`   | Metoda dostępu `add` `E` zdarzeń w klasie lub strukturze zawierającej jest wywoływana. Jeśli `E` nie jest statyczna, wyrażenie wystąpienia jest `this`. | 
|                   | `E -= value`   | Metoda dostępu `remove` `E` zdarzeń w klasie lub strukturze zawierającej jest wywoływana. Jeśli `E` nie jest statyczna, wyrażenie wystąpienia jest `this`. | 
|                   | `T.E += value` | Metoda dostępu `add` `E` zdarzeń w klasie lub strukturze `T` jest wywoływana. Jeśli `E` nie jest statyczna, występuje błąd czasu powiązania. | 
|                   | `T.E -= value` | Metoda dostępu `remove` `E` zdarzeń w klasie lub strukturze `T` jest wywoływana. Jeśli `E` nie jest statyczna, występuje błąd czasu powiązania. | 
|                   | `e.E += value` | Metoda dostępu `add` `E` zdarzeń w klasie, strukturze lub interfejsie określona przez typ `e` jest wywoływana z wyrażeniem wystąpienia `e`. Gdy `E` jest statyczny, występuje błąd czasu powiązania. | 
|                   | `e.E -= value` | Metoda dostępu `remove` `E` zdarzeń w klasie, strukturze lub interfejsie określona przez typ `e` jest wywoływana z wyrażeniem wystąpienia `e`. Gdy `E` jest statyczny, występuje błąd czasu powiązania. | 
| Dostęp indeksatora    | `e[x,y]`       | Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszego indeksatora w klasie, strukturze lub interfejsie przyznanym przez typ e. Metoda dostępu `get` indeksatora jest wywoływana z wyrażeniem wystąpienia `e` i listą argumentów `(x,y)`. Błąd czasu powiązania występuje, gdy indeksator jest tylko do zapisu. | 
|                   | `e[x,y] = value` | Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszego indeksatora w klasie, strukturze lub interfejsie przyznanym przez typ `e`. Metoda dostępu `set` indeksatora jest wywoływana z wyrażeniem wystąpienia `e` i listą argumentów `(x,y,value)`. Błąd czasu powiązania występuje, gdy indeksator jest tylko do odczytu. | 
| Wywołanie operatora | `-x`         | Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszego operatora jednoargumentowego w klasie lub strukturze podanej przez typ `x`. Wybrany operator jest wywoływany z listą argumentów `(x)`. | 
|                     | `x + y`      | Rozpoznanie przeciążenia jest stosowane w celu wybrania najlepszego operatora binarnego w klasach lub strukturach określonych przez typy `x` i `y`. Wybrany operator jest wywoływany z listą argumentów `(x,y)`. | 
| Wywołanie konstruktora wystąpienia | `new T(x,y)` | Rozpoznanie przeciążenia jest stosowane do wybierania najlepszego konstruktora wystąpienia w klasie lub strukturze `T`. Konstruktor wystąpienia jest wywoływany z listą argumentów `(x,y)`. | 

### <a name="argument-lists"></a>Listy argumentów

Każdy element członkowski funkcji i obiekt delegowany zawiera listę argumentów, która dostarcza wartości rzeczywiste lub odwołania do zmiennych parametrów elementu członkowskiego funkcji. Składnia określająca listę argumentów wywołania elementu członkowskiego funkcji zależy od kategorii elementu członkowskiego funkcji:

*  W przypadku konstruktorów wystąpień, metod, indeksatorów i delegatów argumenty są określone jako *argument_list*, zgodnie z poniższym opisem. W przypadku indeksatorów podczas wywoływania metody dostępu `set`, lista argumentów dodatkowo zawiera wyrażenie określone jako prawy operand operatora przypisania.
*  Dla właściwości, lista argumentów jest pusta podczas wywoływania metody dostępu `get` i składa się z wyrażenia określonego jako prawy operand operatora przypisania podczas wywoływania metody dostępu `set`.
*  Dla zdarzeń lista argumentów składa się z wyrażenia określonego jako prawy operand operatora `+=` lub `-=`.
*  Dla operatorów zdefiniowanych przez użytkownika lista argumentów składa się z pojedynczego operandu jednoargumentowego operatora lub dwóch operandów operatora binarnego.

Argumenty właściwości ([Właściwości](classes.md#properties)), zdarzenia ([zdarzenia](classes.md#events)) i Operatory zdefiniowane przez użytkownika ([Operatory](classes.md#operators)) są zawsze przekazane jako parametry wartości ([Parametry wartości](classes.md#value-parameters)). Argumenty indeksatorów ([indeksatorów](classes.md#indexers)) są zawsze przekazane jako parametry wartości ([Parametry wartości](classes.md#value-parameters)) lub tablice parametrów ([tablice parametrów](classes.md#parameter-arrays)). Parametry referencyjne i wyjściowe nie są obsługiwane dla tych kategorii elementów członkowskich funkcji.

Argumenty konstruktora wystąpienia, metody, indeksatora lub wywołania delegata są określone jako *argument_list*:

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

*Argument_list* składa się z jednego lub więcej *argumentów*s, rozdzielonych przecinkami. Każdy argument składa się z opcjonalnego *argument_name* , po którym następuje *argument_value*. *Argument* o *argument_name* jest określany jako ***argument nazwany***, podczas gdy *Argument* bez *argument_name* jest ***argumentem pozycyjnym***. Występuje błąd dla argumentu pozycyjnego, który ma być wyświetlany po nazwanym argumencie w *argument_list*.

*Argument_value* może przyjmować jedną z następujących form:

*  *Wyrażenie*wskazujące, że argument jest przenoszona jako parametr wartości ([Parametry wartości](classes.md#value-parameters)).
*  Słowo kluczowe `ref` następuje *variable_reference* ([odwołania do zmiennych](variables.md#variable-references)) wskazujące, że argument jest przenoszona jako parametr odwołania ([parametry odwołania](classes.md#reference-parameters)). Zmienna musi być ostatecznie przypisana ([przypisanie](variables.md#definite-assignment)), zanim będzie mogła zostać przeniesiona jako parametr odwołania. Słowo kluczowe `out` następuje *variable_reference* ([odwołania do zmiennych](variables.md#variable-references)), wskazujące, że argument jest przekazywane jako parametr wyjściowy ([Parametry wyjściowe](classes.md#output-parameters)). Zmienna jest uznawana za ostatecznie przypisaną ([przypisanie](variables.md#definite-assignment)) po wywołaniu elementu członkowskiego, w którym zmienna jest przenoszona jako parametr wyjściowy.

#### <a name="corresponding-parameters"></a>Odpowiednie parametry

Dla każdego argumentu na liście argumentów musi istnieć odpowiedni parametr w elemencie członkowskim funkcji lub w wywołaniu delegowania.

Lista parametrów użyta w poniższym przykładzie jest określona w następujący sposób:

*  W przypadku metod wirtualnych i indeksatorów zdefiniowanych w klasach lista parametrów jest wybierana z najbardziej szczegółowej deklaracji lub przesłonięcia składowej funkcji, rozpoczynając od typu statycznego odbiornika i przeszukiwania przez klasy bazowe.
*  W przypadku metod interfejsu i indeksatorów lista parametrów jest wybierana jako najbardziej Szczegółowa definicja elementu członkowskiego, rozpoczynając od typu interfejsu i przeszukiwaniem przez interfejsy podstawowe. Jeśli nie zostanie znaleziona lista parametrów unikatowych, jest to lista parametrów z niedostępnymi nazwami i żadne parametry opcjonalne nie są konstruowane, dlatego wywołania nie mogą używać parametrów nazwanych ani pomijać opcjonalnych argumentów.
*  Dla metod częściowych jest używana lista parametrów definiująca Deklaracja metody częściowej.
*  Dla wszystkich innych elementów członkowskich funkcji i delegatów istnieje tylko jedna lista parametrów, która jest używana.

Pozycja argumentu lub parametru jest definiowana jako liczba argumentów lub parametrów poprzedzających je na liście argumentów lub liście parametrów.

Odpowiednie parametry argumentów elementu członkowskiego funkcji są określane w następujący sposób:

*  Argumenty w *argument_list* konstruktorów wystąpień, metodach, indeksatorach i delegatów:
    * Argument pozycyjny, gdzie stały parametr występuje w tym samym położeniu na liście parametrów odpowiada temu parametrowi.
    * Argument pozycyjny składowej funkcji z tablicą parametrów wywoływaną w jej normalnej postaci odpowiada tablicy parametrów, która musi występować w tym samym położeniu na liście parametrów.
    * Argument pozycyjny elementu członkowskiego funkcji z tablicą parametrów wywołaną w jego rozwiniętej postaci, gdzie żaden stały parametr nie występuje w tym samym położeniu na liście parametrów, odnosi się do elementu w tablicy parametrów.
    * Nazwany argument odpowiada parametrowi o tej samej nazwie na liście parametrów.
    * W przypadku indeksatorów podczas wywoływania metody dostępu `set` wyrażenie określone jako prawy operand operatora przypisania odpowiada parametrowi niejawnemu `value` deklaracji akcesora `set`.
*  W przypadku właściwości podczas wywoływania metody dostępu `get` nie ma argumentów. Podczas wywoływania metody dostępu `set`, wyrażenie określone jako prawy operand operatora przypisania odpowiada parametrowi niejawnemu `value` deklaracji metody dostępu `set`.
*  Dla operatorów jednoargumentowych zdefiniowanych przez użytkownika (w tym konwersji) pojedynczy operand odpowiada jednemu parametrowi deklaracji operatora.
*  W przypadku operatorów binarnych zdefiniowanych przez użytkownika lewy argument operacji odpowiada pierwszemu parametrowi, a prawy operand odpowiada drugiemu parametrowi deklaracji operatora.

#### <a name="run-time-evaluation-of-argument-lists"></a>Ocena czasu wykonywania dla list argumentów

W czasie wykonywania wywołania elementu członkowskiego funkcji ([Sprawdzanie w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) wyrażenia lub odwołania do zmiennych listy argumentów są oceniane w kolejności od lewej do prawej w następujący sposób:

*  W przypadku parametru wartości wyrażenie argumentu jest oceniane i niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) do odpowiedniego typu parametru. Wartość będąca wynikiem jest wartością początkową parametru value w wywołaniu elementu członkowskiego funkcji.
*  Dla parametru reference lub Output jest oceniane odwołanie do zmiennej, a wynikowa lokalizacja magazynu stanowi lokalizację magazynu reprezentowanego przez parametr w wywołaniu elementu członkowskiego funkcji. Jeśli odwołanie do zmiennej określone jako parametr Reference lub Output jest elementem tablicy *reference_type*, wykonywane jest sprawdzanie w czasie wykonywania, aby upewnić się, że typ elementu tablicy jest identyczny z typem parametru. Jeśli to sprawdzenie nie powiedzie się, zostanie zgłoszony `System.ArrayTypeMismatchException`.

Metody, indeksatory i konstruktory wystąpień mogą deklarować jako tablicę parametrów ([tablice parametrów](classes.md#parameter-arrays)). Takie składowe funkcji są wywoływane w ich normalnej formie lub w rozwiniętej formie, w zależności od tego, który ma zastosowanie ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)):

*  Gdy element członkowski funkcji z tablicą parametrów jest wywoływany w jego normalnej postaci, argument określony dla tablicy parametrów musi być pojedynczym wyrażeniem, które jest niejawnie konwertowane ([konwersje niejawne](conversions.md#implicit-conversions)) na typ tablicy parametrów. W takim przypadku tablica parametrów działa dokładnie tak, jak parametr value.
*  Gdy element członkowski funkcji z tablicą parametrów jest wywoływany w jego rozwiniętej postaci, wywołanie musi określać zero lub więcej argumentów pozycyjnych dla tablicy parametrów, gdzie każdy argument jest wyrażeniem, które jest niejawnie konwertowane ([konwersje niejawne](conversions.md#implicit-conversions)) na typ elementu tablicy parametrów. W takim przypadku wywołanie tworzy wystąpienie typu tablicy parametrów o długości odpowiadającej liczbie argumentów, inicjuje elementy instancji Array z podaną wartością argumentów i używa nowo utworzonego wystąpienia tablicy jako wartości rzeczywistej argument.

Wyrażenia listy argumentów są zawsze oceniane w kolejności, w jakiej zostały wpisane. W tym przykładzie przykład
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
tworzy dane wyjściowe
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

Reguły współwariancji tablicy ([Kowariancja tablicowa](arrays.md#array-covariance)) zezwalają na wartość typu tablicy `A[]` być odwołaniem do wystąpienia typu tablicy `B[]`, pod warunkiem, że istnieje niejawna konwersja odwołań z `B` do `A`. Ze względu na te reguły, gdy element tablicy *reference_type* jest przenoszona jako parametr Reference lub Output, wymagane jest sprawdzenie w czasie wykonywania, aby upewnić się, że rzeczywisty typ elementu tablicy jest identyczny z parametrem. W przykładzie
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
drugie wywołanie `F` powoduje wygenerowanie `System.ArrayTypeMismatchException`, ponieważ rzeczywisty typ elementu `b` jest `string` i nie `object`.

Gdy element członkowski funkcji z tablicą parametrów jest wywoływany w rozwiniętej postaci, wywołanie jest przetwarzane dokładnie tak, jakby wyrażenie tworzenia tablicy z inicjatorem tablicy ([wyrażenia tworzenia tablicy](expressions.md#array-creation-expressions)) zostało wstawione wokół rozwiniętych parametrów. Na przykład, dana deklaracja
```csharp
void F(int x, int y, params object[] args);
```
następujące wywołania rozwiniętej formy metody
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
odpowiada dokładnie na
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

W szczególności należy zauważyć, że pusta tablica jest tworzona, gdy dla tablicy parametrów nie podano żadnych argumentów.

Gdy argumenty są pomijane z elementu członkowskiego funkcji z odpowiednimi parametrami opcjonalnymi, domyślne argumenty deklaracji elementu członkowskiego funkcji są niejawnie przekazane. Ponieważ są one zawsze stałe, ich ocena nie będzie miała wpływu na kolejność oceny pozostałych argumentów.

### <a name="type-inference"></a>Wnioskowanie o typie

Gdy metoda ogólna jest wywoływana bez określenia argumentów typu, proces ***wnioskowania o typie*** próbuje wnioskować o argumenty typu dla wywołania. Obecność wnioskowania o typie umożliwia bardziej wygodną składnię do wywoływania metody ogólnej i umożliwia programisty uniknięcie określania nadmiarowych informacji o typie. Na przykład, dana Deklaracja metody:
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
możliwe jest wywołanie metody `Choose` bez jawnego określenia argumentu typu:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

Za pomocą wnioskowania o typie argumenty typu `int` i `string` są określane na podstawie argumentów metody.

Wnioskowanie o typie odbywa się w ramach przetwarzania powiązań metody wywołań metod ([wywołań metod](expressions.md#method-invocations)) i jest wykonywana przed etapem rozwiązywania przeciążenia wywołania. Gdy dana grupa metod jest określona w wywołaniu metody, a żadne argumenty typu nie są określone jako część wywołania metody, wnioskowanie typu jest stosowane do każdej metody ogólnej w grupie metod. Jeśli wnioskowanie o typie powiedzie się, argumenty typu wywnioskowanego są używane do określenia typów argumentów dla kolejnego rozwiązania przeciążenia. Jeśli rozwiązanie przeciążenia wybiera metodę rodzajową jako jedną do wywołania, wówczas argumenty typu wywnioskowanego są używane jako argumenty typu dla wywołania. Jeśli wnioskowanie o typie dla określonej metody nie powiedzie się, ta metoda nie bierze udziału w rozpoznawaniu przeciążenia. Błąd wnioskowania o typie, w obu przypadkach nie powoduje błędu powiązania. Jednak często prowadzi do błędu w czasie trwania powiązania, gdy rozwiązanie przeciążenia nie może znaleźć żadnych odpowiednich metod.

Jeśli podana liczba argumentów jest inna niż liczba parametrów w metodzie, wnioskowanie natychmiast zakończy się niepowodzeniem. W przeciwnym razie Załóżmy, że metoda ogólna ma następujący podpis:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

Z wywołaniem metody formularza `M(E1...Em)` zadania wnioskowania o typie jest znalezienie unikatowych argumentów typu `S1...Sn` dla każdego z parametrów typu `X1...Xn`, tak że wywołanie `M<S1...Sn>(E1...Em)` będzie prawidłowe.

Podczas procesu wnioskowania każdy parametr typu `Xi` jest *ustalony* do określonego typu `Si` lub *nieustalony* ze skojarzonym zestawem *granic*. Każda granica jest częścią typu `T`. Początkowo Każda zmienna typu `Xi` jest nierozwiązana z pustym zestawem granic.

Wnioskowanie o typie odbywa się w fazach. Każda faza podejmie próbę ustalenia argumentów typu dla większej liczby zmiennych typu w oparciu o wyniki poprzedniej fazy. Pierwsza faza polega na wstępnym wnioskach o granicach, podczas gdy druga faza naprawia zmienne typu dla określonych typów i wnioskuje o dalsze granice. Druga faza może być konieczna wielokrotnie.

*Uwaga:* Wnioskowanie o typie ma miejsce nie tylko wtedy, gdy wywoływana jest metoda ogólna. Wnioskowanie o typie dla konwersji grup metod jest opisane w [wnioskach o typie do konwersji grup metod](expressions.md#type-inference-for-conversion-of-method-groups) i znalezienie najlepszego wspólnego typu zestawu wyrażeń jest opisany w artykule [Znajdowanie najlepszego wspólnego typu zestawu wyrażeń](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).

#### <a name="the-first-phase"></a>Pierwsza faza

Dla każdego z argumentów metody `Ei`:

*   Jeśli `Ei` jest funkcją anonimową, *jawne wnioskowanie o typie parametru* ([jawne wnioskowanie o typie parametrów](expressions.md#explicit-parameter-type-inferences)) zostało wykonane z `Ei` do `Ti`
*   W przeciwnym razie, jeśli `Ei` ma typ `U`, a `xi` jest parametrem wartości, następuje *przewnioskowanie o niższej granicy* *od* `U` *do* `Ti`.
*   W przeciwnym razie, jeśli `Ei` ma typ `U` i `xi` jest parametrem `ref` lub `out`, nastąpiło *dokładne wnioskowanie* *z* `U` *do* `Ti`.
*   W przeciwnym razie dla tego argumentu nie są wykonywane żadne wnioskowanie.


#### <a name="the-second-phase"></a>Druga faza

Druga faza przebiega w następujący sposób:

*   Wszystkie *nienaprawione* zmienne typu `Xi` które nie są *zależne od* ([zależność](expressions.md#dependence)) dowolnych `Xj` są stałe ([naprawianie](expressions.md#fixing)).
*   Jeśli nie istnieją takie zmienne typu, wszystkie *nienaprawione* zmienne typu `Xi` są *rozwiązane* , dla których wszystkie następujące wstrzymano:
    *   Istnieje co najmniej jedna zmienna typu `Xj`, która zależy od `Xi`
    *   `Xi` nie ma pustego zestawu granic
*   Jeśli nie istnieją takie zmienne typu, a nadal istnieją *nienaprawione* zmienne typu, wnioskowanie o typie kończy się niepowodzeniem.
*   W przeciwnym razie, jeśli nie istnieją żadne dalsze zmienne typu *nienaprawionego* , wnioskowanie o typie powiedzie się.
*   W przeciwnym razie dla wszystkich argumentów `Ei` z odpowiadającym typem parametru `Ti`, gdzie *typy danych wyjściowych* ([typy wyjściowe](expressions.md#output-types)) zawierają *nienaprawione* zmienne typu, `Xj` ale *typy wejściowe* ([typy wejściowe](expressions.md#input-types)) nie, *wnioskowanie o typie danych wyjściowych* ([wnioskowania typu wyjściowego](expressions.md#output-type-inferences)) są tworzone *z* `Ei` *do* `Ti`. Następnie druga faza jest powtarzana.

#### <a name="input-types"></a>Typy wejściowe

Jeśli `E` jest grupą metod lub niejawnie wpisaną funkcją anonimową, a `T` jest typem delegata lub typem drzewa wyrażenia, wszystkie typy parametrów `T` są *typami wejściowymi* `E` *z typem* `T`.

####  <a name="output-types"></a>Typy danych wyjściowych

Jeśli `E` jest grupą metod lub funkcją anonimową, a `T` jest typem delegata lub typem drzewa wyrażenia, zwracany typ `T` jest *typem danych wyjściowych* `E` *z typem* `T`.

#### <a name="dependence"></a>Od

*Niestała* zmienna typu `Xi` *zależy bezpośrednio od* nieustalonej zmiennej typu `Xj` jeśli dla pewnego argumentu `Ek` z typem `Tk` `Xj` występuje w *typie danych wejściowych* `Ek` z typem `Tk` i `Xi` występuje w *typie danych wyjściowych* `Ek` z typem `Tk`.

`Xj` *zależy od* `Xi`, jeśli `Xj` *zależy bezpośrednio* od `Xi` lub, jeśli `Xi`, *zależy bezpośrednio* od `Xk`, a `Xk` *zależy* od `Xj`. W tym przypadku "zależy od" jest przechodnie, ale nie jest zamykany zwrotnie "jest zależny od bezpośredniej".

#### <a name="output-type-inferences"></a>Wnioskowanie o typie danych wyjściowych

*Wnioskowanie o typie danych wyjściowych* jest wykonywane *z* wyrażenia `E` *do* typu `T` w następujący sposób:

*  Jeśli `E` jest funkcją anonimową z odroczonym typem zwracanym `U` ([wywnioskowany typ zwracany](expressions.md#inferred-return-type)), a `T` jest typem delegata lub typem drzewa *wyrażenia z `Tb`* typu zwracanego, *a następnie* z `U` *do* `Tb`[](expressions.md#lower-bound-inferences).
*  W przeciwnym razie, jeśli `E` jest grupą metod, a `T` jest typem delegata lub typem drzewa wyrażenia z typami parametrów `T1...Tk` i zwracany typ `Tb`, a Rozpoznanie przeciążenia `E` z typami *`T1...Tk` powoduje utworzenie* pojedynczej metody z typem zwracanym `U`, a następnie przekazanie *od* `U` *do* `Tb`.
*  W przeciwnym razie, jeśli `E` jest wyrażeniem typu `U`, następuje *Dolna granica wnioskowania* *z* `U` *do* `T`.
*  W przeciwnym razie nie są wykonywane żadne wnioskowania.

#### <a name="explicit-parameter-type-inferences"></a>Wnioskowanie typu jawnego parametru

*Jawne wnioskowanie o typie parametru* zostało wykonane *z* wyrażenia `E` *do* typu `T` w następujący sposób:

*  Jeśli `E` jest jawnie wpisaną funkcją anonimową z typami parametrów `U1...Uk`, a `T` jest typem delegata lub typem drzewa wyrażenia z typami parametrów `V1...Vk` następnie dla każdego `Ui` *dokładnego wnioskowania* ([dokładne wnioskowania](expressions.md#exact-inferences)) nastąpi *od* `Ui` *do* odpowiadającego `Vi`.

#### <a name="exact-inferences"></a>Dokładne wnioskowanie

*Dokładne wnioskowanie* *z* typu `U` *do* typu `V` jest wykonywane w następujący sposób:

*  Jeśli `V` jest jedną z *nieustalonych* `Xi`, `U` jest dodawana do zestawu dokładnych granic dla `Xi`.

*  W przeciwnym razie zestawy `V1...Vk` i `U1...Uk` są określane przez sprawdzenie, czy którykolwiek z następujących przypadków ma zastosowanie:

   *  `V` jest typem tablicy `V1[...]`, a `U` jest typem tablicy `U1[...]` tej samej rangi
   *  `V` jest typem `V1?`, a `U` jest typem `U1?`
   *  `V` jest konstruowanym typem `C<V1...Vk>`i `U` jest typem skonstruowanym `C<U1...Uk>`

   Jeśli którykolwiek z tych przypadków ma zastosowanie, to *dokładne wnioskowanie* jest wykonywane *z* poszczególnych `Ui` *do* odpowiednich `Vi`.

*  W przeciwnym razie nie wprowadzono żadnych wniosków.

#### <a name="lower-bound-inferences"></a>Wnioskowanie o niższych ograniczeniach

*Mniej ograniczone wnioskowanie* *z* typu `U` *do* typu `V` jest wykonywane w następujący sposób:

*  Jeśli `V` jest jedną z *nieustalonych* `Xi`, `U` zostanie dodana do zestawu dolnych granic dla `Xi`.
*  W przeciwnym razie, jeśli `V` jest typem `V1?`, a `U` jest typem `U1?` następnie przeprowadzono dolną granicę od `U1` do `V1`.
*  W przeciwnym razie zestawy `U1...Uk` i `V1...Vk` są określane przez sprawdzenie, czy którykolwiek z następujących przypadków ma zastosowanie:
   *  `V` jest typem tablicy `V1[...]`, a `U` jest typem tablicy `U1[...]` (lub parametrem typu, którego efektywny typ podstawowy to `U1[...]`) o tej samej rangi
   *  `V` jest jedną z `IEnumerable<V1>`, `ICollection<V1>` lub `IList<V1>` i `U` jest typem tablicy jednowymiarowej `U1[]`(lub parametrem typu, którego obowiązuje typ podstawowy)
   *  `V` jest klasą konstruowaną, strukturą, interfejsem lub typem delegata `C<V1...Vk>` i istnieje unikatowy typ `C<U1...Uk>` taki, że `U` (lub, jeśli `U` jest parametrem typu, jego skuteczna Klasa bazowa lub którykolwiek element członkowski jego efektywnego zestawu interfejsów) jest taka sama jak, dziedziczy po (bezpośrednio lub pośrednio), lub implementuje (bezpośrednio lub pośrednio) `C<U1...Uk>`.

      (Ograniczenie "unikatowość" oznacza, że w interfejsie Case `C<T> {} class U: C<X>, C<Y> {}`nie są wykonywane żadne wnioskowanie podczas wnioskowania od `U` do `C<T>`, ponieważ `U1` może być `X` lub `Y`).

   Jeśli którykolwiek z tych przypadków stosuje, wnioskowanie jest wykonywane *z* każdego `Ui` *do* odpowiadającego `Vi` w następujący sposób:

   *  Jeśli `Ui` nie jest znany jako typ referencyjny, zostanie wykonane *dokładne wnioskowanie*
   *  W przeciwnym razie, jeśli `U` jest typem tablicy, tworzone jest *mniej ograniczone wnioskowanie*
   *  W przeciwnym razie, jeśli `V` jest `C<V1...Vk>`, wnioskowanie zależy od typu i-ty parametru `C`:
      *  Jeśli jest to współwariant, zostanie wprowadzony *niższy zakres wnioskowania* .
      *  Jeśli jest kontrawariantne, zostanie wykonane *górne ograniczenie wnioskowania* .
      *  Jeśli jest niezmienna, następuje *dokładne wnioskowanie* .
*  W przeciwnym razie nie są wykonywane żadne wnioskowania.

#### <a name="upper-bound-inferences"></a>Wnioskowanie o górnych granicach

*Górną granicę wnioskowania* *z* typu `U` *do* typu `V` są wykonywane w następujący sposób:

*  Jeśli `V` jest jedną z *nieustalonych* `Xi`, `U` zostanie dodana do zestawu górnych granic dla `Xi`.
*  W przeciwnym razie zestawy `V1...Vk` i `U1...Uk` są określane przez sprawdzenie, czy którykolwiek z następujących przypadków ma zastosowanie:
   *  `U` jest typem tablicy `U1[...]`, a `V` jest typem tablicy `V1[...]` tej samej rangi
   *  `U` jest jedną z `IEnumerable<Ue>`, `ICollection<Ue>` lub `IList<Ue>` i `V` jest jednowymiarowym typem tablicy `Ve[]`
   *  `U` jest typem `U1?`, a `V` jest typem `V1?`
   *  `U` jest konstruowany typ klasy, struktury, interfejsu lub delegata `C<U1...Uk>` a `V` jest klasą, strukturą, interfejsem lub typem delegata, który jest identyczny z, dziedziczy po (bezpośrednio lub pośrednio) lub implementuje (bezpośrednio lub pośrednio) unikatowy typ `C<V1...Vk>`

      (Ograniczenie "unikatowość" oznacza, że jeśli mamy `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, podczas wnioskowania od `C<U1>` do `V<Q>`nie są wykonywane żadne wnioskowanie. Wnioskowanie nie jest wykonywane z `U1` do `X<Q>` lub `Y<Q>`.)

   Jeśli którykolwiek z tych przypadków stosuje, wnioskowanie jest wykonywane *z* każdego `Ui` *do* odpowiadającego `Vi` w następujący sposób:
   *  Jeśli `Ui` nie jest znany jako typ referencyjny, zostanie wykonane *dokładne wnioskowanie*
   *  W przeciwnym razie, jeśli `V` jest typem tablicy, zostanie wykonane *górne ograniczenie wnioskowania*
   *  W przeciwnym razie, jeśli `U` jest `C<U1...Uk>`, wnioskowanie zależy od typu i-ty parametru `C`:
      *  Jeśli jest to współwariant, nadano *górną granicę* .
      *  Jeśli jest kontrawariantne, zostanie wykonane *obniżenie wnioskowania o niższym poziomie* .
      *  Jeśli jest niezmienna, następuje *dokładne wnioskowanie* .
*  W przeciwnym razie nie są wykonywane żadne wnioskowania.   

#### <a name="fixing"></a>Mocowanie

*Nieustalona* zmienna typu `Xi` z zestawem granic jest *ustalona* w następujący sposób:

*  Zestaw *typów kandydatów* `Uj` uruchamiany jako zestaw wszystkich typów w zestawie granic dla `Xi`.
*  Następnie analizujemy poszczególne powiązania `Xi` z kolei: dla każdego dokładnego `U` `Xi` wszystkich typów `Uj`, które nie są identyczne z `U` są usuwane z zestawu kandydatów. Dla każdej dolnej granicy `U` `Xi` wszystkie typy `Uj`, do których *nie* istnieje niejawna konwersja z `U` są usuwane z zestawu kandydatów. Dla każdej górnej granicy `U` `Xi` wszystkie typy `Uj` z których *nie* istnieje niejawna konwersja do `U` są usuwane z zestawu kandydatów.
*  Jeśli wśród pozostałych typów kandydatów `Uj` istnieje unikatowy typ `V`, z którego istnieje niejawna konwersja na wszystkie inne typy kandydatów, `Xi` jest naprawiona `V`.
*  W przeciwnym razie wnioskowanie o typie nie powiedzie się.

#### <a name="inferred-return-type"></a>Wywnioskowany typ zwracany

Wywnioskowany typ zwracany funkcji anonimowej `F` jest używany podczas wnioskowania o typie i rozpoznawania przeciążenia. Wywnioskowany typ zwracany można określić tylko dla funkcji anonimowej, w której znane są wszystkie typy parametrów, ponieważ są one jawnie podane przez funkcję anonimowej konwersji lub wywnioskowane podczas wnioskowania o typie w otaczającym ogólnym wywołanie metody.

***Wywnioskowany typ wyniku*** jest określany w następujący sposób:

*  Jeśli treść `F` jest *wyrażeniem* , które ma typ, wywnioskowany typ wyniku `F` jest typem tego wyrażenia.
*  Jeśli treść `F` jest *blok* , a zestaw wyrażeń w instrukcjach `return` bloku ma najlepszy wspólny typ `T` ([znalezienie najlepszego wspólnego typu zestawu wyrażeń](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), a następnie typ wyniku wywnioskowanego `F` jest `T`.
*  W przeciwnym razie nie można wywnioskować typu wyniku dla `F`.

***Wywnioskowany typ zwracany*** jest określany w następujący sposób:

*  Jeśli `F` jest Async i treścią `F` jest wyrażenie sklasyfikowane jako Nothing ([klasyfikacje wyrażeń](expressions.md#expression-classifications)) lub blok instrukcji, gdzie żadne instrukcje return nie mają wyrażeń, odroczony typ zwracany jest `System.Threading.Tasks.Task`
*  Jeśli `F` jest Async i ma wywnioskowany typ wyniku `T`, zostanie `System.Threading.Tasks.Task<T>`wnioskowany typ zwracany.
*  Jeśli `F` nie jest Async i ma wywnioskowany typ wyniku `T`, zostanie `T`wnioskowany typ zwracany.
*  W przeciwnym razie nie można wywnioskować zwracanego typu dla `F`.

Przykładowo wnioskowanie o typie obejmujące funkcje anonimowe, należy wziąć pod uwagę metodę rozszerzenia `Select` zadeklarowaną w klasie `System.Linq.Enumerable`:
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

Przy założeniu, że przestrzeń nazw `System.Linq` została zaimportowana z klauzulą `using` i dana klasa `Customer` z właściwością `Name` typu `string`, można użyć metody `Select`, aby wybrać nazwy listy klientów:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

Wywołanie metody rozszerzenia ([wywołania metody rozszerzenia](expressions.md#extension-method-invocations)) `Select` jest przetwarzane przez ponowne zapisanie wywołania do wywołania metody statycznej:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Ponieważ argumenty typu nie zostały jawnie określone, wnioskowanie typu jest używane do wywnioskowania argumentów typu. Najpierw argument `customers` jest powiązany z parametrem `source`, które wywołują `T` do `Customer`. Następnie przy użyciu opisanego powyżej procesu wnioskowania o typie funkcji anonimowej `c` jest dany typ `Customer`, a wyrażenie `c.Name` jest powiązane z typem zwracanym parametru `selector`, wnioskowanie `S` `string`. W rezultacie wywołanie jest równoważne
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
a wynik jest typu `IEnumerable<string>`.

Poniższy przykład ilustruje sposób, w jaki anonimowe funkcje wnioskowania umożliwiają informacje o typie "Flow" między argumentami w wywołaniu metody ogólnej. Dana metoda:
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

Wnioskowanie o typie dla wywołania:
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
działa w następujący sposób: najpierw argument `"1:15:30"` jest powiązany z parametrem `value`, które wywołują `X` do `string`. Następnie parametr pierwszej funkcji anonimowej, `s`, ma przyznany typ wnioskowany `string`, a wyrażenie `TimeSpan.Parse(s)` jest powiązane z typem zwracanym `f1`, wnioskowanie `Y` do `System.TimeSpan`. Na koniec parametr drugiej funkcji anonimowej, `t`, ma przyznany typ wnioskowany `System.TimeSpan`, a wyrażenie `t.TotalSeconds` jest powiązane z typem zwracanym `f2`, wnioskowanie `Z` do `double`. W związku z tym wynik wywołania jest typu `double`.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Wnioskowanie o typie dla konwersji grup metod

Podobnie jak w przypadku wywołań metod ogólnych, wnioskowanie o typie należy również zastosować, gdy grupa metod `M` zawierająca metodę generyczną jest konwertowana na dany typ delegata `D` ([konwersje grup metod](conversions.md#method-group-conversions)). Dana metoda
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
a grupa metod `M` przypisywana do typu delegata `D` zadanie wnioskowania o typie ma na celu znalezienie argumentów typu `S1...Sn` tak, aby wyrażenie:
```csharp
M<S1...Sn>
```
staną się zgodne ([deklaracje delegatów](delegates.md#delegate-declarations)) z `D`.

W przeciwieństwie do algorytmu wnioskowania o typie dla wywołań metod ogólnych, w tym przypadku są tylko *typy*argumentów, bez *wyrażeń*argumentów. W szczególności nie ma żadnych funkcji anonimowych i w związku z tym nie ma potrzeby stosowania wielu faz wnioskowania.

Zamiast tego wszystkie `Xi` są uznawane za *nienaprawione*, *a z każdego* typu argumentu `Uj` `D` *do* odpowiedniego typu parametru `Tj` `M`. Jeśli dla dowolnego `Xi` nie znaleziono żadnych granic, wnioskowanie o typie kończy się niepowodzeniem. W przeciwnym razie wszystkie `Xi` są *naprawione* odpowiednimi `Si`, które są wynikiem wnioskowania o typie.

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Znajdowanie najlepszego wspólnego typu zestawu wyrażeń

W niektórych przypadkach wspólny typ musi zostać wywnioskowany dla zestawu wyrażeń. W szczególności typy elementów niejawnie wpisanych tablic i zwracane typy funkcji anonimowych z treści *bloku* są dostępne w ten sposób.

Intuicyjnie, z uwzględnieniem zestawu wyrażeń `E1...Em` ten wnioskowanie powinno być równoważne wywołaniu metody
```csharp
Tr M<X>(X x1 ... X xm)
```
z `Ei` jako argumenty.

Dokładniej, wnioskowanie rozpoczyna się od *nieustalonej* zmiennej typu `X`. Następnie *wnioskowania typu danych wyjściowych* są następnie wykonywane *z* poszczególnych `Ei` *do* `X`. Na koniec `X` jest *naprawiony* i, jeśli to się powiedzie, typ otrzymany `S` jest najlepszym typowym typem dla wyrażeń. Jeśli taka `S` nie istnieje, wyrażenia nie mają najlepszego wspólnego typu.

### <a name="overload-resolution"></a>Rozpoznanie przeciążenia

Rozwiązanie przeciążania to mechanizm umożliwiający wybranie najlepszego elementu członkowskiego funkcji w celu wywołania listy argumentów i zestawu elementów członkowskich funkcji kandydujących. Rozwiązanie przeciążania wybiera element członkowski funkcji do wywołania w następujących odrębnych C#kontekstach w obrębie:

*  Wywołanie metody o nazwie w *invocation_expression* ([wywołania metody](expressions.md#method-invocations)).
*  Wywołanie konstruktora wystąpienia o nazwie w *object_creation_expression* ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)).
*  Wywołanie metody dostępu indeksatora za poorednictwem *element_access* ([dostęp do elementu](expressions.md#element-access)).
*  Wywołanie wstępnie zdefiniowanego lub zdefiniowanego przez użytkownika operatora przywoływanego w wyrażeniu ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution) i [Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)).

Każdy z tych kontekstów definiuje zestaw elementów członkowskich funkcji kandydujących i listę argumentów we własnym unikatowy sposób, jak opisano szczegółowo w sekcjach wymienionych powyżej. Na przykład zestaw kandydatów dla wywołania metody nie obejmuje metod oznaczonych `override` ([odnośnik elementów członkowskich](expressions.md#member-lookup)), a metody w klasie bazowej nie są kandydatami, jeśli stosowana jest jakakolwiek metoda w klasie pochodnej ([wywołania metod](expressions.md#method-invocations)).

Po zidentyfikowaniu elementów członkowskich funkcji kandydujących i listy argumentów wybór najlepszego elementu członkowskiego funkcji jest taki sam we wszystkich przypadkach:

*  Z uwzględnieniem zestawu odpowiednich członków funkcji kandydujących należy zlokalizować element członkowski najlepszych funkcji w tym zestawie. Jeśli zestaw zawiera tylko jeden element członkowski funkcji, to element członkowski funkcji jest najlepszą funkcją. W przeciwnym razie najlepszą składową funkcji jest element członkowski funkcji, który jest lepszy niż wszystkie inne elementy członkowskie funkcji w odniesieniu do danej listy argumentów, pod warunkiem, że każdy element członkowski funkcji jest porównywany z innymi składowymi funkcji przy użyciu reguł w [lepszym elemencie członkowskim funkcji](expressions.md#better-function-member). Jeśli nie ma dokładnie jednego elementu członkowskiego funkcji, który jest lepszy niż wszystkie inne składowe funkcji, wywołanie elementu członkowskiego funkcji jest niejednoznaczne i występuje błąd w czasie powiązania.

W poniższych sekcjach opisano dokładne znaczenie ***elementów członkowskich funkcji*** i ***lepszy element członkowski funkcji***.

#### <a name="applicable-function-member"></a>Odpowiedni element członkowski funkcji

Członek funkcji jest wskazany jako ***element członkowski funkcji*** w odniesieniu do listy argumentów `A`, gdy spełnione są wszystkie z następujących warunków:

*  Każdy argument w `A` odpowiada parametrowi w deklaracji elementu członkowskiego funkcji, zgodnie z opisem w [odpowiednich parametrach](expressions.md#corresponding-parameters), oraz dowolnego parametru, do którego nie odpowiada żaden argument, jest parametrem opcjonalnym.
*  Dla każdego argumentu w `A`tryb przekazywania parametrów argumentu (tj. Value, `ref`lub `out`) jest identyczny z trybem przekazywania parametrów dla odpowiedniego parametru, i
   *  dla parametru value lub tablicy parametrów niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z argumentu do typu odpowiadającego parametru lub
   *  dla parametru `ref` lub `out` typ argumentu jest identyczny z typem odpowiadającego parametru. Po wszystkich parametr `ref` lub `out` jest aliasem dla argumentu, który został przeszukany.

W przypadku składowej funkcji, która zawiera tablicę parametrów, jeśli element członkowski funkcji jest stosowany przez powyższe reguły, jest on uznawany za stosowany w jego ***normalnej postaci***. Jeśli element członkowski funkcji zawierający tablicę parametrów nie ma zastosowania w jego normalnej postaci, element członkowski funkcji może być stosowany w jego ***rozwiniętej postaci***:

*  Rozwinięty formularz jest konstruowany przez zastąpienie tablicy parametrów w deklaracji elementu członkowskiego funkcji wartością zero lub więcej parametrów wartości typu elementu tablicy parametrów w taki sposób, że liczba argumentów na liście argumentów `A` zgodna z łączną liczbą parametrów. Jeśli `A` ma mniejszą liczbę argumentów niż liczba stałych parametrów w deklaracji elementu członkowskiego funkcji, rozwinięta forma składowej funkcji nie może być skonstruowana i nie ma zastosowania.
*  W przeciwnym razie rozszerzona forma jest stosowana, jeśli dla każdego argumentu w `A` tryb przekazywania parametru argumentu jest taka sama jak tryb przekazywania parametrów odpowiadającego parametru, i
   *  dla parametru wartości ustalonej lub parametru wartości utworzonego przez ekspansję niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z typu argumentu do typu odpowiadającego parametru lub
   *  dla parametru `ref` lub `out` typ argumentu jest identyczny z typem odpowiadającego parametru.

#### <a name="better-function-member"></a>Lepsza składowa funkcji

Na potrzeby określenia lepszego elementu członkowskiego funkcja, Oddzielna lista argumentów A jest zbudowana zawierające tylko wyrażenia argumentów w kolejności, w jakiej występują na liście pierwotnych argumentów.

Listy parametrów dla każdego elementu członkowskiego funkcji kandydującej są konstruowane w następujący sposób:

*  Rozwinięty formularz jest używany, jeśli element członkowski funkcji ma zastosowanie tylko w rozwiniętym formularzu.
*  Parametry opcjonalne bez odpowiednich argumentów są usuwane z listy parametrów
*  Parametry są zmieniane w kolejności, tak że pojawiają się w tym samym miejscu co odpowiadający argument na liście argumentów.

Podano listę argumentów `A` z zestawem wyrażeń argumentów `{E1, E2, ..., En}` i dwóch odpowiednich członków funkcji `Mp` i `Mq` z typami parametrów `{P1, P2, ..., Pn}` i `{Q1, Q2, ..., Qn}`, `Mp` jest zdefiniowana jako ***lepsza składowa funkcji*** niż `Mq` Jeśli

*  dla każdego argumentu niejawna konwersja z `Ex` na `Qx` nie jest lepsza niż niejawna konwersja z `Ex` na `Px`i
*  dla co najmniej jednego argumentu konwersja z `Ex` na `Px` jest lepsza niż konwersja z `Ex` do `Qx`.

Podczas przeprowadzania tej oceny, jeśli `Mp` lub `Mq` ma zastosowanie w rozwiniętej postaci, a następnie `Px` lub `Qx` odwołuje się do parametru w rozwiniętej postaci listy parametrów.

W przypadku, gdy sekwencje typu parametrów `{P1, P2, ..., Pn}` i `{Q1, Q2, ..., Qn}` są równoważne (tj. Każda `Pi` ma konwersję tożsamości na odpowiadającą `Qi`), w kolejności są stosowane następujące reguły podziału powiązania, aby określić lepszy element członkowski funkcji.

*  Jeśli `Mp` jest metodą nierodzajową, a `Mq` jest metodą rodzajową, `Mp` jest lepsza niż `Mq`.
*  W przeciwnym razie, jeśli `Mp` ma zastosowanie w swojej normalnej formie i `Mq` ma tablicę `params` i ma zastosowanie tylko w rozwiniętej postaci, `Mp` jest lepsza niż `Mq`.
*  W przeciwnym razie, jeśli `Mp` ma więcej zadeklarowanych parametrów niż `Mq`, `Mp` jest lepsza niż `Mq`. Taka sytuacja może wystąpić, jeśli obie metody mają `params` tablice i są stosowane tylko w rozwiniętych formularzach.
*  W przeciwnym razie, jeśli wszystkie parametry `Mp` mają odpowiadający argument, a argumenty domyślne muszą zostać zastąpione dla co najmniej jednego parametru opcjonalnego w `Mq`, `Mp` jest lepszy niż `Mq`.
*  W przeciwnym razie, jeśli `Mp` ma więcej konkretnych typów parametrów niż `Mq`, `Mp` jest lepiej niż `Mq`. Niech `{R1, R2, ..., Rn}` i `{S1, S2, ..., Sn}` reprezentują typy parametrów, które nie są tworzone i są nierozwinięte `Mp` i `Mq`. typy parametrów `Mp`są bardziej szczegółowe niż `Mq`, jeśli dla każdego parametru `Rx` nie są mniej specyficzne niż `Sx`, a dla co najmniej jednego parametru `Rx` jest bardziej szczegółowy niż `Sx`:
   *  Parametr typu jest mniej specyficzny niż parametr niebędący typem.
   *  Rekursywnie skonstruowany typ jest bardziej szczegółowy niż inny skonstruowany typ (z tą samą liczbą argumentów typu), jeśli co najmniej jeden argument typu jest bardziej szczegółowy i żaden argument typu nie jest mniej specyficzny niż odpowiedni argument typu w drugim.
   *  Typ tablicy jest bardziej szczegółowy niż inny typ tablicy (o tej samej liczbie wymiarów), jeśli typ elementu pierwszego jest bardziej szczegółowy niż typ elementu drugiego.
*  W przeciwnym razie, jeśli jeden element członkowski jest niezniesionym operatorem, a drugi jest operatorem podnoszenia, to niezniesione.
*  W przeciwnym razie żaden element członkowski funkcji nie jest lepszy.

#### <a name="better-conversion-from-expression"></a>Lepsza konwersja z wyrażenia

Nadano niejawną konwersję `C1`, która konwertuje z wyrażenia `E` na typ `T1`oraz niejawną konwersję `C2`, która konwertuje z wyrażenia `E` do typu `T2`, `C1` to ***lepsza konwersja*** niż `C2`, jeśli `E` nie dokładnie pasuje `T2` i co najmniej jednego z następujących elementów:

* `E` dokładnie dopasowuje `T1` ([dokładnie pasujące wyrażenie](expressions.md#exactly-matching-expression))
* `T1` jest lepszym celem konwersji niż `T2` ([lepsza wartość docelowa konwersji](expressions.md#better-conversion-target))

#### <a name="exactly-matching-expression"></a>Wyrażenie dokładnie pasujące

Uwzględniając `E` wyrażenia i `T`typu, `E` dokładnie pasuje do `T`, jeśli jedno z następujących posiada:

*  `E` ma `S`typu, a konwersja tożsamości istnieje z `S` do `T`
*  `E` to funkcja anonimowa, `T` jest typem delegata `D` lub typem drzewa wyrażenia `Expression<D>` i jedną z następujących wartości:
   *  Wywnioskowany typ zwracany `X` istnieje dla `E` w kontekście listy parametrów `D` ([wnioskowany typ zwracany](expressions.md#inferred-return-type)) i konwersja tożsamości istnieje z `X` do zwracanego typu `D`
   *  `E` jest nieasync i `D` ma zwracany typ `Y` lub `E` jest typem asynchronicznym, a `D` ma zwracany typ `Task<Y>`i jedną z następujących wartości:
      * Treść `E` jest wyrażeniem, które dokładnie pasuje do `Y`
      * Treść `E` jest blokiem instrukcji, gdzie każda instrukcja return zwraca wyrażenie, które dokładnie pasuje do `Y`

#### <a name="better-conversion-target"></a>Lepszy cel konwersji

Podano dwa różne typy `T1` i `T2`, `T1` jest lepszym celem konwersji niż `T2`, jeśli nie istnieje niejawna konwersja z `T2` na `T1`, a co najmniej jeden z następujących elementów:

*  Niejawna konwersja z `T1` na `T2` istnieje
*  `T1` jest typem delegata `D1` lub typem drzewa wyrażenia, `Expression<D1>``T2` jest typem delegata `D2` lub typem drzewa wyrażenia `Expression<D2>`, `D1` ma zwracany typ `S1` i jedną z następujących wartości:
   * `D2` jest zwracana wartość void
   * `D2` ma `S2`typu zwracanego, a `S1` jest lepszą konwersją docelową niż `S2`
*  `T1` jest `Task<S1>`, `T2` jest `Task<S2>`, a `S1` jest lepszym celem konwersji niż `S2`
*  `T1` jest `S1` lub `S1?` gdzie `S1` jest podpisanym typem całkowitym, a `T2` jest `S2` lub `S2?`, gdzie `S2` jest typem całkowitym bez znaku. W szczególności:
   * `S1` jest `sbyte` i `S2` jest `byte`, `ushort`, `uint`lub `ulong`
   * `S1` jest `short` i `S2` jest `ushort`, `uint`lub `ulong`
   * `S1` jest `int` i `S2` `uint`lub `ulong`
   * `S1` jest `long` i `S2` jest `ulong`

#### <a name="overloading-in-generic-classes"></a>Przeciążanie klas ogólnych

Chociaż sygnatury jako zadeklarowane muszą być unikatowe, możliwe jest zastępowanie argumentów typu w identycznych sygnaturach. W takich przypadkach reguły łamania i przeciążania przeciążenia powyżej spowodują wybranie najbardziej szczegółowego elementu członkowskiego.

W poniższych przykładach przedstawiono nieprawidłowe i nieprawidłowe przeciążenia zgodne z tą regułą:

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>Sprawdzanie czasu kompilacji dynamicznego rozpoznawania przeciążenia

W przypadku większości operacji powiązanych dynamicznie zestaw możliwych kandydatów do rozwiązania jest nieznany w czasie kompilacji. W niektórych przypadkach, jednak zestaw kandydatów jest znany w czasie kompilacji:

*  Wywołania metody statycznej z argumentami dynamicznymi
*  Wywołania metody wystąpienia, w których odbiorca nie jest wyrażeniem dynamicznym
*  Wywołania indeksatora, w których odbiorca nie jest wyrażeniem dynamicznym
*  Wywołania konstruktora z argumentami dynamicznymi

W takich przypadkach jest przeprowadzane ograniczone sprawdzanie czasu kompilacji dla każdego kandydata, aby zobaczyć, czy którykolwiek z nich może być stosowany w czasie wykonywania. Ta kontrola obejmuje następujące kroki:

*  Wnioskowanie o typie częściowym: dowolny argument typu, który nie zależy bezpośrednio lub pośrednio od argumentu typu `dynamic` jest wnioskowany przy użyciu reguł [typu wnioskowanie](expressions.md#type-inference). Pozostałe argumenty typu są nieznane.
*  Sprawdzenie częściowego zastosowania: zastosowanie jest sprawdzane zgodnie z [odpowiednimi elementami członkowskimi funkcji](expressions.md#applicable-function-member), ale ignorowanie parametrów, których typy są nieznane.
*  Jeśli żaden kandydat nie przekaże tego testu, wystąpi błąd w czasie kompilacji.

### <a name="function-member-invocation"></a>Wywołanie elementu członkowskiego funkcji

W tej sekcji opisano proces, który odbywa się w czasie wykonywania w celu wywołania określonego elementu członkowskiego funkcji. Przyjęto założenie, że proces powiązania czasu już ustalił określony element członkowski do wywołania, prawdopodobnie przez zastosowanie rozpoznawania przeciążenia do zestawu elementów członkowskich funkcji kandydujących.

Do celów opisywania procesu wywołania składowe funkcji są podzielone na dwie kategorie:

*  Statyczne składowe funkcji. Są to konstruktory wystąpień, metody statyczne, dostęp do właściwości statycznych i Operatory zdefiniowane przez użytkownika. Statyczne składowe funkcji są zawsze niewirtualne.
*  Elementy członkowskie funkcji wystąpienia. Są to metody wystąpień, metod dostępu do właściwości wystąpienia i obiektu dostępu indeksatora. Elementy członkowskie funkcji wystąpienia są niewirtualne lub wirtualne i są zawsze wywoływane w konkretnym wystąpieniu. Wystąpienie jest obliczane przez wyrażenie wystąpienia i jest dostępne w składowej funkcji jako `this` ([ten dostęp](expressions.md#this-access)).

Przetwarzanie elementu członkowskiego funkcji w czasie wykonywania składa się z następujących kroków, gdzie `M` jest elementem członkowskim funkcji i, jeśli `M` jest składową wystąpienia, `E` jest wyrażeniem wystąpienia:

*  Jeśli `M` jest składową funkcji statycznej:
   * Lista argumentów jest szacowana zgodnie z opisem w liście [argumentów](expressions.md#argument-lists).
   * `M` jest wywoływana.

*  Jeśli `M` jest składową funkcji wystąpienia zadeklarowaną w *value_type*:
   * `E` jest oceniane. Jeśli ta Ocena spowoduje wyjątek, kolejne kroki nie są wykonywane.
   * Jeśli `E` nie jest sklasyfikowana jako zmienna, tworzona jest tymczasowa zmienna lokalna typu `E`, a wartość `E` jest przypisana do tej zmiennej. następnie `E` jest ponownie klasyfikowane jako odwołanie do tej tymczasowej zmiennej lokalnej. Zmienna tymczasowa jest dostępna jako `this` w `M`, ale nie w żaden inny sposób. W tym przypadku, tylko wtedy, gdy `E` jest prawdziwa zmienna jest możliwe, że obiekt wywołujący przestrzega zmian, które `M` `this`.
   * Lista argumentów jest szacowana zgodnie z opisem w liście [argumentów](expressions.md#argument-lists).
   * `M` jest wywoływana. Zmienna, do której odwołuje się `E`, to zmienna, do której odwołuje się `this`.

*  Jeśli `M` jest składową funkcji wystąpienia zadeklarowaną w *reference_type*:
   * `E` jest oceniane. Jeśli ta Ocena spowoduje wyjątek, kolejne kroki nie są wykonywane.
   * Lista argumentów jest szacowana zgodnie z opisem w liście [argumentów](expressions.md#argument-lists).
   * Jeśli typ `E` jest *value_type*, konwersja z opakowaniem ([konwersje](types.md#boxing-conversions)napakowywania) do konwersji `E` na typ `object`, a `E` jest uważana za typ `object` w poniższych krokach. W tym przypadku `M` może być tylko składową `System.Object`.
   * Wartość `E` jest sprawdzana jako prawidłowa. Jeśli wartość `E` jest `null`, zostanie zgłoszony `System.NullReferenceException` i nie zostaną wykonane żadne dalsze kroki.
   * Implementacja elementu członkowskiego funkcji do wywołania została określona:
     * Jeśli typ powiązania `E` jest interfejsem, element członkowski funkcji do wywołania jest implementacją `M` dostarczaną przez typ czasu wykonywania wystąpienia przywoływanego przez `E`. Ten element członkowski funkcji jest określany przez zastosowanie reguł mapowania interfejsu ([Mapowanie interfejsu](interfaces.md#interface-mapping)) do określenia implementacji `M` dostarczonej przez typ czasu wykonywania wystąpienia przywoływanego przez `E`.
     * W przeciwnym razie, jeśli `M` jest składową funkcji wirtualnej, element członkowski funkcji do wywołania jest implementacją `M` dostarczaną przez typ czasu wykonywania wystąpienia przywoływanego przez `E`. Ten element członkowski funkcji jest określany przez zastosowanie zasad określania najbardziej pochodnej implementacji ([metod wirtualnych](classes.md#virtual-methods)) `M` w odniesieniu do typu czasu wykonywania, do którego odwołuje się `E`.
     * W przeciwnym razie `M` jest składową funkcji niewirtualnej, a element członkowski funkcji do wywołania jest `M` samym sobą.
   * Implementacja elementu członkowskiego funkcji określona w powyższym kroku jest wywoływana. Obiekt, do którego odwołuje się `E`, jest obiektem, do którego odwołuje się `this`.

#### <a name="invocations-on-boxed-instances"></a>Wywołania w wystąpieniach opakowanych

Element członkowski funkcji zaimplementowany w *value_type* może być wywoływany za pomocą wystąpienia zapakowanego *value_type* , w następujących sytuacjach:

*  Gdy element członkowski funkcji jest `override` metody dziedziczonej z typu `object` i jest wywoływany przez wyrażenie wystąpienia typu `object`.
*  Gdy element członkowski funkcji jest implementacją składowej funkcji interfejsu i jest wywoływany za pomocą wyrażenia wystąpienia *INTERFACE_TYPE*.
*  Gdy element członkowski funkcji jest wywoływany przez delegata.

W takich sytuacjach zapakowane wystąpienie jest traktowane jako zmienna *value_type*, a ta zmienna to zmienna, do której odwołuje się `this` w wywołaniu elementu członkowskiego funkcji. W szczególności oznacza to, że gdy element członkowski funkcji jest wywoływany w przypadku wystąpienia zapakowanego, możliwe jest, aby element członkowski funkcji modyfikował wartość zawartą w zapakowanym wystąpieniu.

## <a name="primary-expressions"></a>Wyrażenia podstawowe

Wyrażenia podstawowe zawierają najprostszą postać wyrażeń.

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

Wyrażenia podstawowe są podzielone między *array_creation_expression*s i *primary_no_array_creation_expression*s. W ten sposób w ten sposób traktowanie wyrażenia tworzenia tablicowe, a nie wymienienie go wraz z innymi prostymi formularzami wyrażeń, umożliwia gramatykę w celu uniemożliwienia potencjalnie mylącego kodu, takiego jak
```csharp
object o = new int[3][1];
```
który w przeciwnym razie mógłby być interpretowany jako
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>Literały

*Primary_expression* , która składa się z *literału* ([literałów](lexical-structure.md#literals)), jest sklasyfikowany jako wartość.


### <a name="interpolated-strings"></a>Ciągi interpolowane

*Interpolated_string_expression* składa się ze znaku `$`, po którym następuje zwykły lub Verbatim literał ciągu, w którym znajdują się przerwy, rozdzielane przez `{` i `}`, otaczające wyrażenia i specyfikacje formatowania. Interpolowane wyrażenie ciągu jest wynikiem *interpolated_string_literal* , który został podzielony na poszczególne tokeny, zgodnie z opisem w [interpolowanych literałach ciągu](lexical-structure.md#interpolated-string-literals).

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

*Constant_expression* w interpolacji musi mieć niejawną konwersję, aby `int`.

*Interpolated_string_expression* jest klasyfikowany jako wartość. Jeśli jest on natychmiast konwertowany na `System.IFormattable` lub `System.FormattableString` z niejawną konwersją ciągu ([niejawnymi konwersji ciągów](conversions.md#implicit-interpolated-string-conversions)), interpolowane wyrażenie ciągu ma ten typ. W przeciwnym razie ma typ `string`.

Jeśli typ ciągu interpolowanego jest `System.IFormattable` lub `System.FormattableString`, znaczenie jest wywołanie `System.Runtime.CompilerServices.FormattableStringFactory.Create`. Jeśli typ jest `string`, znaczenie wyrażenia jest wywołaniem `string.Format`. W obu przypadkach lista argumentów wywołania składa się z literału ciągu formatu z symbolami zastępczymi dla każdej interpolacji oraz argumentu dla każdego wyrażenia odpowiadającego posiadaczom miejsc.

Literał ciągu formatu jest skonstruowany w następujący sposób, gdzie `N` jest liczbą interpolacji w *interpolated_string_expression*:

*  Jeśli *interpolated_regular_string_whole* lub *interpolated_verbatim_string_whole* następuje po znaku `$`, literał ciągu formatu jest tym tokenem.
*  W przeciwnym razie literał ciągu formatu składa się z: 
   *  Najpierw *interpolated_regular_string_start* lub *interpolated_verbatim_string_start*
   *  Następnie dla każdej liczby `I` od `0` do `N-1`: 
      * Reprezentacja dziesiętna `I`
      * Następnie, Jeśli odpowiednia *Interpolacja* ma *constant_expression*, `,` (przecinek), a następnie dziesiętną reprezentację wartości *constant_expression*
      * Następnie *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* lub *interpolated_verbatim_string_end* bezpośrednio po odpowiednim interpolce.

Kolejne argumenty są po prostu *wyrażeniami* z *interpolacji* (jeśli istnieją) w kolejności.

Do zrobienia: przykłady.


### <a name="simple-names"></a>Proste nazwy

*Simple_name* składa się z identyfikatora, opcjonalnie po którym następuje lista argumentów typu:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

*Simple_name* ma postać `I` lub formularza `I<A1,...,Ak>`, gdzie `I` jest pojedynczym identyfikatorem, a `<A1,...,Ak>` jest opcjonalny *type_argument_list*. Gdy nie określono *type_argument_list* , należy wziąć pod uwagę `K` wartość zero. *Simple_name* jest oceniane i sklasyfikowane w następujący sposób:

*  Jeśli `K` jest równa zero, a *simple_name* pojawia się w *bloku* , a jeśli *blok*(lub otaczający *blok*) obszar deklaracji zmiennych lokalnych ([deklaracji](basic-concepts.md#declarations)) zawiera zmienną lokalną, parametr lub stałą z nazwą `I`, wówczas *simple_name* odwołuje się do tej zmiennej lokalnej, parametru lub stałej i jest sklasyfikowana jako zmienna lub wartość.
*  Jeśli `K` ma wartość zero, a *simple_name* pojawia się w treści deklaracji metody ogólnej i jeśli ta deklaracja zawiera parametr typu o nazwie `I`, *simple_name* odwołuje się do tego parametru typu.
*  W przeciwnym razie dla każdego typu wystąpienia `T` ([Typ wystąpienia](classes.md#the-instance-type)), rozpoczynając od typu wystąpienia, bezpośrednio otaczającą deklarację typu i kontynuując typ wystąpienia każdej klasy lub deklaracji struktury (jeśli istnieje):
   *  Jeśli `K` ma wartość zero, a deklaracja `T` zawiera parametr typu o nazwie `I`, wówczas *simple_name* odwołuje się do tego parametru typu.
   *  W przeciwnym razie, jeśli odnośnik elementu członkowskiego ([odnośnik elementów członkowskich](expressions.md#member-lookup)) `I` w `T` z argumentami typu  `K`generuje dopasowanie:
      * Jeśli `T` jest typem wystąpienia bezpośrednio otaczającej klasy lub typu struktury, a odnośnik identyfikuje jedną lub więcej metod, wynik jest grupą metod ze skojarzonym wyrażeniem wystąpienia `this`. Jeśli określono listę argumentów typu, jest ona używana w wywołaniu metody ogólnej ([wywołania metody](expressions.md#method-invocations)).
      * W przeciwnym razie, jeśli `T` jest typem wystąpienia bezpośrednio otaczającej klasy lub typu struktury, jeśli wyszukiwanie identyfikuje element członkowski wystąpienia, a jeśli odwołanie występuje w treści konstruktora wystąpienia, metody wystąpienia lub akcesora wystąpienia, wynik jest taki sam jak dostęp do[elementu członkowskiego](expressions.md#member-access)w formularzu `this.I`. Może się to zdarzyć tylko wtedy, gdy `K` wynosi zero.
      * W przeciwnym razie wynik jest taki sam jak dostęp do elementu członkowskiego ([dostęp do elementu członkowskiego](expressions.md#member-access)) formularza `T.I` lub `T.I<A1,...,Ak>`. W tym przypadku jest to błąd czasu powiązania dla *simple_name* , aby odwołać się do elementu członkowskiego wystąpienia.

*  W przeciwnym razie dla każdej przestrzeni nazw `N`, rozpoczynając od przestrzeni nazw, w której występuje *simple_name* , kontynuując z każdą otaczającą przestrzenią nazw (jeśli istnieje) i kończącą się globalną przestrzenią nazw, następujące kroki są oceniane do momentu zlokalizowania jednostki:
   *  Jeśli `K` ma wartość zero, a `I` to nazwa przestrzeni nazw w `N`, wówczas:
      * Jeśli lokalizacja, w której występuje *simple_name* , jest ujęta w deklarację przestrzeni nazw dla `N`, a Deklaracja przestrzeni nazw zawiera *extern_alias_directive* lub *using_alias_directive* , która kojarzy nazwę `I` z przestrzenią nazw lub typem, wówczas *simple_name* jest niejednoznaczna i wystąpi błąd w czasie kompilacji.
      * W przeciwnym razie *simple_name* odwołuje się do przestrzeni nazw o nazwie `I` w `N`.
   *  W przeciwnym razie, jeśli `N` zawiera dostępny typ o nazwie `I` i `K`parametrów typu  , wówczas:
      * Jeśli `K` ma wartość zero, a lokalizacja, w której występuje *simple_name* , jest ujęta w deklarację przestrzeni nazw dla `N` a Deklaracja przestrzeni nazw zawiera *extern_alias_directive* lub *using_alias_directive* , który kojarzy nazwę `I` z przestrzenią nazw lub typem, wówczas *simple_name* jest niejednoznaczna i wystąpi błąd kompilacji.
      * W przeciwnym razie *namespace_or_type_name* odwołuje się do typu złożonego za pomocą podanych argumentów typu.
   *  W przeciwnym razie, jeśli lokalizacja, w której występuje *simple_name* , jest ujęta w deklarację przestrzeni nazw dla `N`:
      * Jeśli `K` ma wartość zero, a Deklaracja przestrzeni nazw zawiera *extern_alias_directive* lub *using_alias_directive* , które kojarzą nazwę `I` z zaimportowaną przestrzenią nazw lub typem, wówczas *simple_name* odwołuje się do tej przestrzeni nazw lub typu.
      * W przeciwnym razie, jeśli przestrzenie nazw i deklaracje typów zaimportowane przez *using_namespace_directive*s i *using_static_directive*s deklaracji przestrzeni nazw zawierają dokładnie jeden dostępny typ lub statyczny element członkowski, który ma nazwę `I` i `K` parametry typu, a następnie *simple_name* odwołuje się do tego typu lub elementu członkowskiego skonstruowanego za pomocą podanych argumentów typu.
      * W przeciwnym razie, jeśli przestrzenie nazw i typy zaimportowane przez *using_namespace_directive*s deklaracji przestrzeni nazw zawierają więcej niż jeden statyczny element członkowski typu lub nierozszerzającej metody o nazwie `I` i `K` parametrów typu, wówczas *simple_name* jest niejednoznaczna i wystąpi błąd.

   Należy zauważyć, że cały krok jest ściśle równoległy do odpowiedniego kroku w przetwarzaniu *namespace_or_type_name* ([nazw i typów](basic-concepts.md#namespace-and-type-names)).

*  W przeciwnym razie *simple_name* jest niezdefiniowana i wystąpi błąd w czasie kompilacji.


### <a name="parenthesized-expressions"></a>Wyrażenia w nawiasach

*Parenthesized_expression* składa się z *wyrażenia* ujętego w nawiasy.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

*Parenthesized_expression* jest oceniane przez obliczenie *wyrażenia* w nawiasach. Jeśli *wyrażenie* w nawiasach oznacza przestrzeń nazw lub typ, wystąpi błąd w czasie kompilacji. W przeciwnym razie wynik *parenthesized_expression* jest wynikiem obliczenia zawartego *wyrażenia*.

### <a name="member-access"></a>Dostęp do elementu członkowskiego

*Member_access* składa się z *primary_expression*, *predefined_type*lub *qualified_alias_member*, po którym następuje token "`.`", po którym następuje *Identyfikator*, opcjonalnie po którym następuje *type_argument_list*.

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

*Qualified_alias_member* produkcyjne jest definiowana w [kwalifikatorach aliasów przestrzeni nazw](namespaces.md#namespace-alias-qualifiers).

*Member_access* ma postać `E.I` lub formularza `E.I<A1, ..., Ak>`, gdzie `E` jest wyrażeniem podstawowym, `I` jest pojedynczym identyfikatorem, a `<A1, ..., Ak>` jest opcjonalną *type_argument_list*. Gdy nie określono *type_argument_list* , należy wziąć pod uwagę `K` wartość zero.

*Member_access* z *primary_expressionem* typu `dynamic` jest dynamicznie powiązany ([powiązanie dynamiczne](expressions.md#dynamic-binding)). W takim przypadku kompilator klasyfikuje dostęp do elementu członkowskiego jako dostęp do właściwości typu `dynamic`. Poniższe reguły do określenia znaczenia *member_access* są następnie stosowane w czasie wykonywania przy użyciu typu czasu wykonywania, a nie typu czasu kompilacji *primary_expression*. Jeśli ta Klasyfikacja czasu wykonywania prowadzi do grupy metod, dostęp do elementu członkowskiego musi być *primary_expression* *invocation_expression*.

*Member_access* jest oceniane i sklasyfikowane w następujący sposób:

*  Jeśli `K` ma wartość zero, a `E` jest przestrzenią nazw, a `E` zawiera zagnieżdżoną przestrzeń nazw o nazwie `I`, wynik jest tą przestrzenią nazw.
*  W przeciwnym razie, jeśli `E` jest przestrzenią nazw i `E` zawiera dostępny typ o nazwie `I` i `K` typ, a następnie wynik jest typem skonstruowanym z podanym argumentem typu.
*  Jeśli `E` jest *predefined_type* lub *primary_expression* sklasyfikowane jako typ, jeśli `E` nie jest parametrem typu, a w przypadku wyszukania elementu członkowskiego ([odnośnik elementu członkowskiego](expressions.md#member-lookup)) `I` w `E` z `K` parametry typu są generowane dopasowanie, `E.I` zostanie obliczone i sklasyfikowane w następujący sposób:
   *  Jeśli `I` identyfikuje typ, wynik jest typem skonstruowanym z podanym argumentem typu.
   *  Jeśli `I` identyfikuje jedną lub więcej metod, wynik jest grupą metod bez skojarzonego wyrażenia wystąpienia. Jeśli określono listę argumentów typu, jest ona używana w wywołaniu metody ogólnej ([wywołania metody](expressions.md#method-invocations)).
   *  Jeśli `I` identyfikuje właściwość `static`, wynik jest dostęp do właściwości bez skojarzonego wyrażenia wystąpienia.
   *  Jeśli `I` identyfikuje pole `static`:
      * Jeśli pole jest `readonly` i odwołanie występuje poza statycznym konstruktorem klasy lub struktury, w której jest zadeklarowane pole, wynik jest wartością, a mianowicie wartość pola statycznego `I` w `E`.
      * W przeciwnym razie wynik jest zmienną, a mianowicie pole statyczne `I` w `E`.
   *  Jeśli `I` identyfikuje zdarzenie `static`:
      * Jeśli odwołanie występuje w klasie lub strukturze, w której zdarzenie jest zadeklarowane, a zdarzenie zostało zadeklarowane bez *event_accessor_declarations* ([zdarzenia](classes.md#events)), `E.I` jest przetwarzane dokładnie tak, jakby `I` było polem statycznym.
      * W przeciwnym razie wynik jest dostęp do zdarzenia bez skojarzonego wyrażenia wystąpienia.
   *  Jeśli `I` identyfikuje stałą, wynik jest wartością, a mianowicie wartością tej stałej.
    * Jeśli `I` identyfikuje element członkowski wyliczenia, wynik jest wartością, a mianowicie wartością tego elementu członkowskiego wyliczenia.
    * W przeciwnym razie `E.I` jest nieprawidłowym odwołaniem do elementu członkowskiego i wystąpi błąd w czasie kompilacji.
*  Jeśli `E` jest dostęp do właściwości, indeksatora, zmienna lub wartość, typ `T`, a odnośnik elementu członkowskiego ([odnośnik elementów członkowskich](expressions.md#member-lookup)) `I` w `T` z `K`argumenty typu  da dopasowanie, a następnie `E.I` zostanie obliczone i sklasyfikowane w następujący sposób:
   *  Po pierwsze, jeśli `E` jest dostęp do właściwości lub indeksatora, zostanie uzyskana wartość dostępu właściwości lub indeksatora ([wartości wyrażeń](expressions.md#values-of-expressions)), a `E` zostanie przeliczona jako wartość.
   *  Jeśli `I` identyfikuje jedną lub więcej metod, wynik jest grupą metod ze skojarzonym wyrażeniem wystąpienia `E`. Jeśli określono listę argumentów typu, jest ona używana w wywołaniu metody ogólnej ([wywołania metody](expressions.md#method-invocations)).
   *  Jeśli `I` identyfikuje Właściwość wystąpienia,
      * Jeśli `E` jest `this`, `I` identyfikuje automatycznie implementowaną Właściwość ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)) bez metody ustawiającej, a odwołanie występuje w konstruktorze wystąpienia dla klasy lub typu struktury `T`, a następnie wynik to zmienna, a mianowicie ukryte pole zapasowe dla właściwości automatycznej podaną przez `I` w wystąpieniu `T` podanym przez `this`.
      * W przeciwnym razie wynik jest dostęp do właściwości ze skojarzonym wyrażeniem wystąpienia `E`.
   *  Jeśli `T` jest *class_type* i `I` identyfikuje pole wystąpienia tego *class_type*:
      * Jeśli wartość `E` jest `null`, zostanie zgłoszony `System.NullReferenceException`.
      * W przeciwnym razie, jeśli pole jest `readonly` i odwołanie występuje poza konstruktorem wystąpienia klasy, w której jest zadeklarowane pole, wynik jest wartością, a mianowicie wartość pola `I` w obiekcie, do którego odwołuje się `E`.
      * W przeciwnym razie wynik jest zmienną, a mianowicie pola `I` w obiekcie, do którego odwołuje się `E`.
   *  Jeśli `T` jest *struct_type* i `I` identyfikuje pole wystąpienia tego *struct_type*:
      * Jeśli `E` jest wartością lub jeśli pole jest `readonly` i odwołanie występuje poza konstruktorem wystąpienia struktury, w którym jest zadeklarowane pole, wynik jest wartością, a mianowicie wartość pola `I` w wystąpieniu struktury podanym przez `E`.
      * W przeciwnym razie wynik jest zmienną, a mianowicie pola `I` w wystąpieniu struktury podanym przez `E`.
   *  Jeśli `I` identyfikuje zdarzenie wystąpienia:
      * Jeśli odwołanie występuje w klasie lub strukturze, w której zdarzenie jest zadeklarowane, a zdarzenie zostało zadeklarowane bez *event_accessor_declarations* ([zdarzenia](classes.md#events)), a odwołanie nie występuje po lewej stronie operatora `+=` lub `-=`, a następnie `E.I` jest przetwarzane dokładnie tak, jakby `I` było polem wystąpienia.
      * W przeciwnym razie wynikiem jest dostęp do zdarzenia ze skojarzonym wyrażeniem wystąpienia `E`.
*  W przeciwnym razie podjęto próbę przetworzenia `E.I` jako wywołania metody rozszerzenia ([wywołania metody rozszerzenia](expressions.md#extension-method-invocations)). Jeśli to się nie powiedzie, `E.I` jest nieprawidłowym odwołaniem do elementu członkowskiego i wystąpi błąd w czasie powiązania.

#### <a name="identical-simple-names-and-type-names"></a>Identyczne nazwy proste i nazwy typów

W celu uzyskania dostępu do elementu członkowskiego w formularzu `E.I`, jeśli `E` jest pojedynczym identyfikatorem, a znaczenie `E` jako *simple_name* ([proste nazwy](expressions.md#simple-names)) to stała, pole, właściwość, zmienna lokalna lub parametr z tym samym typem, co znaczenie `E` jako *TYPE_NAME* ([nazwa przestrzeni nazw i typów](basic-concepts.md#namespace-and-type-names)), a następnie oba możliwe znaczenia `E` są dozwolone. Dwa możliwe znaczenia `E.I` nigdy nie są niejednoznaczne, ponieważ `I` musi być elementem członkowskim typu `E` w obu przypadkach. Innymi słowy, reguła po prostu zezwala na dostęp do statycznych elementów członkowskich i zagnieżdżonych typów `E`, w których wystąpił błąd kompilacji. Na przykład:
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a>Niejasności gramatyki

Produkcje dla *simple_name* ([prostych nazw](expressions.md#simple-names)) i *member_access* ([dostęp do elementów członkowskich](expressions.md#member-access)) mogą dać podstawę niejasności w gramatyki. Na przykład, instrukcja:
```csharp
F(G<A,B>(7));
```
może być interpretowany jako wywołanie `F` z dwoma argumentami, `G < A` i `B > (7)`. Alternatywnie, można go interpretować jako wywołanie do `F` z jednym argumentem, który jest wywołaniem metody generycznej `G` z dwoma argumentami typu i jednym argumentem regularnym.

Jeśli sekwencja tokenów może być analizowana (w kontekście) jako *simple_name* ([proste nazwy](expressions.md#simple-names)), *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) lub *pointer_member_access* ([wskaźnik dostępu do składowych](unsafe-code.md#pointer-member-access)) kończące się *type_argument_list* ([argumentami typu](types.md#type-arguments)), zostaje sprawdzony token bezpośrednio po zamykającym tokenie `>`. Jeśli jest to jedna z
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
następnie *type_argument_list* jest zachowywany jako część *simple_name*, *member_access* lub *pointer_member_access* i wszelkie inne możliwe przeanalizowanie sekwencji tokenów jest odrzucane. W przeciwnym razie *type_argument_list* nie jest uważana za część *simple_name*, *member_access* lub *pointer_member_access*, nawet jeśli nie istnieje żadna inna możliwa analiza sekwencji tokenów. Należy pamiętać, że te reguły nie są stosowane podczas analizowania *type_argument_list* w *namespace_or_type_name* ([nazwy przestrzeni nazw i typów](basic-concepts.md#namespace-and-type-names)). Instrukcja
```csharp
F(G<A,B>(7));
```
zgodnie z tą regułą, należy interpretować jako wywołanie `F` z jednym argumentem, który jest wywołaniem metody generycznej `G` z dwoma argumentami typu i jednym argumentem regularnym. Instrukcje
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
Każdy z nich będzie interpretowany jako wywołanie `F` z dwoma argumentami. Instrukcja
```csharp
x = F < A > +y;
```
będzie interpretowany jako operator mniejszości, operator większości, i jednoargumentowy operator plus, tak jakby instrukcja była zapisywana `x = (F < A) > (+y)`, a nie jako *simple_name* z *type_argument_list* po którym następuje operator binarny Plus. W instrukcji
```csharp
x = y is C<T> + z;
```
tokeny `C<T>` są interpretowane jako *namespace_or_type_name* z *type_argument_list*.

### <a name="invocation-expressions"></a>Wyrażenia wywołania

*Invocation_expression* jest używany do wywołania metody.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

*Invocation_expression* jest powiązany dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)), jeśli co najmniej jedna z następujących posiada:

* *Primary_expression* ma `dynamic`typu czasu kompilacji.
* Co najmniej jeden argument opcjonalnej *argument_list* ma typ czasu kompilacji `dynamic`, a *primary_expression* nie ma typu delegata.

W takim przypadku kompilator klasyfikuje *invocation_expression* jako wartość typu `dynamic`. Poniższe reguły do określenia znaczenia *invocation_expression* są następnie stosowane w czasie wykonywania, przy użyciu typu czasu wykonywania, a nie typu czasu kompilacji dla *primary_expression* i argumentów, które mają `dynamic`typu czasu kompilacji. Jeśli *primary_expression* nie ma `dynamic`typu czasu kompilacji, wywołanie metody przejdzie do ograniczonego sprawdzenia czasu kompilacji zgodnie z opisem w [sprawdzaniu poprawności dynamicznego rozpoznawania przeciążenia w czasie kompilacji](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

*Primary_expression* *invocation_expression* musi być grupą metod lub wartością *delegate_type*. Jeśli *primary_expression* jest grupą metod, *invocation_expression* jest wywołaniem metody ([wywołania metody](expressions.md#method-invocations)). Jeśli *primary_expression* jest wartością *delegate_type*, *invocation_expression* jest wywołaniem delegata ([Delegaty wywołań](expressions.md#delegate-invocations)). Jeśli *primary_expression* nie jest grupą metod ani wartością *delegate_type*, wystąpi błąd w czasie trwania powiązania.

Opcjonalne *argument_list* ([listy argumentów](expressions.md#argument-lists)) zawierają wartości lub odwołania do zmiennych dla parametrów metody.

Wyniki oceny *invocation_expression* są klasyfikowane w następujący sposób:

*  Jeśli *invocation_expression* wywołuje metodę lub delegata, która zwraca `void`, wynik nie będzie miał wartości Nothing. Wyrażenie sklasyfikowane jako Nothing nie jest dozwolone tylko w kontekście *statement_expression* ([instrukcji wyrażenia](statements.md#expression-statements)) ani jako treści *lambda_expression* ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)). W przeciwnym razie wystąpi błąd w czasie powiązania.
*  W przeciwnym razie wynik jest wartością typu zwracanego przez metodę lub delegat.

#### <a name="method-invocations"></a>Wywołania metody

W przypadku wywołania metody *primary_expression* *invocation_expression* musi być grupą metod. Grupa metod identyfikuje jedną metodę do wywołania lub zestaw przeciążonych metod, z których można wybrać konkretną metodę do wywołania. W tym ostatnim przypadku określenie konkretnej metody do wywołania jest zależne od kontekstu dostarczonego przez typy argumentów w *argument_list*.

Przetwarzanie w czasie powiązania metody wywołania metod `M(A)`, gdzie `M` jest grupą metod (może także dotyczyć *type_argument_list*), a `A` jest opcjonalne *argument_list*, składa się z następujących kroków:

*  Zestaw metod kandydujących dla wywołania metody jest konstruowany. Dla każdej metody `F` skojarzonej z grupą metod `M`:
   *  Jeśli `F` nie jest ogólny, `F` jest kandydatem, gdy:
      * `M` nie ma listy argumentów typu i
      * `F` ma zastosowanie w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)).
   *  Jeśli `F` jest ogólny i `M` nie ma listy argumentów typu, `F` jest kandydatem, gdy:
      * Wnioskowanie o typie ([wnioskowanie o typie](expressions.md#type-inference)) kończy się powodzeniem, wnioskowanie listy argumentów typu dla wywołania i
      * Gdy argumenty typu wywnioskowane zostaną zastąpione odpowiednimi parametrami typu metody, wszystkie skompilowane typy na liście parametrów F spełniają ich ograniczenia ([spełniające warunki ograniczające](types.md#satisfying-constraints)), a lista parametrów `F` ma zastosowanie w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)).
   *  Jeśli `F` jest ogólny i `M` zawiera listę argumentów typu, `F` jest kandydatem, gdy:
      * `F` ma taką samą liczbę parametrów typu metody jak podano na liście argumentów typu.
      * Gdy argumenty typu są zastępowane odpowiednimi parametrami typu metody, wszystkie skompilowane typy na liście parametrów F spełniają ich ograniczenia ([spełniające ograniczenia](types.md#satisfying-constraints)), a lista parametrów `F` ma zastosowanie w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)).
*  Zestaw metod kandydujących jest zredukowany, aby zawierał tylko metody z najbardziej pochodnych typów: dla każdej metody `C.F` w zestawie, gdzie `C` jest typem, w którym Metoda `F` jest zadeklarowana, wszystkie metody zadeklarowane w typie podstawowym `C` są usuwane z zestawu. Ponadto jeśli `C` jest typem klasy innym niż `object`, wszystkie metody zadeklarowane w typie interfejsu są usuwane z zestawu. (Ta ostatnia reguła ma wpływ tylko wtedy, gdy grupa metod była wynikiem przeszukiwania elementu członkowskiego w parametrze typu mającym obowiązującą klasę bazową inną niż obiekt i niepusty zestaw obowiązujących interfejsów).
*  Jeśli wynikający z tego zestaw metod kandydujących jest pusty, dalsze przetwarzanie w poniższych krokach zostanie porzucone, a zamiast tego zostanie podjęta próba przetworzenia wywołania jako wywołania metody rozszerzenia ([wywołania metody rozszerzenia](expressions.md#extension-method-invocations)). Jeśli to się nie powiedzie, żadne odpowiednie metody nie istnieją i wystąpi błąd w czasie powiązania.
*  Najlepsza Metoda zestawu metod kandydujących jest identyfikowana przy użyciu [reguł rozpoznawania przeciążenia.](expressions.md#overload-resolution) Jeśli nie można zidentyfikować pojedynczej metody, wywołanie metody jest niejednoznaczne i występuje błąd w czasie powiązania. Podczas rozpoznawania przeciążenia parametry metody ogólnej są brane pod uwagę po podstawianiu argumentów typu (dostarczonych lub wywnioskowanych) dla odpowiednich parametrów typu metody.
*  Zostanie wykonana ostateczna weryfikacja wybranej najlepszej metody:
   * Metoda jest weryfikowana w kontekście grupy metod: Jeśli najlepsza metoda jest metodą statyczną, Grupa metod musi mieć wynik *simple_name* lub *member_access* za pośrednictwem typu. Jeśli najlepsza metoda jest metodą wystąpienia, Grupa metod musi mieć wynik *simple_name*, *member_access* za pomocą zmiennej lub wartości lub *base_access*. Jeśli żaden z tych wymagań nie ma wartości true, wystąpi błąd w czasie trwania powiązania.
   * Jeśli najlepsza metoda jest metodą rodzajową, argumenty typu (dostarczone lub wywnioskowane) są sprawdzane względem ograniczeń ([spełnianie ograniczeń](types.md#satisfying-constraints)) zadeklarowanych dla metody ogólnej. Jeśli którykolwiek z argumentów typu nie spełnia odpowiednich ograniczeń w parametrze typu, wystąpi błąd w czasie powiązania.

Po wybraniu metody i sprawdzeniu jej pod kątem czasu wiązania przez powyższe kroki, rzeczywiste wywołanie w czasie wykonywania jest przetwarzane zgodnie z regułami wywołania elementu członkowskiego, które opisano w [sprawdzeniu czasu kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Intuicyjny efekt opisanych powyżej reguł rozpoznawania jest następujący: aby zlokalizować konkretną metodę wywoływaną przez wywołanie metody, należy rozpocząć od typu wskazanego przez wywołanie metody i przetworzyć łańcuch dziedziczenia do momentu, gdy ma to zastosowanie do co najmniej jednego, znaleziono dostępną deklarację metody niezastępującej. Następnie należy wykonać wnioskowanie o typie i metodę rozpoznawania przeciążenia dla zestawu odpowiednich, dostępnych, zastąpień metod zadeklarowanych w tym typie i wywoływać metody w ten sposób. Jeśli żadna metoda nie została znaleziona, spróbuj zamiast tego przetworzyć wywołanie jako wywołanie metody rozszerzenia.

#### <a name="extension-method-invocations"></a>Wywołania metody rozszerzenia

W wywołaniu metody ([wywołania w zapakowanych wystąpieniach](expressions.md#invocations-on-boxed-instances)) jednego z formularzy
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
Jeśli normalne przetwarzanie wywołania nie odnajdzie odpowiednich metod, podejmowana jest próba przetworzenia konstrukcji jako wywołania metody rozszerzenia. Jeśli *wyrażenie* lub którykolwiek z *argumentów* ma `dynamic`typu czasu kompilacji, metody rozszerzające nie będą miały zastosowania.

Celem jest znalezienie najlepszego *type_name* `C`, dzięki czemu można wykonać odpowiednie wywołanie metody statycznej:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

Metoda rozszerzająca `Ci.Mj` ***kwalifikuje*** się, jeśli:

*  `Ci` jest klasą nieogólną, niezagnieżdżoną
*  Nazwa `Mj` jest *identyfikatorem*
*  `Mj` jest dostępne i stosowane w przypadku zastosowania do argumentów jako metody statycznej, jak pokazano powyżej
*  Niejawna tożsamość, odwołanie lub zamiana opakowania istnieje z *wyrażenia* do typu pierwszego parametru `Mj`.

Wyszukiwanie `C` odbywa się w następujący sposób:

*  Począwszy od najbliższej deklaracji przestrzeni nazw, kontynuując każdą zachodzącą deklarację przestrzeni nazw i kończącą się jednostką kompilacji, kolejne próby są wykonywane w celu znalezienia kandydującego zestawu metod rozszerzających:
   * Jeśli dana przestrzeń nazw lub jednostka kompilacji bezpośrednio zawiera deklaracje typu nieogólnego `Ci` z uprawnionymi metodami rozszerzenia `Mj`, zestaw tych metod rozszerzających jest zestaw kandydatów.
   * Jeśli typy `Ci` zaimportowane przez *using_static_declarations* i bezpośrednio zadeklarowane w przestrzeniach nazw zaimportowanych przez *using_namespace_directive*s w danej przestrzeni nazw lub jednostce kompilacji, bezpośrednio zawierają kwalifikujące się metody rozszerzenia `Mj`, wówczas zestaw tych metod rozszerzających jest zestaw kandydatów.
*  Jeśli w żadnej deklaracji przestrzeni nazw lub jednostce kompilacji nie zostanie znaleziony zestaw kandydatów, wystąpi błąd w czasie kompilacji.
*  W przeciwnym razie Rozpoznanie przeciążenia jest stosowane do zestawu kandydatów zgodnie z opisem w ([rozpoznawanie przeciążenia](expressions.md#overload-resolution)). Jeśli nie zostanie znaleziona żadna Najlepsza Metoda, wystąpi błąd w czasie kompilacji.
*  `C` jest typem, w ramach którego Najlepsza Metoda jest zadeklarowana jako Metoda rozszerzenia.

Przy użyciu `C` jako obiektu docelowego wywołanie metody jest następnie przetwarzane jako wywołanie metody statycznej (sprawdzanie w[czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Powyższe zasady oznaczają, że metody wystąpień mają pierwszeństwo przed metodami rozszerzania, te metody rozszerzające dostępne w deklaracji wewnętrznej przestrzeni nazw mają pierwszeństwo przed metodami rozszerzenia dostępnymi w deklaracjach zewnętrznych przestrzeni nazw i tym rozszerzeniu Metody zadeklarowane bezpośrednio w przestrzeni nazw mają pierwszeństwo przed metodami rozszerzenia importowanymi do tej samej przestrzeni nazw za pomocą dyrektywy using Namespace. Na przykład:
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

W przykładzie metoda `B`ma pierwszeństwo przed pierwszą metodą rozszerzenia, a metoda `C`ma pierwszeństwo przed obiema metodami rozszerzenia.

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

Dane wyjściowe tego przykładu to:
```console
E.F(1)
D.G(2)
C.H(3)
```
`D.G` ma pierwszeństwo przed `C.G`, a `E.F` ma pierwszeństwo przed `D.F` i `C.F`.

#### <a name="delegate-invocations"></a>Delegowanie wywołań

Dla wywołania delegata *primary_expression* *invocation_expression* musi być wartością *delegate_type*. Co więcej, biorąc pod uwagę *delegate_type* być elementem członkowskim funkcji z tą samą listą parametrów co *delegate_type*, *Delegate_type* musi mieć zastosowanie ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)) w odniesieniu do *argument_list* *invocation_expression*.

Przetwarzanie w czasie wykonywania delegata wywołania formularza `D(A)`, gdzie `D` jest *primary_expression* *delegate_type* i `A` jest opcjonalne *argument_list*, składa się z następujących kroków:

*  `D` jest oceniane. Jeśli ta Ocena spowoduje wyjątek, nie są wykonywane żadne dalsze kroki.
*  Wartość `D` jest sprawdzana jako prawidłowa. Jeśli wartość `D` jest `null`, zostanie zgłoszony `System.NullReferenceException` i nie zostaną wykonane żadne dalsze kroki.
*  W przeciwnym razie `D` jest odwołaniem do wystąpienia delegata. Wywołania elementu członkowskiego funkcji ([Sprawdzanie w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) są wykonywane na każdej z wywoływanych jednostek na liście wywołań delegata. W przypadku wywoływanych jednostek składających się z wystąpienia i metody wystąpienia wystąpienie dla wywołania jest wystąpieniem zawartym w jednostce, którą można wywołać.

### <a name="element-access"></a>Dostęp do elementu

*Element_access* składa się z *primary_no_array_creation_expression*, po którym następuje token "`[`", po którym następuje *argument_list*, po którym następuje token "`]`". *Argument_list* składa się z jednego lub więcej *argumentów*s, rozdzielonych przecinkami.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

*Argument_list* *element_access* nie może zawierać argumentów `ref` lub `out`.

*Element_access* jest powiązany dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)), jeśli co najmniej jedna z następujących posiada:

* *Primary_no_array_creation_expression* ma `dynamic`typu czasu kompilacji.
* Co najmniej jedno wyrażenie *argument_list* ma typ czasu kompilacji `dynamic`, a *primary_no_array_creation_expression* nie ma typu tablicy.

W takim przypadku kompilator klasyfikuje *element_access* jako wartość typu `dynamic`. Poniższe reguły do określenia znaczenia *element_access* są następnie stosowane w czasie wykonywania, przy użyciu typu czasu wykonywania, a nie typu czasu kompilacji dla *primary_no_array_creation_expression* i *argument_list* wyrażeń, które mają `dynamic`typu czasu kompilacji. Jeśli *primary_no_array_creation_expression* nie ma `dynamic`typu czasu kompilacji, dostęp do elementu jest poddawany ograniczonemu sprawdzeniu czasu kompilacji, zgodnie z opisem w [sprawdzeniu poprawności dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Jeśli *primary_no_array_creation_expression* *element_access* jest wartością *array_type*, *element_access* jest dostęp do tablicy ([dostęp do tablicy](expressions.md#array-access)). W przeciwnym razie *primary_no_array_creation_expression* musi być zmienną lub wartością typu klasy, struktury lub interfejsu, który ma co najmniej jeden element członkowski indeksatora, w tym przypadku *element_access* jest dostępem indeksatora ([dostęp do indeksatora](expressions.md#indexer-access)).

#### <a name="array-access"></a>Dostęp do tablicy

W przypadku dostępu do tablicy *primary_no_array_creation_expression* *element_access* musi być wartością *array_type*. Ponadto *argument_list* dostępu do tablicy nie może zawierać nazwanych argumentów. Liczba wyrażeń w *argument_list* musi być taka sama jak ranga *array_type*, a każde wyrażenie musi być typu `int`, `uint`, `long`, `ulong`lub musi być niejawnie konwertowane na jeden lub więcej z tych typów.

Wynikiem oceny dostępu do tablicy jest zmienna typu elementu tablicy, a mianowicie element tablicy wybrany przez wartości w wyrażeniach w *argument_listach*...

Przetwarzanie dostępu do tablicy w czasie wykonywania `P[A]`, gdzie `P` jest *primary_no_array_creation_expression* *array_type* , a `A` to *argument_list*, składa się z następujących kroków:

*  `P` jest oceniane. Jeśli ta Ocena spowoduje wyjątek, nie są wykonywane żadne dalsze kroki.
*  Wyrażenia indeksu *argument_list* są oceniane w kolejności, od lewej do prawej. Po dokonaniu oceny każdego wyrażenia indeksu niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) do jednego z następujących typów jest wykonywana: `int`, `uint`, `long``ulong`. Pierwszy typ na tej liście, dla którego istnieje niejawna konwersja, jest wybierany. Na przykład jeśli wyrażenie index jest typu `short`, to niejawna konwersja na `int` jest wykonywana, ponieważ możliwe jest niejawne konwersje z `short` do `int` i z `short` `long`. Jeśli Obliczanie wyrażenia indeksu lub kolejnej niejawnej konwersji powoduje wyjątek, nie są oceniane żadne dalsze wyrażenia indeksu i nie są wykonywane żadne dalsze kroki.
*  Wartość `P` jest sprawdzana jako prawidłowa. Jeśli wartość `P` jest `null`, zostanie zgłoszony `System.NullReferenceException` i nie zostaną wykonane żadne dalsze kroki.
*  Wartość każdego wyrażenia w *argument_list* jest sprawdzana względem rzeczywistych granic każdego wymiaru wystąpienia tablicy, do którego odwołuje się `P`. Jeśli co najmniej jedna wartość znajduje się poza zakresem, zgłaszany jest `System.IndexOutOfRangeException` i nie są wykonywane żadne dalsze kroki.
*  Obliczono lokalizację elementu tablicy podaną przez wyrażenia indeksu, a ta lokalizacja jest wynikiem dostępu do tablicy.

#### <a name="indexer-access"></a>Dostęp indeksatora

W przypadku dostępu indeksatora *primary_no_array_creation_expression* *element_access* musi być zmienną lub wartością typu klasy, struktury lub interfejsu, a ten typ musi implementować jeden lub więcej indeksatorów, które są stosowane w odniesieniu do *argument_list* *element_access*.

Przetwarzanie w czasie wiązania dostępu indeksatora `P[A]`, gdzie `P` jest *primary_no_array_creation_expressionem* klasy, struktury lub typu interfejsu `T`, a `A` to *argument_list*, składa się z następujących kroków:

*  Zestaw indeksatorów udostępnianych przez `T` jest skonstruowany. Zestaw składa się z wszystkich indeksatorów zadeklarowanych w `T` lub typu podstawowego `T`, które nie są `override` deklaracji i są dostępne w bieżącym kontekście ([dostęp do elementu członkowskiego](basic-concepts.md#member-access)).
*  Zestaw jest zredukowany do tych indeksatorów, które mają zastosowanie i nie są ukryte przez inne indeksatory. Poniższe reguły są stosowane do każdego indeksatora `S.I` w zestawie, gdzie `S` jest typem, w którym zadeklarowano `I` indeksatora:
   * Jeśli `I` nie ma zastosowania w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)), `I` jest usuwany z zestawu.
   * Jeśli `I` ma zastosowanie w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)), wszystkie indeksatory zadeklarowane w typie podstawowym `S` są usuwane z zestawu.
   * Jeśli `I` ma zastosowanie w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)), a `S` jest typem klasy innym niż `object`, wszystkie indeksatory zadeklarowane w interfejsie są usuwane z zestawu.
*  Jeśli zestaw wyników indeksatorów jest pusty, nie ma żadnych odpowiednich indeksatorów i wystąpi błąd w czasie powiązania.
*  Najlepszy indeksator zestawu indeksatorów kandydujących jest identyfikowany przy użyciu [reguł rozpoznawania przeciążenia.](expressions.md#overload-resolution) Jeśli nie można zidentyfikować pojedynczego najlepszego indeksatora, dostęp indeksatora jest niejednoznaczny i wystąpi błąd w czasie powiązania.
*  Wyrażenia indeksu *argument_list* są oceniane w kolejności, od lewej do prawej. Wynikiem przetwarzania dostępu indeksatora jest wyrażenie sklasyfikowane jako dostęp indeksatora. Wyrażenie dostępu indeksatora odwołuje się do indeksatora określonego w powyższym kroku i ma skojarzone wyrażenie wystąpienia `P` i skojarzoną listę argumentów `A`.

W zależności od kontekstu, w którym jest używany, dostęp indeksatora powoduje wywołanie *metody dostępu get* lub *set metody dostępu* indeksatora. Jeśli dostęp indeksatora jest elementem docelowym przypisania, *metoda dostępu set* jest wywoływana, aby przypisać nową wartość ([przypisanie proste](expressions.md#simple-assignment)). We wszystkich innych przypadkach *metoda dostępu get* jest wywoływana w celu uzyskania bieżącej wartości ([wartości wyrażeń](expressions.md#values-of-expressions)).

### <a name="this-access"></a>Ten dostęp

*This_access* składa się z zastrzeżonego słowa `this`.

```antlr
this_access
    : 'this'
    ;
```

*This_access* jest dozwolony tylko w *bloku* konstruktora wystąpienia, metody wystąpienia lub akcesora wystąpienia. Ma jedną z następujących znaczenia:

*  Gdy `this` jest używana w *primary_expression* w konstruktorze wystąpienia klasy, zostanie sklasyfikowana jako wartość. Typ wartości to typ wystąpienia ([Typ wystąpienia](classes.md#the-instance-type)) klasy, w której występuje użycie, a wartość to odwołanie do konstruowanego obiektu.
*  Gdy `this` jest używana w *primary_expression* w metodzie wystąpienia lub metodach dostępu do wystąpienia klasy, jest klasyfikowana jako wartość. Typ wartości to typ wystąpienia ([Typ wystąpienia](classes.md#the-instance-type)) klasy, w której występuje użycie, a wartość to odwołanie do obiektu, dla którego wywołana została metoda lub akcesor.
*  Gdy `this` jest używany w *primary_expression* w konstruktorze wystąpienia struktury, jest on klasyfikowany jako zmienna. Typ zmiennej to typ wystąpienia ([Typ wystąpienia](classes.md#the-instance-type)) struktury, w której występuje użycie, a zmienna reprezentuje strukturę konstruowaną. Zmienna `this` konstruktora wystąpienia struktury zachowuje się dokładnie tak samo jak parametr `out` typu struct — w szczególności oznacza to, że zmienna musi być ostatecznie przypisana w każdej ścieżce wykonywania konstruktora wystąpień.
*  Gdy `this` jest używana w *primary_expression* w metodzie wystąpienia lub metodach dostępu do wystąpienia struktury, jest klasyfikowana jako zmienna. Typ zmiennej to typ wystąpienia ([Typ wystąpienia](classes.md#the-instance-type)) struktury, w której występuje użycie.
   * Jeśli metoda lub akcesor nie jest iteratorem ([iteratorów](classes.md#iterators)), zmienna `this` reprezentuje strukturę, dla której wywołano metodę lub akcesor, i zachowuje się dokładnie tak samo jak parametr `ref` typu struktury.
   * Jeśli metoda lub akcesor jest iteratorem, zmienna `this` reprezentuje kopię struktury, dla której wywołano metodę lub akcesor, i zachowuje się dokładnie tak samo jak parametr wartości typu struktury.

Użycie `this` w *primary_expression* w kontekście innym niż wymienione powyżej jest błędem czasu kompilacji. W szczególności nie jest możliwe odwoływanie się do `this` w metodzie statycznej, metodzie dostępu do właściwości statycznych lub w *variable_initializer* deklaracji pola.

### <a name="base-access"></a>Dostęp podstawowy

*Base_access* składa się z zastrzeżonego słowa `base` po którym następuje token "`.`" i identyfikator lub *argument_list* ujęte w nawiasy kwadratowe:

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

*Base_access* służy do uzyskiwania dostępu do składowych klasy bazowej, które są ukryte przez podobne nazwy członków w bieżącej klasie lub strukturze. *Base_access* jest dozwolony tylko w *bloku* konstruktora wystąpienia, metody wystąpienia lub akcesora wystąpienia. Gdy `base.I` występuje w klasie lub strukturze, `I` musi zwrócić element członkowski klasy podstawowej tej klasy lub struktury. Podobnie, gdy `base[E]` występuje w klasie, odpowiedni indeksator musi istnieć w klasie bazowej.

W czasie trwania powiązania *base_access* wyrażenia `base.I` formularzy i `base[E]` są oceniane dokładnie tak, jakby były zapisywane `((B)this).I` i `((B)this)[E]`, gdzie `B` jest klasą bazową klasy lub struktury, w której występuje konstrukcja. W ten sposób `base.I` i `base[E]` odpowiada `this.I` i `this[E]`, z wyjątkiem tego, że `this` jest wyświetlany jako wystąpienie klasy bazowej.

Gdy *base_access* odwołuje się do elementu członkowskiego funkcji wirtualnej (metody, właściwości lub indeksatora), określenie, który element członkowski funkcji ma zostać wywołany w czasie wykonywania ([Sprawdzanie w czasie kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) jest zmieniany. Wywoływany element członkowski funkcji jest określany przez znalezienie najbardziej pochodnej implementacji ([metod wirtualnych](classes.md#virtual-methods)) elementu członkowskiego funkcji w odniesieniu do `B` (zamiast w odniesieniu do typu czasu wykonywania `this`, tak jak zwykle w przypadku dostępu niebazowego). W tym celu w `override` elementu członkowskiego funkcji `virtual` *base_access* może służyć do wywołania dziedziczonej implementacji elementu członkowskiego funkcji. Jeśli element członkowski funkcji, do którego odwołuje się *base_access* , jest abstrakcyjny, wystąpi błąd w czasie powiązania.

### <a name="postfix-increment-and-decrement-operators"></a>Operatory przyrostka i zmniejszenie

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

Operand przyrostkowej operacji zwiększania lub zmniejszania musi być wyrażeniem sklasyfikowanym jako zmienna, dostęp do właściwości lub dostęp indeksatora. Wynik operacji jest wartością tego samego typu co argument operacji.

Jeśli *primary_expression* ma typ czasu kompilacji `dynamic` następnie operator jest powiązany dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)), *post_increment_expression* lub *post_decrement_expression* ma typ czasu kompilacji `dynamic` i w czasie wykonywania są stosowane następujące reguły z typem czasu wykonywania *primary_expression*.

Jeśli operand przyrostowej operacji zwiększania lub zmniejszania jest dostępną właściwością lub indeksatorem, właściwość lub indeksator musi mieć zarówno `get`, jak i `set`. W przeciwnym razie wystąpi błąd w czasie powiązania.

Metoda rozpoznawania przeciążenia operatora jednoargumentowego (Metoda[rozpoznawania przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) została zastosowana w celu wybrania implementacji określonego operatora. Wstępnie zdefiniowane operatory `++` i `--` istnieją dla następujących typów: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`i dowolnego typu wyliczeniowego. Wstępnie zdefiniowane operatory `++` zwracają wartość wygenerowaną przez dodanie 1 do operandu, a wstępnie zdefiniowane operatory `--` zwracają wartość wygenerowaną przez odjęcie 1 od operandu. W kontekście `checked`, jeśli wynik tego dodania lub odejmowania znajduje się poza zakresem typu wyników, a typ wyniku jest typem całkowitym lub typem wyliczeniowym, generowany jest `System.OverflowException`.

Przetwarzanie w czasie wykonywania przyrostkowej przyrostowej lub zmniejszania operacji w postaci `x++` lub `x--` składa się z następujących kroków:

*   Jeśli `x` jest sklasyfikowana jako zmienna:
    * `x` jest oceniane, aby utworzyć zmienną.
    * Wartość `x` jest zapisywana.
    * Wybrany operator jest wywoływany z zapisaną wartością `x` jako argument.
    * Wartość zwrócona przez operator jest przechowywana w lokalizacji podawanej przez ocenę `x`.
    * Zapisana wartość `x` jest wynikiem operacji.
*   Jeśli `x` jest klasyfikowany jako dostęp do właściwości lub indeksatora:
    * Wyrażenie wystąpienia (jeśli `x` nie jest `static`) i lista argumentów (jeśli `x` jest dostępem indeksatora), które są skojarzone z `x` są oceniane, a wyniki są używane w kolejnych `get`ach i `set` metodach dostępu.
    * Metoda dostępu `get` `x` jest wywoływana, a zwrócona wartość jest zapisywana.
    * Wybrany operator jest wywoływany z zapisaną wartością `x` jako argument.
    * Metoda dostępu `set` `x` jest wywoływana z wartością zwracaną przez operator jako argument `value`.
    * Zapisana wartość `x` jest wynikiem operacji.

Operatory `++` i `--` obsługują również notację prefiksu ([Operatory przyrostu i zmniejszania prefiksu](expressions.md#prefix-increment-and-decrement-operators)). Zwykle wynik `x++` lub `x--` jest wartością `x` przed operacją, natomiast wynik `++x` lub `--x` jest wartością `x` po operacji. W obu przypadkach `x` sama ma taką samą wartość po operacji.

Implementację `operator ++` lub `operator --` można wywołać przy użyciu notacji przyrostkowej lub prefiksowej. Nie można mieć odrębnych implementacji operatora dla dwóch notacji.

### <a name="the-new-operator"></a>Operator new

Operator `new` jest używany do tworzenia nowych wystąpień typów.

Istnieją trzy formy `new` wyrażeń:

*  Wyrażenia tworzenia obiektów są używane do tworzenia nowych wystąpień typów klas i typów wartości.
*  Wyrażenia tworzenia tablic są używane do tworzenia nowych wystąpień typów tablicowych.
*  Wyrażenia tworzenia delegatów są używane do tworzenia nowych wystąpień typów delegatów.

Operator `new` sugeruje utworzenie wystąpienia typu, ale niekoniecznie ma dynamiczną alokację pamięci. W szczególności wystąpienia typów wartości nie wymagają dodatkowej pamięci poza zmiennymi, w których się znajdują, i nie występują dynamiczne alokacje, gdy `new` jest używany do tworzenia wystąpień typów wartości.

#### <a name="object-creation-expressions"></a>Wyrażenia tworzenia obiektów

*Object_creation_expression* jest używany do tworzenia nowego wystąpienia *class_type* lub *value_type*.

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

*Typ* *object_creation_expression* musi być *class_type*, *value_type* lub *type_parameter*. *Typ* nie może być `abstract` *class_type*.

Opcjonalne *argument_list* ([listy argumentów](expressions.md#argument-lists)) są dozwolone tylko wtedy, gdy *Typ* jest *class_type* lub *struct_type*.

Wyrażenie tworzenia obiektu może pominąć listę argumentów konstruktora i otaczające nawiasy, pod warunkiem że zawiera Inicjator obiektu lub inicjator kolekcji. Pominięcie listy argumentów konstruktora i otaczające nawiasy są równoważne do określenia pustej listy argumentów.

Przetwarzanie wyrażenia tworzenia obiektu, które obejmuje inicjatora obiektów lub inicjator kolekcji, składa się z pierwszego przetwarzania konstruktora wystąpienia, a następnie przetwarzania inicjowania elementu członkowskiego lub elementu określonego przez inicjator obiektów (inicjatory[obiektów](expressions.md#object-initializers)) lub inicjator kolekcji (inicjatory[kolekcji](expressions.md#collection-initializers)).

Jeśli którykolwiek z argumentów w opcjonalnym *argument_list* ma typ czasu kompilowania `dynamic`, *object_creation_expression* jest powiązana dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)), a w czasie wykonywania są stosowane następujące reguły przy użyciu typu czasu wykonywania tych argumentów *argument_list* z typem czasu kompilacji `dynamic`. Jednak tworzenie obiektów jest ograniczone przez ograniczony czas kompilacji, zgodnie z opisem w [sprawdzaniu poprawności dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Przetwarzanie powiązania *object_creation_expression* formularza `new T(A)`, gdzie `T` jest *class_type* lub *value_type* , a `A` jest opcjonalne *argument_list*, składa się z następujących kroków:

*   Jeśli `T` jest *value_type* i `A` nie istnieje:
    * *Object_creation_expression* jest domyślnym wywołaniem konstruktora. Wynik *object_creation_expression* jest wartością typu `T`, a mianowicie wartością domyślną dla `T` zgodnie z definicją w [typie System. ValueType](types.md#the-systemvaluetype-type).
*   W przeciwnym razie, jeśli `T` jest *type_parameter* i `A` nie istnieje:
    * Jeśli dla `T`nie określono ograniczenia typu wartości lub ograniczenia konstruktora ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), wystąpi błąd w czasie trwania powiązania.
    * Wynik *object_creation_expression* jest wartością typu czasu wykonywania, z którym jest powiązany parametr typu, a mianowicie wynik wywołania domyślnego konstruktora tego typu. Typ czasu wykonywania może być typem referencyjnym lub typem wartości.
*   W przeciwnym razie, jeśli `T` jest *class_type* lub *struct_type*:
    * Jeśli `T` jest *class_type*`abstract`, wystąpi błąd w czasie kompilacji.
    * Konstruktor wystąpienia do wywołania jest określany przy użyciu reguł rozdzielczości przeciążenia przeciążania [przeciążenia](expressions.md#overload-resolution). Zestaw konstruktorów wystąpień kandydatów składa się z wszystkich dostępnych konstruktorów wystąpień zadeklarowanych w `T`, które mają zastosowanie w odniesieniu do `A` ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)). Jeśli zestaw konstruktorów wystąpień kandydatów jest pusty lub nie można zidentyfikować pojedynczego konstruktora najlepszego wystąpienia, wystąpi błąd w czasie powiązania.
    * Wynik *object_creation_expression* jest wartością typu `T`, a mianowicie wartość wygenerowaną przez wywołanie konstruktora wystąpienia określonego w powyższym kroku.
*  W przeciwnym razie *object_creation_expression* jest nieprawidłowy i wystąpi błąd w czasie powiązania.

Nawet jeśli *object_creation_expression* jest powiązana dynamicznie, typ czasu kompilacji nadal jest `T`.

W czasie wykonywania *object_creation_expression* formularza `new T(A)`, gdzie `T` jest *class_type* lub *struct_type* , a `A` jest opcjonalne *argument_list*, składa się z następujących kroków:

*   Jeśli `T` jest *class_type*:
    * Przydzielono nowe wystąpienie klasy `T`. Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia, zostanie zgłoszony `System.OutOfMemoryException` i nie są wykonywane żadne dalsze kroki.
    * Wszystkie pola nowego wystąpienia są inicjowane do ich wartości domyślnych ([wartości domyślne](variables.md#default-values)).
    * Konstruktor wystąpienia jest wywoływany zgodnie z regułami wywołania elementu członkowskiego funkcji ([Sprawdzanie czasu kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Odwołanie do nowo przydzielonych wystąpień jest automatycznie przenoszone do konstruktora wystąpień i można uzyskać do niego dostęp z poziomu tego konstruktora jako `this`.
*   Jeśli `T` jest *struct_type*:
    * Wystąpienie typu `T` jest tworzone przez przydzielenie tymczasowej zmiennej lokalnej. Ponieważ Konstruktor wystąpienia *struct_type* jest wymagany do jednokrotnego przypisania wartości do każdego pola tworzonego wystąpienia, nie jest wymagane inicjowanie zmiennej tymczasowej.
    * Konstruktor wystąpienia jest wywoływany zgodnie z regułami wywołania elementu członkowskiego funkcji ([Sprawdzanie czasu kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Odwołanie do nowo przydzielonych wystąpień jest automatycznie przenoszone do konstruktora wystąpień i można uzyskać do niego dostęp z poziomu tego konstruktora jako `this`.

#### <a name="object-initializers"></a>Inicjatory obiektów

***Inicjator obiektu*** określa wartości dla zero lub więcej pól, właściwości lub indeksowane elementy obiektu.

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

Inicjator obiektu składa się z kolejności inicjatorów składowych, zawartych w tokenach `{` i `}` i rozdzielonych przecinkami. Każda *member_initializer* wyznacza element docelowy dla inicjalizacji. *Identyfikator* musi mieć nazwę dostępnego pola lub właściwości obiektu, który jest inicjowany, podczas gdy *argument_list* ujęty w nawiasy kwadratowe muszą określać argumenty dostępnego indeksatora dla inicjowanego obiektu. Wystąpił błąd inicjatora obiektu, aby uwzględnić więcej niż jednego inicjatora elementu członkowskiego dla tego samego pola lub właściwości.

Każdy *initializer_target* następuje znak równości i wyrażenie, inicjator obiektu lub inicjator kolekcji. Wyrażenia w inicjatorze obiektów nie są możliwe do odwoływania się do nowo utworzonego obiektu, który jest inicjowany.

Inicjator elementu członkowskiego, który określa wyrażenie po znaku równości jest przetwarzane w taki sam sposób jak przypisanie ([przypisanie proste](expressions.md#simple-assignment)) do obiektu docelowego.

Inicjator elementu członkowskiego, który określa Inicjator obiektu po znaku równości, jest ***zagnieżdżonym inicjatorem obiektów***, czyli inicjalizacją osadzonego obiektu. Zamiast przypisywać nową wartość do pola lub właściwości, przypisania w inicjatorze zagnieżdżonych obiektów są traktowane jako przypisania do elementów członkowskich pola lub właściwości. Inicjatory zagnieżdżonych obiektów nie mogą być stosowane do właściwości z typem wartości lub do pól tylko do odczytu z typem wartości.

Inicjator elementu członkowskiego, który określa inicjator kolekcji po znaku równości jest inicjalizacją osadzonej kolekcji. Zamiast przypisywać nową kolekcję do pola docelowego, właściwości lub indeksatora elementy podane w inicjatorze są dodawane do kolekcji, do której odwołuje się element docelowy. Element docelowy musi być typu kolekcji, który spełnia wymagania określone w [inicjatorach kolekcji](expressions.md#collection-initializers).

Argumenty inicjatora indeksu zawsze będą oceniane dokładnie jeden raz. W związku z tym nawet jeśli argumenty zakończą się nigdy nie są używane (np. z powodu pustego inicjatora zagnieżdżonego), zostaną ocenione pod kątem ich efektów ubocznych.

Następująca Klasa reprezentuje punkt z dwoma współrzędnymi:
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

Wystąpienie `Point` można utworzyć i zainicjować w następujący sposób:
```csharp
Point a = new Point { X = 0, Y = 1 };
```
ma ten sam skutek co
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
gdzie `__a` jest niewidoczna w inny sposób i niedostępna zmienna tymczasowa. Następująca Klasa reprezentuje prostokąt utworzony na podstawie dwóch punktów:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

Wystąpienie `Rectangle` można utworzyć i zainicjować w następujący sposób:
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
ma ten sam skutek co
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
gdzie `__r`, `__p1` i `__p2` są zmiennymi tymczasowymi, które w przeciwnym razie są niewidoczne i niedostępne.

Jeśli Konstruktor `Rectangle`przydzieli dwa osadzone wystąpienia `Point`
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
Następująca konstrukcja może służyć do inicjowania osadzonych wystąpień `Point` zamiast przypisywania nowych wystąpień:
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
ma ten sam skutek co
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

Zgodnie z odpowiednią definicją C, Poniższy przykład:
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
jest odpowiednikiem tej serii przypisań:
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
gdzie `__c`itd., są generowane zmienne, które są niewidoczne i niedostępne dla kodu źródłowego. Należy zauważyć, że argumenty dla `[0,0]` są oceniane tylko raz, a argumenty dla `[1,2]` są oceniane raz, mimo że nigdy nie są używane.

#### <a name="collection-initializers"></a>Inicjatory kolekcji

Inicjator kolekcji określa elementy kolekcji.

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

Inicjator kolekcji składa się z sekwencji elementów inicjujących, ujętej w tokeny `{` i `}` i oddzielone przecinkami. Każdy inicjator elementów określa element, który ma zostać dodany do inicjowanego obiektu kolekcji i składa się z listy wyrażeń ujętych w `{` i tokeny `}` i rozdzielonych przecinkami.  Inicjator elementu pojedynczego wyrażenia może być zapisany bez nawiasów klamrowych, ale nie może być wyrażeniem przypisania, aby uniknąć niejednoznaczności z inicjatorami elementów członkowskich. *Non_assignment_expression* produkcji jest zdefiniowany w [wyrażeniu](expressions.md#expression).

Poniżej znajduje się przykład wyrażenia tworzenia obiektu zawierającego inicjator kolekcji:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

Obiekt kolekcji, do którego zastosowano inicjator kolekcji, musi być typu, który implementuje `System.Collections.IEnumerable` lub wystąpi błąd w czasie kompilacji. Dla każdego określonego elementu w kolejności inicjator kolekcji wywołuje metodę `Add` w obiekcie docelowym z listą wyrażeń inicjatora elementu jako listą argumentów, stosując normalne wyszukiwanie elementów członkowskich i rozpoznawanie przeciążenia dla każdego wywołania. W ten sposób obiekt kolekcji musi mieć odpowiednie wystąpienie lub metodę rozszerzenia o nazwie `Add` dla każdego inicjatora elementu.

Następująca Klasa reprezentuje kontakt z nazwą i listą numerów telefonów:
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

`List<Contact>` można utworzyć i zainicjować w następujący sposób:
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
ma ten sam skutek co
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
gdzie `__clist`, `__c1` i `__c2` są zmiennymi tymczasowymi, które w przeciwnym razie są niewidoczne i niedostępne.

#### <a name="array-creation-expressions"></a>Wyrażenia tworzenia tablicy

*Array_creation_expression* służy do tworzenia nowego wystąpienia *array_type*.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

Wyrażenie tworzenia tablicy w pierwszym formularzu przypisuje wystąpienie tablicy typu, które powoduje usunięcie każdego z poszczególnych wyrażeń z listy wyrażeń. Na przykład wyrażenie tworzenia tablicy `new int[10,20]` tworzy wystąpienie tablicy typu `int[,]`, a wyrażenie tworzenia tablicy `new int[10][,]` tworzy tablicę typu `int[][,]`. Każde wyrażenie na liście wyrażeń musi być typu `int`, `uint`, `long`lub `ulong`lub niejawnie konwertowane na jeden lub więcej z tych typów. Wartość każdego wyrażenia określa długość odpowiedniego wymiaru w nowo przydzielonym wystąpieniu tablicy. Ponieważ długość wymiaru tablicy nie może być ujemna, jest to błąd czasu kompilacji, który ma *constant_expression* z ujemną wartością na liście wyrażeń.

W przypadku niebezpiecznego kontekstu ([niebezpieczne konteksty](unsafe-code.md#unsafe-contexts)) nie określono układu tablic.

Jeśli wyrażenie tworzenia tablicowe w pierwszym formularzu zawiera inicjatora tablicy, każde wyrażenie na liście wyrażeń musi być stałą, a długość rangi i wymiary określona przez listę wyrażeń musi być zgodna z właściwościami inicjatora tablicy.

W wyrażeniu tworzenia tablicy drugiego lub trzeciego formularza ranga określonego typu tablicy lub specyfikatora rangi musi być zgodna z inicjatorem tablicy. Długości poszczególnych wymiarów są wywnioskowane na podstawie liczby elementów w każdym z odpowiednich poziomów zagnieżdżenia inicjatora tablicy. W tym celu wyrażenie
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
dokładnie odpowiada
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

Wyrażenie tworzenia tablicy trzeciego formularza jest określane jako ***wyrażenie tworzenia tablicy niejawnie wpisanej***. Jest on podobny do drugiego formularza, z tą różnicą, że typ elementu tablicy nie jest jawnie określony, ale określony jako najlepszy typowy typ ([znalezienie najlepszego wspólnego typu zestawu wyrażeń](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) zestawu wyrażeń w inicjatorze tablicy. Dla tablicy wielowymiarowej, tj., w której *rank_specifier* zawiera co najmniej jeden przecinek, ten zestaw składa się ze wszystkich *wyrażeń*, które znajdują się w zagnieżdżonych *array_initializer*s.

Inicjatory tablicy są szczegółowo opisane w [inicjatorach tablicy](arrays.md#array-initializers).

Wynik obliczania wyrażenia tworzenia tablicy jest klasyfikowany jako wartość, a mianowicie odwołanie do nowo przydzielonych wystąpień tablicy. Przetwarzanie wyrażenia tworzenia tablicy w czasie wykonywania składa się z następujących kroków:

*  Wyrażenia długości wymiarów *expression_list* są oceniane w kolejności od lewej do prawej. Po dokonaniu oceny każdego wyrażenia niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) do jednego z następujących typów jest wykonywana: `int`, `uint`, `long``ulong`. Pierwszy typ na tej liście, dla którego istnieje niejawna konwersja, jest wybierany. Jeśli Obliczanie wyrażenia lub kolejnej niejawnej konwersji powoduje wyjątek, nie są oceniane żadne dalsze wyrażenia i nie są wykonywane żadne dalsze kroki.
*  Obliczone wartości dla długości wymiarów są weryfikowane w następujący sposób. Jeśli co najmniej jedna z wartości jest mniejsza od zera, zostanie zgłoszony `System.OverflowException` i nie są wykonywane żadne dalsze kroki.
*  Przydzielono wystąpienie tablicy z podaną długością wymiaru. Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia, zostanie zgłoszony `System.OutOfMemoryException` i nie są wykonywane żadne dalsze kroki.
*  Wszystkie elementy nowego wystąpienia tablicy są inicjowane do ich wartości domyślnych ([wartości domyślne](variables.md#default-values)).
*  Jeśli wyrażenie tworzenia tablicy zawiera inicjatora tablicy, każde wyrażenie w inicjatorze tablicy jest oceniane i przypisywane do odpowiadającego mu elementu tablicy. Obliczenia i przypisania są wykonywane w kolejności, w której wyrażenia są zapisywane w inicjatorze tablicy — innymi słowy, elementy są inicjowane w kolejności rosnącego indeksu, a najwyższego rzędu. Jeśli Obliczanie danego wyrażenia lub kolejnego przypisania do odpowiadającego elementu tablicy powoduje wyjątek, nie są inicjowane żadne dalsze elementy (a pozostałe elementy będą mieć wartości domyślne).

Wyrażenie tworzenia tablicy umożliwia utworzenie wystąpienia tablicy z elementami typu tablicy, ale elementy takich tablic muszą być inicjowane ręcznie. Na przykład, instrukcja
```csharp
int[][] a = new int[100][];
```
Tworzy tablicę jednowymiarową z 100 elementami typu `int[]`. Wartość początkowa każdego elementu jest `null`. Nie jest możliwe dla tego samego wyrażenia tworzenia tablicy, które również tworzy wystąpienia tablic podrzędnych, i instrukcji
```csharp
int[][] a = new int[100][5];        // Error
```
powoduje błąd czasu kompilacji. Zamiast tego należy wykonać ręczne tworzenie wystąpienia tablic podrzędnych, jak w
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

Gdy tablica tablic ma kształt "prostokątny", jeśli podtablice mają taką samą długość, bardziej wydajne jest użycie tablicy wielowymiarowej. W powyższym przykładzie Tworzenie wystąpienia tablicy tablic tworzy 101 obiektów — jedną tablicę zewnętrzną i tablicę podrzędną 100. W przeciwieństwie do
```csharp
int[,] = new int[100, 5];
```
tworzy tylko jeden obiekt, dwuwymiarową tablicę i wykonuje alokację w pojedynczej instrukcji.

Poniżej przedstawiono przykłady niejawnie wpisanych wyrażeń tworzenia tablicy:
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

Ostatnie wyrażenie powoduje błąd czasu kompilacji, ponieważ żadne `int` ani `string` nie są niejawnie konwertowane na inne, więc nie ma żadnego najlepszego wspólnego typu. W tym przypadku należy użyć jawnie określonego wyrażenia tworzenia tablicy, na przykład określając typ, który ma być `object[]`. Alternatywnie, jeden z elementów może być rzutowany na wspólny typ podstawowy, który następnie stanie się wywnioskowanym typem elementu.

Niejawnie wpisane wyrażenia tworzenia tablicy można łączyć z inicjatorami anonimowych obiektów ([wyrażeniami tworzenia obiektów anonimowych](expressions.md#anonymous-object-creation-expressions)) w celu tworzenia anonimowo wpisanych struktur danych. Na przykład:
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a>Delegowanie wyrażeń tworzenia

*Delegate_creation_expression* służy do tworzenia nowego wystąpienia *delegate_type*.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

Argumentem wyrażenia tworzenia delegata musi być grupa metod, funkcja anonimowa lub wartość typu czasu kompilacji `dynamic` lub *delegate_type*. Jeśli argument jest grupą metod, identyfikuje metodę i, dla metody wystąpienia, obiekt, dla którego ma zostać utworzony delegat. Jeśli argument jest funkcją anonimową, bezpośrednio definiuje parametry i treść metody obiektu docelowego delegata. Jeśli argument jest wartością, określa wystąpienie delegata, dla którego ma zostać utworzona kopia.

Jeśli *wyrażenie* ma typ czasu kompilacji `dynamic`, *delegate_creation_expression* jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)), a Poniższe reguły są stosowane w czasie wykonywania przy użyciu typu w czasie wykonywania *wyrażenia*. W przeciwnym razie reguły są stosowane w czasie kompilacji.

Przetwarzanie powiązania *delegate_creation_expression* formularza `new D(E)`, gdzie `D` jest *delegate_type* i `E` jest *wyrażeniem*, składa się z następujących kroków:

*  Jeśli `E` jest grupą metod, wyrażenie tworzenia delegata jest przetwarzane w taki sam sposób jak konwersja grup metod ([konwersje grup metod](conversions.md#method-group-conversions)) z `E` do `D`.
*  Jeśli `E` jest funkcją anonimową, wyrażenie tworzenia delegata jest przetwarzane w taki sam sposób jak w przypadku konwersji funkcji anonimowej ([konwersje funkcji anonimowych](conversions.md#anonymous-function-conversions)) z `E` do `D`.
*  Jeśli `E` jest wartością, `E` muszą być zgodne ([deklaracje delegatów](delegates.md#delegate-declarations)) z `D`, a wynik jest odwołaniem do nowo utworzonego delegata typu `D`, który odwołuje się do tej samej listy wywołań co `E`. Jeśli `E` nie jest zgodny z `D`, wystąpi błąd w czasie kompilacji.

Przetwarzanie *delegate_creation_expression* formularza `new D(E)`, gdzie `D` jest *delegate_type* i `E` jest *wyrażeniem*, składa się z następujących kroków:

*   Jeśli `E` jest grupą metod, wyrażenie tworzenia delegata jest oceniane jako konwersja grup metod ([konwersje grup](conversions.md#method-group-conversions)metod) z `E` do `D`.
*   Jeśli `E` jest funkcją anonimową, tworzenie delegatów jest oceniane jako anonimowa funkcja konwersji z `E` do `D` ([konwersje funkcji anonimowych](conversions.md#anonymous-function-conversions)).
*   Jeśli `E` jest wartością *delegate_type*:
    * `E` jest oceniane. Jeśli ta Ocena spowoduje wyjątek, nie są wykonywane żadne dalsze kroki.
    * Jeśli wartość `E` jest `null`, zostanie zgłoszony `System.NullReferenceException` i nie zostaną wykonane żadne dalsze kroki.
    * Zostanie przydzielono nowe wystąpienie typu delegata `D`. Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia, zostanie zgłoszony `System.OutOfMemoryException` i nie są wykonywane żadne dalsze kroki.
    * Nowe wystąpienie delegata jest inicjowane z tą samą listą wywołania co wystąpienie delegata określone przez `E`.

Lista wywołań delegata jest określana podczas tworzenia wystąpienia delegata, a następnie pozostaje stała dla całego okresu istnienia delegata. Innymi słowy, nie można zmienić docelowej możliwej jednostki delegata po jego utworzeniu. Gdy dwa Delegaty są połączone lub jeden z nich jest usuwany z innego ([deklaracje delegatów](delegates.md#delegate-declarations)), nowe wyniki delegata; zawartość żadnego z istniejących delegatów nie została zmieniona.

Nie można utworzyć delegata, który odwołuje się do właściwości, indeksatora, zdefiniowanego przez użytkownika operatora, konstruktora wystąpienia, destruktora lub konstruktora statycznego.

Jak opisano powyżej, gdy delegat jest tworzony na podstawie grupy metod, formalna lista parametrów i zwracany typ delegata określają, które z przeciążonych metod wybrać. W przykładzie
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
pole `A.f` jest inicjowane z delegatem, który odwołuje się do drugiej metody `Square`, ponieważ ta metoda dokładnie pasuje do formalnej listy parametrów i zwracanego typu `DoubleFunc`. Druga metoda `Square` nie jest obecna, wystąpił błąd w czasie kompilacji.

#### <a name="anonymous-object-creation-expressions"></a>Wyrażenia tworzenia obiektów anonimowych

*Anonymous_object_creation_expression* jest używany do tworzenia obiektu typu anonimowego.

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

Inicjator obiektu anonimowego deklaruje typ anonimowy i zwraca wystąpienie tego typu. Typ anonimowy to typ klasy pustego, który dziedziczy bezpośrednio z `object`. Elementy członkowskie typu anonimowego są sekwencją właściwości tylko do odczytu wywnioskowanej z inicjatora obiektu anonimowego używanego do tworzenia wystąpienia typu. W przypadku inicjatora obiektów anonimowych formularza
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
deklaruje anonimowy typ formularza
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
gdzie każdy `Tx` jest typem odpowiedniego wyrażenia `ex`. Wyrażenie użyte w *member_declarator* musi mieć typ. W ten sposób jest to błąd czasu kompilacji dla wyrażenia w *member_declarator* być wartością null lub funkcją anonimową. Jest to również błąd czasu kompilacji dla wyrażenia, aby można było mieć niebezpieczny typ.

Nazwy typu anonimowego i parametru do jego metody `Equals` są generowane automatycznie przez kompilator i nie można się do niego odwoływać w tekście programu.

W ramach tego samego programu dwa anonimowe Inicjatory obiektów, które określają sekwencję właściwości tych samych nazw i typów czasu kompilacji w tej samej kolejności, spowodują wystąpienie tego samego typu anonimowego.

W przykładzie
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
przypisanie w ostatnim wierszu jest dozwolone, ponieważ `p1` i `p2` są tego samego typu anonimowego.

Metody `Equals` i `GetHashcode` na typach anonimowych przesłaniają metody dziedziczone z `object`i są zdefiniowane w warunkach `Equals` i `GetHashcode` właściwości, tak aby dwa wystąpienia tego samego typu anonimowego były równe, jeśli wszystkie ich właściwości są równe.

Element członkowski deklarator może być skrócony do prostej nazwy ([wnioskowanie o typie](expressions.md#type-inference)), dostęp do składowej ([Sprawdzanie czasu kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), dostęp podstawowy ([dostęp podstawowy](expressions.md#base-access)) lub dostęp do składowej o wartości null ([wyrażenia warunkowe o wartości null jako inicjatory projekcji](expressions.md#null-conditional-expressions-as-projection-initializers)). Jest to nazywane ***inicjatorem projekcji*** i jest skrótem dla deklaracji i przypisywania do właściwości o tej samej nazwie. W każdym przypadku element członkowski Deklaratory formularzy
```csharp
identifier
expr.identifier
```
są dokładnie równoważne następujących odpowiednio:
```csharp
identifier = identifier
identifier = expr.identifier
```

W tym celu w inicjatorze projekcji *Identyfikator* wybiera zarówno wartość, jak i pole lub właściwość, do której zostanie przypisana wartość. Intuicyjnie, inicjator projekcji nie tylko wartości, ale również nazwę wartości.

### <a name="the-typeof-operator"></a>Operator typeof

Operator `typeof` służy do uzyskiwania obiektu `System.Type` dla typu.

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

Pierwsza forma *typeof_expression* składa się ze słowa kluczowego `typeof`, po którym następuje *Typ*ujęty w nawiasy. Wynikiem wyrażenia tego formularza jest obiekt `System.Type` dla wskazanego typu. Istnieje tylko jeden obiekt `System.Type` dla danego typu. Oznacza to, że dla typu `T`, `typeof(T) == typeof(T)` ma zawsze wartość true. *Typ* nie może być `dynamic`.

Druga forma *typeof_expression* składa się ze słowa kluczowego `typeof`, po którym następuje *unbound_type_name*w nawiasach. *Unbound_type_name* jest bardzo podobna do *TYPE_NAME* (nazw[i typów](basic-concepts.md#namespace-and-type-names)nazw), z tą różnicą, że *unbound_type_name* zawiera *generic_dimension_specifier*s, gdzie *TYPE_NAME* zawiera *type_argument_list*s. Gdy argument operacji *typeof_expression* jest sekwencją tokenów, które spełniają gramatyke obu *unbound_type_name* i *TYPE_NAME*, mianowicie, gdy nie zawiera *generic_dimension_specifier* ani *type_argument_list*, sekwencja tokenów jest uznawana za *TYPE_NAME*. Znaczenie *unbound_type_name* jest określane w następujący sposób:

*  Przekonwertuj sekwencję tokenów na *TYPE_NAME* , zastępując każdy *generic_dimension_specifier* *type_argument_list* o tej samej liczbie przecinki i słowo kluczowe `object` jako każdy *type_argument*.
*  Oceń wynikowy *TYPE_NAME*, ignorując wszystkie ograniczenia dotyczące parametrów typu.
*  *Unbound_type_name* jest rozpoznawany jako niezwiązany typ ogólny skojarzony z wynikiem skonstruowanego typu ([powiązane i niepowiązane typy](types.md#bound-and-unbound-types)).

Wynik *typeof_expression* jest obiektem `System.Type` dla wynikowego niepowiązanego typu ogólnego.

Trzecia postać *typeof_expression* składa się ze słowa kluczowego `typeof`, po którym następuje słowo kluczowe `void` ujęte w nawiasy. Wynikiem wyrażenia tego formularza jest obiekt `System.Type`, który reprezentuje nieobecność typu. Obiekt typu zwracany przez `typeof(void)` jest odrębny od obiektu typu zwracanego dla dowolnego typu. Ten obiekt typu specjalnego jest przydatny w bibliotekach klas, które umożliwiają odbicie w metodach w języku, gdzie te metody chcą mieć sposób reprezentowania zwracanego typu dowolnej metody, w tym metod void, z wystąpieniem `System.Type`.

Operatora `typeof` można używać w parametrze typu. Wynik jest obiektem `System.Type` dla typu czasu wykonywania, który został powiązany z parametrem typu. Operatora `typeof` można również używać dla typu złożonego lub niepowiązanego typu ogólnego ([powiązane i niepowiązane typy](types.md#bound-and-unbound-types)). Obiekt `System.Type` dla niepowiązanego typu ogólnego nie jest taki sam jak obiekt `System.Type` typu wystąpienia. Typ wystąpienia zawsze jest zamkniętym skonstruowanym typem w czasie wykonywania, dlatego jego obiekt `System.Type` zależy od argumentów typu run w użyciu, podczas gdy niezwiązany typ ogólny nie ma argumentów typu.

Przykład
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
tworzy następujące dane wyjściowe:
```console
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

Należy pamiętać, że `int` i `System.Int32` są tego samego typu.

Należy również zauważyć, że wynik `typeof(X<>)` nie zależy od argumentu Type, ale wynikiem `typeof(X<T>)` jest.

### <a name="the-checked-and-unchecked-operators"></a>Sprawdzone i niesprawdzone operatory

Operatory `checked` i `unchecked` są używane do kontrolowania ***kontekstu sprawdzania przepełnienia*** dla operacji arytmetycznych typu całkowitego i konwersji.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

Operator `checked` oblicza ujęte wyrażenie w sprawdzonym kontekście, a operator `unchecked` oblicza wyrażenie zawarte w niesprawdzonym kontekście. *Checked_expression* lub *unchecked_expression* odpowiada dokładnie na *parenthesized_expression* ([wyrażeń ujętych w nawiasy](expressions.md#parenthesized-expressions)), z tą różnicą, że zawarte wyrażenie jest oceniane w danym kontekście sprawdzania przepełnienia.

Kontekst sprawdzania przepełnienia można również kontrolować za pomocą instrukcji `checked` i `unchecked` ([instrukcje sprawdzone i niesprawdzone](statements.md#the-checked-and-unchecked-statements)).

Kontekst sprawdzania przepełnienia ma wpływ na następujące operacje `checked` i `unchecked` operatory i instrukcje:

*  Wstępnie zdefiniowane `++` i `--` operatory jednoargumentowe (operatory[przyrostka i zmniejszenie](expressions.md#postfix-increment-and-decrement-operators) [prefiksu oraz operatory przyrostu i zmniejszania](expressions.md#prefix-increment-and-decrement-operators)), gdy operand jest typu całkowitego.
*  Wstępnie zdefiniowany operator jednoargumentowy `-` ([jednoargumentowy operator minus](expressions.md#unary-minus-operator)), gdy operand jest typu całkowitego.
*  Wstępnie zdefiniowane `+`, `-`, `*`i `/` operatory binarne ([Operatory arytmetyczne](expressions.md#arithmetic-operators)), gdy oba operandy są typami całkowitymi.
*  Jawne konwersje numeryczne ([jawne konwersje liczbowe](conversions.md#explicit-numeric-conversions)) z jednego typu całkowitego na inny typ całkowity lub z `float` lub `double` do typu całkowitego.

Gdy jedna z powyższych operacji tworzy wynik, który jest zbyt duży do reprezentowania w typie docelowym, kontekst, w którym wykonywana jest operacja, kontroluje Wynikowe zachowanie:

*  W kontekście `checked`, jeśli operacja jest wyrażeniem stałym ([wyrażenia stałe](expressions.md#constant-expressions)), wystąpi błąd w czasie kompilacji. W przeciwnym razie, gdy operacja zostanie wykonana w czasie wykonywania, zostanie zgłoszony `System.OverflowException`.
*  W kontekście `unchecked` wynik jest obcinany przez odrzucenie wszystkich bitów o dużej kolejności, które nie mieszczą się w typie docelowym.

Dla wyrażeń niestałych (wyrażeń, które są oceniane w czasie wykonywania), które nie są ujęte w żadnej `checked` lub `unchecked` operatory lub instrukcje, domyślny kontekst sprawdzania przepełnienia jest `unchecked`, chyba że zewnętrzne czynniki (takie jak przełączniki kompilatora i konfiguracja środowiska wykonawczego) są wywoływane w celu `checked` oceny.

W przypadku wyrażeń stałych (wyrażenia, które mogą być w pełni oceniane w czasie kompilacji), domyślny kontekst sprawdzania przepełnienia jest zawsze `checked`. Jeśli wyrażenie stałe nie jest jawnie umieszczane w kontekście `unchecked`, przepełnienia, które wystąpiły podczas oceny czasu kompilowania wyrażenia, zawsze powodują błędy w czasie kompilacji.

Nie ma to wpływ na treść funkcji anonimowej `checked` ani kontekstów `unchecked`, w których występuje funkcja anonimowa.

W przykładzie
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
nie zgłoszono błędów czasu kompilacji, ponieważ żadne wyrażenia nie mogą być oceniane w czasie kompilacji. W czasie wykonywania Metoda `F` zgłasza `System.OverflowException`, a metoda `G` zwraca-727379968 (Dolna część 32 bitów wyniku spoza zakresu). Zachowanie metody `H` zależy od domyślnego kontekstu sprawdzania przepełnienia dla kompilacji, ale jest taka sama jak `F` lub taka sama jak `G`.

W przykładzie
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
przepełnienia występujące podczas oceny wyrażeń stałych w `F` i `H` powodują zgłoszenie błędów w czasie kompilacji, ponieważ wyrażenia są oceniane w kontekście `checked`. Przepełnienie ma również miejsce podczas oceniania wyrażenia stałego w `G`, ale ponieważ Ocena odbywa się w kontekście `unchecked`, przepełnienie nie zostanie zgłoszone.

Operatory `checked` i `unchecked` mają wpływ tylko na kontekst sprawdzania przepełnienia dla tych operacji, które są umieszczone w formie tekstowej w tokenach "`(`" i "`)`". Operatory nie mają wpływu na składowe funkcji, które są wywoływane w wyniku obliczania zawartego wyrażenia. W przykładzie
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
Użycie `checked` w `F` nie wpływa na ocenę `x * y` w `Multiply`, dlatego `x * y` jest oceniane w kontekście domyślnego sprawdzania przepełnienia.

Operator `unchecked` jest wygodny podczas pisania stałych z podpisanych typów całkowitych w notacji szesnastkowej. Na przykład:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Obie stałe szesnastkowe powyżej są typu `uint`. Ze względu na to, że stałe są poza zakresem `int`, bez operatora `unchecked`, rzutowania do `int` spowodują błędy w czasie kompilacji.

Operatory `checked` i `unchecked` i instrukcje umożliwiają programistom kontrolowanie niektórych aspektów niektórych obliczeń liczbowych. Jednak zachowanie niektórych operatorów numerycznych zależy od typów danych argumentów operacji. Na przykład mnożenie dwóch miejsc dziesiętnych zawsze powoduje wyjątek w przypadku przepełnienia nawet w jawnej konstrukcji `unchecked`. Podobnie mnożenie dwóch wartości zmiennoprzecinkowych nigdy nie powoduje wyjątku w przypadku przepełnienia nawet w jawnej konstrukcji `checked`. Ponadto inne operatory nigdy nie mają wpływ na tryb sprawdzania, bez względu na to, czy jest to ustawienie domyślne, czy jawne.

### <a name="default-value-expressions"></a>Wyrażenia wartości domyślnych

Domyślne wyrażenie wartości służy do uzyskiwania wartości domyślnej ([wartości domyślne](variables.md#default-values)) typu. Zwykle wyrażenie wartości domyślnej jest używane dla parametrów typu, ponieważ może być nieznane, jeśli parametr typu jest typem wartości lub typem referencyjnym. (Nie istnieje konwersja z literału `null` do parametru typu, chyba że parametr typu jest znany jako typ referencyjny).

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Jeśli *Typ* w *default_value_expression* jest obliczany w czasie wykonywania do typu referencyjnego, wynik jest `null` konwertowany na ten typ. Jeśli *Typ* w *default_value_expression* jest obliczany w czasie wykonywania do typu wartości, wynik jest wartością domyślną *value_type*([konstruktory domyślne](types.md#default-constructors)).

*Default_value_expression* jest wyrażeniem stałym ([wyrażenia stałe](expressions.md#constant-expressions)), jeśli typ jest typem referencyjnym lub parametrem typu, który jest znany jako typ referencyjny ([ograniczenia parametru typu](classes.md#type-parameter-constraints)). Ponadto *default_value_expression* jest wyrażeniem stałym, jeśli typ jest jednym z następujących typów wartości: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`lub dowolnego typu wyliczenia.


### <a name="nameof-expressions"></a>Wyrażenia nameof

*Nameof_expression* służy do uzyskania nazwy jednostki programu jako ciągu stałej.

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

Mówiąc gramatycznie, argument operacji *named_entity* jest zawsze wyrażeniem. Ponieważ `nameof` nie jest zastrzeżonym słowem kluczowym, wyrażenie NameOf jest zawsze niejednoznaczne syntaktycznie z wywołaniem prostej nazwy `nameof`. Ze względów zgodności, jeśli nazwa wyszukiwania nazw ([proste nazwy](expressions.md#simple-names)) `nameof` się powiedzie, wyrażenie jest traktowane jako *invocation_expression* --niezależnie od tego, czy wywołanie jest prawne. W przeciwnym razie jest *nameof_expression*.

Znaczenie *named_entity* *nameof_expression* jest jego znaczeniem jako wyrażenia; oznacza to, że jako *simple_name*, *base_access* lub *member_access*. Jednak jeśli odnośnik opisany w temacie [proste nazwy](expressions.md#simple-names) i [dostęp do elementu członkowskiego](expressions.md#member-access) powoduje błąd, ponieważ element członkowski wystąpienia został znaleziony w kontekście statycznym, *nameof_expression* nie tworzy takiego błędu.

Jest to błąd czasu kompilacji dla *named_entity* wyznaczania grupy metod w celu uzyskania *type_argument_list*. Jest to błąd czasu kompilowania dla *named_entity_target* typu `dynamic`.

*Nameof_expression* jest wyrażeniem stałym typu `string`i nie ma wpływu na środowisko uruchomieniowe. *Named_entity* nie jest oceniana i jest ignorowany w celach analizy z określonym przydziałem ([ogólne reguły dla prostych wyrażeń](variables.md#general-rules-for-simple-expressions)). Jego wartość to ostatni identyfikator *named_entity* przed opcjonalnym *type_argument_list*końcowym, przekształcony w następujący sposób:

* Prefiks "`@`", jeśli jest używany, jest usuwany.
* Każda *unicode_escape_sequence* jest przekształcana do odpowiedniego znaku Unicode.
* Wszystkie *formatting_characters* są usuwane.

Są to te same przekształcenia, które są stosowane w [identyfikatorach](lexical-structure.md#identifiers) podczas testowania równości między identyfikatorami.

Do zrobienia: przykłady

### <a name="anonymous-method-expressions"></a>Wyrażenia metody anonimowej

*Anonymous_method_expression* to jeden z dwóch sposobów definiowania funkcji anonimowej. Są one dokładniej opisane w [wyrażeniach funkcji anonimowych](expressions.md#anonymous-function-expressions).

## <a name="unary-operators"></a>Operatory jednoargumentowe

Operatory `?`, `+`, `-`, `!`, `~`, `++`, `--`, Cast i `await` są nazywane operatorami jednoargumentowymi.

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

Jeśli argument operacji *unary_expression* ma typ czasu kompilacji `dynamic`, jest on dynamicznie powiązany ([powiązanie dynamiczne](expressions.md#dynamic-binding)). W takim przypadku typ czasu kompilacji *unary_expression* jest `dynamic`, a rozwiązanie opisane poniżej zostanie wykonane w czasie wykonywania przy użyciu typu czasu wykonywania argumentu operacji.

### <a name="null-conditional-operator"></a>Operator warunkowy o wartości null

Operator warunkowy null stosuje listę operacji do operandu tylko wtedy, gdy ten operand ma wartość różną od null. W przeciwnym razie wynik zastosowania operatora jest `null`.

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

Lista operacji może obejmować dostęp do składowych i operacje dostępu do elementów (które mogą być w stanie null — warunkowo), a także wywołania.

Na przykład wyrażenie `a.b?[0]?.c()` jest *null_conditional_expression* z *primary_expression* `a.b` i *null_conditional_operations* `?[0]` (dostęp warunkowy o wartości null), `?.c` (dostęp do elementu członkowskiego o wartości null) i `()` (wywołanie).

W przypadku *null_conditional_expression* `E` z *primary_expression* `P`, niech `E0` być wyrażeniem, które zostało uzyskane przez usunięcie wiodącego `?` z każdej *null_conditional_operations* `E`. Koncepcyjnie, `E0` jest wyrażeniem, które zostanie ocenione, jeśli żadne sprawdzenie wartości null nie zostało reprezentowane przez `?`s nie `null`.

Ponadto, niech `E1` być wyrażeniem, które można usunąć przez usunięcie wiodącego `?` z zaledwie pierwszego *null_conditional_operations* w `E`. Może to prowadzić do *wyrażenia podstawowego* (jeśli istnieje tylko jeden `?`) lub do innego *null_conditional_expression*.

Na przykład jeśli `E` jest wyrażeniem `a.b?[0]?.c()`, `E0` jest wyrażeniem `a.b[0].c()` i `E1` jest wyrażeniem `a.b[0]?.c()`.

Jeśli `E0` jest sklasyfikowany jako Nothing, `E` zaklasyfikowane jako Nothing. W przeciwnym razie E jest klasyfikowane jako wartość.

`E0` i `E1` są używane do określenia znaczenia `E`:

*  Jeśli `E` występuje jako *statement_expression* znaczenie `E` jest takie samo jak instrukcja

   ```csharp
   if ((object)P != null) E1;
   ```

   z tą różnicą, że P jest oceniane tylko raz.

*  W przeciwnym razie, jeśli `E0` jest sklasyfikowany jako Brak błędu w czasie kompilacji.

*  W przeciwnym razie niech `T0` być typem `E0`.

   *  Jeśli `T0` jest parametrem typu, który nie jest znany jako typ referencyjny lub typ wartości niedopuszczający wartości null, wystąpi błąd w czasie kompilacji.

   *  Jeśli `T0` ma typ wartości niedopuszczający wartości null, typ `E` jest `T0?`, a znaczenie `E` jest takie samo jak

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      z tą różnicą, że `P` jest oceniane tylko raz.

   *  W przeciwnym razie typ E jest T0, a znaczenie E jest takie samo jak

      ```csharp
      ((object)P == null) ? null : E1
      ```

      z tą różnicą, że `P` jest oceniane tylko raz.

Jeśli `E1` należy do *null_conditional_expression*, te reguły są stosowane ponownie, zagnieżdżając testy dla `null` do momentu braku dalszych `?`, a wyrażenie zostało zredukowane w dół do `E0`wyrażenia podstawowego.

Na przykład, jeśli wyrażenie `a.b?[0]?.c()` występuje jako wyrażenie instrukcji, jak w instrukcji:
```csharp
a.b?[0]?.c();
```
jego znaczenie jest równoważne z:
```csharp
if (a.b != null) a.b[0]?.c();
```
co ponownie jest równoważne:
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
Z tą różnicą, że `a.b` i `a.b[0]` są oceniane tylko raz.

Jeśli występuje w kontekście, w którym jego wartość jest używana, jak w:
```csharp
var x = a.b?[0]?.c();
```
przy założeniu, że typ końcowego wywołania nie jest typem wartości niedopuszczających wartości null, jego znaczenie jest równoważne:
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
z tą różnicą, że `a.b` i `a.b[0]` są oceniane tylko raz.

#### <a name="null-conditional-expressions-as-projection-initializers"></a>Wyrażenia warunkowe o wartości null jako inicjatory projekcji

Wyrażenie warunkowe o wartości null jest dozwolone tylko jako *member_declarator* w *anonymous_object_creation_expression* ([wyrażenia anonimowego tworzenia obiektów](expressions.md#anonymous-object-creation-expressions)), jeśli zostanie zakończone przez (opcjonalnie wartość null) członek. Gramatycznie ten wymóg może być wyrażony jako:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Jest to specjalny przypadek gramatyki dla *null_conditional_expression* powyżej. Produkcja dla *member_declarator* w [wyrażeniach tworzenia obiektów anonimowych](expressions.md#anonymous-object-creation-expressions) , a następnie zawiera tylko *null_conditional_member_access*.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>Wyrażenia warunkowe o wartości null jako wyrażenia instrukcji

Wyrażenie warunkowe o wartości null jest dozwolone tylko jako *statement_expression* ([instrukcje wyrażenia](statements.md#expression-statements)), jeśli zostanie zakończone wywołaniem. Gramatycznie ten wymóg może być wyrażony jako:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Jest to specjalny przypadek gramatyki dla *null_conditional_expression* powyżej. Środowisko produkcyjne dla *statement_expression* w [instrukcjach wyrażeń](statements.md#expression-statements) zawiera tylko *null_conditional_invocation_expression*.


### <a name="unary-plus-operator"></a>Jednoargumentowy operator plus

Aby można było wykonać operację `+x`, do wybrania implementacji określonego operatora jest stosowana metoda rozpoznawania przeciążenia operatora[jednoargumentowego](expressions.md#unary-operator-overload-resolution). Operand jest konwertowany na typ parametru wybranego operatora, a typ wyniku jest typem zwracanym operatora. Wstępnie zdefiniowane operatory jednoargumentowe Plus są następujące:

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

Dla każdego z tych operatorów wynik jest po prostu wartością operandu.

### <a name="unary-minus-operator"></a>Jednoargumentowy operator minus

Aby można było wykonać operację `-x`, do wybrania implementacji określonego operatora jest stosowana metoda rozpoznawania przeciążenia operatora[jednoargumentowego](expressions.md#unary-operator-overload-resolution). Operand jest konwertowany na typ parametru wybranego operatora, a typ wyniku jest typem zwracanym operatora. Wstępnie zdefiniowane operatory negacji to:

*  Negacja liczby całkowitej:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   Wynik jest obliczany przez odjęcie `x` od zera. Jeśli wartość `x` jest najmniejszą reprezentacją wartości typu operandu (-2 ^ 31 dla `int` lub-2 ^ 63 dla `long`), matematyczna Negacja `x` nie jest reprezentacja w typie operandu. W takim przypadku w kontekście `checked` zostanie zgłoszony `System.OverflowException`; Jeśli występuje w kontekście `unchecked`, wynik jest wartością operandu, a przepełnienie nie zostanie zgłoszone.

   Jeśli argument operacji operatora negacji jest typu `uint`, jest konwertowany na typ `long`i typ wyniku jest `long`. Wyjątkiem jest reguła zezwalająca na zapisanie wartości `int`-2147483648 (-2 ^ 31) jako dziesiętnego literału liczb całkowitych ([literałów liczb całkowitych](lexical-structure.md#integer-literals)).

   Jeśli argument operacji operatora negacji jest typu `ulong`, wystąpi błąd w czasie kompilacji. Wyjątkiem jest reguła zezwalająca na zapisanie wartości `long`-zakresu od (-2 ^ 63) jako dziesiętnego literału liczb całkowitych ([literałów liczb całkowitych](lexical-structure.md#integer-literals)).

*  Negacja zmiennoprzecinkowa:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   Wynikiem jest wartość `x` z odwróconym znakiem. Jeśli `x` jest NaN, wynik jest również NaN.

*  Negacja dziesiętna:

   ```csharp
   decimal operator -(decimal x);
   ```

   Wynik jest obliczany przez odjęcie `x` od zera. Negacja dziesiętna jest równoznaczna z użyciem jednoargumentowego operatora minus typu `System.Decimal`.

### <a name="logical-negation-operator"></a>Operator logiczny negacji

Aby można było wykonać operację `!x`, do wybrania implementacji określonego operatora jest stosowana metoda rozpoznawania przeciążenia operatora[jednoargumentowego](expressions.md#unary-operator-overload-resolution). Operand jest konwertowany na typ parametru wybranego operatora, a typ wyniku jest typem zwracanym operatora. Istnieje tylko jeden wstępnie zdefiniowany operator logiczny negacji:
```csharp
bool operator !(bool x);
```

Ten operator oblicza logiczne Negacja operandu: Jeśli operand jest `true`, wynik jest `false`. Jeśli argument jest `false`, wynik jest `true`.

### <a name="bitwise-complement-operator"></a>Operator dopełnienia bitowego

Aby można było wykonać operację `~x`, do wybrania implementacji określonego operatora jest stosowana metoda rozpoznawania przeciążenia operatora[jednoargumentowego](expressions.md#unary-operator-overload-resolution). Operand jest konwertowany na typ parametru wybranego operatora, a typ wyniku jest typem zwracanym operatora. Wstępnie zdefiniowane operatory dopełnienia bitowego są następujące:
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

Dla każdego z tych operatorów wynik operacji jest bitowym uzupełnieniem `x`.

Każdy typ wyliczeniowy `E` niejawnie udostępnia następujący operator dopełnienia bitowego:

```csharp
E operator ~(E x);
```

Wynik oceny `~x`, gdzie `x` jest wyrażeniem typu wyliczenia `E` z `U`typu podstawowego, jest dokładnie taka sama jak Ocena `(E)(~(U)x)`, z tą różnicą, że konwersja do `E` jest zawsze wykonywana tak jak w kontekście `unchecked` ([Operatory sprawdzone i niezaznaczone](expressions.md#the-checked-and-unchecked-operators)).

### <a name="prefix-increment-and-decrement-operators"></a>Operatory prefiksów inkrementacji i dekrementacji

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

Operand operacji zwiększania lub zmniejszania prefiksu musi być wyrażeniem sklasyfikowanym jako zmienna, dostępem do właściwości lub dostępem indeksatora. Wynik operacji jest wartością tego samego typu co argument operacji.

Jeśli operand operacji przyrostu lub zmniejszania prefiksu jest właściwością lub indeksatorem, właściwość lub indeksator musi mieć zarówno `get`, jak i metodę dostępu `set`. W przeciwnym razie wystąpi błąd w czasie powiązania.

Metoda rozpoznawania przeciążenia operatora jednoargumentowego (Metoda[rozpoznawania przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) została zastosowana w celu wybrania implementacji określonego operatora. Wstępnie zdefiniowane operatory `++` i `--` istnieją dla następujących typów: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`i dowolnego typu wyliczeniowego. Wstępnie zdefiniowane operatory `++` zwracają wartość wygenerowaną przez dodanie 1 do operandu, a wstępnie zdefiniowane operatory `--` zwracają wartość wygenerowaną przez odjęcie 1 od operandu. W kontekście `checked`, jeśli wynik tego dodania lub odejmowania znajduje się poza zakresem typu wyników, a typ wyniku jest typem całkowitym lub typem wyliczeniowym, generowany jest `System.OverflowException`.

Przetwarzanie w czasie wykonywania operacji przyrostu lub zmniejszania prefiksu w formularzu `++x` lub `--x` składa się z następujących kroków:

*   Jeśli `x` jest sklasyfikowana jako zmienna:
    * `x` jest oceniane, aby utworzyć zmienną.
    * Wybrany operator jest wywoływany z wartością `x` jako argumentem.
    * Wartość zwrócona przez operator jest przechowywana w lokalizacji podawanej przez ocenę `x`.
    * Wartość zwracana przez operatora jest wynikiem operacji.
*   Jeśli `x` jest klasyfikowany jako dostęp do właściwości lub indeksatora:
    * Wyrażenie wystąpienia (jeśli `x` nie jest `static`) i lista argumentów (jeśli `x` jest dostępem indeksatora), które są skojarzone z `x` są oceniane, a wyniki są używane w kolejnych `get`ach i `set` metodach dostępu.
    * Metoda dostępu `get` `x` jest wywoływana.
    * Wybrany operator jest wywoływany z wartością zwracaną przez metodę dostępu `get` jako argument.
    * Metoda dostępu `set` `x` jest wywoływana z wartością zwracaną przez operator jako argument `value`.
    * Wartość zwracana przez operatora jest wynikiem operacji.

Operatory `++` i `--` obsługują również notację przyrostkową ([przyrostowe i zmniejszające się operatory](expressions.md#postfix-increment-and-decrement-operators)). Zwykle wynik `x++` lub `x--` jest wartością `x` przed operacją, natomiast wynik `++x` lub `--x` jest wartością `x` po operacji. W obu przypadkach `x` sama ma taką samą wartość po operacji.

Implementację `operator++` lub `operator--` można wywołać przy użyciu notacji przyrostkowej lub prefiksowej. Nie można mieć odrębnych implementacji operatora dla dwóch notacji.

### <a name="cast-expressions"></a>Wyrażenia rzutowania

*Cast_expression* służy do jawnej konwersji wyrażenia na dany typ.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

*Cast_expression* formularza `(T)E`, gdzie `T` jest *typem* , a `E` jest *unary_expression*, wykonuje jawną konwersję ([jawne konwersje](conversions.md#explicit-conversions)) wartości `E` do typu `T`. Jeśli nie istnieje jawna konwersja z `E` na `T`, wystąpi błąd w czasie trwania powiązania. W przeciwnym razie wynik jest wartością wygenerowaną przez konwersję jawną. Wynik jest zawsze klasyfikowany jako wartość, nawet jeśli `E` oznacza zmienną.

Gramatyka *cast_expression* prowadzi do pewnej składni niejasności. Na przykład wyrażenie `(x)-y` może być interpretowane jako *cast_expression* (rzutowanie `-y` do typu `x`) lub jako *additive_expression* połączone z *parenthesized_expression* (które oblicza wartość `x - y)`.

Aby rozwiązać *cast_expression* niejasności, istnieje następująca reguła: sekwencja jednego lub więcej *tokenów*([białych znaków](lexical-structure.md#white-space)) ujętych w nawiasy jest uważana za początek *cast_expression* tylko wtedy, gdy co najmniej jeden z następujących warunków jest spełniony:

*  Sekwencja tokenów jest poprawną gramatyką dla *typu*, ale nie dla *wyrażenia*.
*  Sekwencja tokenów jest poprawną gramatyką dla *typu*, a token bezpośrednio po nawiasach zamykających jest tokenem "`~`", tokenem "`!`", tokenem "`(`", *identyfikatorem* ([sekwencjami ucieczki znaków Unicode](lexical-structure.md#unicode-character-escape-sequences)), *literałem* ([literałów](lexical-structure.md#literals)) lub *słowem kluczowym* ([Keywords](lexical-structure.md#keywords)), z wyjątkiem `as` i `is`.

Termin "poprawna Gramatyka" oznacza tylko, że sekwencja tokenów musi być zgodna z konkretną produkcją gramatyczną. W szczególności nie należy traktować faktycznego znaczenia identyfikatorów składników. Na przykład jeśli `x` i `y` są identyfikatorami, `x.y` jest poprawną gramatyką dla typu, nawet jeśli `x.y` nie wskazuje typu.

Z poziomu reguły uściślania, która następuje, jeśli `x` i `y` są identyfikatorami, `(x)y`, `(x)(y)`i `(x)(-y)` są *cast_expression*s, ale `(x)-y` nie jest, nawet jeśli `x` identyfikuje typ. Jeśli jednak `x` jest słowem kluczowym, który identyfikuje wstępnie zdefiniowany typ (na przykład `int`), wówczas wszystkie cztery formy są *cast_expression*s (ponieważ takie słowo kluczowe nie może być wyrażeniem przez siebie).

### <a name="await-expressions"></a>Wyrażenia await

Operator await jest używany do zawieszania oceny otaczającej funkcji asynchronicznej do momentu zakończenia operacji asynchronicznej reprezentowanej przez operand.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

*Await_expression* jest dozwolony tylko w treści funkcji asynchronicznej ([Iteratory](classes.md#iterators)). W najbliższych otaczającej funkcji asynchronicznej *await_expression* mogą nie wystąpić w tych miejscach:

*  Wewnątrz zagnieżdżonej funkcji anonimowej (innej niż Async)
*  Wewnątrz bloku *lock_statement*
*  W niebezpiecznym kontekście

Należy zauważyć, że *await_expression* nie może wystąpić w większości miejsc w *query_expression*, ponieważ są składniowo przekształcone w celu użycia nieasynchronicznych wyrażeń lambda.

W funkcji asynchronicznej `await` nie może być używany jako identyfikator. W związku z tym nie istnieje niejednoznaczność składni między wyrażeniami await i różnymi wyrażeniami obejmującymi identyfikatory. Poza funkcjami asynchronicznymi, `await` pełni rolę normalnego identyfikatora.

Argument operacji *await_expression* nosi nazwę ***zadanie***. Reprezentuje operację asynchroniczną, która może być lub nie można jej zakończyć w chwili szacowania *await_expression* . Celem operatora await jest wstrzymanie wykonywania otaczającej funkcji asynchronicznej do momentu zakończenia oczekującego zadania, a następnie uzyskanie wyniku.

#### <a name="awaitable-expressions"></a>Wyrażenia oczekujące

Zadanie wyrażenia await jest wymagane, aby można było ***oczekiwać***. W wyrażeniu `t` oczekuje się, że występuje jedno z następujących elementów:

*  `t` jest typu czasu kompilacji `dynamic`
*  `t` ma dostępne wystąpienie lub metodę rozszerzającą o nazwie `GetAwaiter` bez parametrów i bez parametrów typu oraz typem zwracanym `A`, dla którego wszystkie następujące blokady:
   * `A` implementuje interfejs `System.Runtime.CompilerServices.INotifyCompletion` (dalej znany jako `INotifyCompletion` dla zwięzłości)
   * `A` ma dostępną, czytelną Właściwość wystąpienia `IsCompleted` typu `bool`
   * `A` ma dostępną metodę wystąpienia `GetResult` bez parametrów i bez parametrów typu

Celem metody `GetAwaiter` jest uzyskanie ***oczekiwania*** dla zadania. Typ `A` jest nazywany ***typem await*** dla wyrażenia await.

Właściwość `IsCompleted` ma na celu określenie, czy zadanie zostało już ukończone. Jeśli tak, nie ma potrzeby wstrzymania oceny.

Celem `INotifyCompletion.OnCompleted` jest podpisywanie "kontynuacji" do zadania; tj. delegat (typu `System.Action`), który zostanie wywołany po zakończeniu zadania.

Celem metody `GetResult` jest uzyskanie wyniku zadania po jego zakończeniu. Ten wynik może powieść się, prawdopodobnie z wartością wyniku lub może być wyjątek, który jest generowany przez metodę `GetResult`.

#### <a name="classification-of-await-expressions"></a>Klasyfikacja wyrażeń await

Wyrażenie `await t` jest sklasyfikowane tak samo jak w przypadku `(t).GetAwaiter().GetResult()`wyrażenia. W takim przypadku, jeśli typ zwracany `GetResult` jest `void`, *await_expression* jest sklasyfikowany jako Nothing. Jeśli ma typ zwracany inny niż void `T`, *await_expression* zostanie sklasyfikowany jako wartość typu `T`.

#### <a name="runtime-evaluation-of-await-expressions"></a>Ocena czasu wykonywania dla wyrażeń await

W czasie wykonywania wyrażenie `await t` jest oceniane w następujący sposób:

*  `a` oczekiwania jest uzyskiwany przez obliczenie `(t).GetAwaiter()`wyrażenia.
*  `b` `bool` jest uzyskiwany poprzez obliczenie `(a).IsCompleted`wyrażenia.
*  Jeśli `b` jest `false`, Ocena zależy od tego, czy `a` implementuje interfejs `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (dalej znany jako `ICriticalNotifyCompletion` for zwięzłości). To sprawdzanie jest wykonywane w czasie wiązania; tj. w czasie wykonywania, jeśli `a` ma typ czasu kompilacji `dynamic`i w przeciwnym razie w czasie kompilacji. Niech `r` oznacza delegata wznawiania ([Iteratory](classes.md#iterators)):
    * Jeśli `a` nie implementuje `ICriticalNotifyCompletion`, zostanie obliczone wyrażenie `(a as (INotifyCompletion)).OnCompleted(r)`.
    * Jeśli `a` implementuje `ICriticalNotifyCompletion`, zostanie obliczone wyrażenie `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)`.
    * Szacowanie jest zawieszone, a sterowanie jest zwracane do bieżącego obiektu wywołującego funkcji asynchronicznej.
*  Bezpośrednio po (jeśli `b` była `true`) lub po późniejszym wywołaniu delegata wznawiania (jeśli `b` była `false`) zostanie obliczone wyrażenie `(a).GetResult()`. Jeśli zwraca wartość, ta wartość jest wynikiem *await_expression*. W przeciwnym razie wynik nie będzie miał żadnego skutku.

Implementacja metody interfejsu w programie await `INotifyCompletion.OnCompleted` i `ICriticalNotifyCompletion.UnsafeOnCompleted` powinna spowodować, że delegat `r` być wywoływany najwyżej raz. W przeciwnym razie zachowanie otaczającej funkcji asynchronicznej jest niezdefiniowane.

## <a name="arithmetic-operators"></a>Operatory arytmetyczne

Operatory `*`, `/`, `%`, `+`i `-` są określane jako operatory arytmetyczne.

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

Jeśli argument operacji operatora arytmetycznego ma typ czasu kompilacji `dynamic`, wyrażenie jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)). W takim przypadku typ czasu kompilacji wyrażenia jest `dynamic`, a rozwiązanie opisane poniżej będzie odbywać się w czasie wykonywania przy użyciu typu czasu wykonywania tych operandów, które mają `dynamic`typu kompilacja.

### <a name="multiplication-operator"></a>Operator mnożenia

Aby można było wykonać operację `x * y`, do wybrania implementacji określonego operatora jest stosowana metoda rozpoznawania przeciążenia operatora binarnego ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)). Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.

Poniżej przedstawiono wstępnie zdefiniowane operatory mnożenia. Operator All Oblicza iloczyn `x` i `y`.

*  Mnożenie całkowite:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   W kontekście `checked`, jeśli produkt znajduje się poza zakresem typu wyników, zostanie zgłoszony `System.OverflowException`. W kontekście `unchecked` przepełnienia nie są raportowane i wszelkie znaczące bity o dużej kolejności poza zakresem typu wynik są odrzucane.


*  Mnożenie zmiennoprzecinkowe:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   Produkt jest obliczany zgodnie z regułami arytmetycznymi IEEE 754. W poniższej tabeli przedstawiono wyniki wszystkich możliwych kombinacji niezerowych wartości, zer, nieskończoności i NaN. W tabeli `x` i `y` są dodatnimi skończonemi wartościami. `z` jest wynikiem `x * y`. Jeśli wynik jest za duży dla typu docelowego, `z` jest nieskończoność. Jeśli wynik jest za mały dla typu docelowego, `z` wynosi zero.

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | \+ y   | -y   | +0  | -0  | \+ plik inf | -inf | {1&gt;NaN&lt;1} | 
   | {1&gt;+&lt;1}x   | +z   | -z   | +0  | -0  | \+ plik inf | -inf | {1&gt;NaN&lt;1} | 
   | {1&gt;-&lt;1}x   | -z   | +z   | -0  | +0  | -inf | \+ plik inf | {1&gt;NaN&lt;1} | 
   | +0   | +0   | -0   | +0  | -0  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 
   | -0   | -0   | +0   | -0  | +0  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 
   | \+ plik inf | \+ plik inf | -inf | {1&gt;NaN&lt;1} | {1&gt;NaN&lt;1} | \+ plik inf | -inf | {1&gt;NaN&lt;1} | 
   | -inf | -inf | \+ plik inf | {1&gt;NaN&lt;1} | {1&gt;NaN&lt;1} | -inf | \+ plik inf | {1&gt;NaN&lt;1} | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | {1&gt;NaN&lt;1} | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 

*  Mnożenie dziesiętne:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Jeśli wartość wyniku jest zbyt duża, aby reprezentować w formacie `decimal`, zostanie zgłoszony `System.OverflowException`. Jeśli wartość wynikowa jest zbyt mała do reprezentowania w formacie `decimal`, wynik wynosi zero. Skala wyniku, przed zaokrągleniem, jest sumą skali dwóch operandów.

   Mnożenie dziesiętne jest równoważne użyciu operator mnożenia typu `System.Decimal`.


### <a name="division-operator"></a>Operator dzielenia

Aby można było wykonać operację `x / y`, do wybrania implementacji określonego operatora jest stosowana metoda rozpoznawania przeciążenia operatora binarnego ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)). Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.

Poniżej znajdują się wstępnie zdefiniowane operatory dzielenia. Operator All Oblicza iloraz `x` i `y`.

*  Dzielenie liczb całkowitych:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Jeśli wartość argumentu po prawej stronie jest równa zero, zostanie zgłoszony `System.DivideByZeroException`.

   Podział zaokrągla wynik do zera. W związku z tym wartość bezwzględna wyniku to największa możliwa liczba całkowita, która jest mniejsza lub równa wartości bezwzględnej ilorazu dwóch operandów. Wynik ma wartość zero lub wartość dodatnią, gdy dwa operandy mają ten sam znak i zero lub wartość ujemną, gdy dwa operandy mają przeciwne znaki.

   Jeśli Lewy argument operacji jest najmniejszym możliwym do zaprezentowania `int` lub `long` wartość, a prawy operand jest `-1`, występuje przepełnienie. W kontekście `checked` powoduje to `System.ArithmeticException` (lub jego podklasę) do wyrzucania. W kontekście `unchecked` jest to zdefiniowane przez implementację w zależności od tego, czy `System.ArithmeticException` (lub podklasa tego elementu) jest zgłaszany, czy przepełnienie zostanie nieraportowane z wartością uzyskaną dla lewego operandu.

*  Dzielenie zmiennoprzecinkowe:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   Iloraz jest obliczany zgodnie z regułami arytmetycznymi IEEE 754. W poniższej tabeli przedstawiono wyniki wszystkich możliwych kombinacji niezerowych wartości, zer, nieskończoności i NaN. W tabeli `x` i `y` są dodatnimi skończonemi wartościami. `z` jest wynikiem `x / y`. Jeśli wynik jest za duży dla typu docelowego, `z` jest nieskończoność. Jeśli wynik jest za mały dla typu docelowego, `z` wynosi zero.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | \+ y   | -y   | +0   | -0   | \+ plik inf | -inf | {1&gt;NaN&lt;1}  | 
   | {1&gt;+&lt;1}x   | +z   | -z   | \+ plik inf | -inf | +0   | -0   | {1&gt;NaN&lt;1}  | 
   | {1&gt;-&lt;1}x   | -z   | +z   | -inf | \+ plik inf | -0   | +0   | {1&gt;NaN&lt;1}  | 
   | +0   | +0   | -0   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | +0   | -0   | {1&gt;NaN&lt;1}  | 
   | -0   | -0   | +0   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | -0   | +0   | {1&gt;NaN&lt;1}  | 
   | \+ plik inf | \+ plik inf | -inf | \+ plik inf | -inf | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | -inf | -inf | \+ plik inf | -inf | \+ plik inf | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 

*  Dzielenie dziesiętne:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Jeśli wartość argumentu po prawej stronie jest równa zero, zostanie zgłoszony `System.DivideByZeroException`. Jeśli wartość wyniku jest zbyt duża, aby reprezentować w formacie `decimal`, zostanie zgłoszony `System.OverflowException`. Jeśli wartość wynikowa jest zbyt mała do reprezentowania w formacie `decimal`, wynik wynosi zero. Skala wyniku jest najmniejszą skalą, która będzie zachować wynik równy najbliższej wartości dziesiętnej możliwej do zaprezentowania do rzeczywistego wyniku matematycznego.

   Dzielenie dziesiętne jest równoważne użyciu operator dzielenia typu `System.Decimal`.


### <a name="remainder-operator"></a>Operator reszty

Aby można było wykonać operację `x % y`, do wybrania implementacji określonego operatora jest stosowana metoda rozpoznawania przeciążenia operatora binarnego ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)). Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.

Poniżej przedstawiono wstępnie zdefiniowane operatory pozostałej reszty. Operator All obliczy resztę podziału między `x` i `y`.

*  Reszta całkowita:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   Wynik `x % y` jest wartością wygenerowaną przez `x - (x / y) * y`. Jeśli `y` wynosi zero, zostanie zgłoszony `System.DivideByZeroException`.

   Jeśli Lewy argument operacji jest najmniejszą wartością `int` lub `long`, a prawy operand jest `-1`, zostanie zgłoszony `System.OverflowException`. W żadnym przypadku `x % y` zgłosić wyjątek, w którym `x / y` nie zgłosić wyjątku.

*  Pozostała liczba zmiennoprzecinkowa:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   W poniższej tabeli przedstawiono wyniki wszystkich możliwych kombinacji niezerowych wartości, zer, nieskończoności i NaN. W tabeli `x` i `y` są dodatnimi skończonemi wartościami. `z` jest wynikiem `x % y` i jest obliczany jako `x - n * y`, gdzie `n` jest największą możliwą liczbą całkowitą, która jest mniejsza lub równa `x / y`. Ta metoda przetwarzania reszty jest analogiczna do tego, który jest używany przez operandy całkowite, ale różni się od definicji IEEE 754 (w której `n` jest liczbą całkowitą najbliżej `x / y`).

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | \+ y   | -y   | +0   | -0   | \+ plik inf | -inf | {1&gt;NaN&lt;1}  | 
   | {1&gt;+&lt;1}x   | +z   | +z   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | x    | x    | {1&gt;NaN&lt;1}  | 
   | {1&gt;-&lt;1}x   | -z   | -z   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;-&lt;1}x   | {1&gt;-&lt;1}x   | {1&gt;NaN&lt;1}  | 
   | +0   | +0   | +0   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | +0   | +0   | {1&gt;NaN&lt;1}  | 
   | -0   | -0   | -0   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | -0   | -0   | {1&gt;NaN&lt;1}  | 
   | \+ plik inf | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | -inf | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 

*  Reszta dziesiętna:

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   Jeśli wartość argumentu po prawej stronie jest równa zero, zostanie zgłoszony `System.DivideByZeroException`. Skala wyniku przed zaokrągleniem jest większa od skali dwóch operandów, a znak wyniku, jeśli wartość jest różna od zera, jest taka sama jak w przypadku `x`.

   Reszta dziesiętna jest równoważna z użyciem operatora pozostałej części typu `System.Decimal`.


### <a name="addition-operator"></a>Operator dodawania

Aby można było wykonać operację `x + y`, do wybrania implementacji określonego operatora jest stosowana metoda rozpoznawania przeciążenia operatora binarnego ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)). Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.

Wstępnie zdefiniowane operatory dodawania są wymienione poniżej. W przypadku typów liczbowych i wyliczeniowych wstępnie zdefiniowane operatory dodawania obliczają sumę dwóch argumentów operacji. Gdy jeden lub oba operandy są typu String, wstępnie zdefiniowane operatory dodawania łączą ciąg reprezentujący operandy.

*  Dodanie liczby całkowitej:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   W kontekście `checked`, jeśli suma jest poza zakresem typu wyników, zostanie zgłoszony `System.OverflowException`. W kontekście `unchecked` przepełnienia nie są raportowane i wszelkie znaczące bity o dużej kolejności poza zakresem typu wynik są odrzucane.

*  Dodawanie zmiennoprzecinkowe:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   Suma jest obliczana zgodnie z regułami arytmetycznymi IEEE 754. W poniższej tabeli przedstawiono wyniki wszystkich możliwych kombinacji niezerowych wartości, zer, nieskończoności i NaN. W tabeli `x` i `y` są wartość różną od zera, a `z` jest wynikiem `x + y`. Jeśli `x` i `y` mają taką samą wartość, ale przeciwległe znaki, `z` to dodatnia wartość zero. Jeśli `x + y` jest zbyt duża do reprezentowania w typie docelowym, `z` jest nieskończoność z tym samym znakiem, co `x + y`.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | {1&gt;y&lt;1}    | +0   | -0   | \+ plik inf | -inf | {1&gt;NaN&lt;1}  | 
   | x    | {1&gt;z&lt;1}    | x    | x    | \+ plik inf | -inf | {1&gt;NaN&lt;1}  | 
   | +0   | {1&gt;y&lt;1}    | +0   | +0   | \+ plik inf | -inf | {1&gt;NaN&lt;1}  | 
   | -0   | {1&gt;y&lt;1}    | +0   | -0   | \+ plik inf | -inf | {1&gt;NaN&lt;1}  | 
   | \+ plik inf | \+ plik inf | \+ plik inf | \+ plik inf | \+ plik inf | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | -inf | -inf | -inf | -inf | {1&gt;NaN&lt;1}  | -inf | {1&gt;NaN&lt;1}  | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 

*  Dodawanie dziesiętne:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Jeśli wartość wyniku jest zbyt duża, aby reprezentować w formacie `decimal`, zostanie zgłoszony `System.OverflowException`. Skala wyniku przed zaokrąglaniem jest większym skalą dwóch operandów.

   Dodawanie dziesiętne jest równoznaczne z użyciem operatora dodawania typu `System.Decimal`.

*  Dodawanie wyliczenia. Każdy typ wyliczeniowy niejawnie udostępnia następujące wstępnie zdefiniowane operatory, gdzie `E` jest typem wyliczenia, a `U` jest podstawowym typem `E`:

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   W czasie wykonywania te operatory są oceniane dokładnie jako `(E)((U)x + (U)y)`.

*  Łączenie ciągów:

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   Te przeciążenia operatora `+` binarnego wykonują łączenie ciągów. Jeśli argument operacji łączenia ciągów jest `null`, zostanie zastąpiony pusty ciąg. W przeciwnym razie dowolny argument niebędący ciągiem jest konwertowany na reprezentację ciągu przez wywoływanie metody wirtualnej `ToString` dziedziczonej z typu `object`. Jeśli `ToString` zwraca `null`, zostanie zastąpiony pusty ciąg.

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   Wynik operatora łączenia ciągów jest ciągiem zawierającym znaki operandu po lewej stronie, po którym następuje znak operandu z prawej strony. Operator łączenia ciągów nigdy nie zwraca wartości `null`. Jeśli nie jest dostępna wystarczająca ilość pamięci do przydzielenia ciągu powstałego, może zostać zgłoszony `System.OutOfMemoryException`.

*  Delegowanie kombinacji. Każdy typ delegata niejawnie udostępnia następujący wstępnie zdefiniowany operator, gdzie `D` jest typem delegata:

   ```csharp
   D operator +(D x, D y);
   ```

   Operator `+` binarnych wykonuje kombinację delegata, gdy oba operandy mają typ delegata `D`. (Jeśli operandy mają różne typy delegatów, występuje błąd czasu powiązania). Jeśli pierwszy operand jest `null`, wynik operacji jest wartością drugiego operandu (nawet jeśli jest również `null`). W przeciwnym razie, jeśli drugi operand jest `null`, wynikiem operacji jest wartość pierwszego operandu. W przeciwnym razie wynik operacji jest nowym wystąpieniem delegata, które wywołuje pierwszy operand, a następnie wywołuje drugi operand. Aby zapoznać się z przykładami kombinacji delegatów, zobacz [operator odejmowania](expressions.md#subtraction-operator) i [delegowanie wywołania](delegates.md#delegate-invocation). Ponieważ `System.Delegate` nie jest typem delegata, `operator` `+` nie jest dla niego zdefiniowany.

### <a name="subtraction-operator"></a>Operator odejmowania

Aby można było wykonać operację `x - y`, do wybrania implementacji określonego operatora jest stosowana metoda rozpoznawania przeciążenia operatora binarnego ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)). Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.

Poniżej przedstawiono wstępnie zdefiniowane operatory odejmowania. Operatory wszystkie odjęcie `y` od `x`.

*  Odejmowanie całkowite:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   W kontekście `checked`, jeśli różnica jest poza zakresem typu wyników, generowany jest `System.OverflowException`. W kontekście `unchecked` przepełnienia nie są raportowane i wszelkie znaczące bity o dużej kolejności poza zakresem typu wynik są odrzucane.

*  Odejmowanie zmiennoprzecinkowe:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   Różnica jest obliczana zgodnie z regułami arytmetycznymi IEEE 754. W poniższej tabeli przedstawiono wyniki wszystkich możliwych kombinacji niezerowych wartości, zer, nieskończoności i NaNs. W tabeli `x` i `y` są wartość różną od zera, a `z` jest wynikiem `x - y`. Jeśli `x` i `y` są równe, `z` jest dodatnią wartością zerową. Jeśli `x - y` jest zbyt duża do reprezentowania w typie docelowym, `z` jest nieskończoność z tym samym znakiem, co `x - y`.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | {1&gt;y&lt;1}    | +0   | -0   | \+ plik inf | -inf | {1&gt;NaN&lt;1} | 
   | x    | {1&gt;z&lt;1}    | x    | x    | -inf | \+ plik inf | {1&gt;NaN&lt;1} | 
   | +0   | -y   | +0   | +0   | -inf | \+ plik inf | {1&gt;NaN&lt;1} | 
   | -0   | -y   | -0   | +0   | -inf | \+ plik inf | {1&gt;NaN&lt;1} | 
   | \+ plik inf | \+ plik inf | \+ plik inf | \+ plik inf | {1&gt;NaN&lt;1}  | \+ plik inf | {1&gt;NaN&lt;1} | 
   | -inf | -inf | -inf | -inf | -inf | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 

*  Odejmowanie dziesiętne:

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   Jeśli wartość wyniku jest zbyt duża, aby reprezentować w formacie `decimal`, zostanie zgłoszony `System.OverflowException`. Skala wyniku przed zaokrąglaniem jest większym skalą dwóch operandów.

   Odejmowanie dziesiętne jest równoważne użyciu operator odejmowania typu `System.Decimal`.

*  Odejmowanie wyliczenia. Każdy typ wyliczeniowy niejawnie udostępnia następujący wstępnie zdefiniowany operator, gdzie `E` jest typem wyliczenia, a `U` jest podstawowym typem `E`:

   ```csharp
   U operator -(E x, E y);
   ```

   Ten operator jest oceniany dokładnie jako `(U)((U)x - (U)y)`. Innymi słowy operator oblicza różnicę między wartościami porządkowymi `x` i `y`, a typ wyniku jest podstawowym typem wyliczenia.

   ```csharp
   E operator -(E x, U y);
   ```

   Ten operator jest oceniany dokładnie jako `(E)((U)x - y)`. Innymi słowy, operator odejmuje wartość od podstawowego typu wyliczenia, która zwraca wartość wyliczenia.

*  Usuwanie delegata. Każdy typ delegata niejawnie udostępnia następujący wstępnie zdefiniowany operator, gdzie `D` jest typem delegata:

   ```csharp
   D operator -(D x, D y);
   ```

   Operator `-` binarnych wykonuje usuwanie delegata, gdy oba operandy mają typ delegata `D`. Jeśli operandy mają różne typy delegatów, wystąpi błąd w czasie trwania powiązania. Jeśli pierwszy operand jest `null`, wynik operacji jest `null`. W przeciwnym razie, jeśli drugi operand jest `null`, wynikiem operacji jest wartość pierwszego operandu. W przeciwnym razie oba operandy reprezentują listy wywołań ([deklaracje delegatów](delegates.md#delegate-declarations)), które mają co najmniej jeden wpis, a wynik jest nową listą wywołania składającą się z listy pierwszego operandu, z której usunięto wpisy drugiego operandu, pod warunkiem, że lista drugi operand jest właściwą ciągłą podlistą pierwszego elementu.     (Aby określić równość podlistów, odpowiednie wpisy są porównywane jako dla operatora równości delegata ([operatorzy równości](expressions.md#delegate-equality-operators)). W przeciwnym razie wynik jest wartością operandu po lewej stronie. Żadna z list argumentów operacji nie została zmieniona w procesie. Jeśli drugi operand jest zgodny z wieloma podlistami ciągłego wpisów na liście pierwszego operandu, zostanie usunięta podlista z prawej strony. Jeśli usunięcie spowoduje powstanie pustej listy, wynik jest `null`. Na przykład:

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a>Operatory przesunięcia

Operatory `<<` i `>>` służą do wykonywania operacji przesunięcia bitowego.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

Jeśli argument operacji *shift_expression* ma typ czasu kompilacji `dynamic`, wyrażenie jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)). W takim przypadku typ czasu kompilacji wyrażenia jest `dynamic`, a rozwiązanie opisane poniżej będzie odbywać się w czasie wykonywania przy użyciu typu czasu wykonywania tych operandów, które mają `dynamic`typu kompilacja.

W przypadku operacji `x << count` lub `x >> count`, rozpoznawanie przeciążenia operatora binarnego ([rozpoznawanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowane w celu wybrania implementacji określonego operatora. Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.

Podczas deklarowania przeciążonego operatora przesunięcia typ pierwszego operandu musi być zawsze klasą lub strukturą zawierającą deklarację operatora, a typ drugiego operandu musi być zawsze `int`.

Poniżej przedstawiono wstępnie zdefiniowane operatory przesunięcia.

*  Przesuń w lewo:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   Operator `<<` przesuwa `x` pozostawione przez liczbę bitów obliczanych w sposób opisany poniżej.

   Bity o dużej kolejności poza zakresem wyników `x` są odrzucane, pozostałe bity są przesunięte w lewo, a puste pozycje bitu w kolejności są ustawione na zero.

*  Przesuń w prawo:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   Operator `>>` przesuwa `x` bezpośrednio przez liczbę bitów obliczanych w sposób opisany poniżej.

   Gdy `x` jest typu `int` lub `long`, `x` bity o niskiej kolejności są odrzucane, pozostałe bity są przesunięte w prawo, a puste pozycje bitowe są ustawiane na zero, jeśli `x` nie są ujemne i są ustawione na jeden, jeśli `x` jest ujemna.

   Gdy `x` jest typu `uint` lub `ulong`, `x` bity o niskiej kolejności są odrzucane, pozostałe bity są przesunięte w prawo, a puste pozycje bitu w kolejności są ustawione na zero.

Dla wstępnie zdefiniowanych operatorów liczba bitów do przesunięcia jest obliczana w następujący sposób:

*  Gdy typ `x` jest `int` lub `uint`, liczba przesunięć jest określona przez pięć bitów `count`. Innymi słowy, licznik przesunięć jest obliczany na podstawie `count & 0x1F`.
*  Gdy typ `x` jest `long` lub `ulong`, liczba przesunięć jest podawana w niskiej kolejności dla `count`. Innymi słowy, licznik przesunięć jest obliczany na podstawie `count & 0x3F`.

Jeśli wynikowa liczba przesunięć jest równa zero, operatory przesunięcia po prostu zwracają wartość `x`.

Operacje przesunięcia nigdy nie powodują przepełniania i tworzą te same wyniki w kontekście `checked` i `unchecked`.

Gdy argument operacji po lewej stronie operatora `>>` ma typ całkowity ze znakiem, operator wykonuje arytmetyczne przesunięcie w prawo, co powoduje, że wartość najbardziej znaczącego bitu (bit znaku) operandu jest propagowana do pustych pozycji w kolejności całkowitej. Gdy argument operacji po lewej stronie operatora `>>` jest typu całkowitego bez znaku, operator wykonuje logiczne przesunięcie w prawo, co powoduje, że puste pozycje bitu o wysokiej kolejności są zawsze ustawione na zero. Aby wykonać odwrotną operację, która została wywnioskowana z typu operandu, można użyć jawnych rzutowania. Na przykład jeśli `x` jest zmienną typu `int`, operacja `unchecked((int)((uint)x >> y))` wykonuje logiczne przesunięcie w prawo od `x`.

## <a name="relational-and-type-testing-operators"></a>Operatory relacyjne i testowe typu

Operatory `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` i `as` są nazywane operatorami relacyjnymi i testowymi.

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

Operator `is` jest opisany w operatorze [is](expressions.md#the-is-operator) i operator `as` został opisany w [operatorze as](expressions.md#the-as-operator).

Operatory `==`, `!=`, `<`, `>`, `<=` i `>=` są ***operatorami porównania***.

Jeśli argument operacji operatora porównania ma typ czasu kompilacji `dynamic`, wyrażenie jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)). W takim przypadku typ czasu kompilacji wyrażenia jest `dynamic`, a rozwiązanie opisane poniżej będzie odbywać się w czasie wykonywania przy użyciu typu czasu wykonywania tych operandów, które mają `dynamic`typu kompilacja.

W przypadku operacji w formularzu `x` *op* `y`, gdzie *op* jest operatorem porównania, do wybrania implementacji określonego operatora jest stosowane rozpoznawanie przeciążenia ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)). Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.

Wstępnie zdefiniowane operatory porównania są opisane w poniższych sekcjach. Wszystkie wstępnie zdefiniowane operatory porównania zwracają wynik typu `bool`, zgodnie z opisem w poniższej tabeli.


| __Operacja__ | __wynik__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `true`, jeśli `x` jest równe `y`, `false` w przeciwnym razie                 | 
| `x != y`      | `true`, jeśli `x` nie jest równe `y`, `false` w przeciwnym razie             | 
| `x < y`       | `true`, jeśli `x` jest mniejsza niż `y`, `false` w przeciwnym razie                | 
| `x > y`       | `true`, jeśli `x` jest większa niż `y`, `false` w przeciwnym razie             | 
| `x <= y`      | `true`, jeśli `x` jest mniejsze niż lub równe `y`, `false` w przeciwnym razie    | 
| `x >= y`      | `true`, jeśli `x` jest większy lub równy `y`, `false` w przeciwnym razie | 

### <a name="integer-comparison-operators"></a>Operatory porównywania liczb całkowitych

Wstępnie zdefiniowane Operatory porównywania liczb całkowitych to:
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

Każdy z tych operatorów porównuje wartości liczbowe dwóch argumentów operacji i zwraca `bool` wartość, która wskazuje, czy określona relacja jest `true` czy `false`.

### <a name="floating-point-comparison-operators"></a>Operatory porównania zmiennoprzecinkowego

Wstępnie zdefiniowane operatory porównania zmiennoprzecinkowego to:
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

Operatory porównują operandy zgodnie z regułami standardu IEEE 754:

*  Jeśli każdy operand jest NaN, wynik jest `false` dla wszystkich operatorów z wyjątkiem `!=`, dla których wynik jest `true`. Dla każdego z dwóch operandów `x != y` zawsze daje ten sam wynik co `!(x == y)`. Jeśli jednak jeden lub oba operandy są NaN, operatory `<`, `>`, `<=`i `>=` nie generują tych samych wyników co logiczne Negacja operatora przeciwległego. Na przykład jeśli jeden z `x` i `y` ma wartość NaN, `x < y` jest `false`, ale `!(x >= y)` jest `true`.
*  Gdy żaden operand nie jest NaN, operatory porównują wartości dwóch argumentów zmiennoprzecinkowych w odniesieniu do kolejności

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   gdzie `min` i `max` to najmniejsza i największa wartość dodatnia wartości, które mogą być reprezentowane w danym formacie zmiennoprzecinkowym. Znaczące efekty tego porządku to:
   * Ujemne i dodatnie zera są uważane za równe.
   * Ujemna nieskończoność jest uznawana za mniej niż wszystkie inne wartości, ale jest równa innej nieskończoności ujemnej.
   * Nieskończoność dodatnia jest uznawana za większą niż wszystkie inne wartości, ale jest równa innej nieskończoności dodatniej.

### <a name="decimal-comparison-operators"></a>Operatory porównywania dziesiętnego

Wstępnie zdefiniowane Operatory porównywania dziesiętnego to:
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

Każdy z tych operatorów porównuje wartości liczbowe dwóch argumentów operacji dziesiętnych i zwraca `bool` wartość, która wskazuje, czy określona relacja jest `true` czy `false`. Każde porównanie dziesiętne jest równoważne użyciu odpowiadającego operatora relacyjnego lub równości typu `System.Decimal`.

### <a name="boolean-equality-operators"></a>Operatory równości wartości logicznych

Wstępnie zdefiniowane operatory równości wartości logicznych to:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

Wynik `==` jest `true`, jeśli zarówno `x`, jak i `y` są `true` lub w przypadku `x` obu `y` i `false`. W przeciwnym razie wynik jest `false`.

Wynik `!=` jest `false`, jeśli zarówno `x`, jak i `y` są `true` lub w przypadku `x` obu `y` i `false`. W przeciwnym razie wynik jest `true`. Gdy operandy są typu `bool`, operator `!=` generuje ten sam wynik jako operator `^`.

### <a name="enumeration-comparison-operators"></a>Operatory porównania wyliczenia

Każdy typ wyliczeniowy niejawnie udostępnia następujące wstępnie zdefiniowane operatory porównania:
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

Wynik oceny `x op y`, gdzie `x` i `y` są wyrażeniami typu wyliczenia `E` z typem źródłowym `U`, a `op` jest jednym z operatorów porównania, jest dokładnie taka sama jak Ocena `((U)x) op ((U)y)`. Innymi słowy operatory porównywania typów wyliczeniowych po prostu porównują zasadnicze wartości dwóch operandów.

### <a name="reference-type-equality-operators"></a>Operatory równości typów referencyjnych

Zdefiniowane operatory równości typów referencyjnych są następujące:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

Operatory zwracają wynik porównania dwóch odwołań dla równości lub nierówności.

Ponieważ wstępnie zdefiniowane operatory równości typu referencyjnego akceptują operandy typu `object`, mają zastosowanie do wszystkich typów, które nie deklarują odpowiednich `operator ==` i `operator !=` członków. Z drugiej strony, wszystkie odpowiednie operatory równości zdefiniowane przez użytkownika skutecznie ukrywają wstępnie zdefiniowane operatory równości typów odwołań.

Operatory równości wstępnie zdefiniowanego typu referencyjnego wymagają jednego z następujących elementów:

*  Oba operandy są wartością typu znanym jako *reference_type* lub `null`literału. Ponadto jawna konwersja odwołań ([jawne konwersje odwołań](conversions.md#explicit-reference-conversions)) istnieje z typu każdego operandu do typu innego operandu.
*  Jeden operand jest wartością typu `T`, gdzie `T` jest *type_parameter* , a drugi operand jest `null`literału. Ponadto `T` nie ma ograniczenia typu wartości.

Jeśli jeden z tych warunków nie jest spełniony, wystąpi błąd w czasie trwania powiązania. Istotne implikacje tych reguł są następujące:

*  Jest to błąd czasu powiązania, który służy do porównywania dwóch odwołań, które są znane jako różne w czasie trwania powiązania. Na przykład, jeśli typy czasu powiązania operandów są dwoma typami klas `A` i `B`, a jeśli żadna z nich nie `A` ani nie `B`, wówczas nie byłoby możliwe, aby dwa operandy odwołują się do tego samego obiektu. W rezultacie operacja jest traktowana jako błąd czasu powiązania.
*  Wstępnie zdefiniowane operatory równości typu referencyjnego nie zezwalają na porównywanie operandów typu wartości. W związku z tym, chyba że typ struktury deklaruje własne operatory równości, nie można porównać wartości tego typu struktury.
*  Operatory równości typu referencyjnego ze wstępnie zdefiniowanymi odwołaniami nigdy nie powodują wykonywania operacji opakowywania dla ich argumentów operacji. Nie będzie to miało znaczenia w przypadku wykonywania takich operacji pakowania, ponieważ odwołania do nowo przydzielonych wystąpień opakowanych będą się różnić od wszystkich innych odwołań.
*  Jeśli argument operacji typu parametru typu `T` jest porównywany z `null`, a typ czasu wykonywania `T` jest typem wartości, wynik porównania jest `false`.

Poniższy przykład sprawdza, czy argument nieograniczonego typu parametru typu jest `null`.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

Konstrukcja `x == null` jest dozwolona nawet wtedy, gdy `T` może reprezentować typ wartości, a wynik jest po prostu zdefiniowany do `false`, gdy `T` jest typem wartości.

W przypadku operacji w postaci `x == y` lub `x != y`, jeśli istnieją odpowiednie `operator ==` lub `operator !=`, reguły rozpoznawania przeciążenia operatora ([rozpoznawania przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) będą wybierać ten operator zamiast wstępnie zdefiniowanego operatora równości typu odwołania. Jednak zawsze jest możliwe wybranie wstępnie zdefiniowanego operatora równości typów odwołań przez jawne rzutowanie jednego lub obu operandów na typ `object`. Przykład
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
tworzy dane wyjściowe
```console
True
False
False
False
```

Zmienne `s` i `t` odnoszą się do dwóch odrębnych `string` wystąpień zawierających te same znaki. Pierwsze wyniki porównania `True` ze względu na to, że wstępnie zdefiniowany operator równości ciągów ([Operatory równości ciągów](expressions.md#string-equality-operators)) jest wybierany, gdy oba operandy są typu `string`. Pozostałe porównuje wszystkie `False` wyjściowe, ponieważ jest wybierany wstępnie zdefiniowany operator równości typów referencyjnych, gdy jeden lub oba operandy są typu `object`.

Należy zauważyć, że powyższa technika nie jest istotna dla typów wartości. Przykład
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
dane wyjściowe `False` ponieważ rzutowania tworzą odwołania do dwóch oddzielnych wystąpień wartości `int` opakowanych.

### <a name="string-equality-operators"></a>Operatory równości ciągów

Wstępnie zdefiniowane operatory równości ciągów są:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

Dwie `string` wartości są uważane za równe, gdy spełniony jest jeden z następujących warunków:

*  Obie wartości są `null`.
*  Obie wartości są odwołaniami o wartościach innych niż null do wystąpień ciągów, które mają identyczne długości i identyczne znaki w każdej pozycji znaku.

Operatory równości ciągów porównują wartości ciągu, a nie odwołania do ciągu. Gdy dwa oddzielne wystąpienia ciągu zawierają dokładnie tę samą sekwencję znaków, wartości ciągów są równe, ale odwołania są różne. Zgodnie z opisem w [operatorach równości typu referencyjnego](expressions.md#reference-type-equality-operators)operatory równości typu odwołania mogą służyć do porównywania odwołań ciągów zamiast wartości ciągów.

### <a name="delegate-equality-operators"></a>Deleguj operatory równości

Każdy typ delegata niejawnie udostępnia następujące wstępnie zdefiniowane operatory porównania:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

Dwa wystąpienia delegatów są uważane za równe w następujący sposób:

*  Jeśli jedno z wystąpień delegatów jest `null`, są równe, tylko wtedy, gdy oba są `null`.
*  Jeśli Delegaty mają różne typy czasu wykonywania, nigdy nie są równe.
*  Jeśli oba wystąpienia delegatów mają listę wywołań ([deklaracje delegatów](delegates.md#delegate-declarations)), te wystąpienia są równe, gdy i tylko wtedy, gdy ich listy wywołań są takie same, a każdy wpis na liście wywołań jednego z nich jest równy (jak zdefiniowano poniżej) do odpowiadającego wpisu w podanej kolejności na liście wywołań drugiej.

Następujące reguły określają równość wpisów listy wywołań:

*  Jeśli dwa wpisy listy wywołań odnoszą się do tej samej metody statycznej, wpisy są równe.
*  Jeśli dwa wywołania listy wywołań odnoszą się do tej samej metody niestatycznej w tym samym obiekcie docelowym (zgodnie z definicją przez operatory równości odwołań), wpisy są równe.
*  Wpisy listy wywołań uzyskane z oceny semantycznie identyczne *anonymous_method_expression*s lub *lambda_expression*s z tym samym (prawdopodobnie pustym) zestawem przechwyconych zewnętrznych wystąpień zmiennych są dozwolone (ale nie są wymagane), aby były równe.

### <a name="equality-operators-and-null"></a>Operatory równości i null

Operatory `==` i `!=` zezwalają jednemu operandowi na wartość typu null, a drugi jako literał `null`, nawet jeśli nie istnieje zdefiniowany lub zdefiniowany przez użytkownika operator (w niepodniesionym lub zniesionym formularzu) dla tej operacji.

Dla operacji jednego z formularzy
```csharp
x == null
null == x
x != null
null != x
```
gdzie `x` jest wyrażeniem typu dopuszczającego wartość null, jeśli nie można znaleźć odpowiedniego operatora w rozpoznaniu przeciążenia operatora ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)), wynik jest obliczany na podstawie właściwości `HasValue` `x`. W każdym przypadku pierwsze dwa formularze są tłumaczone na `!x.HasValue`, a ostatnie dwa formularze są tłumaczone na `x.HasValue`.

### <a name="the-is-operator"></a>Operator is

Operator `is` jest używany do dynamicznego sprawdzenia, czy typ obiektu w czasie wykonywania jest zgodny z danym typem. Wynik `E is T`operacji, gdzie `E` jest wyrażeniem, a `T` jest typem, jest wartością logiczną wskazującą, czy `E` można pomyślnie skonwertować do typu `T` przez konwersję odwołania, konwersję z opakowaniem lub konwersję rozpakowywanie. Operacja jest szacowana w następujący sposób, gdy argumenty typu zostały zastąpione dla wszystkich parametrów typu:

*  Jeśli `E` jest funkcją anonimową, wystąpi błąd w czasie kompilacji
*  Jeśli `E` jest grupą metod lub literałem `null`, jeśli typ `E` jest typem referencyjnym lub typem dopuszczającym wartość null, a wartością `E` jest zero, wynik ma wartość false.
*  W przeciwnym razie niech `D` reprezentuje typ dynamiczny `E` w następujący sposób:
   * Jeśli typ `E` jest typem referencyjnym, `D` jest typem czasu wykonywania odwołania wystąpienia przez `E`.
   * Jeśli typ `E` jest typem dopuszczającym wartość null, `D` jest typem podstawowym tego typu dopuszczającego wartość null.
   * Jeśli typ `E` jest typem wartości niedopuszczających wartości null, `D` jest typem `E`.
*  Wynik operacji zależy od `D` i `T` w następujący sposób:
   * Jeśli `T` jest typem referencyjnym, wynik ma wartość true, jeśli `D` i `T` są tego samego typu, jeśli `D` jest typem referencyjnym i niejawną konwersją referencyjną z `D` do `T` istnieje, lub jeśli `D` jest typem wartości, a konwersja z `D` do `T` istnieje.
   * Jeśli `T` jest typem dopuszczającym wartość null, wynik ma wartość true, jeśli `D` jest typem źródłowym `T`.
   * Jeśli `T` jest typem wartości niedopuszczających wartości null, wynik ma wartość true, jeśli `D` i `T` są tego samego typu.
   * W przeciwnym razie wynik ma wartość false.

Należy zauważyć, że konwersje zdefiniowane przez użytkownika nie są uznawane za przez operator `is`.

### <a name="the-as-operator"></a>Operator as

Operator `as` jest używany do jawnej konwersji wartości na dany typ referencyjny lub typ dopuszczający wartość null. W przeciwieństwie do wyrażenia CAST ([wyrażenia rzutowania](expressions.md#cast-expressions)) operator `as` nigdy nie zgłasza wyjątku. Zamiast tego, jeśli wskazana konwersja nie jest możliwa, obliczona wartość jest `null`.

W operacji `E as T`formularza `E` musi być wyrażeniem, a `T` musi być typem referencyjnym, parametrem typu znanym jako typ referencyjny lub typem dopuszczającym wartość null. Ponadto co najmniej jeden z następujących elementów musi mieć wartość true lub w przeciwnym razie wystąpi błąd w czasie kompilacji:

*  Tożsamość ([konwersja tożsamości](conversions.md#identity-conversion)), niejawne dopuszczanie wartości null ([niejawne konwersje dopuszczające wartość null](conversions.md#implicit-nullable-conversions)), niejawne odwołanie ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions)), opakowanie ([konwersje opakowania](conversions.md#boxing-conversions)), jawne dopuszczanie wartości null ([jawne konwersje do wartości null](conversions.md#explicit-nullable-conversions)), jawne odwołanie ([jawne konwersje odwołań](conversions.md#explicit-reference-conversions)) lub odpakowywanie ([konwersje](conversions.md#unboxing-conversions)rozpakowywania) istnieje w `E` do `T`
*  Typ `E` lub `T` jest typem otwartym.
*  `E` jest `null`m literałem.

Jeśli typ czasu kompilacji `E` nie jest `dynamic`, operacja `E as T` daje ten sam wynik co
```csharp
E is T ? (T)(E) : (T)null
```
z tą różnicą, że `E` jest obliczone tylko raz. Kompilator może być oczekiwany do zoptymalizowania `E as T` wykonywania co najwyżej jednego sprawdzenia typu dynamicznego, w przeciwieństwie do dwóch kontroli typu dynamicznego IMPLIKOWANYCH przez rozszerzenie powyżej.

Jeśli typ czasu kompilacji `E` jest `dynamic`, w przeciwieństwie do operatora rzutowania, operator `as` nie jest powiązany dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)). W związku z tym rozwinięcie w tym przypadku jest następujące:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Należy zauważyć, że niektóre konwersje, takie jak konwersje zdefiniowane przez użytkownika, nie są możliwe za pomocą operatora `as` i powinny być wykonywane przy użyciu wyrażeń rzutowania.

W przykładzie
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
parametr typu `T` `G` jest znany jako typ referencyjny, ponieważ ma ograniczenie klasy. Nie ma jednak `U` parametru typu `H`; w związku z tym użycie operatora `as` w `H` jest niedozwolone.

## <a name="logical-operators"></a>Operatory logiczne

Operatory `&`, `^`i `|` są nazywane operatorami logicznymi.

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

Jeśli argument operacji operatora logicznego ma typ czasu kompilacji `dynamic`, wyrażenie jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)). W takim przypadku typ czasu kompilacji wyrażenia jest `dynamic`, a rozwiązanie opisane poniżej będzie odbywać się w czasie wykonywania przy użyciu typu czasu wykonywania tych operandów, które mają `dynamic`typu kompilacja.

Aby można było wykonać operację `x op y`, gdzie `op` jest jednym z operatorów logicznych, do wybrania implementacji określonego operatora jest stosowana metoda rozpoznawania przeciążenia ([rozpoznawanie przeciążeń operatora binarnego](expressions.md#binary-operator-overload-resolution)). Operandy są konwertowane na typy parametrów wybranego operatora, a typ wyniku jest typem zwracanym operatora.

Wstępnie zdefiniowane operatory logiczne są opisane w poniższych sekcjach.

### <a name="integer-logical-operators"></a>Operatory logiczne Integer

Wstępnie zdefiniowane operatory logiczne integer są:
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

Operator `&` oblicza bitowe logiczne `AND` dwóch operandów, operator `|` oblicza bitowe logiczne `OR` dwóch operandów, a operator `^` oblicza bitową logiczną wyłączną `OR` dwóch operandów. Z tych operacji nie jest możliwe przepełnienie.

### <a name="enumeration-logical-operators"></a>Wyliczanie operatorów logicznych

Każdy typ wyliczeniowy `E` niejawnie udostępnia następujące wstępnie zdefiniowane operatory logiczne:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

Wynik oceny `x op y`, gdzie `x` i `y` są wyrażeniami typu wyliczenia `E` z typem źródłowym `U`, a `op` jest jednym z operatorów logicznych, jest dokładnie taka sama jak Ocena `(E)((U)x op (U)y)`. Innymi słowy operatory logiczne typu wyliczeniowy po prostu wykonują operacje logiczne na podstawowym typie dwóch argumentów operacji.

### <a name="boolean-logical-operators"></a>Operatory logiczne (Boolean)

Wstępnie zdefiniowane operatory logiczne Boolean to:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

Wynik `x & y` jest `true`, jeśli `x` i `y` są `true`. W przeciwnym razie wynik jest `false`.

Wynik `x | y` jest `true`, jeśli `x` lub `y` jest `true`. W przeciwnym razie wynik jest `false`.

Wynik `x ^ y` jest `true`, jeśli `x` jest `true` i `y` jest `false`, lub `x` jest `false` i `y` jest `true`. W przeciwnym razie wynik jest `false`. Gdy operandy są typu `bool`, operator `^` oblicza ten sam wynik jako operator `!=`.

### <a name="nullable-boolean-logical-operators"></a>Logiczne operatory wartości null

Typ Boolean dopuszczający wartość null `bool?` może reprezentować trzy wartości, `true`, `false`i `null`i jest koncepcyjnie podobny do typu, który jest używany dla wyrażeń logicznych w SQL. Aby upewnić się, że wyniki utworzone przez operatory `&` i `|` dla argumentów operacji `bool?` są spójne z logiką trzech wartości, są dostępne następujące wstępnie zdefiniowane operatory:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

Poniższa tabela zawiera listę wyników wytwarzanych przez te operatory dla wszystkich kombinacji wartości `true`, `false`i `null`.

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a>Warunkowe operatory logiczne

Operatory `&&` i `||` są nazywane warunkowymi operatorami logicznymi. Są one również nazywane operatorami logicznymi "krótkie-obwóding".

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

Operatory `&&` i `||` są warunkowymi wersjami operatorów `&` i `|`:

*  `x && y` operacji odpowiada `x & y`operacji, z tą różnicą, że `y` jest obliczana tylko wtedy, gdy `x` nie `false`.
*  `x || y` operacji odpowiada `x | y`operacji, z tą różnicą, że `y` jest obliczana tylko wtedy, gdy `x` nie `true`.

Jeśli argument operacji warunkowego operatora logicznego ma typ czasu kompilacji `dynamic`, wyrażenie jest dynamicznie powiązane ([powiązanie dynamiczne](expressions.md#dynamic-binding)). W takim przypadku typ czasu kompilacji wyrażenia jest `dynamic`, a rozwiązanie opisane poniżej będzie odbywać się w czasie wykonywania przy użyciu typu czasu wykonywania tych operandów, które mają `dynamic`typu kompilacja.

Operacja `x && y` lub `x || y` jest przetwarzana przez zastosowanie rozpoznawania przeciążenia ([przeciążanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)), tak jakby operacja była zapisywana `x & y` lub `x | y`. Następnie

*  Jeśli rozwiązanie przeciążenia nie może znaleźć pojedynczego najlepszego operatora lub w przypadku rozpoznawania przeciążenia wybiera jeden ze wstępnie zdefiniowanych wartości całkowitych, występuje błąd w czasie powiązania.
*  W przeciwnym razie, jeśli wybrany operator jest jednym ze wstępnie zdefiniowanych logicznych operatorów logicznych ([Boolean logiczne operatory](expressions.md#boolean-logical-operators)) lub logicznej wartości null operatorów logicznych (wartość[null logiczne operatorów logicznych](expressions.md#nullable-boolean-logical-operators)), operacja jest przetwarzana zgodnie z opisem w logicznych [warunkowych operatorach logicznych](expressions.md#boolean-conditional-logical-operators).
*  W przeciwnym razie wybrany operator jest operatorem zdefiniowanym przez użytkownika, a operacja jest przetwarzana zgodnie z opisem w [warunkowych operatory logicznej zdefiniowanej przez użytkownika](expressions.md#user-defined-conditional-logical-operators).

Nie można bezpośrednio przeciążać warunkowych operatorów logicznych. Jednak ze względu na to, że warunkowe operatory logiczne są oceniane pod względem zwykłych operatorów logicznych, przeciążenia zwykłych operatorów logicznych są, z pewnymi ograniczeniami, również jako przeciążenia warunkowych operatorów logicznych. Opisano to dokładniej w [warunkowych operatory logicznej zdefiniowanej przez użytkownika](expressions.md#user-defined-conditional-logical-operators).

### <a name="boolean-conditional-logical-operators"></a>Logiczne warunkowe operatory logiczne

Gdy operandy `&&` lub `||` są typu `bool`lub gdy operandy są typów, które nie definiują odpowiednich `operator &` ani `operator |`, ale definiują konwersje niejawne na `bool`, operacja jest przetwarzana w następujący sposób:

*  `x && y` operacji jest oceniane jako `x ? y : false`. Innymi słowy, `x` jest najpierw oceniane i konwertowane na `bool`typu. Następnie Jeśli `x` jest `true`, `y` jest oceniana i konwertowana na typ `bool`i zostanie to wynik operacji. W przeciwnym razie wynik operacji jest `false`.
*  `x || y` operacji jest oceniane jako `x ? true : y`. Innymi słowy, `x` jest najpierw oceniane i konwertowane na `bool`typu. Następnie Jeśli `x` jest `true`, wynik operacji jest `true`. W przeciwnym razie `y` jest oceniane i konwertowane na typ `bool`i zostanie to wynikiem operacji.

### <a name="user-defined-conditional-logical-operators"></a>Operatory logiczne warunkowe zdefiniowane przez użytkownika

Gdy operandy `&&` lub `||` są typami, które deklarują odpowiednie `operator &` zdefiniowane przez użytkownika lub `operator |`, obie poniższe wartości muszą mieć wartość true, gdzie `T` jest typem, w którym zadeklarowano wybrany operator:

*  Typ zwracany i typ każdego parametru wybranego operatora muszą być `T`. Innymi słowy operator musi obliczać `AND` logiczne lub logiczne `OR` dwóch operandów typu `T`i musi zwrócić wynik typu `T`.
*  `T` musi zawierać deklaracje `operator true` i `operator false`.

Jeśli jedno z tych wymagań nie zostanie spełnione, występuje błąd w czasie trwania powiązania. W przeciwnym razie operacja `&&` lub `||` jest Szacowana przez połączenie `operator true` zdefiniowanej przez użytkownika lub `operator false` z wybranym operatorem zdefiniowanym przez użytkownika:

*  `x && y` operacji jest oceniane jako `T.false(x) ? x : T.&(x, y)`, gdzie `T.false(x)` jest wywołaniem `operator false` zadeklarowanym w `T`, a `T.&(x, y)` to wywołanie wybranej `operator &`. Innymi słowy, `x` jest najpierw oceniane i `operator false` jest wywoływana w wyniku, aby określić, czy `x` ma wartość false. Następnie Jeśli `x` ma wartość false, wynik operacji jest wartością obliczoną wcześniej dla `x`. W przeciwnym razie `y` jest oceniane i wybrane `operator &` są wywoływane na wartości obliczonej wcześniej dla `x` oraz wartości obliczanej dla `y`, aby utworzyć wynik operacji.
*  `x || y` operacji jest oceniane jako `T.true(x) ? x : T.|(x, y)`, gdzie `T.true(x)` jest wywołaniem `operator true` zadeklarowanym w `T`, a `T.|(x,y)` to wywołanie wybranej `operator|`. Innymi słowy `x` jest najpierw oceniane i `operator true` jest wywoływana w wyniku, aby określić, czy `x` ma wartość "prawda". Następnie Jeśli `x` ma wartość "prawda", wynik operacji jest wartością obliczoną wcześniej dla `x`. W przeciwnym razie `y` jest oceniane i wybrane `operator |` są wywoływane na wartości obliczonej wcześniej dla `x` oraz wartości obliczanej dla `y`, aby utworzyć wynik operacji.

W każdej z tych operacji wyrażenie podane przez `x` jest oceniane tylko raz, a wyrażenie podane przez `y` nie jest oceniane lub oceniane dokładnie raz.

Aby zapoznać się z przykładem typu, który implementuje `operator true` i `operator false`, zobacz [typ Boolean bazy danych](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>Operator łączenia wartości null

Operator `??` jest nazywany operatorem łączenia wartości null.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

Wyrażenie łączenia o wartości null w formularzu `a ?? b` wymaga, aby `a` być typu dopuszczającego wartość null lub typ referencyjny. Jeśli `a` ma wartość różną od null, wynik `a ?? b` jest `a`; w przeciwnym razie wynik jest `b`. Operacja oblicza `b` tylko wtedy, gdy `a` ma wartość null.

Operator łączenia wartości null jest prawym przyciskiem asocjacyjnym, co oznacza, że operacje są pogrupowane od prawej do lewej. Na przykład wyrażenie `a ?? b ?? c` jest oceniane jako `a ?? (b ?? c)`. Ogólnie rzecz biorąc, wyrażenie formularza `E1 ?? E2 ?? ... ?? En` zwraca pierwsze operandy o wartości innej niż null lub wartość null, jeśli wszystkie operandy mają wartość null.

Typ wyrażenia `a ?? b` zależy od tego, które konwersje niejawne są dostępne dla operandów. W kolejności preferencji typ `a ?? b` jest `A0`, `A`lub `B`, gdzie `A` jest typem `a` (pod warunkiem, że `a` ma typ), `B` jest typem `b` (pod warunkiem, że `b` ma typ), a `A0` jest typem podstawowym `A`, jeśli `A` jest typem dopuszczającym wartość null lub `A` w przeciwnym razie. W `a ?? b` jest przetwarzana w następujący sposób:

*  Jeśli `A` istnieje i nie jest typem dopuszczającym wartość null lub typem referencyjnym, wystąpi błąd w czasie kompilacji.
*  Jeśli `b` jest wyrażeniem dynamicznym, typem wyniku jest `dynamic`. W czasie wykonywania `a` jest najpierw oceniane. Jeśli `a` nie ma wartości null, `a` zostanie przekonwertowana na dynamiczny i zostanie to wynik. W przeciwnym razie `b` jest oceniane i zostanie to wynik.
*  W przeciwnym razie, jeśli `A` istnieje i jest typem dopuszczającym wartość null, a niejawna konwersja istnieje z `b` do `A0`, typem wyniku jest `A0`. W czasie wykonywania `a` jest najpierw oceniane. Jeśli `a` nie ma wartości null, `a` jest odpakowany do typu `A0`i zostanie to wynikiem. W przeciwnym razie `b` jest oceniane i konwertowane na typ `A0`i zostanie to wynikiem.
*  W przeciwnym razie, jeśli `A` istnieje i istnieje niejawna konwersja z `b` do `A`, typem wyniku jest `A`. W czasie wykonywania `a` jest najpierw oceniane. Jeśli `a` nie ma wartości null, `a` zostanie wynikiem. W przeciwnym razie `b` jest oceniane i konwertowane na typ `A`i zostanie to wynikiem.
*  W przeciwnym razie, jeśli `b` ma typ `B` i istnieje niejawna konwersja z `a` do `B`, typem wyniku jest `B`. W czasie wykonywania `a` jest najpierw oceniane. Jeśli `a` nie ma wartości null, `a` jest odpakowany do typu `A0` (jeśli `A` istnieje i dopuszcza wartość null) i konwertowane na typ `B`i zostanie to wynik. W przeciwnym razie `b` są oceniane i staną się wynikiem.
*  W przeciwnym razie `a` i `b` są niezgodne, a wystąpi błąd w czasie kompilacji.

## <a name="conditional-operator"></a>Operator warunkowy

Operator `?:` jest nazywany operatorem warunkowym. Jest ona czasem nazywana również operatorem Trzyelementowy.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

Wyrażenie warunkowe formularza `b ? x : y` najpierw oblicza warunek `b`. Następnie Jeśli `b` jest `true`, `x` jest oceniane i zostanie wynikiem operacji. W przeciwnym razie `y` są oceniane i staną się wynikiem operacji. Wyrażenie warunkowe nigdy nie oblicza jednocześnie `x` i `y`.

Operator warunkowy jest z prawej strony asocjacyjny, co oznacza, że operacje są pogrupowane od prawej do lewej. Na przykład wyrażenie `a ? b : c ? d : e` jest oceniane jako `a ? b : (c ? d : e)`.

Pierwszy operand operatora `?:` musi być wyrażeniem, które może być niejawnie konwertowane na `bool`lub wyrażenie typu implementującego `operator true`. Jeśli żadne z tych wymagań nie są spełnione, wystąpi błąd w czasie kompilacji.

Drugi i trzeci operand, `x` i `y`, operatora `?:` sterują typem wyrażenia warunkowego.

*  Jeśli `x` ma typ `X` i `y` ma `Y` typ
   * Jeśli niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z `X` do `Y`, ale nie z `Y` do `X`, wówczas `Y` jest typem wyrażenia warunkowego.
   * Jeśli niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z `Y` do `X`, ale nie z `X` do `Y`, wówczas `X` jest typem wyrażenia warunkowego.
   * W przeciwnym razie nie można określić typu wyrażenia i występuje błąd czasu kompilacji.
*  Jeśli tylko jeden z `x` i `y` ma typ, a zarówno `x`, jak i `y`, z są niejawnie konwertowane do tego typu, to jest typem wyrażenia warunkowego.
*  W przeciwnym razie nie można określić typu wyrażenia i występuje błąd czasu kompilacji.

Przetwarzanie wyrażenia warunkowego `b ? x : y` w czasie wykonywania obejmuje następujące kroki:

*  Najpierw `b` jest oceniane i `bool` wartość `b` jest określana:
   * Jeśli niejawna konwersja z typu `b` na `bool` istnieje, to niejawna konwersja jest wykonywana w celu utworzenia wartości `bool`.
   * W przeciwnym razie `operator true` zdefiniowane przez typ `b` jest wywoływana w celu utworzenia wartości `bool`.
*  Jeśli wartość `bool` generowana przez krok powyżej jest `true`, `x` jest oceniana i konwertowana na typ wyrażenia warunkowego, a tym samym wynik wyrażenia warunkowego.
*  W przeciwnym razie `y` jest oceniane i konwertowane na typ wyrażenia warunkowego, a następnie zostanie wynikiem wyrażenia warunkowego.

## <a name="anonymous-function-expressions"></a>Wyrażenia funkcji anonimowych

***Funkcja anonimowa*** jest wyrażeniem, które reprezentuje definicję metody "w wierszu". Funkcja anonimowa nie ma wartości ani typu w i sama, ale jest możliwa do przekonwertowania na zgodny delegat lub typ drzewa wyrażenia. Obliczanie konwersji funkcji anonimowej zależy od typu docelowego konwersji: Jeśli jest to typ delegata, konwersja szacuje się na wartość delegata odwołującą się do metody, która definiuje funkcja anonimowa. Jeśli jest to typ drzewa wyrażenia, konwersja szacuje się na drzewo wyrażenia, które reprezentuje strukturę metody jako strukturę obiektu.

Z przyczyn historycznych istnieją dwa rodzaje składni funkcji anonimowych, czyli *lambda_expression*s i *anonymous_method_expression*s. Do niemal wszystkich celów *lambda_expression*s są bardziej zwięzłe i bardziej wyraźne niż *anonymous_method_expression*s, które pozostaną w języku w celu zapewnienia zgodności z poprzednimi wersjami.

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

Operator `=>` ma takie samo pierwszeństwo jak przypisanie (`=`) i jest skojarzony z prawej strony.

Funkcja anonimowa z modyfikatorem `async` jest funkcją asynchroniczną i jest zgodna z regułami opisanymi w [iteratorach](classes.md#iterators).

Parametry funkcji anonimowej w postaci *lambda_expression* mogą być jawnie lub niejawnie wpisane. Na liście jawnie wpisanych parametrów typ każdego parametru jest jawnie określony. W przypadku niejawnie wpisanej listy parametrów typy parametrów są wywnioskowane z kontekstu, w którym występuje funkcja anonimowa — w przypadku gdy funkcja anonimowa jest konwertowana na zgodny typ delegata lub typ drzewa wyrażenia, ten typ zapewnia typy parametrów ([konwersje funkcji anonimowych](conversions.md#anonymous-function-conversions)).

W funkcji anonimowej z pojedynczym niejawnie określonym parametrem, nawiasy można pominąć z listy parametrów. Innymi słowy, funkcja anonimowa formularza
```csharp
( param ) => expr
```
może być skrócony do
```csharp
param => expr
```

Lista parametrów funkcji anonimowej w postaci *anonymous_method_expression* jest opcjonalna. W przypadku podaną wartość parametry muszą być jawnie wpisane. W przeciwnym razie funkcja anonimowa jest konwertowany na delegata z dowolną listą parametrów, która nie zawiera `out` parametrów.

Treść *bloku* funkcji anonimowej jest osiągalna ([punkty końcowe i osiągalność](statements.md#end-points-and-reachability)), chyba że funkcja anonimowa występuje wewnątrz nieosiągalnej instrukcji.

Poniżej przedstawiono kilka przykładów funkcji anonimowych:

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

Zachowanie *lambda_expression*s i *anonymous_method_expression*s jest takie samo, z wyjątkiem następujących punktów:

*  *anonymous_method_expression*s zezwala na całkowite pomijanie listy parametrów, co daje Convertibility do delegowania typów dowolnej listy parametrów wartości.
*  *lambda_expression*s Zezwalaj na pomijanie typów parametrów i wywnioskowane *anonymous_method_expression*s wymaga jawnego określenia typów parametrów.
*  Treść *lambda_expression* może być wyrażeniem lub blokiem instrukcji, podczas gdy treść *anonymous_method_expression* musi być blokiem instrukcji.
*  Tylko *lambda_expression*s mają konwersje na zgodne typy drzewa wyrażeń ([Typy drzewa wyrażeń](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Sygnatury funkcji anonimowych

Opcjonalna *anonymous_function_signature* funkcji anonimowej definiuje nazwy i opcjonalnie typy parametrów formalnych dla funkcji anonimowej. Zakresem parametrów funkcji anonimowej jest *anonymous_function_body*. ([Zakresy](basic-concepts.md#scopes)) Wraz z listą parametrów (jeśli jest to określone), Metoda anonimowa ([Deklaracja) stanowi](basic-concepts.md#declarations)przestrzeń deklaracji. Jest to błąd czasu kompilacji dla nazwy parametru funkcji anonimowej, aby dopasować nazwę zmiennej lokalnej, stałej lokalnej lub parametru, którego zakres zawiera *anonymous_method_expression* lub *lambda_expression*.

Jeśli funkcja anonimowa ma *explicit_anonymous_function_signature*, zestaw zgodnych typów delegatów i typów drzewa wyrażeń jest ograniczony do tych, które mają te same typy parametrów i Modyfikatory w tej samej kolejności. W przeciwieństwie do konwersji grup metod ([konwersje grup metod](conversions.md#method-group-conversions)), antywariancja typów parametrów funkcji anonimowych nie jest obsługiwana. Jeśli funkcja anonimowa nie ma *anonymous_function_signature*, zestaw zgodnych typów delegatów i typów drzewa wyrażeń jest ograniczony do tych, które nie mają `out` parametrów.

Należy zauważyć, że *anonymous_function_signature* nie może zawierać atrybutów ani tablicy parametrów. Niemniej jednak *anonymous_function_signature* może być zgodny z typem delegata, którego lista parametrów zawiera tablicę parametrów.

Należy również pamiętać, że konwersja na typ drzewa wyrażenia, nawet jeśli jest zgodna, może nadal kończyć się niepowodzeniem w czasie kompilacji ([Typy drzewa wyrażeń](types.md#expression-tree-types)).

### <a name="anonymous-function-bodies"></a>Anonimowe treści funkcji

Treść (*wyrażenie* lub *blok*) funkcji anonimowej podlega następującym zasadom:

*  Jeśli funkcja anonimowa zawiera podpis, parametry określone w podpisie są dostępne w treści. Jeśli funkcja anonimowa nie ma podpisu, można ją przekonwertować na typ delegata lub typ wyrażenia z parametrami ([konwersje funkcji anonimowych](conversions.md#anonymous-function-conversions)), ale w treści nie można uzyskać dostępu do parametrów.
*  Z wyjątkiem `ref` lub `out` parametrów określonych w sygnaturze (jeśli istnieją) najbliższej otaczającej funkcji anonimowej, jest to błąd czasu kompilacji dla treści, aby uzyskać dostęp do parametru `ref` lub `out`.
*  Gdy typ `this` jest typem struktury, jest to błąd czasu kompilacji dla treści w celu uzyskania dostępu `this`. Jest to prawdziwe, czy dostęp jest jawny (jak w `this.x`) czy niejawny (jak w `x` gdzie `x` jest elementem członkowskim struktury). Ta zasada po prostu uniemożliwia takiemu dostępowi i nie wpływa na to, czy wyniki wyszukiwania elementów członkowskich są elementami członkowskimi struktury.
*  Treść ma dostęp do zmiennych zewnętrznych ([zmiennych zewnętrznych](expressions.md#outer-variables)) funkcji anonimowej. Dostęp do zmiennej zewnętrznej będzie odwoływać się do wystąpienia zmiennej aktywnej w chwili, gdy *lambda_expression* lub *anonymous_method_expression* jest obliczana ([Obliczanie wyrażeń funkcji anonimowych](expressions.md#evaluation-of-anonymous-function-expressions)).
*  Jest to błąd czasu kompilacji dla treści, który zawiera instrukcję `goto`, instrukcję `break` lub instrukcję `continue`, której element docelowy znajduje się poza treścią lub w treści zawartej funkcji anonimowej.
*  Instrukcja `return` w treści zwraca kontrolę z wywołania najbliższej otaczającej funkcji anonimowej, a nie z otaczającego elementu członkowskiego funkcji. Wyrażenie określone w instrukcji `return` musi być niejawnie konwertowane na typ zwracany typu delegata lub typu drzewa wyrażenia, do którego najbliższe *lambda_expression* lub *anonymous_method_expression* są konwertowane ([konwersje funkcji anonimowych](conversions.md#anonymous-function-conversions)).

Jest on jawnie nieokreślony, niezależnie od tego, czy istnieje sposób, aby wykonać blok anonimowej funkcji innej niż Ocena i wywołanie *lambda_expression* lub *anonymous_method_expression*. W szczególności kompilator może zdecydować się na zaimplementowanie funkcji anonimowej przez syntezowanie jednej lub więcej nazwanych metod lub typów. Nazwy wszelkich takich elementów, które można wyszukiwać, muszą mieć postać zastrzeżona do użycia przez kompilator.

### <a name="overload-resolution-and-anonymous-functions"></a>Rozpoznawanie przeciążenia i funkcje anonimowe

Funkcje anonimowe na liście argumentów uczestniczą w wnioskach o typie i przeciążeniu. Aby uzyskać dokładne reguły, zapoznaj się z [wnioskami o typie](expressions.md#type-inference) i [rozpoznaniem przeciążenia](expressions.md#overload-resolution) .

Poniższy przykład ilustruje efekt funkcji anonimowych podczas rozpoznawania przeciążenia.

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

Klasa `ItemList<T>` ma dwie metody `Sum`. Każdy przyjmuje `selector` argument, który wyodrębnia wartość do sumowania z elementu listy. Wyodrębniona wartość może być `int` lub `double`, a wynikiem podsumowania jest również `int` lub `double`.

Metody `Sum` mogą na przykład służyć do obliczania sum z listy wierszy szczegółów w kolejności.

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

W pierwszym wywołaniu `orderDetails.Sum`są stosowane obie metody `Sum`, ponieważ funkcja anonimowa `d => d. UnitCount` jest zgodna zarówno z `Func<Detail,int>`, jak i `Func<Detail,double>`. Jednak rozwiązanie przeciążenia wybiera pierwszą metodę `Sum`, ponieważ konwersja do `Func<Detail,int>` jest lepsza niż konwersja do `Func<Detail,double>`.

W drugim wywołaniu `orderDetails.Sum`stosowana jest tylko druga metoda `Sum`, ponieważ funkcja anonimowa `d => d.UnitPrice * d.UnitCount` generuje wartość typu `double`. W rezultacie rozwiązanie przeciążenia wybiera drugą metodę `Sum` dla tego wywołania.

### <a name="anonymous-functions-and-dynamic-binding"></a>Funkcje anonimowe i powiązanie dynamiczne

Funkcja anonimowa nie może być odbiorcą, argumentem lub argumentem operacji z dynamiczną operacją.

### <a name="outer-variables"></a>Zmienne zewnętrzne

Każda zmienna lokalna, parametr wartości lub tablica parametrów, której zakres zawiera *lambda_expression* lub *anonymous_method_expression* jest nazywana ***zewnętrzną zmienną*** funkcji anonimowej. W składowej funkcji wystąpienia klasy `this` wartość jest uznawana za parametr wartości i jest zewnętrzną zmienną każdej funkcji anonimowej zawartej w składowej funkcji.

#### <a name="captured-outer-variables"></a>Przechwycone zmienne zewnętrzne

Gdy do zmiennej zewnętrznej odwołuje się funkcja anonimowa, zmienna zewnętrzna jest określana jako ***przechwycona*** przez funkcję anonimową. Zwykle okres istnienia zmiennej lokalnej jest ograniczony do wykonywania bloku lub instrukcji, z którą jest skojarzony ([zmienne lokalne](variables.md#local-variables)). Jednak okres istnienia przechwyconej zmiennej zewnętrznej jest rozszerzany co najmniej do momentu, aż obiekt delegowany lub drzewo wyrażenia utworzone za pomocą funkcji anonimowej staną się uprawnione do wyrzucania elementów bezużytecznych.

W przykładzie
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
Zmienna lokalna `x` jest przechwytywana przez funkcję anonimową, a okres istnienia `x` jest rozszerzany co najmniej do momentu, aż obiekt delegowany zwrócony przez `F` będzie uprawniony do wyrzucania elementów bezużytecznych (które nie są wykonywane do momentu zakończenia tego programu). Ponieważ każde wywołanie funkcji anonimowej działa w tym samym wystąpieniu `x`, dane wyjściowe tego przykładu to:
```console
1
2
3
```

Gdy zmienna lokalna lub parametr wartości jest przechwytywany przez funkcję anonimową, zmienna lub parametr lokalny nie jest już traktowany jako zmienna stała ([zmienne stałe i ruchome](unsafe-code.md#fixed-and-moveable-variables)), ale zamiast tego jest traktowana jako ruchoma zmienna. W ten sposób każdy kod `unsafe`, który pobiera adres przechwyconej zmiennej zewnętrznej, musi najpierw użyć instrukcji `fixed`, aby naprawić zmienną.

Należy pamiętać, że w przeciwieństwie do zmiennej nieprzechwyconej, przechwycona zmienna lokalna może być jednocześnie narażona na wiele wątków wykonywania.

#### <a name="instantiation-of-local-variables"></a>Tworzenie wystąpienia zmiennych lokalnych

Zmienna lokalna jest uznawana za ***wystąpienie*** , gdy wykonanie wejdzie w zakres zmiennej. Na przykład po wywołaniu następującej metody zmienna lokalna `x` jest tworzona i inicjowana trzy razy — raz dla każdej iteracji pętli.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

Jednak przeniesienie deklaracji `x` poza pętlą powoduje utworzenie pojedynczego wystąpienia `x`:
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

Gdy nie przechwycono, nie ma możliwości dokładnego zaobserwowania, jak często jest tworzona zmienna lokalna — ponieważ okresy istnienia wystąpień są rozłączane, istnieje możliwość, że każde wystąpienie będzie używać tej samej lokalizacji magazynu. Jednak gdy funkcja anonimowa przechwytuje zmienną lokalną, efekty tworzenia wystąpienia stają się oczywiste.

Przykład
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
tworzy dane wyjściowe:
```console
1
3
5
```

Jednakże gdy deklaracja `x` jest przenoszona poza pętlę:
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
dane wyjściowe:
```console
5
5
5
```

Jeśli pętla for deklaruje zmienną iteracji, ta zmienna jest uważana za zadeklarowaną poza pętlą. W tym przypadku, jeśli przykład zostanie zmieniony w celu przechwycenia samej zmiennej iteracji:

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
przechwytywane jest tylko jedno wystąpienie zmiennej iteracji, co daje wynik:
```console
3
3
3
```

Istnieje możliwość, że Delegaty funkcji anonimowych do udostępniania niektórych przechwyconych zmiennych jeszcze mają osobne wystąpienia innych. Na przykład jeśli `F` zostanie zmieniony na
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
trzy Delegaty przechwytują to samo wystąpienie `x`, ale osobne wystąpienia `y`, a dane wyjściowe to:
```console
1 1
2 1
3 1
```

Oddzielne funkcje anonimowe mogą przechwytywać to samo wystąpienie zmiennej zewnętrznej. W przykładzie:
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
dwie funkcje anonimowe przechwytują to samo wystąpienie zmiennej lokalnej `x`i w ten sposób mogą "komunikować się" za pomocą tej zmiennej. Dane wyjściowe przykładu to:
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Obliczanie wyrażeń funkcji anonimowych

Funkcja anonimowa `F` musi być zawsze konwertowana na typ delegata `D` lub typ drzewa wyrażenia `E`bezpośrednio lub przez wykonanie wyrażenia tworzenia delegata `new D(F)`. Ta konwersja określa wynik funkcji anonimowej, zgodnie z opisem w [konwersji funkcji anonimowych](conversions.md#anonymous-function-conversions).

## <a name="query-expressions"></a>Wyrażenia zapytań

***Wyrażenia zapytania*** zapewniają składnię języka zintegrowanego dla zapytań, które są podobne do relacyjnych i hierarchicznych języków zapytań, takich jak SQL i XQuery.

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

Wyrażenie zapytania rozpoczyna się od klauzuli `from` i kończyć się klauzulą `select` lub `group`. Po klauzuli początkowej `from` może następować zero lub więcej `from`, `let`, `where`, `join` lub `orderby` klauzule. Każda klauzula `from` jest generatorem wprowadzającym ***zmienną zakresu*** , która przekracza elementy ***sekwencji***. Każda klauzula `let` wprowadza zmienną zakresu reprezentującą wartość obliczoną przy użyciu wartości z poprzednich zmiennych zakresu. Każda klauzula `where` jest filtrem, który wyklucza elementy z wyniku. Każda klauzula `join` porównuje określone klucze sekwencji źródłowej z kluczami innej sekwencji, które dają pasujące pary. Każda klauzula `orderby` zmienia kolejność elementów zgodnie z określonymi kryteriami. Ostateczna klauzula `select` lub `group` określa kształt wyniku w zakresie zmiennych zakresu. Na koniec klauzula `into` może służyć do "łączenia" zapytań, traktując wyniki jednego zapytania jako generatora w kolejnych zapytaniach.

### <a name="ambiguities-in-query-expressions"></a>Niejasności w wyrażeniach zapytania

Wyrażenia zapytań zawierają wiele "kontekstowych słów kluczowych", tj. identyfikatorów, które mają specjalne znaczenie w danym kontekście. Są to `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` i `by`. Aby uniknąć niejasności w wyrażeniach zapytań spowodowanych mieszanym użyciem tych identyfikatorów jako słowa kluczowe lub nazw prostych, te identyfikatory są uznawane za słowa kluczowe w dowolnym miejscu w wyrażeniu zapytania.

W tym celu wyrażenie zapytania jest dowolnym wyrażeniem rozpoczynającym się od "`from identifier`", po którym następuje dowolny token z wyjątkiem "`;`", "`=`" lub "`,`".

Aby użyć tych słów jako identyfikatorów w wyrażeniu zapytania, mogą one być poprzedzone prefiksem "`@`" ([identyfikatorami](lexical-structure.md#identifiers)).

### <a name="query-expression-translation"></a>Tłumaczenie wyrażenia zapytania

C# Język nie określa semantyki wykonywania wyrażeń zapytań. Zamiast tego wyrażenia zapytania są tłumaczone na wywołania metod, które są zgodne z *wzorcem wyrażenia zapytania* ([wzorzec wyrażenia zapytania](expressions.md#the-query-expression-pattern)). W odróżnieniu od tego wyrażenia zapytania są tłumaczone na wywołania metod o nazwach `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`i `Cast`. Te metody powinny mieć określone sygnatury i typy wyników, zgodnie z opisem w [wzorcu wyrażenia zapytania](expressions.md#the-query-expression-pattern). Metody te mogą być wystąpieniem metod obiektu, które są badane lub metod rozszerzających, które są zewnętrzne względem obiektu i implementują rzeczywiste wykonanie zapytania.

Tłumaczenie z wyrażeń zapytania do wywołania metody jest mapowaniem składni, które występuje przed wykonaniem dowolnego powiązania typu lub Rozpoznanie przeciążenia. Tłumaczenie jest gwarantowane pod kątem poprawności składniowej, ale nie gwarantuje, że jest to bardziej prawidłowy C# kod. Po translacji wyrażeń zapytania wyniki wywoływanych metod są przetwarzane jako zwykłe wywołania metod i może to spowodować błędy odkrywania, na przykład jeśli metody nie istnieją, jeśli argumenty mają nieprawidłowe typy lub jeśli metody są ogólne i Wnioskowanie o typie nie powiodło się.

Wyrażenie zapytania jest przetwarzane przez wielokrotne zastosowanie następujących tłumaczeń, dopóki nie będzie możliwe dalsze zmniejszenie. Tłumaczenia są wymienione w kolejności aplikacji: w każdej sekcji przyjęto założenie, że tłumaczenia w powyższych sekcjach zostały wykonane wyczerpująco, a po wyczerpaniu sekcja nie będzie później oglądany w przetwarzaniu tego samego wyrażenia zapytania.

Przypisanie do zmiennych zakresu nie jest dozwolone w wyrażeniach zapytań. Jednakże C# implementacja może nie zawsze wymuszać tego ograniczenia, ponieważ czasami nie jest to możliwe przy użyciu schematu tłumaczenia składni przedstawionego w tym miejscu.

Niektóre tłumaczenia wprowadzają zmienne zakresów z przezroczystymi identyfikatorami wskazywanymi przez `*`. Specjalne właściwości przezroczystych identyfikatorów są omówione bardziej szczegółowo w [przezroczystych identyfikatorach](expressions.md#transparent-identifiers).

#### <a name="select-and-groupby-clauses-with-continuations"></a>Klauzule SELECT i GroupBy z kontynuacjami

Wyrażenie zapytania z kontynuacją
```csharp
from ... into x ...
```
jest przetłumaczony na
```csharp
from x in ( from ... ) ...
```

W przypadku tłumaczeń w poniższych sekcjach przyjęto założenie, że zapytania nie mają kontynuacji `into`.

Przykład
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
jest przetłumaczony na
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
końcowe tłumaczenie to
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>Jawne typy zmiennych zakresu

Klauzula `from`, która jawnie określa typ zmiennej zakresu
```csharp
from T x in e
```
jest przetłumaczony na
```csharp
from x in ( e ) . Cast < T > ( )
```

Klauzula `join`, która jawnie określa typ zmiennej zakresu
```csharp
join T x in e on k1 equals k2
```
jest przetłumaczony na
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

W przypadku tłumaczeń w poniższych sekcjach przyjęto założenie, że zapytania nie mają jawnych typów zmiennych zakresu.

Przykład
```csharp
from Customer c in customers
where c.City == "London"
select c
```
jest przetłumaczony na
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
końcowe tłumaczenie to
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

Jawne typy zmiennych zakresu są przydatne do wykonywania zapytań dotyczących kolekcji implementujących nieogólny interfejs `IEnumerable`, ale nie do ogólnego interfejsu `IEnumerable<T>`. W powyższym przykładzie jest to przypadek, jeśli `customers` były typu `ArrayList`.

#### <a name="degenerate-query-expressions"></a>Wygeneruj wyrażenia zapytania

Wyrażenie zapytania w formularzu
```csharp
from x in e select x
```
jest przetłumaczony na
```csharp
( e ) . Select ( x => x )
```

Przykład
```csharp
from c in customers
select c
```
jest przetłumaczony na
```csharp
customers.Select(c => c)
```

Wygeneruj wyrażenie zapytania, które w sposób nieprosty wybiera elementy źródła. Późniejsza faza tłumaczenia powoduje usunięcie niegenerowanych zapytań, które zostały wprowadzone przez inne etapy tłumaczenia, zastępując je ich źródłem. Ważne jest jednak, aby upewnić się, że wynik wyrażenia zapytania nigdy nie jest obiektem źródłowym, co spowoduje ujawnienie typu i tożsamości źródła do klienta zapytania. W związku z tym ten krok chroni wygenerowanie zapytań pisanych bezpośrednio w kodzie źródłowym przez jawne wywołanie `Select` na źródle. Następnie do realizatorów `Select` i innych operatorów zapytań, aby upewnić się, że te metody nigdy nie zwracają samego obiektu źródłowego.

#### <a name="from-let-where-join-and-orderby-clauses"></a>Klauzule from, let, WHERE, join i OrderBy

Wyrażenie zapytania z drugą klauzulą `from`, po którym następuje klauzula `select`
```csharp
from x1 in e1
from x2 in e2
select v
```
jest przetłumaczony na
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

Wyrażenie zapytania z drugą klauzulą `from`, po którym następuje coś innego niż klauzula `select`:

```csharp
from x1 in e1
from x2 in e2
...
```
jest przetłumaczony na
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

Wyrażenie zapytania z klauzulą `let`
```csharp
from x in e
let y = f
...
```
jest przetłumaczony na
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

Wyrażenie zapytania z klauzulą `where`
```csharp
from x in e
where f
...
```
jest przetłumaczony na
```csharp
from x in ( e ) . Where ( x => f )
...
```

Wyrażenie zapytania z klauzulą `join` bez `into`, po którym następuje klauzula `select`
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
jest przetłumaczony na
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

Wyrażenie zapytania z klauzulą `join` bez `into`, po którym następuje element inny niż klauzula `select`
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
jest przetłumaczony na
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

Wyrażenie zapytania z klauzulą `join` z `into` po którym następuje klauzula `select`
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
jest przetłumaczony na
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Wyrażenie zapytania z klauzulą `join` zawierającą `into`, a następnie element inny niż klauzula `select`
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
jest przetłumaczony na
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

Wyrażenie zapytania z klauzulą `orderby`
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
jest przetłumaczony na
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

Jeśli klauzula porządkowania określa `descending` wskaźnik kierunku, zamiast tego zostanie utworzone wywołanie `OrderByDescending` lub `ThenByDescending`.

Poniższe tłumaczenia zakładają, że nie istnieją klauzule `let`, `where`, `join` lub `orderby` i nie więcej niż jedna początkowa klauzula `from` w każdym wyrażeniu zapytania.

Przykład
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
jest przetłumaczony na
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

Przykład
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
jest przetłumaczony na
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
końcowe tłumaczenie to
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
gdzie `x` to identyfikator wygenerowany przez kompilator, który jest w przeciwnym razie niewidoczny i niedostępny.

Przykład
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
jest przetłumaczony na
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
końcowe tłumaczenie to
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
gdzie `x` to identyfikator wygenerowany przez kompilator, który jest w przeciwnym razie niewidoczny i niedostępny.

Przykład
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
jest przetłumaczony na
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

Przykład
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
jest przetłumaczony na
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
końcowe tłumaczenie to
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
gdzie `x` i `y` są wygenerowanymi przez kompilator identyfikatorami, które w przeciwnym razie są niewidoczne i niedostępne.

Przykład
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
ma końcowe tłumaczenie
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a>Wybierz klauzule

Wyrażenie zapytania w formularzu
```csharp
from x in e select v
```
jest przetłumaczony na
```csharp
( e ) . Select ( x => v )
```
z wyjątkiem sytuacji, gdy v jest identyfikatorem x, tłumaczenie jest po prostu
```csharp
( e )
```

Na przykład
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
jest po prostu przetłumaczony na
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a>Klauzule GroupBy

Wyrażenie zapytania w formularzu
```csharp
from x in e group v by k
```
jest przetłumaczony na
```csharp
( e ) . GroupBy ( x => k , x => v )
```
z wyjątkiem sytuacji, gdy v jest identyfikatorem x, tłumaczenie jest
```csharp
( e ) . GroupBy ( x => k )
```

Przykład
```csharp
from c in customers
group c.Name by c.Country
```
jest przetłumaczony na
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a>Identyfikatory przezroczyste

Niektóre tłumaczenia wprowadzają zmienne zakresów z ***przezroczystymi identyfikatorami*** wskazywanymi przez `*`. Przezroczyste identyfikatory nie są właściwą funkcją języka; istnieją one tylko jako pośredni krok w procesie tłumaczenia wyrażenia zapytania.

Gdy tłumaczenie zapytania powoduje iniekcję przezroczystego identyfikatora, dalsze etapy tłumaczenia propagują przezroczysty identyfikator do funkcji anonimowych i inicjatorów obiektów anonimowych. W tych kontekstach przezroczyste identyfikatory mają następujące zachowanie:

*  Gdy przezroczysty identyfikator występuje jako parametr w funkcji anonimowej, elementy członkowskie skojarzonego typu anonimowego są automatycznie w zakresie w treści funkcji anonimowej.
*  Gdy członek z przezroczystym identyfikatorem znajduje się w zakresie, członkowie tego elementu członkowskiego są również w zakresie.
*  Gdy przezroczysty identyfikator występuje jako element członkowski deklarator w inicjatorze obiektu anonimowego, wprowadza element członkowski z przezroczystym identyfikatorem.
*  W opisanych powyżej krokach, przezroczyste identyfikatory są zawsze wprowadzane wraz z typami anonimowymi, z zamiarem przechwytywania wielu zmiennych zakresu jako elementów członkowskich jednego obiektu. Implementacja programu C# może korzystać z innego mechanizmu niż typy anonimowe, aby grupować jednocześnie wiele zmiennych zakresu. Poniższe przykłady translacji założono, że są używane typy anonimowe i pokazują, jak przezroczyste identyfikatory mogą być przetłumaczone.

Przykład
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
jest przetłumaczony na
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

który jest bardziej przetłumaczony na
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
które po wymazaniu przezroczystych identyfikatorów są równoważne
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
gdzie `x` to identyfikator wygenerowany przez kompilator, który jest w przeciwnym razie niewidoczny i niedostępny.

Przykład
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
jest przetłumaczony na
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
który jest bardziej skrócony do
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
końcowe tłumaczenie to
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
gdzie `x`, `y`i `z` są wygenerowanymi przez kompilator identyfikatorami, które w przeciwnym razie są niewidoczne i niedostępne.

### <a name="the-query-expression-pattern"></a>Wzorzec wyrażenia zapytania

***Wzorzec wyrażenia zapytania*** stanowi wzorzec metod, które typy mogą zaimplementować do obsługi wyrażeń zapytań. Ponieważ wyrażenia zapytania są tłumaczone na wywołania metod przy użyciu mapowania składni, typy mają znaczną elastyczność w sposobie implementowania wzorca wyrażenia zapytania. Na przykład metody wzorca mogą być implementowane jako metody instancji lub jako metody rozszerzające, ponieważ dwie mają tę samą składnię wywołania, a metody mogą zażądać delegatów lub drzew wyrażeń, ponieważ funkcje anonimowe są konwertowane do obu tych elementów.

Poniżej przedstawiono zalecany kształt typu ogólnego `C<T>`, który obsługuje wzorzec wyrażenia zapytania. Typ ogólny jest używany w celu zilustrowania właściwych relacji między parametrami i typami wyników, ale istnieje możliwość zaimplementowania wzorca dla typów innych niż ogólne.

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

W powyższych metodach użyto ogólnych typów delegatów `Func<T1,R>` i `Func<T1,T2,R>`, ale mogą być równie dobrze używane inne Delegaty lub typy drzewa wyrażeń z tymi samymi relacjami w typach parametrów i wyników.

Zwróć uwagę na zalecaną relację między `C<T>` i `O<T>`, która zapewnia, że metody `ThenBy` i `ThenByDescending` są dostępne tylko w wyniku `OrderBy` lub `OrderByDescending`. Zauważ również, że zalecany kształt wyniku `GroupBy`--sekwencji sekwencji, gdzie każda sekwencja wewnętrzna ma dodatkową właściwość `Key`.

Przestrzeń nazw `System.Linq` zawiera implementację wzorca operatora zapytania dla dowolnego typu, który implementuje interfejs `System.Collections.Generic.IEnumerable<T>`.

## <a name="assignment-operators"></a>Operatory przypisania

Operatory przypisania przypisują nową wartość do zmiennej, właściwości, zdarzenia lub elementu indeksatora.

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

Lewy operand przypisania musi być wyrażeniem sklasyfikowanym jako zmienna, dostępem do właściwości, dostępem indeksatora lub dostępem do zdarzeń.

Operator `=` nosi nazwę ***prostego operatora przypisania***. Przypisuje wartość operandu Right do zmiennej, właściwości lub elementu indeksatora podaną przez lewy argument operacji. Lewy argument operacji prostego operatora przypisania nie może być dostępem do zdarzeń (z wyjątkiem opisanych w [zdarzeniach przypominających pola](classes.md#field-like-events)). Prosty operator przypisania został opisany w [prostym przypisaniu](expressions.md#simple-assignment).

Operatory przypisania inne niż operator `=` są nazywane ***operatorami przypisania złożonego***. Te operatory wykonują wskazane operacje na dwóch operandach, a następnie przypisują wartość wyniki do zmiennej, właściwości lub elementu indeksatora podanego przez lewy operand. Operatory przypisania złożonego są opisane w [przypisaniu złożonym](expressions.md#compound-assignment).

Operatory `+=` i `-=` z wyrażeniem dostępu do zdarzenia jako lewy operand są nazywane *operatorami przypisania zdarzenia*. Żaden inny operator przypisania nie jest prawidłowy w przypadku dostępu do zdarzenia jako lewego operandu. Operatory przypisania zdarzeń są opisane w [przypisaniu zdarzeń](expressions.md#event-assignment).

Operatory przypisania są z prawej strony skojarzenia, co oznacza, że operacje są pogrupowane od prawej do lewej. Na przykład wyrażenie `a = b = c` jest oceniane jako `a = (b = c)`.

### <a name="simple-assignment"></a>Przypisanie proste

Operator `=` nosi nazwę prostego operatora przypisania.

Jeśli lewy operand prostego przypisywania ma postać `E.P` lub `E[Ei]` gdzie `E` ma `dynamic`typu czasu kompilacji, przypisanie jest powiązane dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)). W takim przypadku typem czasu kompilowania w wyrażeniu przypisania jest `dynamic`, a rozwiązanie opisane poniżej będzie odbywać się w czasie wykonywania na podstawie typu `E`w czasie wykonywania.

W prostym przypisaniu, prawy operand musi być wyrażeniem, które jest niejawnie konwertowane na typ operandu po lewej stronie. Operacja przypisuje wartość operandu Right do zmiennej, właściwości lub elementu indeksatora podaną przez lewy argument operacji.

Wynikiem prostego wyrażenia przypisania jest wartość przypisana do lewego operandu. Wynik ma ten sam typ co lewy operand i jest zawsze sklasyfikowany jako wartość.

Jeśli argument operacji po lewej stronie jest dostępną właściwością lub indeksatorem, właściwość lub indeksator musi mieć metodę dostępu `set`. W przeciwnym razie wystąpi błąd w czasie powiązania.

Przetwarzanie prostego przypisania formularza `x = y` w czasie wykonywania obejmuje następujące kroki:

*  Jeśli `x` jest sklasyfikowana jako zmienna:
   * `x` jest oceniane, aby utworzyć zmienną.
   * `y` jest oceniane i, w razie potrzeby, konwertowane na typ `x` za pomocą konwersji niejawnej ([konwersje niejawne](conversions.md#implicit-conversions)).
   * Jeśli zmienna określona przez `x` jest elementem tablicy *reference_type*, wykonywane jest sprawdzanie w czasie wykonywania, aby upewnić się, że wartość obliczona dla `y` jest zgodna z wystąpieniem tablicy, którego `x` jest elementem. Sprawdzanie kończy się powodzeniem, jeśli `y` jest `null`, lub Jeśli niejawna konwersja odwołań ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions)) istnieje z rzeczywistego typu wystąpienia, do którego odwołuje się `y` do rzeczywistego typu elementu wystąpienia tablicy zawierającego `x`. W przeciwnym razie zostanie zgłoszony `System.ArrayTypeMismatchException`.
   * Wartość będąca wynikiem oceny i konwersji `y` jest przechowywana w lokalizacji podawanej przez ocenę `x`.
*  Jeśli `x` jest klasyfikowany jako dostęp do właściwości lub indeksatora:
   * Wyrażenie wystąpienia (jeśli `x` nie jest `static`) i lista argumentów (jeśli `x` jest dostępem indeksatora), które są skojarzone z `x` są oceniane, a wyniki są używane w kolejnym wywołaniu metody `set`.
   * `y` jest oceniane i, w razie potrzeby, konwertowane na typ `x` za pomocą konwersji niejawnej ([konwersje niejawne](conversions.md#implicit-conversions)).
   * Metoda dostępu `set` `x` jest wywoływana z wartością obliczaną dla `y` jako argument `value`.

Reguły współwariancji tablicy ([Kowariancja tablicowa](arrays.md#array-covariance)) zezwalają na wartość typu tablicy `A[]` być odwołaniem do wystąpienia typu tablicy `B[]`, pod warunkiem, że istnieje niejawna konwersja odwołań z `B` do `A`. Ze względu na te reguły przypisanie do elementu tablicy *reference_type* wymaga sprawdzenia w czasie wykonywania, aby upewnić się, że przypisywana wartość jest zgodna z wystąpieniem tablicy. W przykładzie
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
Ostatnie przypisanie powoduje wygenerowanie `System.ArrayTypeMismatchException`, ponieważ wystąpienie `ArrayList` nie może być przechowywane w elemencie `string[]`.

Gdy właściwość lub indeksator zadeklarowany w *struct_type* jest elementem docelowym przypisania, wyrażenie wystąpienia skojarzone z właściwością lub dostępem indeksatora musi być sklasyfikowane jako zmienna. Jeśli wyrażenie wystąpienia jest sklasyfikowane jako wartość, wystąpi błąd w czasie powiązania. Ze względu na [dostęp do elementu członkowskiego](expressions.md#member-access)ta sama reguła dotyczy również pól.

Uwzględniając deklaracje:
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
w przykładzie
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
przypisania do `p.X`, `p.Y`, `r.A`i `r.B` są dozwolone, ponieważ `p` i `r` są zmiennymi. Jednak w przykładzie
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
wszystkie przypisania są nieprawidłowe, ponieważ `r.A` i `r.B` nie są zmiennymi.

### <a name="compound-assignment"></a>Przypisanie złożone

Jeśli argument operacji po lewej stronie przypisania złożonego ma postać `E.P` lub `E[Ei]` gdzie `E` ma `dynamic`typu czasu kompilacji, przypisanie jest powiązane dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)). W takim przypadku typem czasu kompilowania w wyrażeniu przypisania jest `dynamic`, a rozwiązanie opisane poniżej będzie odbywać się w czasie wykonywania na podstawie typu `E`w czasie wykonywania.

Operacja `x op= y` jest przetwarzana przez zastosowanie rozpoznawania przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)), tak jakby operacja była zapisywana `x op y`. Następnie

*  Jeśli zwracany typ wybranego operatora jest niejawnie konwertowany na typ `x`, operacja zostanie oceniona jako `x = x op y`, z tą różnicą, że `x` jest oceniana tylko raz.
*  W przeciwnym razie, jeśli wybrany operator jest wstępnie zdefiniowanym operatorem, Jeśli zwracany typ wybranego operatora jest jawnie konwertowany na typ `x`i jeśli `y` jest niejawnie konwertowany na typ `x` lub operator jest operatorem przesunięcia, wówczas operacja jest szacowana jako `x = (T)(x op y)`, gdzie `T` jest typem `x`, z wyjątkiem tego, że `x` jest oceniane tylko raz.
*  W przeciwnym razie złożone przypisanie jest nieprawidłowe i wystąpi błąd w czasie trwania powiązania.

Termin "oceniony tylko raz" oznacza, że podczas oceny `x op y`wyniki wszelkich wyrażeń składników `x` są tymczasowo zapisywane, a następnie ponownie używane podczas wykonywania przypisania do `x`. Na przykład w `A()[B()] += C()`przypisywania, gdzie `A` jest metodą zwracającą `int[]`, a `B` i `C` są metodami zwracającymi `int`, metody są wywoływane tylko raz, w kolejności `A`, `B`, `C`.

Gdy argument operacji po lewej stronie przypisania złożonego jest dostęp do właściwości lub indeksatora, właściwość lub indeksator musi mieć zarówno metodę dostępu `get`, jak i metodę dostępu `set`. W przeciwnym razie wystąpi błąd w czasie powiązania.

Druga reguła powyżej zezwala `x op= y` na ocenę jako `x = (T)(x op y)` w określonych kontekstach. Reguła istnieje tak, aby wstępnie zdefiniowane operatory mogły być używane jako operatory złożone, gdy argument operacji po lewej stronie jest typu `sbyte`, `byte`, `short`, `ushort`lub `char`. Nawet jeśli oba argumenty są jednego z tych typów, wstępnie zdefiniowane operatory generują wynik typu `int`, zgodnie z opisem w [binarnej promocji liczbowych](expressions.md#binary-numeric-promotions). W związku z tym bez rzutowania nie można przypisać wyniku do lewego operandu.

Intuicyjny efekt reguły dla wstępnie zdefiniowanych operatorów to po prostu, że `x op= y` jest dozwolony, jeśli oba `x op y` i `x = y` są dozwolone. W przykładzie
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
intuicyjną przyczyną każdego błędu jest to, że odpowiednie proste przypisanie mogło również być błędem.

Oznacza to również, że operacje przypisania złożonego obsługują podniesione operacje. W przykładzie
```csharp
int? i = 0;
i += 1;             // Ok
```
użyto `+(int?,int?)` operatora, który został zniesiony.

### <a name="event-assignment"></a>Przypisanie zdarzenia

Jeśli argument operacji po lewej stronie operatora `+=` lub `-=` został sklasyfikowany jako dostęp do zdarzenia, wyrażenie jest oceniane w następujący sposób:

*  Wyrażenie wystąpienia (jeśli istnieje) dostępu do zdarzenia jest oceniane.
*  Jest oceniany prawy operand operatora `+=` lub `-=`, a jeśli jest to wymagane, konwertowane na typ operandu lewego przez niejawną konwersję ([konwersje niejawne](conversions.md#implicit-conversions)).
*  Metoda dostępu do zdarzenia jest wywoływana, z listą argumentów składającą się z prawego operandu, po dokonaniu oceny i, w razie potrzeby, konwersji. Jeśli operator został `+=`, metoda dostępu `add` jest wywoływana; Jeśli operator został `-=`, metoda dostępu `remove` jest wywoływana.

Wyrażenie przypisania zdarzenia nie zwraca wartości. W związku z tym wyrażenie przypisania zdarzenia jest prawidłowe tylko w kontekście *statement_expression* ([instrukcji wyrażeń](statements.md#expression-statements)).

## <a name="expression"></a>Wyrażenie

*Wyrażenie* jest *non_assignment_expression* lub *przypisanie*.

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a>Wyrażenia stałe

*Constant_expression* jest wyrażeniem, które może być w pełni oceniane w czasie kompilacji.

```antlr
constant_expression
    : expression
    ;
```

Wyrażenie stałe musi być literałem `null` lub wartością z jednym z następujących typów: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`lub dowolnego typu wyliczenia. Tylko następujące konstrukcje są dozwolone w wyrażeniach stałych:

*  Literały (w tym literał `null`).
*  Odwołania do `const` składowych klas i typów struktury.
*  Odwołania do elementów członkowskich typów wyliczeniowych.
*  Odwołania do parametrów `const` lub zmiennych lokalnych
*  Podwyrażenia w nawiasach, które są same wyrażeniami stałymi.
*  Wyrażenia rzutowania, pod warunkiem, że typ docelowy to jeden z typów wymienionych powyżej.
*  wyrażenia `checked` i `unchecked`
*  Wyrażenia wartości domyślnych
*  Wyrażenia nameof
*  Wstępnie zdefiniowane `+`, `-`, `!`i `~` operatory jednoargumentowe.
*  Wstępnie zdefiniowane `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`i `>=` operatory binarne, pod warunkiem, że każdy operand jest typu wymienionego powyżej.
*  Operator warunkowy `?:`.

Następujące konwersje są dozwolone w wyrażeniach stałych:

*  Konwersje tożsamości
*  Konwersje numeryczne
*  Konwersje wyliczenia
*  Konwersje wyrażeń stałych
*  Konwersje niejawne i jawne, pod warunkiem, że źródło konwersji jest wyrażeniem stałym, którego wartość jest równa null.

Inne konwersje, w tym opakowanie, rozpakowywanie i niejawne konwersje odwołań wartości innych niż null, są niedozwolone w wyrażeniach stałych. Na przykład:
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
Inicjowanie i jest błędem, ponieważ wymagana jest konwersja z opakowaniem. Inicjalizacja str jest błędem, ponieważ wymagana jest niejawna konwersja odwołania z wartości innej niż null.

Gdy wyrażenie spełnia wymagania wymienione powyżej, wyrażenie jest oceniane w czasie kompilacji. Jest to prawdziwe, nawet jeśli wyrażenie jest podwyrażeniem większego wyrażenia, które zawiera niestałe konstrukcje.

Ocena czasu kompilacji wyrażeń stałych używa tych samych zasad, co w przypadku oceny w czasie wykonywania wyrażeń niestałych, z tą różnicą, że w przypadku oceny czasu wykonywania wywołał wyjątek, Ocena czasu kompilacji powoduje wystąpienie błędu kompilacji.

Jeśli wyrażenie stałe nie jest jawnie umieszczane w kontekście `unchecked`, nadmiary występują w operacjach arytmetycznych typu całkowitego i konwersje podczas obliczania czasu kompilacji wyrażenia zawsze powodują błędy czasu kompilacji ([wyrażenia stałe](expressions.md#constant-expressions)).

Wyrażenia stałe występują w kontekstach wymienionych poniżej. W tych kontekstach występuje błąd czasu kompilacji, jeśli wyrażenie nie może być w pełni oceniane w czasie kompilacji.

*  Stałe deklaracje ([stałe](classes.md#constants)).
*  Wyliczanie deklaracji elementów członkowskich ([składowe wyliczenia](enums.md#enum-members)).
*  Domyślne argumenty list parametrów formalnych ([Parametry metody](classes.md#method-parameters))
*  etykiety `case` instrukcji `switch` ([instrukcja SWITCH](statements.md#the-switch-statement)).
*  instrukcje `goto case` ([instrukcja goto](statements.md#the-goto-statement)).
*  Długości wymiarów w wyrażeniu tworzenia tablicy ([wyrażenia tworzenia tablic](expressions.md#array-creation-expressions)), które zawierają inicjator.
*  Atrybuty ([atrybuty](attributes.md)).

Niejawna konwersja wyrażeń stałych ([niejawne konwersje wyrażeń stałych](conversions.md#implicit-constant-expression-conversions)) umożliwia przekonwertowanie stałego wyrażenia typu `int` na `sbyte`, `byte`, `short`, `ushort`, `uint`lub `ulong`, pod warunkiem, że wartość wyrażenia stałego mieści się w zakresie typu docelowego.

## <a name="boolean-expressions"></a>Wyrażenia logiczne

*Boolean_expression* jest wyrażeniem, które zwraca wynik typu `bool`; bezpośrednio lub poprzez zastosowanie `operator true` w niektórych kontekstach określonych w poniższej.

```antlr
boolean_expression
    : expression
    ;
```

Wyrażenie warunkowe kontrolujące *if_statement* ([instrukcja if](statements.md#the-if-statement)), *while_statement* ([instrukcja while](statements.md#the-while-statement)), *do_statement* ([instrukcja](statements.md#the-do-statement)do) lub *for_statement* ([instrukcja for](statements.md#the-for-statement)) jest *Boolean_expression*. Wyrażenie warunkowe kontrolki operatora `?:` ([operator warunkowy](expressions.md#conditional-operator)) ma takie same reguły jak *Boolean_expression*, ale z powodu pierwszeństwa operatorów jest klasyfikowane jako *conditional_or_expression*.

`E` *Boolean_expression* jest wymagana, aby można było utworzyć wartość typu `bool`w następujący sposób:

*  Jeśli `E` jest niejawnie konwertowany na `bool`, w czasie wykonywania, że stosowana jest niejawna konwersja.
*  W przeciwnym razie rozpoznawanie przeciążania operatora jednoargumentowego ([jednoargumentowego rozpoznawania operatora](expressions.md#unary-operator-overload-resolution)) służy do znajdowania unikatowej najlepszej implementacji operatora `true` na `E`, a ta implementacja została zastosowana w czasie wykonywania.
*  Jeśli żaden taki operator nie zostanie znaleziony, wystąpi błąd w czasie trwania powiązania.

Typ struktury `DBBool` w [typie Boolean bazy danych](structs.md#database-boolean-type) zawiera przykład typu, który implementuje `operator true` i `operator false`.
