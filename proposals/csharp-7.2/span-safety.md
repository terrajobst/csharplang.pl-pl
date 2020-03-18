---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485043"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a>Wymuszanie czasu kompilowania bezpieczeństwa dla typów typu referencyjnego

## <a name="introduction"></a>Wprowadzenie

Głównym powodem dodatkowych reguł bezpieczeństwa podczas pracy z typami, takimi jak `Span<T>` i `ReadOnlySpan<T>`, jest to, że takie typy muszą być ograniczone do stosu wykonywania.
 
Istnieją dwa powody, dla których `Span<T>` i podobne typy muszą być typami tylko stosu.

1. `Span<T>` jest semantycznie strukturą zawierającą odwołanie i `(ref T data, int length)`zakresem. Niezależnie od rzeczywistej implementacji, zapisy do takiej struktury nie są niepodzielne. Współbieżne "rozerwanie" takiej struktury doprowadziłoby do `length` nie pasuje do `data`, powodującego dostęp poza zakresem i naruszenia bezpieczeństwa typów, co ostatecznie może doprowadzić do uszkodzenia sterty GC w pozornie "bezpiecznym" kodzie.
2. Niektóre implementacje `Span<T>` dosłownie zawierają zarządzany wskaźnik w jednym z jego pól. Wskaźniki zarządzane nie są obsługiwane jako pola sterty i kodu, które zarządzają, aby umieścić zarządzany wskaźnik na stercie GC zazwyczaj awarie w czasie JIT.

Wszystkie powyższe problemy są rozwiązywane, jeśli wystąpienia `Span<T>` są ograniczone do istniejących tylko na stosie wykonywania. 

Dodatkowy problem występuje ze względu na składanie. Zalecane jest utworzenie bardziej złożonych typów danych, które mogłyby osadzić `Span<T>` i `ReadOnlySpan<T>` wystąpienia. Takie złożone typy muszą być strukturami i współdzielić wszystkie zagrożenia i wymagania dotyczące `Span<T>`. W związku z tym opisane tutaj reguły bezpieczeństwa powinny być wyświetlane jako mające zastosowanie do całego zakresu **_typów referencyjnych_** .

