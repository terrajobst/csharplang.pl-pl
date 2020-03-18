---
ms.openlocfilehash: 833ea0469149cbd434e8950e844740a3adb4d386
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484595"
---
# <a name="improved-overload-candidates"></a>Ulepszone kandydujące metody rozwiązywania przeciążenia

## <a name="summary"></a>Podsumowanie
[summary]: #summary

Reguły rozpoznawania przeciążenia zostały zaktualizowane niemal każdej C# aktualizacji języka w celu poprawy środowiska dla programistów, co oznacza, że niejednoznaczne wywołania wybierają wybór oczywisty. Należy to zrobić uważnie, aby zachować zgodność z poprzednimi wersjami, ale ponieważ zwykle rozwiązujemy przypadki błędów, te udoskonalenia zwykle dobrze.

1. Gdy grupa metod zawiera zarówno wystąpienie, jak i statyczne elementy członkowskie, odrzucamy elementy członkowskie wystąpienia, jeśli są wywoływane bez odbiorcy wystąpienia lub kontekstu, i odrzucają statyczne elementy członkowskie, jeśli są wywoływane z odbiornikiem wystąpienia. Gdy nie ma odbiornika, dołączane są tylko statyczne elementy członkowskie w kontekście statycznym, w przeciwnym razie elementy członkowskie statyczne i wystąpienia. Gdy odbiorca jest niejednoznacznie wystąpieniem lub typem ze względu na sytuację kolorową, zawieramy oba te elementy. Statyczny kontekst, w którym niejawny odbiornik tego wystąpienia nie może być używany, zawiera treść elementów członkowskich, w których nie jest zdefiniowany, takich jak statyczne elementy członkowskie, a także miejsca, w których nie można ich użyć, takich jak inicjatory pól i konstruktory.
2. Gdy grupa metod zawiera kilka metod ogólnych, których argumenty typu nie spełniają ograniczeń, te elementy członkowskie są usuwane z zestawu kandydatów.
3. Dla konwersji grup metod, metody kandydujące, których typ zwracany nie jest zgodny z typem zwracanym delegata, są usuwane z zestawu.
