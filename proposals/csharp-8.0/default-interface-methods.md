---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485183"
---
# <a name="default-interface-methods"></a><span data-ttu-id="a7c53-101">domyślne metody interfejsu</span><span class="sxs-lookup"><span data-stu-id="a7c53-101">default interface methods</span></span>

* <span data-ttu-id="a7c53-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="a7c53-102">[x] Proposed</span></span>
* <span data-ttu-id="a7c53-103">[] Prototyp: [w toku](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span><span class="sxs-lookup"><span data-stu-id="a7c53-103">[ ] Prototype: [In progress](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span></span>
* <span data-ttu-id="a7c53-104">[] Implementacja: brak</span><span class="sxs-lookup"><span data-stu-id="a7c53-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="a7c53-105">[] — Specyfikacja: w toku, poniżej</span><span class="sxs-lookup"><span data-stu-id="a7c53-105">[ ] Specification: In progress, below</span></span>

## <a name="summary"></a><span data-ttu-id="a7c53-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a7c53-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a7c53-107">Dodawanie obsługi _metod rozszerzenia wirtualnego_ — metody w interfejsach z konkretnymi implementacjami.</span><span class="sxs-lookup"><span data-stu-id="a7c53-107">Add support for _virtual extension methods_ - methods in interfaces with concrete implementations.</span></span> <span data-ttu-id="a7c53-108">Klasa lub struktura implementująca taki interfejs jest wymagana do posiadania jednej _najbardziej konkretnej_ implementacji dla metody interfejsu, implementowanej przez klasę lub strukturę lub dziedziczoną z jej klas podstawowych lub interfejsów.</span><span class="sxs-lookup"><span data-stu-id="a7c53-108">A class or struct that implements such an interface is required to have a single _most specific_ implementation for the interface method, either implemented by the class or struct, or inherited from its base classes or interfaces.</span></span> <span data-ttu-id="a7c53-109">Wirtualne metody rozszerzenia umożliwiają autorowi interfejsu API dodawanie metod do interfejsu w przyszłych wersjach bez przerywania zgodności źródłowej lub binarnej z istniejącymi implementacjami tego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a7c53-109">Virtual extension methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>

<span data-ttu-id="a7c53-110">Są one podobne do ["domyślnych metod"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)języka Java.</span><span class="sxs-lookup"><span data-stu-id="a7c53-110">These are similar to Java's ["Default Methods"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html).</span></span>

<span data-ttu-id="a7c53-111">(W oparciu o możliwą technikę implementacji) Ta funkcja wymaga odpowiedniej obsługi w interfejsie wiersza polecenia/CLR.</span><span class="sxs-lookup"><span data-stu-id="a7c53-111">(Based on the likely implementation technique) this feature requires corresponding support in the CLI/CLR.</span></span> <span data-ttu-id="a7c53-112">Programy korzystające z tej funkcji nie mogą działać w starszych wersjach platformy.</span><span class="sxs-lookup"><span data-stu-id="a7c53-112">Programs that take advantage of this feature cannot run on earlier versions of the platform.</span></span>

## <a name="motivation"></a><span data-ttu-id="a7c53-113">Motywacją</span><span class="sxs-lookup"><span data-stu-id="a7c53-113">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a7c53-114">Najważniejsze motywacje dla tej funkcji to</span><span class="sxs-lookup"><span data-stu-id="a7c53-114">The principal motivations for this feature are</span></span>

- <span data-ttu-id="a7c53-115">Domyślne metody interfejsu umożliwiają autorowi interfejsu API dodawanie metod do interfejsu w przyszłych wersjach bez przerywania zgodności źródłowej lub binarnej z istniejącymi implementacjami tego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a7c53-115">Default interface methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>
- <span data-ttu-id="a7c53-116">Funkcja umożliwia C# współdziałanie z interfejsami API przeznaczonymi dla [systemu Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) i [iOS (SWIFT)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), które obsługują podobne funkcje.</span><span class="sxs-lookup"><span data-stu-id="a7c53-116">The feature enables C# to interoperate with APIs targeting [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) and [iOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), which support similar features.</span></span>
- <span data-ttu-id="a7c53-117">W miarę wyłączania, Dodawanie domyślnych implementacji interfejsu zapewnia elementy funkcji języka "cechy" (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span><span class="sxs-lookup"><span data-stu-id="a7c53-117">As it turns out, adding default interface implementations provides the elements of the "traits" language feature (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span></span> <span data-ttu-id="a7c53-118">Cechy zostały sprawdzone jako zaawansowana technika programowania (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span><span class="sxs-lookup"><span data-stu-id="a7c53-118">Traits have proven to be a powerful programming technique (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a7c53-119">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="a7c53-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a7c53-120">Składnia interfejsu jest rozszerzona, aby umożliwić</span><span class="sxs-lookup"><span data-stu-id="a7c53-120">The syntax for an interface is extended to permit</span></span>

- <span data-ttu-id="a7c53-121">deklaracje składowych, które deklarują stałe, operatory, konstruktory statyczne i zagnieżdżone typy;</span><span class="sxs-lookup"><span data-stu-id="a7c53-121">member declarations that declare constants, operators, static constructors, and nested types;</span></span>
- <span data-ttu-id="a7c53-122">*treść* metody lub indeksatora, właściwości lub metody dostępu do zdarzeń (czyli implementacji "domyślna");</span><span class="sxs-lookup"><span data-stu-id="a7c53-122">a *body* for a method or indexer, property, or event accessor (that is, a "default" implementation);</span></span>
- <span data-ttu-id="a7c53-123">deklaracje elementów członkowskich, które deklarują pola statyczne, metody, właściwości, indeksatory i zdarzenia;</span><span class="sxs-lookup"><span data-stu-id="a7c53-123">member declarations that declare static fields, methods, properties, indexers, and events;</span></span>
- <span data-ttu-id="a7c53-124">deklaracje elementów członkowskich przy użyciu jawnej składni implementacji interfejsu; lub</span><span class="sxs-lookup"><span data-stu-id="a7c53-124">member declarations using the explicit interface implementation syntax; and</span></span>
- <span data-ttu-id="a7c53-125">Modyfikatory jawnego dostępu (domyślny dostęp jest `public`).</span><span class="sxs-lookup"><span data-stu-id="a7c53-125">Explicit access modifiers (the default access is `public`).</span></span>

<span data-ttu-id="a7c53-126">Członkowie z organami zezwalają interfejsowi na określenie implementacji "default" dla metody w klasach i strukturach, które nie oferują implementacji zastępującej.</span><span class="sxs-lookup"><span data-stu-id="a7c53-126">Members with bodies permit the interface to provide a "default" implementation for the method in classes and structs that do not provide an overriding implementation.</span></span>

<span data-ttu-id="a7c53-127">Interfejsy nie mogą zawierać stanu wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a7c53-127">Interfaces may not contain instance state.</span></span> <span data-ttu-id="a7c53-128">Gdy pola statyczne są teraz dozwolone, pola wystąpień nie są dozwolone w interfejsach.</span><span class="sxs-lookup"><span data-stu-id="a7c53-128">While static fields are now permitted, instance fields are not permitted in interfaces.</span></span> <span data-ttu-id="a7c53-129">Funkcja autowłaściwości wystąpienia nie jest obsługiwana w interfejsach, ponieważ niejawnie deklaruje pole ukryte.</span><span class="sxs-lookup"><span data-stu-id="a7c53-129">Instance auto-properties are not supported in interfaces, as they would implicitly declare a hidden field.</span></span>

<span data-ttu-id="a7c53-130">Metody static i Private umożliwiają przydatną refaktoryzację i organizację kodu używaną do implementowania publicznego interfejsu API interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a7c53-130">Static and private methods permit useful refactoring and organization of code used to implement the interface's public API.</span></span>

<span data-ttu-id="a7c53-131">Przesłonięcie metody w interfejsie musi używać jawnej składni implementacji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a7c53-131">A method override in an interface must use the explicit interface implementation syntax.</span></span>

<span data-ttu-id="a7c53-132">Wystąpił błąd podczas deklarowania typu klasy, typu struktury lub typu wyliczeniowego w zakresie parametru typu, który został zadeklarowany za pomocą *variance_annotation*.</span><span class="sxs-lookup"><span data-stu-id="a7c53-132">It is an error to declare a class type, struct type, or enum type within the scope of a type parameter that was declared with a *variance_annotation*.</span></span>  <span data-ttu-id="a7c53-133">Na przykład deklaracja `C` poniżej jest błędem.</span><span class="sxs-lookup"><span data-stu-id="a7c53-133">For example, the declaration of `C` below is an error.</span></span>

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a><span data-ttu-id="a7c53-134">Konkretne metody w interfejsach</span><span class="sxs-lookup"><span data-stu-id="a7c53-134">Concrete methods in interfaces</span></span>

<span data-ttu-id="a7c53-135">Najprostszą formą tej funkcji jest możliwość deklarowania *konkretnej metody* w interfejsie, który jest metodą z treścią.</span><span class="sxs-lookup"><span data-stu-id="a7c53-135">The simplest form of this feature is the ability to declare a *concrete method* in an interface, which is a method with a body.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

<span data-ttu-id="a7c53-136">Klasa implementująca ten interfejs nie musi implementować jej konkretnej metody.</span><span class="sxs-lookup"><span data-stu-id="a7c53-136">A class that implements this interface need not implement its concrete method.</span></span>

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

<span data-ttu-id="a7c53-137">Ostateczne przesłonięcie `IA.M` w klasie `C` jest konkretną metodą `M` zadeklarowaną w `IA`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-137">The final override for `IA.M` in class `C` is the concrete method `M` declared in `IA`.</span></span> <span data-ttu-id="a7c53-138">Należy zauważyć, że Klasa nie dziedziczy członków z jego interfejsów; to nie jest zmieniane przez tę funkcję:</span><span class="sxs-lookup"><span data-stu-id="a7c53-138">Note that a class does not inherit members from its interfaces; that is not changed by this feature:</span></span>

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

<span data-ttu-id="a7c53-139">W ramach elementu członkowskiego wystąpienia interfejs `this` ma typ otaczającego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a7c53-139">Within an instance member of an interface, `this` has the type of the enclosing interface.</span></span>

### <a name="modifiers-in-interfaces"></a><span data-ttu-id="a7c53-140">Modyfikatory w interfejsach</span><span class="sxs-lookup"><span data-stu-id="a7c53-140">Modifiers in interfaces</span></span>

<span data-ttu-id="a7c53-141">Składnia interfejsu jest swobodna, aby zezwalać na Modyfikatory elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="a7c53-141">The syntax for an interface is relaxed to permit modifiers on its members.</span></span> <span data-ttu-id="a7c53-142">Dozwolone są następujące elementy: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`i `partial`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-142">The following are permitted: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`, and `partial`.</span></span>

> <span data-ttu-id="a7c53-143">Do ***zrobienia***: Sprawdź, jakie istnieją inne modyfikatory.</span><span class="sxs-lookup"><span data-stu-id="a7c53-143">***TODO***: check what other modifiers exist.</span></span>

<span data-ttu-id="a7c53-144">Element członkowski interfejsu, którego deklaracja zawiera treść, jest składową `virtual`, chyba że jest używany modyfikator `sealed` lub `private`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-144">An interface member whose declaration includes a body is a `virtual` member unless the `sealed` or `private` modifier is used.</span></span> <span data-ttu-id="a7c53-145">Modyfikator `virtual` może być używany w składowej funkcji, która w przeciwnym razie mógłby być niejawnie `virtual`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-145">The `virtual` modifier may be used on a function member that would otherwise be implicitly `virtual`.</span></span> <span data-ttu-id="a7c53-146">Podobnie, chociaż `abstract` jest wartością domyślną w elementach członkowskich interfejsu bez treści, modyfikator ten może być jawnie określony.</span><span class="sxs-lookup"><span data-stu-id="a7c53-146">Similarly, although `abstract` is the default on interface members without bodies, that modifier may be given explicitly.</span></span> <span data-ttu-id="a7c53-147">Niewirtualny element członkowski można zadeklarować za pomocą słowa kluczowego `sealed`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-147">A non-virtual member may be declared using the `sealed` keyword.</span></span>

<span data-ttu-id="a7c53-148">Wystąpił błąd dla elementu członkowskiego `private` lub `sealed` funkcji w interfejsie, aby nie mieć treści.</span><span class="sxs-lookup"><span data-stu-id="a7c53-148">It is an error for a `private` or `sealed` function member of an interface to have no body.</span></span> <span data-ttu-id="a7c53-149">Element członkowski funkcji `private` nie może mieć modyfikatora `sealed`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-149">A `private` function member may not have the modifier `sealed`.</span></span>

<span data-ttu-id="a7c53-150">Modyfikatory dostępu mogą być używane w przypadku elementów członkowskich interfejsu wszystkich rodzajów elementów członkowskich, które są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="a7c53-150">Access modifiers may be used on interface members of all kinds of members that are permitted.</span></span> <span data-ttu-id="a7c53-151">Poziom dostępu `public` jest wartością domyślną, ale może być jawnie określony.</span><span class="sxs-lookup"><span data-stu-id="a7c53-151">The access level `public` is the default but it may be given explicitly.</span></span>

> <span data-ttu-id="a7c53-152">***Problem otwarty:*** Musimy określić precyzyjne znaczenie modyfikatorów dostępu, takich jak `protected` i `internal`, i które deklaracje i nie zastępować ich (w interfejsie pochodnym) ani implementować (w klasie implementującej interfejs).</span><span class="sxs-lookup"><span data-stu-id="a7c53-152">***Open Issue:*** We need to specify the precise meaning of the access modifiers such as `protected` and `internal`, and which declarations do and do not override them (in a derived interface) or implement them (in a class that implements the interface).</span></span>

<span data-ttu-id="a7c53-153">Interfejsy mogą deklarować składowe `static`, w tym typy zagnieżdżone, metody, indeksatory, właściwości, zdarzenia i konstruktory statyczne.</span><span class="sxs-lookup"><span data-stu-id="a7c53-153">Interfaces may declare `static` members, including nested types, methods, indexers, properties, events, and static constructors.</span></span> <span data-ttu-id="a7c53-154">Domyślny poziom dostępu dla wszystkich elementów członkowskich interfejsu jest `public`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-154">The default access level for all interface members is `public`.</span></span>

<span data-ttu-id="a7c53-155">Interfejsy nie mogą deklarować konstruktorów wystąpień, destruktorów ani pól.</span><span class="sxs-lookup"><span data-stu-id="a7c53-155">Interfaces may not declare instance constructors, destructors, or fields.</span></span>

> <span data-ttu-id="a7c53-156">***Zamknięty problem:*** Czy deklaracje operatora powinny być dozwolone w interfejsie?</span><span class="sxs-lookup"><span data-stu-id="a7c53-156">***Closed Issue:*** Should operator declarations be permitted in an interface?</span></span> <span data-ttu-id="a7c53-157">Prawdopodobnie nie są to operatory konwersji, ale co z innymi?</span><span class="sxs-lookup"><span data-stu-id="a7c53-157">Probably not conversion operators, but what about others?</span></span> <span data-ttu-id="a7c53-158">***Decyzja***: operatory są dozwolone *z wyjątkiem* operatorów konwersji, równości i nierówności.</span><span class="sxs-lookup"><span data-stu-id="a7c53-158">***Decision***: Operators are permitted *except* for conversion, equality, and inequality operators.</span></span>

> <span data-ttu-id="a7c53-159">***Zamknięty problem:*** Czy `new` być dozwolone w deklaracjach składowych interfejsu, które ukrywają elementy członkowskie z interfejsów podstawowych?</span><span class="sxs-lookup"><span data-stu-id="a7c53-159">***Closed Issue:*** Should `new` be permitted on interface member declarations that hide members from base interfaces?</span></span> <span data-ttu-id="a7c53-160">***Decyzja***: tak.</span><span class="sxs-lookup"><span data-stu-id="a7c53-160">***Decision***: Yes.</span></span>

> <span data-ttu-id="a7c53-161">***Zamknięty problem:*** Obecnie nie zezwalamy na `partial` w interfejsie ani jego elementach członkowskich.</span><span class="sxs-lookup"><span data-stu-id="a7c53-161">***Closed Issue:*** We do not currently permit `partial` on an interface or its members.</span></span> <span data-ttu-id="a7c53-162">To wymaga oddzielnej propozycji.</span><span class="sxs-lookup"><span data-stu-id="a7c53-162">That would require a separate proposal.</span></span> <span data-ttu-id="a7c53-163">***Decyzja***: tak.</span><span class="sxs-lookup"><span data-stu-id="a7c53-163">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a><span data-ttu-id="a7c53-164">Zastąpienia w interfejsach</span><span class="sxs-lookup"><span data-stu-id="a7c53-164">Overrides in interfaces</span></span>

<span data-ttu-id="a7c53-165">Deklaracje przesłonięcia (tj. te zawierające modyfikator `override`) umożliwiają programistom dostarczenie najbardziej szczegółowej implementacji wirtualnego elementu członkowskiego w interfejsie, w którym kompilator lub środowisko uruchomieniowe nie wyszuka jednego z nich.</span><span class="sxs-lookup"><span data-stu-id="a7c53-165">Override declarations (i.e. those containing the `override` modifier) allow the programmer to provide a most specific implementation of a virtual member in an interface where the compiler or runtime would not otherwise find one.</span></span> <span data-ttu-id="a7c53-166">Umożliwia również włączenie abstrakcyjnego elementu członkowskiego z interfejsu Super do domyślnego elementu członkowskiego w interfejsie pochodnym.</span><span class="sxs-lookup"><span data-stu-id="a7c53-166">It also allows turning an abstract member from a super-interface into a default member in a derived interface.</span></span> <span data-ttu-id="a7c53-167">Deklaracja przesłonięcia może *jawnie* przesłonić konkretną metodę interfejsu podstawowego przez zakwalifikowanie deklaracji z nazwą interfejsu (w tym przypadku nie jest dozwolony modyfikator dostępu).</span><span class="sxs-lookup"><span data-stu-id="a7c53-167">An override declaration is permitted to *explicitly* override a particular base interface method by qualifying the declaration with the interface name (no access modifier is permitted in this case).</span></span> <span data-ttu-id="a7c53-168">Niejawne zastąpienia są niedozwolone.</span><span class="sxs-lookup"><span data-stu-id="a7c53-168">Implicit overrides are not permitted.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

<span data-ttu-id="a7c53-169">Deklaracje przesłonięcia w interfejsach nie mogą być deklarowane `sealed`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-169">Override declarations in interfaces may not be declared `sealed`.</span></span>

<span data-ttu-id="a7c53-170">Elementy członkowskie funkcji publicznych `virtual` w interfejsie mogą zostać zastąpione jawnie w interfejsie pochodnym (przez zakwalifikowanie nazwy w deklaracji override z typem interfejsu, który pierwotnie zadeklarował metodę, i pominięciem modyfikatora dostępu).</span><span class="sxs-lookup"><span data-stu-id="a7c53-170">Public `virtual` function members in an interface may be overridden in a derived interface explicitly (by qualifying the name in the override declaration with the interface type that originally declared the method, and omitting an access modifier).</span></span>

<span data-ttu-id="a7c53-171">elementy członkowskie funkcji `virtual` w interfejsie mogą zostać zastąpione wyłącznie jawnie (niejawnie) w interfejsach pochodnych, a składowe, które nie są `public` mogą być implementowane tylko w klasie lub strukturze jawnie (niejawnie).</span><span class="sxs-lookup"><span data-stu-id="a7c53-171">`virtual` function members in an interface may only be overridden explicitly (not implicitly) in derived interfaces, and members that are not `public` may only be implemented in a class or struct explicitly (not implicitly).</span></span> <span data-ttu-id="a7c53-172">W obu przypadkach zastąpiony lub zaimplementowany element członkowski musi być *dostępny* , gdy zostanie przesłonięty.</span><span class="sxs-lookup"><span data-stu-id="a7c53-172">In either case, the overridden or implemented member must be *accessible* where it is overridden.</span></span>

### <a name="reabstraction"></a><span data-ttu-id="a7c53-173">Reabstraction</span><span class="sxs-lookup"><span data-stu-id="a7c53-173">Reabstraction</span></span>

<span data-ttu-id="a7c53-174">Metoda wirtualna (konkretna) zadeklarowana w interfejsie może zostać zastąpiona jako abstrakcyjna w interfejsie pochodnym</span><span class="sxs-lookup"><span data-stu-id="a7c53-174">A virtual (concrete) method declared in an interface may be overridden to be abstract in a derived interface</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

<span data-ttu-id="a7c53-175">Modyfikator `abstract` nie jest wymagany w deklaracji `IB.M` (jest to wartość domyślna w interfejsach), ale prawdopodobnie dobrym sposobem jest jawne w deklaracji przesłonięcia.</span><span class="sxs-lookup"><span data-stu-id="a7c53-175">The `abstract` modifier is not required in the declaration of `IB.M` (that is the default in interfaces), but it is probably good practice to be explicit in an override declaration.</span></span>

<span data-ttu-id="a7c53-176">Jest to przydatne w przypadku interfejsów pochodnych, w których domyślna implementacja metody jest nieodpowiedni i bardziej odpowiednia implementacja powinna być dostarczana przez implementację klas.</span><span class="sxs-lookup"><span data-stu-id="a7c53-176">This is useful in derived interfaces where the default implementation of a method is inappropriate and a more appropriate implementation should be provided by implementing classes.</span></span>

> <span data-ttu-id="a7c53-177">***Problem otwarty:*** Czy powinno być dozwolone przedzielenie?</span><span class="sxs-lookup"><span data-stu-id="a7c53-177">***Open Issue:*** Should reabstraction be permitted?</span></span>

### <a name="the-most-specific-override-rule"></a><span data-ttu-id="a7c53-178">Najbardziej Szczegółowa reguła przesłonięcia</span><span class="sxs-lookup"><span data-stu-id="a7c53-178">The most specific override rule</span></span>

<span data-ttu-id="a7c53-179">Firma Microsoft wymaga, aby każdy interfejs i Klasa mieli *najbardziej specyficzne przesłonięcie* dla każdego wirtualnego elementu członkowskiego spośród zastąpień występujących w typie lub jego interfejsie bezpośrednim i pośrednim.</span><span class="sxs-lookup"><span data-stu-id="a7c53-179">We require that every interface and class have a *most specific override* for every virtual member among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="a7c53-180">*Najbardziej konkretny przesłonięcie* jest unikatowym przesłonięciem, który jest bardziej szczegółowy niż każdy inny przesłonięcie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-180">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="a7c53-181">Jeśli nie ma żadnych zastąpień, sam element członkowski jest uznawany za najbardziej konkretne przesłonięcie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-181">If there is no override, the member itself is considered the most specific override.</span></span>

<span data-ttu-id="a7c53-182">`M1` przesłonięcia jest uznawana za *bardziej precyzyjną* niż inne `M2` przesłaniania, jeśli `M1` jest zadeklarowana dla typu `T1`, `M2` jest zadeklarowana dla typu `T2`i albo</span><span class="sxs-lookup"><span data-stu-id="a7c53-182">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>

1. <span data-ttu-id="a7c53-183">`T1` zawiera `T2` między interfejsami bezpośrednimi lub pośrednimi lub</span><span class="sxs-lookup"><span data-stu-id="a7c53-183">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
2. <span data-ttu-id="a7c53-184">`T2` jest typem interfejsu, ale `T1` nie jest typem interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a7c53-184">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="a7c53-185">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a7c53-185">For example:</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

<span data-ttu-id="a7c53-186">Najbardziej konkretna reguła przesłonięcia gwarantuje, że konflikt (tj. niejednoznaczności wynikające z dziedziczenia rombu) jest rozpoznawany jawnie przez programistę w punkcie, w którym występuje konflikt.</span><span class="sxs-lookup"><span data-stu-id="a7c53-186">The most specific override rule ensures that a conflict (i.e. an ambiguity arising from diamond inheritance) is resolved explicitly by the programmer at the point where the conflict arises.</span></span>

<span data-ttu-id="a7c53-187">Ponieważ obsługujemy jawne zastąpienia abstrakcyjne w interfejsach, można to zrobić również w klasach</span><span class="sxs-lookup"><span data-stu-id="a7c53-187">Because we support explicit abstract overrides in interfaces, we could do so in classes as well</span></span>

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> <span data-ttu-id="a7c53-188">***Problem z otwartym***: czy należy obsługiwać jawne zastąpienia abstrakcyjne interfejsu w klasach?</span><span class="sxs-lookup"><span data-stu-id="a7c53-188">***Open issue***: should we support explicit interface abstract overrides in classes?</span></span>

<span data-ttu-id="a7c53-189">Ponadto jest to błąd, jeśli w deklaracji klasy najbardziej specyficzne przesłonięcie jakiejś metody interfejsu jest abstrakcyjnym przesłonięciem zadeklarowanym w interfejsie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-189">In addition, it is an error if in a class declaration the most specific override of some interface method is an abstract override that was declared in an interface.</span></span> <span data-ttu-id="a7c53-190">Jest to istniejąca reguła przestanowa przy użyciu nowej terminologii.</span><span class="sxs-lookup"><span data-stu-id="a7c53-190">This is an existing rule restated using the new terminology.</span></span>

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

<span data-ttu-id="a7c53-191">Istnieje możliwość, że właściwość wirtualna zadeklarowana w interfejsie ma najbardziej specyficzne przesłonięcie dla metody dostępu `get` w jednym interfejsie i najbardziej konkretne przesłonięcie dla metody dostępu `set` w innym interfejsie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-191">It is possible for a virtual property declared in an interface to have a most specific override for its `get` accessor in one interface and a most specific override for its `set` accessor in a different interface.</span></span> <span data-ttu-id="a7c53-192">Jest to uznawane za naruszenie dla *najbardziej konkretnej reguły przesłonięcia* .</span><span class="sxs-lookup"><span data-stu-id="a7c53-192">This is considered a violation of the *most specific override* rule.</span></span>

### <a name="static-and-private-methods"></a><span data-ttu-id="a7c53-193">Metody `static` i `private`</span><span class="sxs-lookup"><span data-stu-id="a7c53-193">`static` and `private` methods</span></span>

<span data-ttu-id="a7c53-194">Ponieważ interfejsy mogą teraz zawierać kod wykonywalny, warto użyć abstrakcyjnego kodu do metod prywatnych i statycznych.</span><span class="sxs-lookup"><span data-stu-id="a7c53-194">Because interfaces may now contain executable code, it is useful to abstract common code into private and static methods.</span></span> <span data-ttu-id="a7c53-195">Teraz zezwalamy na te interfejsy.</span><span class="sxs-lookup"><span data-stu-id="a7c53-195">We now permit these in interfaces.</span></span>

> <span data-ttu-id="a7c53-196">***Problem zamknięty***: czy mamy obsługiwać metody prywatne?</span><span class="sxs-lookup"><span data-stu-id="a7c53-196">***Closed issue***: Should we support private methods?</span></span> <span data-ttu-id="a7c53-197">Czy mamy obsługiwać metody statyczne?</span><span class="sxs-lookup"><span data-stu-id="a7c53-197">Should we support static methods?</span></span> <span data-ttu-id="a7c53-198">**Decyzja: tak**</span><span class="sxs-lookup"><span data-stu-id="a7c53-198">**Decision: YES**</span></span>

> <span data-ttu-id="a7c53-199">***Problem otwarty***: czy mamy pozwolić na `protected` lub `internal` lub inny dostęp do metod interfejsu?</span><span class="sxs-lookup"><span data-stu-id="a7c53-199">***Open issue***: should we permit interface methods to be `protected` or `internal` or other access?</span></span> <span data-ttu-id="a7c53-200">Jeśli tak, jakie są semantyki?</span><span class="sxs-lookup"><span data-stu-id="a7c53-200">If so, what are the semantics?</span></span> <span data-ttu-id="a7c53-201">Czy są one `virtual` domyślnie?</span><span class="sxs-lookup"><span data-stu-id="a7c53-201">Are they `virtual` by default?</span></span> <span data-ttu-id="a7c53-202">Jeśli tak, czy istnieje sposób, aby nie były wirtualne?</span><span class="sxs-lookup"><span data-stu-id="a7c53-202">If so, is there a way to make them non-virtual?</span></span>

> <span data-ttu-id="a7c53-203">***Otwórz problem***: Jeśli obsługujemy metody statyczne, czy są obsługiwane (statyczne) operatory?</span><span class="sxs-lookup"><span data-stu-id="a7c53-203">***Open issue***: If we support static methods, should we support (static) operators?</span></span>

### <a name="base-interface-invocations"></a><span data-ttu-id="a7c53-204">Wywołania interfejsu podstawowego</span><span class="sxs-lookup"><span data-stu-id="a7c53-204">Base interface invocations</span></span>

<span data-ttu-id="a7c53-205">Kod w typie, który pochodzi od interfejsu z metodą domyślną, może jawnie wywołać implementację "Base" tego interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a7c53-205">Code in a type that derives from an interface with a default method can explicitly invoke that interface's "base" implementation.</span></span>

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

<span data-ttu-id="a7c53-206">Metoda wystąpienia (niestatycznego) może wywoływać implementację metody dostępnego wystąpienia w bezpośrednim interfejsie podstawowym, wpisując nazwę przy użyciu składni `base(Type).M`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-206">An instance (nonstatic) method is permitted to invoke the implementation of an accessible instance method in a direct base interface nonvirtually by naming it using the syntax `base(Type).M`.</span></span> <span data-ttu-id="a7c53-207">Jest to przydatne, gdy przesłonięcie wymagane do pomyślnego przeprowadzenia dziedziczenia jest rozpoznawane przez delegowanie do jednej konkretnej implementacji podstawowej.</span><span class="sxs-lookup"><span data-stu-id="a7c53-207">This is useful when an override that is required to be provided due to diamond inheritance is resolved by delegating to one particular base implementation.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

<span data-ttu-id="a7c53-208">Gdy dostęp do `virtual` lub `abstract` jest uzyskiwany przy użyciu składni `base(Type).M`, wymagane jest, aby `Type` zawierać unikatowy *przesłonięcie* dla `M`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-208">When a `virtual` or `abstract` member is accessed using the syntax `base(Type).M`, it is required that `Type` contains a unique *most specific override* for `M`.</span></span>

### <a name="binding-base-clauses"></a><span data-ttu-id="a7c53-209">Powiązane klauzule podstawowe</span><span class="sxs-lookup"><span data-stu-id="a7c53-209">Binding base clauses</span></span>

<span data-ttu-id="a7c53-210">Interfejsy zawierają teraz typy.</span><span class="sxs-lookup"><span data-stu-id="a7c53-210">Interfaces now contain types.</span></span>  <span data-ttu-id="a7c53-211">Te typy mogą być używane w klauzuli Base jako interfejsy podstawowe.</span><span class="sxs-lookup"><span data-stu-id="a7c53-211">These types may be used in the base clause as base interfaces.</span></span>  <span data-ttu-id="a7c53-212">W przypadku tworzenia powiązania z klauzulą podstawową może być konieczne poznanie zestawu interfejsów podstawowych, aby powiązać te typy (np. do wyszukania w nich i rozwiązania dostępu chronionego).</span><span class="sxs-lookup"><span data-stu-id="a7c53-212">When binding a base clause, we may need to know the set of base interfaces to bind those types (e.g. to lookup in them and to resolve protected access).</span></span>  <span data-ttu-id="a7c53-213">Znaczenie klauzuli podstawowej interfejsu jest zdefiniowane cyklicznie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-213">The meaning of an interface's base clause is thus circularly defined.</span></span>  <span data-ttu-id="a7c53-214">Aby przerwać cykl, dodamy nowe reguły językowe odpowiadające podobnej regule dla klas.</span><span class="sxs-lookup"><span data-stu-id="a7c53-214">To break the cycle, we add a new language rules corresponding to a similar rule already in place for classes.</span></span>

<span data-ttu-id="a7c53-215">Podczas określania znaczenia *interface_base* interfejsu, tymczasowo przyjęto, że interfejsy podstawowe są puste.</span><span class="sxs-lookup"><span data-stu-id="a7c53-215">While determining the meaning of the *interface_base* of an interface, the base interfaces are temporarily assumed to be empty.</span></span> <span data-ttu-id="a7c53-216">Intuicyjnie gwarantuje to, że znaczenie klauzuli podstawowej nie może rekursywnie zależeć od siebie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-216">Intuitively this ensures that the meaning of a base clause cannot recursively depend on itself.</span></span> 

<span data-ttu-id="a7c53-217">**Używamy następujących reguł:**</span><span class="sxs-lookup"><span data-stu-id="a7c53-217">**We used to have the following rules:**</span></span>

<span data-ttu-id="a7c53-218">"Gdy klasa B dziedziczy z klasy A, jest to błąd czasu kompilacji dla elementu, który jest zależny od B. Klasa **bezpośrednio zależy** od jej bezpośredniej klasy podstawowej (jeśli istnieje) i **bezpośrednio zależy** od ~~**klasy**~~ , w której jest od razu zagnieżdżona (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="a7c53-218">"When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the ~~**class**~~ within which it is immediately nested (if any).</span></span> <span data-ttu-id="a7c53-219">W związku z tą definicją kompletny zestaw ~~**klas**~~ , od których zależy Klasa, jest bezpośrednie i przechodnie zamknięcie **bezpośrednio zależy** od relacji. "</span><span class="sxs-lookup"><span data-stu-id="a7c53-219">Given this definition, the complete set of ~~**classes**~~ upon which a class depends is the reflexive and transitive closure of the **directly depends on** relationship."</span></span>

<span data-ttu-id="a7c53-220">Jest to błąd czasu kompilacji dla interfejsu, aby bezpośrednio lub pośrednio dziedziczyć po sobie samym.</span><span class="sxs-lookup"><span data-stu-id="a7c53-220">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>
<span data-ttu-id="a7c53-221">**Podstawowe interfejsy** interfejsu to jawne interfejsy podstawowe i ich interfejsy podstawowe.</span><span class="sxs-lookup"><span data-stu-id="a7c53-221">The **base interfaces** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="a7c53-222">Innymi słowy, zestaw interfejsów podstawowych to pełne przechodnie zamknięcie jawnych interfejsów podstawowych, ich jawne interfejsy podstawowe i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="a7c53-222">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span>

<span data-ttu-id="a7c53-223">**Są one dostosowywane w następujący sposób:**</span><span class="sxs-lookup"><span data-stu-id="a7c53-223">**We are adjusting them as follows:**</span></span>

<span data-ttu-id="a7c53-224">Gdy klasa B dziedziczy z klasy A, jest to błąd czasu kompilacji dla elementu, który jest zależny od B. Klasa **bezpośrednio zależy** od jej bezpośredniej klasy podstawowej (jeśli istnieje) i **bezpośrednio zależy** od _**typu**_ , w którym jest od razu zagnieżdżona (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="a7c53-224">When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the _**type**_ within which it is immediately nested (if any).</span></span>

<span data-ttu-id="a7c53-225">Gdy interfejs IB rozszerza interfejs IA, jest to błąd czasu kompilacji dla IA, który jest zależny od IB.</span><span class="sxs-lookup"><span data-stu-id="a7c53-225">When an interface IB extends an interface IA, it is a compile-time error for IA to depend on IB.</span></span> <span data-ttu-id="a7c53-226">Interfejs **bezpośrednio zależy** od jego bezpośrednich interfejsów podstawowych (jeśli istnieje) i **bezpośrednio zależy** od typu, w którym jest natychmiast zagnieżdżony (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="a7c53-226">An interface **directly depends on** its direct base interfaces (if any) and **directly depends on** the type within which it is immediately nested (if any).</span></span>

<span data-ttu-id="a7c53-227">Uwzględniając te definicje, kompletny zestaw **typów** , od którego zależy typ, jest zwrotne i przechodnie zamknięcie **bezpośrednie zależy** od relacji.</span><span class="sxs-lookup"><span data-stu-id="a7c53-227">Given these definitions, the complete set of **types** upon which a type depends is the reflexive and transitive closure of the **directly depends on** relationship.</span></span>

### <a name="effect-on-existing-programs"></a><span data-ttu-id="a7c53-228">Wpływ na istniejące programy</span><span class="sxs-lookup"><span data-stu-id="a7c53-228">Effect on existing programs</span></span>

<span data-ttu-id="a7c53-229">Przedstawione tutaj reguły nie mają wpływu na znaczenie istniejących programów.</span><span class="sxs-lookup"><span data-stu-id="a7c53-229">The rules presented here are intended to have no effect on the meaning of existing programs.</span></span>

<span data-ttu-id="a7c53-230">Przykład 1:</span><span class="sxs-lookup"><span data-stu-id="a7c53-230">Example 1:</span></span>

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

<span data-ttu-id="a7c53-231">Przykład 2:</span><span class="sxs-lookup"><span data-stu-id="a7c53-231">Example 2:</span></span>

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

<span data-ttu-id="a7c53-232">Te same reguły dają podobne wyniki do analogicznej sytuacji obejmującej domyślne metody interfejsu:</span><span class="sxs-lookup"><span data-stu-id="a7c53-232">The same rules give similar results to the analogous situation involving default interface methods:</span></span>

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> <span data-ttu-id="a7c53-233">***Problem zamknięty***: Upewnij się, że jest to zamierzenie zgodne ze specyfikacją.</span><span class="sxs-lookup"><span data-stu-id="a7c53-233">***Closed issue***: confirm that this is an intended consequence of the specification.</span></span> <span data-ttu-id="a7c53-234">**Decyzja: tak**</span><span class="sxs-lookup"><span data-stu-id="a7c53-234">**Decision: YES**</span></span>

### <a name="runtime-method-resolution"></a><span data-ttu-id="a7c53-235">Rozdzielczość metody środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="a7c53-235">Runtime method resolution</span></span>

> <span data-ttu-id="a7c53-236">***Zamknięty problem:*** Specyfikacja powinna opisywać algorytm rozpoznawania metody środowiska uruchomieniowego w miarę metod domyślnych interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a7c53-236">***Closed Issue:*** The spec should describe the runtime method resolution algorithm in the face of interface default methods.</span></span> <span data-ttu-id="a7c53-237">Musimy upewnić się, że semantyka jest spójna z semantyką języka, na przykład, które zadeklarowane metody nie zastępują ani nie implementują metody `internal`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-237">We need to ensure that the semantics are consistent with the language semantics, e.g. which declared methods do and do not override or implement an `internal` method.</span></span>

### <a name="clr-support-api"></a><span data-ttu-id="a7c53-238">Interfejs API obsługi CLR</span><span class="sxs-lookup"><span data-stu-id="a7c53-238">CLR support API</span></span>

<span data-ttu-id="a7c53-239">Aby kompilatory wykrywają, kiedy są kompilowane dla środowiska uruchomieniowego, które obsługuje tę funkcję, biblioteki dla takich środowisk uruchomieniowych są modyfikowane w celu anonsowania tego faktu przez interfejs API omówiony w <https://github.com/dotnet/corefx/issues/17116>.</span><span class="sxs-lookup"><span data-stu-id="a7c53-239">In order for compilers to detect when they are compiling for a runtime that supports this feature, libraries for such runtimes are modified to advertise that fact through the API discussed in <https://github.com/dotnet/corefx/issues/17116>.</span></span> <span data-ttu-id="a7c53-240">Dodamy</span><span class="sxs-lookup"><span data-stu-id="a7c53-240">We add</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> <span data-ttu-id="a7c53-241">***Problem otwarty***: czy Najlepsza nazwa funkcji *CLR* ?</span><span class="sxs-lookup"><span data-stu-id="a7c53-241">***Open issue***: Is that the best name for the *CLR* feature?</span></span> <span data-ttu-id="a7c53-242">Funkcja CLR wykonuje znacznie więcej niż tylko ten (np. ogranicza ograniczenia ochrony, obsługuje zastąpień w interfejsach itp.).</span><span class="sxs-lookup"><span data-stu-id="a7c53-242">The CLR feature does much more than just that (e.g. relaxes protection constraints, supports overrides in interfaces, etc).</span></span> <span data-ttu-id="a7c53-243">Być może powinna być wywoływana podobnie jak "konkretne metody w interfejsie" lub "cechy"?</span><span class="sxs-lookup"><span data-stu-id="a7c53-243">Perhaps it should be called something like "concrete methods in interfaces", or "traits"?</span></span>

### <a name="further-areas-to-be-specified"></a><span data-ttu-id="a7c53-244">Dalsze obszary do określenia</span><span class="sxs-lookup"><span data-stu-id="a7c53-244">Further areas to be specified</span></span>

- <span data-ttu-id="a7c53-245">[] Warto wykazać rodzaje źródłowych i binarnych efektów zgodności spowodowanych przez dodawanie domyślnych metod interfejsu i zastąpień do istniejących interfejsów.</span><span class="sxs-lookup"><span data-stu-id="a7c53-245">[ ] It would be useful to catalog the kinds of source and binary compatibility effects caused by adding default interface methods and overrides to existing interfaces.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="a7c53-246">Wady</span><span class="sxs-lookup"><span data-stu-id="a7c53-246">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a7c53-247">Ta propozycja wymaga skoordynowanej aktualizacji specyfikacji CLR (aby można było obsługiwać konkretne metody w interfejsach i rozpoznawaniu metod).</span><span class="sxs-lookup"><span data-stu-id="a7c53-247">This proposal requires a coordinated update to the CLR specification (to support concrete methods in interfaces and method resolution).</span></span> <span data-ttu-id="a7c53-248">Jest to dość stosunkowo "kosztowne" i może być warto w połączeniu z innymi funkcjami, które również przewidujemy, że zmiany środowiska CLR będą wymagały zmian.</span><span class="sxs-lookup"><span data-stu-id="a7c53-248">It is therefore fairly "expensive" and it may be worth doing in combination with other features that we also anticipate would require CLR changes.</span></span>

## <a name="alternatives"></a><span data-ttu-id="a7c53-249">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="a7c53-249">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a7c53-250">Brak.</span><span class="sxs-lookup"><span data-stu-id="a7c53-250">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="a7c53-251">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="a7c53-251">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="a7c53-252">Otwarte pytania są wywoływane w całej propozycji powyżej.</span><span class="sxs-lookup"><span data-stu-id="a7c53-252">Open questions are called out throughout the proposal, above.</span></span>
- <span data-ttu-id="a7c53-253">Aby uzyskać listę otwartych pytań, zobacz również <https://github.com/dotnet/csharplang/issues/406>.</span><span class="sxs-lookup"><span data-stu-id="a7c53-253">See also <https://github.com/dotnet/csharplang/issues/406> for a list of open questions.</span></span>
- <span data-ttu-id="a7c53-254">Szczegółowa Specyfikacja musi opisywać mechanizm rozpoznawania używany w czasie wykonywania w celu wybrania precyzyjnej metody do wywołania.</span><span class="sxs-lookup"><span data-stu-id="a7c53-254">The detailed specification must describe the resolution mechanism used at runtime to select the precise method to be invoked.</span></span>
- <span data-ttu-id="a7c53-255">Interakcje metadanych tworzonych przez nowe kompilatory i zużywane przez starsze kompilatory muszą być szczegółowo opracowywane.</span><span class="sxs-lookup"><span data-stu-id="a7c53-255">The interaction of metadata produced by new compilers and consumed by older compilers needs to be worked out in detail.</span></span> <span data-ttu-id="a7c53-256">Na przykład musimy upewnić się, że reprezentacja metadanych, której używamy, nie powoduje dodania domyślnej implementacji w interfejsie, aby przerwać istniejącą klasę, która implementuje ten interfejs w przypadku skompilowania przez starszy kompilator.</span><span class="sxs-lookup"><span data-stu-id="a7c53-256">For example, we need to ensure that the metadata representation that we use does not cause the addition of a default implementation in an interface to break an existing class that implements that interface when compiled by an older compiler.</span></span> <span data-ttu-id="a7c53-257">Może to mieć wpływ na reprezentację metadanych, której można użyć.</span><span class="sxs-lookup"><span data-stu-id="a7c53-257">This may affect the metadata representation that we can use.</span></span>
- <span data-ttu-id="a7c53-258">Projekt musi uwzględniać współdziałanie z innymi językami i istniejącymi kompilatorami dla innych języków.</span><span class="sxs-lookup"><span data-stu-id="a7c53-258">The design must consider interoperation with other languages and existing compilers for other languages.</span></span>

## <a name="resolved-questions"></a><span data-ttu-id="a7c53-259">Rozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="a7c53-259">Resolved Questions</span></span>

### <a name="abstract-override"></a><span data-ttu-id="a7c53-260">Przesłonięcie abstrakcyjne</span><span class="sxs-lookup"><span data-stu-id="a7c53-260">Abstract Override</span></span>

<span data-ttu-id="a7c53-261">Wcześniejsza Specyfikacja robocza zawiera możliwość "reabstract" dziedziczonej metody:</span><span class="sxs-lookup"><span data-stu-id="a7c53-261">The earlier draft spec contained the ability to "reabstract" an inherited method:</span></span>

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

<span data-ttu-id="a7c53-262">Moje uwagi dla 2017-03-20 wykazały, że nie postanowiły tego zezwolenia.</span><span class="sxs-lookup"><span data-stu-id="a7c53-262">My notes for 2017-03-20 showed that we decided not to allow this.</span></span> <span data-ttu-id="a7c53-263">Jednak istnieją co najmniej dwa przypadki użycia:</span><span class="sxs-lookup"><span data-stu-id="a7c53-263">However, there are at least two use cases for it:</span></span>

1. <span data-ttu-id="a7c53-264">Interfejsy API języka Java, z którymi niektórzy użytkownicy tej funkcji mają nadzieję, że zależą od tej możliwości.</span><span class="sxs-lookup"><span data-stu-id="a7c53-264">The Java APIs, with which some users of this feature hope to interoperate, depend on this facility.</span></span>
2. <span data-ttu-id="a7c53-265">Programowanie z *cechami* korzyści z tego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="a7c53-265">Programming with *traits* benefits from this.</span></span> <span data-ttu-id="a7c53-266">Reabstraction jest jednym z elementów funkcji języka "cechy" (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span><span class="sxs-lookup"><span data-stu-id="a7c53-266">Reabstraction is one of the elements of the "traits" language feature (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span></span> <span data-ttu-id="a7c53-267">W przypadku klas są dozwolone następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="a7c53-267">The following is permitted with classes:</span></span>

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

<span data-ttu-id="a7c53-268">Niestety, ten kod nie może być refaktoryzacją jako zestaw interfejsów (cech), chyba że jest to dozwolone.</span><span class="sxs-lookup"><span data-stu-id="a7c53-268">Unfortunately this code cannot be refactored as a set of interfaces (traits) unless this is permitted.</span></span> <span data-ttu-id="a7c53-269">Zgodnie z *regułą Jared Greed*, powinno być dozwolone.</span><span class="sxs-lookup"><span data-stu-id="a7c53-269">By the *Jared principle of greed*, it should be permitted.</span></span>

> <span data-ttu-id="a7c53-270">***Zamknięty problem:*** Czy powinno być dozwolone przedzielenie?</span><span class="sxs-lookup"><span data-stu-id="a7c53-270">***Closed issue:*** Should reabstraction be permitted?</span></span> <span data-ttu-id="a7c53-271">OPCJĘ Moje notatki były nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="a7c53-271">[YES] My notes were wrong.</span></span> <span data-ttu-id="a7c53-272">W [informacjach](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) o programie LDM należy powiedzieć, że w interfejsie jest dozwolone reabstrakcja.</span><span class="sxs-lookup"><span data-stu-id="a7c53-272">The [LDM notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) say that reabstraction is permitted in an interface.</span></span> <span data-ttu-id="a7c53-273">Nie w klasie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-273">Not in a class.</span></span>

### <a name="virtual-modifier-vs-sealed-modifier"></a><span data-ttu-id="a7c53-274">Modyfikator wirtualny vs Sealed</span><span class="sxs-lookup"><span data-stu-id="a7c53-274">Virtual Modifier vs Sealed Modifier</span></span>

<span data-ttu-id="a7c53-275">Z [Aleksey Tsingauz](https://github.com/AlekseyTs):</span><span class="sxs-lookup"><span data-stu-id="a7c53-275">From [Aleksey Tsingauz](https://github.com/AlekseyTs):</span></span>

> <span data-ttu-id="a7c53-276">Postanowiono zezwolić na jawne określenie modyfikatorów w elementach członkowskich interfejsu, chyba że istnieje powód, aby nie zezwalać na niektóre z nich.</span><span class="sxs-lookup"><span data-stu-id="a7c53-276">We decided to allow modifiers explicitly stated on interface members, unless there is a reason to disallow some of them.</span></span> <span data-ttu-id="a7c53-277">Pozwala to na interesujące pytanie wokół modyfikatora wirtualnego.</span><span class="sxs-lookup"><span data-stu-id="a7c53-277">This brings an interesting question around virtual modifier.</span></span> <span data-ttu-id="a7c53-278">Czy powinna być wymagana dla członków z implementacją domyślną?</span><span class="sxs-lookup"><span data-stu-id="a7c53-278">Should it be required on members with default implementation?</span></span>
>
> <span data-ttu-id="a7c53-279">Możemy powiedzieć, że:</span><span class="sxs-lookup"><span data-stu-id="a7c53-279">We could say that:</span></span>
>
> - <span data-ttu-id="a7c53-280">Jeśli nie ma żadnej implementacji i nie określono żadnych wirtualnych ani zapieczętowanych, Załóżmy, że element członkowski jest abstrakcyjny.</span><span class="sxs-lookup"><span data-stu-id="a7c53-280">if there is no implementation and neither virtual, nor sealed are specified, we assume the member is abstract.</span></span>
> - <span data-ttu-id="a7c53-281">Jeśli istnieje implementacja i nie określono elementu abstract ani zapieczętowanego, Załóżmy, że element członkowski jest wirtualny.</span><span class="sxs-lookup"><span data-stu-id="a7c53-281">if there is an implementation and neither abstract, nor sealed are specified, we assume the member is virtual.</span></span>
> - <span data-ttu-id="a7c53-282">modyfikator zapieczętowany jest wymagany do, aby metoda nie była wirtualna ani abstrakcyjna.</span><span class="sxs-lookup"><span data-stu-id="a7c53-282">sealed modifier is required to make a method neither virtual, nor abstract.</span></span>
>
> <span data-ttu-id="a7c53-283">Alternatywnie możemy powiedzieć, że modyfikator wirtualny jest wymagany dla wirtualnego elementu członkowskiego.</span><span class="sxs-lookup"><span data-stu-id="a7c53-283">Alternatively, we could say that virtual modifier is required for a virtual member.</span></span> <span data-ttu-id="a7c53-284">Oznacza to, że jeśli istnieje członek z implementacją, która nie jest jawnie oznaczona modyfikatorem wirtualnym, nie jest to wirtualne ani abstrakcyjne.</span><span class="sxs-lookup"><span data-stu-id="a7c53-284">I.e, if there is a member with implementation not explicitly marked with virtual modifier, it is neither virtual, nor abstract.</span></span> <span data-ttu-id="a7c53-285">Takie podejście może zapewnić lepszą wydajność, gdy metoda jest przenoszona z klasy do interfejsu:</span><span class="sxs-lookup"><span data-stu-id="a7c53-285">This approach might provide better experience when a method is moved from a class to an interface:</span></span>
>
> - <span data-ttu-id="a7c53-286">metoda abstrakcyjna pozostaje abstrakcyjna.</span><span class="sxs-lookup"><span data-stu-id="a7c53-286">an abstract method stays abstract.</span></span>
> - <span data-ttu-id="a7c53-287">Metoda wirtualna pozostaje wirtualna.</span><span class="sxs-lookup"><span data-stu-id="a7c53-287">a virtual method stays virtual.</span></span>
> - <span data-ttu-id="a7c53-288">Metoda bez modyfikatora nie pozostaje ani wirtualna, ani abstrakcyjna.</span><span class="sxs-lookup"><span data-stu-id="a7c53-288">a method without any modifier stays neither virtual, nor abstract.</span></span>
> - <span data-ttu-id="a7c53-289">modyfikator zapieczętowany nie może zostać zastosowany do metody, która nie jest przesłonięciem.</span><span class="sxs-lookup"><span data-stu-id="a7c53-289">sealed modifier cannot be applied to a method that is not an override.</span></span>
>
> <span data-ttu-id="a7c53-290">Co myślisz?</span><span class="sxs-lookup"><span data-stu-id="a7c53-290">What do you think?</span></span>

> <span data-ttu-id="a7c53-291">***Zamknięty problem:*** Czy konkretna metoda (z implementacją) ma być niejawnie `virtual`?</span><span class="sxs-lookup"><span data-stu-id="a7c53-291">***Closed Issue:*** Should a concrete method (with implementation) be implicitly `virtual`?</span></span> <span data-ttu-id="a7c53-292">OPCJĘ</span><span class="sxs-lookup"><span data-stu-id="a7c53-292">[YES]</span></span>

<span data-ttu-id="a7c53-293">***Decyzje:*** Wprowadzone w programie LDM 2017-04-05:</span><span class="sxs-lookup"><span data-stu-id="a7c53-293">***Decisions:*** Made in the LDM 2017-04-05:</span></span>

1. <span data-ttu-id="a7c53-294">niewirtualna powinna być jawnie wyrażona za poorednictwem `sealed` lub `private`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-294">non-virtual should be explicitly expressed through `sealed` or `private`.</span></span>
2. <span data-ttu-id="a7c53-295">`sealed` jest słowem kluczowym, aby utworzyć elementy członkowskie wystąpienia interfejsu z niewirtualnymi jednostkami</span><span class="sxs-lookup"><span data-stu-id="a7c53-295">`sealed` is the keyword to make interface instance members with bodies non-virtual</span></span>
3. <span data-ttu-id="a7c53-296">Chcemy zezwolić na wszystkie Modyfikatory w interfejsach</span><span class="sxs-lookup"><span data-stu-id="a7c53-296">We want to allow all modifiers in interfaces</span></span>  
4. <span data-ttu-id="a7c53-297">Domyślna dostępność dla członków interfejsu jest publiczna, w tym typy zagnieżdżone</span><span class="sxs-lookup"><span data-stu-id="a7c53-297">Default accessibility for interface members is public, including nested types</span></span>
5. <span data-ttu-id="a7c53-298">prywatne elementy członkowskie funkcji w interfejsach są niejawnie zapieczętowane, a `sealed` nie są na nich dozwolone.</span><span class="sxs-lookup"><span data-stu-id="a7c53-298">private function members in interfaces are implicitly sealed, and `sealed` is not permitted on them.</span></span>
6. <span data-ttu-id="a7c53-299">Klasy prywatne (w interfejsach) są dozwolone i mogą być zapieczętowane, co oznacza zapieczętowany w klasie Sense zapieczętowany.</span><span class="sxs-lookup"><span data-stu-id="a7c53-299">Private classes (in interfaces) are permitted and can be sealed, and that means sealed in the class sense of sealed.</span></span>
7. <span data-ttu-id="a7c53-300">Nieobecny wniosek, część częściowa nadal nie jest dozwolona w przypadku interfejsów lub ich członków.</span><span class="sxs-lookup"><span data-stu-id="a7c53-300">Absent a good proposal, partial is still not allowed on interfaces or their members.</span></span>

### <a name="binary-compatibility-1"></a><span data-ttu-id="a7c53-301">Zgodność binarna 1</span><span class="sxs-lookup"><span data-stu-id="a7c53-301">Binary Compatibility 1</span></span>

<span data-ttu-id="a7c53-302">Gdy biblioteka zawiera implementację domyślną</span><span class="sxs-lookup"><span data-stu-id="a7c53-302">When a library provides a default implementation</span></span>

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

<span data-ttu-id="a7c53-303">Rozumiemy, że implementacja `I1.M` w `C` jest `I1.M`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-303">We understand that the implementation of `I1.M` in `C` is `I1.M`.</span></span> <span data-ttu-id="a7c53-304">Co zrobić, jeśli zestaw zawierający `I2` został zmieniony w następujący sposób i ponownie skompilowany</span><span class="sxs-lookup"><span data-stu-id="a7c53-304">What if the assembly containing `I2` is changed as follows and recompiled</span></span>

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

<span data-ttu-id="a7c53-305">ale `C` nie jest ponownie kompilowana.</span><span class="sxs-lookup"><span data-stu-id="a7c53-305">but `C` is not recompiled.</span></span> <span data-ttu-id="a7c53-306">Co się stanie po uruchomieniu programu?</span><span class="sxs-lookup"><span data-stu-id="a7c53-306">What happens when the program is run?</span></span> <span data-ttu-id="a7c53-307">Wywołanie `(C as I1).M()`</span><span class="sxs-lookup"><span data-stu-id="a7c53-307">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="a7c53-308">Działa `I1.M`</span><span class="sxs-lookup"><span data-stu-id="a7c53-308">Runs `I1.M`</span></span>
2. <span data-ttu-id="a7c53-309">Działa `I2.M`</span><span class="sxs-lookup"><span data-stu-id="a7c53-309">Runs `I2.M`</span></span>
3. <span data-ttu-id="a7c53-310">Zgłasza jakiś rodzaj błędu czasu wykonywania</span><span class="sxs-lookup"><span data-stu-id="a7c53-310">Throws some kind of runtime error</span></span>

<span data-ttu-id="a7c53-311">***Decyzja:*** 2017-04-11: działa `I2.M`, co jest jednoznacznym bardziej szczegółowym przesłonięciem w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="a7c53-311">***Decision:*** Made 2017-04-11: Runs `I2.M`, which is the unambiguously most specific override at runtime.</span></span>

### <a name="event-accessors-closed"></a><span data-ttu-id="a7c53-312">Metody dostępu zdarzeń (zamknięte)</span><span class="sxs-lookup"><span data-stu-id="a7c53-312">Event accessors (closed)</span></span>

> <span data-ttu-id="a7c53-313">***Zamknięty problem:*** Czy można zastąpić zdarzenie "rozkład elementowy"?</span><span class="sxs-lookup"><span data-stu-id="a7c53-313">***Closed Issue:*** Can an event be overridden "piecewise"?</span></span>

<span data-ttu-id="a7c53-314">Rozważmy ten przypadek:</span><span class="sxs-lookup"><span data-stu-id="a7c53-314">Consider this case:</span></span>

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

<span data-ttu-id="a7c53-315">Ta "częściowa" implementacja zdarzenia nie jest dozwolona, ponieważ, jak w klasie, Składnia deklaracji zdarzenia nie zezwala na tylko jeden akcesor; należy podać oba (lub nie).</span><span class="sxs-lookup"><span data-stu-id="a7c53-315">This "partial" implementation of the event is not permitted because, as in a class, the syntax for an event declaration does not permit only one accessor; both (or neither) must be provided.</span></span> <span data-ttu-id="a7c53-316">Można to zrobić w ten sam sposób, zezwalając na abstrakcyjną metodę usuwania w składni, aby była niejawnie abstrakcyjna przez nieobecność treści:</span><span class="sxs-lookup"><span data-stu-id="a7c53-316">You could accomplish the same thing by permitting the abstract remove accessor in the syntax to be implicitly abstract by the absence of a body:</span></span>

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

<span data-ttu-id="a7c53-317">Należy zauważyć, że *jest to nowa (proponowana) składnia*.</span><span class="sxs-lookup"><span data-stu-id="a7c53-317">Note that *this is a new (proposed) syntax*.</span></span> <span data-ttu-id="a7c53-318">W bieżącej gramatyki metody dostępu do zdarzeń mają obowiązkową treść.</span><span class="sxs-lookup"><span data-stu-id="a7c53-318">In the current grammar, event accessors have a mandatory body.</span></span>

> <span data-ttu-id="a7c53-319">***Zamknięty problem:*** Czy metoda dostępu do zdarzenia jest abstrakcyjna (niejawnie) przez pominięcie treści, podobnie jak metody w interfejsach i Akcesory właściwości są abstrakcyjne (niejawnie) przez pominięcie treści?</span><span class="sxs-lookup"><span data-stu-id="a7c53-319">***Closed Issue:*** Can an event accessor be (implicitly) abstract by the omission of a body, similarly to the way that methods in interfaces and property accessors are (implicitly) abstract by the omission of a body?</span></span>

<span data-ttu-id="a7c53-320">***Decyzja:*** (2017-04-18) nie, deklaracje zdarzeń wymagają zarówno konkretnych metod dostępu, jak i żadnego z nich.</span><span class="sxs-lookup"><span data-stu-id="a7c53-320">***Decision:*** (2017-04-18) No, event declarations require both concrete accessors (or neither).</span></span>

### <a name="reabstraction-in-a-class-closed"></a><span data-ttu-id="a7c53-321">Reabstract w klasie (zamknięty)</span><span class="sxs-lookup"><span data-stu-id="a7c53-321">Reabstraction in a Class (closed)</span></span>

<span data-ttu-id="a7c53-322">***Zamknięty problem:*** Należy upewnić się, że jest to dozwolone (w przeciwnym razie dodanie domyślnej implementacji będzie istotną zmianą):</span><span class="sxs-lookup"><span data-stu-id="a7c53-322">***Closed Issue:*** We should confirm that this is permitted (otherwise adding a default implementation would be a breaking change):</span></span>

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

<span data-ttu-id="a7c53-323">***Decyzja:*** (2017-04-18) tak, dodanie treści do deklaracji elementu członkowskiego interfejsu nie powinno powodować podzielenia dysku C.</span><span class="sxs-lookup"><span data-stu-id="a7c53-323">***Decision:*** (2017-04-18) Yes, adding a body to an interface member declaration shouldn't break C.</span></span>

### <a name="sealed-override-closed"></a><span data-ttu-id="a7c53-324">Zapieczętowane przesłonięcie (zamknięte)</span><span class="sxs-lookup"><span data-stu-id="a7c53-324">Sealed Override (closed)</span></span>

<span data-ttu-id="a7c53-325">W powyższym pytaniu niejawnie przyjęto, że modyfikator `sealed` można zastosować do `override` w interfejsie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-325">The previous question implicitly assumes that the `sealed` modifier can be applied to an `override` in an interface.</span></span> <span data-ttu-id="a7c53-326">Jest to sprzeczne ze specyfikacją wersji roboczej.</span><span class="sxs-lookup"><span data-stu-id="a7c53-326">This contradicts the draft specification.</span></span> <span data-ttu-id="a7c53-327">Czy chcemy zezwolić na zastępowanie zastąpień?</span><span class="sxs-lookup"><span data-stu-id="a7c53-327">Do we want to permit sealing an override?</span></span> <span data-ttu-id="a7c53-328">Należy wziąć pod uwagę źródłową i binarną efekty zgodności zamknięć.</span><span class="sxs-lookup"><span data-stu-id="a7c53-328">Source and binary compatibility effects of sealing should be considered.</span></span>

> <span data-ttu-id="a7c53-329">***Zamknięty problem:*** Czy zezwalamy na zastępowanie zastąpień?</span><span class="sxs-lookup"><span data-stu-id="a7c53-329">***Closed Issue:*** Should we permit sealing an override?</span></span>

<span data-ttu-id="a7c53-330">***Decyzja:*** (2017-04-18) nie można `sealed` na zastąpień w interfejsach.</span><span class="sxs-lookup"><span data-stu-id="a7c53-330">***Decision:*** (2017-04-18) Let's not allowed `sealed` on overrides in interfaces.</span></span> <span data-ttu-id="a7c53-331">Jedynym zastosowaniem `sealed` w składowych interfejsu jest to, aby były one niewirtualne w swojej wstępnej deklaracji.</span><span class="sxs-lookup"><span data-stu-id="a7c53-331">The only use of `sealed` on interface members is to make them non-virtual in their initial declaration.</span></span>

### <a name="diamond-inheritance-and-classes-closed"></a><span data-ttu-id="a7c53-332">Dziedziczenie i klasy rombów (zamknięte)</span><span class="sxs-lookup"><span data-stu-id="a7c53-332">Diamond inheritance and classes (closed)</span></span>

<span data-ttu-id="a7c53-333">Wersja robocza propozycji preferuje zastąpienia klas przesłania do interfejsów w scenariuszach dziedziczenia rombu:</span><span class="sxs-lookup"><span data-stu-id="a7c53-333">The draft of the proposal prefers class overrides to interface overrides in diamond inheritance scenarios:</span></span>

> <span data-ttu-id="a7c53-334">Firma Microsoft wymaga, aby każdy interfejs i Klasa miały *najbardziej specyficzne przesłonięcia* dla każdej metody interfejsu w zastąpieniach występujących w typie lub jego bezpośrednich i pośrednich interfejsach.</span><span class="sxs-lookup"><span data-stu-id="a7c53-334">We require that every interface and class have a *most specific override* for every interface method among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="a7c53-335">*Najbardziej konkretny przesłonięcie* jest unikatowym przesłonięciem, który jest bardziej szczegółowy niż każdy inny przesłonięcie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-335">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="a7c53-336">Jeśli nie ma żadnych zastąpień, sama metoda jest uznawana za najbardziej konkretne przesłonięcie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-336">If there is no override, the method itself is considered the most specific override.</span></span>
>
> <span data-ttu-id="a7c53-337">`M1` przesłonięcia jest uznawana za *bardziej precyzyjną* niż inne `M2` przesłaniania, jeśli `M1` jest zadeklarowana dla typu `T1`, `M2` jest zadeklarowana dla typu `T2`i albo</span><span class="sxs-lookup"><span data-stu-id="a7c53-337">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>
>
> 1. <span data-ttu-id="a7c53-338">`T1` zawiera `T2` między interfejsami bezpośrednimi lub pośrednimi lub</span><span class="sxs-lookup"><span data-stu-id="a7c53-338">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
> 2. <span data-ttu-id="a7c53-339">`T2` jest typem interfejsu, ale `T1` nie jest typem interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a7c53-339">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="a7c53-340">Ten scenariusz jest</span><span class="sxs-lookup"><span data-stu-id="a7c53-340">The scenario is this</span></span>

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

<span data-ttu-id="a7c53-341">Należy potwierdzić to zachowanie (lub wybrać inny sposób)</span><span class="sxs-lookup"><span data-stu-id="a7c53-341">We should confirm this behavior (or decide otherwise)</span></span>

> <span data-ttu-id="a7c53-342">***Zamknięty problem:*** Potwierdź wersję roboczą powyżej, aby uzyskać *najbardziej szczegółowe przesłonięcie* , ponieważ dotyczy ona klas i interfejsów mieszanych (Klasa ma priorytet nad interfejsem).</span><span class="sxs-lookup"><span data-stu-id="a7c53-342">***Closed Issue:*** Confirm the draft spec, above, for *most specific override* as it applies to mixed classes and interfaces (a class takes priority over an interface).</span></span> <span data-ttu-id="a7c53-343">Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span><span class="sxs-lookup"><span data-stu-id="a7c53-343">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span></span>

### <a name="interface-methods-vs-structs-closed"></a><span data-ttu-id="a7c53-344">Metody interfejsu a struktury (zamknięte)</span><span class="sxs-lookup"><span data-stu-id="a7c53-344">Interface methods vs structs (closed)</span></span>

<span data-ttu-id="a7c53-345">Istnieją między innymi interakcje między domyślnymi metodami interfejsu i strukturami.</span><span class="sxs-lookup"><span data-stu-id="a7c53-345">There are some unfortunate interactions between default interface methods and structs.</span></span>

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

<span data-ttu-id="a7c53-346">Należy zauważyć, że elementy członkowskie interfejsu nie są dziedziczone:</span><span class="sxs-lookup"><span data-stu-id="a7c53-346">Note that interface members are not inherited:</span></span>

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

<span data-ttu-id="a7c53-347">W związku z tym klient musi mieć pole struktury, aby wywołać metody interfejsu</span><span class="sxs-lookup"><span data-stu-id="a7c53-347">Consequently, the client must box the struct to invoke interface methods</span></span>

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

<span data-ttu-id="a7c53-348">Opakowanie w ten sposób obniża główne korzyści typu `struct`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-348">Boxing in this way defeats the principal benefits of a `struct` type.</span></span> <span data-ttu-id="a7c53-349">Ponadto wszelkie metody mutacji nie będą miały żadnego efektu, ponieważ działają na *kopii w ramce* struktury:</span><span class="sxs-lookup"><span data-stu-id="a7c53-349">Moreover, any mutation methods will have no apparent effect, because they are operating on a *boxed copy* of the struct:</span></span>

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> <span data-ttu-id="a7c53-350">***Zamknięty problem:*** Co można zrobić:</span><span class="sxs-lookup"><span data-stu-id="a7c53-350">***Closed Issue:*** What can we do about this:</span></span>
>
> 1. <span data-ttu-id="a7c53-351">Zabroń `struct` dziedziczyć implementację domyślną.</span><span class="sxs-lookup"><span data-stu-id="a7c53-351">Forbid a `struct` from inheriting a default implementation.</span></span> <span data-ttu-id="a7c53-352">Wszystkie metody interfejsu byłyby traktowane jako abstrakcyjne w `struct`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-352">All interface methods would be treated as abstract in a `struct`.</span></span> <span data-ttu-id="a7c53-353">Następnie może upłynąć trochę czasu, aby zdecydować, jak lepiej ją ulepszyć.</span><span class="sxs-lookup"><span data-stu-id="a7c53-353">Then we may take time later to decide how to make it work better.</span></span>
> 2. <span data-ttu-id="a7c53-354">Weź pod pewnym rodzaj strategii generowania kodu, który unika pakowania.</span><span class="sxs-lookup"><span data-stu-id="a7c53-354">Come up with some kind of code generation strategy that avoids boxing.</span></span> <span data-ttu-id="a7c53-355">Wewnątrz metody, takiej jak `IB.Increment`, typ `this` może być zbliżone do parametru typu ograniczonego do `IB`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-355">Inside a method like `IB.Increment`, the type of `this` would perhaps be akin to a type parameter constrained to `IB`.</span></span> <span data-ttu-id="a7c53-356">W połączeniu z tym, aby uniknąć pakowania w obiekt wywołujący, metody nieabstrakcyjne byłyby dziedziczone z interfejsów.</span><span class="sxs-lookup"><span data-stu-id="a7c53-356">In conjunction with that, to avoid boxing in the caller, non-abstract methods would be inherited from interfaces.</span></span> <span data-ttu-id="a7c53-357">Może to spowodować znaczne zwiększenie działania kompilatora i środowiska CLR.</span><span class="sxs-lookup"><span data-stu-id="a7c53-357">This may increase compiler and CLR implementation work substantially.</span></span>
> 3. <span data-ttu-id="a7c53-358">Nie martw się o nią i po prostu pozostaw ją jako wartą.</span><span class="sxs-lookup"><span data-stu-id="a7c53-358">Not worry about it and just leave it as a wart.</span></span>
> 4. <span data-ttu-id="a7c53-359">Inne pomysły?</span><span class="sxs-lookup"><span data-stu-id="a7c53-359">Other ideas?</span></span>

<span data-ttu-id="a7c53-360">***Decyzja:*** Nie martw się o nią i po prostu pozostaw ją jako wartą.</span><span class="sxs-lookup"><span data-stu-id="a7c53-360">***Decision:*** Not worry about it and just leave it as a wart.</span></span> <span data-ttu-id="a7c53-361">Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span><span class="sxs-lookup"><span data-stu-id="a7c53-361">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span></span>

### <a name="base-interface-invocations-closed"></a><span data-ttu-id="a7c53-362">Wywołania interfejsu podstawowego (zamknięte)</span><span class="sxs-lookup"><span data-stu-id="a7c53-362">Base interface invocations (closed)</span></span>

<span data-ttu-id="a7c53-363">Specyfikacja wersja robocza sugeruje składnię dla wywołań interfejsu podstawowego inspirowanych przez język Java: `Interface.base.M()`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-363">The draft spec suggests a syntax for base interface invocations inspired by Java: `Interface.base.M()`.</span></span> <span data-ttu-id="a7c53-364">Musimy wybrać składnię co najmniej dla początkowego prototypu.</span><span class="sxs-lookup"><span data-stu-id="a7c53-364">We need to select a syntax, at least for the initial prototype.</span></span> <span data-ttu-id="a7c53-365">Moją ulubioną `base<Interface>.M()`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-365">My favorite is `base<Interface>.M()`.</span></span>

> <span data-ttu-id="a7c53-366">***Zamknięty problem:*** Jaka jest składnia podstawowego wywołania elementu członkowskiego?</span><span class="sxs-lookup"><span data-stu-id="a7c53-366">***Closed Issue:*** What is the syntax for a base member invocation?</span></span>

<span data-ttu-id="a7c53-367">***Decyzja:*** Składnia jest `base(Interface).M()`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-367">***Decision:*** The syntax is `base(Interface).M()`.</span></span> <span data-ttu-id="a7c53-368">Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span><span class="sxs-lookup"><span data-stu-id="a7c53-368">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span></span> <span data-ttu-id="a7c53-369">Interfejs tak nazwany musi być interfejsem podstawowym, ale nie musi być bezpośrednim interfejsem podstawowym.</span><span class="sxs-lookup"><span data-stu-id="a7c53-369">The interface so named must be a base interface, but does not need to be a direct base interface.</span></span>

> <span data-ttu-id="a7c53-370">***Problem otwarty:*** Czy w składowych klasy są dozwolone wywołania interfejsu podstawowego?</span><span class="sxs-lookup"><span data-stu-id="a7c53-370">***Open Issue:*** Should base interface invocations be permitted in class members?</span></span>

<span data-ttu-id="a7c53-371">***Decyzja***: tak.</span><span class="sxs-lookup"><span data-stu-id="a7c53-371">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a><span data-ttu-id="a7c53-372">Zastępowanie niepublicznych członków interfejsu (zamknięty)</span><span class="sxs-lookup"><span data-stu-id="a7c53-372">Overriding non-public interface members (closed)</span></span>

<span data-ttu-id="a7c53-373">W interfejsie niepubliczne składowe z interfejsów podstawowych są zastępowane przy użyciu modyfikatora `override`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-373">In an interface, non-public members from base interfaces are overridden using the `override` modifier.</span></span> <span data-ttu-id="a7c53-374">Jeśli jest to przesłonięcie jawne, które nazywa interfejs zawierający element członkowski, modyfikator dostępu zostanie pominięty.</span><span class="sxs-lookup"><span data-stu-id="a7c53-374">If it is an "explicit" override that names the interface containing the member, the access modifier is omitted.</span></span>

> <span data-ttu-id="a7c53-375">***Zamknięty problem:*** Jeśli jest to przesłonięcie "niejawne", które nie ma nazwy interfejsu, czy modyfikator dostępu musi być zgodny?</span><span class="sxs-lookup"><span data-stu-id="a7c53-375">***Closed Issue:*** If it is an "implicit" override that does not name the interface, does the access modifier have to match?</span></span>

<span data-ttu-id="a7c53-376">***Decyzja:*** Tylko publiczne elementy członkowskie mogą zostać przesłonięte niejawnie, a dostęp musi być zgodny.</span><span class="sxs-lookup"><span data-stu-id="a7c53-376">***Decision:*** Only public members may be implicitly overridden, and the access must match.</span></span> <span data-ttu-id="a7c53-377">Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span><span class="sxs-lookup"><span data-stu-id="a7c53-377">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

> <span data-ttu-id="a7c53-378">***Problem otwarty:*** Czy modyfikator dostępu jest wymagany, opcjonalny lub pomijany w jawnym przesłonięciu, takim jak `override void IB.M() {}`?</span><span class="sxs-lookup"><span data-stu-id="a7c53-378">***Open Issue:*** Is the access modifier required, optional, or omitted on an explicit override such as `override void IB.M() {}`?</span></span>

> <span data-ttu-id="a7c53-379">***Problem otwarty:*** Czy `override` jest wymagane, opcjonalne lub pomijane w jawnym przesłonięciu, takim jak `void IB.M() {}`?</span><span class="sxs-lookup"><span data-stu-id="a7c53-379">***Open Issue:*** Is `override` required, optional, or omitted on an explicit override such as `void IB.M() {}`?</span></span>

<span data-ttu-id="a7c53-380">Jak jeden implementuje niepubliczny element członkowski interfejsu w klasie?</span><span class="sxs-lookup"><span data-stu-id="a7c53-380">How does one implement a non-public interface member in a class?</span></span> <span data-ttu-id="a7c53-381">Być może należy ją wykonać jawnie?</span><span class="sxs-lookup"><span data-stu-id="a7c53-381">Perhaps it must be done explicitly?</span></span>

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> <span data-ttu-id="a7c53-382">***Zamknięty problem:*** Jak jeden implementuje niepubliczny element członkowski interfejsu w klasie?</span><span class="sxs-lookup"><span data-stu-id="a7c53-382">***Closed Issue:*** How does one implement a non-public interface member in a class?</span></span>

<span data-ttu-id="a7c53-383">***Decyzja:*** Niepubliczne elementy członkowskie interfejsu można zaimplementować jawnie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-383">***Decision:*** You can only implement non-public interface members explicitly.</span></span> <span data-ttu-id="a7c53-384">Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span><span class="sxs-lookup"><span data-stu-id="a7c53-384">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

<span data-ttu-id="a7c53-385">***Decyzja***: w składowych interfejsu nie jest dozwolone słowo kluczowe `override`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-385">***Decision***: No `override` keyword permitted on interface members.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a><span data-ttu-id="a7c53-386">Zgodność binarna 2 (zamknięty)</span><span class="sxs-lookup"><span data-stu-id="a7c53-386">Binary Compatibility 2 (closed)</span></span>

<span data-ttu-id="a7c53-387">Rozważmy następujący kod, w którym każdy typ znajduje się w osobnym zestawie</span><span class="sxs-lookup"><span data-stu-id="a7c53-387">Consider the following code in which each type is in a separate assembly</span></span>

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

<span data-ttu-id="a7c53-388">Rozumiemy, że implementacja `I1.M` w `C` jest `I2.M`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-388">We understand that the implementation of `I1.M` in `C` is `I2.M`.</span></span> <span data-ttu-id="a7c53-389">Co zrobić, jeśli zestaw zawierający `I3` został zmieniony w następujący sposób i ponownie skompilowany</span><span class="sxs-lookup"><span data-stu-id="a7c53-389">What if the assembly containing `I3` is changed as follows and recompiled</span></span>

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

<span data-ttu-id="a7c53-390">ale `C` nie jest ponownie kompilowana.</span><span class="sxs-lookup"><span data-stu-id="a7c53-390">but `C` is not recompiled.</span></span> <span data-ttu-id="a7c53-391">Co się stanie po uruchomieniu programu?</span><span class="sxs-lookup"><span data-stu-id="a7c53-391">What happens when the program is run?</span></span> <span data-ttu-id="a7c53-392">Wywołanie `(C as I1).M()`</span><span class="sxs-lookup"><span data-stu-id="a7c53-392">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="a7c53-393">Działa `I1.M`</span><span class="sxs-lookup"><span data-stu-id="a7c53-393">Runs `I1.M`</span></span>
2. <span data-ttu-id="a7c53-394">Działa `I2.M`</span><span class="sxs-lookup"><span data-stu-id="a7c53-394">Runs `I2.M`</span></span>
3. <span data-ttu-id="a7c53-395">Działa `I3.M`</span><span class="sxs-lookup"><span data-stu-id="a7c53-395">Runs `I3.M`</span></span>
4. <span data-ttu-id="a7c53-396">Wartość 2 lub 3, deterministycznie</span><span class="sxs-lookup"><span data-stu-id="a7c53-396">Either 2 or 3, deterministically</span></span>
5. <span data-ttu-id="a7c53-397">Zgłasza jakiś rodzaj wyjątku czasu wykonywania</span><span class="sxs-lookup"><span data-stu-id="a7c53-397">Throws some kind of runtime exception</span></span>

<span data-ttu-id="a7c53-398">***Decyzja***: Zgłoś wyjątek (5).</span><span class="sxs-lookup"><span data-stu-id="a7c53-398">***Decision***: Throw an exception (5).</span></span> <span data-ttu-id="a7c53-399">Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span><span class="sxs-lookup"><span data-stu-id="a7c53-399">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span></span>

### <a name="permit-partial-in-interface-closed"></a><span data-ttu-id="a7c53-400">Zezwolić na `partial` w interfejsie?</span><span class="sxs-lookup"><span data-stu-id="a7c53-400">Permit `partial` in interface?</span></span> <span data-ttu-id="a7c53-401">napis</span><span class="sxs-lookup"><span data-stu-id="a7c53-401">(closed)</span></span>

<span data-ttu-id="a7c53-402">Mając na względzie, że interfejsy mogą być używane w sposób analogiczny do sposobu używania klas abstrakcyjnych, warto zadeklarować je `partial`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-402">Given that interfaces may be used in ways analogous to the way abstract classes are used, it may be useful to declare them `partial`.</span></span> <span data-ttu-id="a7c53-403">Jest to szczególnie przydatne w przypadku generatorów.</span><span class="sxs-lookup"><span data-stu-id="a7c53-403">This would be particularly useful in the face of generators.</span></span>

> <span data-ttu-id="a7c53-404">***Propozycja:*** Usuń ograniczenie języka, które interfejsy i elementy członkowskie interfejsów nie mogą być deklarowane `partial`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-404">***Proposal:*** Remove the language restriction that interfaces and members of interfaces may not be declared `partial`.</span></span>

<span data-ttu-id="a7c53-405">***Decyzja***: tak.</span><span class="sxs-lookup"><span data-stu-id="a7c53-405">***Decision***: Yes.</span></span> <span data-ttu-id="a7c53-406">Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span><span class="sxs-lookup"><span data-stu-id="a7c53-406">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span></span>

### <a name="main-in-an-interface-closed"></a><span data-ttu-id="a7c53-407">`Main` w interfejsie?</span><span class="sxs-lookup"><span data-stu-id="a7c53-407">`Main` in an interface?</span></span> <span data-ttu-id="a7c53-408">napis</span><span class="sxs-lookup"><span data-stu-id="a7c53-408">(closed)</span></span>

> <span data-ttu-id="a7c53-409">***Problem otwarty:*** Czy `static Main` w interfejsie kandydatem może być punkt wejścia programu?</span><span class="sxs-lookup"><span data-stu-id="a7c53-409">***Open Issue:*** Is a `static Main` method in an interface a candidate to be the program's entry point?</span></span>

<span data-ttu-id="a7c53-410">***Decyzja***: tak.</span><span class="sxs-lookup"><span data-stu-id="a7c53-410">***Decision***: Yes.</span></span> <span data-ttu-id="a7c53-411">Zobacz: <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span><span class="sxs-lookup"><span data-stu-id="a7c53-411">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span></span>

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a><span data-ttu-id="a7c53-412">Potwierdź zamiar obsługi publicznych metod niewirtualnych (zamknięty)</span><span class="sxs-lookup"><span data-stu-id="a7c53-412">Confirm intent to support public non-virtual methods (closed)</span></span>

<span data-ttu-id="a7c53-413">Czy możemy potwierdzić (lub odwrócić) decyzję, aby zezwolić na niewirtualne metody publiczne w interfejsie?</span><span class="sxs-lookup"><span data-stu-id="a7c53-413">Can we please confirm (or reverse) our decision to permit non-virtual public methods in an interface?</span></span>

```csharp
interface IA
{
    public sealed void M() { }
}
```

> <span data-ttu-id="a7c53-414">***Problem częściowo zamknięty:*** (2017-04-18) uważamy, że będzie on przydatny, ale powróci do niego.</span><span class="sxs-lookup"><span data-stu-id="a7c53-414">***Semi-Closed Issue:*** (2017-04-18) We think it is going to be useful, but will come back to it.</span></span> <span data-ttu-id="a7c53-415">Jest to blok do wyzwolenia modelu psychicznego.</span><span class="sxs-lookup"><span data-stu-id="a7c53-415">This is a mental model tripping block.</span></span>

<span data-ttu-id="a7c53-416">***Decyzja***: tak.</span><span class="sxs-lookup"><span data-stu-id="a7c53-416">***Decision***: Yes.</span></span> <span data-ttu-id="a7c53-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span><span class="sxs-lookup"><span data-stu-id="a7c53-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span></span>

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a><span data-ttu-id="a7c53-418">Czy `override` w interfejsie wprowadza nowy element członkowski?</span><span class="sxs-lookup"><span data-stu-id="a7c53-418">Does an `override` in an interface introduce a new member?</span></span> <span data-ttu-id="a7c53-419">napis</span><span class="sxs-lookup"><span data-stu-id="a7c53-419">(closed)</span></span>

<span data-ttu-id="a7c53-420">Istnieje kilka sposobów, aby sprawdzić, czy deklaracja przesłonięcia wprowadza nowy element członkowski, czy nie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-420">There are a few ways to observe whether an override declaration introduces a new member or not.</span></span>

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> <span data-ttu-id="a7c53-421">***Problem otwarty:*** Czy deklaracja przesłonięcia w interfejsie wprowadza nowy element członkowski?</span><span class="sxs-lookup"><span data-stu-id="a7c53-421">***Open Issue:*** Does an override declaration in an interface introduce a new member?</span></span> <span data-ttu-id="a7c53-422">napis</span><span class="sxs-lookup"><span data-stu-id="a7c53-422">(closed)</span></span>

<span data-ttu-id="a7c53-423">W klasie Metoda zastępująca ma wartość "Visible" w niektórych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="a7c53-423">In a class, an overriding method is "visible" in some senses.</span></span> <span data-ttu-id="a7c53-424">Na przykład nazwy jego parametrów mają pierwszeństwo przed nazwami parametrów w zastąpionej metodzie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-424">For example, the names of its parameters take precedence over the names of parameters in the overridden method.</span></span> <span data-ttu-id="a7c53-425">Może istnieć możliwość duplikowania tego zachowania w interfejsach, ponieważ zawsze jest określone przesłonięcie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-425">It may be possible to duplicate that behavior in interfaces, as there is always a most specific override.</span></span> <span data-ttu-id="a7c53-426">Ale chcesz zduplikować to zachowanie?</span><span class="sxs-lookup"><span data-stu-id="a7c53-426">But do we want to duplicate that behavior?</span></span>

<span data-ttu-id="a7c53-427">Ponadto możliwe jest "przesłonięcie" metody przesłaniania?</span><span class="sxs-lookup"><span data-stu-id="a7c53-427">Also, it is possible to "override" an override method?</span></span> <span data-ttu-id="a7c53-428">[Moot]</span><span class="sxs-lookup"><span data-stu-id="a7c53-428">[Moot]</span></span>

<span data-ttu-id="a7c53-429">***Decyzja***: w składowych interfejsu nie jest dozwolone słowo kluczowe `override`.</span><span class="sxs-lookup"><span data-stu-id="a7c53-429">***Decision***: No `override` keyword permitted on interface members.</span></span> <span data-ttu-id="a7c53-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span><span class="sxs-lookup"><span data-stu-id="a7c53-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span></span>

### <a name="properties-with-a-private-accessor-closed"></a><span data-ttu-id="a7c53-431">Właściwości z prywatnym akcesorem (zamknięty)</span><span class="sxs-lookup"><span data-stu-id="a7c53-431">Properties with a private accessor (closed)</span></span>

<span data-ttu-id="a7c53-432">Załóżmy, że prywatne elementy członkowskie nie są wirtualne, a kombinacja wirtualnych i prywatnych jest niedozwolona.</span><span class="sxs-lookup"><span data-stu-id="a7c53-432">We say that private members are not virtual, and the combination of virtual and private is disallowed.</span></span> <span data-ttu-id="a7c53-433">Ale na czym polega właściwość z prywatnym akcesorem?</span><span class="sxs-lookup"><span data-stu-id="a7c53-433">But what about a property with a private accessor?</span></span>

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

<span data-ttu-id="a7c53-434">Czy jest to dozwolone?</span><span class="sxs-lookup"><span data-stu-id="a7c53-434">Is this allowed?</span></span> <span data-ttu-id="a7c53-435">Czy metoda dostępu do `set` jest tutaj `virtual` lub nie?</span><span class="sxs-lookup"><span data-stu-id="a7c53-435">Is the `set` accessor here `virtual` or not?</span></span> <span data-ttu-id="a7c53-436">Czy można go zastąpić, gdzie jest dostępny?</span><span class="sxs-lookup"><span data-stu-id="a7c53-436">Can it be overridden where it is accessible?</span></span> <span data-ttu-id="a7c53-437">Czy następujące niejawnie implementuje tylko metodę dostępu `get`?</span><span class="sxs-lookup"><span data-stu-id="a7c53-437">Does the following implicitly implement only the `get` accessor?</span></span>

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

<span data-ttu-id="a7c53-438">Jest najprawdopodobniej przyczyną błędu, ponieważ IA. Zestaw P. set nie jest wirtualny, a także ponieważ nie jest dostępny?</span><span class="sxs-lookup"><span data-stu-id="a7c53-438">Is the following presumably an error because IA.P.set isn't virtual and also because it isn't accessible?</span></span>

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

<span data-ttu-id="a7c53-439">***Decyzja***: pierwszy przykład wygląda prawidłowo, a ostatni nie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-439">***Decision***: The first example looks valid, while the last does not.</span></span> <span data-ttu-id="a7c53-440">Jest to rozwiązanie podobne do tego, jak już działa C#.</span><span class="sxs-lookup"><span data-stu-id="a7c53-440">This is resolved analogously to how it already works in C#.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a><span data-ttu-id="a7c53-441">Wywołania interfejsu podstawowego, zaokrąglone 2 (zamknięte)</span><span class="sxs-lookup"><span data-stu-id="a7c53-441">Base Interface Invocations, round 2 (closed)</span></span>

<span data-ttu-id="a7c53-442">Nasze poprzednie "rozwiązanie" do obsługi wywołań podstawowych nie zapewnia wystarczającej wyrazistości.</span><span class="sxs-lookup"><span data-stu-id="a7c53-442">Our previous "resolution" to how to handle base invocations doesn't actually provide sufficient expressiveness.</span></span> <span data-ttu-id="a7c53-443">Powoduje to, że w C# programie i CLR, w przeciwieństwie do języka Java, należy określić interfejs zawierający deklarację metody i lokalizację implementacji, która ma zostać wywołana.</span><span class="sxs-lookup"><span data-stu-id="a7c53-443">It turns out that in C# and the CLR, unlike Java, you need to specify both the interface containing the method declaration and the location of the implementation you want to invoke.</span></span>

<span data-ttu-id="a7c53-444">Proponuję następującą składnię dla wywołań podstawowych w interfejsach.</span><span class="sxs-lookup"><span data-stu-id="a7c53-444">I propose the following syntax for base calls in interfaces.</span></span> <span data-ttu-id="a7c53-445">Nie podoba mi się, ale ilustruje to, co każda składnia musi być w stanie wyrazić:</span><span class="sxs-lookup"><span data-stu-id="a7c53-445">I’m not in love with it, but it illustrates what any syntax must be able to express:</span></span>

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

<span data-ttu-id="a7c53-446">Jeśli nie ma niejednoznaczności, możesz napisać ją dokładniej</span><span class="sxs-lookup"><span data-stu-id="a7c53-446">If there is no ambiguity, you can write it more simply</span></span>

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

<span data-ttu-id="a7c53-447">Lub</span><span class="sxs-lookup"><span data-stu-id="a7c53-447">Or</span></span>

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

<span data-ttu-id="a7c53-448">Lub</span><span class="sxs-lookup"><span data-stu-id="a7c53-448">Or</span></span>

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

<span data-ttu-id="a7c53-449">***Decyzja***: podjęto decyzję o `base(N.I1<T>).M(s)`, co oznacza, że jeśli mamy powiązanie z wywołaniem, w dalszej części tego problemu może wystąpić problem.</span><span class="sxs-lookup"><span data-stu-id="a7c53-449">***Decision***: Decided on `base(N.I1<T>).M(s)`, conceding that if we have an invocation binding there may be problem here later on.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a><span data-ttu-id="a7c53-450">Ostrzeżenie dla struktury nie implementującej metody domyślnej?</span><span class="sxs-lookup"><span data-stu-id="a7c53-450">Warning for struct not implementing default method?</span></span> <span data-ttu-id="a7c53-451">napis</span><span class="sxs-lookup"><span data-stu-id="a7c53-451">(closed)</span></span>

<span data-ttu-id="a7c53-452">@vancem potwierdzenia, że firma Microsoft powinna poważnie rozważyć wygenerowanie ostrzeżenia, jeśli deklaracja typu wartości nie przesłania niektórych metod interfejsu, nawet jeśli dziedziczy implementację tej metody z interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a7c53-452">@vancem asserts that we should seriously consider producing a warning if a value type declaration fails to override some interface method, even if it would inherit an implementation of that method from an interface.</span></span> <span data-ttu-id="a7c53-453">Ponieważ powoduje to zapakowanie i podłączenie ograniczonych wywołań.</span><span class="sxs-lookup"><span data-stu-id="a7c53-453">Because it causes boxing and undermines constrained calls.</span></span>

<span data-ttu-id="a7c53-454">***Decyzja***: Wygląda na to coś bardziej dopasowanego do analizatora.</span><span class="sxs-lookup"><span data-stu-id="a7c53-454">***Decision***: This seems like something more suited for an analyzer.</span></span> <span data-ttu-id="a7c53-455">Wydaje się również, że to ostrzeżenie może być spowodowane zakłóceniami, ponieważ mogłoby być uruchamiane nawet wtedy, gdy domyślna metoda interfejsu nigdy nie zostanie wywołana i nie nastąpi żadne opakowanie.</span><span class="sxs-lookup"><span data-stu-id="a7c53-455">It also seems like this warning could be noisy, since it would fire even if the default interface method is never called and no boxing will ever occur.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a><span data-ttu-id="a7c53-456">Konstruktory statyczne interfejsu (zamknięte)</span><span class="sxs-lookup"><span data-stu-id="a7c53-456">Interface static constructors (closed)</span></span>

<span data-ttu-id="a7c53-457">Kiedy są uruchamiane konstruktory statyczne interfejsów?</span><span class="sxs-lookup"><span data-stu-id="a7c53-457">When are interface static constructors run?</span></span>  <span data-ttu-id="a7c53-458">Bieżąca wersja robocza interfejsu wiersza polecenia jest proponowana podczas uzyskiwania dostępu do pierwszej statycznej metody lub pola.</span><span class="sxs-lookup"><span data-stu-id="a7c53-458">The current CLI draft proposes that it occurs when the first static method or field is accessed.</span></span> <span data-ttu-id="a7c53-459">Jeśli nie ma żadnego z tych elementów, może to oznaczać, że nigdy nie będzie można go uruchomić?</span><span class="sxs-lookup"><span data-stu-id="a7c53-459">If there are neither of those then it might never be run??</span></span>

<span data-ttu-id="a7c53-460">[2018-10-09 zespół CLR proponuje "przechodzenie do dublowanych elementów ValueType (Sprawdź dostęp do każdej metody wystąpienia)"]</span><span class="sxs-lookup"><span data-stu-id="a7c53-460">[2018-10-09 The CLR team proposes "Going to mirror what we do for valuetypes (cctor check on access to each instance method)"]</span></span>

<span data-ttu-id="a7c53-461">***Decyzja***: konstruktory statyczne są również uruchamiane na wpisach w metodach wystąpień, jeśli Konstruktor statyczny nie został `beforefieldinit`, w którym przypadki konstruktory statyczne są uruchamiane przed dostępem do pierwszego pola statycznego.</span><span class="sxs-lookup"><span data-stu-id="a7c53-461">***Decision***: Static constructors are also run on entry to instance methods, if the static constructor was not `beforefieldinit`, in which case static constructors are run before access to the first static field.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a><span data-ttu-id="a7c53-462">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="a7c53-462">Design meetings</span></span>

<span data-ttu-id="a7c53-463">[2017-03-08. uwagi dotyczące spotkań z poprawkami programu ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
2017-03-21. [uwagi dotyczące spotkań](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) programu LDM
[2017-03-23 spotkanie "środowisko CLR dla domyślnych metod interfejsu"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md) , [2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
2017-04-05 rozwiązania do [spotkań](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md) z [poprawkami LDM
](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md) [2017-04-11](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md) } [uwagi
2017-04-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) [2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md) [ Notatki dotyczące spotkania](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md) [2018-11-14](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)
Meeting



</span><span class="sxs-lookup"><span data-stu-id="a7c53-463">[2017-03-08 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 meeting "CLR Behavior for Default Interface Methods"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)
[2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)
[2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
[2017-06-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)</span></span>