[Specyfikacja języka roboczego](#draft-language-specification) jest zaprojektowana w celu zapewnienia, że wartości typu "ref" są wykonywane tylko na stosie.

## <a name="generalized-ref-like-types-in-source-code"></a>Uogólnione typy `ref-like` w kodzie źródłowym

struktury `ref-like` są jawnie oznaczone w kodzie źródłowym przy użyciu modyfikatora `ref`:

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

Wyznaczenie struktury jako ref-like umożliwi strukturze, która ma pola wystąpienia przypominającego odwołania, a także wprowadzi wszystkie wymagania dotyczące typów referencyjnych, które mają zastosowanie do struktury. 

## <a name="metadata-representation-or-ref-like-structs"></a>Reprezentacja metadanych lub struktury przypominające odwołania

Struktury typu ref-like zostaną oznaczone atrybutem **System. Runtime. CompilerServices. IsRefLikeAttribute** .

Ten atrybut zostanie dodany do wspólnych bibliotek podstawowych, takich jak `mscorlib`. W przypadku, gdy atrybut nie jest dostępny, kompilator generuje wewnętrzne, podobne do innych atrybutów osadzonych na żądanie, takich jak `IsReadOnlyAttribute`.

Zostanie podjęta dodatkowa miara, która uniemożliwia korzystanie z struktur typu referencyjnego w kompilatorach, które nie znają reguł bezpieczeństwa (obejmuje C# to kompilatory przed tą, w której ta funkcja jest zaimplementowana). 

Bez innych dobrych alternatyw, które pracują w starych kompilatorach bez obsługi, atrybut `Obsolete` ze znanym ciągiem zostanie dodany do wszystkich struktur typu ref. Kompilatory, które wiedzą, jak używać typów odwołań typu REF, zignorują tę konkretną formę `Obsolete`.

Typowa reprezentacja metadanych:

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

Uwaga: nie jest to cel, aby uczynić go tak, aby wszelkie użycie typów odwołań typu ref na starych kompilatorów kończyło się niepowodzeniem 100%. Jest to trudne do osiągnięcia i nie jest absolutnie konieczne. Na przykład zawsze można uzyskać dostęp do `Obsolete` przy użyciu kodu dynamicznego lub, na przykład, utworzyć tablicę typów typu referencyjnego za pomocą odbicia.

W szczególności, jeśli użytkownik chce w rzeczywistości umieścić atrybut `Obsolete` lub `Deprecated` w typie podobnym do, nie zostanie wybrana opcja inna niż nie emituje wstępnie zdefiniowanego, ponieważ atrybut `Obsolete` nie może zostać zastosowany więcej niż raz.  

## <a name="examples"></a>Przykłady:

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a>Specyfikacja języka wersji roboczej

Poniżej opisano zestaw reguł bezpieczeństwa dla typów referencyjnych (`ref struct`s), aby upewnić się, że wartości tych typów występują tylko na stosie. Inny, prostszy zestaw reguł bezpieczeństwa byłby możliwy, jeśli nie można przekazywać elementów lokalnych przez odwołanie. Ta specyfikacja umożliwia również bezpieczne ponowne przypisanie odwołań lokalnych.

### <a name="overview"></a>Omówienie

Jest on kojarzony z każdym wyrażeniem w czasie kompilacji, koncepcji zakresu, w którym wyrażenie jest dozwolone, do którego jest dozwolony, "bezpieczne do ucieczki". Podobnie w przypadku każdej lvalue utrzymujemy koncepcję zakresu, do którego odnosi się odwołanie, do "ref-Safe-to-Escape". Dla danego wyrażenia lvalue mogą się one różnić.

Są one analogiczne do "bezpiecznej do zwrócenia" funkcji ref locales, ale jest bardziej szczegółowy. Gdzie wyrażenie "Safe-to-Return" jest rejestrowane tylko w przypadku, gdy (lub nie) może to zmienić metodę otaczającą jako całość, rekordy bezpiecznych do ucieczki, w których zakres może przekroczyć (w jakim zakresie może nie ucieczki). Podstawowy mechanizm bezpieczeństwa jest wymuszany w następujący sposób. Przypisanie z wyrażenia E1 z zakresem bezpiecznego do ucieczki S1 do (lvalue) Expression E2 z zakresem "bezpieczny do-ucieczk" S2, występuje błąd, jeśli S2 jest szerszym zakresem niż S1. Przez konstrukcję dwa zakresy S1 i S2 znajdują się w relacji zagnieżdżania, ponieważ wyrażenie prawne zawsze jest bezpieczne do zwrócenia z jakiegoś zakresu otaczającego wyrażenie.

W odpowiednim czasie, na potrzeby analizy, do obsługi tylko dwóch zakresów — zewnętrznego względem metody i zakresu najwyższego poziomu metody. Wynika to z faktu, że nie można tworzyć wartości typu referencyjnego z zakresami wewnętrznymi, a odwołania lokalne nie obsługują ponownego przypisywania. Reguły te mogą jednak obsługiwać więcej niż dwa poziomy zakresu.

Precyzyjne reguły obliczania stanu *bezpiecznego i powrotu* wyrażenia oraz reguły regulujące warunki prawne wyrażeń, należy stosować.

### <a name="ref-safe-to-escape"></a>odwołanie-bezpieczne-do-ucieczki

Wartość *ref-Safe-to-Escape* jest zakresem zawierającym wyrażenie lvalue, do którego jest bezpieczna w przypadku odwołania do lvalue. Jeśli zakresem jest cała Metoda, mówimy, że odwołanie do lvalue jest *bezpieczne do zwrócenia* z metody.

### <a name="safe-to-escape"></a>bezpieczna do wyjścia

*Bezpieczny do ucieczki* jest zakresem zawierającym wyrażenie, które jest bezpieczne dla wartości, do której ma zostać zmieniony. Jeśli zakresem jest cała Metoda, Załóżmy, że wartość jest *bezpieczna do zwrócenia* z metody.

Wyrażenie, którego typ nie jest typem `ref struct`, jest *bezpieczne do zwrócenia* z całej otaczającej metody. W przeciwnym razie odwołujemy się do poniższych reguł.

#### <a name="parameters"></a>Parametry

Lvalue wyznaczania parametru formalnego to *ref-Safe-to-Escape* (poprzez odwołanie) w następujący sposób:
- Jeśli parametr jest parametrem `ref`, `out`lub `in`, jest to metoda *ref-Safe-to-ucieczki* z całej metody (np. przez instrukcję `return ref`); przypadku
- Jeśli parametr jest `this`m parametrem typu struktury, jest to *odwołanie-bezpieczne-do-ucieczki* do zakresu najwyższego poziomu metody (ale nie z całej metody); [Przykład](#struct-this-escape)
- W przeciwnym razie parametr jest parametrem wartości i jest *typu ref-Safe-to-Escape* do zakresu najwyższego poziomu metody (ale nie z samej metody).

Wyrażenie, które jest rvalue, wyznaczające użycie parametru formalnego, jest *bezpieczne do ucieczki* (według wartości) z całej metody (np. przez instrukcję `return`). Dotyczy to również parametru `this`.

#### <a name="locals"></a>Zmienne lokalne

Lvalue wyznaczający zmienną lokalną to *ref-Safe-to-Escape* (poprzez odwołanie) w następujący sposób:
- Jeśli zmienna jest zmienną `ref`, jej wartość *ref-Safe-to-ucieczki* jest pobierana z wyrażenia *ref-Safe-* to-ucieczki w jego wyrażeniu inicjowania; przypadku
- Zmienna ma wartość *ref-Safe-to-ucieczki* zakresu, w którym został zadeklarowany.

Wyrażenie, które jest rvalue, wyznaczające użycie zmiennej lokalnej jest *bezpieczne do ucieczki* (według wartości) w następujący sposób:
- Jednak Powyższa reguła ogólna, której typ nie jest typem `ref struct`, jest *bezpieczna do zwrócenia* z całej otaczającej metody.
- Jeśli zmienna jest zmienną iteracji pętli `foreach`, zakres *bezpiecznego do ucieczki* zmiennej jest taka sama jak wartość " *bezpieczna do ucieczki* " `foreach` wyrażeniu pętli.
- Lokalny typ `ref struct` i niezainicjowany w punkcie deklaracji jest *bezpieczny-do-Return* z całej otaczającej metody.
- W przeciwnym razie typ zmiennej jest typu `ref struct`, a deklaracja zmiennej wymaga inicjatora. Zakres *bezpiecznej do ucieczki* zmiennej jest taki sam jak *bezpieczny dla* niego inicjatora.

#### <a name="field-reference"></a>Odwołanie do pola

Lvalue wyznaczający odwołanie do pola, `e.F`, to *ref-Safe-to-Escape* (poprzez odwołanie) w następujący sposób:
- Jeśli `e` ma typ referencyjny, jest to metoda *ref-Safe-to-Escape* z całej metody; przypadku
- Jeśli `e` ma typ wartości, jego wartość *ref-Safe-to-Escape* jest pobierana z `e`*ref-Safe-to* -ucieczki.

Rvalue wyznaczający odwołanie do pola, `e.F`, ma zakres *bezpieczny-do-ucieczki* , który jest taki sam jak *bezpieczny* dla `e`.

#### <a name="operators-including-"></a>Operatory, w tym `?:`

Aplikacja operatora zdefiniowanego przez użytkownika jest traktowana jako wywołanie metody.

Dla operatora, który daje element rvalue, taki jak `e1 + e2` lub `c ? e1 : e2`, *bezpieczna do ucieczki* wyniku jest najwęższym zakresem od *bezpiecznych do ucieczki* operandów operatora.  W wyniku tego dla operatora jednoargumentowego, który daje rvalue, taki jak `+e`, *bezpieczna do ucieczki* wyniku jest *bezpieczna do ucieczki* operandu.

Dla operatora, który daje lvalue, taki jak `c ? ref e1 : ref e2`
- wartość *ref-Safe-to-Escape* wyniku to najwęższy zakres między *odwołaniami typu ref-Safe-do-ucieczki* operatora.
- *bezpieczne do ucieczki* operandy muszą zgadzać się, a to jest *bezpieczna do ucieczki* wyników lvalue.

#### <a name="method-invocation"></a>Wywołanie metody

Lvalue wynikający z wywołania metody zwracającej odwołanie `e1.M(e2, ...)` ma wartość *ref-Safe-to-ucieczki* najmniejszy z następujących zakresów:
- Cała zaotaczająca Metoda
- *odwołania* do wszystkich argumentów `ref` i `out` (z wyłączeniem odbiorcy)
- Dla każdego `in` parametru metody, jeśli istnieje odpowiednie wyrażenie, które jest lvalue, jego *ref-Safe-to-Escape*, w przeciwnym razie najbliższy zakres otaczający
- *bezpieczne* dla wszystkich wyrażeń argumentów (w tym odbiorcy)

> Uwaga: ostatni punktor jest niezbędny do obsługi kodu, takiego jak
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> lub
> ```csharp
> return ref M(sp, 0);
> ```

Rvalue wynikający z wywołania metody `e1.M(e2, ...)` jest *bezpieczna do ucieczki* z najmniejszego z następujących zakresów:
- Cała zaotaczająca Metoda
- *bezpieczne* dla wszystkich wyrażeń argumentów (w tym odbiorcy)

#### <a name="an-rvalue"></a>Rvalue
Rvalue ma wartość *ref-Safe-to-ucieczki* z najbliższego zakresu otaczającego. Dzieje się tak na przykład w wywołaniu, takim jak `M(ref d.Length)`, gdzie `d` jest typu `dynamic`. Jest ona również spójna z (i prawdopodobnie subsumes) z obsługą argumentów odpowiadających parametrom `in`. *

#### <a name="property-invocations"></a>Wywołania właściwości

Wywołanie właściwości (`get` lub `set`) traktowane jako wywołanie metody źródłowej metody przez powyższe reguły.

#### `stackalloc`

Wyrażenie stackalloc jest rvalue, który jest *bezpieczny-do-ucieczki* do zakresu najwyższego poziomu metody (ale nie z całej metody).

#### <a name="constructor-invocations"></a>Wywołania konstruktora

Wyrażenie `new`, które wywołuje Konstruktor, przestrzega te same reguły jak wywołanie metody, które jest uważane za typ, który jest tworzony.

Dodatkowo *bezpieczne do ucieczki* nie jest szersze niż *najmniejszy ze wszystkich* argumentów/operandów wyrażeń inicjatora obiektów, rekursywnie, Jeśli inicjator jest obecny. 

#### <a name="span-constructor"></a>Konstruktor zakresu
Język opiera się na `Span<T>` nie ma konstruktora następującej formy:

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

Taki Konstruktor sprawia, że `Span<T>`, które są używane jako pola, które można odróżnić od pola `ref`. Reguły bezpieczeństwa opisane w tym dokumencie zależą od `ref` pól, które nie są prawidłową konstrukcją C#w programie, lub .NET.

#### <a name="default-expressions"></a>wyrażenia `default`

Wyrażenie `default` jest *bezpieczne-do-ucieczki* z całej otaczającej metody.

## <a name="language-constraints"></a>Ograniczenia języka

Chcemy upewnić się, że żadna `ref` zmienna lokalna i żadna zmienna typu `ref struct` nie odwołuje się do pamięci stosu lub zmiennych, które nie są już aktywne. W związku z tym mamy następujące ograniczenia dotyczące języka:

- Ani parametru ref, ani lokalnego nie, ani parametru ani lokalnego typu `ref struct` nie można znieść do funkcji lambda ani lokalnej.

- Ani parametr ref, ani parametr typu `ref struct` nie może być argumentem metody iteratora ani metody `async`.

- Ani nie lokalnego ani lokalnego typu `ref struct` nie mogą znajdować się w zakresie w punkcie instrukcji `yield return` lub wyrażenia `await`.

- Typ `ref struct` nie może być używany jako argument typu lub jako typ elementu w typie krotki.

- Typ `ref struct` nie może być zadeklarowanym typem pola, z tą różnicą, że może to być zadeklarowany typ pola wystąpienia innego `ref struct`.

- Typ `ref struct` nie może być typem elementu tablicy.

- Wartość typu `ref struct` nie może być opakowana:
  - Nie istnieje konwersja z typu `ref struct` do typu `object` lub `System.ValueType`.
  - Nie można zadeklarować typu `ref struct` w celu zaimplementowania dowolnego interfejsu
  - Żadna metoda wystąpienia nie została zadeklarowana w `object` lub w `System.ValueType`, ale nie została zastąpiona w `ref struct` typie może być wywoływana z odbiornikiem tego typu `ref struct`.
  - Żadna metoda wystąpienia typu `ref struct` nie może zostać przechwycona przez konwersję metody na typ delegata.

- W przypadku ponownego przypisania odwołań `ref e1 = ref e2`*odwołanie typu "bezpieczny-do-ucieczki"* `e2` musi być co najmniej tak szerokie jak *"ref-Safe-to-Escape* " `e1`.

- Dla instrukcji return ref `return ref e1`, Metoda *ref-Safe-to-ucieczk* `e1` musi być typu *ref-Safe-to-Escape* z całej metody. (Do zrobienia: potrzebna jest również reguła, która `e1` musi być *bezpieczna do ucieczki* z całej metody lub czy jest nadmiarowa?)

- W przypadku instrukcji return `return e1`, *bezpieczna do ucieczki* `e1` musi być *bezpieczna do ucieczki* z całej metody.

- W przypadku przypisania `e1 = e2`, jeśli typ `e1` jest typem `ref struct`, wówczas *bezpieczna do ucieczki* `e2` musi być co najmniej tak szeroki zakres *jak w przypadku* `e1`.

- W przypadku wywołania metody, jeśli istnieje argument `ref` lub `out` typu `ref struct` (włącznie z odbiornikiem), z *bezpiecznym do-ucieczk* E1, a żaden argument (w tym odbiorca) może mieć węższy *bezpieczny do ucieczki* niż E1. [Przykład](#method-arguments-must-match)

- Funkcja lokalna lub funkcja anonimowa nie może odwoływać się do lokalnego lub parametru typu `ref struct` zadeklarowanego w otaczającym zakresie.

> ***Problem otwarty:*** Potrzebujemy pewnej reguły, która pozwala na wygenerowanie błędu, gdy konieczne jest przelanie wartości stosu typu `ref struct` w wyrażeniu await, na przykład w kodzie
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a>Wyjaśnienia
Te wyjaśnienia i przykłady ułatwiają wyjaśnienie, dlaczego istnieje wiele reguł bezpieczeństwa

### <a name="method-arguments-must-match"></a>Argumenty metody muszą być zgodne
Podczas wywoływania metody, w której występuje `out`, `ref` parametr, który jest `ref struct`, w tym odbiorcę, wszystkie `ref struct` muszą mieć ten sam okres istnienia. Jest to konieczne, C# ponieważ należy podjąć wszystkie decyzje dotyczące bezpieczeństwa okresu istnienia na podstawie informacji dostępnych w podpisie metody oraz okresu istnienia wartości w lokacji wywołania. 

Jeśli istnieją `ref` parametry, które są `ref struct`, possiblity może zamienić ich zawartość. W związku z tym należy upewnić się, że wszystkie te **potencjalne** zamiany są zgodne. Jeśli język nie wymusił, że umożliwi to niewłaściwy kod, taki jak poniższy.

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

Ograniczenie na odbiorniku jest konieczne, ponieważ chociaż żadna z jej zawartości nie jest bezpieczna w trybie ref-do-ucieczki, może przechowywać podane wartości. Oznacza to, że w przypadku niezgodnych okresów istnienia można utworzyć otwór bezpieczeństwa typu w następujący sposób:

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a>Struktura ta ucieczki
Gdy chodzi o zakres reguł bezpieczeństwa, wartość `this` w składowej wystąpienia jest modelowana jako parametr do elementu członkowskiego. Teraz dla `struct` typ `this` jest w rzeczywistości `ref S`, gdzie w `class` jest po prostu `S` (dla elementów członkowskich `class / struct` o nazwie S). 

Jeszcze `this` ma inne reguły ucieczki niż inne parametry `ref`. W przeciwnym razie nie jest bezpieczna w trybie ref-do-ucieczki, a inne parametry są:

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

Przyczyna tego ograniczenia jest w rzeczywistości niewielka, aby można było wykonać `struct`go wywołania elementu członkowskiego. Istnieją pewne reguły, które należy wykonać w odniesieniu do elementu członkowskiego, na którym znajdują się `struct` członkowie, gdzie odbiorca jest rvalue. Jednak jest to bardzo rozwiązanie. 

Przyczyną tego ograniczenia jest faktyczne wywołanie interfejsu. W przeciwnym razie jest to możliwe niezależnie od tego, czy Poniższa próba lub nie powinna zostać skompilowana;

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

Rozważ przypadek, w którym `T` jest tworzona jako `struct`. Jeśli parametr `this` ma wartość Ref-Safe-to-Escape, zwracany `p.Get` może wskazywać na stos (w odróżnieniu od typu wystąpienia `T`). Oznacza to, że język nie może zezwolić na kompilację tego przykładu, ponieważ może on zwrócić `ref` do lokalizacji stosu. Z drugiej strony, jeśli `this` nie jest bezpieczny-do-ucieczki, `p.Get` nie może odwoływać się do stosu, co z tego powodu jest bezpieczne do zwrócenia. 

Dzieje się tak dlatego, że ucieczka `this` w `struct` jest naprawdę wszystkie interfejsy. Może ona być bezwzględnie niezawodna, ale jej działanie jest wyłączone. Projekt ostatecznie zatrzymał się w celu zwiększenia elastyczności interfejsów. 

Istnieje możliwość, że firma Microsoft może w przyszłości złagodzić ten problem. 

## <a name="future-considerations"></a>Zagadnienia w przyszłości

### <a name="length-one-spant-over-ref-values"></a>Długość jednego zakresu\<T > za pośrednictwem wartości ref
Chociaż obecnie nie jest to dozwolone, istnieją przypadki, w których tworzenie długości jednego `Span<T>` wystąpienia przez wartość byłoby korzystne:

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

Ta funkcja uzyskuje bardziej atrakcyjny sposób, jeśli znosimy ograniczenia dotyczące [buforów o stałym rozmiarze](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) , ponieważ umożliwi to `Span<T>` wystąpienia o większej długości. 

Jeśli kiedykolwiek chcesz przejść do tej ścieżki, język może obsłużyć to przez zapewnienie, że takie `Span<T>` wystąpienia były tylko w dół. To są tylko *bezpieczne do wyjścia* do zakresu, w którym zostały utworzone. Dzięki temu język nigdy nie musiał rozważać `ref` wartość ucieczki metody za pomocą `ref struct` zwracają lub pola `ref struct`. Może to spowodować również konieczność wprowadzenia dalszych zmian w celu rozpoznania takich konstruktorów jako przechwycenia `ref` parametru w ten sposób.
