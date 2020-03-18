---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484490"
---
# <a name="suppress-emitting-of-localsinit-flag"></a>Pomiń emitowanie flagi `localsinit`.

* [x] proponowane
* [] Prototyp: nie uruchomiono
* [] Implementacja: nie uruchomiono
* [] Specyfikacja: nie uruchomiono

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Zezwalaj na pomijanie emitowania flagi `localsinit` za pośrednictwem atrybutu `SkipLocalsInitAttribute`. 

## <a name="motivation"></a>Motywacją
[motivation]: #motivation


### <a name="background"></a>Tło
Dla specyfikacji CLR zmienne lokalne, które nie zawierają odwołań, nie są inicjowane do określonej wartości przez maszynę wirtualną/JIT. Odczyt z takich zmiennych bez inicjalizacji jest bezpieczny dla typów, ale w przeciwnym razie zachowanie jest niezdefiniowane i specyficzne dla implementacji. Zwykle niezainicjowane elementy lokalne zawierają wszelkie wartości pozostawione w pamięci, która jest teraz zajęta przez ramkę stosu. Może to prowadzić do niedeterministycznych zachowań i trudno odtworzyć błędy. 

Istnieją dwa sposoby "przypisywania" zmiennej lokalnej: 
- przechowując wartość lub 
- określając flagę `localsinit`, która wymusza, że wszystkie elementy, które są przydzielone, tworzą pulę pamięci lokalnej, która ma zostać zainicjowana zero: dotyczy to zarówno zmiennych lokalnych, jak i danych `stackalloc`.    

Użycie niezainicjowanych danych jest niezalecane i nie jest dozwolone w kodzie możliwym do zweryfikowania. Mimo że można udowodnić, że przez metodę analizy przepływu można określić, że algorytm weryfikacji jest ostrożny i po prostu wymaga, aby `localsinit` został ustawiony.

C# Kompilator historyczny emituje flagę `localsinit` na wszystkich metodach, które deklarują elementy lokalne.

C# Program korzysta z analizy o określonym przypisaniu, która jest bardziej rygorystyczna niż wymagana Specyfikacja środowiskaC# CLR (wymaga również określenia zakresu wartości lokalnych), dlatego nie gwarantuje, że kod wynikający zostanie formalnie zweryfikowany:
- Środowisko CLR C# i reguły mogą nie wyrażać zgody na to, czy przekazanie argumentu lokalnego jako `out` jest `use`.
- Środowiska CLR C# i reguły mogą nie zgadzać się na traktowanie gałęzi warunkowych, gdy są znane warunki (Propagacja stała).
- Środowisko CLR może również po prostu wymagać `localinits`, ponieważ jest to dozwolone.  

### <a name="problem"></a>Problem
W przypadku aplikacji o wysokiej wydajności koszt wymuszonej inicjacji zero może być zauważalny. Jest to szczególnie zauważalne, gdy `stackalloc` jest używany.

W niektórych przypadkach JIT może Elide początkowej zero inicjacji pojedynczych lokalnych, gdy takie inicjowanie jest "zabite" przez kolejne przydziały. Nie wszystkie JITs to i że Optymalizacja ma limity. Nie pomaga w `stackalloc`.

Aby zilustrować, że problem występuje w rzeczywistości, występuje znana usterka, w której metoda nie zawiera żadnych `IL` lokalnych nie ma flagi `localsinit`. Usterka jest już wykorzystywana przez użytkowników przez umieszczenie `stackalloc` w takich metodach — celowo aby uniknąć kosztów inicjacji. Jest to pomimo faktu, że brak `IL` lokalnych jest niestabilną metryką i może się różnić w zależności od zmian w strategii codegen. Usterka powinna zostać naprawiona, a użytkownicy powinni uzyskać bardziej udokumentowaną i niezawodną metodę pomijania flagi. 

## <a name="detailed-design"></a>Szczegółowy projekt

Zezwól na określanie `System.Runtime.CompilerServices.SkipLocalsInitAttribute` jako sposobu, aby wypowiedzieć kompilatorowi, że nie emituje `localsinit` flagę.
 
Wynikiem tego jest to, że wartości lokalne nie mogą być inicjowane przez kompilator JIT, który jest w większości przypadków niezauważalny w C#.  
Oprócz tego `stackalloc` dane nie będą inicjowane od zera. Jest to w nieskończoność zauważalne, ale jest również najbardziej umotywowany scenariusz.

Dozwolone i rozpoznawane elementy docelowe atrybutów to: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`. Jednak kompilator nie wymaga, aby atrybut został zdefiniowany z wymienionymi obiektami docelowymi, a w tym przypadku zestaw atrybutu jest zdefiniowany. 

Gdy atrybut jest określony w kontenerze (`class`, `module`, zawierający metodę dla zagnieżdżonej metody,...), flaga ma wpływ na wszystkie metody zawarte w kontenerze.

Metody z syntezą "dziedziczą" flagę z kontenera logicznego/właściciela. 

Flaga ma wpływ tylko na strategię codegen dla rzeczywistych treści metod. I.E. Flaga nie ma wpływu na metody abstrakcyjne i nie jest propagowana do zastępowania/implementowania metod.

Jest to jawnie **_Funkcja kompilatora_** , a **_nie funkcja języka_** .  
Podobnie jak w przypadku przełączników wiersza polecenia kompilatora funkcja kontroluje szczegóły implementacji konkretnej strategii codegen i nie musi być wymagana przez C# specyfikację.

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

- Stare/inne kompilatory mogą nie przestrzegać atrybutu.
Ignorowanie atrybutu jest zgodne. Może to spowodować niewielkie trafienie wydajności.

- Flaga `localinits` nie może wyzwolić błędów weryfikacji.
Użytkownicy, którzy pytają o tę funkcję, zwykle nie są zainteresowani weryfikacją. 
 
- Zastosowanie atrybutu na wyższym poziomie niż w przypadku pojedynczej metody ma wpływ nielokalny, który jest zauważalny w przypadku użycia `stackalloc`. Jest to jednak najbardziej żądany scenariusz.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

- Pomiń flagę `localinits`, gdy metoda jest zadeklarowana w kontekście `unsafe`. Może to spowodować, że zachowanie dyskretne i niebezpieczne zmieni się z deterministyczne na niejednoznaczne w przypadku `stackalloc`.

- Pomiń flagę `localinits`.
Nawet nierówne.

- Pomiń flagę `localinits`, chyba że `stackalloc` jest używana w treści metody.
Nie dotyczy najbardziej żądanego scenariusza i może wyłączyć kod niemożliwy do zweryfikowania bez opcji przywrócenia tej z powrotem.

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

- Czy atrybut ma być rzeczywiście emitowany do metadanych? 

## <a name="design-meetings"></a>Spotkania projektowe

Jeszcze nie. 