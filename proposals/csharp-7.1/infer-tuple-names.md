---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484721"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a>Wywnioskowanie nazw krotek (alias. inicjatory projekcji krotki)

## <a name="summary"></a>Podsumowanie
[summary]: #summary

W wielu typowych przypadkach ta funkcja umożliwia pominięcie nazw elementów krotki i ich wywnioskowanie. Na przykład zamiast wpisywać `(f1: x.f1, f2: x?.f2)`, można wywnioskować nazwy elementów "F1" i "F2" z `(x.f1, x?.f2)`.

Powoduje to równoległe zachowanie typów anonimowych, które umożliwiają wnioskowanie nazw członków podczas tworzenia. Na przykład `new { x.f1, y?.f2 }` deklaruje członków "F1" i "F2".

Jest to szczególnie przydatne w przypadku używania krotek w LINQ:

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Istnieją dwie części zmiany:

1.  Spróbuj wnioskować o nazwę kandydującą dla każdego elementu krotki, który nie ma jawnej nazwy:
    -   Używanie tych samych reguł jako wnioskowania o nazwach dla typów anonimowych.
        - W C#programie można to zrobić w trzech przypadkach: `y` (identyfikator), `x.y` (prosty dostęp do elementów członkowskich) i `x?.y` (dostęp warunkowy).
        - W języku VB pozwala to na dodatkowe przypadki, takie jak `x.y()`.
    -   Odrzucanie zarezerwowanych nazw krotek (z uwzględnieniem wielkości liter w C#, bez uwzględniania wielkości liter w języku VB), ponieważ są one zabronione lub już niejawne. Na przykład takie jak `ItemN`, `Rest`i `ToString`.
    -   Jeśli nazwy kandydatów są zduplikowane (z uwzględnieniem wielkości liter C#w, w języku VB, bez uwzględniania wielkości liter), należy porzucić te kandydaci,
2.  Podczas konwersji (które sprawdzają i ostrzegają o upuszczaniu nazw z literałów krotek), wywnioskowane nazwy nie generują żadnych ostrzeżeń. Pozwala to uniknąć przerwania istniejącego kodu krotki.

Należy zauważyć, że reguła obsługi duplikatów jest inna niż w przypadku typów anonimowych. Na przykład `new { x.f1, x.f1 }` generuje błąd, ale nadal będzie można używać `(x.f1, x.f1)` (tylko bez nazw odroczonych). Pozwala to uniknąć przerwania istniejącego kodu krotki.

W celu zachowania spójności taka sama zostałaby zastosowana w przypadku krotek utworzonych przez dekonstrukcja przypisań (w C#):

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

To samo dotyczy krotek w języku VB, przy użyciu reguł specyficznych dla języka VB do wnioskowania nazwy z wyrażenia i nazwy bez uwzględniania wielkości liter.

W przypadku korzystania C# z kompilatora 7,1 (lub nowszego) z wersją językową "7,0" nazwy elementów zostaną wywnioskowane (bez dostępności funkcji), ale w celu uzyskania dostępu do nich wystąpi błąd użycia lokacji. Spowoduje to ograniczenie dodawania nowego kodu, który będzie później narażony na problem ze zgodnością (opisany poniżej).

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Główna wada polega na tym, że nadano przerwanie C# zgodności z 7,0:

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

Rada zgodności stwierdziła, że ten podział jest akceptowalny, biorąc pod względu na to, że jest ograniczony, C# i przedział czasu od momentu wysłania krotek (w 7,0).

## <a name="references"></a>Dokumentacja
- [LDM Kwiecień, czwarty 2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- [Dyskusja](https://github.com/dotnet/csharplang/issues/370) w witrynie GitHub (z podziękowaniami @alrz w celu wprowadzenia tego problemu)
- [Projekt krotek](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
