---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484455"
---
# <a name="covariant-return-types"></a>typy zwracane przez współwarianty

* [x] proponowane
* [] Prototyp: nie uruchomiono
* [] Implementacja: nie uruchomiono
* [] Specyfikacja: nie uruchomiono

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Obsługa _typów zwracanych współwariantu_. W odniesieniu do metody przesłaniania można w specjalny sposób mieć bardziej pochodny typ referencyjny niż Metoda zastąpień.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Jest to typowy wzorzec w kodzie, który różne nazwy metod muszą być stworzone, aby obejść ograniczenia języka, które zastępują muszą zwracać ten sam typ co przesłoniętą metodę. Zobacz poniżej, aby zapoznać się z przykładem z bazy kodu Roslyn.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Obsługa _typów zwracanych współwariantu_. W odniesieniu do metody przesłaniania można w specjalny sposób mieć bardziej pochodny typ referencyjny niż Metoda zastąpień. Dotyczy to metod i właściwości, które są obsługiwane w klasach i interfejsach.

Jest to przydatne w przypadku wzorca fabrycznego. Na przykład, w bazie kodu Roslyn.

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

Implementacja tego elementu będzie mogła emitować kompilator do emisji metody zastępującej jako "Nowa" metoda wirtualna, która ukrywa metodę klasy bazowej wraz z _metodą mostka_ implementującą metodę klasy bazowej z wywołaniem metody klasy pochodnej.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

- [] Każda zmiana języka musi być płacona za siebie.
- [] Powinnamy upewnić się, że wydajność jest rozsądna, nawet w przypadku hierarchii dziedziczenia głębokiego
- [] Upewnij się, że artefakty strategii translacji nie wpływają na semantykę języka, nawet gdy zużywamy nowe IL ze starych kompilatorów.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Możemy osłabić reguły języka, aby umożliwić, w źródle,

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

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

- [] Jak interfejsy API, które zostały skompilowane do korzystania z tej funkcji, działają w starszych wersjach języka?

## <a name="design-meetings"></a>Spotkania projektowe

Jeszcze nie. W <https://github.com/dotnet/roslyn/issues/357>istnieje kilka dyskusji.