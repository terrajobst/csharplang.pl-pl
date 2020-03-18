---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/26/2020
ms.locfileid: "79485211"
---
# <a name="nullable-reference-types-specification"></a>Specyfikacja typów odwołań dopuszczających wartość null

Jest to w toku — brakuje kilku części lub są one niekompletne. ***

## <a name="syntax"></a>Składnia

### <a name="nullable-reference-types"></a>Typy referencyjne dopuszczające wartość null

Typy referencyjne dopuszczające wartość null mają tę samą składnię `T?` co krótka forma wartości null, ale nie ma odpowiedniej długiej formy.

Na potrzeby specyfikacji bieżąca `nullable_type` produkcyjna zostanie zmieniona na `nullable_value_type`i zostanie dodana `nullable_reference_type` produkcyjna:

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

`non_nullable_reference_type` w `nullable_reference_type` musi być typem odwołania niedopuszczający wartości null (klasą, interfejsem, delegatem lub tablicą) lub parametrem typu, który jest ograniczony jako typ referencyjny niedopuszczający wartości null (za pomocą ograniczenia `class` lub klasy innej niż `object`).

Typy referencyjne dopuszczające wartość null nie mogą wystąpić w następujących położeniach:

- jako klasa bazowa lub interfejs
- jako odbiornik `member_access`
- jak `type` w `object_creation_expression`
- jak `delegate_type` w `delegate_creation_expression`
- jak `type` w `is_expression`, `catch_clause` lub `type_pattern`
- jako `interface` w w pełni kwalifikowanej nazwie elementu członkowskiego interfejsu

Ostrzeżenie jest udzielane na `nullable_reference_type`, gdzie kontekst adnotacji dopuszczający wartość null jest wyłączony.

### <a name="nullable-class-constraint"></a>Ograniczenie klasy dopuszczające wartość null

Ograniczenie `class` ma odpowiednik wartości null `class?`:

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a>Operator łagodniejszej o wartości null

Operator `!` po naprawieniu jest nazywany operatorem łagodniejszej o wartości null.

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

`primary_expression` musi być typu referencyjnego.  

Przyrostkowy operator `!` nie ma efektu środowiska uruchomieniowego — oblicza wynik wyrażenia bazowego. Jedyną rolą jest zmiana stanu wartości null wyrażenia i ograniczenie ostrzeżeń podawanych w jego użyciu.

### <a name="nullable-implicitly-typed-local-variables"></a>niejawnie wpisane wartości null zmienne lokalne

`var` wnioskuje typ z adnotacjami dla typów referencyjnych.
Na przykład, w `var s = "";` `var` jest wywnioskowany jako `string?`.

### <a name="nullable-compiler-directives"></a>Dyrektywy kompilatora dopuszczające wartość null

dyrektywy `#nullable` kontrolują adnotacje dopuszczające wartość null i konteksty ostrzeżenia.

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

dyrektywy `#pragma warning` są rozwinięte, aby umożliwić zmianę kontekstu ostrzeżeń o wartości null i Zezwalanie na włączenie poszczególnych ostrzeżeń nawet wtedy, gdy są one domyślnie wyłączone:

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

Należy zauważyć, że nowa forma `pragma_warning_body` używa `nullable_action`, a nie `warning_action`.

## <a name="nullable-contexts"></a>Konteksty dopuszczające wartość null

Każdy wiersz kodu źródłowego ma *kontekst adnotacji dopuszczający wartości null* i *kontekst ostrzegawczy do wartości null*. Kontrolują, czy adnotacje dopuszczające wartość null mają wpływ, oraz czy są podane ostrzeżenia o wartości null. Kontekst adnotacji danego wiersza jest *wyłączony* lub *włączony*. Kontekst ostrzeżenia danego wiersza jest *wyłączony* lub *włączony*.

