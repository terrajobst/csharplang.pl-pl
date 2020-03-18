---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/13/2020
ms.locfileid: "79485519"
---
# <a name="readonly-instance-members"></a><span data-ttu-id="35d2c-101">Elementy członkowskie wystąpień tylko do odczytu</span><span class="sxs-lookup"><span data-stu-id="35d2c-101">Readonly Instance Members</span></span>

<span data-ttu-id="35d2c-102">Problem z championed: <https://github.com/dotnet/csharplang/issues/1710></span><span class="sxs-lookup"><span data-stu-id="35d2c-102">Championed Issue: <https://github.com/dotnet/csharplang/issues/1710></span></span>

## <a name="summary"></a><span data-ttu-id="35d2c-103">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="35d2c-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="35d2c-104">Określ sposób, aby określić, że poszczególne elementy członkowskie wystąpienia w strukturze nie modyfikują stanu, w taki sam sposób, w jaki `readonly struct` określa, że żaden element członkowski wystąpienia nie modyfikuje stanu.</span><span class="sxs-lookup"><span data-stu-id="35d2c-104">Provide a way to specify individual instance members on a struct do not modify state, in the same way that `readonly struct` specifies no instance members modify state.</span></span>

<span data-ttu-id="35d2c-105">Warto zauważyć, że `readonly instance member`! = `pure instance member`.</span><span class="sxs-lookup"><span data-stu-id="35d2c-105">It is worth noting that `readonly instance member` != `pure instance member`.</span></span> <span data-ttu-id="35d2c-106">Element członkowski wystąpienia `pure` gwarantuje, że żaden stan nie zostanie zmodyfikowany.</span><span class="sxs-lookup"><span data-stu-id="35d2c-106">A `pure` instance member guarantees no state will be modified.</span></span> <span data-ttu-id="35d2c-107">Element członkowski wystąpienia `readonly` gwarantuje tylko, że stan wystąpienia nie zostanie zmodyfikowany.</span><span class="sxs-lookup"><span data-stu-id="35d2c-107">A `readonly` instance member only guarantees that instance state will not be modified.</span></span>

<span data-ttu-id="35d2c-108">Wszystkie elementy członkowskie wystąpienia na `readonly struct` mogą być uznawane za niejawnie `readonly instance members`.</span><span class="sxs-lookup"><span data-stu-id="35d2c-108">All instance members on a `readonly struct` could be considered implicitly `readonly instance members`.</span></span> <span data-ttu-id="35d2c-109">Jawne `readonly instance members` zadeklarowane w strukturach, które nie są w trybie tylko do odczytu, działają w taki sam sposób.</span><span class="sxs-lookup"><span data-stu-id="35d2c-109">Explicit `readonly instance members` declared on non-readonly structs would behave in the same manner.</span></span> <span data-ttu-id="35d2c-110">Na przykład nadal mogą tworzyć ukryte kopie, jeśli Wywołano element członkowski wystąpienia (w bieżącym wystąpieniu lub w polu wystąpienia), które było samo, a nie tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="35d2c-110">For example, they would still create hidden copies if you called an instance member (on the current instance or on a field of the instance) which was itself not-readonly.</span></span>

## <a name="motivation"></a><span data-ttu-id="35d2c-111">Motywacją</span><span class="sxs-lookup"><span data-stu-id="35d2c-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="35d2c-112">Obecnie użytkownicy mają możliwość tworzenia typów `readonly struct`, które kompilator wymusza, że wszystkie pola są tylko do odczytu (i przez rozszerzenie, które nie modyfikują stanu przez członków wystąpienia).</span><span class="sxs-lookup"><span data-stu-id="35d2c-112">Today, users have the ability to create `readonly struct` types which the compiler enforces that all fields are readonly (and by extension, that no instance members modify the state).</span></span> <span data-ttu-id="35d2c-113">Istnieje jednak kilka scenariuszy, w których masz istniejący interfejs API, który uwidacznia dostępne pola lub ma mieszany i niemutację elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="35d2c-113">However, there are some scenarios where you have an existing API that exposes accessible fields or that has a mix of mutating and non-mutating members.</span></span> <span data-ttu-id="35d2c-114">W tych okolicznościach nie można oznaczyć typu jako `readonly` (będzie to istotna zmiana).</span><span class="sxs-lookup"><span data-stu-id="35d2c-114">Under these circumstances, you cannot mark the type as `readonly` (it would be a breaking change).</span></span>

