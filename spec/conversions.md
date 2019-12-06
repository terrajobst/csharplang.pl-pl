---
ms.openlocfilehash: 4d6d28a3127bc701867afe157aa5496377a06f69
ms.sourcegitcommit: 63d276488c9770a565fd787020783ffc1d2af9d6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868007"
---
# <a name="conversions"></a>Konwersje

***Konwersja*** umożliwia przetraktowanie wyrażenia jako elementu określonego typu. Konwersja może spowodować, że wyrażenie danego typu ma być traktowane jako mające inny typ lub może spowodować wyrażenie bez typu, aby uzyskać typ. Konwersje mogą być ***niejawne*** lub ***jawne***, a to określa, czy jest wymagane jawne rzutowanie. Na przykład konwersja z typu `int` na typ `long` jest niejawna, dlatego wyrażenia typu `int` mogą być niejawnie traktowane jako `long`typu. Odwrotna konwersja z typu `long` do typu `int`, jest jawna i dlatego wymagane jest jawne rzutowanie.

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

Niektóre konwersje są definiowane przez język. Programy mogą także definiować własne konwersje ([konwersje zdefiniowane przez użytkownika](conversions.md#user-defined-conversions)).

## <a name="implicit-conversions"></a>Konwersje niejawne

Następujące konwersje są klasyfikowane jako konwersje niejawne:

*  Konwersje tożsamości
*  Niejawne konwersje liczbowe
*  Niejawne konwersje wyliczenia
*  Niejawne konwersje ciągu interpolowanego
*  Niejawne konwersje dopuszczające wartość null
*  Konwersje literałów null
*  Niejawne konwersje odwołań
*  Konwersje z opakowania
*  Niejawne konwersje dynamiczne
*  Niejawne konwersje wyrażeń stałych
*  Konwersje niejawne zdefiniowane przez użytkownika
*  Konwersje funkcji anonimowych
*  Konwersje grup metod

Konwersje niejawne mogą odbywać się w różnych sytuacjach, w tym wywołaniach elementów członkowskich funkcji ([Sprawdzanie czasu kompilacji dynamicznego rozpoznawania przeciążenia](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), wyrażenia rzutowania ([wyrażenia rzutowania](expressions.md#cast-expressions)) i przypisania ([Operatory przypisywania](expressions.md#assignment-operators)).

Wstępnie zdefiniowane konwersje niejawne zawsze kończą się powodzeniem i nigdy nie powodują zgłaszania wyjątków. Odpowiednio zaprojektowane zdefiniowane przez użytkownika Konwersje niejawne powinny również mieć te cechy.

Na potrzeby konwersji typy `object` i `dynamic` są uważane za równoważne.

Jednak konwersje dynamiczne ([niejawne konwersje dynamiczne](conversions.md#implicit-dynamic-conversions) i [jawne konwersje dynamiczne](conversions.md#explicit-dynamic-conversions)) mają zastosowanie tylko do wyrażeń typu `dynamic` ([Typ dynamiczny](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Konwersja tożsamości

Konwersja tożsamości jest konwertowana z dowolnego typu na ten sam typ. Ta konwersja istnieje w taki sposób, aby jednostka, która już ma wymagany typ, mogła zostać poddana konwersji na ten typ.

*  Ponieważ `object` i `dynamic` są uważane za równoważne, istnieje konwersja tożsamości między `object` i `dynamic`i między tworzonymi typami, które są takie same podczas zastępowania wszystkich wystąpień `dynamic` z `object`.

### <a name="implicit-numeric-conversions"></a>Niejawne konwersje liczbowe

Niejawne konwersje liczbowe są następujące:

*  Z `sbyte` do `short`, `int`, `long`, `float`, `double`lub `decimal`.
*  Z `byte` do `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`lub `decimal`.
*  Z `short` do `int`, `long`, `float`, `double`lub `decimal`.
*  Z `ushort` do `int`, `uint`, `long`, `ulong`, `float`, `double`lub `decimal`.
*  Od `int` do `long`, `float`, `double`lub `decimal`.
*  Z `uint` do `long`, `ulong`, `float`, `double`lub `decimal`.
*  Od `long` do `float`, `double`lub `decimal`.
*  Od `ulong` do `float`, `double`lub `decimal`.
*  Z `char` do `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`lub `decimal`.
*  Z `float` `double`.

Konwersje z `int`, `uint`, `long`lub `ulong` do `float` oraz od `long` lub `ulong` do `double` mogą spowodować utratę precyzji, ale nigdy nie spowoduje utraty wielkości. Inne niejawne konwersje numeryczne nigdy nie tracą żadnych informacji.

Brak niejawnych konwersji do typu `char`, dlatego wartości innych typów całkowitych nie są automatycznie konwertowane na typ `char`.

### <a name="implicit-enumeration-conversions"></a>Niejawne konwersje wyliczenia

Niejawna konwersja Wyliczenie zezwala na konwersję *decimal_integer_literal* `0` na dowolną *enum_type* i *nullable_type* , których typem podstawowym jest *enum_type*. W tym drugim przypadku konwersja jest oceniana przez przekonwertowanie na bazowe *enum_type* i Zawijanie wyniku ([Typy dopuszczające wartość null](types.md#nullable-types)).

### <a name="implicit-interpolated-string-conversions"></a>Niejawne konwersje ciągu interpolowanego

Niejawna konwersja ciągu interpolowanego zezwala na konwersję *interpolated_string_expression* ([ciąg interpolowany](expressions.md#interpolated-strings)) na `System.IFormattable` lub `System.FormattableString` (który implementuje `System.IFormattable`).

Gdy ta konwersja jest stosowana, wartość ciągu nie jest złożona z ciągu interpolowanego. Zamiast tego tworzone jest wystąpienie `System.FormattableString`, jak dokładniej opisane w [ciągach interpolowanych](expressions.md#interpolated-strings).

### <a name="implicit-nullable-conversions"></a>Niejawne konwersje dopuszczające wartość null

Wstępnie zdefiniowane konwersje niejawne, które działają na typach wartości, które nie dopuszczają wartości null, mogą również być używane z dopuszczaniem wartości null dla tych typów. Dla każdej wstępnie zdefiniowanej tożsamości niejawnej i konwersji liczbowych, które konwertują z typu wartości niedopuszczających wartości null `S` do `T`typu wartości niedopuszczających wartości null, istnieją następujące niejawne konwersje dopuszczające wartość null:

*  Niejawna konwersja z `S?` na `T?`.
*  Niejawna konwersja z `S` na `T?`.

Obliczanie niejawnej konwersji wartości null na podstawie bazowej konwersji z `S` na `T` postępuje w następujący sposób:

*  Jeśli konwersja dopuszcza wartość null z `S?` do `T?`:
    * Jeśli wartością źródłową jest null (`HasValue` właściwość ma wartość false), wynik jest wartością null typu `T?`.
    * W przeciwnym razie konwersja jest oceniana jako odpakowanie od `S?` do `S`, a po niej bazowa konwersja z `S` do `T`, po którym następuje Zawijanie ([Typy dopuszczające wartości null](types.md#nullable-types)) od `T` do `T?`.

*  Jeśli konwersja do wartości null jest z `S` do `T?`, konwersja jest szacowana jako bazowa konwersja z `S` do `T`, a po nim otoka z `T` `T?`.

### <a name="null-literal-conversions"></a>Konwersje literałów null

Niejawna konwersja istnieje w literale `null` do dowolnych typów dopuszczających wartość null. Ta konwersja generuje wartość null ([Typy Nullable](types.md#nullable-types)) danego typu dopuszczającego wartości null.

### <a name="implicit-reference-conversions"></a>Niejawne konwersje odwołań

Konwersje niejawnych odwołań są następujące:

*  Z dowolnych *reference_type* do `object` i `dynamic`.
*  Z dowolnych `S` *class_type* do wszystkich *class_type* `T`dostarczone `S` pochodzą z `T`.
*  Z dowolnego *class_typeu* `S` do dowolnego *INTERFACE_TYPE* `T`podano `S` implementuje `T`.
*  Z dowolnych `S` *INTERFACE_TYPE* do wszystkich *interface_type* `T`dostarczone `S` pochodzą z `T`.
*  Z *array_type* `S` z typem elementu `SE` do *array_type* `T` z typem elementu `TE`, pod warunkiem spełnione są wszystkie z następujących warunków:
    * `S` i `T` różnią się tylko w typie elementu. Innymi słowy, `S` i `T` mają taką samą liczbę wymiarów.
    * Zarówno `SE`, jak i `TE` są *reference_type*s.
    * W `SE` do `TE`istnieje niejawna konwersja odwołania.
*  Z dowolnego *array_type* do `System.Array` i interfejsów, które implementuje.
*  Z jednowymiarowego typu tablicy `S[]` do `System.Collections.Generic.IList<T>` i jego interfejsów podstawowych, pod warunkiem, że istnieje niejawna tożsamość lub konwersja odwołań z `S` do `T`.
*  Z dowolnego *delegate_type* do `System.Delegate` i interfejsów, które implementuje.
*  Z literału null do dowolnego *reference_type*.
*  Z dowolnych *reference_type* do *reference_type* `T`, jeśli ma niejawną tożsamość lub konwersję odwołania do *reference_type* `T0` i `T0` ma konwersję tożsamości na `T`.
*  Z dowolnego *reference_type* do interfejsu lub typu delegata `T`, jeśli ma niejawną tożsamość lub konwersję odniesienia do interfejsu lub typu delegata `T0` a `T0` jest konwersją wariancji ([Konwersja wariancji](interfaces.md#variance-conversion)) na `T`.
*  Niejawne konwersje obejmujące parametry typu, które są znane jako typy odwołań. Zobacz [niejawne konwersje dotyczące parametrów typu](conversions.md#implicit-conversions-involving-type-parameters) , aby uzyskać więcej informacji na temat niejawnych konwersji obejmujących parametry typu.

Konwersje niejawnych odwołań są tymi konwersjami między *reference_type*s, które mogą być sprawdzone, aby zawsze kończyły się powodzeniem, dlatego nie wymagają kontroli w czasie wykonywania.

Konwersje odwołań, niejawne lub jawne, nigdy nie zmieniają tożsamości referencyjnej konwertowanego obiektu. Innymi słowy, podczas gdy konwersja odwołania może zmienić typ odwołania, nigdy nie zmienia typu lub wartości obiektu, do którego odwołuje się.

### <a name="boxing-conversions"></a>Konwersje z opakowania

Konwersja z opakowania zezwala na niejawne przekonwertowanie *value_type* na typ referencyjny. Konwersja napakowywania istnieje z dowolnego *non_nullable_value_type* do `object` i `dynamic`, do `System.ValueType` i do wszystkich *INTERFACE_TYPE* wdrożonych przez *non_nullable_value_type*. Ponadto *enum_type* można przekonwertować na typ `System.Enum`.

Konwersja z opakowania istnieje z *nullable_type* do typu referencyjnego, jeśli i tylko wtedy, gdy istnieje konwersja z bazowego *non_nullable_value_type* do typu odwołania.

Typ wartości ma zapakowywanie konwersji do typu interfejsu `I`, jeśli ma ona konwersję na typ interfejsu `I0` i `I0` ma konwersję tożsamości na `I`.

Typ wartości ma konwersję z opakowaniem do typu interfejsu `I`, jeśli ma ona konwersję do interfejsu lub typu delegata `I0`, a `I0` jest zmienności wariancji ([Konwersja wariancji](interfaces.md#variance-conversion)) na `I`.

Opakowanie wartości *non_nullable_value_type* obejmuje przydzielenie wystąpienia obiektu i skopiowanie wartości *value_type* do tego wystąpienia. Struktura może być opakowana do typu `System.ValueType`, ponieważ jest klasą bazową dla wszystkich struktur ([dziedziczenie](structs.md#inheritance)).

Wypakowywanie wartości *nullable_type* jest następująca:

*  Jeśli wartością źródłową jest null (`HasValue` właściwość ma wartość false), wynik jest odwołaniem null typu docelowego.
*  W przeciwnym razie wynik jest odwołaniem do opakowanej `T` wygenerowanej przez odpakowanie i opakowanie wartości źródłowej.

Konwersje napakowywania są opisane w dalszej [konwersji opakowania](types.md#boxing-conversions).

### <a name="implicit-dynamic-conversions"></a>Niejawne konwersje dynamiczne

Niejawna konwersja dynamiczna istnieje z wyrażenia typu `dynamic` do dowolnego typu `T`. Konwersja jest powiązana dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)), co oznacza, że niejawna konwersja zostanie poszukiwana w czasie wykonywania z typu wyrażenia do `T`. Jeśli konwersja nie zostanie znaleziona, zostanie zgłoszony wyjątek czasu wykonywania.

Należy zauważyć, że ta niejawna konwersja pozornie narusza wskazówkę na początku [konwersji niejawnych](conversions.md#implicit-conversions) , których niejawna konwersja nigdy nie powinna spowodować wyjątku. Nie jest to jednak sama konwersja, ale *Wyszukiwanie* konwersji, która powoduje wyjątek. Ryzyko wyjątków w czasie wykonywania jest związane z użyciem powiązania dynamicznego. Jeśli dynamiczne powiązanie konwersji nie jest wymagane, można najpierw przekonwertować wyrażenie na `object`, a następnie do żądanego typu.

Poniższy przykład ilustruje niejawne konwersje dynamiczne:

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

Przypisania do `s2` i `i` używają niejawnych konwersji dynamicznych, w przypadku których powiązanie operacji jest wstrzymane do czasu wykonania. W czasie wykonywania niejawne konwersje są poszukiwane z typu czasu wykonywania `d` -- `string`--do typu docelowego. Znaleziono konwersję do `string` ale nie `int`.

### <a name="implicit-constant-expression-conversions"></a>Niejawne konwersje wyrażeń stałych

Niejawna konwersja wyrażeń stałych umożliwia wykonywanie następujących konwersji:

*  *Constant_expression* ([wyrażenia stałe](expressions.md#constant-expressions)) typu `int` można przekonwertować na typ `sbyte`, `byte`, `short`, `ushort`, `uint`lub `ulong`, pod warunkiem, że wartość *constant_expression* należy do zakresu typu docelowego.
*  *Constant_expression* typu `long` można przekonwertować na `ulong`typu, pod warunkiem, że wartość *constant_expression* nie jest ujemna.

### <a name="implicit-conversions-involving-type-parameters"></a>Niejawne konwersje obejmujące parametry typu

Następujące konwersje niejawne istnieją dla danego parametru typu `T`:

*  Od `T` do swojej skutecznej klasy bazowej `C`, od `T` do dowolnej klasy bazowej `C`i od `T` do dowolnego interfejsu zaimplementowanego przez `C`. W czasie wykonywania, jeśli `T` jest typem wartości, konwersja jest wykonywana jako konwersja z opakowania. W przeciwnym razie konwersja jest wykonywana jako niejawna konwersja odwołania lub konwersja tożsamości.
*  Od `T` do typu interfejsu `I` w `T`obowiązującym interfejsie i od `T` do dowolnego interfejsu podstawowego `I`. W czasie wykonywania, jeśli `T` jest typem wartości, konwersja jest wykonywana jako konwersja z opakowania. W przeciwnym razie konwersja jest wykonywana jako niejawna konwersja odwołania lub konwersja tożsamości.
*  Z `T` do `U`parametru typu, udostępnione `T` zależy od `U` ([ograniczenia parametru typu](classes.md#type-parameter-constraints)). W czasie wykonywania, jeśli `U` jest typem wartości, wówczas `T` i `U` są takie same, a konwersja nie jest przeprowadzana. W przeciwnym razie, jeśli `T` jest typem wartości, konwersja jest wykonywana jako konwersja z opakowania. W przeciwnym razie konwersja jest wykonywana jako niejawna konwersja odwołania lub konwersja tożsamości.
*  Z literału wartości null do `T`dostarczone `T` jest znany jako typ referencyjny.
*  Od `T` do typu referencyjnego `I`, jeśli ma niejawną konwersję na typ referencyjny, `S0` a `S0` ma konwersję tożsamości na `S`. W czasie wykonywania konwersja jest wykonywana w taki sam sposób, jak konwersja do `S0`.
*  Od `T` do typu interfejsu `I`, jeśli ma niejawną konwersję do interfejsu lub typu delegata, `I0` a `I0` jest konwersją wariancji na `I` ([Konwersja wariancji](interfaces.md#variance-conversion)). W czasie wykonywania, jeśli `T` jest typem wartości, konwersja jest wykonywana jako konwersja z opakowania. W przeciwnym razie konwersja jest wykonywana jako niejawna konwersja odwołania lub konwersja tożsamości.

Jeśli `T` jest znany jako typ referencyjny ([ograniczenia parametru typu](classes.md#type-parameter-constraints)), powyższe konwersje są klasyfikowane jako niejawne konwersje odwołań ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions)). Jeśli `T` nie jest znany jako typ referencyjny, powyższe konwersje są klasyfikowane jako konwersje z opakowaniem ([konwersje z opakowania](conversions.md#boxing-conversions)).

### <a name="user-defined-implicit-conversions"></a>Konwersje niejawne zdefiniowane przez użytkownika

Niejawna konwersja zdefiniowana przez użytkownika składa się z opcjonalnej standardowej konwersji niejawnej, a następnie wykonania operatora niejawnej konwersji zdefiniowanej przez użytkownika, a następnie innej opcjonalnej standardowej konwersji niejawnej. Dokładne reguły oceniania niejawnych konwersji zdefiniowanych przez użytkownika są opisane w temacie [Przetwarzanie konwersji niejawnych zdefiniowanych przez użytkownika](conversions.md#processing-of-user-defined-implicit-conversions).

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>Konwersje funkcji anonimowych i konwersje grup metod

Funkcje anonimowe i grupy metod nie mają typów w i samych siebie, ale mogą być niejawnie konwertowane na typy delegatów lub typy drzewa wyrażeń. Konwersje funkcji anonimowych są opisane bardziej szczegółowo w przypadku [konwersji funkcji anonimowych](conversions.md#anonymous-function-conversions) i konwersji grup metod w przypadku [konwersji grup metod](conversions.md#method-group-conversions).

## <a name="explicit-conversions"></a>Konwersje jawne

Następujące konwersje są klasyfikowane jako Konwersje jawne:

*  Wszystkie konwersje niejawne.
*  Jawne konwersje numeryczne.
*  Jawne konwersje Wyliczenie.
*  Jawne konwersje dopuszczające wartość null.
*  Jawne konwersje odwołań.
*  Jawne konwersje interfejsów.
*  Konwersje rozpakowywanie.
*  Jawne konwersje dynamiczne
*  Jawne konwersje zdefiniowane przez użytkownika.

Jawne konwersje mogą wystąpić w wyrażeniach rzutowania ([wyrażenia rzutowania](expressions.md#cast-expressions)).

Zestaw jawnych konwersji zawiera wszystkie konwersje niejawne. Oznacza to, że dozwolone są nadmiarowe wyrażenia rzutowania.

Konwersje jawne, które nie są konwersjemi niejawnymi to konwersje, które nie mogą być sprawdzane w celu pomyślnego przeprowadzenia konwersji, które są znane do utraty informacji, a konwersje w różnych domenach typów są wystarczająco różne, aby oznaczać jawne służąc.

### <a name="explicit-numeric-conversions"></a>Jawne konwersje numeryczne

Jawne konwersje numeryczne to konwersje z *numeric_type* do innej *numeric_type* , dla których niejawna konwersja liczbowa ([niejawne konwersje liczbowe](conversions.md#implicit-numeric-conversions)) jeszcze nie istnieje:

*  Z `sbyte` do `byte`, `ushort`, `uint`, `ulong`lub `char`.
*  Z `byte` `sbyte` i `char`.
*  Z `short` do `sbyte`, `byte`, `ushort`, `uint`, `ulong`lub `char`.
*  Od `ushort` do `sbyte`, `byte`, `short`lub `char`.
*  Z `int` do `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`lub `char`.
*  Z `uint` do `sbyte`, `byte`, `short`, `ushort`, `int`lub `char`.
*  Z `long` do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`lub `char`.
*  Z `ulong` do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`lub `char`.
*  Od `char` do `sbyte`, `byte`lub `short`.
*  Z `float` do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`lub `decimal`.
*  Z `double` do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`lub `decimal`.
*  Z `decimal` do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`lub `double`.

Ponieważ jawne konwersje obejmują wszystkie niejawnych i jawnych konwersji liczbowych, zawsze możliwe jest przekonwertowanie z dowolnego *numeric_type* na inne *numeric_type* przy użyciu wyrażenia CAST ([wyrażenia rzutowania](expressions.md#cast-expressions)).

Jawne konwersje numeryczne mogą spowodować utratę informacji lub spowodować, że wyjątki mają być zgłaszane. Jawna konwersja liczbowa jest przetwarzana w następujący sposób:

*  Dla konwersji z typu całkowitego na inny typ całkowity, przetwarzanie zależy od kontekstu sprawdzania przepełnienia ([Operatory sprawdzone i niesprawdzane](expressions.md#the-checked-and-unchecked-operators)), w którym odbywa się konwersja:
    * W kontekście `checked` konwersja kończy się powodzeniem, jeśli wartość operandu źródłowego należy do zakresu typu docelowego, ale zgłasza `System.OverflowException`, jeśli wartość operandu źródła jest spoza zakresu typu docelowego.
    * W kontekście `unchecked` konwersja zawsze kończy się powodzeniem i przechodzi w następujący sposób.
        * Jeśli typ źródła jest większy niż typ docelowy, wartość źródłowa zostanie obcięta przez odrzucenie jej najbardziej znaczących bitów. Następnie wynik jest traktowany jako wartość typu docelowego.
        * Jeśli typ źródła jest mniejszy niż typ docelowy, wartość źródłowa jest podwyższana lub równa zero, tak że jest to ten sam rozmiar, co typ docelowy. Jeśli typ źródła jest podpisany, jest używane rozszerzenie-znak. Jeśli typ źródła jest niepodpisany, jest używane rozszerzenie o wartości zero. Następnie wynik jest traktowany jako wartość typu docelowego.
        * Jeśli typ źródła ma taki sam rozmiar jak typ docelowy, wartość źródłowa jest traktowana jako wartość typu docelowego.
*  Dla konwersji z `decimal` na typ całkowity, wartość źródłowa jest zaokrąglana do zera do najbliższej wartości całkowitej, a ta wartość całkowita będzie wynikiem konwersji. Jeśli uzyskana wartość całkowita jest poza zakresem typu docelowego, zostanie zgłoszony `System.OverflowException`.
*  W przypadku konwersji z `float` lub `double` na typ całkowity, przetwarzanie zależy od kontekstu sprawdzania przepełnienia ([Operatory sprawdzone i niezaznaczone](expressions.md#the-checked-and-unchecked-operators)), w których odbywa się konwersja:
    * W kontekście `checked` konwersja wykonuje następujące czynności:
        * Jeśli wartość operandu jest NaN lub nieskończona, zostanie zgłoszony `System.OverflowException`.
        * W przeciwnym razie argument źródłowy jest zaokrąglany do zera do najbliższej wartości całkowitej. Jeśli ta wartość całkowita znajduje się w zakresie typu docelowego, ta wartość jest wynikiem konwersji.
        * W przeciwnym razie zostanie zgłoszony `System.OverflowException`.
    * W kontekście `unchecked` konwersja zawsze kończy się powodzeniem i przechodzi w następujący sposób.
        * Jeśli wartość operandu jest NaN lub nieskończona, wynik konwersji jest nieokreśloną wartością typu docelowego.
        * W przeciwnym razie argument źródłowy jest zaokrąglany do zera do najbliższej wartości całkowitej. Jeśli ta wartość całkowita znajduje się w zakresie typu docelowego, ta wartość jest wynikiem konwersji.
        * W przeciwnym razie wynik konwersji jest nieokreśloną wartością typu docelowego.
*  W przypadku konwersji z `double` na `float`wartość `double` jest zaokrąglana do najbliższej wartości `float`. Jeśli wartość `double` jest zbyt mała, aby reprezentować ją jako `float`, wynik przyjmie wartość dodatnią zero lub ujemną zero. Jeśli wartość `double` jest zbyt duża, aby reprezentować ją jako `float`, wynik będzie dodatnią nieskończoność lub ujemną nieskończoność. Jeśli `double` wartością jest NaN, wynik jest również NaN.
*  Dla konwersji z `float` lub `double` do `decimal`, wartość źródłowa jest konwertowana na `decimal` reprezentacja i zaokrąglana do najbliższej liczby po 28 miejsc dziesiętnych, jeśli jest to wymagane ([Typ dziesiętny](types.md#the-decimal-type)). Jeśli wartość źródłowa jest zbyt mała, aby reprezentować ją jako `decimal`, wynik zmieni się na zero. Jeśli wartością źródłową jest NaN, nieskończoność lub zbyt duża do reprezentowania jako `decimal`, zostanie zgłoszony `System.OverflowException`.
*  W przypadku konwersji z `decimal` na `float` lub `double`wartość `decimal` jest zaokrąglana do najbliższej wartości `double` lub `float`. Ta konwersja może spowodować utratę precyzji, ale nigdy nie powoduje zgłoszenia wyjątku.

### <a name="explicit-enumeration-conversions"></a>Jawne konwersje wyliczenia

Jawne konwersje Wyliczenie są następujące:

*  Z `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`lub `decimal` do dowolnej *enum_type*.
*  Z dowolnego *enum_type* do `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`lub `decimal`.
*  Z dowolnego *enum_type* do dowolnego innego *enum_type*.

Jawna konwersja wyliczenia między dwoma typami jest przetwarzana przez traktowanie wszelkich uczestniczących *enum_type* jako typ podstawowy tego *enum_type*, a następnie wykonywanie niejawnej lub jawnej konwersji liczbowej między wynikiem. Na przykład w przypadku *enum_type* `E` z i typem bazowym `int`konwersja z `E` na `byte` jest przetwarzana jako jawna konwersja liczbowa ([jawne konwersje liczbowe](conversions.md#explicit-numeric-conversions)) od `int` do `byte`, a konwersja z `byte` do `E` jest przetwarzana jako niejawna konwersja liczbowa ([niejawne konwersje liczbowe](conversions.md#implicit-numeric-conversions)) od `byte` do `int`.

### <a name="explicit-nullable-conversions"></a>Jawne konwersje dopuszczające wartość null

***Jawne konwersje wartości null*** zezwalają na wstępnie zdefiniowane Konwersje jawne, które działają dla typów wartościowych niedopuszczających wartości null, aby były również używane z dopuszczaniem wartości null dla tych typów. Dla każdej z wstępnie zdefiniowanych konwersji jawnych, które konwertują z typu wartości niedopuszczających wartości null `S` na typ wartości niedopuszczających wartości null `T` ([konwersja tożsamości](conversions.md#identity-conversion), [niejawne konwersje liczbowe](conversions.md#implicit-numeric-conversions), [niejawne konwersje wyliczenia](conversions.md#implicit-enumeration-conversions), [jawne konwersje liczbowe](conversions.md#explicit-numeric-conversions)i [jawne konwersje wyliczenia](conversions.md#explicit-enumeration-conversions)), istnieją następujące konwersje dopuszczające wartość null:

*  Jawna konwersja z `S?` na `T?`.
*  Jawna konwersja z `S` na `T?`.
*  Jawna konwersja z `S?` na `T`.

Obliczanie konwersji dopuszczającej wartość null na podstawie bazowej konwersji z `S`, aby `T` kontynuowały się w następujący sposób:

*  Jeśli konwersja dopuszcza wartość null z `S?` do `T?`:
    * Jeśli wartością źródłową jest null (`HasValue` właściwość ma wartość false), wynik jest wartością null typu `T?`.
    * W przeciwnym razie konwersja jest oceniana jako odpakowanie od `S?` do `S`, a po niej bazowa konwersja z `S` do `T`, po którym następuje Zawijanie z `T` `T?`.
*  Jeśli konwersja do wartości null jest z `S` do `T?`, konwersja jest szacowana jako bazowa konwersja z `S` do `T`, a po nim otoka z `T` `T?`.
*  Jeśli konwersja do wartości null jest z `S?` do `T`, konwersja jest oceniana jako odpakowanie od `S?` do `S`, a następnie bazowa konwersja z `S` do `T`.

Należy zauważyć, że próba odpakowania wartości null spowoduje zgłoszenie wyjątku, jeśli wartość jest `null`.

### <a name="explicit-reference-conversions"></a>Jawne konwersje odwołań

Jawne konwersje odwołań są następujące:

*  Z `object` i `dynamic` do innych *reference_type*.
*  Z dowolnego *class_type* `S` do dowolnego *class_type* `T`, podane `S` jest klasą bazową `T`.
*  Z dowolnego *class_type* `S` do dowolnego *INTERFACE_TYPE* `T`, podane `S` nie jest zapieczętowane i podane `S` nie implementuje `T`.
*  Z dowolnego *interface_type* `S` do dowolnego *class_type* `T`, podane `T` nie jest zapieczętowane ani dostarczone `T` implementuje `S`.
*  Z dowolnego *interface_type* `S` do dowolnego *INTERFACE_TYPE* `T`, dostarczony `S` nie pochodzi od `T`.
*  Z *array_type* `S` z typem elementu `SE` do *array_type* `T` z typem elementu `TE`, pod warunkiem spełnione są wszystkie z następujących warunków:
    * `S` i `T` różnią się tylko w typie elementu. Innymi słowy, `S` i `T` mają taką samą liczbę wymiarów.
    * Zarówno `SE`, jak i `TE` są *reference_type*s.
    * Istnieje jawna konwersja odwołań z `SE` do `TE`.
*  Z `System.Array` i interfejsów, które implementuje do dowolnego *array_type*.
*  Z jednowymiarowego typu tablicy `S[]` do `System.Collections.Generic.IList<T>` i jego interfejsów podstawowych, pod warunkiem, że istnieje jawna konwersja odwołań z `S` do `T`.
*  Od `System.Collections.Generic.IList<S>` i jego interfejsów podstawowych do jednowymiarowego typu tablicy `T[]`, pod warunkiem, że istnieje jawna tożsamość lub konwersja odwołania z `S` do `T`.
*  Z `System.Delegate` i interfejsów, które implementuje do dowolnego *delegate_type*.
*  Z typu referencyjnego do typu referencyjnego `T`, jeśli ma on jawną konwersję referencyjną do typu referencyjnego `T0` i `T0` ma `T`konwersji tożsamości.
*  Z typu referencyjnego do interfejsu lub typu delegata `T`, jeśli ma on jawną konwersję referencyjną do interfejsu lub typu delegata `T0`, a `T0` jest konwersją wariancji do `T` lub `T`, można przekonwertować wariancję na `T0` ([Konwersja wariancji](interfaces.md#variance-conversion)).
*  Od `D<S1...Sn>` do `D<T1...Tn>`, gdzie `D<X1...Xn>` jest ogólnym typem delegata, `D<S1...Sn>` nie jest zgodny z lub identyczna z `D<T1...Tn>`, a dla każdego parametru typu `Xi` `D`:
    * Jeśli `Xi` jest niezmienna, `Si` jest taka sama jak `Ti`.
    * Jeśli `Xi` jest współwariantem, istnieje niejawna lub jawna tożsamość lub konwersja odwołania z `Si` do `Ti`.
    * Jeśli `Xi` jest kontrawariantne, wówczas `Si` i `Ti` są identyczne lub oba typy odwołań.
*  Jawne konwersje obejmujące parametry typu, które są znane jako typy odwołań. Aby uzyskać więcej informacji na temat jawnych konwersji obejmujących parametry typu, zobacz [jawne konwersje dotyczące parametrów typu](conversions.md#explicit-conversions-involving-type-parameters).

Jawne konwersje referencyjne to konwersje między typami referencyjnymi wymagającymi kontroli w czasie wykonywania, aby upewnić się, że są poprawne.

Aby konwersja jawnego odwołania powiodła się w czasie wykonywania, wartość operandu źródła musi być `null`lub rzeczywisty typ obiektu, do którego odwołuje się operand źródłowy, musi być typem, który można przekonwertować na typ docelowy przez niejawną konwersję odwołania ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions)) lub konwersję z opakowaniem ([konwersje na opakowanie](conversions.md#boxing-conversions)). Jeśli jawna konwersja referencyjna nie powiedzie się, zostanie zgłoszony `System.InvalidCastException`.

Konwersje odwołań, niejawne lub jawne, nigdy nie zmieniają tożsamości referencyjnej konwertowanego obiektu. Innymi słowy, podczas gdy konwersja odwołania może zmienić typ odwołania, nigdy nie zmienia typu lub wartości obiektu, do którego odwołuje się.

### <a name="unboxing-conversions"></a>Odpakowywanie konwersji

Konwersja rozpakowywania umożliwia jawne przekonwertowanie typu odwołania na *value_type*. Konwersja rozpakowywania istnieje z typów `object`, `dynamic` i `System.ValueType` do wszelkich *non_nullable_value_type*oraz od dowolnego *interface_type* do dowolnego *non_nullable_value_type* implementującego *INTERFACE_TYPE*. Ponadto typ `System.Enum` może być rozpakowany do dowolnego *enum_type*.

Konwersja rozpakowywania istnieje z typu referencyjnego na *nullable_type* , jeśli istnieje konwersja rozpakowywanie z typu referencyjnego na bazowe *non_nullable_value_type* *nullable_type*.

Typ wartości `S` ma rozpakowywanie konwersji z typu interfejsu `I`, jeśli ma ona konwersję rozpakowywanie z typu interfejsu, `I0` a `I0` ma konwersję tożsamości na `I`.

Typ wartości `S` ma rozpakowywanie konwersji z typu interfejsu `I`, jeśli ma ona konwersję rozpakowywania z interfejsu lub typu delegata, `I0` a `I0` jest konwersją wariancji do `I` lub `I` jest konwertowany na `I0` ([Konwersja wariancji](interfaces.md#variance-conversion)).

Operacja rozpakowywania polega na pierwszym sprawdzeniu, że wystąpienie obiektu jest wartością opakowaną danego *value_type*, a następnie kopiując wartość z wystąpienia. Rozpakowywanie odwołania o wartości null do *nullable_type* powoduje utworzenie wartości null *nullable_type*. Struktura może być oddzielna od typu `System.ValueType`, ponieważ jest klasą bazową dla wszystkich struktur ([dziedziczenie](structs.md#inheritance)).

Konwersje rozpakowywania są szczegółowo opisane w [konwersji](types.md#unboxing-conversions)rozpakowywania.

### <a name="explicit-dynamic-conversions"></a>Jawne konwersje dynamiczne

Jawna konwersja dynamiczna istnieje w wyrażeniu typu `dynamic` do dowolnego typu `T`. Konwersja jest powiązana dynamicznie ([powiązanie dynamiczne](expressions.md#dynamic-binding)), co oznacza, że jawna konwersja będzie poszukiwana w czasie wykonywania z typu wyrażenia do `T`. Jeśli konwersja nie zostanie znaleziona, zostanie zgłoszony wyjątek czasu wykonywania.

Jeśli dynamiczne powiązanie konwersji nie jest wymagane, można najpierw przekonwertować wyrażenie na `object`, a następnie do żądanego typu.

Załóżmy, że zdefiniowano następującą klasę:
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

Poniższy przykład ilustruje jawne konwersje dynamiczne:
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

Najlepszą konwersją `o` na `C` można znaleźć w czasie kompilacji jako jawną konwersję odwołań. To nie powiedzie się w czasie wykonywania, ponieważ `"1"` nie jest w rzeczywistości `C`. Konwersja `d` na `C` jednak jako jawna konwersja dynamiczna jest zawieszona do czasu wykonywania, w której zdefiniowana przez użytkownika konwersja z typu czasu wykonywania `d` -- `string`--do `C` zostanie znaleziona i powiedzie się.

### <a name="explicit-conversions-involving-type-parameters"></a>Jawne konwersje obejmujące parametry typu

Następujące Konwersje jawne istnieją dla danego parametru typu `T`:

*  Od efektywnej klasy bazowej `C` `T` do `T` oraz z dowolnej klasy podstawowej `C` do `T`. W czasie wykonywania, jeśli `T` jest typem wartości, konwersja jest wykonywana jako konwersja rozpakowywanie. W przeciwnym razie konwersja jest wykonywana jako jawna konwersja odwołania lub konwersja tożsamości.
*  Z dowolnego typu interfejsu do `T`. W czasie wykonywania, jeśli `T` jest typem wartości, konwersja jest wykonywana jako konwersja rozpakowywanie. W przeciwnym razie konwersja jest wykonywana jako jawna konwersja odwołania lub konwersja tożsamości.
*  Od `T` do wszelkich *interface_type* `I` z zastrzeżeniem, że nie istnieje jeszcze niejawna konwersja z `T` na `I`. W czasie wykonywania, jeśli `T` jest typem wartości, konwersja jest wykonywana jako konwersja z opakowania, po którym następuje jawna konwersja odwołania. W przeciwnym razie konwersja jest wykonywana jako jawna konwersja odwołania lub konwersja tożsamości.
*  Z parametru typu `U` do `T`, podane `T` zależy od `U` ([ograniczenia parametru typu](classes.md#type-parameter-constraints)). W czasie wykonywania, jeśli `U` jest typem wartości, wówczas `T` i `U` są takie same, a konwersja nie jest przeprowadzana. W przeciwnym razie, jeśli `T` jest typem wartości, konwersja jest wykonywana jako konwersja rozpakowywanie. W przeciwnym razie konwersja jest wykonywana jako jawna konwersja odwołania lub konwersja tożsamości.

Jeśli `T` jest znany jako typ referencyjny, powyższe konwersje są wszystkie klasyfikowane jako jawne konwersje odwołań ([jawne konwersje odwołań](conversions.md#explicit-reference-conversions)). Jeśli `T` nie jest znany jako typ referencyjny, konwersje powyżej są klasyfikowane jako konwersje rozpakowywania ([konwersje rozpakowywanie](conversions.md#unboxing-conversions)).

Powyższe zasady nie zezwalają na bezpośrednią jawną konwersję z nieograniczonego parametru typu do typu innego niż interfejs, który może być zaskakujące. Przyczyną tej reguły jest zapobieganie nieporozumieniu i wykonywanie semantyki takich konwersji. Na przykład przeanalizujmy następującą deklarację:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

Jeśli dozwolone jest bezpośrednie przekonwertowanie `t` na `int`, jeden z nich może łatwo oczekiwać, że `X<int>.F(7)` zwróci `7L`. Jednakże, ponieważ standardowe konwersje liczbowe są brane pod uwagę tylko wtedy, gdy typy są znane jako liczbowe w czasie trwania powiązania. Aby semantyka była oczywista, należy napisać powyższy przykład:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

Ten kod zostanie teraz skompilowany, ale wykonanie `X<int>.F(7)` następnie zgłosi wyjątek w czasie wykonywania, ponieważ `int` opakowane nie mogą być konwertowane bezpośrednio do `long`.

### <a name="user-defined-explicit-conversions"></a>Jawne konwersje zdefiniowane przez użytkownika

Jawna konwersja zdefiniowana przez użytkownika składa się z opcjonalnej standardowej konwersji jawnej, po której następuje wykonanie zdefiniowanego przez użytkownika operatora konwersji jawnej lub jawnej, a następnie innej opcjonalnej konwersji jawnej. Dokładne reguły oceniania jawnych konwersji zdefiniowanych przez użytkownika są opisane w temacie [Przetwarzanie konwersji jawnych zdefiniowanych przez użytkownika](conversions.md#processing-of-user-defined-explicit-conversions).

## <a name="standard-conversions"></a>Konwersje standardowe

Standardowe konwersje są wstępnie zdefiniowanymi konwersjemi, które mogą wystąpić w ramach konwersji zdefiniowanej przez użytkownika.

### <a name="standard-implicit-conversions"></a>Standardowe konwersje niejawne

Następujące niejawne konwersje są klasyfikowane jako standardowe konwersje niejawne:

*  Konwersje tożsamości ([konwersja tożsamości](conversions.md#identity-conversion))
*  Niejawne konwersje numeryczne ([niejawne konwersje liczbowe](conversions.md#implicit-numeric-conversions))
*  Niejawne konwersje null ([niejawne konwersje dopuszczające wartość null](conversions.md#implicit-nullable-conversions))
*  Niejawne konwersje odwołań ([niejawne konwersje odwołań](conversions.md#implicit-reference-conversions))
*  Konwersje z opakowaniem ([konwersje z opakowania](conversions.md#boxing-conversions))
*  Niejawne konwersje wyrażeń stałych ([niejawne konwersje dynamiczne](conversions.md#implicit-dynamic-conversions))
*  Niejawne konwersje dotyczące parametrów typu ([niejawne konwersje obejmujące parametry typu](conversions.md#implicit-conversions-involving-type-parameters))

Standardowe konwersje niejawne wykluczają specjalne konwersje niejawne zdefiniowane przez użytkownika.

### <a name="standard-explicit-conversions"></a>Standardowe Konwersje jawne

Standardowe Konwersje jawne to wszystkie standardowe konwersje niejawne oraz podzestaw jawnych konwersji, dla których istnieje odwrotna niejawna konwersja. Innymi słowy, jeśli istnieje standardowa niejawna konwersja z typu `A` do typu `B`, wówczas istnieje standardowa jawna konwersja z typu `A`, aby wpisać `B` i z typu `B` do typu `A`.

## <a name="user-defined-conversions"></a>Konwersje zdefiniowane przez użytkownika

C#zezwala na rozszerzanie wstępnie zdefiniowanych niejawnych i jawnych konwersji przez ***zdefiniowane przez użytkownika Konwersje***. Konwersje zdefiniowane przez użytkownika są wprowadzane przez deklarując operatory konwersji ([Operatory konwersji](classes.md#conversion-operators)) w typach klasy i struktury.

### <a name="permitted-user-defined-conversions"></a>Dozwolone konwersje zdefiniowane przez użytkownika

C#zezwala na zadeklarowanie tylko określonych konwersji zdefiniowanych przez użytkownika. W szczególności nie jest możliwe ponowne zdefiniowanie istniejącej konwersji niejawnej lub jawnej.

Dla danego typu źródła `S` i typu docelowego `T`, jeśli `S` lub `T` są typami dopuszczających wartości null, pozwól `S0` i `T0` odnoszą się do ich typów podstawowych, w przeciwnym razie `S0` i `T0` są równe `S` i `T` odpowiednio. Klasa lub struktura może deklarować konwersję z typu źródłowego `S` do typu docelowego `T` tylko wtedy, gdy spełnione są wszystkie następujące warunki:

*  `S0` i `T0` są różnymi typami.
*  `S0` lub `T0` jest klasą lub typem struktury, w której odbywa się deklaracja operatora.
*  Ani `S0` ani `T0` jest *interface_typeem*.
*  Oprócz konwersji zdefiniowanych przez użytkownika konwersja nie istnieje z `S` do `T` lub `T` do `S`.

Ograniczenia dotyczące konwersji zdefiniowanych przez użytkownika są szczegółowo omówione w [operatorach konwersji](classes.md#conversion-operators).

### <a name="lifted-conversion-operators"></a>Przenoszone operatory konwersji

Po zdefiniowaniu zdefiniowanego przez użytkownika operatora konwersji, który konwertuje typ wartości niedopuszczający wartości null `S` na typ wartości niedopuszczający wartości null `T`, istnieje ***zniesiony Operator konwersji*** , który konwertuje z `S?` na `T?`. Ten podnieśny Operator konwersji wykonuje odpakowanie od `S?` do `S`, a po nim konwersja zdefiniowana przez użytkownika z `S` na `T`, a następnie otokę z `T` do `T?`, z tą różnicą, że `S?` wartość null jest konwertowana bezpośrednio na `T?`ową wartość null.

Przenoszone operatory konwersji mają tę samą niejawną lub niejawną klasyfikację, jak jej podstawowy zdefiniowany przez użytkownika Operator konwersji. Termin "konwersja zdefiniowana przez użytkownika" dotyczy użycia operatorów konwersji zdefiniowanych przez użytkownika i podnoszenia.

### <a name="evaluation-of-user-defined-conversions"></a>Obliczanie konwersji zdefiniowanych przez użytkownika

Zdefiniowana przez użytkownika konwersja konwertuje wartość z typu, nazywaną ***typem źródła***, na inny typ o nazwie ***Typ docelowy***. Ocena zdefiniowanych przez użytkownika centrów konwersji na potrzeby znajdowania ***najbardziej określonego*** zdefiniowanego przez użytkownika operatora konwersji dla określonych typów źródłowych i docelowych. To oznaczenie jest podzielone na kilka kroków:

*  Znajdowanie zestawu klas i struktur, z których będą brane operatory konwersji zdefiniowane przez użytkownika. Ten zestaw składa się z typu źródła i jego klas bazowych oraz typu docelowego i jego klas bazowych (z niejawnymi założeniami, które tylko klasy i struktury mogą deklarować operatorów zdefiniowanych przez użytkownika, a typy nieklasowe nie mają klas bazowych). Na potrzeby tego kroku, jeśli typem źródłowym lub docelowym jest *nullable_type*, zamiast niego zostanie użyty ich typ podstawowy.
*  Z tego zestawu typów można określić, które operatory konwersji zdefiniowane przez użytkownika i przenoszone. Aby operator konwersji miał zastosowanie, musi być możliwe wykonanie konwersji standardowej ([Konwersje standardowe](conversions.md#standard-conversions)) z typu źródłowego na typ operandu operatora i musi być możliwe wykonanie konwersji standardowej z typu wynikowego operatora na typ docelowy.
*  Z zestawu odpowiednich operatorów zdefiniowanych przez użytkownika, określający, który operator jest jednoznacznie najbardziej specyficzny. Ogólnie rzecz biorąc, najbardziej konkretny operator jest operatorem, którego typ operandu jest "najbliższy" do typu źródłowego i którego typ wyniku to "najbliższy" do typu docelowego. Zdefiniowane przez użytkownika operatory konwersji są preferowane przez przenoszone operatory konwersji. Dokładne reguły dotyczące ustanawiania najbardziej określonego zdefiniowanego przez użytkownika operatora konwersji są zdefiniowane w poniższych sekcjach.

Po zidentyfikowaniu określonego zdefiniowanego przez użytkownika operatora konwersji rzeczywiste wykonanie konwersji zdefiniowanej przez użytkownika obejmuje trzy kroki:

*  Po pierwsze, w razie potrzeby, przeprowadzenie konwersji standardowej z typu źródłowego na typ operandu operatora konwersji zdefiniowanego przez użytkownika lub podnoszenia.
*  Następnie wywoływanie operatora konwersji zdefiniowanego przez użytkownika lub zniesionego w celu przeprowadzenia konwersji.
*  Na koniec, w razie potrzeby, przeprowadzenie standardowej konwersji z typu wynikowego operatora konwersji zdefiniowanego przez użytkownika lub przekształcenia na typ docelowy.

Obliczanie zdefiniowanej przez użytkownika konwersji nigdy nie obejmuje więcej niż jednego operatora konwersji zdefiniowanego przez użytkownika lub podniesionego. Inaczej mówiąc, konwersja z typu `S` do typu `T` nigdy nie wykona najpierw konwersji zdefiniowanej przez użytkownika z `S` na `X`, a następnie wykonaj konwersję zdefiniowaną przez użytkownika z `X` do `T`.

Dokładne definicje oceny niejawnych lub jawnych konwersji zdefiniowanych przez użytkownika są podano w poniższych sekcjach. Definicje korzystają z następujących warunków:

*  Jeśli standardowa niejawna konwersja ([niejawne konwersje](conversions.md#standard-implicit-conversions)) istnieje z typu `A` do typu `B`, a jeśli nie `A` ani `B` są *INTERFACE_TYPE*s, wówczas `A` jest określana ***przez*** `B`, a `B` jest określana ***jako `A`*** .
*  ***Najbardziej obejmujący typ*** w zestawie typów to jeden typ, który obejmuje wszystkie inne typy w zestawie. Jeśli żaden typ pojedynczy nie obejmuje wszystkich innych typów, zestaw nie ma żadnego typu obejmującego. W bardziej intuicyjnych warunkach, najbardziej obejmujący typ to "największy" typ w zestawie — jeden typ, do którego każdy inny typ może być niejawnie konwertowany.
*  Najbardziej pożądany ***Typ*** w zestawie typów to jeden typ, który jest obejmujący wszystkie inne typy w zestawie. Jeśli żaden typ pojedynczy nie zostanie określony przez wszystkie inne typy, zestaw nie ma żadnego typu z typem. W bardziej intuicyjnych warunkach, najbardziej odnoszące się do typu "najmniejszy" typ w zestawie — jeden typ, który może być niejawnie konwertowany na każdy z innych typów.

### <a name="processing-of-user-defined-implicit-conversions"></a>Przetwarzanie konwersji niejawnych zdefiniowanych przez użytkownika

Zdefiniowana przez użytkownika niejawna konwersja z typu `S` na typ `T` jest przetwarzana w następujący sposób:

*  Określ typy `S0` i `T0`. Jeśli `S` lub `T` mają Typy dopuszczające wartość null, `S0` i `T0` są ich typami podstawowymi, w przeciwnym razie `S0` i `T0` są równe odpowiednio `S` i `T`.
*  Znajdź zestaw typów, `D`, z których będą rozpatrywane operatory konwersji zdefiniowane przez użytkownika. Ten zestaw składa się z `S0` (jeśli `S0` jest klasą lub strukturą), klasy bazowe `S0` (jeśli `S0` jest klasą) i `T0` (jeśli `T0` jest klasą lub strukturą).
*  Znajdź zestaw odpowiednich operatorów konwersji zdefiniowanych przez użytkownika i podnoszenia, `U`. Ten zestaw składa się z zdefiniowanych przez użytkownika i poddanych niejawnych operatorów konwersji zadeklarowanych przez klasy lub struktury w `D`, które konwertują z typu obejmującego `S` do typu, który obejmuje `T`. Jeśli `U` jest pusty, konwersja jest niezdefiniowana i wystąpi błąd podczas kompilowania.
*  Znajdź najbardziej konkretny typ źródła, `SX`w `U`operatory:
    * Jeśli którykolwiek z operatorów w `U` skonwertować z `S`, `SX` `S`.
    * W przeciwnym razie `SX` jest najbardziej powiązanego typu w połączonym zestawie typów źródłowych operatorów w `U`. Jeśli nie można znaleźć dokładnie jednego z najbardziej pokrytych typów, konwersja jest niejednoznaczna i wystąpi błąd w czasie kompilacji.
*  Znajdź najbardziej konkretny typ docelowy, `TX`w `U`operatory:
    * Jeśli którykolwiek z operatorów w `U` skonwertować na `T`, `TX` `T`.
    * W przeciwnym razie `TX` to najbardziej obejmujący typ w połączonym zestawie typów docelowych operatorów w `U`. Jeśli nie można znaleźć dokładnie jednego typu, który nie zawiera, konwersja jest niejednoznaczna i wystąpi błąd kompilacji.
*  Znajdź najbardziej specyficzny Operator konwersji:
    * Jeśli `U` zawiera dokładnie jeden operator konwersji zdefiniowany przez użytkownika, który konwertuje z `SX` na `TX`, to jest to najbardziej specyficzny Operator konwersji.
    * W przeciwnym razie, jeśli `U` zawiera dokładnie jeden podnieśny Operator konwersji, który konwertuje z `SX` na `TX`, to jest to najbardziej konkretny Operator konwersji.
    * W przeciwnym razie konwersja jest niejednoznaczna i wystąpi błąd w czasie kompilacji.
*  Na koniec zastosuj konwersję:
    * Jeśli `S` nie jest `SX`, zostanie przeprowadzona standardowa niejawna konwersja z `S` na `SX`.
    * Najbardziej specyficzny Operator konwersji jest wywoływany do konwersji z `SX` na `TX`.
    * Jeśli `TX` nie jest `T`, zostanie przeprowadzona standardowa niejawna konwersja z `TX` na `T`.

### <a name="processing-of-user-defined-explicit-conversions"></a>Przetwarzanie konwersji jawnych zdefiniowanych przez użytkownika

Jawna konwersja zdefiniowana przez użytkownika z typu `S` na typ `T` jest przetwarzana w następujący sposób:

*  Określ typy `S0` i `T0`. Jeśli `S` lub `T` mają Typy dopuszczające wartość null, `S0` i `T0` są ich typami podstawowymi, w przeciwnym razie `S0` i `T0` są równe odpowiednio `S` i `T`.
*  Znajdź zestaw typów, `D`, z których będą rozpatrywane operatory konwersji zdefiniowane przez użytkownika. Ten zestaw składa się z `S0` (jeśli `S0` jest klasą lub strukturą), klasy bazowe `S0` (jeśli `S0` jest klasą), `T0` (jeśli `T0` jest klasą lub strukturą), a klasy bazowe `T0` (jeśli `T0` jest klasą).
*  Znajdź zestaw odpowiednich operatorów konwersji zdefiniowanych przez użytkownika i podnoszenia, `U`. Ten zestaw składa się z zdefiniowanych przez użytkownika i podnoszenia niejawnych lub jawnych operatorów konwersji zadeklarowanych przez klasy lub struktury w `D`, które konwertują z typu obejmującego lub ponoszące `S` do typu, który obejmuje lub podano przez `T`. Jeśli `U` jest pusty, konwersja jest niezdefiniowana i wystąpi błąd podczas kompilowania.
*  Znajdź najbardziej konkretny typ źródła, `SX`w `U`operatory:
    * Jeśli którykolwiek z operatorów w `U` skonwertować z `S`, `SX` `S`.
    * W przeciwnym razie, jeśli którykolwiek z operatorów w `U` konwertować z typów, które obejmują `S`, `SX` jest najbardziej połączoną typu w połączonym zestawie typów źródłowych tych operatorów. Jeśli nie można znaleźć typu z większością typów, konwersja jest niejednoznaczna i wystąpi błąd w czasie kompilacji.
    * W przeciwnym razie `SX` to najbardziej obejmujący typ w połączonym zestawie typów źródłowych operatorów w `U`. Jeśli nie można znaleźć dokładnie jednego typu, który nie zawiera, konwersja jest niejednoznaczna i wystąpi błąd kompilacji.
*  Znajdź najbardziej konkretny typ docelowy, `TX`w `U`operatory:
    * Jeśli którykolwiek z operatorów w `U` skonwertować na `T`, `TX` `T`.
    * W przeciwnym razie, jeśli którykolwiek z operatorów w `U` skonwertować na typy, które są objęte `T`, wówczas `TX` to najbardziej obejmujący typ w połączonym zestawie typów docelowych tych operatorów. Jeśli nie można znaleźć dokładnie jednego typu, który nie zawiera, konwersja jest niejednoznaczna i wystąpi błąd kompilacji.
    * W przeciwnym razie `TX` jest najbardziej powiązanego typu w połączonym zestawie typów docelowych operatorów w `U`. Jeśli nie można znaleźć typu z większością typów, konwersja jest niejednoznaczna i wystąpi błąd w czasie kompilacji.
*  Znajdź najbardziej specyficzny Operator konwersji:
    * Jeśli `U` zawiera dokładnie jeden operator konwersji zdefiniowany przez użytkownika, który konwertuje z `SX` na `TX`, to jest to najbardziej specyficzny Operator konwersji.
    * W przeciwnym razie, jeśli `U` zawiera dokładnie jeden podnieśny Operator konwersji, który konwertuje z `SX` na `TX`, to jest to najbardziej konkretny Operator konwersji.
    * W przeciwnym razie konwersja jest niejednoznaczna i wystąpi błąd w czasie kompilacji.
*  Na koniec zastosuj konwersję:
    * Jeśli `S` nie jest `SX`, zostanie przeprowadzona standardowa jawna konwersja z `S` do `SX`.
    * Najbardziej szczegółowy zdefiniowany przez użytkownika Operator konwersji jest wywoływany do konwersji z `SX` na `TX`.
    * Jeśli `TX` nie jest `T`, zostanie przeprowadzona standardowa jawna konwersja z `TX` do `T`.

## <a name="anonymous-function-conversions"></a>Konwersje funkcji anonimowych

*Anonymous_method_expression* lub *lambda_expression* są klasyfikowane jako funkcja anonimowa ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)). Wyrażenie nie ma typu, ale może być niejawnie konwertowane na zgodny typ delegata lub typ drzewa wyrażenia. W specjalnej postaci funkcja anonimowa `F` jest zgodna z typem delegata `D` udostępnionym:

*  Jeśli `F` zawiera *anonymous_function_signature*, `D` i `F` mają taką samą liczbę parametrów.
*  Jeśli `F` nie zawiera *anonymous_function_signature*, `D` mogą mieć zero lub więcej parametrów dowolnego typu, o ile żaden parametr elementu `D` ma modyfikator parametru `out`.
*  Jeśli `F` ma jawnie wpisaną listę parametrów, każdy parametr w `D` ma ten sam typ i Modyfikatory co odpowiedni parametr w `F`.
*  Jeśli `F` zawiera niejawnie wpisaną listę parametrów, `D` nie ma `ref` lub `out` parametrów.
*  Jeśli treść `F` jest wyrażeniem, a `D` ma `void` typem zwracanym lub `F` jest Async i `D` ma typ zwracany `Task`, wtedy gdy każdy parametr z `F` ma typ odpowiadającego parametru w `D`, treść `F` jest prawidłowym wyrażeniem ( [wyrażenia](expressions.md)WRT), które byłyby dozwolone jako *statement_expression* ([instrukcje wyrażeń](statements.md#expression-statements)).
*  Jeśli treść `F` jest blokiem instrukcji, a `D` ma `void` typem zwracanym lub `F` jest Async, a `D` ma zwracany typ `Task`, wtedy gdy każdy parametr z `F` ma typ odpowiadającego parametru w `D`, treść `F` jest prawidłowym blokiem instrukcji (WRT [Blocks](statements.md#blocks)), w którym żadna instrukcja `return` nie określa wyrażenia.
*  Jeśli treść `F` jest wyrażeniem, *a `F` jest* nieasynchroniczna i `D` ma typ zwracany inny niż void `T`*lub* `F` jest Async i `D` ma typ zwracany `Task<T>`, wtedy gdy każdy parametr z `F` ma typ odpowiadającego parametru w `D`, treść `F` jest prawidłowym wyrażeniem [(WRT](expressions.md)Expressions), które jest niejawnie konwertowane na `T`.
*  Jeśli treść `F` jest blokiem instrukcji, *a `F` nie jest Async i `D`* ma typ zwracany inny niż void `T`, *lub* `F` jest Async i `D` ma typ zwracany `Task<T>`, a następnie, gdy każdy parametr `F` ma typ odpowiadającego parametru w `D`, treść `F` jest prawidłowym blokiem instrukcji ( [bloki](statements.md#blocks)WRT) z nieosiągalnym punktem końcowym, w którym każda instrukcja `return` określa wyrażenie, które jest niejawnie konwertowane na `T`.

Na potrzeby zwięzłości Ta sekcja używa krótkiej formy typów zadań `Task` i `Task<T>` ([funkcje asynchroniczne](classes.md#async-functions)).

Wyrażenie lambda `F` jest zgodne z typem drzewa wyrażenia `Expression<D>`, jeśli `F` jest zgodne z typem delegata `D`. Należy zauważyć, że nie dotyczy to metod anonimowych, tylko wyrażeń lambda.

Niektóre wyrażenia lambda nie mogą być konwertowane na typy drzewa wyrażeń: nawet jeśli konwersja *istnieje*, kończy się niepowodzeniem w czasie kompilacji. Dzieje się tak, jeśli wyrażenie lambda:

*  Ma treść *bloku*
*  Zawiera proste lub złożone operatory przypisania
*  Zawiera dynamiczne wyrażenie powiązane
*  Jest asynchroniczny

Poniższe przykłady używają ogólnego typu delegata `Func<A,R>`, który reprezentuje funkcję, która przyjmuje argument typu `A` i zwraca wartość typu `R`:
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
parametry i zwracane typy każdej funkcji anonimowej są określane na podstawie typu zmiennej, do której jest przypisana funkcja anonimowa.

Pierwsze przypisanie pomyślnie konwertuje funkcję anonimową na typ delegata `Func<int,int>` ponieważ, gdy `x` jest określony typ `int`, `x+1` jest prawidłowym wyrażeniem, które jest niejawnie konwertowane na typ `int`.

Podobnie drugie przypisanie pomyślnie konwertuje funkcję anonimową na typ delegata `Func<int,double>`, ponieważ wynik `x+1` (typu `int`) jest niejawnie konwertowany na typ `double`.

Jednak trzecie przypisanie jest błędem czasu kompilacji, ponieważ w przypadku `x` danego typu `double`, wynik `x+1` (typu `double`) nie jest niejawnie konwertowany na typ `int`.

Czwarte przypisanie pomyślnie konwertuje anonimową funkcję asynchroniczną na typ delegata `Func<int, Task<int>>`, ponieważ wynik `x+1` (typu `int`) jest niejawnie konwertowany na typ wyniku `int` typu zadania `Task<int>`.

Funkcje anonimowe mogą mieć wpływ na rozwiązanie przeciążania i uczestniczyć w wnioskach o typie. Aby uzyskać więcej informacji, zobacz [elementy członkowskie funkcji](expressions.md#function-members) .

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>Obliczanie konwersji funkcji anonimowych na typy delegatów

Konwersja funkcji anonimowej na typ delegata tworzy wystąpienie delegata, które odwołuje się do funkcji anonimowej i (prawdopodobnie puste) zestaw przechwyconych zmiennych zewnętrznych, które są aktywne w czasie szacowania. Gdy obiekt delegowany jest wywoływany, jest wykonywana treść funkcji anonimowej. Kod w treści jest wykonywany przy użyciu zestawu przechwyconych zmiennych zewnętrznych, do których odwołuje się delegat.

Lista wywołań delegata utworzonego na podstawie funkcji anonimowej zawiera pojedynczy wpis. Nie określono dokładnego obiektu docelowego i docelowej metody delegata. W szczególności nie określono, czy obiekt docelowy delegata jest `null`, `this` wartość elementu członkowskiego otaczającej funkcji lub innego obiektu.

Konwersje semantycznie identycznych funkcji anonimowych z tym samym (prawdopodobnie pustym) zestawem przechwyconych wystąpień zmiennych zewnętrznych do tych samych typów delegatów są dozwolone (ale nie są wymagane) do zwrócenia tego samego wystąpienia delegata. Termin o wartości semantycznie jest używany w tym miejscu do oznaczania, że wykonanie funkcji anonimowych będzie we wszystkich przypadkach dawać te same skutki mające te same argumenty. Ta reguła umożliwia zoptymalizowanie kodu, takiego jak poniższe.

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

Ponieważ dwie Delegaty funkcji anonimowych mają ten sam (pusty) zbiór przechwyconych zmiennych zewnętrznych i ponieważ funkcje anonimowe są semantycznie identyczne, kompilator może odwoływać się do tej samej metody docelowej. W rzeczywistości kompilator może zwrócić to samo wystąpienie delegata z obu wyrażeń funkcji anonimowej.

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>Obliczanie konwersji funkcji anonimowych na typy drzewa wyrażeń

Konwersja funkcji anonimowej na typ drzewa wyrażenia tworzy drzewo wyrażenia ([Typy drzewa wyrażeń](types.md#expression-tree-types)). Dokładniejsze Obliczanie konwersji funkcji anonimowych prowadzi do konstrukcji struktury obiektu, która reprezentuje strukturę samej funkcji anonimowej. Precyzyjna struktura drzewa wyrażenia, a także dokładny proces tworzenia go, to implementacja zdefiniowana.

### <a name="implementation-example"></a>Przykład implementacji

W tej sekcji opisano możliwe implementację konwersji funkcji anonimowych w warunkach innych C# konstrukcji. Opisana tutaj implementacja opiera się na tych samych zasadach, które są używane przez C# kompilator firmy Microsoft, ale nie oznacza to, że jest to dozwolone wdrożenie, ani nie jest jedynym możliwym z nich. Zawiera krótkie informacje tylko o konwersjach na drzewa wyrażeń, ponieważ ich dokładna semantyka wykracza poza zakres tej specyfikacji.

W pozostałej części tej sekcji przedstawiono kilka przykładów kodu, który zawiera anonimowe funkcje o różnych właściwościach. Dla każdego przykładu podano odpowiednie tłumaczenie dla kodu, który używa C# tylko innych konstrukcji. W przykładach identyfikator `D` jest uznawany za reprezentujący następujący typ delegata:
```csharp
public delegate void D();
```

Najprostsza forma funkcji anonimowej to taka, która nie przechwytuje żadnych zmiennych zewnętrznych:
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

Można to przetłumaczyć na wystąpienie delegata, który odwołuje się do metody statycznej wygenerowanej przez kompilator, w której umieszczony jest kod funkcji anonimowej:
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

W poniższym przykładzie funkcja anonimowa odwołuje się do członków wystąpienia `this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

Można to przetłumaczyć na metodę wystąpienia wygenerowanego przez kompilator zawierający kod funkcji anonimowej:
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

Okres istnienia zmiennej lokalnej musi zostać przedłużony do co najmniej okresu istnienia delegata funkcji anonimowej. Można to osiągnąć przez "podnoszenia" zmiennej lokalnej do pola klasy wygenerowanej przez kompilator. Utworzenie wystąpienia zmiennej lokalnej (utworzenie wystąpienia[zmiennych lokalnych](expressions.md#instantiation-of-local-variables)) odpowiada za utworzenie wystąpienia klasy wygenerowanej przez kompilator i uzyskanie dostępu do zmiennej lokalnej odpowiada na dostęp do pola w wystąpieniu klasy wygenerowanej przez kompilator. Ponadto funkcja anonimowa jest metodą wystąpienia klasy wygenerowanej przez kompilator:
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

Na koniec następująca funkcja anonimowa przechwytuje `this` a także dwie zmienne lokalne o różnych okresach istnienia:
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

W tym przykładzie Klasa wygenerowana przez kompilator jest tworzona dla każdego bloku instrukcji, w którym są przechwytywane elementy lokalne, tak że lokalne w różnych blokach mogą mieć niezależne okresy istnienia. Wystąpienie `__Locals2`, Klasa wygenerowana przez kompilator dla wewnętrznego bloku instrukcji, zawiera zmienną lokalną `z` i pole odwołujące się do wystąpienia `__Locals1`.  Wystąpienie `__Locals1`, Klasa wygenerowana przez kompilator dla bloku instrukcji zewnętrznych, zawiera zmienną lokalną `y` i pole, które odwołuje się do `this` otaczającego elementu członkowskiego funkcji. Dzięki tym strukturom danych możliwe jest dotarcie do wszystkich przechwyconych zmiennych zewnętrznych za pomocą wystąpienia `__Local2`, a kod funkcji anonimowej może zostać zaimplementowany jako metoda wystąpienia tej klasy.

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

Ta sama technika zastosowana w tym miejscu do przechwytywania zmiennych lokalnych może być również używana podczas konwertowania funkcji anonimowych na drzewa wyrażeń: odwołania do obiektów generowanych przez kompilator mogą być przechowywane w drzewie wyrażenia, a dostęp do zmiennych lokalnych może być reprezentowane jako dostęp do pól dla tych obiektów. Zaletą tego podejścia jest umożliwienie współdzielenia "podnoszenia" zmiennych lokalnych między delegatami i drzewami wyrażeń.

## <a name="method-group-conversions"></a>Konwersje grup metod

Niejawna konwersja ([niejawne konwersje](conversions.md#implicit-conversions)) istnieje z grupy metod ([klasyfikacje wyrażeń](expressions.md#expression-classifications)) do zgodnego typu delegata. Uwzględniając typ delegata `D` i wyrażenie `E`, które jest sklasyfikowane jako grupa metod, niejawna konwersja istnieje z `E` do `D`, jeśli `E` zawiera co najmniej jedną metodę, która jest stosowana w jej normalnej formie ([odpowiedni element członkowski funkcji](expressions.md#applicable-function-member)) do listy argumentów skonstruowanej przy użyciu typów parametrów i modyfikatorów `D`, zgodnie z opisem w poniższej tabeli.

Aplikacja czasu kompilowania konwersji z grupy metod `E` do typu delegata `D` została opisana w poniższej. Należy zauważyć, że istnienie niejawnej konwersji z `E` na `D` nie gwarantuje, że konwersja aplikacji w czasie kompilacji zakończy się powodzeniem bez błędu.

*  Wybrano jedną metodę `M` odpowiadającą wywołaniu metody ([wywołania metody](expressions.md#method-invocations)) `E(A)`formularza, z następującymi modyfikacjami:
    * Lista argumentów `A` jest listą wyrażeń, z których każda została sklasyfikowana jako zmienna i z typem i modyfikatorem (`ref` lub `out`) odpowiedniego parametru w *formal_parameter_list* `D`.
    * Metody kandydata są brane pod uwagę tylko te metody, które są stosowane w ich normalnej formie ([element członkowski funkcji](expressions.md#applicable-function-member)), a nie tylko w ich rozwiniętej postaci.
*  Jeśli algorytm [wywołań metod](expressions.md#method-invocations) wywołuje błąd, wystąpi błąd czasu kompilacji. W przeciwnym razie algorytm tworzy jedną najlepszą metodę `M` ma taką samą liczbę parametrów jak `D`, a konwersja jest uznawana za istnieje.
*  Wybrana metoda `M` musi być zgodna ([zgodność z delegatem](delegates.md#delegate-compatibility)) z typem delegata `D`lub w przeciwnym razie wystąpi błąd w czasie kompilacji.
*  Jeśli wybrana metoda `M` jest metodą wystąpienia, wyrażenie wystąpienia skojarzone z `E` określa obiekt docelowy delegata.
*  Jeśli wybrana metoda M jest metodą rozszerzenia, która jest oznaczona za pomocą dostępu do elementu członkowskiego w wyrażeniu wystąpienia, to wyrażenie wystąpienia określa obiekt docelowy delegata.
*  Wynikiem konwersji jest wartość typu `D`, a mianowicie nowo utworzonego delegata, który odwołuje się do wybranej metody i obiektu docelowego.
*  Należy zauważyć, że ten proces może prowadzić do utworzenia delegata metody rozszerzenia, jeśli algorytm [wywołania metody](expressions.md#method-invocations) nie może znaleźć metody wystąpienia, ale kończy przetwarzanie wywołania `E(A)` jako wywołanie metody rozszerzenia ([wywołania metody rozszerzenia](expressions.md#extension-method-invocations)). Delegat utworzony w ten sposób przechwytuje również metodę rozszerzenia oraz jej pierwszy argument.

Poniższy przykład ilustruje konwersje grup metod:
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

Przypisanie do `d1` niejawnie konwertuje `F` grupy metod na wartość typu `D1`.

Przypisanie do `d2` pokazuje, jak można utworzyć delegata do metody, która ma mniej pochodne typy parametrów (kontrawariantne) i bardziej pochodny typ zwracany (współvariant).

Przypisanie do `d3` pokazuje, jak taka konwersja nie istnieje, jeśli metoda nie ma zastosowania.

Przypisanie do `d4` pokazuje, jak metoda musi być stosowana w jego normalnej postaci.

Przypisanie do `d5` pokazuje, jak parametry i zwracane typy delegata i metody mogą się różnić tylko dla typów referencyjnych.

Podobnie jak w przypadku wszystkich innych konwersji niejawnych i jawnych, operator rzutowania może służyć do jawnego wykonania konwersji grup metod. W tym przykładzie przykład
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
Zamiast tego może być zapisany
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

Grupy metod mogą mieć wpływ na rozwiązanie przeciążania i uczestniczyć w wnioskach o typie. Aby uzyskać więcej informacji, zobacz [elementy członkowskie funkcji](expressions.md#function-members) .

Obliczanie w czasie wykonywania konwersji grup metod odbywa się w następujący sposób:

*  Jeśli metoda wybrana w czasie kompilacji to metoda wystąpienia lub jest to metoda rozszerzająca, do której można uzyskać dostęp jako metoda wystąpienia, obiekt docelowy delegata jest określany na podstawie wyrażenia wystąpienia skojarzonego z `E`:
    * Wyrażenie wystąpienia jest oceniane. Jeśli ta Ocena spowoduje wyjątek, nie są wykonywane żadne dalsze kroki.
    * Jeśli wyrażenie wystąpienia ma *reference_type*, wartość obliczona przez wyrażenie wystąpienia zostaje obiektem docelowym. Jeśli wybrana metoda jest metodą wystąpienia, a obiekt docelowy jest `null`, zostanie zgłoszony `System.NullReferenceException` i nie są wykonywane żadne dalsze kroki.
    * Jeśli wyrażenie wystąpienia ma *value_type*, operacja pakowania ([konwersje z opakowania](types.md#boxing-conversions)) jest wykonywana w celu przekonwertowania wartości na obiekt, a obiekt ten zostaje obiektem docelowym.
*  W przeciwnym razie wybrana metoda jest częścią wywołania metody statycznej, a obiekt docelowy delegata jest `null`.
*  Zostanie przydzielono nowe wystąpienie typu delegata `D`. Jeśli nie ma wystarczającej ilości pamięci do przydzielenia nowego wystąpienia, zostanie zgłoszony `System.OutOfMemoryException` i nie są wykonywane żadne dalsze kroki.
*  Nowe wystąpienie delegata jest inicjowane z odwołaniem do metody, która została określona w czasie kompilacji i odwołaniem do obiektu docelowego obliczonego powyżej.
