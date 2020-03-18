---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484658"
---
# <a name="pattern-based-fixed-statement"></a>Instrukcja `fixed` na podstawie wzorca

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Wprowadź wzorzec, który zezwoli, aby typy uczestniczyły w instrukcji `fixed`. 

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Język zapewnia mechanizm przypinania zarządzanych danych i uzyskiwania natywnego wskaźnika do bazowego buforu.

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

Zestaw typów, które mogą uczestniczyć w `fixed` jest stałe i ograniczony do tablic i `System.String`. Typy "Special" zakodowana nie są skalowane, gdy są wprowadzane nowe elementy podstawowe, takie jak `ImmutableArray<T>`, `Span<T>``Utf8String`. 

Ponadto bieżące rozwiązanie dla `System.String` opiera się na dość sztywnym interfejsie API. Kształt interfejsu API oznacza, że `System.String` jest obiektem ciągłym, który osadza dane zakodowane UTF16 w ustalonym przesunięciu z nagłówka obiektu. Takie podejście znalazło problemy w kilku propozycjach, które mogą wymagać zmian w układzie bazowym. Może być możliwe przełączenie się do bardziej elastycznego, który oddzieli `System.String` obiektu od jego wewnętrznej reprezentacji na potrzeby niezarządzanej międzyoperacyjności. 

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

## <a name="pattern"></a>*Wzorzec* ##
"Ustalony" "stały" oparty na wzorcu musi:
-   Podaj odwołania zarządzane, aby przypiąć wystąpienie i zainicjować wskaźnik (najlepiej to jest to samo odwołanie)
-   Przekazywanie jednoznacznie typu niezarządzanego elementu (tj. "char" dla "String")
-   Należy określić zachowanie w przypadku wartości pustej, jeśli nie ma niczego do odwołania. 
-   Nie powinno wypychania autorów interfejsu API do decyzji projektowych, które pogarszają użycie typu poza `fixed`.

Uważam, że powyższe może być spełnione poprzez rozpoznanie specjalnie nazwanego elementu członkowskiego zwracającego odwołanie: `ref [readonly] T GetPinnableReference()`.

Aby można było używać instrukcji `fixed`, muszą być spełnione następujące warunki:

1. Dla tego typu jest dostarczany tylko jeden element członkowski.
1. Zwraca przez `ref` lub `ref readonly`. (`readonly` jest dozwolone, aby autorzy typów niezmiennych/tylko do odczytu mogli zaimplementować wzorzec bez dodawania zapisywalnego interfejsu API, który może być używany w bezpiecznym kodzie)
1. T jest typem niezarządzanym.
(ponieważ `T*` jest typem wskaźnika. Ograniczenie zostanie naturalnie rozwinięte, jeśli/gdy pojęcie "niezarządzane" jest rozwinięte)
1. Zwraca zarządzane `nullptr`, gdy nie ma żadnych danych do przypięcia — prawdopodobnie najtańszym sposobem przekazania opróżniania.
(należy zauważyć, że "" ciąg zwraca odwołanie do ' \ 0 ', ponieważ ciągi są zakończone null)

Alternatywnie, `#3` możemy zezwolić na wynik w pustych przypadkach jako niezdefiniowane lub specyficzne dla implementacji. Jednak może to spowodować, że interfejs API będzie bardziej niebezpieczny i podatny na nadużycia i niezamierzone obciążenia zgodności. 

## <a name="translation"></a>*Tłumaczenie* ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

staną się następujące pseudokodzie (nie wszystkie można wyrazić elementu w C#)

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

- GetPinnableReference jest przeznaczona do użycia tylko w `fixed`, ale nic nie pozwala na korzystanie z niego w bezpiecznym kodzie, dlatego implementacja musi mieć na uwadze.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Użytkownicy mogą wprowadzać GetPinnableReference lub podobną składową i używać jej jako
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

Nie ma rozwiązania dla `System.String` w przypadku pożądanego rozwiązania alternatywnego.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

- [] Zachowanie w stanie "pusty".`nullptr`  - lub `undefined`? 
- [] Czy należy uwzględnić metody rozszerzające? 
- [] Jeśli w `System.String`zostanie wykryty wzorzec, czy powinien on zostać przegrany? 

## <a name="design-meetings"></a>Spotkania projektowe

Jeszcze nie. 
