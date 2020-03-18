---
ms.openlocfilehash: aa0c233e7739ae7a0a7752251aae6f89df18ba93
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484518"
---
# <a name="c-language-proposals"></a>C#Propozycje dotyczące języka

Projekty dotyczące języka to dokumenty, które opisują bieżące informacje o danej funkcji języka.

Propozycje mogą być *aktywne*, *nieaktywne*, *odrzucone* lub *gotowe*. *Aktywne* propozycje są przechowywane bezpośrednio w folderze propozycji, *nieaktywne* i *odrzucone* propozycje są przechowywane w [nieaktywnych](proposals/inactive) i [odrzuconych](proposals/rejected) podfolderach, a *gotowe* propozycje są archiwizowane w folderze odpowiadającym wersji językowej, do której są częścią.

## <a name="lifetime-of-a-proposal"></a>Okres istnienia propozycji

Propozycja jest uruchamiana, gdy zespół projektowy języka zadecyduje, że może to być dobry dodatek do danego języka. Zwykle zostanie ona *uruchomiona, ale*jeśli chcemy przechwycić pomysł, nie chce już teraz korzystać z tego elementu, propozycja może również zostać uruchomiona w *nieaktywnym* podfolderze. Propozycje mogą nawet zacząć się bezpośrednio w stanie *odrzucone* , jeśli chcemy zarejestrować coś, czego nie zamierzamy robić. Na przykład jeśli nie można zaimplementować popularnego i cyklicznego żądania, możemy przechwycić ten element jako odrzucony wniosek.

Propozycja może się zacząć w przypadku [problemu z dyskusją](https://github.com/dotnet/csharplang/labels/Discussion)lub może pochodzić z dyskusji w spotkaniu projektu języka lub dotrzeć z wielu innych. Głównym zagadnieniem jest to, że zespół projektowy uważa, że należy to zrobić, i że istnieje ktoś, kto chce go napisać. Zwykle członkiem zespołu projektu języka będzie przypisywać siebie jako liderów dla tej funkcji, śledzony przez [problem liderów](https://github.com/dotnet/csharplang/labels/Proposal%20champion). Liderów jest odpowiedzialny za przeniesienie propozycji przez proces projektowania.

Propozycja jest *aktywna* , jeśli jest przenoszona w przód przez projektowanie i wdrażanie do kolejnej wersji. Gdy *wszystko będzie gotowe*, oznacza to, że implementacja została scalona w wersji, a funkcja została określona, jest przenoszona do podkatalogu odpowiadającego jego wydania.

Jeśli funkcja wykryje, że nie będzie można jej umieścić w języku, np. ponieważ okazuje się niewykonalne, nie będzie można dodać wystarczającej wartości lub po prostu nie jest odpowiednia dla danego języka, zostanie [odrzucona](proposals/rejected). Jeśli funkcja ma uzasadnioną obietnicę, ale nie ma obecnie priorytetów na korzystanie z niej, może być zadeklarowana jako [nieaktywna](proposals/inactive) , aby uniknąć załadowania folderu głównego. Doskonale nastąpi w przypadku pracy w nieaktywnych lub odrzuconych propozycjach, a także do nastąpił ich w przyszłości. Kategorie umożliwiają odzwierciedlenie bieżącego zamiaru projektowania.

## <a name="nature-of-a-proposal"></a>Charakter oferty

Propozycja powinna być zgodna z [szablonem propozycji](proposal-template.md). Dobrą propozycją powinna być:

- Dopasuj do ogólnej i estetycznego języka.
- Nie należy wprowadzać wyrazów alternatywnych dla istniejących funkcji.
- Dodaj wiele wartości dla czystego zestawu użytkowników.
- Nie należy znacznie dodawać do złożoności języka, szczególnie w przypadku nowych użytkowników.  

## <a name="discussion-of-proposals"></a>Omówienie propozycji

Opinie i dyskusja odbywają się w [kwestiach dyskusji](https://github.com/dotnet/csharplang/labels/Discussion). Po dodaniu nowej propozycji do folderu propozycji powinien on zostać ogłoszony w kwestii dyskusji przez liderów lub autora propozycji. 

 