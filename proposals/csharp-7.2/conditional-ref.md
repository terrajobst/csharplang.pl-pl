---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484700"
---
# <a name="conditional-ref-expressions"></a>Warunkowe wyrażenia ref

Wzorzec powiązania zmiennej ref z jednym lub innym wyrażeniem warunkowo nie jest obecnie można wyrazić elementu w C#.

Typowym obejściem jest wprowadzenie metody podobnej do:

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

Należy zauważyć, że nie jest to dokładne zastąpienie Trzyelementowy, ponieważ wszystkie argumenty muszą być oceniane w miejscu wywołania.

Następujące elementy nie będą działały zgodnie z oczekiwaniami:

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

Proponowana składnia będzie wyglądać następująco:

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

Powyższa próba ze specyfikatorem "Choice" może być _prawidłowo_ zapisywana przy użyciu parametru "Trzyelementowy" jako:

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

Różnica polega na tym, że wyrażenia sekwencji i alternatywne są dostępne w sposób _naprawdę_ warunkowo, więc nie widzimy awarii, jeśli ```arr == null```

Odwołaniem do sieci Trzyelementowy jest tylko typ Trzyelementowy, w przypadku którego alternatywa i skutki są odwołaniami. W naturalny sposób będzie wymagane przekazanie/alternatywne operandy LValues. Będzie również wymagało, aby z kolei wszystkie typy, które są do siebie konwertowane.

Typ wyrażenia będzie obliczany podobnie jak w przypadku zwykłego elementu Trzyelementowy. I.E. w przypadku, gdy sekwencje i alternatywy mają konwersję tożsamości, ale różne typy, będą stosowane istniejące reguły scalania typu.

Bezpieczne do zwrócenia będą uznawane za nieostrożnie od operandów warunkowych. Jeśli funkcja nie będzie mogła zwrócić całej rzeczy, nie będzie mogła zostać zwrócona.

Parametr "Trzyelementowy" ref to element LValue, który może być przesłany/przypisywany/zwracany przez odwołanie;

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

Jest to LValue, ale może być również przypisany do. 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

Typ "Trzyelementowy" można również użyć w zwykłym kontekście (nie ref). Mimo że nie będzie to typowe, ponieważ można używać zwykłej sieci Trzyelementowy.

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

Uwagi dotyczące implementacji: 

Złożoność implementacji wydaje się stanowić rozmiar rozwiązania typu umiarkowane-to-Large. -I. E nie jest bardzo kosztowna.
Nie myślę, że potrzebujemy żadnych zmian w składni lub analizie.
Nie ma wpływu na metadane lub międzyoperacyjność. Ta funkcja jest całkowicie oparta na wyrażeniu.
Brak wpływu na Debugowanie/PDB
