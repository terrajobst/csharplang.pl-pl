---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/23/2019
ms.locfileid: "79484945"
---
# <a name="records"></a><span data-ttu-id="abea1-101">rekordy</span><span class="sxs-lookup"><span data-stu-id="abea1-101">records</span></span>

* <span data-ttu-id="abea1-102">[x] proponowane</span><span class="sxs-lookup"><span data-stu-id="abea1-102">[x] Proposed</span></span>
* <span data-ttu-id="abea1-103">[] Prototyp: [ukończono](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="abea1-103">[ ] Prototype: [Complete](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="abea1-104">[] Implementacja: [w toku](https://github.com/dotnet/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="abea1-104">[ ] Implementation: [In Progress](https://github.com/dotnet/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="abea1-105">[] Specyfikacja: zawarta Specyfikacja robocza</span><span class="sxs-lookup"><span data-stu-id="abea1-105">[ ] Specification: Draft specification enclosed</span></span>

## <a name="summary"></a><span data-ttu-id="abea1-106">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="abea1-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="abea1-107">Rekordy są nową, uproszczoną formą deklaracji C# dla typów klas i struktur, które łączą zalety wielu prostszych funkcji.</span><span class="sxs-lookup"><span data-stu-id="abea1-107">Records are a new, simplified declaration form for C# class and struct types that combine the benefits of a number of simpler features.</span></span> <span data-ttu-id="abea1-108">Opisujemy nowe funkcje (parametry obiektu wywołującego i *z*parametrami s), nadaj składnię i semantykę dla deklaracji rekordu, a następnie podaj kilka przykładów.</span><span class="sxs-lookup"><span data-stu-id="abea1-108">We describe the new features (caller-receiver parameters and *with-expression*s), give the syntax and semantics for record declarations, and then provide some examples.</span></span>


## <a name="motivation"></a><span data-ttu-id="abea1-109">Motywacją</span><span class="sxs-lookup"><span data-stu-id="abea1-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="abea1-110">Znacząca liczba deklaracji typu w programie C# jest nieco większa niż zagregowane kolekcje danych o określonym typie.</span><span class="sxs-lookup"><span data-stu-id="abea1-110">A significant number of type declarations in C# are little more than aggregate collections of typed data.</span></span> <span data-ttu-id="abea1-111">Niestety, zadeklarowanie takich typów wymaga dużej ilości kodu standardowego.</span><span class="sxs-lookup"><span data-stu-id="abea1-111">Unfortunately, declaring such types requires a great deal of boilerplate code.</span></span> <span data-ttu-id="abea1-112">*Rekordy* zapewniają mechanizm deklarujący typ danych przez Opisywanie elementów członkowskich agregacji wraz z dodatkowym kodem lub odchyleniami od zwykłych standardowych (jeśli istnieją).</span><span class="sxs-lookup"><span data-stu-id="abea1-112">*Records* provide a mechanism for declaring a datatype by describing the members of the aggregate along with additional code or deviations from the usual boilerplate, if any.</span></span>

<span data-ttu-id="abea1-113">Zobacz [przykłady](#examples)poniżej.</span><span class="sxs-lookup"><span data-stu-id="abea1-113">See [Examples](#examples), below.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="abea1-114">Szczegółowy projekt</span><span class="sxs-lookup"><span data-stu-id="abea1-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a><span data-ttu-id="abea1-115">Caller — parametry odbiornika</span><span class="sxs-lookup"><span data-stu-id="abea1-115">caller-receiver parameters</span></span>

<span data-ttu-id="abea1-116">Obecnie *argumentem domyślnym* parametru metody musi być</span><span class="sxs-lookup"><span data-stu-id="abea1-116">Currently a method parameter's *default-argument* must be</span></span> 
- <span data-ttu-id="abea1-117">*wyrażenie stałe*; oraz</span><span class="sxs-lookup"><span data-stu-id="abea1-117">a *constant-expression*; or</span></span>
- <span data-ttu-id="abea1-118">wyrażenie `new S()`, gdzie `S` jest typem wartości; oraz</span><span class="sxs-lookup"><span data-stu-id="abea1-118">an expression of the form `new S()` where `S` is a value type; or</span></span>
- <span data-ttu-id="abea1-119">wyrażenie `default(S)`, gdzie `S` jest typem wartości</span><span class="sxs-lookup"><span data-stu-id="abea1-119">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="abea1-120">Rozszerzającmy ten sposób, aby dodać następujące elementy</span><span class="sxs-lookup"><span data-stu-id="abea1-120">We extend this to add the following</span></span>
- <span data-ttu-id="abea1-121">wyrażenie `this.Identifier`</span><span class="sxs-lookup"><span data-stu-id="abea1-121">an expression of the form `this.Identifier`</span></span>

<span data-ttu-id="abea1-122">Ten nowy formularz nosi nazwę *domyślnego argumentu Caller-Receiver*i jest dozwolony tylko wtedy, gdy spełnione są wszystkie poniższe warunki</span><span class="sxs-lookup"><span data-stu-id="abea1-122">This new form is called a *caller-receiver default-argument*, and is allowed only if all of the following are satisfied</span></span>
- <span data-ttu-id="abea1-123">Metoda, w której występuje, jest metodą wystąpienia; lub</span><span class="sxs-lookup"><span data-stu-id="abea1-123">The method in which it appears is an instance method; and</span></span>
- <span data-ttu-id="abea1-124">Wyrażenie `this.Identifier` jest powiązane z elementem członkowskim wystąpienia typu otaczającego, który musi być polem lub właściwością. lub</span><span class="sxs-lookup"><span data-stu-id="abea1-124">The expression `this.Identifier` binds to an instance member of the enclosing type, which must be either a field or a property; and</span></span>
- <span data-ttu-id="abea1-125">Element członkowski, do którego jest powiązany (i metoda dostępu `get`, jeśli jest to właściwość), jest co najmniej tak samo jak metoda; lub</span><span class="sxs-lookup"><span data-stu-id="abea1-125">The member to which it binds (and the `get` accessor, if it is a property) is at least as accessible as the method; and</span></span>
- <span data-ttu-id="abea1-126">Typ `this.Identifier` jest niejawnie konwertowany przez tożsamość lub konwersję dopuszczającą wartości null na typ parametru (jest to istniejące ograniczenie dla *argumentu default*).</span><span class="sxs-lookup"><span data-stu-id="abea1-126">The type of `this.Identifier` is implicitly convertible by an identity or nullable conversion to the type of the parameter (this is an existing constraint on *default-argument*).</span></span>

<span data-ttu-id="abea1-127">Gdy argument zostanie pominięty z wywołania elementu członkowskiego funkcji dla odpowiadającego opcjonalnego parametru z *argumentem default-Receiver Caller*, wartość elementu członkowskiego odbiorcy zostanie niejawnie przeniesiona.</span><span class="sxs-lookup"><span data-stu-id="abea1-127">When an argument is omitted from an invocation of a function member for a corresponding optional parameter with a *caller-receiver default-argument*, the value of the receiver's member is implicitly passed.</span></span> 

> <span data-ttu-id="abea1-128">**Uwagi dotyczące projektowania**: główna przyczyna parametru Caller-Receiver ma obsługiwać *wyrażenie with*.</span><span class="sxs-lookup"><span data-stu-id="abea1-128">**Design Notes**: the main reason for the caller-receiver parameter is to support the *with-expression*.</span></span> <span data-ttu-id="abea1-129">Pomysłem jest to, że można zadeklarować metodę podobną do tej</span><span class="sxs-lookup"><span data-stu-id="abea1-129">The idea is that you can declare a method like this</span></span>
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> <span data-ttu-id="abea1-130">a następnie użyj go jak tego</span><span class="sxs-lookup"><span data-stu-id="abea1-130">and then use it like this</span></span>
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> <span data-ttu-id="abea1-131">Aby utworzyć nowy `Point` tak jak istniejący `Point`, ale z wartością `X` zmieniony.</span><span class="sxs-lookup"><span data-stu-id="abea1-131">To create a new `Point` just like an existing `Point` but with the value of `X` changed.</span></span>
> 
> <span data-ttu-id="abea1-132">Jest to pytanie otwarte, niezależnie od tego, czy forma składni *wyrażenia with* jest dodawana, gdy mamy obsługę parametrów odbiornika wywołującego, więc jest to możliwe, *zamiast* tego jako *uzupełnienie* *wyrażenia with*.</span><span class="sxs-lookup"><span data-stu-id="abea1-132">It is an open question whether or not the syntactic form of the *with-expression* is worth adding once we have support for caller-receiver parameters, so it is possible we would do this *instead of* rather than *in addition to* the *with-expression*.</span></span>

- <span data-ttu-id="abea1-133">[] **Problem otwarty**: co to jest kolejność, w której jest oceniany *argument "odbiornik wywołujący"* , w odniesieniu do innych argumentów?</span><span class="sxs-lookup"><span data-stu-id="abea1-133">[ ] **Open issue**: What is the order in which a *caller-receiver default-argument* is evaluated with respect to other arguments?</span></span> <span data-ttu-id="abea1-134">Czy na pewno nie określono go?</span><span class="sxs-lookup"><span data-stu-id="abea1-134">Should we say that it is unspecified?</span></span>

### <a name="with-expressions"></a><span data-ttu-id="abea1-135">wyrażenia with</span><span class="sxs-lookup"><span data-stu-id="abea1-135">with-expressions</span></span>

<span data-ttu-id="abea1-136">Proponowany jest nowy formularz wyrażenia:</span><span class="sxs-lookup"><span data-stu-id="abea1-136">A new expression form is proposed:</span></span>

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

<span data-ttu-id="abea1-137">Token `with` jest nowym słowem kluczowym zależnym od kontekstu.</span><span class="sxs-lookup"><span data-stu-id="abea1-137">The token `with` is a new context-sensitive keyword.</span></span>

<span data-ttu-id="abea1-138">Każdy *Identyfikator* po lewej stronie *with_initializer* musi być powiązany z dostępnym polem wystąpienia lub właściwością typu *primary_expression* *with_expression*.</span><span class="sxs-lookup"><span data-stu-id="abea1-138">Each *identifier* on the left of a *with_initializer* must bind to an accessible instance field or property of the type of the *primary_expression* of the *with_expression*.</span></span> <span data-ttu-id="abea1-139">Nie mogą istnieć zduplikowane nazwy między tymi identyfikatorami danego *with_expression*.</span><span class="sxs-lookup"><span data-stu-id="abea1-139">There may be no duplicated name among these identifiers of a given *with_expression*.</span></span>

<span data-ttu-id="abea1-140">*With_expression* formularza</span><span class="sxs-lookup"><span data-stu-id="abea1-140">A *with_expression* of the form</span></span>

> <span data-ttu-id="abea1-141">`with` *e1* `{` *Identyfikator* = *e2*,... `}`</span><span class="sxs-lookup"><span data-stu-id="abea1-141">*e1* `with` `{` *identifier* = *e2*, ... `}`</span></span>

<span data-ttu-id="abea1-142">jest traktowany jako wywołanie formularza</span><span class="sxs-lookup"><span data-stu-id="abea1-142">is treated as an invocation of the form</span></span>

> <span data-ttu-id="abea1-143">*e1*`.With(`*identifier2*`:` *e2*,...`)`</span><span class="sxs-lookup"><span data-stu-id="abea1-143">*e1*`.With(`*identifier2*`:` *e2*, ...`)`</span></span>

<span data-ttu-id="abea1-144">Gdzie, dla każdej metody o nazwie `With`, która jest członkiem dostępnego wystąpienia *E1*, wybieramy *identifier2* jako nazwę pierwszego parametru w tej metodzie, która ma parametr Caller-Receiver, który jest tym samym członkiem, co pole wystąpienia lub właściwość powiązane z *identyfikatorem*.</span><span class="sxs-lookup"><span data-stu-id="abea1-144">Where, for each method named `With` that is an accessible instance member of *e1*, we select *identifier2* as the name of the first parameter in that method that has a caller-receiver parameter that is the same member as the instance field or property bound to *identifier*.</span></span> <span data-ttu-id="abea1-145">Jeśli ten parametr nie może być zidentyfikowany, ta metoda jest eliminowana z uwagi.</span><span class="sxs-lookup"><span data-stu-id="abea1-145">If no such parameter can be identified that method is eliminated from consideration.</span></span> <span data-ttu-id="abea1-146">Metoda, która ma zostać wywołana, została wybrana spośród pozostałych kandydatów przez rozpoznanie przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="abea1-146">The method to be invoked is selected from among the remaining candidates by overload resolution.</span></span>

> <span data-ttu-id="abea1-147">**Uwagi dotyczące projektowania**: określone parametry obiektu wywołującego-Receiver, wiele korzyści z *wyrażenia with —* są dostępne bez tego formularza specjalnej składni.</span><span class="sxs-lookup"><span data-stu-id="abea1-147">**Design Notes**: Given caller-receiver parameters, many of the benefits of the *with-expression* are available without this special syntax form.</span></span> <span data-ttu-id="abea1-148">W związku z tym rozważamy, czy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="abea1-148">We are therefore considering whether or not it is needed.</span></span> <span data-ttu-id="abea1-149">Jej główną korzyścią jest umożliwienie jednemu programowi w odniesieniu do nazw pól i właściwości, a nie pod względem nazw parametrów.</span><span class="sxs-lookup"><span data-stu-id="abea1-149">Its main benefit is allowing one to program in terms of the names of fields and properties, rather than in terms of the names of parameters.</span></span> <span data-ttu-id="abea1-150">W ten sposób poprawimy zarówno czytelność, jak i jakość narzędzi (np. przejście do definicji w identyfikatorze *with_expression* przejdzie do właściwości, a nie do parametru metody).</span><span class="sxs-lookup"><span data-stu-id="abea1-150">In this way we improve both readability and the quality of tooling (e.g. go-to-definition on the identifier of a *with_expression* would navigate to the property rather than to a method parameter).</span></span>

- <span data-ttu-id="abea1-151">[] **Otwórz problem**: ten opis należy zmodyfikować, aby obsługiwać metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="abea1-151">[ ] **Open issue**: This description should be modified to support extension methods.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="abea1-152">dopasowanie wzorca</span><span class="sxs-lookup"><span data-stu-id="abea1-152">pattern-matching</span></span>

<span data-ttu-id="abea1-153">Patrz [Specyfikacja dopasowania wzorca](csharp-8.0/patterns.md#positional-pattern) dla specyfikacji `Deconstruct` i jej relacji do dopasowania do wzorca.</span><span class="sxs-lookup"><span data-stu-id="abea1-153">See the [Pattern Matching Specification](csharp-8.0/patterns.md#positional-pattern) for a specification of `Deconstruct` and its relationship to pattern-matching.</span></span>

> <span data-ttu-id="abea1-154">**Uwagi dotyczące projektowania**: na podstawie `Deconstruct` wygenerowanego przez kompilator, zgodnie z opisem w niniejszej umowie, oraz specyfikacji dla deklaracji typu wzorzec-rekord</span><span class="sxs-lookup"><span data-stu-id="abea1-154">**Design Notes**: By virtue of the compiler-generated `Deconstruct` as specified herein, and the specification for pattern-matching, a record declaration</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="abea1-155">Program obsługuje dopasowanie do wzorca pozycyjnego w następujący sposób</span><span class="sxs-lookup"><span data-stu-id="abea1-155">will support positional pattern-matching as follows</span></span>
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a><span data-ttu-id="abea1-156">deklaracje typu rekordu</span><span class="sxs-lookup"><span data-stu-id="abea1-156">record type declarations</span></span>

<span data-ttu-id="abea1-157">Składnia `class` lub `struct` deklaracji jest rozszerzona o obsługę parametrów wartości; parametry są właściwościami typu:</span><span class="sxs-lookup"><span data-stu-id="abea1-157">The syntax for a `class` or `struct` declaration is extended to support value parameters; the parameters become properties of the type:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> <span data-ttu-id="abea1-158">**Uwagi dotyczące projektowania**: ponieważ typy rekordów są często przydatne bez potrzeby dla elementów członkowskich zadeklarowanych jawnie w treści klasy, firma Microsoft modyfikuje składnię deklaracji, aby umożliwić jej po prostu średnik.</span><span class="sxs-lookup"><span data-stu-id="abea1-158">**Design Notes**: Because record types are often useful without the need for any members explicitly declared in a class-body, we modify the syntax of the declaration to allow a body to be simply a semicolon.</span></span>

<span data-ttu-id="abea1-159">Klasa (struktura) zadeklarowana z *parametrami Record* jest nazywana *klasą rekordów* (*strukturą rekordów*), z której jest *typem rekordu*.</span><span class="sxs-lookup"><span data-stu-id="abea1-159">A class (struct) declared with the *record-parameters* is called a *record class* (*record struct*), either of which is a *record type*.</span></span>

- <span data-ttu-id="abea1-160">[] **Otwórz problem**: musimy uwzględnić *primary_constructor_body* w gramatyce, aby mogła ona występować w deklaracji typu rekordu.</span><span class="sxs-lookup"><span data-stu-id="abea1-160">[ ] **Open issue**: We need to include *primary_constructor_body* in the grammar so that it can appear inside a record type declaration.</span></span>
- <span data-ttu-id="abea1-161">[] **Otwórz problem**: Jakie są reguły konfliktów nazw dla nazw parametrów?</span><span class="sxs-lookup"><span data-stu-id="abea1-161">[ ] **Open issue**: What are the name conflict rules for the parameter names?</span></span> <span data-ttu-id="abea1-162">Najprawdopodobniej jeden z nich nie może powodować konfliktu z parametrem typu lub innym *parametrem rekordu*.</span><span class="sxs-lookup"><span data-stu-id="abea1-162">Presumably one is not allowed to conflict with a type parameter or another *record-parameter*.</span></span>
- <span data-ttu-id="abea1-163">[] **Otwórz problem**: musimy określić zakres parametrów rekordu.</span><span class="sxs-lookup"><span data-stu-id="abea1-163">[ ] **Open issue**: We need to specify the scope of the record-parameters.</span></span> <span data-ttu-id="abea1-164">Gdzie można ich używać?</span><span class="sxs-lookup"><span data-stu-id="abea1-164">Where can they be used?</span></span> <span data-ttu-id="abea1-165">Najprawdopodobniej w inicjatorach pola wystąpienia i *primary_constructor_body* co najmniej.</span><span class="sxs-lookup"><span data-stu-id="abea1-165">Presumably within instance field initializers and *primary_constructor_body* at least.</span></span>
- <span data-ttu-id="abea1-166">[] **Problem otwarty**: Czy można utworzyć deklarację typu rekordu jako częściowej?</span><span class="sxs-lookup"><span data-stu-id="abea1-166">[ ] **Open issue**: Can a record type declaration be partial?</span></span> <span data-ttu-id="abea1-167">Jeśli tak, należy powtórzyć parametry dla każdej części?</span><span class="sxs-lookup"><span data-stu-id="abea1-167">If so, must the parameters be repeated on each part?</span></span>

#### <a name="members-of-a-record-type"></a><span data-ttu-id="abea1-168">Elementy członkowskie typu rekordu</span><span class="sxs-lookup"><span data-stu-id="abea1-168">Members of a record type</span></span>

<span data-ttu-id="abea1-169">Oprócz elementów członkowskich zadeklarowanych w *treści klasy*, typ rekordu ma następujące dodatkowe elementy członkowskie:</span><span class="sxs-lookup"><span data-stu-id="abea1-169">In addition to the members declared in the *class-body*, a record type has the following additional members:</span></span>

##### <a name="primary-constructor"></a><span data-ttu-id="abea1-170">Konstruktor podstawowy</span><span class="sxs-lookup"><span data-stu-id="abea1-170">Primary Constructor</span></span>

<span data-ttu-id="abea1-171">Typ rekordu ma Konstruktor `public`, którego sygnatura odnosi się do parametrów wartości deklaracji typu.</span><span class="sxs-lookup"><span data-stu-id="abea1-171">A record type has a `public` constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="abea1-172">Jest to nazywane *konstruktorem podstawowym* dla typu i powoduje pominięcie niejawnie zadeklarowanego *konstruktora domyślnego* .</span><span class="sxs-lookup"><span data-stu-id="abea1-172">This is called the *primary constructor* for the type, and causes the implicitly declared *default constructor* to be suppressed.</span></span>

<span data-ttu-id="abea1-173">W czasie wykonywania podstawowy Konstruktor</span><span class="sxs-lookup"><span data-stu-id="abea1-173">At runtime the primary constructor</span></span>

* <span data-ttu-id="abea1-174">Inicjuje pola zapasowe generowane przez kompilator dla właściwości odpowiadających parametrom wartości (jeśli te właściwości są udostępniane przez kompilator; [Zobacz 1.1.2](#1.1.2)); następnie</span><span class="sxs-lookup"><span data-stu-id="abea1-174">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; [see 1.1.2](#1.1.2)); then</span></span>
* <span data-ttu-id="abea1-175">wykonuje inicjatory pola wystąpienia pojawiające się w *treści klasy*; a następnie</span><span class="sxs-lookup"><span data-stu-id="abea1-175">executes the instance field initializers appearing in the *class-body*; and then</span></span>
* <span data-ttu-id="abea1-176">wywołuje Konstruktor klasy bazowej:</span><span class="sxs-lookup"><span data-stu-id="abea1-176">invokes a base class constructor:</span></span>
    * <span data-ttu-id="abea1-177">Jeśli w *record_base_arguments*znajdują się argumenty, zostanie wywołany Konstruktor podstawowy wybrany przez metodę rozpoznawania przeciążenia z tymi argumentami.</span><span class="sxs-lookup"><span data-stu-id="abea1-177">If there are arguments in the *record_base_arguments*, a base constructor selected by overload resolution with these arguments is invoked;</span></span>
    * <span data-ttu-id="abea1-178">W przeciwnym razie Konstruktor podstawowy jest wywoływany bez argumentów.</span><span class="sxs-lookup"><span data-stu-id="abea1-178">Otherwise a base constructor is invoked with no arguments.</span></span>
* <span data-ttu-id="abea1-179">wykonuje treść każdej *primary_constructor_body*, jeśli istnieje, w kolejności źródłowej.</span><span class="sxs-lookup"><span data-stu-id="abea1-179">executes the body of each *primary_constructor_body*, if any, in source order.</span></span>

- <span data-ttu-id="abea1-180">[] **Otwórz problem**: musimy określić tę kolejność, szczególnie w przypadku jednostek kompilacji dla częściowych.</span><span class="sxs-lookup"><span data-stu-id="abea1-180">[ ] **Open issue**: We need to specify that order, particularly across compilation units for partials.</span></span>
- <span data-ttu-id="abea1-181">[] **Otwórz problem**: musimy określić, że każdy jawnie zadeklarowany Konstruktor musi być łańcuchem do głównego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="abea1-181">[ ] **Open Issue**: We need to specify that every explicitly declared constructor must chain to the primary constructor.</span></span>
- <span data-ttu-id="abea1-182">[] **Otwórz problem**: czy zezwolić na zmianę modyfikatora dostępu dla głównego konstruktora?</span><span class="sxs-lookup"><span data-stu-id="abea1-182">[ ] **Open issue**: Should it be allowed to change the access modifier on the primary constructor?</span></span>
- <span data-ttu-id="abea1-183">[] **Problem otwarty**: w strukturze rekordów występuje błąd w przypadku braku parametrów rekordu?</span><span class="sxs-lookup"><span data-stu-id="abea1-183">[ ] **Open issue**: In a record struct, it is an error for there to be no record parameters?</span></span>

##### <a name="primary-constructor-body"></a><span data-ttu-id="abea1-184">Podstawowa treść konstruktora</span><span class="sxs-lookup"><span data-stu-id="abea1-184">Primary constructor body</span></span>

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

<span data-ttu-id="abea1-185">*Primary_constructor_body* może być używana tylko w deklaracji typu rekordu.</span><span class="sxs-lookup"><span data-stu-id="abea1-185">A *primary_constructor_body* may only be used within a record type declaration.</span></span> <span data-ttu-id="abea1-186">*Identyfikator* *primary_constructor_body* powinien określać typ rekordu, w którym jest zadeklarowany.</span><span class="sxs-lookup"><span data-stu-id="abea1-186">The *identifier* of a *primary_constructor_body* shall name the record type in which it is declared.</span></span>

<span data-ttu-id="abea1-187">*Primary_constructor_body* nie deklaruje elementu członkowskiego samodzielnie, ale jest sposobem, w jaki programista może dostarczyć atrybuty dla i określić dostęp do obiektu podstawowego konstruktora typu rekordu.</span><span class="sxs-lookup"><span data-stu-id="abea1-187">The *primary_constructor_body* does not declare a member on its own, but is a way for the programmer to provide attributes for, and specify the access of, a record type's primary constructor.</span></span> <span data-ttu-id="abea1-188">Umożliwia także programistom dostarczanie dodatkowego kodu, który zostanie wykonany w przypadku konstruowania wystąpienia typu rekordu.</span><span class="sxs-lookup"><span data-stu-id="abea1-188">It also enables the programmer to provide additional code that will be executed when an instance of the record type is constructed.</span></span>

- <span data-ttu-id="abea1-189">[] **Otwórz problem**: należy pamiętać, że Konstruktor domyślny struktury pomija ten element.</span><span class="sxs-lookup"><span data-stu-id="abea1-189">[ ] **Open issue**: We should note that a struct default constructor bypasses this.</span></span>
- <span data-ttu-id="abea1-190">[] **Otwórz problem**: należy określić kolejność wykonywania inicjalizacji.</span><span class="sxs-lookup"><span data-stu-id="abea1-190">[ ] **Open issue**: We should specify the execution order of initialization.</span></span>
- <span data-ttu-id="abea1-191">[] **Problem otwarty**: czy w deklaracji typu bez rekordu nie ma zezwolenia na *primary_constructor_body* (najprawdopodobniej bez atrybutów i modyfikatorów) i traktuje ją jak kod inicjatora pola wystąpienia?</span><span class="sxs-lookup"><span data-stu-id="abea1-191">[ ] **Open issue**: Should we allow something like a *primary_constructor_body* (presumably without attributes and modifiers) in a non-record type declaration, and treat it like we would the code of an instance field initializer?</span></span>

##### <a name="properties"></a><span data-ttu-id="abea1-192">Właściwości</span><span class="sxs-lookup"><span data-stu-id="abea1-192">Properties</span></span>

<span data-ttu-id="abea1-193">Dla każdego parametru rekordu deklaracji typu rekordu istnieje odpowiedni element członkowski właściwości `public`, którego nazwa i typ są pobierane z deklaracji parametru wartości.</span><span class="sxs-lookup"><span data-stu-id="abea1-193">For each record parameter of a record type declaration there is a corresponding `public` property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="abea1-194">Jego nazwa jest *identyfikatorem* *record_property_name*, jeśli istnieje, lub *identyfikatorem* *record_parameter* w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="abea1-194">Its name is the *identifier* of the *record_property_name*, if present, or the *identifier* of the *record_parameter* otherwise.</span></span> <span data-ttu-id="abea1-195">Jeśli żadna konkretna (czyli nieabstrakcyjna) Właściwość publiczna z metodą dostępu `get` i z tą nazwą i typem jest jawnie zadeklarowana lub dziedziczona, jest generowana przez kompilator w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="abea1-195">If no concrete (i.e. non-abstract) public property with a `get` accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

* <span data-ttu-id="abea1-196">Dla *struktury rekordu* lub *klasy rekordu*`sealed`:</span><span class="sxs-lookup"><span data-stu-id="abea1-196">For a *record struct* or a `sealed` *record class*:</span></span>
 * <span data-ttu-id="abea1-197">`private` pole `readonly` jest tworzone jako pole zapasowe dla właściwości `readonly`.</span><span class="sxs-lookup"><span data-stu-id="abea1-197">A `private` `readonly` field is produced as a backing field for a `readonly` property.</span></span> <span data-ttu-id="abea1-198">Jej wartość jest inicjowana podczas konstruowania z wartością odpowiadającego parametru podstawowego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="abea1-198">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span>
 * <span data-ttu-id="abea1-199">Metoda dostępu `get` właściwości jest zaimplementowana w celu zwrócenia wartości pola zapasowego.</span><span class="sxs-lookup"><span data-stu-id="abea1-199">The property's `get` accessor is implemented to return the value of the backing field.</span></span>
 * <span data-ttu-id="abea1-200">Każdy "zgodny" `get` akcesora dziedziczonej właściwości wirtualnej jest zastępowany.</span><span class="sxs-lookup"><span data-stu-id="abea1-200">Each "matching" inherited virtual property's `get` accessor is overridden.</span></span>

> <span data-ttu-id="abea1-201">**Uwagi dotyczące projektowania**: innymi słowy, jeśli rozszerzono klasę bazową lub implementuje interfejs, który deklaruje publiczną właściwość abstrakcyjną o tej samej nazwie i typie jako parametr rekordu, ta właściwość jest zastępowana lub zaimplementowana.</span><span class="sxs-lookup"><span data-stu-id="abea1-201">**Design notes**: In other words, if you extend a base class or implement an interface that declares a public abstract property with the same name and type as a record parameter, that property is overridden or implemented.</span></span>

- <span data-ttu-id="abea1-202">[] **Problem otwarty**: Czy można zmienić modyfikator dostępu dla właściwości, gdy jest on jawnie zadeklarowany?</span><span class="sxs-lookup"><span data-stu-id="abea1-202">[ ] **Open issue**: Should it be possible to change the access modifier on a property when it is explicitly declared?</span></span>
- <span data-ttu-id="abea1-203">[] **Problem otwarty**: czy istnieje możliwość zastąpienia pola dla właściwości?</span><span class="sxs-lookup"><span data-stu-id="abea1-203">[ ] **Open issue**: Should it be possible to substitute a field for a property?</span></span>

##### <a name="object-methods"></a><span data-ttu-id="abea1-204">Metody obiektów</span><span class="sxs-lookup"><span data-stu-id="abea1-204">Object Methods</span></span>

<span data-ttu-id="abea1-205">Dla *struktury rekordu* lub *klasy rekordu*`sealed`, implementacje metod `object.GetHashCode()` i `object.Equals(object)` są wytwarzane przez kompilator, chyba że zostaną dostarczone przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="abea1-205">For a *record struct* or a `sealed` *record class*, implementations of the methods `object.GetHashCode()` and `object.Equals(object)` are produced by the compiler unless provided by the user.</span></span>

- <span data-ttu-id="abea1-206">[] **Otwórz problem**: należy precyzyjnie określić ich implementację.</span><span class="sxs-lookup"><span data-stu-id="abea1-206">[ ] **Open issue**: We should precisely specify their implementation.</span></span>
- <span data-ttu-id="abea1-207">[] **Otwórz problem**: należy również dodać `IEquatable<T>` interfejsu dla typu rekordu i określić, że są udostępniane implementacje.</span><span class="sxs-lookup"><span data-stu-id="abea1-207">[ ] **Open issue**: We should also add the interface `IEquatable<T>` for the record type and specify that implementations are provided.</span></span>
- <span data-ttu-id="abea1-208">[] **Otwórz problem**: należy również określić, że implementujemy każdy `IEquatable<T>.Equals`.</span><span class="sxs-lookup"><span data-stu-id="abea1-208">[ ] **Open issue**: We should also specify that we implement every `IEquatable<T>.Equals`.</span></span>
- <span data-ttu-id="abea1-209">[] **Problem otwarty**: należy określić dokładnie, w jaki sposób rozwiązujemy problem z równą się w celu dziedziczenia rekordów: w tym celu generują metody równości, takie jak symetryczne, przechodnie, zwrotne itd.</span><span class="sxs-lookup"><span data-stu-id="abea1-209">[ ] **Open issue**: We should specify precisely how we solve the problem of Equals in the face of record inheritance: specifically how we generate equality methods such that they are symmetric, transitive, reflexive, etc.</span></span>
- <span data-ttu-id="abea1-210">[] **Otwórz problem**: zaproponowano implementację `operator ==` i `operator !=` dla typów rekordów.</span><span class="sxs-lookup"><span data-stu-id="abea1-210">[ ] **Open issue**: It has been proposed that we implement `operator ==` and `operator !=` for record types.</span></span>
- <span data-ttu-id="abea1-211">[] **Otwórz problem**: Czy chcesz automatycznie wygenerować implementację `object.ToString`?</span><span class="sxs-lookup"><span data-stu-id="abea1-211">[ ] **Open issue**: Should we auto-generate an implementation of `object.ToString`?</span></span>

##### `Deconstruct`

<span data-ttu-id="abea1-212">Typ rekordu ma metodę `public` wygenerowaną przez kompilator `void Deconstruct`, chyba że jest ona dostarczana przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="abea1-212">A record type has a compiler-generated `public` method `void Deconstruct` unless one with any signature is provided by the user.</span></span> <span data-ttu-id="abea1-213">Każdy parametr jest `out` parametr o tej samej nazwie i typie co odpowiedni parametr typu rekordu.</span><span class="sxs-lookup"><span data-stu-id="abea1-213">Each parameter is an `out` parameter of the same name and type as the corresponding parameter of the record type.</span></span> <span data-ttu-id="abea1-214">Wdrożenie tej metody przez kompilator przypisuje każdy parametr `out` z wartością odpowiedniej właściwości.</span><span class="sxs-lookup"><span data-stu-id="abea1-214">The compiler-provided implementation of this method shall assign each `out` parameter with the value of the corresponding property.</span></span>

<span data-ttu-id="abea1-215">Zapoznaj [się ze specyfikacją dopasowania wzorca](csharp-8.0/patterns.md#positional-pattern) dla semantyki `Deconstruct`.</span><span class="sxs-lookup"><span data-stu-id="abea1-215">See [the pattern-matching specification](csharp-8.0/patterns.md#positional-pattern) for the semantics of `Deconstruct`.</span></span>

##### <a name="with-method"></a><span data-ttu-id="abea1-216">`With`, Metoda</span><span class="sxs-lookup"><span data-stu-id="abea1-216">`With` method</span></span>

<span data-ttu-id="abea1-217">O ile nie istnieje zadeklarowany przez użytkownika element członkowski o nazwie `With` zadeklarowany, typ rekordu ma metodę dostarczoną przez kompilator o nazwie `With`, której typ zwracany jest samym typem rekordu i zawierający jeden parametr wartości odpowiadający każdemu *parametrowi rekordu* w tej samej kolejności, w której te parametry występują w deklaracji typu rekordu.</span><span class="sxs-lookup"><span data-stu-id="abea1-217">Unless there is a user-declared member named `With` declared, a record type has a compiler-provided method named `With` whose return type is the record type itself, and containing one value parameter corresponding to each *record-parameter* in the same order that these parameters appear in the record type declaration.</span></span> <span data-ttu-id="abea1-218">Każdy parametr musi mieć *domyślny argument Caller* dla odpowiadającej właściwości.</span><span class="sxs-lookup"><span data-stu-id="abea1-218">Each parameter shall have a *caller-receiver default-argument* of the corresponding property.</span></span>

<span data-ttu-id="abea1-219">W klasie `abstract` rekordów, dostarczona przez kompilator `With` Metoda jest abstrakcyjna.</span><span class="sxs-lookup"><span data-stu-id="abea1-219">In an `abstract` record class, the compiler-provided `With` method is abstract.</span></span> <span data-ttu-id="abea1-220">W strukturze rekordów lub zapieczętowanej klasie rekordów, `With` Metoda udostępniona przez kompilator jest `sealed`.</span><span class="sxs-lookup"><span data-stu-id="abea1-220">In a record struct, or a sealed record class, the compiler-provided `With` method is `sealed`.</span></span> <span data-ttu-id="abea1-221">W przeciwnym razie metoda `With` podana przez kompilator to "wirtualna, a jej implementacja zwróci nowe wystąpienie utworzone przez wywołanie konstruktora podstawowego z parametrami jako argumentami, aby utworzyć nowe wystąpienie z parametrów i zwrócić to nowe wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="abea1-221">Otherwise the compiler-provided `With` method is \`virtual and its implementation shall return a new instance produced by invoking the primary constructor with the parameters as arguments to create a new instance from the parameters, and return that new instance.</span></span>

- <span data-ttu-id="abea1-222">[] **Otwórz problem**: należy również określić warunki, które zastępują lub implementują dziedziczone metody `With` wirtualnej lub metody `With` z zaimplementowanych interfejsów.</span><span class="sxs-lookup"><span data-stu-id="abea1-222">[ ] **Open issue**: We should also specify under what conditions we override or implement inherited virtual `With` methods or `With` methods from implemented interfaces.</span></span>
- <span data-ttu-id="abea1-223">[] **Otwórz problem**: należy powiedzieć, co się dzieje, gdy odziedziczymy niewirtualną metodę `With`.</span><span class="sxs-lookup"><span data-stu-id="abea1-223">[ ] **Open issue**: We should say what happens when we inherit a non-virtual `With` method.</span></span>

> <span data-ttu-id="abea1-224">**Uwagi dotyczące projektowania**: ponieważ typy rekordów są domyślnie niezmienne, Metoda `With` zapewnia sposób tworzenia nowego wystąpienia, które jest takie samo jak istniejące wystąpienie, ale z wybranymi właściwościami, które mają nowe wartości.</span><span class="sxs-lookup"><span data-stu-id="abea1-224">**Design notes**: Because record types are by default immutable, the `With` method provides a way of creating a new instance that is the same as an existing instance but with selected properties given new values.</span></span> <span data-ttu-id="abea1-225">Na przykład podanym</span><span class="sxs-lookup"><span data-stu-id="abea1-225">For example, given</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="abea1-226">istnieje element członkowski dostarczony przez kompilator</span><span class="sxs-lookup"><span data-stu-id="abea1-226">there is a compiler-provided member</span></span>
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> <span data-ttu-id="abea1-227">Co umożliwia zmienną typu rekordu</span><span class="sxs-lookup"><span data-stu-id="abea1-227">Which enables an variable of the record type</span></span>
> ```cs
> var p = new Point(3, 4);
> ```
> <span data-ttu-id="abea1-228">do zamienienia na wystąpienie, które ma co najmniej jedną właściwość inną</span><span class="sxs-lookup"><span data-stu-id="abea1-228">to be replaced with an instance that has one or more properties different</span></span>
> ```cs
>     p = p.With(X: 5);
> ```
> <span data-ttu-id="abea1-229">Można to również wyrazić przy użyciu *with_expression*:</span><span class="sxs-lookup"><span data-stu-id="abea1-229">This can also be expressed using the *with_expression*:</span></span>
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a><span data-ttu-id="abea1-230">Przykłady</span><span class="sxs-lookup"><span data-stu-id="abea1-230">Examples</span></span>

#### <a name="compatibility-of-record-types"></a><span data-ttu-id="abea1-231">Zgodność typów rekordów</span><span class="sxs-lookup"><span data-stu-id="abea1-231">Compatibility of record types</span></span>

<span data-ttu-id="abea1-232">Ponieważ programista może dodawać członków do deklaracji typu rekordu, często istnieje możliwość zmiany zestawu elementów rekordu bez wpływu na istniejących klientów.</span><span class="sxs-lookup"><span data-stu-id="abea1-232">Because the programmer can add members to a record type declaration, it is often possible to change the set of record elements without affecting existing clients.</span></span> <span data-ttu-id="abea1-233">Na przykład, przy użyciu początkowej wersji typu rekordu</span><span class="sxs-lookup"><span data-stu-id="abea1-233">For example, given an initial version of a record type</span></span>

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

<span data-ttu-id="abea1-234">Nowy element typu rekordu można compatibly dodać w następnej wersji typu bez wpływu na zgodność binarną lub źródłową:</span><span class="sxs-lookup"><span data-stu-id="abea1-234">A new element of the record type can be compatibly added in the next revision of the type without affecting binary or source compatibility:</span></span>

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a><span data-ttu-id="abea1-235">przykład struktury rekordu</span><span class="sxs-lookup"><span data-stu-id="abea1-235">record struct example</span></span>

<span data-ttu-id="abea1-236">Ta struktura rekordu</span><span class="sxs-lookup"><span data-stu-id="abea1-236">This record struct</span></span>

```cs
public struct Pair(object First, object Second);
```

<span data-ttu-id="abea1-237">jest tłumaczony na ten kod</span><span class="sxs-lookup"><span data-stu-id="abea1-237">is translated to this code</span></span>

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- <span data-ttu-id="abea1-238">[] **Otwórz problem**: czy należy wykonać implementację Equals (para druga) jako publiczną składową pary?</span><span class="sxs-lookup"><span data-stu-id="abea1-238">[ ] **Open issue**: should the implementation of Equals(Pair other) be a public member of Pair?</span></span>
- <span data-ttu-id="abea1-239">[] **Otwórz problem**: ta implementacja `Equals` nie jest symetryczna w celu dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="abea1-239">[ ] **Open issue**: This implementation of `Equals` is not symmetric in the face of inheritance.</span></span>
- <span data-ttu-id="abea1-240">[] **Problem otwarty**: czy powinien zostać zadeklarować rekord `operator ==` i `operator !=`?</span><span class="sxs-lookup"><span data-stu-id="abea1-240">[ ] **Open issue**: Should a record declare `operator ==` and `operator !=`?</span></span>

> <span data-ttu-id="abea1-241">**Uwagi dotyczące projektowania**: ponieważ jeden typ rekordu może dziedziczyć z innego, a ta implementacja `Equals` nie może być symetryczna w tym przypadku, nie jest poprawna.</span><span class="sxs-lookup"><span data-stu-id="abea1-241">**Design notes**: Because one record type can inherit from another, and this implementation of `Equals` would not be symmetric in that case, it is not correct.</span></span> <span data-ttu-id="abea1-242">Proponujemy zaimplementowanie równości w ten sposób:</span><span class="sxs-lookup"><span data-stu-id="abea1-242">We propose to implement equality this way:</span></span>
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> <span data-ttu-id="abea1-243">Rekordy pochodne byłyby `override EqualityContract`.</span><span class="sxs-lookup"><span data-stu-id="abea1-243">Derived records would `override EqualityContract`.</span></span> <span data-ttu-id="abea1-244">Bardziej atrakcyjną alternatywą jest ograniczenie dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="abea1-244">The less attractive alternative is to restrict inheritance.</span></span>

#### <a name="sealed-record-example"></a><span data-ttu-id="abea1-245">przykład zapieczętowanego rekordu</span><span class="sxs-lookup"><span data-stu-id="abea1-245">sealed record example</span></span>

<span data-ttu-id="abea1-246">Ta klasa zapieczętowanego rekordu</span><span class="sxs-lookup"><span data-stu-id="abea1-246">This sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa);
```

<span data-ttu-id="abea1-247">jest przetłumaczony na ten kod</span><span class="sxs-lookup"><span data-stu-id="abea1-247">is translated into this code</span></span>

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a><span data-ttu-id="abea1-248">przykład klasy rekordu abstrakcyjnego</span><span class="sxs-lookup"><span data-stu-id="abea1-248">abstract record class example</span></span>

<span data-ttu-id="abea1-249">Ta klasa typu abstrakcyjnego</span><span class="sxs-lookup"><span data-stu-id="abea1-249">This abstract record class</span></span>

```cs
public abstract class Person(string Name);
```

<span data-ttu-id="abea1-250">jest przetłumaczony na ten kod</span><span class="sxs-lookup"><span data-stu-id="abea1-250">is translated into this code</span></span>

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a><span data-ttu-id="abea1-251">Łączenie abstrakcyjnych i zapieczętowanych rekordów</span><span class="sxs-lookup"><span data-stu-id="abea1-251">combining abstract and sealed records</span></span>

<span data-ttu-id="abea1-252">Podanej powyżej klasy `Person` rekordów abstrakcyjnych</span><span class="sxs-lookup"><span data-stu-id="abea1-252">Given the abstract record class `Person` above, this sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

<span data-ttu-id="abea1-253">jest przetłumaczony na ten kod</span><span class="sxs-lookup"><span data-stu-id="abea1-253">is translated into this code</span></span>

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a><span data-ttu-id="abea1-254">Wady</span><span class="sxs-lookup"><span data-stu-id="abea1-254">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="abea1-255">Podobnie jak w przypadku dowolnej funkcji języka należy zwrócić uwagę na to, czy dodatkowa złożoność tego języka jest zapłacona w dodatkowym przejrzystości oferowanym C# przez funkcję.</span><span class="sxs-lookup"><span data-stu-id="abea1-255">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="abea1-256">Alternatywy</span><span class="sxs-lookup"><span data-stu-id="abea1-256">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="abea1-257">Rozważamy Dodawanie *konstruktorów podstawowych* w C# 6.</span><span class="sxs-lookup"><span data-stu-id="abea1-257">We considered adding *primary constructors* in C# 6.</span></span> <span data-ttu-id="abea1-258">Chociaż zajmują one taką samą powierzchnię syntaktyczną jak ta propozycja, stwierdzamy, że są one niewielkimi korzyściami oferowanymi przez rekordy.</span><span class="sxs-lookup"><span data-stu-id="abea1-258">Although they occupy the same syntactic surface as this proposal, we found that they fell short of the advantages offered by records.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="abea1-259">Nierozwiązane pytania</span><span class="sxs-lookup"><span data-stu-id="abea1-259">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="abea1-260">W treści wniosku są wyświetlane otwarte pytania.</span><span class="sxs-lookup"><span data-stu-id="abea1-260">Open questions appear in the body of the proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="abea1-261">Spotkania projektowe</span><span class="sxs-lookup"><span data-stu-id="abea1-261">Design meetings</span></span>

<span data-ttu-id="abea1-262">TBD</span><span class="sxs-lookup"><span data-stu-id="abea1-262">TBD</span></span>
