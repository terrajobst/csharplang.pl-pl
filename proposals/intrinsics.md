---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484567"
---
# <a name="compiler-intrinsics"></a>Funkcje wewnętrzne kompilatora

## <a name="summary"></a>Podsumowanie

Ta propozycja zawiera konstrukcje języka, które uwidaczniają elementy kodów IL niskiego poziomu, które nie są obecnie dostępne efektywnie lub w ogóle: `ldftn`, `ldvirtftn`, `ldtoken` i `calli`. Te kody operacji niskiego poziomu mogą być ważne w kodzie o wysokiej wydajności, a deweloperzy potrzebują wydajnych metod uzyskiwania dostępu do nich.

## <a name="motivation"></a>Motywacją

Motywacje i tło tej funkcji opisano w następujących kwestiach (jak jest potencjalną implementacją funkcji): 

https://github.com/dotnet/csharplang/issues/191

Ta alternatywna propozycja projektu jest dostępna po przejrzeniu implementacji prototypu oryginalnej oferty przez @msjabby, a także użycia w całej znaczącej bazie kodu. Ten projekt został wykonany z istotnymi danymi wejściowymi z @mjsabby, @tmat i @jkotas.

## <a name="detailed-design"></a>Szczegółowy projekt 

### <a name="allow-address-of-to-target-methods"></a>Zezwalaj na adres metod docelowych

Grupy metod będą teraz dozwolone jako argumenty dla wyrażenia adresu. Typ takiego wyrażenia zostanie `void*`. 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

Nie istnieje żadna konwersja delegatów w tym miejscu jedynym mechanizmem filtrowania elementów członkowskich w grupie metod jest dostęp statyczny/wystąpienie. Jeśli nie można rozróżnić członków, wystąpi błąd w czasie kompilacji.

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

Wyrażenie AddressOf w tym kontekście zostanie zaimplementowane w następujący sposób:

- Ldftn: gdy metoda jest niewirtualna.
- ldvirtftn: Kiedy metoda jest wirtualna.

Ograniczenia tej funkcji:

- Metody wystąpień można określić tylko w przypadku użycia wyrażenia wywołania dla wartości
- Funkcji lokalnych nie można używać w `&`. Szczegóły implementacji tych metod celowo nie są określone przez język. Dotyczy to również tego, czy są to elementy statyczne a wystąpienia, czy dokładnie do których podpisu są emitowane.

### <a name="handleof"></a>handleof

`handleof` kontekstowe słowo kluczowe przetłumaczy pole, element członkowski lub typ na odpowiedni typ `RuntimeHandle` za pomocą instrukcji `ldtoken`. Dokładny typ wyrażenia będzie zależeć od rodzaju nazwy w `handleof`:

- pole: `RuntimeFieldHandle`
- Typ: `RuntimeTypeHandle`
- Metoda: `RuntimeMethodHandle`

Argumenty do `handleof` są identyczne z `nameof`. Musi to być prosta nazwa, kwalifikowana nazwa, dostęp do składowej, dostęp podstawowy do określonego elementu członkowskiego lub ten dostęp do określonego elementu członkowskiego. Wyrażenie argumentu identyfikuje definicję kodu, ale nigdy nie jest oceniane.

Wyrażenie `handleof` jest oceniane w czasie wykonywania i ma zwracany typ `RuntimeHandle`. Można to zrobić w bezpiecznym kodzie, a także w przypadku niebezpiecznego. 

``` 
RuntimeHandle stringHandle = handleof(string);
```

Ograniczenia tej funkcji:

- Właściwości nie można używać w wyrażeniu `handleof`.
- Nie można użyć wyrażenia `handleof`, jeśli istnieje nazwa `handleof` istniejąca w zakresie. Na przykład typ, przestrzeń nazw itp...

### <a name="calli"></a>Calli

Kompilator doda obsługę nowego typu funkcji `extern`, która efektywnie przekształci się w instrukcję `.calli`. Atrybut extern zostanie oznaczony atrybutem następującego kształtu:

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

