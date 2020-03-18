---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2019
ms.locfileid: "79485155"
---
# <a name="lambda-discard-parameters"></a>Odrzuć parametry lambda

## <a name="summary"></a>Podsumowanie

Zezwalaj na odrzucanie (`_`) do użycia jako parametry wyrażeń lambda i metod anonimowych.
Na przykład:
- wyrażenia lambda: `(_, _) => 0`, `(int _, int _) => 0`
- Metody anonimowe: `delegate(int _, int _) { return 0; }`

## <a name="motivation"></a>Motywacją

Nieużywane parametry nie muszą mieć nazwy. Intencje odrzucania są jasne, czyli nie są używane/odrzucane.

## <a name="detailed-design"></a>Szczegółowy projekt

[Parametry metody](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) Na liście parametrów metody lambda lub anonimowej zawierającej więcej niż jeden parametr o nazwie `_`, takie parametry są odrzucane parametry.
Uwaga: Jeśli pojedynczy parametr ma nazwę `_`, jest to zwykły parametr dla powodów zgodności z poprzednimi wersjami.

Parametry Discard nie wprowadzają żadnych nazw do żadnych zakresów.
Uwaga: oznacza to, że nie powodują ukrycia nazw `_` (podkreślenia).

[Proste nazwy](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) Jeśli `K` jest równa zero, a *simple_name* pojawia się w *bloku* , a jeśli *blok*(lub otaczający *blok*) obszar deklaracji zmiennych lokalnych ([deklaracji](basic-concepts.md#declarations)) zawiera zmienną lokalną, parametr (z wyjątkiem odrzucania parametrów) lub stała z nazwą `I`, wówczas *simple_name* odwołuje się do tej zmiennej lokalnej, parametru lub stałej i jest klasyfikowana jako zmienna lub wartość.

[Zakresy](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) Z wyjątkiem odrzucania parametrów zakres parametru zadeklarowany w *lambda_expression* ([wyrażenia funkcji anonimowej](expressions.md#anonymous-function-expressions)) to *anonymous_function_body* *lambda_expression* z wyjątkiem odrzucania parametrów, zakres parametru zadeklarowany w *anonymous_method_expression* ([wyrażenia funkcji anonimowych](expressions.md#anonymous-function-expressions)) to *blok* tego *anonymous_method_expression*.

## <a name="related-spec-sections"></a>Powiązane sekcje specyfikacji
- [Odpowiednie parametry](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
