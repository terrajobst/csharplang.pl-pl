---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484497"
---
# <a name="null-conditional-await"></a>oczekiwanie na wartość null

* [x] proponowane
* [] Prototype: brak
* [] Implementacja: brak
* [] — Specyfikacja: rozpoczęta, poniżej

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Obsługuj wyrażenie `await? e`, które czeka `e`, jeśli ma wartość inną niż null, w przeciwnym razie powoduje `null`.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Jest to typowy wzorzec kodowania, a ta funkcja mogłaby mieć całkiem synergię z istniejącymi operatorami do łączenia wartości null i wartościami null.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

Dodamy nową formę *await_expression*:

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

Operator `await` warunkowego null oczekuje na jego operand tylko wtedy, gdy ten operand ma wartość różną od null. W przeciwnym razie wynik zastosowania operatora ma wartość null.

Typ wyniku jest obliczany przy użyciu [reguł dla operatora warunkowego null](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).

> **Uwaga:** Jeśli `e` jest typu `Task`, wówczas `await? e;` nie będzie nic robić, jeśli `e` jest `null`, i await `e`, jeśli nie jest `null`.
>
> Jeśli `e` jest typu `Task<K>` gdzie `K` jest typem wartości, `await? e` będzie zwracać wartość typu `K?`.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Podobnie jak w przypadku dowolnej funkcji języka należy zwrócić uwagę na to, czy dodatkowa złożoność tego języka jest zapłacona w dodatkowym przejrzystości oferowanym C# przez funkcję.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Chociaż wymaga pewnego kodu, użycie tego operatora może być często zastąpione przez wyrażenie podobne do `(e == null) ? null : await e` lub instrukcji, takiej jak `if (e != null) await e`.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

- [] Wymaga przeglądu LDM

## <a name="design-meetings"></a>Spotkania projektowe

Brak.