Oba konteksty można określić na poziomie projektu (poza kodem C# źródłowym) lub w dowolnym miejscu w pliku źródłowym za pośrednictwem dyrektyw przedprocesora `#nullable`. Jeśli nie zostaną podane żadne ustawienia poziomu projektu, domyślnie dla obu kontekstów należy *wyłączyć*.

Dyrektywa `#nullable` kontroluje kontekst adnotacji i ostrzeżeń w tekście źródłowym i ma pierwszeństwo przed ustawieniami poziomu projektu.

Dyrektywa ustawia konteksty, które kontroluje dla kolejnych wierszy kodu, do momentu przesłania przez inną dyrektywę lub do końca pliku źródłowego.

Efekty dyrektyw są następujące:

- `#nullable disable`: określa, że adnotacja dopuszczająca wartość null i konteksty ostrzeżeń mają być *wyłączone*
- `#nullable enable`: ustawia do *włączenia* adnotację z dopuszczaniem wartości null i kontekstów ostrzeżeń
- `#nullable restore`: przywraca ustawienia projektu adnotacji z dopuszczaniem wartości null i ostrzeżeń
- `#nullable disable annotations`: ustawia kontekst adnotacji dopuszczającej wartość null na *wyłączony*
- `#nullable enable annotations`: ustawia kontekst adnotacji dopuszczający *enabled* wartość null
- `#nullable restore annotations`: przywraca ustawienia projektu dla kontekstu adnotacji wartości null
- `#nullable disable warnings`: ustawia dla *wyłączonego* kontekstu ostrzeżeń wartość null
- `#nullable enable warnings`: ustawia kontekst ostrzegawczy dopuszczający *enabled* wartość null
- `#nullable restore warnings`: przywraca ustawienia projektu dla kontekstu ostrzeżenia wartości null

## <a name="nullability-of-types"></a>Wartość zerowa typów

Dany typ może mieć jedną z czterech nullabilities: *Oblivious* *, null,* *nullable* i *Unknown*. 

*Niedopuszczające wartości null* i *nieznane* typy mogą powodować ostrzeżenia, jeśli do nich zostanie przypisana potencjalna wartość `null`. Typy *Oblivious* i *nullable dopuszczają* jednak wartość "*null-przypisanie*" i mogą mieć przypisane wartości `null` bez ostrzeżeń. 

Typy *Oblivious* i *niedopuszczające wartości null* mogą zostać wyznaczone lub przypisane bez ostrzeżeń. Wartości *dopuszczające wartość null* i *nieznane* typy, jednak są "o*wartości null*" i mogą spowodować ostrzeżenia w przypadku wyróżnienia lub przypisania bez prawidłowej kontroli wartości null. 

*Domyślny stan zerowy* typu zwracanego wartością null to "może mieć wartość null". Domyślny stan o wartości null dla niezerowego typu zwracanego to "not null".

Rodzaj typu i kontekst adnotacji dopuszczający wartość null, występujący w ustalaniu jego wartości zerowej:

- Typ wartości niezerowej `S` jest zawsze *niezerowy*
- Typ wartości null `S?` jest zawsze *dopuszczany do wartości null*
- Typ referencyjny bez adnotacji `C` w *wyłączonym* kontekście adnotacji to *Oblivious*
- Typ referencyjny bez adnotacji `C` w *włączonym* kontekście adnotacji jest *niezerowy*
- Typ referencyjny dopuszczający wartość null `C?` *dopuszcza wartość null* (ale ostrzeżenie może być wynikiem *wyłączonego* kontekstu adnotacji)

Parametry typu dodatkowo przyjmują ograniczenia do konta:

- Parametr typu `T`, gdzie wszystkie ograniczenia (jeśli istnieją) mają typy zwracające wartość null (*nullable* i *Unknown*) lub ograniczenie `class?` jest *nieznane*
- Parametr typu `T`, gdzie co najmniej jedno ograniczenie ma wartość *Oblivious* lub nie może *mieć wartości null* albo jeden z `struct` lub `class` ograniczeniami jest
    - *Oblivious* w *wyłączonym* kontekście adnotacji
    - Brak możliwości *null* w *włączonym* kontekście adnotacji
- Parametr typu dopuszczającego wartość null `T?`, gdzie co najmniej jeden z ograniczeń `T`jest *Oblivious* lub ma *wartość null* lub jeden z ograniczeń `struct` lub `class`, jest
    - *dopuszczanie wartości null* w *wyłączonym* kontekście adnotacji (ale jest wyświetlane ostrzeżenie)
    - *dopuszczanie wartości null* w kontekście *włączonej* adnotacji

Dla parametru typu `T``T?` jest dozwolone tylko wtedy, gdy `T` jest znany typ wartości lub znany jako typ referencyjny.

### <a name="oblivious-vs-nonnullable"></a>Oblivious vs o wartości null

`type` jest uznawana za występuje w danym kontekście adnotacji, gdy ostatni token typu znajduje się w tym kontekście.

Czy dany typ referencyjny `C` w kodzie źródłowym jest interpretowany jako Oblivious lub niedopuszczający wartości null, zależy od kontekstu adnotacji tego kodu źródłowego. Jednak po ustanowieniu jest uważany za część tego typu i "podróżuje", np. podczas podstawiania argumentów typu ogólnego. Jest tak, jakby Adnotacja, taka jak `?` w typie, ale niewidoczna.

## <a name="constraints"></a>Ograniczenia

Typów referencyjnych dopuszczających wartość null można używać jako ograniczeń ogólnych. Ponadto `object` jest teraz prawidłowym ograniczeniem. Brak ograniczenia jest teraz równoznaczne z ograniczeniem `object?` (zamiast `object`), ale (w przeciwieństwie do `object` przed) `object?` nie jest zakazane jako jawne ograniczenie.

`class?` jest nowym ograniczeniem wskazującym "możliwy do dołączenia typ referencyjny", podczas gdy `class` oznacza "niezerowy typ odwołania".

Wartość null argumentu typu lub ograniczenia nie ma wpływu na to, czy typ spełnia ograniczenie, chyba że jest to już dziś, (typy wartości null nie spełniają ograniczenia `struct`). Jeśli jednak argument typu nie spełnia wymagań dotyczących wartości NULL ograniczenia, może zostać wyświetlone ostrzeżenie.

## <a name="null-state-and-null-tracking"></a>Stan o wartości null i śledzenie wartości null

Każde wyrażenie w podanej lokalizacji źródłowej ma *stan null*, który wskazuje, że istnieje prawdopodobieństwo, że może on zostać obliczony na wartość null. Stan o wartości null ma wartość "not null" lub "może mieć wartość null". Stan null służy do określenia, czy należy podać ostrzeżenie dotyczące konwersji i dereferencji z niebezpiecznymi wartościami null.

### <a name="null-tracking-for-variables"></a>Śledzenie wartości null dla zmiennych

W przypadku niektórych wyrażeń wskazujących zmienne lub właściwości stan o wartości null jest śledzony między wystąpieniami, na podstawie przypisań do nich, testów wykonywanych na nich i przepływów sterowania między nimi. Jest to podobne do tego, jak określone przypisanie jest śledzone dla zmiennych. Śledzone wyrażenia są tymi, które są następujące:

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

Gdzie są identyfikatory pól lub właściwości.

***Opisz przejścia stanu o wartości null podobne do określonego przypisania***

### <a name="null-state-for-expressions"></a>Stan zerowy dla wyrażeń

Stan o wartości null wyrażenia jest pochodną jego formy i typu oraz od wartości null zmiennych, w których to dotyczy.

### <a name="literals"></a>Literały

Stan null literału `null` ma wartość "null". Stan null literału `default`, który jest konwertowany na typ, który nie jest typem wartości o wartości null, jest "może mieć wartość null". Stan null dowolnego innego literału to "not null".

### <a name="simple-names"></a>Proste nazwy

Jeśli `simple_name` nie jest sklasyfikowana jako wartość, jej stanem null jest "not null". W przeciwnym razie jest to śledzone wyrażenie, a jego stan null jest jego śledzony stan o wartości null w tej lokalizacji źródłowej.

### <a name="member-access"></a>Dostęp do elementu członkowskiego

Jeśli `member_access` nie jest sklasyfikowana jako wartość, jej stanem null jest "not null". W przeciwnym razie, jeśli jest to śledzone wyrażenie, jego stan o wartości null jest jego śledzonym stanem null w tej lokalizacji źródłowej. W przeciwnym razie stan null jest domyślnym stanem null jego typu.

### <a name="invocation-expressions"></a>Wyrażenia wywołania

Jeśli `invocation_expression` wywołuje składową, która jest zadeklarowana z co najmniej jednym atrybutem dla specjalnego zachowania null, stan null jest określany przez te atrybuty. W przeciwnym razie stan null wyrażenia jest domyślnym stanem null jego typu.

### <a name="element-access"></a>Dostęp do elementu

Jeśli `element_access` wywoła indeksator, który jest zadeklarowany z co najmniej jednym atrybutem dla specjalnego zachowania null, stan null jest określany przez te atrybuty. W przeciwnym razie stan null wyrażenia jest domyślnym stanem null jego typu.

### <a name="base-access"></a>Dostęp podstawowy

Jeśli `B` oznacza typ podstawowy typu otaczającego, `base.I` ma taki sam stan null jak `((B)this).I`, a `base[E]` ma ten sam stan null jak `((B)this)[E]`.

### <a name="default-expressions"></a>Wyrażenia domyślne

`default(T)` ma stan null "non-null", jeśli `T` jest znany jako typ wartości o wartości null. W przeciwnym razie ma stan null "może mieć wartość null".

### <a name="null-conditional-expressions"></a>Wyrażenia warunkowe o wartości null

`null_conditional_expression` ma stan null "może mieć wartość null".

### <a name="cast-expressions"></a>Wyrażenia rzutowania

Jeśli wyrażenie rzutowania `(T)E` wywołuje konwersję zdefiniowaną przez użytkownika, stan null wyrażenia jest domyślnym stanem null dla tego typu. W przeciwnym razie, jeśli `T` ma wartość null (*nullable* lub *Unknown*), oznacza to, że wartość zerowa to "może mieć wartość null". W przeciwnym razie stan null jest taki sam jak stan null `E`.

### <a name="await-expressions"></a>Wyrażenia await

Stan null `await E` jest domyślnym stanem null jego typu.

### <a name="the-as-operator"></a>Operator `as`

Wyrażenie `as` ma stan null "może mieć wartość null".

### <a name="the-null-coalescing-operator"></a>Operator łączenia wartości null

`E1 ?? E2` ma ten sam stan zerowy co `E2`

### <a name="the-conditional-operator"></a>Operator warunkowy

Stan o wartości null `E1 ? E2 : E3` ma wartość "not null", jeśli stan null obu `E2` i `E3` to "not null". W przeciwnym razie jest to "może mieć wartość null".

### <a name="query-expressions"></a>Wyrażenia zapytań

Stan null wyrażenia zapytania jest domyślnym stanem null jego typu.

### <a name="assignment-operators"></a>Operatory przypisania

`E1 = E2` i `E1 op= E2` mają taki sam stan null jak `E2` po zastosowaniu niejawnych konwersji.

### <a name="unary-and-binary-operators"></a>Operatory jednoargumentowe i binarne

Jeśli operator jednoargumentowy lub dwuargumentowy wywołuje zdefiniowany przez użytkownika operator, który jest zadeklarowany z co najmniej jednym atrybutem dla specjalnego zachowania null, stan null jest określany przez te atrybuty. W przeciwnym razie stan null wyrażenia jest domyślnym stanem null jego typu.

***Coś specjalnego do wykonywania w przypadku `+` binarnych za pośrednictwem ciągów i delegatów?***

### <a name="expressions-that-propagate-null-state"></a>Wyrażenia propagowania stanu o wartości null

`(E)`, `checked(E)` i `unchecked(E)` wszystkie mają taki sam stan null jak `E`.

### <a name="expressions-that-are-never-null"></a>Wyrażenia, które nigdy nie mają wartości null

Stan zerowy następujących formularzy wyrażeń ma zawsze wartość "not null":

- dostęp `this`
- ciągi interpolowane
- wyrażenia `new` (obiekt, delegat, obiekt anonimowy i wyrażenia tworzenia tablicy)
- wyrażenia `typeof`
- wyrażenia `nameof`
- funkcje anonimowe (anonimowe metody i wyrażenia lambda)
- wyrażenia łagodniejszej o wartości null
- wyrażenia `is`

## <a name="type-inference"></a>Wnioskowanie o typie

### <a name="type-inference-for-var"></a>Wnioskowanie o typie dla `var`

Typ wywnioskowany dla zmiennych lokalnych zadeklarowanych za pomocą `var` jest informowany o stanie null wyrażenia inicjowania.

```csharp
var x = E;
```

Jeśli typ `E` jest typem referencyjnym dopuszczającym wartość null `C?` a stan null `E` ma wartość "not null", typ wywnioskowany dla `x` to `C`. W przeciwnym razie typ wnioskowany jest typem `E`.

Wartość null typu wywnioskowanego dla `x` jest określana zgodnie z opisem powyżej, na podstawie kontekstu adnotacji `var`, podobnie jak w przypadku, gdy typ został jawnie określony w tym położeniu.

### <a name="type-inference-for-var"></a>Wnioskowanie o typie dla `var?`

Typ wywnioskowany dla zmiennych lokalnych zadeklarowanych za pomocą `var?` jest niezależny od stanu null wyrażenia inicjowania.

```csharp
var? x = E;
```

Jeśli typ `T` `E` jest typem wartości null lub typem referencyjnym dopuszczającym wartość null, wówczas typ, który został wywnioskowany dla `x` jest `T`. W przeciwnym razie, jeśli `T` jest typem wartości o wartości null `S` wywnioskowanym typem jest `S?`. W przeciwnym razie, jeśli `T` jest typem odwołania o wartości null, `C` wnioskowany typ jest `C?`. W przeciwnym razie deklaracja jest niedozwolona.

Wartość null typu wywnioskowanego dla `x` zawsze *dopuszcza wartość null*.

### <a name="generic-type-inference"></a>Wnioskowanie o typie ogólnym

Wnioskowanie o typie ogólnym jest rozszerzane, aby ułatwić podjęcie decyzji, czy wywnioskowane typy odwołań powinny mieć wartość null, czy nie. Jest to najlepszy nakład pracy, a nie w i sama sama z nich, ale może prowadzić do dopuszczających wartość null ostrzeżeń, gdy wywnioskowane typy wybranych przeciążeń są stosowane do argumentów.

Wnioskowanie o typie nie polega na kontekście adnotacji typów przychodzących. Zamiast tego zostanie wywnioskowany `type`, który uzyskuje własny kontekst adnotacji, w którym "byłby", jeśli został on jawnie wyrażony. Jest to podkreślenie roli typu wnioskowania jako wygoda do tego, co można napisać samodzielnie.

Dokładniej, kontekst adnotacji dla argumentu wywnioskowanego typu jest kontekstem tokenu, który miał być poprzedzony listą parametrów typu `<...>`. tj. nazwa wywoływanej metody ogólnej. Dla wyrażeń zapytania, które są tłumaczone na takie wywołania, kontekst jest pobierany z początkowego słowa kluczowego kontekstowego klauzuli zapytania, z którego jest generowane wywołanie.

### <a name="the-first-phase"></a>Pierwsza faza

Typy odwołań dopuszczające wartość null są przepływem do granic z wyrażeń początkowych, jak opisano poniżej. Ponadto wprowadzono dwa nowe rodzaje granic, mianowicie `null` i `default`. Ich celem jest przechodzenie między wystąpieniami `null` lub `default` w wyrażeniach wejściowych, które mogą spowodować, że typ wnioskowany będzie dopuszczać wartości null, nawet wtedy, gdy nie byłoby w przeciwnym razie. Działa to nawet w przypadku typów *wartości* null, które są rozszerzone w celu wybrania "wartości null" w procesie wnioskowania.

Określenie, które granice dodać w pierwszej fazie, zostało ulepszone w następujący sposób:

Jeśli argument `Ei` ma typ referencyjny, `U` typ używany do wnioskowania zależy od stanu null `Ei` oraz jego zadeklarowanego typu:
- Jeśli zadeklarowany typ to typ referencyjny niebędący wartością null `U0` lub typ referencyjny dopuszczający wartość null `U0?`
    - Jeśli stan null `Ei` ma wartość "not null", wówczas `U` jest `U0`
    - Jeśli stan null `Ei` ma wartość "null", wówczas `U` jest `U0?`
- W przeciwnym razie, jeśli `Ei` ma zadeklarowany typ, `U` jest tego typu
- W przeciwnym razie, jeśli `Ei` jest `null`, `U` jest to granica specjalna `null`
- W przeciwnym razie, jeśli `Ei` jest `default`, `U` jest to granica specjalna `default`
- W przeciwnym razie nie wprowadzono wnioskowania.

### <a name="exact-upper-bound-and-lower-bound-inferences"></a>Dokładne, górne i dolne ograniczenia wnioskowania

W wnioskach *z* typu `U` *do* typu `V`, jeśli `V` jest typem referencyjnym null `V0?`, wówczas `V0` jest używany zamiast `V` w poniższych klauzulach.
- Jeśli `V` jest jedną z nieustalonych zmiennych typu, `U` zostanie dodana jako dokładne, górne lub dolne powiązana jak wcześniej
- W przeciwnym razie, jeśli `U` jest `null` lub `default`, nie zostaną wykonane żadne wnioskowanie
- W przeciwnym razie, jeśli `U` jest typem referencyjnym dopuszczającym wartość null `U0?`, wówczas `U0` jest używany zamiast `U` w kolejnych klauzulach.

Jest to, że wartość null odnosząca się bezpośrednio do jednej z nieustalonych zmiennych typu jest zachowywana w granicach. W odniesieniu do wniosków, które powtarzają się w przypadku typów źródłowych i docelowych, z drugiej strony, wartość null jest ignorowana. Może być niezgodny, ale jeśli nie, ostrzeżenie zostanie wystawione później w przypadku wybrania i zastosowania przeciążenia.

### <a name="fixing"></a>Mocowanie

Obecnie nie jest to dobre zadanie opisywania, co się dzieje, gdy wiele granic jest do siebie konwertowane tożsamości, ale różnią się. Może się to zdarzyć między `object` i `dynamic`, między typami krotek, które różnią się tylko nazwami elementów, między typami konstruowanymi, a teraz między `C` i `C?` dla typów referencyjnych.

Ponadto musimy propagować "wartość null" z wyrażeń wejściowych do typu wyniku. 

Aby obsłużyć te etapy, możemy dodać więcej faz, które są teraz:

1. Zbierz wszystkie typy we wszystkich granicach jako kandydaci, usuwając `?` ze wszystkich, które są dopuszczającymi wartości null
2. Eliminowanie kandydatów na podstawie wymagań ścisłych, dolnych i górnych granic (utrzymywanie granic `null` i `default`)
3. Eliminowanie kandydatów, które nie mają niejawnej konwersji na wszystkich innych kandydatów
4. Jeśli pozostałe kandydaci nie wszystkie mają do siebie konwersje tożsamości, wówczas wnioskowanie o typie kończy się niepowodzeniem
5. *Scal* pozostałe kandydaci zgodnie z poniższym opisem
6. Jeśli wynikiem jest typ referencyjny lub typ wartości, która ma wartość null, a *wszystkie* dokładne granice lub *dowolne* dolne granice są typami wartości null, typem referencyjnym dopuszczającym wartość null, `null` lub `default`, wówczas `?` jest dodawane do uzyskanego kandydata, co oznacza, że jest to typ wartości null lub typ referencyjny.

*Scalanie* jest opisane między dwoma typami kandydatów. Jest on przechodni i komutatywna, więc kandydaci mogą być scalane w dowolnej kolejności z tym samym ostatecznym wynikiem. Ta wartość jest niezdefiniowana, jeśli dwa typy kandydatów nie są do siebie konwertowane.

Funkcja *merge* przyjmuje dwa typy kandydujące i kierunek ( *+* lub *-* ):

- *Scal*(`T`, `T`, *d*) = t
- *Scal*(`S`, `T?`, *+* ) = *merge*(`S?`, `T`, *+)* = *merge*(`S`, `T`, *+* )`?`
- *Scal*(`S`, `T?`, *-* ) = *merge*(`S?`, *`T`,-)* = *merge*(`S`, `T`, *-* )
- *Scal*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+* ) = `C<`*scalanie*(`S1`, `T1`, *D1*)`,...,`*scalanie*(`Sn`, `Tn`, *DN*)`>`, *gdzie*
    - `di` =  *+* , jeśli `i`parametr typu `C<...>` jest współwariantem
    - `di` =  *-* , jeśli `i`parametr typu `C<...>` jest antylub niezmienny