Dzięki temu deweloperzy mogą definiować metody w następującej postaci:

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

Ograniczenia dotyczące metody, która ma zastosowany atrybut `CallIndirect`:

- Nie może mieć atrybutu `DllImport`.
- Nie może być ogólny.

## <a name="open-issues"></a>Otwarte problemy

### <a name="callingconvention"></a>CallingConvention

`CallIndirectAttribute` zaprojektowana używa `CallingConvention` Wyliczenie, które nie ma wpisu dla zarządzanych konwencji wywoływania. Wyliczenie należy rozszerzyć w celu uwzględnienia tej konwencji wywoływania lub atrybutu musi mieć inne podejście.

## <a name="considerations"></a>Zagadnienia do rozważenia

### <a name="disambiguating-method-groups"></a>Niejednoznaczność grup metod

Istnieje kilka dyskusji na temat funkcji, które mogłyby ułatwić odróżnienie grup metod, które przechodzą do wyrażenia adresu. Na przykład dodawanie elementów podpisu do składni:

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

Zostało odrzucone, ponieważ nie można było nawiązać istotnego przypadku, a w tym miejscu nie można envisioned prostej składni. Istnieje również stosunkowo proste obejście: proste Definiowanie innej metody, która jest niejednoznaczna i używa C# kodu do wywołania żądanej funkcji. 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

Jest to jeszcze prostsze, jeśli `static` funkcje lokalne wprowadzają język. Następnie obejście można zdefiniować w tej samej funkcji, która używała niejednoznacznego adresu operacji:

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a>LoadTypeTokenInt32

Oryginalna propozycja umożliwiająca ładowanie tokenów metadanych jako wartości `int` w czasie kompilacji. Zasadniczo mają `tokenof`, które mają te same argumenty co `handleof`, ale są oceniane w czasie kompilacji do stałej `int`. 

Zostało to odrzucone, ponieważ powoduje znaczący problem dotyczący ponownego zapisu IL (z którego korzysta program .NET). Takie odpisanie często manipuluje tabelami metadanych w sposób, który może unieważniać te wartości. Nie ma żadnego rozsądnego sposobu na aktualizację tych wartości, gdy są one przechowywane jako proste wartości `int`.

Podstawowym pomysłem posiadania nieprzezroczystego uchwytu dla wpisów metadanych będzie dalsza Eksploracja przez zespół środowiska uruchomieniowego. 

## <a name="future-considerations"></a>Zagadnienia w przyszłości

### <a name="static-local-functions"></a>statyczne funkcje lokalne

Odnosi się do [propozycji](https://github.com/dotnet/csharplang/issues/1565) , aby zezwolić na modyfikator `static` w funkcjach lokalnych. Takie funkcje byłyby gwarantowane jako `static` i z dokładnym podpisem określonym w kodzie źródłowym. Taka funkcja powinna być prawidłowym argumentem, aby `&`, ponieważ nie zawiera on żadnych problemów lokalnych.

### <a name="nativecallableattribute"></a>NativeCallableAttribute

Środowisko CLR zawiera funkcję, która umożliwia emitowanie zarządzanych metod w taki sposób, że są one bezpośrednio wywoływane z kodu natywnego. W tym celu należy dodać `NativeCallableAttribute` do metod. Taka metoda jest wywoływana tylko z kodu natywnego i dlatego musi zawierać tylko typy danych kopiowalnych w podpisie. Wywoływanie z kodu zarządzanego powoduje błąd w czasie wykonywania. 

Ta funkcja będzie dobrze wzórem z tą propozycją, ponieważ będzie to możliwe:

- Przekazywanie funkcji zdefiniowanej w kodzie zarządzanym do kodu natywnego jako wskaźnika funkcji (za pośrednictwem adresu) bez dodatkowych kosztów w kodzie zarządzanym i natywnym. 
- Środowisko uruchomieniowe może wprowadzić błędy witryny dla takich funkcji w kodzie zarządzanym, aby uniemożliwić ich wywoływanie w czasie kompilacji.




