---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484609"
---
# <a name="stackalloc-array-initializers"></a>Inicjatory tablic stackalloc

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Zezwalaj na używanie składni inicjatora tablicy z `stackalloc`.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

W czasie tworzenia można inicjować ich elementy. Wydaje się, że jest to możliwe w `stackalloc` przypadku.

Pytanie, dlaczego taka składnia nie jest dozwolona w `stackalloc` występuje dość często.  
Zobacz na przykład [#1112](https://github.com/dotnet/csharplang/issues/1112)

## <a name="detailed-design"></a>Szczegółowy projekt

Zwykłe tablice można utworzyć za pomocą następującej składni:

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

Należy zezwolić na tworzenie tablic przyznanych przez stos przy użyciu:  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

Semantyka wszystkich przypadków jest w przybliżeniu taka sama jak w przypadku tablic.  
Na przykład: w ostatnim przypadku typ elementu jest wywnioskowany z inicjatora i musi być typem niezarządzanym.

Uwaga: funkcja nie jest zależna od elementu docelowego, który jest `Span<T>`. Jest to tak samo samo jak w przypadku `T*`, więc nie wydaje się, że jest to uzasadnione w przypadku `Span<T>` przypadku.  

## <a name="translation"></a>Tłumaczenie

Implementacja algorytmie może po prostu zainicjować tablicę bezpośrednio po utworzeniu za pomocą szeregu przypisań elementów.  

Podobnie jak w przypadku tablic, może być możliwe i pożądane, aby wykrywać przypadki, w których wszystkie lub większość elementów są typami danych kopiowalnych i używają bardziej wydajnych technik przez kopiowanie do wstępnie utworzonego stanu wszystkich elementów stałych. 

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Jest to wygodna funkcja. Nie można nic robić.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Spotkania projektowe

Jeszcze nie. 
