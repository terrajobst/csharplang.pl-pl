---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2019
ms.locfileid: "79485113"
---
# <a name="readonly-references"></a>Odwołania tylko do odczytu

* [x] proponowane
* [x] prototyp
* Implementacja [x]: uruchomiono
* [] Specyfikacja: nie uruchomiono

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Funkcja "odwołania tylko do odczytu" jest w rzeczywistości grupą funkcji, które wykorzystują wydajność przekazywania zmiennych przez odwołanie, ale bez uwidaczniania danych do modyfikacji:  
- parametry `in`
- `ref readonly` zwraca
- struktury `readonly`
- `ref`/`in` metody rozszerzenia
- `ref readonly` lokalne
- `ref` wyrażenia warunkowe

## <a name="passing-arguments-as-readonly-references"></a>Przekazywanie argumentów jako odwołań tylko do odczytu.

Istnieje już oferta, która dotyka tego tematu https://github.com/dotnet/roslyn/issues/115 jako specjalnego przypadku parametrów tylko do odczytu bez przechodzenia do wielu szczegółów.
W tym miejscu chcę potwierdzić, że pomysł nie jest bardzo nowy.

### <a name="motivation"></a>Motywacją

Przed tą funkcją C# nie było efektywnego wyrażania chęci przekazania zmiennych struktury do wywołań metod do odczytu, bez zamiaru modyfikacji. Okresowe przekazanie argumentu przez wartość oznacza kopiowanie, które dodaje niepotrzebne koszty.  Dzięki temu użytkownicy mogą używać argumentów przez-ref i polegać na komentarzach/dokumentacji, aby wskazać, że dane nie powinny być zmienione przez wywoływany. Nie jest to dobre rozwiązanie z wielu powodów.  
Przykłady to wielowektorowe operatory matematyczne w bibliotekach graficznych, takich jak [XNA](https://msdn.microsoft.com/library/bb194944.aspx) , które są znane jako operandy ref ze względu na wydajność. Istnieje kod w kompilatorze Roslyn, który używa struktur, aby uniknąć przydziałów, a następnie przekazuje je przez odwołanie, aby uniknąć kopiowania kosztów.

### <a name="solution-in-parameters"></a>Rozwiązanie (`in` parametry)

Podobnie jak parametry `out`, parametry `in` są przenoszone jako odwołania zarządzane z dodatkowymi gwarancjami z wywoływanego elementu.  
W przeciwieństwie do parametrów `out`, które _muszą_ być przypisane przez wywoływany przed innym użyciem, `in` parametry nie mogą być przypisywane przez obiekt wywoływany.

W wyniku `in` parametry umożliwiają skuteczne przekazywanie argumentów pośrednich bez ujawniania argumentów do mutacji przez wywoływany.

### <a name="declaring-in-parameters"></a>Deklarowanie parametrów `in`

parametry `in` są zadeklarowane za pomocą słowa kluczowego `in` jako modyfikatora w sygnaturze parametru.

We wszystkich celach `in` parametr jest traktowany jako zmienna `readonly`a. Większość ograniczeń dotyczących używania parametrów `in` wewnątrz metody jest taka sama jak w przypadku pól `readonly`.

> W rzeczywistości parametr `in` może reprezentować pole `readonly`. Podobieństwo ograniczeń nie jest współzakresem.

Na przykład pola `in` parametru, który ma typ struktury, są rekursywnie klasyfikowane jako zmienne `readonly`.

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- parametry `in` są dozwolone wszędzie tam, gdzie są dozwolone zwykłe parametry ByVal. Dotyczy to indeksatorów, operatorów (w tym konwersji), delegatów, wyrażeń lambda, funkcji lokalnych.

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- `in` nie jest dozwolona w połączeniu z `out` lub ze wszystkimi, które `out` nie łączą się z.

- Nie jest dozwolone Przeciążenie na `ref`/`out`/`in` różnice.

- Możliwe jest Przeciążenie zwykłych i `in`ych różnic.

- Na potrzeby OHI (Przeciążenie, ukrywanie, implementowanie) `in` zachowuje się podobnie do `out` parametr.
Wszystkie te same reguły mają zastosowanie.
Na przykład Metoda zastępująca będzie musiała dopasować `in` parametry z parametrami `in` o typie konwersji tożsamości.

- Na potrzeby konwersji obiektów delegowanych/lambda/grup metod `in` zachowuje się podobnie do `out` parametru.
Wyrażenia lambda i odpowiednie sugestie konwersji grup metod będą musiały pasować do `in` parametrów delegata docelowego z parametrami `in` o typie konwersji tożsamości.

- Na potrzeby wariancji ogólnej `in` parametry nie są wariantem.

> Uwaga: nie ma ostrzeżeń dotyczących parametrów `in`, które mają typ referencyjny lub podstawowy.
Może to być bezpunktowe, ale w niektórych przypadkach użytkownik musi/chcieć przekazać elementy pierwotne jako `in`. Przykłady — przesłanianie metody ogólnej, takiej jak `Method(in T param)`, gdy `T` został zastąpiony jako `int`lub gdy mają takie metody jak `Volatile.Read(in int location)`
>
> Możliwe jest posiadanie analizatora, który ostrzega w przypadku niewydajnego użycia parametrów `in`, ale zasady takiej analizy byłyby zbyt rozmyte, aby były częścią specyfikacji języka.

### <a name="use-of-in-at-call-sites-in-arguments"></a>Używanie `in` w lokacjach wywołań. (`in` argumenty)

Istnieją dwa sposoby przekazywania argumentów do parametrów `in`.

#### <a name="in-arguments-can-match-in-parameters"></a>argumenty `in` mogą być zgodne z parametrami `in`:

Argument z modyfikatorem `in` w miejscu wywołania może pasować do `in` parametrów.

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- argument `in` musi być LValue do _odczytu_ (*).
Przykład: `M1(in 42)` jest nieprawidłowy

> (*) Pojęcie [LValue/rvalue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) różni się między językami.  
Tutaj, LValue I oznaczają wyrażenie reprezentujące lokalizację, do której można odwoływać się bezpośrednio.
I RValue oznacza wyrażenie, które zwraca wynik tymczasowy, który nie jest na siebie własny.  

- W szczególności prawidłowe jest przekazywanie `readonly` pól, `in` parametrów lub innych `readonly`ych zmiennych jako argumentów `in`.
Przykład: `dictionary[in Guid.Empty]` jest dozwolony. `Guid.Empty` to statyczne pole tylko do odczytu.

- argument `in` musi mieć typ _identyfikujący tożsamość_ do typu parametru.
Przykład: `M1<object>(in Guid.Empty)` jest nieprawidłowy. `Guid.Empty` nie jest możliwa do _konwersji tożsamości_ na `object`

Motywacja dla powyższych reguł polega na tym, że argumenty `in`ą gwarantuje _alias_ zmiennej argumentu. Wywoływany zawsze otrzymuje bezpośrednie odwołanie do tej samej lokalizacji, co reprezentowane przez argument.

- w rzadkich sytuacjach, gdy argumenty `in` muszą być rozłożone na stosie z powodu `await` wyrażeń używanych jako operandy tego samego wywołania, zachowanie jest takie samo jak w przypadku argumentów `out` i `ref` — Jeśli zmienna nie może zostać rozlana w sposób referencyjny, zgłaszany jest błąd.

Przykłady:
1. `M1(in staticField, await SomethingAsync())` jest prawidłowy.
`staticField` to pole statyczne, do którego można uzyskać dostęp więcej niż jeden raz bez zauważalnych efektów ubocznych. W związku z tym można podać kolejność efektów ubocznych i wymagań dotyczących aliasów.
2. `M1(in RefReturningMethod(), await SomethingAsync())` spowoduje wystąpienie błędu.
`RefReturningMethod()` jest `ref` zwracająca metodę. Wywołanie metody mogło mieć zauważalne efekty uboczne, dlatego należy je ocenić przed operandem `SomethingAsync()`. Jednak wynik wywołania jest odwołaniem, które nie może być zachowane przez punkt zawieszenia `await`, co sprawia, że bezpośrednie wymaganie referencyjne nie jest możliwe.

> Uwaga: Błędy rozlewania stosu są uznawane za ograniczenia specyficzne dla implementacji. W związku z tym nie mają one wpływu na rozpoznanie przeciążenia lub wnioskowanie lambda.

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a>Zwykłe argumenty ByVal mogą pasować do `in` parametrów:

Regularne argumenty bez modyfikatorów mogą odpowiadać `in` parametrom. W takim przypadku argumenty mają takie same ograniczenia swobodne jak zwykłe argumenty ByVal.

W tym scenariuszu jest to, że `in` parametry w interfejsach API mogą spowodować niewygodę dla użytkownika, gdy argumenty nie mogą być przekazane jako odwołanie bezpośrednie-ex: literały, obliczone lub `await`e wyniki lub argumenty, które wystąpiły w celu uzyskania bardziej szczegółowych typów.  
Wszystkie te przypadki mają proste rozwiązanie do przechowywania wartości argumentu w tymczasowym lokalnym odpowiednim typie i przekazanie go jako argumentu `in`.  
Aby ograniczyć potrzebę tego, że kompilator kodu standardowego może wykonać to samo przekształcenie, w razie potrzeby, gdy `in` modyfikator nie jest obecny w witrynie wywołania.  

Ponadto, w niektórych przypadkach, takich jak wywołanie operatorów lub `in` metody rozszerzenia, nie ma znaczenia, aby określić `in` w ogóle. To samo wymaga określenia zachowania zwykłych argumentów ByVal, gdy pasują do `in` parametrów.

W szczególności:

- prawidłowe jest przekazanie RValues.
W takim przypadku jest przesyłane odwołanie do tymczasowej.
Przykład:
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- niejawne konwersje są dozwolone.

> Jest to w rzeczywistości szczególnym przypadkiem przekazania RValue  

W takim przypadku jest przesyłane odwołanie do tymczasowej przekonwertowanej wartości.
Przykład:
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- w przypadku odbiorcy metody rozszerzenia `in` (w przeciwieństwie do `ref` metod rozszerzenia) RValues lub niejawne _konwersje this-arguments_ są dozwolone.
W takim przypadku jest przesyłane odwołanie do tymczasowej przekonwertowanej wartości.
Przykład:
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
Więcej informacji na temat `ref`/`in` metod rozszerzenia znajduje się w dalszej części tego dokumentu.

- rozłożenie argumentu z powodu `await` operandy mogą rozlać "według wartości", jeśli jest to konieczne.
W scenariuszach, w których bezpośrednie odwołanie do argumentu nie jest możliwe z powodu interwencji, `await` zamiast niej zostanie rozlana kopia wartości argumentu.  
Przykład:
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
Ponieważ wynik wywołania bocznego jest odwołaniem, które nie może zostać zachowane między `await` zawieszeniem, zamiast tego zostanie zachowana tymczasowa zawierająca wartość rzeczywistą (podobnie jak w przypadku zwykłego parametru ByVal).

#### <a name="omitted-optional-arguments"></a>Pominięto opcjonalne argumenty

Aby określić wartość domyślną, jest dozwolony parametr `in`. Powoduje to, że odpowiedni argument jest opcjonalny.

Pominięcie opcjonalnego argumentu w witrynie wywołania skutkuje przekazaniem wartości domyślnej za pośrednictwem tymczasowej.

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a>Ogólne zachowanie aliasu

Podobnie jak zmienne `ref` i `out`, zmienne `in` są odwołaniami/aliasami do istniejących lokalizacji.

Gdy wywoływany element nie może zapisywać w nich, odczyt parametru `in` może obserwować różne wartości jako efekt uboczny innych ocen.

Przykład:

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a>`in` parametry i przechwytywanie zmiennych lokalnych.  
Na potrzeby przechwytywania lambda/async `in` parametry zachowują się tak samo jak parametry `out` i `ref`.

- nie można przechwycić parametrów `in` w zamknięciu
- parametry `in` są niedozwolone w metodach iteratora
- parametry `in` są niedozwolone w metodach asynchronicznych

### <a name="temporary-variables"></a>Zmienne tymczasowe.  
Niektóre zastosowania przekazywania parametrów `in` mogą wymagać pośredniego użycia tymczasowej zmiennej lokalnej:  
- argumenty `in` są zawsze przesyłane jako aliasy bezpośrednie, gdy lokacja wywołań używa `in`. W takim przypadku czas tymczasowy nie jest używany.
- argumenty `in` nie muszą być bezpośrednimi aliasami, gdy lokacja wywołań nie korzysta z `in`. Gdy argument nie jest LValue, może być używany tymczasowy.
- parametr `in` może mieć wartość domyślną. Gdy odpowiedni argument zostanie pominięty w miejscu wywołania, wartość domyślna jest przenoszona za pośrednictwem tymczasowej.
- argumenty `in` mogą mieć niejawne konwersje, w tym te, które nie zachowują tożsamości. W tych przypadkach jest używany tymczasowy.
- odbiorniki zwykłych wywołań struktury nie mogą być zapisywalne LValues (**istniejąca sprawa!** ). W tych przypadkach jest używany tymczasowy.

Czas życia argumentu obiekty tymczasowe jest zgodny z najbliższym zakresem obejmującym lokację wywołania.

Formalny czas życia zmiennych tymczasowych jest semantycznie znaczący w scenariuszach obejmujących analizę ucieczki zmiennych zwracanych przez odwołanie.

### <a name="metadata-representation-of-in-parameters"></a>Reprezentacja metadanych parametrów `in`.
Gdy `System.Runtime.CompilerServices.IsReadOnlyAttribute` jest stosowany do parametru ByRef, oznacza to, że parametr jest parametrem `in`.

Ponadto, jeśli metoda jest *abstrakcyjna* lub *wirtualna*, sygnatura takich parametrów (i tylko tych parametrów) musi mieć `modreq[System.Runtime.InteropServices.InAttribute]`.

**Motywacja**: jest to wykonywane w celu zapewnienia, że w przypadku metody przesłaniania parametrów `in` są zgodne.

Te same wymagania dotyczą metod `Invoke` w delegatach.

**Motywacja**: to gwarantuje, że istniejący kompilator nie będzie mógł po prostu zignorować `readonly` podczas tworzenia lub przypisywania delegatów.

## <a name="returning-by-readonly-reference"></a>Zwracanie przez odwołanie tylko do odczytu.

### <a name="motivation"></a>Motywacją
Motywacja tej funkcji podrzędnej jest w przybliżeniu symetryczna do przyczyn parametrów `in`-unikając kopiowania, ale po stronie zwracającej. Przed tą funkcją, metoda lub indeksator miał dwie opcje: 1) zwraca przez odwołanie i jest narażony na możliwe mutacje lub 2) zwraca przez wartość, która skutkuje kopiowaniem.

