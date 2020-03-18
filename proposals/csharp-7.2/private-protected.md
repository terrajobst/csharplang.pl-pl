---
ms.openlocfilehash: 6cf489595654236c18edee94c0af380e605c9571
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485176"
---
# <a name="private-protected"></a>private protected

* [x] proponowane
* [x] prototyp: [ukończono](https://github.com/dotnet/roslyn/blob/master/docs/features/private-protected.md)
* Implementacja [x]: [ukończono](https://github.com/dotnet/roslyn/blob/master/docs/features/private-protected.md)
* Specyfikacja [x]: [ukończono](#detailed-design)

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Uwidocznij poziom ułatwień dostępu `protectedAndInternal` C# CLR w `private protected`.

## <a name="motivation"></a>Motywacją
[motivation]: #motivation

Istnieje wiele sytuacji, w których interfejs API zawiera elementy członkowskie, które są przeznaczone tylko do implementacji i używane przez podklasy zawarte w zestawie, który udostępnia typ. Środowisko CLR zapewnia dla tego celu poziom dostępności, ale nie jest dostępne w programie C#. W związku z tym właściciele interfejsu API są zmuszeni do korzystania z `internal` ochrony i samodzielnego lub niestandardowego analizatora lub do korzystania z `protected` z dodatkową dokumentacją wyjaśniającą, że podczas gdy element członkowski jest wyświetlany w publicznej dokumentacji typu, nie jest on przeznaczony do obsługi publicznego interfejsu API.  Aby zapoznać się z przykładami tych ostatnich, zobacz członków Roslyn `CSharpCompilationOptions`, których nazwy rozpoczynają się od `Common`.

Bezpośrednie zapewnianie pomocy technicznej dla tego poziomu C# dostępu w programie umożliwia ich wyrażanie w sposób naturalny w języku.

## <a name="detailed-design"></a>Szczegółowy projekt
[design]: #detailed-design

### <a name="private-protected-access-modifier"></a>Modyfikator dostępu `private protected`

Zalecamy dodanie nowej kombinacji modyfikatora dostępu `private protected` (które mogą być wyświetlane w dowolnej kolejności wśród modyfikatorów). To mapuje do koncepcji CLR protectedAndInternal i pożyczy tej samej składni obecnie używanej w [ C++/CLI](https://docs.microsoft.com/cpp/dotnet/how-to-define-and-consume-classes-and-structs-cpp-cli#BKMK_Member_visibility).

Członek zadeklarowany `private protected` może być dostępny w podklasie kontenera, jeśli ta podklasa jest w tym samym zestawie, co element członkowski.

Firma Microsoft modyfikuje specyfikację języka w następujący sposób (Dodatki pogrubione). Numery sekcji nie są wyświetlane poniżej, ponieważ mogą się one różnić w zależności od wersji specyfikacji, do której jest zintegrowana.

-----

> Zadeklarowana dostępność elementu członkowskiego może być jedną z następujących czynności:
- Public, który jest wybierany przez dołączenie modyfikatora publicznego w deklaracji elementu członkowskiego. Intuicyjne znaczenie publiczne to "dostęp nieograniczony".
- Chroniony, który jest wybierany przez dołączenie chronionego modyfikatora w deklaracji elementu członkowskiego. Intuicyjne znaczenie ochrony to "dostęp ograniczony do klasy zawierającej lub typów pochodzących od klasy zawierającej".
- Wewnętrzna, która jest wybierana przez dołączenie modyfikatora wewnętrznego w deklaracji elementu członkowskiego. Intuicyjne znaczenie elementu Internal to "dostęp ograniczony do tego zestawu".
- Chroniona wewnętrznie, która jest wybierana przez uwzględnienie zarówno modyfikatora chronionego, jak i wewnętrznego w deklaracji elementu członkowskiego. Intuicyjne znaczenie chronionego elementu wewnętrznego jest "dostępne w tym zestawie, jak również typy pochodzące z klasy zawierającej".
- **Prywatna chroniona, która jest wybierana przez uwzględnienie zarówno modyfikatora Private, jak i chronionego w deklaracji elementu członkowskiego. Intuicyjne znaczenie prywatnych chronionych jest "dostępne w tym zestawie według typów pochodzących od klasy zawierającej".**

-----

> W zależności od kontekstu, w którym jest realizowana Deklaracja elementu członkowskiego, dozwolone są tylko niektóre typy zadeklarowanych ułatwień dostępu. Ponadto, gdy Deklaracja elementu członkowskiego nie zawiera żadnych modyfikatorów dostępu, kontekst, w którym odbywa się deklaracja, określa domyślny dostępną dostępność. 
- Przestrzenie nazw niejawnie mają publicznie zadeklarowane ułatwienia dostępu. W deklaracjach przestrzeni nazw nie są dozwolone Modyfikatory dostępu.
- Typy zadeklarowane bezpośrednio w jednostkach kompilacji lub przestrzeniach nazw (w przeciwieństwie do innych typów) mogą mieć publiczne lub wewnętrzne zadeklarowane ułatwienia dostępu i domyślne dla wewnętrznej zadeklarowanej dostępności.
- Elementy członkowskie klasy mogą mieć dowolne pięć rodzajów zadeklarowanych dostępności i domyślnie dla prywatnych zadeklarowanych dostępności. [Uwaga: typ zadeklarowany jako składowa klasy może mieć którykolwiek z pięciu rodzajów zadeklarowanej dostępności, natomiast typ zadeklarowany jako element członkowski przestrzeni nazw może mieć tylko publiczne lub wewnętrzne zadeklarowane ułatwienia dostępu. Notatka końcowa]
- Elementy członkowskie struktury mogą mieć publiczne, wewnętrzne lub prywatne deklarowane ułatwienia dostępu i domyślne dla prywatnych zadeklarowanych dostępności, ponieważ struktury są niejawnie zapieczętowane. Elementy członkowskie struktury wprowadzone w strukturze (która nie jest dziedziczona przez tę strukturę) nie mogą*mieć chronionej* ~~lub~~ chronionej wewnętrznej lub chronionej**w sposób prywatny** dostępności. [Uwaga: typ zadeklarowany jako element członkowski struktury może mieć publiczne, wewnętrzne lub prywatne zadeklarowane ułatwienia dostępu, podczas gdy typ zadeklarowany jako składowa przestrzeni nazw może mieć tylko publiczne lub wewnętrzne zadeklarowane ułatwienia dostępu. Notatka końcowa]
- Członkowie interfejsu niejawnie mają publicznie zadeklarowane ułatwienia dostępu. W deklaracjach składowych interfejsu nie można używać modyfikatorów dostępu.
- Elementy członkowskie wyliczenia niejawnie mają publicznie zadeklarowane ułatwienia dostępu. Modyfikatory dostępu są niedozwolone w deklaracjach składowych wyliczenia.

-----

> Domena dostępności zagnieżdżonej składowej M zadeklarowana w typie T w programie P, jest zdefiniowana w następujący sposób (stwierdzając, że M może być typem):
- Jeśli zadeklarowana dostępność M jest publiczna, domena dostępności M jest domeną dostępności T.
- Jeśli zadeklarowana dostępność M jest chroniona wewnętrznie, niech D należy do Unii tekstu programu P i tekstu programu dowolnego typu pochodzącego z T, który jest zadeklarowany poza P. Domena dostępności M jest częścią wspólną domeny dostępności T z D.
- **Jeśli zadeklarowana dostępność M jest chroniona prywatnie, niech D to część wspólna tekstu programu P i tekstu programu dowolnego typu pochodzącego z T. Domena dostępności M jest częścią wspólną domeny dostępności T z D.**
- Jeśli zadeklarowana dostępność M jest chroniona, niech D należy do Unii tekstu programu T i tekstu programu dowolnego typu pochodzącego z T. Domena dostępności M jest częścią wspólną domeny dostępności T z D.
- Jeśli zadeklarowana dostępność M jest wewnętrzna, domena dostępności M jest częścią wspólną domeny dostępności T z tekstem programu P.
- Jeśli zadeklarowana dostępność M jest prywatna, domena dostępności M jest tekstem programu T.

-----

> Po uzyskaniu dostępu do chronionego **lub prywatnego wystąpienia chronionego** poza tekstem programu klasy, w którym jest zadeklarowana, oraz gdy jest dostępny chroniony wewnętrzny element członkowski poza tekstem programu w ramach programu, w którym jest zadeklarowany, dostęp odbywa się w deklaracji klasy, która pochodzi od klasy, w której jest zadeklarowana. Ponadto dostęp musi odbywać się za pośrednictwem wystąpienia tego typu klasy pochodnej lub klasy z tego typu. To ograniczenie uniemożliwia jednej klasie pochodnej dostęp do chronionych elementów członkowskich innych klas pochodnych, nawet gdy elementy członkowskie są dziedziczone z tej samej klasy bazowej.

-----

> Dozwolone Modyfikatory dostępu i domyślny dostęp dla deklaracji typu zależą od kontekstu, w którym odbywa się deklaracja (§ 9.5.2):
- Typy zadeklarowane w jednostkach kompilacji lub przestrzeniach nazw mogą mieć dostęp publiczny lub wewnętrzny. Wartość domyślna to dostęp wewnętrzny.
- Typy zadeklarowane w klasach mogą mieć publiczny, chroniony wewnętrzny, chroniony **przez prywatny**, chroniony, wewnętrzny lub prywatny dostęp. Wartość domyślna to dostęp prywatny.
- Typy zadeklarowane w strukturach mogą mieć dostęp publiczny, wewnętrzny lub prywatny. Wartość domyślna to dostęp prywatny.

-----

> Deklaracja klasy statycznej podlega następującym ograniczeniom:
- Klasa statyczna nie powinna zawierać modyfikatora sealed ani abstract. (Jednak ponieważ Klasa statyczna nie może być skonkretyzowany ani pochodną, zachowuje się tak, jakby była zapieczętowana i abstrakcyjna).
- Klasa statyczna nie powinna zawierać specyfikacji klasy bazowej (§ 16.2.5) i nie może jawnie określić klasy bazowej lub listy zaimplementowanych interfejsów. Klasa statyczna niejawnie dziedziczy z obiektu typu.
- Klasa statyczna powinna zawierać tylko statyczne elementy członkowskie (§ 16.4.8). [Uwaga: wszystkie stałe i zagnieżdżone typy są klasyfikowane jako statyczne elementy członkowskie. Notatka końcowa]
- Klasa statyczna nie powinna mieć członków z chronioną **, prywatną chronioną** lub chronioną wewnętrzną dostępnością.

> Jest to błąd czasu kompilacji, który narusza którekolwiek z tych ograniczeń. 

-----

> Deklaracja klasy składowej może mieć jeden z ~~pięciu~~**możliwych do** użycia (§ 9.5.2): Public, **Private Protected**, protected internal, protected, internal lub Private. Z wyjątkiem chronionych wewnętrznych **i prywatnych kombinacji chronionych** ,**s**jest to błąd czasu kompilacji, aby określić więcej niż jeden modyfikator dostępu. Gdy deklaracja klasy składowej nie zawiera żadnych modyfikatorów dostępu, założono, że Private.

-----

> Typy niezagnieżdżone mogą mieć zainstalowaną publiczną lub wewnętrzną dostępność, a domyślnie mają wewnętrznie zadeklarowane ułatwienia dostępu. Typy zagnieżdżone mogą mieć te formy zadeklarowanej dostępności, a także co najmniej jedną dodatkową postać zadeklarowanej dostępności, w zależności od tego, czy typ zawierający jest klasą czy strukturą:
- Zagnieżdżony typ, który jest zadeklarowany w klasie, może mieć jedną z ~~pięciu~~**postaci** zadeklarowanych dostępności (Public, **Private Protected**, protected internal, protected, internal lub private), podobnie jak w przypadku innych elementów członkowskich klasy, domyślnie jest to prywatne deklarowane ułatwienia dostępu.
- Zagnieżdżony typ, który jest zadeklarowany w strukturze, może mieć jedną z trzech postaci zadeklarowanych dostępności (Public, internal lub private), podobnie jak w przypadku innych elementów członkowskich struktury, domyślnie do prywatnych zadeklarowanych dostępności.

-----

> Metoda zastąpiona przez deklarację przesłonięcia jest znana jako zastąpiona metoda bazowa dla metody przesłonięcia M zadeklarowanej w klasie C, zastąpiona metoda bazowa jest określana przez badanie każdego typu klasy bazowej C, rozpoczynając od bezpośredniego typu klasy podstawowej C i Kontynuując każdy kolejny, bezpośredni typ klasy bazowej, do czasu, aż w danym typie klasy bazowej, znajduje się co najmniej jedna metoda dostępna, która ma taki sam podpis jak M po podstawieniu argumentów typu. Na potrzeby lokalizowania zastąpionej metody bazowej Metoda jest uważana za dostępną, jeśli jest publiczna, jeśli jest chroniona, jeśli jest chroniona wewnętrznie, lub **Jeśli jest chroniona wewnętrznie lub** **prywatnie** i zadeklarowana w tym samym programie jako C.

-----

> Użycie modyfikatorów akcesora podlega następującym ograniczeniom:
- Modyfikator akcesora nie może być używany w interfejsie ani w jawnej implementacji elementu członkowskiego interfejsu.
- Dla właściwości lub indeksatora, który nie ma modyfikatora przesłaniania, modyfikator akcesora jest dozwolony tylko wtedy, gdy właściwość lub indeksator ma metodę dostępu get i Set, a następnie jest dozwolony tylko w jednym z tych metod dostępu.
- Dla właściwości lub indeksatora zawierającego modyfikator przesłonięcia, metoda dostępu jest zgodna z modyfikatorem akcesora, o ile istnieje, przesłanianej metody dostępu.
- Modyfikator akcesora deklaruje dostępność, która jest ściśle bardziej restrykcyjna niż zadeklarowana dostępność właściwości lub indeksatora. Aby precyzyjnie:
  - Jeśli właściwość lub indeksator ma zadeklarowaną dostępność publiczną, modyfikator akcesora może być **prywatny, chroniony**wewnętrznie, wewnętrzny, chroniony lub prywatny.
  - Jeśli właściwość lub indeksator ma zadeklarowane ułatwienia dostępu chronione wewnętrznie, modyfikator akcesora może być **prywatną chronioną**, wewnętrzną, chronioną lub prywatną.
  - Jeśli właściwość lub indeksator ma zadeklarowaną dostępność wewnętrzną lub chronioną, modyfikator akcesora jest **prywatny lub** prywatny.
  - **Jeśli właściwość lub indeksator ma zadeklarowaną dostępność prywatnej ochrony, modyfikator akcesora jest prywatny.**
  - Jeśli właściwość lub indeksator ma zadeklarowaną dostępność prywatną, nie można użyć modyfikatora metody dostępu.

-----

> Ponieważ dziedziczenie nie jest obsługiwane dla struktur, zadeklarowana dostępność elementu członkowskiego struktury nie może być chroniona, **prywatna**lub chroniona wewnętrznie.

-----

## <a name="drawbacks"></a>Wady
[drawbacks]: #drawbacks

Podobnie jak w przypadku dowolnej funkcji języka należy zwrócić uwagę na to, czy dodatkowa złożoność tego języka jest zapłacona w dodatkowym przejrzystości oferowanym C# przez funkcję.

## <a name="alternatives"></a>Alternatywy
[alternatives]: #alternatives

Alternatywą jest udostępnienie interfejsu API łączącego atrybut i Analizator. Ten atrybut jest umieszczany przez programistę na `internal` składowej, aby wskazuje, że element członkowski jest przeznaczony do użycia tylko w podklasach, a Analizator sprawdza, czy te ograniczenia są przestrzegane. 

## <a name="unresolved-questions"></a>Nierozwiązane pytania
[unresolved]: #unresolved-questions

Implementacja jest w dużym stopniu ukończona. Jedynym otwartym elementem roboczym jest Szkicowanie odpowiedniej specyfikacji dla języka VB.

## <a name="design-meetings"></a>Spotkania projektowe

TBD
