---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/13/2020
ms.locfileid: "79485519"
---
# <a name="readonly-instance-members"></a>Elementy członkowskie wystąpień tylko do odczytu

Problem z championed: <https://github.com/dotnet/csharplang/issues/1710>

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Określ sposób, aby określić, że poszczególne elementy członkowskie wystąpienia w strukturze nie modyfikują stanu, w taki sam sposób, w jaki `readonly struct` określa, że żaden element członkowski wystąpienia nie modyfikuje stanu.

Warto zauważyć, że `readonly instance member`! = `pure instance member`. Element członkowski wystąpienia `pure` gwarantuje, że żaden stan nie zostanie zmodyfikowany. Element członkowski wystąpienia `readonly` gwarantuje tylko, że stan wystąpienia nie zostanie zmodyfikowany.

Wszystkie elementy członkowskie wystąpienia na `readonly struct` mogą być uznawane za niejawnie `readonly instance members`. Jawne `readonly instance members` zadeklarowane w strukturach, które nie są w trybie tylko do odczytu, działają w taki sam sposób. Na przykład nadal mogą tworzyć ukryte kopie, jeśli Wywołano element członkowski wystąpienia (w bieżącym wystąpieniu lub w polu wystąpienia), które było samo, a nie tylko do odczytu.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Obecnie użytkownicy mają możliwość tworzenia typów `readonly struct`, które kompilator wymusza, że wszystkie pola są tylko do odczytu (i przez rozszerzenie, które nie modyfikują stanu przez członków wystąpienia). Istnieje jednak kilka scenariuszy, w których masz istniejący interfejs API, który uwidacznia dostępne pola lub ma mieszany i niemutację elementów członkowskich. W tych okolicznościach nie można oznaczyć typu jako `readonly` (będzie to istotna zmiana).

Zwykle nie ma to znacznie wpływu, z wyjątkiem przypadków `in` parametrów. W przypadku `in` parametrów dla struktur nie należących do tylko do odczytu kompilator wykona kopię parametru dla każdego wywołania elementu członkowskiego wystąpienia, ponieważ nie może zagwarantować, że wywołanie nie modyfikuje stanu wewnętrznego. Może to prowadzić do wielu kopii i gorszenia ogólnej wydajności niż w przypadku, gdy tylko została przeniesiona bezpośrednio przez wartość. Aby zapoznać się z przykładem, zobacz ten kod w witrynie [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)

Niektóre inne scenariusze, w których mogą występować ukryte kopie, obejmują `static readonly fields` i `literals`. Jeśli są one obsługiwane w przyszłości, `blittable constants` będzie kończyć się w tej samej łodzie; oznacza to, że wszystkie obecnie wymagają pełnej kopii (w wywołaniu elementu członkowskiego wystąpienia), jeśli struktura nie jest oznaczona `readonly`.

## <a name="design"></a>Projekt
[design]: #design

Zezwalaj użytkownikowi na określenie, że element członkowski wystąpienia jest sam, `readonly` i nie modyfikuje stanu wystąpienia (ze wszystkimi odpowiednimi sprawdzeniami wykonanymi przez kompilator). Na przykład:

```csharp
public struct Vector2
{
    public float x;
    public float y;

    public readonly float GetLengthReadonly()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public float GetLength()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public readonly float GetLengthIllegal()
    {
        var tmp = MathF.Sqrt(LengthSquared);

        x = tmp;    // Compiler error, cannot write x
        y = tmp;    // Compiler error, cannot write y

        return tmp;
    }

    public float LengthSquared
    {
        readonly get
        {
            return (x * x) +
                   (y * y);
        }
    }
}

public static class MyClass
{
    public static float ExistingBehavior(in Vector2 vector)
    {
        // This code causes a hidden copy, the compiler effectively emits:
        //    var tmpVector = vector;
        //    return tmpVector.GetLength();
        //
        // This is done because the compiler doesn't know that `GetLength()`
        // won't mutate `vector`.

        return vector.GetLength();
    }

    public static float ReadonlyBehavior(in Vector2 vector)
    {
        // This code is emitted exactly as listed. There are no hidden
        // copies as the `readonly` modifier indicates that the method
        // won't mutate `vector`.

        return vector.GetLengthReadonly();
    }
}
```

Tylko do odczytu można zastosować do metod dostępu do właściwości, aby wskazać, że `this` nie zostanie mutacja w metodzie dostępu. W poniższych przykładach są dostępne tylko elementy ustawiające tylko do odczytu, ponieważ te metody dostępu modyfikują stan pola elementu członkowskiego, ale nie modyfikują wartości tego pola elementu członkowskiego.

```csharp
public int Prop1
{
    readonly get
    {
        return this._store["Prop1"];
    }
    readonly set
    {
        this._store["Prop1"] = value;
    }
}
```

Gdy `readonly` jest stosowany do składni właściwości, oznacza to, że wszystkie metody dostępu są `readonly`.

```csharp
public readonly int Prop2
{
    get
    {
        return this._store["Prop2"];
    }
    set
    {
        this._store["Prop2"] = value;
    }
}
```

Tylko do odczytu można stosować tylko do metod dostępu, które nie mutacją typu zawierającego.

```csharp
public int Prop3
{
    readonly get
    {
        return this._prop3;
    }
    set
    {
        this._prop3 = value;
    }
}
```

Tylko do odczytu można zastosować do niektórych właściwości, które są implementowane, ale nie ma znaczących efektów. Kompilator będzie traktować wszystkie zaimplementowane metody pobierające jako ReadOnly, niezależnie od tego, czy `readonly` słowo kluczowe jest obecne.

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

Tylko do odczytu można zastosować do zdarzeń implementowanych ręcznie, ale nie zdarzeń przypominających pola. Nie można zastosować tylko do odczytu do poszczególnych metod dostępu do zdarzeń (Dodaj/Usuń).

```csharp
// Allowed
public readonly event Action<EventArgs> Event1
{
    add { }
    remove { }
}

// Not allowed
public readonly event Action<EventArgs> Event2;
public event Action<EventArgs> Event3
{
    readonly add { }
    readonly remove { }
}
public static readonly event Event4
{
    add { }
    remove { }
}
```

Inne przykłady składni:

* Wyrażenia składowanych elementów członkowskich: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`
* Ograniczenia ogólne: `public static readonly void GenericMethod<T>(T value) where T : struct { }`

Kompilator emituje element członkowski wystąpienia, jak zwykle, i dodatkowo emituje rozpoznany atrybut kompilatora wskazujący, że element członkowski wystąpienia nie modyfikuje stanu. To efektywnie powoduje, że ukryty `this` parametr stanie się `in T` zamiast `ref T`.

Umożliwi to użytkownikowi bezpieczne wywoływanie wymienionej metody wystąpienia bez kompilatora, aby wykonać kopię.

Ograniczenia obejmują:

* Modyfikator `readonly` nie może zostać zastosowany do metod statycznych, konstruktorów ani destruktorów.
* Modyfikator `readonly` nie może zostać zastosowany do delegatów.
* Modyfikator `readonly` nie może zostać zastosowany do elementów członkowskich klasy lub interfejsu.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Obecnie te same wady, które istnieją w `readonly struct` metodach. Pewien kod może nadal spowodować ukrycie kopii.

## <a name="notes"></a>Uwagi
[notes]: #notes

Może być również możliwe użycie atrybutu lub innego słowa kluczowego.

Ta propozycja jest nieco powiązana z (ale jest bardziej podzbiorem) `functional purity` i/lub `constant expressions`, z których oba miały pewne istniejące wnioski.