### <a name="solution-ref-readonly-returns"></a>Rozwiązanie (`ref readonly` zwraca)  
Funkcja umożliwia elementowi członkowskiemu zwracanie zmiennych przez odwołanie bez uwidaczniania ich do mutacji.

### <a name="declaring-ref-readonly-returning-members"></a>Deklarowanie `ref readonly` zwracających elementy członkowskie

Kombinacja modyfikatorów `ref readonly` w sygnaturze zwrotnym służy do wskazywania, że element członkowski zwraca odwołanie tylko do odczytu.

Dla wszystkich celów `ref readonly` skład jest traktowany jako zmienna `readonly` — podobna do `readonly` pól i `in` parametrów.

Na przykład pola elementu członkowskiego `ref readonly`, który ma typ struktury, są rekursywnie klasyfikowane jako zmienne `readonly`. -Może przekazać je jako argumenty `in`, ale nie jako argumenty `ref` lub `out`.

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- w tych samych miejscach są dozwolone `ref readonly` Returns, `ref` są dozwolone.
Dotyczy to indeksatorów, delegatów, wyrażeń lambda, funkcji lokalnych.

- Nie jest dozwolone Przeciążenie na `ref`/`ref readonly`/różnice.

- Jest dozwolone Przeciążenie na zwykłych elementach ByVal i `ref readonly` Zwróć różnice.

