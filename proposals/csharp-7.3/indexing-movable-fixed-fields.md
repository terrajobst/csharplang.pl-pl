---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484630"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a><span data-ttu-id="34428-101">Pola indeksowania `fixed` nie powinny wymagać przypinania niezależnie od elementu przenośnego/nieruchomego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="34428-101">Indexing `fixed` fields should not require pinning regardless of the movable/unmovable context.</span></span> #

<span data-ttu-id="34428-102">Zmiana ma rozmiar poprawki błędu.</span><span class="sxs-lookup"><span data-stu-id="34428-102">The change has the size of a bug fix.</span></span> <span data-ttu-id="34428-103">Może ona znajdować się w 7,3 i nie powoduje konfliktu z dowolnym kierunkiem.</span><span class="sxs-lookup"><span data-stu-id="34428-103">It can be in 7.3 and does not conflict with whatever direction we take further.</span></span>
<span data-ttu-id="34428-104">Ta zmiana ma na celu jedynie umożliwienie działania następującego scenariusza, nawet jeśli `s` jest ruchoma.</span><span class="sxs-lookup"><span data-stu-id="34428-104">This change is only about allowing the following scenario to work even though `s` is moveable.</span></span> <span data-ttu-id="34428-105">Jest ona już prawidłowa, gdy `s` nie jest ruchoma.</span><span class="sxs-lookup"><span data-stu-id="34428-105">It is already valid when `s` is not moveable.</span></span> 

<span data-ttu-id="34428-106">Uwaga: w obu przypadkach nadal wymaga `unsafe` kontekstu.</span><span class="sxs-lookup"><span data-stu-id="34428-106">NOTE: in either case, it still requires `unsafe` context.</span></span> <span data-ttu-id="34428-107">Możliwe jest odczytanie niezainicjowanych danych lub nawet poza zakresem.</span><span class="sxs-lookup"><span data-stu-id="34428-107">It is possible to read uninitialized data or even out of range.</span></span> <span data-ttu-id="34428-108">To nie zmienia się.</span><span class="sxs-lookup"><span data-stu-id="34428-108">That is not changing.</span></span>

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

<span data-ttu-id="34428-109">Główne "wyzwanie", które widzimy tutaj, to wyjaśnienie złagodzenia w specyfikacji. W szczególności, ponieważ następujące elementy nadal wymagają przypinania.</span><span class="sxs-lookup"><span data-stu-id="34428-109">The main “challenge” that I see here is how to explain the relaxation in the spec. In particular, since the following would still need pinning.</span></span> <span data-ttu-id="34428-110">(ponieważ `s` jest ruchome i jawnie używamy pola jako wskaźnika)</span><span class="sxs-lookup"><span data-stu-id="34428-110">(because `s` is moveable and we explicitly use the field as a pointer)</span></span>

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

<span data-ttu-id="34428-111">Jednym z powodów, dla których jest wymagane Przypinanie obiektu docelowego, gdy jest on Przenośny, jest artefaktem naszej strategii generowania kodu — zawsze Konwertujemy na niezarządzany wskaźnik i wymuszają, aby użytkownik mógł przypiąć za pośrednictwem instrukcji `fixed`.</span><span class="sxs-lookup"><span data-stu-id="34428-111">One reason why we require pinning of the target when it is movable is the artifact of our code generation strategy, - we always convert to an unmanaged pointer and thus force the user to pin via `fixed` statement.</span></span> <span data-ttu-id="34428-112">Jednak konwersja do niezarządzanej jest niepotrzebna podczas indeksowania.</span><span class="sxs-lookup"><span data-stu-id="34428-112">However, conversion to unmanaged is unnecessary when doing indexing.</span></span> <span data-ttu-id="34428-113">Ten sam niebezpieczny wskaźnik matematyczny jest równie stosowany, gdy mamy odbiornik w postaci zarządzanego wskaźnika.</span><span class="sxs-lookup"><span data-stu-id="34428-113">The same unsafe pointer math is equally applicable when we have the receiver in the form of a managed pointer.</span></span> <span data-ttu-id="34428-114">Jeśli to zrobisz, pośrednie odwołanie jest zarządzane (śledzenie GC), a Przypinanie jest zbędne.</span><span class="sxs-lookup"><span data-stu-id="34428-114">If we do that, then the intermediate ref is managed (GC-tracked) and the pinning is unnecessary.</span></span>

<span data-ttu-id="34428-115">Zmiana https://github.com/dotnet/roslyn/pull/24966 jest prototypem żądania ściągnięcia, które osłabi ten wymóg.</span><span class="sxs-lookup"><span data-stu-id="34428-115">The change https://github.com/dotnet/roslyn/pull/24966 is a prototype PR that relaxes this requirement.</span></span>
