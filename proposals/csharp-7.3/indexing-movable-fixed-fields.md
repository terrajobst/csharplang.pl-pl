---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484630"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a>Pola indeksowania `fixed` nie powinny wymagać przypinania niezależnie od elementu przenośnego/nieruchomego kontekstu. #

Zmiana ma rozmiar poprawki błędu. Może ona znajdować się w 7,3 i nie powoduje konfliktu z dowolnym kierunkiem.
Ta zmiana ma na celu jedynie umożliwienie działania następującego scenariusza, nawet jeśli `s` jest ruchoma. Jest ona już prawidłowa, gdy `s` nie jest ruchoma. 

Uwaga: w obu przypadkach nadal wymaga `unsafe` kontekstu. Możliwe jest odczytanie niezainicjowanych danych lub nawet poza zakresem. To nie zmienia się.

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int p = s.myFixedField[5]; // indexing fixed-size array fields would be ok
    }
}
```

Główne "wyzwanie", które widzimy tutaj, to wyjaśnienie złagodzenia w specyfikacji. W szczególności, ponieważ następujące elementy nadal wymagają przypinania. (ponieważ `s` jest ruchome i jawnie używamy pola jako wskaźnika)

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int* ptr = s.myFixedField; // taking a pointer explicitly still requires pinning.
        int p = ptr[5];
    }
}
```

Jednym z powodów, dla których jest wymagane Przypinanie obiektu docelowego, gdy jest on Przenośny, jest artefaktem naszej strategii generowania kodu — zawsze Konwertujemy na niezarządzany wskaźnik i wymuszają, aby użytkownik mógł przypiąć za pośrednictwem instrukcji `fixed`. Jednak konwersja do niezarządzanej jest niepotrzebna podczas indeksowania. Ten sam niebezpieczny wskaźnik matematyczny jest równie stosowany, gdy mamy odbiornik w postaci zarządzanego wskaźnika. Jeśli to zrobisz, pośrednie odwołanie jest zarządzane (śledzenie GC), a Przypinanie jest zbędne.

Zmiana https://github.com/dotnet/roslyn/pull/24966 jest prototypem żądania ściągnięcia, które osłabi ten wymóg.