- Na potrzeby OHI (Przeciążenie, ukrywanie, implementowanie) `ref readonly` jest podobny, ale różni się od `ref`.
Na przykład metoda, która zastępuje `ref readonly` jeden, musi być `ref readonly` i mieć typ zamienny tożsamości.

- Na potrzeby konwersji obiektów delegowanych/lambda/grup metod `ref readonly` jest podobna, ale różni się od `ref`.
Wyrażenia lambda i odpowiednie sugestie konwersji grup metod muszą pasować do `ref readonly` zwracanego delegata docelowego z `ref readonly` zwracanym typem, który jest konwertowany na tożsamość.

- Na potrzeby wariancji ogólnej `ref readonly` zwracane są nievariant.

> Uwaga: nie ma żadnych ostrzeżeń dotyczących `ref readonly` zwraca, które mają typ referencyjny lub podstawowy.
Może to być bezpunktowe, ale w niektórych przypadkach użytkownik musi/chcieć przekazać elementy pierwotne jako `in`. Przykłady — przesłanianie metody ogólnej, takiej jak `ref readonly T Method()`, gdy `T` został zastąpiony jako `int`.
>
>Możliwe jest posiadanie analizatora, który ostrzega w przypadku niewydajnego użycia `ref readonly` zwraca, ale reguły dla takiej analizy byłyby zbyt rozmyte, aby były częścią specyfikacji języka.

