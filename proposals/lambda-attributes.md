---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484574"
---
# <a name="lambda-attributes"></a>Atrybuty lambda

* [x] proponowane
* [] Prototyp
* [] Implementacja
* [] — Specyfikacja

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Zezwalaj na stosowanie atrybutów do wyrażeń lambda (i metod anonimowych) oraz do parametrów metody lambda/anonimowej, ponieważ mogą one być stosowane w zwykłych metodach.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Dwie podstawowe motywacje:

1. Aby zapewnić widoczność metadanych dla analizatorów w czasie kompilacji.
2. Aby zapewnić, że metadane są widoczne dla odbicia i narzędzi w czasie wykonywania.

Przykładowo (1): w przypadku kodu wrażliwego na wydajność warto mieć możliwość zdefiniowania analizatora, który flaguje, gdy zamknięcia i Delegaty są przydzieleni dla wyrażeń lambda, które są blisko stanu.  Często deweloper tego kodu będzie mógł uniknąć przechwytywania jakiegokolwiek stanu, aby kompilator mógł wygenerować metodę statyczną i delegatem pamięci podręcznej dla tej metody, lub deweloper upewni się, że jedynym zamykanym stanem jest `this`, co pozwala kompilatorowi co najmniej uniknąć przydziału obiektu zamknięcia.  Jednak bez obsługi języka w celu ograniczania tego, co może być przechwytywane, jest to, że wszystko jest zbyt łatwe do odróżnienia od stanu.  Byłoby to przydatne, jeśli deweloper może dodawać adnotacje lambda z atrybutami, aby wskazać, jaki stan można zamknąć, na przykład:

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

Następnie można napisać Analizator do flagi, gdy stan jest przechwytywany nieprawidłowo, na przykład:

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

- Używając takiej samej składni atrybutu jak w przypadku normalnych metod, atrybuty mogą być stosowane na początku metody lambda lub anonimowej, na przykład:

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- Aby uniknąć niejednoznaczności w przypadku, gdy atrybut ma zastosowanie do metody lambda lub do jednego z argumentów, atrybuty mogą być używane tylko wtedy, gdy parens są używane wokół dowolnych argumentów, na przykład:

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- Przy użyciu metod anonimowych parens nie są konieczne w celu zastosowania atrybutu do metody przed słowem kluczowym `delegate`, na przykład:

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- Można zastosować wiele atrybutów, używając standardowej składni rozdzielanej przecinkami lub składni pełnego atrybutu, na przykład:

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- Atrybuty mogą być stosowane do parametrów metody anonimowej lub wyrażenia lambda, ale tylko wtedy, gdy parens są używane wokół dowolnych argumentów, na przykład:

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- Wiele atrybutów można zastosować do parametrów metody anonimowej lub wyrażenia lambda przy użyciu składni rozdzielanej przecinkami lub pełnego atrybutu, na przykład:

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- atrybuty dostosowane do `return`mogą być również używane w wyrażeniach lambda, na przykład:

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- Kompilator wyprowadza atrybuty do wygenerowanej metody i argumentów do tych metod, tak jak w przypadku innych metod.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Nie dotyczy

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Nie dotyczy

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

Nie dotyczy

## <a name="design-meetings"></a>Spotkania projektowe

Nie dotyczy