<span data-ttu-id="35d2c-115">Zwykle nie ma to znacznie wpływu, z wyjątkiem przypadków `in` parametrów.</span><span class="sxs-lookup"><span data-stu-id="35d2c-115">This normally doesn't have much impact, except in the case of `in` parameters.</span></span> <span data-ttu-id="35d2c-116">W przypadku `in` parametrów dla struktur nie należących do tylko do odczytu kompilator wykona kopię parametru dla każdego wywołania elementu członkowskiego wystąpienia, ponieważ nie może zagwarantować, że wywołanie nie modyfikuje stanu wewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="35d2c-116">With `in` parameters for non-readonly structs, the compiler will make a copy of the parameter for each instance member invocation, since it cannot guarantee that the invocation does not modify internal state.</span></span> <span data-ttu-id="35d2c-117">Może to prowadzić do wielu kopii i gorszenia ogólnej wydajności niż w przypadku, gdy tylko została przeniesiona bezpośrednio przez wartość.</span><span class="sxs-lookup"><span data-stu-id="35d2c-117">This can lead to a multitude of copies and worse overall performance than if you had just passed the struct directly by value.</span></span> <span data-ttu-id="35d2c-118">Aby zapoznać się z przykładem, zobacz ten kod w witrynie [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)</span><span class="sxs-lookup"><span data-stu-id="35d2c-118">For an example, see this code on [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)</span></span>

<span data-ttu-id="35d2c-119">Niektóre inne scenariusze, w których mogą występować ukryte kopie, obejmują `static readonly fields` i `literals`.</span><span class="sxs-lookup"><span data-stu-id="35d2c-119">Some other scenarios where hidden copies can occur include `static readonly fields` and `literals`.</span></span> <span data-ttu-id="35d2c-120">Jeśli są one obsługiwane w przyszłości, `blittable constants` będzie kończyć się w tej samej łodzie; oznacza to, że wszystkie obecnie wymagają pełnej kopii (w wywołaniu elementu członkowskiego wystąpienia), jeśli struktura nie jest oznaczona `readonly`.</span><span class="sxs-lookup"><span data-stu-id="35d2c-120">If they are supported in the future, `blittable constants` would end up in the same boat; that is they all currently necessitate a full copy (on instance member invocation) if the struct is not marked `readonly`.</span></span>

## <a name="design"></a><span data-ttu-id="35d2c-121">Projekt</span><span class="sxs-lookup"><span data-stu-id="35d2c-121">Design</span></span>
[design]: #design

<span data-ttu-id="35d2c-122">Zezwalaj użytkownikowi na określenie, że element członkowski wystąpienia jest sam, `readonly` i nie modyfikuje stanu wystąpienia (ze wszystkimi odpowiednimi sprawdzeniami wykonanymi przez kompilator).</span><span class="sxs-lookup"><span data-stu-id="35d2c-122">Allow a user to specify that an instance member is, itself, `readonly` and does not modify the state of the instance (with all the appropriate verification done by the compiler, of course).</span></span> <span data-ttu-id="35d2c-123">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="35d2c-123">For example:</span></span>

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

<span data-ttu-id="35d2c-124">Tylko do odczytu można zastosować do metod dostępu do właściwości, aby wskazać, że `this` nie zostanie mutacja w metodzie dostępu.</span><span class="sxs-lookup"><span data-stu-id="35d2c-124">Readonly can be applied to property accessors to indicate that `this` will not be mutated in the accessor.</span></span> <span data-ttu-id="35d2c-125">W poniższych przykładach są dostępne tylko elementy ustawiające tylko do odczytu, ponieważ te metody dostępu modyfikują stan pola elementu członkowskiego, ale nie modyfikują wartości tego pola elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="35d2c-125">The following examples have readonly setters because those accessors modify the state of member field, but do not modify the value of that member field.</span></span>

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

<span data-ttu-id="35d2c-126">Gdy `readonly` jest stosowany do składni właściwości, oznacza to, że wszystkie metody dostępu są `readonly`.</span><span class="sxs-lookup"><span data-stu-id="35d2c-126">When `readonly` is applied to the property syntax, it means that all accessors are `readonly`.</span></span>

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

<span data-ttu-id="35d2c-127">Tylko do odczytu można stosować tylko do metod dostępu, które nie mutacją typu zawierającego.</span><span class="sxs-lookup"><span data-stu-id="35d2c-127">Readonly can only be applied to accessors which do not mutate the containing type.</span></span>

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

<span data-ttu-id="35d2c-128">Tylko do odczytu można zastosować do niektórych właściwości, które są implementowane, ale nie ma znaczących efektów.</span><span class="sxs-lookup"><span data-stu-id="35d2c-128">Readonly can be applied to some auto-implemented properties, but it won't have a meaningful effect.</span></span> <span data-ttu-id="35d2c-129">Kompilator będzie traktować wszystkie zaimplementowane metody pobierające jako ReadOnly, niezależnie od tego, czy `readonly` słowo kluczowe jest obecne.</span><span class="sxs-lookup"><span data-stu-id="35d2c-129">The compiler will treat all auto-implemented getters as readonly whether or not the `readonly` keyword is present.</span></span>

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