### <a name="returning-from-ref-readonly-members"></a>Powrót z `ref readonly` elementów członkowskich
Wewnątrz treści metody składnia jest taka sama jak w przypadku zwykłych zwrotów ref. `readonly` zostanie wywnioskowana z metody zawierającej.

Motywacja to, że `return ref readonly <expression>` jest niepotrzebna i zezwala tylko na niezgodności w części `readonly`, która zawsze spowoduje błędy.
`ref` jest jednak wymagana w celu zapewnienia spójności z innymi scenariuszami, w których coś jest przesyłane za pośrednictwem ścisłych aliasów a przez wartość.

> W przeciwieństwie do przypadku parametrów `in`, `ref readonly` zwraca nigdy nie zwracaj przez kopię lokalną. Biorąc pod uwagę, że kopia powinna przestać istnieć natychmiast po powrocie z tego rozwiązania, byłoby bezpunktowe i niebezpieczne. W związku z tym `ref readonly` zwracane są zawsze bezpośrednimi odwołaniami.

Przykład:

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- Argument `return ref` musi być LValue (**istniejąca reguła**)
- Argument `return ref` musi być "bezpiecznie do return" (**istniejąca reguła**)
- W elemencie członkowskim `ref readonly` argument `return ref` _nie musi być zapisywalny_ .
Na przykład taki element członkowski może odesłać odwołanie do pola tylko do odczytu lub jednego z jego parametrów `in`.

