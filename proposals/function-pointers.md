---
ms.openlocfilehash: 8bf3a18dc42e225e64bd3ccda2106aed89b421ed
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108392"
---
# <a name="function-pointers"></a>Wskaźniki funkcji

## <a name="summary"></a>Podsumowanie

Ta propozycja zawiera konstrukcje języka, które uwidaczniają opcode kodu IL, które nie są obecnie dostępne efektywnie lub w C# ogóle: `ldftn` i `calli`. Te kody kodów IL mogą być ważne w kodzie o wysokiej wydajności, a deweloperzy potrzebują wydajnych metod uzyskiwania dostępu do nich.

## <a name="motivation"></a>Motywacją

Motywacje i tło tej funkcji opisano w następujących kwestiach (jak jest potencjalną implementacją funkcji):

https://github.com/dotnet/csharplang/issues/191

Jest to alternatywna propozycja projektu dla elementów [wewnętrznych kompilatora](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)

## <a name="detailed-design"></a>Szczegółowy projekt

### <a name="function-pointers"></a>Wskaźniki funkcji

Język będzie zezwalać na deklarację wskaźników funkcji przy użyciu składni `delegate*`. Pełna składnia została szczegółowo opisana w następnej sekcji, ale jest to podobne do składni używanej przez `Func` i `Action` deklaracji typu.

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

Te typy są reprezentowane za pomocą typu wskaźnika funkcji, jak opisano w ECMA-335. Oznacza to, że wywołanie `delegate*` będzie używać `calli`, gdzie wywołanie `delegate` będzie używać `callvirt` na `Invoke` metodzie.
Syntaktycznie chociaż wywołanie jest identyczne dla obu konstrukcji.

Definicja ECMA-335 wskaźników metody obejmuje konwencję wywoływania jako część podpisu typu (sekcja 7,1).
Domyślna konwencja wywoływania zostanie `managed`. Alternatywne formularze można określić przez dodanie odpowiedniego modyfikatora po składni `delegate*`: `managed`, `cdecl`, `stdcall`, `thiscall`lub `unmanaged`. Przykład:

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

Konwersje między typami `delegate*` są wykonywane na podstawie ich podpisu, w tym konwencji wywoływania.

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

Typ `delegate*` jest typem wskaźnika, który oznacza, że ma wszystkie możliwości i ograniczenia standardowego typu wskaźnika:

- Prawidłowy tylko w kontekście `unsafe`.
- Metody, które zawierają parametr `delegate*` lub typu zwracanego można wywołać tylko z kontekstu `unsafe`.
- Nie można przekonwertować na `object`.
- Nie można użyć jako argumentu ogólnego.
- Może niejawnie skonwertować `delegate*` na `void*`.
- Można jawnie skonwertować `void*` na `delegate*`.

Dotyczących

- Nie można zastosować atrybutów niestandardowych do `delegate*` ani żadnego z jej elementów.
- Nie można oznaczyć `delegate*` parametru jako `params`
- Typ `delegate*` ma wszystkie ograniczenia normalnego typu wskaźnika.

### <a name="function-pointer-syntax"></a>Składnia wskaźnika funkcji

Składnia pełnego wskaźnika funkcji jest reprezentowana przez następującą gramatykę:

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

Konwencja wywoływania `unmanaged` reprezentuje domyślną konwencję wywoływania dla kodu natywnego na bieżącej platformie i jest zaszyfrowana jako WinAPI.
Wszystkie `calling_convention`s są kontekstowymi słowami kluczowymi, gdy są poprzedzone przez `delegate*`.

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a>Konwersje wskaźników funkcji

