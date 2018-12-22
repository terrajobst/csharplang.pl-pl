# <a name="expressions"></a>Wyrażenia

Wyrażenie jest sekwencją operatorów i argumentów operacji. W tym rozdziale określa składnię, kolejność oceny operatorów i argumentów i znaczenie wyrażenia.

## <a name="expression-classifications"></a>Wyrażenie klasyfikacji

Wyrażenie jest klasyfikowana jako jeden z następujących czynności:

*  Wartość. Każda wartość ma skojarzony typ.
*  Zmienna. Co zmienna ma skojarzony typ, a mianowicie zadeklarowanym typem zmiennej.
*  Przestrzeń nazw. Wyrażenia z tą klasyfikacją może się pojawić tylko po lewej stronie *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)). W dowolnym kontekście wyrażenia sklasyfikowane jako przestrzeń nazw powoduje błąd kompilacji.
*  Typ. Wyrażenia z tą klasyfikacją może się pojawić tylko po lewej stronie *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)), lub jako argument dla `as` — operator ([jako operator ](expressions.md#the-as-operator)), `is` — operator ([jest operatorem](expressions.md#the-is-operator)), lub `typeof` — operator ([typeof operator](expressions.md#the-typeof-operator)). W dowolnym kontekście sklasyfikowane jako typ wyrażenia powoduje błąd kompilacji.
*  Grupy metod, czyli zestaw przeciążonych metod wynikające z wyszukanie członka ([wyszukanie członka](expressions.md#member-lookup)). Grupy metod może mieć wyrażenia skojarzonego wystąpienia i skojarzony typ listy argumentów. Po wywołaniu metody wystąpienia wyniku obliczenia wyrażenia wystąpienia staje się wystąpienia, reprezentowane przez `this` ([dostęp](expressions.md#this-access)). Grupy metod jest dozwolony w *invocation_expression* ([wyrażenia wywołania](expressions.md#invocation-expressions)), *delegate_creation_expression* ([delegowanie tworzenia wyrażenia](expressions.md#delegate-creation-expressions)) i po lewej stronie operatora i mogą być niejawnie konwertowane na typ delegata zgodne ([metody konwersji grup](conversions.md#method-group-conversions)). W dowolnym kontekście wyrażenia sklasyfikowane jako grupy metod powoduje błąd kompilacji.
*  Literał null. Wyrażenia z tą klasyfikacją można niejawnie przekonwertować typu odwołania lub typ dopuszczający wartość null.
*  Funkcja anonimowa. Wyrażenia z tą klasyfikacją mogą być niejawnie konwertowane na typ zgodny delegata lub typ drzewa wyrażenia.
*  Dostęp do właściwości. Dostęp do każdej właściwości jest skojarzony typ, to znaczy typu właściwości. Ponadto dostęp do właściwości może mieć wyrażenia skojarzonego wystąpienia. Gdy metody dostępu ( `get` lub `set` bloku) wystąpienia dostęp do właściwości jest wywoływana, wynikiem obliczenia wyrażenia wystąpienia staje się wystąpienia, reprezentowane przez `this` ([dostęp](expressions.md#this-access)).
*  Dostęp do zdarzenia. Dostęp do każdego zdarzenia jest skojarzony typ, a mianowicie typ zdarzenia. Ponadto dostęp do zdarzenia może mieć wyrażenia skojarzonego wystąpienia. Dostęp do zdarzenia może być wyświetlana jako argument operacji po lewej stronie ekranu `+=` i `-=` operatorów ([przypisanie zdarzenia](expressions.md#event-assignment)). W dowolnym kontekście wyrażenia sklasyfikowane jako dostęp do zdarzenia powoduje błąd kompilacji.
*  Dostęp indeksatora. Dostęp indeksatora, co ma skojarzony typ, a mianowicie typ elementu indeksatora. Ponadto dostęp indeksatora ma wyrażenia skojarzonego wystąpienia oraz lista argumentów skojarzonych. Gdy metody dostępu ( `get` lub `set` bloku) indeksatora dostępu jest wywoływana, wynikiem obliczenia wyrażenia wystąpienia staje się wystąpienia, reprezentowane przez `this` ([dostęp](expressions.md#this-access)) i wynik Ocena listy argumentów staje się lista parametrów wywołania.
*  Brak elementów. Operacja wykonywana, gdy wyrażenie jest wywołanie metody z typem zwracanym `void`. Sklasyfikowane jako nic nie jest prawidłowy tylko w kontekście wyrażenia *statement_expression* ([instrukcje wyrażeń](statements.md#expression-statements)).

Ostateczny wynik wyrażenia nigdy nie jest przestrzeń nazw, typu, grupy metod lub dostęp do zdarzenia. Przeciwnie jak wspomniano powyżej, te wyrażenia są kategorie pośrednich konstrukcji, które są dozwolone tylko w określonych kontekstach.

Dostęp do właściwości lub indeksatora dostępu jest zawsze sklasyfikowany jako wartość, wykonując wywołanie *pobierająca* lub *ustawiająca metoda dostępu*. Określonej metody dostępu jest ustalany w kontekście dostępu właściwości lub indeksatora: Jeśli dostęp jest elementem docelowym przypisania, *ustawiająca metoda dostępu* wywoływana w celu przypisania nowej wartości ([przypisanie proste](expressions.md#simple-assignment)). W przeciwnym razie *pobierająca* wywoływana w celu uzyskania bieżącej wartości ([wartości wyrażeń](expressions.md#values-of-expressions)).

### <a name="values-of-expressions"></a>Wartości wyrażeń

Większość konstrukcji, które obejmują wyrażenie ostatecznie wymaga wyrażenia do oznaczania ***wartość***. W takich przypadkach Jeśli rzeczywiste wyrażenie wskazuje przestrzeni nazw, typu, grupy metod lub nothing, kompilacji wystąpi błąd. Jednak jeśli wyrażenie wskazuje dostęp do właściwości, indeksatora dostępu lub zmienną, wartość właściwości, indeksatora lub zmienna zostanie zastąpiony niejawnie:

*  Wartość zmiennej jest po prostu wartości, które są obecnie przechowywane w lokalizacji przechowywania, określone przez zmienną. Zmienna muszą być rozważone zdecydowanie przypisany ([asercję określonego przypisania](variables.md#definite-assignment)) przed jej wartość można uzyskać lub w przeciwnym razie wystąpi błąd kompilacji.
*  Wartość właściwości wyrażenia dostęp jest uzyskiwany przez *pobierająca* właściwości. Jeśli nie ma właściwości *pobierająca*, wystąpi błąd kompilacji. W przeciwnym razie wywołanie funkcji elementu członkowskiego ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) jest wykonywane, i wynik wywołania staje się wartość wyrażenia dostępu do właściwości.
*  Wartość wyrażenia dostęp indeksatora jest uzyskiwany przez *pobierająca* indeksatora. Jeśli nie ma indeksatora *pobierająca*, wystąpi błąd kompilacji. W przeciwnym razie wywołanie funkcji elementu członkowskiego ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) odbywa się za pomocą argumentu listy skojarzone z wyrażeniem dostęp indeksatora i wynik wywołania staje się wartością wyrażenia dostęp indeksatora.

## <a name="static-and-dynamic-binding"></a>Powiązanie statycznych i dynamicznych

Proces określania znaczenie operacji na podstawie typu lub wartości składowych wyrażeń (argumenty, argumenty operacji, odbiorniki) jest często nazywany ***powiązania***. Na przykład znaczenie wywołania metody jest określana na podstawie typu odbiornik i argumentów. Znaczenie operator jest określana na podstawie typu argumentów.

W języku C# znaczenie operacji jest zazwyczaj określana w czasie kompilacji, na podstawie typu jego składowych wyrażeń w czasie kompilacji. Podobnie jeśli wyrażenie zawiera błąd, błąd jest wykrywane i zgłaszana przez kompilator. To podejście jest nazywane ***wiązanie statyczne***.

Jednak jeśli wyrażenie jest wyrażeniem dynamicznej (czyli ma typ `dynamic`) oznacza to, że wszystkie powiązania, które uczestniczy w powinna być oparta na jego typu run-time (czyli rzeczywisty typ obiektu, który wskazuje, w czasie wykonywania) zamiast typu ma pod czas kompilacji. Powiązanie takie działanie, w związku z tym jest odroczone do czasu, w którym ma zostać wykonana podczas uruchamiania programu operacji. Jest to określane jako ***wiązanie dynamiczne***.

Podczas operacji dynamicznie jest związany, niewielkiego lub żadnego sprawdzanie jest wykonywane przez kompilator. Zamiast tego w przypadku niepowodzenia powiązania środowiska wykonawczego błędy są zgłaszane jako wyjątki w czasie wykonywania.

Następujące operacje w języku C# jest zależna od powiązania:

*  Dostęp do elementu członkowskiego: `e.M`
*  Wywołanie metody: `e.M(e1, ..., eN)`
*  Wywołanie delegata:`e(e1, ..., eN)`
*  Dostęp do elementu: `e[e1, ..., eN]`
*  Tworzenie obiektów: `new C(e1, ..., eN)`
*  Przeciążone operatory jednoargumentowe: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`
*  Przeciążone operatory dwuargumentowe: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<` , `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`
*  Operatory przypisania: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`
*  Konwersje jawne i niejawne

W przypadku nie wyrażeń dynamicznych C# domyślnie wiązanie statyczne, co oznacza, że typy kompilacji w składowych wyrażeń są używane w procesie wyboru. Jednak gdy jedno z wyrażeń składowych w operacjach wymienionych powyżej jest wyrażenia dynamicznego, operacja jest zamiast tego dynamicznie powiązany.

### <a name="binding-time"></a>Powiązania w czasie

Wiązanie statyczne odbywa się w czasie kompilacji, natomiast wiązanie dynamiczne odbywa się w czasie wykonywania. W poniższych sekcjach termin ***powiązania w czasie*** odwołuje się do kompilacji lub czasu wykonywania, w zależności od tego, kiedy wiązanie ma miejsce.

Poniższy przykład ilustruje pojęcia statyczne i dynamiczne powiązania i czasu powiązania:
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

Statycznie powiązane są dwa pierwsze wywołania metody: przeciążenia `Console.WriteLine` jest pobierany na podstawie ich argumentu typu kompilacji. Dlatego podczas wiązania jest kompilacji.

Trzecie wywołanie jest dynamicznie powiązane: przeciążenia `Console.WriteLine` jest pobierany na podstawie typu środowiska wykonawczego jej argumentu. Dzieje się tak, ponieważ argument jest wyrażenia dynamicznego — jej typ kompilacji jest `dynamic`. W związku z tym czas powiązania trzecie wywołanie jest środowiska wykonawczego.

### <a name="dynamic-binding"></a>Wiązanie dynamiczne

Wiązanie dynamiczne ma na celu Zezwalaj na interakcję z programów C# ***obiektów dynamicznych***, czyli system typów obiektów, które nie podlegają normalnych reguł języka C#. Obiekty dynamiczne mogą być obiektów z innych języków programowania przy użyciu różnych typów systemów lub mogą być obiekty, które są programowo Instalatora, aby implementować własne semantykę wiązania dla różnych operacji.

Mechanizm, za pomocą którego obiekt dynamiczny implementuje własne semantyka jest definiowany przez implementację. Danego interfejsu--ponownie definiowany przez implementację — jest implementowany przez obiekty dynamiczne w celu sygnalizowania, że do języka C# czasu wykonywania, do których mają specjalne semantyki. W związku z tym w każdym przypadku, gdy operacje na obiekt dynamiczny dynamicznie są powiązane, semantyka własne powiązania, a nie te języka C#, jak określono w tym dokumencie przejąć.

W trakcie celem wiązanie dynamiczne umożliwia współdziałanie z obiektami dynamicznymi, C# umożliwia wiązanie dynamiczne na wszystkich obiektach, czy są one dynamiczne, czy nie. Dzięki temu gładsze integracji z obiektami dynamicznymi, wyniki operacji na nich same nie mogą być obiektami dynamicznymi, ale są nadal typu nieznany do programisty należy w czasie kompilacji. Również wiązanie dynamiczne mogą pomóc wyeliminować podatne kod oparty na odbiciu, nawet wtedy, gdy żadne obiekty, które są zaangażowane są obiektami dynamicznymi.

Poniżej opisano każdego konstrukcji w języku dokładnie gdy wiązanie dynamiczne jest stosowany, co skompilować kontrola czasu — Jeśli dowolny — są stosowane i jakie kompilacji wynik i wyrażenia klasyfikacji.

### <a name="types-of-constituent-expressions"></a>Typy składowych wyrażeń

Podczas operacji statycznie jest związany, typ wyrażenia składowych (np. odbiornik i argument, indeksu lub operand) zawsze uważany jest za typów w czasie kompilacji to wyrażenie.

Podczas operacji dynamicznie jest związany, typ wyrażenia składowych jest określany na różne sposoby w zależności od typu składowych wyrażeń w czasie kompilacji:

*  Wyrażenie składowych typów w czasie kompilacji `dynamic` ma typ wartości rzeczywistej. wyrażenie daje w wyniku w czasie wykonywania
*  Składowe wyrażenia, którego typ kompilacji jest parametrem typu została uznana za typu, który jest powiązany parametr typu w czasie wykonywania
*  W przeciwnym razie składowych wyrażeń została uznana za typów w czasie kompilacji.

## <a name="operators"></a>Operatory

Wyrażenia są konstruowane na podstawie ***operandy*** i ***operatory***. Operatory wyrażenie wskazuje operacji do zastosowania do operandów. Przykłady operatorów `+`, `-`, `*`, `/`, i `new`. Przykładami operandy są literały, pola, zmienne lokalne i wyrażeń.

Istnieją trzy rodzaje operatory:

*  Operatory jednoargumentowe. Operatory jednoargumentowe wykorzystać jeden z operandów i używać notacji albo prefiksu (takie jak `--x`) lub notacji przyrostkowej (takie jak `x++`).
*  Operatory binarne. Operatory dwuargumentowe mają dwa operandy i używają notacji infiks (takie jak `x + y`).
*  operator trójargumentowy. Tylko jeden operator trójargumentowy, `?:`, istnieje; ma trzy operandy i używa wrostkowe notacji (`c ? x : y`).

Kolejność obliczania operatorów w wyrażeniu jest określana przez ***pierwszeństwo*** i ***kojarzenie*** operatorów ([pierwszeństwo i kojarzenie operatorów](expressions.md#operator-precedence-and-associativity)) .

Operandy w wyrażeniu są przetwarzane od lewej do prawej. Na przykład w `F(i) + G(i++) * H(i)`, Metoda `F` jest wywoływana przy użyciu starej wartości `i`, then — metoda `G` jest wywoływana z stara wartość `i`, a na koniec metody `H` jest wywoływana z nową wartością `i`. To jest oddzielne i niepowiązanych pierwszeństwo operatorów.

Niektórych operatorów może być ***przeciążone***. Przeciążanie operatora zezwala implementacje operatora zdefiniowanego przez użytkownika może być określony dla operacji, w przypadku, gdy jeden lub oba operandy są typu klasy lub struktury zdefiniowany przez użytkownika ([przeciążania operatora](expressions.md#operator-overloading)).

### <a name="operator-precedence-and-associativity"></a>Pierwszeństwo i kojarzenie operatorów

Gdy wyrażenie zawiera wiele operatorów ***pierwszeństwo*** operatorów określa kolejność, w jakiej są oceniane poszczególnych operatorach. Na przykład, wyrażenie `x + y * z` jest oceniane jako `x + (y * z)` ponieważ `*` operator ma wyższy priorytet niż plik binarny `+` operatora. Pierwszeństwo operatora jest ustanawiane zgodnie z definicją jego produkcji skojarzone gramatyki. Na przykład *additive_expression* składa się z sekwencji *multiplicative_expression*rozdzielone s `+` lub `-` operatorów, co daje `+` i `-` operatory niższy priorytet niż `*`, `/`, i `%` operatorów.

Poniższa tabela zawiera podsumowanie wszystkich operatorów w kolejność pierwszeństwa od najwyższego do najniższego:

| __Sekcja__                                                                                   | __Kategoria__                | __Operatory__ | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [Wyrażenia podstawowe](expressions.md#primary-expressions)                                     | Podstawowy                     | `x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate` | 
| [Operatory jednoargumentowe](expressions.md#unary-operators)                                             | Jednoargumentowy                       | `+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x` | 
| [Operatory arytmetyczne](expressions.md#arithmetic-operators)                                   | Mnożenia              | `*`  `/`  `%` | 
| [Operatory arytmetyczne](expressions.md#arithmetic-operators)                                   | Dodatku                    | `+`  `-`      | 
| [Operatory przesunięcia](expressions.md#shift-operators)                                             | Shift                       | `<<`  `>>`    | 
| [Operatory relacyjne i badania typu](expressions.md#relational-and-type-testing-operators) | Relacyjne i badania typu | `<`  `>`  `<=`  `>=`  `is`  `as` | 
| [Operatory relacyjne i badania typu](expressions.md#relational-and-type-testing-operators) | Równość                    | `==`  `!=`    | 
| [Operatory logiczne](expressions.md#logical-operators)                                         | AND logiczne                 | `&`           | 
| [Operatory logiczne](expressions.md#logical-operators)                                         | XOR logiczne                 | `^`           | 
| [Operatory logiczne](expressions.md#logical-operators)                                         | OR logiczne                  | <code>&#124;</code>           |
| [Operatory logiczne warunkowe](expressions.md#conditional-logical-operators)                 | AND warunkowe             | `&&`          | 
| [Operatory logiczne warunkowe](expressions.md#conditional-logical-operators)                 | OR warunkowe              | <code>&#124;&#124;</code>          | 
| [Wartość null operatora łączącego](expressions.md#the-null-coalescing-operator)                   | Łączenie wartości null             | `??`          | 
| [Operator warunkowy](expressions.md#conditional-operator)                                   | Warunkowe                 | `?:`          | 
| [Operatory przypisania](expressions.md#assignment-operators), [wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)  | Wyrażenie lambda i przypisanie | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

W przypadku argumentu operacji między dwa operatory o tym samym priorytecie, łączność operatorów kontrolki kolejność, w której są wykonywane operacje:

*  Z wyjątkiem operatorów przypisania i null operatora łączącego wszystkie operatory dwuargumentowe to ***lewostronne***, co oznacza, że operacje są wykonywane od lewej do prawej. Na przykład `x + y + z` jest oceniane jako `(x + y) + z`.
*  Operatory przypisania, operatora łączącego o wartości null i operator warunkowy (`?:`) są ***zespolony z prawej***, co oznacza, że operacje są wykonywane od prawej do lewej. Na przykład `x = y = z` jest oceniane jako `x = (y = z)`.

Pierwszeństwo i kojarzenie mogą być kontrolowane za pomocą nawiasów. Na przykład `x + y * z` najpierw mnoży `y` przez `z` , a następnie dodaje wynik do `x`, ale `(x + y) * z` najpierw dodaje `x` i `y` i następnie mnoży wynik przez `z`.

### <a name="operator-overloading"></a>Przeładowanie operatora

Wszystkie operatory jednoargumentowe i binarny ma wstępnie zdefiniowane implementacji, które są automatycznie dostępne na dowolne wyrażenie. Oprócz wstępnie zdefiniowanych implementacje implementacje zdefiniowanych przez użytkownika mogą zostać wprowadzone przez dołączenie `operator` deklaracje klas i struktur ([operatory](classes.md#operators)). Implementacjami operatorów zdefiniowanych przez użytkownika zawsze pierwszeństwo wstępnie zdefiniowanego operatora implementacji: Tylko, gdy nie ma zastosowania operatora zdefiniowanego przez użytkownika przez implementacje będzie implementacje wstępnie zdefiniowanego operatora uznaje się, zgodnie z opisem w [Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution) i [przeciążenia operatora binarnego rozpoznawanie](expressions.md#binary-operator-overload-resolution).

***Operatory jednoargumentowe z możliwością przeciążenia*** są:
```csharp
+   -   !   ~   ++   --   true   false
```

Mimo że `true` i `false` nie są jawnie używane w wyrażeniach (i w związku z tym nie są uwzględnione w tabeli pierwszeństwo w [pierwszeństwo i kojarzenie operatorów](expressions.md#operator-precedence-and-associativity)), są traktowane jako operatorów, ponieważ są one wywoływane w wielu kontekstach wyrażenia: wyrażenia logiczne ([wyrażeń logicznych](expressions.md#boolean-expressions)) i wyrażenia dotyczące zasad dostępu ([operator warunkowy](expressions.md#conditional-operator)) i warunkowego logiczne operatory ([warunkowego operatorów logicznych](expressions.md#conditional-logical-operators)).

***z możliwością przeciążenia operatorów binarnych*** są:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Mogą być przeciążone operatory wymienionych powyżej. W szczególności, nie jest możliwe przeciążenia dostęp do elementu członkowskiego, wywołanie metody lub `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, i `is` operatorów.

Gdy jest przeciążony operator binarny, odpowiedniego operatora przypisania, jest również niejawnie przeciążona. Na przykład przeciążenia operatora `*` jest również przeciążenia operatora `*=`. Jest to dokładniejszym opisem zawartym w [przydział złożony](expressions.md#compound-assignment). Należy pamiętać, że operator przypisania, sama (`=`) nie mogą być przeciążone. Przypisanie zawsze wykonuje prostą kopię operacja wartości do zmiennej.

Rzutowanie operacje, takie jak `(T)x`, są przeciążone, zapewniając konwersje zdefiniowane przez użytkownika ([zdefiniowane przez użytkownika konwersje](conversions.md#user-defined-conversions)).

Element dostępu, takich jak `a[x]`, nie jest uważany za z możliwością przeciążenia operatora. Zamiast tego użytkownika indeksowanie jest obsługiwane przy użyciu indeksatorów ([indeksatory](classes.md#indexers)).

W wyrażeniach operatory są wywoływane przy użyciu notacji operatora, a w deklaracji, operatory są wywoływane przy użyciu notacji funkcjonalności. W poniższej tabeli przedstawiono relację między operatora i funkcjonalności notacji jednoargumentowy i operatory binarne. W pierwszej pozycji *op* oznacza wszelkie operatora prefiksowego jednoargumentowego z możliwością przeciążenia. W drugiej pozycji *op* oznacza przyrostkowe jednoargumentowe `++` i `--` operatorów. W trzecim wpis *op* oznacza wszelkie Oczekiwano operatora binarnego.


| __Notacja operatora__ | __Notacja funkcji__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

Deklaracje operatora zdefiniowanego przez użytkownika zawsze należy wymagać co najmniej jeden z parametrów typu klasy lub struktury, który zawiera deklarację operatora. W związku z tym nie jest możliwe dla operatora zdefiniowanego przez użytkownika mieć taką samą sygnaturę jak wstępnie zdefiniowanego operatora.

Deklaracje operatora zdefiniowanego przez użytkownika nie można zmodyfikować składni, pierwszeństwo i łączność operatora. Na przykład `/` operator jest zawsze operatora binarnego, zawsze ma poziom priorytetu określona w [pierwszeństwo i kojarzenie operatorów](expressions.md#operator-precedence-and-associativity)i jest zawsze lewostronne.

Chociaż jest to możliwe dla operatora zdefiniowanego przez użytkownika wykonać żadnych obliczeń, który go pleases implementacji, które tworzą wyniki niż te, które oczekują intuicyjnie jest niewskazane. Na przykład implementacja `operator ==` należy porównać dwa operandy pod kątem równości i zwraca odpowiednią `bool` wynik.

Opisy poszczególnych operatorach w [wyrażenia podstawowe](expressions.md#primary-expressions) za pośrednictwem [warunkowego operatorów logicznych](expressions.md#conditional-logical-operators) Określ wstępnie zdefiniowanych implementacjami operatorów i wszelkie dodatkowe reguły, które są stosowane Każdy operator. Wprowadź opis wyrażenia ***Rozpoznanie przeciążenia operatora jednoargumentowego***, ***Rozpoznanie przeciążenia operatora binarnego***, i ***promocji na liczbowe***, definicje są znaleziono, w poniższych sekcjach.

### <a name="unary-operator-overload-resolution"></a>Rozpoznanie przeciążenia operatora jednoargumentowego

Operacja formularza `op x` lub `x op`, gdzie `op` jest operatora jednoargumentowego z możliwością przeciążenia i `x` to wyrażenie typu `X`, jest przetwarzana w następujący sposób:

*  Zestaw operatory zdefiniowane przez użytkownika Release candidate dostarczone przez `X` operacji `operator op(x)` jest określany za pomocą zasad [Release Candidate zdefiniowane przez użytkownika operatory](expressions.md#candidate-user-defined-operators).
*  Jeśli zestaw Release candidate operatory zdefiniowane przez użytkownika nie jest pusta, to staje się zestaw operatorów Release candidate dla tej operacji. W przeciwnym razie wstępnie zdefiniowanych jednoargumentowe `operator op` implementacji, łącznie z podniesionym formularzy, stają się zestaw operatorów Release candidate dla tej operacji. Wstępnie zdefiniowane implementacje danego operatora są określone w opisie operatora ([wyrażenia podstawowe](expressions.md#primary-expressions) i [operatory jednoargumentowe](expressions.md#unary-operators)).
*  Zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution) są stosowane do zestawu operatorów Release candidate, aby wybrać najlepsze operatora w odniesieniu do listy argumentów `(x)`, a ten operator staje się wynikiem przeciążenia proces rozpoznawania. W przypadku niepowodzenia funkcja rozpoznawania przeciążeń do wybrania jednego operatora najlepsze występuje błąd wiązania.

### <a name="binary-operator-overload-resolution"></a>Rozpoznanie przeciążenia operatora binarnego

Operacji formularza `x op y`, gdzie `op` jest Oczekiwano operatora binarnego `x` to wyrażenie typu `X`, i `y` to wyrażenie typu `Y`, jest przetwarzana w następujący sposób:

*  Zestaw operatory zdefiniowane przez użytkownika Release candidate dostarczone przez `X` i `Y` operacji `operator op(x,y)` jest określana. Zestaw składa się z Unii operatory Release candidate, dostarczone przez `X` i operatory Release candidate, dostarczone przez `Y`, każdy określone, używając reguł [Release Candidate zdefiniowane przez użytkownika operatory](expressions.md#candidate-user-defined-operators). Jeśli `X` i `Y` tego samego typu lub, jeśli `X` i `Y` pochodzą od typu podstawowego typowych operatorów udostępnionego Release candidate występować tylko w połączony zestaw raz, a następnie.
*  Jeśli zestaw Release candidate operatory zdefiniowane przez użytkownika nie jest pusta, to staje się zestaw operatorów Release candidate dla tej operacji. W przeciwnym razie wstępnie zdefiniowanych danych binarnych `operator op` implementacji, łącznie z podniesionym formularzy, stają się zestaw operatorów Release candidate dla tej operacji. Wstępnie zdefiniowane implementacje danego operatora są określone w opisie operatora ([operatorów arytmetycznych](expressions.md#arithmetic-operators) za pośrednictwem [warunkowego operatorów logicznych](expressions.md#conditional-logical-operators)). Dla wstępnie zdefiniowanego typu wyliczeniowego i delegata operatorów tylko operatory uważane za są identyczne ze zdefiniowanymi przez typ wyliczenia lub delegata, który jest typem powiązania w czasie, jeden z argumentów.
*  Zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution) są stosowane do zestawu operatorów Release candidate, aby wybrać najlepsze operatora w odniesieniu do listy argumentów `(x,y)`, a ten operator staje się wynikiem przeciążenia proces rozpoznawania. W przypadku niepowodzenia funkcja rozpoznawania przeciążeń do wybrania jednego operatora najlepsze występuje błąd wiązania.

### <a name="candidate-user-defined-operators"></a>Operatory zdefiniowane przez użytkownika Release Candidate

Podany typ `T` i operacji `operator op(A)`, gdzie `op` jest Oczekiwano operatora i `A` jest zdefiniowane przez użytkownika operatory, które są dostarczane przez listę argumentów, zbiór Release candidate `T` dla `operator op(A)` jest określana w następujący sposób:

*  Określanie typu `T0`. Jeśli `T` jest typem zerowalnym `T0` jest jej typ podstawowy, w przeciwnym razie `T0` jest równa `T`.
*  Dla wszystkich `operator op` deklaracji w `T0` i zniesione wszystkie rodzaje tych operatorów, jeśli co najmniej jeden operator ma zastosowanie ([dotyczy funkcja składowa](expressions.md#applicable-function-member)) w odniesieniu do listy argumentów `A`, następnie zestawu Operatory Release Candidate składa się z tych operatorów stosowanych w `T0`.
*  W przeciwnym razie, jeśli `T0` jest `object`, zestaw operatorów Release candidate jest pusty.
*  W przeciwnym razie zestaw operatorów Release candidate udostępniane przez `T0` to zestaw operatorów Release candidate, dostarczone przez bezpośrednie klasy bazowej `T0`, lub od klasy bazowej `T0` Jeśli `T0` jest parametrem typu.

### <a name="numeric-promotions"></a>Promocji na liczbowe

Promocji na liczbowe składa się z automatyczne wykonywanie niektórych niejawne konwersje operandów jednoargumentowe wstępnie zdefiniowanych i operatory dwuargumentowe liczbowych. Promocji na liczbowe nie jest mechanizm różne, ale raczej efekt zastosowania rozwiązania przeciążenia operatorów wstępnie zdefiniowane. Promocji na liczbowe specjalnie nie wpływa na obliczania operatorów zdefiniowanych przez użytkownika, mimo że operatory zdefiniowane przez użytkownika mogą zostać wdrożone wykazują podobne skutki.

Jako przykład promocji na liczbowe należy wziąć pod uwagę wstępnie zdefiniowanych implementacjach dane binarne `*` operator:

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

Kiedy przeciążenie reguł rozwiązywania ([Rozpoznanie przeciążenia](expressions.md#overload-resolution)) są stosowane do tego zestawu operatorów, efekt polega na wybraniu pierwszej operatorów, dla których istnieje niejawne konwersje z argumentami o typach. Na przykład dla operacji `b * s`, gdzie `b` jest `byte` i `s` jest `short`, przeciążenia, wybiera rozpoznawania `operator *(int,int)` jako operator najlepsze. W związku z tym, powoduje, że `b` i `s` są konwertowane na `int`, a typ wyniku jest `int`. Podobnie, dla operacji `i * d`, gdzie `i` jest `int` i `d` jest `double`, przeciążenia, wybiera rozpoznawania `operator *(double,double)` jako operator najlepsze.

#### <a name="unary-numeric-promotions"></a>Jednoargumentowy liczbowych promocje

Jednoargumentowy liczbowych podwyższania poziomu pojawia się dla argumentów operacji jest wstępnie zdefiniowane `+`, `-`, i `~` operatorów jednoargumentowych. Jednoargumentowy liczbowych podwyższania poziomu po prostu składa się z konwersji argumentów operacji typu `sbyte`, `byte`, `short`, `ushort`, lub `char` na typ `int`. Ponadto w przypadku jednoargumentowe `-` operatora jednoargumentowego liczbowych podwyższania poziomu konwertuje argumentów operacji typu `uint` na typ `long`.

#### <a name="binary-numeric-promotions"></a>Binarny promocji liczbowe

Binarny liczbowych podwyższania poziomu pojawia się dla argumentów operacji jest wstępnie zdefiniowane `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, i `<=` operatory binarne. Binarny liczbowych podwyższania poziomu niejawnie konwertuje oba operandy do typu wspólnego, które w razie nierelacyjnych operatorów, staje się również typ wyniku operacji. Binarny liczbowych podwyższania poziomu polega na stosowaniu następujące reguły, w kolejności, w jakiej znajdują się w tym miejscu:

*  Jeśli jeden z operandów jest typu `decimal`, to drugi operand jest konwertowany na typ `decimal`, lub występuje błąd powiązania, to drugi operand jest typu `float` lub `double`.
*  W przeciwnym razie, jeśli jeden z operandów jest typu `double`, to drugi operand jest konwertowany na typ `double`.
*  W przeciwnym razie, jeśli jeden z operandów jest typu `float`, to drugi operand jest konwertowany na typ `float`.
*  W przeciwnym razie, jeśli jeden z operandów jest typu `ulong`, to drugi operand jest konwertowany na typ `ulong`, lub występuje błąd powiązania, to drugi operand jest typu `sbyte`, `short`, `int`, lub `long`.
*  W przeciwnym razie, jeśli jeden z operandów jest typu `long`, to drugi operand jest konwertowany na typ `long`.
*  W przeciwnym razie, jeśli jeden z operandów jest typu `uint` i drugi operand jest typu `sbyte`, `short`, lub `int`, oba operandy są konwertowane na typ `long`.
*  W przeciwnym razie, jeśli jeden z operandów jest typu `uint`, to drugi operand jest konwertowany na typ `uint`.
*  W przeciwnym razie oba operandy są konwertowane na typ `int`.

Należy pamiętać, że pierwszej reguły nie zezwala na wszystkie operacje mieszać `decimal` to typ `double` i `float` typów. Reguła wynika z faktu, że istnieje nie niejawne konwersje między `decimal` typu i `double` i `float` typów.

Należy również zauważyć, że nie jest możliwe, że argument typu `ulong` kiedy drugi operand jest typ całkowity ze znakiem. Przyczyną jest to, że istnieje nie typu całkowitego, która może reprezentować pełny zakres `ulong` oraz podpisanych typów całkowitych.

W obu powyższych przypadkach wyrażenia rzutowania można jawnie przekonwertować jeden argument do typu, który jest zgodny z drugiego operandu.

W przykładzie
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
występuje błąd powiązania, ponieważ `decimal` nie może zostać pomnożona przez `double`. Błąd został rozwiązany przez jawnej konwersji drugiego operandu `decimal`, wykonując następujące czynności:

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>Operatory podniesionym

***Podniesiony operatory*** zezwala na wstępnie zdefiniowanych i zdefiniowanych przez użytkownika operatorów, które działają na typy wartości nieprzyjmujące ma być używany z formularzami dopuszcza wartości null z tych typów. Operatory podniesionym są konstruowane na podstawie wstępnie zdefiniowanych i zdefiniowanych przez użytkownika operatorów, które spełniają określone wymagania zgodnie z opisem w następujących czynności:

*   Operatorów jednoargumentowych

    ```csharp
    +  ++  -  --  !  ~
    ```

    podniesionym formie operatora istnieje, jeśli typy argumentów i wynik są typami wartości niedopuszczającym wartości. Podniesionym formularza jest tworzony przez dodanie jednego `?` modyfikator typom argumentów i wyniku. Operator podniesionym tworzy wartość null, jeśli argument ma wartość null. W przeciwnym razie podniesionym operator dekoduje operand, stosuje operator podstawowych i otacza wynik.

*   Aby uzyskać operatory binarne

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    podniesionym formie operatora istnieje, jeśli typy argumentów i wynik są wszystkie typy wartości nieprzyjmujące wartości. Podniesionym formularza jest tworzony przez dodanie jednego `?` modyfikator dla każdego typu argumentu operacji i wyników. Operator podniesionym tworzy wartość null, jeśli jeden lub oba operandy mają wartość null (wyjątek jest `&` i `|` Operatorzy `bool?` typ, zgodnie z opisem w [logiczna operatorów logicznych](expressions.md#boolean-logical-operators)). W przeciwnym razie podniesionym operator dekoduje operandów, stosuje operator podstawowych i otacza wynik.

*   Aby uzyskać Operatory równości

    ```csharp
    ==  !=
    ```

    istnieje podniesionym formie operatora, jeśli typy argumentów operacji są typy wartości nieprzyjmujące i czy typ wyniku `bool`. Podniesionym formularza jest tworzony przez dodanie jednego `?` modyfikator dla każdego typu operand. Operator podniesionym uwzględnia równości dwóch wartości null i wartości null nierówne na jakąkolwiek wartość inną niż null. Jeśli oba operandy są inne niż null, operator podniesionym dekoduje operandów i stosuje bazowego operatora, który generuje `bool` wynik.

*   Aby uzyskać operatory relacyjne

    ```csharp
    <  >  <=  >=
    ```

    istnieje podniesionym formie operatora, jeśli typy argumentów operacji są typy wartości nieprzyjmujące i czy typ wyniku `bool`. Podniesionym formularza jest tworzony przez dodanie jednego `?` modyfikator dla każdego typu operand. Operator podniesionym generuje wartość `false` Jeśli jeden lub oba operandy są wartości null. W przeciwnym razie podniesionym operator dekoduje argumenty operacji i stosuje bazowego operatora, który generuje `bool` wynik.

## <a name="member-lookup"></a>Wyszukiwanie elementu członkowskiego

Wyszukiwanie elementu członkowskiego jest proces, według której jest określana znaczenie nazwę w kontekście określonego typu. Wyszukanie członka może wystąpić w ramach oceny *simple_name* ([proste nazwy](expressions.md#simple-names)) lub *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) w wyrażenie. Jeśli *simple_name* lub *member_access* występuje jako *primary_expression* z *invocation_expression* ([ Wywołań metod](expressions.md#method-invocations)), jest nazywany można wywołać elementu członkowskiego.

W przypadku metody lub zdarzenia, czy jest stała, pole lub właściwość typu delegata ([delegatów](delegates.md)) lub typu `dynamic` ([typu dynamicznego](types.md#the-dynamic-type)), a następnie element członkowski jest nazywany *można wywołać*.

Wyszukanie członka bierze pod uwagę nie tylko nazwę elementu członkowskiego, ale także liczbę parametrów typu, który element członkowski ma i tego, czy element jest dostępny. Na potrzeby wyszukanie członka metody rodzajowe i zagnieżdżonych typów rodzajowych ma liczbę parametrów typu w swoich deklaracjach odpowiednich i inni członkowie mają parametrów typu.

Wyszukiwanie elementu członkowskiego nazwy `N` z `K`  parametrów typu w typie `T` są przetwarzane w następujący sposób:

*  Po pierwsze, zbiór dostępne elementy członkowskie o nazwie `N` jest określana:
    * Jeśli `T` jest parametrem typu, a następnie zestaw jest złożenie zestawy dostępne elementy członkowskie o nazwie `N` w każdym z typami określonymi jako ograniczenia podstawowego lub ograniczenia dodatkowej ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) dla  `T`, wraz z zestawu dostępnych składowych o nazwie `N` w `object`.
    * W przeciwnym razie zestaw zawiera wszystkie dostępne ([dostęp do elementu członkowskiego](basic-concepts.md#member-access)) elementy członkowskie o nazwie `N` w `T`, w tym dziedziczone elementy członkowskie i dostępne elementy członkowskie, o nazwie `N` w `object`. Jeśli `T` jest zbudowany typ, zestaw elementów członkowskich są uzyskiwane poprzez zastąpienie argumentów typu zgodnie z opisem w [członkowie typy utworzone](classes.md#members-of-constructed-types). Elementy członkowskie, które obejmują `override` modyfikator są wykluczone z zestawu.
*  Następnie, jeśli `K` wynosi zero, wszystkie zagnieżdżone typy, których deklaracje zawierają parametry typu są usuwane. Jeśli `K` jest nie jest zerowa, wszystkie elementy członkowskie z różną liczbę typów parametrów są usuwane. Należy pamiętać, że w przypadku `K` zero, metody ma typ parametry nie są usuwane, ponieważ procesu wnioskowania typu ([wnioskowanie o typie](expressions.md#type-inference)) można wywnioskować argumentów typu.
*  Następnie, jeśli element jest *wywoływane*, wszystkich innych niż-*można wywołać* elementy członkowskie są usuwane z zestawu.
*  Następnie elementy członkowskie, które są ukryte przez innych członków są usuwane z zestawu. Dla każdego członka `S.M` w zestawie, gdzie `S` jest typem, w którym element członkowski `M` zadeklarowano, stosowane są następujące reguły:
    * Jeśli `M` jest — stała, pola, właściwości, zdarzenia lub elementu członkowskiego wyliczenia, a następnie wszystkich elementów członkowskich zadeklarowanych w podstawowym typem `S` są usuwane z zestawu.
    * Jeśli `M` jest deklaracja typu, a następnie wszystkie inne niż typy zadeklarowane w typie podstawowym z `S` są usuwane z zestawu, a następnie wpisz wszystkie deklaracje z taką samą liczbę parametrów typu jako `M` zadeklarowane w typie podstawowym z `S` są usuwane z tego zestawu.
    * Jeśli `M` jest metodą, a następnie wszyscy członkowie-metoda zadeklarowana w typie podstawowym z `S` są usuwane z zestawu.
*  Następnie składowych interfejsu, ukryte przez elementy członkowskie klasy są usuwane z zestawu. Ten krok tylko obowiązuje, jeśli `T` jest parametrem typu i `T` innych niż ma zarówno skuteczne klasę bazową `object` i pusty interfejs skuteczne, konfiguruje ([ograniczenia parametru typu](classes.md#type-parameter-constraints)). Dla każdego członka `S.M` w zestawie, gdzie `S` jest typem, w którym element członkowski `M` zadeklarowano, stosowane są następujące reguły, jeśli `S` jest inny niż deklarację klasy `object`:
    * Jeśli `M` jest — stała, pola, właściwości, zdarzenia, składowej wyliczenia lub deklaracji typu, a następnie wszystkich elementów członkowskich zadeklarowanych w deklaracji interfejsu są usuwane z zestawu.
    * Jeśli `M` jest metodą, a następnie wszystkich-metoda elementów członkowskich zadeklarowanych w deklaracji interfejsu są usuwane z zestawu i wszystkich metod z taką samą sygnaturę jak `M` zadeklarowane w interfejsie deklaracji są usuwane z zestawu.
*  Ponadto usunięto ukrytych członków, jest określana wynik wyszukiwania:
    * Jeżeli zestaw zawiera jeden element członkowski, który nie jest metodą, ten element jest wynikiem wyszukiwania.
    * W przeciwnym razie jeśli zestaw zawiera tylko metody, tej grupy metod jest wynikiem wyszukiwania.
    * W przeciwnym razie wyszukiwanie jest niejednoznaczne i występuje błąd wiązania.

Element członkowski wyszukiwań w typów innych niż parametrów typu i interfejsy i wyszukiwania elementu członkowskiego w interfejsach, które są ściśle pojedyncze dziedziczenie (każdego interfejsu w łańcuch dziedziczenia ma dokładnie zero lub jeden bezpośredni interfejs podstawowy) powoduje reguły wyszukiwania po prostu który uzyskiwany składowe bazowe Ukryj członków przy użyciu tej samej nazwie lub podpisie. Takie pojedyncze dziedziczenie wyszukiwań nigdy nie są niejednoznaczne. Niejednoznaczności, które prawdopodobnie mogą wynikać z elementu członkowskiego wyszukiwań w interfejsach dziedziczenia wielokrotnego są opisane w [interfejs dostępu do elementu członkowskiego](interfaces.md#interface-member-access).

### <a name="base-types"></a>Typy podstawowe

Na potrzeby wyszukiwania elementu członkowskiego, typem `T` została uznana za następujące typy podstawowe:

*  Jeśli `T` jest `object`, następnie `T` nie ma podstawowego typu.
*  Jeśli `T` jest *enum_type*, typy podstawowe z `T` są typami klas `System.Enum`, `System.ValueType`, i `object`.
*  Jeśli `T` jest *struct_type*, typy podstawowe z `T` są typami klas `System.ValueType` i `object`.
*  Jeśli `T` jest *class_type*, typy podstawowe z `T` są klasy bazowe `T`, łącznie z typem klasy `object`.
*  Jeśli `T` jest *interface_type*, typy podstawowe z `T` są interfejsy podstawowe z `T` i typ klasy `object`.
*  Jeśli `T` jest *array_type*, typy podstawowe z `T` są typami klas `System.Array` i `object`.
*  Jeśli `T` jest *delegate_type*, typy podstawowe z `T` są typami klas `System.Delegate` i `object`.

## <a name="function-members"></a>Elementy członkowskie — funkcja

Składowe funkcji są elementy członkowskie, które zawierać instrukcji wykonywalnych. Składowe funkcji są zawsze członkowie typów i nie może być elementów członkowskich przestrzeni nazw. Język C# definiuje następujące kategorie funkcji elementów członkowskich:

*  Metody
*  Właściwości
*  Zdarzenia
*  Indeksatory
*  Operatory zdefiniowane przez użytkownika
*  Konstruktory wystąpień
*  Konstruktory statyczne
*  Destruktory

Z wyjątkiem destruktory i konstruktory statyczne (których nie można jawnie wywołać) instrukcje zawarte w składowe funkcji są wykonywane za pośrednictwem wywołania elementu członkowskiego funkcji. Składnia pisania wywołanie funkcji elementu członkowskiego jest zależna od kategorię składowej określonej funkcji.

Na liście argumentów ([listy argumentów](expressions.md#argument-lists)) funkcja elementu członkowskiego wywołania zawiera rzeczywiste wartości i odwołań do zmiennych dla parametrów funkcji elementu członkowskiego.

Wywołań metod ogólnych może wykorzystywać wnioskowanie o typie do określania zestawu argumentów typu do przekazania do metody. Ten proces jest opisany w [wnioskowanie o typie](expressions.md#type-inference).

Wywołania metody, indeksatory, operatory i konstruktory wystąpień zatrudniać Rozpoznanie przeciążenia, aby określić, które zestaw Release candidate elementów członkowskich funkcja do wywołania. Ten proces jest opisany w [Rozpoznanie przeciążenia](expressions.md#overload-resolution).

Po członka określonej funkcji został określony w momencie powiązania, prawdopodobnie za pośrednictwem rozwiązania przeciążenia rzeczywisty proces środowiska wykonawczego wywołanie funkcji elementu członkowskiego jest opisany w [sprawdzanie dynamiczne przeciążonymkompilacji](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Poniższa tabela zawiera podsumowanie przetwarzania, które odbywa się w konstrukcji obejmujące sześć kategorii funkcji elementów członkowskich, które mogą być wywoływane jawnie. W tabeli `e`, `x`, `y`, i `value` wskazują sklasyfikowane jako zmiennych lub wartości wyrażenia `T` wskazuje wyrażenie sklasyfikowane jako typ `F` jest prostą nazwą metody i `P` jest prostą nazwą właściwości.


| __Construct__     | __Przykład__    | __Opis__ |
|-------------------|----------------|-----------------|
| Wywołanie metody | `F(x,y)`       | Rozpoznanie przeciążenia jest stosowany do wybrania najlepszej metody `F` w zawierającego klasy lub struktury. Metoda jest wywoływana z listy argumentów `(x,y)`. Jeśli metoda nie jest `static`, wyrażenie wystąpienia jest `this`. | 
|                   | `T.F(x,y)`     | Rozpoznanie przeciążenia jest stosowany do wybrania najlepszej metody `F` w klasie lub strukturze `T`. Występuje błąd powiązania, jeśli metoda nie jest `static`. Metoda jest wywoływana z listy argumentów `(x,y)`. | 
|                   | `e.F(x,y)`     | Rozpoznanie przeciążenia jest stosowany do Wybierz najlepszą metodę F klasy, struktury lub interfejsu, określone przez typ `e`. Występuje błąd powiązania, jeśli metoda jest `static`. Metoda jest wywoływana za pomocą wyrażenia wystąpienia `e` i listą argumentów `(x,y)`. | 
| Dostęp do właściwości   | `P`            | `get` Metody dostępu właściwości `P` w zawierającego klasy lub struktury jest wywoływana. Występuje błąd kompilacji, jeśli `P` jest tylko do zapisu. Jeśli `P` nie `static`, wyrażenie wystąpienia jest `this`. | 
|                   | `P = value`    | `set` Metody dostępu właściwości `P` w zawierającego klasy lub struktury jest wywoływana z listy argumentów `(value)`. Występuje błąd kompilacji, jeśli `P` jest tylko do odczytu. Jeśli `P` nie `static`, wyrażenie wystąpienia jest `this`. | 
|                   | `T.P`          | `get` Metody dostępu właściwości `P` w klasie lub strukturze `T` jest wywoływana. Występuje błąd kompilacji, jeśli `P` nie `static` lub jeśli `P` jest tylko do zapisu. | 
|                   | `T.P = value`  | `set` Metody dostępu właściwości `P` w klasie lub strukturze `T` jest wywoływana z listy argumentów `(value)`. Występuje błąd kompilacji, jeśli `P` nie `static` lub jeśli `P` jest tylko do odczytu. | 
|                   | `e.P`          | `get` Metody dostępu właściwości `P` w klasy, struktury lub interfejsu, określone przez typ `e` wywoływaną za pomocą wyrażenia wystąpienia `e`. Występuje błąd powiązania, jeśli `P` jest `static` lub jeśli `P` jest tylko do zapisu. | 
|                   | `e.P = value`  | `set` Metody dostępu właściwości `P` w klasy, struktury lub interfejsu, określone przez typ `e` wywoływaną za pomocą wyrażenia wystąpienia `e` i listą argumentów `(value)`. Występuje błąd powiązania, jeśli `P` jest `static` lub jeśli `P` jest tylko do odczytu. | 
| Dostęp do zdarzenia      | `E += value`   | `add` Metody dostępu zdarzenia `E` w zawierającego klasy lub struktury jest wywoływana. Jeśli `E` jest nie statyczne wyrażenie wystąpienia jest `this`. | 
|                   | `E -= value`   | `remove` Metody dostępu zdarzenia `E` w zawierającego klasy lub struktury jest wywoływana. Jeśli `E` jest nie statyczne wyrażenie wystąpienia jest `this`. | 
|                   | `T.E += value` | `add` Metody dostępu zdarzenia `E` w klasie lub strukturze `T` jest wywoływana. Występuje błąd powiązania, jeśli `E` nie jest statyczne. | 
|                   | `T.E -= value` | `remove` Metody dostępu zdarzenia `E` w klasie lub strukturze `T` jest wywoływana. Występuje błąd powiązania, jeśli `E` nie jest statyczne. | 
|                   | `e.E += value` | `add` Metody dostępu zdarzenia `E` w klasy, struktury lub interfejsu, określone przez typ `e` wywoływaną za pomocą wyrażenia wystąpienia `e`. Występuje błąd powiązania, jeśli `E` jest statyczna. | 
|                   | `e.E -= value` | `remove` Metody dostępu zdarzenia `E` w klasy, struktury lub interfejsu, określone przez typ `e` wywoływaną za pomocą wyrażenia wystąpienia `e`. Występuje błąd powiązania, jeśli `E` jest statyczna. | 
| Dostęp indeksatora    | `e[x,y]`       | Rozpoznanie przeciążenia jest stosowany do Wybierz najlepsze indeksatora w klasy, struktury lub interfejsu, określone przez typ e. `get` Akcesor indeksatora jest wywoływana z wyrażenia wystąpienia `e` i listą argumentów `(x,y)`. Błąd powiązania występuje, jeśli indeksatora jest tylko do zapisu. | 
|                   | `e[x,y] = value` | Rozpoznanie przeciążenia jest stosowany do Wybierz najlepsze indeksatora w klasy, struktury lub interfejsu, określone przez typ `e`. `set` Akcesor indeksatora jest wywoływana z wyrażenia wystąpienia `e` i listą argumentów `(x,y,value)`. Błąd powiązania występuje, jeśli indeksatora jest tylko do odczytu. | 
| Operator wywołania | `-x`         | Rozpoznanie przeciążenia jest stosowany do Wybierz najlepsze operatora jednoargumentowego klasy lub struktury, określone przez typ `x`. Wybrane operator jest wywoływany z listy argumentów `(x)`. | 
|                     | `x + y`      | Rozpoznanie przeciążenia jest stosowany do wybrania najlepszej operatora binarnego klasach lub strukturach określone przez typy `x` i `y`. Wybrane operator jest wywoływany z listy argumentów `(x,y)`. | 
| Wywołanie konstruktora wystąpienia | `new T(x,y)` | Rozpoznanie przeciążenia jest stosowany do Wybierz najlepsze konstruktora wystąpienia klasy lub struktury `T`. Konstruktor wystąpienia jest wywoływana z listy argumentów `(x,y)`. | 

### <a name="argument-lists"></a>Listy argumentów

Każdy element członkowski i delegata wywołania funkcji obejmuje udostępniającej rzeczywiste wartości i odwołań do zmiennych parametrów funkcja składowa listy argumentów. Składnia służąca do określania listy argumentów wywołania funkcji elementu członkowskiego zależy od kategorii element członkowski funkcji:

*  Na przykład konstruktorów, metod, indeksatorów i delegatów, argumenty są określane jako *argument_list*, zgodnie z poniższym opisem. Dla indeksatorów, podczas wywoływania `set` akcesora do listy argumentów Ponadto obejmuje wyrażenie określone jako prawy operand operatora przypisania.
*  W przypadku właściwości lista argumentów jest pusta, podczas wywoływania `get` metody dostępu i składa się z wyrażenie określone jako prawy operand operatora przypisania podczas wywoływania `set` metody dostępu.
*  W przypadku zdarzeń z listą argumentów składa się z wyrażenie określone jako prawy operand `+=` lub `-=` operatora.
*  Operatory zdefiniowane przez użytkownika na liście argumentów składa się z jeden argument operacji operatora jednoargumentowego lub dwa argumenty operacji operatora binarnego.

Argumenty właściwości ([właściwości](classes.md#properties)), zdarzenia ([zdarzenia](classes.md#events)) i operatorów zdefiniowanych przez użytkownika ([operatory](classes.md#operators)) zawsze są przekazywane jako wartości parametrów ([ Wartości parametrów](classes.md#value-parameters)). Argumenty indeksatorów ([indeksatory](classes.md#indexers)) zawsze są przekazywane jako wartości parametrów ([wartości parametrów](classes.md#value-parameters)) lub parameter — tablice ([Parameter — tablice](classes.md#parameter-arrays)). Parametry odwołań i dane wyjściowe nie są obsługiwane dla tych kategorii funkcji elementów członkowskich.

Argumenty wywołania konstruktora, metoda, indeksator lub delegata wystąpienia są określane jako *argument_list*:

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

*Argument_list* składa się z co najmniej jeden *argument*s, oddzielonych przecinkami. Każdy argument składa się opcjonalny *argument_name* następuje *argument_value*. *Argument* z *argument_name* nazywa się ***nazwany argument***, podczas gdy *argument* bez  *argument_name* jest ***argument pozycyjny***. Jest to błąd dla argumentu pozycyjnego pojawi się po argumentu nazwanego w *argument_list*.

*Argument_value* może mieć jedną z następujących form:

*  *Wyrażenie*, co oznacza, że argument jest przekazywany jako parametr wartości ([wartości parametrów](classes.md#value-parameters)).
*  Słowo kluczowe `ref` następuje *variable_reference* ([odwołań do zmiennych](variables.md#variable-references)), który wskazuje, że argument jest przekazywany jako parametr przekazany przez odwołanie ([odwołania do parametrów ](classes.md#reference-parameters)). Zmienna musi zostać zdecydowanie przypisany ([asercję określonego przypisania](variables.md#definite-assignment)) zanim może być przekazywany jako parametr przekazany przez odwołanie. Słowo kluczowe `out` następuje *variable_reference* ([odwołań do zmiennych](variables.md#variable-references)), co oznacza, że argument jest przekazywany jako parametr wyjściowy ([parametrówwyjściowych](classes.md#output-parameters)). Zmienna jest uznawany za zdecydowanie przypisany ([asercję określonego przypisania](variables.md#definite-assignment)) po funkcji wywołanie elementu członkowskiego, w którym zmienna jest przekazywana jako parametr wyjściowy.

#### <a name="corresponding-parameters"></a>Odpowiednich parametrów

Dla każdego argumentu na liście argumentów musi istnieć odpowiedni parametr w funkcji składowej lub delegata wywoływanego.

Lista parametrów używanych w następujących jest określany w następujący sposób:

*  Dla metod wirtualnych i zdefiniowane indeksatory w klasach lista parametrów jest pobierana z bardziej konkretny od pozostałych deklaracji lub zastąpienie elementu członkowskiego funkcji, rozpoczynając od typu statycznego odbiornik i przeszukiwania jej klas podstawowych.
*  Dla metody interfejsu i indeksatorów, lista parametrów jest pobierane tworzą bardziej konkretny od pozostałych definicji elementu członkowskiego, rozpoczynając od typu interfejsu i przeszukiwania interfejsy podstawowe. Jeśli zostanie znaleziony żadnej listy parametrów unikatowy, listę parametrów z nazwami niedostępne i nie opcjonalne parametry jest konstruowany tak, aby wywołania nie można użyć nazwanych parametrów lub pominięcie argumentów opcjonalnych.
*  Dla metod częściowych jest używany parametr listę definiujące deklaracji metody częściowej.
*  W przypadku pozostałych funkcji elementów członkowskich i delegatów istnieje tylko jeden parametr listę, która jest używana.

Pozycja argumentu lub parametr jest zdefiniowany jako liczba argumentów lub parametry poprzedzającym go na liście argumentów lub listy parametrów.

Odpowiednich parametrów dla funkcji składowej argumentów są określane w następujący sposób:

*  Argumenty w *argument_list* konstruktory wystąpień, metod, indeksatorów i delegatów:
    * Argument pozycyjny realizowana stały parametr w tej samej pozycji w liście parametrów odnosi się do tego parametru.
    * Argument pozycyjny funkcja elementu członkowskiego z tablicą parametrów, wywoływana w postaci normalne odnosi się do tablicy parametrów, która musi wystąpić na tej samej pozycji w liście parametrów.
    * Argument pozycyjny funkcja elementu członkowskiego z tablicą parametrów, wywoływana w postaci rozwiniętej ma stały parametru realizowana na tej samej pozycji w liście parametrów, odnosi się do elementu w tablicy parametrów.
    * Argument nazwany odnosi się do parametru o takiej samej nazwie w liście parametrów.
    * Dla indeksatorów, podczas wywoływania `set` dostępu, wyrażenie określone jako prawy operand operatora przypisania odpowiada niejawny `value` parametru `set` deklaracji metody dostępu.
*  W przypadku właściwości podczas wywoływania `get` ma metody dostępu są bez argumentów. Podczas wywoływania `set` dostępu, wyrażenie określone jako prawy operand operatora przypisania odpowiada niejawny `value` parametru `set` deklaracji metody dostępu.
*  Operatory jednoargumentowe zdefiniowanych przez użytkownika (w tym konwersji) jeden argument operacji jest odpowiada jeden parametr Deklaracja operatora.
*  Dla zdefiniowanych przez użytkownika operatory dwuargumentowe lewy operand odnosi się do pierwszego parametru i prawy operand odnosi się do drugiego parametru Deklaracja operatora.

#### <a name="run-time-evaluation-of-argument-lists"></a>Ocena środowiska wykonawczego z listami argumentów

Podczas przetwarzania środowiska wykonawczego wywołanie funkcji elementu członkowskiego ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), wyrażeń lub zmiennych odwołania do listy argumentów są obliczane w kolejności od lewej do prawej, jako następujące:

*  Wartość parametru, jest obliczane wyrażenie argumentu i niejawną konwersję ([niejawne konwersje](conversions.md#implicit-conversions)) do odpowiedniego parametru typu jest wykonywane. Wartość wynikowa staje się wartością początkową parametru wartości w wywołanie funkcji elementu członkowskiego.
*  Dla parametru odwołania lub danych wyjściowych odwołanie do zmiennej jest szacowana, a lokalizacja wynikowa magazynu staje się lokalizacji przechowywania, reprezentowany przez parametr w wywołaniu elementu członkowskiego funkcji. Jeśli odwołanie do zmiennej podawana jako parametr odwołanie lub danych wyjściowych jest element tablicy *reference_type*, sprawdzanie w czasie wykonania jest przeprowadzana w celu zapewnienia, że typ elementu tablicy jest taka sama jak typ parametru. Jeżeli to sprawdzenie nie powiedzie się, `System.ArrayTypeMismatchException` zgłaszany.

Metody, indeksatory i konstruktory wystąpień może deklarować ich parametr najdalej z prawej strony tablicy parametrów ([Parameter — tablice](classes.md#parameter-arrays)). Takich funkcji elementów członkowskich są wywoływane w ich normalnym formularza lub w postaci rozwiniętej zależności, na których jest stosowane ([dotyczy funkcja składowa](expressions.md#applicable-function-member)):

*  Wywołana funkcja składowa z tablicą parametrów w postaci normalne argument dla tablicy parametrów musi być wyrażeniem pojedynczym, który jest niejawnie konwertowany ([niejawne konwersje](conversions.md#implicit-conversions)) na typ tablicy parametrów. W tym przypadku tablicy parametrów funkcjonuje dokładnie parametru wartości.
*  Wywołana funkcja składowa z tablicą parametrów w postaci rozwiniętej wywołania należy określić zero lub więcej argumentów pozycyjnych tablicy parametrów, w którym każdy argument jest wyrażeniem, które jest niejawnie konwertowany ([niejawne Konwersje](conversions.md#implicit-conversions)) na typ elementu tablicy parametrów. W takim przypadku wywołanie tworzy wystąpienie typu parametru tablicy o długości odpowiadającej liczbę argumentów, inicjuje elementy wystąpienia tablicy wartościami podany argument i używa wystąpienia nowo utworzona tablica jako rzeczywisty argument.

Wyrażenia listy argumentów są zawsze obliczane w kolejności, w której są zapisywane. W związku z tym w tym przykładzie
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
generuje dane wyjściowe
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

Reguły odchyleń wspólnej tablicy ([Kowariancja tablicy](arrays.md#array-covariance)) zezwala na wartość typu tablicowego `A[]` jako odwołanie do wystąpienia typu tablicowego `B[]`, o ile istnieje niejawna konwersja odwołania `B` do `A`. Ze względu na tych zasad, gdy element tablicy *reference_type* jest przekazywany jako parametr odwołanie lub danych wyjściowych, sprawdzanie w czasie wykonania jest wymagany w celu zagwarantowania, że typ rzeczywisty element tablicy jest taka sama jak w przypadku parametru. W przykładzie
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
drugie wywołanie `F` powoduje, że `System.ArrayTypeMismatchException` zostanie wygenerowany, ponieważ element rzeczywisty typ `b` jest `string` i nie `object`.

Wywołana funkcja składowa z tablicą parametrów w postaci rozwiniętej wywołanie jest przetwarzany, dokładnie tak, jakby wyrażenie tworzenia tablicy za pomocą inicjatora tablicy ([wyrażenie tworzenia tablicy](expressions.md#array-creation-expressions)) został wstawiony wokół Parametry rozwinięty. Na przykład biorąc pod uwagę deklaracji
```csharp
void F(int x, int y, params object[] args);
```
następujące wywołania rozszerzonej postaci metody
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
dokładnie odpowiadać
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

W szczególności należy pamiętać, że pusta tablica jest tworzony w przypadku zero argumentów dla tablicy parametrów.

Jeśli argumenty zostały pominięte w funkcja składowa za pomocą odpowiednich parametrów opcjonalnych, argumenty domyślne deklaracji elementu członkowskiego funkcji niejawnie są przekazywane. Ponieważ to jest zawsze stałe, ich oceny nie ma wpływu na kolejność oceny pozostałe argumenty.

### <a name="type-inference"></a>Wnioskowanie o typie

Po wywołaniu metody ogólnej bez określania argumentów typu ***wnioskowanie o typie*** proces próbuje wywnioskować argumentów typu na wywołanie. Obecność wnioskowanie o typie umożliwia bardziej wygodne składni ma być używany do wywoływania metody rodzajowej i pozwala programisty uniknąć określania informacji o typie nadmiarowe. Na przykład biorąc pod uwagę deklaracji metody:
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
istnieje możliwość wywołania `Choose` metody bez jawne określenie argumentów typu:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

Za pomocą wnioskowanie o typie, argumenty typu `int` i `string` są ustalane na podstawie argumenty do metody.

Wnioskowanie o typie występuje w ramach przetwarzania powiązania w czasie wywołania metody ([wywołań metody opisywanego](expressions.md#method-invocations)) i ma miejsce przed wykonaniem kroku rozdzielczość przeciążenia wywołania. Gdy w wywołaniu metody zostanie określona grupa konkretnej metody bez argumentów typu są określone jako część wywołania metody, wnioskowanie o typie są stosowane do każdej metody rodzajowej, w grupie metody. Jeśli wnioskowanie o typie zakończy się powodzeniem, argumentami typu wywnioskowanego są używane do określ typy tych argumentów dla kolejnych przeciążeniu rozdzielczości. Jeśli funkcja rozpoznawania przeciążeń wybiera metody rodzajowej, do wywołania, argumentami typu wywnioskowanego są używane jako argumenty typu rzeczywistego wywołania. W przypadku niepowodzenia wnioskowanie o typie dla konkretnej metody tej metody nie uczestniczy w przeciążeniu rozdzielczości. Błąd wnioskowanie o typie, w i samego siebie, nie powoduje błąd wiązania. Jednak często prowadzi do Błąd powiązania w czasie gdy funkcja rozpoznawania przeciążeń następnie nie odnajdzie żadnych odpowiednich metod.

Jeśli podana liczba argumentów jest inna niż liczba parametrów w metodzie, następnie wnioskowania natychmiast kończy się niepowodzeniem. W przeciwnym razie Załóżmy, że metody ogólnej ma następujący podpis:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

Za pomocą wywołania metody formularza `M(E1...Em)` zadanie wnioskowanie o typie jest znalezienie argumentów typu unikatowy `S1...Sn` dla każdego z parametrów typu `X1...Xn` tak, aby wywołanie `M<S1...Sn>(E1...Em)` staje się nieprawidłowy.

W trakcie procesu wnioskowania każdego parametru typu `Xi` jest *stałej* do określonego typu `Si` lub *Niepoprawione* skojarzony zestaw *granice*. Każdy z granicami jest określonego typu `T`. Początkowo każda zmienna typu `Xi` jest Niepoprawione z pustą parą granic.

Wnioskowanie o typie odbywa się etapami. Każda faza podejmie próbę wywnioskować argumentów typu dla więcej zmiennych typu na podstawie otrzymanych wyników poprzedniej fazy. Pierwsza faza sprawia, że niektóre początkowej wniosków granice drugiej fazy zmiennych typu do określonych typów poprawek i dalsze wnioskuje granice. Drugi etap musi być powtarzane wielokrotnie.

*Uwaga:* Typ wnioskowania odbywa się nie tylko w przypadku, gdy wywoływana jest metoda ogólnego. Wnioskowanie o typie dla konwersji grupy metod jest opisana w [wnioskowanie konwersji grupy metod typu](expressions.md#type-inference-for-conversion-of-method-groups) i znajdowanie najlepszy typ wspólnego zestawu wyrażeń jest opisany w [znajdowanie najlepszy typ wspólnego zestawu wyrażeń](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).

#### <a name="the-first-phase"></a>Pierwsza faza

Dla każdego z podanych argumentów metody `Ei`:

*   Jeśli `Ei` jest funkcją anonimową *wnioskowanie o typie parametru jawne* ([parametr jawny typ wniosków](expressions.md#explicit-parameter-type-inferences)) zostało wprowadzone przy użyciu `Ei` do `Ti`
*   W przeciwnym razie, jeśli `Ei` ma typ `U` i `xi` parametr wartość, a następnie *wnioskowania dolna granica* wykonano *z* `U` *do* `Ti`.
*   W przeciwnym razie, jeśli `Ei` ma typ `U` i `xi` jest `ref` lub `out` parametru, a następnie *dokładnie wnioskowania* wykonano *z* `U` *do* `Ti`.
*   W przeciwnym razie wykonywane nie wnioskowania dla tego argumentu.


#### <a name="the-second-phase"></a>Drugi etap

Drugi etap przebiega w następujący sposób:

*   Wszystkie *Niepoprawione* wpisz zmienne `Xi` nie obsługują *zależą od* ([zależność](expressions.md#dependence)) wszelkie `Xj` są stałe ([ustalanie](expressions.md#fixing)).
*   Jeśli istnieje żadnych zmiennych typu, wszystkie *Niepoprawione* wpisz zmienne `Xi` są *stałej* dla którego pomieścić wszystkie z następujących czynności:
    *   Istnieje co najmniej jedną zmienną typu `Xj` zależy `Xi`
    *   `Xi` ma inne niż pusty zestaw granic
*   Jeśli istnieje żadnych zmiennych typu i nadal istnieją *Niepoprawione* wpisz zmiennych, wpisz wnioskowania kończy się niepowodzeniem.
*   W przeciwnym razie, jeśli jest to żadnych dalszych *Niepoprawione* zmiennych typu istnieje, wnioskowanie o typie zakończy się pomyślnie.
*   Inny sposób, w przypadku wszystkich argumentów `Ei` przy użyciu odpowiedniego parametru typu `Ti` gdzie *danych wyjściowych typy* ([danych wyjściowych typy](expressions.md#output-types)) zawierają *Niepoprawione* typ zmiennych `Xj` ale *typów wejściowych* ([typów wejściowych](expressions.md#input-types)) w przeciwnym razie *danych wyjściowych wnioskowanie o typie* ([wniosków o typie danych wyjściowych ](expressions.md#output-type-inferences)) składa się *z* `Ei` *do* `Ti`. Następnie drugiej fazy jest powtarzany.

#### <a name="input-types"></a>Typy wejściowe

Jeśli `E` jest grupa metod lub niejawnie wpisanych funkcja anonimowa i `T` jest delegatem typu lub typ drzewa wyrażeń, a następnie wszystkie typy parametrów z `T` są *typów wejściowych* z `E` *z typem* `T`.

####  <a name="output-types"></a>Typy danych wyjściowych

Jeśli `E` jest grupa metod lub funkcją anonimową i `T` jest delegatem typu lub typ drzewa wyrażeń, a następnie zwracany typ `T` jest *wyjściowych typu* `E` *z typem*  `T`.

#### <a name="dependence"></a>Zależność

*Niepoprawione* zmienna typu `Xi` *zależy bezpośrednio od* zmienną typu Niepoprawione `Xj` if dla niektórych argumentów `Ek` z typem `Tk` `Xj` występuje w *typu danych wejściowych* z `Ek` z typem `Tk` i `Xi` odbywa się w *typ danych wyjściowych* z `Ek` z typem `Tk`.

`Xj` *zależy od* `Xi` Jeśli `Xj` *zależy bezpośrednio od* `Xi` lub jeśli `Xi` *zależy bezpośrednio od* `Xk` i `Xk` *zależy od* `Xj`. Tak więc "jest zależny od" jest przechodnia, ale nie zwrotnej zamknięcia "zależy bezpośrednio od".

#### <a name="output-type-inferences"></a>Wniosków o typie danych wyjściowych

*Danych wyjściowych wnioskowanie o typie* wykonano *z* wyrażenie `E` *do* typu `T` w następujący sposób:

*  Jeśli `E` jest funkcją anonimową z wnioskowanym typem zwracanym `U` ([Inferred zwracany typ](expressions.md#inferred-return-type)) i `T` jest typu delegata lub typ drzewa wyrażeń z typem zwracanym `Tb`, następnie *wnioskowania dolna granica* ([wniosków dolna granica](expressions.md#lower-bound-inferences)) składa się *z* `U` *do* `Tb`.
*  W przeciwnym razie, jeśli `E` jest grupa metod i `T` jest typu delegata lub typ drzewa wyrażeń z typami parametrów `T1...Tk` i zwracany typ `Tb`i rozpoznawanie przeciążenia `E` z typami `T1...Tk` daje pojedyncza metoda z typem zwracanym `U`, a następnie *wnioskowania dolna granica* składa się *z* `U` *do* `Tb`.
*  W przeciwnym razie, jeśli `E` to wyrażenie z typem `U`, a następnie *wnioskowania dolna granica* składa się *z* `U` *do* `T`.
*  W przeciwnym razie są wprowadzane nie wniosków.

#### <a name="explicit-parameter-type-inferences"></a>Parametr jawny typ wniosków

*Wnioskowanie o typie parametru jawne* wykonano *z* wyrażenie `E` *do* typu `T` w następujący sposób:

*  Jeśli `E` jest funkcją anonimową jawnie wpisanych z typami parametrów `U1...Uk` i `T` jest typu delegata lub typ drzewa wyrażeń z typami parametrów `V1...Vk` następnie dla każdego `Ui` *dokładnie wnioskowanie* ([dokładnie wniosków](expressions.md#exact-inferences)) składa się *z* `Ui` *do* odpowiednich `Vi`.

#### <a name="exact-inferences"></a>Dokładne wniosków

*Dokładnie wnioskowania* *z* typu `U` *do* typu `V` składa się w następujący sposób:

*  Jeśli `V` jest jednym z *Niepoprawione* `Xi` następnie `U` zostanie dodany do zestawu dokładne granice dla `Xi`.

*  W przeciwnym wypadku ustawia `V1...Vk` i `U1...Uk` są określane przez sprawdzanie w przypadku spełnienia jednego z następujących przypadków:

   *  `V` jest typem tablicowym `V1[...]` i `U` jest typem tablicowym `U1[...]` o tej samej randze
   *  `V` jest to typ `V1?` i `U` jest typem `U1?`
   *  `V` jest zbudowany typ `C<V1...Vk>`i `U` jest zbudowany typ `C<U1...Uk>`

   Przypadku spełnienia jednego z tych przypadków następnie *dokładnie wnioskowania* wykonano *z* każdego `Ui` *do* odpowiednich `Vi`.

*  W przeciwnym razie są wprowadzane nie wniosków.

#### <a name="lower-bound-inferences"></a>Dolna granica wniosków

A *wnioskowania dolna granica* *z* typu `U` *do* typu `V` składa się w następujący sposób:

*  Jeśli `V` jest jednym z *Niepoprawione* `Xi` następnie `U` zostanie dodany do zestawu dolne granice dla `Xi`.
*  W przeciwnym razie, jeśli `V` jest typem `V1?`i `U` jest typem `U1?` , a następnie wnioskowania dolna granica składa się z `U1` do `V1`.
*  W przeciwnym wypadku ustawia `U1...Uk` i `V1...Vk` są określane przez sprawdzanie w przypadku spełnienia jednego z następujących przypadków:
   *  `V` jest typem tablicowym `V1[...]` i `U` jest typem tablicowym `U1[...]` (lub parametrem typu, którego typ podstawowy skuteczne jest `U1[...]`) o tej samej randze
   *  `V` Przykładem jest `IEnumerable<V1>`, `ICollection<V1>` lub `IList<V1>` i `U` jest typem tablicy jednowymiarowej `U1[]`(lub parametrem typu, którego typ podstawowy skuteczne jest `U1[]`)
   *  `V` skonstruowany typ klasy, struktury, interfejsu lub delegata `C<V1...Vk>` a typ unikatowego `C<U1...Uk>` tak, aby `U` (lub, jeśli `U` jest parametrem typu, klasą bazową skuteczne lub dowolny członek swój zestaw skuteczne interfejsu) jest taka sama jak, dziedziczy (bezpośrednio lub pośrednio) lub implementuje (bezpośrednio lub pośrednio) `C<U1...Uk>`.

      (Ograniczenie "unikatowości" oznacza, że w przypadku interfejsu `C<T> {} class U: C<X>, C<Y> {}`, a następnie wykonywane nie wnioskowania, gdy wnioskowanie z `U` do `C<T>` ponieważ `U1` może być `X` lub `Y`.)

   Przypadku spełnienia jednego z tych przypadków, a następnie następuje wnioskowanie *z* każdego `Ui` *do* odpowiednich `Vi` w następujący sposób:

   *  Jeśli `Ui` nie jest znany jako typ odwołania, a następnie *dokładnie wnioskowania* wykonano
   *  W przeciwnym razie, jeśli `U` jest typem tablicy, a następnie *wnioskowania dolna granica* wykonano
   *  W przeciwnym razie, jeśli `V` jest `C<V1...Vk>` , a następnie wnioskowania zależy od parametru typu i tym `C`:
      *  Jeśli jest kowariantny, a następnie *wnioskowania dolna granica* wykonano.
      *  Jeśli jest kontrawariantny, a następnie *wnioskowania górną granicę* wykonano.
      *  Jeśli jest to niezmienna, a następnie *dokładnie wnioskowania* wykonano.
*  W przeciwnym razie są wprowadzane nie wniosków.

#### <a name="upper-bound-inferences"></a>Górna granica wniosków

*Wnioskowania górną granicę* *z* typu `U` *do* typu `V` składa się w następujący sposób:

*  Jeśli `V` jest jednym z *Niepoprawione* `Xi` następnie `U` zostanie dodany do zestawu górną granicę dla `Xi`.
*  W przeciwnym wypadku ustawia `V1...Vk` i `U1...Uk` są określane przez sprawdzanie w przypadku spełnienia jednego z następujących przypadków:
   *  `U` jest typem tablicowym `U1[...]` i `V` jest typem tablicowym `V1[...]` o tej samej randze
   *  `U` Przykładem jest `IEnumerable<Ue>`, `ICollection<Ue>` lub `IList<Ue>` i `V` jest typem tablicy jednowymiarowej `Ve[]`
   *  `U` jest to typ `U1?` i `V` jest typem `V1?`
   *  `U` jest zbudowany klasy, struktury, typ interfejsu lub delegata `C<U1...Uk>` i `V` to klasy, struktury, typ interfejsu lub delegata, która jest taka sama jak dziedziczy (bezpośrednio lub pośrednio) lub implementuje (bezpośrednio lub pośrednio) typ unikatowego `C<V1...Vk>`

      (Ograniczenie "unikatowości" oznacza, że jeśli będziemy mieć `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, a następnie wykonywane nie wnioskowania, gdy wnioskowanie z `C<U1>` do `V<Q>`. Wniosków nie składają się z `U1` do jednej `X<Q>` lub `Y<Q>`.)

   Przypadku spełnienia jednego z tych przypadków, a następnie następuje wnioskowanie *z* każdego `Ui` *do* odpowiednich `Vi` w następujący sposób:
   *  Jeśli `Ui` nie jest znany jako typ odwołania, a następnie *dokładnie wnioskowania* wykonano
   *  W przeciwnym razie, jeśli `V` jest typem tablicy, a następnie *wnioskowania górną granicę* wykonano
   *  W przeciwnym razie, jeśli `U` jest `C<U1...Uk>` , a następnie wnioskowania zależy od parametru typu i tym `C`:
      *  Jeśli jest kowariantny, a następnie *wnioskowania górną granicę* wykonano.
      *  Jeśli jest kontrawariantny, a następnie *wnioskowania dolna granica* wykonano.
      *  Jeśli jest to niezmienna, a następnie *dokładnie wnioskowania* wykonano.
*  W przeciwnym razie są wprowadzane nie wniosków.   

#### <a name="fixing"></a>Naprawianie

*Niepoprawione* zmienna typu `Xi` z zestawem granice jest *stałej* w następujący sposób:

*  Zbiór *typy Release candidate* `Uj` rozpoczyna się jako zbiór wszystkich typów w zestawie granice dla `Xi`.
*  Każda granica sprawdzamy, następnie `Xi` z kolei: Dla każdej granicy dokładnie `U` z `Xi` wszystkich typów `Uj` które nie są identyczne z `U` są usuwane z zestawu Release candidate. Dla każdego dolna granica `U` z `Xi` wszystkich typów `Uj` do miejsca, które jest *nie* niejawna konwersja z `U` są usuwane z zestawu Release candidate. Dla każdego górną granicę `U` z `Xi` wszystkich typów `Uj` z tego miejsca, które jest *nie* niejawną konwersję do `U` są usuwane z zestawu Release candidate.
*  Jeśli wśród pozostałych typów Release candidate `Uj` istnieje unikatowy typ `V` z której istnieje niejawna konwersja na wszystkich innych Release candidate typy, następnie `Xi` zostanie usunięty z `V`.
*  W przeciwnym razie wnioskowanie o typie kończy się niepowodzeniem.

#### <a name="inferred-return-type"></a>Wnioskowany typ zwracany

Wywnioskowane zwracany typ funkcji anonimowych `F` jest używany podczas rozpoznawania typu wnioskowania i przeciążenia. Wnioskowany typ zwracany można określić tylko dla funkcja anonimowa, na którym wszystkich parametrów, które są znane typy, albo ponieważ one są określone jawnie, dostępne za pośrednictwem konwersję funkcja anonimowa lub wywnioskowane podczas wnioskowania o typie na otaczającego ogólny Wywołanie metody.

***Wywnioskować typ wyniku*** jest określany w następujący sposób:

*  Jeśli treść `F` jest *wyrażenie* ma typ, a następnie typ wyniku wywnioskowane `F` jest typ tego wyrażenia.
*  Jeśli treść `F` jest *bloku* i zestawu wyrażeń w bloku `return` instrukcji jest najlepszy typ typowe `T` ([znajdowanie najlepsze typ wspólny zestaw wyrażeń](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), następnie wnioskowany typ wyniku `F` jest `T`.
*  W przeciwnym razie nie można wywnioskować typu wyniku dla `F`.

***Wywnioskować zwracanego typu*** jest określany w następujący sposób:

*  Jeśli `F` jest asynchroniczne i treści `F` albo wyrażenie, sklasyfikowanych jako nic nie jest ([klasyfikacje wyrażenie](expressions.md#expression-classifications)), lub blok instrukcji, których nie instrukcjach return mają wyrażeń, wnioskowany typ zwracany jest `System.Threading.Tasks.Task`
*  Jeśli `F` async i ma typ wyniku wywnioskowane `T`, wnioskowany typ zwracany jest `System.Threading.Tasks.Task<T>`.
*  Jeśli `F` jest inne niż async i ma typ wyniku wywnioskowane `T`, wnioskowany typ zwracany jest `T`.
*  W przeciwnym razie nie można wywnioskować zwracanego typu dla `F`.

Jako przykład wnioskowanie o typie, obejmujące funkcje anonimowe należy wziąć pod uwagę `Select` — metoda rozszerzenia jest zadeklarowana w `System.Linq.Enumerable` klasy:
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

Zakładając, że `System.Linq` przestrzeń nazw została zaimportowana z `using` klauzuli i biorąc klasy `Customer` z `Name` właściwości typu `string`, `Select` metoda może służyć do wybierz nazwy listę klientów:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

Wywołanie metody rozszerzenia ([wywołań metod rozszerzenia](expressions.md#extension-method-invocations)) z `Select` jest przetwarzany, zapisując je ponownie wywołania do wywołania metody statycznej:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Ponieważ argumenty typu nie zostały jawnie określone, wnioskowanie o typie umożliwia wywnioskować argumentów typu. Po pierwsze, `customers` argument jest powiązany z `source` parametru wnioskowanie `T` jako `Customer`. Następnie przy użyciu funkcji anonimowych procesu wnioskowania opisanych powyżej, wpisz `c` jest danego typu `Customer`i wyrażenie `c.Name` jest powiązana z typem zwracanym `selector` parametru wnioskowanie `S` jako `string`. W związku z tym wywołanie jest równoważne
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
a wynik jest typu `IEnumerable<string>`.

W poniższym przykładzie pokazano sposób anonimowy typ funkcji wnioskowanie umożliwia informacje o typie "przepływ" między argumentów w wywołaniu metody rodzajowej. Podane metody:
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

Wnioskowanie o typie wywołania:
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
przechodzi w następujący sposób: Po pierwsze, argument `"1:15:30"` jest powiązany z `value` parametru wnioskowanie `X` jako `string`. Następnie, parametr pierwszy funkcja anonimowa `s`, otrzymuje wnioskowany typ `string`i wyrażenie `TimeSpan.Parse(s)` jest powiązana z typem zwracanym `f1`, wnioskowanie `Y` jako `System.TimeSpan`. Na koniec parametru Druga funkcja anonimowa `t`, otrzymuje wnioskowany typ `System.TimeSpan`i wyrażenie `t.TotalSeconds` jest powiązana z typem zwracanym `f2`, wnioskowanie `Z` jako `double`. W efekcie jest wynik wywołania typu `double`.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Wnioskowanie o typie dla konwersji grupy metod

Podobnie jak wywołania metody rodzajowe, wnioskowanie o typie należy również będą stosowane podczas grupy metod `M` zawierający metodę ogólną jest konwertowany na typ danego obiektu delegowanego `D` ([metody konwersji grup](conversions.md#method-group-conversions)). Biorąc pod uwagę — metoda
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
i grupy metod `M` przypisywanych do typu delegata `D` zadanie wnioskowanie o typie jest znalezienie argumentów typu `S1...Sn` tak, aby wyrażenie:
```csharp
M<S1...Sn>
```
staje się niezgodne ([delegować deklaracje](delegates.md#delegate-declarations)) przy użyciu `D`.

W przeciwieństwie do algorytmu wnioskowania typu dla wywołań metody rodzajowej, w tym przypadku istnieją tylko argument *typy*, żaden argument *wyrażenia*. W szczególności, nie istnieją żadne funkcje anonimowe i dlatego nie ma potrzeby wielu faz wnioskowania.

Zamiast tego wszystkie `Xi` są traktowane jako *Niepoprawione*i *wnioskowania dolna granica* wykonano *z* typ każdego argumentu `Uj` z `D` *do* odpowiedni typ parametru `Tj` z `M`. Jeśli dla żadnego z `Xi` nie znaleziono żadnych granic, wnioskowanie o typie kończy się niepowodzeniem. W przeciwnym razie wszystkie `Xi` są *stałej* do odpowiadającego `Si`, które są wynikiem wnioskowanie o typie.

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Znajdowanie najlepszy typ wspólnego zestawu wyrażeń

W niektórych przypadkach wspólny typ musi wywnioskować zestawu wyrażeń. W szczególności typy elementu niejawnie wpisane tablice i funkcje anonimowe przy użyciu typów zwracanych *bloku* treści znajdują się w ten sposób.

Intuicyjne, biorąc pod uwagę zestaw wyrażeń `E1...Em` to wnioskowania powinien być odpowiednikiem wywołania metody
```csharp
Tr M<X>(X x1 ... X xm)
```
za pomocą `Ei` jako argumenty.

Bardziej precyzyjne wnioskowania rozpoczyna się od *Niepoprawione* zmienna typu `X`. *Dane wyjściowe wniosków typu* są przeprowadzane *z* każdego `Ei` *do* `X`. Na koniec `X` jest *stałej* i, jeśli to się powiedzie, wynikowy typ `S` jest wynikowy najlepszy typ wspólnych wyrażeń. Jeśli nie ma takiego `S` istnieje, wyrażenia mają nie najlepiej wspólnego typu.

### <a name="overload-resolution"></a>Rozpoznanie przeciążenia

Rozpoznanie przeciążenia jest mechanizmem powiązania w czasie do wybierania najlepszych element członkowski funkcji do wywołania danej listy argumentów i zestaw elementów członkowskich funkcji Release candidate. Funkcja rozpoznawania przeciążeń wybiera element członkowski funkcji do wywołania w następujących kontekstów distinct w języku C#:

*  Wywołanie metody o nazwie w *invocation_expression* ([wywołań metody opisywanego](expressions.md#method-invocations)).
*  Wywołanie konstruktora wystąpienia o nazwie w *object_creation_expression* ([wyrażenia tworzenia obiektów](expressions.md#object-creation-expressions)).
*  Wywołanie metody dostępu indeksatora za pośrednictwem *element_access* ([dostępu do elementu](expressions.md#element-access)).
*  Wywołanie operatora wstępnie zdefiniowanych lub zdefiniowanych przez użytkownika, do którego odwołuje się wyrażenie ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution) i [Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)).

Każda z tych kontekstach definiuje zestaw elementów członkowskich z funkcją Release candidate i listy argumentów w własny unikatowy sposób opisanych szczegółowo w sekcji wymienionych powyżej. Na przykład zestaw kandydatami do wywołania metody nie ma metody oznaczone `override` ([wyszukanie członka](expressions.md#member-lookup)), i metody w klasie bazowej nie są obiektami, jeśli dotyczy dowolną metodę w klasie pochodnej ([ Wywołań metod](expressions.md#method-invocations)).

Po zidentyfikowaniu składowe funkcji Release candidate i listą argumentów, wybór najlepszych funkcja składowa jest taka sama we wszystkich przypadkach:

*  Biorąc pod uwagę zestaw elementów członkowskich funkcji odpowiednie Release candidate, najlepszych funkcji elementu członkowskiego, znajduje się zestaw. Jeśli zestaw zawiera tylko jeden element członkowski funkcji, ten element członkowski funkcji jest najlepsze element członkowski funkcji. W przeciwnym razie najlepszą funkcja składowa jest składowej jedną funkcję, która jest lepsze niż inne składowe funkcji w odniesieniu do listy podany argument, pod warunkiem, że każda funkcja składowa jest porównywana do wszystkich innych funkcji elementów członkowskich przy użyciu reguł w [ Element członkowski funkcji lepsze](expressions.md#better-function-member). Jeśli nie istnieje dokładnie jednego członka funkcji, który jest lepsze niż wszystkie inne elementy członkowskie funkcji, następnie wywołanie funkcji elementu członkowskiego jest niejednoznaczny i występuje błąd wiązania.

Następujące sekcje definiują dokładny znaczenia warunków ***dotyczy funkcja składowa*** i ***lepsza funkcja składowa***.

#### <a name="applicable-function-member"></a>Dotyczy funkcji składowej

Funkcja składowa jest nazywany ***dotyczy funkcja składowa*** w odniesieniu do listy argumentów `A` gdy są spełnione wszystkie z następujących czynności:

*  Każdy argument `A` odnosi się do parametru w deklaracji funkcji elementu członkowskiego, zgodnie z opisem w [parametry odpowiadające](expressions.md#corresponding-parameters), i każdego parametru, do którego odnosi się żaden argument nie jest parametrem opcjonalnym.
*  Dla każdego argumentu w `A`, przekazywanie tryb argumentu parametru (czyli wartość `ref`, lub `out`) jest taka sama jak trybu przekazywania parametru odpowiadającego mu parametru i
   *  dla parametru wartości lub tablicą parametrów niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z argumentem typu odpowiadającego mu parametru lub
   *  Aby uzyskać `ref` lub `out` parametr, typ argumentu jest taka sama jak typu odpowiadającego mu parametru. Gdy wszystkie `ref` lub `out` parametru jest aliasem dla przekazanego argumentu.

Funkcja elementu członkowskiego, obejmującą tablicy parametrów Jeśli funkcja składowa ma zastosowanie powyższych reguł, jest ono obowiązywać w jego ***normalny formularz***. Jeśli funkcja składowa, która obejmuje tablicy parametrów nie ma zastosowania w postaci normalne, funkcja składowa zamiast tego można stosować w jego ***rozwinięte formularza***:

*  Rozwinięty formularza jest tworzony przez zastąpienie tablicy parametrów w deklaracji funkcji elementu członkowskiego o wartości zero lub większą liczbę parametrów wartość parametru typu elementu tablicy, takie tego liczba argumentów w liście argumentów `A` pasuje do sumy Liczba parametrów. Jeśli `A` ma mniej argumentów niż liczba parametrów stałych w deklaracji funkcji elementu członkowskiego, rozwinięty formularza funkcja składowa nie można utworzyć i dlatego nie ma zastosowania.
*  W przeciwnym razie jest odpowiednie, jeśli dla każdego argumentu w rozszerzonej postaci `A` trybu przekazywania parametru argumentu jest taka sama jak trybu przekazywania parametru odpowiadającego mu parametru i
   *  dla parametru wartości stałej lub utworzone przez rozszerzenie niejawna konwersja parametru wartości ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z typem argumentu typu odpowiadającego mu parametru lub
   *  Aby uzyskać `ref` lub `out` parametr, typ argumentu jest taka sama jak typu odpowiadającego mu parametru.

#### <a name="better-function-member"></a>Lepsza funkcja składowa

Na potrzeby określania, Lepsza funkcja składowa listy argumentów stripped-down A jest tworzona, zawierający tylko argument wyrażenia samodzielnie w kolejności, w jakiej znajdują się na oryginalnej liście argumentów.

Listy parametrów dla każdej z funkcji Release candidate, której członkami są skonstruowane w następujący sposób:

*  Rozwinięty formularza jest używana, jeśli element członkowski funkcji było stosowane tylko w formularzu rozwinięty.
*  Parametry opcjonalne bez odpowiedniego argumentów są usuwane z listy parametrów
*  Parametry zostaną ponownie uporządkowane, tak że występuje ono w tym samym położeniu jako odpowiadający argument na liście argumentów.

Podanej listy argumentów `A` z zestawem wyrażenia argumentu `{E1, E2, ..., En}` i dwie składowe dotyczy funkcji `Mp` i `Mq` z typami parametrów `{P1, P2, ..., Pn}` i `{Q1, Q2, ..., Qn}`, `Mp` jest definiowany jako ***lepsza funkcja składowa*** niż `Mq` Jeśli

*  dla każdego argumentu niejawna konwersja z `Ex` do `Qx` nie jest lepsze niż niejawna konwersja z `Ex` do `Px`, i
*  co najmniej jednego argumentu, konwersja z `Ex` do `Px` jest lepsze niż konwersja z `Ex` do `Qx`.

Podczas przeprowadzania oceny, w tym przypadku `Mp` lub `Mq` jest następnie stosowane w postaci rozwiniętej `Px` lub `Qx` odwołuje się do parametru w postaci rozwiniętej listy parametrów.

W przypadku, gdy parametr typu sekwencje `{P1, P2, ..., Pn}` i `{Q1, Q2, ..., Qn}` są równoważne (czyli każdy `Pi` ma konwersję tożsamości do odpowiednich `Qi`), stosowane są następujące reguły podziału tie w kolejności, w celu ustalenia, tym lepiej element członkowski funkcji.

*  Jeśli `Mp` jest metodą nieogólnego i `Mq` jest metody rodzajowej, `Mp` jest lepsze niż `Mq`.
*  W przeciwnym razie, jeśli `Mp` ma zastosowanie w postaci normalne i `Mq` ma `params` macierz, a następnie występuje tylko w postaci rozwiniętej następnie `Mp` jest lepsze niż `Mq`.
*  W przeciwnym razie, jeśli `Mp` zadeklarował więcej parametrów niż `Mq`, następnie `Mp` jest lepsze niż `Mq`. Taka sytuacja może wystąpić, jeśli obie metody mają `params` tablic oraz czy ma to zastosowanie tylko w rozwiniętym formularzy.
*  W przeciwnym razie jeśli wszystkie parametry `Mp` mają odpowiedni argument argumenty domyślne powinny być zastępowane dla co najmniej jeden parametr opcjonalny w `Mq` następnie `Mp` jest lepsze niż `Mq`.
*  W przeciwnym razie, jeśli `Mp` zawiera bardziej szczegółowe typy parametrów niż `Mq`, następnie `Mp` jest lepsze niż `Mq`. Pozwól `{R1, R2, ..., Rn}` i `{S1, S2, ..., Sn}` reprezentują typy parametrów bez wystąpień i nierozwiniętymi `Mp` i `Mq`. `Mp`firmy typy parametrów są bardziej szczegółowe niż `Mq`firmy if, dla każdego parametru `Rx` nie jest mniej określonych niż `Sx`, a w przypadku co najmniej jeden parametr `Rx` jest bardziej szczegółowe niż `Sx`:
   *  Parametr typu jest mniej określonych niż parametru bez typu.
   *  Cyklicznie, skonstruowanego typu jest bardziej szczegółowe niż innym typem skonstruowany (z tą samą liczbą argumentów typu), jeśli co najmniej jeden argument typu jest bardziej szczegółowe, a nie argument typu jest mniej określonych niż argument Typ w innym.
   *  Typ tablicy jest bardziej szczegółowe niż innego typu tablicy (z taką samą liczbę wymiarów), jeśli typ elementu w pierwszym jest bardziej szczegółowe niż typ elementu w drugim.
*  W przeciwnym razie jeśli jeden element członkowski jest operator niepodniesione, a drugi to podniesionym operatora, lepiej jest jeden niepodniesione.
*  W przeciwnym wypadku żadna funkcja składowa jest lepiej.

#### <a name="better-conversion-from-expression"></a>Lepsze Konwersja wyrażenia

Biorąc pod uwagę niejawną konwersję `C1` konwertuje wyrażenie `E` do typu `T1`i niejawną konwersję `C2` konwertuje wyrażenie `E` do typu `T2`, `C1` jest ***lepsze konwersji*** niż `C2` Jeśli `E` nie jest dokładnie dopasowana `T2` i zawiera co najmniej jeden z następujących czynności:

* `E` dokładnie odpowiada `T1` ([dokładnie wyrażeniu dopasowania](expressions.md#exactly-matching-expression))
* `T1` jest ona lokalizacją docelową konwersji lepsze niż `T2` ([lepsze docelowy konwersji](expressions.md#better-conversion-target))

#### <a name="exactly-matching-expression"></a>Dokładnie pasujące wyrażenie

Podane wyrażenie `E` i typ `T`, `E` dokładnie pasuje `T` jeśli posiada jedną z następujących czynności:

*  `E` zawiera typ `S`, i istnieje konwersja tożsamości z `S` do `T`
*  `E` jest funkcją anonimową `T` jest typem delegowanym `D` lub typ drzewa wyrażeń `Expression<D>` i zawiera jedną z następujących czynności:
   *  Wnioskowany typ zwracany `X` istnieje `E` w kontekście listy parametrów `D` ([Inferred zwracany typ](expressions.md#inferred-return-type)), i istnieje konwersja tożsamości z `X` do zwracanego typu `D`
   *  Albo `E` jest inne niż async i `D` ma typ zwracany `Y` lub `E` jest asynchroniczne i `D` ma typ zwracany `Task<Y>`, i zawiera jedną z następujących czynności:
      * Treść `E` jest wyrażeniem, który dokładnie pasuje `Y`
      * Treść `E` jest blok instrukcji, w którym każdej instrukcji return zwraca wyrażenie który dokładnie pasuje `Y`

#### <a name="better-conversion-target"></a>Lepsze docelowy konwersji

Biorąc pod uwagę dwa różne typy `T1` i `T2`, `T1` jest ona lokalizacją docelową konwersji lepsze niż `T2` Jeśli niejawna konwersja z `T2` do `T1` istnieje, i zawiera co najmniej jeden z następujących czynności:

*  Niejawna konwersja z `T1` do `T2` istnieje
*  `T1` jest to typ delegowany `D1` lub typ drzewa wyrażeń `Expression<D1>`, `T2` jest typem delegowanym `D2` lub typ drzewa wyrażeń `Expression<D2>`, `D1` ma typ zwracany `S1` i jedna z przechowuje następujące:
   * `D2` Zwraca wartość void
   * `D2` ma typ zwracany `S2`, i `S1` jest ona lokalizacją docelową konwersji lepsze niż `S2`
*  `T1` jest `Task<S1>`, `T2` jest `Task<S2>`, i `S1` jest ona lokalizacją docelową konwersji lepsze niż `S2`
*  `T1` jest `S1` lub `S1?` gdzie `S1` jest typ całkowity ze znakiem i `T2` jest `S2` lub `S2?` gdzie `S2` jest nieoznaczoną liczbę całkowitą. W szczególności:
   * `S1` jest `sbyte` i `S2` jest `byte`, `ushort`, `uint`, lub `ulong`
   * `S1` jest `short` i `S2` jest `ushort`, `uint`, lub `ulong`
   * `S1` jest `int` i `S2` jest `uint`, lub `ulong`
   * `S1` jest `long` i `S2` jest `ulong`

#### <a name="overloading-in-generic-classes"></a>Przeciążenie klas ogólnych

Chociaż podpisów, ponieważ nie zadeklarowano muszą być unikatowe, jest to możliwe, że identycznych podpisach powoduje zastąpienie argumentów typu. W takich przypadkach reguły podziału tie przeciążonym powyżej wybierze najbardziej określonym elemencie członkowskim.

W poniższych przykładach pokazano przeciążenia, które są prawidłowe i nieprawidłowa przy uwzględnieniu tej reguły:

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>Sprawdzanie Rozpoznanie przeciążenia dynamicznej kompilacji

Najbardziej dynamicznie powiązanych operacji ewentualnymi kandydatami do rozpoznawania zestawu jest nieznany w czasie kompilacji. W niektórych przypadkach jednak zestaw Release candidate jest znany w czasie kompilacji:

*  Wywołania metody statycznej z argumentów dynamicznych
*  Wystąpienie wywołania metody, której odbiornik nie jest wyrażeniem dynamiczne
*  Wywołania indeksatora, której odbiornik nie jest wyrażeniem dynamiczne
*  Konstruktor wywołuje z argumentów dynamicznych

W takich przypadkach ograniczone sprawdzanie w czasie kompilacji jest wykonywane dla każdego Release candidate zobaczyć, jeśli któryś z nich prawdopodobnie można zastosować w czasie wykonywania. To sprawdzenie składa się z następujących czynności:

*  Wnioskowanie o typie częściowym: Dowolny typ argumentu, który nie zależy od bezpośrednio lub pośrednio argumentu typu `dynamic` wynika, używając reguł [wnioskowanie o typie](expressions.md#type-inference). Pozostałe argumenty typu są nieznane.
*  Sprawdzanie możliwości zastosowania częściowej: Zastosowanie zaznaczono zgodnie z opisem w [dotyczy funkcja składowa](expressions.md#applicable-function-member), ale ignoruje parametry, których typ jest nieznany.
*  Jeśli żaden kandydat zakończy się pomyślnie tego testu, występuje błąd kompilacji.

### <a name="function-member-invocation"></a>Wywołania elementu — funkcja

W tej sekcji opisano proces, który ma miejsce w czasie wykonywania można zainicjować członka określonej funkcji. Zakłada się, proces wiązania czas stwierdził już określony element członkowski do wywołania, prawdopodobnie stosując przeciążenia, aby zestaw elementów członkowskich funkcji kandydata.

Do celów opisu proces wywołania funkcji elementów członkowskich są podzielone na dwie kategorie:

*  Funkcja statyczne elementy członkowskie. Są to konstruktory wystąpień, metod statycznych, metod dostępu do właściwości statycznych i operatory zdefiniowane przez użytkownika. Funkcja statyczne elementy członkowskie są zawsze niewirtualną.
*  Elementy członkowskie wystąpień funkcji. Są metody wystąpienia, wystąpienia metod dostępu do właściwości i metod dostępu do indeksatora. Elementy członkowskie wystąpień funkcji to-virtual lub wirtualnych i zawsze są wywoływane na konkretnym wystąpieniu. Wystąpienie jest obliczana przez wyrażenie wystąpienia i staje się dostępny w ramach funkcji elementu członkowskiego jako `this` ([dostęp](expressions.md#this-access)).

Przetwarzanie czasu wykonywania wywołania funkcji elementu członkowskiego składa się z następujących czynności, gdzie `M` jest funkcja składowa i, jeśli `M` jest składową wystąpienia `E` jest wyrażeniem wystąpienie:

*  Jeśli `M` jest składową statyczną funkcję:
   * Lista argumentów jest obliczane zgodnie z opisem w [listy argumentów](expressions.md#argument-lists).
   * `M` jest wywoływane.

*  Jeśli `M` jest to funkcja składowa zadeklarowana w *value_type*:
   * `E` jest oceniany. Jeśli ta ocena powoduje wyjątek, nie trzeba wykonywać dalszych czynności są wykonywane.
   * Jeśli `E` nie jest sklasyfikowany jako zmienną, a następnie tymczasowej zmiennej lokalnej `E`firmy tworzony jest typ i wartość `E` jest przypisany do tej zmiennej. `E` następnie jest sklasyfikowany jako odwołanie do tego tymczasowej zmiennej lokalnej. Zmienna tymczasowa jest dostępny jako `this` w ramach `M`, ale nie w jakikolwiek inny sposób. W związku z tym, tylko wtedy, gdy `E` jest wartość true, zmienna jest możliwe do obiektu wywołującego obserwować zmiany, `M` sprawia, że aby `this`.
   * Lista argumentów jest obliczane zgodnie z opisem w [listy argumentów](expressions.md#argument-lists).
   * `M` jest wywoływane. Zmienna odwołuje się `E` staje się zmienna odwołuje się `this`.

*  Jeśli `M` jest to funkcja składowa zadeklarowana w *reference_type*:
   * `E` jest oceniany. Jeśli ta ocena powoduje wyjątek, nie trzeba wykonywać dalszych czynności są wykonywane.
   * Lista argumentów jest obliczane zgodnie z opisem w [listy argumentów](expressions.md#argument-lists).
   * Jeśli typ `E` jest *value_type*, konwersja boxing ([konwersje Boxing](types.md#boxing-conversions)) jest wykonywane w celu przekonwertowania `E` na typ `object`, i `E` jest uznawany za Typ `object` w poniższych krokach. W tym przypadku `M` może być tylko składową z `System.Object`.
   * Wartość `E` jest sprawdzany w celu był prawidłowy. Jeśli wartość `E` jest `null`, `System.NullReferenceException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.
   * Implementacja elementu członkowskiego funkcji do wywołania, jest określana:
     * Jeśli typ powiązania — czas `E` jest interfejsem, element członkowski funkcji do wywołania to implementacja `M` dostarczone przez typu run-time wystąpienia odwołuje się `E`. Ta funkcja składowa jest określana przez stosowanie reguł mapowania interfejsu ([mapowania interfejsu](interfaces.md#interface-mapping)) do określenia implementacji `M` dostarczone przez typu run-time wystąpienia odwołuje się `E`.
     * W przeciwnym razie, jeśli `M` jest elementem członkowskim funkcji wirtualnej element członkowski funkcji do wywołania to implementacja `M` dostarczone przez typu run-time wystąpienia odwołuje się `E`. Ten element członkowski funkcji zależy od stosowania reguł do określania wykonania najbardziej pochodnej ([metody wirtualnej](classes.md#virtual-methods)) z `M` względem typu run-time wystąpienia odwołuje się `E`.
     * W przeciwnym razie `M` jest elementem członkowskim niewirtualną i jest element członkowski funkcji do wywołania `M` sam.
   * Implementacja elementu członkowskiego funkcji, które są określone w poprzednim kroku jest wywoływana. Zawiera odwołanie do obiektu `E` staje się zawiera odwołanie do obiektu `this`.

#### <a name="invocations-on-boxed-instances"></a>Wywołania na wystąpieniach ramce

Funkcja składowa zaimplementowane w *value_type* może być wywoływany za pośrednictwem spakowany wystąpienie tego *value_type* w następujących sytuacjach:

*  Gdy funkcja składowa jest `override` metody dziedziczone z typu `object` i jest wywoływana za pomocą wyrażenia typu `object`.
*  Gdy funkcja składowa jest implementacją funkcja składowa interfejsu i jest wywoływana za pomocą wyrażenia wystąpienia *interface_type*.
*  Gdy funkcja składowa zostanie wywołana przez delegata.

W takich przypadkach wystąpienie programu prostokątnych jest uważany za zawiera zmienną *value_type*, a ta zmienna staje się zmienna odwołuje się `this` w ramach wywołanie funkcji elementu członkowskiego. W szczególności oznacza to, że gdy funkcja składowa zostanie wywołana w wystąpieniu spakowany, możliwe jest funkcja elementu członkowskiego do modyfikowania wartości zawarte w wystąpieniu programu prostokątnych.

## <a name="primary-expressions"></a>Wyrażenia podstawowe

Wyrażenia podstawowe obejmują najprostszej formy wyrażeń.

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

Wyrażenia podstawowe są dzielone między *array_creation_expression*s i *primary_no_array_creation_expression*s. Traktowanie wyrażenie tworzenia tablicy w ten sposób, zamiast dołączając ją wraz z innych form proste wyrażenie umożliwia gramatykę na potrzeby nie zezwalaj na kod może być mylące, takich jak
```csharp
object o = new int[3][1];
```
które w przeciwnym razie może być interpretowany jako
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>Literały

A *primary_expression* składający się z *literału* ([literały](lexical-structure.md#literals)) jest klasyfikowana jako wartość.


### <a name="interpolated-strings"></a>Ciągi interpolowane

*Interpolated_string_expression* składa się z `$` logowanie następuje regularnie lub verbatim literału ciągu, na którym dziury rozdzielone `{` i `}`, należy ująć wyrażenia i formatowanie wymagania. Wyrażenie ciągu interpolowanego jest wynikiem *interpolated_string_literal* , ma zostały podzielone na oddzielne tokeny, zgodnie z opisem w [interpolowane literałów ciągów](lexical-structure.md#interpolated-string-literals).

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

*Constant_expression* interpolacji musi mieć niejawną konwersję do `int`.

*Interpolated_string_expression* zostanie sklasyfikowany jako wartość. Jeśli od razu jest konwertowany na `System.IFormattable` lub `System.FormattableString` za pomocą konwersji niejawnych ciąg interpolowany ([niejawne interpolowane ciąg konwersje](conversions.md#implicit-interpolated-string-conversions)), wyrażenie ciągu interpolowanego ma tego typu. W przeciwnym razie ma typ `string`.

Jeśli typ ciągu interpolowanego `System.IFormattable` lub `System.FormattableString`, znaczenie jest wywołaniem `System.Runtime.CompilerServices.FormattableStringFactory.Create`. Jeśli typ jest `string`, znaczenie wyrażenia jest wywołaniem `string.Format`. W obu przypadkach liście argumentów wywołania składa się z literału przy użyciu symboli zastępczych dla każdego interpolacji i argument każde wyrażenie odpowiadające posiadaczy miejsce ciągu formatu.

Literał ciągu formatu jest tworzony następująco, gdzie `N` jest liczba interpolations w *interpolated_string_expression*:

*  Jeśli *interpolated_regular_string_whole* lub *interpolated_verbatim_string_whole* następuje `$` Zaloguj się, a następnie literał ciągu formatu jest tego tokenu.
*  W przeciwnym razie literału ciągu formatu składa się z: 
   *  Pierwszy *interpolated_regular_string_start* lub *interpolated_verbatim_string_start*
   *  Następnie dla każdego numeru `I` z `0` do `N-1`: 
      * W formie dziesiętnej `I`
      * Następnie, jeśli odpowiedni *interpolacji* ma *constant_expression*, `,` (przecinek), a następnie dziesiętna reprezentacja wartości *constant_expression*
      * A następnie *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* lub *interpolated_ verbatim_string_end* natychmiast po odpowiednich interpolacji.

Pozostałe argumenty są po prostu *wyrażeń* z *interpolations* (jeśli istnieje), w kolejności.

TODO: przykłady.


### <a name="simple-names"></a>Proste nazwy

A *simple_name* składa się z identyfikatorem, opcjonalnie następuje lista argumentów typu:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

A *simple_name* jest formularza `I` lub w postaci `I<A1,...,Ak>`, gdzie `I` jest pojedynczy identyfikator i `<A1,...,Ak>` to opcjonalna *type_argument_list*. Gdy nie *type_argument_list* jest określony, należy wziąć pod uwagę `K` zero. *Simple_name* jest obliczane i sklasyfikowany w następujący sposób:

*  Jeśli `K` jest równa zero i *simple_name* pojawia się w obrębie *bloku* i, jeśli *bloku*firmy (lub otaczającego *bloku*firmy) lokalne Deklaracja zmiennej miejsca ([deklaracje](basic-concepts.md#declarations)) zawiera zmienną lokalną, parametr lub stałą o nazwie `I`, a następnie *simple_name* odwołuje się do tej zmiennej lokalnej parametr lub stała i jest klasyfikowana jako wartości lub zmienne.
*  Jeśli `K` jest równa zero i *simple_name* pojawia się w treści deklaracji metody rodzajowe i jeśli deklaracja zawiera parametr typu o nazwie `I`, a następnie *simple_name*odwołuje się do tego parametru typu.
*  W przeciwnym razie dla każdego typu wystąpienia `T` ([typu wystąpienia](classes.md#the-instance-type)), począwszy od typu wystąpienia natychmiastowo otaczającą deklaracji typu i kontynuowanie przy użyciu typu wystąpienia każdej otaczającej klasie lub strukturze Deklaracja (jeśli istnieje):
   *  Jeśli `K` to zero, a deklaracja `T` obejmuje parametru typu o nazwie `I`, a następnie *simple_name* odwołuje się do tego parametru typu.
   *  W przeciwnym razie, jeśli wyszukiwanie elementu członkowskiego ([wyszukanie członka](expressions.md#member-lookup)) z `I` w `T` z `K`  argumentami typu tworzy dopasowania:
      * Jeśli `T` jest typem wystąpienia natychmiastowo otaczającą typu klasy lub struktury i wyszukiwanie identyfikuje jeden lub więcej metod, wynik jest grupą metoda z wyrażeniem skojarzonego wystąpienia `this`. Jeśli lista argumentów typu został określony, jest używany podczas wywoływania metody ogólnej ([wywołań metody opisywanego](expressions.md#method-invocations)).
      * W przeciwnym razie, jeśli `T` jest typu wystąpienia natychmiastowo otaczającą typu klasy lub struktury, wyszukiwanie identyfikuje elementu członkowskiego wystąpienia, a odwołanie występuje w treści konstruktora wystąpienia, metoda wystąpienia lub metodę dostępu wystąpienia wynik jest taki sam dostęp do elementu członkowskiego ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `this.I`. To tylko możliwe, gdy `K` wynosi zero.
      * W przeciwnym razie wynikiem jest taka sama jak dostęp do elementu członkowskiego ([dostęp do elementu członkowskiego](expressions.md#member-access)) w postaci `T.I` lub `T.I<A1,...,Ak>`. W tym przypadku jest to błąd czasu powiązania dla *simple_name* do odwoływania się do elementu członkowskiego wystąpienia.

*  W przeciwnym razie dla każdej przestrzeni nazw `N`, począwszy od przestrzeni nazw, w którym *simple_name* problem wystąpi, kontynuując każdego otaczającej przestrzeni nazw (jeśli istnieje), a skończywszy globalnej przestrzeni nazw, poniższe kroki to etapy sprawdzane, dopóki nie znajduje się jednostka:
   *  Jeśli `K` jest równa zero i `I` jest nazwą przestrzeni nazw w `N`, następnie:
      * Jeśli lokalizacja gdzie *simple_name* występuje jest ujęta w deklarację przestrzeni nazw dla `N` i zawiera deklarację przestrzeni nazw *extern_alias_directive* lub  *using_alias_directive* który kojarzy nazwę `I` z przestrzeni nazw lub typ, a następnie *simple_name* jest niejednoznaczny i występuje błąd kompilacji.
      * W przeciwnym razie *simple_name* odwołuje się do przestrzeni nazw o nazwie `I` w `N`.
   *  W przeciwnym razie, jeśli `N` zawiera dostępny typ o nazwie `I` i `K`  parametry typu, następnie:
      * Jeśli `K` to zero i lokalizacja gdzie *simple_name* występuje jest ujęta w deklarację przestrzeni nazw dla `N` i zawiera deklarację przestrzeni nazw *extern_alias_directive*lub *using_alias_directive* który kojarzy nazwę `I` z przestrzeni nazw lub typ, a następnie *simple_name* jest niejednoznaczny i występuje błąd kompilacji.
      * W przeciwnym razie *namespace_or_type_name* odwołuje się do typu skonstruowany przy użyciu argumentów danego typu.
   *  W przeciwnym razie, jeśli lokalizacja gdzie *simple_name* występuje jest ujęta w deklarację przestrzeni nazw dla `N`:
      * Jeśli `K` jest równa zero i zawiera deklarację przestrzeni nazw *extern_alias_directive* lub *using_alias_directive* który kojarzy nazwę `I` z zaimportowaną przestrzenią nazw lub Typ, a następnie *simple_name* odwołuje się do tej przestrzeni nazw lub typu.
      * W przeciwnym razie, jeśli deklaracji przestrzeni nazw i typ zaimportowany przez *using_namespace_directive*s i *using_static_directive*s deklarację przestrzeni nazw zawierać dokładnie jeden typ dostępny lub -extension członka statycznego o nazwie `I` i `K`  parametry typu, a następnie *simple_name* odwołuje się do tego typu lub elementu członkowskiego, skonstruowany przy użyciu argumentów danego typu.
      * W przeciwnym razie, jeśli obszary nazw i typy zaimportowany przez *using_namespace_directive*s deklarację przestrzeni nazw zawiera więcej niż jeden dostępny typ lub metoda bez rozszerzenia członka statycznego o nazwie `I` i `K`  parametry typu, a następnie *simple_name* jest niejednoznaczny i występuje błąd.

   Należy pamiętać, że ten krok jest dokładnie zbliżony do odpowiedniego krok przetwarzania w *namespace_or_type_name* ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)).

*  W przeciwnym razie *simple_name* jest niezdefiniowana, i występuje błąd kompilacji.


### <a name="parenthesized-expressions"></a>Wyrażenia ujętego w nawiasy

A *parenthesized_expression* składa się z *wyrażenie* w nawiasach.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

A *parenthesized_expression* jest oceniany przez dokonanie oceny oprogramowania *wyrażenie* w nawiasach. Jeśli *wyrażenie* między nawiasami wskazuje przestrzeń nazw lub typ, wystąpi błąd kompilacji. W przeciwnym razie wynik *parenthesized_expression* wynika z oceny zamkniętego *wyrażenie*.

### <a name="member-access"></a>Dostęp do elementu członkowskiego

A *member_access* składa się z *primary_expression*, *predefined_type*, lub *qualified_alias_member*, a następnie "`.`"token, a następnie *identyfikator*, opcjonalnie następuje *type_argument_list*.

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

*Qualified_alias_member* produkcji jest zdefiniowany w [kwalifikatory alias Namespace](namespaces.md#namespace-alias-qualifiers).

A *member_access* jest formularza `E.I` lub w postaci `E.I<A1, ..., Ak>`, gdzie `E` jest wyrażeniem podstawowym, `I` jest pojedynczy identyfikator i `<A1, ..., Ak>` to opcjonalna  *type_argument_list*. Gdy nie *type_argument_list* jest określony, należy wziąć pod uwagę `K` zero.

A *member_access* z *primary_expression* typu `dynamic` dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)). W tym przypadku kompilator klasyfikuje dostęp do elementu członkowskiego jako dostęp do właściwości typu `dynamic`. Reguły poniżej, aby określić znaczenia *member_access* zostaną następnie zastosowane w czasie wykonywania, zamiast typ kompilacji przy użyciu typu run-time *primary_expression*. Jeśli ta klasyfikacja środowiska wykonawczego prowadzi do grupy metod, a następnie dostęp do elementu członkowskiego musi być *primary_expression* z *invocation_expression*.

*Member_access* jest obliczane i sklasyfikowany w następujący sposób:

*  Jeśli `K` jest równa zero i `E` jest przestrzenią nazw i `E` zawiera zagnieżdżone przestrzenie nazw o nazwie `I`, wynik jest tego obszaru nazw.
*  W przeciwnym razie, jeśli `E` jest przestrzenią nazw i `E` zawiera dostępny typ o nazwie `I` i `K`  parametry typu, wynik jest zbudowany z argumentami typu danego typu.
*  Jeśli `E` jest *predefined_type* lub *primary_expression* sklasyfikowane jako typ, jeśli `E` nie jest parametrem typu i jeśli wyszukiwanie elementu członkowskiego ([wyszukanie członka](expressions.md#member-lookup)) z `I` w `E` z `K`  parametrów typu generuje dopasowania, następnie `E.I` jest obliczane i sklasyfikowany w następujący sposób:
   *  Jeśli `I` identyfikuje typ, wynik jest zbudowany z argumentami typu danego typu.
   *  Jeśli `I` identyfikuje co najmniej jedną metodę, wynik jest grupy metod z Brak wyrażenia skojarzonego wystąpienia. Jeśli lista argumentów typu został określony, jest używany podczas wywoływania metody ogólnej ([wywołań metody opisywanego](expressions.md#method-invocations)).
   *  Jeśli `I` identyfikuje `static` właściwości, a następnie wynik jest dostęp do właściwości, za pomocą wyrażenia nie skojarzonego wystąpienia.
   *  Jeśli `I` identyfikuje `static` pola:
      * Jeśli to pole jest `readonly` odwołania jest wykonywane poza statycznego konstruktora klasy lub struktury, w którym zadeklarowany jest pole, a następnie wynik jest wartością, czyli wartość pola statycznego `I` w `E`.
      * W przeciwnym razie wynik jest zmienną, a mianowicie pole statyczne `I` w `E`.
   *  Jeśli `I` identyfikuje `static` zdarzeń:
      * Jeśli występuje odwołanie, w obrębie klasy lub struktury, w którym zdarzenie jest zadeklarowana, a zdarzenie była zadeklarowana bez *event_accessor_declarations* ([zdarzenia](classes.md#events)), następnie `E.I` dokładnie przetwarzania tak, jakby `I` zostały pole statyczne.
      * W przeciwnym razie wynikiem jest dostęp do zdarzenia przy użyciu Brak wyrażenia skojarzonego wystąpienia.
   *  Jeśli `I` identyfikuje stałą, a następnie wynik jest wartością, czyli wartość tej stałej.
    * Jeśli `I` identyfikuje element członkowski wyliczenia, a następnie wynik jest wartością, czyli wartość tego elementu członkowskiego wyliczenia.
    * W przeciwnym razie `E.I` jest odwołaniem nieprawidłowej składowej i występuje błąd kompilacji.
*  Jeśli `E` jest dostęp do właściwości, indeksatora dostępu, zmienna lub wartości, którego typ jest `T`i wyszukać składowej ([wyszukanie członka](expressions.md#member-lookup)) z `I` w `T` z `K`  argumentami typu tworzy następnie dopasowania `E.I` jest obliczane i sklasyfikowany w następujący sposób:
   *  Po pierwsze, jeśli `E` jest właściwość lub indeksator dostępu, a następnie wartość właściwości lub indeksatora dostęp jest uzyskiwany ([wartości wyrażeń](expressions.md#values-of-expressions)) i `E` jest sklasyfikowany jako wartość.
   *  Jeśli `I` identyfikuje co najmniej jednej metody, a następnie wynik jest grupą metoda z wyrażeniem skojarzonego wystąpienia `E`. Jeśli lista argumentów typu został określony, jest używany podczas wywoływania metody ogólnej ([wywołań metody opisywanego](expressions.md#method-invocations)).
   *  Jeśli `I` identyfikuje właściwości wystąpienia
      * Jeśli `E` jest `this`, `I` identyfikuje automatycznie implementowanej właściwości ([automatycznie implementowane właściwości](classes.md#automatically-implemented-properties)) bez metody ustawiającej i odwołanie ma miejsce w konstruktorze wystąpienia dla Typ klasy lub struktury `T`, a następnie wynik jest zmienną, a mianowicie pole zapasowe ukryte dotyczący właściwości automatycznej podane przez `I` w wystąpieniu programu `T` przez `this`.
      * W przeciwnym razie wynikiem jest dostęp do właściwości, za pomocą wyrażenia skojarzonego wystąpienia `E`.
   *  Jeśli `T` jest *class_type* i `I` identyfikuje pole wystąpienia, *class_type*:
      * Jeśli wartość `E` jest `null`, a następnie `System.NullReferenceException` zgłaszany.
      * W przeciwnym razie, jeśli pole jest `readonly` odwołania jest wykonywane poza konstruktora wystąpienia klasy, w którym zadeklarowany jest pole, a następnie wynik jest wartością, czyli wartość pola `I` w obiekcie wywoływanym przez `E`.
      * W przeciwnym razie wynik jest zmienną, czyli pola `I` w obiekcie wywoływanym przez `E`.
   *  Jeśli `T` jest *struct_type* i `I` identyfikuje pole wystąpienia, *struct_type*:
      * Jeśli `E` jest wartością, czy pole jest `readonly` odwołania jest wykonywane poza konstruktora wystąpienia struktury, w którym zadeklarowany jest pole, a następnie wynik jest wartością, czyli wartość pola `I` w wystąpieniu struktury przez  `E`.
      * W przeciwnym razie wynik jest zmienną, czyli pola `I` w wystąpieniu struktury przez `E`.
   *  Jeśli `I` identyfikuje wystąpienie zdarzenia:
      * Jeśli występuje odwołanie, w obrębie klasy lub struktury, w którym zdarzenie jest zadeklarowana, a zdarzenie była zadeklarowana bez *event_accessor_declarations* ([zdarzenia](classes.md#events)), a odwołanie nie jest wykonywane jako po lewej stronie `+=` lub `-=` operatora, a następnie `E.I` jest przetwarzany dokładnie tak, jakby `I` został pola wystąpienia.
      * W przeciwnym razie wynikiem jest dostęp do zdarzenia przy użyciu wyrażenia skojarzonego wystąpienia `E`.
*  W przeciwnym razie zostanie podjęta próba przetworzenia `E.I` jako wywołanie metody rozszerzenia ([wywołań metod rozszerzenia](expressions.md#extension-method-invocations)). W przypadku niepowodzenia `E.I` jest odwołaniem nieprawidłowej składowej i występuje błąd wiązania.

#### <a name="identical-simple-names-and-type-names"></a>Identyczne nazwy proste i nazwy typów

Dostępu do elementu członkowskiego w postaci `E.I`, jeśli `E` jest pojedynczy identyfikator i, jeśli znaczenie `E` jako *simple_name* ([proste nazwy](expressions.md#simple-names)) jest stała, pole, właściwość lokalnej zmiennej lub parametru za pomocą tego samego typu co znaczenie `E` jako *type_name* ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)), a następnie obie znaczeń z `E` są dozwolone. Znaczenie dwa możliwe `E.I` nigdy nie są niejednoznaczne, ponieważ `I` niekoniecznie musi należeć do typu `E` w obu przypadkach. Innymi słowy, reguła po prostu zezwala na dostęp do statycznych elementów członkowskich i zagnieżdżone typy `E` gdzie błąd kompilacji w przeciwnym razie będą miały miejsce. Na przykład:
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

#### <a name="grammar-ambiguities"></a>Gramatyka niejednoznaczności

Produkcji dla *simple_name* ([proste nazwy](expressions.md#simple-names)) i *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)) może powodować powstanie niejednoznaczności w Gramatyka wyrażeń. Na przykład instrukcja:
```
F(G<A,B>(7));
```
można zinterpretować jako wywołanie `F` za pomocą dwóch argumentów `G < A` i `B > (7)`. Ewentualnie może zostać zinterpretowane jako wywołanie `F` jeden argument, który jest wywołanie metody ogólnej `G` dwa argumenty typu i jeden argument regularne.

Jeśli sekwencja tokeny może być analizowana (w kontekście) jako *simple_name* ([proste nazwy](expressions.md#simple-names)), *member_access* ([dostęp do elementu członkowskiego](expressions.md#member-access)), lub *pointer_member_access* ([dostęp do elementu członkowskiego wskaźnika](unsafe-code.md#pointer-member-access)) kończą się ciągiem *type_argument_list* ([argumentów typu](types.md#type-arguments)), token następujący bezpośrednio po zamykającym `>` token jest sprawdzany pod. Jeśli jest to jeden z
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
a następnie *type_argument_list* są przechowywane jako część *simple_name*, *member_access* lub *pointer_member_access* oraz inne możliwe analizy sekwencja tokenów jest odrzucany. W przeciwnym razie *type_argument_list* nie jest uważany za część *simple_name*, *member_access* lub *pointer_member_access*nawet wtedy, gdy nie ma żadnych możliwych analizy sekwencja tokeny. Należy zauważyć, że te reguły są stosowane podczas analizowania *type_argument_list* w *namespace_or_type_name* ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)). Wykonywanie instrukcji
```csharp
F(G<A,B>(7));
```
zgodnie z regułą, są interpretowane jako zostaną wywołanie `F` jeden argument, który jest wywołanie metody ogólnej `G` dwa argumenty typu i jeden argument regularne. Instrukcje
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
będzie każdego interpretowana jako wywołanie `F` za pomocą dwóch argumentów. Wykonywanie instrukcji
```csharp
x = F < A > +y;
```
będzie interpretowany jako operator less than, większa niż operatora i operator, plus jednoargumentowy tak, jakby zaprojektował instrukcji `x = (F < A) > (+y)`, a nie jako *simple_name* z *type_argument_list* następuje operator plus pliku binarnego. W instrukcji
```csharp
x = y is C<T> + z;
```
tokeny `C<T>` są interpretowane jako *namespace_or_type_name* z *type_argument_list*.

### <a name="invocation-expressions"></a>Wyrażenia wywołania

*Invocation_expression* służy do wywoływania metody.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

*Invocation_expression* dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)) jeśli zawiera co najmniej jeden z następujących czynności:

* *Primary_expression* ma typ kompilacji `dynamic`.
* Co najmniej jeden argument opcjonalne *argument_list* ma typ kompilacji `dynamic` i *primary_expression* nie ma typu delegata.

W tym przypadku kompilator klasyfikuje *invocation_expression* jako wartość typu `dynamic`. Reguły poniżej, aby określić znaczenia *invocation_expression* zostaną następnie zastosowane w czasie wykonywania, przy użyciu typu run-time zamiast typ kompilacji udostępnianych przez *primary_expression* i argumenty, które mają typ kompilacji `dynamic`. Jeśli *primary_expression* nie ma typu kompilacji `dynamic`, a następnie wywołania metody ulega sprawdzenie w czasie kompilacji ograniczona zgodnie z opisem w [sprawdzanie Rozpoznanie przeciążenia dynamicznej kompilacji ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

*Primary_expression* z *invocation_expression* musi być grupy metod lub wartość *delegate_type*. Jeśli *primary_expression* jest grupą metoda *invocation_expression* jest wywołanie metody ([wywołań metody opisywanego](expressions.md#method-invocations)). Jeśli *primary_expression* jest wartością *delegate_type*, *invocation_expression* to wywołanie delegata ([delegowanie wywołań](expressions.md#delegate-invocations)). Jeśli *primary_expression* nie jest grupa metod ani wartości *delegate_type*, występuje błąd wiązania.

Opcjonalny *argument_list* ([listy argumentów](expressions.md#argument-lists)) zawiera wartości i odwołań do zmiennych parametrów metody.

Wynik obliczania wartości *invocation_expression* jest klasyfikowany w następujący sposób:

*  Jeśli *invocation_expression* wywołuje metody lub delegata, która zwraca `void`, wynik jest nothing. Wyrażenie zostało zaklasyfikowane jako nic nie jest dozwolona tylko w kontekście *statement_expression* ([instrukcje wyrażeń](statements.md#expression-statements)) lub jako treść metody *lambda_expression*([Wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)). W przeciwnym razie wystąpi błąd wiązania.
*  W przeciwnym razie wynik jest wartością typu zwracane przez metody lub delegata.

#### <a name="method-invocations"></a>Wywołania metod

Na wywołanie metody *primary_expression* z *invocation_expression* musi być grupą metod. Grupy metod identyfikuje jednej metody do wywołania lub zestaw przeciążonych metod do wyboru określonej metody do wywołania. W tym ostatnim przypadku określonej metody do wywołania jest na podstawie podanego przez typy argumentów kontekstu *argument_list*.

Powiązania w czasie przetwarzania wywołania metody w postaci `M(A)`, gdzie `M` jest grupa metod (prawdopodobnie w tym *type_argument_list*), i `A` to opcjonalna *argument_ Lista*, składa się z następujących czynności:

*  Jest tworzony zestaw metod do wywołania metody. Dla każdej metody `F` skojarzone z grupą metod `M`:
   *  Jeśli `F` jest nieogólnego, `F` jest kandydatem po:
      * `M` nie zawiera typu argumentu listy, a
      * `F` ma zastosowanie w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)).
   *  Jeśli `F` ogólnego i `M` ma nie listy argumentów typu `F` jest kandydatem po:
      * Wnioskowanie o typie ([wnioskowanie o typie](expressions.md#type-inference)) zakończy się powodzeniem, wnioskowanie listy argumentów typu połączenia, a
      * Gdy argumentami typu wywnioskowanego są zastępowane dla parametrów typu odpowiedniej metody, wszystkie typy utworzone na liście parametrów F spełnia ich ograniczenia ([spełniających ograniczenia](types.md#satisfying-constraints)) oraz listę parametrów `F` jest stosowana w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)).
   *  Jeśli `F` ogólnego i `M` zawiera listę argumentów typu `F` jest kandydatem po:
      * `F` ma taką samą liczbę parametrów typu w metodzie, jak podano w liście argumentów typu i
      * Gdy argumenty typu są zastępowane dla parametrów typu odpowiedniej metody, wszystkie typy utworzone na liście parametrów F spełnia ich ograniczenia ([spełniających ograniczenia](types.md#satisfying-constraints)) oraz listę parametrów `F` ma zastosowanie w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)).
*  Zestaw metod Release candidate jest ograniczona do zawierają tylko metody z najbardziej pochodnej typów: Dla każdej metody `C.F` w zestawie, gdzie `C` jest typem, w którym metoda `F` zadeklarowano wszystkich metod zadeklarowanych w podstawowym typem `C` są usuwane z zestawu. Ponadto jeśli `C` jest inny niż typ klasy `object`, wszystkie metody, które zostało zadeklarowane w typie interfejsu są usuwane z zestawu. (Ta zasada ostatnie tylko ma wpływ podczas grupy metod, wynikiem wyszukiwania elementu członkowskiego dla parametru typu o skutecznej klasy bazowej, innego niż obiekt i pusty interfejs skuteczne, konfiguruje).
*  Jeśli wynikowy zestaw metod jest pusta, dalsze przetwarzanie wzdłuż poniższe kroki są porzucone, a zamiast tego zostanie podjęta próba można przetworzyć wywołania jako wywołania metody rozszerzenia ([wywołań metod rozszerzenia](expressions.md#extension-method-invocations)). W przypadku niepowodzenia, a nie dotyczy istnieją metody, występuje błąd wiązania.
*  Najlepszą metodą zestaw metod Release candidate jest identyfikowany za pomocą zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution). Jeśli nie można zidentyfikować pojedynczy najlepszą metodę, wywołanie metody jest niejednoznaczny i występuje błąd wiązania. Podczas rozpoznawania przeciążenia parametry metody ogólnej są traktowane jako po argumentów typu (dostarczane lub wywnioskowane) podstawieniu dla odpowiednich parametrów typu metody.
*  Odbywa się ostatecznej weryfikacji wybranej najlepszą metodę:
   * Metoda sprawdzania poprawności w kontekście grupy metod: Jeśli najlepszą metodę statyczną metodę, grupy metod musi mieć związek z *simple_name* lub *member_access* za pośrednictwem typu. Jeśli najlepsza metoda jest metodą wystąpienia, grupy metod musi mieć związek z *simple_name*, *member_access* za pośrednictwem zmiennej lub wartość lub *base_access*. Jeśli żadna z tych wymagań nie jest spełniony, występuje błąd wiązania.
   * Najlepszą metodę w przypadku metody rodzajowej, argumenty typu (dostarczane lub wywnioskowane) są porównywane z ograniczeniami ([spełniających ograniczenia](types.md#satisfying-constraints)) zadeklarowany dla metody rodzajowej. Jeśli którykolwiek z argumentów typu nie spełniają odpowiednie elementem dla parametru typu, występuje błąd wiązania.

Gdy metoda zostanie wybrane i sprawdzone w momencie powiązania przez powyższe kroki, rzeczywiste wywołanie środowiska wykonawczego są przetwarzane zgodnie z regułami wywołania elementu funkcji opisanych w [sprawdzanie Rozpoznanie przeciążenia dynamicznej kompilacji ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Intuicyjne efekt zasad rozpoznawania opisanych powyżej jest następująca: Aby zlokalizować konkretnej metody wywoływane przez wywołanie metody, rozpoczynać się typ wskazany przez wywołanie metody i kontynuować górę łańcucha dziedziczenia, dopóki nie zostanie znaleziony co najmniej jedną metodę ma to zastosowanie, dostępny, -Zastępowanie deklaracji. Następnie wykonaj wnioskowanie o typie przeciążenia w zestawie, który ma to zastosowanie, dostępny, -zastępowanie metod zadeklarowanych w tego typu i wywołaj metodę wybrany w ten sposób. Jeśli nie znaleziono metody, spróbuj zamiast tego można przetworzyć wywołania jako wywołania metody rozszerzenia.

#### <a name="extension-method-invocations"></a>Wywołań metod rozszerzenia

W wywołaniu metody ([wywołań w wystąpieniach spakowany](expressions.md#invocations-on-boxed-instances)) z jednego z formularzy
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
Jeśli normalnego przetwarzania wywołania wykryje żadnych odpowiednich metod, podejmowana jest próba przetworzenia konstrukcji jako wywołanie metody rozszerzenia. Jeśli *expr* ani żadnego z *args* ma typ kompilacji `dynamic`, nie będą miały zastosowania metody rozszerzenia.

Celem jest znalezienie najlepszych *type_name* `C`, dzięki czemu odpowiedniego wywołania metody statyczne mogą być wykonywane:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

Metody rozszerzenia `Ci.Mj` jest ***kwalifikujących się*** jeśli:

*  `Ci` jest klasą nieogólnego, -nested
*  Nazwa `Mj` jest *identyfikator*
*  `Mj` jest dostępna i ma zastosowanie, po zastosowaniu do argumentów jako metoda statyczna, jak pokazano powyżej
*  Istnieje niejawna tożsamości, odwołanie lub konwersja boxing z *expr* na typ pierwszego parametru `Mj`.

Wyszukaj `C` przechodzi w następujący sposób:

*  Począwszy od najbliższej otaczającej deklaracji przestrzeni nazw, kontynuując każdy otaczający deklarację przestrzeni nazw, a kończąc zawierającego jednostki kompilacji, kolejne próby znalezienia kandydata zestaw metod rozszerzenia:
   * Jeśli danej jednostce przestrzeni nazw lub kompilacji bezpośrednio zawiera deklaracje typu nieogólnego `Ci` przy użyciu metody rozszerzenia kwalifikujących się `Mj`, zestaw tych metod rozszerzenia jest zestaw Release candidate.
   * Jeśli typy `Ci` importowane przez *using_static_declarations* i bezpośrednio zadeklarowane w przestrzeniach nazw importowanych przez *using_namespace_directive*s w danej przestrzeni nazw lub kompilacji jednostce bezpośrednio zawiera metody rozszerzenia kwalifikujących się `Mj`, zestaw tych metod rozszerzenia jest zestaw Release candidate.
*  Jeśli nie ustawiono Release candidate znajduje się w każdej przestrzeni nazw otaczającej jednostki kompilacji lub deklaracji, wystąpi błąd kompilacji.
*  W przeciwnym razie funkcja rozpoznawania przeciążeń jest stosowany do Release candidate, ustaw zgodnie z opisem w ([Rozpoznanie przeciążenia](expressions.md#overload-resolution)). Jeśli nie pojedynczego najlepszą metodę zostanie znaleziony, występuje błąd kompilacji.
*  `C` jest to typ, w ramach którego najlepszą metodę jest zadeklarowany jako metodę rozszerzenia.

Za pomocą `C` jako obiekt docelowy wywołania metody które są następnie przetwarzane jako wywołanie metody statycznej ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Poprzedni reguły oznaczają, że wystąpienie metody mają pierwszeństwo przed metody rozszerzenia, że metody rozszerzenia są dostępne w deklaracji przestrzeni nazw wewnętrzny mają pierwszeństwo przed metody rozszerzenia są dostępne w deklaracje zewnętrzne przestrzeni nazw i rozszerzenia metody zadeklarowane jako bezpośrednio w przestrzeni nazw mają pierwszeństwo przed metody rozszerzenia zaimportowane do tej samej przestrzeni nazw z za pomocą dyrektywy przestrzeni nazw. Na przykład:
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

W tym przykładzie `B`firmy metody mają pierwszeństwo przed z pierwszą metodą rozszerzenia i `C`firmy metody mają pierwszeństwo przed obu metod rozszerzenia.

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

Dane wyjściowe, w tym przykładzie są:
```
E.F(1)
D.G(2)
C.H(3)
```
`D.G` pierwszeństwo `C.G`, i `E.F` ma pierwszeństwo przed zarówno `D.F` i `C.F`.

#### <a name="delegate-invocations"></a>Delegowanie wywołań

Na wywołanie delegata *primary_expression* z *invocation_expression* musi być wartością z *delegate_type*. Co więcej, biorąc pod uwagę *delegate_type* jako funkcja składowa o tej samej listy parametrów jako *delegate_type*, *delegate_type* muszą być stosowane () [Dotyczy funkcja składowa](expressions.md#applicable-function-member)) w odniesieniu do *argument_list* z *invocation_expression*.

Przetwarzanie środowiska wykonawczego wywołanie delegata w postaci `D(A)`, gdzie `D` jest *primary_expression* z *delegate_type* i `A` jest opcjonalny *argument_list*, składa się z następujących czynności:

*  `D` jest oceniany. Jeśli ta ocena powoduje wyjątek, nie trzeba wykonywać dalszych czynności są wykonywane.
*  Wartość `D` jest sprawdzany w celu był prawidłowy. Jeśli wartość `D` jest `null`, `System.NullReferenceException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.
*  W przeciwnym razie `D` jest odwołanie do wystąpienia delegata. Element członkowski wywołania funkcji ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) są wykonywane na każdej z nich wywoływane na liście wywołanie delegata. Wywoływane jednostek składające się z wystąpieniem i metoda wystąpienia wystąpienie wywołania zasady jest wystąpienie zawartego w obiekcie możliwy do wywołania.

### <a name="element-access"></a>Dostęp do elementu

*Element_access* składa się z *primary_no_array_creation_expression*, a następnie "`[`" token, a następnie *argument_list*, a następnie " `]`"tokenu. *Argument_list* składa się z co najmniej jeden *argument*s, oddzielonych przecinkami.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

*Argument_list* z *element_access* nie mogą zawierać `ref` lub `out` argumentów.

*Element_access* dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)) jeśli zawiera co najmniej jeden z następujących czynności:

* *Primary_no_array_creation_expression* ma typ kompilacji `dynamic`.
* Co najmniej jednego wyrażenia *argument_list* ma typ kompilacji `dynamic` i *primary_no_array_creation_expression* nie ma typu tablicowego.

W tym przypadku kompilator klasyfikuje *element_access* jako wartość typu `dynamic`. Reguły poniżej, aby określić znaczenia *element_access* zostaną następnie zastosowane w czasie wykonywania, przy użyciu typu run-time zamiast typ kompilacji udostępnianych przez *primary_no_array_creation_expression*i *argument_list* wyrażeń, które mają typ kompilacji `dynamic`. Jeśli *primary_no_array_creation_expression* nie ma typu kompilacji `dynamic`, a następnie dostęp do elementu ulega sprawdzenie w czasie kompilacji ograniczona zgodnie z opisem w [kompilacji sprawdzanie dynamiczny Rozpoznanie przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Jeśli *primary_no_array_creation_expression* z *element_access* jest wartością *array_type*, *element_access* jest dostęp do tablicy ([tablicy dostępu](expressions.md#array-access)). W przeciwnym razie *primary_no_array_creation_expression* musi być zmienną lub wartość klasy, struktury lub typ interfejsu, który ma co najmniej jednego członka indeksatora, w którym to przypadku *element_access* jest dostęp indeksatora ([dostęp indeksatora](expressions.md#indexer-access)).

#### <a name="array-access"></a>Dostęp do tablicy

Aby uzyskać dostęp do tablicy *primary_no_array_creation_expression* z *element_access* musi być wartością z *array_type*. Ponadto *argument_list* tablicy dostępu nie może zawierać argumenty nazwane. Liczba wyrażeń w *argument_list* musi być taka sama jak ranga *array_type*, a każde wyrażenie musi być typu `int`, `uint`, `long`, `ulong`, lub musi umożliwiać niejawną konwersję do co najmniej jednej z tych typów.

Wynik oceny dostęp do tablicy jest zmienną typu elementu tablicy, a mianowicie elementu tablicy, wybrana przez wartości wyrażeń w *argument_list*.

Przetwarzanie środowiska wykonawczego dostęp do tablicy w formie `P[A]`, gdzie `P` jest *primary_no_array_creation_expression* z *array_type* i `A` jest *argument_list*, składa się z następujących czynności:

*  `P` jest oceniany. Jeśli ta ocena powoduje wyjątek, nie trzeba wykonywać dalszych czynności są wykonywane.
*  Wyrażenia indeksu *argument_list* są obliczane w kolejności od lewej do prawej. Po dokonaniu oceny każde wyrażenie indeksu niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)) jest wykonywane na jeden z następujących typów: `int`, `uint`, `long`, `ulong`. Pierwszy typ na tej liście, dla której istnieje niejawna konwersja jest wybierany. Na przykład jeśli wyrażenie indeksu ma typ `short` następnie niejawną konwersję do `int` jest wykonywana, ponieważ niejawne konwersje z elementu `short` do `int` i z `short` do `long` są możliwe. Jeśli Szacowanie wyrażenia indeksu lub kolejnych niejawnej konwersji powoduje wyjątek, nie dalsze wyrażenia indeksu są obliczane i żadne dodatkowe kroki są wykonywane.
*  Wartość `P` jest sprawdzany w celu był prawidłowy. Jeśli wartość `P` jest `null`, `System.NullReferenceException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.
*  Wartość każde wyrażenie w *argument_list* jest sprawdzana względem rzeczywiste granice każdego wymiaru instancji tabeli odwołuje się `P`. Jeśli co najmniej jednej wartości są poza zakresem, `System.IndexOutOfRangeException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.
*  Lokalizacja elementu tablicy, określone przez wyrażeń indeksu jest obliczana, a ta lokalizacja staje się wynik dostęp do tablicy.

#### <a name="indexer-access"></a>Dostęp indeksatora

Aby uzyskać dostęp indeksatora *primary_no_array_creation_expression* z *element_access* musi być zmienną lub wartość klasy, struktury lub typ interfejsu i ten typ musi implementować jeden lub więcej Indeksatory, które mają zastosowanie w odniesieniu do *argument_list* z *element_access*.

Powiązania w czasie przetwarzania dostęp indeksatora formularza `P[A]`, gdzie `P` jest *primary_no_array_creation_expression* klasy, struktury lub interfejsu typu `T`, i `A` jest *argument_list*, składa się z następujących czynności:

*  Set indeksatorów, dostarczone przez `T` jest konstruowany. Zestaw składa się z wszystkich indeksatorów zadeklarowanych w `T` lub podstawowym typem `T` , które nie są `override` deklaracje i są dostępne w bieżącym kontekście ([dostęp do elementu członkowskiego](basic-concepts.md#member-access)).
*  Zestaw jest ograniczone do tych indeksatorów, które są stosowane i nie jest ukryta za innymi indeksatorami. Następujące reguły są stosowane do każdego indeksatora `S.I` w zestawie, gdzie `S` jest typem, w której indeksator `I` jest zadeklarowany jako:
   * Jeśli `I` nie jest stosowana w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)), następnie `I` zostanie usunięty z zestawu.
   * Jeśli `I` jest stosowana w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)), następnie wszystkie indeksatory zadeklarowane w typie podstawowym z `S` są usuwane z zestawu.
   * Jeśli `I` jest stosowana w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)) i `S` jest inny niż typ klasy `object`, wszystkie indeksatory zadeklarowanych w interfejsie są usuwane z zestawu.
*  Jeśli wynikowy zestaw indeksatory Release candidate jest pusta, następnie istnieje nie dotyczy indeksatory i występuje błąd wiązania.
*  Najlepsze indeksatora set indeksatorów Release candidate jest identyfikowany za pomocą zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution). Jeśli nie można zidentyfikować pojedynczy indeksatora najlepsze, dostęp do indeksatora jest niejednoznaczny i występuje błąd wiązania.
*  Wyrażenia indeksu *argument_list* są obliczane w kolejności od lewej do prawej. Wynik przetworzenia dostęp indeksatora to wyrażenie, który został sklasyfikowany jako dostęp indeksatora. Wyrażenie dostępu do indeksatora odwołuje się do indeksatora, określone w poprzednim kroku, a ma wyrażenia skojarzonego wystąpienia `P` i lista argumentów skojarzonych z `A`.

W zależności od kontekstu, w którym jest używany, dostęp indeksatora powoduje wywołanie albo *pobierająca* lub *ustawiająca metoda dostępu* indeksatora. Jeśli dostęp indeksatora jest elementem docelowym przypisania, *ustawiająca metoda dostępu* wywoływana w celu przypisania nowej wartości ([przypisanie proste](expressions.md#simple-assignment)). We wszystkich innych przypadkach *pobierająca* wywoływana w celu uzyskania bieżącej wartości ([wartości wyrażeń](expressions.md#values-of-expressions)).

### <a name="this-access"></a>Ten dostęp

A *this_access* składa się z słowo zastrzeżone `this`.

```antlr
this_access
    : 'this'
    ;
```

A *this_access* jest dozwolona tylko w *bloku* konstruktora wystąpienia, metoda wystąpienia lub metodę dostępu, której wystąpienia. Daje on następujące znaczenie:

*  Gdy `this` jest używany w *primary_expression* w konstruktorze wystąpienia klasy, jest klasyfikowana jako wartość. Typ wartości jest typu wystąpienia ([typu wystąpienia](classes.md#the-instance-type)) klasy, w ramach którego występuje użycia, a wartość jest odwołanie do obiektu podczas konstruowania.
*  Gdy `this` jest używany w *primary_expression* w ramach metodę wystąpienia lub metoda dostępu do wystąpienia klasy jest klasyfikowana jako wartość. Typ wartości jest typu wystąpienia ([typu wystąpienia](classes.md#the-instance-type)) klasy, w którym występuje użycia, a wartość jest odwołanie do obiektu, dla której wywołano metodę lub metodę dostępu.
*  Gdy `this` jest używany w *primary_expression* w konstruktorze wystąpienia struktury, jest klasyfikowana jako zmienną. Typ zmiennej jest typem wystąpienia ([typu wystąpienia](classes.md#the-instance-type)) struktury, w którym występuje użycia, a zmiennej reprezentuje struktury budowany. `this` Zmiennej konstruktora wystąpienia struktury zachowuje się tak samo jak `out` parametr typu struktury — w szczególności oznacza to, zmienna musi być zdecydowanie przypisana w każdej ścieżce wykonywania wystąpienia Konstruktor.
*  Gdy `this` jest używany w *primary_expression* w ramach metodę wystąpienia lub metoda dostępu do wystąpienia struktury jest klasyfikowana jako zmienną. Typ zmiennej jest typem wystąpienia ([typu wystąpienia](classes.md#the-instance-type)) struktury, w którym występuje użycia.
   * Jeśli metoda lub metoda dostępu nie jest iteratorem ([Iteratory](classes.md#iterators)), `this` zmienna reprezentuje struktury, dla której metodę lub metodę dostępu została wywołana i zachowuje się tak samo jak `ref` parametr typu struktury.
   * Jeśli ta metoda lub metody dostępu jest iteratorem, `this` zmienna reprezentuje kopiowania struktury, dla której metodę lub metodę dostępu została wywołana i zachowuje się tak samo jak wartość parametru typu struktury.

Korzystanie z `this` w *primary_expression* w kontekście innych niż wymienione powyżej jest błąd w czasie kompilacji. W szczególności, nie jest możliwe do odwoływania się do `this` w statycznej metody dostępu właściwość statyczna lub *variable_initializer* deklaracji pola.

### <a name="base-access"></a>Dostęp bazowy

A *base_access* składa się z słowo zastrzeżone `base` następuje "`.`" token i identyfikator lub moduł *argument_list* w nawiasach kwadratowych:

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

A *base_access* umożliwia dostęp do składowych klasy bazowej, ukryte przez podobnie nazwanych elementów członkowskich w bieżącej klasie lub strukturze. A *base_access* jest dozwolona tylko w *bloku* konstruktora wystąpienia, metoda wystąpienia lub metodę dostępu, której wystąpienia. Gdy `base.I` odbywa się w klasie lub strukturze, `I` musi oznaczają składową klasy bazowej tej klasy lub struktury. Podobnie, gdy `base[E]` występuje w klasie, indeksator musi istnieć w klasie bazowej.

W czasie powiązania *base_access* wyrażeń formularza `base.I` i `base[E]` są oceniane, dokładnie tak, jakby były one napisane `((B)this).I` i `((B)this)[E]`, gdzie `B` jest klasą bazową klasy lub struktury, w której występuje konstrukcja. W efekcie `base.I` i `base[E]` odpowiadają `this.I` i `this[E]`, z wyjątkiem `this` są wyświetlane jako wystąpienia klasy bazowej.

Gdy *base_access* odwołuje się funkcja wirtualna elementu członkowskiego (metoda, właściwość lub indeksator), oznaczanie funkcji elementu członkowskiego do wywołania w czasie wykonywania ([sprawdzanie Rozpoznanie przeciążenia dynamicznej kompilacji ](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) zostanie zmieniony. Funkcja składowa, wywoływana jest określana przez wyszukiwanie najbardziej pochodnej ([metody wirtualnej](classes.md#virtual-methods)) elementu członkowskiego funkcji w odniesieniu do `B` (zamiast w odniesieniu do typów w czasie wykonywania z `this`, jak będzie zwykle w programie access-base). W związku z tym, w ramach `override` z `virtual` funkcja składowa *base_access* może służyć do wywołania dziedziczona implementacja funkcji elementu członkowskiego. Jeśli odwołuje się funkcja składowa *base_access* jest abstrakcyjny, występuje błąd wiązania.

### <a name="postfix-increment-and-decrement-operators"></a>Inkrementacja przyrostkowa i operatory dekrementacji

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

Argument przyrostka inkrementacji i dekrementacji operacji musi być wyrażeniem sklasyfikowane jako zmienną, dostęp do właściwości lub indeksatora dostępu. Wynikiem operacji jest wartością tego samego typu jako argumentu operacji.

Jeśli *primary_expression* ma typ kompilacji `dynamic` , a następnie operator jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)), *post_increment_expression*lub *post_decrement_expression* ma typ kompilacji `dynamic` i następujące zasady są stosowane w czasie wykonywania za pomocą typu run-time z *primary_expression*.

Jeśli argument przyrostkowe Zwiększ operacja dekrementacji jest właściwością lub dostęp indeksatora, właściwość lub indeksator musi mieć zarówno `get` i `set` metody dostępu. Jeśli nie jest to możliwe, występuje błąd wiązania.

Rozpoznanie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Wstępnie zdefiniowane `++` i `--` operatory istnieją dla następujących typów: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`i dowolnego typu wyliczeniowego. Wstępnie zdefiniowane `++` operatory zwracają wartość, o których produkowane przez dodanie 1 argument i wstępnie zdefiniowane `--` operatory zwracają wartość, o których produkowane przez odjęcie ilości 1 z operandu. W `checked` kontekstu, jeśli wynik to dodawania lub odejmowania jest spoza zakresu typu wyniku, a typ wyniku jest typu całkowitoliczbowego lub typu wyliczeniowego `System.OverflowException` zgłaszany.

Przetwarzanie środowiska wykonawczego przyrostka inkrementacji lub dekrementacji operacji formularza `x++` lub `x--` składa się z następujących czynności:

*   Jeśli `x` zostanie sklasyfikowany jako zmienną:
    * `x` jest oceniany w celu utworzenia zmiennej.
    * Wartość `x` jest zapisywany.
    * Operatora jest wywoływana z wartością zapisaną `x` jako argumentem.
    * Wartość zwracana przez operator znajduje się w lokalizacji określonej przez ocenę `x`.
    * Zapisane wartości `x` staje się wynik operacji.
*   Jeśli `x` zostanie sklasyfikowany jako właściwość lub indeksator dostępu:
    * Wyrażenia wystąpienia (Jeśli `x` nie jest `static`) i listą argumentów (Jeśli `x` jest dostęp indeksatora) skojarzony z `x` są oceniane, a wyniki są używane w kolejnych żadaniach `get` i `set` wywołania metody dostępu.
    * `get` Akcesor `x` wywoływaną i jest zapisywany w zwracanej wartości.
    * Operatora jest wywoływana z wartością zapisaną `x` jako argumentem.
    * `set` Akcesor `x` jest wywoływana z wartością zwróconą przez operatora jako jego `value` argumentu.
    * Zapisane wartości `x` staje się wynik operacji.

`++` i `--` Operatorzy pomocy technicznej notacji prefiksów ([przedrostkowe i operatory dekrementacji](expressions.md#prefix-increment-and-decrement-operators)). Zazwyczaj wynikiem `x++` lub `x--` jest wartością `x` przed wykonaniem operacji, podczas gdy wynik `++x` lub `--x` jest wartością `x` po wykonaniu operacji. W obu przypadkach `x` ma taką samą wartość po wykonaniu operacji.

`operator ++` Lub `operator --` implementacja może być wywoływany przy użyciu notacji przyrostka lub przedrostka. Nie jest możliwe operator oddzielne implementacje dla dwóch notacji.

### <a name="the-new-operator"></a>New — operator

`new` Operator jest używany do tworzenia nowych wystąpień typów.

Istnieją trzy rodzaje `new` wyrażeń:

*  Wyrażenia tworzenia obiektów są używane do tworzenia nowych wystąpień typów klas i typów wartości.
*  Wyrażenie tworzenia tablicy są używane do tworzenia nowych wystąpień typów tablicowych.
*  Wyrażenia tworzenia delegata są używane do tworzenia nowych wystąpień obiektu delegowanego typów.

`new` Operator wymaga utworzenia wystąpienia typu, ale nie musi oznaczać dynamicznej alokacji pamięci. W szczególności wystąpień typów wartości wymagane nie dodatkowe pamięci powyżej zmiennych, w których są one przechowywane i występować nie dynamicznej alokacji podczas `new` służy do tworzenia wystąpień typów wartości.

#### <a name="object-creation-expressions"></a>Wyrażenia tworzenia obiektów

*Object_creation_expression* służy do tworzenia nowego wystąpienia *class_type* lub *value_type*.

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

*Typu* z *object_creation_expression* musi być *class_type*, *value_type* lub *type_parameter* . *Typu* nie może być `abstract` *class_type*.

Opcjonalny *argument_list* ([listy argumentów](expressions.md#argument-lists)) jest dozwolona tylko wtedy, gdy *typu* jest *class_type* lub *struct_ Typ*.

Wyrażenie tworzenia obiektu można pominąć listy argumentów konstruktora, a otaczającym nawiasów pod warunkiem, że obejmuje Inicjator obiektu lub inicjatora kolekcji. Pominięcie listy argumentów konstruktora i otaczający nawiasów jest równoznaczne z użyciem pustą listą argumentów.

Przetwarzanie wyrażenie tworzenia obiektu, który obejmuje Inicjator obiektu lub inicjatora kolekcji składa się z przetwarzaniem pierwsze konstruktora wystąpienia i następnie przetwarzania inicjalizacje element członkowski lub element określony przez inicjatora obiektów ([ Inicjatory obiektów](expressions.md#object-initializers)) lub inicjatora kolekcji ([inicjatory kolekcji](expressions.md#collection-initializers)).

Jeśli którykolwiek z argumentów opcjonalny *argument_list* ma typ kompilacji `dynamic` , a następnie *object_creation_expression* dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)) i następujące zasady są stosowane w czasie wykonywania za pomocą typu run-time tych argumentów *argument_list* mają typów w czasie kompilacji `dynamic`. Jednak tworzenie obiektu ulega sprawdzenie w czasie kompilacji ograniczona zgodnie z opisem w [kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Powiązania w czasie przetwarzania *object_creation_expression* formularza `new T(A)`, gdzie `T` jest *class_type* lub *value_type* i `A` to opcjonalna *argument_list*, składa się z następujących czynności:

*   Jeśli `T` jest *value_type* i `A` nie występuje:
    * *Object_creation_expression* jest wywołanie konstruktora domyślnego. Wynik *object_creation_expression* jest wartość typu `T`, czyli wartość domyślna dla `T` zgodnie z definicją w [typu System.ValueType](types.md#the-systemvaluetype-type).
*   W przeciwnym razie, jeśli `T` jest *type_parameter* i `A` nie występuje:
    * Jeśli nie ograniczenie typu wartości lub ograniczenie konstruktora ([ograniczenia parametru typu](classes.md#type-parameter-constraints)) została określona dla `T`, występuje błąd wiązania.
    * Wynik *object_creation_expression* jest wartością typu run-time, powiązanej parametr typu, a mianowicie wynik wywoływania domyślnego konstruktora typu. Typu run-time może być typem referencyjnym lub typem wartości.
*   W przeciwnym razie, jeśli `T` jest *class_type* lub *struct_type*:
    * Jeśli `T` jest `abstract` *class_type*, wystąpi błąd kompilacji.
    * Konstruktor wystąpienia do wywołania, jest określany za pomocą zasad rozpoznawania przeciążenia [Rozpoznanie przeciążenia](expressions.md#overload-resolution). Zestaw konstruktory wystąpień Release candidate składa się z wszystkie konstruktory dostępne wystąpienia zadeklarowanych w `T` które są stosowane w odniesieniu do `A` ([dotyczy funkcja składowa](expressions.md#applicable-function-member)). Jeśli zestaw Release candidate konstruktorów wystąpienia jest pusta lub nie można zidentyfikować pojedynczy najlepsze konstruktora wystąpienia, występuje błąd wiązania.
    * Wynik *object_creation_expression* jest wartość typu `T`, czyli wartość utworzone przez wywołanie konstruktora wystąpienia, określone w poprzednim kroku.
*  W przeciwnym razie *object_creation_expression* jest nieprawidłowy, i występuje błąd wiązania.

Nawet wtedy, gdy *object_creation_expression* jest dynamicznie powiązane, typ kompilacji jest nadal `T`.

Przetwarzanie środowiska wykonawczego *object_creation_expression* formularza `new T(A)`, gdzie `T` jest *class_type* lub *struct_type* i `A` to opcjonalna *argument_list*, składa się z następujących czynności:

*   Jeśli `T` jest *class_type*:
    * Nowe wystąpienie klasy `T` jest przydzielony. Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia `System.OutOfMemoryException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.
    * Wszystkie pola nowego wystąpienia są inicjowane do wartości domyślnych ([wartości domyślne](variables.md#default-values)).
    * Konstruktora wystąpienia jest wywoływana, zgodnie z regułami funkcji elementu członkowskiego wywołania ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Odwołanie do nowo przydzielonego wystąpienia jest automatycznie przekazywana do konstruktora wystąpienia, a wystąpienie jest możliwy z w ramach tego Konstruktor jako `this`.
*   Jeśli `T` jest *struct_type*:
    * Wystąpienie typu `T` jest tworzony przez przydzielanie tymczasowej zmiennej lokalnej. Ponieważ konstruktora wystąpienia *struct_type* jest wymagany do zdecydowanie przypisania wartości do każdego pola wystąpienia tworzony, niezbędne jest nie inicjowania zmiennej tymczasowej.
    * Konstruktora wystąpienia jest wywoływana, zgodnie z regułami funkcji elementu członkowskiego wywołania ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Odwołanie do nowo przydzielonego wystąpienia jest automatycznie przekazywana do konstruktora wystąpienia, a wystąpienie jest możliwy z w ramach tego Konstruktor jako `this`.

#### <a name="object-initializers"></a>Inicjatory obiektów

***Inicjatora obiektu*** określa wartości zero lub więcej pól, właściwości lub indeksowanych elementy obiektu.

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

Inicjatora obiektu, który składa się z sekwencji inicjatorów składowych, ujęta w `{` i `}` tokeny i je przecinkami. Każdy *member_initializer* wyznacza miejsce docelowe dla inicjowania. *Identyfikator* należy nazywać dostępne pola lub właściwości obiektu inicjowany, natomiast *argument_list* ujęte w nawiasy kwadratowe, należy określić argumenty dla indeksatora dostępny na inicjowany obiekt. Jest to błąd dla inicjatora obiektów dołączyć więcej niż jeden inicjator składowej dla tego samego pola lub właściwości.

Każdy *initializer_target* następuje znak równości i wyrażenia, inicjatora obiektów lub inicjatora kolekcji. Nie jest możliwe dla wyrażeń w obrębie inicjatora obiektów do odwoływania się do nowo utworzony obiekt, który inicjuje go.

Inicjator składowej, określająca wyrażenie po znaku równości jest przetwarzana w taki sam sposób jak przypisania ([przypisanie proste](expressions.md#simple-assignment)) do obiektu docelowego.

Inicjator składowej, który określa inicjatora obiektu po znaku równości ***inicjatora obiektów zagnieżdżonych***, czyli inicjalizacji osadzonego obiektu. Zamiast przypisywania nową wartość do pola lub właściwości, przydziały w inicjatorze obiektu zagnieżdżone są traktowane jako przypisania do elementów członkowskich pola lub właściwości. Inicjatory obiektów zagnieżdżonych nie można zastosować do właściwości z typem wartości lub pola tylko do odczytu z typem wartości.

Inicjator składowej, która określa inicjatora kolekcji po znaku równości jest inicjalizacji embedded kolekcji. Zamiast przypisywania nową kolekcję, do pola docelowego, właściwość lub indeksator, elementy zawarte w inicjatorze są dodawane do kolekcji odwołuje się element docelowy. Element docelowy musi być typu kolekcji, który spełnia wymagania określone w [inicjatory kolekcji](expressions.md#collection-initializers).

Argumenty do inicjatora indeksu będzie obliczane zawsze dokładnie jeden raz. W związku z tym nawet wtedy, gdy argumenty znajdą się wprowadzenie nigdy używane (np. z powodu pustego inicjalizatora zagnieżdżonych), ich zostanie obliczone dla ich skutki uboczne.

Następujące klasy reprezentuje punkt o współrzędnych dwa:
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

Wystąpienie `Point` mogą być tworzone i inicjowane w następujący sposób:
```csharp
Point a = new Point { X = 0, Y = 1 };
```
która ma taki sam skutek jak
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
gdzie `__a` jest niewidoczna w przeciwnym razie i są niedostępne na zmiennej tymczasowej. Następujące klasy reprezentuje prostokąt, który został utworzony na podstawie dwa punkty:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

Wystąpienie `Rectangle` mogą być tworzone i inicjowane w następujący sposób:
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
która ma taki sam skutek jak
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
gdzie `__r`, `__p1` i `__p2` to tymczasowe zmienne, które są niewidoczne i są niedostępne.

Jeśli `Rectangle`firmy Konstruktor przydziela dwa osadzone `Point` wystąpień
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
konstrukcja następujące może służyć do zainicjowania osadzonego `Point` wystąpień zamiast przypisywać nowych wystąpień:
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
która ma taki sam skutek jak
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

Podane odpowiednie definicje języka c, w poniższym przykładzie:
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
jest równoważne z tej serii przypisania:
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
gdzie `__c`itp wygenerowanego zmiennych, które są niewidoczne i niedostępne do kodu źródłowego. Należy pamiętać, że argumenty `[0,0]` ocenianą tylko raz i argumenty `[1,2]` są wykonywane tylko raz, nawet jeśli nigdy nie są one używane.

#### <a name="collection-initializers"></a>Inicjatory kolekcji

Inicjator kolekcji określa elementów kolekcji.

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

Inicjator kolekcji składa się z sekwencji inicjatory elementów, ujęta w `{` i `}` tokeny i je przecinkami. Inicjator każdego elementu określa element do dodania do inicjowany obiekt kolekcji i składa się z listą wyrażeń ujęta w `{` i `}` tokeny i je przecinkami.  Element pojedynczego wyrażenia inicjatora mogą być zapisywane bez nawiasów klamrowych, ale następnie nie może być wyrażenia przypisania, aby uniknąć niejednoznaczności z inicjatorami elementu członkowskiego. *Non_assignment_expression* produkcji jest zdefiniowany w [wyrażenie](expressions.md#expression).

Oto przykład wyrażenie tworzenia obiektu, które obejmuje inicjatora kolekcji:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

Obiekt kolekcji, do którego zastosowano inicjatora kolekcji musi być typu, który implementuje `System.Collections.IEnumerable` lub występuje błąd kompilacji. Dla każdego określonego elementu w kolejności, wywołuje inicjatora kolekcji `Add` metody w elemencie docelowym obiekt z listy wyrażenia inicjatora elementu jako listę argumentów, stosując wyszukanie członka normalne i przeciążenia dla każdego wywołania. W związku z tym, obiekt kolekcji musi mieć odpowiednie wystąpienie lub rozszerzenie metody o nazwie `Add` dla każdego inicjatora elementu.

Następujące klasy reprezentuje kontaktu z nazwą i Lista numerów telefonów:
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

A `List<Contact>` mogą być tworzone i inicjowane w następujący sposób:
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
która ma taki sam skutek jak
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
gdzie `__clist`, `__c1` i `__c2` to tymczasowe zmienne, które są niewidoczne i są niedostępne.

#### <a name="array-creation-expressions"></a>Wyrażenie tworzenia tablicy

*Array_creation_expression* służy do tworzenia nowego wystąpienia *array_type*.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

Wyrażenie tworzenia tablicy pierwszego formularza przydziela wystąpienie tablicy typu, który powoduje usunięcie wszystkich poszczególnych wyrażenia z listy wyrażeń. Na przykład, wyrażenie tworzenia tablicy `new int[10,20]` tworzy wystąpienie tablicy typu `int[,]`, a wyrażenie tworzenia tablicy `new int[10][,]` tworzy tablicę typu `int[][,]`. Każde wyrażenie w lista wyrażeń musi być typu `int`, `uint`, `long`, lub `ulong`, lub niejawnie konwertowane na co najmniej jeden z tych typów. Wartość każde wyrażenie określa długość odpowiedniego wymiaru w wystąpieniu nowo przydzielonego tablicy. Ponieważ długość tablicy drugiego wymiaru musi być nieujemna, jest błąd kompilacji, aby *constant_expression* z ujemną wartością na liście wyrażeń.

Z wyjątkiem składowych w niebezpiecznym kontekście ([niebezpieczny kontekst](unsafe-code.md#unsafe-contexts)), układ macierzy jest nieokreślona.

Jeśli wyrażenie tworzenia tablicy pierwszy formularz zawiera inicjatora tablicy, każde wyrażenie w lista wyrażeń musi być stałą i rangi i wymiar długości określonej przez lista wyrażeń musi być zgodne z nazwami inicjatora tablicy.

Wyrażenie tworzenia tablicy drugi lub trzeci formularz ranga specyfikatora typu, lub rangę określonej tablicy musi odpowiadać w przypadku inicjatora tablicy. Długości wymiarów poszczególnych są dedukowane z liczbą elementów w każdej z odpowiadającymi im poziomami zagnieżdżenia inicjatora tablicy. W związku z tym wyrażenie
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
dokładnie odpowiada
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

Wyrażenie tworzenia tablicy trzeci formularza jest określany jako ***niejawnie wpisane wyrażenie tworzenia tablicy***. Jest on podobny do drugiej formy, z tą różnicą, że typ elementu tablicy nie zostanie jawnie podana, ale uznane za najlepiej wspólnego typu ([znajdowanie najlepszy typ wspólnego zestawu wyrażeń](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) zestawu wyrażeń w tablicy Inicjator. Dla tablicy wielowymiarowej czyli takie, gdzie *rank_specifier* zawiera co najmniej jeden przecinek, ten zestaw zawiera wszystkie *wyrażenie*s znaleziony w zagnieżdżonych *array_initializer*s.

Inicjatory tablic są dokładniejszym opisem zawartym w [Array inicjatory](arrays.md#array-initializers).

Wynik obliczania wartości wyrażenie tworzenia tablicy jest klasyfikowany jako wartości, to znaczy odwołanie do wystąpienia nowo przydzielonego tablicy. Przetwarzanie środowiska wykonawczego wyrażenie tworzenia tablicy składa się z następujących czynności:

*  Wymiar wyrażenia długości *expression_list* są obliczane w kolejności od lewej do prawej. Po dokonaniu oceny każde wyrażenie niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)) jest wykonywane na jeden z następujących typów: `int`, `uint`, `long`, `ulong`. Pierwszy typ na tej liście, dla której istnieje niejawna konwersja jest wybierany. Jeśli Szacowanie wyrażenia lub kolejnych niejawnej konwersji powoduje wyjątek, nie dalsze wyrażenia są obliczane i nie trzeba wykonywać dalszych czynności są wykonywane.
*  Wartości obliczane dla długości wymiarów są weryfikowane w następujący sposób. Jeśli jeden lub więcej wartości jest mniejsza od zera, `System.OverflowException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.
*  Wystąpienie tablicy o długości danego wymiaru jest przydzielany. Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia `System.OutOfMemoryException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.
*  Wszystkie elementy w nowym wystąpieniu tablicy są inicjowane do wartości domyślnych ([wartości domyślne](variables.md#default-values)).
*  Jeśli wyrażenie tworzenia tablicy zawiera inicjatora tablicy, każde wyrażenie w inicjatorze tablicy jest obliczane i przypisane do odpowiadającego mu elementu tablicy. Wersje ewaluacyjne i przydziały są wykonywane w kolejności wyrażenia są zapisywane w inicjatorze tablicy — innymi słowy, elementy są inicjowane rosnąco indeks, z tym wymiarem po prawej stronie najpierw zwiększyć. Jeśli Szacowanie danego wyrażenia lub następne przypisanie do odpowiedniego elementu tablicy, powoduje wyjątek, następnie nie dodatkowe elementy są inicjowane (i wszystkie pozostałe elementy dlatego mają wartości domyślne).

Wyrażenie tworzenia tablicy pozwala na tworzenie wystąpienia tablicę z elementami typu tablicowego, ale elementy takie tablicy musi zostać zainicjowana ręcznie. Na przykład instrukcja
```csharp
int[][] a = new int[100][];
```
tworzy tablicę jednowymiarową elementami 100 typu `int[]`. Początkowa wartość każdy element jest `null`. Nie jest możliwe dla tego samego wyrażenie tworzenia tablicy również utworzyć podrzędne tablice i — instrukcja
```csharp
int[][] a = new int[100][5];        // Error
```
spowoduje błąd kompilacji. Podczas tworzenia wystąpienia podrzędne tablic zamiast tego muszą być wykonywane ręcznie, jak
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

Gdy tablicy tablic ma kształt "prostokątna", to znaczy, gdy podrzędne tablice są wszystkie tę samą długość, jest bardziej wydajne, aby używać tablicy wielowymiarowej. W powyższym przykładzie, podczas tworzenia wystąpienia tablicy tablic tworzy obiekty 101 — jedna tablica zewnętrzne i 100 podrzędne tablice. Z kolei
```csharp
int[,] = new int[100, 5];
```
tworzy tylko jeden obiekt dwuwymiarową tablicę i wykonuje alokacji w pojedynczej instrukcji.

Poniżej przedstawiono przykłady niejawnie typizowana tablica tworzenia wyrażeń:
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

Ostatnie wyrażenie powoduje błąd w czasie kompilacji, ponieważ ani `int` ani `string` jest niejawnie konwertowany do drugiego, a więc nie najlepiej często wpisz. Wyrażenie tworzenia tablicy jawnie wpisanych musi być używane w tym przypadku, na przykład określenie typu, który ma być `object[]`. Alternatywnie jeden z elementów mogą być rzutowane na wspólnej typ podstawowy, który następnie stałyby typu wywnioskowanego elementu.

Niejawnie typizowana tablica tworzenia wyrażenia może być łączone z inicjatorach obiektu anonimowego ([wyrażenia tworzenia obiektu anonimowego](expressions.md#anonymous-object-creation-expressions)) do utworzenia anonimowo wpisane struktur danych. Na przykład:
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

#### <a name="delegate-creation-expressions"></a>Wyrażenia tworzenia delegata

A *delegate_creation_expression* służy do tworzenia nowego wystąpienia *delegate_type*.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

Argument wyrażenia tworzenia delegata musi być grupy metody, funkcją anonimową lub wartość dowolnego typu czasu kompilacji `dynamic` lub *delegate_type*. Jeśli argument jest grupa metod, określa on metodę i dla metody wystąpienia, obiektu, dla której chcesz utworzyć delegata. Jeśli argument jest funkcją anonimową bezpośrednio definiuje parametry i treści metody docelowej delegata. Jeśli argument jest wartością identyfikuje wystąpienie delegata, o której chcesz utworzyć kopię.

Jeśli *wyrażenie* ma typ kompilacji `dynamic`, *delegate_creation_expression* dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)) i poniższymi regułami są stosowane w czasie wykonywania za pomocą typu run-time z *wyrażenie*. W przeciwnym razie zasady są stosowane w czasie kompilacji.

Powiązania w czasie przetwarzania *delegate_creation_expression* formularza `new D(E)`, gdzie `D` jest *delegate_type* i `E` jest *wyrażenia* , składa się z następujących czynności:

*  Jeśli `E` jest grupą metoda wyrażenie tworzenia delegata jest przetwarzana w taki sam sposób jak konwersja grup — metoda ([metody konwersji grup](conversions.md#method-group-conversions)) z `E` do `D`.
*  Jeśli `E` jest funkcją anonimową wyrażenie tworzenia delegata jest przetwarzana w taki sam sposób jak konwersję funkcja anonimowa ([konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions)) z `E` do `D`.
*  Jeśli `E` jest wartością, `E` musi być zgodny ([delegować deklaracje](delegates.md#delegate-declarations)) za pomocą `D`, a wynik jest odwołaniem do nowo utworzony delegat typu `D` odwołujący się do tego samego wywołania listy jako `E`. Jeśli `E` nie jest zgodny z `D`, wystąpi błąd kompilacji.

Przetwarzanie środowiska wykonawczego *delegate_creation_expression* formularza `new D(E)`, gdzie `D` jest *delegate_type* i `E` jest *wyrażenia* , składa się z następujących czynności:

*   Jeśli `E` jest grupą metoda wyrażenie tworzenia delegata jest oceniane jako metoda konwersji grupy ([metody konwersji grup](conversions.md#method-group-conversions)) z `E` do `D`.
*   Jeśli `E` jest funkcją anonimową tworzenia delegata jest oceniane jako funkcja anonimowa konwersja `E` do `D` ([konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions)).
*   Jeśli `E` jest wartością *delegate_type*:
    * `E` jest oceniany. Jeśli ta ocena powoduje wyjątek, nie trzeba wykonywać dalszych czynności są wykonywane.
    * Jeśli wartość `E` jest `null`, `System.NullReferenceException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.
    * Nowe wystąpienie typu delegata `D` jest przydzielony. Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia `System.OutOfMemoryException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.
    * Nowe wystąpienie delegata jest inicjowany za pomocą tej samej listy wywołań jako wystąpienie delegata, określone przez `E`.

Listy wywołań obiektu delegowanego jest ustalany w chwili delegata zostanie uruchomiony, a następnie pozostaje stała przez cały okres istnienia obiektu delegowanego. Innymi słowy nie jest możliwa zmiana jednostek wywoływalnej docelowego delegata po jego utworzeniu. Gdy dwa delegaty są połączone lub jeden zostanie usunięta z innego ([delegować deklaracje](delegates.md#delegate-declarations)), nowe delegowanie wyniki; nie istniejące pełnomocnik ma jego zawartość, zmienione.

Nie jest możliwe do utworzenia delegata, która odwołuje się do właściwości, indeksatora, operatora zdefiniowanego przez użytkownika, Konstruktor wystąpienia, destruktor lub Konstruktor statyczny.

Zgodnie z powyższym opisem kiedy delegat jest tworzony z grupy metod formalnej listy parametrów i zwracany typ delegata określenie przeciążonych metod, aby wybrać. W przykładzie
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
`A.f` pola jest inicjowany z delegatem, który odwołuje się do drugiego `Square` metody, ponieważ ta metoda dokładnie odpowiada formalnej listy parametrów i zwracanym typem `DoubleFunc`. Gdyby drugi `Square` metody, które nie są obecne, będzie mieć wystąpił błąd kompilacji.

#### <a name="anonymous-object-creation-expressions"></a>Anonimowy obiekt tworzenia wyrażeń

*Anonymous_object_creation_expression* służy do tworzenia obiektu typu anonimowego.

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

Inicjator obiektu anonimowego deklaruje typu anonimowego i zwraca wystąpienie tego typu. Typ anonimowy jest typem klasy pustego, który dziedziczy bezpośrednio z `object`. Elementy członkowskie typu anonimowego są sekwencję właściwości tylko do odczytu, wnioskowany z Inicjator obiektu anonimowego użyty do utworzenia wystąpienia typu. W szczególności Inicjator obiektu anonimowego formularza
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
deklaruje typu anonimowego formularza
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
gdzie każdy `Tx` jest typu odpowiedniego wyrażenia `ex`. Wyrażenie użyte w *member_declarator* musi mieć typ. Dlatego jest to błąd kompilacji w wyrażeniu w *member_declarator* jako wartość null lub jest funkcją anonimową. Jest również błąd kompilacji, aby wyrażenie ma niezabezpieczonego typu.

Nazwy typu anonimowego i parametru, aby jego `Equals` metody są automatycznie generowane przez kompilator i nie może się odwoływać tekstu programu.

W tym samym programie dwóch inicjatory obiekt anonimowy, które określają sekwencję właściwości tych samych nazw i typów w czasie kompilacji w tej samej kolejności dadzą wystąpień tego samego typu anonimowego.

W przykładzie
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
przypisanie w ostatnim wierszu jest dozwolona, ponieważ `p1` i `p2` są tego samego typu anonimowego.

`Equals` i `GetHashcode` metody dla typów anonimowych zastąpienia metody dziedziczone z `object`i są definiowane w kategoriach `Equals` i `GetHashcode` właściwości, tak aby dwóch wystąpień tego samego typu anonimowego są takie same Jeśli i tylko wtedy, gdy ich właściwości są takie same.

Deklarator składowej można stosować skrót do prostą nazwą ([wnioskowanie o typie](expressions.md#type-inference)), dostęp do elementu członkowskiego ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), dostępu bazowego ([podstawowa dostępu](expressions.md#base-access)) lub dostęp do elementu członkowskiego warunkowe null ([wyrażenia warunkowe Null jako rzutowanie inicjatorach](expressions.md#null-conditional-expressions-as-projection-initializers)). Jest to nazywane ***inicjatora projekcji*** i jest skrótem deklarację i przypisanie do właściwości o takiej samej nazwie. W szczególności deklaratory członków formularzy
```csharp
identifier
expr.identifier
```
odpowiadają dokładnie następujące polecenie, odpowiednio:
```csharp
identifier = identifier
identifier = expr.identifier
```

W związku z tym, w przypadku inicjatora projekcji *identyfikator* wybiera zarówno wartość, jak i pola lub właściwości, do którego jest przypisana wartość. Intuicyjnie inicjatora projekcji projektów nie tylko wartości, ale również nazwę wartości.

### <a name="the-typeof-operator"></a>Typeof — operator

`typeof` Operator jest używany do uzyskiwania `System.Type` obiektu dla typu.

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

Pierwszy formularz *typeof_expression* składa się z `typeof` — słowo kluczowe, a następnie ujęty w nawiasy *typu*. Jest wynikiem wyrażenia tego formularza `System.Type` obiektu dla wskazanego typu. Jest tylko jedna `System.Type` obiektu dla danego typu. Oznacza to, że dla typu `T`, `typeof(T) == typeof(T)` ma zawsze wartość true. *Typu* nie może być `dynamic`.

Drugiej formy *typeof_expression* składa się z `typeof` — słowo kluczowe, a następnie ujęty w nawiasy *unbound_type_name*. *Unbound_type_name* jest bardzo podobny do *type_name* ([nazw Namespace i typ](basic-concepts.md#namespace-and-type-names)) z tą różnicą, że *unbound_type_name* zawiera *generic_dimension_specifier*s gdzie *type_name* zawiera *type_argument_list*s. Gdy argument operacji *typeof_expression* to sekwencja tokenów spełnia gramatyki zarówno *unbound_type_name* i *type_name*, to znaczy jeśli zawiera on ani *generic_dimension_specifier* ani *type_argument_list*, sekwencja tokenów jest uważany za *type_name*. Znaczenie *unbound_type_name* jest określany w następujący sposób:

*  Konwertuj sekwencja tokenów *type_name* , zastępując każdego *generic_dimension_specifier* z *type_argument_list* mających taką samą liczbę przecinkami i słowo kluczowe `object` ponieważ do każdego *type_argument*.
*  Oceń wynikowy *type_name*, podczas ignoruje wszystkie ograniczenia parametru typu.
*  *Unbound_type_name* jest rozpoznawana jako ogólnego typu niepowiązanego skojarzone z wynikowego skonstruowanego typu ([powiązane i wskazanych typów](types.md#bound-and-unbound-types)).

Wynik *typeof_expression* jest `System.Type` obiekt wynikowy niepowiązany typ ogólny.

Trzecia formą *typeof_expression* składa się z `typeof` — słowo kluczowe, a następnie ujęty w nawiasy `void` — słowo kluczowe. Jest wynikiem wyrażenia tego formularza `System.Type` obiekt, który reprezentuje braku typu. Typ obiektu zwracanego przez `typeof(void)` różni się od obiektu typu zwracane dla dowolnego typu. Ten obiekt specjalny typ przydaje się w bibliotekach klas, umożliwiające odbicia do metody w języku, gdzie chcesz tych metod, aby sposobem reprezentowania zwracany typ dowolnej metody, w tym metody void, z wystąpieniem `System.Type`.

`typeof` Operator może być używany dla parametru typu. Wynik jest `System.Type` obiektu dla typu run-time, który był powiązany z parametrem typu. `typeof` Operator może również służyć skonstruowanego typu lub niepowiązanych typu ogólnego ([powiązane i wskazanych typów](types.md#bound-and-unbound-types)). `System.Type` Obiektu dla typu niepowiązanego dla ogólnego nie jest taka sama jak `System.Type` obiektu typu wystąpienia. Typ wystąpienia jest zawsze zamknięte skonstruowanego typu w czasie wykonywania więc jego `System.Type` obiekt jest zależny od argumentów typu run-time w użyciu, podczas niepowiązanych typu ogólnego nie ma argumentów typu.

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
generuje następujące wyniki:
```
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

Należy pamiętać, że `int` i `System.Int32` tego samego typu.

Należy również zauważyć, że wynik `typeof(X<>)` nie zależy od argumentu typu, ale wynik `typeof(X<T>)` jest.

### <a name="the-checked-and-unchecked-operators"></a>Operatory checked i unchecked

`checked` i `unchecked` operatory są używane do sterowania ***przepełnienia, sprawdzając kontekst*** typu całkowitego operacje arytmetyczne i konwersje.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

`checked` Operator oblicza wyrażenie zawarte w kontekście sprawdzanym i `unchecked` operator oblicza wyrażenie zawarte w kontekście niesprawdzanym. A *checked_expression* lub *unchecked_expression* odpowiadają dokładnie *parenthesized_expression* ([wyrażeniach z nawiasami](expressions.md#parenthesized-expressions)), z tą różnicą, że zawarte wyrażenie jest obliczane w danym przepełnienia, sprawdzając kontekst.

Przepełnienie, sprawdzając kontekst można także kontrolować za pomocą `checked` i `unchecked` instrukcji ([instrukcji checked i unchecked](statements.md#the-checked-and-unchecked-statements)).

Następujące operacje dotyczy przepełnienia, sprawdzając kontekst ustanowione przez `checked` i `unchecked` operatory i instrukcje:

*  Wstępnie zdefiniowane `++` i `--` operatorów jednoargumentowych ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators) i [przedrostkowe i operatory dekrementacji](expressions.md#prefix-increment-and-decrement-operators)), gdy argument jest całkowitoliczbowego Typ.
*  Wstępnie zdefiniowane `-` operatora jednoargumentowego ([jednoargumentowy minus operator](expressions.md#unary-minus-operator)), gdy argument jest typu całkowitego.
*  Wstępnie zdefiniowane `+`, `-`, `*`, i `/` operatory binarne ([operatorów arytmetycznych](expressions.md#arithmetic-operators)), gdy oba operandy są typu całkowitoliczbowego.
*  Jawnych konwersji liczbowych ([jawnych konwersji liczbowych](conversions.md#explicit-numeric-conversions)) z jednego typu całkowitego, do innego typu całkowitego lub z `float` lub `double` na typ całkowitoliczbowy.

Po jednej z powyższych operacji rezultatów, jest zbyt duży do reprezentowania w typie docelowym, kontekst, w którym ta operacja jest wykonywane formantów wynikowe zachowania:

*  W `checked` kontekstu, jeśli operacja jest wyrażeniem stałym ([wyrażeń stałych](expressions.md#constant-expressions)), wystąpi błąd kompilacji. W przeciwnym razie, gdy operacja jest wykonywana w czasie wykonywania `System.OverflowException` zgłaszany.
*  W `unchecked` kontekstu, wynik zostanie obcięta przez odrzucenie wszelkich najbardziej znaczących bitów, które nie mieszczą się w typie docelowym.

Niestałe wyrażeń (wyrażeń, które są oceniane w czasie wykonywania), nie są ujęte w jakikolwiek `checked` lub `unchecked` operatorów lub instrukcji przepełnienie domyślne sprawdzanie kontekstu jest `unchecked` , chyba że czynniki zewnętrzne, (takie jak kompilator przełączniki i konfiguracji środowiska wykonywania) wymagają `checked` oceny.

Wyrażenia stałe (wyrażenia, które mogą być w pełni obliczane w czasie kompilacji), przepełnienie domyślne sprawdzanie kontekstu jest zawsze `checked`. Chyba, że wyrażenie stałe jest jawnie umieszczona na `unchecked` kontekstu, przepełnienia, które występują podczas kompilacji obliczenia wyrażenia zawsze powodują błędy czasu kompilacji.

Treść funkcją anonimową nie mają wpływu `checked` lub `unchecked` kontekstów, w których występuje funkcja anonimowa.

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
są zgłaszane żadne błędy kompilacji, ponieważ żadna z wyrażeń nie mogą być obliczane w czasie kompilacji. W czasie wykonywania `F` metoda zgłasza wyjątek `System.OverflowException`i `G` metoda zwraca-727379968 (niższe 32 bity wyniku spoza zakresu). Zachowanie `H` metoda zależy od przepełnienie domyślne, sprawdzając kontekst dla kompilacji, ale jest taki sam jak `F` lub taka sama jak `G`.

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
przepełnienia, występujące podczas obliczania wyrażenia stałe w `F` i `H` powodują błędy czasu kompilacji, należy podać, ponieważ wyrażenia są obliczane w `checked` kontekstu. Również występuje przepełnienie podczas obliczania wyrażenia stałe w `G`, ale ponieważ obliczanie odbywa się w `unchecked` kontekstu, przeciążenia nie został zgłoszony.

`checked` i `unchecked` operatory wpływ tylko na przepełnienie, sprawdzając kontekst dla tych operacji, które są w formie tekstu zawarte w "`(`"i"`)`" tokenów. Operatory nie mają wpływu na elementach członkowskich funkcji, które są wywoływane w wyniku obliczenia wyrażenia zawarte. W przykładzie
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
Korzystanie z `checked` w `F` nie ma wpływu na ocenę `x * y` w `Multiply`, więc `x * y` jest oceniany w przepełnienie domyślne sprawdzanie kontekstu.

`unchecked` Operator jest wygodne, podczas zapisywania stałe podpisanych typów całkowitych w formacie szesnastkowym. Na przykład:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Stałe szesnastkowe powyżej są typu `uint`. Ponieważ stałe są poza `int` zakresu bez `unchecked` operatorów, rzutowania na `int` dałby w efekcie błędy czasu kompilacji.

`checked` i `unchecked` operatory i instrukcje umożliwiają deweloperom kontrolować niektóre aspekty niektórych obliczeń numerycznych. Zachowanie niektóre operatory numeryczne zależy jednak od tego, jaki ich operandami typów danych. Na przykład, mnożenie dwóch miejsc po przecinku zawsze powoduje wyjątek przy przepełnieniu nawet w sposób jawny `unchecked` konstruowania. Podobnie, dwa pomnożenie liczby zmiennoprzecinkowe nigdy nie wyników wyjątek przy przepełnieniu nawet w sposób jawny `checked` konstruowania. Ponadto inne operatory nigdy nie ma wpływ tryb sprawdzania, czy domyślne lub jawny.

### <a name="default-value-expressions"></a>Wyrażenia wartości domyślnych

Wyrażenie wartości domyślnej jest używana do uzyskiwania wartości domyślne ([wartości domyślne](variables.md#default-values)) typu. Zazwyczaj wyrażenie wartości domyślnej jest używana dla parametrów typu, ponieważ może być znane nie, jeśli parametr typu jest typem wartości lub typem referencyjnym. (Nie istnieje konwersja z `null` literału z parametrem typu, chyba że parametr typu jest znany jako typ odwołania.)

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Jeśli *typu* w *default_value_expression* obliczane w czasie wykonywania do typu odwołania, wynik jest `null` konwertowane do tego typu. Jeśli *typu* w *default_value_expression* ocenia w czasie wykonywania na typ wartości, wynik jest *value_type*jego wartość domyślna ([domyślne Konstruktory](types.md#default-constructors)).

A *default_value_expression* jest wyrażeniem stałym ([wyrażeń stałych](expressions.md#constant-expressions)) Jeśli typ jest typem referencyjnym lub parametr typu, który jest znany jako typu odwołania ([parametr typu ograniczenia](classes.md#type-parameter-constraints)). Ponadto *default_value_expression* jest wyrażeniem stałym, jeśli typ jest jednym z następujących typów wartości: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, lub dowolny typ wyliczenia.


### <a name="nameof-expressions"></a>Wyrażenie Nameof

A *nameof_expression* służy do uzyskiwania nazwy jednostki programu jako ciąg stałej.

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

Gramatycznie rzecz biorąc, *named_entity* operand jest zawsze wyrażenia. Ponieważ `nameof` nie jest zastrzeżonym słowem kluczowym wyrażenie nameof ma zawsze wartość składniowo niejednoznaczne za pomocą wywołania prostą nazwę `nameof`. Ze względu na zgodność, jeśli wyszukiwanie nazw ([proste nazwy](expressions.md#simple-names)) o nazwie `nameof` zakończy się powodzeniem, wyrażenie jest traktowane *invocation_expression* — niezależnie od tego, czy jest wywołanie prawne. W przeciwnym razie jest *nameof_expression*.

Znaczenie *named_entity* z *nameof_expression* jest znaczenie go jako wyrażenie; czyli albo jako *simple_name*, *base_access*  lub *member_access*. Jednak gdy wyszukiwanie opisany w [proste nazwy](expressions.md#simple-names) i [dostęp do elementu członkowskiego](expressions.md#member-access) powoduje błąd, ponieważ znaleziono składową wystąpienia w ramach kontekstu statycznego *nameof_expression*powoduje nie takich błąd.

Jest to błąd czasu kompilacji dla *named_entity* wyznaczanie grupy metod, aby *type_argument_list*. Jest to błąd czasu kompilacji dla *named_entity_target* typ `dynamic`.

A *nameof_expression* jest wyrażeniem stałym typu `string`, nie ma zastosowania w czasie wykonywania. W szczególności jego *named_entity* nie jest oceniany i jest ignorowane dla celów analizy asercję określonego przypisania ([ogólne reguły dotyczące wyrażeń prostych](variables.md#general-rules-for-simple-expressions)). Jej wartość jest ostatni identyfikator *named_entity* przed końcowym znakiem opcjonalne *type_argument_list*, przekształcone w następujący sposób:

* Prefiks "`@`", jeśli używany, zostanie usunięty.
* Każdy *unicode_escape_sequence* jest przekształcana na jego odpowiedniego znaku Unicode.
* Wszelkie *formatting_characters* są usuwane.

Są one stosowane przekształcenia [identyfikatory](lexical-structure.md#identifiers) podczas testowania równości pomiędzy identyfikatorów.

TODO: przykłady

### <a name="anonymous-method-expressions"></a>Wyrażenia metody anonimowej

*Anonymous_method_expression* jest jeden z dwóch sposobów definiowania funkcja anonimowa. Te ustawienia zostały opisane dalej w [wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions).

## <a name="unary-operators"></a>Operatory jednoargumentowe

`?`, `+`, `-`, `!`, `~`, `++`, `--`, Multiemisji, i `await` operatory są nazywane operatorami jednoargumentowymi.

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

Jeśli argument operacji *unary_expression* ma typ kompilacji `dynamic`, dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)). W tym przypadku typ czasu kompilacji *unary_expression* jest `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania za pomocą typu run-time operandu.

### <a name="null-conditional-operator"></a>Operatorów warunkowych działających z wartością null

Operatorów warunkowych działających z wartością null dotyczy listy operacji argumentem operacji tylko wtedy, gdy ten argument jest różna od null. W przeciwnym razie wynik zastosowania operatora jest `null`.

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

Lista operacji mogą obejmować dostęp do elementu członkowskiego i operacje dostępu do elementu, (które mogą być warunkowe null), a także wywołania.

Na przykład, wyrażenie `a.b?[0]?.c()` jest *null_conditional_expression* z *primary_expression* `a.b` i *null_conditional_operations* `?[0]` (dostęp do elementu warunkowe null), `?.c` (dostęp do składowej warunkowe null) i `()` (wywołanie).

Dla *null_conditional_expression* `E` z *primary_expression* `P`systemowi `E0` być wyrażeniem uzyskane w formie tekstu, usuwając wiodących `?`z każdego *null_conditional_operations* z `E` mieć jeden. Model `E0` jest wyrażeniem, które zostanie obliczone, jeśli żadne sprawdzanie wartości null, reprezentowane przez `?`uważają, że s `null`.

Ponadto poinformuj `E1` być wyrażeniem uzyskane w formie tekstu, usuwając wiodące `?` z tylko pierwszy *null_conditional_operations* w `E`. Może to prowadzić do *wyrażenia podstawowe* (jeśli istnieje tylko jeden `?`) lub do innego *null_conditional_expression*.

Na przykład jeśli `E` jest wyrażeniem `a.b?[0]?.c()`, następnie `E0` jest wyrażeniem `a.b[0].c()` i `E1` jest wyrażeniem `a.b[0]?.c()`.

Jeśli `E0` zostanie sklasyfikowany jako nothing, następnie `E` zostanie sklasyfikowany jako nothing. W przeciwnym razie E jest klasyfikowany jako wartość.

`E0` i `E1` służą do określania znaczenie `E`:

*  Jeśli `E` występuje jako *statement_expression* znaczenie `E` jest taka sama jak instrukcji

   ```csharp
   if ((object)P != null) E1;
   ```

   z tą różnicą, że P jest oceniane tylko raz.

*  W przeciwnym razie, jeśli `E0` zostanie sklasyfikowany jako nic nie wystąpi błąd kompilacji.

*  W przeciwnym razie umożliwiają `T0` typem `E0`.

   *  Jeśli `T0` jest parametrem typu, który nie jest znany jako typem referencyjnym lub typem wartościowym niebędącym występuje błąd kompilacji.

   *  Jeśli `T0` jest typem wartości niedopuszczającym wartości, a następnie typ `E` jest `T0?`oraz na znaczenie właściwości `E` jest taka sama jak

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      z tą różnicą, że `P` jest oceniane tylko raz.

   *  W przeciwnym razie typ E jest T0 i znaczenie E jest taka sama jak

      ```csharp
      ((object)P == null) ? null : E1
      ```

      z tą różnicą, że `P` jest oceniane tylko raz.

Jeśli `E1` sama *null_conditional_expression*, następnie te reguły są stosowane ponownie zagnieżdżania testów dla `null` aż nie wystąpią żadnych dalszych `?`firmy, i w dół do końca została zmniejszona wyrażenie wyrażenia podstawowe `E0`.

Na przykład jeśli wyrażenie `a.b?[0]?.c()` występuje jako instrukcja wyrażenie, tak jak w instrukcji:
```csharp
a.b?[0]?.c();
```
jego znaczenie jest równoważne:
```csharp
if (a.b != null) a.b[0]?.c();
```
co ponownie odpowiada:
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
Z tą różnicą, że `a.b` i `a.b[0]` są oceniane tylko raz.

Jeśli pojawi się w kontekście, gdy jej wartość jest używana, w:
```csharp
var x = a.b?[0]?.c();
```
i przy założeniu, że typ końcowe wywołanie nie jest typem wartości niedopuszczającym wartości, jego znaczenie jest równoważne:
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
Z tą różnicą, że `a.b` i `a.b[0]` są oceniane tylko raz.

#### <a name="null-conditional-expressions-as-projection-initializers"></a>Wyrażenia warunkowe null jako inicjatory projekcji

Wyrażenie warunkowe null jest dozwolony tylko jako *member_declarator* w *anonymous_object_creation_expression* ([wyrażenia tworzenia obiektu anonimowego](expressions.md#anonymous-object-creation-expressions)) Jeśli kończy się ona dostęp do elementu członkowskiego (opcjonalnie warunkowe null). Gramatycznie ten wymóg może być wyrażona jako:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Jest to szczególny przypadek gramatyka dla *null_conditional_expression* powyżej. Produkcji dla *member_declarator* w [wyrażenia tworzenia obiektu anonimowego](expressions.md#anonymous-object-creation-expressions) następnie zawiera tylko *null_conditional_member_access*.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>Wyrażenia warunkowe null jako wyrażenia instrukcji

Wyrażenie warunkowe null jest dozwolony tylko jako *statement_expression* ([instrukcje wyrażeń](statements.md#expression-statements)), jeśli kończy się je za pomocą wywołania. Gramatycznie ten wymóg może być wyrażona jako:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Jest to szczególny przypadek gramatyka dla *null_conditional_expression* powyżej. Produkcji dla *statement_expression* w [instrukcje wyrażeń](statements.md#expression-statements) następnie zawiera tylko *null_conditional_invocation_expression*.


### <a name="unary-plus-operator"></a>Jednoargumentowy operator plus

Dla operacji formularza `+x`, Rozpoznanie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Operand jest konwertowany na typ parametru operatora, a typ wyniku jest typem zwracanym operatora. Wstępnie zdefiniowane jednoargumentowe plus operatory są następujące:

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

Dla każdej z tych operatorów wynik jest po prostu wartość operandu.

### <a name="unary-minus-operator"></a>Jednoargumentowy operator minus

Dla operacji formularza `-x`, Rozpoznanie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Operand jest konwertowany na typ parametru operatora, a typ wyniku jest typem zwracanym operatora. Operatory negacji wstępnie zdefiniowane są:

*  Liczba całkowita negacji:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   Wynik jest obliczana przez odjęcie `x` od zera. Jeśli wartość elementu `x` jest najmniejszą wartość stałego typu operand (-2 ^ 31 `int` lub -2 ^ 63 dla `long`), następnie matematyczne negację `x` nie jest reprezentowanych w ramach typu operand. Jeśli ten problem wystąpi w ramach `checked` kontekstu, `System.OverflowException` zgłaszany; jeśli jego wystąpieniu w ciągu `unchecked` kontekstu, wynikiem jest wartość operandu i przeciążenia nie został zgłoszony.

   Jeśli argument operator negacji jest typu `uint`, jest konwertowany na typ `long`, a typ wyniku jest `long`. Wyjątek stanowi regułę, która pozwala na `int` wartość -2147483648 (-2 ^ 31) do zapisania jako dziesiętna liczba całkowita literału ([literały całkowite](lexical-structure.md#integer-literals)).

   Jeśli argument operator negacji jest typu `ulong`, wystąpi błąd kompilacji. Wyjątek stanowi regułę, która pozwala na `long` wartość wartość -9223372036854775808 (-2 ^ 63) do zapisania jako dziesiętna liczba całkowita literału ([literały całkowite](lexical-structure.md#integer-literals)).

*  Zmiennoprzecinkowe negacji:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   Wynik jest wartością `x` przy użyciu konta, jego odwrócony. Jeśli `x` jest wartością typu NaN, wynik jest również wartością typu NaN.

*  Negacja dziesiętne:

   ```csharp
   decimal operator -(decimal x);
   ```

   Wynik jest obliczana przez odjęcie `x` od zera. Negacja dziesiętny jest równoważne użyciu Jednoargumentowy operator typu odejmowania `System.Decimal`.

### <a name="logical-negation-operator"></a>Operator logiczny negacji

Dla operacji formularza `!x`, Rozpoznanie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Operand jest konwertowany na typ parametru operatora, a typ wyniku jest typem zwracanym operatora. Istnieje tylko jeden operator negacji logicznej wstępnie zdefiniowane:
```csharp
bool operator !(bool x);
```

Ten operator oblicza Negacja logiczna operandu: Jeśli argument jest `true`, wynik jest `false`. Jeśli argument jest `false`, wynik jest `true`.

### <a name="bitwise-complement-operator"></a>Operator dopełnienia bitowego

Dla operacji formularza `~x`, Rozpoznanie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Operand jest konwertowany na typ parametru operatora, a typ wyniku jest typem zwracanym operatora. Operatory wstępnie zdefiniowanych dopełnienia bitowego to:
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

Dla każdej z tych operatorów, wynik operacji jest uzupełnienie bitowe `x`.

Każdy typ wyliczenia `E` niejawnie udostępnia następujące operator dopełnienia bitowego:

```csharp
E operator ~(E x);
```

Wynik obliczania wartości `~x`, gdzie `x` to wyrażenie typu wyliczeniowego `E` z typu podstawowego `U`, jest dokładnie taka sama jak ocena `(E)(~(U)x)`, z tą różnicą, że konwersja `E` jest zawsze jest wykonywane jako if w `unchecked` kontekstu ([operatory checked i unchecked](expressions.md#the-checked-and-unchecked-operators)).

### <a name="prefix-increment-and-decrement-operators"></a>Przedrostkowe i operatory dekrementacji

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

Argument prefiksu inkrementacji lub dekrementacji operacji musi być wyrażeniem sklasyfikowane jako zmienną, dostęp do właściwości lub indeksatora dostępu. Wynikiem operacji jest wartością tego samego typu jako argumentu operacji.

Jeśli argument prefiksu Zwiększ operacja dekrementacji jest właściwością lub dostęp indeksatora, właściwość lub indeksator musi mieć zarówno `get` i `set` metody dostępu. Jeśli nie jest to możliwe, występuje błąd wiązania.

Rozpoznanie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Wstępnie zdefiniowane `++` i `--` operatory istnieją dla następujących typów: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`i dowolnego typu wyliczeniowego. Wstępnie zdefiniowane `++` operatory zwracają wartość, o których produkowane przez dodanie 1 argument i wstępnie zdefiniowane `--` operatory zwracają wartość, o których produkowane przez odjęcie ilości 1 z operandu. W `checked` kontekstu, jeśli wynik to dodawania lub odejmowania jest spoza zakresu typu wyniku, a typ wyniku jest typu całkowitoliczbowego lub typu wyliczeniowego `System.OverflowException` zgłaszany.

Przetwarzanie środowiska wykonawczego przyrostu prefiks lub zmniejszanie operacji formularza `++x` lub `--x` składa się z następujących czynności:

*   Jeśli `x` zostanie sklasyfikowany jako zmienną:
    * `x` jest oceniany w celu utworzenia zmiennej.
    * Wybrane operator jest wywoływany z wartością `x` jako argumentem.
    * Wartość zwracana przez operator znajduje się w lokalizacji określonej przez ocenę `x`.
    * Wartość zwracana przez operator staje się wynik operacji.
*   Jeśli `x` zostanie sklasyfikowany jako właściwość lub indeksator dostępu:
    * Wyrażenia wystąpienia (Jeśli `x` nie jest `static`) i listą argumentów (Jeśli `x` jest dostęp indeksatora) skojarzony z `x` są oceniane, a wyniki są używane w kolejnych żadaniach `get` i `set` wywołania metody dostępu.
    * `get` Akcesor `x` zostanie wywołana.
    * Operatora jest wywoływana z wartością zwróconą przez `get` dostępu jako argumentem.
    * `set` Akcesor `x` jest wywoływana z wartością zwróconą przez operatora jako jego `value` argumentu.
    * Wartość zwracana przez operator staje się wynik operacji.

`++` i `--` Operatorzy pomocy technicznej przyrostkowe notacji ([przyrostka inkrementacji i dekrementacji operatory](expressions.md#postfix-increment-and-decrement-operators)). Zazwyczaj wynikiem `x++` lub `x--` jest wartością `x` przed wykonaniem operacji, podczas gdy wynik `++x` lub `--x` jest wartością `x` po wykonaniu operacji. W obu przypadkach `x` ma taką samą wartość po wykonaniu operacji.

`operator++` Lub `operator--` implementacja może być wywoływany przy użyciu notacji przyrostka lub przedrostka. Nie jest możliwe operator oddzielne implementacje dla dwóch notacji.

### <a name="cast-expressions"></a>Wyrażenia CAST

A *cast_expression* jest używana do jawnie konwersji wyrażenia do danego typu.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

A *cast_expression* formularza `(T)E`, gdzie `T` jest *typu* i `E` jest *unary_expression*, wykonuje jawnego Konwersja ([jawne konwersje](conversions.md#explicit-conversions)) wartości `E` na typ `T`. Jeśli istnieje nie jawna konwersja z `E` do `T`, występuje błąd wiązania. W przeciwnym razie wynikiem jest wartość, o których produkowane przez jawną konwersję. Wynik jest zawsze klasyfikowane jako wartości, nawet jeśli `E` wskazuje zmienną.

Gramatyka dla *cast_expression* prowadzi do niektórych niejasności składni. Na przykład, wyrażenie `(x)-y` można albo być interpretowane jako *cast_expression* (rzutowanie typu `-y` na typ `x`) lub jako *additive_expression* w połączeniu z *parenthesized_expression* (które oblicza wartość `x - y)`.

Aby rozwiązać *cast_expression* niejasności, następująca reguła istnieje: Sekwencji jednego lub więcej *tokenu*s ([biały znak](lexical-structure.md#white-space)) ujęta w nawiasy jest traktowana jako początek *cast_expression* tylko wtedy, gdy spełnione są co najmniej jedną z następujących:

*  Sekwencja tokenów jest poprawny gramatyka dla *typu*, ale nie dla *wyrażenie*.
*  Sekwencja tokenów jest poprawny gramatyka dla *typu*, i tokenu bezpośrednio po nawiasie zamykającym jest tokenem "`~`", token "`!`", token "`(`",  *Identyfikator* ([sekwencje ucieczki znaków Unicode](lexical-structure.md#unicode-character-escape-sequences)), *literału* ([literały](lexical-structure.md#literals)), lub dowolnego *— słowo kluczowe*([Słowa kluczowe](lexical-structure.md#keywords)) z wyjątkiem `as` i `is`.

Termin "gramatyka poprawne" powyżej oznacza tylko, że sekwencja tokeny muszą być zgodne do konkretnego gramatyczne środowiska produkcyjnego. W szczególności nie uważa rzeczywiste znaczenie żadnych składowych identyfikatorów. Na przykład jeśli `x` i `y` to identyfikatory, następnie `x.y` jest poprawny gramatyka dla typu, nawet wtedy, gdy `x.y` rzeczywiście nie określa typu.

Z reguły uściślania następuje, jeśli `x` i `y` to identyfikatory, `(x)y`, `(x)(y)`, i `(x)(-y)` są *cast_expression*s, ale `(x)-y` nie jest dostępna, nawet wtedy, gdy `x` identyfikuje typ. Jednak jeśli `x` jest słowem kluczowym, który identyfikuje wstępnie zdefiniowany typ (takich jak `int`), wówczas są wszystkie cztery formularze *cast_expression*s (ponieważ słów kluczowych nie można prawdopodobnie wyrażenia przez siebie).

### <a name="await-expressions"></a>Wyrażeń await

Operator "await" jest używany do zawieszenia oceny otaczającej funkcji asynchronicznej przed ukończeniem operacji asynchronicznej, reprezentowane przez operand.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

*Await_expression* jest dozwolona tylko w treści funkcji asynchronicznej ([Iteratory](classes.md#iterators)). W najbliższej otaczającej funkcji asynchronicznej *await_expression* nie może wystąpić w następujących miejscach:

*  Wewnątrz funkcji anonimowych zagnieżdżonych (innych niż asynchroniczny)
*  Wewnątrz bloku *lock_statement*
*  W niebezpiecznym kontekście

Należy pamiętać, że *await_expression* nie może wystąpić w większości miejsc w ramach *query_expression*, ponieważ te składniowo są przekształcane do użycia innego niż asynchronicznych wyrażeń lambda.

Wewnątrz funkcji asynchronicznej `await` nie można użyć jako identyfikatora. Nie ma związku z tym żadnych niejednoznaczności składniowe między wyrażeń await i wyrażenia różnych, dotyczące identyfikatorów. Poza funkcje asynchroniczne `await` działa jak normalne identyfikatora.

Argument operacji *await_expression* nosi nazwę ***zadań***. Reprezentuje operację asynchroniczną, która może być lub może nie być ukończone w czasie *await_expression* jest oceniany. Operator "await" ma na celu wstrzymał wykonywanie otaczającej funkcji asynchronicznej, aż oczekiwane zadanie zostało ukończone, a następnie uzyskać jego wynik.

#### <a name="awaitable-expressions"></a>Oczekujący wyrażeń

Zadanie wykonywania wyrażenia oczekiwania musi być ***oczekujący***. Wyrażenie `t` jest oczekujący, jeśli zawiera jedną z następujących czynności:

*  `t` typu czasu kompilacji `dynamic`
*  `t` zawiera dostępnej metody rozszerzenia lub wystąpienia, o nazwie `GetAwaiter` żadnych parametrów i bez parametrów typu i zwracany typ `A` dla którego pomieścić wszystkie z następujących czynności:
   * `A` implementuje interfejs `System.Runtime.CompilerServices.INotifyCompletion` (nazywanym dalej `INotifyCompletion` dla zwięzłości)
   * `A` ma właściwość wystąpienia dostępny, można odczytać `IsCompleted` typu `bool`
   * `A` ma dostępną metodą wystąpienia `GetResult` bez parametrów i bez parametrów typu

Celem `GetAwaiter` metody jest uzyskanie ***awaiter*** dla zadania. Typ `A` nosi nazwę ***typu awaiter*** wyrażenia await.

Celem `IsCompleted` właściwość ma na celu określenie, jeśli zadanie zostało już ukończone. Jeśli tak, nie ma potrzeby wstrzymania oceny.

Celem `INotifyCompletion.OnCompleted` metodą jest utworzenie "kontynuacja" do zadania; czyli delegata (typu `System.Action`), zostanie wywołana po ukończeniu zadania.

Celem `GetResult` metodą jest można uzyskać wynik zadania po jego zakończeniu. Ten wynik może być wykonana pomyślnie, prawdopodobnie z wartością wynik lub może być wyjątek, który jest generowany przez `GetResult` metody.

#### <a name="classification-of-await-expressions"></a>Klasyfikacja wyrażeń await

Wyrażenie `await t` klasyfikowany jest taki sam sposób jak wyrażenie `(t).GetAwaiter().GetResult()`. W związku z tym Jeśli zwracany typ metody `GetResult` jest `void`, *await_expression* zostanie sklasyfikowany jako nothing. Jeśli ma on zwracany typ inny niż void `T`, *await_expression* zostanie sklasyfikowany jako wartość typu `T`.

#### <a name="runtime-evaluation-of-await-expressions"></a>Środowisko uruchomieniowe oceny wyrażeń await

W czasie wykonywania, wyrażenie `await t` jest obliczane w następujący sposób:

*  Awaiter `a` uzyskuje się przez obliczenie wyrażenia `(t).GetAwaiter()`.
*  A `bool` `b` uzyskuje się przez obliczenie wyrażenia `(a).IsCompleted`.
*  Jeśli `b` jest `false` , a następnie oceny jest zależna od tego, czy `a` implementuje interfejs `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (nazywanym dalej `ICriticalNotifyCompletion` dla zwięzłości). To sprawdzenie odbywa się w czasie; wiązania czyli w czasie wykonywania przypadku `a` ma typów w czasie kompilacji `dynamic`i w czasie kompilacji, w przeciwnym razie. Pozwól `r` oznaczają delegata wznowienie ([Iteratory](classes.md#iterators)):
    * Jeśli `a` nie implementuje `ICriticalNotifyCompletion`, następnie wyrażenie `(a as (INotifyCompletion)).OnCompleted(r)` jest oceniany.
    * Jeśli `a` zaimplementować `ICriticalNotifyCompletion`, następnie wyrażenie `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` jest oceniany.
    * Ocena jest następnie wstrzymana, a zwróceniem sterowania do bieżącego obiektu wywołującego funkcji asynchronicznej.
*  Albo natychmiast po (Jeśli `b` został `true`), lub nowszych wywołanie delegata wznawiania (Jeśli `b` został `false`), wyrażenie `(a).GetResult()` jest oceniany. Jeśli wartość jest zwracana, ta wartość jest wynikiem *await_expression*. W przeciwnym razie wynikiem jest nothing.

Awaiter implementację metody interfejsu `INotifyCompletion.OnCompleted` i `ICriticalNotifyCompletion.UnsafeOnCompleted` powinno spowodować delegata `r` wywoływanej co najwyżej jeden raz. W przeciwnym razie zachowanie otaczającej funkcji asynchronicznej jest niezdefiniowane.

## <a name="arithmetic-operators"></a>Operatory arytmetyczne

`*`, `/`, `%`, `+`, I `-` operatory są nazywane operatorów arytmetycznych.

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

Jeśli argument operacji jest operator arytmetyczny ma typ kompilacji `dynamic`, a następnie wyrażenie jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)). W tym przypadku jest to typ kompilacji wyrażenia `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania za pomocą typu run-time tych operandów, które mają typ kompilacji `dynamic`.

### <a name="multiplication-operator"></a>Operator mnożenia

Dla operacji formularza `x * y`, Rozpoznanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.

Operatory mnożenia wstępnie zdefiniowane są wymienione poniżej. Wszystkie operatory Oblicz iloczyn `x` i `y`.

*  Liczba całkowita mnożenia:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   W `checked` kontekstu, jeśli produkt jest spoza zakresu typu wyniku, `System.OverflowException` zgłaszany. W `unchecked` kontekstu, przepełnienia nie są zgłaszane i wszystkie znaczące bity wyższego rzędu spoza zakresu typu wyniku są odrzucane.


*  Zmiennoprzecinkowe mnożenia:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   Produkt jest obliczany zgodnie z regułami IEEE 754 arytmetyczne. W poniższej tabeli wymieniono wyniki wszystkich możliwych kombinacji wartości skończoną wartość różną od zera, zer, nieskończoności i NaN firmy. W tabeli `x` i `y` wartości skończoną dodatnią. `z` jest wynikiem `x * y`. Jeśli wynik jest za duża dla typu miejsca docelowego `z` jest nieskończoność. Jeśli wynik jest za mały dla typu miejsca docelowego `z` wynosi zero.

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | + y   | -y   | +0  | -0  | + inf | -inf | NaN | 
   | + x   | + z   | -z   | +0  | -0  | + inf | -inf | NaN | 
   | -x   | -z   | + z   | -0  | +0  | -inf | + inf | NaN | 
   | +0   | +0   | -0   | +0  | -0  | NaN  | NaN  | NaN | 
   | -0   | -0   | +0   | -0  | +0  | NaN  | NaN  | NaN | 
   | + inf | + inf | -inf | NaN | NaN | + inf | -inf | NaN | 
   | -inf | -inf | + inf | NaN | NaN | -inf | + inf | NaN | 
   | NaN  | NaN  | NaN  | NaN | NaN | NaN  | NaN  | NaN | 

*  Mnożenie dziesiętne:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Jeśli wartość wynikowa jest zbyt duży do reprezentowania w `decimal` formacie `System.OverflowException` zgłaszany. Jeśli wartość wyniku jest zbyt mała do reprezentowania w `decimal` formatu, wynik wynosi zero. Skala wyniku przed dowolnego zaokrąglanie jest suma skale dwóch argumentów operacji.

   Mnożenie dziesiętny jest równoważne użyciu operator mnożenia typu `System.Decimal`.


### <a name="division-operator"></a>Operator dzielenia

Dla operacji formularza `x / y`, Rozpoznanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.

Poniżej wymieniono operatory dzielenia wstępnie zdefiniowane. Wszystkie operatory obliczeniowe iloraz `x` i `y`.

*  Dzielenie liczby całkowitej:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Jeśli wartość prawy operand jest równa zero, `System.DivideByZeroException` zgłaszany.

   Podział zostanie zaokrąglony wynik w kierunku zera. Dlatego wartość bezwzględna wynik jest największa możliwa liczba całkowita, która jest większa niż wartość bezwzględna ilorazu dwóch argumentów operacji. Wynik jest raz lub zero dodatnie dwa operandy mają ten sam znak i zero lub ujemna, gdy dwa operandy przeciwne znaków.

   Jeśli lewy operand jest najmniejszą stałego `int` lub `long` wartość i prawy operand jest `-1`, występuje przepełnienie. W `checked` kontekstu, powoduje to, że `System.ArithmeticException` (lub jego podklasę) zostanie wygenerowany. W `unchecked` kontekstu, jest zdefiniowane w implementacji czy `System.ArithmeticException` (lub jego podklasę) jest zgłaszany lub przeciążenia niezgłoszonych, Lewy argument operacji jest wartością wynikową.

*  Dzielenie zmiennoprzecinkowe:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   Iloraz jest obliczane zgodnie z regułami IEEE 754 arytmetyczne. W poniższej tabeli wymieniono wyniki wszystkich możliwych kombinacji wartości skończoną wartość różną od zera, zer, nieskończoności i NaN firmy. W tabeli `x` i `y` wartości skończoną dodatnią. `z` jest wynikiem `x / y`. Jeśli wynik jest za duża dla typu miejsca docelowego `z` jest nieskończoność. Jeśli wynik jest za mały dla typu miejsca docelowego `z` wynosi zero.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | + y   | -y   | +0   | -0   | + inf | -inf | NaN  | 
   | + x   | + z   | -z   | + inf | -inf | +0   | -0   | NaN  | 
   | -x   | -z   | + z   | -inf | + inf | -0   | +0   | NaN  | 
   | +0   | +0   | -0   | NaN  | NaN  | +0   | -0   | NaN  | 
   | -0   | -0   | +0   | NaN  | NaN  | -0   | +0   | NaN  | 
   | + inf | + inf | -inf | + inf | -inf | NaN  | NaN  | NaN  | 
   | -inf | -inf | + inf | -inf | + inf | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Dzielenie dziesiętne:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Jeśli wartość prawy operand jest równa zero, `System.DivideByZeroException` zgłaszany. Jeśli wartość wynikowa jest zbyt duży do reprezentowania w `decimal` formacie `System.OverflowException` zgłaszany. Jeśli wartość wyniku jest zbyt mała do reprezentowania w `decimal` formatu, wynik wynosi zero. Skalowanie w wyniku jest najmniejsza skalowania, który zachowa wynik równa najbliższej dziesiętnej wartość true wyniku matematyczne.

   Dzielenie dziesiętne jest równoważne użyciu operator dzielenia typu `System.Decimal`.


### <a name="remainder-operator"></a>Operator reszty

Dla operacji formularza `x % y`, Rozpoznanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.

Poniżej wymieniono operatory resztę wstępnie zdefiniowane. Wszystkie operatory obliczeniowe w pozostałej części podział `x` i `y`.

*  Pozostała liczba całkowita:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   Wynik `x % y` wartość jest generowany przez `x - (x / y) * y`. Jeśli `y` wynosi zero, `System.DivideByZeroException` zgłaszany.

   Jeśli lewy operand jest najmniejszym `int` lub `long` wartość i prawy operand jest `-1`, `System.OverflowException` zgłaszany. W żadnym przypadku nie jest `x % y` zgłosić wyjątek, gdzie `x / y` nie zgłasza wyjątku.

*  Zmiennoprzecinkową resztę:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   W poniższej tabeli wymieniono wyniki wszystkich możliwych kombinacji wartości skończoną wartość różną od zera, zer, nieskończoności i NaN firmy. W tabeli `x` i `y` wartości skończoną dodatnią. `z` jest wynikiem `x % y` i jest obliczana jako `x - n * y`, gdzie `n` jest największa możliwa liczba całkowita, która jest mniejsza niż lub równa `x / y`. Ta metoda obliczeniowych resztę jest odpowiednikiem używanym dla liczby całkowitej argumentów, ale różni się od definicji IEEE 754 (w którym `n` jest liczbą całkowitą najbliższą `x / y`).

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | + y   | -y   | +0   | -0   | + inf | -inf | NaN  | 
   | + x   | + z   | + z   | NaN  | NaN  | x    | x    | NaN  | 
   | -x   | -z   | -z   | NaN  | NaN  | -x   | -x   | NaN  | 
   | +0   | +0   | +0   | NaN  | NaN  | +0   | +0   | NaN  | 
   | -0   | -0   | -0   | NaN  | NaN  | -0   | -0   | NaN  | 
   | + inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | -inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Reszta dziesiętne:

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   Jeśli wartość prawy operand jest równa zero, `System.DivideByZeroException` zgłaszany. Skala wyniku przed dowolnego zaokrąglanie jest większej skali dwóch argumentów operacji, a znak wyniku, jeśli różna od zera, jest taki sam, jak w przypadku `x`.

   Dziesiętna reszta jest równoważne użyciu operator reszty typu `System.Decimal`.


### <a name="addition-operator"></a>Operator dodawania

Dla operacji formularza `x + y`, Rozpoznanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.

Operatory dodawania wstępnie zdefiniowane są wymienione poniżej. W przypadku typów liczbowych i wyliczenie operatory dodawania wstępnie zdefiniowanych Obliczanie sumy dwóch argumentów operacji. Gdy jeden lub obydwa operandy są typu ciąg, operatory dodawania wstępnie zdefiniowanych łączenie ciąg reprezentujący argumenty operacji.

*  Dodanie liczby całkowitej:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   W `checked` kontekstu, jeśli suma jest spoza zakresu typu wyniku, `System.OverflowException` zgłaszany. W `unchecked` kontekstu, przepełnienia nie są zgłaszane i wszystkie znaczące bity wyższego rzędu spoza zakresu typu wyniku są odrzucane.

*  Dodanie zmiennoprzecinkowa:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   Suma jest obliczana zgodnie z regułami IEEE 754 arytmetyczne. W poniższej tabeli wymieniono wyniki wszystkich możliwych kombinacji wartości skończoną wartość różną od zera, zer, nieskończoności i NaN firmy. W tabeli `x` i `y` są ograniczone wartości niezerowych i `z` jest wynikiem `x + y`. Jeśli `x` i `y` mieć tej samej wielkości, ale odwrotnych znaków, `z` jest dodatnia zero. Jeśli `x + y` jest zbyt duży, aby przedstawić w typie docelowym `z` jest nieskończony o ten sam znak co `x + y`.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | t    | +0   | -0   | + inf | -inf | NaN  | 
   | x    | z    | x    | x    | + inf | -inf | NaN  | 
   | +0   | t    | +0   | +0   | + inf | -inf | NaN  | 
   | -0   | t    | +0   | -0   | + inf | -inf | NaN  | 
   | + inf | + inf | + inf | + inf | + inf | NaN  | NaN  | 
   | -inf | -inf | -inf | -inf | NaN  | -inf | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Dodanie dziesiętne:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Jeśli wartość wynikowa jest zbyt duży do reprezentowania w `decimal` formacie `System.OverflowException` zgłaszany. Skala wyniku przed dowolnego zaokrąglanie jest większej skali dwóch argumentów operacji.

   Dodanie dziesiętny jest równoważne użyciu operator dodawania typu `System.Decimal`.

*  Dodanie wyliczenia. Każdy typ wyliczenia niejawnie udostępnia następujące wstępnie zdefiniowane operatorów, gdzie `E` jest typem wyliczeniowym i `U` jest podstawowym typem `E`:

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   W czasie wykonywania tych operatorów są oceniane dokładnie jako `(E)((U)x + (U)y)`.

*  Łączenie ciągów:

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   Taka przeciążona funkcja sprawdza pliku binarnego `+` operator wykonywać ciągów. Jeśli argument ciągów jest `null`, zostanie zastąpiony ciągiem pustym. W przeciwnym razie którykolwiek z argumentów niż ciąg jest konwertowany na jego reprezentację ciągu za pomocą wywołania wirtualnego `ToString` metody dziedziczone z typu `object`. Jeśli `ToString` zwraca `null`, zostanie zastąpiony ciągiem pustym.

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

   Wynik operatora łączenia ciągów jest ciągiem, który składa się ze znaków lewy operand następują znaki prawy operand. Nigdy nie zwraca operatora łączenia ciągów `null` wartość. A `System.OutOfMemoryException` mogą być generowane, jeśli nie ma wystarczającej ilości pamięci do przydzielenia do ciągu wynikowego.

*  Kombinacji delegatów. Każdy typ delegata niejawnie udostępnia następujące wstępnie zdefiniowanego operatora, gdzie `D` jest typ delegata:

   ```csharp
   D operator +(D x, D y);
   ```

   Plik binarny `+` operator wykonuje delegatów, gdy oba operandy są typu delegata, niektóre `D`. (Jeśli operandów delegata różnych typów, występuje błąd wiązania.) Jeśli pierwszy argument jest `null`, wynik operacji jest wartość drugiego operandu (nawet jeśli jest to `null`). W przeciwnym razie, jeśli drugi argument operacji jest `null`, wynik operacji jest wartość pierwszego operandu. W przeciwnym razie wynik operacji jest nowe delegowanie, wystąpienie, gdy wywoływany, wywołuje pierwszy operand, a następnie wywołuje drugiego operandu. Aby uzyskać przykłady delegatów, zobacz [operator odejmowania](expressions.md#subtraction-operator) i [delegować wywołania](delegates.md#delegate-invocation). Ponieważ `System.Delegate` nie jest typem delegowanym `operator`  `+` nie zdefiniowano dla niego.

### <a name="subtraction-operator"></a>Operator odejmowania

Dla operacji formularza `x - y`, Rozpoznanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.

Poniżej wymieniono operatory odejmowania wstępnie zdefiniowane. Operatory wszystkie odjąć `y` z `x`.

*  Liczba całkowita odejmowania:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   W `checked` kontekstu, jeśli różnica jest spoza zakresu typu wyniku, `System.OverflowException` zgłaszany. W `unchecked` kontekstu, przepełnienia nie są zgłaszane i wszystkie znaczące bity wyższego rzędu spoza zakresu typu wyniku są odrzucane.

*  Zmiennoprzecinkowe odejmowania:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   Różnica jest obliczane zgodnie z regułami IEEE 754 arytmetyczne. W poniższej tabeli wymieniono wyniki wszystkich możliwych kombinacji wartości skończoną wartość różną od zera, zer, nieskończoności i NaNs. W tabeli `x` i `y` są ograniczone wartości niezerowych i `z` jest wynikiem `x - y`. Jeśli `x` i `y` są takie same `z` jest dodatnia zero. Jeśli `x - y` jest zbyt duży, aby przedstawić w typie docelowym `z` jest nieskończony o ten sam znak co `x - y`.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   | NaN  | t    | +0   | -0   | + inf | -inf | NaN | 
   | x    | z    | x    | x    | -inf | + inf | NaN | 
   | +0   | -y   | +0   | +0   | -inf | + inf | NaN | 
   | -0   | -y   | -0   | +0   | -inf | + inf | NaN | 
   | + inf | + inf | + inf | + inf | NaN  | + inf | NaN | 
   | -inf | -inf | -inf | -inf | -inf | NaN  | NaN | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN | 

*  Dziesiętna odejmowania:

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   Jeśli wartość wynikowa jest zbyt duży do reprezentowania w `decimal` formacie `System.OverflowException` zgłaszany. Skala wyniku przed dowolnego zaokrąglanie jest większej skali dwóch argumentów operacji.

   Odejmowanie dziesiętny jest równoważne użyciu operator odejmowania typu `System.Decimal`.

*  Wyliczenie odejmowania. Każdy typ wyliczenia niejawnie udostępnia następujące wstępnie zdefiniowanego operatora, gdzie `E` jest typem wyliczeniowym i `U` jest podstawowym typem `E`:

   ```csharp
   U operator -(E x, E y);
   ```

   Ten operator jest oceniane dokładnie jako `(U)((U)x - (U)y)`. Innymi słowy, operator oblicza różnicę między wartości porządkowe `x` i `y`, a typ wyniku jest podstawowym typem wyliczenia.

   ```csharp
   E operator -(E x, U y);
   ```

   Ten operator jest oceniane dokładnie jako `(E)((U)x - y)`. Innymi słowy operator odejmuje wartość z podstawowym typem wyliczenia, reaguje wartością wyliczenia.

*  Delegowanie usuwania. Każdy typ delegata niejawnie udostępnia następujące wstępnie zdefiniowanego operatora, gdzie `D` jest typ delegata:

   ```csharp
   D operator -(D x, D y);
   ```

   Plik binarny `-` operator wykonuje usuwanie delegata, gdy oba operandy są typu delegata, niektóre `D`. Jeśli argumenty operacji typu delegowanego innego, występuje błąd wiązania. Jeśli pierwszy argument jest `null`, wynik operacji jest `null`. W przeciwnym razie, jeśli drugi argument operacji jest `null`, wynik operacji jest wartość pierwszego operandu. W przeciwnym razie oba operandy reprezentują wywołania list ([delegować deklaracje](delegates.md#delegate-declarations)) co najmniej jeden wpis, a wynik jest nową listę wywołania składająca się z pierwszego operandu listy z drugiego operandu wpisy usunięte z podano listę drugi argument operacji jest odpowiednie podlisty ciągłych w pierwszej kolejności.     (Aby określić równości podlisty, odpowiednie pozycje są porównywane jak operator równości delegata ([delegować Operatory równości](expressions.md#delegate-equality-operators)).) W przeciwnym razie wynikiem jest wartość operandu po lewej stronie. Żaden z operandów list jest zmieniany w procesie. Jeśli drugi argument operacji lista odpowiada wielu podlisty ciągłych wpisów na liście pierwszy operand, podlisty pasującego najdalej z prawej strony pozycji ciągłe zostaną usunięte. Jeśli wyniki usuwania w pustej listy, wynik jest `null`. Na przykład:

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

`<<` i `>>` operatory są używane do wykonywania operacji przesunięcia bitowego.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

Jeśli argument operacji *shift_expression* ma typ kompilacji `dynamic`, a następnie wyrażenie jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)). W tym przypadku jest to typ kompilacji wyrażenia `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania za pomocą typu run-time tych operandów, które mają typ kompilacji `dynamic`.

Dla operacji formularza `x << count` lub `x >> count`, Rozpoznanie przeciążenia operatora binarnego ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.

Podczas deklarowania przeciążonego operatora przesunięcia, typ pierwszy operand musi być zawsze, klasy lub struktury, zawierające Deklaracja operatora, a typ drugiego argumentu operacji musi być zawsze `int`.

Operatory przesunięcia wstępnie zdefiniowane są wymienione poniżej.

*  Przesunięcie w lewo:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   `<<` Operatora przesunięcia `x` lewo o liczbę bitów obliczane zgodnie z poniższym opisem.

   Najbardziej znaczących bitów spoza zakresu typu wyniku `x` są odrzucane, pozostałych bitów są przesuwane w lewo i pozycje bitów pusty niskiego rzędu są ustawione na zero.

*  Przesunięcie w prawo:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   `>>` Operatora przesunięcia `x` prawo o liczbę bitów obliczane zgodnie z poniższym opisem.

   Podczas `x` typu `int` lub `long`, bity niskiego rzędu `x` są odrzucane, pozostałe usługi bits zostaną przesunięte w prawo i pozycje bitów pusty wyższego rzędu są ustawione na zero, jeśli `x` jest ujemna i ustaw, jeśli jeden `x` jest ujemna.

   Podczas `x` typu `uint` lub `ulong`, bity niskiego rzędu `x` są odrzucane, pozostałych bitów zostaną przesunięte w prawo i pozycje bitów pusty wyższego rzędu są ustawione na zero.

Dla operatorów wstępnie zdefiniowanej liczby bitów, aby przenieść jest obliczany w następujący sposób:

*  Gdy typ `x` jest `int` lub `uint`, licznik przesunięć jest nadawana przez ostatnie pięć bity `count`. Innymi słowy, licznik przesunięć jest obliczany na podstawie `count & 0x1F`.
*  Gdy typ `x` jest `long` lub `ulong`, licznik przesunięć jest nadawana przez ostatnie sześć bity `count`. Innymi słowy, licznik przesunięć jest obliczany na podstawie `count & 0x3F`.

Jeśli wynikowe licznik przesunięć wynosi zero, operatory przesunięcia po prostu zwraca wartość `x`.

Operacje przesunięcia nigdy nie powodują przepełnienia i działają tak samo w `checked` i `unchecked` kontekstów.

Jeśli lewy operand `>>` operator ma typ całkowity ze znakiem, operator dokonuje arytmetycznego przesunięcia bezpośrednio którym wartość najbardziej znaczący bit (bit znaku) operand jest propagowana do pozycji bitów pusty wyższego rzędu. Jeśli lewy operand `>>` operator jest nieoznaczoną liczbę całkowitą, operator wykonuje Przesunięcie logiczne bezpośrednio polegającego pozycje bitów pusty wyższego rzędu są zawsze ustawione na zero. Do wykonania operacji przeciwny tego wnioskowany z typu argumentu operacji, może służyć ma jawnych rzutowań. Na przykład jeśli `x` jest zmienną typu `int`, operacja `unchecked((int)((uint)x >> y))` wykonuje Przesunięcie logiczne po prawej stronie `x`.

## <a name="relational-and-type-testing-operators"></a>Operatory relacyjne i badania typu

`==`, `!=`, `<`, `>`, `<=`, `>=`, `is` i `as` operatory są nazywane operatory relacyjne i badania typu.

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

`is` Operator jest opisana w [jest operatorem](expressions.md#the-is-operator) i `as` operator jest opisana w [jako operator](expressions.md#the-as-operator).

`==`, `!=`, `<`, `>`, `<=` i `>=` operatory są ***operatory porównania***.

Jeśli argument operacji operatora porównania ma typ kompilacji `dynamic`, a następnie wyrażenie jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)). W tym przypadku jest to typ kompilacji wyrażenia `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania za pomocą typu run-time tych operandów, które mają typ kompilacji `dynamic`.

Dla operacji formularza `x` *op* `y`, gdzie *op* jest operator porównania, funkcja rozpoznawania przeciążeń ([operatora binarnego przeciążenia](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.

Operatory porównania wstępnie zdefiniowane są opisane w poniższych sekcjach. Wszystkie operatory porównania wstępnie zdefiniowanych zwracają wynik o typie `bool`, zgodnie z opisem w poniższej tabeli.


| __Operacja__ | __wynik__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `true` Jeśli `x` jest równa `y`, `false` inaczej                 | 
| `x != y`      | `true` Jeśli `x` nie jest równa `y`, `false` inaczej             | 
| `x < y`       | `true` Jeśli `x` jest mniejsza niż `y`, `false` inaczej                | 
| `x > y`       | `true` Jeśli `x` jest większa niż `y`, `false` inaczej             | 
| `x <= y`      | `true` Jeśli `x` jest mniejsza niż lub równa `y`, `false` inaczej    | 
| `x >= y`      | `true` Jeśli `x` jest większa niż lub równa `y`, `false` inaczej | 

### <a name="integer-comparison-operators"></a>Operatory porównania liczba całkowita

Operatory porównania wstępnie zdefiniowanej liczby całkowitej są:
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

Każda z tych operatorów porównuje wartości liczbowych całkowitą dwa argumenty i zwraca `bool` wartość, która wskazuje, czy określonej relacji jest `true` lub `false`.

### <a name="floating-point-comparison-operators"></a>Operatory porównania zmiennoprzecinkowych

Operatory porównania zmiennoprzecinkowych wstępnie zdefiniowane są:
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

Operatory porównują operandy zgodnie z regułami IEEE 754 standardowe:

*  Jeśli jeden z operandów jest wartością typu NaN, wynik jest `false` dla wszystkich operatorów, z wyjątkiem `!=`, dla których jest wynik `true`. Dla dowolnego dwa operandy `x != y` zawsze daje ten sam wynik jako `!(x == y)`. Jednakże, jeśli jeden lub obydwa operandy są NaN, `<`, `>`, `<=`, i `>=` operatorów nie daje te same wyniki logiczną negację operatora przeciwnego. Na przykład, jeśli jedna z `x` i `y` jest NaN, następnie `x < y` jest `false`, ale `!(x >= y)` jest `true`.
*  Jeśli żaden z operandów nie jest wartością typu NaN, operatory porównują wartości dwa operandy zmiennoprzecinkowych w odniesieniu do porządkowania

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   gdzie `min` i `max` to najmniejsza i największa dodatnią skończoną wartości, które mogą być reprezentowane w danym formacie zmiennoprzecinkowych. Godne uwagi efekty ta kolejność są:
   * Zer ujemny i dodatni, są uznawane za równe.
   * Minus nieskończoności jest uważany za mniej niż inne wartości, ale równa innego ujemna nieskończoność.
   * Nieskończoności dodatniej jest uważany za większy niż inne wartości, ale równa innego nieskończoności dodatniej.

### <a name="decimal-comparison-operators"></a>Operatory porównania dziesiętna

Operatory porównania dziesiętna wstępnie zdefiniowane są:
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

Każda z tych operatorów porównuje wartości liczbowych dwa operandy dziesiętnych i zwraca `bool` wartość, która wskazuje, czy określonej relacji jest `true` lub `false`. Każde porównanie dziesiętny jest równoważne użyciu odpowiednie relacyjnych lub operator równości typu `System.Decimal`.

### <a name="boolean-equality-operators"></a>Operatory logiczne równości

Operatory równości logiczne wstępnie zdefiniowane są:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

Wynik `==` jest `true` Jeśli oba `x` i `y` są `true` lub jeśli oba `x` i `y` są `false`. W przeciwnym razie wynikiem jest `false`.

Wynik `!=` jest `false` Jeśli oba `x` i `y` są `true` lub jeśli oba `x` i `y` są `false`. W przeciwnym razie wynikiem jest `true`. Kiedy operandy są typu `bool`, `!=` operatora daje ten sam wynik jako `^` operatora.

### <a name="enumeration-comparison-operators"></a>Operatory porównania wyliczenia

Każdy typ wyliczenia niejawnie zawiera następujące operatory porównania wstępnie zdefiniowane:
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

Wynik obliczania wartości `x op y`, gdzie `x` i `y` są wyrażeniami typu wyliczeniowego `E` z typu podstawowego `U`, i `op` jest jednym z operatorów porównania, jest dokładnie taka sama jak Ocena `((U)x) op ((U)y)`. Innymi słowy operatory porównania typu wyliczenia po prostu Porównaj podstawowej wartości całkowitej dwóch argumentów operacji.

### <a name="reference-type-equality-operators"></a>Operatory porównania typu odwołania

Operatory porównania odwołanie wstępnie zdefiniowany typ to:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

Operatory zwracają wynik porównanie dwa odwołania pod względem równości lub bez równości.

Ponieważ operatory równości odwołanie wstępnie zdefiniowany typ akceptuje argumentów operacji typu `object`, odnoszą się do wszystkich typów, które nie są deklarowane dotyczy `operator ==` i `operator !=` elementów członkowskich. Z drugiej strony wszystkie operatory równości zdefiniowanych przez użytkownika dotyczy skutecznie ukryć Operatory równości odwołanie wstępnie zdefiniowanego typu.

Operatory równości odwołanie wstępnie zdefiniowany typ wymagają jednego z następujących czynności:

*  Oba operandy są wartości typu, znane jako *reference_type* lub literału `null`. Ponadto konwersję jawnego odwołania ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)) istnieje z typu oba operandy typu to drugi operand.
*  Jeden z operandów jest wartość typu `T` gdzie `T` jest *type_parameter* , a drugi operand jest literał `null`. Ponadto `T` nie ma ograniczenia typu wartości.

Jeśli jeden z tych warunków jest spełniony, występuje błąd wiązania. Dostępne są następujące istotne konsekwencje tych reguł:

*  Jest to błąd powiązania w czasie, aby użyć operatorów równości typu odwołania wstępnie zdefiniowanych, aby porównać dwa odwołania, które są znane jako różne podczas wiązania. Na przykład, jeśli typy powiązań czasu operandy są dwa typy klas `A` i `B`, a jeśli żadna `A` ani `B` dziedziczy po drugiej, a następnie byłoby niemożliwe w przypadku dwóch argumentów operacji odwoływać się do tego samego obiektu. W związku z tym operacja jest uznawany za błąd wiązania.
*  Operatory porównania odwołanie wstępnie zdefiniowany typ nie zezwalają na wartości operandy typu, który można porównać. Dlatego chyba że typ struktury deklaruje swój własny operatory porównania, nie jest możliwe do porównania wartości tego typu struktury.
*  Operatory porównania odwołanie wstępnie zdefiniowany typ nigdy nie powodują operacje na polach wystąpić w przypadku ich argumentów. Jest całkowicie nieprzydatna wykonania takich operacji pakowania, ponieważ odwołania do nowo przydzielonego wystąpienia spakowany niekoniecznie będzie różnić się od inne odnośniki do.
*  Jeśli argument operacji typu parametru typu `T` jest porównywany z `null`oraz typ środowiska wykonawczego `T` jest typem wartości wynikiem porównania jest `false`.

Poniższy przykład sprawdza, czy argument nieograniczony parametr typu jest `null`.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

`x == null` Konstrukcja jest dozwolone mimo `T` może reprezentować typ wartości, a wynik jest definiowana po prostu być `false` podczas `T` jest typem wartości.

Dla operacji formularza `x == y` lub `x != y`, jeśli wszystkie odpowiednie `operator ==` lub `operator !=` istnieje Rozpoznanie przeciążenia operatora ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) wybierze reguł, które Operator zamiast operatora równości odwołanie wstępnie zdefiniowanego typu. Jednak zawsze jest możliwe wybrać operator równości typu odwołania wstępnie zdefiniowane przez jawne rzutowanie jedno lub oba operandy na typ `object`. Przykład
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
generuje dane wyjściowe
```
True
False
False
False
```

`s` i `t` zmienne odnoszą się do dwóch odrębnych `string` wystąpienia zawierające te same znaki. Wyświetla pierwszy porównania `True` ponieważ ciąg wstępnie zdefiniowanego operatora równości ([operatory porównania ciągów](expressions.md#string-equality-operators)) zostaje wybrany, gdy oba operandy są typu `string`. Pozostałych porównań, wszystkie dane wyjściowe `False` ponieważ operatora równości typu odwołania wstępnie jest zaznaczone, gdy co najmniej jeden z operandów jest typu `object`.

Należy pamiętać, że powyższe technika nie istotnych dla typów wartości. Przykład
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
dane wyjściowe `False` ponieważ rzutowania utworzyć odwołania do dwóch osobnych wystąpień opakowany `int` wartości.

### <a name="string-equality-operators"></a>Operatory porównania ciągów

Operatory porównania wstępnie zdefiniowanych parametrów są:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

Dwa `string` wartości są uznawane za równe, gdy jest spełniony jeden z następujących czynności:

*  Obie wartości są `null`.
*  Obie wartości są inne niż null odwołania do wystąpienia ciągu, które mają identyczne długości i znaków identyczny w każdej pozycji znaku.

Operatory porównania ciągu porównanie wartości ciągu zamiast odwołania do ciągu. Jeśli dwa wystąpienia ciągu oddzielne zawierają dokładnie tej samej sekwencji znaków, wartości ciągi są równe, ale różnią się odwołania. Zgodnie z opisem w [Operatory równości typu odwołania](expressions.md#reference-type-equality-operators), operatory równości typu odwołania może służyć do porównywania odwołań ciągu zamiast wartości ciągu.

### <a name="delegate-equality-operators"></a>Operatory porównania delegata

Każdy typ delegata niejawnie zawiera następujące operatory porównania wstępnie zdefiniowane:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

Delegat dwóch wystąpień uważa za równe w następujący sposób:

*  Jeśli jedno z wystąpień delegata jest `null`, są równe tylko wtedy, gdy oba są `null`.
*  Jeśli delegatów mają różne typu run-time, nigdy nie są takie same.
*  Jeśli oba wystąpienia delegata ma listę wywołań ([delegować deklaracje](delegates.md#delegate-declarations)), te wystąpienia są takie same, tylko wtedy, gdy ich wywołania listy mają taką samą długość, a każdy wpis na liście wywołania jednej firmy jest taki sam, (zdefiniowany poniżej) Aby odpowiadający mu wpis w kolejności na liście wywołania drugiej strony.

Równość wpisów na liście wywołania decydują następujące reguły:

*  Jeśli dwa wywołania wpisów na liście zarówno odnoszą się do tej samej statycznej metody, a następnie pozycje są równe.
*  Jeśli dwa wywołania wpisów na liście oba odnoszą się do tej samej metody niestatycznych na tego samego obiektu docelowego (zgodnie z definicją Operatory równości odwołań), wpisy są równe.
*  Wywołanie listy wpisów utworzone z wersji ewaluacyjnej semantycznie identycznych *anonymous_method_expression*s lub *lambda_expression*s przy użyciu tego samego zestawu (prawdopodobnie pusta) przechwyconej zmiennej zewnętrzne wystąpienia są dozwolone (ale nie muszą) być taki sam.

### <a name="equality-operators-and-null"></a>Operatory równości i o wartości null

`==` i `!=` operatory zezwala na wartość typu dopuszczającego wartość null, a drugie to jeden z operandów `null` literału, nawet jeśli żaden operator wstępnie zdefiniowanych lub zdefiniowanych przez użytkownika (w unlifted lub zniesione formularza) istnieje dla tej operacji.

Dla operacji o jednym z formularzy
```csharp
x == null
null == x
x != null
null != x
```
gdzie `x` to wyrażenie typu dopuszczającego wartość null, jeśli Rozpoznanie przeciążenia operatora ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) nie może znaleźć zastosowanie operatora, wynik zamiast tego jest obliczany na podstawie `HasValue` Właściwość `x`. W szczególności pierwsze dwa formularze są tłumaczone na `!x.HasValue`, a ostatnie dwa formularze są tłumaczone na `x.HasValue`.

### <a name="the-is-operator"></a>Is — operator

`is` Operator jest używany do dynamicznie Sprawdź, czy typu run-time obiektu jest zgodne z danym typem. Wynik operacji `E is T`, gdzie `E` to wyrażenie i `T` to typ, jest wartością logiczną wartość wskazującą czy `E` pomyślnie można przekonwertować na typ `T` przy konwersji odwołania, konwersji boxing Konwersja lub konwersji rozpakowującej. Operacja jest obliczane w następujący sposób, po mają zostać podstawione argumentów typu dla wszystkich parametrów typu:

*  Jeśli `E` jest funkcją anonimową, wystąpi błąd kompilacji
*  Jeśli `E` jest grupa metod lub `null` literału, jeśli typ `E` jest typem referencyjnym lub typ dopuszczający wartość null i wartości `E` jest wartość null, wynikiem jest wartość false.
*  W przeciwnym razie umożliwiają `D` reprezentuje typ dynamiczny `E` w następujący sposób:
   * Jeśli typ `E` jest typem referencyjnym `D` jest typu run-time odwołania wystąpienie przez `E`.
   * Jeśli typ `E` jest typem zerowalnym `D` jest podstawowym typem tego typu dopuszczającego wartość null.
   * Jeśli typ `E` jest typem wartości niedopuszczającym wartości `D` jest typem `E`.
*  Wynikiem operacji jest zależna od `D` i `T` w następujący sposób:
   * Jeśli `T` jest typem referencyjnym, wynikiem jest wartość true, jeśli `D` i `T` są tego samego typu, jeśli `D` jest typem odwołania i niejawna konwersja odwołania z `D` do `T` istnieje, lub jeśli `D` jest to typ wartości i konwersja boxing `D` do `T` istnieje.
   * Jeśli `T` jest typem zerowalnym wynikiem jest wartość true, jeśli `D` jest podstawowym typem `T`.
   * Jeśli `T` jest typem wartości niedopuszczającym wartości, wynikiem jest wartość true, jeśli `D` i `T` tego samego typu.
   * W przeciwnym razie wynikiem jest wartość false.

Konwersje zdefiniowane przez użytkownika nie są traktowane przez `is` operatora.

### <a name="the-as-operator"></a>Operator as

`as` Operator jest używany do jawnie przekonwertować wartość typu danego odwołania lub typ dopuszczający wartość null. W przeciwieństwie do wyrażenia rzutowania ([rzutowane wyrażenia,](expressions.md#cast-expressions)), `as` operator nigdy nie zgłasza wyjątku. Zamiast tego, jeśli wskazany konwersja nie jest możliwe, wartość wynikowa zależy `null`.

W przypadku operacji formularza `E as T`, `E` musi być wyrażeniem i `T` musi być typem referencyjnym, parametr typu, znane jako typu odwołania lub typ dopuszczający wartość null. Ponadto co najmniej jedną z następujących musi mieć wartość true lub w przeciwnym razie wystąpi błąd w czasie kompilacji:

*  Tożsamość ([konwersji tożsamości](conversions.md#identity-conversion)), niejawne dopuszcza wartości null ([niejawne konwersje dopuszczającego wartość null](conversions.md#implicit-nullable-conversions)), niejawne odwołanie ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)), pakowania ([ Konwersje boxing](conversions.md#boxing-conversions)), jest to jawne dopuszczającego wartość null ([jawne konwersje dopuszczającego wartość null](conversions.md#explicit-nullable-conversions)), jawnego odwołania ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)), i konwersja unboxing ([Konwersje rozpakowywania](conversions.md#unboxing-conversions)) istnieje konwersja z `E` do `T`.
*  Typ `E` lub `T` jest typem otwartym.
*  `E` jest `null` literału.

Jeśli typ czasu kompilacji `E` nie `dynamic`, operacja `E as T` daje ten sam wynik jako
```csharp
E is T ? (T)(E) : (T)null
```
z tą różnicą, że `E` jest obliczany tylko raz. Kompilator można oczekiwać w celu zoptymalizowania `E as T` sprawdzania co najwyżej jednego typu dynamicznego zamiast dwóch kontroli typu dynamicznego też dorozumianych przez rozszerzenie powyżej.

Jeśli typ czasu kompilacji `E` jest `dynamic`, w odróżnieniu od operatora rzutowania `as` operator nie jest powiązany dynamicznie ([wiązanie dynamiczne](expressions.md#dynamic-binding)). W związku z tym rozszerzeniem w tym przypadku to:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Należy zauważyć, że niektóre konwersji, takie jak konwersje zdefiniowane przez użytkownika nie można zrobić za pomocą `as` operatora i zamiast tego powinny być wykonywane przy użyciu wyrażenia cast.

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
Parametr typu `T` z `G` jest znany jako typ odwołania, ponieważ ma ona ograniczenia klasy. Parametr typu `U` z `H` nie jest jednak; dlatego używanie `as` operatora w `H` jest niedozwolone.

## <a name="logical-operators"></a>Operatory logiczne

`&`, `^`, I `|` operatory są nazywane operatorów logicznych.

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

Jeśli operand logicznego operatora ma typ kompilacji `dynamic`, a następnie wyrażenie jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)). W tym przypadku jest to typ kompilacji wyrażenia `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania za pomocą typu run-time tych operandów, które mają typ kompilacji `dynamic`.

Dla operacji formularza `x op y`, gdzie `op` jest jednym z operatorów logicznych Rozpoznanie przeciążenia ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) jest stosowany do wybrania implementacji określonego operatora. Operandy są konwertowane na typy parametrów operatora wybrany, a typ wyniku jest typem zwracanym operatora.

Operatory logiczne wstępnie zdefiniowane są opisane w poniższych sekcjach.

### <a name="integer-logical-operators"></a>Operatory logiczne liczba całkowita

Operatory logiczne wstępnie zdefiniowanej liczby całkowitej są:
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

`&` Operator oblicza operatora testu koniunkcji logicznej `AND` z dwóch argumentów operacji `|` operator oblicza operatora testu koniunkcji logicznej `OR` z dwóch argumentów operacji i `^` operator oblicza wyłączny sumy bitowej dla logicznej `OR` z dwóch argumentów operacji. Nie przepełnienia są możliwe do tych operacji.

### <a name="enumeration-logical-operators"></a>Operatory logiczne wyliczenia

Każdy typ wyliczenia `E` niejawnie udostępnia następujące wstępnie zdefiniowane operatory logiczne:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

Wynik obliczania wartości `x op y`, gdzie `x` i `y` są wyrażeniami typu wyliczeniowego `E` z typu podstawowego `U`, i `op` jest jednym z operatorów logicznych, jest dokładnie taka sama jak Ocena `(E)((U)x op (U)y)`. Innymi słowy operatorów logicznych typu wyliczenia po prostu wykonać operacji logicznej na podstawowym typem dwóch argumentów operacji.

### <a name="boolean-logical-operators"></a>Operatory logiczne logiczne

Wstępnie zdefiniowane logiczna operatory logiczne to:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

Wynik `x & y` jest `true` Jeśli oba `x` i `y` są `true`. W przeciwnym razie wynikiem jest `false`.

Wynik `x | y` jest `true` Jeśli `x` lub `y` jest `true`. W przeciwnym razie wynikiem jest `false`.

Wynik `x ^ y` jest `true` Jeśli `x` jest `true` i `y` jest `false`, lub `x` jest `false` i `y` jest `true`. W przeciwnym razie wynikiem jest `false`. Kiedy operandy są typu `bool`, `^` operator oblicza ten sam wynik jako `!=` operatora.

### <a name="nullable-boolean-logical-operators"></a>Operatory dopuszczające wartość null, boolean i logiczne

Dopuszcza wartości null typu boolean `bool?` może reprezentować trzech wartości `true`, `false`, i `null`i jest podobna do przechowywanymi w trzech użytej dla wyrażeń logicznych w języku SQL. Aby upewnij się, że wyniki generowane przez `&` i `|` operatory `bool?` operandy są zgodne z logiką przechowywanymi w trzech SQL, następujące operatory wstępnie zdefiniowanych znajdują się:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

W poniższej tabeli przedstawiono efektów tych operatorów dla wszystkich kombinacjach wartości `true`, `false`, i `null`.

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

## <a name="conditional-logical-operators"></a>Operatory logiczne warunkowe

`&&` i `||` operatory są nazywane warunkowego operatorów logicznych. Są również nazywane "short-circuiting" operatorów logicznych.

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

`&&` i `||` operatory są wersjami warunkowego `&` i `|` operatory:

*  Operacja `x && y` odnosi się do operacji `x & y`, chyba że `y` jest oceniane tylko wtedy, gdy `x` nie `false`.
*  Operacja `x || y` odnosi się do operacji `x | y`, chyba że `y` jest oceniane tylko wtedy, gdy `x` nie `true`.

Jeśli operand logicznego operatora warunkowego ma typ kompilacji `dynamic`, a następnie wyrażenie jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)). W tym przypadku jest to typ kompilacji wyrażenia `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania za pomocą typu run-time tych operandów, które mają typ kompilacji `dynamic`.

Operacji formularza `x && y` lub `x || y` jest przetwarzany przez zastosowanie Rozpoznanie przeciążenia ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) tak, jakby został napisany operacji `x & y` lub `x | y`. Następnie

*  Rozpoznanie przeciążenia kończy się niepowodzeniem, można znaleźć jednego operatora najlepsze lub jeśli funkcja rozpoznawania przeciążeń wybiera jeden z operatorów logicznych wstępnie zdefiniowanej liczby całkowitej, występuje błąd wiązania.
*  W przeciwnym razie, jeśli wybrany operator jest jednym z wstępnie zdefiniowanych logiczna operatorów logicznych ([logiczna operatorów logicznych](expressions.md#boolean-logical-operators)) lub operatory dopuszczające wartość null, boolean i logiczne ([operatory dopuszczające wartość null, boolean i logiczne](expressions.md#nullable-boolean-logical-operators)), Operacja zostanie przetworzona, zgodnie z opisem w [operatory logiczne, warunkowe i logiczne](expressions.md#boolean-conditional-logical-operators).
*  W przeciwnym razie wybrane operator jest zdefiniowany przez użytkownika operator, a operacja zostanie przetworzona, zgodnie z opisem w [zdefiniowanych przez użytkownika operatorów logicznych warunkowych](expressions.md#user-defined-conditional-logical-operators).

Nie jest możliwe bezpośrednio przeciążanie operatorów logicznych warunkowych. Jednak ponieważ operatorów logicznych warunkowego są oceniane pod względem regularne operatorów logicznych, przeciążenia regularne operatory logiczne z pewnymi ograniczeniami również pełnią funkcję przeciążenia operatorów logicznych warunkowych. Jest to dokładniejszym opisem zawartym w [zdefiniowanych przez użytkownika operatorów logicznych warunkowych](expressions.md#user-defined-conditional-logical-operators).

### <a name="boolean-conditional-logical-operators"></a>Operatory logiczne, warunkowe i logiczne

Podczas argumenty operacji `&&` lub `||` typu `bool`, lub gdy operandy typów, które nie zostaną zdefiniowane odpowiednim `operator &` lub `operator |`, ale definiuje konwersji niejawnych do `bool`, operacja jest przetwarzane w następujący sposób:

*  Operacja `x && y` jest oceniane jako `x ? y : false`. Innymi słowy `x` najpierw jest obliczane i konwertowane na typ `bool`. Następnie, jeśli `x` jest `true`, `y` jest obliczane i konwertowane na typ `bool`, a ten staje się wynik operacji. W przeciwnym razie wynik operacji jest `false`.
*  Operacja `x || y` jest oceniane jako `x ? true : y`. Innymi słowy `x` najpierw jest obliczane i konwertowane na typ `bool`. Następnie, jeśli `x` jest `true`, wynik operacji jest `true`. W przeciwnym razie `y` jest obliczane i konwertowane na typ `bool`, a ten staje się wynik operacji.

### <a name="user-defined-conditional-logical-operators"></a>Zdefiniowane przez użytkownika operatorów logicznych warunkowych

Gdy argumenty operacji `&&` lub `||` są typów, które deklarują odpowiednim zdefiniowanych przez użytkownika `operator &` lub `operator |`, oba następujące muszą być spełnione, gdzie `T` jest typem, w którym zadeklarowany jest operatora:

*  Zwracany typ i typ każdego parametru operatora musi być `T`. Oznacza to, należy obliczyć logiczny operator `AND` lub logicznej `OR` z dwóch argumentów operacji typu `T`i musi zwrócić wynik o typie `T`.
*  `T` musi zawierać deklaracje `operator true` i `operator false`.

Błąd powiązania występuje, jeśli żadnego z tych wymagań nie został spełniony. W przeciwnym razie `&&` lub `||` operacji jest oceniany, łącząc zdefiniowany przez użytkownika `operator true` lub `operator false` przy użyciu wybranego operatora zdefiniowanego przez użytkownika:

*  Operacja `x && y` jest oceniane jako `T.false(x) ? x : T.&(x, y)`, gdzie `T.false(x)` jest wywołanie `operator false` zadeklarowanych w `T`, i `T.&(x, y)` jest wywołaniem wybranego `operator &`. Innymi słowy `x` najpierw jest obliczane i `operator false` jest wywoływane na wynik, aby określić, czy `x` ma zdecydowanie wartość false. Następnie, jeśli `x` ma zdecydowanie wartość FAŁSZ, wynik operacji jest wartością, który został wcześniej obliczane `x`. W przeciwnym razie `y` zostało ocenione, a wybranym `operator &` jest wywoływane na wartość wcześniej obliczane `x` i wartość jest obliczana dla `y` do uzyskania wyniku operacji.
*  Operacja `x || y` jest oceniane jako `T.true(x) ? x : T.|(x, y)`, gdzie `T.true(x)` jest wywołanie `operator true` zadeklarowanych w `T`, i `T.|(x,y)` jest wywołaniem wybranego `operator|`. Innymi słowy `x` najpierw jest obliczane i `operator true` jest wywoływane na wynik, aby określić, czy `x` zdecydowanie obowiązuje. Następnie, jeśli `x` ma zdecydowanie wartość true, wynik operacji jest wartością, który został wcześniej obliczane `x`. W przeciwnym razie `y` zostało ocenione, a wybranym `operator |` jest wywoływane na wartość wcześniej obliczane `x` i wartość jest obliczana dla `y` do uzyskania wyniku operacji.

W jednej z tych operacji, wyrażenie podane `x` jest tylko wykonywane tylko raz i wyrażenie podane `y` nie obliczeniem i oceniane tylko raz.

Aby uzyskać przykład typ, który implementuje `operator true` i `operator false`, zobacz [bazy danych typu boolean](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>Wartość null operatora łączącego

`??` Operator jest nazywany operatora łączącego o wartości null.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

Wyrażenie łączącego o wartości null w postaci `a ?? b` wymaga `a` będzie typ dopuszczający wartość null typu lub odwołania. Jeśli `a` jest różna od null, wynik `a ?? b` jest `a`; w przeciwnym razie wynikiem jest `b`. Daje w wyniku operacji `b` tylko wtedy, gdy `a` ma wartość null.

Wartość null operatora łączącego jest zespolony z prawej, co oznacza, że operacje są pogrupowane od prawej do lewej. Na przykład, wyrażenie w formie `a ?? b ?? c` jest oceniane jako `a ?? (b ?? c)`. Ogólnie rzecz biorąc warunki, wyrażenie w formie `E1 ?? E2 ?? ... ?? En` zwraca pierwszy operandów, która jest inna niż null lub wartość null, jeśli wszystkie argumenty mają wartość null.

Typ wyrażenia `a ?? b` zależy od których niejawne konwersje są dostępne na operandach. Według priorytetu, typu `a ?? b` jest `A0`, `A`, lub `B`, gdzie `A` jest typem `a` (pod warunkiem, że `a` ma typ), `B` jest typem `b` () pod warunkiem, że `b` ma typ), a `A0` jest podstawowym typem `A` Jeśli `A` jest typem zerowalnym lub `A` inaczej. W szczególności `a ?? b` są przetwarzane w następujący sposób:

*  Jeśli `A` istnieje i nie jest typu dopuszczającego wartość null lub typ referencyjny, wystąpi błąd kompilacji.
*  Jeśli `b` jest wyrażeniem dynamiczny typ wyniku jest `dynamic`. W czasie wykonywania `a` najpierw jest oceniany. Jeśli `a` nie ma wartości null, `a` jest konwertowana na dynamiczne, a ten staje się wynik. W przeciwnym razie `b` jest obliczane i staje się on wynik.
*  W przeciwnym razie, jeśli `A` istnieje i jest typu dopuszczającego wartość null, i istnieje niejawna konwersja z `b` do `A0`, typ wyniku jest `A0`. W czasie wykonywania `a` najpierw jest oceniany. Jeśli `a` nie ma wartości null, `a` jest nieopakowanych, aby wpisać `A0`, i staje się on wynik. W przeciwnym razie `b` jest obliczane i konwertowane na typ `A0`, i staje się on wynik.
*  W przeciwnym razie, jeśli `A` istnieje i istnieje niejawna konwersja z `b` do `A`, typ wyniku jest `A`. W czasie wykonywania `a` najpierw jest oceniany. Jeśli `a` nie ma wartości null, `a` staje się wynik. W przeciwnym razie `b` jest obliczane i konwertowane na typ `A`, i staje się on wynik.
*  W przeciwnym razie, jeśli `b` ma typ `B` i istnieje niejawna konwersja z `a` do `B`, typ wyniku jest `B`. W czasie wykonywania `a` najpierw jest oceniany. Jeśli `a` nie ma wartości null, `a` jest nieopakowanych, aby wpisać `A0` (Jeśli `A` istnieje i ma wartość null) i zostanie przekonwertowana na typ `B`, i staje się on wynik. W przeciwnym razie `b` jest obliczane i staje się wynik.
*  W przeciwnym razie `a` i `b` występuje są niezgodne, a błąd w czasie kompilacji.

## <a name="conditional-operator"></a>Operator warunkowy

`?:` Operator jest nazywany operatora warunkowego. Jest to czasami również operator trójargumentowy.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

Wyrażenie warunkowe w postaci `b ? x : y` najpierw ocenia stan `b`. Następnie, jeśli `b` jest `true`, `x` jest obliczane i staje się wynik operacji. W przeciwnym razie `y` jest obliczane i staje się wynik operacji. Wyrażenie warunkowe nigdy nie ocenia zarówno `x` i `y`.

Operator warunkowy jest zespolony z prawej, co oznacza, że operacje są pogrupowane od prawej do lewej. Na przykład, wyrażenie w formie `a ? b : c ? d : e` jest oceniane jako `a ? b : (c ? d : e)`.

Pierwszy argument operacji `?:` operator musi być wyrażeniem, które mogą być niejawnie konwertowane na `bool`, lub wyrażenie typu, który implementuje `operator true`. Jeśli żadna z tych wymagań nie jest spełniony, wystąpi błąd kompilacji.

Drugi i trzeci operand, `x` i `y`, z `?:` operator kontrolowanie typu wyrażenia warunkowego.

*  Jeśli `x` ma typ `X` i `y` ma typ `Y` następnie
   * Jeśli niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z `X` do `Y`, ale nie z `Y` do `X`, następnie `Y` jest typem wyrażenia warunkowego.
   * Jeśli niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z `Y` do `X`, ale nie z `X` do `Y`, następnie `X` jest typem wyrażenia warunkowego.
   * W przeciwnym razie można określić żadnego typu wyrażenia i występuje błąd kompilacji.
*  Jeśli tylko jeden z `x` i `y` ma typ i zarówno `x` i `y`, z są niejawnie konwertowane na tego typu, który jest typu wyrażenia warunkowego.
*  W przeciwnym razie można określić żadnego typu wyrażenia i występuje błąd kompilacji.

Przetwarzanie czasu wykonywania wyrażenia warunkowego formularza `b ? x : y` składa się z następujących czynności:

*  Po pierwsze, `b` jest obliczane i `bool` wartość `b` jest określana:
   * Jeśli niejawna konwersja z typu `b` do `bool` istnieje, to niejawna konwersja jest przeprowadzana w celu wygenerowania `bool` wartość.
   * W przeciwnym razie `operator true` zdefiniowane przez typ `b` jest wywoływana w celu wygenerowania `bool` wartość.
*  Jeśli `bool` wartość utworzony w kroku powyżej `true`, następnie `x` jest obliczane i konwertowane na typ wyrażenia warunkowego, a ten staje się wynikiem wyrażenia warunkowego.
*  W przeciwnym razie `y` jest obliczane i konwertowane na typ wyrażenia warunkowego, a ten staje się wynikiem wyrażenia warunkowego.

## <a name="anonymous-function-expressions"></a>Funkcja anonimowa wyrażeń

***Funkcja anonimowa*** jest wyrażeniem, które reprezentuje definicję metody "w tekście". Funkcja anonimowa nie ma wartości lub typem w i samego siebie, ale jest konwertowany na zgodny delegata lub typ drzewa wyrażenia. Ocena konwersję funkcja anonimowa zależy od typ docelowy konwersji: Jeśli jest to typ delegowany, konwersja wynikiem jest wartość delegata odwołuje się do metody, która definiuje funkcja anonimowa. Jeśli typ drzewa wyrażeń, konwersja daje w wyniku drzewo wyrażenia, który reprezentuje strukturę metodę jako struktury obiektu.

Ze względów historycznych istnieją dwa składni odmian funkcje anonimowe, mianowicie *lambda_expression*s i *anonymous_method_expression*s. Dla prawie wszystkich celów *lambda_expression*s są bardziej zwięzłe i ekspresyjny niż *anonymous_method_expression*s, które pozostają w języku dla zapewnienia zgodności.

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

`=>` Operator ma ten sam priorytet, jako przydział (`=`) i jest zespolony z prawej.

Funkcja anonimowa z `async` modyfikator jest funkcji asynchronicznej i zgodna z zasadami opisanymi w [Iteratory](classes.md#iterators).

Parametry funkcją anonimową w formie *lambda_expression* można wpisać jawnie lub niejawnie. Na liście parametrów jawnie wpisanych jest jawnie określony typ każdego parametru. Na liście parametrów niejawnie wpisane typy parametrów są dedukowane z kontekstu, w którym następuje funkcja anonimowa — w szczególności, gdy funkcja anonimowa jest konwertowany na typ zgodny delegata lub typ drzewa wyrażeń, który zapewnia typ typy parametrów ([konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions)).

W funkcją anonimową z parametrem pojedynczy, niejawnie wpisane w liście parametrów można pominąć nawiasów. Innymi słowy funkcja anonimowa formularza
```csharp
( param ) => expr
```
można stosować skrót do
```csharp
param => expr
```

Lista parametrów funkcji anonimowych w formie *anonymous_method_expression* jest opcjonalne. Biorąc pod uwagę, parametry muszą jawnie wpisane. Jeśli nie, funkcja anonimowa jest konwertowany na delegata z każdego parametru listy, niezawierający `out` parametrów.

A *bloku* treści funkcją anonimową jest osiągalny ([punktów końcowych i osiągalności](statements.md#end-points-and-reachability)), chyba że funkcja anonimowa odbywa się wewnątrz instrukcji jest nieosiągalny.

Przykłady funkcji anonimowych, wykonaj poniższe:

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

Zachowanie *lambda_expression*s i *anonymous_method_expression*s jest taki sam, z wyjątkiem następujące kwestie:

*  *anonymous_method_expression*s zezwala na liście parametrów, aby można pominąć w całości, reaguje przetwarzania na typy wszystkie listy parametrów wielowartościowych delegatów.
*  *lambda_expression*s zezwala na używanie typów parametru pominięcia i wywnioskować, natomiast *anonymous_method_expression*s wymagają typy parametrów, należy jawnie podać.
*  Treść *lambda_expression* może być wyrażenie lub blok instrukcji, natomiast treści *anonymous_method_expression* musi być blok instrukcji.
*  Tylko *lambda_expression*ma konwersji na typy drzewa wyrażenie zgodne ([typy drzewa wyrażenie](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Funkcja anonimowa podpisów

Opcjonalny *anonymous_function_signature* z funkcją anonimową definiuje nazwy i opcjonalnie typy parametrów formalnych dla funkcji anonimowych. Zakres parametry funkcja anonimowa jest *anonymous_function_body*. ([Zakresy](basic-concepts.md#scopes)) wraz z listy parametrów (jeśli) anonimowe —-treści metody stanowi miejsca deklaracji ([deklaracje](basic-concepts.md#declarations)). Ten sposób jest to błąd kompilacji dla nazwy parametru funkcja anonimowa dopasowany do nazwy zmiennej lokalnej, stała lokalna lub parametr, którego zakres obejmuje *anonymous_method_expression* lub *lambda_ wyrażenie*.

Jeśli funkcja anonimowa *explicit_anonymous_function_signature*, zbiór delegata zgodne typy i drzewa wyrażenie jest ograniczone do tych, które mają te same typy parametrów i Modyfikatory w tej samej kolejności. W przeciwieństwie do metody konwersji grup ([metody konwersji grup](conversions.md#method-group-conversions)), ma przeciwwskazań wariancję typy parametrów funkcji anonimowych nie jest obsługiwane. Jeśli nie jest funkcją anonimową *anonymous_function_signature*, zbiór delegata zgodne typy i drzewa wyrażenie jest ograniczone do tych, które nie mają `out` parametrów.

Należy pamiętać, że *anonymous_function_signature* nie może zawierać atrybutów lub tablicy parametrów. Niemniej jednak *anonymous_function_signature* może być zgodny z typem delegowanym, którego lista parametrów zawiera tablicy parametrów.

Należy zauważyć, tej konwersji na typ drzewa wyrażeń, nawet wtedy, gdy zgodne, może nadal się nie powieść w czasie kompilacji ([typy drzewa wyrażenie](types.md#expression-tree-types)).

### <a name="anonymous-function-bodies"></a>Gdy ciało funkcji anonimowych

Treść (*wyrażenie* lub *bloku*) jest funkcją anonimową się następujące zasady:

*  Jeśli funkcja anonimowa zawiera podpis, określone w sygnaturze parametry są dostępne w treści. Jeśli funkcja anonimowa nie ma sygnatury może on zostać przekonwertowany do typu delegata lub typ wyrażenia o parametry ([konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions)), ale parametry nie są dostępne w treści.
*  Z wyjątkiem `ref` lub `out` parametry określone w sygnaturze (jeśli istnieją) w najbliższej otaczającej funkcja anonimowa, jest to błąd czasu kompilacji, treści, aby uzyskać dostęp do `ref` lub `out` parametru.
*  Gdy typ `this` jest typem struktury jest to błąd czasu kompilacji, treści, aby uzyskać dostęp do `this`. Jest to istotne, czy dostęp jest jawne (podobnie jak w `this.x`) lub niejawne (podobnie jak w `x` gdzie `x` jest to składowa struktury). Ta reguła po prostu zabrania takiego dostępu i nie ma wpływu na czy wyszukanie członka skutkuje członka struktury.
*  Treść ma dostęp do zmiennych zewnętrznych ([zmienne zewnętrzne](expressions.md#outer-variables)) funkcji anonimowych. Dostęp do zewnętrznej zmiennej będzie odwoływać się do wystąpienia w zmiennej, która jest aktywna w czasie *lambda_expression* lub *anonymous_method_expression* jest oceniany ([oceny Funkcja anonimowa wyrażeń](expressions.md#evaluation-of-anonymous-function-expressions)).
*  Jest to błąd kompilacji w treści zawierać `goto` instrukcji `break` instrukcji lub `continue` instrukcji, którego cel jest poza treścią lub w treści zawartej w anonimowej funkcji.
*  A `return` instrukcji w treści nie zwróci kontroli z wywołania najbliższej otaczającej funkcja anonimowa, nie z otaczającego element członkowski funkcji. Wyrażenie określone w `return` instrukcja musi być niejawnie konwertowane na typ zwracany typ delegata lub typ drzewa wyrażeń, do którego najbliższej otaczającej *lambda_expression* lub *anonymous_ method_expression* jest konwertowany ([konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions)).

Jest on jawnie nieokreślony, czy istnieje sposób wykonania bloku funkcją anonimową innych niż ocena i wywołanie *lambda_expression* lub *anonymous_method_expression*. W szczególności kompilator może zadecydować o stosowaniu funkcją anonimową przez Syntetyzujące jeden lub więcej nazwanych, metody lub typów. Nazwy takie syntetyzowany elementy muszą być zarezerwowana do użytku kompilatora formularza.

### <a name="overload-resolution-and-anonymous-functions"></a>Rozpoznanie przeciążenia i funkcje anonimowe

Funkcje anonimowe na liście argumentów uczestniczyć w wnioskowanie o typie i rozpoznanie przeciążenia. Zapoznaj się [wnioskowanie o typie](expressions.md#type-inference) i [Rozpoznanie przeciążenia](expressions.md#overload-resolution) dokładnie reguł.

Poniższy przykład ilustruje efekt funkcjami anonimowymi w przeciążeniu rozdzielczości.

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

`ItemList<T>` Klasa ma dwa `Sum` metody. Każdy przyjmuje `selector` argumentu, który wyodrębnia wartość do zsumowania za pośrednictwem z elementu listy. Wyodrębniona wartość może być `int` lub `double` i wynikowa suma jest podobnie `int` lub `double`.

`Sum` Metody na przykład można użyć do obliczenia sum z listy wierszy szczegółów zamówienia.

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

W pierwszym wywołaniu `orderDetails.Sum`, zarówno `Sum` dotyczą metod ponieważ funkcja anonimowa `d => d. UnitCount` jest zgodny z obu `Func<Detail,int>` i `Func<Detail,double>`. Jednak funkcja rozpoznawania przeciążeń wybiera pierwszy `Sum` metody ponieważ konwersja na `Func<Detail,int>` jest lepsze niż konwersja `Func<Detail,double>`.

W drugim wywołaniu `orderDetails.Sum`, tylko drugi `Sum` metoda stosowana jest ponieważ funkcja anonimowa `d => d.UnitPrice * d.UnitCount` tworzy wartość typu `double`. W efekcie przeciążenia rozpoznawania wybiera drugiej `Sum` metodę dla tego wywołania.

### <a name="anonymous-functions-and-dynamic-binding"></a>Funkcje anonimowe i wiązanie dynamiczne

Funkcja anonimowa nie może być odbiorcy, argument lub argument dynamicznie powiązane działania.

### <a name="outer-variables"></a>Zmienne zewnętrzne

Dowolnej zmiennej lokalnej, wartość parametru lub tablicy parametrów, których zakres obejmuje *lambda_expression* lub *anonymous_method_expression* nosi nazwę ***zewnętrzna zmienna*** funkcji anonimowych. W to funkcja składowa klasy `this` wartość uznaje się za parametrem wartości i nie jest zewnętrzna zmienna funkcji anonimowych zawartych w funkcji elementu członkowskiego.

#### <a name="captured-outer-variables"></a>Przechwyconych zmiennych zewnętrznych

Zewnętrzna zmienna odwołuje się funkcją anonimową, zewnętrzna zmienna jest określane jako zostały ***przechwycone*** , funkcja anonimowa. Zazwyczaj, okresu istnienia zmiennej lokalnej jest ograniczona do wykonywania bloku lub instrukcji, z którym jest skojarzona ([zmienne lokalne](variables.md#local-variables)). Jednak okres istnienia przechwyconej zmiennej zewnętrzne jest rozszerzany co najmniej do delegata lub drzewa wyrażeń utworzone na podstawie funkcja anonimowa kwalifikuje się do wyrzucania elementów bezużytecznych.

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
Zmienna lokalna `x` jest przechwytywany przez funkcja anonimowa, a okres istnienia `x` jest co najmniej rozszerzone do delegata zwróciło `F` kwalifikuje się do wyrzucania elementów bezużytecznych, (która nie jest realizowane aż do końca bardzo program). Ponieważ każdego wywołania funkcji anonimowych operuje na to samo wystąpienie elementu `x`, dane wyjściowe z przykładu:
```
1
2
3
```

Podczas lokalnej zmiennej lub parametru wartości są przechwytywane przez funkcję anonimową, zmienną lokalną ani parametrem przestaje być uważany za jako zmienna ustalona ([stałe i zmienne ruchome](unsafe-code.md#fixed-and-moveable-variables)), ale zamiast tego jest uważany za ruchoma Zmienna. Ten sposób dowolny `unsafe` najpierw użyć kodu, który przyjmuje adres przechwyconej zmiennej zewnętrzne `fixed` instrukcję, aby naprawić zmiennej.

Należy pamiętać, że w przeciwieństwie do Niewychwycone zmiennej przechwyconej zmiennej lokalnej mogą jednocześnie łączyć się z wielu wątków wykonania.

#### <a name="instantiation-of-local-variables"></a>Podczas tworzenia wystąpienia zmiennych lokalnych

Zmienna lokalna jest uważany za ***tworzone*** podczas wykonywania wprowadza zakres zmiennej. Na przykład, gdy wywoływana jest metoda następujące, zmienna lokalna `x` jest tworzone i inicjowane trzy razy — raz dla każdej iteracji pętli.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

Jednak przenoszenie deklaracji `x` poza pętlę skutkuje pojedynczego wystąpienia `x`:
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

Nie są przechwytywane, nie ma żadnego sposobu, aby przyjrzeć się dokładnie, jak często tworzone jest zmienną lokalną, ponieważ okres istnienia wystąpień są rozłączne, jest możliwe dla każdego wystąpienia po prostu używać tej samej lokalizacji magazynu. Jednak po funkcją anonimową przechwytuje zmienną lokalną, skutki wystąpienia stają się oczywiste.

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
generuje dane wyjściowe:
```
1
3
5
```

Jednakże, jeśli deklaracja `x` zostanie przeniesiony poza pętlę:
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
Dane wyjściowe to:
```
5
5
5
```

Czy zdefiniowana Pętla for deklaruje zmienną iteracji, że sama zmienna jest uznawane za można zadeklarować poza pętlą. W związku z tym jeśli przykładzie jest zmieniany na przechwytywanie sama Zmienna iteracji:

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
tylko jedno wystąpienie zmiennej iteracji są przechwytywane, która generuje dane wyjściowe:
```
3
3
3
```

Istnieje możliwość dla obiektów delegowanych funkcja anonimowa udostępnić niektóre przechwyconych zmiennych, ale mają oddzielne wystąpienia innych. Na przykład jeśli `F` zostanie zmieniona na
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
trzy delegatów przechwytywania to samo wystąpienie elementu `x` ale oddzielnych wystąpień `y`, a dane wyjściowe są:
```
1 1
2 1
3 1
```

Osobne funkcje anonimowe można przechwycić to samo wystąpienie elementu zewnętrzna zmienna. W przykładzie:
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
te dwie funkcje anonimowe przechwytywania to samo wystąpienie elementu Zmienna lokalna `x`, a zatem "Komunikacja" za pośrednictwem tej zmiennej. Dane wyjściowe z przykładu jest:
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Obliczanie wyrażeń funkcja anonimowa

Funkcja anonimowa `F` zawsze musi zostać skonwertowany do typu delegata `D` lub typ drzewa wyrażeń `E`, bezpośrednio lub za pośrednictwem wykonywania wyrażenia tworzenia delegata `new D(F)`. Ta konwersja określa wynik funkcja anonimowa, zgodnie z opisem w [konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions).

## <a name="query-expressions"></a>Wyrażenia zapytań

***Wyrażenia zapytań*** zapewniają składnię języka zintegrowanych zapytań, który jest podobny do relacyjne i hierarchiczne zapytania języków, takich jak SQL i języka XQuery.

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

Wyrażenie zapytania zaczyna się od `from` klauzula i kończy się znakiem albo `select` lub `group` klauzuli. Początkowy `from` klauzuli może następować zero lub więcej `from`, `let`, `where`, `join` lub `orderby` klauzul. Każdy `from` klauzula jest generator Przedstawiamy ***zmiennej zakresu*** której zakresy nad elementami ***sekwencji***. Każdy `let` klauzuli wprowadza zmienną zakresu reprezentujących wartości obliczane przy użyciu poprzednich zmiennych zakresu. Każdy `where` klauzula jest filtr, który wyklucza elementów z wynikiem. Każdy `join` klauzuli porównuje klucze określonej sekwencji źródłowej z kluczami sekwencji innego, reaguje pasujących par. Każdy `orderby` klauzuli zmienia kolejność elementów zgodnie z określonymi kryteriami. Końcowe `select` lub `group` klauzula określa kształt wyniku w postaci zmiennych zakresu. Na koniec `into` klauzuli może służyć do "splice" zapytań przez traktowanie wyniki jednego zapytania jako generator w kolejnych zapytań.

### <a name="ambiguities-in-query-expressions"></a>Niejednoznaczności w wyrażeniach zapytań

Wyrażenia zapytań zawiera szereg "kontekstowymi słowami kluczowymi", czyli identyfikatorów, które mają specjalne znaczenie w danym kontekście. W szczególności są to `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` i `by`. Aby uniknąć niejednoznaczności w wyrażeniach zapytań spowodowany mieszane użycie tych identyfikatorów jak słowa kluczowe lub nazwy proste, te identyfikatory są uważane za słów kluczowych, gdy występuje w dowolnym miejscu w wyrażeniu zapytania.

W tym celu w wyrażeniu kwerendy jest dowolne wyrażenie, które zaczyna się od "`from identifier`"następuje dowolny token, z wyjątkiem"`;`","`=`"or"`,`".

Aby można było używać tych słów jako identyfikatorów w wyrażeniu zapytania, może być prefiksem "`@`" ([identyfikatory](lexical-structure.md#identifiers)).

### <a name="query-expression-translation"></a>Translacja wyrażeń zapytania

W języku C# nie określa semantykę wykonywania wyrażenia zapytania. Zamiast wyrażenia kwerendy są tłumaczone na wywołania metody, które będą zgodne *wzorzec wyrażenia kwerendy* ([wzorzec wyrażenia kwerendy](expressions.md#the-query-expression-pattern)). W szczególności wyrażenia kwerendy są tłumaczone na wywołania metody o nazwie `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, i `Cast`. Te metody powinny mieć określonej sygnatury i typy wyników, zgodnie z opisem w [wzorzec wyrażenia kwerendy](expressions.md#the-query-expression-pattern). Te metody mogą być metod wystąpień obiektu, którego dotyczy kwerenda lub metody rozszerzenia, które są zewnętrzne w stosunku do obiektu i implementują rzeczywiste wykonanie zapytania.

Tłumaczenie z wyrażenia zapytań wywołań metod jest składni mapowanie, które występuje, zanim wszystkie powiązania typu lub Rozpoznanie przeciążenia została wykonana. Tłumaczenie jest gwarantowane jest syntaktycznie prawidłowy, ale nie jest gwarantowana w celu wygenerowania semantycznie poprawny kod C#. Następujące Translacja wyrażeń zapytania, wynikowy wywołań metody opisywanego są przetwarzane jako wywołania metod regularnych, a to z kolei może ujawnić z błędów, na przykład jeśli te metody nie istnieje, jeśli argumenty mają nieprawidłowe typy lub w przypadku metod ogólnych i wnioskowanie o typie kończy się niepowodzeniem.

Wyrażenie zapytania są przetwarzane przez zastosowanie następujących tłumaczenia wielokrotnie, dopóki nie będą możliwe nie dalsze obniżki. Tłumaczenia są wymienione w kolejności stosowania: każdej sekcji przyjęto założenie, że tłumaczenia w poprzednich sekcjach, które mogły zostać wykonane wyczerpująco, a po wyczerpaniu sekcji będzie nie później należy powrócić podczas przetwarzania wyrażenia zapytania.

Przypisanie do zmiennych zakresu są niedozwolone w wyrażeniach zapytań. Jednak implementacja języka C# może nie zawsze wymuszać to ograniczenie, ponieważ jest to czasami nie jest możliwe przy użyciu schematu składni tłumaczenia przedstawionych w tym miejscu.

Niektóre tłumaczenia wstrzyknąć zmiennych zakresu o identyfikatorach przezroczyste wskazywane przez `*`. Specjalne właściwości przezroczyste identyfikatory zostały omówione w dalszych [przezroczyste identyfikatory](expressions.md#transparent-identifiers).

#### <a name="select-and-groupby-clauses-with-continuations"></a>Wybierz i grupowania klauzul z kontynuacjami

Wyrażenie zapytania z kontynuacji
```csharp
from ... into x ...
```
jest tłumaczony na
```csharp
from x in ( from ... ) ...
```

Tłumaczenia w poniższych sekcjach założono kwerendy nie mają `into` kontynuacji.

Przykład
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
jest tłumaczony na
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
ostateczne tłumaczenie jest
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>Typy zmiennych zakresu jawne

A `from` klauzula, która jawnie określa typ zmiennej zakresu
```csharp
from T x in e
```
jest tłumaczony na
```csharp
from x in ( e ) . Cast < T > ( )
```

A `join` klauzula, która jawnie określa typ zmiennej zakresu
```
join T x in e on k1 equals k2
```
jest tłumaczony na
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

Tłumaczenia w poniższych sekcjach przyjęto założenie, że zapytania mają typy zmiennych nie jawnego zakresu.

Przykład
```csharp
from Customer c in customers
where c.City == "London"
select c
```
jest tłumaczony na
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
ostateczne tłumaczenie jest
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

Typy zmiennych zakresu jawne są przydatne w przypadku zapytań kolekcje, które implementują niepodstawowy `IEnumerable` interfejs, ale nie ogólnego `IEnumerable<T>` interfejsu. W powyższym przykładzie będzie to sytuacji, gdy `customers` były typu `ArrayList`.

#### <a name="degenerate-query-expressions"></a>Wyrażenia zapytań wymiaru degeneracji

W wyrażeniu kwerendy w postaci
```csharp
from x in e select x
```
jest tłumaczony na
```csharp
( e ) . Select ( x => x )
```

Przykład
```csharp
from c in customers
select c
```
jest tłumaczony na
```csharp
customers.Select(c => c)
```

Wyrażenie zapytania wymiaru degeneracji to taki, który przypadku wybiera elementy źródła. Nowsze fazy tłumaczenia usuwa wymiaru degeneracji zapytania wprowadzone przez innych kroków translacji, zastępując je ze swoim źródłem. Ważne jest jednak do upewnij się, że wynik kwerendy wyrażenie nigdy nie jest obiekt źródłowy, jak, może ujawnić typu i tożsamość źródła do klienta zapytania. W związku z tym ten krok chroni wymiaru degeneracji zapytania zapisywane bezpośrednio w kodzie źródłowym, wywołując jawnie `Select` na "source". Jest ona maksymalnie implementacje z `Select` i innych operatorów zapytań, aby upewnić się, że te metody nigdy nie należy zwracać samego obiektu źródłowego.

#### <a name="from-let-where-join-and-orderby-clauses"></a>Od pozwolić w przypadku gdy klauzule sprzężenia i orderby

Wyrażenie zapytania przy użyciu drugiej `from` klauzuli `select` — klauzula
```csharp
from x1 in e1
from x2 in e2
select v
```
jest tłumaczony na
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

Wyrażenie zapytania przy użyciu drugiej `from` klauzuli następuje coś innego niż `select` klauzuli:

```csharp
from x1 in e1
from x2 in e2
...
```
jest tłumaczony na
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

Wyrażenie zapytania przy użyciu `let` — klauzula
```csharp
from x in e
let y = f
...
```
jest tłumaczony na
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

Wyrażenie zapytania przy użyciu `where` — klauzula
```csharp
from x in e
where f
...
```
jest tłumaczony na
```csharp
from x in ( e ) . Where ( x => f )
...
```

Wyrażenie zapytania przy użyciu `join` klauzuli bez `into` następuje `select` — klauzula
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
jest tłumaczony na
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

Wyrażenie zapytania przy użyciu `join` klauzuli bez `into` następuje coś innego niż `select` — klauzula
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
jest tłumaczony na
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

Wyrażenie zapytania przy użyciu `join` klauzula `into` następuje `select` — klauzula
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
jest tłumaczony na
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Wyrażenie zapytania przy użyciu `join` klauzula `into` następuje coś innego niż `select` — klauzula
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
jest tłumaczony na
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

Wyrażenie zapytania przy użyciu `orderby` — klauzula
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
jest tłumaczony na
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

Jeśli kolejność klauzula Określa `descending` wskaźnik kierunku, wywołanie `OrderByDescending` lub `ThenByDescending` jest generowany w zamian.

Następujące tłumaczenia przyjęto założenie, że istnieją nie `let`, `where`, `join` lub `orderby` klauzul i nie więcej niż jednego początkowego `from` klauzuli w każdym wyrażeniu zapytania.

Przykład
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
jest tłumaczony na
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
jest tłumaczony na
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
ostateczne tłumaczenie jest
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
gdzie `x` jest identyfikatorem wygenerowanego przez kompilator, który jest niewidoczna i są niedostępne.

Przykład
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
jest tłumaczony na
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
ostateczne tłumaczenie jest
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
gdzie `x` jest identyfikatorem wygenerowanego przez kompilator, który jest niewidoczna i są niedostępne.

Przykład
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
jest tłumaczony na
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
jest tłumaczony na
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
ostateczne tłumaczenie jest
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
gdzie `x` i `y` to identyfikatory wygenerowanego przez kompilator, które w przeciwnym razie są niewidoczne i są niedostępne.

Przykład
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
ma ostateczne tłumaczenie
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a>Wybierz klauzule

W wyrażeniu kwerendy w postaci
```csharp
from x in e select v
```
jest tłumaczony na
```csharp
( e ) . Select ( x => v )
```
z wyjątkiem sytuacji, gdy v jest identyfikator x, tłumaczenie jest po prostu
```csharp
( e )
```

Na przykład
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
po prostu przetłumaczyć
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a>Groupby klauzule

W wyrażeniu kwerendy w postaci
```csharp
from x in e group v by k
```
jest tłumaczony na
```csharp
( e ) . GroupBy ( x => k , x => v )
```
z wyjątkiem sytuacji, gdy v jest identyfikator x, tłumaczenie jest
```csharp
( e ) . GroupBy ( x => k )
```

Przykład
```csharp
from c in customers
group c.Name by c.Country
```
jest tłumaczony na
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a>Identyfikatory przezroczyste

Niektóre tłumaczenia wstrzyknąć zmiennych zakresu przy użyciu ***przezroczyste identyfikatory*** wskazywane przez `*`. Identyfikatory przezroczysty nie są funkcją języka właściwego; istnieją one tylko jako pośredniego kroku w procesie tłumaczenia wyrażenia zapytania.

Podczas translacji zapytania wprowadza użyciem przezroczystego identyfikatora, dalszych kroków translacji propagować użyciem przezroczystego identyfikatora do inicjatory obiekt anonimowy i funkcjami anonimowymi. W tych kontekstach przezroczyste identyfikatory mają następujące zachowanie:

*  W przypadku przezroczyste identyfikator jako parametr w funkcją anonimową, elementy członkowskie typu anonimowego skojarzone są wykonywane automatycznie w zakresie treści funkcji anonimowych.
*  Element członkowski z użyciem przezroczystego identyfikatora znajduje się w zakresie, elementy członkowskie tego członka należą również zakres.
*  W przypadku użyciem przezroczystego identyfikatora wystąpienia jako deklarator składowej w inicjatorze obiektu anonimowego wprowadza element członkowski z użyciem przezroczystego identyfikatora.
*  W tłumaczenia opisane powyżej przejrzyste identyfikatory są zawsze wprowadzone wraz z anonimowych typów, z celem przechwytywania wielu zmiennych zakresu jako elementy członkowskie pojedynczego obiektu. Implementacja języka C# jest dozwolone mechanizm innego niż typy anonimowe umożliwiają grupowanie wielu zmiennych zakresu. W poniższych przykładach tłumaczenia założono anonimowych typów są używane i pokazują, jak przezroczyste identyfikatory mogą być tłumaczone natychmiast.

Przykład
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
jest tłumaczony na
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

który jest dalsze przetłumaczyć
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
które, gdy usuwane są identyfikatory przezroczyste, jest odpowiednikiem
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
gdzie `x` jest identyfikatorem wygenerowanego przez kompilator, który jest niewidoczna i są niedostępne.

Przykład
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
jest tłumaczony na
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
który jest dalsze skrócony do
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
ostateczne tłumaczenie jest
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
gdzie `x`, `y`, i `z` to identyfikatory wygenerowanego przez kompilator, które w przeciwnym razie są niewidoczne i są niedostępne.

### <a name="the-query-expression-pattern"></a>Wzorzec wyrażenia zapytania

***Wzorzec wyrażenia kwerendy*** ustanawia wzorzec metod, których typy można zaimplementować w celu obsługi wyrażenia zapytania. Ponieważ wyrażenia kwerendy są przekształcane wywołań metod za pomocą składni mapowania, typy mają znaczną elastyczność w zakresie sposobu wdrażania do wzorca wyrażenia zapytania. Na przykład metody wzorca można można zaimplementować jako wystąpienie metody lub metody rozszerzenia ponieważ dwa mieć tej samej składni wywołania i metody mogą żądać delegatów lub drzew wyrażeń, ponieważ funkcje anonimowe są konwertowane na wartość oba.

Zalecane kształt typu rodzajowego `C<T>` , obsługuje wzorzec wyrażenia kwerendy znajdują się poniżej. Typ ogólny jest używana w celu zilustrowania odpowiednie relacje między typami parametrów i wynik, ale istnieje możliwość zaimplementowania wzorca dla typów innych niż ogólne także.

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

Powyższych metod korzystają z typów Delegat ogólny `Func<T1,R>` i `Func<T1,T2,R>`, ale może równie dobrze korzystali innych typów delegat lub wyrażenie drzewa za pomocą tej samej relacji w typach parametrów i wynik.

Zwróć uwagę, zalecane relacji między `C<T>` i `O<T>` , dzięki któremu `ThenBy` i `ThenByDescending` metody są dostępne tylko w wyniku `OrderBy` lub `OrderByDescending`. Należy również zauważyć zalecane kształt wyniku `GroupBy` — sekwencji, gdzie każda sekwencja wewnętrzny ma dodatkowy `Key` właściwości.

`System.Linq` Przestrzeń nazw zawiera implementację wzorca operatora zapytania dla dowolnego typu, który implementuje `System.Collections.Generic.IEnumerable<T>` interfejsu.

## <a name="assignment-operators"></a>Operatory przypisania

Operatory przypisania przypisać nową wartość do zmiennej, właściwości, zdarzenia lub elementu indeksatora.

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

Lewy operand przypisania musi być wyrażeniem sklasyfikowane jako zmienną, dostęp do właściwości, indeksatora lub zdarzenia dostęp do.

`=` Operator jest nazywany ***operator przypisania prostego***. Przypisuje wartość prawy operand do zmiennej, właściwość lub indeksator elementu przez lewy operand. Lewy operand prosty operator przypisania nie może być dostęp do zdarzenia (z wyjątkiem sytuacji opisanej w [zdarzenia podobne do pól](classes.md#field-like-events)). Operator przypisania prostego jest opisana w [przypisanie proste](expressions.md#simple-assignment).

Operatory przypisania w innych niż `=` operator są nazywane ***złożone operatory przypisania***. Te operatory wykonać operacji wskazanych w dwóch argumentów operacji, a następnie przypisz wynikowej wartości do zmiennej, właściwość lub indeksator elementu przez lewy operand. Złożone operatory przypisania są opisane w [przydział złożony](expressions.md#compound-assignment).

`+=` i `-=` operatory o wyrażenie dostępu do zdarzeń jako lewy operand są nazywane *operatory przypisania zdarzeń*. Żaden operator przypisywania jest prawidłowy dostęp do zdarzenia jako lewy operand. Operatory przypisania zdarzeń są opisane w [przypisanie zdarzenia](expressions.md#event-assignment).

Operatory przypisania są łączność do prawej strony, co oznacza, że operacje są pogrupowane od prawej do lewej. Na przykład, wyrażenie w formie `a = b = c` jest oceniane jako `a = (b = c)`.

### <a name="simple-assignment"></a>Przypisanie proste

`=` Operator jest nazywany prosty operator przypisania.

Jeśli lewy operand przypisanie proste ma postać `E.P` lub `E[Ei]` gdzie `E` ma typ kompilacji `dynamic`, a następnie przypisanie dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)). W tym przypadku jest typu wyrażenia przypisania w czasie kompilacji `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania, na podstawie typu środowiska wykonawczego `E`.

W proste zadanie prawy operand musi być wyrażeniem, który jest niejawnie konwertowany na typ operandu po lewej stronie. Operacja przypisuje wartość prawy operand do zmiennej, właściwość lub indeksator elementu przez lewy operand.

Wynik wyrażenia przypisanie proste jest wartość przypisana do lewy operand. Wynik ma ten sam typ jako lewy operand i zawsze jest klasyfikowana jako wartość.

Jeśli lewy operand jest dostępu właściwość lub indeksator, właściwość lub indeksator musi mieć `set` metody dostępu. Jeśli nie jest to możliwe, występuje błąd wiązania.

Przetwarzanie środowiska wykonawczego przypisanie proste formularza `x = y` składa się z następujących czynności:

*  Jeśli `x` zostanie sklasyfikowany jako zmienną:
   * `x` jest oceniany w celu utworzenia zmiennej.
   * `y` jest sprawdzane, a w razie potrzeby konwertowane na typ `x` za pośrednictwem niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)).
   * Jeśli zmienna, podane przez `x` jest element tablicy *reference_type*, sprawdzanie w czasie wykonania, odbywa się na upewnij się, że wartość obliczona dla `y` jest zgodny z wystąpieniem tablicy `x` jest element. Sprawdzanie zakończy się pomyślnie, jeśli `y` jest `null`, lub jeśli niejawna konwersja odwołania ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) istnieje rzeczywisty typ wystąpienia odwołuje się `y` do typu rzeczywistego elementu wystąpienie tablicy zawierającego `x`. W przeciwnym razie `System.ArrayTypeMismatchException` zgłaszany.
   * Wartość wynikające z oceny i konwersja `y` są przechowywane w lokalizacji określonej przez ocenę `x`.
*  Jeśli `x` zostanie sklasyfikowany jako właściwość lub indeksator dostępu:
   * Wyrażenia wystąpienia (Jeśli `x` nie jest `static`) i listą argumentów (Jeśli `x` jest dostęp indeksatora) skojarzony z `x` są oceniane, a wyniki są używane w kolejnych żadaniach `set` wywołania metody dostępu.
   * `y` jest sprawdzane, a w razie potrzeby konwertowane na typ `x` za pośrednictwem niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)).
   * `set` Akcesor `x` jest wywoływana z wartością obliczane `y` jako jego `value` argumentu.

Reguły odchyleń wspólnej tablicy ([Kowariancja tablicy](arrays.md#array-covariance)) zezwala na wartość typu tablicowego `A[]` jako odwołanie do wystąpienia typu tablicowego `B[]`, o ile istnieje niejawna konwersja odwołania `B` do `A`. Ze względu na tych zasad, przypisanie do elementu tablicy *reference_type* wymaga sprawdzenia środowiska wykonawczego, aby upewnić się, że wartość jest przypisana jest zgodna z wystąpienia tablicy. W przykładzie
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
powoduje, że ostatnie przypisanie `System.ArrayTypeMismatchException` zostanie wygenerowany, ponieważ wystąpienie `ArrayList` nie mogą być przechowywane w elemencie `string[]`.

Gdy właściwość lub indeksator zadeklarowanych w *struct_type* jest elementem docelowym przypisania, wyrażenia wystąpienia skojarzony z właściwością lub dostęp indeksatora musi być klasyfikowane jako zmienną. Wyrażenia wystąpienia jest klasyfikowana jako wartość, czas powiązania błędu. Z powodu [dostęp do elementu członkowskiego](expressions.md#member-access), ta sama zasada dotyczy również pola.

Podane deklaracje:
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
W przykładzie
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
przypisania do `p.X`, `p.Y`, `r.A`, i `r.B` są dozwolone, ponieważ `p` i `r` zmiennych. Jednak w przykładzie
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
przydziały są wszystkie nieprawidłowe, ponieważ `r.A` i `r.B` nie są zmiennymi.

### <a name="compound-assignment"></a>Przydział złożony

Jeśli lewy operand przypisania złożonego mają postać `E.P` lub `E[Ei]` gdzie `E` ma typ kompilacji `dynamic`, a następnie przypisanie dynamicznie jest powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)). W tym przypadku jest typu wyrażenia przypisania w czasie kompilacji `dynamic`, i rozpoznawanie opisanych poniżej będzie odbywać się w czasie wykonywania, na podstawie typu środowiska wykonawczego `E`.

Operacja formularza `x op= y` jest przetwarzany przez zastosowanie operatora binarnego funkcja rozpoznawania przeciążeń ([Rozpoznanie przeciążenia operatora binarnego](expressions.md#binary-operator-overload-resolution)) tak, jakby operacji został napisany `x op y`. Następnie

*  Jeśli typ zwracany operatora niejawnej konwersji na typ `x`, operacja jest oceniane jako `x = x op y`, chyba że `x` jest oceniane tylko raz.
*  W przeciwnym razie, jeśli wybrany operator jest wstępnie zdefiniowanego operatora, jeśli typ zwracany operatora jest jawnie konwertowany na typ `x`i jeśli `y` jest niejawnie konwertowany na typ `x` lub operatora operator, przesunięcia, a następnie operacji jest obliczana jako `x = (T)(x op y)`, gdzie `T` jest typem `x`, chyba że `x` jest oceniane tylko raz.
*  W przeciwnym razie przypisania złożonego jest nieprawidłowy i występuje błąd wiązania.

Termin "oceniane tylko raz," oznacza, że podczas obliczania `x op y`, wyniki żadnych składowych wyrażeń `x` zapisywane tymczasowo i następnie ponownie użyte podczas przeprowadzania przypisanie do `x`. Na przykład w przypisaniu `A()[B()] += C()`, gdzie `A` jest metodą, zwracając `int[]`, i `B` i `C` metod zwracanie `int`, te metody są wywoływane tylko raz, w kolejności `A`, `B`, `C`.

Jeśli lewy operand przypisania złożonego jest dostęp do właściwości lub indeksatora dostępu, właściwość lub indeksator musi mieć zarówno `get` metody dostępu i `set` metody dostępu. Jeśli nie jest to możliwe, występuje błąd wiązania.

Druga reguła powyżej zezwala `x op= y` mogło zostać ocenione jako `x = (T)(x op y)` w określonych kontekstach. Istnieje reguła w taki sposób, że wstępnie zdefiniowanych operatory mogą być używane jako operatory złożone, kiedy lewy operand jest typu `sbyte`, `byte`, `short`, `ushort`, lub `char`. Nawet gdy oba argumenty mają jeden z tych typów, wstępnie zdefiniowanych operatorów daje wynik o typie `int`, zgodnie z opisem w [binarne promocji liczbowych](expressions.md#binary-numeric-promotions). W związku z tym bez rzutowania nie byłoby możliwe Przypisz wynik do lewy operand.

Intuicyjne wpływu reguły dla wstępnie zdefiniowanego operatorów jest po prostu `x op= y` jest dozwolone, jeśli obie z `x op y` i `x = y` są dozwolone. W przykładzie
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
intuicyjne Przyczyna, dla każdego błędu jest, czy odpowiednie przypisanie proste również byłoby błąd.

Oznacza to, że przydział złożony, który obsługuje operacje zniesione operacji. W przykładzie
```csharp
int? i = 0;
i += 1;             // Ok
```
operator podniesionym `+(int?,int?)` jest używany.

### <a name="event-assignment"></a>Przypisanie zdarzenia

Jeśli lewy operand `+=` lub `-=` operator jest klasyfikowana jako dostęp do zdarzenia, a następnie wyrażenie jest obliczane w następujący sposób:

*  Wyrażenia wystąpienia, dostępu do zdarzeń jest oceniany.
*  Prawy operand `+=` lub `-=` operator jest obliczane i, w razie potrzeby konwertowane na typ lewy operand za pośrednictwem niejawnej konwersji ([niejawne konwersje](conversions.md#implicit-conversions)).
*  Metoda dostępu do zdarzeń zdarzenia jest wywoływana, z listy argumentów, składający się z prawy operand po dokonaniu oceny oraz, w razie potrzeby konwersji. Jeśli operator `+=`, `add` metody dostępu jest zalecane, jeśli był operator `-=`, `remove` zostanie wywołana metoda dostępu.

Wyrażenie przypisania zdarzeń nie przekazuje wartość. W związku z tym, wyrażenie przypisania zdarzenia jest prawidłowy tylko w kontekście *statement_expression* ([instrukcje wyrażeń](statements.md#expression-statements)).

## <a name="expression"></a>Wyrażenie

*Wyrażenie* jest *non_assignment_expression* lub *przypisania*.

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

A *constant_expression* jest wyrażeniem, które mogą zostać w pełni oszacowane w czasie kompilacji.

```antlr
constant_expression
    : expression
    ;
```

Musi być wyrażeniem stałym `null` literałem lub wartość składającą się z jednym z następujących typów: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, `bool`, `object`, `string`, lub dowolny typ wyliczenia. W wyrażeniach stałych dopuszczalne są następujące elementy:

*  Literały (w tym `null` literału).
*  Odwołuje się do `const` składowych typu klasy i struktury.
*  Odwołania do elementów członkowskich typów wyliczeń.
*  Odwołuje się do `const` parametry lub zmienne lokalne
*  Ujęty w nawiasy wyrażeń podrzędnych, które są także wyrażeń stałych.
*  Wyrażenia CAST, podać typ docelowy jest jednym z typów wymienionych powyżej.
*  `checked` i `unchecked` wyrażeń
*  Wyrażenia wartości domyślnych
*  Wyrażenie Nameof
*  Wstępnie zdefiniowane `+`, `-`, `!`, i `~` operatorów jednoargumentowych.
*  Wstępnie zdefiniowane `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, i `>=` operatory dwuargumentowe, pod warunkiem, każdy argument jest typu wymienionych powyżej.
*  `?:` Operatora warunkowego.

Poniższe konwersje są dozwolone w wyrażeniach stałych:

*  Konwersje tożsamości
*  Konwersje liczbowe
*  Konwersje wyliczenia
*  Konwersje stałego wyrażenia
*  Konwersje jawne i niejawne odwołanie, pod warunkiem że źródło konwersje jest wyrażeniem stałym, którego wynikiem jest wartość null.

Innych konwersji, w tym pakowanie, Rozpakowywanie i niejawne konwersje odwołań z innych niż null wartości nie są dozwolone w wyrażeń stałych. Na przykład:
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
Inicjowanie i występuje błąd, ponieważ wymagana jest konwersja boxing. Inicjowanie str występuje błąd, ponieważ niejawna konwersja odwołania z wartości innej niż null jest wymagany.

Zawsze, gdy wyrażenie spełnia wymagania wymienione powyżej, wyrażenie jest obliczane w czasie kompilacji. Ta zasada obowiązuje, nawet wtedy, gdy wyrażenie jest większego wyrażenia zawierającego konstrukcje niestałe wyrażenie podrzędne.

Ocena kompilacji wyrażeń stałych używa te same reguły jako czasu wykonywania oceny wyrażeń stałą, z wyjątkiem tego, gdzie czasu wykonywania oceny będzie mieć zgłoszony wyjątek, ocena kompilacji powoduje błąd kompilacji występuje.

Chyba, że wyrażenie stałe jest jawnie umieszczona na `unchecked` kontekstu, przepełnienia, które występują w typu całkowitego operacjach arytmetycznych i konwersjach podczas obliczania kompilacji wyrażenia zawsze powodują błędy czasu kompilacji ([Wyrażeń stałych](expressions.md#constant-expressions)).

Wyrażenia stałe występują w kontekstach wymienione poniżej. W tych kontekstach błąd kompilacji występuje, jeśli wyrażenie nie może zostać w pełni oszacowane w czasie kompilacji.

*  Deklaracje stałych ([stałe](classes.md#constants)).
*  Deklaracji elementu członkowskiego wyliczenia ([typu wyliczeniowego](enums.md#enum-members)).
*  Domyślne argumenty list parametrów formalnych ([parametry metody](classes.md#method-parameters))
*  `case` etykiety `switch` — instrukcja ([instrukcji switch](statements.md#the-switch-statement)).
*  `goto case` instrukcje ([instrukcji goto](statements.md#the-goto-statement)).
*  Wymiar długości w wyrażenie tworzenia tablicy ([wyrażenie tworzenia tablicy](expressions.md#array-creation-expressions)) zawierającej inicjatora.
*  Atrybuty ([atrybuty](attributes.md)).

Wyrażenie stałe w niejawnych konwersji ([konwersje niejawne wyrażenie stałe](conversions.md#implicit-constant-expression-conversions)) pozwala na stałe wyrażenie typu `int` są konwertowane na `sbyte`, `byte`, `short`, `ushort`, `uint`, lub `ulong`, o ile wartości stałe wyrażenia znajduje się w zakresie typu miejsca docelowego.

## <a name="boolean-expressions"></a>Wyrażenia logiczne

A *boolean_expression* jest wyrażeniem, które daje wynik o typie `bool`; albo bezpośrednio lub za pośrednictwem aplikacji `operator true` w pewnych kontekstach, jak określono poniżej.

```antlr
boolean_expression
    : expression
    ;
```

Kontrolowanie wyrażenie warunkowe *if_statement* ([if — instrukcja](statements.md#the-if-statement)), *while_statement* ([while, instrukcja](statements.md#the-while-statement)), *do_statement* ([instrukcji](statements.md#the-do-statement)), lub *for_statement* ([dla instrukcji](statements.md#the-for-statement)) jest *boolean_ wyrażenie*. Kontrolowanie wyrażenie warunkowe `?:` — operator ([operator warunkowy](expressions.md#conditional-operator)) te same reguły jako *boolean_expression*, ale ze względów operatora jest klasyfikowany pierwszeństwo jako *conditional_or_expression*.

A *boolean_expression* `E` jest wymagana, aby można było utworzyć wartości typu `bool`, wykonując następujące czynności:

*  Jeśli `E` jest niejawnie konwertowany na `bool` , a następnie w czasie wykonywania jest stosowany ten niejawnej konwersji.
*  W przeciwnym razie przeciążenia operatora jednoargumentowego ([Rozpoznanie przeciążenia operatora jednoargumentowego](expressions.md#unary-operator-overload-resolution)) jest używana do znajdowania unikatowy stosowania najlepszych operator `true` na `E`, i że wykonanie jest stosowane w czasie wykonywania.
*  Jeśli zostanie znaleziony żaden operator takich, występuje błąd wiązania.

`DBBool` Typ struktury w [bazy danych typu boolean](structs.md#database-boolean-type) stanowi przykład typ, który implementuje `operator true` i `operator false`.
