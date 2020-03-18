---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484665"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a><span data-ttu-id="804dc-101">Zaimplementowane w polu właściwości</span><span class="sxs-lookup"><span data-stu-id="804dc-101">Auto-Implemented Property Field-Targeted Attributes</span></span>

## <a name="summary"></a><span data-ttu-id="804dc-102">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="804dc-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="804dc-103">Ta funkcja umożliwia deweloperom stosowanie atrybutów bezpośrednio do pól zapasowych właściwości, które są implementowane.</span><span class="sxs-lookup"><span data-stu-id="804dc-103">This feature intends to allow developers to apply attributes directly to the backing fields of auto-implemented properties.</span></span>

## <a name="motivation"></a><span data-ttu-id="804dc-104">Motywacją</span><span class="sxs-lookup"><span data-stu-id="804dc-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="804dc-105">Obecnie nie jest możliwe stosowanie atrybutów do pól zapasowych właściwości, które są implementowane.</span><span class="sxs-lookup"><span data-stu-id="804dc-105">Currently it is not possible to apply attributes to the backing fields of auto-implemented properties.</span></span>  <span data-ttu-id="804dc-106">W takich przypadkach, gdy Deweloper musi używać atrybutu określania wartości docelowej, są wymuszane deklarowanie pola ręcznie i używanie bardziej szczegółowej składni właściwości.</span><span class="sxs-lookup"><span data-stu-id="804dc-106">In those cases where the developer must use a field-targeting attribute they are forced to declare the field manually and use the more verbose property syntax.</span></span>  <span data-ttu-id="804dc-107">Uwzględniając, C# że ma zawsze obsługiwane atrybuty dotyczące pól w wygenerowanym polu tworzenia kopii zapasowych dla zdarzeń, ma sens, aby zwiększyć te same funkcje do ich właściwości kin.</span><span class="sxs-lookup"><span data-stu-id="804dc-107">Given that C# has always supported field-targeted attributes on the generated backing field for events it makes sense to extend the same functionality to their property kin.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="804dc-108">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="804dc-108">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="804dc-109">Krótko mówiąc, następujące byłyby prawne C# i nie generują ostrzeżenia:</span><span class="sxs-lookup"><span data-stu-id="804dc-109">In short, the following would be legal C# and not produce a warning:</span></span>

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

<span data-ttu-id="804dc-110">Spowoduje to zastosowanie atrybutów do pola zapasowego wygenerowanego przez kompilator:</span><span class="sxs-lookup"><span data-stu-id="804dc-110">This would result in the field-targeted attributes being applied to the compiler-generated backing field:</span></span>

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

<span data-ttu-id="804dc-111">Jak wspomniano, to zapewnia parzystość ze składnią zdarzenia z C# 1,0, ponieważ następujące informacje są już dozwolone i działają zgodnie z oczekiwaniami:</span><span class="sxs-lookup"><span data-stu-id="804dc-111">As mentioned, this brings parity with event syntax from C# 1.0 as the following is already legal and behaves as expected:</span></span>

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a><span data-ttu-id="804dc-112">Wady</span><span class="sxs-lookup"><span data-stu-id="804dc-112">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="804dc-113">Istnieją dwa potencjalne wady dotyczące implementowania tej zmiany:</span><span class="sxs-lookup"><span data-stu-id="804dc-113">There are two potential drawbacks to implementing this change:</span></span>

1. <span data-ttu-id="804dc-114">Próba zastosowania atrybutu do pola właściwości, która jest zaimplementowana, powoduje ostrzeżenie kompilatora, że atrybuty w tym bloku zostaną zignorowane.</span><span class="sxs-lookup"><span data-stu-id="804dc-114">Attempting to apply an attribute to the field of an auto-implemented property produces a compiler warning that the attributes in that block will be ignored.</span></span>  <span data-ttu-id="804dc-115">Jeśli kompilator został zmieniony tak, aby obsługiwał te atrybuty, zostaną one zastosowane do pola zapasowego podczas kolejnej kompilacji, która mogłaby zmienić zachowanie programu w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="804dc-115">If the compiler were changed to support those attributes they would be applied to the backing field on a subsequent recompilation which could alter the behavior of the program at runtime.</span></span>
1. <span data-ttu-id="804dc-116">Kompilator nie sprawdza obecnie AttributeUsage obiektów docelowych atrybutów podczas próby zastosowania ich do pola właściwości, która jest implementowana.</span><span class="sxs-lookup"><span data-stu-id="804dc-116">The compiler does not currently validate the AttributeUsage targets of the attributes when attempting to apply them to the field of the auto-implemented property.</span></span>  <span data-ttu-id="804dc-117">Jeśli kompilator został zmieniony tak, aby obsługiwał atrybuty do pól, a nie można zastosować danego atrybutu do pola, kompilator emituje błąd zamiast ostrzeżenia, przerywając kompilację.</span><span class="sxs-lookup"><span data-stu-id="804dc-117">If the compiler were changed to support field-targeted attributes and the attribute in question cannot be applied to a field the compiler would emit an error instead of a warning, breaking the build.</span></span>

## <a name="alternatives"></a><span data-ttu-id="804dc-118">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="804dc-118">Alternatives</span></span>
[alternatives]: #alternatives

## <a name="unresolved-questions"></a><span data-ttu-id="804dc-119">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="804dc-119">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="804dc-120">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="804dc-120">Design meetings</span></span>