### <a name="safe-to-return-rules"></a>Bezpieczne reguły powrotu.
Normalne bezpieczne reguły powrotu dla odwołań będą również stosowane do odwołań tylko do odczytu.

Należy zauważyć, że `ref readonly` można uzyskać od zwykłego `ref` lokalnego/parametru/Return, ale nie odwrotnie. W przeciwnym razie bezpieczeństwo zwrotów `ref readonly` jest wywnioskowane w taki sam sposób jak w przypadku zwykłych `ref` Returns.

Biorąc pod uwagę, że RValues może być przekazana jako parametr `in` i zwrócony jako `ref readonly` potrzebujemy jeszcze jednej reguły — **rvalues nie są bezpieczne do zwrócenia przez odwołanie**.

> Rozważ sytuację, w której RValue jest przekazana do parametru `in` za pośrednictwem kopii, a następnie zwrócona z powrotem w postaci `ref readonly`. W kontekście obiektu wywołującego wynik takiego wywołania jest odwołaniem do danych lokalnych i nie jest to niebezpieczne do zwrócenia.
> Gdy RValues nie będą bezpiecznie zwracać, istniejąca reguła `#6` już obsłużyć ten przypadek.

Przykład:
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

Zaktualizowane reguły `safe to return`:

1.  **odwołania do zmiennych na stercie są bezpieczne do zwrócenia**
2.  **Parametry ref/in są bezpieczne do zwrócenia**
`in` parametry naturalnie można zwrócić tylko do odczytu.
3.  **parametry out są bezpieczne do zwrócenia** (ale muszą być ostatecznie przypisane, jak już dzisiaj)
4.  **pola struktury wystąpienia są bezpieczne do zwrócenia, o ile odbiornik jest bezpieczny do zwrócenia**
5.  **"This" nie jest bezpieczny do powrotu z elementów członkowskich struktury**
6.  **odwołanie zwrócone z innej metody jest bezpieczne do zwrócenia, jeśli wszystkie odwołania/przekroczenia przekazania do tej metody są bezpieczne do zwrócenia.** 
w *szczególności nie ma znaczenia, jeśli odbiornik jest bezpieczny do zwrócenia, niezależnie od tego, czy odbiornik jest strukturą, klasą, czy typem jako parametr typu ogólnego.*
7.  **Rvalues nie są bezpieczne do zwrócenia przez odwołanie.** 
 *, w odniesieniu do rvalues, są bezpieczne do przekazania jako parametry.*

> Uwaga: Istnieją dodatkowe reguły dotyczące bezpieczeństwa funkcji Returns, które mają być odtwarzane w przypadku, gdy są używane typy odwołania i odwołania ref.
> Reguły są równie stosowane do `ref` i `ref readonly` członków, dlatego nie są tu wymienione.

### <a name="aliasing-behavior"></a>Zachowanie aliasu.
`ref readonly` Członkowie zapewniają takie samo zachowanie aliasu jak zwykłe `ref` elementy członkowskie (z wyjątkiem do odczytu).
W związku z tym na potrzeby przechwytywania w wyrażeniach lambda, Async, Iteratory, rozlewania stosu itp... te same ograniczenia mają zastosowanie. co. ze względu na niezdolność do przechwytywania rzeczywistych odwołań i ze względu na charakter częściowej oceny członków takie scenariusze są niedozwolone.

> Jest to dozwolone i wymagane do tworzenia kopii, gdy `ref readonly` Return jest odbiornikiem zwykłych metod struktury, które przyjmują `this` jako zwykłe odwołanie do zapisu. Historycznie we wszystkich przypadkach, gdy takie wywołania są stosowane do zmiennej ReadOnly, tworzony jest kopia lokalna.

### <a name="metadata-representation"></a>Reprezentacja metadanych.
Gdy `System.Runtime.CompilerServices.IsReadOnlyAttribute` jest stosowany do powrotu metody zwracanej przez ByRef, oznacza to, że metoda zwraca odwołanie tylko do odczytu.

