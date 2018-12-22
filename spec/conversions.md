# <a name="conversions"></a>Konwersje

A ***konwersji*** umożliwia wyrażenia traktowana jako określonego typu. Konwersja może spowodować, że wyrażenie danego typu należy traktować jako posiadające innego typu lub może spowodować, że wyrażenie bez typu, można pobrać typu. Konwersje może być ***niejawne*** lub ***jawne***, i określa, czy jawnego rzutowania jest wymagana. Na przykład konwersja z typu `int` na typ `long` jest niejawny, więc wyrażeń o typie `int` niejawnie może być traktowany jako typ `long`. Przeciwny konwersji z typu `long` na typ `int`, jest jawne jawnego rzutowania to wymagane.

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

Niektóre konwersje są definiowane przez język. Programy mogą również definiować własne konwersje ([zdefiniowane przez użytkownika konwersje](conversions.md#user-defined-conversions)).

## <a name="implicit-conversions"></a>Niejawne konwersje

Poniższe konwersje są klasyfikowane jako niejawne konwersje:

*  Konwersje tożsamości
*  Niejawne konwersje liczbowe
*  Wyliczenie niejawne konwersje.
*  Niejawne konwersje dopuszczającego wartość null
*  Konwersje literał null
*  Konwersje niejawne odwołanie
*  Konwersje boxing
*  Niejawne konwersje dynamiczne
*  Konwersje niejawne stałego wyrażenia
*  Niejawne konwersje zdefiniowane przez użytkownika
*  Funkcja anonimowa konwersje
*  Konwersje grupy — metoda

Niejawne konwersje może wystąpić w wielu sytuacjach, włączając funkcję elementu członkowskiego wywołania ([kompilacji sprawdzanie dynamiczne przeciążonym](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), rzutowane wyrażenia, ([rzutowane wyrażenia,](expressions.md#cast-expressions)), i przydziały ([operatory przypisania](expressions.md#assignment-operators)).

Wstępnie zdefiniowane niejawne konwersje zawsze kończą się pomyślnie i nigdy nie powoduje wyjątki zostanie wygenerowany. Prawidłowo zaprojektowana zdefiniowanych przez użytkownika konwersje niejawne powinny działać również następujące cechy.

Na potrzeby konwersji typów `object` i `dynamic` są uważane za równoważne.

Jednak dynamiczne konwersje ([niejawne konwersje dynamiczne](conversions.md#implicit-dynamic-conversions) i [jawne konwersje dynamiczne](conversions.md#explicit-dynamic-conversions)) mają zastosowanie tylko do wyrażenia typu `dynamic` ([typu dynamicznego](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Konwersja tożsamości

Konwertuje konwersję tożsamości z dowolnego typu tego samego typu. Ta konwersja istnieje taki sposób, że nazywany jednostki, która już ma wymagany typ być konwertowany do tego typu.

*  Ponieważ obiekt i dynamiczne są uważane za równoważne to konwersja tożsamości między `object` i `dynamic`oraz między typami skonstruowany, które są takie same, podczas zamiany wszystkich wystąpień `dynamic` z `object`.

### <a name="implicit-numeric-conversions"></a>Niejawne konwersje liczbowe

Niejawne konwersje liczbowe są:

*  Z `sbyte` do `short`, `int`, `long`, `float`, `double`, lub `decimal`.
*  Z `byte` do `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, lub `decimal`.
*  Z `short` do `int`, `long`, `float`, `double`, lub `decimal`.
*  Z `ushort` do `int`, `uint`, `long`, `ulong`, `float`, `double`, lub `decimal`.
*  Z `int` do `long`, `float`, `double`, lub `decimal`.
*  Z `uint` do `long`, `ulong`, `float`, `double`, lub `decimal`.
*  Z `long` do `float`, `double`, lub `decimal`.
*  Z `ulong` do `float`, `double`, lub `decimal`.
*  Z `char` do `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, lub `decimal`.
*  Z `float` do `double`.

Konwersje z `int`, `uint`, `long`, lub `ulong` do `float` i `long` lub `ulong` do `double` może spowodować utratę dokładności, ale nigdy nie będzie przyczyny utraty wielkości. Inne niejawnych konwersji liczbowych nigdy nie utracą żadnych informacji.

Brak konwersji niejawnych do `char` wpisz, aby wartości całkowite typy nie są automatycznie konwertowane `char` typu.

### <a name="implicit-enumeration-conversions"></a>Konwersje niejawne, wyliczenie

Umożliwia wyliczenie niejawna konwersja *decimal_integer_literal* `0` ma zostać przekonwertowany do dowolnego *enum_type* i do dowolnego *nullable_type* którego Typ podstawowy jest *enum_type*. W tym ostatnim przypadku konwersja jest obliczane przez konwersję do bazowego *enum_type* i opakowywanie wynik ([typów dopuszczających wartości zerowe](types.md#nullable-types)).

### <a name="implicit-interpolated-string-conversions"></a>Konwersje niejawne ciąg interpolowany

Niejawny interpolowane zezwala konwersji ciągu *interpolated_string_expression* ([ciągi interpolowane](expressions.md#interpolated-strings)) ma zostać przekonwertowane na `System.IFormattable` lub `System.FormattableString` (który implementuje `System.IFormattable`).

Po zastosowaniu tej konwersji wartości ciągu nie składa się z ciągu interpolowanym. Zamiast tego wystąpienia `System.FormattableString` jest tworzone, jak opisane w [ciągi interpolowane](expressions.md#interpolated-strings).

### <a name="implicit-nullable-conversions"></a>Niejawne konwersje dopuszczającego wartość null

Wstępnie zdefiniowane niejawne konwersje, które działają na typy wartości nieprzyjmujące można również z formularzami dopuszcza wartości null z tych typów. Dla każdego z wstępnie zdefiniowanych niejawne tożsamości i konwersji liczbowych, które konwertują z typem wartościowym niebędącym `S` z typem wartościowym niebędącym `T`, istnieją następujące niejawne konwersje dopuszczającego wartość null:

*  Niejawna konwersja z `S?` do `T?`.
*  Niejawna konwersja z `S` do `T?`.

Ocena niejawną konwersję dopuszczającego wartość null oparte na podstawowej konwersja `S` do `T` przechodzi w następujący sposób:

*  Jeśli konwersja dopuszcza wartości null z `S?` do `T?`:
    * Jeśli wartość źródłowa ma wartość null (`HasValue` właściwość ma wartość FAŁSZ), wynik jest wartością null typu `T?`.
    * W przeciwnym razie konwersja jest oceniane jako rozpakowanie z `S?` do `S`, a następnie bazowego konwersja `S` do `T`, a następnie zawijania ([typów dopuszczających wartości zerowe](types.md#nullable-types)) z `T` do `T?`.

*  Jeśli konwersja dopuszcza wartości null z `S` do `T?`, konwersja jest oceniane jako bazowego konwersja `S` do `T` następuje zawijania z `T` do `T?`.

### <a name="null-literal-conversions"></a>Konwersje literał null

Istnieje niejawna konwersja z `null` literału do dowolnego typu dopuszczającego wartość null. Ta konwersja daje wartość null ([typów dopuszczających wartości zerowe](types.md#nullable-types)) danego typu dopuszczającego wartość null.

### <a name="implicit-reference-conversions"></a>Konwersje niejawne odwołanie

Konwersje niejawne odwołanie są:

*  Za pomocą dowolnego *reference_type* do `object` i `dynamic`.
*  Za pomocą dowolnego *class_type* `S` do dowolnego *class_type* `T`, podana `S` jest tworzony na podstawie `T`.
*  Za pomocą dowolnego *class_type* `S` do dowolnego *interface_type* `T`, podana `S` implementuje `T`.
*  Za pomocą dowolnego *interface_type* `S` do dowolnego *interface_type* `T`, podana `S` jest tworzony na podstawie `T`.
*  Z *array_type* `S` z typem elementu `SE` do *array_type* `T` z typem elementu `TE`, o ile spełnione są wszystkie z następujących czynności:
    * `S` i `T` różnią się tylko w typie elementu. Innymi słowy `S` i `T` mają taką samą liczbę wymiarów.
    * Zarówno `SE` i `TE` są *reference_type*s.
    * Istnieje niejawna konwersja odwołania `SE` do `TE`.
*  Za pomocą dowolnego *array_type* do `System.Array` i interfejsy implementuje.
*  Z typu tablicy jednowymiarowej `S[]` do `System.Collections.Generic.IList<T>` i interfejsy podstawowe, pod warunkiem, że istnieje niejawna konwersja tożsamości lub odwołania z `S` do `T`.
*  Za pomocą dowolnego *delegate_type* do `System.Delegate` i interfejsy implementuje.
*  Z literału null do dowolnego *reference_type*.
*  Za pomocą dowolnego *reference_type* do *reference_type* `T` ma niejawną konwersję tożsamości lub odwołanie do *reference_type* `T0` i `T0` ma konwersję tożsamości do `T`.
*  Za pomocą dowolnego *reference_type* na typ interfejsu lub delegata `T` ma niejawna konwersja tożsamości lub odwołanie do typu interfejsu lub delegata `T0` i `T0` jest odchylenie przekonwertować ([ Konwersja wariancji](interfaces.md#variance-conversion)) do `T`.
*  Niejawne konwersje dotyczące parametrów typu, które są znane jako typy odwołań. Zobacz [niejawne konwersje dotyczące parametrów typu](conversions.md#implicit-conversions-involving-type-parameters) uzyskać więcej informacji dotyczących niejawnych konwersji dotyczące parametrów typu.

Konwersje niejawne odwołanie są te konwersje między *reference_type*s, który można sprawdzone zawsze powiedzie się i dlatego wymagają żadne testy w czasie wykonywania.

Konwersje odwołań, jawnych lub niejawnych, nigdy nie ulegną zmianie referencyjną tożsamość obiektu konwersji. Innymi słowy gdy konwersja odwołania może zmienić typ odwołania, nigdy się nie zmienia typu lub wartości przywoływanego obiektu.

### <a name="boxing-conversions"></a>Konwersje boxing

Konwersja boxing pozwala *value_type* niejawnie są konwertowane na typ odwołania. Konwersja boxing istnieje pochodzących z dowolnych *non_nullable_value_type* do `object` i `dynamic`, `System.ValueType` i do dowolnego *interface_type* implementowany przez *non_ nullable_value_type*. Ponadto *enum_type* można przekonwertować na typ `System.Enum`.

Istnieje konwersja boxing z *nullable_type* typem odwołania, jeśli i tylko wtedy, gdy konwersja boxing istnieje z bazowego *non_nullable_value_type* typem odwołania.

Typ wartości jest opakowywanie konwersji do typu interfejsu `I` czy ma on opakowywanie konwersji do typu interfejsu `I0` i `I0` ma konwersję tożsamości do `I`.

Typ wartości jest opakowywanie konwersji do typu interfejsu `I` ma opakowywanie konwersji na typ interfejsu lub delegata `I0` i `I0` jest odchylenie przekonwertować ([konwersji wariancji](interfaces.md#variance-conversion)) do `I`.

Konwersja boxing wartość *non_nullable_value_type* składa się z przydzielanie wystąpienia obiektu i kopiowanie *value_type* wartość do tego wystąpienia. Struktura może być zapakowany typowi `System.ValueType`, ponieważ klasa bazowa dla wszystkich struktur ([dziedziczenia](structs.md#inheritance)).

Konwersja boxing wartość *nullable_type* przechodzi w następujący sposób:

*  Jeśli wartość źródłowa ma wartość null (`HasValue` właściwość ma wartość FAŁSZ), wynik jest odwołaniem do wartości null typu docelowego.
*  W przeciwnym razie wynik jest odwołaniem do spakowanej `T` wytworzonych przez rozpakowanie i pakowanie wartość źródła.

Konwersje boxing są dokładniejszym opisem zawartym w [konwersje Boxing](types.md#boxing-conversions).

### <a name="implicit-dynamic-conversions"></a>Niejawne konwersje dynamiczne

Istnieje niejawna konwersja dynamicznych z wyrażenia typu `dynamic` do dowolnego typu `T`. Konwersja jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)), co oznacza, że będą poszukiwane niejawnej konwersji w czasie wykonywania z typu run-time wyrażenia, aby `T`. Jeśli konwersja nie zostanie znaleziony, zwracany jest wyjątek czasu wykonywania.

Należy pamiętać, że to niejawna konwersja pozornie narusza porad na początku [niejawne konwersje](conversions.md#implicit-conversions) czy niejawną konwersję nigdy nie powinna spowodować wyjątek. Jednakże nie jest konwersja, ale *znajdowanie* konwersji, który powoduje wyjątek. Ryzyko wyjątków w czasie wykonywania jest związane ze stosowaniem wiązanie dynamiczne. Jeśli wiązanie dynamiczne konwersji jest niepożądany, wyrażenie może być najpierw konwertowany `object`, a następnie do żądanego typu.

Poniższy przykład ilustruje niejawne konwersje dynamicznej:

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

Przypisania do `s2` i `i` stosować zarówno dynamiczne konwersje niejawne, gdzie powiązanie operacji jest zawieszona do czasu uruchomienia. W czasie wykonywania, niejawne konwersje są pozyskiwane od typu run-time z `d`  --  `string` — na typ docelowy. Konwersja okaże się `string` , ale nie `int`.

### <a name="implicit-constant-expression-conversions"></a>Konwersje niejawne stałego wyrażenia

Wyrażenie stałe w niejawnych konwersji zezwala na następujące konwersje:

*  A *constant_expression* ([wyrażeń stałych](expressions.md#constant-expressions)) typu `int` można przekonwertować na typ `sbyte`, `byte`, `short`, `ushort`, `uint`, lub `ulong`, podana wartość *constant_expression* znajduje się w zakresie typu miejsca docelowego.
*  A *constant_expression* typu `long` można przekonwertować na typ `ulong`, podana wartość *constant_expression* nie jest ujemny.

### <a name="implicit-conversions-involving-type-parameters"></a>Niejawne konwersje dotyczące parametrów typu

Poniższe konwersje niejawne istnieje dla danego typu parametru `T`:

*  Z `T` z klasą bazową skuteczne `C`, z `T` do każdej klasy bazowej `C`i z `T` do dowolnego interfejsu implementowany przez `C`. W czasie wykonywania, jeśli `T` jest typem wartości konwersja jest wykonywana jako konwersji pakującej. W przeciwnym razie konwersja jest wykonywana niejawna konwersja odwołania lub konwersji tożsamości.
*  Z `T` do typu interfejsu `I` w `T`ustawione przez interfejs skuteczne i z `T` na dowolnym interfejsem podstawowy `I`. W czasie wykonywania, jeśli `T` jest typem wartości konwersja jest wykonywana jako konwersji pakującej. W przeciwnym razie konwersja jest wykonywana niejawna konwersja odwołania lub konwersji tożsamości.
*  Z `T` z parametrem typu `U`, podana `T` zależy od `U` ([ograniczenia parametru typu](classes.md#type-parameter-constraints)). W czasie wykonywania, jeśli `U` jest typem wartości `T` i `U` niekoniecznie są tego samego typu i konwersja nie jest wykonywana. W przeciwnym razie, jeśli `T` jest typem wartości konwersja jest wykonywana jako konwersji pakującej. W przeciwnym razie konwersja jest wykonywana niejawna konwersja odwołania lub konwersji tożsamości.
*  Z literału null do `T`, podana `T` jest znany jako typ odwołania.
*  Z `T` typem odwołania `I` czy ma on niejawną konwersję do typu referencyjnego `S0` i `S0` ma konwersję tożsamości do `S`. W czasie wykonywania konwersji jest wykonywany ten sam sposób jak konwersja `S0`.
*  Z `T` do typu interfejsu `I` ma niejawną konwersję na typ interfejsu lub delegata `I0` i `I0` jest odchylenie przekonwertować do `I` ([konwersji wariancji](interfaces.md#variance-conversion) ). W czasie wykonywania, jeśli `T` jest typem wartości konwersja jest wykonywana jako konwersji pakującej. W przeciwnym razie konwersja jest wykonywana niejawna konwersja odwołania lub konwersji tożsamości.

Jeśli `T` jest znany jako typu odwołania ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), konwersje powyżej są klasyfikowane jako konwersje niejawne odwołanie ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)). Jeśli `T` jest nie jest znany jako typ odwołania, konwersji powyżej są klasyfikowane jako konwersje boxing ([konwersje Boxing](conversions.md#boxing-conversions)).

### <a name="user-defined-implicit-conversions"></a>Niejawne konwersje zdefiniowane przez użytkownika

Zdefiniowane przez użytkownika konwersji niejawnych składa się z opcjonalne standardowa niejawną konwersję, następuje wykonanie operatora niejawnej konwersji zdefiniowanej przez użytkownika, następuje innego opcjonalne standardowa niejawnej konwersji. Szczegółowe reguły oceny niejawne konwersje zdefiniowane przez użytkownika są opisane w [przetwarzanie zdefiniowanych przez użytkownika konwersje niejawne](conversions.md#processing-of-user-defined-implicit-conversions).

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>Funkcja anonimowa konwersji i konwersje grupy — metoda

Funkcje anonimowe i grupy metod nie mają typów w i samodzielnie, ale może być niejawnie konwertowane na delegowanie typów lub drzewa wyrażeń. Funkcja anonimowa konwersje zostały opisane bardziej szczegółowo w [konwersje funkcja anonimowa](conversions.md#anonymous-function-conversions) i metody konwersji grupy w [metody konwersji grup](conversions.md#method-group-conversions).

## <a name="explicit-conversions"></a>Konwersje jawne

Poniższe konwersje są klasyfikowane jako jawne konwersje:

*  Wszystkie konwersje niejawne.
*  Jawnych konwersji liczbowych.
*  Konwersje jawne wyliczenia.
*  Jawne konwersje dopuszczającego wartość null.
*  Konwersje jawne odwołań.
*  Konwersje jawne interfejsu.
*  Rozpakowywanie konwersji.
*  Jawne konwersje dynamiczne
*  Konwersje jawne zdefiniowanych przez użytkownika.

Konwersje jawne mogą występować w wyrażenia cast ([rzutowane wyrażenia,](expressions.md#cast-expressions)).

Zestaw jawne konwersje obejmuje wszystkie niejawne konwersje. Oznacza to, że wyrażenia cast nadmiarowy są dozwolone.

Jawne konwersje, które nie są niejawne konwersje są konwersje, które nie sprawdzone zawsze została wykonana pomyślnie, konwersje, które są znane, być może spowodować utratę informacji i konwersje między domenami typów różni się wystarczająco na jawną wartość zapis.

### <a name="explicit-numeric-conversions"></a>Jawnych konwersji liczbowych

Jawne konwersje liczbowe są konwersje z *numeric_type* do innego *numeric_type* dla którego niejawnych konwersji liczbowych ([niejawnych konwersji liczbowych](conversions.md#implicit-numeric-conversions)) jeszcze nie istnieje:

*  Z `sbyte` do `byte`, `ushort`, `uint`, `ulong`, lub `char`.
*  Z `byte` do `sbyte` i `char`.
*  Z `short` do `sbyte`, `byte`, `ushort`, `uint`, `ulong`, lub `char`.
*  Z `ushort` do `sbyte`, `byte`, `short`, lub `char`.
*  Z `int` do `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, lub `char`.
*  Z `uint` do `sbyte`, `byte`, `short`, `ushort`, `int`, lub `char`.
*  Z `long` do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, lub `char`.
*  Z `ulong` do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, lub `char`.
*  Z `char` do `sbyte`, `byte`, lub `short`.
*  Z `float` do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, lub `decimal`.
*  Z `double` do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, lub `decimal`.
*  Z `decimal` do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, lub `double`.

Ponieważ Konwersje jawne zawierają wszystkie jawne i niejawne konwersje liczbowe, zawsze jest możliwe do przekonwertowania z dowolnego *numeric_type* do żadnej innej *numeric_type* przy użyciu (wyrażenia cast [Rzutowane wyrażenia,](expressions.md#cast-expressions)).

Jawnych konwersji liczbowych, prawdopodobnie utratę danych lub może spowodować wyjątki zostanie wygenerowany. Jawna konwersja liczbowa jest przetwarzana w następujący sposób:

*  Do konwersji z typu całkowitego do innego typu całkowitego przetwarzania zależy od przepełnienia, sprawdzając kontekst ([operatory checked i unchecked](expressions.md#the-checked-and-unchecked-operators)) umieść w używającą konwersji:
    * W `checked` kontekstu, konwersja powiedzie się, jeśli wartość operandu źródła znajduje się w zakresie typu miejsca docelowego, ale zgłasza `System.OverflowException` Jeśli wartość operandu źródła znajduje się poza zakresem typu miejsca docelowego.
    * W `unchecked` kontekstu, konwersja zawsze powiedzie się i następuje.
        * Jeśli typ źródła jest większy niż typ docelowy, a następnie wartość źródłowa zostanie obcięta przez odrzucenie jego "dodatkowe" najbardziej znaczące bity. Wynik jest traktowane jako wartość typu miejsca docelowego.
        * Jeśli typ źródła jest mniejszy niż typ docelowy, następnie wartość źródłowa jest rozszerzona o znak lub rozszerzone zero tak, aby jest taki sam rozmiar jak typ docelowy. Rozszerzenia znaku jest używana, jeśli typ źródła jest podpisany; rozszerzenie zero jest używana, jeśli typ źródła jest niepodpisany. Wynik jest traktowane jako wartość typu miejsca docelowego.
        * Jeśli typ źródła jest taki sam rozmiar jak typ docelowy, wartość źródłowa jest traktowany jako wartość typu miejsca docelowego.
*  Do konwersji z `decimal` na typ całkowity, wartość źródłowa jest zaokrąglana w kierunku zera do najbliższej wartości całkowitej, a ta wartość całkowitą staje się wynik konwersji. Jeśli wynikową wartość całkowitą znajduje się poza zakresem typu docelowego `System.OverflowException` zgłaszany.
*  Do konwersji z `float` lub `double` na typ całkowity, przetwarzania zależy od przepełnienia, sprawdzając kontekst ([operatory checked i unchecked](expressions.md#the-checked-and-unchecked-operators)) umieść w używającą konwersji:
    * W `checked` kontekstu, konwersja będzie kontynuowane w następujący sposób:
        * Jeśli wartość argument jest NaN lub nieskończone, `System.OverflowException` zgłaszany.
        * W przeciwnym razie źródłowy argument operacji jest zaokrąglana w kierunku zera do najbliższej wartości całkowitej. Jeśli ta wartość całkowitą zakresu docelowego, ta wartość jest wynik konwersji.
        * W przeciwnym razie `System.OverflowException` zgłaszany.
    * W `unchecked` kontekstu, konwersja zawsze powiedzie się i następuje.
        * Jeśli wartość operandu jest wartością typu NaN lub nieskończoną, wynik konwersji jest wartością nieokreślonego typu miejsca docelowego.
        * W przeciwnym razie źródłowy argument operacji jest zaokrąglana w kierunku zera do najbliższej wartości całkowitej. Jeśli ta wartość całkowitą zakresu docelowego, ta wartość jest wynik konwersji.
        * W przeciwnym razie wynik konwersji jest wartością nieokreślonego typu miejsca docelowego.
*  Do konwersji z `double` do `float`, `double` wartość jest zaokrąglana do najbliższej `float` wartość. Jeśli `double` wartość jest zbyt mała, aby przedstawić jako `float`, wynik staje się zerem dodatnia lub ujemna zero. Jeśli `double` jest zbyt duża, aby reprezentowały `float`, wynik staje się nieskończoność dodatnia lub ujemna nieskończoność. Jeśli `double` wartość jest wartością typu NaN, wynik jest również wartością typu NaN.
*  Do konwersji z `float` lub `double` do `decimal`, wartość źródłowa jest konwertowany na `decimal` reprezentacji i zaokrąglony do najbliższej liczby po 28 miejsce dziesiętne, jeśli jest to wymagane ([typu dziesiętnego](types.md#the-decimal-type)). Jeśli wartość źródłowa jest zbyt mała, aby przedstawić jako `decimal`, wynik staje się zerem. Jeśli wartość źródłowa jest NaN, nieskończoność, lub jest zbyt duży, aby przedstawić jako `decimal`, `System.OverflowException` zgłaszany.
*  Do konwersji z `decimal` do `float` lub `double`, `decimal` wartość jest zaokrąglana do najbliższej `double` lub `float` wartość. Gdy ta konwersja może spowodować utratę dokładności, nigdy nie powoduje zgłoszenie wyjątku.

### <a name="explicit-enumeration-conversions"></a>Konwersje jawne, wyliczenie

Konwersje jawne wyliczenia są:

*  Z `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, lub `decimal` do dowolnego *enum_type*.
*  Za pomocą dowolnego *enum_type* do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, lub `decimal`.
*  Za pomocą dowolnego *enum_type* do żadnej innej *enum_type*.

Konwersja jawna wyliczenie między dwoma typami jest przetwarzany przez traktowanie wszelkie uczestnictwa *enum_type* jako typu bazowego *enum_type*, a następnie wykonuje niejawny lub jawny Konwersja liczbowa między typami wynikowe. Na przykład, biorąc pod uwagę *enum_type* `E` z i leżący u podstaw typ `int`, konwersja `E` do `byte` jest przetwarzany jako jawna konwersja liczbowa ([jawne Konwersje liczbowe](conversions.md#explicit-numeric-conversions)) z `int` do `byte`i konwersja `byte` do `E` jest przetwarzany jako niejawnych konwersji liczbowych ([niejawnych konwersji liczbowych](conversions.md#implicit-numeric-conversions)) z `byte` do `int`.

### <a name="explicit-nullable-conversions"></a>Jawne konwersje dopuszczającego wartość null

***Jawne konwersje nullable*** zezwolenia wstępnie zdefiniowane jawne konwersje, które działają w typach wartości innych niż null ma być używany z formularzami dopuszcza wartości null z tych typów. Dla każdego z wstępnie zdefiniowanych jawne konwersje, które konwertują z typem wartościowym niebędącym `S` z typem wartościowym niebędącym `T` ([konwersji tożsamości](conversions.md#identity-conversion), [niejawnych konwersji liczbowych](conversions.md#implicit-numeric-conversions), [Konwersje niejawne wyliczenie](conversions.md#implicit-enumeration-conversions), [jawnych konwersji liczbowych](conversions.md#explicit-numeric-conversions), i [Konwersje jawne wyliczenie](conversions.md#explicit-enumeration-conversions)), następujące występują konwersje dopuszczającego wartość null:

*  Konwersja jawna z `S?` do `T?`.
*  Konwersja jawna z `S` do `T?`.
*  Konwersja jawna z `S?` do `T`.

Ocena nullable konwersji oparta na podstawowej konwersja `S` do `T` przechodzi w następujący sposób:

*  Jeśli konwersja dopuszcza wartości null z `S?` do `T?`:
    * Jeśli wartość źródłowa ma wartość null (`HasValue` właściwość ma wartość FAŁSZ), wynik jest wartością null typu `T?`.
    * W przeciwnym razie konwersja jest oceniane jako rozpakowanie z `S?` do `S`, a następnie bazowego konwersja `S` do `T`, a następnie zawijania z `T` do `T?`.
*  Jeśli konwersja dopuszcza wartości null z `S` do `T?`, konwersja jest oceniane jako bazowego konwersja `S` do `T` następuje zawijania z `T` do `T?`.
*  Jeśli konwersja dopuszcza wartości null z `S?` do `T`, konwersja jest oceniane jako rozpakowanie z `S?` do `S` następuje bazowego konwersja `S` do `T`.

Należy pamiętać, że próba do odkodowania dopuszczającej wartość null spowoduje zgłoszenie wyjątku, jeśli wartość jest `null`.

### <a name="explicit-reference-conversions"></a>Konwersje jawne odwołań

Konwersje jawne odwołania są:

*  Z `object` i `dynamic` do żadnej innej *reference_type*.
*  Za pomocą dowolnego *class_type* `S` do dowolnego *class_type* `T`, podana `S` jest klasą bazową dla `T`.
*  Za pomocą dowolnego *class_type* `S` do dowolnego *interface_type* `T`, podana `S` jest zapieczętowany i nie podano `S` nie implementuje `T`.
*  Za pomocą dowolnego *interface_type* `S` do dowolnego *class_type* `T`, podana `T` jest zapieczętowany lub nie podano `T` implementuje `S`.
*  Za pomocą dowolnego *interface_type* `S` do dowolnego *interface_type* `T`, podana `S` nie pochodzi od `T`.
*  Z *array_type* `S` z typem elementu `SE` do *array_type* `T` z typem elementu `TE`, o ile spełnione są wszystkie z następujących czynności:
    * `S` i `T` różnią się tylko w typie elementu. Innymi słowy `S` i `T` mają taką samą liczbę wymiarów.
    * Zarówno `SE` i `TE` są *reference_type*s.
    * Istnieje konwersja jawnego odwołania z `SE` do `TE`.
*  Z `System.Array` i interfejsy implementuje do dowolnego *array_type*.
*  Z typu tablicy jednowymiarowej `S[]` do `System.Collections.Generic.IList<T>` i interfejsy podstawowe, pod warunkiem że jest konwersja jawna odwołania z `S` do `T`.
*  Z `System.Collections.Generic.IList<S>` i bazowej interfejsy na typ tablicy jednowymiarowej `T[]`pod warunkiem, że istnieje jawna konwersja tożsamości lub odwołania z `S` do `T`.
*  Z `System.Delegate` i interfejsy implementuje do dowolnego *delegate_type*.
*  Z typem referencyjnym, typem odwołania `T` czy ma on konwersję jawnego odwołania do typu referencyjnego `T0` i `T0` ma konwersję tożsamości `T`.
*  Z typu odwołania do typu interfejsu lub delegata `T` ma konwersję jawnego odwołania do typu interfejsu lub delegata `T0` i `T0` jest odchylenie przekonwertować do `T` lub `T` jest Wariancja można przekonwertować na `T0` ([konwersji wariancji](interfaces.md#variance-conversion)).
*  Z `D<S1...Sn>` do `D<T1...Tn>` gdzie `D<X1...Xn>` jest typem ogólnym delegatów `D<S1...Sn>` nie jest zgodne z programem lub taka sama jak `D<T1...Tn>`i dla każdego parametru typu `Xi` z `D` przechowuje następujące:
    * Jeśli `Xi` to niezmienna, następnie `Si` jest taka sama jak `Ti`.
    * Jeśli `Xi` jest kowariantny, to jawne lub niejawne tożsamości lub odwołanie konwersja `Si` do `Ti`.
    * Jeśli `Xi` jest kontrawariantny, następnie `Si` i `Ti` są identyczne lub oba typy odwołań.
*  Konwersje jawne dotyczące parametrów typu, które są znane jako typy odwołań. Aby uzyskać szczegółowe informacje na temat jawne konwersje dotyczące parametrów typu, zobacz [jawne konwersje dotyczące parametrów typu](conversions.md#explicit-conversions-involving-type-parameters).

Konwersje jawne odwołania są te konwersje między typami odwołań, które wymagają sprawdzania w trakcie wykonywania, upewnij się, że są poprawne.

Do konwersji jawnego odwołania zakończyło się sukcesem w czasie wykonywania, wartość operandu źródła musi być `null`, rzeczywisty typ obiektu, który odwołuje się źródłowy argument operacji musi być typem, który można przekonwertować na typ docelowy niejawne odwołanie Konwersja ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions)) lub konwersja boxing ([konwersje Boxing](conversions.md#boxing-conversions)). W przypadku niepowodzenia konwersji jawnego odwołania `System.InvalidCastException` zgłaszany.

Konwersje odwołań, jawnych lub niejawnych, nigdy nie ulegną zmianie referencyjną tożsamość obiektu konwersji. Innymi słowy gdy konwersja odwołania może zmienić typ odwołania, nigdy się nie zmienia typu lub wartości przywoływanego obiektu.

### <a name="unboxing-conversions"></a>Konwersji rozpakowującej

Konwersja rozpakowująca zezwala na typ odwołania, aby być jawnie konwertowane na *value_type*. Konwersja rozpakowująca istnieje z typów `object`, `dynamic` i `System.ValueType` do dowolnego *non_nullable_value_type*i z dowolnego *interface_type* do dowolnego *non_ nullable_value_type* implementującej *interface_type*. Ponadto typ `System.Enum` może być rozpakowany do dowolnego *enum_type*.

Konwersja rozpakowująca istnieje z typem referencyjnym, aby *nullable_type* istnienia konwersji rozpakowującej z typu odwołania do bazowego *non_nullable_value_type* z  *nullable_type*.

Typ wartości `S` ma konwersji rozpakowującej z typu interfejsu `I` czy ma on konwersji rozpakowującej z typu interfejsu `I0` i `I0` ma konwersję tożsamości do `I`.

Typ wartości `S` ma konwersji rozpakowującej z typu interfejsu `I` ma konwersji rozpakowującej z typu interfejsu lub delegata `I0` i `I0` jest odchylenie przekonwertować do `I` lub `I`jest odchylenie przekonwertować do `I0` ([konwersji wariancji](interfaces.md#variance-conversion)).

Operacja rozpakowania składa się z wcześniejszego sprawdzenia, że wystąpienie obiektu jest zapakowaną wartością danego *value_type*, a następnie skopiować wartość spoza wystąpienia. Odwołanie o wartości null do rozpakowywania *nullable_type* generuje wartość null *nullable_type*. Struktura może być rozpakowany z typu `System.ValueType`, ponieważ klasa bazowa dla wszystkich struktur ([dziedziczenia](structs.md#inheritance)).

Konwersje Rozpakowywanie są dokładniejszym opisem zawartym w [konwersje rozpakowywania](types.md#unboxing-conversions).

### <a name="explicit-dynamic-conversions"></a>Jawne konwersje dynamiczne

Istnieje jawna konwersja dynamicznych z wyrażenia typu `dynamic` do dowolnego typu `T`. Konwersja jest dynamicznie powiązany ([wiązanie dynamiczne](expressions.md#dynamic-binding)), co oznacza, że będą poszukiwane jawnej konwersji w czasie wykonywania z typu run-time wyrażenia, aby `T`. Jeśli konwersja nie zostanie znaleziony, zwracany jest wyjątek czasu wykonywania.

Jeśli wiązanie dynamiczne konwersji jest niepożądany, wyrażenie może być najpierw konwertowany `object`, a następnie do żądanego typu.

Przyjęto założenie, że zdefiniowano następujące klasy:
```csharp
class C
{
    int i;

    public C(int i) { this.i = i; }

    public static explicit operator C(string s) 
    {
        return new C(int.Parse(s));
    }
}
```

Poniższy przykład ilustruje jawne konwersje dynamicznej:
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

Najlepsza konwersja z `o` do `C` znajduje się w czasie kompilacji do konwersji jawnego odwołania. To nie powiedzie się w czasie wykonywania, ponieważ `"1"` nie jest w rzeczywistości `C`. Konwersja `d` do `C` jednak jako dynamicznej konwersji jawnej zostanie zawieszony w celu czasu wykonywania, gdzie zdefiniowane przez użytkownika konwersja z typu run-time dla `d`  --  `string` — do `C` zostanie znaleziony, i zakończy się pomyślnie.

### <a name="explicit-conversions-involving-type-parameters"></a>Konwersje jawne dotyczące parametrów typu

Poniższe Konwersje jawne istnieje dla danego typu parametru `T`:

*  Z klasy bazowej skuteczne `C` z `T` do `T` i z dowolnej klasy bazowej `C` do `T`. W czasie wykonywania, jeśli `T` jest typem wartości konwersja jest wykonywana jako konwersji rozpakowującej. W przeciwnym razie konwersja jest wykonywana jako konwersji jawnej odwołania lub adaptacji tożsamości.
*  Z dowolnego typu interfejsu, aby `T`. W czasie wykonywania, jeśli `T` jest typem wartości konwersja jest wykonywana jako konwersji rozpakowującej. W przeciwnym razie konwersja jest wykonywana jako konwersji jawnej odwołania lub adaptacji tożsamości.
*  Z `T` do dowolnego *interface_type* `I` pod warunkiem jeszcze nie istnieje niejawna konwersja z `T` do `I`. W czasie wykonywania, jeśli `T` jest typem wartości konwersja jest wykonywana jako konwersji pakującej, następuje konwersję jawnego odwołania. W przeciwnym razie konwersja jest wykonywana jako konwersji jawnej odwołania lub adaptacji tożsamości.
*  Z parametrem typu `U` do `T`, podana `T` zależy od `U` ([ograniczenia parametru typu](classes.md#type-parameter-constraints)). W czasie wykonywania, jeśli `U` jest typem wartości `T` i `U` niekoniecznie są tego samego typu i konwersja nie jest wykonywana. W przeciwnym razie, jeśli `T` jest typem wartości konwersja jest wykonywana jako konwersji rozpakowującej. W przeciwnym razie konwersja jest wykonywana jako konwersji jawnej odwołania lub adaptacji tożsamości.

Jeśli `T` jest znane jako typ odwołania, konwersji powyżej wszystkich sklasyfikowanych jako Konwersje jawne odwołań ([Konwersje jawne odwołań](conversions.md#explicit-reference-conversions)). Jeśli `T` jest nie jest znany jako typ odwołania, konwersji powyżej są klasyfikowane jako konwersji unboxing ([konwersje rozpakowywania](conversions.md#unboxing-conversions)).

Powyższe zasady nie zezwalają na to bezpośrednia konwersji jawnej z parametrem typu bez ograniczeń do typu innego niż interfejs może być Zaskakujące. Przyczyna, dla tej reguły jest uniknąć zamieszania i semantyka takie konwersje Wyczyść. Na przykład przeanalizujmy następującą deklarację:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

Jeśli bezpośrednie jawnej konwersji z `t` do `int` były dozwolone, można łatwo oczekiwać, że że `X<int>.F(7)` zwróci `7L`. Jednak w ten sposób, ponieważ standardowe konwersje liczbowe są traktowane tylko, gdy typy są znane jako liczbowe w czasie powiązania. Aby można było wprowadzić semantykę clear powyższym przykładzie zamiast tego jest pisana:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

Ten kod teraz będzie kompilowany ale wykonywanie `X<int>.F(7)` następnie zgłasza wyjątek w czasie wykonywania, od spakowany `int` nie można przekonwertować bezpośrednio do `long`.

### <a name="user-defined-explicit-conversions"></a>Jawne konwersje zdefiniowane przez użytkownika

Zdefiniowane przez użytkownika konwersja jawna składa się z opcjonalnych standardowa konwersja jawna, następuje wykonanie jawnych lub niejawnych konwersji zdefiniowanej przez użytkownika operatora, następuje innego opcjonalne standardowych konwersji jawnej. Szczegółowe reguły oceny zdefiniowanych przez użytkownika Konwersje jawne są opisane w [przetwarzanie zdefiniowanych przez użytkownika Konwersje jawne](conversions.md#processing-of-user-defined-explicit-conversions).

## <a name="standard-conversions"></a>Konwersje standardowe

Konwersje standardowe są te wstępnie zdefiniowane konwersje, które mogą wystąpić w ramach konwersji zdefiniowanej przez użytkownika.

### <a name="standard-implicit-conversions"></a>Standardowe konwersje niejawne

Poniższe konwersje niejawne są klasyfikowane jako standardowe i niejawne konwersje:

*  Konwersje tożsamości ([konwersji tożsamości](conversions.md#identity-conversion))
*  Niejawnych konwersji liczbowych ([niejawnych konwersji liczbowych](conversions.md#implicit-numeric-conversions))
*  Niejawne konwersje dopuszcza wartości null ([niejawne konwersje nullable](conversions.md#implicit-nullable-conversions))
*  Konwersje niejawne odwołanie ([konwersje niejawne odwołanie](conversions.md#implicit-reference-conversions))
*  Konwersje boxing ([konwersje Boxing](conversions.md#boxing-conversions))
*  Konwersje niejawne wyrażenie stałe ([niejawne konwersje dynamiczne](conversions.md#implicit-dynamic-conversions))
*  Niejawne konwersje dotyczące parametrów typu ([niejawne konwersje dotyczące parametrów typu](conversions.md#implicit-conversions-involving-type-parameters))

Standardowe konwersje niejawne wykluczyć niejawne konwersje zdefiniowane przez użytkownika.

### <a name="standard-explicit-conversions"></a>Standardowe Konwersje jawne

Standardowe Konwersje jawne są wszystkie niejawne konwersje standardowe oraz podzbiór Konwersje jawne, dla których istnieje przeciwny niejawna konwersja standardowa. Innymi słowy, jeśli niejawne standard istnieje konwersja z typu `A` do typu `B`, a następnie istnieje standardowa jawna konwersja z typu `A` na typ `B` i z typu `B` na typ `A`.

## <a name="user-defined-conversions"></a>Konwersje zdefiniowane przez użytkownika

C# umożliwia zostać rozszerzony o wstępnie zdefiniowanych Konwersje jawne i niejawne ***zdefiniowane przez użytkownika konwersje***. Konwersje zdefiniowane przez użytkownika zostały wprowadzone przez zadeklarowanie operatory konwersji ([operatory konwersji](classes.md#conversion-operators)) w typach klasy i struktury.

### <a name="permitted-user-defined-conversions"></a>Dozwolone konwersje zdefiniowane przez użytkownika

C# zezwala na tylko niektórych konwersje zdefiniowane przez użytkownika mają zostać zadeklarowane. W szczególności nie jest możliwe ponowne zdefiniowanie już istniejącymi jawnych lub niejawnych konwersji.

Dla typu danego źródła `S` i docelowy typ `T`, jeśli `S` lub `T` są typy dopuszczające wartości zerowe umożliwiają `S0` i `T0` odnoszą się do ich typów podstawowych, w przeciwnym razie `S0` i `T0` są równa `S` i `T` odpowiednio. Klasy lub struktury może zadeklarować konwersji z typu źródłowego `S` z typem docelowym `T` tylko wtedy, gdy spełnione są wszystkie z następujących czynności:

*  `S0` i `T0` są różnych typów.
*  Albo `S0` lub `T0` jest typem klasy lub struktury, w którym odbywa się Deklaracja operatora.
*  Ani `S0` ani `T0` jest *interface_type*.
*  Z wyjątkiem konwersje zdefiniowane przez użytkownika konwersja nie istnieje z `S` do `T` lub `T` do `S`.

Ograniczenia, które są stosowane do konwersje zdefiniowane przez użytkownika zostały omówione w dalszych [operatory konwersji](classes.md#conversion-operators).

### <a name="lifted-conversion-operators"></a>Operatory konwersji podniesionym

Danego operatora konwersji zdefiniowanej przez użytkownika, który konwertuje z typem wartościowym niebędącym `S` z typem wartościowym niebędącym `T`, ***zniesione operatora konwersji*** istnieje ten konwertuje z `S?` do `T?`. Operator konwersji podniesionym wykonuje rozpakowanie z `S?` do `S` konwersja zdefiniowana przez użytkownika, z którym następuje `S` do `T` następuje zawijania z `T` do `T?`, chyba że o wartości null cenionym `S?` bezpośrednio konwertowany na wartość null, zwracająca `T?`.

Operator konwersji podniesionym ma tej samej klasyfikacji jawnych lub niejawnych jako jego podstawowy operatora konwersji zdefiniowanej przez użytkownika. Termin "konwersja zdefiniowana przez użytkownika" dotyczy użycia obu tych elementów zdefiniowanych przez użytkownika oraz węzły podniesione operatory konwersji.

### <a name="evaluation-of-user-defined-conversions"></a>Ocena konwersje zdefiniowane przez użytkownika

Konwersja zdefiniowana przez użytkownika konwertuje wartość z typu, o nazwie ***typ źródła***, do innego typu o nazwie ***docelowy typ***. Ocena konwersji zdefiniowanej przez użytkownika centra dotyczące wyszukiwania ***bardziej konkretny od pozostałych*** operator konwersji zdefiniowanej przez użytkownika dla określonego typami źródłowym i docelowym. Oznaczanie jest dzielony na kilku kroków:

*  Znajdowanie zestawu klas i struktur, z którego będzie brana operatory konwersji zdefiniowane przez użytkownika. Ten zestaw składa się z typu źródłowego i jej klasy bazowe i typ docelowy i jej klas podstawowych (z założenia niejawne, tylko klasy i struktury można zadeklarować operatory zdefiniowane przez użytkownika, oraz że typy w swojej klasie mają nie mają klas bazowych). Na potrzeby tego kroku, jeśli typ źródłowych lub docelowych *nullable_type*, jego typ podstawowy jest używana zamiast tego.
*  Z tego zbioru typów określająca, które zdefiniowanych przez użytkownika oraz węzły podniesione operatory konwersji mają zastosowanie. Dla operatora konwersji, która będzie stosowana, musi istnieć możliwość wykonywania konwersja standardowa ([konwersje standardowe](conversions.md#standard-conversions)) z typu źródłowego do operandu typ operatora, a musi być możliwe przeprowadzenie konwersją standardową z typu wyniku operator na typ docelowy.
*  Z zestawu, odpowiednie operatorów zdefiniowanych przez użytkownika, określania, które operator jest jednoznacznie najbardziej szczegółowy. Ogólnie rzecz biorąc bardziej konkretny od pozostałych operator jest operator, którego typ argumentu jest "najbliżej" typ źródła i którego typ wyniku jest "najbliższy" na typ docelowy. Operatory konwersji zdefiniowane przez użytkownika są preferowane względem operatory konwersji podniesionym. Szczegółowe reguły ustanawiania bardziej konkretny od pozostałych operator konwersji zdefiniowany przez użytkownika są definiowane w poniższych sekcjach.

Po zidentyfikowaniu bardziej konkretny od pozostałych operatora konwersji zdefiniowanej przez użytkownika rzeczywiste wykonanie konwersji zdefiniowanej przez użytkownika obejmuje trzy kroki:

*  Po pierwsze w razie potrzeby wykonywania konwersja standardowa typu źródłowego na typ argumentu operacji operatora konwersji zdefiniowanej przez użytkownika lub urządzenie.
*  Następnie wywoływania operatora konwersji zdefiniowane przez użytkownika lub urządzenie, aby dokonać konwersji.
*  Na koniec w razie potrzeby wykonywania konwersja standardowa ze typ wyniku operator konwersji zdefiniowany przez użytkownika lub urządzenie na typ docelowy.

Obliczanie nigdy nie konwersji zdefiniowanej przez użytkownika obejmuje więcej niż jeden operator konwersji zdefiniowany przez użytkownika lub urządzenie. Innymi słowy, konwersja z typu `S` na typ `T` nigdy nie najpierw wykona zdefiniowanych przez użytkownika konwersja `S` do `X` , a następnie wykonanie konwersji zdefiniowanej przez użytkownika, z `X` do `T`.

Dokładne definicji oceny jawne lub niejawne konwersje zdefiniowane przez użytkownika są podane w poniższych sekcjach. Definicje, należy użyć następujących warunków:

*  Jeśli standardowa niejawnej konwersji ([standardowe i niejawne konwersje](conversions.md#standard-implicit-conversions)) istnieje z typem `A` z typem `B`i jeśli żadna `A` ani `B` są *interface_type*s, a następnie `A` jest nazywany ***wchodzących w skład*** `B`, i `B` jest nazywany ***uwzględniający*** `A`.
*  ***Najbardziej obsługę typu*** w zestaw typów jest jeden typ, który obejmuje wszystkie typy w zestawie. Jeśli nie jeden typ obejmuje wszystkie inne typy, następnie zestaw nie ma najbardziej obsługę typu. W sposób bardziej intuicyjne, najbardziej obsługę typem jest typ "największą" w zestawie — jeden typ, do którego każdego z innych typów mogą być niejawnie konwertowane.
*  ***Najbardziej wchodzących w skład typu*** w zestaw typów jest jeden typ, który jest objęty przez wszystkie typy w zestawie. Jeśli nie jednego typu jest objęty przez wszystkie inne typy, następnie zestawu ma nie najbardziej wchodzących w skład typu. W sposób bardziej intuicyjne, najbardziej encompassed typem jest typ "najmniejszy" w zestawie — jeden typ, który może być niejawnie konwertowane na wszystkich innych typów.

### <a name="processing-of-user-defined-implicit-conversions"></a>Przetwarzanie niejawne konwersje zdefiniowane przez użytkownika

Zdefiniowane przez użytkownika niejawna konwersja z typu `S` na typ `T` są przetwarzane w następujący sposób:

*  Określ typy `S0` i `T0`. Jeśli `S` lub `T` są typy dopuszczające wartości zerowe `S0` i `T0` są ich typów podstawowych, w przeciwnym razie `S0` i `T0` są równe `S` i `T` odpowiednio.
*  Znajdź zestaw typów, `D`, z których konwersja zdefiniowana przez użytkownika operatory będą uznawane za. Ten zestaw składa się z `S0` (Jeśli `S0` jest klasa lub struktura), klasy bazowe `S0` (Jeśli `S0` jest klasą), i `T0` (Jeśli `T0` jest klasy lub struktury).
*  Znajdź zestaw operatorów dotyczy konwersji zdefiniowanej przez użytkownika i urządzenie, `U`. Ten zestaw składa się z operatorów zdefiniowanych przez użytkownika i podniesionym niejawna konwersja zadeklarowana przez klasy lub struktury, w `D` konwersji z typu obejmujący `S` typowi wchodzących w skład `T`. Jeśli `U` jest pusta, konwersja jest niezdefiniowana, i występuje błąd kompilacji.
*  Znajdź specyficzny typ źródła `SX`, operatorów w `U`:
    * Jeśli dowolny z operatorów w `U` konwersji z `S`, następnie `SX` jest `S`.
    * W przeciwnym razie `SX` najbardziej encompassed typ w połączony zestaw typów źródła operatorów w `U`. Jeśli dokładnie jeden najbardziej wchodzących w skład nie można odnaleźć typu, a następnie konwersja jest niejednoznaczna, i występuje błąd kompilacji.
*  Znajdź specyficzny typ docelowy `TX`, operatorów w `U`:
    * Jeśli dowolny z operatorów w `U` przekonwertować `T`, następnie `TX` jest `T`.
    * W przeciwnym razie `TX` najbardziej obsługę typ w połączony zestaw operatorów w typy elementów docelowych `U`. Jeśli nie można znaleźć dokładnie jeden typ najbardziej obsługę, następnie konwersja jest niejednoznaczna i występuje błąd kompilacji.
*  Znajdź bardziej konkretny od pozostałych operatora konwersji:
    * Jeśli `U` zawiera dokładnie jeden operator konwersji zdefiniowanej przez użytkownika, który konwertuje z `SX` do `TX`, jest bardziej konkretny od pozostałych operatora konwersji.
    * W przeciwnym razie, jeśli `U` zawiera dokładnie jeden operator konwersji podniesionym, który konwertuje z `SX` do `TX`, jest bardziej konkretny od pozostałych operatora konwersji.
    * W przeciwnym razie konwersja jest niejednoznaczna i występuje błąd kompilacji.
*  Na koniec Zastosuj konwersji:
    * Jeśli `S` nie `SX`, następnie niejawna konwersja standardowa ze `S` do `SX` jest wykonywane.
    * Bardziej konkretny od pozostałych operatora konwersji jest wywoływane w celu konwersji z `SX` do `TX`.
    * Jeśli `TX` nie `T`, następnie niejawna konwersja standardowa ze `TX` do `T` jest wykonywane.

### <a name="processing-of-user-defined-explicit-conversions"></a>Przetwarzanie jawne konwersje zdefiniowane przez użytkownika

Zdefiniowane przez użytkownika konwersja jawna z typu `S` na typ `T` są przetwarzane w następujący sposób:

*  Określ typy `S0` i `T0`. Jeśli `S` lub `T` są typy dopuszczające wartości zerowe `S0` i `T0` są ich typów podstawowych, w przeciwnym razie `S0` i `T0` są równe `S` i `T` odpowiednio.
*  Znajdź zestaw typów, `D`, z których konwersja zdefiniowana przez użytkownika operatory będą uznawane za. Ten zestaw składa się z `S0` (Jeśli `S0` jest klasa lub struktura), klasy bazowe `S0` (Jeśli `S0` jest klasą), `T0` (Jeśli `T0` jest klasa lub struktura) i klasy bazowe `T0` (Jeśli `T0`jest klasą).
*  Znajdź zestaw operatorów dotyczy konwersji zdefiniowanej przez użytkownika i urządzenie, `U`. Ten zestaw składa się z zdefiniowane przez użytkownika i podniesionym niejawne lub operatory konwersji jawnej zdeklarowane klas lub struktur w `D` , konwersji z typu obejmujący lub wchodzą w skład `S` typ obejmujący lub wchodzących w skład `T`. Jeśli `U` jest pusta, konwersja jest niezdefiniowana, i występuje błąd kompilacji.
*  Znajdź specyficzny typ źródła `SX`, operatorów w `U`:
    * Jeśli dowolny z operatorów w `U` konwersji z `S`, następnie `SX` jest `S`.
    * W przeciwnym razie, jeśli dowolny z operatorów w `U` konwersji z typów, które obejmują `S`, następnie `SX` najbardziej encompassed typ w połączony zestaw typów źródła tych operatorów. Jeśli nie większość wchodzących w skład można znaleźć typu, a następnie konwersja jest niejednoznaczna, i występuje błąd kompilacji.
    * W przeciwnym razie `SX` najbardziej obsługę typ w połączony zestaw typów źródła operatorów w `U`. Jeśli nie można znaleźć dokładnie jeden typ najbardziej obsługę, następnie konwersja jest niejednoznaczna i występuje błąd kompilacji.
*  Znajdź specyficzny typ docelowy `TX`, operatorów w `U`:
    * Jeśli dowolny z operatorów w `U` przekonwertować `T`, następnie `TX` jest `T`.
    * W przeciwnym razie, jeśli dowolny z operatorów w `U` konwersji na typy, które wchodzą w skład `T`, następnie `TX` najbardziej obsługę typ w połączony zestaw docelowy typy tych operatorów. Jeśli nie można znaleźć dokładnie jeden typ najbardziej obsługę, następnie konwersja jest niejednoznaczna i występuje błąd kompilacji.
    * W przeciwnym razie `TX` najbardziej encompassed typ w połączony zestaw operatorów w typy elementów docelowych `U`. Jeśli nie większość wchodzących w skład można znaleźć typu, a następnie konwersja jest niejednoznaczna, i występuje błąd kompilacji.
*  Znajdź bardziej konkretny od pozostałych operatora konwersji:
    * Jeśli `U` zawiera dokładnie jeden operator konwersji zdefiniowanej przez użytkownika, który konwertuje z `SX` do `TX`, jest bardziej konkretny od pozostałych operatora konwersji.
    * W przeciwnym razie, jeśli `U` zawiera dokładnie jeden operator konwersji podniesionym, który konwertuje z `SX` do `TX`, jest bardziej konkretny od pozostałych operatora konwersji.
    * W przeciwnym razie konwersja jest niejednoznaczna i występuje błąd kompilacji.
*  Na koniec Zastosuj konwersji:
    * Jeśli `S` nie `SX`, następnie jawna konwersja standardowa ze `S` do `SX` jest wykonywane.
    * Bardziej konkretny od pozostałych operator konwersji zdefiniowany przez użytkownika jest wywoływane w celu konwersji z `SX` do `TX`.
    * Jeśli `TX` nie `T`, następnie jawna konwersja standardowa ze `TX` do `T` jest wykonywane.

## <a name="anonymous-function-conversions"></a>Funkcja anonimowa konwersje

*Anonymous_method_expression* lub *lambda_expression* zostanie sklasyfikowany jako funkcja anonimowa ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)). Wyrażenie nie ma typu, ale mogą być niejawnie konwertowane na typ zgodny delegata lub typ drzewa wyrażenia. W szczególności funkcją anonimową `F` jest zgodny z typem delegowanym `D` podane:

*  Jeśli `F` zawiera *anonymous_function_signature*, następnie `D` i `F` mają taką samą liczbę parametrów.
*  Jeśli `F` nie zawiera *anonymous_function_signature*, następnie `D` może mieć zero lub więcej parametrów, dowolnego typu, tak długo, jak żadnego parametru `D` ma `out` modyfikator parametru.
*  Jeśli `F` ma listę parametrów jawnie wpisanych, każdego parametru w `D` ma tego samego typu i Modyfikatory jak odpowiadającym mu parametrem w `F`.
*  Jeśli `F` ma listę parametrów niejawnie wpisane, `D` nie ma przypisanego `ref` lub `out` parametrów.
*  Jeśli treść `F` jest wyrażeniem, a następnie `D` ma `void` zwracany typ lub `F` jest asynchroniczne i `D` ma typ zwracany `Task`, a następnie podczas każdego parametru `F` jest dla danego typu odpowiadającym mu parametrem w `D`, treść `F` jest prawidłowym wyrażeniem (wrt [wyrażeń](expressions.md)) mogą być dozwolone jako *statement_expression* ([Instrukcje wyrażeń](statements.md#expression-statements)).
*  Jeśli treść `F` jest blok instrukcji, a następnie `D` ma `void` zwracany typ lub `F` jest asynchroniczne i `D` ma typ zwracany `Task`, a następnie po każdego parametru `F` jest dla danego typu odpowiadającym mu parametrem w `D`, treść `F` to blok prawidłową instrukcję (wrt [bloki](statements.md#blocks)) w którym nie `return` Instrukcja określa wyrażenia.
*  Jeśli treść `F` , stanowi wyrażenie i *albo* `F` jest inne niż async i `D` ma typ zwracany void nie `T`, *lub* `F` jest asynchroniczne i `D` ma typ zwracany `Task<T>`, a następnie podczas każdego parametru `F` otrzymuje typ odpowiadającym mu parametrem w `D`, treść `F` jest prawidłowym wyrażeniem (wrt [ Wyrażenia](expressions.md)) jest niejawnie konwertowany na `T`.
*  Jeśli treść `F` jest blok instrukcji i *albo* `F` jest inne niż async i `D` ma typ zwracany void nie `T`, *lub* `F` jest asynchroniczne i `D` ma typ zwracany `Task<T>`, a następnie po każdego parametru `F` otrzymuje typ odpowiadającym mu parametrem w `D`, treść `F` to blok prawidłową instrukcję (wrt [bloków ](statements.md#blocks)) za pomocą — dostępny punkt końcowy, w których każdy `return` Instrukcja określa wyrażenie, które jest niejawnie konwertowany na `T`.

Na potrzeby zwięzłości, ta sekcja używa krótkiej formy typy zadań `Task` i `Task<T>` ([funkcje asynchroniczne](classes.md#async-functions)).

Wyrażenie lambda `F` jest zgodny z typem drzewa wyrażenie `Expression<D>` Jeśli `F` jest zgodny z typem delegata `D`. Należy pamiętać, że to nie dotyczą metod anonimowych, tylko wyrażenia lambda.

Niektóre wyrażenia lambda nie można przekonwertować na typy drzewa wyrażeń: Mimo że konwersja *istnieje*, niepowodzenia w czasie kompilacji. To jest wyrażeniem lambda w sytuacji, gdy:

*  Ma *bloku* treści
*  Zawiera operatory przypisania proste lub złożone
*  Zawiera wyrażenie dynamicznie powiązane
*  Jest asynchroniczne

Przykłady, które należy wykonać, użyj typu delegata ogólnego `Func<A,R>` reprezentuje funkcji, która przyjmuje argument typu `A` i zwraca wartość typu `R`:
```csharp
delegate R Func<A,R>(A arg);
```

W przypisaniach
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
typy parametrów i zwrotu każdej funkcji anonimowe są ustalane na podstawie typu zmiennej, do której przypisany jest funkcja anonimowa.

Pierwsze przypisanie pomyślnie konwertuje funkcja anonimowa typ delegata `Func<int,int>` ponieważ gdy `x` jest danego typu `int`, `x+1` jest prawidłowym wyrażeniem, które jest niejawnie konwertowany na typ `int`.

Podobnie, drugie przypisanie pomyślnie konwertuje funkcja anonimowa typ delegata `Func<int,double>` ponieważ wynikiem `x+1` (typu `int`) jest niejawnie konwertowany na typ `double`.

Jednak trzeci przydziału jest błąd w czasie kompilacji, ponieważ gdy `x` jest danego typu `double`, wynik `x+1` (typu `double`) nie jest niejawnie konwertowany na typ `int`.

Czwarty przypisania pomyślnie Konwertuje funkcję asynchroniczną anonimowy typ delegata `Func<int, Task<int>>` ponieważ wynikiem `x+1` (typu `int`) jest niejawnie konwertowany na typ wyniku `int` tego typu zadania `Task<int>`.

Funkcje anonimowe może mieć wpływ na rozpoznanie przeciążenia i uczestniczyć w wnioskowanie o typie. Zobacz [funkcji elementów członkowskich](expressions.md#function-members) więcej szczegółowych informacji.

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>Ocena funkcja anonimowa konwersji na typy delegatów

Funkcja anonimowa konwersji do typu delegata tworzy wystąpienie delegata, który odwołuje się funkcja anonimowa, a także (prawdopodobnie pusta) zestaw przechwyconych zmiennych zewnętrznych, które są aktywne w czasie oceny. Gdy obiekt delegowany jest wywoływany, jest wykonywany treści funkcji anonimowych. Kod w treści jest wykonywane przy użyciu zestawu przechwyconych zmiennych zewnętrznych przywoływane przez delegata.

Lista wywołanie delegata z funkcją anonimową zawiera pojedynczy wpis. Obiekt docelowy dokładne i docelowej metody delegata są nieokreślone. W szczególności jest nieokreślony czy obiektu docelowego delegata jest `null`, `this` wartość otaczającego element członkowski funkcji lub innego obiektu.

Konwersje semantycznie identyczne funkcje anonimowe przy użyciu tego samego zestawu (prawdopodobnie pusta) przechwyconego wystąpień zmiennych zewnętrznych te same typy delegatów są dozwolone (ale nie muszą) do zwracane to samo wystąpienie delegata. Termin semantycznie identyczne jest używany w tym miejscu oznacza, że wykonanie funkcji anonimowych we wszystkich przypadkach generuje ten sam wpływ, biorąc pod uwagę te same argumenty. Ta reguła pozwala następującego optymalizację kodu.

```csharp
delegate double Function(double x);

class Test
{
    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void F(double[] a, double[] b) {
        a = Apply(a, (double x) => Math.Sin(x));
        b = Apply(b, (double y) => Math.Sin(y));
        ...
    }
}
```

Ponieważ dwa delegaty funkcja anonimowa mają takie same (pusty) Ustaw przechwyconych zmiennych zewnętrznych, a ponieważ funkcje anonimowe są semantycznie identyczne, kompilator jest mogą mieć delegatów odnoszą się do tej samej metody docelowej. Rzeczywiście kompilator może zwrócić bardzo tego samego wystąpienia delegata z oba wyrażenia funkcja anonimowa.

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>Ocena funkcja anonimowa konwersji na typy drzewa wyrażeń

Konwersja z funkcją anonimową na typ drzewa wyrażeń generuje drzewo wyrażenia ([typy drzewa wyrażenie](types.md#expression-tree-types)). Mówiąc ściślej oceny konwersji funkcja anonimowa prowadzi do tworzenia struktury obiektu, który reprezentuje strukturę sama funkcja anonimowa. Dokładne strukturę drzewa wyrażeń, a także dokładnie proces tworzenia, jest definiowany przez implementację.

### <a name="implementation-example"></a>Przykład wdrożenia

W tej sekcji opisano możliwe wykonania konwersji funkcja anonimowa pod względem innych konstrukcji języka C#. Implementacja opisane w tym miejscu opiera się na te same zasady, które są używane przez kompilator Microsoft C#, ale w żadnym wypadku nie stanowi implementację obowiązkowego ani czy jest możliwe tylko jeden. Krótko zawiera informacje o konwersji na drzew wyrażeń, zgodnie z ich dokładną semantyka jest poza zakresem tej specyfikacji.

Dalszej części tej sekcji zawiera kilka przykładów kodu, który zawiera funkcje anonimowe o różnej charakterystyce. Przykładowo każda znajduje się odpowiedni translacji na kod, który używa tylko innych C# konstrukcji. W przykładach identyfikator `D` przejmuje reprezentują następujący typ delegata:
```csharp
public delegate void D();
```

Najprostsza forma funkcją anonimową to taki, który przechwytuje nie zmiennych zewnętrznych:
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

Mogą to być tłumaczone do tworzenia wystąpienia delegata, który odwołuje się do wygenerowanego przez kompilator metody statycznej, w którym znajduje się kod funkcja anonimowa:
```csharp
class Test
{
    static void F() {
        D d = new D(__Method1);
    }

    static void __Method1() {
        Console.WriteLine("test");
    }
}
```

W poniższym przykładzie funkcja anonimowa odwołuje się elementy członkowskie wystąpień `this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

Mogą to być tłumaczone wygenerowanego przez kompilator metodą wystąpienia, zawierającego kod funkcja anonimowa:
```csharp
class Test
{
    int x;

    void F() {
        D d = new D(__Method1);
    }

    void __Method1() {
        Console.WriteLine(x);
    }
}
```

W tym przykładzie funkcja anonimowa przechwytuje zmienną lokalną:
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

Okres istnienia zmiennej lokalnej muszą teraz rozszerzone do co najmniej okresu istnienia obiektu delegowanego funkcja anonimowa. Można to osiągnąć przez "podnoszenia" zmiennej lokalnej w polu klasy wygenerowanego przez kompilator. Podczas tworzenia wystąpienia zmiennej lokalnej ([wystąpienia zmienne lokalne](expressions.md#instantiation-of-local-variables)) odnosi się do tworzenia wystąpienia klasy wygenerowanego przez kompilator i uzyskiwania dostępu do zmiennej lokalnej odnosi się do uzyskiwania dostępu do pola w wystąpieniu programu Klasa wygenerowanego przez kompilator. Ponadto funkcja anonimowa staje się metodą wystąpienia klasy wygenerowanego przez kompilator:
```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.y = 123;
        D d = new D(__locals1.__Method1);
    }

    class __Locals1
    {
        public int y;

        public void __Method1() {
            Console.WriteLine(y);
        }
    }
}
```

Ponadto następujące anonimowe funkcji przechwytywania `this` oraz dwie zmienne lokalne przy użyciu różnych okresów istnienia:
```csharp
class Test
{
    int x;

    void F() {
        int y = 123;
        for (int i = 0; i < 10; i++) {
            int z = i * 2;
            D d = () => { Console.WriteLine(x + y + z); };
        }
    }
}
```

W tym miejscu klasy wygenerowanego przez kompilator jest tworzony dla każdej instrukcji bloku, w których zmienne lokalne są przechwytywane w taki sposób, że zmienne w różne bloki można okresy istnienia niezależny. Wystąpienie `__Locals2`, klasa wygenerowanego przez kompilator dla bloku instrukcji wewnętrzny zawiera zmienną lokalną `z` i pola, które odwołuje się do wystąpienia `__Locals1`.  Wystąpienie `__Locals1`, klasa wygenerowanego przez kompilator dla bloku instrukcji zewnętrzne zawiera zmienną lokalną `y` i pola, które odwołuje się do `this` otaczającej funkcji elementu członkowskiego. Za pomocą tych struktur danych, można dotrzeć do wszystkich przechwyconych zmiennych zewnętrznych za pośrednictwem wystąpienia `__Local2`, i w ten sposób można zaimplementować kod funkcja anonimowa jako metodę wystąpienia tej klasy.

```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.__this = this;
        __locals1.y = 123;
        for (int i = 0; i < 10; i++) {
            __Locals2 __locals2 = new __Locals2();
            __locals2.__locals1 = __locals1;
            __locals2.z = i * 2;
            D d = new D(__locals2.__Method1);
        }
    }

    class __Locals1
    {
        public Test __this;
        public int y;
    }

    class __Locals2
    {
        public __Locals1 __locals1;
        public int z;

        public void __Method1() {
            Console.WriteLine(__locals1.__this.x + __locals1.y + z);
        }
    }
}
```

Tę samą technikę, w tym miejscu stosowana do przechwytywania zmiennych lokalnych można również podczas konwertowania funkcjami anonimowymi w drzewach wyrażeń: Odwołania do obiektów wygenerowanego przez kompilator mogą być przechowywane w drzewie wyrażeń a dostęp do zmiennych lokalnych może być reprezentowany jako pole uzyskuje dostęp do tych obiektów. Zaletą tego podejścia jest możliwość "urządzenie" zmienne lokalne, które mogą być współużytkowane przez delegatów i drzew wyrażeń.

## <a name="method-group-conversions"></a>Konwersje grupy — metoda

Niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje w grupie — metoda ([klasyfikacje wyrażenie](expressions.md#expression-classifications)) do typu delegata zgodne. Podany typ delegata `D` i wyrażenie `E` , zostanie sklasyfikowany jako grupy metod, istnieje niejawna konwersja z `E` do `D` Jeśli `E` zawiera co najmniej jedną metodę, która ma zastosowanie w jego (normalny formularz [Dotyczy funkcja składowa](expressions.md#applicable-function-member)) do listy argumentów skonstruowany przez użycie typów parametrów i modyfikatorów elementu `D`, zgodnie z opisem poniżej.

Stosowanie kompilacji konwersja grupy metod `E` do typu delegata `D` opisano poniżej. Należy pamiętać, że istnienie niejawna konwersja z `E` do `D` nie gwarantuje, że aplikacja kompilacji konwersji powiedzie się bez błędów.

*  Jedna metoda `M` jest zaznaczone, odpowiadający wywołaniu metody ([wywołań metody opisywanego](expressions.md#method-invocations)) w postaci `E(A)`, z następującymi zmianami:
    * Lista argumentów `A` znajduje się lista wyrażeń, wszystkich sklasyfikowanych jako zmienna oraz typ i modyfikator (`ref` lub `out`) z odpowiadającym mu parametrem w *formal_parameter_list* z `D`.
    * Metody Release candidate uważane za są tylko metody, które mają zastosowanie w ich normalnym formie ([dotyczy funkcja składowa](expressions.md#applicable-function-member)), wyklucza te mające zastosowanie tylko w postaci rozwinięty.
*  Jeśli algorytm [wywołań metody opisywanego](expressions.md#method-invocations) spowoduje błąd, a następnie wystąpi błąd kompilacji. W przeciwnym razie algorytm tworzy pojedynczy najlepszą metodę `M` mających taką samą liczbę parametrów jak `D` i konwersja jest uznawany za istnieje.
*  Wybranej metody `M` musi być zgodny ([delegować zgodności](delegates.md#delegate-compatibility)) z typem delegata `D`, lub w przeciwnym razie wystąpi błąd kompilacji.
*  Jeżeli wybrane metody `M` jest metodą wystąpienia, wyrażenie wystąpienia skojarzone z `E` określa obiektu docelowego delegata.
*  Jeśli wybranej metody M jest metodą rozszerzenia, która jest oznaczona za pomocą dostęp do elementu członkowskiego, na wyrażeniu wystąpienie, to wyrażenie wystąpienia określa obiektu docelowego delegata.
*  Wynik konwersji jest wartość typu `D`, czyli nowo utworzony delegat, który odwołuje się do wybranego obiektu metody i docelowej.
*  Należy zauważyć, że ten proces może prowadzić do utworzenia delegata z metodą rozszerzenia, jeśli algorytm [wywołań metody opisywanego](expressions.md#method-invocations) nie można odnaleźć metody wystąpienia, ale powiedzie się podczas przetwarzania wywołania elementu `E(A)` jako rozszerzenie Wywołanie metody ([wywołań metod rozszerzenia](expressions.md#extension-method-invocations)). Obiekt delegowany, utworzony w ten sposób przechwytuje metodę rozszerzenia, a także swój pierwszy argument.

W poniższym przykładzie pokazano metodę konwersji grup:
```csharp
delegate string D1(object o);

delegate object D2(string s);

delegate object D3();

delegate string D4(object o, params object[] a);

delegate string D5(int i);

class Test
{
    static string F(object o) {...}

    static void G() {
        D1 d1 = F;            // Ok
        D2 d2 = F;            // Ok
        D3 d3 = F;            // Error -- not applicable
        D4 d4 = F;            // Error -- not applicable in normal form
        D5 d5 = F;            // Error -- applicable but not compatible

    }
}
```

Przypisanie do `d1` niejawnie konwertuje grupy metod `F` na wartość typu `D1`.

Przypisanie do `d2` pokazuje, jak można utworzyć obiekt delegowany do metody, która ma mniej pochodnego (kontrawariantne) typy parametrów i bardziej pochodne typ zwracany (kowariantne).

Przypisanie do `d3` pokazuje jak nie istnieje konwersja, jeśli metoda nie ma zastosowania.

Przypisanie do `d4` pokazuje, jak metoda musi być stosowane w postaci normalny.

Przypisanie do `d5` pokazuje, jak typy parametrów i zwrotu delegata i metody mogą różnią się tylko dla typów odwołań.

Podobnie jak w przypadku wszystkich innych Konwersje jawne i niejawne, aby jawnie przeprowadzić konwersję grupy metoda może służyć operatora rzutowania. W związku z tym w tym przykładzie
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
Zamiast tego można zapisać
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

Grupy metod może mieć wpływ na rozpoznanie przeciążenia i uczestniczyć w wnioskowanie o typie. Zobacz [funkcji elementów członkowskich](expressions.md#function-members) więcej szczegółowych informacji.

Obliczanie czasu wykonywania metody konwersji grupy rozpoczynające się w następujący sposób:

*  Jeśli metoda wybrana w czasie kompilacji jest metodą wystąpienia lub jest metodą rozszerzenia, który jest dostępny jako metodę wystąpienia, obiektu docelowego delegata jest określana na podstawie wyrażenia wystąpienia skojarzone z `E`:
    * Obliczane jest wyrażenie wystąpienia. Jeśli ta ocena powoduje wyjątek, nie trzeba wykonywać dalszych czynności są wykonywane.
    * Jeżeli to wyrażenie wystąpienia *reference_type*, że wartość obliczona przez wyrażenia wystąpienia staje się obiektem docelowym. Jeśli wybrany metoda jest metodą wystąpienia, a obiekt docelowy jest `null`, `System.NullReferenceException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.
    * Jeżeli to wyrażenie wystąpienia *value_type*, operację pakowania ([konwersje Boxing](types.md#boxing-conversions)) jest przeprowadzana w celu konwertowania wartości na obiekt, a ten obiekt staje się obiektem docelowym.
*  W przeciwnym razie jest częścią wywołanie metody statycznej w wybranej metody i obiekt docelowy delegata jest `null`.
*  Nowe wystąpienie typu delegata `D` jest przydzielony. Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia `System.OutOfMemoryException` jest generowany, a nie trzeba wykonywać dalszych czynności są wykonywane.
*  Nowe wystąpienie delegata jest inicjowany z odwołaniem do metody, która została określona w czasie kompilacji i odwołanie do obiektu docelowego jest obliczana powyżej.