<span data-ttu-id="35d2c-130">Tylko do odczytu można zastosować do zdarzeń implementowanych ręcznie, ale nie zdarzeń przypominających pola.</span><span class="sxs-lookup"><span data-stu-id="35d2c-130">Readonly can be applied to manually-implemented events, but not field-like events.</span></span> <span data-ttu-id="35d2c-131">Nie można zastosować tylko do odczytu do poszczególnych metod dostępu do zdarzeń (Dodaj/Usuń).</span><span class="sxs-lookup"><span data-stu-id="35d2c-131">Readonly cannot be applied to individual event accessors (add/remove).</span></span>

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

<span data-ttu-id="35d2c-132">Inne przykłady składni:</span><span class="sxs-lookup"><span data-stu-id="35d2c-132">Some other syntax examples:</span></span>

* <span data-ttu-id="35d2c-133">Wyrażenia składowanych elementów członkowskich: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span><span class="sxs-lookup"><span data-stu-id="35d2c-133">Expression bodied members: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span></span>
* <span data-ttu-id="35d2c-134">Ograniczenia ogólne: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span><span class="sxs-lookup"><span data-stu-id="35d2c-134">Generic constraints: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span></span>

<span data-ttu-id="35d2c-135">Kompilator emituje element członkowski wystąpienia, jak zwykle, i dodatkowo emituje rozpoznany atrybut kompilatora wskazujący, że element członkowski wystąpienia nie modyfikuje stanu.</span><span class="sxs-lookup"><span data-stu-id="35d2c-135">The compiler would emit the instance member, as usual, and would additionally emit a compiler recognized attribute indicating that the instance member does not modify state.</span></span> <span data-ttu-id="35d2c-136">To efektywnie powoduje, że ukryty `this` parametr stanie się `in T` zamiast `ref T`.</span><span class="sxs-lookup"><span data-stu-id="35d2c-136">This effectively causes the hidden `this` parameter to become `in T` instead of `ref T`.</span></span>

<span data-ttu-id="35d2c-137">Umożliwi to użytkownikowi bezpieczne wywoływanie wymienionej metody wystąpienia bez kompilatora, aby wykonać kopię.</span><span class="sxs-lookup"><span data-stu-id="35d2c-137">This would allow the user to safely call said instance method without the compiler needing to make a copy.</span></span>

<span data-ttu-id="35d2c-138">Ograniczenia obejmują:</span><span class="sxs-lookup"><span data-stu-id="35d2c-138">The restrictions would include:</span></span>

* <span data-ttu-id="35d2c-139">Modyfikator `readonly` nie może zostać zastosowany do metod statycznych, konstruktorów ani destruktorów.</span><span class="sxs-lookup"><span data-stu-id="35d2c-139">The `readonly` modifier cannot be applied to static methods, constructors or destructors.</span></span>
* <span data-ttu-id="35d2c-140">Modyfikator `readonly` nie może zostać zastosowany do delegatów.</span><span class="sxs-lookup"><span data-stu-id="35d2c-140">The `readonly` modifier cannot be applied to delegates.</span></span>
* <span data-ttu-id="35d2c-141">Modyfikator `readonly` nie może zostać zastosowany do elementów członkowskich klasy lub interfejsu.</span><span class="sxs-lookup"><span data-stu-id="35d2c-141">The `readonly` modifier cannot be applied to members of class or interface.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="35d2c-142">Wady</span><span class="sxs-lookup"><span data-stu-id="35d2c-142">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="35d2c-143">Obecnie te same wady, które istnieją w `readonly struct` metodach.</span><span class="sxs-lookup"><span data-stu-id="35d2c-143">Same drawbacks as exist with `readonly struct` methods today.</span></span> <span data-ttu-id="35d2c-144">Pewien kod może nadal spowodować ukrycie kopii.</span><span class="sxs-lookup"><span data-stu-id="35d2c-144">Certain code may still cause hidden copies.</span></span>

## <a name="notes"></a><span data-ttu-id="35d2c-145">Uwagi</span><span class="sxs-lookup"><span data-stu-id="35d2c-145">Notes</span></span>
[notes]: #notes

<span data-ttu-id="35d2c-146">Może być również możliwe użycie atrybutu lub innego słowa kluczowego.</span><span class="sxs-lookup"><span data-stu-id="35d2c-146">Using an attribute or another keyword may also be possible.</span></span>

<span data-ttu-id="35d2c-147">Ta propozycja jest nieco powiązana z (ale jest bardziej podzbiorem) `functional purity` i/lub `constant expressions`, z których oba miały pewne istniejące wnioski.</span><span class="sxs-lookup"><span data-stu-id="35d2c-147">This proposal is somewhat related to (but is more a subset of) `functional purity` and/or `constant expressions`, both of which have had some existing proposals.</span></span>