Ponadto sygnatury wynikowe takich metod (i tylko te metody) muszą mieć `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.

**Motywacja**: to zapewnia, że istniejące kompilatory nie mogą po prostu ignorować `readonly` podczas wywoływania metod przy użyciu `ref readonly` zwraca

## <a name="readonly-structs"></a>Struktury tylko do odczytu
W skrócie — funkcja, która wprowadza `this` parametr wszystkich elementów członkowskich wystąpienia struktury, z wyjątkiem konstruktorów, `in` parametru.

### <a name="motivation"></a>Motywacją
Kompilator musi założyć, że każde wywołanie metody w wystąpieniu struktury może zmodyfikować wystąpienie. W rzeczywistości odwołanie do zapisu jest przesyłane do metody jako parametr `this` i w pełni włącza takie zachowanie. Aby zezwolić na takie wywołania na zmiennych `readonly`, wywołania są stosowane do kopii tymczasowych. Mogą one być nieintuicyjne i czasami zmuszają osoby do porzucenia `readonly` ze względów wydajnościowych.  
Przykład: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/

Po dodaniu obsługi parametrów `in` i `ref readonly` zwraca problem z kopiowaniem z obroną będzie gorszy, ponieważ zmienne tylko do odczytu staną się bardziej popularne.

### <a name="solution"></a>Rozwiązanie
Zezwalaj na modyfikator `readonly` w deklaracjach struktury, co spowoduje, że `this` być traktowany jako parametr `in` dla wszystkich metod wystąpienia struktury z wyjątkiem konstruktorów.

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a>Ograniczenia dotyczące elementów członkowskich struktury tylko do odczytu
- Pola wystąpienia struktury tylko do odczytu muszą być tylko do odczytu.  
**Motywacja:** można pisać tylko do zewnątrz, ale nie do elementów członkowskich.
- Autowłaściwości wystąpienia struktury tylko do odczytu muszą być tylko do odczytu.  
**Motywacja:** konsekwencja ograniczenia dotyczącego pól wystąpień.
- Struktura ReadOnly nie może deklarować zdarzeń przypominających pola.  
**Motywacja:** konsekwencja ograniczenia dotyczącego pól wystąpień.

### <a name="metadata-representation"></a>Reprezentacja metadanych.
Gdy `System.Runtime.CompilerServices.IsReadOnlyAttribute` jest stosowana do typu wartości, oznacza to, że typ jest `readonly struct`.

W szczególności:
-  Tożsamość typu `IsReadOnlyAttribute` jest nieważna. W rzeczywistości może być osadzony przez kompilator w zawierającym go zestawie, jeśli jest to konieczne.

## <a name="refin-extension-methods"></a>`ref`/`in` metody rozszerzenia
Istnieje w rzeczywistości istniejąca propozycja (https://github.com/dotnet/roslyn/issues/165) i odpowiedni prototyp żądania ściągnięcia (https://github.com/dotnet/roslyn/pull/15650).
Chcę tylko potwierdzić, że ten pomysł nie jest zupełnie nowy. Jest to jednak istotne tutaj, ponieważ `ref readonly` eleganckie usuwa najbardziej contentious problem dotyczący takich metod — co należy zrobić z odbiornikami RValue.

Ogólnym pomysłem jest umożliwienie metodom rozszerzania `this` parametru przez odwołanie, o ile typ jest znany jako typ struktury.

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

Przyczyny zapisu takich metod rozszerzających są przede wszystkim następujące:  
1.  Unikaj kopiowania, gdy odbiornik jest dużą strukturą
2.  Zezwalaj na mutację metod rozszerzających w strukturach

Powody, dla których nie chcemy zezwalać na takie klasy  
1.  Jest to bardzo ograniczony cel.
2.  Spowoduje to przerwanie długotrwałej niezmiennej, którą wywołanie metody nie może wyłączyć odbiornika`null`, aby stał się `null` po wywołaniu.
> Obecnie zmienna inna niż`null` nie może stać się `null`, chyba że zostanie _jawnie_ przypisana lub przeniesiona przez `ref` lub `out`.
> Znacznie ułatwia to czytelność lub inną postać analizy "może to być wartość zerowa.
3.  Jest trudne do uzgodnienia z semantyką "Szacuj raz" z dostępem warunkowym o wartości null.
Przykład: `obj.stringField?.RefExtension(...)`-musi przechwycić kopię `stringField`, aby sprawdzenie wartości null było zrozumiałe, ale przypisania do `this` wewnątrz RefExtension nie zostaną odzwierciedlone z powrotem do pola.

Możliwość deklarowania metod rozszerzenia w **strukturach** przyjmujących pierwszy argument przez odwołanie było długotrwałym żądaniem. Jedną z rozważań dotyczących blokowania była "co się stanie, jeśli odbiornik nie jest LValue?".

- Istnieje poprzednik, że jakakolwiek metoda rozszerzająca może być również wywoływana jako metoda statyczna (czasami jest to jedyny sposób na rozwiązanie niejednoznaczności). Może to spowodować, że odbiorniki RValue powinny być niedozwolone.
- Z drugiej strony jest stosowana procedura tworzenia wywołań na kopii w podobnych sytuacjach, gdy są wykorzystywane metody wystąpień struktury.

Przyczyną niejawnego kopiowania jest fakt, że większość metod struktury nie modyfikuje struktury, chociaż nie jest w stanie wskazywać, że. Z tego względu najbardziej praktyczne rozwiązanie miało po prostu nawiązać wywołanie w kopii, ale ta metoda jest znana w celu uszkodzenia wydajności i spowodowania błędów.

Teraz, mając dostępność parametrów `in`, istnieje możliwość, aby rozszerzenie zasygnalizować zamiar. W związku z tym Conundrum można rozwiązać przez wymaganie, aby rozszerzenia `ref` miały być wywoływane przy użyciu odbiorców zapisywalnych, podczas gdy rozszerzenia `in` umożliwiają niejawne kopiowanie, jeśli jest to konieczne.

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a>`in` rozszerzenia i typy ogólne.
Celem metod rozszerzenia `ref` jest mutacja odbiorcy bezpośrednio lub poprzez wywoływanie mutacji członków. W związku z tym `ref this T` rozszerzenia są dozwolone, o ile `T` jest ograniczone do struktury.

Z drugiej strony metody rozszerzenia `in` istnieją przede wszystkim do zredukowania niejawnego kopiowania. Jednak każde użycie parametru `in T` będzie musiało zostać wykonane za pomocą elementu członkowskiego interfejsu. Ze względu na to, że wszystkie elementy członkowskie interfejsu są traktowane jako mutacje, wszelkie takie zastosowania wymagały kopiowania. — Zamiast zmniejszać kopiowanie, efekt byłby przeciwieństwem. Dlatego `in this T` nie jest dozwolone, gdy `T` jest parametrem typu ogólnego, niezależnie od ograniczeń.

### <a name="valid-kinds-of-extension-methods-recap"></a>Prawidłowe rodzaje metod rozszerzających (podsumowanie):
Następujące formy deklaracji `this` w metodzie rozszerzenia są teraz dozwolone:
1) `this T arg`-regularne rozszerzenie ByVal. (**istniejąca sprawa**)
- T może być dowolnego typu, w tym typów referencyjnych lub parametrów typu.
Wystąpienie będzie taka sama jak zmienna po wywołaniu.
Zezwala na niejawne konwersje rodzaju _konwersji tego argumentu_ .
Może być wywoływana w RValues.

- `in this T self` - `in` rozszerzenie.
T musi być rzeczywistym typem struktury.
Wystąpienie będzie taka sama jak zmienna po wywołaniu.
Zezwala na niejawne konwersje rodzaju _konwersji tego argumentu_ .
Może być wywoływana w RValues (w razie konieczności może być wywoływana w tempie).

- `ref this T self` - `ref` rozszerzenie.
T musi być typem struktury lub parametrem typu ogólnego ograniczonym jako strukturą.
Wystąpienie może być zapisywane przez wywołanie.
Zezwala tylko na konwersje tożsamości.
Musi być wywoływana na zapisywalnym LValue. (nigdy nie wywołane za pośrednictwem temp).

## <a name="readonly-ref-locals"></a>Lokalne odwołania tylko do odczytu.

### <a name="motivation"></a>Motywacją.
Po wprowadzeniu elementów członkowskich `ref readonly` były jasne przed użyciem, że muszą zostać sparowane z odpowiednim rodzajem lokalnego. Obliczanie elementu członkowskiego może dawać lub obserwować efekty uboczne, dlatego jeśli wynik musi być używany więcej niż jeden raz, musi on być przechowywany. Zwykłe `ref` lokalne nie pomagają w tym miejscu, ponieważ nie mogą być przypisane do `readonly` odwołania.   

### <a name="solution"></a>Narzędzie.
Zezwalaj na deklarowanie `ref readonly` lokalnych. Jest to nowy rodzaj `ref` lokalnych, które nie są zapisywalne. W wyniku `ref readonly` elementy lokalne mogą akceptować odwołania do zmiennych tylko do odczytu bez uwidaczniania tych zmiennych do zapisu.

### <a name="declaring-and-using-ref-readonly-locals"></a>Deklarowanie i używanie `ref readonly` lokalnych.

Składnia takich elementów lokalnych używa modyfikatorów `ref readonly` w witrynie deklaracji (w tej konkretnej kolejności). Podobnie jak w przypadku zwykłych `ref` lokalnych, `ref readonly` lokalnych musi być inicjowane ref w deklaracji. W przeciwieństwie do zwykłych `ref` lokalnych, `ref readonly` lokalnych może odwoływać się do `readonly` LValues, takich jak parametry `in`, `readonly` pól, `ref readonly` metod.

We wszystkich celach `ref readonly` lokalny jest traktowany jako zmienna `readonly`a. Większość ograniczeń dotyczących użycia jest taka sama jak w przypadku `readonly` pól lub `in` parametrów.

Na przykład pola `in` parametru, który ma typ struktury, są rekursywnie klasyfikowane jako zmienne `readonly`.   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a>Ograniczenia dotyczące stosowania `ref readonly` lokalnych
Z wyjątkiem `readonly`, `ref readonly` lokalnych zachowuje się jak zwykłe `ref` lokalne i podlegają dokładnie tym samym ograniczeniom.  
Na przykład ograniczenia związane z przechwytywaniem w zamknięciu, deklarowanie w metodach `async` lub analiza `safe-to-return` równo dotyczy `ref readonly` lokalnych.

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a>Wyrażenia `ref` Trzyelementowy. (alias "warunkowe LValues")

### <a name="motivation"></a>Motywacją
Korzystanie z `ref` i `ref readonly` lokalnych narażonych na konieczność inicjalizacji takich elementów lokalnych przy użyciu jednej lub innej zmiennej docelowej na podstawie warunku.

Typowym obejściem jest wprowadzenie metody podobnej do:

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

Należy zauważyć, że `Choice` nie jest dokładnym zastąpieniem Trzyelementowy, ponieważ _wszystkie_ argumenty muszą być oceniane w miejscu wywołania, co było wiodące w nieintuicyjnym zachowaniu i błędach.

Następujące elementy nie będą działały zgodnie z oczekiwaniami:

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a>Rozwiązanie
Zezwalaj na specjalne wyrażenie warunkowe, którego wynikiem jest odwołanie do jednego z argumentów LValue na podstawie warunku.

### <a name="using-ref-ternary-expression"></a>Używanie wyrażenia `ref` Trzyelementowy.

Składnia `ref` wersji wyrażenia warunkowego jest `<condition> ? ref <consequence> : ref <alternative>;`

Podobnie jak w przypadku zwykłego wyrażenia warunkowego tylko `<consequence>` lub `<alternative>` jest obliczana w zależności od wyniku wyrażenia warunku logicznego.

W przeciwieństwie do zwykłego wyrażenia warunkowego, `ref` wyrażenie warunkowe:
- wymaga, aby `<consequence>` i `<alternative>` były LValues.
- `ref` wyrażeniu warunkowym jest LValue i
- `ref` wyrażenie warunkowe jest zapisywalne, jeśli oba `<consequence>` i `<alternative>` są zapisywalne LValues

Przykłady:  
`ref` Trzyelementowy jest LValue i ponieważ może być przekazane/przypisane/zwrócone przez odwołanie;
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

Jest to LValue, ale może być również przypisany do.
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

Może służyć jako odbiornik wywołania metody i w razie potrzeby pomijać kopiowanie.
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

`ref` Trzyelementowy może być używana w zwykłym kontekście (nie ref).
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Widzę dwa główne argumenty dla rozszerzonej obsługi odwołań i odwołań tylko do odczytu:

1) Problemy, które są rozwiązywane w tym miejscu, są bardzo stare. Dlaczego nagle rozwiązać ten problem, szczególnie ponieważ nie może on pomóc w istniejącym kodzie?

Po znalezieniu C# i użyciu platformy .NET w nowych domenach niektóre problemy stają się coraz bardziej widoczne.  
Jako przykłady środowisk, które są bardziej krytyczne niż średnia o nadmiarach obliczeń, można wyświetlić listę

* scenariusze chmur/centrów danych, w których obliczanie jest rozliczane i czas odpowiedzi to przewagę konkurencyjną.
* Gry/VR/AR z wymaganiami dotyczącymi opóźnień w czasie rzeczywistym     

Ta funkcja nie ogranicza żadnej istniejącej siły, takiej jak bezpieczeństwo typu, a jednocześnie pozwala na obniżenie niższych kosztów w niektórych typowych scenariuszach.

2) Czy można zagwarantować, że obiekt wywoływany będzie odtwarzany przez reguły, gdy zdecyduje się na `readonly` umów?

W przypadku korzystania z `out`są podobne zaufania. Niepoprawna implementacja `out` może spowodować nieokreślone zachowanie, ale w rzeczywistości zdarza się to rzadko.  

Zastosowanie formalnych reguł weryfikacji znanych z `ref readonly` mogłoby jeszcze bardziej złagodzić problem z zaufaniem.

### <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Główny konkurencyjny projekt to naprawdę "nic nie rób".

### <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a>Spotkania projektowe

https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md