- *Scal*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-* ) = `C<`*scalanie*(`S1`, `T1`, *D1*)`,...,`*scalanie*(`Sn`, `Tn`, *DN*)`>`, *gdzie*
    - `di` =  *-* , jeśli `i`parametr typu `C<...>` jest współwariantem
    - `di` =  *+* , jeśli `i`parametr typu `C<...>` jest antylub niezmienny
- *Scal*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*merge*(`S1`, `T1`, *d*)`n1,...,`*scalanie*(`Sn`, `Tn`, *d*) `nn)`, *gdzie*
    - `ni` jest nieobecny, jeśli `si` i `ti` się różnić lub jeśli oba nie są obecne
    - `ni` jest `si`, jeśli `si` i `ti` są takie same
- *Scal*(`object`, `dynamic`) = *merge*(`dynamic`, `object`) = `dynamic`

## <a name="warnings"></a>Ostrzeżenia

### <a name="potential-null-assignment"></a>Potencjalne przypisanie o wartości null

### <a name="potential-null-dereference"></a>Potencjalna odwołująca się wartość null

### <a name="constraint-nullability-mismatch"></a>Niezgodność wartości NULL ograniczenia

### <a name="nullable-types-in-disabled-annotation-context"></a>Typy dopuszczające wartości null w wyłączonym kontekście adnotacji

## <a name="attributes-for-special-null-behavior"></a>Atrybuty dla specjalnego zachowania o wartości null


