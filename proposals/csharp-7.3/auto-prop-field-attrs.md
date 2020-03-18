---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484665"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a>Zaimplementowane w polu właściwości

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Ta funkcja umożliwia deweloperom stosowanie atrybutów bezpośrednio do pól zapasowych właściwości, które są implementowane.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Obecnie nie jest możliwe stosowanie atrybutów do pól zapasowych właściwości, które są implementowane.  W takich przypadkach, gdy Deweloper musi używać atrybutu określania wartości docelowej, są wymuszane deklarowanie pola ręcznie i używanie bardziej szczegółowej składni właściwości.  Uwzględniając, C# że ma zawsze obsługiwane atrybuty dotyczące pól w wygenerowanym polu tworzenia kopii zapasowych dla zdarzeń, ma sens, aby zwiększyć te same funkcje do ich właściwości kin.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Krótko mówiąc, następujące byłyby prawne C# i nie generują ostrzeżenia:

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

Spowoduje to zastosowanie atrybutów do pola zapasowego wygenerowanego przez kompilator:

```csharp
[Serializable]
public class Foo 
{
    [NonSerialized]
    private string _mySecretBackingField;
    
    public string MySecret
    {
        get { return _mySecretBackingField; }
        set { _mySecretBackingField = value; }
    }
}
```

Jak wspomniano, to zapewnia parzystość ze składnią zdarzenia z C# 1,0, ponieważ następujące informacje są już dozwolone i działają zgodnie z oczekiwaniami:

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Istnieją dwa potencjalne wady dotyczące implementowania tej zmiany:

1. Próba zastosowania atrybutu do pola właściwości, która jest zaimplementowana, powoduje ostrzeżenie kompilatora, że atrybuty w tym bloku zostaną zignorowane.  Jeśli kompilator został zmieniony tak, aby obsługiwał te atrybuty, zostaną one zastosowane do pola zapasowego podczas kolejnej kompilacji, która mogłaby zmienić zachowanie programu w czasie wykonywania.
1. Kompilator nie sprawdza obecnie AttributeUsage obiektów docelowych atrybutów podczas próby zastosowania ich do pola właściwości, która jest implementowana.  Jeśli kompilator został zmieniony tak, aby obsługiwał atrybuty do pól, a nie można zastosować danego atrybutu do pola, kompilator emituje błąd zamiast ostrzeżenia, przerywając kompilację.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Spotkania projektowe
