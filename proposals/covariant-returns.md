---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484455"
---
# <a name="covariant-return-types"></a><span data-ttu-id="3fe56-101">typy zwracane przez współwarianty</span><span class="sxs-lookup"><span data-stu-id="3fe56-101">covariant return types</span></span>

* <span data-ttu-id="3fe56-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="3fe56-102">[x] Proposed</span></span>
* <span data-ttu-id="3fe56-103">[] Prototyp: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="3fe56-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="3fe56-104">[] Implementacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="3fe56-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="3fe56-105">[] Specyfikacja: nie uruchomiono</span><span class="sxs-lookup"><span data-stu-id="3fe56-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="3fe56-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="3fe56-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3fe56-107">Obsługa _typów zwracanych współwariantu_.</span><span class="sxs-lookup"><span data-stu-id="3fe56-107">Support _covariant return types_.</span></span> <span data-ttu-id="3fe56-108">W odniesieniu do metody przesłaniania można w specjalny sposób mieć bardziej pochodny typ referencyjny niż Metoda zastąpień.</span><span class="sxs-lookup"><span data-stu-id="3fe56-108">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span>

## <a name="motivation"></a><span data-ttu-id="3fe56-109">Motywacją</span><span class="sxs-lookup"><span data-stu-id="3fe56-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3fe56-110">Jest to typowy wzorzec w kodzie, który różne nazwy metod muszą być stworzone, aby obejść ograniczenia języka, które zastępują muszą zwracać ten sam typ co przesłoniętą metodę.</span><span class="sxs-lookup"><span data-stu-id="3fe56-110">It is a common pattern in code that different method names have to be invented to work around the language constraint that overrides must return the same type as the overridden method.</span></span> <span data-ttu-id="3fe56-111">Zobacz poniżej, aby zapoznać się z przykładem z bazy kodu Roslyn.</span><span class="sxs-lookup"><span data-stu-id="3fe56-111">See below for an example from the Roslyn code base.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="3fe56-112">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="3fe56-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="3fe56-113">Obsługa _typów zwracanych współwariantu_.</span><span class="sxs-lookup"><span data-stu-id="3fe56-113">Support _covariant return types_.</span></span> <span data-ttu-id="3fe56-114">W odniesieniu do metody przesłaniania można w specjalny sposób mieć bardziej pochodny typ referencyjny niż Metoda zastąpień.</span><span class="sxs-lookup"><span data-stu-id="3fe56-114">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span> <span data-ttu-id="3fe56-115">Dotyczy to metod i właściwości, które są obsługiwane w klasach i interfejsach.</span><span class="sxs-lookup"><span data-stu-id="3fe56-115">This would apply to methods and properties, and be supported in classes and interfaces.</span></span>

<span data-ttu-id="3fe56-116">Jest to przydatne w przypadku wzorca fabrycznego.</span><span class="sxs-lookup"><span data-stu-id="3fe56-116">This would be useful in the factory pattern.</span></span> <span data-ttu-id="3fe56-117">Na przykład, w bazie kodu Roslyn.</span><span class="sxs-lookup"><span data-stu-id="3fe56-117">For example, in the Roslyn code base we would have</span></span>

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

<span data-ttu-id="3fe56-118">Implementacja tego elementu będzie mogła emitować kompilator do emisji metody zastępującej jako "Nowa" metoda wirtualna, która ukrywa metodę klasy bazowej wraz z _metodą mostka_ implementującą metodę klasy bazowej z wywołaniem metody klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="3fe56-118">The implementation of this would be for the compiler to emit the overriding method as a "new" virtual method that hides the base class method, along with a _bridge method_ that implements the base class method with a call to the derived class method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="3fe56-119">Wady</span><span class="sxs-lookup"><span data-stu-id="3fe56-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="3fe56-120">[] Każda zmiana języka musi być płacona za siebie.</span><span class="sxs-lookup"><span data-stu-id="3fe56-120">[ ] Every language change must pay for itself.</span></span>
- <span data-ttu-id="3fe56-121">[] Powinnamy upewnić się, że wydajność jest rozsądna, nawet w przypadku hierarchii dziedziczenia głębokiego</span><span class="sxs-lookup"><span data-stu-id="3fe56-121">[ ] We should ensure that the performance is reasonable, even in the case of deep inheritance hierarchies</span></span>
- <span data-ttu-id="3fe56-122">[] Upewnij się, że artefakty strategii translacji nie wpływają na semantykę języka, nawet gdy zużywamy nowe IL ze starych kompilatorów.</span><span class="sxs-lookup"><span data-stu-id="3fe56-122">[ ] We should ensure that artifacts of the translation strategy do not affect language semantics, even when consuming new IL from old compilers.</span></span>

## <a name="alternatives"></a><span data-ttu-id="3fe56-123">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="3fe56-123">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="3fe56-124">Możemy osłabić reguły języka, aby umożliwić, w źródle,</span><span class="sxs-lookup"><span data-stu-id="3fe56-124">We could relax the language rules slightly to allow, in source,</span></span>

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a><span data-ttu-id="3fe56-125">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="3fe56-125">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="3fe56-126">[] Jak interfejsy API, które zostały skompilowane do korzystania z tej funkcji, działają w starszych wersjach języka?</span><span class="sxs-lookup"><span data-stu-id="3fe56-126">[ ] How will APIs that have been compiled to use this feature work in older versions of the language?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="3fe56-127">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="3fe56-127">Design meetings</span></span>

<span data-ttu-id="3fe56-128">Jeszcze nie.</span><span class="sxs-lookup"><span data-stu-id="3fe56-128">None yet.</span></span> <span data-ttu-id="3fe56-129">W <https://github.com/dotnet/roslyn/issues/357>istnieje kilka dyskusji.</span><span class="sxs-lookup"><span data-stu-id="3fe56-129">There has been some discussion at <https://github.com/dotnet/roslyn/issues/357>.</span></span>