W niebezpiecznym kontekście zestaw dostępnych konwersji niejawnych (konwersje niejawne) jest rozszerzony w celu uwzględnienia następujących niejawnych konwersji wskaźnika:
- [_Istniejące konwersje_](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- Od _funcptr typu\__ `F0` do innego _funcptr\_typu_ `F1`, pod warunkiem że spełnione są wszystkie z następujących warunków:
    - `F0` i `F1` mają tę samą liczbę parametrów, a każdy `D0n` parametru w `F0` ma takie same `ref`, `out`, lub `in` modyfikatory, co odpowiadający mu parametr `D1n` w `F1`.
    - Dla każdego parametru wartości (parametru bez modyfikatora `ref`, `out`lub `in`), konwersja tożsamości, niejawna konwersja odwołania lub niejawna konwersja wskaźnika istnieje z typu parametru w `F0` do odpowiedniego typu parametru w `F1`.
    - Dla każdego `ref`, `out`lub parametru `in` typ parametru w `F0` jest taki sam jak odpowiedni typ parametru w `F1`.
    - Jeśli zwracanym typem jest przez wartość (No `ref` lub `ref readonly`), tożsamość, odwołanie niejawne lub niejawny wskaźnik nie istnieje z typu zwracanego `F1` do zwracanego typu `F0`.
    - Jeśli zwracanym typem jest przez odwołanie (`ref` lub `ref readonly`), typ zwracany i Modyfikatory `ref` `F1` są takie same jak typ zwracany i `ref` Modyfikatory `F0`.
    - Konwencja wywoływania `F0` jest taka sama jak Konwencja wywoływania `F1`.

### <a name="allow-address-of-to-target-methods"></a>Zezwalaj na adres dla metod docelowych

Grupy metod będą teraz dozwolone jako argumenty dla wyrażenia adresu. Typ takiego wyrażenia będzie `delegate*`, który ma odpowiednik sygnatury metody docelowej i zarządzanej konwencji wywoływania:

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

W niebezpiecznym kontekście Metoda `M` jest zgodna z typem wskaźnika funkcji `F`, jeśli spełnione są wszystkie następujące warunki:
- `M` i `F` mają tę samą liczbę parametrów, a każdy parametr w `D` ma takie same Modyfikatory `ref`, `out`lub `in` jako odpowiedni parametr w `F`.
- Dla każdego parametru wartości (parametru bez modyfikatora `ref`, `out`lub `in`), konwersja tożsamości, niejawna konwersja odwołania lub niejawna konwersja wskaźnika istnieje z typu parametru w `M` do odpowiedniego typu parametru w `F`.
- Dla każdego `ref`, `out`lub parametru `in` typ parametru w `M` jest taki sam jak odpowiedni typ parametru w `F`.
- Jeśli zwracanym typem jest przez wartość (No `ref` lub `ref readonly`), tożsamość, odwołanie niejawne lub niejawny wskaźnik nie istnieje z typu zwracanego `F` do zwracanego typu `M`.
- Jeśli zwracanym typem jest przez odwołanie (`ref` lub `ref readonly`), typ zwracany i Modyfikatory `ref` `F` są takie same jak typ zwracany i `ref` Modyfikatory `M`.
- Konwencja wywoływania `M` jest taka sama jak Konwencja wywoływania `F`.
- `M` jest metodą statyczną.

W niebezpiecznym kontekście istnieje niejawna konwersja z wyrażenia address-of, którego obiektem docelowym jest grupa metod `E` do zgodnego typu wskaźnika funkcji `F` Jeśli `E` zawiera co najmniej jedną metodę, która jest stosowana w jego normalnej postaci do listy argumentów skonstruowanych przy użyciu typów parametrów i modyfikatorów `F`, zgodnie z opisem w poniższej tabeli.
- Wybrano jedną metodę `M` odpowiadającą wywołaniu metody formularza `E(A)` z następującymi modyfikacjami:
   - Lista argumentów `A` jest listą wyrażeń, z których każda została sklasyfikowana jako zmienna i z typem i modyfikatorem (`ref`, `out`lub `in`) odpowiedniego _formalnego\_parametru\_listę_ `D`.
   - Metody kandydata to tylko te metody, które mają zastosowanie w ich normalnej postaci, nie są stosowane w ich rozwiniętej postaci.
   - Metody kandydata to tylko te metody, które są statyczne.
- Jeśli algorytm wywołań metod wywołuje błąd, wystąpi błąd czasu kompilacji. W przeciwnym razie algorytm tworzy jedną najlepszą metodę `M` ma taką samą liczbę parametrów jak `F`, a konwersja jest uznawana za istnieje.
- Wybrana metoda `M` musi być zgodna (zgodnie z definicją powyżej) z typem wskaźnika funkcji `F`. W przeciwnym razie wystąpi błąd w czasie kompilacji.
- Wynikiem konwersji jest wskaźnik funkcji typu `F`.

Niejawna konwersja istnieje z wyrażenia address-of, którego Target jest grupą metod `E` do `void*`, jeśli istnieje tylko jedna metoda statyczna `M` w `E`.
Jeśli istnieje jedna metoda statyczna, należy `M`jedną najlepszą metodę z `E`.
W przeciwnym razie wystąpi błąd w czasie kompilacji.

Oznacza to, że deweloperzy mogą zależeć od reguł rozpoznawania przeciążenia, aby współdziałać z operatorem address-of:

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

Operator address-of zostanie zaimplementowany przy użyciu instrukcji `ldftn`.

Ograniczenia tej funkcji:

- Dotyczy tylko metod oznaczonych jako `static`.
- W `&`nie można używać funkcji lokalnych innych niż`static`. Szczegóły implementacji tych metod celowo nie są określone przez język. Dotyczy to również tego, czy są to elementy statyczne a wystąpienia, czy dokładnie do których podpisu są emitowane.

### <a name="better-function-member"></a>Lepsza składowa funkcji

Lepsza Specyfikacja elementu członkowskiego funkcji zostanie zmieniona w taki sposób, aby obejmowała następujący wiersz:

> `delegate*` jest bardziej szczegółowy niż `void*`

Oznacza to, że można przeciążać `void*` i `delegate*` i nadal korzystać z operatora address-of.

## <a name="open-issues"></a>Otwarte problemy

### <a name="nativecallableattribute"></a>NativeCallableAttribute

Jest to atrybut używany przez środowisko CLR, aby uniknąć zarządzania prologuą natywną podczas wywoływania. Metody oznaczone przez ten atrybut są wywoływane tylko z kodu natywnego, niezarządzanego (nie można wywoływać metod, utworzyć delegata itp.). Atrybut nie jest specjalny dla mscorlib; środowisko uruchomieniowe będzie traktować dowolny atrybut o tej samej nazwie z tą samą semantyką.

Istnieje możliwość, że środowisko uruchomieniowe i język współdziałają ze sobą w celu zapewnienia pełnej obsługi. Język może zdecydować się na traktowanie `static` członków z atrybutem `NativeCallable` jako `delegate*` z określoną konwencją wywoływania.

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

Ponadto język może również chcieć:

- Oflaguj wszystkie wywołania zarządzane do metody oznaczonej `NativeCallable` jako błąd. Nie można wywołać funkcji z kodu zarządzanego kompilator powinien uniemożliwić deweloperom podejmowanie próby takiego wywołania.
- Zapobiegaj konwersji grup metod na `delegate`, gdy metoda jest otagowana przy użyciu `NativeCallable`.

Nie jest to konieczne do obsługi `NativeCallable`. Kompilator może obsługiwać atrybut `NativeCallable` jako używa istniejącej składni. Program powinien po prostu wykonać rzutowanie na `void*` przed rzutowanie na poprawną sygnaturę `delegate*`. To nie będzie gorsze niż obecnie pomoc techniczna.

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a>Rozszerzalny zestaw niezarządzanych konwencji wywoływania

Zestaw niezarządzanych konwencji wywoływania obsługiwanych przez bieżące kodowania ECMA-335 jest nieaktualny. Zaobserwowano prośby o dodanie obsługi dla bardziej niezarządzanych konwencji wywoływania, na przykład:

- [vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120
- StdCall z jawnym tym https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750

Projekt tej funkcji powinien zezwalać na rozszerzanie zestawu niezarządzanych konwencji wywoływania zgodnie z wymaganiami w przyszłości. Problemy obejmują ograniczoną ilość miejsca na potrzeby kodowania konwencji wywoływania (12 z 16 wartości są pobierane w `IMAGE_CEE_CS_CALLCONV_MASK`) i liczbę miejsc, które muszą być dotknięcia, aby dodać nową konwencję wywoływania. Potencjalnym rozwiązaniem jest wprowadzenie nowego kodowania, które reprezentuje konwencję wywoływania przy użyciu [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) wyliczeniem.

W przypadku elementu Reference https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h ma listę konwencji wywoływania obsługiwanych przez LLVM. Chociaż jest mało prawdopodobne, że platforma .NET będzie musiała obsługiwać wszystkie te elementy, pokazuje, że przestrzeń konwencji wywoływania jest bardzo głęboka.

## <a name="considerations"></a>Zagadnienia do rozważenia

### <a name="allow-instance-methods"></a>Zezwalaj na metody wystąpień

Propozycja może zostać rozszerzona w celu obsługi metod wystąpień przez skorzystanie z konwencji wywoływania interfejsu wiersza polecenia `EXPLICITTHIS` (o C# nazwie `instance` w kodzie). Ta forma wskaźników funkcji interfejsu wiersza polecenia umieszcza parametr `this` jako jawny pierwszy parametr składni wskaźnika funkcji.

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

Jest to dźwięk, ale dodaje pewną skomplikowanie do propozycji. Szczególnie ze względu na to, że wskaźniki funkcji, które różnią się konwencją wywoływania `instance` i `managed` byłyby niezgodne, mimo że oba przypadki C# są używane do wywoływania metod zarządzanych z tą samą sygnaturą. Również w każdym przypadku należy wziąć pod uwagę, że jest to wartość cenna, aby istniała prosta obejście: Użyj funkcji lokalnej `static`.

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a>Nie wymagaj niebezpiecznego w deklaracji

Zamiast żądać `unsafe` przy każdym użyciu `delegate*`, wystarczy tylko wtedy, gdy grupa metod jest konwertowana na `delegate*`. Jest to miejsce, w którym podstawowe problemy z bezpieczeństwem są odtwarzane (wiedząc, że nie można zwolnić zawierającego go zestawu, gdy wartość jest aktywna). Wymaganie `unsafe` w innych lokalizacjach może być postrzegane jako nadmierne.

Jest to sposób, w jaki projekt był pierwotnie przewidziany. Jednak reguły języka powstają bardzo niewygodna. Nie można ukryć faktu, że jest to wartość wskaźnika i jest ono utrzymywane przez nawet bez słowa kluczowego `unsafe`. Na przykład konwersja do `object` nie może być dozwolona, nie może być elementem członkowskim `class`itd... C# Projekt jest wymagany do `unsafe` dla wszystkich używanych wskaźników i dlatego ten projekt następuje poniżej.

Deweloperzy nadal będą mogli przedstawiać _bezpieczną_ otokę na podstawie `delegate*` wartości w taki sam sposób, jak w przypadku normalnych typów wskaźnika. Należy wziąć pod uwagę:

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a>Korzystanie z delegatów

Zamiast używać nowego elementu składni `delegate*`, po prostu Użyj istniejących typów `delegate` z `*` następującym po typie:

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

Obsługa konwencji wywoływania może odbywać się poprzez dodawanie adnotacji do typów `delegate` z atrybutem określającym `CallingConvention` wartość. Brak atrybutu oznacza zarządzaną konwencję wywoływania.

Kodowanie tego w IL jest problematyczne. Wartość bazowa musi być reprezentowana jako wskaźnik jeszcze:

1. Mieć unikatowy typ, aby zezwalać na przeciążenia z różnymi typami wskaźników funkcji.
1. Być równoważne do celów OHI między granicami zestawu.

Ostatnim punktem jest szczególnie problematyczne. Oznacza to, że każdy zestaw, który używa `Func<int>*` musi zakodować odpowiedni typ w metadanych, mimo że `Func<int>*` jest zdefiniowany w zestawie, chociaż nie kontroluje.
Dodatkowo każdy inny typ, który jest zdefiniowany przy użyciu nazwy `System.Func<T>` w zestawie, który nie jest typem mscorlib, musi być inny niż wersja zdefiniowana w bibliotece Mscorlib.

Jedna z opcji, która została zbadana, obemituje taki wskaźnik jako `mod_req(Func<int>) void*`. To nie działa, ponieważ `mod_req` nie można powiązać z `TypeSpec` i dlatego nie może kierować ogólnych wystąpień.

### <a name="named-function-pointers"></a>Wskaźniki nazwanych funkcji

Składnia wskaźnika funkcji może być nieskomplikowana, szczególnie w złożonych przypadkach, takich jak zagnieżdżone wskaźniki funkcji. Zamiast przypisywania przez deweloperów podpisu za każdym razem, gdy język może zezwalać na nazwane deklaracje wskaźników funkcji w ramach `delegate`.

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

Częścią problemu jest podstawowy interfejs wiersza polecenia, którego nazwy nie są w związku z tym, że jest C# to całkowicie wynalazk i wymaga to, aby można było włączyć obsługę metadanych. Jest to doable, ale jest istotny dla pracy. Zasadniczo wymaga C# posiadania pomocnika do tabeli typu def dla tych nazw.

Ponadto po zbadaniu argumentów dla nazwanych wskaźników funkcji można było zastosować równie dobrze do wielu innych scenariuszy. Na przykład byłoby to wygodne do deklarowania nazwanych krotek, aby zmniejszyć konieczność wpisania pełnego podpisu we wszystkich przypadkach.

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

Po omówieniu zdecydowała się nie zezwalać na nazwaną deklarację typów `delegate*`. Jeśli okaże się, że jest to istotne, na podstawie informacji zwrotnych dotyczących użycia klienta sprawdzimy rozwiązanie nazewnictwa, które działa w przypadku wskaźników funkcji, krotek, typów ogólnych itp... Jest to prawdopodobnie podobne w formularzu do innych sugestii, takich jak Pełna obsługa `typedef` w języku.

## <a name="future-considerations"></a>Zagadnienia w przyszłości

### <a name="static-local-functions"></a>statyczne funkcje lokalne

Odnosi się do [propozycji](https://github.com/dotnet/csharplang/issues/1565) , aby zezwolić na modyfikator `static` w funkcjach lokalnych. Takie funkcje byłyby gwarantowane jako `static` i z dokładnym podpisem określonym w kodzie źródłowym. Taka funkcja powinna być prawidłowym argumentem, aby `&`, ponieważ nie zawiera ona żadnych problemów z funkcją lokalną dzisiaj

### <a name="static-delegates"></a>elementy delegowane statyczne

Odnosi się to do [propozycji](https://github.com/dotnet/csharplang/issues/302) zezwalania dla deklaracji typów `delegate`, które mogą odwoływać się tylko do `static` członków. Zalety takich `delegate` wystąpień może być bezpłatna i lepsza w scenariuszach z uwzględnieniem wydajności.

Jeśli funkcja wskaźnika funkcji jest zaimplementowana, `static delegate` propozycja prawdopodobnie zostanie zamknięta. Proponowana zaletą tej funkcji jest bezpłatna alokacja. Stwierdzono niedawne badania, które nie są możliwe do osiągnięcia ze względu na zwolnienie zestawu. Musi istnieć silne dojście z `static delegate` do metody, do której się odwołuje, aby zapobiec wyładowaniu zestawu z tego elementu.

Aby zapewnić, że każde wystąpienie `static delegate` będzie wymagane do przydzielenia nowego uchwytu, który uruchamia licznik w celach oferty. Istnieją pewne projekty, w których alokacja może być amortyzowana do pojedynczej alokacji dla każdej lokacji wywołania, ale jest to nieskomplikowana złożona i niepozornie zawarta.

Oznacza to, że deweloperzy muszą przede wszystkim podejmować decyzje o następujących możliwościach:

1. Bezpieczeństwo przed rozładunkiem zestawu: wymaga to alokacji, dlatego `delegate` jest już wystarczającą opcją.
1. Brak zabezpieczeń przed rozładunkiem zestawu: Użyj `delegate*`. Może to być opakowany w `struct`, aby zezwolić na użycie poza kontekstem `unsafe` w pozostałej części kodu.
