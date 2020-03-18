---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485183"
---
# <a name="default-interface-methods"></a>domyślne metody interfejsu

* [x] proponowane
* [] Prototyp: [w toku](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)
* [] Implementacja: brak
* [] — Specyfikacja: w toku, poniżej

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Dodawanie obsługi _metod rozszerzenia wirtualnego_ — metody w interfejsach z konkretnymi implementacjami. Klasa lub struktura implementująca taki interfejs jest wymagana do posiadania jednej _najbardziej konkretnej_ implementacji dla metody interfejsu, implementowanej przez klasę lub strukturę lub dziedziczoną z jej klas podstawowych lub interfejsów. Wirtualne metody rozszerzenia umożliwiają autorowi interfejsu API dodawanie metod do interfejsu w przyszłych wersjach bez przerywania zgodności źródłowej lub binarnej z istniejącymi implementacjami tego interfejsu.

Są one podobne do ["domyślnych metod"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)języka Java.

(W oparciu o możliwą technikę implementacji) Ta funkcja wymaga odpowiedniej obsługi w interfejsie wiersza polecenia/CLR. Programy korzystające z tej funkcji nie mogą działać w starszych wersjach platformy.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Najważniejsze motywacje dla tej funkcji to

- Domyślne metody interfejsu umożliwiają autorowi interfejsu API dodawanie metod do interfejsu w przyszłych wersjach bez przerywania zgodności źródłowej lub binarnej z istniejącymi implementacjami tego interfejsu.
- Funkcja umożliwia C# współdziałanie z interfejsami API przeznaczonymi dla [systemu Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) i [iOS (SWIFT)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), które obsługują podobne funkcje.
- W miarę wyłączania, Dodawanie domyślnych implementacji interfejsu zapewnia elementy funkcji języka "cechy" (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>). Cechy zostały sprawdzone jako zaawansowana technika programowania (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Składnia interfejsu jest rozszerzona, aby umożliwić

- deklaracje składowych, które deklarują stałe, operatory, konstruktory statyczne i zagnieżdżone typy;
- *treść* metody lub indeksatora, właściwości lub metody dostępu do zdarzeń (czyli implementacji "domyślna");
- deklaracje elementów członkowskich, które deklarują pola statyczne, metody, właściwości, indeksatory i zdarzenia;
- deklaracje elementów członkowskich przy użyciu jawnej składni implementacji interfejsu; lub
- Modyfikatory jawnego dostępu (domyślny dostęp jest `public`).

Członkowie z organami zezwalają interfejsowi na określenie implementacji "default" dla metody w klasach i strukturach, które nie oferują implementacji zastępującej.

Interfejsy nie mogą zawierać stanu wystąpienia. Gdy pola statyczne są teraz dozwolone, pola wystąpień nie są dozwolone w interfejsach. Funkcja autowłaściwości wystąpienia nie jest obsługiwana w interfejsach, ponieważ niejawnie deklaruje pole ukryte.

Metody static i Private umożliwiają przydatną refaktoryzację i organizację kodu używaną do implementowania publicznego interfejsu API interfejsu.

Przesłonięcie metody w interfejsie musi używać jawnej składni implementacji interfejsu.

Wystąpił błąd podczas deklarowania typu klasy, typu struktury lub typu wyliczeniowego w zakresie parametru typu, który został zadeklarowany za pomocą *variance_annotation*.  Na przykład deklaracja `C` poniżej jest błędem.

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a>Konkretne metody w interfejsach

Najprostszą formą tej funkcji jest możliwość deklarowania *konkretnej metody* w interfejsie, który jest metodą z treścią.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

Klasa implementująca ten interfejs nie musi implementować jej konkretnej metody.

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

Ostateczne przesłonięcie `IA.M` w klasie `C` jest konkretną metodą `M` zadeklarowaną w `IA`. Należy zauważyć, że Klasa nie dziedziczy członków z jego interfejsów; to nie jest zmieniane przez tę funkcję:

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

W ramach elementu członkowskiego wystąpienia interfejs `this` ma typ otaczającego interfejsu.

### <a name="modifiers-in-interfaces"></a>Modyfikatory w interfejsach

Składnia interfejsu jest swobodna, aby zezwalać na Modyfikatory elementów członkowskich. Dozwolone są następujące elementy: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`i `partial`.

> Do ***zrobienia***: Sprawdź, jakie istnieją inne modyfikatory.

Element członkowski interfejsu, którego deklaracja zawiera treść, jest składową `virtual`, chyba że jest używany modyfikator `sealed` lub `private`. Modyfikator `virtual` może być używany w składowej funkcji, która w przeciwnym razie mógłby być niejawnie `virtual`. Podobnie, chociaż `abstract` jest wartością domyślną w elementach członkowskich interfejsu bez treści, modyfikator ten może być jawnie określony. Niewirtualny element członkowski można zadeklarować za pomocą słowa kluczowego `sealed`.

Wystąpił błąd dla elementu członkowskiego `private` lub `sealed` funkcji w interfejsie, aby nie mieć treści. Element członkowski funkcji `private` nie może mieć modyfikatora `sealed`.

Modyfikatory dostępu mogą być używane w przypadku elementów członkowskich interfejsu wszystkich rodzajów elementów członkowskich, które są dozwolone. Poziom dostępu `public` jest wartością domyślną, ale może być jawnie określony.

> ***Problem otwarty:*** Musimy określić precyzyjne znaczenie modyfikatorów dostępu, takich jak `protected` i `internal`, i które deklaracje i nie zastępować ich (w interfejsie pochodnym) ani implementować (w klasie implementującej interfejs).

Interfejsy mogą deklarować składowe `static`, w tym typy zagnieżdżone, metody, indeksatory, właściwości, zdarzenia i konstruktory statyczne. Domyślny poziom dostępu dla wszystkich elementów członkowskich interfejsu jest `public`.

Interfejsy nie mogą deklarować konstruktorów wystąpień, destruktorów ani pól.

> ***Zamknięty problem:*** Czy deklaracje operatora powinny być dozwolone w interfejsie? Prawdopodobnie nie są to operatory konwersji, ale co z innymi? ***Decyzja***: operatory są dozwolone *z wyjątkiem* operatorów konwersji, równości i nierówności.

> ***Zamknięty problem:*** Czy `new` być dozwolone w deklaracjach składowych interfejsu, które ukrywają elementy członkowskie z interfejsów podstawowych? ***Decyzja***: tak.

> ***Zamknięty problem:*** Obecnie nie zezwalamy na `partial` w interfejsie ani jego elementach członkowskich. To wymaga oddzielnej propozycji. ***Decyzja***: tak. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a>Zastąpienia w interfejsach

Deklaracje przesłonięcia (tj. te zawierające modyfikator `override`) umożliwiają programistom dostarczenie najbardziej szczegółowej implementacji wirtualnego elementu członkowskiego w interfejsie, w którym kompilator lub środowisko uruchomieniowe nie wyszuka jednego z nich. Umożliwia również włączenie abstrakcyjnego elementu członkowskiego z interfejsu Super do domyślnego elementu członkowskiego w interfejsie pochodnym. Deklaracja przesłonięcia może *jawnie* przesłonić konkretną metodę interfejsu podstawowego przez zakwalifikowanie deklaracji z nazwą interfejsu (w tym przypadku nie jest dozwolony modyfikator dostępu). Niejawne zastąpienia są niedozwolone.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

Deklaracje przesłonięcia w interfejsach nie mogą być deklarowane `sealed`.

Elementy członkowskie funkcji publicznych `virtual` w interfejsie mogą zostać zastąpione jawnie w interfejsie pochodnym (przez zakwalifikowanie nazwy w deklaracji override z typem interfejsu, który pierwotnie zadeklarował metodę, i pominięciem modyfikatora dostępu).

elementy członkowskie funkcji `virtual` w interfejsie mogą zostać zastąpione wyłącznie jawnie (niejawnie) w interfejsach pochodnych, a składowe, które nie są `public` mogą być implementowane tylko w klasie lub strukturze jawnie (niejawnie). W obu przypadkach zastąpiony lub zaimplementowany element członkowski musi być *dostępny* , gdy zostanie przesłonięty.

### <a name="reabstraction"></a>Reabstraction

Metoda wirtualna (konkretna) zadeklarowana w interfejsie może zostać zastąpiona jako abstrakcyjna w interfejsie pochodnym

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

Modyfikator `abstract` nie jest wymagany w deklaracji `IB.M` (jest to wartość domyślna w interfejsach), ale prawdopodobnie dobrym sposobem jest jawne w deklaracji przesłonięcia.

Jest to przydatne w przypadku interfejsów pochodnych, w których domyślna implementacja metody jest nieodpowiedni i bardziej odpowiednia implementacja powinna być dostarczana przez implementację klas.

> ***Problem otwarty:*** Czy powinno być dozwolone przedzielenie?

### <a name="the-most-specific-override-rule"></a>Najbardziej Szczegółowa reguła przesłonięcia

Firma Microsoft wymaga, aby każdy interfejs i Klasa mieli *najbardziej specyficzne przesłonięcie* dla każdego wirtualnego elementu członkowskiego spośród zastąpień występujących w typie lub jego interfejsie bezpośrednim i pośrednim. *Najbardziej konkretny przesłonięcie* jest unikatowym przesłonięciem, który jest bardziej szczegółowy niż każdy inny przesłonięcie. Jeśli nie ma żadnych zastąpień, sam element członkowski jest uznawany za najbardziej konkretne przesłonięcie.

`M1` przesłonięcia jest uznawana za *bardziej precyzyjną* niż inne `M2` przesłaniania, jeśli `M1` jest zadeklarowana dla typu `T1`, `M2` jest zadeklarowana dla typu `T2`i albo

1. `T1` zawiera `T2` między interfejsami bezpośrednimi lub pośrednimi lub
2. `T2` jest typem interfejsu, ale `T1` nie jest typem interfejsu.

Na przykład:

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

Najbardziej konkretna reguła przesłonięcia gwarantuje, że konflikt (tj. niejednoznaczności wynikające z dziedziczenia rombu) jest rozpoznawany jawnie przez programistę w punkcie, w którym występuje konflikt.

Ponieważ obsługujemy jawne zastąpienia abstrakcyjne w interfejsach, można to zrobić również w klasach

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> ***Problem z otwartym***: czy należy obsługiwać jawne zastąpienia abstrakcyjne interfejsu w klasach?

Ponadto jest to błąd, jeśli w deklaracji klasy najbardziej specyficzne przesłonięcie jakiejś metody interfejsu jest abstrakcyjnym przesłonięciem zadeklarowanym w interfejsie. Jest to istniejąca reguła przestanowa przy użyciu nowej terminologii.

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

Istnieje możliwość, że właściwość wirtualna zadeklarowana w interfejsie ma najbardziej specyficzne przesłonięcie dla metody dostępu `get` w jednym interfejsie i najbardziej konkretne przesłonięcie dla metody dostępu `set` w innym interfejsie. Jest to uznawane za naruszenie dla *najbardziej konkretnej reguły przesłonięcia* .

### <a name="static-and-private-methods"></a>Metody `static` i `private`

Ponieważ interfejsy mogą teraz zawierać kod wykonywalny, warto użyć abstrakcyjnego kodu do metod prywatnych i statycznych. Teraz zezwalamy na te interfejsy.

> ***Problem zamknięty***: czy mamy obsługiwać metody prywatne? Czy mamy obsługiwać metody statyczne? **Decyzja: tak**

> ***Problem otwarty***: czy mamy pozwolić na `protected` lub `internal` lub inny dostęp do metod interfejsu? Jeśli tak, jakie są semantyki? Czy są one `virtual` domyślnie? Jeśli tak, czy istnieje sposób, aby nie były wirtualne?

> ***Otwórz problem***: Jeśli obsługujemy metody statyczne, czy są obsługiwane (statyczne) operatory?

### <a name="base-interface-invocations"></a>Wywołania interfejsu podstawowego

Kod w typie, który pochodzi od interfejsu z metodą domyślną, może jawnie wywołać implementację "Base" tego interfejsu.

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

Metoda wystąpienia (niestatycznego) może wywoływać implementację metody dostępnego wystąpienia w bezpośrednim interfejsie podstawowym, wpisując nazwę przy użyciu składni `base(Type).M`. Jest to przydatne, gdy przesłonięcie wymagane do pomyślnego przeprowadzenia dziedziczenia jest rozpoznawane przez delegowanie do jednej konkretnej implementacji podstawowej.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

Gdy dostęp do `virtual` lub `abstract` jest uzyskiwany przy użyciu składni `base(Type).M`, wymagane jest, aby `Type` zawierać unikatowy *przesłonięcie* dla `M`.

### <a name="binding-base-clauses"></a>Powiązane klauzule podstawowe

Interfejsy zawierają teraz typy.  Te typy mogą być używane w klauzuli Base jako interfejsy podstawowe.  W przypadku tworzenia powiązania z klauzulą podstawową może być konieczne poznanie zestawu interfejsów podstawowych, aby powiązać te typy (np. do wyszukania w nich i rozwiązania dostępu chronionego).  Znaczenie klauzuli podstawowej interfejsu jest zdefiniowane cyklicznie.  Aby przerwać cykl, dodamy nowe reguły językowe odpowiadające podobnej regule dla klas.

Podczas określania znaczenia *interface_base* interfejsu, tymczasowo przyjęto, że interfejsy podstawowe są puste. Intuicyjnie gwarantuje to, że znaczenie klauzuli podstawowej nie może rekursywnie zależeć od siebie. 

**Używamy następujących reguł:**

"Gdy klasa B dziedziczy z klasy A, jest to błąd czasu kompilacji dla elementu, który jest zależny od B. Klasa **bezpośrednio zależy** od jej bezpośredniej klasy podstawowej (jeśli istnieje) i **bezpośrednio zależy** od ~~**klasy**~~ , w której jest od razu zagnieżdżona (jeśli istnieje). W związku z tą definicją kompletny zestaw ~~**klas**~~ , od których zależy Klasa, jest bezpośrednie i przechodnie zamknięcie **bezpośrednio zależy** od relacji. "

Jest to błąd czasu kompilacji dla interfejsu, aby bezpośrednio lub pośrednio dziedziczyć po sobie samym.
**Podstawowe interfejsy** interfejsu to jawne interfejsy podstawowe i ich interfejsy podstawowe. Innymi słowy, zestaw interfejsów podstawowych to pełne przechodnie zamknięcie jawnych interfejsów podstawowych, ich jawne interfejsy podstawowe i tak dalej.

**Są one dostosowywane w następujący sposób:**

Gdy klasa B dziedziczy z klasy A, jest to błąd czasu kompilacji dla elementu, który jest zależny od B. Klasa **bezpośrednio zależy** od jej bezpośredniej klasy podstawowej (jeśli istnieje) i **bezpośrednio zależy** od _**typu**_ , w którym jest od razu zagnieżdżona (jeśli istnieje).

Gdy interfejs IB rozszerza interfejs IA, jest to błąd czasu kompilacji dla IA, który jest zależny od IB. Interfejs **bezpośrednio zależy** od jego bezpośrednich interfejsów podstawowych (jeśli istnieje) i **bezpośrednio zależy** od typu, w którym jest natychmiast zagnieżdżony (jeśli istnieje).

Uwzględniając te definicje, kompletny zestaw **typów** , od którego zależy typ, jest zwrotne i przechodnie zamknięcie **bezpośrednie zależy** od relacji.

### <a name="effect-on-existing-programs"></a>Wpływ na istniejące programy

Przedstawione tutaj reguły nie mają wpływu na znaczenie istniejących programów.

Przykład 1:

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

Przykład 2:

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

Te same reguły dają podobne wyniki do analogicznej sytuacji obejmującej domyślne metody interfejsu:

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> ***Problem zamknięty***: Upewnij się, że jest to zamierzenie zgodne ze specyfikacją. **Decyzja: tak**

### <a name="runtime-method-resolution"></a>Rozdzielczość metody środowiska uruchomieniowego

> ***Zamknięty problem:*** Specyfikacja powinna opisywać algorytm rozpoznawania metody środowiska uruchomieniowego w miarę metod domyślnych interfejsu. Musimy upewnić się, że semantyka jest spójna z semantyką języka, na przykład, które zadeklarowane metody nie zastępują ani nie implementują metody `internal`.

### <a name="clr-support-api"></a>Interfejs API obsługi CLR

Aby kompilatory wykrywają, kiedy są kompilowane dla środowiska uruchomieniowego, które obsługuje tę funkcję, biblioteki dla takich środowisk uruchomieniowych są modyfikowane w celu anonsowania tego faktu przez interfejs API omówiony w <https://github.com/dotnet/corefx/issues/17116>. Dodamy

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> ***Problem otwarty***: czy Najlepsza nazwa funkcji *CLR* ? Funkcja CLR wykonuje znacznie więcej niż tylko ten (np. ogranicza ograniczenia ochrony, obsługuje zastąpień w interfejsach itp.). Być może powinna być wywoływana podobnie jak "konkretne metody w interfejsie" lub "cechy"?

### <a name="further-areas-to-be-specified"></a>Dalsze obszary do określenia

- [] Warto wykazać rodzaje źródłowych i binarnych efektów zgodności spowodowanych przez dodawanie domyślnych metod interfejsu i zastąpień do istniejących interfejsów.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Ta propozycja wymaga skoordynowanej aktualizacji specyfikacji CLR (aby można było obsługiwać konkretne metody w interfejsach i rozpoznawaniu metod). Jest to dość stosunkowo "kosztowne" i może być warto w połączeniu z innymi funkcjami, które również przewidujemy, że zmiany środowiska CLR będą wymagały zmian.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Brak.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

- Otwarte pytania są wywoływane w całej propozycji powyżej.
- Aby uzyskać listę otwartych pytań, zobacz również <https://github.com/dotnet/csharplang/issues/406>.
- Szczegółowa Specyfikacja musi opisywać mechanizm rozpoznawania używany w czasie wykonywania w celu wybrania precyzyjnej metody do wywołania.
- Interakcje metadanych tworzonych przez nowe kompilatory i zużywane przez starsze kompilatory muszą być szczegółowo opracowywane. Na przykład musimy upewnić się, że reprezentacja metadanych, której używamy, nie powoduje dodania domyślnej implementacji w interfejsie, aby przerwać istniejącą klasę, która implementuje ten interfejs w przypadku skompilowania przez starszy kompilator. Może to mieć wpływ na reprezentację metadanych, której można użyć.
- Projekt musi uwzględniać współdziałanie z innymi językami i istniejącymi kompilatorami dla innych języków.

## <a name="resolved-questions"></a>Rozwiązane pytania

### <a name="abstract-override"></a>Przesłonięcie abstrakcyjne

Wcześniejsza Specyfikacja robocza zawiera możliwość "reabstract" dziedziczonej metody:

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

Moje uwagi dla 2017-03-20 wykazały, że nie postanowiły tego zezwolenia. Jednak istnieją co najmniej dwa przypadki użycia:

1. Interfejsy API języka Java, z którymi niektórzy użytkownicy tej funkcji mają nadzieję, że zależą od tej możliwości.
2. Programowanie z *cechami* korzyści z tego rozwiązania. Reabstraction jest jednym z elementów funkcji języka "cechy" (https://en.wikipedia.org/wiki/Trait_(computer_programming)). W przypadku klas są dozwolone następujące elementy:

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

Niestety, ten kod nie może być refaktoryzacją jako zestaw interfejsów (cech), chyba że jest to dozwolone. Zgodnie z *regułą Jared Greed*, powinno być dozwolone.

> ***Zamknięty problem:*** Czy powinno być dozwolone przedzielenie? OPCJĘ Moje notatki były nieprawidłowe. W [informacjach](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) o programie LDM należy powiedzieć, że w interfejsie jest dozwolone reabstrakcja. Nie w klasie.

### <a name="virtual-modifier-vs-sealed-modifier"></a>Modyfikator wirtualny vs Sealed

Z [Aleksey Tsingauz](https://github.com/AlekseyTs):

> Postanowiono zezwolić na jawne określenie modyfikatorów w elementach członkowskich interfejsu, chyba że istnieje powód, aby nie zezwalać na niektóre z nich. Pozwala to na interesujące pytanie wokół modyfikatora wirtualnego. Czy powinna być wymagana dla członków z implementacją domyślną?
>
> Możemy powiedzieć, że:
>
> - Jeśli nie ma żadnej implementacji i nie określono żadnych wirtualnych ani zapieczętowanych, Załóżmy, że element członkowski jest abstrakcyjny.
> - Jeśli istnieje implementacja i nie określono elementu abstract ani zapieczętowanego, Załóżmy, że element członkowski jest wirtualny.
> - modyfikator zapieczętowany jest wymagany do, aby metoda nie była wirtualna ani abstrakcyjna.
>
> Alternatywnie możemy powiedzieć, że modyfikator wirtualny jest wymagany dla wirtualnego elementu członkowskiego. Oznacza to, że jeśli istnieje członek z implementacją, która nie jest jawnie oznaczona modyfikatorem wirtualnym, nie jest to wirtualne ani abstrakcyjne. Takie podejście może zapewnić lepszą wydajność, gdy metoda jest przenoszona z klasy do interfejsu:
>
> - metoda abstrakcyjna pozostaje abstrakcyjna.
> - Metoda wirtualna pozostaje wirtualna.
> - Metoda bez modyfikatora nie pozostaje ani wirtualna, ani abstrakcyjna.
> - modyfikator zapieczętowany nie może zostać zastosowany do metody, która nie jest przesłonięciem.
>
> Co myślisz?

> ***Zamknięty problem:*** Czy konkretna metoda (z implementacją) ma być niejawnie `virtual`? OPCJĘ

***Decyzje:*** Wprowadzone w programie LDM 2017-04-05:

1. niewirtualna powinna być jawnie wyrażona za poorednictwem `sealed` lub `private`.
2. `sealed` jest słowem kluczowym, aby utworzyć elementy członkowskie wystąpienia interfejsu z niewirtualnymi jednostkami
3. Chcemy zezwolić na wszystkie Modyfikatory w interfejsach  
4. Domyślna dostępność dla członków interfejsu jest publiczna, w tym typy zagnieżdżone
5. prywatne elementy członkowskie funkcji w interfejsach są niejawnie zapieczętowane, a `sealed` nie są na nich dozwolone.
6. Klasy prywatne (w interfejsach) są dozwolone i mogą być zapieczętowane, co oznacza zapieczętowany w klasie Sense zapieczętowany.
7. Nieobecny wniosek, część częściowa nadal nie jest dozwolona w przypadku interfejsów lub ich członków.

### <a name="binary-compatibility-1"></a>Zgodność binarna 1

Gdy biblioteka zawiera implementację domyślną

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

Rozumiemy, że implementacja `I1.M` w `C` jest `I1.M`. Co zrobić, jeśli zestaw zawierający `I2` został zmieniony w następujący sposób i ponownie skompilowany

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

ale `C` nie jest ponownie kompilowana. Co się stanie po uruchomieniu programu? Wywołanie `(C as I1).M()`

1. Działa `I1.M`
2. Działa `I2.M`
3. Zgłasza jakiś rodzaj błędu czasu wykonywania

***Decyzja:*** 2017-04-11: działa `I2.M`, co jest jednoznacznym bardziej szczegółowym przesłonięciem w czasie wykonywania.

### <a name="event-accessors-closed"></a>Metody dostępu zdarzeń (zamknięte)

> ***Zamknięty problem:*** Czy można zastąpić zdarzenie "rozkład elementowy"?

Rozważmy ten przypadek:

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

Ta "częściowa" implementacja zdarzenia nie jest dozwolona, ponieważ, jak w klasie, Składnia deklaracji zdarzenia nie zezwala na tylko jeden akcesor; należy podać oba (lub nie). Można to zrobić w ten sam sposób, zezwalając na abstrakcyjną metodę usuwania w składni, aby była niejawnie abstrakcyjna przez nieobecność treści:

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

Należy zauważyć, że *jest to nowa (proponowana) składnia*. W bieżącej gramatyki metody dostępu do zdarzeń mają obowiązkową treść.

> ***Zamknięty problem:*** Czy metoda dostępu do zdarzenia jest abstrakcyjna (niejawnie) przez pominięcie treści, podobnie jak metody w interfejsach i Akcesory właściwości są abstrakcyjne (niejawnie) przez pominięcie treści?

***Decyzja:*** (2017-04-18) nie, deklaracje zdarzeń wymagają zarówno konkretnych metod dostępu, jak i żadnego z nich.

### <a name="reabstraction-in-a-class-closed"></a>Reabstract w klasie (zamknięty)

***Zamknięty problem:*** Należy upewnić się, że jest to dozwolone (w przeciwnym razie dodanie domyślnej implementacji będzie istotną zmianą):

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

***Decyzja:*** (2017-04-18) tak, dodanie treści do deklaracji elementu członkowskiego interfejsu nie powinno powodować podzielenia dysku C.

### <a name="sealed-override-closed"></a>Zapieczętowane przesłonięcie (zamknięte)

W powyższym pytaniu niejawnie przyjęto, że modyfikator `sealed` można zastosować do `override` w interfejsie. Jest to sprzeczne ze specyfikacją wersji roboczej. Czy chcemy zezwolić na zastępowanie zastąpień? Należy wziąć pod uwagę źródłową i binarną efekty zgodności zamknięć.

> ***Zamknięty problem:*** Czy zezwalamy na zastępowanie zastąpień?

***Decyzja:*** (2017-04-18) nie można `sealed` na zastąpień w interfejsach. Jedynym zastosowaniem `sealed` w składowych interfejsu jest to, aby były one niewirtualne w swojej wstępnej deklaracji.

### <a name="diamond-inheritance-and-classes-closed"></a>Dziedziczenie i klasy rombów (zamknięte)

Wersja robocza propozycji preferuje zastąpienia klas przesłania do interfejsów w scenariuszach dziedziczenia rombu:

> Firma Microsoft wymaga, aby każdy interfejs i Klasa miały *najbardziej specyficzne przesłonięcia* dla każdej metody interfejsu w zastąpieniach występujących w typie lub jego bezpośrednich i pośrednich interfejsach. *Najbardziej konkretny przesłonięcie* jest unikatowym przesłonięciem, który jest bardziej szczegółowy niż każdy inny przesłonięcie. Jeśli nie ma żadnych zastąpień, sama metoda jest uznawana za najbardziej konkretne przesłonięcie.
>
> `M1` przesłonięcia jest uznawana za *bardziej precyzyjną* niż inne `M2` przesłaniania, jeśli `M1` jest zadeklarowana dla typu `T1`, `M2` jest zadeklarowana dla typu `T2`i albo
>
> 1. `T1` zawiera `T2` między interfejsami bezpośrednimi lub pośrednimi lub
> 2. `T2` jest typem interfejsu, ale `T1` nie jest typem interfejsu.

Ten scenariusz jest

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

Należy potwierdzić to zachowanie (lub wybrać inny sposób)

> ***Zamknięty problem:*** Potwierdź wersję roboczą powyżej, aby uzyskać *najbardziej szczegółowe przesłonięcie* , ponieważ dotyczy ona klas i interfejsów mieszanych (Klasa ma priorytet nad interfejsem). Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.

### <a name="interface-methods-vs-structs-closed"></a>Metody interfejsu a struktury (zamknięte)

Istnieją między innymi interakcje między domyślnymi metodami interfejsu i strukturami.

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

Należy zauważyć, że elementy członkowskie interfejsu nie są dziedziczone:

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

W związku z tym klient musi mieć pole struktury, aby wywołać metody interfejsu

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

Opakowanie w ten sposób obniża główne korzyści typu `struct`. Ponadto wszelkie metody mutacji nie będą miały żadnego efektu, ponieważ działają na *kopii w ramce* struktury:

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> ***Zamknięty problem:*** Co można zrobić:
>
> 1. Zabroń `struct` dziedziczyć implementację domyślną. Wszystkie metody interfejsu byłyby traktowane jako abstrakcyjne w `struct`. Następnie może upłynąć trochę czasu, aby zdecydować, jak lepiej ją ulepszyć.
> 2. Weź pod pewnym rodzaj strategii generowania kodu, który unika pakowania. Wewnątrz metody, takiej jak `IB.Increment`, typ `this` może być zbliżone do parametru typu ograniczonego do `IB`. W połączeniu z tym, aby uniknąć pakowania w obiekt wywołujący, metody nieabstrakcyjne byłyby dziedziczone z interfejsów. Może to spowodować znaczne zwiększenie działania kompilatora i środowiska CLR.
> 3. Nie martw się o nią i po prostu pozostaw ją jako wartą.
> 4. Inne pomysły?

***Decyzja:*** Nie martw się o nią i po prostu pozostaw ją jako wartą. Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.

### <a name="base-interface-invocations-closed"></a>Wywołania interfejsu podstawowego (zamknięte)

Specyfikacja wersja robocza sugeruje składnię dla wywołań interfejsu podstawowego inspirowanych przez język Java: `Interface.base.M()`. Musimy wybrać składnię co najmniej dla początkowego prototypu. Moją ulubioną `base<Interface>.M()`.

> ***Zamknięty problem:*** Jaka jest składnia podstawowego wywołania elementu członkowskiego?

***Decyzja:*** Składnia jest `base(Interface).M()`. Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>. Interfejs tak nazwany musi być interfejsem podstawowym, ale nie musi być bezpośrednim interfejsem podstawowym.

> ***Problem otwarty:*** Czy w składowych klasy są dozwolone wywołania interfejsu podstawowego?

***Decyzja***: tak. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a>Zastępowanie niepublicznych członków interfejsu (zamknięty)

W interfejsie niepubliczne składowe z interfejsów podstawowych są zastępowane przy użyciu modyfikatora `override`. Jeśli jest to przesłonięcie jawne, które nazywa interfejs zawierający element członkowski, modyfikator dostępu zostanie pominięty.

> ***Zamknięty problem:*** Jeśli jest to przesłonięcie "niejawne", które nie ma nazwy interfejsu, czy modyfikator dostępu musi być zgodny?

***Decyzja:*** Tylko publiczne elementy członkowskie mogą zostać przesłonięte niejawnie, a dostęp musi być zgodny. Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.

> ***Problem otwarty:*** Czy modyfikator dostępu jest wymagany, opcjonalny lub pomijany w jawnym przesłonięciu, takim jak `override void IB.M() {}`?

> ***Problem otwarty:*** Czy `override` jest wymagane, opcjonalne lub pomijane w jawnym przesłonięciu, takim jak `void IB.M() {}`?

Jak jeden implementuje niepubliczny element członkowski interfejsu w klasie? Być może należy ją wykonać jawnie?

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> ***Zamknięty problem:*** Jak jeden implementuje niepubliczny element członkowski interfejsu w klasie?

***Decyzja:*** Niepubliczne elementy członkowskie interfejsu można zaimplementować jawnie. Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.

***Decyzja***: w składowych interfejsu nie jest dozwolone słowo kluczowe `override`. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a>Zgodność binarna 2 (zamknięty)

Rozważmy następujący kod, w którym każdy typ znajduje się w osobnym zestawie

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

Rozumiemy, że implementacja `I1.M` w `C` jest `I2.M`. Co zrobić, jeśli zestaw zawierający `I3` został zmieniony w następujący sposób i ponownie skompilowany

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

ale `C` nie jest ponownie kompilowana. Co się stanie po uruchomieniu programu? Wywołanie `(C as I1).M()`

1. Działa `I1.M`
2. Działa `I2.M`
3. Działa `I3.M`
4. Wartość 2 lub 3, deterministycznie
5. Zgłasza jakiś rodzaj wyjątku czasu wykonywania

***Decyzja***: Zgłoś wyjątek (5). Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.

### <a name="permit-partial-in-interface-closed"></a>Zezwolić na `partial` w interfejsie? napis

Mając na względzie, że interfejsy mogą być używane w sposób analogiczny do sposobu używania klas abstrakcyjnych, warto zadeklarować je `partial`. Jest to szczególnie przydatne w przypadku generatorów.

> ***Propozycja:*** Usuń ograniczenie języka, które interfejsy i elementy członkowskie interfejsów nie mogą być deklarowane `partial`.

***Decyzja***: tak. Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.

### <a name="main-in-an-interface-closed"></a>`Main` w interfejsie? napis

> ***Problem otwarty:*** Czy `static Main` w interfejsie kandydatem może być punkt wejścia programu?

***Decyzja***: tak. Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a>Potwierdź zamiar obsługi publicznych metod niewirtualnych (zamknięty)

Czy możemy potwierdzić (lub odwrócić) decyzję, aby zezwolić na niewirtualne metody publiczne w interfejsie?

```csharp
interface IA
{
    public sealed void M() { }
}
```

> ***Problem częściowo zamknięty:*** (2017-04-18) uważamy, że będzie on przydatny, ale powróci do niego. Jest to blok do wyzwolenia modelu psychicznego.

***Decyzja***: tak. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a>Czy `override` w interfejsie wprowadza nowy element członkowski? napis

Istnieje kilka sposobów, aby sprawdzić, czy deklaracja przesłonięcia wprowadza nowy element członkowski, czy nie.

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> ***Problem otwarty:*** Czy deklaracja przesłonięcia w interfejsie wprowadza nowy element członkowski? napis

W klasie Metoda zastępująca ma wartość "Visible" w niektórych przypadkach. Na przykład nazwy jego parametrów mają pierwszeństwo przed nazwami parametrów w zastąpionej metodzie. Może istnieć możliwość duplikowania tego zachowania w interfejsach, ponieważ zawsze jest określone przesłonięcie. Ale chcesz zduplikować to zachowanie?

Ponadto możliwe jest "przesłonięcie" metody przesłaniania? [Moot]

***Decyzja***: w składowych interfejsu nie jest dozwolone słowo kluczowe `override`. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.

### <a name="properties-with-a-private-accessor-closed"></a>Właściwości z prywatnym akcesorem (zamknięty)

Załóżmy, że prywatne elementy członkowskie nie są wirtualne, a kombinacja wirtualnych i prywatnych jest niedozwolona. Ale na czym polega właściwość z prywatnym akcesorem?

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

Czy jest to dozwolone? Czy metoda dostępu do `set` jest tutaj `virtual` lub nie? Czy można go zastąpić, gdzie jest dostępny? Czy następujące niejawnie implementuje tylko metodę dostępu `get`?

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

Jest najprawdopodobniej przyczyną błędu, ponieważ IA. Zestaw P. set nie jest wirtualny, a także ponieważ nie jest dostępny?

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

***Decyzja***: pierwszy przykład wygląda prawidłowo, a ostatni nie. Jest to rozwiązanie podobne do tego, jak już działa C#. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a>Wywołania interfejsu podstawowego, zaokrąglone 2 (zamknięte)

Nasze poprzednie "rozwiązanie" do obsługi wywołań podstawowych nie zapewnia wystarczającej wyrazistości. Powoduje to, że w C# programie i CLR, w przeciwieństwie do języka Java, należy określić interfejs zawierający deklarację metody i lokalizację implementacji, która ma zostać wywołana.

Proponuję następującą składnię dla wywołań podstawowych w interfejsach. Nie podoba mi się, ale ilustruje to, co każda składnia musi być w stanie wyrazić:

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

Jeśli nie ma niejednoznaczności, możesz napisać ją dokładniej

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

Lub

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

Lub

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

***Decyzja***: podjęto decyzję o `base(N.I1<T>).M(s)`, co oznacza, że jeśli mamy powiązanie z wywołaniem, w dalszej części tego problemu może wystąpić problem. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a>Ostrzeżenie dla struktury nie implementującej metody domyślnej? napis

@vancem potwierdzenia, że firma Microsoft powinna poważnie rozważyć wygenerowanie ostrzeżenia, jeśli deklaracja typu wartości nie przesłania niektórych metod interfejsu, nawet jeśli dziedziczy implementację tej metody z interfejsu. Ponieważ powoduje to zapakowanie i podłączenie ograniczonych wywołań.

***Decyzja***: Wygląda na to coś bardziej dopasowanego do analizatora. Wydaje się również, że to ostrzeżenie może być spowodowane zakłóceniami, ponieważ mogłoby być uruchamiane nawet wtedy, gdy domyślna metoda interfejsu nigdy nie zostanie wywołana i nie nastąpi żadne opakowanie. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a>Konstruktory statyczne interfejsu (zamknięte)

Kiedy są uruchamiane konstruktory statyczne interfejsów?  Bieżąca wersja robocza interfejsu wiersza polecenia jest proponowana podczas uzyskiwania dostępu do pierwszej statycznej metody lub pola. Jeśli nie ma żadnego z tych elementów, może to oznaczać, że nigdy nie będzie można go uruchomić?

[2018-10-09 zespół CLR proponuje "przechodzenie do dublowanych elementów ValueType (Sprawdź dostęp do każdej metody wystąpienia)"]

***Decyzja***: konstruktory statyczne są również uruchamiane na wpisach w metodach wystąpień, jeśli Konstruktor statyczny nie został `beforefieldinit`, w którym przypadki konstruktory statyczne są uruchamiane przed dostępem do pierwszego pola statycznego. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a>Spotkania projektowe

[2017-03-08. uwagi dotyczące spotkań z poprawkami programu ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
2017-03-21. [uwagi dotyczące spotkań](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) programu LDM
[2017-03-23 spotkanie "środowisko CLR dla domyślnych metod interfejsu"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md) , [2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
2017-04-05 rozwiązania do [spotkań](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md) z [poprawkami LDM
](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md) [2017-04-11](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md) } [uwagi
2017-04-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) [2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md) [ Notatki dotyczące spotkania](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md) [2018-11-14](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)
Meeting




