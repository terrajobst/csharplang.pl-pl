---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484672"
---
# <a name="non-trailing-named-arguments"></a>Argumenty nazwane inne niż końcowe

## <a name="summary"></a>Podsumowanie
[summary]: #summary
Zezwalaj na używanie nazwanych argumentów w położeniu niekońcowym, o ile są one używane w poprawnej pozycji. Na przykład: `DoSomething(isEmployed:true, name, age);`.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Główną motywacją jest uniknięcie wpisywania nadmiarowych informacji. Często należy nazwać argument, który jest literałem (na przykład `null`, `true`) na potrzeby wyjaśnienia kodu, a nie przekazywania argumentów poza kolejnością.
Jest to obecnie niedozwolone (`CS1738`), chyba że wszystkie poniższe argumenty są również nazwane.

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

Kilka dodatkowych przykładów:
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

Będzie to również współpracowało z params:
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

W § 7.5.1 (listy argumentów) Specyfikacja obecnie mówi:
> *Argument* o *nazwie argumentu* jest określany jako __argument nazwany__, podczas gdy *Argument* bez *argumentu-Name* jest __argumentem pozycyjnym__. Występuje błąd dla argumentu pozycyjnego, który ma być wyświetlany po nazwanym argumencie na *liście argumentów*.

Celem tej oferty jest usunięcie tego błędu i zaktualizowanie reguł dotyczących znajdowania odpowiedniego parametru dla argumentu (§ 7.5.1.1):

Argumenty na liście argumentów konstruktorów wystąpień, metod, indeksatorów i delegatów:
- [istniejące reguły]
- Nienazwany argument odpowiada żadnemu parametrowi, gdy znajduje się po nazwie argumentu "out-of-position" lub nazwanego argumentu params.

W szczególności zapobiega to wywoływaniu `void M(bool a = true, bool b = true, bool c = true, );` z `M(c: false, valueB);`. Pierwszy argument jest używany poza pozycją (argument jest używany w pierwszej pozycji, ale parametr o nazwie "c" znajduje się w trzecim miejscu), dlatego następujące argumenty powinny mieć nazwę.

Innymi słowy, nazwane argumenty, które nie są końcowe są dozwolone tylko wtedy, gdy nazwa i pozycja powodują znalezienie tego samego odpowiadającego parametru.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Ta propozycja zaostrza istniejące subtleties z nazwanymi argumentami w ramach rozpoznawania przeciążenia. Przykład:

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

Tę sytuację można uzyskać dzisiaj, zamieniając parametry:

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

Podobnie, jeśli masz dwie metody `void M(int a, int b)` i `void M(int x, string y)`, pomylone wywołanie `M(x: 1, 2)` spowoduje wygenerowanie diagnostyki na podstawie drugiego przeciążenia ("nie można konwertować z" int "na" String ""). Ten problem już istnieje, gdy nazwany argument jest używany w pozycji końcowej.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Istnieje kilka rozwiązań alternatywnych, które należy wziąć pod uwagę:

- Stan quo
- Zapewnianie pomocy środowiska IDE do wypełniania wszystkich nazw końcowych argumentów podczas wpisywania konkretnej nazwy w środku.

Oba te osoby poniosły więcej informacji o większej szczegółowości, ponieważ wprowadzają wiele nazwanych argumentów nawet wtedy, gdy potrzebna jest tylko jedna nazwa literału na początku listy argumentów.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Spotkania projektowe
[ldm]: #ldm
Funkcja została krótko omówiona w programie LDM na 16 maja 2017, z zasadą zatwierdzania (OK, aby przenieść do propozycji/prototypu). Został on również krótko omówiony w 28 czerwca 2017.

Odnosi się do początkowej dyskusji https://github.com/dotnet/csharplang/issues/518 odnosi się do problemu championed https://github.com/dotnet/csharplang/issues/570
